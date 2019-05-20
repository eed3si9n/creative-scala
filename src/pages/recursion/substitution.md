## 再帰に関する論理的な考察

```tut:invisible
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
```

自然数の構造的再帰の達人となりました。
ここで置き換えモデルへと戻ってこれが新しいツールである再帰と使えるか見てみましょう。

置き換えは、式の値を分かっている値と置き換えることができると言っていることを思い出してください。
メソッド呼び出しの場合は、メソッドの本文をパラメータを適当に名前を変えることで置き換えることができました。

再帰の最初の例は、以下のように書かれた `boxes` でした。

```tut:silent
val aBox = Image.rectangle(20, 20).fillColor(Color.royalBlue)

def boxes(count: Int): Image =
  count match {
    case 0 => Image.empty
    case n => aBox beside boxes(n-1)
  }
```

`boxes(3)` に対して置き換えを使うことで何が得られるか見てみましょう。

まず最初の置き換えは

```tut:silent
boxes(3)
// Substitute body of `boxes`
3 match {
  case 0 => Image.empty
  case n => aBox beside boxes(n-1)
}
```

となります。`match` 式の評価と置き換え方を知っているので、以下が得られます。

```tut:silent
3 match {
  case 0 => Image.empty
  case n => aBox beside boxes(n-1)
}
// Substitute right-hand side expression of `case n`
aBox beside boxes(2)
```

次に `boxes(2)` を置き換えて以下を得られます。

```tut:silent
aBox beside boxes(2)
// Substitute body of boxes
aBox beside {
  2 match {
    case 0 => Image.empty
    case n => aBox beside boxes(n-1)
  }
}
// Substitute right-hand side expression of `case n`
aBox beside {
  aBox beside boxes(1)
}
```

この過程を何度か繰り返すことで以下を得られます。

```tut:silent
aBox beside {
  aBox beside {
    1 match {
      case 0 => Image.empty
      case n => aBox beside boxes(n-1)
    }
  }
}
// Substitute right-hand side expression of `case n`
aBox beside {
  aBox beside {
      aBox beside boxes(0)
  }
}
// Substitute body of boxes
aBox beside {
  aBox beside {
    aBox beside {
      0 match {
        case 0 => Image.empty
        case n => aBox beside boxes(n-1)
      }
    }
  }
}
// Substitute right-hand side expression of `case 0`
aBox beside {
  aBox beside {
    aBox beside {
      Image.empty
    }
  }
}
```

最後の結果を単純化したもの

```tut:silent
aBox beside aBox beside aBox beside Image.empty
```

はまさに私たちが期待するものと同じものです。
そのため、置き換えは再帰に関しても論理的に考察することができると言うことができます。
これは素晴らしいことです!
しかし、置き換えは書き出さなければかなり複雑でついていくのが難しくなります。
再帰そのものは正しいと仮定して、各ステップでの何を導入しているかだけを考えるのが再帰について考察する実践的な方法です。

例えば、`boxes` を論理的に考察する場合、

```tut:silent
def boxes(count: Int): Image =
  count match {
    case 0 => Image.empty
    case n => aBox beside boxes(n-1)
  }
```

調べるだけで基底ケースが正しいことが分かります。
再帰ケースを見たときに、`boxes(n-1)` は正しいと**仮定**します。
そして、「再帰ケースで行っていることは正しくて、再帰そのものが正しいでしょうか?」と問います。
答はイエスです。再帰の `boxes(n-1)` が `n-1`個の箱を一列に作ったとき、もう1つの箱を頭に並べるのは正しいことだからです。
この方法を使った論理思考は置き換えを使ったときよりも非常に簡潔で**かつ**構造的再帰を使っているならば正しいことが保証されています。

### 練習問題 {-}

以下は少し現実味に欠ける構造的再帰の例です。
これらのメソッドが、言っていることを実際に行うかを**実行せずに**確認しましょう。

```tut:silent
// 自然数が与えられたとき、その数を返します。
// Examples:
//   identity(0) == 0
//   identity(3) == 3
def identity(n: Int): Int =
  n match {
    case 0 => 0
    case n => 1 + identity(n-1)
  }
```

<div class="solution">
もちろんです!
基底ケースは率直なものです。
再帰ケースを見るときは、`identify(n-1)` が `n-1` の identity (`n-1` そのものです) を返すことを仮定します。その場合、`n` の identity は `1 + identity(n-1)` です。
</div>

```tut:silent
// 自然数が与えられたとき、その倍を返します。
// Examples:
//   double(0) == 0
//   double(3) == 6
def double(n: Int): Int =
  n match {
    case 0 => 0
    case n => 2 * double(n-1)
  }
```

<div class="solution">
これはダメです!
このメソッドは 2つの異なる形で壊れています。
まず第一に、再帰ケースで掛け算を行っているため、いずれはゼロの基底ケースで掛け算する必要があり、全体の結果もゼロとなります。

これを修正するために、`1` のケースを追加を試みることはできます (そして構造的再帰の骨組みがどうしてうまくいかなったのかを疑問に思うかもしれません)。

```tut:silent
def double(n: Int): Int =
  n match {
    case 0 => 0
    case 1 => 1
    case n => 2 * double(n-1)
  }
```

しかし、これも正しい結果を返しません! 再帰ケースで間違ったことを行ってます。掛ける代わりに足し算を行うべきです。

代数の復習をしてみましょう:

```scala
2(n-1 + 1) == 2(n-1) + 2
```

そのため、`double(n-1)` が `2(n-1)` ならば、私たちは 2 を掛けるのでは無く、2 を**足す**べきです。
正しいメソッドは

```tut:silent
def double(n: Int): Int =
  n match {
    case 0 => 0
    case n => 2 + double(n-1)
  }
```
</div>

です。
