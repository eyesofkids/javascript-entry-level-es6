# Promise

Promise是一個異步程式設計的新特性，ES6標準中包含了Promises/A+社區標準的內容。

> future、promise、delay、deferred指的是以建構同步的使用在某些並行(concurrent)程式語言中。它們描述一個物件，扮演一個初始時未知的結果代理，通常是因為它的值運算尚未完成。 ~ 維基百科 [Futures and promises](https://en.wikipedia.org/wiki/Futures_and_promises)

## 異步Callback回調

我需要再次強調，並非所有的使用callbacks(回調)函式的API都是異步的，相反其實大部份的callbacks(回調)函式的結構都是同步的，如果你自己寫的callbacks(回調)結構那更不用說一定是同步的。在JavaScript語言中，除了事件處理9成以上都是異步外，只有少部份的API是異步的，要讓callbacks(回調)的結構轉變為異步，只有以下幾種方式:

- 使用計時器(timer)函式: setTimeout, setInterval
- 特殊的函式: nextTick, setImmediate
- 執行I/O: 監聽網路、資料庫查詢或讀寫外部資源
- 訂閱事件

> 執行I/O的API通常會出現在伺服器端(Node.js)，例如讀寫檔案、資料庫互動等等，這些API都會經過特別的設計。瀏覽器端只有少數幾個。

那麼像那些為尚未支援ES6 Promise瀏覽器打造的polyfill(填充)函式庫，又是怎麼作的？

一方面的確是用上面說的這些特別的方式，尤其是計時器的函式，另一方面還有一些各別瀏覽器中獨自的功能。有個舊的專案[asap](https://github.com/kriskowal/asap)，裡面很多研究出來的作法，都併到知名的Promise函式庫 - [Q](https://github.com/kriskowal/q)之中。

[es6-promise](https://github.com/stefanpenner/es6-promise)專案中也有個自己寫的[asap.js](https://github.com/stefanpenner/es6-promise/blob/master/lib/es6-promise/asap.js)，它是來自另一個異步程式的工具函式庫[RSVP.js](https://github.com/tildeio/rsvp.js)。

## Deferred物件

如果你有用過這近幾年jQuery中的ajax相關方法(1.5版本之後)，其實你就有用過它裡面Deferred(延期)的設計了。Deferred(延期)算是很早就有實作的一種技術，最知名的是由jQuery函式庫在1.5版本(2011年左右)中實作的Deferred物件，用於註冊多個callbacks(回調)進入callbacks(回調)佇列，呼叫callbacks(回調)佇列，以及在成功或失敗狀態轉接任何同步或異步函式。Deferred的設計在jQuery中API豐富而且應用廣泛，尤其是在ajax相關方法中，更是受到很多程式設計師的歡迎，jQuery所設計的Deferred物件中其中有一個`deferred.promise()`可以回傳Promise物件，歷經多次的改版，現在在3.0版本中的Promise物件與現在的ES6 Promise標準，已經是相容的設計，你可以把它視為是ES6的Promise另一種擴充版本。

Deferred(延期)的設計在不同函式庫中略有不同，例如知名的Q函式庫(或Angular中的$q)，它把Promise物件視為Deferred物件的一個屬性，在使用上兩者扮演不同的角色。Deferred(延期)並不在我們要討論的細節，ES6的Promise相對而言，簡單得很多。

## Promises/A+標準

所謂的[Promises/A+](https://promisesaplus.com/)標準，其實就一頁幾千字的網頁而已，看似簡單但實際沒那麼容易。以下是從標準網頁翻譯來的幾個章節。

### 專門用語

- `promise` (承諾)是一個帶有遵照這個規格的then方法的物件或函式
- `thenable` 是一個有定義then方法的物件或函式
- `value` 合法的JavaScript值(包含undefined、thenable與promise)
- `exception` (例外)使用throw語句丟出來的值
- `reason` (理由)表明為什麼promise被拒絕(rejected)的值

> 另外很常見到有個專有名詞 `settled`(固定的)一個promise最後的狀態，也就是fulfilled(已實現)或rejected(已拒絕)

### Promise狀態

promise必定是以下三種狀態中的其中一種: pending(等待中)、fulfilled(已實現)或rejected(已拒絕)。

2.1.1 當處在pending(等待中)時，一個promise:
    2.1.1.1 可能會轉變到不是fulfilled(已實現)就是rejected(已拒絕)狀態
2.2.1 當處在fulfilled(已實現)時，一個promise:
    2.2.1.1 必定不會再轉變到其他任何狀態
    2.2.1.2 必定有不能再更動的值
2.3.1 當處在rejected(已拒絕)，一個promise:
    2.3.1.1 必定不會再轉變到其他任何狀態
    2.3.1.2 必定有不能再更動的值reason(理由)

這個狀態用下面的圖解說明，應該可以很清楚的理解:

![Promise狀態](https://raw.githubusercontent.com/eyesofkids/javascript-entry-level-es6/master/assets/promise_1.png)

狀態是Promise結構很重要的一個特性，因為Promise結構一開始都是代表懸而未決的值，所以一開始在promise物件建立出來後，狀態都是pending(等待中)，之後可以轉變到fulfilled(已實現)就是rejected(已拒絕)其中一個，然後就固定不會再變了。通常有value(值)的情況下是轉變到fulfilled(已實現)狀態，而如果是有reason(理由)時，代表要轉變到rejected(已拒絕)狀態。

### Promise物件的建立與使用

#### Promise物件的建立

一個簡單的Promise語法結構如下:

```js
const promise = new Promise(function(resolve, reject) {
  // 成功時
  resolve(value)
  // 失敗時
  reject(reason)
});

promise.then(function(value) {
  // on fulfillment(已實現時)
}, function(reason) {
  // on rejection(已拒絕時)
})
```

首先先看Promise的建構函式，它的語法如下(來自[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)):

```
new Promise( function(resolve, reject) { ... } )
```

用箭頭函式可以簡化一下:

```
new Promise( (resolve, reject) => { ... } )
```

建構函式的傳入參數值需要一個函式。

這個函式中又有兩個傳入參數值，`resolve`(解決)與`reject`(拒絕)都是要求一定是函式類型。成功的話，也就是有合法值的情況下執行`resolve(value)`，promise物件的狀態會跑到fulfilled(已實現)固定住。失敗或是發生錯誤時用執行`reject(reason)`，reason(理由)通常是用Error物件，然後promise物件的狀態會跑到rejected(已拒絕)狀態固定住。

這種函式稱之為executor(執行者)函式，又稱之為[Revealing Constructor Pattern](https://blog.domenic.me/the-revealing-constructor-pattern/)，executor會在物件實體化時回傳物件實體前立即執行，也就是說當傳入這個函式時，Promise物件會立即決定裡面的狀態，看是要以`resolve`來回傳值，還是要用`reject`來作錯誤處理。也因為它與一般的物件實體化的過程不太一樣，所以常會先包到一個函式中，使用時再呼叫這個函式來產生promise物件，例如像下面這樣的程式碼:

```js
function firstPromise(value) {
    return new Promise(function(resolve, reject){
        if(value)
            resolve(value) // fulfilled已實現，成功
        else
            reject(reason) // 有錯誤，已拒絕，失敗
    });
}    
```

Promise建構函式與Promise.prototype物件的設計，主要是要讓設計師作Promisify用的，也就是要把原本異步或同步的程式碼，包裝成為Promise物件來使用。包裝後才能使用Promise的語法結構，也就是我們所說的異步程式的語法結構。

> 註: 有一些術語或形容詞的意思大概都是相同的，fulfilled(已實現)、resolve(解決)、successful(成功的)、completed(完成的)差不多意思。而rejected(已拒絕)、fail(失敗)、error(錯誤)是另一個意思。

#### then與catch

在Promise的標準中，一直不斷的提到一個方法 - `then`，中文是"然後、接著、接下來"的意思，這個是一個Promise的重要方法。有定義then方法的物件被稱之為`thenable`物件，標準中花了一個章節在講`then`方法的規格，它的語法如下(出自[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)):

```js
p.then(onFulfilled, onRejected);

p.then(function(value) {
   // fulfillment
  }, function(reason) {
  // rejection
});
```

`then`方法一樣有用兩個函式當作傳入參數，`onFulfilled`是當promise物件的狀態轉變為fulfilled(已實現)呼叫的函式，有一個傳入參數值可用，就是value(值)。而`onRejected`是當promise物件的狀態轉變為rejected(已拒絕)呼叫的函式，也有一個傳入參數值可以用，就是reason(理由)。

為什麼說它是"一樣"？因為比對到promise物件的建構式，那個傳入的函式參數值的樣子非常像，也是兩個函式當作傳入參數，只是名稱的定義上有點不同，但接近同意義。

那麼`then`方法最後的回傳值是什麼？是另一個promise物件。

> Promises/A+標準 2.2.7
> `then`必須回傳一個promise。`promise2 = promise1.then(onFulfilled, onRejected);`

為什麼要這樣設計，主要是要作連鎖(chained)的結構，也就是稱之為合成(composition)的一種運算，在JavaScript中如果回傳值都是相同的函式，可以使用連鎖的語法。像下面這樣的程式碼:

```js
const promise = new Promise(function(resolve, reject) {
  resolve(1)
})

promise.then(function(value) {
  console.log(value) // 1
  return value + 1
}).then(function(value) {
  console.log(value) // 2
  return value + 2
}).then(function(value) {
  console.log(value) // 4
})
```

`then`中的onFulfilled函式，也就是第一個函式傳入參數，它是為了有值時使用的函式，經過連鎖的結構，如果要把值往下傳遞，可以用回傳值的方式，上面的例子可以看到用`return`語句來回傳值，這個值可以不斷的往下面的`then`方法傳送。

onRejected函式，也就是第二個函式的傳入參數，也有用回傳值往下傳遞的特性，不過因為它是使用於錯誤的處理，除非你是有要用來修正錯誤之類的理由，不然說實在這樣作會有點混亂，因為不論是onFulfilled函式或onRejected函式的傳遞值，都只會記錄到新產生的那個Promise物件，而且沒有區分。

```js
const p1 = new Promise((resolve, reject) => {
    //throw new Error('rejected!')
    reject(new Error('rejected!')) // 與使用reject同樣效果

    //resolve(4)
})

p1.then((val) => console.log(val))
    .catch((err) => {
        console.log(err.message)
        return 100
    })
    .then((val) => console.log(val, 'done'))
```

`then`方法中的兩個函式傳入參數，第1個是在promise物件有值的情況下才會執行，也就是進入到fulfilled(已實現)狀態。第2個則是在promise物件發生錯誤、失敗，才會執行。這兩個函式都可以寫出來，但為了方便進行多個不同程式碼的連鎖，通常在只使用`then`方法時，都只寫第1個函式傳入參數。

錯誤處理則交給另一個`catch`方法來作，`catch`只需要一個函式傳入參數，(出自[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)):

```js
p.catch(onRejected);

p.catch(function(reason) {
   // rejection
});
```

`catch`方法相當於`then(undefined, onRejected)`，也就是`then`方法的第一個函式傳入參數沒有給定值的情況，它算是個為方便使用的`then`方法語法糖。`catch`方法正如其名，它就是要取代同步`try...catch`語句用的異步例外處理方式。

> 註: 不過也因為`catch`方法與`try...catch`中的`catch`同名，造成IE8以下的瀏覽器產生衝突與錯誤。有些函式庫會用`caught`這個名稱來取代它，或是乾脆用`then`方法就好。

## 錯誤處理

在promise建構函式中，直接使用`throw`語句相當於用`reject`方法，一個簡單的範例如下:

```js
// 用throw語句取代reject
const p1 = new Promise((resolve, reject) => {
    throw new Error('rejected!') // 用throw語句
    //相當於用以下的語句
    //reject(new Error('rejected!'))
})

p1.then((val) => {
        return val + 2
        console.log(val + 2)
    }).then((val) => console.log(val + 2))
    .catch((err) => console.log('error:', err.message))
    .then((val) => console.log('done'))

//最後結果:
//error: rejected!
//done
```

原先在錯誤處理章節中，對於`throw`語句的說明是，`throw`語句執行後會直接進到最近的`catch`方法的區塊中，執行區塊中程式碼後中斷之後程式碼。但這個理解是針對一般的同步程式結構，對異步的Promise結構並不適用。所以**並不會**中斷Promise結構的繼續執行，上面最後有個`then`中會輸出'done'字串，它依然會輸出。

如果你有看過其他的Promise教學，用`then`方法與`catch`來組成一個有錯誤處理的流程，例如像以下的範例程式，來自[JavaScript Promises](http://www.html5rocks.com/en/tutorials/es6/promises/):

```js
asyncThing1().then(function() {
  return asyncThing2();
}).then(function() {
  return asyncThing3();
}).catch(function(err) {
  return asyncRecovery1();
}).then(function() {
  return asyncThing4();
}, function(err) {
  return asyncRecovery2();
}).catch(function(err) {
  console.log("Don't worry about it");
}).then(function() {
  console.log("All done!");
});
```

然後畫了像在這篇文章中[JavaScript Promises](http://www.html5rocks.com/en/tutorials/es6/promises/)的流程圖。

我要建議的是不要單純用只有onFulfilled函式的`then`方法，以及`catch`方法來看整體流程，其實有點混亂。從完整的`then`方法可以把流程看得更清楚，每個`then`方法(或`catch`方法)都會回傳一個完整的Promise物件，當新的Promise往下個then傳遞時，因原來其中程式碼執行的不同，狀態也不同。


> Promises/A+標準 2.2.7
>
> `then`必須回傳一個promise。
> `promise2 = promise1.then(onFulfilled, onRejected);`
>
> 2.2.7.1 當不論是onFulfilled或onRejected其中有一個是有回傳值`x`，執行Promise解析程序`[[Resolve]](promise2, x)`
>
> 2.2.7.2 當不論是onFulfilled或onRejected其一丟出例外`e`，promise2必須用`e`作為理由而拒絕(rejected)
>
> 2.2.7.3 當onFulfilled不是一個函式，而且promise1是fulfilled(已實現)時，promise2必須使用與promise1同樣的值被fulfilled(實現)
>
> 2.2.7.4 當onRejected不是一個函式，而且promise1是rejected(已拒絕)時，promise2必須使用與promise1同樣的理由被rejected(拒絕)

標準的2.2.7.4正好說明了，當你單純用只有onFulfilled函式的`then`方法時，如果發生在上面的Promise物件rejected(已拒絕)情況，為何會一直往下發生rejected(已拒絕)的連鎖反應，因為這個時候onRejected函式沒有傳入值，相當於undefined，也就是不算個函式，滿足了這個規則。會一直連鎖反應到某個`catch`方法(或是有onRejected函式的then方法)執行過後，才會又恢復使用2.2.7.1的正常規則。

標準的2.2.7.3恰好相反，它是在一堆只有`catch`方法的連鎖結構出現，`catch`方法代表沒有onFulfilled函式的`then`方法，只要最上面的Promise物件fulfilled(已實現)時，會一直連鎖反應到某個`then`方法，才會又恢復使用2.2.7.1的正常規則。當然，這種情況很少見，因為在整個結構中，我們使用一連串的`catch`方法是不太可能的，頂多只會使用一兩個而已。

我把上面的範例用箭頭函式簡化過，像下面這樣的範例:

```js
async1()
    .then(() => async2())
    .then(() => async3())
    .catch((err) => errorHandler1())
    .then(() => async4(), (err) => errorHandler2())
    .catch((err) => console.log('Don\'t worry about it'))
    .then(() => console.log('All done!'))
```

然後再把`then`方法中的兩個函式傳入參數都補齊，`catch`方法也改用`then`方法來改寫。這樣作只是要方便解說這個規則影響流程是怎麼跑的，看得更清楚而已。實際使用你還是可以用上面的範例，當然前提是你已經很了解這些流程的規則。

```js
async1()
    .then(() => async2(), undefined)       
    .then(() => async3(), undefined)
    .then(undefined, (err) => errorHandler1())
    .then(() => async4(), (err) => errorHandler2())
    .then(undefined, (err) => console.log('Don\'t worry about it'))
    .then(() => console.log('All done!'), undefined)
```

情況: 當async1回傳的promise物件的狀態是rejected時。

1. 2.2.7.4規則，async2不會被執行，新的Promise物件直接是rejected狀態
2. 2.2.7.4規則，async3不會被執行，新的Promise物件直接是rejected狀態
3. 2.2.7.1規則，errorHandler1()被執行，新的Promise物件為onFulfilled狀態
4. 2.2.7.1規則，async4()被執行，新的Promise物件為onFulfilled狀態
5. 2.2.7.3規則，跳過輸出字串，新的Promise物件為onFulfilled狀態，回傳值繼續傳遞
6. 2.2.7.1規則，輸出字串


### then中onFulfilled回傳值

then中onFulfilled回傳值，或是Promise建構函式中的resolve中的參數值，這是已經進入正常的"執行Promise解析程序"中。相較於onRejected函式我們只會關心它的reason(理由)，通常是個Error物件，用於錯誤處理中，所以就單純多了。

then中onFulfilled回傳值可以有三種不同類型:

- 值
- promise物件
- thenable物件

值就一般在JavaScript的各種值，這沒什麼好講的，因為then方法最後會回傳Promise物件，所以這個回傳值會跟著這個新產生的回傳Promise物件到下一個連鎖方法去，這個回傳值可以用下個then方法的onFulfilled函式取到第1個傳入參數值中。

promise物件就你不希望`then`幫你回傳promise物件，自己建立一個，這也合情合理。通常是使用promis建構式new一個，或是直接用`Promise.resolve(value)`靜態方法，產生一個fulfilled(已實現)狀態的promise物件。

thenable物件是最特殊的，根據標準的定義為:

> `thenable` 是一個有定義then方法的物件或函式

實際上按照標準上的解說，thenable是提供給沒有符合標準的其他實作函式庫或框架，利用合理的`then`方法來進行同化的的方式。thenable物件是個單純物件，然後裡面有個then方法的定義而已，例如以下的範例:

```js
const thenable1 = {
    then: function(onFulfill, onReject) {
        onFulfill('fulfilled!')
    }
}

const thenable2 = {
    then: function(resolve) {
        throw new TypeError('Throwing')
        resolve('Resolving')
    }
}

const thenable3 = {
    then: function(resolve) {
        resolve('Resolving')
        throw new TypeError('Throwing')
    }
}
```

### 靜態方法 Promise.resolve與Promise.reject


### 靜態方法 Promise.all與Promise.race



### 執行順序

http://stackoverflow.com/questions/29111626/javascript-promise-then-ordering
http://stackoverflow.com/questions/36526639/es6-promise-execution-order
http://stackoverflow.com/questions/29853578/understanding-javascript-promises-stacks-and-chaining/29854205#29854205

### 實踐

---

** https://gist.github.com/domenic/3889970



** http://www.datchley.name/es6-promises/



** http://exploringjs.com/es6/ch_promises.html

http://www.datchley.name/promise-patterns-anti-patterns/

http://stackoverflow.com/questions/37651780/why-does-the-promise-constructor-need-an-executor

http://stackoverflow.com/questions/28687566/difference-between-defer-promise-and-promise/28692824#28692824

http://www.html5rocks.com/en/tutorials/es6/promises/

http://www.2ality.com/2014/10/es6-promises-api.html

https://blog.domenic.me/the-revealing-constructor-pattern/

https://davidwalsh.name/promises

https://pouchdb.com/2015/05/18/we-have-a-problem-with-promises.html

http://de.slideshare.net/domenicdenicola/callbacks-promises-and-coroutines-oh-my-the-evolution-of-asynchronicity-in-javascript

http://liubin.org/promises-book/

http://www.mattgreer.org/articles/promises-in-wicked-detail/

https://zeit.co/blog/async-and-await

http://solutionoptimist.com/2013/12/27/javascript-promise-chains-2/
