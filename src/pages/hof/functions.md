## 関数

```tut:invisible
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
```

前の節でのエラーメッセージが言っているように、全てのメソッドは `_` を使って関数に変換することができ、その関数には同じパラメータを渡すことができます。

```tut:silent:book
// Parametric equation for rose with k = 7
def rose(angle: Angle) =
  Point.cartesian((angle * 7).cos * angle.cos, (angle * 7).cos * angle.sin)
```
```tut:book
rose _

(rose _)(0.degrees)
```

関数はだいたいメソッドと同じものですが、関数は第1級値として使うことができます:

- 関数を他のメソッドや関数への引数として渡すことができます
- メソッドや関数から戻り値として返すことができます
- `val` を使って名前を付けることができます

```tut:book
val roseFn = rose _
roseFn(0.degrees)
```

### 関数型

メソッドに関数を渡すためには、(パラメータを宣言するときには型を宣言する必要があるため) それらの型を書ける必要があります。

関数の型は `(A, B) => C` のように書き、このとき `A` と `B` はパラメータの型で、`C` は結果の型です。
このパターンは引数を取らない関数から、任意の引数の関数まで一般化することができます。

上の例では、`f` は 2つの `Int` をパラメータとして受け取り、`Int` を返す関数である必要があります。これは、`(Int, Int) => Int` と書くことができます。

<div class="callout callout-info">
#### 関数型宣言の構文 {-}

関数型を宣言するには、以下のように書きます:

```scala
(A, B, ...) => C
```

ここで

- `A, B, ...` は入力パラメータの型で
- `C` は結果の型

関数が 1つのパラメータのみ受け取る場合は括弧は省略することができます:

```scala
A => B
```
</div>


### 関数リテラル

関数にはリテラル構文があります。
例えば、以下は入力値に `42` を加算する関数です。

```tut:book
(x: Int) => x + 42
```

関数に引数を適用するのは通常通りに書くことができます。

```tut:book
val add42 = (x: Int) => x + 42
add42(0)
```

<div class="callout callout-info">
#### 関数リテラルの構文 {-}

関数リテラルの宣言構文は:

```scala
(parameter: type, ...) => 式
```

- 省略可能な `parameter` は、関数パラメータの名前
- `type` は関数パラメータの型
- `式` は関数の結果を決定する
</div>


### オブジェクトとしての関数

Scala はオブジェクト指向言語なので、全ての第1級値はオブジェクトです。
そのため関数はメソッドを持つことができ、それらを使って関数合成を行うことができます:

```tut:book
val addTen = (a: Int) => a + 10
val double = (a: Int) => a * 2
val combined = addTen andThen double // this composes the two functions
combined(5)
```

#### 練習問題 {-}

##### 関数型 {-}

上で定義された `roseFn` の型はなんでしょうか? この型は何を意味するでしょう?

<div class="solution">
型は `Angle => Point` です。これは `roseFn` が、`Angle` 型の単一のパラメータを受け取り、`Point` 型の値を返すことを意味します。別の言い方をすると、`roseFn` は `Angle` を `Point` へと変換します。
</div>

##### 関数リテラル {-}

`roseFn` を関数リテラルとして書いてみましょう。

<div class="solution">
```tut:book
val roseFn = (angle: Angle) =>
  Point.cartesian((angle * 7).cos * angle.cos, (angle * 7).cos * angle.sin)
```
</div>


