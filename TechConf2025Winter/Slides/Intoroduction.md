---
marp: true
header: "“シン”我々はなぜEFCoreを使うのか ～DDDとEFCoreから考える値オブジェクトのすゝめ～"
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

# “シン”我々はなぜEFCoreを使うのか 
～DDDとEFCoreから考える値オブジェクトのすゝめ～

<div class="columns">
<div>

### この番組は、ご覧の本の影響で、<br>お送りいたします。
 ![w:250](../image.png)  ![w:250](../image%201.png) 

</div>
<div class="large">

### 振り返り
- EFCoreを利用するのはDB中心から<br>**オブジェクト中心**への移行が目的
- オブジェクト中心にすることで、<br>より複雑なエンティティの関係性を記述しやすくなる
- しかし、さらに複雑な**業務ロジック**が存在するプロダクトでは、エンティティ同士の関係をオブジェクトとして記述するだけでは不十分である
</div>
</div>

---

# メロスは激怒した、
## 必ず、かの邪知暴虐の**貧血ドメインモデル**を除かねばならなぬと決意した。
メロスにはCI/CDが分からぬ、モダンなフロントエンドも、機械学習もわからぬ。
メロスは、SES村のIT土方である。Excelでテスト設計書を書き、E2Eテストの結果をスクリーンショットして、DBダンプをExcelに添付して暮らしてきた。
# けれども、**スパゲッティコードに向かう匂い**に対しては、人一番に敏感であった。

---

# 自己紹介
## 森 大樹 （もり　たいき）
施工管理クラウド①　品質管理クラウド[コンクリート]　福岡オフィス
### 経歴
- 新卒で人材派遣の営業：２年半（2020年4月～2022年9月） 
- SESで3つの現場を経験：２年半（2022年10月～2025年4月）
言語：Javaメイン　/　フレームワーク：Spring・Seaser2 Backbone.jsなど
- KENTEMに入社：2025年5月～
BEエンジニア

---

# 楽しい仕事
- 皆さんは、コードを書いているときと、ドキュメントを整理しているとき、
どっちが楽しいですか？
- 以下のような仕事をしてみたいと思いますか？
    - ほとんどコードをそのまま書き写したようなExcelの設計書を書く、
    細かい体裁（インデントや差分の文字色指定）を指摘されながらメンテする
    - 影響範囲になりそうなキーワードをGrep検索して書き出し、すべての行に対して変更の要否を書き出してレビュー依頼する。（レビュー依頼される）
    - 変更する予定のソースから、画面の影響箇所までの呼び出しツリーをすべて書き出す。（そして判定条件網羅のE2Eテストを画面から行う。）

---
<div class="columns">
<div class="center">
<br>
<br>
<br>
<br>
やりたくないですよね？<br>
でも、前職では<br>
つらい仕事ばっかりさせらたので・・・
</div>
<div>

![h:480](../%E3%82%B7%E3%82%B0%E3%83%9E.jpg)
</div>
</div>

--- 
<div class="columns">
<div>

KENTEMにやってきて…
![w:360](../%E4%BF%BA%E3%81%AF%E5%BC%B1%E3%81%84.jpg)
</div>
<div>

### もう一つは…
![w:480](../aki.jpg)

レガシーコードに親しんだ私だからこそ
嫌な未来が想像されるときがあります

</div>
</div>

--- 

## なぜ面倒な仕事が生まれたか

事業的に成功し、長期に稼働し続けるシステムのコード
　≒レガシーコードの現場では、たいてい面倒な仕事がつきもの
<div class="columns" >
<div class="midium">

成功した事業のシステムの特徴
- 事業が成功すればするほど、事業のルールが次々に変更・追加される

- 成功した事業は長く続き、開発メンバーの入れ替えが必ず発生し、業務ルールの暗黙知が必ず喪失する

- はじめはシンプルな機能でも、局所最適解的にスピードとコスト最小を求めた改修（経営判断としては正しい）を行ううちに、大規模で複雑になっていく

</div>
<div class="midium">

成功した事業のシステム「**開発**」の特徴
- 複雑化した（そして名無しの）業務ロジックをコードから読み取ろうとすると認知負荷が高い
→仕様のドキュメント化（Wikiや設計書）
- 業務ロジックが凝集せず、あちこちに散らばっている
→変更箇所の漏れが多発し、力業でマネジメントする
- スピード重視で場当たり的な実装を行ったことで業務ロジックがカオス化
    →変更の副作用を担保するために、すべての流れをトレースする必要
</div>
</div>
先に述べた面倒な仕事は、極めて合理的なマネジメント戦略であるともいえます

--- 



## どのように面倒な仕事を回避できそうか

<div class="midium">

### 課題①：複雑化した（そして名無しの）業務ロジックをコードから読み取ろうとすると認知負荷が高い<br>→仕様のドキュメント化（Wikiや設計書）へ逃げる

- ビジネスの知識を（ドキュメントではなく）コードで雄弁に表現する
    - ドキュメントやコード内コメントはメンテナンスコストが高く、劣化コピーになりがち。
    - コードこそが仕様を表現する唯一の真実です。

### BAD

```csharp
// 120って何？単位は？何の制約？
if ((placedAt - shippedAt) > 120) throw new Exception("NG");
```

### GOOD

```csharp
// 配送時間クラスのファクトリメソッド内でチェック
var time = TransportTime.From(shippedAt, placedAt); // 120分超ならここで失敗（仕様が型にある）
// 必ず正しい状態のオブジェクトが生成される
```

</div>

--- 

### 課題②：業務ロジックが凝集せず、あちこちに散らばっている

→変更箇所の漏れが多発し、力業でマネジメントするしかなくなる

- 業務のルールを凝集させ、適切にカプセル化する
    - 一つのルールは一つの個所にだけ実装すればよいようにする
    - それ以外の場所からは触らせないように守る

<div class="columns">
<div class="midium">

### BAD

```csharp
// API POST: 120分チェック
if ((placedAt - shippedAt) > 120) return BadRequest();
・・・

// API PUT: 120分チェック
if ((placedAt - shippedAt) > 120) return BadRequest();
・・・

// Batch: 120分チェック
status = (placedAt - shippedAt) <= 120 ? "OK" : "NG";
・・・
```
</div>
<div class="midium">

### GOOD

```csharp
// どのAPIでもBatchでも、TransportTime型を利用する時点でチェックが適用される
var time = TransportTime.From(shippedAt, placedAt);
```
</div>
</div>

---

### 課題③：業務の本質を理解せず、場当たり的な実装を行ったためにロジックがカオス化する
→変更の副作用を担保するために、すべての流れをトレースする必要がある

###  偶発的複雑性を排除し、本質的複雑さにのみフォーカスする
1. 偶発的複雑性：コードの都合で生まれる複雑性
    - フレームワークの都合が入り込んで複雑になる、通信の都合で複雑になる
    説明が十分でない命名のせいで複雑になる
    - システムの価値に影響しない複雑性です
    
2. 本質的複雑性：システム化の対象とする業務ルールがそもそも複雑
    - こちらは仕方のないこと（というかシステムの提供する価値そのもの）です

---
### 課題③：業務の本質を理解せず、場当たり的な実装を行ったためにロジックがカオス化する
→変更の副作用を担保するために、すべての流れをトレースする必要がある

<div class="columns" >
<div class="midium">

### Bad

```csharp
public async Task PlaceAsync(Guid id, DateTime placedAt)
{
    var e = await db.Deliveries.FindAsync(id);

    // DB列（int）を直接いじる＝“仕様”がどこにもいない
    e.TransportMinutes = (int)(placedAt - e.ShippedAt).TotalMinutes;

    // あちこちにifが増える（例外条件が増えるたびに追記）
    if (e.TransportMinutes > 120 && e.CustomerCode != "A") throw new Exception("NG");

    await db.SaveChangesAsync();
}
```
</div>
<div class="midium">

### Good

```csharp
public class ConcreteDelivery
{
    public Guid Id { get; private set; }
    public DateTime ShippedAt { get; private set; }
    public TransportTime TransportTime { get; private set; } = default!;

    private ConcreteDelivery() { } // EF用

    public ConcreteDelivery(Guid id, DateTime shippedAt)
    {
        Id = id;
        ShippedAt = shippedAt;
    }

    public void MarkPlaced(DateTime placedAt)
        => TransportTime = TransportTime.From(ShippedAt, placedAt); // 不正なら生成できない
}

// DbContext
modelBuilder.Entity<ConcreteDelivery>()
    .ComplexProperty(x => x.TransportTime, ct =>
        ct.Property(p => p.Minutes).HasColumnName("TransportMinutes"));
```
</div></div>


---

## つまり・・・？

# 将来の自分のためにちゃんと自己文書化されたコードになるように設計しよう
というモチベーションが湧いてきます（動けばいいわけではない）

　湧いてきましたよね？その体で進めていきます。

---

でも、リファクタリングって、やらせてもらえます？
その価値を上司やほかのメンバーに説明できますか？

そのために必要な考え方こそ **ドメイン駆動設計**です
# ドメイン駆動設計 とは
# **事業価値から逆算して、設計判断を行う**ための知見
