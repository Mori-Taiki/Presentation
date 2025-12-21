---
marp: true
header: "ドメイン駆動設計の戦術"
theme: default
paginate: true
style: |
  section {
    background-color: #f1f0eb;
  }
  .columns {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 32px;
    align-items: start;
  }
  .center {
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;   
  }
  .small {
    font-size: 16px;
  } 
  .midium {
    font-size: 20px;
  } 
  .large {
    font-size: 26px;
  } 
---

# 閑話休題
好きな漫画とかコメントしてください

---

# ドメイン駆動開発の戦術

- 複雑で独自性の高い分野はどのように実装すべきか→ドメインモデル
- しかし、ドメインモデルの実装には理解すべき前提が多い
(ユビキタス言語・区切られた文脈・集約・エンティティ・値オブジェクト...)
- まずは、いつでも使える値オブジェクトから始めよう

---

# 値オブジェクトを使って、<br>エンティティに**振る舞い**を持たせる

---

## 値オブジェクト
### 値オブジェクトの原則
- **識別子を持たず**、**値そのものに意味があり**、**不変である**ことを前提に扱うオブジェクト
- 例：お金をintで扱わずに、intのプロパティを持つ Money型として扱う
    - 交換可能性：1000円という値は、どの1000円札でも良い
     ＝ 識別子を持たない
    - 同一性：ある1000円札と、他の1000円札はイコールであるとみなせる
    - 不変性：1000円から100円引くときに、1000円札そのものを変更せず、900円を新たに作り出す

--- 

### プリミティブ型に固執するデメリット

<div class="small">

### BAD①：業務（ドメイン）上の意味が読み取れないロジック

```csharp
// 120って何？単位は？どのドメインの制約？
if ((placedAt - shippedAt) > 120) throw new Exception("NG");
```
### BAD②：あちこちに散らばった業務知識

```csharp
// Controller 
const int LimitMinutes = 120
// API POST: 120分チェック
if ((placedAt - shippedAt) > LimitMinutes) return BadRequest();
・・・

// API PUT: 120分チェック
if ((placedAt - shippedAt) > LimitMinutes) return BadRequest();
・・・

// API PATCH: 120分チェック
if ((placedAt - shippedAt) > LimitMinutes) return BadRequest();
・・・

// BATCH
const int LimitMinutes = 120
status = (placedAt - shippedAt) <= LimitMinutes? "OK" : "NG";
・・・
```
</div>

### →こうなるとドキュメントで業務知識を補完するしかなくなる

---
### 値オブジェクトを使うことで解決されること
<div class="small">

### オブジェクトにすることで・・・
1. その値に名前を与えることができる
2. その値に振る舞いを持たせることができる
</div>
<div class="small">

### GOOD：コードから業務知識が読み取れる

```csharp

// 利用箇所
var time = TransportTime.From(shippedAt, placedAt); // 120分超ならここで失敗（仕様が型にある）

// 定義
public sealed record class TransportTime
{
    public int Minutes { get; private set; }
    public const int LimitMinutes = 120;

    private TransportTime() { } // EF用

    private TransportTime(int minutes)
    {
            // ルールは一か所に凝集する
        if (minutes < 0) throw new ArgumentOutOfRangeException(nameof(minutes));
        if (minutes > LimitMinutes) throw new InvalidOperationException($"運搬時間は{LimitMinutes}分以内。");
        // もし夏季は60分以内で運搬する、というルールが発生した場合、このクラスにまとめる
        Minutes = minutes; // 常に不正ではない値だけが初期化される
    }

    public static TransportTime From(DateTime shippedAt, DateTime placedAt)
        => new((int)Math.Ceiling((placedAt - shippedAt).TotalMinutes));
}
```
</div>

--- 

- Q1：このような状態をなんという？
    - 高凝集
- 値オブジェクトを利用することで凝集度を高くすることができる
    - 逆にチェックロジックがあちこちに散らばっている状態を低凝集という

---

<div class="columns" >
<div class="midium">

# static固執のデメリット
## ？「一か所にチェックロジックをまとめたいなら、staticな値Helper（値Util）クラスじゃダメなの？」

- ①メソッド化しても、呼びだされなければ意味が無いので、リスクは残る
- ②そのロジックが複雑に絡み合い、どんどん複雑化する可能性
    - 凝集レベルでいうと論理的凝集に当たり、
    7つの凝集度の中で下から２番目

</div>
<div class="midium">

``` csharp
public static class ConcretePlacementHelper
{
    public static void Post(
        ConcretePlacementDto dto,
        ConcreteDbContext db)
    {
        Validate(dto);

        EnsureWithinAllowedTime(dto);

        var entity = new ConcretePlacement
        {
            DeliveryTime = dto.DeliveryTime,
            PlacementTime = dto.PlacementTime,
            Volume = dto.Volume
        };

        db.ConcretePlacements.Add(entity);
        db.SaveChanges();
    }

    // RESTの公開メソッド群
    // PUT GET など

    // ---- 以下、ドメインロジック ----

    private static void Validate(ConcretePlacementDto dto)
    {
        if (dto.Volume <= 0)
        {
            throw new InvalidOperationException("数量は正の値である必要があります");
        }
    }

    private static void EnsureWithinAllowedTime(ConcretePlacementDto dto)
    {
        var duration = dto.PlacementTime - dto.DeliveryTime;

        if (duration > TimeSpan.FromHours(2))
        {
            throw new InvalidOperationException("打設時間超過です");
        }
    }
}
```

</div>
</div>

---

### BAD①：呼び忘れる

```csharp
public static class DeliveryRules
{
    public static void EnsureWithin120(DateTime shippedAt, DateTime placedAt)
    {
        if ((placedAt - shippedAt).TotalMinutes > 120)
            throw new InvalidOperationException("運搬時間NG");
    }
}

// API PUT: ちゃんと呼ぶ
DeliveryRules.EnsureWithin120(e.ShippedAt, placedAt);
e.TransportMinutes = (int)(placedAt - e.ShippedAt).TotalMinutes;

// API POST: 呼び忘れてもコンパイルは通る（事故が起きる）
e.TransportMinutes = (int)(placedAt - e.ShippedAt).TotalMinutes;
```

---

### BAD②：論理的凝集に陥る

```csharp
public static class DeliveryRules
{
    public static void EnsureWithin120(...) { ... }
    public static void EnsurePhotoCount(...) { ... }
    public static void EnsureCustomerException(...) { ... }
    // “配送”に関するものが何でも入る → 増えるほど追えない
}
```

- しかも、同じクラスに置かれていることで、switchでの分岐やif elseの増加につながる
    - ここまでくると密結合

---

##  Q：どちらが複雑？：値オブジェクトで書かれたロジック

<div class="columns" >
<div class="midium">

### BAD：3つの自由な値

```csharp
public sealed record class DeliveryBad
{
    public DateTime ShippedAt { get; init; }
    public DateTime PlacedAt  { get; init; }
    public int ElapsedMinutes { get; init; } // ←自由な値

    public bool IsWithin120()
        => ElapsedMinutes <= 120; 
}

var bad = new DeliveryBad
{
    ShippedAt = new DateTime(2025, 12, 16,  9,  0, 0),
    PlacedAt  = new DateTime(2025, 12, 16, 12,  0, 0),//180分
    ElapsedMinutes = 120　// （嘘の状態）
    // 時間差は180分なのに ElapsedMinutes=120 が入ってしまう

};
```
</div>
<div class="midium">

### GOOD：2つの値と１つの計算される値

```csharp
public sealed record class ConcreteDelivery
{
    public DateTime ShippedAt { get; }
    public DateTime PlacedAt  { get; }
    // “かかった時間”は状態として持たない。常に計算される。
    public TimeSpan TransportDuration => PlacedAt - ShippedAt;

    public ConcreteDelivery(DateTime shippedAt, DateTime placedAt)
    {
        if (placedAt < shippedAt) 
            throw new ArgumentException
                ("打設時刻は出荷時刻以降である必要があります");
        ShippedAt = shippedAt;
        PlacedAt  = placedAt;

        if (TransportDuration > TimeSpan.FromMinutes(120))
            throw new InvalidOperationException
                ("運搬時間は120分以内でなければなりません");
    }
}

// 利用例（不正値を生成しようとすると例外発生）
var delivery = new ConcreteDelivery(
    shippedAt: new DateTime(2025, 12, 16,  9,  0, 0),
    placedAt:  new DateTime(2025, 12, 16, 10, 10, 0)
);

```

</div>
</div>


---

- 内部状態に関するすべてのロジックを値オブジェクトの中に記述する
- 値の自由度を下げ、複雑さを低減する
    - この考え方は、Reactにおいて計算によってStateをできるだけ減らそうという教えと同じ
    - 姓・名がある場合にはフルネームは不要…みたいな

---

# EFCoreで表現する値オブジェクト

## 翻って、EFCoreとは何だったか

- DBの都合を隠蔽し、文字列のSQLではなく、「オブジェクト」を中心とするものであった
    - ①オブジェクト中心であることで、レコード同士の関連を表現しやすくなる
    - ②(New)オブジェクト中心であることで、複雑な「業務ロジック」を表現（モデル化）しやすくなる

- 中核的領域では、業務ロジックは「本質的に複雑」であるため、オブジェクトを中心にする必要がある(DBを抽象化する)
---


- 逆に、ただEFCoreを使うだけだと、トランザクションスクリプトとアクティブレコードの悪いとこ取りになる
    - レコードは**どこからでも変更**でき、ドメインロジックと**密結合**している
    しかも**手続き的に記述**されており、SQLの隠蔽だけが残る
    - これは**貧血ドメインモデル**と呼ばれ、
「"トランザクションスクリプト”と“アクティブレコード”両方の性質を持つ」　アンチパターンである
    ![hisoka.jpg](hisoka.jpg)
- 値オブジェクトによって、値にまつわるルールがカプセル化される

---

## Complex TypesとFluent APIで値オブジェクトをDBカラムに紐づける

- 値オブジェクトはPOCOである必要があり、フレームワークに依存させてはいけない
    - EFCoreに依存させると、結局DBの複雑性を持ち込ませることになるから
- POCOの値オブジェクトをどうやってDBに紐づけるのか？ マッパーの独自実装が必要？→その必要は無い

---

### Complex Typesを利用して、値オブジェクトをマッピングする
### Step1 POCOで値オブジェクトを書く
```csharp
public sealed record class TransportTime
{
    public int Minutes { get; private set; }
    public const int LimitMinutes = 120;

    private TransportTime() { } // EF用おまじない

    private TransportTime(int minutes)
    {
        // ルールは一か所に凝集する
        if (minutes < 0) throw new ArgumentOutOfRangeException(nameof(minutes));
        if (minutes > LimitMinutes) throw new InvalidOperationException($"運搬時間は{LimitMinutes}分以内。");
        // もし夏季は60分以内で運搬する、というルールが発生した場合、このクラスにまとめる
        Minutes = minutes; // 常に不正ではない値だけが初期化される
    }

    public static TransportTime From(DateTime shippedAt, DateTime placedAt)
        => new((int)Math.Ceiling((placedAt - shippedAt).TotalMinutes));
}
```
- おまじないはあるが、EFCoreへのパッケージ依存はしていない

---

### Step2 Complex TypeとしてFluent APIでDBに結び付ける
- コンクリートの配送(ConcreteDelivery)というエンティティの中に、
    配送時間(TranceportTime)がある場合の例
```csharp
// DbContext
modelBuilder.Entity<ConcreteDelivery>()
    .ComplexProperty(x => x.TransportTime, ct =>
        ct.Property(p => p.Minutes).HasColumnName("TransportMinutes"));
```
![alt text](image-11.png)


---

- 注意点：ListのComplex Typesはサポートされていない

```csharp
List<TransportTime> TrancsportTimes // これはそのままだとテーブルに紐づけられない
```

- なぜか…？

---

### 値オブジェクトとエンティティ

|値オブジェクト|↔|エンティティ|
|----|-|----|
|不変|↔|可変|
|IDが無い|↔|IDがある|
- 値オブジェクトは、**値が一緒であれば、同じものである**とみなす
- エンティティの一部として値オブジェクトがある
    - Complex TypesがListをサポートしていないことについて困った場合、
    それは値オブジェクトではなく、エンティティかも

---

```csharp
public class ConcreteDelivery // Aggregate Root(Entity)
{
    public Guid Id { get; private set; }
    public DateTime ShippedAt { get; private set; }

    // これはエンティティ
    public List<TransportCheckpoint> Checkpoints { get; private set; } = new();

    private ConcreteDelivery() { } // EF

    public void MarkPlaced(DateTime placedAt)
    {
        CurrentTransportTime = TransportTime.From(ShippedAt, placedAt); // ルールはVOに集約
        AddCheckpoint(placedAt);
    }

    public void AddCheckpoint(string kind, DateTime at)
    {
        var t = TransportTime.From(ShippedAt, at); // 120分ルール
        Checkpoints.Add(TransportCheckpoint.New(kind, at, t));
    }
}

public class TransportCheckpoint // Entity (履歴の1件)
{
    public Guid Id { get; private set; }   // ←識別子がある（つまりエンティティ）
    public Guid DeliveryId { get; private set; }
    public DateTime RecordedAt { get; private set; }
    public TransportTime TransportTime { get; private set; } = default!; // ここに値オブジェクトが現れる

    private TransportCheckpoint() { } // EF
    private TransportCheckpoint(Guid id, Guid deliveryId, DateTime at, TransportTime t)
        => (Id, DeliveryId, RecordedAt, TransportTime) = (id, deliveryId, at, t);

    public static TransportCheckpoint New(string kind, DateTime at, TransportTime t)
        => new(Guid.NewGuid(), Guid.Empty, at, t);
}

```

---

### OwnsManyを利用して、中間テーブルを値オブジェクトとみなす

- どうしても値オブジェクトのリストを持つべきだと思われるとき
    - Listの中身が重複せず、値が同じなら同じものであるとみなせるもの
    - 中間テーブルを介してマスタを参照するようなテーブル構造のもの
- 例えば：記事に対するタグ、エンジニアに対する資格一覧、工事に対する所属者IDなど
    - 「エンジニア」が持つ「資格」の例
    - IDという値を持つ値オブジェクトだと考えられる

---

<div class="columns" >
<div class="midium">

エンティティ
```csharp
public readonly record struct QualificationId(string Value);

public sealed class Engineer
{
    public Guid Id { get; private set; }

    private readonly List<EngineerQualification> _qualifications = new();
    public IReadOnlyCollection<EngineerQualification> Qualifications => _qualifications;

    private Engineer() { } // EF

    public Engineer(Guid id) => Id = id;

    public void AddQualification(QualificationId qualificationId)
    {
        if (_qualifications.Any(x => x.QualificationId == qualificationId)) return;
        _qualifications.Add(new EngineerQualification(qualificationId));
    }

    public void RemoveQualification(QualificationId qualificationId)
        => _qualifications.RemoveAll(x => x.QualificationId == qualificationId);
}
```

紐づきを表現する値オブジェクト

``` csharp
// “エンジニアと資格の紐づき”だけを表す値オブジェクト（追加情報なしの中間テーブル）
public sealed record class EngineerQualification
{
    public QualificationId QualificationId { get; private set; }

    private EngineerQualification() { } // EF
    public EngineerQualification(QualificationId qualificationId) => QualificationId = qualificationId;
}

// 資格情報マスタ
public sealed class QualificationMaster
{
    public string Id { get; private set; } = default!; // 例: "基本情報", "応用情報"
    public string Name { get; private set; } = default!;
}

```

</div>
<div class="midium">
    
DbContextの定義

```csharp
using Microsoft.EntityFrameworkCore;

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Engineer>(b =>
    {
        b.HasKey(x => x.Id);

        b.OwnsMany(x => x.Qualifications, q =>
        {
            q.ToTable("EngineerQualifications");      // ← 中間テーブル
            q.WithOwner().HasForeignKey("EngineerId");

            // 値オブジェクトの中身を1列にマップ
            q.OwnsOne(x => x.QualificationId, id =>
            {
                id.Property(p => p.Value)
                    .HasColumnName("QualificationId")
                    .IsRequired();
            });

            // 中間テーブルなので複合キーにする（永続化上のキーであって、ドメインIDではない）
            q.HasKey("EngineerId", "QualificationId");

            // 同一エンジニア内での重複防止にもなる
        });
    });

    modelBuilder.Entity<QualificationMaster>(b =>
    {
        b.ToTable("QualificationMasters");
        b.HasKey(x => x.Id);
    });
}

```
</div>
</div>

---
    
DB上はエンティティであるものをOwned Manyを用いて値オブジェクトとして定義可能
- この値オブジェクトに振る舞いを持たせることはあまりないかもしれないが…
    - 他のプロパティが値オブジェクトになっているときに一貫性が出る
    - 集約的にうれしい（集約ルートからしか制御させない、トランザクションの境界）

---

## 値オブジェクトを利用するときの注意点

### 良い名前を付ける
- 良い名前とは・・・？
- DDDは**業務**の複雑さに向き合うものである
    - 業務エキスパートと**同じ言葉**を使う（ユビキタス言語）
### 同じ言葉（ユビキタス言語）
- 業務エキスパートと、開発と、電話サポートと・・・
すべての関係者が**同じ言葉**を使ってコミュニケーションできるようにする
- フロントエンドとバックエンドでもできる同じ言葉を使うようにする

---

### 文脈を区切る（Bounded Context）
 別のものを同じにしてはいけない
- 文脈によって意味が変わるものは別のものである
    - この時はこのルール、この時はこのルールとすると
    結局密結合・低凝集（論理的凝集）
    
### BAD：違うものを同じところに押し込める

<div class="midium">

```csharp
public enum EmailUsage { Billing, Community }

public sealed record Email(string Value, EmailUsage Usage)
{
    public static Email Create(string value, EmailUsage usage)
    {
        // “メールアドレス”という同じ言葉に、文脈の差分を押し込めている
        return usage switch
        {
            EmailUsage.Billing   => value.EndsWith("@company.com") ? new(value, usage) : throw new Exception("請求文脈の制約NG"),
            EmailUsage.Community => !value.EndsWith("@company.com") ? new(value, usage) : throw new Exception("コミュニティ文脈の制約NG"),
            _ => throw new NotSupportedException()
        };
    }
}

```
</div>

---

### GOOD：区切られた文脈によって、モジュールを分割する

<div class="midium">

```csharp
namespace Billing; // 請求（Billing）という区切られた文脈

public sealed record Email(string Value)
{
    public static Email Create(string value)
        => value.EndsWith("@company.com")
            ? new(value)
            : throw new Exception("請求文脈：社用ドメイン必須");
}
```

```csharp
namespace Community; // コミュニティという区切られた文脈

public sealed record Email(string Value)
{
    public static Email Create(string value)
        => !value.EndsWith("@company.com")
            ? new(value)
            : throw new Exception("コミュニティ文脈：社用ドメイン禁止");
}

```
</div>

--- 

### 注意：形容詞をつけて区別すると、同じ言葉ではなくなる
- 業務エキスパートは、いちいち「請求の」「コミュニティの」アドレスと言うか？

### Warning：形容詞で区別してしまう

<div class="midium">

```csharp
public sealed record BillingEmail(string Value)
{
    public static BillingEmail Create(string value)
        => value.EndsWith("@company.com")
            ? new(value)
            : throw new Exception("請求先は社用メール必須");
}

public sealed record CommunityEmail(string Value)
{
    public static CommunityEmail Create(string value)
        => !value.EndsWith("@company.com")
            ? new(value)
            : throw new Exception("コミュニティは社用メール禁止");
}

```
</div>

---
## 値オブジェクトまとめ

- 値オブジェクトは、ドメイン駆動設計やEFCoreに限らず
オブジェクト指向言語であれば汎用的に利用可能な実装パターンである
- プリミティブ値に固執せず、**型**の情報と**振る舞い**を与え、
業務上意味のある概念として表現する
- 業務上の言葉を**モデル化**する行為ともいえる
