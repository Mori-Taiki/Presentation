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

# 事業的優位性の源泉

新規事業を立ち上げるとして、どちらが長期的に儲かりそうか、考えてみてください

1. 単純な業務課題を解決する 
2. 複雑な業務課題を解決する

- 我々が扱うビジネス（業務領域）は往々にして複雑である
    - シンプルな（複雑ではない）課題は参入障壁が低かったり、そもそも解決するまでも無かったりする
    - 困難を解決するからこそ、ビジネス（お金を払ってやってもらいたい）になる

---
# 事業的優位性の源泉
新規事業を立ち上げるとして、どちらが長期的に儲かりそうか、考えてみてください

1. 良く知られた方法で解決できる 
2. 他社とは違う新しい方法でしか解決できない

- 自社で開発する以上は、他に解決できる一般化される方法がないと言える
    - つまり、他社と比較した際の差別化要因が必要

---

## 事業的優位性の源泉
![h:400](syacho.png)

---

# ドメイン駆動設計の戦略

- 業務ロジックの**複雑さ**、競合他社との**差別化**
の2軸のマトリクスで区分分けする

![h:400](image-4.png)


---

# 業務領域の特徴をつかむ　練習問題①

<div class="columns" >
<div class="midium">

## この業務はどの領域か

- Q1-1：あなたは高級料理店の支配人です。
マトリクスの中に業務を振り分けてください
    - A：食材の下ごしらえ
    - B：複雑な給与計算　
    - C：創造的なレシピ開発

## この業務は誰がやるか
- Q1-2：それぞれの業務領域について、
あなたはどんなリソースで解決しますか？
    - A：見習いの新人
    - B：外部の人材（税理士）
    - C：エース料理長

</div>
<div >

![alt text](image-4.png)
![alt text](image-6.png)
</div>
</div>

---

- 独特だがシンプルな作業は、自社の人材の教育の機会とする
- 複雑な領域に自社の高コストなリソースを振り分けるべき
- 複雑だが一般的な課題は外部に解決策がある
![alt text](image-6.png)

---
# 業務領域の特徴をつかむ　練習問題②

<div class="columns" >
<div class="midium">

## この機能はどの領域か
- Q2-1：あなたはECサイトAppのアーキテクトです。
以下の業務をマトリクスの中に振り分けてください
    - A：商品マスタの登録機能
    - B：顧客の趣向に合わせた商品のレコメンド機能
    - C：決済機能

## この機能をどんなリソースで解決するか

- Q2-2：それぞれの業務領域について、
あなたはどんなリソースで解決しますか？
    - A：若手の新人
    - B：エースエンジニア
    - C：外部ライブラリ

</div>
<div >

![alt text](image-5.png)
![alt text](image-7.png)
</div>
</div>

---

# 中核的業務領域に向き合う

## 複雑で差別化要因になる業務領域とは？
例えば、コンクリートに関する業務知識
- 「工場で練り混ぜられてから120分以内に打設されなければならない」
- ただし、「気温が25度を超える場合は90分以内に打設しなければならない」
- 「コンクリートを現場で配合する場合、午前中と午後で表面水量を計測し、
単位水量を規定量内に保たなければならない」

### 業務に固有で、複雑になりがち<br>→ これをシステムで管理・計算して**価値**（面倒ごとの解決）として提供する

---

# **本質的**複雑性に向き合う
<div class="columns" >
<div>

###  本質的複雑性
- 業務領域そのものの複雑性
  - システムの価値そのもの
### 偶発的複雑性
- システム都合の複雑性
  - 永続化（RDB）の都合
  - View（公開するAPI）の都合

## **ドメインモデル**の**戦略**<br>→ 本質的複雑性のみに注力する

</div>
<div>

![alt text](image-15.png)
</div>

---

# アーキテクチャの戦略的選択

![](DDDstrategy.png)

---

### ３つのアーキテクチャの特徴

![](architecture.png)

---

# ドメインモデル

### 値オブジェクト、エンティティ、（そして集約）といった
ドメインモデルを構成する部品  を用いて
# 業務領域を**モデル化**する

---

## モデル化って何？
![alt text](image.png)


---

### Q　なぜモデル化するのか
# A　役に立つ形を模索するため

---

![alt text](image-1.png)

--- 

# ドメインモデルの戦略
<div class="columns">
<div>

ドメインモデルでは業務を**モデル化**してPOCOで業務ロジックを記述する

## POCO：**Plain-Old C# Object** 
### フレームワークやRDBに依存しない<br>**素の**C#オブジェクト
- POCOで業務ロジックを実現できれば、**業務ロジックだけに集中**できる
- モデル化する過程で、
エンジニアの**業務知識が磨かれる**

</div>
<div>

![alt text](image-15.png)
</div>
</div>

--- 

実装する機能がどこに位置するかを考えてアーキテクチャを選定する
→これがドメイン駆動開発の戦略
![alt text](DDDstrategy.png)

---

# アーキテクチャの戦略的選択
ドメインモデルはコストが高いため、
# **中核的な業務領域**で<br>**複雑さに向き合う**ときにのみ利用する

### 補完的な業務領域では、業務ルールがシンプルなので、手続き的な実装で十分
アクティブレコード または **トランザクションスクリプト**を選択する

## 複雑な業務領域でトランザクションスクリプトを使うと<br>**スパゲッティコード**になる

---

<div class="columns" >
<div class="midium">

# 我々の実装はどこに位置する？
### よく見る実装
## テーブル定義を行うエンティティ

``` csharp
public class ConcretePlacement
{
    public int Id { get; set; }

    public DateTime DeliveryTime { get; set; }
    public DateTime PlacementTime { get; set; }
    public decimal Volume { get; set; }
}
```



</div>
<div>

**Controller**また**Service**から呼ばれるクラス
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
--- 
<div class="columns" >
<div>
<br>

# これ、<br>**貧血**ドメインモデル<span class="small">と呼ばれる</span><br>**アンチパターン**です


``` csharp
public class ConcretePlacement
{
    public int Id { get; set; }

    public DateTime DeliveryTime { get; set; }
    public DateTime PlacementTime { get; set; }
    public decimal Volume { get; set; }
}
```

</div><div>

![alt text](image-9.png)
</div>
</div>

--- 

<div class="columns" >
<div class="midium">

# 貧血ドメインモデル

# **振る舞い**を持たないエンティティ

``` csharp
public class ConcretePlacement
{
    public int Id { get; set; }

    public DateTime DeliveryTime { get; set; }
    public DateTime PlacementTime { get; set; }
    public decimal Volume { get; set; }
}
```
- レコードをオブジェクトにしてはいるが、
**手続き的処理**を抜け出せていない
- モデルは**貧血**・処理は**トランザクションスクリプト**
![](hisoka.jpg)
</div>
<div class="small">

### トランザクションスクリプトでは、**複雑さ**に向き合えない
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
# 振る舞いを持つって・・・何？