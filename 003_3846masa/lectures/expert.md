# nodejsでワイワイ（発展）

## nodejsとは
- サーバサイドで動くJavaScriptエンジン
- 今までブラウザでしか動かなかったJavaScriptを単体で動かせるようになる
  - rubyとかpythonとかのスクリプト言語と同じ立ち位置に

## 今日やること
- モジュールを使ったJavaScript開発
- 非同期処理の新しい書き方
- その他の新しいJSの書き方

## JavaScriptモジュール
- いにしえの時代（というより学び始め）
  - 好き勝手コードを書いてた
  - 動けばよかった
  - グローバル変数を使いまくるからゴチャゴチャ
    - 干渉しかねないから他人のコードなんて使えない
- jQueryなどの登場
  - 便利なライブラリが増える
  - ローカル変数をうまく活用し始める
  - ライブラリがグローバル変数を1つ持つ
    - $はjQuery，\_はunderscore.jsというように暗黙の了解が生まれる
    - 独特な変数名はどんどん取られていき，わかりにくい変数名が出てくる
- CommonJSの登場
  - 開発者が好きな変数名にライブラリをバインドできるように（モジュール化）
    - 例えば，jQueryを$でなくて好きな変数名でつかえるようになる
  - nodejsの基本概念
- ES6(ES2015)での採用
  - モジュールの概念がES6でブラウザJSでも取り入れられた
    - import/export構文

### コードで見る
#### いにしえの時代
```javascript
function hoge() {
  console.log('何かする');
}
function hoge() {
  // hoge()は既にあるからエラー
  console.log('別の何かをしたいのに...');
}
hoge(); // <= 何がしたいの？
```
#### jQueryなどの登場
```html
<script src="./jquery.min.js"></script>
<script>
  $('#id').text('Hello,jQuery!');
</script>
<script src="./zepto.min.js"></script>
<script>
  // zeptoも$を使うのに，どうなるのさ！？
  $('#id').text('Hello,Zepto!');
  // うまく組まれたライブラリは調整してくれるけど...
  var $idFromjQuery = jQuery('#id');
  var $idFromZepto = Zepto('#id');
  // 決まりはないから，すべてがそうとは限らないよね
</script>
```
#### CommonJSの登場
```javascript
/* 好きに書ける！自由に使える！ */
var jq = require('jquery');
var zp = require('zepto');
var $ = require('sizzle');
```
#### ES6(ES2015)での採用
```javascript
/* ちょっと書き方が変わった */
import jq from 'jquery';
import zp from 'zepto';
import $ from 'sizzle';
/* 一部だけ取り込むことも可能 */
import {parse} from 'url';
import {format as build} from 'url';
```

## 非同期処理
- いにしえの時代
  - Callback関数
    - 深層化していくCallback地獄
    - キャッチできないCallback内のエラー
- PromiseとjQuery.Deferred
  - Callback関数をうまい具合にwrap（ラップ）するライブラリの登場
  - 前述の問題を解決
- Promiseの正式採用
  - ES6(ES2015)でPromiseが採用された
  - ライブラリを使うことなく標準で使えるようになった
  - [Promiseの本](http://azu.github.io/promises-book/)に詳しくまとまっている
- Generatorを組み合わせたcoライブラリ
  - Promiseが多い逐次処理になるとまだ見づらかった
  - C#などにあるasync/await構文のように記述したい
  - Generatorの仕組みを使ってそれらしい記法を確立
- ES7(ES2016)の草案にasync/awaitが入る
  - PromiseをC#のようなasync/await構文で書けるようになるはず
- 余談：async.js
  - nodejsの非同期処理はasync.jsが一時期主流だったらしい
  - 処理する関数を配列として渡して，順番に実行する（多分）
  - 詳しくは[この記事](http://qiita.com/takeharu/items/84ffbee23b8edcbb2e21)とか読んでみてください

### コードで見る
#### いにしえの時代
```javascript
window.onload = function() {
  var xhr = new XMLHttpRequest();
  xhr.open('GET', 'http://example.com', true);
  xhr.onload = function(ev) {
    if (xhr.status !== 200) {
      throw new Error('Can\'t access.');
    }
    alert(xhr.responseText);
  };
  xhr.send();
};
```
#### PromiseとjQuery.Deferred
```javascript
$(function() {
  $.get('http://example.com')
   .done(function(res) {
     alert(res);
   })
   .fail(function(res) {
     throw new Error('Can\'t access.');
   });
});
```
#### Promiseの正式採用
```javascript
function onload() {
  return new Promise(function(resolve) {
    if (document.readyState === 'complete') {
      resolve();
    } else {
      window.addEventListener('load', resolve);
    }
  });
}

onload()
.then(function() {
  return fetch('http://example.com');
})
.then(function(res) {
  if (res.status !== 200) {
    throw new Error('Can\'t access.');
  }
  return res.text();
})
.then(function (text) {
  alert(text);
})
.catch(function (err) {
  console.error(err);
});
```
#### Generatorを組み合わせたcoライブラリ
```javascript
function onload() {
  return new Promise(function(resolve) {
    if (document.readyState === 'complete') {
      resolve();
    } else {
      window.addEventListener('load', resolve);
    }
  });
}

co(function*() {
  yield onload();
  var res = yield fetch('http://example.com');
  if (res.status !== 200) {
    throw new Error('Can\'t access.');
  }
  var text = yield res.text();
  alert(text);
})
.catch(function(err) {
  console.error(err);
});
```
#### ES7(ES2016)の草案にasync/awaitが入る
```javascript
function onload() {
  return new Promise(function(resolve) {
    if (document.readyState === 'complete') {
      resolve();
    } else {
      window.addEventListener('load', resolve);
    }
  });
}

(async function() {
  await onload();
  var res = await fetch('http://example.com');
  if (res.status !== 200) {
    throw new Error('Can\'t access.');
  }
  var text = await res.text();
  alert(text);
})()
.catch(function(err) {
  console.error(err);
});
```
#### 余談：async.js
```javascript
async.waterfall([
  function(callback) {
    if (document.readyState === 'complete') {
      callback();
    } else {
      window.addEventListener('load', callback);
    }
  },
  function(callback) {
    request('http://example.com', callback);
  },
  function(res, body, callback) {
    if (response.statusCode !== 200) {
      callback(new Error('Can\'t access.'));
    } else {
      callback(null, body);
    }
  }
], function(err, result) {
  if (err) {
    console.error(err);
  } else {
    alert(result);
  }
});
```

## Let's Try!!（setTimeoutをPromise化しよう）
指定時間待つsleepモジュール（setTimeoutのラッパーモジュール）をつくろう．

### モジュールの書き方
```javascript
/* module.js */
// module.exportsにメインの関数（オブジェクト）を割り当てる
module.exports = function() {
  console.log('Hello,CommonJS!');
};
```
```javascript
var func = require('./module.js');
func();
// > Hello,CommonJS!
```

### Promiseの書き方
```javascript
// Promiseを返す関数が必要
function factory() {
  function main(resolve, reject) {
    // resolve,rejectは関数
    if (true) {
      // resolveは成功した時の返り値を引数にして実行
      resolve('Success!!');
    } else {
      // rejectは失敗した時のエラーを引数にして実行
      reject(new Error('Failed via reject'));
      // throwで投げても自動でrejectに飛ぶ
      throw new Error('Failed via throw');
    }
  }
  // Promiseを返す関数が必要
  return new Promise(main);
}

var p = factory();
p.then(function(result) {
  // resolveの引数がresultに入る
  console.log(result);
}).catch(function(err) {
  // rejectの引数がerrに入る
  console.error(err);
});
```

### setTimeoutをPromise化しよう
まず，通常のsetTimeoutを思い出す．
```javascript
function cb() {
  console.log('done.');
}
setTimeout(cb, 10 * 1000 /* 10sec. */);
```

Promiseの構文を思い出す．
```javascript
function sleep() {
  return new Promise(function(resolve, reject) {
    resolve();
  });
}
```

setTimeoutをPromiseでラップする．
```javascript
function sleep() {
  return new Promise(function(resolve, reject) {
    setTimeout(resolve, 10 * 1000 /* 10sec. */);
  });
}
```

引数で秒数を指定できるようにする．
```javascript
function sleep(millisec) {
  return new Promise(function(resolve, reject) {
    setTimeout(resolve, millisec);
  });
}
```

最後にモジュール化する．
```javascript
function sleep(millisec) {
  return new Promise(function(resolve, reject) {
    setTimeout(resolve, millisec);
  });
}
module.exports = sleep;
```

モジュール化したsleepを使ってみる．
```javascript
var sleep = require('./sleep.js');
sleep(10 * 1000)
  .then(function() {
    console.log('done.');
  });
```

## Let's Try!!（nodejsのよくある関数をPromise化しよう）
nodejsの標準ライブラリはPromiseではないので，Promise化してみる．

よくあるnodejsのcallbackは，エラーが第1引数，返り値が第2引数以降になる．
```javascript
var stat = require('fs').stat;
var path = './test.txt';

stat(path, function(err, stats) {
  if (err) {
    console.error(err);
  } else {
    console.log(stats);
  }
});
```

なので，Promise化するにはこうすればよい．
```javascript
var stat = require('fs').stat;
var path = './test.txt';
var statPromise = function(path) {
  new Promise(function(resolve, reject) {
    stat(path, function(err, stats) {
      if (err) {
        reject(err);
      } else {
        resolve(stats);
      }
    });
  });
}

statPromise(path)
  .then(function(stats) {
    console.log(stats);
  })
  .catch(function(err) {
    console.error(err);
  });
```

応用するとどんなものもPromise化できる．
```javascript
var promisify = function(func) {
  return function() {
    var args = arguments;
    return new Promise(function(resolve, reject) {
      var newArgs = Array.prototype.slice.call(args);
      newArgs.push(function() {
        var results = Array.prototype.slice.call(arguments);
        var err = results.shift();
        (err) ? reject(err) : resolve(results);
      });
      func.apply(this, newArgs);
    });
  }
}

var stat = require('fs').stat;
var path = './test.txt';
var statPromise = promisify(stat);

statPromise(path)
  .then(function(stats) {
    console.log(stats);
  })
  .catch(function(err) {
    console.error(err);
  });
```

もちろんこのような処理をしてくれるライブラリは色々ある．

bluebird（PromiseのPolyfillのひとつ）にはこの機能が搭載されている．

```javascript
var Promise = require('bluebird');
var fs = Promise.promisifyAll(require('fs'));
// 通常の関数名 + Async というPromise関数ができる
fs.statAsync(path)
  .then(function(stats) {
    console.log(stats);
  })
  .catch(function(err) {
    console.error(err);
  });
```

## Let's Try!!（coを実装しよう）
coライブラリをザックリつくって仕組みを知ろう．

まず，Generatorの挙動を知る．
```javascript
// https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Iterators_and_Generators#.E9.AB.98.E5.BA.A6.E3.81.AA.E3.82.B8.E3.82.A7.E3.83.8D.E3.83.AC.E3.83.BC.E3.82.BF

function* fibonacci(arg){
  var fn1 = 1, fn2 = 1;
  while (true) {
    var current = fn2;
    fn2 = fn1;
    fn1 = fn1 + current;
    // yieldで引数を戻して一旦抜ける
    // 次のnext()の引数が返り値となる
    var reset = yield current;
    // なので，resetはnext()の引数が入る
    // next() -> undefined / next(true) -> true
    if (reset) {
      fn1 = fn2 = 1;
    }
  }
}

var sequence = fibonacci('testArg');
// fibonacciのargに'testArg'が入る
console.log(sequence.next());
// {value: 1, done: false}
console.log(sequence.next().value); // 1
console.log(sequence.next().value); // 2
console.log(sequence.next().value); // 3
console.log(sequence.next().value); // 5
console.log(sequence.next().value); // 8
console.log(sequence.next(true));
// {value: 1, done: false}
console.log(sequence.next().value); // 1
console.log(sequence.next().value); // 2
console.log(sequence.next().value); // 3
```

coライブラリのひな形を作る．
```javascript
function co(generatorFunc) {
  function main(resolve, reject) {
    // anything
  }
  return new Promise(main);
}
```

mainの部分を作る．まず，generatorを作る．
```javascript
function main(resolve, reject) {
  var g = generatorFunc();
}
```

続いて返ってくる値を取得する．
```javascript
function main(resolve, reject) {
  var g = generatorFunc();
  var next = g.next();
}
```

関数が終了しているならば，resolveする．
```javascript
function main(resolve, reject) {
  var g = generatorFunc();
  var next = g.next();
  if (next.done) {
    resolve(next.value);
  }
}
```

関数が続いているならば，then/catchを設定する．
```javascript
function main(resolve, reject) {
  var g = generatorFunc();
  var next = g.next();
  if (next.done) {
    resolve(next.value);
  } else {
    var promise = next.value;
    promise.then(function(result) {
      // anything
    }).catch(function(err) {
      reject(err);
    });
  }
}
```

thenなら，generatorに値を返す．
```javascript
function main(resolve, reject) {
  var g = generatorFunc();
  var next = g.next();
  if (next.done) {
    resolve(next.value);
  } else {
    var promise = next.value;
    promise.then(function(result) {
      g.next(result);
    }).catch(function(err) {
      reject(err);
    });
  }
}
```

うまい具合に再帰させる．
```javascript
function main(resolve, reject) {
  var g = generatorFunc();
  recursive();

  function recursive(value) {
    var next = g.next(value);
    if (next.done) {
      resolve(next.value);
    } else {
      var promise = next.value;
      promise
        .then(recursive)
        .catch(function(err) {
          reject(err);
        });
    }
  }
}
```

最終的にこうなる．
```javascript
function co(generatorFunc) {
  function main(resolve, reject) {
    var g = generatorFunc();
    recursive();

    function recursive(value) {
      var next = g.next(value);
      if (next.done) {
        resolve(next.value);
      } else {
        var promise = next.value;
        promise
          .then(recursive)
          .catch(function(err) {
            reject(err);
          });
      }
    }
  }
  return new Promise(main);
}
```

実際にはPromiseかどうかの判定など色々書くことがある．

## 今日から使える便利な新しいJS知識
Chrome45で使えるものを使っていきます．

### Arrow function
```javascript
(arg) => {
  console.log(arg);
}

[1, 8, 6, 4, 9].sort((a, b) => b - a);
```

### let
```javascript
function varFunc() {
  var i = null;
  for (var i = 0; i < 10; i++) {}
  console.log(i); // 10
}
function letFunc() {
  'use strict';
  let i = null;
  for (let i = 0; i < 10; i++) {}
  console.log(i); // null
}
varFunc();
letFunc();
```

### fetch
```javascript
fetch('http://example.com')
  .then((res) => res.text())
  .then((text) => console.log(text))
  .catch((err) => console.error(err));
```

### Template String
```javascript
var hoge = 'Text';
console.log(`hoge is ${hoge}`);
console.log(`line
line
line`);
console.log(`日本語をエンコード ${encodeURI('日本語')}`);
```

### Array.from
```javascript
var children = document.body.children;
console.log(children.forEach); // undefined
var childrenArray = Array.from(children);
console.log(childrenArray.forEach); // function
```

### Object.assign
```javascript
var obj = {a: 1};
Object.assign(obj, {b: 2});
console.log(obj);
// {a: 1, b: 2}
var cloneObj = Object.assign({}, obj);
console.log(cloneObj);
// {a: 1, b: 2}
```
