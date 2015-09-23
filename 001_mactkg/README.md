# GitHubユーザーになる方法

## この講義で知れること

- 簡単なセキュリティ知識
  - パスワード
  - 公開鍵認証
  - 二段階認証
- おもしろ情報の入手先
- 便利サイト
- GitHubの使い方
  - markdown
  - コマンド: git pull, commit, push, clone
  - CUI is required?: sourcetree or GitHub.app
  - メタファイル: package.json, GemFile, gulpfile.js, bower.json, gruntfile.js
  - その他のファイル: .gitignore, .travis.yml, license.txt
  - ディレクトリ構造: bin, lib, src, docs, dist, examples
  - 便利な機能: Issues, Pull Request, Wiki, Watches, Star, Pages
  - example: p5.js(募集中)
- Dockerfile?
  - Dockerって何?
  - Docker Toolbox
  - VM
  - VirtualBox
  - Why docker?

## Intro
Webアプリケーション使ってますか? 作ってますか? 遊べてますか?
数多くのリソースがインターネットにあり、それらを活用することで楽しくプログラミングすることができます。

NCC内でもコラボレーションのためにSlack, GitHub, Google DriveなどのWebアプリケーションを主に利用していますが、
Webサービスを使うにはGmailアカウントがあると便利です。オープンソースのソフトウェアを活用するならば、
GitHubの使い方や、その周辺のエコシステムについて知っていると大変便利です。

今回はそうしたWebサービスを活用して、プログラミングを楽しむための基礎知識について学びます。

## GitHub
- https://github.com/

バージョン管理システム「Git」を使うのに必要なサーバや便利サービスを提供しているホスティングWebサービスです。
Gitで管理しているファイルのホスティングはもちろん、Pull Requestの仕組みやWiki、タスク管理のIssuesなど様々な機能を提供しています。
Webhookなどを活用すれば、各種Webサービスや自分で作ったサービスなどとも連携させることができます。

### ぶらつきながら知るGitとGitHub
- https://github.com/post-internet/onsen-2015-summer-textbook へ
  - ファイルビューワ
    - 変更履歴(コミット)が見やすい!
    - Gitの機能だけど、見やすくデザインされている
    - コミットに対してコメントをつけられる
  - Issues
    - タスク管理
    - どういう問題か? だれが担当しているのか? どういう種類か? とかとか管理できる。
    - 多くの問題をそこで議論できる。Issue同士の関係も、Issue番号を #9 のように書くことで表現可能
    - https://github.com/post-internet/onsen-2015-summer はIssuesを活用したので御覧ください
  - Wiki
    - おなじみWiki

ここまではただのプロジェクト向け便利ツールとしてのGitHubの一面や、ファイル置き場としてのGitHubを紹介しました

- https://github.com/processing/p5.js へ
  - Branch
    - master branch / topic branch
    - メイン / トピックブランチ(ある機能をつけるためのbranch)
    - branchを変えると... ファイルが全部置き換わる
    - 新しい世界線を作れる。あるコミットを基点に変更を混ぜないようにする。
    - 良ければ変更履歴をmasterへmerge(Pull Request)、要らなければ捨てる!
    - このbranchではどんな作業をしているのか? を名前につける
  - Tags
    - commitに対してtagを付けられる
    - 例えば「このコミットでv1.2です!」とか
    - 区切りを示せる
  - Watch / Star
    - 気になるrepoはWatch! Issuesとかの議論がメールで通知、Dashboardにも流れます
    - Watchまでするほどじゃないけど「便利そう」なrepoはStar! ブクマ的に使えば便利

### ソフトウェアの利用者になってみる
- releases
  - きちんと管理されていればこれが活用されている
  - 「v1.2」とか「v4.0.0」とか欲しければこういうとこからダウンロード
  - 先述した「Tag」をつけると自動的にreleaseとして認識される
  - releaseに対して.zipファイルなどを紐付けることができる。
- download as a zip
  - repoをzipとしてダウンロードする
  - GitHubを使ったことがない人にrepoの中身を落としてきてほしいときに便利
- clone to desktop
  - GitHubのアプリを使ってデスクトップにclone
  - cloneすると落ちてくるものは...
    - ファイル一式
    - 今までの変更履歴
      - だれが、いつ、どんな変更をしたかと、少しのメッセージ
    - branchの情報
    - というイメージで良いと思います

> gitの中身の話をするともう少し実装は工夫して作られていて、それはそれで面白いです。
> そういったことに興味が出てきたら調べてみると良いと思います。解説スライドはたくさんあります。
> でもこんがらがるかなと思うので、まずはこんな感じの理解でいいです。

- meta file, other file, directory structure
  - repoにある「.gitignore」「package.json」「Gruntfile.js」って何?
  - それぞれどんな役割のものがあるのか。
  - docs, example, lib, src, testってそれぞれディレクトリの意味は?

## Docker
- Dockerとは
- 仮想化技術
- Webアプリケーションを簡単に試す方法としてのDocker
- Kitematicの使い方
