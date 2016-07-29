# 異步程式設計與事件迴圈

> JavaScript程式語言在設計時，需考量異步、單執行緒與非阻塞I/O等等的特性

JavaScript程式執行是單執行緒(Single Thread)的。聽起來有點不可思議，現在的電腦硬體不都是多核心(多執行緒)而且有資源豐富嗎？這個設計對於程式執行不會有問題嗎？

我們可以用其它角度來思考，為何JavaScript這樣設計，仍然可以符合程式執行的需求的幾個原因:

- JavaScript程式執行雖然是單執行緒，但伺服器或瀏覽器執行環境並不是: 表面上看起來是只有一個執行緒在執行JavaScript程式，但實際上在背後有數個的其他在執行環境中的執行緒，在輔助程式碼的執行。
- 外部資料的執行時間，大部份都是等待時間: 像連接資料庫、執行資料庫查詢等等的執行語句，真正執行是在資料庫裡，程式只是傳送對應的查詢語句而已，並不是在JavaScript程式中執行查詢，這種情況對JavaScript程式而言，大部份的執行時間中都是在等待查詢結果而已。讀寫檔案、網路連線要求與回應、傳送資料等等，都有類似的情況。大量而且複雜的運算，的確是在語言的程式中執行，不過JavaScript原本就不是設計用來作複雜運算用的，或許要在特殊的執行環境中才有辦法作這件事。

要理解JavaScript是如何執行程式，首先要先理解同步(Synchronous, sync)與異步(Asynchronous, async)程式設計的差異。

同步程式設計是指程式碼的執行順序，都是由上往下依順序執行，一個執行程序完成後才會再接著下一個，一般的程式語言都是按照這樣的流程來執行，JavaScript語言也不例外。例如像連接資料庫存取資料的程式，應該會遵守下列的步驟進行:

```
1. 連接資料庫(給定帳號、密碼、主機、資料庫名)
2. 執行資料庫查詢語句
3. 取得資料，格式化資料
```

這對於"**從資料庫查詢資料**"的這種程式本身並沒有太特別的地方，一般都是這樣執行沒錯。但對於JavaScript這種只有單執行緒的程式語言，這樣作會造成阻塞(blocking)，也就是說當這個資料庫查詢的執行程序，需要很長的一段時間才能結束時，在這期間其他的操作都會停擺，像是滑鼠要點按按鈕之類的功能，就完全沒有作用。因此，我們需要用另一種不同的程式設計方式，來進行這類程式的執行，也就是異步程式設計方法。

異步程式設計的作法，是使用異步callback(回調)的語法，讓會造成阻塞的程式往一個任務佇列(task queue)先丟著，在之後的某個時間再回傳它的值回來。最常使用的例子是用`setTimeout`這個內建的方法，它會在某個設定的時間的執行其中的callback(回調)函式傳入值一次:

```js
console.log('a')

setTimeout(
    function cb(){
        console.log('b')
    }, 1000)

console.log('c')
```

按照同步程式的執行順序，應該是"a->b->c"這個結果，但真正的結果是"a->c->b"，也就是cb這個在`setTimeout`中的callback(回調)函式，在程式執行時，被移到主執行緒外面的任務佇列去了，最後等時間到了再回來作輸出值的動作。

> 註: 請不要誤解，並不是所有的callback(回調)函式都是會丟到任務佇列(task queue)之中執行。只有經過特殊設計過的異步callbak(回調)才會這樣作。

另一個最常見的例子是AJAX程式，AJAX的全名是"Asynchronous JavaScript and XML"，它在名稱上就有異步的字詞，是運用XMLHttpRequest物件與伺服器溝通的一種技術，我們在特性篇有一篇專文介紹它。一個簡單範例如下:

```js
const xhr = new XMLHttpRequest()

xhr.onreadystatechange = function() {
    if (xhr.readyState == 4 && xhr.status == 200) {
      console.log(xhr.responseText)
    }
}

xhr.open("GET", "test.txt", true)
xhr.send()
```

AJAX技術可以不需要刷新瀏覽器的頁面，它是一種在瀏覽器背後與伺服器溝通的機制，因為是與外部環境作溝通，有可能會因為網路連線或伺服器的狀況造成等待時間，所以一開始就被設計為異步的API，也就是說當AJAX執行時，也會先往一個任務佇列(task queue)丟去，之後某個時間點再回來回傳回應的值。

> 註: 任務佇列(task queue)也有其他名稱的講法，例如消息佇列(message queue)、事件佇列(event queue)、回調佇列(callback queue)

異步程式設計中，會使用不同的機制來解決造成阻塞的問題，JavaScript使用Event Loop(事件迴圈)的設計來協助達成異步程式設計，它是一種並行(Concurrency)的機制，事件迴圈可以想成是一個內部迴圈功能，它會不斷地每一段時間就檢查佇列與執行程序，然後決定是否要把佇列中的任務程序，移回目前JavaScript程式的主要執行緒中(呼叫堆疊)執行，原則其實簡單，"當只有在呼叫堆疊空空如也時，才會把佇列中任務移回呼叫堆疊"。

Event Loop(事件迴圈)直接由名稱上理解，主要是為了事件(Events)所設計的，存放有被進行分派的事件程序到任務佇列中，內部的迴圈功能會不斷的重覆檢查，目前現在瀏覽器上的各種HTML元素是不是有被觸發事件，當有事件被觸發時，就立即把對應的執行程序移到JavaScript的呼叫堆疊中來執行。而當某個事件程序沒有任何元素被分派到，就會從任務佇列中刪除。

那麼，什麼樣的執行程序會被移到任務佇列之中？它的順序又是如何的？

依照[W3C的定義](https://www.w3.org/TR/2014/REC-html5-20141028/webappapis.html#event-loops)，Event Loop(事件迴圈)中可能會有一個以上的任務佇列(task queue)，一般認為就是用以下幾種分出不同的佇列，但實作部份要視瀏覽器實作決定，其中的順序是依照FIFO(先進先出)，以下是幾種會包含的任務:

- Events(事件): EventTarget物件異步分派到對應的Events物件
- Parsing(解析): HTML parser
- Callbacks(回調): 呼叫異步回調函式
- 使用外部資源: 資料庫、檔案、Web I/O
- DOM處理的反應: 回應DOM處理時的元素對應事件

> 註: 對照Call Stack(呼叫堆疊)是LIFO(後進先出)的順序。

在瀏覽器端的JavaScript程式語言中，除了一般的事件分派外，還有少數幾個內建的API與相關物件有類似的異步機制，有一些簡單的樣式可以利用它們模擬出異步的執行程式:

- setTimeout
- setInterval
- XMLHttpRequest
- requestAnimationFrame
- WebSocket
- Worker
- 某些HTML5 API，例如File API、Web Database API
- 有使用onload的技術

而在伺服器端(Node.js)的JavaScript程式，大部份的API都會考量到異步的問題，尤其是與I/O相關的，是半點都不能夠有阻塞的情況發生，這稱為非阻塞I/O(Non-blocking I/O)的設計方式，都有對應的異步呼叫方式。

> 註: 有個說法是說"JavaScript是有非阻塞I/O特性的程式語言"，比較好的理解應該是"JavaScript是個沒辦法阻塞住I/O的程式語言"，畢竟只有一條執行緒，一但塞住了就會很麻煩。因此非阻塞I/O(Non-blocking I/O)才會成為它的一種特性。

不過，對於異步程式也有一些問題要考量，分列如下:

- 異步程式不保証執行的時間順序: 在複雜的異步程式中，可能有很多存在於任務佇列的等待被執行的程序，也可能有一個以上的任務佇列，它們的執行時間與順序的沒有辦法保証。
- Run-to-completion(一執行就要執行到完成): 每個任務程序都會保証只要一執行就會到完成，所以任務程序中都是同步的程序，一個完成才會接著下一個任務。這個特性有可能會導致過長時間的任務，阻塞到Event Loop(事件迴圈)的進行，建議是把任務切割成更小的任務。

因此，異步程式並非只有單純的幾個函式呼叫這麼簡單，有很多情況是需要整個執行流程一併考慮的，例如上面的資料庫查詢的例子，如果後面還有一些要把查詢到的資料進行其他運算的程式碼，就會需要進行執行流程上的分離或合併(例如異步合併同步、同步中的異步、異步中的同步等等)。異步程式設計目前來說也有好幾種作法，例如以下幾種:

- Promise語法結構(ES6)
- Generators (ES6)
- 使用工具函式庫，例如[Async](http://caolan.github.io/async/index.html)
- Async函式(ES7)

## 模擬異步程式設計

使用`setTimeout`方法可以簡單模擬出異步程式，將程序放到事件迴圈的任務佇列之中。




https://github.com/vasanthk/async-javascript

http://latentflip.com/loupe/

https://www.youtube.com/watch?time_continue=1608&v=8aGhZQkoFbQ

https://blog.risingstack.com/node-hero-async-programming-in-node-js/

http://blog.carbonfive.com/2013/10/27/the-javascript-event-loop-explained/

https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop
