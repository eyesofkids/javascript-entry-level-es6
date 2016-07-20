# Event loop

JavaScript程式執行是單執行緒(Single Thread)

首先要先理解同步(Synchronous, sync)與異步(Asynchronous, async)程式的差異。

同步程式是指程式碼的執行順序，都是由上往下依順序執行，一個執行完成才會再接著下一個，例如像連接資料庫存取資料的程式，應該會遵守下列的步驟進行:

```
1. 連接資料庫(給定帳號、密碼、主機、資料庫名)
2. 執行資料庫查詢語句
3. 取得資料，格式化資料
```

這對於"從資料庫查詢資料"的這種程式碼本身並沒有什麼特別，一般都是這樣作沒錯，但對於JavaScript這種只能使用單執行緒的程式語言，這樣作會造成阻塞(blocking)，如果這個資料庫查詢的執行，需要很長的一段時間，在這期間其他的操作都會停擺。所以我們需要用另一種不同的設計方式，來進行程式的執行，也就是異步程式的設計方法。

異步程式的其中一種作法，是使用異步callback(回調)的語法，讓會造成阻塞的程式往一個任務佇列(task queue)先丟著，在之後的某個時間再回傳它的值回來。最常使用的例子是用setTimeout這個內建的方法，它會在某個設定的時間的執行其中的callback(回調)函式傳入值一次:

```js
console.log('a')

setTimeout(
    function cb(){
        console.log('b')
    }, 1000)

console.log('c')
```

按照同步程式的執行順序，應該是"a->b->c"這個結果，但真正的結果是"a->c->b"，也就是cb這個在`setTimeout`中的callback(回調)函式，在程式執行時，被移到另一個平行時空(任務佇列)去了，最後等時間到了再回來作輸出值的動作。

異步程式用不同的設計方式來解決造成阻塞的問題，JavaScript中使用Event Loop(事件迴圈)的設計來協助達成異步程式，它是一種並行(Concurrency)模型，事件迴圈可以想成是一個瀏覽器中的內部迴圈機制，它會一段時間就進行檢查佇列與執行程序，然後決定是否要把佇列中的任務程序，移回目前JavaScript程式的主要執行緒中(呼叫堆疊)執行。

事件迴圈直接由名稱上理解，主要是為了事件(Events)所設計的，進行分派的事件，它有一個內部的迴圈機制，不斷的在檢查現在瀏覽器上的各種HTML元素是不是被觸發事件，當有事件被觸發時，就立即把對應的執行程序移到JavaScript的呼叫堆疊中來執行。

那麼，什麼樣的執行程序會被移到任務佇列之中？它的順序又是如何的？

依照[W3C的定義](https://www.w3.org/TR/2014/REC-html5-20141028/webappapis.html#event-loops)，Event Loop中可能會有一個以上的任務佇列，其中的順序是依照FIFO(先進先出)，以下是幾種會包含的任務:

- Events(事件): 異步分派於EventTarget物件對應Events物件
- Parsing(解析): 指的是HTML parser
- Callbacks(回調): 呼叫異步回調函式
- 使用外部資源
- DOM處理的反應: 回應DOM處理時的元素對應事件

在瀏覽器端的JavaScript程式語言中，除了一般的事件分派外，還有少數幾個內建的API與相關物件有類似的異步機制，有一些簡單的作法可以利用它們模擬出異步的執行程式:

- setTimeout
- setInterval
- XMLHttpRequest
- requestAnimationFrame
- WebSocket
- Worker
- 一些HTML5 API，例如File API、Web Database API
- onload

而在伺服器端(Node.js)的JavaScript程式，大部份的API都會考量到阻塞的問題，尤其是與I/O相關的，都有對應的異步呼叫方式。

我們可以再深入思考，為何JavaScript這樣設計，仍然可以符合程式執行的需求的幾個原因:

- JavaScript程式執行雖然是單執行緒，但伺服器或瀏覽器執行環境並不是: 表面上看起來是只有一個執行緒在執行JavaScript程式，但實際上在背後有數個的其他在執行環境中的執行緒，在輔助程式碼的執行，不過這些都是引擎內部的實作部份。

- 外部資料的執行時間，大部份都是等待時間: 像連接資料庫、執行資料庫查詢等等的執行語句，真正執行是在資料庫裡，程式只是傳送對應的查詢語句而已，並不是在JavaScript程式在執行查詢資料，這種情況對JavaScript程式而言，大部份的執行時間中都是在等待查詢結果而已。讀寫檔案、網路連線要求與回應、傳送資料等等，都有類似的情況。大量而且複雜的運算，的確是在語言的程式中執行，不過JavaScript原本就不是設計用來作複雜運算用的，或許要在特殊的執行環境中才有辦法作這件事。

不過，對於異步程式也有一些問題要考量，分列如下:

- 異步程式不保証執行的時間順序: 在複雜的異步程式中，可能有很多存在於任務佇列的等待被執行的程式，也可能有一個以上的任務佇列，它們的執行時間與順序的沒有辦法保証。
- Run-to-completion(一執行就要執行到完成): 每個任務都會保証只要一執行就會到完成，才會接著下一個任務。這個特性有可能會導致過長時間的任務，阻塞住Event Loop(事件迴圈)的進行，建議是把任務切割成更小的任務。 

因此，異步程式並非只有單純的幾個函式呼叫這麼簡單，有很多情況是需要整個執行流程一併考慮的，例如上面的資料庫查詢的例子，如果後面還有一些要把查詢到的資料進行其他運算的程式碼，就會需要進行執行流程上的合併(異步合併同步等等)，使用外部的工具函式庫可以協助你作這件事，例如[Async](http://caolan.github.io/async/index.html)

JavaScript的這種並行(Concurrency)模型，使用的是稱為事件迴圈(Event Loop)的設計。


http://latentflip.com/loupe/

https://www.youtube.com/watch?time_continue=1608&v=8aGhZQkoFbQ

https://blog.risingstack.com/node-hero-async-programming-in-node-js/

http://blog.carbonfive.com/2013/10/27/the-javascript-event-loop-explained/

https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop