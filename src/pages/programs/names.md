## 名前

```tut:invisible
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
```

前の節では多くの新しい概念を紹介しました。
この節では、そのうちの 1つ「値に名前をつけること」をもう少し掘り下げて見てみましょう。

私たちは、名前を使って色んな物を参照します。
例えば「プロフェスール・エミル・ペロ」(Professeur Emile Perrot) はとても香りの強いバラの品種を指し、「チェリー・パフェ」(Cherry Parfait)　は非常に病気に強いけどもほとんど香りがしない品種のことです。
話し言葉においてこの関係が正確にはどのようになっているのかに関して、人々は多くの考察を与えてきました。
プログラミング言語はより制限されているため、正確な定義を与えることができます: 名前は値を指します。
名前が値に**束縛**されている、もしくは名前が**バインディング**を導入するといった言い方をすることもあります。
値が名前を持つ場合は、値をそのまま書いて使うことができる所全てで、代わりに名前を使うことができます。
別の言い方をすると、名前は値が参照する値に評価されます。
ここから必然的に出てくる疑問があります: どうやって値に名前を与えるのでしょう?
Scala ではいくつの方法があるので、それを見ていきましょう。

### オブジェクトリテラル

オブジェクトリテラルの宣言の仕方は既に見ています。

```tut:silent:book
object Example {
  (circle(100) fillColor Color.paleGoldenrod lineColor Color.indianRed).draw
}
```

これは、今までに見てきた他のリテラル式同様にリテラル式ですが、この場合 `Example` という名前のオブジェクトを作ります。
プログラムの中で `Example` という名前を使うと、このオブジェクトに評価されます。

```scala
Example
// Example.type = Example$@76c39258
```

console 内で何回か試してみてください。
名前を使ったときの違いに気づいたでしょうか?
`Example` という名前を**最初**に使ったときは絵が描かれたけども、次回以降は何も起こらなかったことに気づいたかもしれません。
オブジェクトの名前を初めて使ったときにはオブジェクトの本文が評価されて、オブジェクトが作られます。
次回以降の名前の使用時にはオブジェクトが既に存在するので再評価されません。
この場合は、オブジェクトの内部の式が `draw` メソッドを呼ぶため私たちはこの違いに気付くことができました。
もしこれを `1 + 1` (もしくは、`draw` を抜いただけのもの) などに置き換えると違いは分からなくなります。
これに関しては後ほどの章で存分に解説します。

このオブジェクトの型は何なのか気になるかもしれません。
console に聞いてみましょう。

```scala
:type Example
// Example.type
```

`Example` の型は `Example.type` で、他に値を持たない固有の型です。

### `val` 宣言

オブジェクトリテラルはオブジェクトの作成と名前の定義を同時に行います。
この 2つを分けて、既に存在する値に名前を与えられると便利です。
`val` 宣言を用いてそれを行うことができます。

`val` を使うには

```scala
val <名前> = <値>
```

の `<名前>` と `<値>` をそれぞれ名前と値に評価されるような式をそれぞれ書きます。

具体例で解説します。

```tut:silent:book
val one = 1
val anImage = Image.circle(100).fillColor(Color.red)
```

これら 2つの宣言は `one` と `anImage` という名前を定義します。
後からこれらの名前をコードの中で使って値を参照することができます。

```tut:book
one
anImage
```


### 宣言

上の節で、宣言と定義という言い方をしました。
これらの用語が正確には何を意味するのかを理解して、`object` と `val` の違いをより深く見ていきましょう。

式については分かっています。
それらは、値に評価されるプログラムの一部です。
**宣言**や**定義**は、プログラムの別の部分で、それらは値に評価されません。
代わりに、それらは何かに名前を与えます。実は、Scala では値だけではなく型も宣言できますが、これに関してはここではあまり考察しません。
`object` と `val` は両方とも宣言です。

宣言と式が別になっていることの結果の 1つとして、以下のようなプログラム

```tut:fail:book
val one = ( val aNumber = 1 )
```

は `val aNumber = 1` が式ではなく、値に評価されないため書くことができません。

しかし、以下のようには書くことができます。

```tut:book
val aNumber = 1
val one = aNumber
```

### トップレベル

値に名前を与えるのに `object` と `val` 宣言という別々の方法があるのに納得がいかないかもしれません。
名前を宣言するのに `val` を使って、`object` は名前を付けずにオブジェクトの作成を行えばいいんじゃないでしょうか?
名前を付けずにオブジェクトリテラルを宣言することはできるでしょうか?

<div class="solution">
Scala はそれを許しません。
例えば、以下のようには書くことができません。

```tut:fail:book
object {}
```

オブジェクトリテラルは必ず名前を付ける必要があります。
</div>

Scala は、**トップレベル**と呼ばれるコードとその他のものを区別します。
トップレベルにあるコードは、それをラッピングするコードを一切外側に持ちません。
別の言い方をすると、それは `object` に包むことなくファイルに直接書いてコンパイルすることができるものです。

式はトップレベルではないことを見ました。
`val` もトップレベルではありません。
しかし、オブジェクトリテラルはトップレベルです。

この区別は、ちょっと面倒なものです。
他の言語にはこの区別が無いものもあります。
Scala の場合は、Scala が Java コードを実行させるための Java Virtual Machine (JVM) 上に構築されていることによります。
Java がトップレベルとその他のコードを区別するために、Scala も JVM 上で動作するために仕方がなく区別をする必要があります。
Scala の console はこのトップレベル**区別を行わない**ため、Scala を習い始めたときに混乱しやすいポイントとなります (console に書かれたもの全てがオブジェクトにラッピングされたいると思ってください)。

オブジェクトリテラルはトップレベルであることが許されているけども、`val` 宣言は許されていないということは、オブジェクトリテラル内に `val` を宣言できるということでしょうか?
オブジェクトリテラル内に `val` を宣言した場合、後からその名前を参照することができるでしょうか?

<div class="solution">
できます!

このようにして、オブジェクトリテラル内に `val` を置くことができます:

```tut:silent:book
object Example {
  val hi = "Hi!"
}
```

これを後から参照するには、既に使ってきた `.` 構文を使います。

```tut:book
Example.hi
```

`hi` を単独で使うことはできないことに注意してください。

```tut:fail:book
hi
```

Scala に対して、`Example` オブジェクト内で定義された名前 `hi` を参照したいと伝える必要があるからです。
</div>


### スコープ

さっきの練習問題をやったとすると (やったよね?)、オブジェクト内で宣言された名前は、その名前を含んだオブジェクトも参照しないとオブジェクト外で使えないことを見ました。
具体例で解説すると、

```tut:book
object Example {
  val hi = "Hi!"
}
```

以下のようには書くことができません。

```tut:fail:book
hi
```

Scala に対して `Example` 内の中で `hi` を探す必要があると伝える必要があります。

```tut:book
Example.hi
```

名前が修飾無しで使える所のことを名前が**可視** (visible) 状態であるという言い方をして、名前が可視状態である所のことをそのスコープと言います。
そのため、この気取った新しい用語を使うと、「`hi` は `Example` 外では不可視である」または「`Example` 外では `hi` はスコープに無い」と言えます。

名前のスコープはどうやったら分かるでしょうか?
ルールは簡単で、名前は宣言された位置から始まり、直近の外側の中括弧 (`{` と `}`) の終わりまで可視状態にあります。
上の例では、`hi` は `Example` の中括弧に囲まれているので、そこで可視状態にあります。
他では見えません。

オブジェクトリテラルをオブジェクトリテラルの中で宣言することができ、より細かなスコープの区別することができます。
具体例で解説すると、

```tut:silent:book
object Example1 {
  val hi = "Hi!"

  object Example2 {
    val hello = "Hello!"
  }
}
```

`hi` は `Example2` 内でもスコープ内です (`Example2` は `hi` の外側の中括弧内で定義されているため)。
しかし、`hello` のスコープは `Example2` だけに限定されているため、`hi` のスコープよりも小さなものです。

もしスコープ内で既に宣言されている名前を再宣言するとどうなるでしょうか?
これは**シャドーイング**と呼ばれています。
以下のコードでは、`Example2` 内の `hi` は `Example1` 内の `hi` を覆い隠します。

```tut:silent:book
object Example1 {
  val hi = "Hi!"

  object Example2 {
    val hi = "Hello!"
  }
}
```

Scala はこれを許容しますが、コードがすごく分かりづらくなるので一般的には悪い考えです。

オブジェクトリテラルを使わなくても新しいスコープを作ることができます。
Scala は、中括弧を置くことでほぼ全ての場所でスコープを作ることができます。

例えば以下のように書けます。

```tut:silent:book
object Example {
  val good = "Good"

  // Create a new scope
  {
    val morning = good ++ " morning"
    val toYou = morning ++ " to you"
  }

  val day = good ++ " day, sir!"
}
```

`morning` (と `toYou`) は新しいスコープ内で宣言されています。(このスコープには名前が無いため) このスコープを外側から参照することはできないので、宣言されているスコープ外から `morning` を参照することは不可能です。
残りのプログラムに知られたくない秘密があるときはこの方法を使って隠すことができます。

このような Scala での入れ子のスコープの振る舞いは**レキシカルスコープ**と呼ばれます。
全ての言語がレキシカルスコープを持つわけではありません。
例えば、Ruby や Python はレキシカルスコープを持たず、JavaScript はやっと最近になって導入されました。
筆者の意見では、レキシカルスコープを持たない言語を設計するのは、グアテマラ産激辛トウガラシを大量に食べた後で手を洗わずにトイレに行くぐらい馬鹿げたことだと思います。

### 練習問題 {-}

名前とスコープが理解できているかをテストするために、以下のそれぞれの場合で `answer` の値を求めてみましょう。

```tut:silent:book
val a = 1
val b = 2
val answer = a + b
```

<div class="solution">
まずはシンプルな例から。`answer` は `1 + 2` なので `3` です。
</div>

```tut:silent:book
object One {
  val a = 1

  object Two {
    val a = 3
    val b = 2
  }

  object Answer {
    val answer = a + Two.b
  }
}
```

<div class="solution">
これもシンプルな例です。`answer` は `1 + 2` なので `3` です。`Two.a` は `answer` が定義されている場所ではスコープ外です。
</div>

```tut:silent:book
object One {
  val a = 5
  val b = 2

  object Answer {
    val a = 1
    val answer = a + b
  }
}
```

<div class="solution">
ここでは `Answer.a` が `One.a` を覆い隠すので、`answer` は `1 + 2` で `3` となります。
</div>

```tut:silent:book
object One {
  val a = 1
  val b = a + 1
  val answer = a + b
}
```

<div class="solution">
これは完全に普通のコードです。`b` の宣言の右辺項にある式 `a + 1` は普通の式であるので、`answer` は再び `3` となります。
</div>

```tut:silent:book
object One {
  val a = 1

  object Two {
    val b = 2
  }

  val answer = a + b
}
```

<div class="solution">
`b` は `answer` が宣言されている所では `b` はスコープ外であるので、このコードはコンパイルを通りません。
</div>

```tut:fail:silent:book
object One {
  val a = b - 1
  val b = a + 1

  val answer = a + b
}
```

<div class="solution">
引っ掛け問題です! このコードは動作しません。ここでは `a` と `b` がそれぞれに対して定義されているため循環依存となり、解決されません。
</div>
