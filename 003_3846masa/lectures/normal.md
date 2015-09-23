# nodejsでワイワイ（通常）

## この講義でやること
- CommonJSにならったモジュール
- Promiseをつかった非同期処理

## 事前知識
### モジュール
#### いままでのライブラリ
いままでのJSライブラリは，それぞれが固有の変数名を使っていました．

```html
<script src="./jquery.js"></script>
<script>
  console.log($('#id'));
</script>
```

しかし，固有の変数名はライブラリ開発者が勝手に決めたものなので，重複してしまう可能性がありました．
```html
<script src="./jquery.js"></script>
<script src="./zepto.js"></script>
<script>
  // jqueryもzeptoも$に割り当て
  console.log($('#id'));
  // この$はどっち？
</script>
```

#### CommonJSの考え方
nodejsのモジュールはCommonJSと呼ばれています．

CommonJSでは，ライブラリの変数名を **開発者が好きに決めることができます．**

```javascript
var jq = require('jquery');
console.log(jq('#id'));
// $じゃない！！
```

開発者もモジュール作成者も頭を悩ます必要がなくなりましたね．

#### 【実践】nodejsを触ってみよう
適当なフォルダを作ってください．

nodeを実行して，次のコマンドを打ってみましょう．

```javascript
// File System モジュールの読み込み
var fs = require('fs');
// テキストデータの書き込み
fs.writeFileSync('./test.txt', 'Hello,World!');
```

### Promise
#### 非同期処理とは
非同期処理とは，メインのプログラムの流れとは別のタイミングで実行される処理のことです．

たとえば，ProcessingではmousePressed関数がそれにあたるでしょう．

```java
void setup() {
  fill(0);
}

void draw() {
  background(255);
  ellipse(50, 50, 100, 100);
}

void mousePressed() {
  // クリックされた時に実行される
  println("Clicked!");
  fill(255, 0, 0);
}
```

JavaScriptでクリックされた時に実行する関数は次のように書けます．

```javascript
document.addEventListener('click', function() {
  alert('Clicked!');
});
```

このときの``function() {...}``をコールバック関数といいます．

大雑把に言えば， **何か起きたあとで実行される関数** です．

#### 今までのJSの非同期処理
JavaScriptの非同期処理の書き方には問題がありました．

それが通称 **コールバック地獄** と呼ばれるものです．

例えば，JavaScriptでいくつかのファイルをダウンロードして使いたいとします．

```javascript
var xhr_1 = new XMLHttpRequest();
xhr_1.open('GET', './file_01.html', true);
xhr_1.addEventListener('load', function() {
  var result_1 = xhr_1.response;
  var xhr_2 = new XMLHttpRequest();
  xhr_2.open('GET', './file_02.html', true);
  xhr_2.addEventListener('load', function() {
    var result_2 = xhr_2.response;
    var xhr_3 = new XMLHttpRequest();
    xhr_3.open('GET', './file_03.html', true);
    xhr_3.addEventListener('load', function() {
      var result_3 = xhr_3.response;
      console.log(result_1, result_2, result_3);
    });
    xhr_3.send();
  });
  xhr_2.send();
});
xhr_1.send();
```

地獄が想像出来ましたか？

これは誇張して書いていますが，実際このような深い入れ子構造になりやすいです．

#### JSの新しい非同期処理の書き方
そこでPromiseという考え方が生まれました．

```javascript
var results = [];
var pushValue = function(value) {
  results.push(value);
  return results;
};
var fetchText = function(url) {
  return fetch(url).then(function(res) {
    return res.text();
  });
};

fetchText('./file_01.html')
.then(pushValue)
.then(function() { return fetchText('./file_02.html'); })
.then(pushValue)
.then(function() { return fetchText('./file_02.html'); })
.then(pushValue)
.then(function(results) {
  console.log(results);
})
.catch(function(error) {
  console.error(error.stack);
});
```

上から順番に実行されるようになりましたが，まだややこしいですね．

今回の例は並列処理できるので，書き換えてみましょう．

```javascript
var fetchText = function(url) {
  return fetch(url).then(function(res) {
    return res.text();
  });
};

Promise.all([
  fetchText('./file_01.html'),
  fetchText('./file_02.html'),
  fetchText('./file_03.html')
]).then(function(results) {
  console.log(results);
}).catch(function(error) {
  console.error(error.stack);
});
```

一番最初のコードと比べると，かなり流れがわかるようになったと思います．

#### Promiseの流れ
Promiseには，thenとcatchの2種類の関数があります．

thenの中には，**前の実行結果** を引数とした関数を入れます．

catchの中には，**そこまでの処理で発生したエラー** を引数とした関数を入れます．

細かいことを言うと上の説明は違うのですが，大まかに理解する分にはこれでいいと思います．

（詳しいことを知りたい人は参考文献を見てみてください）

```javascript
fetch('http://example.com')
.then(function(res) {
  console.log(res.status);
})
.catch(function(error) {
  console.error(error.stack);
});
```

thenとcatchはいくらでも続けられます．

then内の関数でreturnしたものが次のthenに引き継がれます．

新しいPromiseが生成されたら，それをreturnすることで続きを書くことができます．

```javascript
fetch('http://example.com')
.then(function(res) {
  return res.text();
  // res.text()はPromise
})
.then(function(text) {
  console.log(text);
});
```

Promise.allは，配列で渡した関数群の実行結果たちを，その順番の配列でthenに流します．

```javascript
Promise.all([
  Promise.resolve(1),
  Promise.resolve(5),
  Promise.resolve(25)
])
.then(function(results) {
  console.log(results);
  // [1, 5, 25]
});
```

最低限，thenは続けてcatchでエラー処理と覚えておけば大丈夫です．

今回はそんなに多用しないので安心してください．

#### 【実践】fetchしてみよう
最新版のChromeにはXHRに代わる関数であるfetchが実装されています．

実装されていなくても，Githubの提供するpolyfillで使えるようになります．

nodejsでも，node-fetchモジュールを使うとfetchが使えるようになります．

データを非同期に取得する（AJAX）には，これからこの関数が使われていくと思います．

fetchはPromise実装なので，実際に試してみましょう．

まず適当なフォルダを作って，その中でターミナル（端末・コマンドプロンプト）を開きます．

nodejsではモジュールが必要なので，インストールします．

```bash
npm install node-fetch
```

次にfetchTest.jsというファイルを作りましょう．

```javascript
// fetchTest.js
var fetch = require('node-fetch');
fetch('http://example.com')
.then(function(res) {
  return res.text();
  // res.text()もPromise
})
.then(function(text) {
  console.log(text);
})
.catch(function(error) {
  console.error(error.stack);
});
```

さあ，実行してみましょう．

```bash
node ./fetchTest.js
```

インターネットに接続していれば，しっかりexample.comの内容が出力されたはずです．

#### 参考資料
Promiseについては，[Promiseの本](http://azu.github.io/promises-book/)にとても詳しく書いてあります．

興味のある人は読んでみてください．
