# 自分の代数的データ型

In this chapter we'll learn how to create our own algebraic data types, and bring together all the tools we've seen far.

So far in Creative Scala we've used (algebraic) data types provided by Scala or Doodle, such as `List` and `Point`. In this section we'll learn how to create our own algebraic data types in Scala, opening up new possibilities for the type of programs we can write.

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
