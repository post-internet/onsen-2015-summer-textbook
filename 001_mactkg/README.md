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
- Heroku
  - Heroku button

## Intro
Webアプリケーション使ってますか? 作ってますか? 遊べてますか?
数多くのリソースがインターネットにあり、それらを活用することで楽しくプログラミングすることができます。

NCC内でもコラボレーションのためにSlack, GitHub, Google DriveなどのWebアプリケーションを主に利用していますが、
Webサービスを使うにはGmailアカウントがあると便利です。オープンソースのソフトウェアを活用するならば、
GitHubの使い方や、その周辺のエコシステムについて知っていると大変便利です。

今回はそうしたWebサービスを活用して、プログラミングを楽しむための基礎知識について学びます。

## GitHub
- https://github.com/

バージョン管理システム「Git」を使うのに必要なサーバや便利サービスを提供しているホスティングWebサービス。
Gitで管理しているファイルのホスティングはもちろん、Pull Requestの仕組みやWiki、タスク管理のIssuesなど様々な機能を提供している。
Webhookなどを活用すれば、各種Webサービスや自分で作ったサービスなどとも連携させることができる。

### ぶらつきながら知るGitとGitHub
- https://github.com/post-internet/onsen-2015-summer-textbook
  - ファイルビューワ
    - 変更履歴(コミット)が見やすい!
    - Gitの機能だけど、見やすくデザインされている
    - コミットに対してコメントをつけられる
  - Issues
    - タスク管理
    - どういう問題か? だれが担当しているのか? どういう種類か? とかとか書いておける。
    - https://github.com/post-internet/onsen-2015-summer はIssuesを活用
  - Wiki
    - おなじみWiki
- https://github.com/processing/p5.js
  - Branch
    - master branch / develop branch
    - メイン / 開発 ブランチ
    - branchを変えると… ファイルが全部置き換わる
    - 新しい世界線を作れる。良ければ変更履歴をmasterへ、要らなければ捨てる!
  - Watch / Star
    - 気になるrepoはWatch! Issuesとかの議論がメールで通知、Dashboardにも流れます
    - Watchまでするほどじゃないけど「便利そう」なrepoはStar! ブクマ的に使えば便利

### ソフトウェアの利用者になってみる
- download as a zip
- releases
- clone to desktop
- meta file, other file, directory structure

## Docker
- Webアプリケーションを簡単に試す方法としてのDocker

 ## 開発者としてGitHubを使ってみる(?)
- GitHub App

