## 練習問題

```tut:invisible
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
```

関数の知識をいっぱい得られたので、花を描く問題へと戻ってみます。
今回は以前よりも多くのデザイン作業を行ってもらいます。

花を描くタスクを小さい関数へと分解して組み合わせるのが目標です。
個々の構成要素を関数へと分けることで、より広い創造的自由度が得られるようにしてください。

まずは、自分でこの作業を行ってみてください。詰まったら、以下に私たちが行った分解方法を参照してください。

### 構成要素

私たちは、花の描画の 2つの構成要素を同定しました:

- パラメトリック方程式
- 角度に対する構造的再帰

関数へと抽象化可能な他の構成要素は何でしょう? それらの型は何でしょうか? (これは、意図的に自由回答となっています。研究してください!)

<div class="solution">
パラメトリック曲線を描くとき、異なる曲線の半径を変えたいと思うでしょう。
これは関数へと抽象化できます。
この関数の型はどうあるべきしょうか?
最も明白な方法は `(Point, Double) => Point` で、`Double` パラメータが点をスケールする量とします。
しかし、これは初期値から変わらない `Double` を渡しまわる必要があり、使いづらいものです。

より良い構造は、`Double => (Point => Point)` の型を持つ関数を作ることです。
この関数は、スケール係数を受け取ります。
これは、スケール係数に基づいた `Point` の変換関数を返します。
こうすることで、固定したスケール値を分けることができます。実装は以下のようになります

```tut:silent:book
def scale(factor: Double): Point => Point = 
  (pt: Point) => {
    Point.polar(pt.r * factor, pt.angle)
  }
```

以前に、パラメトリック方程式を `sample` から抽象化したいと言ったと思います。
これは、以下のように簡単に行うことができます。

```tut:invisible
def parametricCircle(angle: Angle): Point =
  Point.cartesian(angle.cos, angle.sin)
```

```tut:silent:book
def sample(start: Angle, samples: Int, location: Angle => Point): Image = {
  // Angle.one is one complete turn. I.e. 360 degrees
  val step = Angle.one / samples
  val dot = triangle(10, 10)
  def loop(count: Int): Image = {
    val angle = step * count
    count match {
      case 0 => Image.empty
      case n =>
        dot.at(location(angle).toVec) on loop(n - 1)
    }
  }
  
  loop(samples)
}
```

イメージで使うプリミティブ図形の選択 (`dot` か `Image.triangle`) を抽象化したいと思うかもしれません。
`location` を `Angle => Image` 関数へと変えることでこれが可能となります。

```tut:silent:book
def sample(start: Angle, samples: Int, location: Angle => Image): Image = {
  // Angle.one is one complete turn. I.e. 360 degrees
  val step = Angle.one / samples
  def loop(count: Int): Image = {
    val angle = step * count
    count match {
      case 0 => Image.empty
      case n => location(angle) on loop(n - 1)
    }
  }
  
  loop(samples)
}
```

構造的再帰のコードから問題に特定な部分を抽象化することができます。
今までは

```scala
def loop(count: Int): Image = {
  val angle = step * count
  count match {
    case 0 => Image.empty
    case n => location(angle) on loop(n - 1)
  }
}
```

でしたが、基底ケース (`Image.empty`) と問題に特定な再帰の部分 (`location(angle) on loop(n - 1)`) を抽出することができます。基底ケースはただの `Image` ですが、再帰の部分は `(Angle, Image) => Image` 型となります。最終結果は以下のようになります。

```tut:silent:book
def sample(start: Angle, samples: Int, empty: Image, combine: (Angle, Image) => Image): Image = {
  // Angle.one is one complete turn. I.e. 360 degrees
  val step = Angle.one / samples
  def loop(count: Int): Image = {
    val angle = step * count
    count match {
      case 0 => empty
      case n => combine(angle, loop(n - 1))
    }
  }
  
  loop(samples)
}
```

これは、非常に抽象的な関数です。この抽象化に気づく人は多くないと私たちは思っていますが、この考え方に興味があれば、畳み込みやモノイドについて調べてみてください。
</div>


### 組み合わせ

構成要素の分解ができたら、次はそれらを組み合わせて面白い結果を作ることができます。やってみましょう。

<div class="solution">
これは回答の一例です。

```tut:silent:book
def parametricCircle(angle: Angle): Point =
  Point.cartesian(angle.cos, angle.sin)
  
def rose(angle: Angle): Point =
  Point.cartesian((angle * 7).cos * angle.cos, (angle * 7).cos * angle.sin)

def scale(factor: Double): Point => Point = 
  (pt: Point) => {
    Point.polar(pt.r * factor, pt.angle)
  }

def sample(start: Angle, samples: Int, location: Angle => Point): Image = {
  // Angle.one is one complete turn. I.e. 360 degrees
  val step = Angle.one / samples
  val dot = triangle(10, 10)
  def loop(count: Int): Image = {
    val angle = step * count
    count match {
      case 0 => Image.empty
      case n => dot.at(location(angle).toVec) on loop(n - 1)
    }
  }
  
  loop(samples)
}

def locate(scale: Point => Point, point: Angle => Point): Angle => Point =
  (angle: Angle) => scale(point(angle))

// Rose on circle
val flower = {
  sample(0.degrees, 200, locate(scale(200), rose _)) on
  sample(0.degrees, 40, locate(scale(150), parametricCircle _)) 
}
```
</div>


### 実験

色々実験して、創造的な可能性を探ってみましょう!

<div class="solution">
[@fig:hof:flower-power] を作った実装は [Flowers.scala](https://github.com/underscoreio/doodle/blob/develop/shared/src/main/scala/doodle/examples/Flowers.scala) に置いてあります。あなたは、どのような花を描いたでしょうか? 教えてください! 私たちの email アドレスは `noel@underscore.io` と `dave@underscore.io` です。
</div>
