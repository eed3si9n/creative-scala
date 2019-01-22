## 抽象化

```tut:invisible
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
```

前の節では名前について色々習いました。
気取ったプログラマー用語を使うと、**名前は式を抽象化する**と言えます。
これは、名前の定義することの本質を一言で言い表しているけども、ちょっと用語を解読してみよう。

抽象化とは、要らない詳細を取り除くという意味です。
例えば、数は抽象化の 1つです。
「1」という数の純粋な概念は自然界には存在しません。
それはいつも 1つのリンゴや 1冊の Creative Scala の本といった具合に 1つの物です。
算数を行うときに、数という概念は何を数えているのかという要らない詳細を抽象化して数だけを操作することができます。

同様に、名前は式を代理します。
式は値をどのように構築するかを教えてくれます。
その値に名前があれば、その値がどのように構築されるかは知らなくてもよくなります。
式は任意の複雑さを持つことができますが、名前を使うことでどれだけ複雑なのかを気にする必要が無くなります。
これが、名前は式を抽象化すると言ったときの意味です。
いつでも式が出てきたら、それは同じ値を持つ名前に置き換えることができます。

抽象化はコードを読み書きしやすくします。
[@fig:programs:sequential-boxes] のように箱の列を作る具体例を用いて説明しましょう。

![ロイヤルブルーで塗った 5つの箱](./src/pages/programs/sequential-boxes.pdf+svg){#fig:programs:sequential-boxes}

この絵を作る単一の式を書くことは可能です。

```tut:silent:book
(
  Image.rectangle(40, 40).
    lineWidth(5.0).
    lineColor(Color.royalBlue.spin(30.degrees)).
    fillColor(Color.royalBlue) beside
  Image.rectangle(40, 40).
    lineWidth(5.0).
    lineColor(Color.royalBlue.spin(30.degrees)).
    fillColor(Color.royalBlue) beside
  Image.rectangle(40, 40).
    lineWidth(5.0).
    lineColor(Color.royalBlue.spin(30.degrees)).
    fillColor(Color.royalBlue) beside
  Image.rectangle(40, 40).
    lineWidth(5.0).
    lineColor(Color.royalBlue.spin(30.degrees)).
    fillColor(Color.royalBlue) beside
  Image.rectangle(40, 40).
    lineWidth(5.0).
    lineColor(Color.royalBlue.spin(30.degrees)).
    fillColor(Color.royalBlue)
)
```

このコードの中に内在するシンプルなパターンがありますが、それが見づらくなっています。
初見で全ての長方形が同じものであることが分かるでしょうか?
基本となる箱のコードに名前を付けるという抽象化を行うことでコードの見通しが良くなります。

```tut:silent:book
val box =
  Image.rectangle(40, 40).
    lineWidth(5.0).
    lineColor(Color.royalBlue.spin(30.degrees)).
    fillColor(Color.royalBlue)

box beside box beside box beside box beside box
```

これでどうやって箱が作られているのかと、絵の中で箱が 5回繰り返されているのが分かりやすくなりました。

### 練習問題 {-}

#### 再びアーチェリー {-}

前の章のアーチェリーのターゲットに戻ってみてみましょう。 [@fig:programs:target3] を見てください。

![アーチェリーのターゲット](./src/pages/programs/target3.pdf+svg){#fig:programs:target3}

前回イメージを作ったときは値に名前をつける方法を知らなかったので、1つの大きい式を書きました。
今回は、イメージの部品にそれぞれ名前をつけて、イメージがどのように構築されているのか他の人が分かりやすいようにしてください。
どの部分に名前を付けるべきで、他の部分は名前をつけるに足らないという判断は自分のセンスで行う必要があります。

<div class="solution">
私たちは、以下のようにターゲット、スタンド、地面を分けて名前を付けました。
これで最終的なイメージがどう構築されているのかの見通しが良くなったと思います。
私たちの考えでは、これ以上細かく部品に名前をつけても役に立たないと思いました。

```tut:silent:book
val coloredTarget =
  (
    Image.circle(10).fillColor(Color.red) on
      Image.circle(20).fillColor(Color.white) on
      Image.circle(30).fillColor(Color.red)
  )

val stand =
  Image.rectangle(6, 20) above Image.rectangle(20, 6).fillColor(Color.brown)

val ground =
  Image.rectangle(80, 25).lineWidth(0).fillColor(Color.green)

val image = coloredTarget above stand above ground
```
</div>


#### 一丁先を行く {-}

より実践的な名前の使い方として、[@fig:programs:street] のような町並みの 1シーンを作ってみよう。
イメージの部品に名前を付けることによってかなりの繰り返しを省けるはずです。

![町並みの風景](./src/pages/programs/street.pdf+svg){#fig:programs:street}

<div class="solution">
これが私たちの解答です。
見ての通り、風景を小さな部品に分けることで比較的小さなコードに収めることができました。

```tut:silent:book
val roof = Image.triangle(50, 30) fillColor Color.brown

val frontDoor =
  (Image.rectangle(50, 15) fillColor Color.red) above (
    (Image.rectangle(10, 25) fillColor Color.black) on
    (Image.rectangle(50, 25) fillColor Color.red)
  )

val house = roof above frontDoor

val tree =
  (
    (Image.circle(25) fillColor Color.green) above
    (Image.rectangle(10, 20) fillColor Color.brown)
  )

val streetSegment =
  (
    (Image.rectangle(30, 3) fillColor Color.yellow) beside
    (Image.rectangle(15, 3) fillColor Color.black) above
    (Image.rectangle(45, 7) fillColor Color.black)
  )

val street = streetSegment beside streetSegment beside streetSegment

val houseAndGarden =
  (house beside tree) above street

val image = (
  houseAndGarden beside
  houseAndGarden beside
  houseAndGarden
) lineWidth 0
```
</div>
