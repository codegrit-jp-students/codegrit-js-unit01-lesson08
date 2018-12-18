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