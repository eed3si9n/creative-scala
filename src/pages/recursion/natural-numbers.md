## 自然数

```tut:invisible
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
val aBox = Image.rectangle(20, 20).fillColor(Color.royalBlue)
```

自然数は 0以上の整数です。つまり、0, 1, 2, 3... の数です。(自然数を 0 ではなく 1 から始めて定義する人もいますが、私たちの目的としてはどちらの定義を使っても大差は無いのでここでは 0 から始まるものと前提を置きます)

自然数の面白い特性として、再帰的に定義できることが挙げられます。つまり、自然数はそれら自身を使って定義することが可能です。このような巡回した定義は無意味な結果になると一見思うかもしれません。これは基底ケース (base case) を定義に含んで再帰を停止させることで回避します。具体的な定義は:

自然数 `n` は

- 0 もしくは
- 1 + `m`、ただし `m` は自然数である。

`0` の場合が基底ケースで、その他の場合が自然数 `n` を別の自然数 `m` で定義するので再帰的になっています。`m` は常に `n` よりも小さく、基底ケースが自然数の最小値なので、この定義は全ての自然数を定義します。

任意の自然数があるとき (例えば 3) 上記の定義を使って分解していくことができます:

3 = 1 + 2 = 1 + (1 + 1) = 1 + (1 + (1 + 0))

再帰ルールを使って等式を可能な限り展開していきました。最後に基底ケースを使って再帰を停止します。

## 構造的再帰

構造的再帰に進みます。自然数の構造的再帰パターンは 2つのものを与えてくれます:

- 自然数を処理するための再利用可能なコードの骨組み、そして
- この骨組みを使って**全ての**自然数の処理を実装することができる保証

`boxes` を以下のように書いたのを思い出してください。

```tut:book
def boxes(count: Int): Image =
  count match {
    case 0 => Image.empty
    case n => aBox beside boxes(n-1)
  }
```

`boxes` を作ったときはこのパターンが何の前触れも無く出てきました。
今見るとこのパターンは自然数の定義に直接沿っていることが分かります。
自然数の再帰的定義を思い出してください: 自然数 `n` は

- 0 もしくは
- 1 + `m`、ただし `m` は自然数である。

`match` 式のパターンはこの定義にマッチします。

```scala
count match {
  case 0 => ???
  case n => ???
}
```

という式は `count` を、`count` が 0 の場合と、それ以外の自然数 `n` の場合 (その場合 `1 + m`) の 2つのケースにおいてチェックしていることを意味します。

`match` 式の右辺はそれぞれのケースにおいて何をするかを指示します。`0` の場合は `Image.empty` です。`n` の場合は、`aBox beside boxes(n-1)` です。

ここが重要な点です。
右辺の構造が、マッチする自然数の構造と同様になっていることに気づいたでしょうか。
基底ケースの `0` にマッチする場合は、私たちの結果も基底ケースの `Image.empty` です。再帰的ケースの `n` にマッチする場合は、右辺の構造も自然数の定義の再帰的ケースの構造に対応します。
定義は `n` は `1 + m` だと言っています。
右辺では私たちは 1 を `aBox` に置き換えて、+ を `beside` に置き換えて、定義も再帰する所では私たちは再帰的に `boxes` を `m` (`n-1` となります) で呼び出します。

```tut:book
def boxes(count: Int): Image =
  count match {
    case 0 => Image.empty
    case n => aBox beside boxes(n-1)
  }
```

繰り返しますが、`match` 式の左辺は自然数の定義と完全に一致します。右辺も定義と一致しますが、自然数の代わりにイメージと置き換えられています。ゼロに相当するイメージは `Image.empty` です。`1 + m` に相当するイメージは `aBox beside boxes(m)` です。

この汎用パターンは自然数を何か別の型へと変換したい全ての場合において適用することができます。
私たちは常に `match` 式を持ちます。
私たちは常に基底ケースと再帰ケースに対応する 2つのパターンを持ちます。
右辺も常に `1`、`+` そして `n-1` を特定の結果に合わせた基底ケースと再帰ケースを持ちます。

<div class="callout callout-info">
#### 自然数の構造的再帰パターン {-}

自然数の構造的再帰の大まかなパターンは

```scala
def name(count: Int): Result =
  count match {
    case 0 => resultBase
    case n => resultUnit add name(n-1)
  }
```

で、`Result`、`resultBase`、`resultUnit` そして `add` は解いている問題に特定のものです。
自然数の構造的再帰パターンを実装するためには

 - 私たちが書いているメソッドが自然数を入力として受け取ることに気づき
 - 結果の型を考えて
 - 結果のための基底、単位 (unit)、そして加算をどうするべきか決める必要があります。
</div>

このシンプルですが強力なツールを使って他に何ができるか探検する準備が整いました。

<div class="callout callout-info">
### 証明とプログラム

数学を勉強したことがあれば、帰納法を用いた証明を見たことがあるでしょう。
帰納法による証明の大まかなパターンは自然数の構造的再帰の大まかなパターンとよく似ています。
これは偶然ではなく、この 2つには深い関係があります。
私たちは自然数の構造的再帰を帰納法による証明の 1つだとみなすことができます。
構造的再帰の骨組みを使うことで全ての自然数の変換を書くことができるという主張は、暗黙的に私たちが使っている数学的基礎に裏付けられています。
この 2つのつながりを使って私たちのコードの性質を証明することもできます。構造的回帰は暗黙的に何らかの性質の証明を定義します。

この証明とプログラムのつながりは**カリー＝ハワード同型対応** (Curry-Howard Isomorphism) と呼ばれます。
</div>

### 練習問題 {-}

#### クロス {-}

最初の練習問題はクロスのイメージを生成する `cross` という関数を作ることです。 [@fig:recursion:cross] は 4つのクロスのイメージを表し、それぞれ `cross` を `0` から `3` と共に呼び出した場合に対応します。

メソッドの骨組みは

```tut:book
def cross(count: Int): Image =
  ???
```

![0 から 3 の `count` により生成されたクロス](./src/pages/recursion/cross.pdf+svg){#fig:recursion:cross}

`cross` の本文にはどのようなパターンを使うことができるでしょう? パターンを書き出してみよう。

<div class="solution">
自然数の構造的再帰ですね。以下のようになるはずです。

```scala
def cross(count: Int): Image =
  count match {
    case 0 => <resultBase>
    case n => <resultUnit> <add> cross(n-1)
  }
```
</div>

どのパターンを使うかが分かったので、プログラムの特定の部分を埋めていく必要があります:

- 基底ケース、そして
- 単位および加法演算。

ヒント: [@fig:recursion:cross] を使って上の要素を探すことができます。

<div class="solution">
絵から、基底ケースが 1つの円であることが分かります。

絵の後続の要素は絵の上、下、右、左に円を追加しています。そのため、私たちの単位はベースと同じく 1つの円ですが、加法演算子は今までに見たようなただの `beside` や `above` では無く、`unit beside (unit above cross(n-1) above unit) beside unit` となります。
</div>

`cross` の実装を完成させなさい。

<div class="solution">
解答例です。

```scala
def cross(count: Int): Image = {
  val unit = Image.circle(20)
  count match {
    case 0 => unit
    case n => unit beside (unit above cross(n-1) above unit) beside unit
  }
}
```
</div>


#### チェス盤 {-}

クロスの練習問題では私たちが作ろうとしているものの再帰的構造を見つけるのが難しいところだと分かりました。それが見えれば、構造的再帰パターンを埋めていくのは単純作業です。

この練習問題と次で、再帰構造を見つける練習をしましょう。
この練習問題のミッションはチェス盤の再帰構造を見つけて、チェス盤を描くメソッドを実装することです。
メソッドの骨組みは

```tut:silent:book
def chessboard(count: Int): Image =
  ???
```

`count` に `0` から `2` を渡してチェス盤を描いた場合の例を [@fig:recursion:chessboards] に示しました。
ヒント: `count` がチェス盤の幅ではなく、原子的な「チェス盤の単位」を返すことに注目してください。

![0 から 2 の `count` により生成されたチェス盤](./src/pages/recursion/chessboards.pdf+svg){#fig:recursion:chessboards}

`chessboard` を実装してみよう。

<div class="solution">
`chessboard` は自然数の構造的再帰なので、このパターンの骨組みはすぐに書くことができます。

```scala
def chessboard(count: Int): Image =
  count match {
    case 0 => resultBase
    case n => resultUnit add chessboard(n-1)
  }
```

前にもやったように、結果に対応する基底、単位、加法演算を決める必要があります。
[@fig:recursion:chessboards] でチェス盤の連続を示すことで私たちはヒントをあげました。
そこから、基底が 2x2 のチェス盤であることが分かります。

```tut:silent:book
val blackSquare = Image.rectangle(30, 30) fillColor Color.black
val redSquare   = Image.rectangle(30, 30) fillColor Color.red

val base =
  (redSquare beside blackSquare) above (blackSquare beside redSquare)
```

次に、単位と加法演算を求めます。
単位は再帰呼出し `chessboard(n-1)` によって得られる値です。
加法演算は `(unit beside unit) above (unit beside unit)` です。

これらを組み合わせるとこうなります。

```tut:silent:book
def chessboard(count: Int): Image = {
  val blackSquare = Image.rectangle(30, 30) fillColor Color.black
  val redSquare   = Image.rectangle(30, 30) fillColor Color.red

  val base =
    (redSquare   beside blackSquare) above (blackSquare beside redSquare)
  count match {
    case 0 => base
    case n =>
      val unit = chessboard(n-1)
      (unit beside unit) above (unit beside unit)
  }
}
```

以前にプログラミング経験のある人はチェス盤を 2つの入れ子のループを使って作ることを思いついたかもしれません。
ここでは私たちは小さいチェス盤から大きいチェス盤へと合成するという別の方法を取っています。
このように問題を別の方法で分解することをしっかり理解することは関数型プログラミングが上手になるための重要なステップとなります。
</div>


#### シェルピンスキーの三角 {-}

[@fig:recursion:sierpinski] に示したシェルピンスキーの三角は有名なフラクタルです。([@fig:recursion:sierpinski] はシェル**ピンク**スキーの三角ですが)

![シェルピンスキーの三角](./src/pages/recursion/sierpinski.pdf+svg){#fig:recursion:sierpinski}

一見複雑に見えますが、構造を分解して自然数の構造的再帰を使って生成することができます。
以下の骨組みを使ってメソッドを実装してみましょう。

```tut:book
def sierpinski(count: Int): Image =
  ???
```

今回はヒント無しです。
今までに見たことを使えばできるはずです。

<div class="solution">
鍵となるステップは、シェルピンスキーの三角の基底が `triangle above (triangle beside triangle)` であると気づくことです。
それに気付けば、コードの構造は `chessboard` と全く同じものです。
以下が私たちの実装です。

```tut:book

def sierpinski(count: Int): Image = {
  val triangle = Image.triangle(10, 10) lineColor Color.magenta
  count match {
    case 0 => triangle above (triangle beside triangle)
    case n =>
      val unit = sierpinski(n-1)
      unit above (unit beside unit)
  }
}
```

</div>
