# 絵を使った演算

これまで数、文字列、その他の簡単なオブジェクトを使った演算をみてきました。これらは特に面白いものではありません。ここからは絵を使った計算に注目して、後ほどアニメーションをみていきます。絵を使うことは、より直接的にクリエイティブな機会を与えてくれます。自分たちのプログラムからリアルなアウトプットが出てくる感覚は他の方法では得られないことです。

私たちはグラフィックスを作るのに Doodle というライブラリを使います。この章では Dooble の初歩を習います。

<div class="callout callout-info">
Doodle 内の sbt console を使って練習問題を実行すると普通に動作すると思います。そうじゃない場合で Dooble を使うときは、コードに始めに以下の import 文を書く必要があります。

```tut:silent
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
```
</div>
