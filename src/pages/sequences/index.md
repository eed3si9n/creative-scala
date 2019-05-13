# 形と列と星

In this chapter we will learn how to build our own shapes out of the primitve lines and curves that make up the triangles, rectangles, and circles we've used so far. 
In doing so we'll learn how to represent sequences of data, and manipulate such sequences using higher-order functions that abstract over structural recursion. 
That's quite a lot of jargon, but we hope you'll see it's not as difficult as it sounds!

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
