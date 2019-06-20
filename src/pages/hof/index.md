# 園芸と高階関数

この章では、花の描き方と第1級値としての関数の使い方を習います。

私たちは、プログラムが値を取り扱うことは分かっていますが、全ての値が**第1級** (first-class) ではありません。第1級値はメソッドのパラメータとして渡したり、メソッド呼び出しの結果として返せるものを指します。

もしも私たちが関数を別の関数の引数として渡したら

- 渡された関数は第1級値として使われており、また
- 関数パラメーターを受け取った関数は**高階関数** (higher-order function) と呼ばれます。

この用語は特に重要なものではありませんが、他の書物でも見かけると思うので (多少おぼろげでも) 知っておくと役に立つと思います。
最初は何のことか分からないかもしれませんが、例を見ていくうちに分かるようになります。

これまでは**関数**と**メソッド**という用語を特に区別せずに使ってきました。
Scala では、これら 2つの用語は関連はしますが、別々の意味を持つことを見ていきます。

背景の説明はここまでにして、

- Scala での関数の作り方
- 第1級関数を使ってプログラムを構造化する方法

を見ていきましょう。これを動機づける例として [@fig:hof:flower-power] にような花を描く例を使います。

![この章で出てくるテクニックを使って作られた花](src/pages/hof/flower-power.pdf+svg){#fig:hof:flower-power}

<div class="callout callout-info">
例題を Doodle の sbt console 内で実行した場合は、何もしなくても動作するはずです。そうじゃない場合は、以下の import 文を使って Doodle を使用可能な状態にする必要があります。

```tut:silent
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
```
</div>
