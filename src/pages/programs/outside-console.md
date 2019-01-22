## console 外でのコーディング

```tut:invisible
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
```

これまで console で書いてきたコードは、console 外で実行すると問題が発生します。例えば、以下のコードを `src/main/scala` 内の `Example.scala` に書いてください。

```tut:silent:book
Image.circle(100) fillColor Color.paleGoldenrod lineColor Color.indianRed
```

次に、sbt を再起動して console に入ってみましょう。以下のようなエラーが表示されるはずです。

```bash
[error] src/main/scala/Example.scala:1: expected class or object definition
[error] circle(100) fillColor Color.paleGoldenrod lineColor Color.indianRed
[error] ^
[error] one error found
```

IDE を使っている場合も似たようなエラーが出てくるはずです。

問題は、このようになっています:

- Scala は、console が起動する前に全てのコードをコンパイルしようとします。
- ファイルに書かれるコードには、直接 console に書かれるコードには無い制約がいくつかあります。

そのため、これらの制約を知って、ファイルに書くコードの書き方を変える必要があります。

エラーメッセージにヒントが隠されています。`expected class or object definition` (クラスまたはオブジェクトを期待する)。クラスが何かはまだ分かりませんが、オブジェクトのことは知っています -- 全ての値はオブジェクトです。Scala では、全てのコードはオブジェクトかクラス内に書かれる必要があります。オブジェクトは、以下のように式をラッピングすることで定義できます。

```tut:silent:book
object Example {
  (circle(100) fillColor Color.paleGoldenrod lineColor Color.indianRed).draw
}
```

次は別の理由でコンパイルが通りません。以下のようなエラーが沢山出てきたと思います。

```bash
[error] doodle/shared/src/main/scala/doodle/examples/Example.scala:2: not found: value circle
[error]   (circle(100) fillColor Color.paleGoldenrod lineColor Color.indianRed).draw
[error]    ^
```

私たちが `circle` という名前を使ったけども、この名前が何を指しているのか分からないと、コンパイラは言っています。
上のコード内の `Color` でも同様の問題が発生します。
名前に関する詳しい解説は後ほど行います。
とりあえず今の所は `import` 文を書いて、これらの名前の値をどこから見つければいいのかを教えてあげましょう。
`Color` という名前は `doodle.core` という**パッケージ**の中にあり、`circle` という名前は `doodle.core` 内の `Image` オブジェクトの中にあります。
`doodle.core` 内の全ての名前と `Image` オブジェクト内の全ての名前を使うようにコンパイラに指示するには以下のように書きます。

```tut:silent:book
import doodle.core._
import doodle.core.Image._
```

コードが完全に動作するためには、コンパイラは他にもいくつかの名前を探す必要があります。
それらは以下のように import します。

```tut:silent:book
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
```

これらの import 文はファイルの一番上に書くので、コード全体はこのようになります:

```scala
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._

object Example {
  (circle(100) fillColor Color.paleGoldenrod lineColor Color.indianRed).draw
}
```

これで問題なくコードがコンパイルするはずです。

これで、sbt 内の console に行った場合は、私たちのコードをさっき名付けた `Example` という名前を使って参照することができます。

```scala
Example // draws the image
```

### 練習問題 {-}

まだ行っていなければ、上のコードを `src/main/scala/Example.scala` というファイルに保存して、コードがコンパイルされて console からアクセスできることを確認してみよう。
