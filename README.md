# Lesson 8 this、スコープ、let、const

## 目的

- let, constの特徴を理解する。
- スコープの概念を理解する。

## はじめに

ここでは、既にこれまで使ってきた`let`および`const`の特徴について詳しく見ていきます。
ES5以前は、変数を宣言する際`var`のみが使われていました。そしてES6になり新たな変数宣言時のキーワードとして`let`および`const`が導入されました。これにより**_スコープ_**と呼ばれる変数の有効範囲が範囲がより細かく明示できるようになりました。`let`と`const`の特性を意識して使うことによってより安全でメンテナンス性に優れたプログラムを作成できるようになります。

## スコープとは

スコープとは変数名や関数名の有効範囲のことです。もしある変数のスコープの有効範囲の外からアクセスしようとしても、その変数にアクセスすることはできません。
スコープの範囲を意識することで、一つのプログラム内で同じ変数名を複数宣言することもできます。
スコープを意識したコードを書くことは予期せぬ不具合を起こさないためにも非常に重要です。
スコープには大きく分けて**_グローバルスコープ_**と**_ローカルスコープ_**の2種類があります。

## グローバルスコープ

JavaScriptコードのどこからでもアクセスできる範囲のことを**_グローバルスコープ_**と言います。Javascriptのファイルあるいは`<script>`タグ内でトップの位置で宣言された変数はグローバルスコープを持ちます。グローバルスコープを持つ変数は**_グローバル変数_**と呼ばれます。また関数についても入れ子(**ネスト**)になっていない関数はどこからでもアクセス可能なグローバルスコープを持ちます。この関数は**_グローバル関数_**と呼ばれます。

```javascript
<script>
let a = 100; // グローバル変数

function global(b) { // グローバル関数
  function local(c) { // ネストされた関数はグローバル関数ではない
    return c * 3;
  }

  return b * 2; // グローバル変数ではない
}

console.log(a); // 100
console.log(global(a)); // 200
// ネストされた関数には外部から直接アクセスすることができない。
console.log(local(a)); // ReferenceError: local is not defined
</script>
```

## ローカルスコープ

コード中の特定の範囲内でしか参照することができない局所的な変数のことを**_ローカル変数_**と言います。またその範囲を**_ローカルスコープ_**と呼びます。ローカル変数は「{}」で囲んだブロック内で宣言されますが、その変数が**_ローカル変数_**になるか、それとも**_グローバル変数_**になるかは変数のキーワードによって決まります。**_ローカルスコープ_**には以降で述べるように**_ブロックスコープ_**と**_関数スコープ_**が存在します。

```js
//ローカルスコープの例

//関数のブロックで囲んだ場合
function func(){
  let a = 100;//ローカル変数
}
//ブロック単体で囲んだ場合
{
  let b = 100;//ローカル変数
}

//ブロック内の変数に外からアクセスすることはできない。
console.log(a); // ReferenceError: a is not defined
console.log(b); // ReferenceError: b is not defined
```

宣言キーワードの`var`、`let`、`const`を付けずに無冠で変数宣言すると、ブロック内で宣言したとしても、自動的に**_グローバルスコープ_**となってしまいます。
```js
{
  block = 100;
}

// ブロック内にアクセスできてしまう
console.log(block); // 100
```

また**_if文_**、**_for文_**などの制御文の中で宣言キーワードなしで変数宣言した場合や、古い宣言方法である`var`を使って宣言された変数は**_ローカルスコープ_**を持ちません。
```js
for (i = 0; i < 10; i++){} //カウンタ変数の宣言キーワードなし

console.log(i);// 10が出力される。つまりアクセスが可能
```
以下のように、結果は**カウンタ変数i**を`var`で宣言しても変わることはありません。
```js
for (var i = 0; i < 10; i++) {} //カウンタ変数のvar宣言

console.log(i);// 10が出力される。つまりアクセスが可能
```
この例のように制御文の外からでもアクセスできてしまう場合、これでは1つのプログラムの中で複数の制御文を記述する際に混乱を招いたり、わざわざ変数名を新しく作らないといけなかったりと不便です。この問題は`let`と`const`を使った宣言ができるようになったことで解決されました。従って宣言時には必ずこれらの新しいキーワードを明記するようにしましょう。


## letとconst

**_スコープ_**を意識した変数宣言をする上で重要なポイントは、むやみに**_スコープ_**の範囲を広げることは良いことではないということです。変数を取り扱う場合`var`は自由度が大きい分、意図しないエラーが発生する可能性があります。

`let`および`const`はES6から新たに追加された宣言方法です。ES6以降の環境では、変数宣言をする際は基本的には特別な意図のない限りあえて`var`を使う理由はないと思います。なので変数宣言には`let`および`const`を使っていくようにしましょう。

次の表は`var`、`let`、`const`によって宣言された変数の特徴についてまとめたものです。

|  | var |let | const|
| -------- | ----- | ------| ----|
| 値の代入 |  可能  | 可能 | 不可|
|同一スコープ内の再宣言   | 可能  | 不可  | 不可  |
| ブロックスコープ  | x  | ○| ○|
| 関数スコープ   | ○  | ○  | ○  |
| グローバルオブジェクトのプロパティ   | ○  | x  | x |

以下でこれらの特徴について見ていくことにします。

## letとconstの宣言
`let`と`const`を使った変数の宣言方法について、すでに見てきたように`let`も`var`も数値、文字列、関数など任意のデータタイプを格納することができます。

```js
// letの場合
let string = 'abc'; // 文字列
let value = 123; // 数値
let arr = [1,2,3]; // 配列
let obj = { name: "Table", number: 1 }; // オブジェクト
let func1 = function() { // 関数
  console.log('sample');
}

console.log(string); // "abc"
console.log(value); // 123
console.log(arr[1]); // 2
console.log(obj.name); // "Table"
func1(); // "sample"
```

```js
//constの場合
const string = 'efg';
const value = 456;
const arr = [4, 5, 6]; // 配列
const obj = { name: 'Chair', number: 2 }; // オブジェクト
const func2 = function(){
console.log('sample');
}

console.log(string); // "efg"
console.log(value); // 456
console.log(arr[1]); // 5
console.log(obj.name); // "Chair"
func2(); // "sample"
```
## 値の代入

`let`は値の代入(変更)が可能です。

```js
let value = 123;
value = 456; // 更新

console.log(value); // 456
```

一方で`const`の場合は値を代入(変更)することができません。

```js
const value = 123;
value = 456;

console.log(value);
// SyntaxError: Invalid regular expression: missing
```

`let`は配列やオブジェクトの代入ができますが、`const`は代入することはできません。変更しようとするとエラーとなります。

```js
/****** letの場合 ******/

// 配列のそのものを代入
let arrayOne = [2,4,6];
arrayOne = [1,3,5]; // 配列そのものの再代入

// 出力結果はエラーにならない
console.log(arrayOne); // [1, 3, 5]


// オブジェクトの再代入
let objOne = {
  name: 'Tanaka'
};
objOne = {
  name: 'Suzuki'
};

//出力結果はエラーにならない
console.log(objOne.name); // "Suzuki"
```

```js
/****** constの場合 ******/

// 配列のそのものを代入
const arrayTwo = [2,4,6];
arrayTwo = [1,3,5]; // 配列そのものの再代入

// 出力結果はエラーとなる
console.log(arrayTwo); // error


// オブジェクトの再代入
const objTwo = {
  name: 'Tanaka'
};
objTwo = {
  name: 'Suzuki'
};

// 出力結果はエラーとなる
console.log(objTwo.name); // error
```

重要なポイントとして、`let`および`const`は配列やオブジェクトといったデータタイプの中身の要素やプロパティを変更することができます。特に値の代入ができないはずだった`const`で要素やプロパティの変更ができてしまっていることに注意してください。`const`は宣言されて以降の再代入は許可しませんが、オブジェクト型の中の可変な参照データは変更可能です。

```js
/****** letの場合 ******/

// 配列要素の変更
let arrayOne = [2,4,6];
arrayOne.push(8); // 要素を追加

console.log(array_1[3]); // 8


// オブジェクトプロパティの変更
let objOne = {
  name: 'Tanaka',
  age: 30,
};
objOne.name = 'Suzuki'; // 更新

console.log(objOne.name); // "Suzuki"
```
```js
/****** constの場合 ******/

//配列要素の変更
const arrayTwo = [2,4,6];
arrayTwo.push(8); // 要素を追加

// 出力結果はエラーにならない
console.log(arrayTwo[3]); // 8


// オブジェクトプロパティの変更
const objTwo = {
  name: 'Tanaka',
  age: 30,
};

objTwo.name = 'Suzuki'; // 更新

//出力結果はエラーにならない
console.log(objTwo.name); // "Suzuki"
```

<!----
これはオブジェクトが参照型と呼ばれるデータタイプであるためです。参照型では数値や文字列などと同様に、宣言時にオブジェクトの実体がコンピュータのメモリ上のどこかに割り当てられます。これらは例えば`const`であれば変更することもできないのですが、それはいわば外側の箱の情報のみで、参照される中身の情報は別の場所に保管されているのです。従ってこれらの情報を変更しても問題ないのです。
---->

`const`によって宣言された変数は値の変更を考えない**定数**としての側面を強く持ちます。従って例えばコード全体を通じて変更することのない文字列や数値定数などで`const`を積極的に利用すると良いでしょう。

```js
//constの利用例
const PI = 3.141592; // 数値定数
const title = 'TITLE'; // 変更しない文字列
```

## 同一スコープ内の再宣言

`var`であれば同一スコープ内で同じ変数名で再宣言が可能でしたが、
`let`と`const`はそのスコープ内で2回宣言することはできません。名前の重複をプログラム側が許さない仕様となることで、誤って別用途の変数に同じ名前を付けて宣言してしまうことを防ぐことができます。

```js
var value = 123;
var value = 456; // この変数で値が上書きされる

console.log(value);// 456
```
```js
let value = 123;
let value = 456; // ２度宣言することはできない

console.log(value); // エラーになる
```

## ブロックスコープ
**_ブロック_**とはコード中で「{}」の括弧で囲まれた範囲のことです。例えば次のような単一の「{}」のみで囲ったものも**_ブロック_**となります。
```js
{
  文; //ここはブロック
}
```
またif文やfor文もブロックを持ちます。
```js
if (true) {
  文; // ここはブロック
}

for (let i = 0; i <= 3; i++) {
  文; // ここはブロック
}
```

`let`あるいは`const`を使うことで、`var`では実現できなかった**_ブロックスコープ_**が有効になります。
`var`では**_if文_**のブロック内で宣言した変数に外部からアクセスできていました。
```js
if (true) {
  var a = 100;
}

console.log(a); // 100
```

`let`および`const`ではアクセスすることはできません。

```js
if (true) {
  let a = 100;
  const b = 123;
}
console.log(a); // error "ReferenceError: a is not defined
console.log(b); // error "ReferenceError: b is not defined
```
**_if文_**の中で宣言された変数は通常、ブロック内の処理にしか利用しません。従って外部からアクセスできてしまうことは、意図せず値を変更してしまう恐れがありある意味非常に危険なのです。`let`や`const`を使って自動的にこれらの危険性を取り除くことができます。


上記のような**_ブロック_**で囲まれた範囲内で宣言された変数がそのブロック内にのみ有効範囲を持つ時、この**_ローカルスコープ_**のことを特に**_ブロックスコープ_**と言います。すでに述べたように変数が**_ブロックスコープ_**を持つには、`let`あるいは`const`というキーワードで宣言されている必要があります。つまり`var`を使った変数宣言では**_ブロックスコープ_**とはならず、**_グローバルスコープ_**となります。

**_グローバルスコープ_**は以下のように単純なブロック「{}」で囲った場合も有効になります。
`var`の場合はブロック内で宣言することはできるものの**_ブロックスコープ_**を持つことができません。

```js
{
  // ブロック内で変数を宣言は可能
  var b = 9;
  console.log(b); // 9
}
```
以下のようにブロック外からアクセスしてもエラーとならず変数にアクセスできてしまいます。

```js
{
  // ブロック内で変数を宣言
  var b = 9;
}
console.log(b); // 9 ※エラーにならない
```
一方、`let`あるいは`const`によって宣言された変数は**_ブロックスコープ_**を持つのでブロック外からアクセスしようとするとエラーとなります。

```js
{
  // ブロック内で変数を宣言
  let a = 8;
  const b = 3.14;
}
console.log(a); // "ReferenceError: a is not defined
console.log(b); // "ReferenceError: b is not defined
```

# 関数スコープ
すでに**_ブロックスコープ_**について説明しましたが、他の**_ローカルスコープ_**の概念として**_関数スコープ_**と呼ばれるものがあります。
関数ブロック内で宣言された変数は**_ローカルスコープ_**をもち、その変数は**ローカル変数**になります。関数内の**ローカル変数**が持つ有効範囲のことを**_関数スコープ_**と言います。

## 関数について
**_関数_**とは、ある処理を繰り返し使用できるように「{}」で囲んだブロック内に処理をまとめたものです。関数の詳細についてはレッスン9で学びますが、ここでは簡単な説明のみに留めます。**_関数_**は**_ブロック_**を持ちます。**_関数_**の**_ブロック_**は内部の変数の有効範囲に影響を与えます。**_関数_**の持つ有効範囲のことを**_関数スコープ_**と言います。

**_関数スコープ_**の特徴として、宣言時のキーワードが`var`、`let`、`const`のいずれであるかに関わらず、関数スコープを持つ変数に外部からアクセスすることはできません。

```js
let textGlobal = 'グローバル変数です';

function sample(){
  let textLocal = 'ローカル変数です';
  console.log(textLocal);
}

sample(); // "ローカル変数です"
console.log(textGlobal); // "グローバル変数です"
console.log(textLocal); // undefined
```

## グローバルオブジェクトのプロパティ

ブラウザでJavaScriptを処理する際は、**_グローバルオブジェクト_**として**windowオブジェクト**が定義されています。
`var`で宣言した変数は**windowオブジェクト**のプロパティになることができますが、`let`および`const`は、**windowオブジェクト**のプロパティになることができません。

```js
var valueVar = "var";
let valueLet = "let";
const valueConst = "const";

console.log(window.valueVar);// "var"
console.log(window.valueLet);// undefined
console.log(window.valueConst);// undefined
```

## this
**_スコープ_**の場所を明示するために`this`キーワードを使うことがあります。
以下で見ていくように`this`が何を意味するのかは、それが使われる文脈によって異なってきます。

### グローバルスコープで使う`this`

試しに次のコードをグローバルスコープ上で実行してみます。

```js
console.log(this);
```

結果はChromeのコンソールで次のような**Windowオブジェクト**が出力されました。

![console log](./images/this-console.png)

つまりグローバルスコープ上でthisを使う時、これはウェブブラウザ上の**windowオブジェクト**を指します。
従って次のような、**厳密等価演算子**を使った場合、この条件式は`true`を返します。

```js
console.log(this === window); // true
```

### 通常の関数と`this`

関数内で記述されたthisは、その関数が呼び出された時、その呼び出し元のオブジェクトを指します。

次の例を見てみましょう。

```js
function sample() {
  console.log(this);
}

console.log(sample());
```

出力結果:
```
[object Window] {
  addEventListener: function addEventListener() { [native code] },
  alert: function alert() { [native code] },
...
```
このように関数スコープ内でthisを使った場合も通常は**windowオブジェクト**を指します。
ただし次のサンプルコードのように、オブジェクトのメソッド定義内でthisが使われる時、このthisは**windowオブジェクト**ではなく、**そのオブジェクト自身**を指します。
```js
let obj = {
  name: 'Max',
  age: 20,
  func: function() {
    this.name = 'Taro'; // オブジェクトの中のnameプロパティを指定
    return this.name;
  }
};

console.log(obj.func()); // "Taro"
```

上記の例では関数呼び出しで`obj.name`と呼び出すところで`this.name`となっています。

これは関数をオブジェクト定義外で作成した場合でも変わりません。この場合でもthisは関連づけられるオブジェクトを指します。

```js
let obj = {
  name: 'Max',
  age: 20,
};

//オブジェクトメソッドを追加
obj.func = function() {
  this.name = 'Taro';
  return this.name;
}

console.log(func()); // "Taro"
```


### イベントとThis

html上にイベントのトリガーとして`<button>`を用意して次のコードがどうなるかを確認してみます。

```html
<!--html-->
<button>ボタン</button>
```

```js
//通常の関数を使ってイベント処理を記述
let btn = document.querySelector('button');
function btnClicked() {
  console.log(this);
}

btn.addEventListener('click', btnClicked);
```

これを実行すると下記のようにボタン要素がコンソールに出力されます。つまりこの時thisは**windowオブジェクト**ではなく、関数を呼び出すトリガーとなった`<button>`要素になります。

出力結果:
```
object HTMLButtonElement] {
  accessKey: "",
  addEventListener: function addEventListener() { [native code] },
...
```

### アロー関数とthis

**_アロー関数_**はES6から追加された関数の定義方法のひとつです。**_アロー関数_**の定義構文では下記のように`=>`の「矢印記号」を使って関数を定義するのが特徴です。

```js
//アロー関数による関数定義の例
let arrowFunc = () => console.log(this);
```

先ほどのボタン要素による関数呼び出しの例では、その関数内のthisは呼び出し元のボタン要素を指していました。ところが**_アロー関数_**を使った場合は、**windowオブジェクト**を指します。

```js
// アロー関数を使った場合
let btn = document.querySelector('button');

//アロー関数を使ったイベント処理の定義
let btnClicked = () => console.log(this);
btn.addEventListener('click', btnClicked);
```

出力結果
```
[object Window] {
  addEventListener: function addEventListener() { [native code] },
  alert: function alert() { [native code] },
...
```

このように**_アロー関数_**では、thisの指し示す内容が使われる状況によって変わることはなく、定義された時の対象を保持(束縛)し続けます。これは**_アロー関数_**の大きな特徴です。

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

## チャレンジ

- [チャレンジ8](./challenge/README.md)

## 更に学ぼう

### 動画で学ぶ

- [JavaScript入門 - ドットインストール](https://dotinstall.com/lessons/basic_javascript_v2)

### 本で学ぶ

- [Eloquent JavaScript 3rd Edition](http://eloquentjavascript.net/)
