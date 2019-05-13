# 構造的再帰

In this chapter we see our first major pattern for structuring computations: *structural recursion over the natural numbers*. That's quite a mouthful, so let's break it down:

- By a pattern, we mean a way of writing code that is useful in lots of different contexts. We'll encounter structural recursion in many different situations throughout this book. 

- By the natural numbers we mean the whole numbers 0, 1, 2, and upwards. 

- By recursion we mean something that refers to itself. Structural recursion means a recursion that follows the structure of the data it is processing. If the data is recursive (refers to itself) then the structural recursion will also refer to itself. We'll see in more detail what this means in a moment.

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

