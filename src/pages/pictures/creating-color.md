## 色の作成

```tut:invisible
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
```

ここまでで、イメージの中で定義済みの色を使う方法を見てきました。独自の色を作りたいとしたらどうすればいいでしょう? この節では、独自の色を作ったり、既存の色を変換して新しいものにする方法を解説します。

### RGB カラー

コンピューターは、異なる量の赤、緑、青成分を混ぜて幅広い色を再現することで色を取り扱います。この RGB モデルは色の[加法混合][additive-model]の一種です。赤、緑、青の成分はそれぞれ 0 から 255 の値を持つことができます。3つ全ての成分が最大値の 255 であるときは、純粋な白を得ることができます。全ての成分が 0 のときは黒となります。

`Color` オブジェクトの `rgb` メソッドを使って独自の RGB カラーを作ることができます。このメソッドは、赤、緑、青成分の 3つのパラメータを取ります。これらは `UnsignedByte`[^byte] と呼ばれる 0 から 255 の数です。`UnsignedByte` には `Int` のようなリテラル式が無いため、`Int` から `UnsignedByte` へと変換する必要があります。これは、`uByte` メソッドを使って行うことができます。`Int` は `UnsignedByte` よりも多くの値を取ることができるので、もしも数が `UnsignedByte` で表現するには小さすぎたり大きすぎる場合は 0 から 255 の範囲で最も近い値に変換されます。具体例で説明します。

```tut:book
0.uByte
255.uByte
128.uByte
-100.uByte // 小さすぎるので、0 に変換する
1000.uByte // 大きすぎるので、255 に変換する
```

(`UnsignedByte` は Doodle の機能で、Scala が提供するものではないことに注意してください)

`UnsignedByte` の作り方が分かったところで、RGB カラーを作ってみましょう。

```tut:silent:book
Color.rgb(255.uByte, 255.uByte, 255.uByte) // White
Color.rgb(0.uByte, 0.uByte, 0.uByte) // Black
Color.rgb(255.uByte, 0.uByte, 0.uByte) // Red
```

### HSL カラー

RGB カラー表現は取り扱いが簡単ではありません。色相、彩度、明度 (HSL) のフォーマットの方が、私たちが色を認知するのにより近い形でモデル化されています。この表現法では色は以下の成分を持ちます:

- **色相** (hue): 色相環上の位置を 0 から 360度の角度で表したもの。
- **彩度** (saturation): 単調なグレーから純粋な色までの色の強度を 0 から 1 までの数で表したもの。
- **明度** (lightness): 黒から純白までの明るさを 0 から 1 までの数で表したもの。

[@fig:pictures:color-wheel] は色相と明度を変えることでどう色が変化するかを示し、[@fig:pictures:saturation] は彩度の変化の影響を示します。

![彩度を 1 に固定して、色相 (角度) と明度 (中心からの距離) の変化を示した色相環。](src/pages/pictures/color-wheel.pdf+svg){#fig:pictures:color-wheel}

![色相と明度を固定することで彩度の変化が色に与える影響を示した勾配。左端は彩度が 0 で右端が彩度が 1 となっている。](src/pages/pictures/saturation.pdf+svg){#fig:pictures:saturation}

`Color.hsl` メソッドを使って HSL 表現の色を作ることができます。このメソッドは、色相、彩度、明度をパラメータとして受け取ります。色相は `Angle` で、`degrees` メソッド (か `radians` メソッド) を用いて `Double` を `Angle` へと変換することができます。

```tut:book
0.degrees
180.degrees
3.14.radians
```

彩度と明度は 0.0 から 1.0 へと標準化されます。`.normalized` メソッドを使って `Double` を標準化された値へと変換します。

```tut:book
0.0.normalized
1.0.normalized
1.2.normalized // Too big, is clipped to 1.0
-1.0.normalized // Too small, is clipped to 0.0
```

これで HSL 表現を使って色を作ることができます。

```tut:silent:book
Color.hsl(0.degrees, 0.8.normalized, 0.6.normalized) // A pastel red
```

この色を見るためには、絵の中で使ってみてください。例えば、[@fig:pictures:triangle-pastel-red] を見てください。

![パステルレッドを用いた三角形のレンダリング](./src/pages/pictures/triangle-pastel-red.pdf+svg){#fig:pictures:triangle-pastel-red}


### 色の操作

構図の効果は、実際に使われた色そのものだけではなく、色と色の間の関係よることが多いと思います。既存の色から新しい色を作ることができるメソッドがいくつかあります。特によく使われるのは:

- `spin` は色相を `Angle` の量だけ回転します。
- `saturate` と `desaturate` はそれぞれ彩度に対して `Normalized` 値を加算したり減算したりします。
- `lighten` と `darken` はそれぞれ明度に対して `Normalized` 値を加算したり減算したりします。

具体例で解説すると

```tut:silent:book
((circle(100) fillColor Color.red) beside
  (circle(100) fillColor Color.red.spin(15.degrees)) beside
    (circle(100) fillColor Color.red.spin(30.degrees))).lineWidth(5.0)
```

は [@fig:pictures:three-circles-spin] を生成します。

![Color.red から始まり、色相を 15度づつ回転させた 3つの円](./src/pages/pictures/three-circles-spin.pdf+svg){#fig:pictures:three-circles-spin}

次は似た例ですが、[@fig:pictures:saturate-and-lighten] のように彩度と明度を操作します。

```tut:silent:book
(((circle(20) fillColor (Color.red darken 0.2.normalized))
  beside (circle(20) fillColor Color.red)
  beside (circle(20) fillColor (Color.red lighten 0.2.normalized))) above
((rectangle(40,40) fillColor (Color.red desaturate 0.6.normalized))
  beside (rectangle(40,40) fillColor (Color.red desaturate 0.3.normalized))
  beside (rectangle(40,40) fillColor Color.red)))
```

![上の 3つの円は明度の変化を示し、下の 3つの四角形は彩度の変化を示す](./src/pages/pictures/saturate-and-lighten.pdf+svg){#fig:pictures:saturate-and-lighten}

[^byte]: 1バイトは 256通りの可能性を持つ値で、コンピューター内の 8ビットを用いて表現する。符号付きバイトは -128 から 127 の整数値を持ち、符号なしバイトは 0 から 255 の値を持つ。

### 透明度

**アルファ**値を与えることで、色に透明度を追加することができます。アルファ値 0.0 は完全な透明な色を表し、アルファ値 1.0 は完全に不透明な色を表します。`Color.rgba` と `Color.hsla` は、`Normalized` のアルファ値を表す 4つめのパラメータを受け取ります。既にある色に `alpha` メソッドを呼ぶことで異なる透明度を持つ新しい色を作ることができます。例えば、[@fig:pictures:rgb-alpha] のようになります。

```tut:silent:book
((circle(40) fillColor (Color.red.alpha(0.5.normalized))) beside
 (circle(40) fillColor (Color.blue.alpha(0.5.normalized))) on
 (circle(40) fillColor (Color.green.alpha(0.5.normalized))))
```

![アルファ値 0.5 の円を使って透明度を表す](./src/pages/pictures/rgb-alpha.pdf+svg){#fig:pictures:rgb-alpha}


### 練習問題 {-}

#### 類似色の三角形 {-}

3つの三角形を、三角に配置して、類似色で色付けしてみよう。類似色とは色相の近い色のことです。(ちょっと手のこんだ) 具体例 [@fig:pictures:complementary-triangles] を見てください。

![類似色の三角形。色は `darkSlateBlue` のバリエーションとして選ばれています。](./src/pages/pictures/complementary-triangles.pdf+svg){#fig:pictures:complementary-triangles}

<div class="solution">
回答を console に書くにはちょっと長くなりすぎてきていますね。次にその対策を見ていきます。

```tut:book
((triangle(40, 40)
       lineWidth 6.0
       lineColor Color.darkSlateBlue
       fillColor (Color.darkSlateBlue lighten 0.3.normalized saturate 0.2.normalized spin 10.degrees)) above
  ((triangle(40, 40)
      lineWidth 6.0
      lineColor (Color.darkSlateBlue spin (-30.degrees))
      fillColor (Color.darkSlateBlue lighten 0.3.normalized saturate 0.2.normalized spin (-20.degrees))) beside
     (triangle(40, 40)
        lineWidth 6.0
        lineColor (Color.darkSlateBlue spin (30.degrees))
        fillColor (Color.darkSlateBlue lighten 0.3.normalized saturate 0.2.normalized spin (40.degrees)))))
```
</div>
