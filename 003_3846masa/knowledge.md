# 雑に学ぶJavaScriptの基礎

ザックリと雑に学ぶので，厳密には違うよ！という部分もあるし，正直理解できないことも多いだろう．（僕自身，雑にしか理解していないのも原因ではある）

心配しなくても大丈夫．すべてを知っている必要はない．

こういうものがあるんだって漠然と知っているだけでもだいぶ違う．

もし，詳しく知りたい人はじっくり学ぶといい．本もあるし，ネットで調べてもいい．もちろん友人や先輩に訊いてみるのもいいだろうし，僕個人としてはそれがオススメだ．

[ドットインストールの動画](http://dotinstall.com/lessons/basic_javascript_v2)も最初の導入としては取っ付き易いかもしれない．

今回の講義はこれらの知識を耳にしたことがある前提で話すので，知らない人は読んで試しておこう．

## JavaScriptの試し方

### WebInspectorを使う方法
Chromeなどのモダンブラウザには，開発者ツールがついている．

Chromeの場合は，``Ctrl + Shift + J``でJavaScriptのコンソールが出る．

### nodejsを使う方法
端末（コマンドプロンプト，ターミナル）上で``node``と打つと，コンソールが開く．

## JavaScriptの型（みたいな何か）

### 変数
JavaScriptは変数宣言のとき，どんな型でも``var``をつける．

困ったらつけとけばいい．

> **注** var以外もあるが追々覚えればよい．

```javascript
var value = 10;
console.log(value);
// > 10
var str = '文章';
console.log(str);
// > 文章
```

### 文字列
文字列はシングルクオーテーションかダブルクオーテーションで囲む．

```javascript
console.log("abc" === 'abc');
// > true
```

### 真偽値
``true`` or ``false``

```javascript
console.log(!true);
// > false
```

### 数値
深く考えることはなく，そのまま打てばいい．

> **注** JavaScriptは整数と小数の区別はない．どちらもNumberだ．

```javascript
console.log(10 + 5.5);
// > 15.5
```

### 関数
JavaScriptでは，関数もオブジェクトなのだ．

```javascript
var testFunc = function() {
  console.log('Hello, World!');
};
testFunc();
// > Hello, World!
```

### 配列
配列には型の制限がない．何でも入る．

```javascript
var arr = [1, 'なんでも入る', [2, '多重'], {key: 'value'}];
console.log(arr);
```

### 連想配列
keyとvalueが1対1になっているものをよく連想配列やハッシュと呼ぶ．

> **注** JavaScriptだとオブジェクトと呼ばれるが，そこは色々ややこしいので今は気にしなくていい．

```javascript
var obj = {key: 'value'};
console.log(obj.key);
// > value
console.log(obj['key']);
// > value
```

### その他

#### DOM
JavaScriptはよくHTMLと一緒に使われる．

```html
<p>Hello,World</p>
<script>
var p = document.querySelector('p'); // 最初のpを取る
p.textContent = 'Hello,JavaScript'; // 中身を変える
</script>
```

#### Math
Mathには，計算で使える便利な関数がいっぱい入ってる．

```javascript
console.log(Math.pow(5, 3)); // 5の3乗
// > 125
console.log(Math.round(1.53)); // 四捨五入
// > 2
```

## 非同期処理

ここからちょっと難しい話．

### ブロッキング
10秒待つにはどうしたらいいだろうか．

こんなコードを書いたとする．

```javascript
var start = new Date(); // 現在時刻をミリ秒で取得
while (new Date() - start <= 10 * 1000); // 10秒間無限ループ
console.log('10sec.');
```

ブラウザで実行すると，ブラウザが10秒間固まる（動かなくなる）．

これをブロッキングという．

### ノンブロッキング
処理時間が長くなると，ブラウザが固まってしまう．

それでは困ってしまうね．

10秒待つにはどうしたらいいのだろうか．

Callbackという考え方がある．

```javascript
setTimeout(function() {
  // callback関数内
  console.log('10sec.');
}, 10 * 1000);
```

実行してみると，ブラウザは固まらない．

``setTimeout``は，callback関数をX秒後に実行させようとする．

メインの流れではない要因に応じて実行される関数をcallbackという．

重たくて時間のかかる処理のあとは，callbackで戻ってくるようにすると思っていい．

------------------------------

もうちょっと難しい話をするよ．

```javascript
setTimeout(function() {
  // callback関数内
  console.log('10sec.');
}, 10 * 1000);

console.log('Hello');
```

こうなるとどうなるだろうか．

正解は，先にHelloが表示される．

setTimeoutは，関数にタイマーをかけるだけなんだ．

タイマーをかけたあとのコードはそのまま実行されていく．

### イベント
メインの流れではない要因って他には何があるんだろう．

例えば，クリックされた時．

```javascript
var button = document.querySelector('button');
button.addEventListener('click', function() {
  console.log('Clicked!');
});
```

buttonがクリックされた時に，Clicked!と表示される．

クリックされた時にはクリックイベントが生まれてる．

それを検知してcallback関数が実行されるんだ．

重たくて時間のかかる処理でなくても，いつ起きるかわからない処理はcallbackになる．

### AJAX
他のデータを取りたいときもあるだろう．

でも，通信が終わるのはどのタイミングかわからない．

もちろんここでもcallback関数が使われる．

```javascript
// https://developer.mozilla.org/ja/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest
var request = new XMLHttpRequest();
request.open('GET', 'http://www.mozilla.org', true);
request.addEventListener('load', function() {
  if (request.status === 200) {
    console.log(request.responseText);
  }
});
request.send();
```

### callback地獄
10秒待って，何かしたあと10秒待って，を繰り返すとどうなってしまうだろう．

```javascript
setTimeout(function() {
  // anything
  setTimeout(function() {
    // anything
    setTimeout(function() {
      // anything...
    }, 10 * 1000);
  }, 10 * 1000);
}, 10 * 1000);
```

callback関数でいっぱい囲むことになってしまう．

読みづらいし最悪だ．まるで地獄．

もし君がJavaScriptを書くことになれば，この地獄と闘うことになるだろうね．
