## 巻き上げ（ホイスティング）について

ここでJavaScriptで特徴的な**_巻き上げ(ホイスティング)_**という現象について少しだけ触れておきます。**_巻き上げ_**とは、宣言した変数が**スコープ**の先頭で記述されたものと判断される仕組みのことです。

次の例では、一見コンソール実行時に変数宣言されていないため、出力結果が単純なエラーとなるように思います。しかし結果はそうはならず`undefined`となります。これは直感に反する結果ではないでしょうか。

```js
console.log(word); // undefined
var word  = 'Hello';
```

出力結果:
```js
undefined
```

理由はスコープ内で最後尾に宣言された変数が、**「スコープ内の先頭に記述されたもの」**と内部で判断されるためです。従って先ほどのコードは実際には次のものと同義になります。

```js
var word;
console.log(word); // undefined
var word  = 'Hello';
```

上の例では宣言に`var`を使いましたが`let`の場合はどうなるでしょうか。ES6では、`let`でも内部的に巻き上げは起きるのですが、その変数を宣言より前で参照すると参照エラーとなります。結果として先ほどのコードの時のような`undefined`にはなりません。この例からもわかるように、やはり`let`と`const`を積極的に使うことでより直感的で安全なプログラムが可能になります。

```js
console.log(word); // ReferenceError: word is not defined
let word  = 'Hello';
```

constでもletと同じ結果になります。

```js
console.log(word); // ReferenceError: word is not defined
const word  = 'Hello';
```


## 関数スコープでの巻き上げ

類似の例として、関数スコープ内での**_巻き上げ_**について見ておきます。次の例では関数を実行しても最初の`first`は表示されません。

```js
var num = 'first';
function func(){
  console.log(num);
  var num  = 'second';
  console.log(num);
}

func();
```

出力結果:
```js
undefined
"second"
```

これは関数スコープ内で自動的に**_巻き上げ_**が起こり、関数内の先頭で変数`num`が内部的に宣言されているからです。従って関数内の最初の`num`はグローバル変数の`num`ではなく**_ローカル変数_**の`num`を指すことになります。この時先ほどのコードは次のコードと同義です。

```js
var num = 'first';

function func(){
//エラーにならない
  var num;
  console.log(num); //undefined
  var num  = 'second';
  console.log(num);
}

func();
```

というわけで関数を使った場合でも、グローバルな領域とローカルな領域に変数の役割を明確に分けて使うには、`let`と`const`を使う必要があります。

```js
let  num = 'first';
function func(){
//ローカル変数の宣言がないためエラーが発生
  console.log(num); // num is not defined
  let num  = 'second';
  console.log(num);
}
func();
```

## 更に学ぼう

### 動画で学ぶ

- [JavaScript入門 - ドットインストール](https://dotinstall.com/lessons/basic_javascript_v2)

### 本で学ぶ

- [Eloquent JavaScript 3rd Edition](http://eloquentjavascript.net/)
