## GitHub

Creative Scala の作業をするのに必要なコードを全てセットアップした[テンプレート]を用意しました。
このテンプレートは、コードを共有するためのウェブサイトである [GitHub][github] に保存されています。

このテンプレートをあなたのコンピュータにコピーすることができ、Git はこれを clone と呼んでいます。
しかし、この方法ではあなたが行う変更を GitHub へ保存し直して、他の人が見れるようにはなりません。

もしもあなたが行う変更を他の人と共有したい場合は、テンプレートプロジェクトを自分の GitHub へコピーする必要があります。
Git はこれを fork と呼んでいます。
GitHub 上でまずレポジトリを fork して、**あなたの fork** を手元に clone してください。
この方法を行えば、自分の変更点を GitHub のあなたの fork へ保存し直すことができます。

このプロセスを始めるには、GitHub アカウントを作成する必要があります。アカウントを持っていない人は作ってください。

アカウントができたら、ブラウザ上から[テンプレートプロジェクト](https://github.com/underscoreio/creative-scala-template)へ行きます。
上の右側に "Fork" と書かれたボタンがあります。
このボタンを押して、自分のテンプレートプロジェクトを作成します。
テンプレートの自分の fork を表示するウェブページに飛ばされるはずです。
このリポジトリの名前を覚えておいてください。GitHub のユーザー名が `yourname` さんだとすると、`yourname/creative-scala-template` みたいな感じになると思います。

あなたの fork を clone するには、以下のコマンドの `yourname` の部分を GitHub ユーザー名に置き換えて実行するだけです。

```bash
git clone git@github.com:yourname/creative-scala-template.git
```

これで、あなたが行う変更を GitHub 上のあなたの fork へ送ることができるようになりました。
Git を使ってこれを行うのは少し複雑です。
何らかの変更を行ったあと以下を実行する必要があります:

  - `add` Git のインデックスと呼ばれるものに変更を追加する
  - `commit` 変更を記録する
  - `push` 変更を fork へ送信する

以下は、コマンドラインからこれを実行する一例です。

```bash
git add
git commit -m "Explain here what you did"
git push
```

GitHub は、[GitHub Desktop](https://desktop.github.com/) という Git を使うための無料のグラフィカルなツールを作っています。
Git を始めたばかりのときはこれが一番分かりやすい方法でしょう。

[github]: https://github.com/
[template]: https://github.com/underscoreio/creative-scala-template
