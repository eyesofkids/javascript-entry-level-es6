# 函式與作用域

## 函式(function)

函式(Function)是JavaScript的非常重要的特性。函式用於程式碼的重覆使用、資訊的隱藏與複合(composition)。我們經常會把一整組的功能程式碼，寫成一個函式，之後可以重覆再使用。

> 注意: 依據ECMAScript標準的定義，函式的`typeof`回傳值是'function'，而不是'object'。由此可見在標準中定義的'object'類型，只是針對"單純"的物件定義而言，具有函式實作的物件，將會歸類為`function`，例如Date、String、Object這些內建的物件，它們的`typeof`回傳值都是'function'。

> 註: 那麼要如何準確的檢查一個變數/常數是否函式類型？請參考這篇[How can I check if a javascript variable is function type?](http://stackoverflow.com/questions/5999998/how-can-i-check-if-a-javascript-variable-is-function-type)的問答。

### 函式定義

函式的基本語法結構如下:

```js
//匿名函式
function() {}

//有名稱的函式
function foo() {}
```

函式的名稱也是一個識別符，命名方式如同變數/常數的命名規則。

而匿名函式並沒有函式的名稱，通常用來當作一個指定值，指定給一個變數/常數，被指定後這個變數/常數名稱，就成了這個函式的名稱。實際上，匿名函式也有其他的用法，例如拿來當作其他函式的傳入參數值，或是進行一次性執行。

函式使用`return`作為最後的回傳值輸出，函式通常會有回傳值，但並非每種函式都需要回傳值，也有可能利用輸出的方式來輸出結果。以下兩種方式對於函式都是可以使用的宣告(定義)方式:

```js

//使用有名稱的函式
function sum(a, b){
    return a+b
}

//常數指定為匿名函式
const sum = function(a, b){
    return a+b
}
```

函式的呼叫是使用函式名稱加上括號(())，以及在括號中傳入對應的參數，即可呼叫這個函式執行。例如:

```js
const newValue = sum(100,0)
console.log(sum(99, 1))
```

ES6中有一種新式的函式語法，稱為"箭頭函式(Arrow Function)"，使用肥箭頭符號(Flat Arrow)(=>)，它是一種匿名函式的縮短寫法，只能用於有回傳值(return)的情況，下面這個寫法相當於上面的`sum`函式定義:

```js
const sum = (a, b) =>  a + b
```

箭頭函式(Arrow Function)因為語法簡單，而且可以綁定this變數，所以算得上最受歡迎的ES6新功能，現在在很多程式碼中被大量使用。我們在特性篇中裡會專門有一章的內容來說明箭頭函式(Arrow Function)。

> 註: this變數與物件有關，在物件的章節會說明。

### 傳入參數

函式的傳入參數是需要討論的，它是函式與外部環境溝通的管道，也就是輸入資料的部份。

不過，你可能會在函式的"傳入參數"常會看到兩個不同的英文字詞，一個是parameter(或簡寫為param)，另一個是argument，這常常會造成混淆，它們的差異在於:

- parameters: 指的是在函式的那些傳入參數名稱的定義。我們在文章中會以"傳入參數定義名稱"來說明。
- arguments: 指的是當函式被呼叫時，傳入到函式中真正的那些值。我們在文章中會以"實際傳入參數值"來說明。

#### 傳入參數預設值

關於函式的傳入參數預設值，在未指定實際的傳入值時，一定是`undefined`
，這與變數宣告後但沒有指定值很相似。而且在函式定義時，我們並沒有辦法直接限定傳入參數的資料類型，所以在函式內的語句部份，一開始都會進行實際傳入參數值的資料類型檢查。

有幾種方式可以用來在函式內的語句中，進行預設值的設定，例如用`typeof`的回傳值來判斷是否為`undefined`，或是用邏輯或(||)運算符的預設值設定方式。用來指定預設值的範例:

```js
//用邏輯或(||)
const link = function (point, url) {
    let point = point || 10
    let url = url || 'http://google.com'
    ...
}

//另一種設定的方式，typeof是回傳類型的字串值
const link = function (point, url) {
    let point = typeof point !== 'undefined' ? point : 10  
    let url = typeof url !== 'undefined' ? url : 'http://google.com' 
    ...
}
```

> 注意: 邏輯或(||)運算符設定預設值，雖然語法簡單，但有可能不精準。它會在實際傳入參數值只要是"falsy"，就直接指定為預設值。

在ES6中加入了函式傳入參數的預設值指定語法，現在可以直接在傳入參數時就定義這些參數的預設值，這個作法是建議的用法:

```js
const link = function (point = 10, url = 'http://google.com') {
    ...
}
```

#### 以函式作為傳入參數

前面有說明函式可以作為變數/常數的指定值。不僅如此，在JavaScript中函式也可以作為實際傳入參數的值，將一個函式傳入到另一個函式中作為參數值，而且在函式的最後也可以回傳函式。這種函式的結構稱之為"高階函式(Higher-order function)"，是一種JavaScript程式語言的獨特特性，高階函式可以讓在函式的定義與使用上能有更多的彈性，它也延申出很多不同的應用結構。你可能常聽到JavaScript的callback(回呼、回調)結構，它就是高階函式的應用。

習慣上，因為函式在定義時，它的傳入參數並沒辦法限定資料類型，所以當要定義傳入參數將會是個函式時，通常會用fn或func作為傳入參數名稱，以此作為辦別。不過，當你在撰寫一個函式時，最好是要加上傳入值的，以及回傳值的註解說明。以下為一個簡單的範例:

```js
const addOne = function(value){
    return value + 1
}

const addOneAndTwo = function(value, fn){
    return fn(value) + 2
}

console.log(addOneAndTwo(10, addOne)) //13
```

#### 無名的傳入參數(unnamed arguments)

這是函式的一種隱藏機制，實際上對於傳入的參數值是有一個隱藏在背後的物件，名稱為`arguments`，它會對傳入參數實際值進行保存，可以直接在函式內的語句中直接取用。`arguments`雖是一個物件資料類型，但它有"陣列"的一些基本特性，不過缺少很大一部份陣列中可使用的方法，所以被稱作"pseudo-array"(偽陣列)。以下為一個簡單的範例:

```js
function sum() {
  return arguments[0]+arguments[1]
}
  
console.log(sum(1, 100))
```

不過，如果你在函式的傳入參數定義名稱中，使用了arguments這個參數名稱，或是在函式中的語句裡，定義了一個名稱為arguments的自訂變數/常數名稱，這個隱藏的物件就會被覆蓋掉失效。總之它的行為相當怪異，有一說是說它一開始設計時就錯了，隱藏的物件對程式設計師來說，並不像隱藏版的寶箱，而是比較像是隱藏版的陷阱。

> 註: 關於arguments的詳細介紹，可以參考[The JavaScript arguments object…and beyond](https://javascriptweblog.wordpress.com/2011/01/18/javascripts-arguments-object-and-beyond/)

#### 不固定傳入參數(Variadic)與其餘(rest)參數

像下面這個範例中，原先sum函式中，定義了要有三個傳入參數，但如果真正在呼叫函式時傳入的參數值(arguments)並沒有的情況下，或是多出來的時候，會發生什麼情況？

前面有說到，沒有預設值的時候會視為`undefined`值，而多出來的情況，是會被直接略過。有的時候需要一種能夠"不固定傳入參數"的機制，在各種函式應用時，才能比較方便。

```js
function sum(x, y, z) {
  return x+y+z
}

console.log(sum(1, 2, 3))  //6
console.log(sum(1, 2))    //NaN
console.log(sum(1, 2, 3, 4))  //6
console.log(sum('1', '2', '3'))  //123
console.log(sum('1', '2')) //12undefined
console.log(sum('1', '2', '3', '4'))  //123
```

雖然上面已經有說過的，這個隱藏的arguments物件，它可以獲取到所有傳入的參數值，用類型陣列的方式來取用，但它的設計相當怪異(有一說是設計錯誤)，使用時要注意很多例外的情況，加上使用前根本也不需要定義(它是隱藏機制)，很容易造成閱讀上的困難。另外，在一些測試的報告中，用了arguments物件本身比有直接使用具有名稱傳入參數慢了幾倍。所以，arguments物件的方式能不用就不要使用。

那麼，有沒有其他的機制可以讓程式設計師能處理不固定的傳入參數？在ES6中加入了其餘參數(rest parameters)的新作法，它使用省略符號(ellipsis)(...)加在傳入參數名稱前面，其餘參數的傳入值是一個標準的陣列值，以下是一個範例:

```js

function sum(...value) {
    let total = 0
    for (let i = 0 ; i< value.length; i++){
        total += value[i]
    }   
    return total
}

console.log(sum(1, 2, 3))  //6
console.log(sum(1, 2))    //3
console.log(sum(1, 2, 3, 4))  //10
console.log(sum('1', '2', '3'))  //123
console.log(sum('1', '2')) //12
console.log(sum('1', '2', '3', '4'))  //1234
```

如果要寫得更漂亮、簡潔的的語法，直接使用Array(陣列)本身的好用方法，像下面這樣把原本的範例重寫一下:

```js
function sum(...value) {
    return value.reduce((prev, curr) => prev + curr )
}
```

> 註: reduce是陣列的方法之一，它可以用來作"累加"

其餘參數只是扮演好參數的角色，代表不確定的其他參數名稱，所以如果一個函式中的參數值有其他的確定傳入參數名稱，其餘參數名稱應該要寫在最後一個位子，而且一個函式只能有一個其餘參數名稱:

```js
function(a, b, ...theArgs) {
  // ...
}
```

其餘參數與arguments物件的幾個比較:

- 其餘參數只是代表其餘的傳入參數值，而arguments物件是代表所有傳入的參數值
- 其餘參數的傳入值是一個標準的陣列，可以使用所有的陣列方法。而arguments物件是"偽"陣列的物件類型，不能使用陣列的大部份方法
- 其餘參數需要定義才能使用，arguments物件不需要定義即可使用，它是隱藏的機制。

### 內部(巢狀)函式

函式中的語句中，還可以包含其他的函式，這稱為內部函式(inner)，或是巢狀函式(nested)的結構。以下為一個簡單的範例:

```js
function addOuter(a, b) {

    function addInner() {
        return a + b
    }
 
    return addInner()
}
 
addOuter(1, 2) //3
```

這樣的程式碼有點類似下面的寫法，不過你可以仔細的比較一下這兩個程式碼之間，傳入參數值的差異:

```js
function addOuter(a, b) {
    return addInner(a, b)
}

function addInner(a, b) {
    return a + b
}

addOuter(1, 2) //3
```

內部函式會具有外部函式所包含的環境值，例如外部函式的傳入參數、宣告變數等等。而內部函式又可以成為一種外部函式的回傳值，所以當內部函式接收到外部函式的環境值，又被回傳出去，內部函式間接變成一種可以讓函式對外曝露函式內部環境值的溝通管道。

內部函式在JavaScript中被廣泛的使用，因為它可以形成所謂"閉包"(closure)的結構。不過對於初學者來說，是不容易理解的一種特性，因為除了它的語法結構之外，還涉及到很多語言內部的設計部份。"閉包"(closure)在特性篇中有一個獨立的章節來說明。

此外，內部函式在JavaScript的物件導向中，也扮演了重要的角色，這個可能會讓初學者覺得很訝異，之後在其他章節的內容再說明了。

## 作用範圍(scope)

"作用範圍(scope)"或稱之為"作用域"，指的是"變數/常數定義與語句的可被使用的範圍"，作用範圍可分為本地端的(local)與全域的(global)。

JavaScript的作用域是使用"函式作用範圍(function scope)"的，或稱之以函式為基礎(function-based)，所以是它是以函式的來區分語句與值定義。

因此，當你在所有函式的外面宣告變數/常數，這個變數/常數即會成為"全域作用範圍"的一員，稱為"全域變數/常數"，也就是說在程式碼裡的任何函式中都可以存取得到。

> 註: 在JavaScript語言中，你應該要把"函式"也當作一種"值"來看待，它可以被指定到一個變數/常數，也可以作為函式回傳值。而在作用範圍中的行為也和"值"類似。

而在ES6中採用了新的作法，加入"區塊作用範圍(block scope)"概念。其中之一就是以`let`與`const`取代`var`的宣告變數的方式。

如果是使用`var`來定義變數，程式碼中的x它不是在函式中定義的，所以會變為"全域變數"：

```js
if (true) {
  var x = 5
}

console.log(x) //5
```

對比使用`let`來宣告變數，程式碼中的y位於區塊中，無法由外部獲取得到：

```js
if (true) {
  let y = 5
}

console.log(y) //y is not defined
```

### callback

### 匿名函式與IIFE

### Hoisting

### 全域作用範圍污染(global scope pollution)

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


### 英文解說

scope/史溝波/ 在英文裡有"視野"、"導彈範圍"的意思。也就是相當於程式語言中，看不看得到(能不能存取得到)的意思。翻譯成"作用域"或"作用範圍"是有點文言，但它的意思就是這樣。

Context名詞，在程式語言中指的是程式碼的上下文內容，在JavaScript語言中Scope是屬於以Function為基礎的(function-based)，那麼Context則是以Object為基礎的(object-based)

http://ryanmorr.com/understanding-scope-and-context-in-javascript/

Namespace(命名空間)與作用範圍(scope)相關的還有一個Namespace(命名空間)，這是一種語法結構或組織方法，讓程式設計師可以把不同的識別符與程式碼敘述，放到不同的"空間"之中，以免造成衝突或混亂。因此，在不同的命名空間(Namespace)可以有獨自的作用範圍(scope)。不過，JavaScript語言中並沒有內建的命名空間(Namespace)的特性，為了解決作用範圍(scope)的問題，這部份就需要使用一些命名空間的解決方式。


## 參考

http://ryanmorr.com/understanding-scope-and-context-in-javascript/