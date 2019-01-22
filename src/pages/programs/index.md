# 大きなプログラムの書き方

console にプログラムを打ち込んでいくのが辛くなってきています。
この章では、より大きなプログラムを書くためのツールを解説します:

- プログラムをファイルに保存することで、何度も繰り返してコードを書き込む必要が無くなります。
- 値に名前を与えることで再利用できるようにします。

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

