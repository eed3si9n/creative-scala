## 高階メソッドと関数

```tut:invisible
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
```

関数は何の役に立つでしょうか?
私たちは既にメソッドを使って再利用可能なコードの断片をパッケージ化して名前を付けることができます。
コードを値として扱うことで他に得られる利点は何でしょう?
先ほど

 - 関数を他の関数やメソッドへパラメータとして渡すこと
 - 戻り値として関数を返すメソッドを定義できること

とは言いましたが、実際にこの機能は使っていません。これらを見ていきましょう。

具体例として、同心円の例題のパターンを考えます:

```tut:silent:book
def concentricCircles(count: Int, size: Int): Image =
  count match {
    case 0 => Image.empty
    case n => Image.circle(size) on concentricCircles(n-1, size + 5)
  }
```

このパターンは、`Image.circle` を別の形に差し替えることで多くの異なるイメージを作ることが可能です。
しかし、`Image.circle` の置き換え提供するたびに私たちは `concentricCircles` の新しい定義を必要とします。

`Image.circle` の代替となるものをパラメータとして渡すことができれば、`concentricCircles` を完全に一般化することができます。
円を描くだけでは無いので、`concentricShapes` と名前を変えて、適当な大きさの形を描くのを `singleShape` に任せることにします。

```tut:silent:book
def concentricShapes(count: Int, singleShape: Int => Image): Image =
  count match {
    case 0 => Image.empty
    case n => singleShape(n) on concentricShapes(n-1, singleShape)
  }
```

これで、同じ `concentricShapes` の定義を再利用してただの円、色々な色の四角形、異なる透明度の円などを描くことができます。
適当な定義の `singleShape` を渡すだけです:

```tut:silent:book
// Passing a function literal directly:
val blackCircles: Image =
  concentricShapes(10, (n: Int) => Image.circle(50 + 5*n))

// Converting a method to a function:
def redCircle(n: Int): Image =
  Image.circle(50 + 5*n) lineColor Color.red

val redCircles: Image =
  concentricShapes(10, redCircle _)
```

### 練習問題 {-}

#### 色と形 {-}

以下のコードから始めて、色と形の関数を書いて [@fig:hof:colors-and-shapes.png] で示したイメージを作ってみましょう。

![色と形](src/pages/hof/colors-and-shapes.pdf+svg){#fig:hof:colors-and-shapes.png}

```tut:silent:book
  def concentricShapes(count: Int, singleShape: Int => Image): Image =
    count match {
      case 0 => Image.empty
      case n => singleShape(n) on concentricShapes(n-1, singleShape)
    }
```

この `concentricShapes` メソッドは以前の練習問題の
`concentricCircles` メソッド同様のものです。
主な違いは、`singleShape` の定義をパラメータとして渡すことです。

この問題について少し考えてみましょう。
私たちは、2つのことをする必要があります:

 1. 3つの目標となるイメージに対してそれぞれ適当な `singleShape` の定義を書くこと

 2. `concentricShapes` にそれぞれ適当な `singleShape` を渡して3回呼び出して、その結果を `beside` を使って並べる

`singleShape` パラメータの定義を注意して見てみましょう。
このパラメータの型は `Int => Image` なので、これは `Int` パラメータを受け取って `Image` を返す関数です。
この型のメソッドは以下のように宣言できます:

```tut:silent:book
def outlinedCircle(n: Int) =
  Image.circle(n * 10)
```

このメソッドを関数へと変換して、`concentricShapes` へ渡して黒い輪郭の同心円を描くことができます:

```tut:silent:book
concentricShapes(10, outlinedCircle _)
```

これは [@fig:hof:colors-and-shapes-step1] で示したものを生成します。

![多くの円の輪郭](src/pages/hof/colors-and-shapes-step1.pdf+svg){#fig:hof:colors-and-shapes-step1}

練習問題の残りは、この関数をコピーして、名前を変えて、期待される色と形の組み合わせを得られるように改造することです:

```tut:silent:book
def circleOrSquare(n: Int) =
  if(n % 2 == 0) Image.rectangle(n*20, n*20) else Image.circle(n*10)

(concentricShapes(10, outlinedCircle) beside concentricShapes(10, circleOrSquare))
```

[@fig:hof:colors-and-shapes-step2] の出力を見てください。

![多くの円と正方形の輪郭](src/pages/hof/colors-and-shapes-step2.pdf+svg){#fig:hof:colors-and-shapes-step2}

ボーナスポイントとして、上の形の例を作れるようになったら、リファクタリングして
色のための関数と形を生成する関数という 2つのベースとなる関数を書いてみましょう。
これらの関数は以下のように**コンビネーター**を使って組み合わせて、コンビネーターの結果を `concentricShapes` へ引数として渡しましょう。

```tut:silent:book
def colored(shape: Int => Image, color: Int => Color): Int => Image =
  (n: Int) => ???
```

<div class="solution">
最もシンプルな解答は、`singleShapes` を以下のように定義することです:

```tut:silent:book
def concentricShapes(count: Int, singleShape: Int => Image): Image =
  count match {
    case 0 => Image.empty
    case n => singleShape(n) on concentricShapes(n-1, singleShape)
  }

def rainbowCircle(n: Int) = {
  val color = Color.blue desaturate 0.5.normalized spin (n * 30).degrees
  val shape = Image.circle(50 + n*12)
  shape lineWidth 10 lineColor color
}

def fadingTriangle(n: Int) = {
  val color = Color.blue fadeOut (1 - n / 20.0).normalized
  val shape = Image.triangle(100 + n*24, 100 + n*24)
  shape lineWidth 10 lineColor color
}

def rainbowSquare(n: Int) = {
  val color = Color.blue desaturate 0.5.normalized spin (n * 30).degrees
  val shape = Image.rectangle(100 + n*24, 100 + n*24)
  shape lineWidth 10 lineColor color
}

val answer =
  (concentricShapes(10, rainbowCircle) beside
   concentricShapes(10, fadingTriangle) beside
   concentricShapes(10, rainbowSquare))
```

しかし、重複したコードがあります。
特に、`rainbowCircle` と `rainbowTriangle` は同じ定義の `color` を用います。
`lineWidth(10)` と `lineColor(color)` も繰り返し呼び出されていますが、重複を避けることができます。
ボーナス解答は、これらを単独の関数へと抽出して、`colored` コンビネーターを使って組み合わせます:

```tut:book
def concentricShapes(count: Int, singleShape: Int => Image): Image =
  count match {
    case 0 => Image.empty
    case n => singleShape(n) on concentricShapes(n-1, singleShape)
  }

def colored(shape: Int => Image, color: Int => Color): Int => Image =
  (n: Int) =>
    shape(n) lineWidth 10 lineColor color(n)

def fading(n: Int): Color =
  Color.blue fadeOut (1 - n / 20.0).normalized

def spinning(n: Int): Color =
  Color.blue desaturate 0.5.normalized spin (n * 30).degrees

def size(n: Int): Double =
  50 + 12 * n

def circle(n: Int): Image =
  Circle(size(n))

def square(n: Int): Image =
  Image.rectangle(2*size(n), 2*size(n))

def triangle(n: Int): Image =
  Image.triangle(2*size(n), 2*size(n))

val answer =
  (concentricShapes(10, colored(circle, spinning)) beside
   concentricShapes(10, colored(triangle, fading)) beside
   concentricShapes(10, colored(square, spinning)))
```
</div>
