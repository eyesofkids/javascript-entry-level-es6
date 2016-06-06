#作用域

### 作用範圍(scope)

作用範圍(scope)或稱之為"作用域"，指的是"數值與運算敘述的可見範圍(可使用的範圍)"。Javascript程式語言的作用範圍是使用"函式作用範圍(function scope)"的，意思它基本上是以一個個函式的來區分其中的運算敘述與數值。在ES6之後有新的作法，加入"區塊作用範圍(block statement scope)"概念。

當你在所有函式的外面宣告變數(或常數)，這個變數(或常數)會變為"全域作用範圍"的一員，稱為"全域變數"。也就是說在任何的函式中都可以被存取得到。全域作用範圍容易造成數值的污染，而且常常會造成不經意的程式錯誤。

其中的x變為全域變數：

```
if (true) {
  var x = 5;
}
console.log(x);
```

ES6之後的可以使用`let`來宣告變數：

```
if (true) {
  let y = 5;
}
console.log(y);
```

#### 全域作用範圍污染(global scope pollution)

全域作用範圍污染，一種是沒有經過`var`宣告的變數，會自動變為全域作用範圍，另一種是在把變數宣告在全域作用範圍中，過多的變數或常數，常常會造成記憶體無法回收，或是因為Javascript是"函式作用範圍"的關係，造成全域變數與函式中的變數衝突的情況。這對程式的除錯也很困難。

我們在撰寫程式時，應遵守基本的一些好習慣原則，而且儘量不要在全域作用域宣告變數或常數。當然除了在學習階段測試一些簡單的程式之外。這樣就可以避免有這種污染情況發生。

以下的範例來自[這裡的問答](http://stackoverflow.com/questions/8862665/what-does-it-mean-global-namespace-would-be-polluted/13352212)：

```
var x = 10;
function example() {
    var x = 20;
    console.log(x); //Prints 20
}
example();
console.log(x); //Prints 10
```

```
var x = 10;
function example() {
    x = 20; //Oops, no var statement
    console.log(x); //Prints 20
}
example();
console.log(x); //Prints 20... oh dear
```
