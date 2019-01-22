# 置き換えモデルによる評価

私たちのプログラムが何を行っているのかを理解するためには Scala の式がどのように評価されるのかというメンタルモデルが必要となります。
これまでの所は、くだけたモデルで何とかやってきました。
この節では、**置き換えモデル**による評価を理解してもう少し形式化されたモデルにしていきます。
プログラミングの多くのこと同様に、気取った用語を使っていますがシンプルな概念です。
この場合、多分高校の数学で習ったことのある置き換えの話で、同じ考え方を新しい文脈に持ってきたものです。

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
