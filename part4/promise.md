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

- `promise` 是一個帶有遵照這個規格的then方法的物件或函式
- `thenable` 是一個有定義then方法的物件或函式
- `value` 合法的JavaScript值(包含undefined、thenable與promise)
- `exception` 使用throw語句丟出來的值
- `reason` 表明為什麼promise被拒絕(rejected)的值

> 另外很常見到有個專有名詞 `settled` 一個promise最後的狀態，也就是fulfilled(已實現)或rejected(已拒絕)

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

一開始promise物件建立出來後，狀態都是pending(等待中)，之後可以轉變到fulfilled(已實現)就是rejected(已拒絕)其中一個，然後就固定不會再變了。


### executor執行者

---
http://www.datchley.name/es6-promises/

http://exploringjs.com/es6/ch_promises.html

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
