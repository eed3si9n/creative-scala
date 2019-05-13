# ジェネレーティブアートの合成

In this chapter we'll explore techniques from generative art, which will in turn allow us to explore key concepts for functional programming. We'll see:

- uses for `map` and `flatMap` that go beyond manipulating collections of data that we've seen in the previous chapters; 
- how we can handle side effects without breaking substitution; and
- some interesting, and possibly beautiful, images that combine elements of structure and randomness.

![An example image generated using the techniques in this chapter](./src/pages/generative/volcano.png){#fig:generative:volcano}

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

