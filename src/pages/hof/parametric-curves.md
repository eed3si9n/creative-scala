## パラメトリック曲線

```tut:invisible
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
```

今の所、円や長方形といった基本的な形しか作ることができません。
目標である花の形を作るにはより細かいコントロールを必要とします。
数学のツールである**パラメトリック方程式**もしくは**パラメトリック曲線**と呼ばれているものを使うことにします。

パラメトリック方程式はいくつかの入力 (「パラメトリック」という通りパラメーターを用います) から空間内の場所である点を得られる関数のことです。
例えば、円のパラメトリック方程式は `Angle` から点に対するものです。

```scala
def parametricCircle(angle: Angle): Point =
  ???
```

これらの点を小さなドットや他の形でプロットすることで私たちが描こうとする大きな形を作ることができます。

[@fig:hof:parametric-circles] では、円のパラメトリック関数によって生成された点で小さな円を描いた場合の例です。
左から右にかけて 90度、45度、22.5度おきに点を描いています。
より多くの点を描くことで、形の輪郭である大きな円がハッキリとしてくることが分かります。

![左から右にかけて 90度、45度、22.5度おきに点を描画したパラメトリックな円](src/pages/hof/parametric-circles.pdf+svg){#fig:hof:parametric-circles}

パラメトリック曲線を作るには Dooble が点をどう表すのかを習い、イメージを空間の特定の点にレイアウトして、高校以来忘れていた幾何学を復習する必要があります。

## 点

Doodle では、`Point` 型を用いて 2次元内の場所を表します。これは 2つの等価な表し方があって、

- デカルト座標と呼ばれる x と y 座標を使った方法と
- 極座標と呼ばれる角度と、その角度における原点からの距離 (半径) を使って表すことができます。

私たちは `Point.cartesian` を使ってデカルト座標で点を作るか、`Point.polar` を使って極座標で点を作ることができます。以下の表は `Point` の主なメソッドを示します。

----------------------------------------------------------------------------------------------------------
演算                                 型      説明                          例
----------------------------------- ------- ---------------------------- ---------------------------------
`Point.cartesian(Double, Double)`   `Point` デカルト座標を使って `Point`     `Point.cartesian(1.0, 1.0)`
                                            を作る。

`Point.polar(Double, Angle)`        `Point` 極座標を使って `Point`         `Point.polar(1.0, 90.degrees)`
`Point(Double, Angle)`                      を作る。

`Point.zero`                        `Point` 原点での `Point`               `Point.zero`
                                            を作る (x と y ともにゼロ)

`Point.x`                           `Double` `Point` の x 座標を得る。      `Point.zero.x`

`Point.y`                           `Double` `Point` の y 座標を得る。      `Point.zero.y`

`Point.r`                           `Double` `Point` の半径を得る。         `Point.zero.r`

`Point.angle`                       `Angle`  `Point` の角度を得る。         `Point.zero.angle`
-----------------------------------------------------------------------------------------------------------


## 柔軟なレイアウト

`Image` を特定の点に置くことはできるでしょうか?
これまでの所は、イメージを `on`、`beside`、`above` のみを使って配置してきました。
さらに `at` メソッドというツールを使った、より柔軟なレイアウトが必要になります。
正方形の 4隅で円を描く例です:

```tut:silent:book
val dot = Image.circle(5).lineWidth(3).lineColor(Color.crimson)
val squareDots =
  dot.at(0, 0).
    on(dot.at(0, 100)).
    on(dot.at(100, 100)).
    on(dot.at(100, 0))
```

これは [@fig:hof:square-dots] で示したイメージを作ります。

![`at` レイアウトを使って 4つのドットを正方形の 4隅に配置したもの](src/pages/hof/square-dots.pdf+svg){#fig:hof:square-dots}

`at` レイアウトをどう使うのか、また何故ドットを `on` で積み上げる必要があるのかを理解するためには Doodle がレイアウトをどのように行っているのかを理解する必要があります。

Doodle において全ての `Image` は**原点** (origin) を持ちます。
ほとんどのイメージの場合はこれはイメージの中央にありますが、そうである必要はありません。
Doodle が複合 `Image` をレイアウトするときは、原点を合わせています。
例えば、複数の `Image` を `above` を使ってレイアウトする場合、それらの原点は垂直に並び、複合 `Image` の原点も原点を結んだ線の中点となります。
[@fig:hof:horizontal-layout] には、`beside` を使ったレイアウトにおいて原点 (赤い円) がどう並ぶのかを示した例があります。
そして、`on` を使った場合は原点は積み上げられていくため、事実上複数のイメージが同じ原点を持つことになります。

![水平レイアウト (`beside`) を使った例で、原点が並ぶことを示す。](src/pages/hof/horizontal-layout.pdf+svg){#fig:hof:horizontal-layout}

`at` を使うことで、`Image` をその原点から移動させることができます。
ここで見ていく例では、全ての要素が同じ原点を共有してほしいので、`at` でイメージを動かした後で `on` を使ってイメージを組み合わせます。

`at` を呼ぶしには 2通りの方法があります:

- `dot.at(100, 100)` といった風に x と y のオフセットを渡します。
- `dot.at(Vec(100, 100))` のように、オフセットを含んだ `Vec` (ベクトル)  を渡します。

`toVec` を使って `Point` を `Vec` へと変換することができます。

```tut:book
Point.cartesian(1.0, 1.0).toVec
```

## 幾何学

もう一つの基礎となるのは、幾何学を用いた点の位置づけです。
ある点が原点から `r` の距離、角度 `a` の位置にあるとき、x と y 座標はそれぞれ `(a.cos) * r`、`(a.sin) * r` となります。
もしくは、極座標を使ってそれを表します!

```tut:book
val polar = Point.polar(1.0, 45.degrees)
val cartesian = Point.cartesian((45.degrees.cos) * 1.0, (45.degrees.sin) * 1.0)

// これらは同じです
polar.toCartesian == cartesian
cartesian.toPolar == polar
```

## 合わせて一緒に

これらを組み合わせてパラメトリックな円を作ることができます。
デカルト座標を使った半径 200 のパラメトリックな円のコードは以下のようになります:

```tut:silent:book
def parametricCircle(angle: Angle): Point =
  Point.cartesian(angle.cos * 200, angle.sin * 200)
```

極座標の場合はシンプルにこんなふうになります:

```tut:silent:book
def parametricCircle(angle: Angle): Point =
  Point.polar(200, angle)
```

そして円上の点を均一にサンプリングします。イメージを作るには、それらの点上に何か (例えば三角形) を描画します。

```tut:silent:book
def sample(start: Angle, samples: Int): Image = {
  // Angle.one is one complete turn. I.e. 360 degrees
  val step = Angle.one / samples
  val dot = triangle(10, 10)
  def loop(count: Int): Image = {
    val angle = step * count
    count match {
      case 0 => Image.empty
      case n =>
        dot.at(parametricCircle(angle).toVec) on loop(n - 1)
    }
  }
  
  loop(samples)
}
```

これは、多分もう慣れ親しんだ構造的再帰となっています。

これを描くと、多くの三角形が円状に並んでいるのが見えるようになります。
例えば [@fig:hof:triangle-circle] は `sample(0.degrees, 72)` の結果を示しています。

![`sample` を使って円状に配置された多くの三角形](src/pages/hof/triangle-circle.pdf+svg){#fig:hof:triangle-circle}

### 花

花を作るための次のステップは、円よりも面白い形を使うことです。これは `parametricCircle` をより面白い方程式に変えることを意味します。
例えば、以下の `rose` を見てみましょう。
これは、最大半径が 200 のバラの曲線です。
角度に掛ける値 (下では `7`) を変えることで別の形を得ることができます。

```tut:silent:book
// Parametric equation for rose with k = 7
def rose(angle: Angle) =
  Point.polar((angle * 7).cos * 200, angle)
```

高密度でサンプリングされた例を [@fig:hof:rose] に示します。

![バラ曲線の例](src/pages/hof/rose.pdf+svg){#fig:hof:rose}

`sample` を変えて `parametricCircle` の代わりに `rose` を呼び出すことも可能ですが、それは少し満足がいきません。
異なるパラメトリック方程式を使って実験してみたいとしたらどうでしょうか?
点を作るメソッド (つまりパラメトリック方程式) を `sample` へのパラメータとして渡せれば便利です。
これはできるでしょうか?
そのためには、以下ができる必要があります:

- メソッドパラメータとしてのメソッドの型を書くことができる
- メソッドの呼び出し (例えば `rose(0.degrees)`) とメソッドそのものへの参照を区別できる

まずは 2つ目の問題から見ていきましょう。メソッドを呼び出さずに参照するとエラーとなります。

```tut:fail:book
rose
```

便利なことに、何をすれば良いのかはエラーメッセージが教えてくれていて、ここでやっと関数を紹介することができます。
