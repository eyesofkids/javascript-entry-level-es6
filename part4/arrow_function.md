# 箭頭函式

## 前言

ECMAScript 6在2015年終於制訂好標準規格，我們簡稱ES2015或ES6規格。這對Javascript市場又起了一大震憾，現階段可以透過像[babel](http://babeljs.io)的編譯器來編輯給ES5(目前大部份的新瀏覽器都支援的規格)使用，也有很多的教學文件可以參考。但瀏覽器各家還在努力中，大概還需要至少一兩年的時間，才能完成大部份的新標準實作。在這之前，雖然還有很多路要走。

ES6的新特性很多，這裡研究其中的一個新特性 - "Arrow Functions"，我使用的是Typescript。

## Arrow Functions

這是啥米碗糕？以下三點簡單來說：

- 中文是"箭頭函式"
- 符號是"=>" (肥箭頭，"->"是瘦箭頭)
- 基本特性是函式的簡短寫法，(() => {} vs. function () {})

一個簡單的範例是：

```ts
var inc = (x) => x+1;
```

相當於

```javascript
var inc = function (x) { return x + 1; };
```

所以你可以少打很多英文，和一些標點符號之類的，所有的函式會變成匿名的函式。基本上的符號使用如下說明：

- 花括號"{}"可有可無，不過如果函式沒回傳東西就要花括號。例如 `()=>{}`
- 只有單一個傳入參數時，括號"()"可以不用，例如 `x=>x*x`

這個語法不是只有那麼簡單，它有一個很重要的特別用途：

- Lexical `this` binding (詞彙上綁定`this`變數)

因為Javascript中的`this`變數是一個很特別難搞的東西，它有可能在不同情況或舊的瀏覽器中，指向不同的數值。一個最典型的範例就是使用像setInterval的函式時：

```
function Person() {
    this.age = 0;
    console.log('base age: ' + this.age);

    setInterval(function growUp() {
        this.age++;
        console.log('new age: ' + this.age);
    }, 1000);
}

var p = new Person();
```

上面的範例的`'base age: ' + this.age`是有數值的，但在setInterval裡面的`'new age: ' + this.age`就抓不到`this`變數。

以前的解決這個問題的作法，是會先用`self = this`，然後在setInterval改用`self`。

有了`肥箭頭`之後，用這個就可直接解決這個問題，其實只是把`function growUp()`改成`()=>`：

```
function Person() {
    this.age = 0;
    console.log('base age: ' + this.age);

    setInterval(() => {
        this.age++;
        console.log('new age: ' + this.age);
    }, 1000);
}

var p = new Person();
```

看一下Typescript編譯的結果，這是給ES5瀏覽器用的，其實和之前的舊解決方式是類似的：

```
function Person() {
    var _this = this;
    this.age = 0;
    console.log('base age: ' + this.age);
    setInterval(function () {
        _this.age++;
        console.log('new age: ' + _this.age);
    }, 1000);
}
var p = new Person();
```

## 並非萬靈丹

肥箭頭函式用於解決一般的`this`問題是很好，但並不適用於全部的情況，尤其是在像jQuery、underscore之類有callback之類的函式時，有可能不是如預期般的結果。

## 參考資料

- [Arrow This](http://blog.getify.com/arrow-this/)
- [Arrow Functions](https://github.com/getify/You-Dont-Know-JS/blob/master/es6%20&%20beyond/ch2.md#arrow-functions)
- [TypeScript Deep Dive](https://basarat.gitbooks.io/typescript/content/docs/arrow-functions.html)
