## リテラル式

Scala の様々な式を探検してみましょう。初めは、最もシンプルな式である**リテラル**です。
リテラル式の具体例です:

```tut:book
3
```

リテラルは「それ自身」に評価されます。式の書き方と console が値を表示する方法は一緒です。ただし、値の書かれた表現とコンピューターのメモリ内の実際の表現には違いがあることを思い出してください。

Scala には多くの異なる形のリテラルがあります。私たちは既に `Int` リテラルを見ました。**浮動小数点数**という別の型があり、これは別のリテラル構文で表されます。これは、コンピューターの実数に対する近似値に対応します。具体例を見てみましょう:

```tut:book
0.1
```

見ての通り、この型は `Double` と呼ばれます。

数字はいいとして、テキストはどうするのでしょうか。Scala の `String` 型は文字の列を表します。文字リテラルはダブルクォートで内容を囲んで書きます。

```tut:book
"To be fond of dancing was a certain step towards falling in love."
```

数行にまたがった文字列を書きたいことがあります。これは、以下のようにトリプルダブルクォートを使って書きます。

```tut:book
"""
A new, a vast, and a powerful language is developed for the future use of analysis,
in which to wield its truths so that these may become of more speedy and accurate
practical application for the purposes of mankind than the means hitherto in our
possession have rendered possible.

  -- Ada Lovelace, the world's first programmer
"""
```

`String` は文字の列です。文字それぞれにも `Char` という型があって、それはシングルクォートを使って書かれます。

```tut:book
'a'
```

最後に、イギリスの論理学者 [George Boole](https://ja.wikipedia.org/wiki/%E3%82%B8%E3%83%A7%E3%83%BC%E3%82%B8%E3%83%BB%E3%83%96%E3%83%BC%E3%83%AB) にちなんで名付けられた `Boolean` 型のリテラル表現を見ていきましょう。気取った名前ですが、値が `true` か `false` であるかというだけの意味で、ブールリテラルはそのまま以下のように書かれます。

```tut:book
true
false
```

リテラル式を使って値を作ることができるようになりましたが、何らかの方法で値と関わりを持つことができなければ何もできません。これまでに `1 + 2` というような複合式は見てきました。次の節ではオブジェクトとメソッドを習って、このような式やもっと面白い式が動作しているのかを理解します。
