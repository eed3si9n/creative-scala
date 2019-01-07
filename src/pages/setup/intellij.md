## IntelliJ

IntelliJ は、Scala や他のプログラミング言語のための統合開発環境 (IDE) です。これはいくつものプログラミングツールを 1つのアプリケーションにまとめたもので、Visual Studio や Eclipse など他の IDE に慣れている人にお勧めします。

まずは IntelliJ Scala Bundle を[ダウンロード][intellij-scala-bundle]してください。

IntelliJ Scala Bundle を起動すると、Welcome to IntelliJ IDEA というダイアログが出てきて、Create New Project、Import Project、Open、Check out from Version Control という選択肢が表示されます。
「Check out from Version Control」を選択して、URL に `https://github.com/underscoreio/creative-scala-template.git` と入力して、「Clone」を選びます。
「Would you like to open it?」と聞かれたら「Yes」を選びます。

オプション画面が出てきたら「Use sbt shell for build and import」選択します。

画面左下の sbt shell に `console` と打ち込んでください。
`(busy) >` と書かれたプロンプトに以下を打ち込んでください:

```scala
Example.image.draw
```

3つの円の画像が現れるはずです!

```scala
:q
```

と打ち込んで `console` を終了します。

[intellij-scala-bundle]: https://github.com/JetBrains/intellij-scala-bundle/releases
