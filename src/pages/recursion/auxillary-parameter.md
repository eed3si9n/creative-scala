## 補助パラメータ

```tut:invisible
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
val aBox = Image.rectangle(20, 20).fillColor(Color.royalBlue)
```

ここまでで自然数の構造的再帰を使ったいくつもの面白いプログラムを見てきました。
このセクションでは**補助パラメータ**を使ってより複雑なプログラムを書くことを可能とする拡張をみていきます。
補助パラメータは再帰呼出しに他の情報を渡すための追加のパラメータのことです。

例えば、[@fig:recursion:growing-boxes] で示す線に沿って並ぶ次々と大きくなっていく箱のような絵を作ることを考えます。

![再帰のたびにサイズが大きくなる箱](./src/pages/recursion/growing-boxes.pdf+svg){#fig:recursion:growing-boxes}

どうやってこのイメージを作ることができるでしょう?

自然数の構造的再帰であることは分かっているので、骨組みはすぐに書くことができます。

```scala
def growingBoxes(count: Int): Image =
  count match {
    case 0 => base
    case n => unit add growingBoxes(n-1)
  }
```

`boxes` で色々やったことを活かすことでもう少し書くことができます。

```scala
def growingBoxes(count: Int): Image =
  count match {
    case 0 => Image.empty
    case n => Image.rectangle(???,???) beside growingBoxes(n-1)
  }
```

ここでつまずくのは、右に行くに従って箱のサイズを大きくする方法です。

トリッキーな方法としては、再帰ケースの順序を逆にして箱のサイズを `n` についての関数にすることです。コードはこうなります。

```tut:book
def growingBoxes(count: Int): Image =
  count match {
    case 0 => Image.empty
    case n => growingBoxes(n-1) beside Image.rectangle(n*10, n*10)
  }
```

補助パラメーターを使った解法に読み進める前にじっくり時間をかけて何故この方法でうまくいくのかを理解してください。

補助パラメーターを使った場合は、単に `growingBoxes` に現在の箱の大きさを指定するもう1つのパラメーターを追加するだけです。
再帰するときにこのサイズを変更します。
コードはこうなります。

```tut:book
def growingBoxes(count: Int, size: Int): Image =
  count match {
    case 0 => Image.empty
    case n => Image.rectangle(size, size) beside growingBoxes(n-1, size + 10)
  }
```

補助パラメータ法は 2つの利点があります。まず 1つの再帰から次で何が変わるのかだけを考えればいい (この場合、箱が大きくなる) のと、呼び出し側でこのパラメータを変更することができることです (例えば、最初の箱を大きくしたり小さくしたり)。

補助パラメータ法をみた所で、練習してみましょう。

#### 箱のグラデーション {-}

この練習問題では、[@fig:recursion:gradient-boxes] のような絵を描きます。
箱の線の描き方は分かっています。
ここでの課題は各ステップで色を変えることです。

ヒント: 各再帰で塗る色を `spin` することができます。

![ロイヤルブルーから始まって徐々に変化する色で塗られた 5つの箱](./src/pages/recursion/gradient-boxes.pdf+svg){#fig:recursion:gradient-boxes}

<div class="solution">
解答を実装する 2通りの方法があります。
補助パラメーター法は `gradientBoxes` にパラメーターを追加して `Color` を構造的再帰に渡してまわる方法です。

```tut:book
def gradientBoxes(n: Int, color: Color): Image =
  n match {
    case 0 => Image.empty
    case n => aBox.fillColor(color) beside gradientBoxes(n-1, color.spin(15.degrees))
  }
```

`growingBoxes` の例で行ったように塗るための色を `n` に関する関数にすることもできます。

```tut:book
def gradientBoxes(n: Int): Image =
  n match {
    case 0 => Image.empty
    case n => aBox.fillColor(Color.royalBlue.spin((15*n).degrees)) beside gradientBoxes(n-1)
  }
```
</div>

#### 同心円 {-}

バリエーションとして、[@fig:recursion:concentric-circles] のような同心円を描いてみましょう。ここでは、色ではなく各ステップでサイズを変えています。その他はパターンとしては同じようになるはずです。実装してみてください。

![ロイヤルブルー色の同心円](./src/pages/recursion/concentric-circles.pdf+svg){#fig:recursion:concentric-circles}

<div class="solution">
これはほとんど `growingBoxes` と同じものです。

```tut:book
def concentricCircles(count: Int, size: Int): Image =
  count match {
    case 0 => Image.empty
    case n => Image.circle(size) on concentricCircles(n-1, size + 5)
  }
```
</div>

#### もう一度、気持ちを込めて {-}

今度は両方のテクニックを組み合わせて各ステップでサイズと色を変更して、[@fig:recursion:colorful-circles] で得られるような絵を描いてみましょう。
自分の好みになるまで色々実験してみてください。

![面白いカラーバリエーションの同心円](./src/pages/recursion/colorful-circles.pdf+svg){#fig:recursion:colorful-circles}

<div class="solution">
これが私たちの解法で、コードの繰り返しを減らすために問題を再利用可能なパーツへと分けてみました。
これでもまだ多くの繰り返しがありますが、それらを減らすためのツールは後ほど見ていきます。

```tut:book
def circle(size: Int, color: Color): Image =
  Image.circle(size).lineWidth(3.0).lineColor(color)

def fadeCircles(n: Int, size: Int, color: Color): Image =
  n match {
    case 0 => Image.empty
    case n => circle(size, color) on fadeCircles(n-1, size+7, color.fadeOutBy(0.05.normalized))
  }

def gradientCircles(n: Int, size: Int, color: Color): Image =
  n match {
    case 0 => Image.empty
    case n => circle(size, color) on gradientCircles(n-1, size+7, color.spin(15.degrees))
  }

def image: Image =
  fadeCircles(20, 50, Color.red) beside gradientCircles(20, 50, Color.royalBlue)
```
</div>
