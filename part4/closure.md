# Closure 閉包

## Closure 閉包

由於JavaScript語言中的函式(function)是頭等函式(first-class function)的設計，代表函式在語言中的應用上享有與一般原始資料類型的值同等地位，函式可以傳入其他函式作為傳入參數，可以當作另一個函式的回傳值，也可以指定為一個變數的值，或是儲存在資料結構中(例如陣列或物件)，在語言中甚至是有自己獨有的資料類型(`typeof`一個函式回傳值是'function')。

一個閉包就是記憶函式以及函式建立當下環境的記錄，環境指的就是獨立變數(自由變數)的值。在JavaScript中，每次當函式被建立時，一個閉包就會被產生，閉包只是一個函式產生時的自有特性，並不是什麼獨特的語法樣式。雖然我們經常使用函式中的函式，也就是巢狀(nested)函式(或內部函式)的語法結構作為範例來說明閉包，這的確是可以觀察閉包的產生的方式，不過這也只是方便說明而且是最常見的情況，但這並非是絕對的。

```js
function aFunc(x){
  function bFunc(){
    console.log( x++ )
  }
  return bFunc
}

const newFunc = aFunc(1)
newFunc()
newFunc()
```

bFunc可以不需要名稱，直接用return匿名函式的語法更簡潔:

```js
function aFunc(x){
  return function(){
    console.log( x++ )
  }
}
```

用箭頭函式更是簡潔，已經快要可以寫成一行了:

```js
function aFunc(x){
  return () => console.log( x++ )
}
```

執行這個範例後，你會發現`x`值會在`aFunc`呼叫後屍骨未寒，陰魂不散的還留在新的`newFunc`函式裡，每當執行一次`newFunc`函式，`x`值就會再+1。

不過，用這個這個閉包範例可能會產生誤解的地方，在於以下幾點:

- "函式呼叫"與"函式建立"實際上是兩件事，在`aFunc`"函式呼叫"過後，`newFunc`才是"函式建立"，這時候`newFunc`中的閉包才會產生。
- 匿名函式的使用，閉包並不是只會在匿名函式才會產生。
- 閉包不是只會產生在巢狀(內部)函式的回傳時。所有函式在建立時都會產生閉包。

要觀察閉包中所記錄的環境變數，可以從瀏覽器的除錯器中看到，像上面的範例如果用除錯器在第一個`newFunc`函式加入中斷點時，執行後應該可以看到像下面的圖:

![Closoure in Scope](https://raw.githubusercontent.com/eyesofkids/javascript-entry-level-es6/master/assets/closure_1.png)


http://stackoverflow.com/questions/111102/how-do-javascript-closures-work?rq=1
https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36#.rsbh1mas5
http://eloquentjavascript.net/03_functions.html
https://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/

## Closure範例程式碼

Closure(閉包)的基本結構是函式中的函式，也就是巢狀(nested)函式，或是稱之為內部(inner)函式的語法結構，

先看範例，一個典型的closure(閉包)會長這個樣子：

```javascript

var varGlobal = 'x';

function outer(paramOuter){
  var varOuter ='y';

  function inner(paramInner){
    var varInner ='z';

    //print
    console.log(varGlobal);
    console.log(varOuter);
    console.log(varInner);
    console.log(paramOuter);
    console.log(paramInner);
  }

  return inner;
}

var func = outer('a');
func('b');

```

## 圖解

用簡單的圖來表示上面的例子，內部(Inner)函式被回傳後，除了自己本身的程式碼外，也會捕抓到了環境的變數值，記住了執行當時的環境：

![Closure](http://eddychang.me/images/closure.png)

> Javscript中的closure就是應用了內部函式的一種語法實作，你可以把它當作是一種資料結構，同時儲存了函式與相關的環境中變數的參考。


## 為什麼可以這麼作？

- 函式(Function)是"函式類型物件(Object)"：既然函式是物件，它可以像一般的數值使用，可以在變數、物件或陣列中儲存，也可以傳入另外的函式裡當參數，也可以被另外的函式當回傳值回傳。
- 內部函式(或巢狀函式)：即是一個函式在另一個函式之中。
- "Function Scope"(函式作用域)規則：內部函式可以看到(或存取得到)外部函式(outer function，即包含內部函式的那個函式)，而形成一個Scope Chain(作用域連鎖)，closure可以有三個作用域：
  - 自已本身的
  - 外部函式的
  - 全域的
- 匿名(anonymous)函式：可以不需要有函式的名稱。（註：上面的範例程式碼中的inner函式可以直接寫在return中）

```javascript

function outer(paramOuter){
  var varOuter ='y';

  return function(paramInner){
    var varInner ='z';

    //print
    console.log(varGlobal);
    console.log(varOuter);
    console.log(varInner);
    console.log(paramOuter);
    console.log(paramInner);
  };
}

```

> 大部份程式語言(C語言類似)都是使用"block scope(區塊作用域)"，這是說在「花括號{}」中的變數或參數，是不能被外界所看到的(可使用的)。在區塊中的程式碼執行完之後，變數及參數的生命就終結了。

> 在Javascript語言設計時為了一些理由，因為它是個用於網頁上的動態腳本語言、為了效能或簡化語法，所以改以使用"Function Scope"。

## 用在何時何處

### 物件封裝/Object factories(物件工廠)

使用closure來宣告Javascript中的物件，而不是用Prototype與new語法。程式碼來自[Why use "closure"?](http://howtonode.org/why-use-closure)：

```javascript
// Define the factory
function newPerson(name, age) {

  // Store the message in a closure
  var message = name + ", who is " + age + " years old, says hi!";

  return {

    // Define a sync function
    greet: function greet() {
      console.log(message);
    },

    // Define a function with async internals
    slowGreet: function slowGreet() {
      setTimeout(function () {
        console.log(message);
      }, 1000);
    }

  };
}

var tim = newPerson("Tim", 28);
tim.greet();
```

傳統的寫法如下：

```javascript
// Define the constructor
function Person(name, age) {

  // Store the message in internal state
  this.message = name + ", who is " + age + " years old, says hi!";

};

// Define a sync method
Person.prototype.greet = function greet() {
  console.log(this.message);
};

// Define a method with async internals
Person.prototype.slowGreet = function slowGreet() {
  var self = this; // Use a closure to preserve `this`
  setTimeout(function () {
    console.log(self.message);
  }, 1000);
};
```


### Module pattern

和上面的範例有點類似，不過重點是放在實作私有(Private Property與Method)。
程式碼來自[Understanding Scope and Context in JavaScript](http://ryanmorr.com/understanding-scope-and-context-in-javascript/)

```javascript
var Module = (function(){
    var privateProperty = 'foo';

    function privateMethod(args){
        // do something
    }

    return {

        publicProperty: '',

        publicMethod: function(args){
            // do something
        },

        privilegedMethod: function(args){
            return privateMethod(args);
        }
    };
})();
```

### 針對events與callback

最常見的是與setTimeout()與setInterval()搭配，這兩個都有callback傳入參數，這有兩種情況，第一種是為了解決IE中的支援問題。

#### IE舊版本問題

以setTimeout()為例，如果我們想要傳入一個參數值到setTimeout()的callback中，你大概會直接這樣寫：

```javascript
function printMessage(message){
    console.log(message);
}

setTimeout(printMessage("hello"),3000);
```

看起來沒問題，但這個程式碼在IE9以下版本是不支援的，這裡有[一段說明](https://msdn.microsoft.com/zh-tw/library/ms536753(v=vs.85).aspx)：

> Note In Windows Internet Explorer, you cannot pass arguments to the callback function directly; however, you can simulate passing arguments by creating an anonymous closure function that references variables within scope of the call to setInterval or setTimeout.

不能直接傳入參數就是，要用Closure。像下面這樣的程式碼：

```javascript
function printMessage(message){

    return function() {
        console.log(message);
    };
}

setTimeout(printMessage("hello"),3000);
```

> Internet Explorer 10 之後版本可以直接傳入參數到setTimeout()與setInterval()中的callback函式中。

#### loop(迴圈)裡的setTimeout()與setInterval()

這也是個典型的問題，事實上在loop(迴圈)裡的callback都不是像你想的那麼直覺得達成你想要的，先看一下setTimeout()的這個程式碼：

```javascript
for(var i = 0; i < 5; i++){
  setTimeout(function() { console.log(i); }, 5000);
}
```

你可能會想要輸出像「0,1,2,3,4,5」這樣的結果，但事實上這個程式碼會輸出「5,5,5,5,5,5」。因為Javascript的callback是異步的，而且是單執行緒的，你在loop(迴圈)中使用callback是沒錯，但因為變數`i`不斷在迴圈中改變數值，一直到最後的`5`才停止，然後再換callback表演時，這時候callback看到的變數`i`就都是`5`。如果我們要記住所在當時環境的變數，要使用closure才作得到：

```javascript
function print(i){
  return function(){
    console.log(i);
  }
}

for(var i = 0; i < 5; i++){
  setTimeout(print(i), 5000);

}
```

這個closure中的函式要分開寫，如果都擠在`setTimeout`的callback裡是不會動的。而且如果你執行這一段程式碼會發現一件特別的事情，事實上所有的輸出「0,1,2,3,4,5」結果，都是在`5`秒後一次輸出，這對照上面說的異步callback機制。

#### 小抄

要在迴圈中使用callback是有特定的另一種語法，可以用匿名函式的作法。你可以用來自[這個網站](http://sanjeev.dwivedi.net/?p=348)的程式碼：

```js
for (var i = 0; i < arr.length; i++) {
  (function(index) {
    // 在這裡面作你要作的事
    // 使用index變數 - 它就是迴圈運算中傳遞過來的'i'數值
  })(i);
}
```

所以上面的`setTimeout`例子可以寫成這樣：

```js
for (var i = 0; i < 5; i++) {
  (function(index) {
    setTimeout(function(){
      console.log(index);
    }, 10000);
  })(i);
}
```

> 在for迴圈中的語法稱為"Immediately-Invoked Function Expression"（立即被呼叫的函式語樣）或簡稱為"IIFE"

## 結論

這篇只有摘要最重要相關說明，如果你想要知道closure是怎麼作到的，那是很底層的技術了，有興趣可以到網路上找找。closure是Javascript中很重要也很強大的一種語法結構，現在到處都可以看到實作的地方。

## 參考資料

- [Why use "closure"?](http://howtonode.org/why-use-closure)
- [Understanding Scope and Context in JavaScript](http://ryanmorr.com/understanding-scope-and-context-in-javascript/)
- [setTimeout method ~MSDN](https://msdn.microsoft.com/zh-tw/library/ms536753(v=vs.85).aspx)
- [HOW TO PASS LOOP VARIABLES IN A JAVASCRIPT CALLBACK](http://sanjeev.dwivedi.net/?p=348)
