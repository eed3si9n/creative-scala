# メソッド

私たちは既にメソッドを使ってきました。メソッドを通じて私たちはオブジェクトと関わりを持つことができます。
この章では、独自のメソッドを書く方法を解説します。

名前は式を抽象化する方法を与えてくれます。
メソッドは式を抽象化して、**一般化**する方法を与えてくれます。
ここで一般化とは、関連する複数のもの (ここでは式) のグループを表現できる能力を指します。
メソッドは式のテンプレートを捉えて、その呼び手がメソッドのパラメータを渡すことでテンプレートの一部を補うことができます。

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
