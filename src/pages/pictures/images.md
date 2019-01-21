## イメージ

```tut:invisible
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
```

まずは以前のように console を使って、簡単な形を描いてみましょう。

```tut:book
Image.circle(10)
```

何が起こっているのでしょう? `Image` はオブジェクトで、`circle` はそのオブジェクトのメソッドです。私たちは `circle` に `10` というパラメータを渡して、私たちが作る円の半径を指定します。結果の型に注目してください。`Image` となっていますね。

Doodle 内で console を実行した場合は、これらのイメージを作るためのメソッドが使用可能な状態になっているので、`circle(10)` とだけ書くこともできます。

```tut:book
circle(10)
```

この円を描画するためには、`draw` メソッドを呼びます。

```scala
circle(10).draw
```

[@fig:pictures:circle] のようなウィンドウが表示されるはずです。

![A circle](src/pages/pictures/circle.pdf+svg){#fig:pictures:circle}

Doodle は、円、長方形、三角形といったいくつかの基本図形をサポートします。長方形を描いてみましょう。

```scala
rectangle(100, 50).draw
```

結果は [@fig:pictures:rectangle] のようになります。

![A rectangle](src/pages/pictures/rectangle.pdf+svg){#fig:pictures:rectangle}

最後に、[@fig:pictures:triangle] のような三角形を描いてみましょう。

```scala
triangle(60, 40).draw
```

![A triangle](src/pages/pictures/triangle.pdf+svg){#fig:pictures:triangle}

### 練習問題 {-}

#### 堂々巡り {-}

1、10、100 単位の幅の円を作ってみましょう。次に、それを描いてみよう!

<div class="solution">
この練習問題は Dooble が正しくインストールされているかの確認を行い、このライブラリを使うのに慣れてもらいます。
Doodle を使うときの注意点として、**イメージの定義**と**イメージの描画**が別であるということがあります。この点に関してはこの本を通して何回も出てきます。

以下のようなコードで円を作ることができます。

```tut:silent:book
circle(1)
circle(10)
circle(100)
```

それぞれの円の `draw` メソッドを呼ぶことで円を描画することができます。

```scala
circle(1).draw
circle(10).draw
circle(100).draw
```
</div>


#### 私のアートのタイプ {-}

円の型は何でしょうか? 長方形の場合は? 三角形の場合は?

<div class="solution">
console で確認できる通り、それらは全て `Image` 型を持ちます。

```scala
:type circle(10)
// doodle.core.Image
:type rectangle(10, 10)
// doodle.core.Image
:type triangle(10, 10)
// doodle.core.Image
```
</div>

#### 私のアートのタイプじゃない {-}

イメージの**描画**の型は何でしょうか? これは何を意味するのでしょう?

<div class="solution">
再び console でこの質問を聞いてみましょう。

```scala
:type circle(10).draw
// Unit
```

見ての通り、イメージの描画の型は `Unit` です。`Unit` は特筆すべき値を返さない式のための型です。これは `draw` の場合に当てはまります。何故なら `draw` は画面に何かを表示するために呼ばれるのであり、戻り値に使い道は無いからです。`Unit` 型を持つ値は 1つだけあります。これは unit値と呼ばれ、リテラル式 `()` として書かれます。

console は unit値をデフォルトでは表示しないことに注意してください。

```scala
()
```

console に型を聞くことで、そこに unit値があることを確認することができます。

```scala
:type ()
// Unit
```
</div>
