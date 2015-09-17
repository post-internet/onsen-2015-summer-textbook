# 事前準備

**注意**

2015/09/17現在，nodejsの最新バージョンはv4.1.0です．

適宜，最新バージョンに読み替えてインストールしてください．

## TOC
- [Nodejsの準備](#nodejsの準備)
  - [Windowsの場合](#windows-の場合)
  - [Homebrewの場合](#mac-with-homebrew-の場合)
  - [UNIXの場合](#macやlinux-の場合)
- [エディタの準備](#エディタ)
- [テンプレートファイル](#テンプレートファイル)

## Nodejsの準備
Nodejsは[nodejs.org](https://nodejs.org/en/)からダウンロードできますが，バージョンによって仕様が違うため，バージョン管理ソフトウェアからインストールすると便利です．

### Windows の場合

#### 1. ダウンロードする
[marcelklehr/nodist](https://github.com/marcelklehr/nodist)を使います．

まず，[marcelklehr/nodist](https://github.com/marcelklehr/nodist)にアクセスして ``Download Zip`` からダウンロードします．

zipファイルがダウンロードできるので，その中身を ``C:\nodist`` に展開しましょう．

#### 2. 環境変数を設定する
環境変数の **PATH** に ``C:\nodist\bin;`` ， **NODIST_PREFIX** に ``C:\nodist`` を追加します．

環境変数の追加の仕方は，[ここ](https://github.com/uzulla/how_to_setup_path_on_windows)が参考になります．

#### 3. Nodejsの最新版をダウンロードする
管理者権限でコマンドプロンプトを起動しましょう．

参考：[Windows - 管理者権限でコマンドプロンプトを起動するショートカット - Qiita](http://qiita.com/takuya0301/items/df6cde3bbaf9e13ef8f0)

nodistを使って最新版をインストールします．
```bash
nodist selfupdate
```

使われるNodejsのバージョンを確認します．
```bash
nodist
# > nodev4.0.0 (global)
node -v
# v4.0.0
```
バージョンが``v4.0.0``であれば完了です．

### mac with Homebrew の場合
参考：[Node.jsの管理をHomebrewからnodebrewに変える - Qiita](http://qiita.com/takeshi81/items/805f504503cd93151ca6)
```bash
brew install nodebrew
echo 'export PATH=$HOME/.nodebrew/current/bin:$PATH' >> ~/.bashrc
source ~/.bashrc

nodebrew install-binary stable
nodebrew use v4.0.0
nodebrew alias default v4.0.0

nodebrew ls
# current: v4.0.0
```

### macやLinux の場合
```bash
curl -L git.io/nodebrew | perl - setup
echo 'export PATH=$HOME/.nodebrew/current/bin:$PATH' >> ~/.bashrc
source ~/.bashrc

nodebrew install-binary stable
nodebrew use v4.0.0
nodebrew alias default v4.0.0

nodebrew ls
# current: v4.0.0
```

## エディタ
好きなエディタを使ってください．（ESLintの使えるエディタがよいでしょう）

もしなければ，[Atom](https://atom.io/)か[VSCode](https://code.visualstudio.com/)のどちらかをインストールしてください．

### Atom
[linter](https://atom.io/packages/linter)と[linter-eslint](https://atom.io/packages/linter-eslint)をインストールしてください．

パッケージインストールはAtomの画面から``Ctrl + ,``で開きます．（macの場合は``Command + ,``）

### VSCode
v0.8.0からeslintが使えるようになりました．

[ここ](https://code.visualstudio.com/Docs/editor/customization#_user-and-workspace-settings)を参考に次のように値を設定します．
```json
{
  "eslint.enable": true,
  "javascript.validate.enable": false
}
```

## テンプレートファイル
準備中です．
