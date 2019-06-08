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
```

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/wy4ckfv6/1/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

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

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/jztan61k/1/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

宣言キーワードの`var`、`let`、`const`を付けずに無冠で変数宣言すると、ブロック内で宣言したとしても、自動的に**_グローバルスコープ_**となってしまいます。

```js
{
  block = 100;
}

// ブロック内にアクセスできてしまう
console.log(block); // 100
```

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/rfz13g7y/1/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

また**_if文_**、**_for文_**などの制御文の中で宣言キーワードなしで変数宣言した場合や、古い宣言方法である`var`を使って宣言された変数は**_ローカルスコープ_**を持ちません。

```js
for (i = 0; i < 10; i++){} //カウンタ変数の宣言キーワードなし

console.log(i); // 10が出力される。つまりアクセスが可能
```

以下のように、結果は**カウンタ変数i**を`var`で宣言しても変わることはありません。

```js
for (var i = 0; i < 10; i++) {} //カウンタ変数のvar宣言

console.log(i); // 10が出力される。つまりアクセスが可能
```

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/ms1htxdk/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

この例のように制御文の外からでもアクセスできてしまう場合、これでは1つのプログラムの中で複数の制御文を記述する際に混乱を招いたり、わざわざ変数名を新しく作らないといけなかったりと不便です。この問題は`let`と`const`を使った宣言ができるようになったことで解決されました。従って宣言時には必ずこれらの新しいキーワードを明記するようにしましょう。
