# Promise

Promise是一個異步程式設計的新特性，ES6標準中包含了Promises/A+社區標準的內容。

## 異步Callback回調

我需要再次強調，並非所有的callbacks(回調)函式的結構，就是異步的，反之其實大部份的callbacks(回調)函式的結構都是同步的。在JavaScript語言中，除了事件處理9成以上都是異步外，只有少部份的API是異步的，要讓函式的結構轉變為異步，只有以下幾種方式:

- 使用計時器(timer)函式: setTimeout, setInterval
- 特別的函式: nextTick, setImmediate
- 執行I/O: 監聽網路、資料庫查詢或讀寫資源
- 訂閱一個事件

那麼像這些為尚未支援ES6 Promise瀏覽器打造的polyfill(填充)函式庫，又是怎麼作的？

一方面的確是用上面說的這些特別的方式，另一方面還有一些各別瀏覽器中獨自的功能。有個舊的專案[asap](https://github.com/kriskowal/asap)，裡面很多研究出來的作法，都併到知名的Promise函式庫 - [Q](https://github.com/kriskowal/q)之中。

[es6-promise](https://github.com/stefanpenner/es6-promise)專案中也有個自己寫的[asap.js](https://github.com/stefanpenner/es6-promise/blob/master/lib/es6-promise/asap.js)，它是來自另一個異步程式的工具函式庫[RSVP.js](https://github.com/tildeio/rsvp.js)。

## Deferred物件與Promise建構式



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
