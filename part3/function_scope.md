# 函式與作用域

## 函式(function)

函式在JavaScript中是很重要的一個語言特性，它是屬於物件。函式用於程式碼的重覆使用、資訊的隱藏與複合(composition)。我們經常會把一整組的功能程式碼，寫成一個函式，之後可以重覆再使用。

函式的基本語法結構如下:

```js
//匿名函式
function () {}

//有名稱的函式
function foo() {}
```

更多的範例:

```js

//使用有名稱的函式
function addTwo(num){
    return num+2
}

//用一個常數指定匿名函式
const sum = function(a, b){
    return a+b
}
```

函式的名稱也是一個識別符，命名方式如同變數or常數的命名規則。上面兩種方式對於函式來說都是可以用的宣告(or定義)方式。

函式的呼叫是使用函式名稱加上括號(())，以及在括號中傳入對應的參數，即可呼叫這個函式執行。例如:

```js
const newValue = addTwo(100)
console.log(sum(99, 1))
```

### 函式的傳入參數


### 內部函式(函式中的函式)

### 匿名函式與IIFE

### 箭頭函式(Arrow Function)

## 作用域(scope)

作用範圍(scope)或稱之為"作用域"，指的是"數值與運算敘述的可見範圍(可使用的範圍)"，在JavaScript中的作用域可分為本地的與全域的。

JavaScript程式語言的作用範圍是使用"函式作用範圍(function scope)"的，意思它基本上是以一個個函式的來區分其中的運算敘述與數值。因此，當你在所有函式的外面宣告變數(或常數)，這個變數(或常數)會變為"全域作用範圍"的一員，稱為"全域變數"。也就是說在任何的函式中都可以被存取得到。全域作用範圍容易造成數值的污染，而且常常會造成不經意的程式錯誤。

與scope相關的還有一個字詞Namespace(命名空間)，Namespace是一種語法結構或組織方法，讓程式設計師可以把不同的識別符與程式碼敘述，放到不同的"空間"之中，以免造成衝突或混亂。因此，在不同的命名空間(Namespace)可以有獨自的作用範圍(scope)，這在程式開發時是很重要的一種結構組織方式。其他的程式語言會使用namespace關鍵字(或package)來宣告命名空間。不過，JavaScript語言中並沒有內建的命名空間(Namespace)的特性，這部份就需要使用一些命名空間的解決方式。

而在ES6中採用了新的作法，加入"區塊作用範圍(block statement scope)"概念。其中之一，就是以`let`取代`var`的宣告變數的方式。

以下的範例參考自[JavaScript: The Good Parts](http://shop.oreilly.com/product/9780596517748.do)，為了簡單起見，我把一些運算簡化。

```js
let foo = function() {
    let a = 3
    let b = 5

    let bar = function() {
        let b = 7
        let c = 11
        // 此時，a is 3, b is 7, c is 11
        a = b + c
        // 此時，a is 18, b is 7, and c is 11
    }

    // 此時, a is 3, b is 5, c is not defined
    bar()
    // 此時，a is 18, b is 5, c is not defined
}

foo()
```

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

### 英文解說

scope/史溝波/ 在英文裡有"視野"、"導彈範圍"的意思。也就是相當於程式語言中，看不看得到(能不能存取得到)的意思。翻譯成"作用域"或"作用範圍"是有點文言，但它的意思就是這樣。

#### 全域作用範圍污染(global scope pollution)

全域作用範圍污染，一種是沒有經過`var`宣告的變數，會自動變為全域作用範圍，另一種是在把變數宣告在全域作用範圍中，過多的變數或常數，常常會造成記憶體無法回收，或是因為JavaScript是"函式作用範圍"的關係，造成全域變數與函式中的變數衝突的情況。這對程式的除錯也很困難。

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
