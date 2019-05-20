## ネストしたメソッド

```tut:invisible
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
```

メソッドは宣言です。
メソッドの本文内には別の宣言や式を含むことができます。
そのため、メソッド宣言は他のメソッド宣言を含むことができます。

なぜこれが役に立つのかを見るために、以前に書いたメソッドをもう一度見てみましょう:

```tut:book
def cross(count: Int): Image = {
  val unit = Image.circle(20)
  count match {
    case 0 => unit
    case n => unit beside (unit above cross(n-1) above unit) beside unit
  }
}
```

`unit` は `cross` メソッドの中で宣言されています。
そのため、`unit` の宣言は `cross` の本文内だけにスコープ付けされています。
他の宣言を間違ってシャドーイングしないように宣言のスコープを必要最小限に制限するのはお行儀が良いことです。
しかし、ここで `cross` の実行時の振る舞いを考察すると、少し嬉しくない特性が見つかるはずです。

ここれは置き換えモデルを使って `cross(1)` を展開します。

```scala
cross(1)
// Expands to
{
  val unit = Image.circle(20)
  1 match {
    case 0 => unit
    case n => unit beside (unit above cross(n-1) above unit) beside unit
  }
}
// Expands to
{
  val unit = Image.circle(20)
  unit beside (unit above cross(0) above unit) beside unit
}
// Expands to
{
  val unit = Image.circle(20)
  unit beside (unit above
  {
    val unit = Image.circle(20)
    0 match {
      case 0 => unit
      case n => unit beside (unit above cross(n-1) above unit) beside unit
    }
  }
  above unit) beside unit
}
// Expands to
{
  val unit = Image.circle(20)
  unit beside (unit above
  {
    val unit = Image.circle(20)
    unit
  }
  above unit) beside unit
}
```

このような巨大な展開を書き出した理由は再帰するたびに `unit` を作り直していることを示すためです。
これは `unit` が作られるたびに何かを println することでも証明できます。

```tut:book
def cross(count: Int): Image = {
  val unit = {
    println("Creating unit")
    Image.circle(20)
  }
  count match {
    case 0 => unit
    case n => unit beside (unit above cross(n-1) above unit) beside unit
  }
}

cross(1)
```

`unit` は非常に小さいのであまり大したことないですが、多くのメモリや時間を取る処理をしている可能性もあり、不必要に繰り返すのは嬉しくないことです。

これは、`unit` を `cross` の外に出すことで解決できます。

```tut:book
val unit = {
  println("Creating unit")
  Image.circle(20)
}

def cross(count: Int): Image = {
  count match {
    case 0 => unit
    case n => unit beside (unit above cross(n-1) above unit) beside unit
  }
}

cross(1)
```

これは、`unit` は必要以上に大きいスコープを持つことになるので、それも嬉しくないです。
ネストされたメソッド、別名内部メソッドを使うとより良く書くことができます。

```tut:book
def cross(count: Int): Image = {
  val unit = {
    println("Creating unit")
    Image.circle(20)
  }
  def loop(count: Int): Image = {
    count match {
      case 0 => unit
      case n => unit beside (unit above loop(n-1) above unit) beside unit
    }
  }

  loop(count)
}

cross(1)
```

これは、スコープを最小にしつつ `unit` を一度だけ作るという求めている振る舞いとなります。
内部メソッド `loop` は以前通り構造的再帰を行います。
`cross` 内で忘れずに呼ぶ必要があります。
私は、このような内部メソッドはループを行っていることを示すために通常 `loop` や `iter` (iterate の略) と名付けています。

このテクニックは既に見てきたことの小さなバリエーションですが、いくつかの練習問題を行ってパターンを習得したか確認してみましょう。

### 練習問題 {-}

#### チェス盤 {-}

`chessboard` をネストしたメソッドを使って書き換えて、`blackSquare`、`redSquare` そして `base` が `chessboard` が呼ばれたときに一度だけ作られるようにしましょう。

```tut:book
def chessboard(count: Int): Image = {
  val blackSquare = Image.rectangle(30, 30) fillColor Color.black
  val redSquare   = Image.rectangle(30, 30) fillColor Color.red

  val base =
    (redSquare   beside blackSquare) above (blackSquare beside redSquare)
  count match {
    case 0 => base
    case n =>
      val unit = cross(n-1)
      (unit beside unit) above (unit beside unit)
  }
}
```

<div class="solution">

これが私たちが行った方法です。`boxes` で使ったのと全く同じパターンです。

```tut:book
def chessboard(count: Int): Image = {
  val blackSquare = Image.rectangle(30, 30) fillColor Color.black
  val redSquare   = Image.rectangle(30, 30) fillColor Color.red
  val base =
    (redSquare   beside blackSquare) above (blackSquare beside redSquare)
  def loop(count: Int): Image =
    count match {
      case 0 => base
      case n =>
        val unit = loop(n-1)
        (unit beside unit) above (unit beside unit)
    }

  loop(count)
}
```
</div>

#### 賢く箱に入れる {-}

以下の `boxes` を書き直して、`aBox` が `boxes` 内のみのスコープに入り、かつ `boxes` が呼ばれたときに一度だけ作られるようにしましょう。

```tut:silent
val aBox = Image.rectangle(20, 20).fillColor(Color.royalBlue)

def boxes(count: Int): Image =
  count match {
    case 0 => Image.empty
    case n => aBox beside boxes(n-1)
  }
```

<div class="solution">

これは2段階に分けて解きます。まずは、`aBox` を `boxes` に取り込みます。

```tut:silent
def boxes(count: Int): Image = {
  val aBox = Image.rectangle(20, 20).fillColor(Color.royalBlue)
  count match {
    case 0 => Image.empty
    case n => aBox beside boxes(n-1)
  }
}
```

次に、内部メソッドを使って再帰のたびに `aBox` が作られるのを防ぎます。

```tut:silent
def boxes(count: Int): Image = {
  val aBox = Image.rectangle(20, 20).fillColor(Color.royalBlue)
  def loop(count: Int): Image =
    count match {
      case 0 => Image.empty
      case n => aBox beside loop(n-1)
    }

  loop(count)
}
```
</div>
