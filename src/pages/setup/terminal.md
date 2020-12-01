## ターミナルとテキストエディタのインストール

この節では、プログラミングが初めてという人のために私たちがお勧めする、ターミナルとテキストエディタを使った Creative Scala のセットアップ方法を解説します。
インストールするものは:

- JVM
- Git
- テキストエディタ、1つ
- Creative Scala のテンプレートプロジェクト

### macOS

ターミナルを開く。(ツールバー右側の虫めがねアイコンに "terminal" と打ち込んでください。)

Java をインストールします。
ターミナルに以下を打ってください:

```bash
java
```

もしもこれが実行されれば Java はインストール済みです。
インストールされていなければ、Java をインストールする画面が表示されます。

Homebrew をインストールします。
以下をターミナルにペーストしてください:

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Homebrew を使って `git` をインストールします。
ターミナルに以下を打ち込んでください:

```bash
brew install git
```

次にテキストエディタ Atom をインストールします。
ターミナルに以下を打ち込んでください:

```bash
brew install Caskroom/cask/atom
```

Atom 内で Scala サポートをインストールしてください: Settings > Install > language-scala

これで Git を使って Creative Scala の作業をするための sbt プロジェクトを取得することができます。
以下を打ち込んでください:

```bash
git clone https://github.com/underscoreio/creative-scala-template.git
```

<div class="callout callout-info">
#### 作業を共有する {-}

代替のセットアップとして、先に Creative Scala template プロジェクトを fork して、それをあなたのコンピュータに clone するという方法があります。
これは、あなたが自分の作業結果を他の人とシェアしたい場合のセットアップで、例えばリモートのインストラクターと Creative Scala を受講している場合や、自分の作業を他の人にも見て欲しい場合に使うことができます。

このセットアップではまず Creative Scala template を **fork** します。
そして**あなたの** fork を clone します。
この代替セットアップ方法に関してはこの章の後ほどの GitHub の節で解説します。
</div>

今作られたディレクトリに変えて、sbt を実行してみましょう。

```bash
cd creative-scala-template
./sbt.sh
```

sbt が起動したはずです。
sbt 内で `console` と打ち込んでみてください。
最後に、以下を打ち込んでください:

```scala
Example.image.draw()
```

3つの円の画像が現れるはずです!

ここまでできれば、Creative Scala の作業をするのに必要なソフトウェアは全てインストールされました。

最後のステップは Atom を起動して、`src/main/scala` 内にある `Example.scala` を開くことです。

### Windows

Java をダウンロードしてインストールします。
"JDK" (Java development kit) で検索してください。
Oracle のサイトが出てくるはずです。
ライセンスを承諾して、JDK をダウンロードしてください。
ダウンロードが完了したら、インストーラーを実行してください。

Atom をダウンロードしてインストールします。
`https://atom.io/` へ行って、Atom の Windows版をダウンロードしてください。
ダウンロードが完了したら、インストーラーを実行してください。

Git をダウンロードしてインストールします。
`https://git-scm.com/` へ行って、Git の Windows版をダウンロードしてください。
ダウンロードが完了したら、インストーラーを実行してください。
最後に Git を開くオプションが出てくるはずなので、それを選択してください。
コマンドプロンプト画面が開きます。
以下を打ち込んでください:

```bash
git clone https://github.com/underscoreio/creative-scala-template.git
```

<div class="callout callout-info">
#### 作業を共有する {-}

代替のセットアップとして、先に Creative Scala template プロジェクトを fork して、それをあなたのコンピュータに clone するという方法があります。
これは、あなたが自分の作業結果を他の人とシェアしたい場合のセットアップで、例えばリモートのインストラクターと Creative Scala を受講している場合や、自分の作業を他の人にも見て欲しい場合に使うことができます。

このセットアップではまず Creative Scala template を **fork** します。
そして**あなたの** fork を clone します。
この代替セットアップ方法に関してはこの章の後ほどの GitHub の節で解説します。
</div>

コマンドプロンプトを開きます。
画面左下の Windows アイコンをクリックしてください。
検索ボックスに「cmd」と入力して、プログラムが出てきたらそれを実行してください。
開いたウィンドウ内で以下を打ち込んでください:

```bash
cd creative-scala-template
```

さっきダウンロードした Create Scala template プロジェクトのディレクトリに変わるはずです。
以下を打ち込んで sbt を起動してください:

```bash
sbt.bat
```

sbt 内で `console` と打ち込んでみてください。
最後に、以下を打ち込んでください:

```scala
Example.image.draw()
```

3つの円の画像が現れるはずです!

ここまでできれば、Creative Scala の作業をするのに必要なソフトウェアは全てインストールされました。

最後のステップは Atom を起動して、`src/main/scala` 内にある `Example.scala` を開くことです。

### Linux

macOS の手順に従って、Homebrew の代わりに使っているディストリビューションのパッケージマネジャーを使ってください。
