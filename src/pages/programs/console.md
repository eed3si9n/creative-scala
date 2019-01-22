## console 内での作業

```tut:invisible
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
```

テキスト・エディタや IDE を使ってコードをファイルに保存できるようになりますが、Scala コンパイラが探せるように正しい場所に保存する必要があります。
Doodle テンプレートから作業している場合は、コードは `src/main/scala/` ディレクトリに保存してください。

保存したコードを console から使うにはどうしたらいいでしょうか?
console 内だけで動作する特別なコマンドがあって、それを使ってファイルに保存したコードを実行することができます。
このコマンドは `:paste`[^load] と呼ばれています。`:paste` に続けて実行したいファイル名を書きます。例えば、`src/main/scala/Example.scala` というファイルに式

```tut:silent:book
circle(100) fillColor Color.paleGoldenrod lineColor Color.indianRed
```

を保存したとすると、以下のように console に書くことでこのコードを実行することができます。

```scala
:paste src/main/scala/Example.scala
// res0: doodle.core.Image = ContextTransform(<function1>,ContextTransform(<function1>,Circle(100.0)))
```

上の例では `res0` という名前が与えられたことに注目してください。あなたが console に打ち込んで追従しているとしたら、今まで console に何を書いたのかによって別の数になったかもしれません。`res0.draw` (もしくは、あなたの console のための別の名前) を評価することでこのイメージを描画することができます。


### console を使うためのコツ

console をより効率良く使うためのコツです:

- 上矢印キーを押すと console に最後に打ち込んだものが出てきます。長いファイル名を何度も打ち込まないでいいようになるので便利です! 上矢印キーを複数回押すことで console 内での履歴をさかのぼることができます。

- `Tab` キーを押すことで、console にコードの補完を行ってもらうことができますが、残念ながらファイル名を探すことはできないので、自分で書く必要があります。例えば、`Stri` と書いて `Tab` を押すと、console は可能な補完例を表示します。`Strin` と書けば、console が `String` と補完できるようになります。

[^load]: `:load` というコマンドもあって、これは `:paste` とは少し異なる動作をします。`:load` はファイル内の各行を別にコンパイルして実行するのに対して、`:paste` はファイル全体を一気にコンパイルします。そのため、微妙に違うセマンティクスを持ちます。`:paste` の方が console 外での Scala コードの振る舞いに近いので、私たちは `:load` の代わりに `:paste` を使います。

<div class="callout callout-warn">
コードをファイルに保存し始めると、次回 sbt を起動したときにコンパイラーが私たちのコードを見て怒るのに気付くかもしれません。その対策方法は次の節で解説するので続きを読んでください。
</div>
