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

這個函式中又有兩個傳入參數值，`resolve`(決定)與`reject`(拒絕)都是要求一定是函式類型。成功的話，也就是有合法值的情況下執行`resolve(value)`，promise物件的狀態會跑到fulfilled(已實現)固定住。失敗或是發生錯誤時用執行`reject(reason)`，reason(理由)通常是用Error物件，然後promise物件的狀態會跑到rejected(已拒絕)狀態固定住。

這種函式稱之為executor(執行者)函式，又稱之為[Revealing Constructor Pattern](https://blog.domenic.me/the-revealing-constructor-pattern/)，executor會在物件實體化時回傳物件實體前立即執行，也就是說當傳入這個函式時，Promise物件會立即決定裡面的狀態，看是要以`resolve`來回傳值，還是要用`reject`來作錯誤處理。也因為它與一般的物件實體化的過程不太一樣，所以常會先包到一個函式中，使用時再呼叫這個函式來產生promise物件，例如像下面這樣的程式碼:

```js
function initPromise(value) {
    return new Promise(function(resolve, reject){
        if(value)
            resolve(value) // fulfilled已實現，成功
        else
            reject(reason) // 錯誤，拒絕
    });
}    
```

Promise建構函式與Promise.prototype物件的設計，主要是要讓設計師作Promisify用的，也就是要把原本異步或同步的程式碼，包裝成為Promise物件來使用。包裝後才能使用Promise的語法結構，也就是我們所說的異步程式的語法結構。

#### then與catch


### all與race



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
