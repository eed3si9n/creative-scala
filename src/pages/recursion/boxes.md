## 箱の線

```tut:invisible
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
```

まずは、[@fig:recursion:sequential-boxes] のように箱を一列並べて描いた例から始めましょう。

![ロイヤルブルーで塗った 5つの箱](./src/pages/recursion/sequential-boxes.pdf+svg){#fig:recursion:sequential-boxes}

まずは手始めに 1つの箱を定義しましょう。

```tut:book
val aBox = Image.rectangle(20, 20).fillColor(Color.royalBlue)
```

1つの箱を一列に並べた場合はこうなります。

```tut:book
val oneBox = aBox
```

2つの箱を隣同士に並べた場合も簡単です。

```tut:book
val twoBoxes = aBox beside oneBox
```

3つの場合も似ています。

```tut:book
val threeBoxes = aBox beside twoBoxes
```

以下、いくつの箱でも作ろうと思えば作ることができます。

これらのイメージを作るのに奇妙な方法だと思ったかもしれません。
例えば、何故以下のように書かないのかと思わなかったでしょうか?

```tut:book
val threeBoxes = aBox beside aBox beside aBox
```

これらの 2つの定義は等価です。
ここでは先にあるイメージを元に後続のイメージを作ることで扱っている構造を強調して、これが構造的再帰への前段階となっています。

このような方法でイメージを書くのは疲れる作業です。
私たちがやりたいのはコンピューターに何らかの方法で描きたい箱の数を伝えることです。
よりテクニカルに言うと、上の式を抽象化したいと言うことができます。
前の章で、メソッドは式を抽象化すると習ったので、この問題を解くのにメソッドを使ってみましょう。

まずはいつも通り、定義したいメソッドの骨組みである、メソッドに入っていくものとメソッドが評価するものから始めましょう。
この場合、私たちは欲しい箱の数として `Int` の `count` を提供して、`Image` を得ます。

```tut:book
def boxes(count: Int): Image =
  ???
```

次に新しいこととして、**構造的再帰**が来ます。
上で `threeBoxes` が `twoBoxes` を使って、`twoBoxes` が `oneBox` を使って定義できることに気づきました。
`box` も、以下のように箱が無い状態を元に定義することができます:

```tut:book
val oneBox = aBox beside Image.empty
```

ここでは、`Image.empty` を使って箱が無い状態を表します。

`boxes` メソッドを既に実装したと想像してください。
もしも正しく実装されたならば、`boxes` は以下の性質を常に保つという事ができるでしょう:

- `boxes(0) == Image.empty`
- `boxes(1) == aBox beside boxes(0)`
- `boxes(2) == aBox beside boxes(1)`
- `boxes(3) == aBox beside boxes(2)`

最後の 3つの性質は全て同じ一般的な形をしています。
`boxes(n) == aBox beside boxes(n - 1)` という 1つの性質を使って、それら全ておよび `n > 0` 全ての場合を記述することができます。

これで、2つの性質だけが残りました。

- `boxes(0) == Image.empty`
- `boxes(n) == aBox beside boxes(n-1)`

これらの 2つの性質は `boxes` の振る舞いを完全に定義します。
実際、これらの性質をコードに変換するだけで `boxes` を実装することができます。

`boxes` の完全な実装は

```tut:book
def boxes(count: Int): Image =
  count match {
    case 0 => Image.empty
    case n => aBox beside boxes(n-1)
  }
```

試してみて、どのような結果が得られるか確かめてください!
この実装は上に書いた性質よりほんの少しだけ冗長ですが、これが私たち最初の自然数の構造的再帰です。

ここで 2つの疑問に答える必要があります。
まず、この `match` 式はどのように動くのでしょう?
このようなメソッドを自分で作れるようになるための原理はあるのでしょうか?
一つづつ疑問に答えます。

### 練習問題: 積み上げられた箱 {-}

`match` 式の詳細に入る前でも `boxes` を改造して [@fig:recursion:stacked-boxes] のようなイメージを作ることができるはずです。

ここでは `match` の構文に慣れるために、`boxes` をコピー・ペーストするのでは無く、練習のために手で書き出してみましょう。

![ロイヤルブルーで塗った 3つの積み上げられた箱](./src/pages/recursion/sequential-boxes.pdf+svg){#fig:recursion:stacked-boxes}

<div class="solution">
`boxes` の `beside` を `above` に変えるだけです。

```tut:book
def stackedBoxes(count: Int): Image =
  count match {
    case 0 => Image.empty
    case n => aBox beside stackedBoxes(n-1)
  }
```
</div>
