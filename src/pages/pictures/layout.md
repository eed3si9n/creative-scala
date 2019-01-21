## レイアウト

```tut:invisible
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
```

ここまでで、基本図形のイメージの作り方を見てきました。レイアウトメソッドを使ってイメージを組み合わせることで、より複雑なイメージを作ることができます。以下のコードを試してみましょう。[@fig:picture:circle-rect] のように、円と長方形が隣り合わせになっているのが見えるはずです。

~~~ scala
(circle(10) beside rectangle(10, 20)).draw
~~~

![A circle beside a rectangle](src/pages/pictures/circle-beside-rectangle.pdf+svg){#fig:picture:circle-rect}

[@tbl:pictures:layout] は、イメージを組み合わせるために Doodle が提供するレイアウトメソッドです。それぞれ使ってみて、何をするのか見てみよう。

----------------------------------------------------------------------------------------
演算子                 型      説明                        使用例
--------------------- ------- -------------------------- -------------------------------
`Image beside Image`  `Image` 2つのイメージを横に並べる     `circle(10) beside circle(20)`

`Image above Image`   `Image` 2つのイメージを縦に並べる     `circle(10) above circle(20)`

`Image below Image`   `Image` 2つのイメージを縦に並べる     `circle(10) below circle(20)`

`Image on Image`      `Image` 2つのイメージを中心で重ねる   `circle(10) on circle(20)`

`Image under Image`   `Image` 2つのイメージを中心で重ねる   `circle(10) under circle(20)`
----------------------------------------------------------------------------------------

: Doodle のレイアウトメソッド {#tbl:pictures:layout}

### 練習問題 {-}

#### The Width of a Circle {-}

これまで見てきたレイアウトメソッドと基礎図形のイメージを使って [@fig:picture:width-of-a-circle] のような絵を描いてみよう。

![The width of a circle](src/pages/pictures/width-of-a-circle.pdf+svg){#fig:picture:width-of-a-circle}

<div class="solution">
これは 3つの小さな円を 1つの大きな円に重ねたものなので、コードではこのように書けます。

```tut:book
(circle(20) beside circle(20) beside circle(20)) on circle(60)
```
</div>
