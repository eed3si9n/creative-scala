## メソッド

```tut:invisible
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
```

前に出てきた章の 1つの中で [@fig:methods:sequential-boxes] で示すイメージを以下のようなプログラムを使って作りました。

![Five boxes filled with Royal Blue](./src/pages/programs/sequential-boxes.pdf+svg){#fig:methods:sequential-boxes}

```tut:silent:book
val box =
  Image.rectangle(40, 40).
    lineWidth(5.0).
    lineColor(Color.royalBlue.spin(30.degrees)).
    fillColor(Color.royalBlue)

box beside box beside box beside box beside box
```

ここで、箱の色を変えたいと思ったとします。
現状だと別の色のためにわざわざ式を書き直す必要があります。

```tut:silent:book
val paleGoldenrod = {
  val box =
    Image.rectangle(40, 40).
      lineWidth(5.0).
      lineColor(Color.paleGoldenrod.spin(30.degrees)).
      fillColor(Color.paleGoldenrod)

  box beside box beside box beside box beside box
}

val lightSteelBlue = {
  val box =
    Image.rectangle(40, 40).
      lineWidth(5.0).
      lineColor(Color.lightSteelBlue.spin(30.degrees)).
      fillColor(Color.lightSteelBlue)

  box beside box beside box beside box beside box
}

val mistyRose = {
  val box =
    Image.rectangle(40, 40).
      lineWidth(5.0).
      lineColor(Color.mistyRose.spin(30.degrees)).
      fillColor(Color.mistyRose)

  box beside box beside box beside box beside box
}
```

これはつかれます。
それぞれの式は少ししか違いがありません。
大まかなパターンをとらえて、色違いだけを表すことができれば嬉しいです。
メソッドを宣言することでまさにそれを実現することができます。

```tut:silent:book
def boxes(color: Color): Image = {
  val box =
    Image.rectangle(40, 40).
      lineWidth(5.0).
      lineColor(color.spin(30.degrees)).
      fillColor(color)

  box beside box beside box beside box beside box
}

// 色違いの箱を作る
boxes(Color.paleGoldenrod)
boxes(Color.lightSteelBlue)
boxes(Color.mistyRose)
```

自分で試してみて、全部書き下した場合とメソッドを使った場合で同じ結果を得られるかみてみましょう。

メソッドの宣言の例を 1つみたので、メソッドの構文の解説をする必要があります。続いて、メソッドの書き方、メソッド呼び出しのセマンティクス、どう置き換えするのかを見ていきましょう。
