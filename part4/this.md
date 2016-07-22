# this

在其他以類別為基礎的程式語言中，`this`指的是目前使用類別進行實體化的物件。而JavaScript語言中因為沒有類別這種東西，設計上也不一樣，`this`的指向的是目前呼叫函式或方法的擁有者(owner)物件，也就是說它與函式如何被調用有關，雖然是同一函式的呼叫，因為不同的物件呼叫，也有可能是不同的this值。

## 函式的呼叫

我們在"函式與作用域"、"物件"與"原型物件導向"的章節中，都有看到函式的一些說明內容，也有看到用函式作為建構式來作物件實體化的工作，這時候會看到以`this`的一些說明，那麼在不是用來當作建構式的函式中，就是我們所認知的一般函式，裡面也有`this`嗎？有的，不論是在物件中的方法，或是一般的函式，每個函式中都有`this`值。以下是一個很簡單的範例，一個是我們所認知的普通函式，一個是在物件中的方法: 

```js
function func(param1, param2){
  console.log(this)
}

const objA = {
  test(){
    console.log(this)
  }
}

func() //undefined
objA.test() //Object(objA)
```

`func`函式在呼叫時的`this`值是`undefined`，原本它應該會回傳全域物件，在瀏覽器中就是window物件，這是因為babel預設會開啟strict mode(嚴格模式)，為了安全性的理由，原本的全域物件變成了`undefined`。

`objA.test`方法在呼叫時的`this`值就是`objA`物件本身，這一點不難理解。用以下的範例可以檢查`this`和`objA`是不是相同。

```js
const objA = {
  test(){
    console.log(this)
    console.log(this === objA) //true
    console.log(objA)
  }
}

objA.test()
```

不過這裡有個小地方會讓你覺得很不可思議的是，`ObjA`物件中的方法竟然可以讀取到`ObjA`物件？

是的，實際上它不止物件中可以讀取到物件本身，物件中的方法還可以執行自己本身的方法，下面的程式碼還可以讓你的瀏覽器無止盡的執行，然後當掉:

```js
//注意: 瀏覽器有可能會當掉
const objA = {
  test(){
    objA.test()
  }
}
```

在函式中也可以這樣作，也是會讓你的瀏覽器最後當掉，這是都是錯誤的示範:

```js
//注意: 瀏覽器有可能會當掉
function func(param1, param2){
  func()
}

func()
```

以上算是題外話，不過這部份在後面可以簡單的說明為什麼可以這樣作。我們先關心幾個重要的議題。

## 深入 函式 中

> 所有的函式在呼叫時，其實都有一個擁有者物件來進行呼叫。所以你可以說，其實所有函式都是物件中的"方法"。所有的函式執行都是以Object.method()方式呼叫。

關於這一點，下面的範例就可以說明一切了，有個全域物件(在瀏覽器中是window物件)是所有函式在呼叫時預設物件，下面三種函式呼叫都是同樣的作用:

```js
function func(param){
  console.log(this)
}

window.func() //只能在不是strict mode下執行
this.func()  //只能在不是strict mode下執行
func()
```

> 對this值來說，它根本不關心函式是在哪裡定義或是怎麼定義的，它只關心是誰呼叫了它。

在JavaScript中函式是一個很奇妙的東西，它的確是一個物件類型，又不太像是一般的物件，以`typeof`的回傳值來說，它回傳的是`function`，代表擁有獨立的回傳類型值。在函式的定義中，它比一般物件多了幾個特別的屬性與方法，其中最特別的是以下這三個，我把它們的定義寫出來:

- call: 以個別提供的`this`值與傳入參數值來呼叫函式
- bind: 建立一個新的函式，這個新函式在呼叫時，會以提供的this值與一連串的傳入參數值來進行呼叫。
- apply: 與call方法功能一樣，只是傳入參數值使用陣列

這個`call`方法是在ES5標準中加入的，這與直接使用一般的程式呼叫方式來執行函式，例如`func()`有何不同？

基本上完全一樣，除了它在參數裡可以傳入一個物件，讓你可以轉換函式的原本的上下文(context)到新的物件之前。(註: Context的說明在下面)

`call`方法可以把函式的定義與呼叫拆成兩件事來作，定義是定義，呼叫是呼叫。以下為一個範例:

```js
function func(param1){
  console.log('func', this)
}

const objA = {
  methodA(){
    console.log('objA methodA', this)
  }
}

const objB = { a:1, b:2 }

func.call(objB) //func Object {a: 1, b: 2}
objA.methodA.call(objB) //objA methodA Object {a: 1, b: 2}
```

這種現實讓你對函式的印象崩壞，不論是一般的`func`呼叫，或是位於物件`objA`中的方法`methodA`，使用了call方法後，竟然`this`值就會變成`call`中的第一個傳入參數值，也就是物件`objB`。~~有種辛辛苦苦養大的小孩，竟然被男(女)朋友拐跑了的心情。~~

`bind`方法更是厲害，它會從原有的函式或方法定義，產生一個新的方法。為了展示它的厲害之處，我幫函式加了傳入參數，我把兩個範例分開，下面是函式部份:

```js
function funcA(param1, param2){
  console.log(this, param1, param2)
}

const objB = { a: 1, b: 2 }

funcA() //undefined undefined undefined

const funcB = funcA.bind(objB, objB.a)

funcB() //Object {a: 1, b: 2} 1 undefined
funcB(objB.b) //Object {a: 1, b: 2} 1 2
```

這是物件中的方法定義的範例，這和上面沒什麼兩樣，只是函式定義在物件objA之中而已:

```js
const objA = {
  methodA(param1, param2){
    console.log('objA methodA', this, param1, param2)
  }
}

const objB = { a: 1, b: 2 }

objA.methodA()

const methodB = objA.methodA.bind(objB, objB.a)
methodB()
methodB(objB.b)
```

不過因為用了物件，應該要讓方法可以直接使用物件中的屬性才是妥善利用，另一種範例如下:

```js
const objA = {
  a: 8,
  b: 7,
  methodA(){
    console.log(this, this.a, this.b)
  }
}

const objB = { a: 1, b: 2 }

objA.methodA() //Object {a: 8, b: 7} 8 7

const methodB = objA.methodA.bind(objB, objB.a)

methodB() //Object {a: 1, b: 2} 1 2
methodB(objB.b) //Object {a: 1, b: 2} 1 2
```

從上面的例子中，可以看到這個bind方法可以用原有的函式，產生一個稱為部份套用(Partially applied)的新函式，也就是對原有的函式的傳入參數值固定住部份傳入參數的值(從左邊開始算)。這是一種很特別的特性，有一些應用情況會用到它。

上面可以看到結論中的事實，函式定義是定義，呼叫是呼叫，`this`的確不在乎函式是在物件中還是在哪裡定義的，還是裡面是定義什麼。`this`只在意誰(物件)呼叫了這個函式。以下為幾個問題的說明。

> 註: function call與function invoke(invocation)是同意義字詞

### this值是何時產生的？

當函式被呼叫(call/invoke)時，有個新物件會被建立，裡面會包含一些資訊，例如傳入的參數值是什麼、函式是如何被呼叫的、函式是被誰呼叫的等等。這個物件裡面有個主要的屬性this參照值，指向呼叫這個函式的物件。

### Context是什麼？

`Context`這個字詞是不易理解的，在英文裡有上下文、環境的意思，什麼叫作"上下文"？這中文翻譯也是有看沒有懂，還記得在國中或高中英文課的時候，英文老師有說過，有些英文字詞的意思需要用"上下文"來推敲才知道它的意思，為什麼要這樣作？老師一定沒有把原因說得很清楚，第一個原因是英文單字你學得不夠多，很多時候考試的試題中通常你都沒讀到，第二個原因是，英文字詞很多時候一個字詞有很多種意思，有時候用於動詞與名詞是兩碼子事，例如"book"你應該第一時間就會說它是"書"，國小就學過了，但是那是用於名詞的情況，用於動詞是"預訂"的意思。

在程式語言中的`Context`指的是物件的環境之中，也就是處於物件所能提供的資料組合中，這個`Context`是由`this`值來提供。

用白話一點的講法來看函式與`this`的關係，`this`是魔法師的角色，而函式是要施展的魔法，魔法的強度或破壞力，會依施法者的資質與能力有所不同，魔法在施展中會運用到施法者的本身的資質(智慧、MP、熟練度…等等)的這整體的素質特性，這就是所謂的`Context`了。

> 註: `Context`與常會看到的另一個名詞`Execution Context`(執行上下文)的意義是不同的，以下會有說明。

## 四種函式呼叫樣式(invocation pattern)

https://howchoo.com/g/ztbjzjqwngq/javascript-function-invocation-and-this-with-examples

https://debugmode.net/2013/09/03/invocation-patterns-in-javascript/

## Scope vs Context

Scope(作用域, 作用範圍)指的是在函式中變數(常數)的可使用範圍，JavaScript使用的是靜態或詞法的(lexical)作用域，意思是說作用域在函式定義時就已經決定了。JavasScript中只有兩種的Scope(作用域)，全域(global)與函式(function)。

Context(上下文)指的是函式在被呼叫執行時，所處的物件環境。上面已經有很詳細的解說了。

> Scope通常被稱為Variable Scope(變數作用域)，意思是代表"變數可存取的作用域"。

> Context通常被稱為this Context，意思是"由this值所代表的上下文"。

## 執行上下文(Execution Context)

執行上下文(Execution Context)看起來與上下文(Context)頗像，但嚴格的來說指的是不同的概念。

JavaScript使用執行上下文(Execution Context)的抽象概念來說明程式是如何被執行的，在ES標準中並沒有明確規定它應該是一個什麼樣的結構。

> 所有的JavaScript程式碼都是在某個執行上下文中被執行。

當一個函式被呼叫時，會產生一個新的物件，裡面包含函式要如何呼叫、呼叫什麼、被誰(物件)呼叫，這個新物件中會有一個this屬性。

All javascript code is executed in an execution context. Global code (code executed inline, normally as a JS file, or HTML page, loads) gets executed in global execution context, and each invocation of a function (possibly as a constructor) has an associated execution context. Code executed with the eval function also gets a distinct execution context but as eval is never normally used by javascript programmers it will not be discussed here. The specified details of execution contexts are to be found in section 10.2 of ECMA 262 (3rd edition).

When a javascript function is called it enters an execution context, if another function is called (or the same function recursively) a new execution context is created and execution enters that context for the duration of the function call. Returning to the original execution context when that called function returns. Thus running javascript code forms a stack of execution contexts.

When an execution context is created a number of things happen in a defined order. First, in the execution context of a function, an "Activation" object is created. The activation object is another specification mechanism. It can be considered as an object because it ends up having accessible named properties, but it is not a normal object as it has no prototype (at least not a defined prototype) and it cannot be directly referenced by javascript code.

The next step in the creation of the execution context for a function call is the creation of an arguments object, which is an array-like object with integer indexed members corresponding with the arguments passed to the function call, in order. It also has length and callee properties (which are not relevant to this discussion, see the spec for details). A property of the Activation object is created with the name "arguments" and a reference to the arguments object is assigned to that property.

Next the execution context is assigned a scope. A scope consists of a list (or chain) of objects. Each function object has an internal [[scope]] property (which we will go into more detail about shortly) that also consists of a list (or chain) of objects. The scope that is assigned to the execution context of a function call consists of the list referred to by the [[scope]] property of the corresponding function object with the Activation object added at the front of the chain (or the top of the list).

Then the process of "variable instantiation" takes place using an object that ECMA 262 refers to as the "Variable" object. However, the Activation object is used as the Variable object (note this, it is important: they are the same object). Named properties of the Variable object are created for each of the function's formal parameters, and if arguments to the function call correspond with those parameters the values of those arguments are assigned to the properties (otherwise the assigned value is undefined). Inner function definitions are used to create function objects which are assigned to properties of the Variable object with names that correspond to the function name used in the function declaration. The last stage of variable instantiation is to create named properties of the Variable object that correspond with all the local variables declared within the function.

The properties created on the Variable object that correspond with declared local variables are initially assigned undefined values during variable instantiation, the actual initialisation of local variables does not happen until the evaluation of the corresponding assignment expressions during the execution of the function body code.

It is the fact that the Activation object, with its arguments property, and the Variable object, with named properties corresponding with function local variables, are the same object, that allows the identifier arguments to be treated as if it was a function local variable.

Finally a value is assigned for use with the this keyword. If the value assigned refers to an object then property accessors prefixed with the this keyword reference properties of that object. If the value assigned (internally) is null then the this keyword will refer to the global object.

The global execution context gets some slightly different handling as it does not have arguments so it does not need a defined Activation object to refer to them. The global execution context does need a scope and its scope chain consists of exactly one object, the global object. The global execution context does go through variable instantiation, its inner functions are the normal top level function declarations that make up the bulk of javascript code. The global object is used as the Variable object, which is why globally declared functions become properties of the global object. As do globally declared variables.

The global execution context also uses a reference to the global object for the this object.

---

Execution context is different and separate from the scope chain in that it is constructed at the time a function is invoked (whether directly – func(); – or as the result of a browser invocation, such as a timeout expiring). The execution context is composed of the activation object (the function's parameters and local variables), a reference to the scope chain, and the value of this.

Execution context is a concept in the language spec that—in layman's terms—roughly equates to the 'environment' a function executes in; that is, variable scope (and the scope chain, variables in closures from outer scopes), function arguments, and the value of the this object.

The call stack is a collection of execution contexts.

## scope chain

This is because JavaScript uses static scope; that is, the scope chain of a function is defined at the moment that function is created and never changes; the scope chain is a property of the function.

The scope chain of the execution context for a function call is constructed by adding the execution context's Activation/Variable object to the front of the scope chain held in the function object's [[scope]] property, so it is important to understand how the internal [[scope]] property is defined.

In ECMAScript functions are objects, they are created during variable instantiation from function declarations, during the evaluation of function expressions or by invoking the Function constructor.

Function objects created with the Function constructor always have a [[scope]] property referring to a scope chain that only contains the global object.

Function objects created with function declarations or function expressions have the scope chain of the execution context in which they are created assigned to their internal [[scope]] property.



## call stack

Each time a function is called, its parameters and this value are stored in a new 'object' on the stack.


http://stackoverflow.com/questions/20279484/how-to-access-the-correct-this-context-inside-a-callback

## this的分界限

當函式被呼叫執行時，this值隨之產生，那如果是函式中的函式呢？像下面這樣的巢狀或內部函式的結構:

```js
const obj = {a:1}

function outter() {

  function inner(){
    console.log(this)
  }

  inner()
}

outter.call(obj) //undefined
```

結果是`undefined`，內部的`inner`函式不知道`this`值是什麼呢，為什麼？因為執行上下文是以函式呼叫作為區分，所以`this`值在不同的函式呼叫時，預設上就會不同。這稱之為`this`或`Context`的界限。

解決方式是要利用作用域鏈(Scope Chain)的設計，也就是說，雖然inner函式與外面的outter分屬不同函式，但inner函式具有存取得到outter函式的作用域的能力，所以可以用這樣的解決方法:

```js
const obj = {a:1}

function outter() {
  //暫存outter的this值
  const that = this

  function inner(){
    console.log(that) //用作用域鏈讀取outter中的that值
  }

  inner()
}

outter.call(obj) //Object {a: 1}
```

`that`是一個隨你高興的變數(常數)名稱，這並不是什麼特殊的關鍵字或保留字，也有人喜歡取`self`或`_that`。它只是為了暫時保存在outter函式被呼叫時的`this`值用的，讓`this`可以傳遞到inner函式之中。

第二種寫法其實也是同樣的概念，只不過用了call來呼叫，outter函式在呼叫時，它裡面是有`this`值的，因此可以當作call的傳入參數值，這範例與上面相同，也是有同樣作用:

```js
const obj = {a:1}

function outter() {

  function inner(){
    console.log(this)
  }

  inner.call(this)
}

outter.call(obj) //Object {a: 1}
```

第三種寫法是用`bind`方法，不過因為`bind`方法會回傳新的函式，函式宣告要變成用函式表達式的方法才行:

```js
const obj = {a:1}

function outter() {

   const inner = function(){
    console.log(this)
  }.bind(this)

  inner()
}

outter.call(obj) //Object {a: 1}
```

那麼在callback(回調)的情況下又是如何？this能順利傳到callback(回調)函式之中嗎？像下面的範例這樣，結果當然是不行:

```js
const obj = {a:1}

function funcCb(cb){
  cb()
}

const callback = function(){ console.log(this) }

funcCb.call(obj, callback) //undefined
```

實際上傳入參數值這個東西，如果是函式的話，都是位於全域物件之下的，這callback(回調)在呼叫時的`this`值就是全域物件。用call或bind方法就可以解決這個問題:

```js
const obj = {a:1}

function funcCb(cb){
  cb.call(this)
}

const callback = function(){ console.log(this) }

funcCb.call(obj, callback) //Object {a: 1}
```

更進階的一種情況，使用例如像`setTimeout`方法，裡面帶有callback(回調)函式的傳入參數，像下面這樣的程式碼:

```js
const obj = {a:1}

function func(){
  setTimeout(
    function(){
      console.log(this)
    }, 2000)
}

func.call(obj) //window物件
```

這也是運用上面類似的幾種作法，其一，用一個函式內的變數(常數)來傳遞`this`值:

```js
const obj = {a:1}

function func(){
  const that = this

  setTimeout(
    function(){
      console.log(that)
    }, 2000)
}

func.call(obj) ////Object {a: 1}
```

其二，直接用bind方法(因為這裡不適合使用call方法)

```js
const obj = {a:1}

function func(){

  setTimeout(
    function(){
      console.log(this)
    }.bind(this), 2000)
}

func.call(obj)
```

或是寫得更清楚點，把其中的callback函式獨立出來:

```js
const obj = {a:1}

function func(){

  function cb(){
    console.log(this)
  }

  setTimeout(cb.bind(this), 2000)
}

func.call(obj)
```

下面愈寫愈長，其實沒有那麼必要，只是說明也可以用另一個已經bind好的函式，傳入當作新的callback(回調)函式:

```js
function func(){

  function cb(){
    console.log(this)
  }

  const cb2 = cb.bind(this)

  setTimeout(cb2, 2000)
}

func.call(obj)
```


## 參考

http://themihirchronicles.tumblr.com/post/129081364525/scope-vs-context-in-javascript

http://davidshariff.com/blog/what-is-the-execution-context-in-javascript/
http://davidshariff.com/blog/javascript-scope-chain-and-closures/

http://stackoverflow.com/questions/7721200/using-javascript-closures-in-settimeout/7722057#7722057

http://www.digital-web.com/articles/scope_in_javascript/
http://stackoverflow.com/questions/3127429/how-does-the-this-keyword-work
http://unschooled.org/2012/03/understanding-javascript-this/
https://www.sitepoint.com/mastering-javascripts-this-keyword/

https://www.sitepoint.com/inner-workings-javascripts-this-keyword/

https://www.sitepoint.com/mastering-javascripts-this-keyword/