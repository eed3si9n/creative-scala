\appendix

# 構文のクイックレファレンス {#syntax-quick-reference}

## リテラルと式

~~~ scala
// リテラル:
123      // Int
123.0    // Double
"Hello!" // String
true     // Boolean

// 算数:
10 + 2   // Int + Int    = Int
10 + 2.0 // Int + Double = Double
10 / 2   // Int / Int    = Double

// Boolean 論理演算:
true && false // logical AND
true || false // logical OR
!true         // logical NOT

// String の連結:
"abc" + "def" // String
"abc" + 123   // auto-conversion from Int to String

// メソッド呼び出しと中置演算子:
1.+(2)    // method call style
1 + 2     // infix operator style
1 + 2 + 3 // equivalent to 1.+(2).+(3)

// 条件式:
if(booleanExpression) expressionA else expressionB

// ブロック:
{
  sideEffectExpression1
  sideEffectExpression2
  resultExpression
}
~~~

## 値とメソッド宣言

~~~ scala
// 値の宣言構文:
val valueName: SomeType = resultExpression // 明示的な型を用いた宣言
val valueName = resultExpression           // 型推論を用いた宣言

// パラメータリストと明示的な結果型を持ったメソッド:
def methodName(argName: ArgType, argName: ArgType): ReturnType =
  resultExpression

// パラメータリストと推論された結果型を持ったメソッド:
def methodName(argName: ArgType, argName: ArgType) =
  resultExpression

// (ブロックを用いた) 複合式のメソッド:
def methodName(argName: ArgType, argName: ArgType): ReturnType = {
  sideEffectExpression1
  sideEffectExpression2
  resultExpression
}

// パラメータリストを持たないメソッド:
def methodName: ReturnType =
  resultExpression

// パラメータリストを持ったメソッドの呼び出し:
methodName(arg, arg)

// パラメータリストを持たないメソッドの呼び出し:
methodName
~~~

## 値としての関数

関数値は `(argName: ArgType, ...) => resultExpression` として書く:

~~~ scala
val double = (num: Int) => num * 2
// double: Int => Int = <function1>

val sum = (a: Int, b: Int) => a + b
sum: (Int, Int) => Int = <function2>
~~~

複数行に渡る関数はブロック式を用いて書く:

~~~ scala
val printAndDouble = (num: Int) => {
  println("The number was " + num)
  num * 2
}
// printAndDouble: Int => Int = <function1>

scala> printAndDouble(10)
// The number was 10
// res0: Int = 20
~~~

パラメータや戻り型として関数を使う場合は、関数の型を書く必要がある。
その構文は `ArgType => ResultType` もしくは `(ArgType, ...) => ResultType`:

~~~ scala
def doTwice(value: Int, func: Int => Int): Int =
  func(func(value))
// doTwice: (value: Int, func: Int => Int)Int

doTwice(1, double)
// res0: Int = 4
~~~

関数値は通常の式として、他の式の一部としてそのまま書くことができる。

~~~ scala
doTwice(1, (num: Int) => num * 10)
// res1: Int = 100
~~~

引数の型は、コンパイラが推論可能ならば省略することができる:

~~~ scala
doTwice(1, num => num * 10)
// res2: Int = 100
~~~

## Doodle レファレンス・ガイド

### インポート文

~~~ scala
// これらの import があれば大丈夫:
import doodle.core._
import doodle.syntax._
~~~

### イメージの作成

~~~ scala
// プリミティブ・イメージ (黒の輪郭、塗りつぶし無し):
val i: Image = Circle(radius)
val i: Image = Rectangle(width, height)
val i: Image = Triangle(width, height)

// 演算子構文で書かれた複合イメージ:
val i: Image = imageA beside imageB // 水平に隣接
val i: Image = imageA above  imageB // 垂直に隣接
val i: Image = imageA below  imageB // 垂直に隣接
val i: Image = imageA on     imageB // 重ね合わせ
val i: Image = imageA under  imageB // 重ね合わせ

// メソッド呼び出し構文で書かれた複合イメージ:
val i: Image = imageA.beside(imageB)
// etc...
~~~

### イメージのスタイリング

~~~ scala
// 演算子構文で書かれたイメージのスタイリング:
val i: Image = image fillColor color   // 新しい塗りつぶし (輪郭は変化なし)
val i: Image = image lineColor color   // 新しい輪郭の色 (塗りつぶしは変化なし)
val i: Image = image lineWidth integer // 新しい輪郭の幅 (塗りつぶしは変化なし)
val i: Image = image fillColor color lineColor otherColor // 新しい塗りつぶしと輪郭

// メソッド呼び出し構文で書かれたイメージのスタイリング:
val i: Image = imageA.fillColor(color)
val i: Image = imageA.fillColor(color).lineColor(otherColor)
// etc...
~~~

### 色

~~~ scala
// 基本色:
val c: Color = Color.red                       // 定義済みの色
val c: Color = Color.rgb(255.uByte, 127.uByte, 0.uByte)          // RGB color
val c: Color = Color.rgba(255.uByte, 127.uByte, 0.uByte, 0.5.normalized)    // RGBA color
val c: Color = Color.hsl(15.degrees, 0.25.normalized, 0.5.normalized)       // HSL color
val c: Color = Color.hsla(15.degrees, 0.25.normalized, 0.5.normalized, 0.5.normalized) // HSLA color

// 演算子構文で書かれた色の変換/混合:
val c: Color = someColor spin       10.degrees     // 色相の変化
val c: Color = someColor lighten    0.1.normalized // 明度の変化
val c: Color = someColor darken     0.1.normalized // 明度の変化
val c: Color = someColor saturate   0.1.normalized // 彩度の変化
val c: Color = someColor desaturate 0.1.normalized // 彩度の変化
val c: Color = someColor fadeIn     0.1.normalized // 透明度の変化
val c: Color = someColor fadeOut    0.1.normalized // 透明度の変化

// メソッド呼び出し構文で書かれた色の変換/混合:
val c: Color = someColor.spin(10.degrees)
val c: Color = someColor.lighten(0.1.normalized)
// etc...
~~~

### パス

~~~ scala
// PathElements のリストからのパスの作成:
val i: Image = OpenPath(List(
  MoveTo(Vec(0, 0).toPoint),
  LineTo(Vec(10, 10).toPoint)
))

// PathElements の列からのパスの作成:
val i: Image = OpenPath(
  (0 until 360 by 30) map { i =>
    LineTo(Vec.polar(i.degrees, 100).toPoint)
  }
)

// 要素の種類:
val e1: PathElement = MoveTo(toVec.toPoint)                        // 線なし
val e2: PathElement = LineTo(toVec.toPoint)                        // 直線
val e3: PathElement = BezierCurveTo(cp1Vec.toPoint, cp2Vec.toPoint, toVec.toPoint) // 曲線

// 注意: 最初の要素が MoveTo じゃない場合は、MoveTo に変換される
~~~

### Angle と Vec

~~~ scala
val a: Angle = 30.degrees                // 度数による角度
val a: Angle = 1.5.radians               // ラジアンによる角度
val a: Angle = math.Pi.radians           // π ラジアン
val a: Angle = 1.turns                   // 回転数による角度

val v: Vec = Vec.zero                    // ゼロベクトル (0,0)
val v: Vec = Vec.unitX                   // x 単位ベクトル (1,0)
val v: Vec = Vec.unitY                   // y 単位ベクトル (0,1)

val v: Vec = Vec(3, 4)                   // デカルト座標からのベクトル
val v: Vec = Vec.polar(30.degrees, 5)    // 極座標からのベクトル
val v: Vec = Vec(2, 1) * 10              // 長さの乗算
val v: Vec = Vec(20, 10) / 10            // 長さの除算
val v: Vec = Vec(2, 1) + Vec(1, 3)       // ベクトルの加算
val v: Vec = Vec(5, 5) - Vec(2, 1)       // ベクトルの減算
val v: Vec = Vec(5, 5) rotate 45.degrees // 反時計回りの回転

val x: Double = Vec(3, 4).x              // x 座標
val y: Double = Vec(3, 4).y              // y 座標
val a: Angle  = Vec(3, 4).angle          // (1, 0) から反時計回り
val l: Double = Vec(3, 4).length         // 長さ
~~~
