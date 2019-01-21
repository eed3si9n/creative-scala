## 色

```tut:invisible
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
```

レイアウトの他に、Doodle を使って私たちのイメージに彩りを与えることができます。[@tbl:pictures:color] で解説するメソッドを試して、結果がどうなるか見てください。

---------------------------------------------------------------------------------------------
演算子                   型      説明                        使用例
----------------------- ------- --------------------------- ---------------------------------
`Image fillColor Color` `Image` 指定した色でイメージを塗りつぶす `circle(10) fillColor Color.red`

`Image lineColor Color` `Image` 指定した色で線画を描く         `circle(10) lineColor Color.blue`

`Image lineWidth Int`   `Image` 指定した筆幅で線画を描く       `circle(10) lineWidth 3`
---------------------------------------------------------------------------------------------

: Doodle でイメージに色を付けるためのいくつかのメソッド {#tbl:pictures:color}

Doodle では、色を作るための方法がいくつかあります。
最もシンプルな方法は [CommonColors.scala][common-colors] で定義済みの色を使うことです。
[@tbl:pictures:colors] によく使われる色を挙げます。

------------------------------------------------------------------
色                      型       使用例
----------------------- ------- ----------------------------------
`Color.red`             `Color` `circle(10) fillColor Color.red`

`Color.blue`            `Color` `circle(10) fillColor Color.blue`

`Color.green`           `Color` `circle(10) fillColor Color.green`

`Color.black`           `Color` `circle(10) fillColor Color.black`

`Color.white`           `Color` `circle(10) fillColor Color.white`

`Color.gray`            `Color` `circle(10) fillColor Color.gray`

`Color.brown`           `Color` `circle(10) fillColor Color.brown`
------------------------------------------------------------------

: 定義済みの色のうちよく使われるもの {#tbl:pictures:colors}

### 練習問題 {-}

#### 邪視の魔除け {-}

邪視に対抗するための伝統的なお守りに似せた [@fig:pictures:evil-eye] のようなイメージを作ってみよう。私は虹彩の部分は `cornflowerBlue`、外側の部分は `darkBlue` を使ったけども、独自に他の色も実験してみて!

![邪視退散!](src/pages/pictures/evil-eye.pdf+svg){#fig:pictures:evil-eye}

<div class="solution">
これが私のお守りです:

```tut:book
((circle(10) fillColor Color.black) on
 (circle(20) fillColor Color.cornflowerBlue) on
 (circle(30) fillColor Color.white) on
 (circle(50) fillColor Color.darkBlue))
```
</div>
