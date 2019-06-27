## メソッド構文

```tut:invisible
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
```

私たちは既にメソッド宣言の 1例を見ています。

```tut:silent:book
def boxes(color: Color): Image = {
  val box =
    Image.rectangle(40, 40).
      lineWidth(5.0).
      lineColor(color.spin(30.degrees)).
      fillColor(color)

  box beside box beside box beside box beside box
}
```

これをモデルとしてメソッド宣言の構文を理解していきましょう。
最初の部分は**キーワード** `def` です。
キーワードは Scala コンパイラにとって特別な意味を持つ単語で、この場合メソッドを宣言するという意味を持ちます。
これまでに `object` と `val` というキーワードも見てきました。

`def` の直後にはメソッドの名前が続き、この場合 `boxes` で、これは `val` や `object` がそれぞれ宣言するものの名前が直後に続くのに似ています。
`val` 宣言同様に、メソッド宣言はトップレベル宣言ではないので、ファイルに書く場合は `object` 宣言 (もしくはその他のトップレベル宣言) にラッピングされている必要があります。

次に、括弧 (`()`) で定義されるメソッドパラメータが続きます。
このメソッドパラメータは、呼び出す人がメソッドが評価する式に差し込むことができる部分です。
メソッドパラメータを宣言するとき、名前と型を与える必要があります。
コロン (`:`) を使って名前と型を分けます。
ここまでは型を宣言する必要はありませんでした。
ほとんどの場合、Scala は**型推論**という仕組みを使って型を自動的に計算してくれます。
しかし型推論はメソッドパラメータの型は推論できないため、私たちが与える必要があります。

メソッドパラメータの次に戻り値の型が来ます。
戻り型はメソッドが呼ばれたときに評価される値の型です。
パラメータ型と違って Scala は戻り型を推論することができますが、自分で書くのが良い作法なので Creative Scala では戻り型を書くことにします。

最後に、メソッドが呼ばれたときに結果として返す値を計算するための式の本文が来ます。
本文は、`boxes` のようにブロック式のときもあれば、単一の式のときもあります。

<div class="callout callout-info">
#### メソッド宣言の構文 {-}

メソッド宣言の構文は

```scala
def methodName(param1: Param1Type, ...): ResultType =
  bodyExpression
```

で

- `methodName` がメソッド名、
- 省略可能な `param1 : Param1Type, ...` は 1つもしくはそれ以上のパラメータ名とパラメータ型の対、
- 省略可能な `ResultType` はメソッドを呼んだ結果得られる値の型で、
- `bodyExpression` はメソッドを呼んだ結果を計算するために評価される式。
</div>

### 練習問題 {-}

簡単な例題でメソッドの宣言を練習してみましょう。

#### 2乗 {-}

`Int` の引数を受け取り、その引数の 2乗の `Int` を返す `squire` というメソッドを書いてみよう。(数の 2乗は自身を掛けることで得られるよ)

<div class="solution">
解答は

```tut:silent:book
def square(x: Int): Int =
  x * x
```

手順を追って解にたどり着くことができます。

名前 (`square`) パラメータの型、そして戻り型 (`Int`) が与えられています。
ここから、以下のようなメソッドの骨組みを書くことができます。

```tut:silent:book
def square(x: Int): Int =
  ???
```

パラメータの名前として `x` を選びました。
これは、適当な選択です。
特に意味のある名前が見つからないときは 1文字の `x`、`v`、`i` といった名前がよく出てきます。

ちなみに、これは既に妥当なコードです。
コンソールに入力してみてください。
このように宣言した場合、`square` を呼ぶとどんな結果となるでしょう?

次に、本文を完成させる必要があります。
2乗は数を自身で掛け算することだと言われているので、`???` を `x * x` で置き換えます。
これは単一の式なので中括弧で囲む必要はありません。
</div>


#### ハーフ {-}

`Double` の引数を受け取って、その引数を半分にした `Double` を返す `halve` というメソッドを書いてみよう。

<div class="solution">
```tut:silent:book
def halve(x: Double): Double =
 x / 2.0
```

`square` で見たのと同じ手順でこの解が得られるはず。
</div>
