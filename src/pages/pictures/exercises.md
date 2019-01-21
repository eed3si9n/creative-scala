## 練習問題

```tut:invisible
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
```

### ターゲット

アーチェリー用のターゲットを、[@fig:pictures:target1] のように 3つの同心円で得点帯を表して描いてみよう。

![シンプルなアーチェリー用ターゲット](src/pages/pictures/target1.pdf+svg){#fig:pictures:target1}

ボーナス得点として、[@fig:pictures:target2] のようにターゲットを練習場に置けるようにスタンドを追加してください。

![スタンド付きのアーチェリー用ターゲット](src/pages/pictures/target2.pdf+svg){#fig:pictures:target2}

<div class="solution">
最もシンプルな解法は `on` 演算子を使って同心円を描くことです。

```tut:silent:book
(circle(10) on circle(20) on circle(30))
```

ボーナス得点のスタンドは 2つの長方形を使って描きます。

```tut:silent:book
(
  circle(10) on
  circle(20) on
  circle(30) above
  rectangle(6, 20) above
  rectangle(20, 6)
)
```
</div>


### ぶれない標的

ターゲットを赤と白で色付けしてみよう。(もし前問で追加した場合は) スタンドは茶色、芝生は緑に色付けしてみよう。
例としては [@fig:pictures:target3] を参照してください。

![色付けしたアーチェリーのターゲット](src/pages/pictures/target3.pdf+svg){#fig:pictures:target3}

<div class="solution">
ここでのコツはカッコをうまく使って合成の順序を制御することです。
`fillColor()`、`lineColor()`、そして `lineWidht()` メソッドは単一のイメージに適用され、イメージが正しい形から構成されているかを確認する必要があります。

```tut:silent:book
(
  ( circle(10) fillColor Color.red ) on
  ( circle(20) fillColor Color.white ) on
  ( circle(30) fillColor Color.red lineWidth 2 ) above
  ( rectangle(6, 20) above rectangle(20, 6) fillColor Color.brown ) above
  ( rectangle(80, 25) lineWidth 0 fillColor Color.green )
)
```
</div>
