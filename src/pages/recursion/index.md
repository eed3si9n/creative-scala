# 構造的再帰

この章では、構造的計算における最初のメジャーなパターンである**自然数の構造的再帰**をみていきます。ちょっと大げさな感じなので分解してみましょう。

- パターンとは、多くの異なる場面で役立つコードの書き方を意味します。この本の色々な場面で構造的再帰が出てきます。
- 自然数とは 0、1、2、それ以上の整数を指します。
- 再帰とは、何かが自分自身を参照することを意味します。構造的再帰は、処理しているデータの構造に沿った再帰という意味です。もしデータが再帰的 (自身を参照している) ならば構造的再帰も自分自身を参照します。これが何を意味をするのかはこれからより詳しくみていきます。

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

