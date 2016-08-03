# Promise

> 一個promise代表一個異步操作的最終結果 ~譯自[Promises/A+](https://promisesaplus.com/)

Promise語法結構提供了更多的程式設計的可能性，它是一個經過長時間實戰的結構，在許多知名的函式庫或框架中都可以見到Promise物件的身影，例如Dojo、jQuery、YUI、 Ember、Angular、WinJS、Q等等，Promises/A+社區則提供了可供遵守的統一標準，在新的ES6標準中包含了Promise的實作，提供原生的語言內建支援，它將是個基礎的開始，往後會有愈來愈多架構在其上的Web API。

## 開始之前

### 異步Callback(回調)

需要再次強調，並非所有的使用callbacks(回調)函式的API都是異步的，相反其實大部份的callbacks(回調)函式的結構都是同步的，如果你自己寫的callbacks(回調)結構那更不用說一定是同步的。在JavaScript語言中，除了事件處理9成以上都是異步外，只有少部份的API是異步的，要讓callbacks(回調)的結構轉變為異步，只有以下幾種方式:

- 使用計時器(timer)函式: `setTimeout`, `setInterval`
- 特殊的函式: `nextTick`, `setImmediate`
- 執行I/O: 監聽網路、資料庫查詢或讀寫外部資源
- 訂閱事件

> 執行I/O的API通常會出現在伺服器端(Node.js)，例如讀寫檔案、資料庫互動等等，這些API都會經過特別的設計。瀏覽器端只有少數幾個。

那麼像那些為尚未支援ES6 Promise瀏覽器打造的polyfill(填充)函式庫，又是怎麼作的？

一方面的確是用上面說的這些特別的方式，尤其是計時器的函式，或是一些各別新式瀏覽器中獨自的功能。有個舊的專案[asap](https://github.com/kriskowal/asap)，裡面很多研究出來的作法，都併到知名的Promise函式庫 - [Q](https://github.com/kriskowal/q)之中。

[es6-promise](https://github.com/stefanpenner/es6-promise)專案中也有個自己的[asap.js](https://github.com/stefanpenner/es6-promise/blob/master/lib/es6-promise/asap.js)，它是來自另一個異步程式的工具函式庫[RSVP.js](https://github.com/tildeio/rsvp.js)。

### Deferred物件

如果你有用過近幾年在jQuery中的ajax相關方法(1.5版本之後)，其實你就有用過它裡面Deferred(延期)的設計了。Deferred(延期)算是很早就被實作的一種技術，最知名的是由jQuery函式庫在1.5版本(2011年左右)中實作的Deferred物件，用於註冊多個callbacks(回調)進入callbacks(回調)佇列，呼叫callbacks(回調)佇列，以及在成功或失敗狀態轉接任何同步或異步函式，這個目的也就是Deferred物件或Promise物件被實作出來的原因。

Deferred的設計在jQuery中API豐富而且應用廣泛，尤其是在ajax相關方法中，更是受到很多程式設計師的歡迎，jQuery所設計的Deferred物件中其中有一個`deferred.promise()`可以回傳Promise物件，歷經多次的改版，現在在3.0版本中的jQuery.Deferred物件與現在的Promises/A+與ES6標準，已經是相容的設計，你可以把它視為是Promise的擴充版本。

Deferred(延期)的設計在不同函式庫中略有不同，例如知名的Q函式庫(或Angular中的$q)，它把Promise物件視為Deferred物件的一個屬性，在使用上兩者扮演不同的角色。Deferred(延期)並不在我們要討論的細節。相對來說ES6中的Promise物件功能較少。

如果你有需要使用外部函式庫或框架中關於Promise的設計，以及使用它們裡面豐富的方法與樣式，不妨先花點時間了解一下ES6中的Promise特性，畢竟這是內建的語言特性，對於一般小型的程式設計也許已經很足夠。

### Promise與異步程式設計

同步程序你應該很熟悉了，大部份你寫的程式碼都是同步的程序。一步一步(一行一行)接著執行。異步程序有一些你可能用過，`setTimeout`、`XMLHttpRequest`(AJAX)之類的API，或是DOM事件的處理，在設計上就是異步的。

我們關心的是以函式(方法)的角度來看異步或同步，函式相當於包裹著要共同來作某件事的程序，雖然函式內的這些程序有可能是同步的也有可能是異步的，我們把當它們當成一大包的程序，JavaScript中的設計也是以函式為執行上下文(EC)的單位。

> 同步函式的結果要不就是回傳一個值，要不然就是執行到一半發生例外，中斷目前的程式然後拋出例外。

異步的函式結果又會是什麼？要不然就最後回傳一個值，要不然就執行到一半發生例外，但是異步的函式發生錯誤時怎麼辦，可以馬上中斷程式然後拋出例外嗎？不行。那該怎麼作？只能用別的方式來處理。也就是說異步的函式，除了與同步函式執行方式不同，同步函式會直接在呼叫堆疊中執行，而異步函式會先送到工作佇列(queue)中，然後用事件迴圈再回到呼叫堆疊執行。而且它們對於錯誤的處理方式也要用不同的方式。

> 同步函式的結果要不就是帶有回傳值的成功，要不就是帶有回傳理由的失敗。

以一個簡單的比喻來說，你開了一間冰店，可能有些原料是自己作的，但也有很多配料或食材，是由別人生產的。同步函式就像你自己作的配料，例如自己製作大冰塊、煮紅豆湯之類的，每個步驟都是你自己監管品質，中間如果發生問題(例外)，例如作大冰塊的冰箱壞了，你也可以第一時間知道，而且需要你自己處理，但作大冰塊這件事就會停擺，影響到後面的工作。異步函式是另一種作法，有些配料是向別的工廠叫貨，例如煉乳或黑糖漿，你可以先打電話請工廠進行生產，等差不多時間到了，這些工廠就會把貨送過來。當工廠發生問題時，你可能只是接獲工廠通知，有可能是要延期交貨或是改向別的工廠叫貨。

promise物件的設計就是針對異步函式的，promise物件最後的結果要不然就用一個回傳值來fulfilled(實現)，要不然就用一個理由(錯誤)來rejected(拒絕)。

你可能會認為這種用失敗(或拒絕)或成功的兩分法結果，似乎有點太武斷了，但在許多異步的結構中，的確是用成功或失敗來作為代表，例如AJAX的語法結構。promise物件用實現(解決)與拒絕來作為兩分法的分別字詞。對於有回傳值的情況，沒有什麼太多的考慮空間，必定都是實現狀態，但對於何時才算是拒絕的狀態，這有可能需要仔細考量，例如以下的情況:

好的拒絕狀態應該是:

- I/O操作時發生錯誤，例如讀寫檔案或是網路上的資料時，中途發生例外情況
- 無法完成預期的工作，例如`accessUsersContacts`函式是要讀取手機上的聯絡人名單，因為權限不足而失敗
- 內部錯誤導致無法進行異步的程序，例如環境的問題或是程式開發者傳送錯誤的傳入值

壞的拒絕狀態例如:

- 沒有找到值或是輸出是空白的情況，例如對資料庫查詢，目前沒有找到結果，回傳值是0。它不應該是個拒絕狀態，而是帶有0值的實現。
- 詢問類的函式，例如`hasPermissionToAccessUsersContacts`函式詢問是否有讀取手機上聯絡人名單的權限，當回傳的結果是false，也就是沒有權限時，應該是一個帶有false值的實現。

不同的想法會導致不同的設計，舉一個明確的實例來說明拒絕狀態的情境設計。jQuery的`ajax()`方法，它會呼叫fail處理函式的條件，除了網路連線的問題外，在雖然有回應但是是屬於失敗類的HTTP狀態碼時，也會一併包含進來。但另一個用於類似功能的`fetch`方法並沒有，`fetch`使用Promise架構，只有在網路連線發生問題才會轉為rejected(拒絕)狀態。

> 註: 在JavaScript中函式的設計，必定有回傳值，沒寫只是回傳undefined，相當於`return undefined`

## Promises/A+標準

所謂的[Promises/A+](https://promisesaplus.com/)標準，其實就只一頁幾千字的網頁而已，裡面的用語並不會太難懂。ES6標準中也有自己的Promise物件章節，但內容明顯用詞艱澀許多。以下使用Promises/A+標準作為一個開始，來解說Promise的內容有什麼。

### 專門用語

- `promise` (承諾)是一個帶有遵照這個規格的then方法的物件
- `thenable` 是一個有定義then方法的物件
- `value` 合法的JavaScript值(包含undefined、thenable與promise)
- `exception` (例外)使用throw語句丟出來的值
- `reason` (理由)表明為什麼promise被拒絕(rejected)的值

> 註: 另外有個常見的專有名詞 `settled`(固定的) 一個promise最後的狀態，也就是fulfilled(已實現)或rejected(已拒絕)

> 註: `reason`(理由)通常是一個Error物件，用於錯誤處理。

### Promise狀態

promise物件必定是以下三種狀態中的其中一種: pending(等待中)、fulfilled(已實現)或rejected(已拒絕)。

2.1.1 當處在pending(等待中)時，一個promise:

    2.1.1.1 可能會轉變到不是fulfilled(已實現)就是rejected(已拒絕)狀態

2.2.1 當處在fulfilled(已實現)時，一個promise:

    2.2.1.1 必定不會再轉變到其他任何狀態
    2.2.1.2 必定有不能再更動的值

2.3.1 當處在rejected(已拒絕)，一個promise:

    2.3.1.1 必定不會再轉變到其他任何狀態
    2.3.1.2 必定有不能再更動的值reason(理由)

這個用下面的圖解說明，應該可以很清楚的理解:

![Promise狀態](https://raw.githubusercontent.com/eyesofkids/javascript-entry-level-es6/master/assets/promise_1.png)

狀態是在Promise結構很重要的一個屬性，因為promise物件一開始都是代表懸而未決的值，所以一開始在promise物件在建立時，狀態都是pending(等待中)，之後可以轉變到fulfilled(已實現)就是rejected(已拒絕)其中一個，然後就固定不變了。通常有value(值)的情況是轉變到fulfilled(已實現)狀態，而如果是有reason(理由)時，代表要轉變到rejected(已拒絕)狀態。

不過，講是這樣講，實際上在實作時，promise物件一旦實體化完成回傳出來，就已經決定好是那一種狀態了，要不就是fulfilled(已實現)，要不就是rejected(已拒絕)。只是在實體化過程中，這個還不存在的(還沒回傳出來的)的promise物件的確是pending(等待中)狀態。下面實作時就看得到這個結果。

## Promise物件建立與基本使用

### Promise物件的建立

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

這個傳入函式稱為executor(執行者)函式，這樣式一種名稱為[暴露的建構式樣式 Revealing Constructor Pattern](https://blog.domenic.me/the-revealing-constructor-pattern/)，executor會在建構式回傳物件實體前立即執行，也就是說當傳入這個函式時，Promise物件會立即決定裡面的狀態，看是要執行`resolve`來回傳值，還是要用`reject`來作錯誤處理。也因為它與一般的物件實體化的過程不太一樣，所以常會先包到一個函式中，使用時再呼叫這個函式來產生promise物件，例如像下面這樣的程式碼:

```js
function asyncFunction(value) {
    return new Promise(function(resolve, reject){
        if(value)
            resolve(value) // 已實現，成功
        else
            reject(reason) // 有錯誤，已拒絕，失敗
    });
}    
```

Promise建構函式與Promise.prototype物件的設計，主要是要讓設計師作Promisify用的，也就是要把原本異步或同步的程式碼或函式，打包成為Promise物件來使用。包裝後才能使用Promise的語法結構，也就是我們所說的異步程式的語法結構。

你可能會認為在`new Promise(function(resolve, reject){...})`中的`resolve`與`reject`兩個傳入參數值的名稱，就一定要是這樣。而且學到最後面，最會讓人搞混的是Promise物件中也有兩個靜態方法(說明在下面章節)，名稱也恰好是`Promise.resolve`與`Promise.reject`。答案並不是一定要這樣命名，它只是對應之後執行時，習慣上這樣命名而已。你可以執行下面的範例，看能不能執行:

```js
const promise = new Promise(function(resolveParam, rejectParam) {
    //resolveParam(1)
    rejectParam(new Error('error!'))
})

promise.then((value) => {
    console.log(value) // 1
    return value + 1
}).then((value) => {
    console.log(value) // 2
    return value + 2
}).catch((err) => console.log(err.message))
```

為什麼可以換傳入參數值的名稱？要回答這個問題，要先來解說一下進入Promise建構函式的大概執行流程，當然這是很簡化的說明而已:

1. 先用一個內部的物件，我把它稱為雛形物件，然後狀態(state)設定為pending(等待中)，值(value)為undefined，理由(reason)為undefined
2. 再來初始化這個物件的工作，用`init(promise, resolver)`函式，傳入建構式中的傳入參數，也就是`function(resolve, reject){...}`當作`resolver`(解決者，解決用函式)傳入參數。
3. 把真正實作的Promise中的兩個內部resolve函式，與reject函式，對映到`init(promise, resolver)`中執行。

下面是把步驟說明實作出來的範例，不過這個只是為了方便解說用的，並不是真正可用的Promise建構函式，參考自[es6-promise](https://github.com/stefanpenner/es6-promise):

```js
//內部用的雛形物件，實作上包含在建構式中用this
const pInternal = {
  state: 'pending',
  value: undefined,
  reason: undefined
}

//這個就是稱為executor的傳入參數
function resolver(resolve, reject){
  resolve(10)
  //reject(new Error('error occured !'))
}

//初始化內部雛形物件用的函式
function init(promise, resolver){
    resolver(function resolvePromise(value){
      resolve(promise, value);
    }, function rejectPromise(reason) {
      reject(promise, reason);
    });

    return promise
}

function resolve(promise, value){
  console.log(value)
  promise.state = 'onFulfilled'
  promise.value = value
}

function reject(promise, reason){
  console.log(reason)
  promise.state = 'onRejected'
  promise.reason = reason
}

//最後生成回傳的promis物件
const promise = init(pInternal, resolver)
console.log(promise)
```

以上面的範例來說，`resolver`函式在`init`中被呼叫時，`resolver`的第1個傳入參數，它的執行程式碼內容會被`init`中的`resolver`呼叫時的第一個傳入參數`resolvePromise`所取代，然後再加上`init`函式傳入參數`promise`物件，最後呼叫執行`resolve(promise, value)`這個方法。也就是說這只是一個函式傳入參數的代入過程。所以如果你改成這樣也是一樣的結果:

```js
//這個就是稱為executor的傳入參數
function resolver(rs, rj){
  rs(10)
  //rj(new Error('error occured !'))
}

//初始化內部雛形物件用的函式
function init(promise, resolver){
  //改用匿名函式
    resolver(function(value){
      resolve(promise, value)
    }, function(reason) {
      reject(promise, reason)
    });

    return promise
}
```

或用箭頭函式更簡化程式碼:

```js
//這個就是稱為executor的傳入參數
function resolver(rs, rj) {
    rs(10)
        //reject(new Error('error occured !'))
}

//初始化內部雛形物件用的函式
function init(promise, resolver) {
    //改用匿名函式與箭頭函式
    resolver((value) => resolve(promise, value), (reason) => reject(promise, reason))
    return promise
}
```
那麼executor(執行者，執行函式)是必要的傳入參數值嗎？是的，如果你沒傳入任何的參數，它會出現一個警告，這個警告訊息中有一個resolver(解決者，解決函式)字詞，它應該算是executor的別名或是第一個函式型傳入參數的名稱，在ES6標準規格書上是寫executor:

```js
const promise = new Promise()
//Uncaught TypeError: Promise resolver undefined is not a function
```

Promise物件中的兩個靜態方法，`Promise.resolve`與`Promise.reject`，它們的實作是接近上面範例中的`resolve`與`reject`內部方法，不要搞混了。下面會談到它們的用法。

你可能會覺得很奇怪，為何要用這種方式來建構一個promise的物件實體？有幾個原因:

1. 暴露的建構式樣式: Promise只是個包裹現有函式或程式的物件，所以把建構式外露出來給程式設計師自行定義其中的程式碼。這才稱之為"暴露的建構式樣式(Revealing Constructor Pattern)"。因為它也是需要用callbacks回調的傳入參數才能進行異步程式執行，所以就會設計成這樣。
2. 封裝: Promise物件並不外露狀態的值，也無法從外部程式修改其值，唯一個可更改狀態的方式，就是要執行初始化的最後結果為兩種的程式。物件一實體化(回傳)後，狀態即由executor的執行結果決定。
3. Throw-safe: 確保在建構函式在執行過程時，如果有throw例外的情況，也是安全的(異步的)。

> 註: 有一些術語或形容詞的意思大概都是相同的，fulfilled(已實現)、resolve(解決)、successful(成功的)、completed(完成的)差不多意思。而rejected(已拒絕)、fail(失敗)、error(錯誤)是另一個意思。

### then與catch

在Promise的標準中，一直不斷的提到一個方法 - `then`，中文是"然後、接著、接下來"的意思，這個是一個Promise的重要方法。有定義then方法的物件被稱之為`thenable`物件，標準中花了一個章節在講`then`方法的規格，它的語法如下(出自[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)):

```js
p.then(onFulfilled, onRejected);

p.then(function(value) {
   // fulfillment
  }, function(reason) {
  // rejection
});
```

`then`方法一樣用兩個函式當作傳入參數，`onFulfilled`是當promise物件的狀態轉變為fulfilled(已實現)呼叫的函式，有一個傳入參數值可用，就是value(值)。而`onRejected`是當promise物件的狀態轉變為rejected(已拒絕)呼叫的函式，也有一個傳入參數值可以用，就是reason(理由)。

為什麼說它是"一樣"？因為比對到promise物件的建構式，與那個傳入的函式參數值的樣子非常像，也是兩個函式當作傳入參數，只是名稱的定義上有點不同，但接近同意義。

那麼`then`方法最後的回傳值是什麼？是另一個新的promise物件。

> Promises/A+標準 2.2.7
>
> `then`必須回傳一個promise。`promise2 = promise1.then(onFulfilled, onRejected);`

這樣設計的目的，主要是要能作連鎖(chained)的語法結構，也就是稱之為合成(composition)的一種運算方式，在JavaScript中如果回傳值相同的函式，可以使用連鎖的語法。像下面這樣的程式碼:

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

`then`方法中的onFulfilled函式，也就是第一個函式傳入參數，它是有值時使用的函式，經過連鎖的結構，如果要把值往下傳遞，可以用回傳值的方式，上面的例子可以看到用`return`語句來回傳值，這個值可以繼續的往下面的`then`方法傳送。

onRejected函式，也就是`then`方法中第二個函式的傳入參數，也有用回傳值往下傳遞的特性，不過因為它是使用於錯誤的處理，除非你是有要用來修正錯誤之類的理由，不然說實在這樣作會有點混亂，因為不論是onFulfilled函式或onRejected函式的傳遞值，都只會記錄到新產生的那個Promise物件，對值來說並沒有區分是onFulfilled回傳的，還是onRejected回傳的。當一直有回傳值時就可以一直傳遞回傳值，當出現錯誤時，因為抓不到之前的值，會導致之前的值不見。

```js
const p1 = new Promise((resolve, reject) => {
    resolve(4)
})

p1.then((val) => {
        console.log(val) //4
        return val + 2
    })
    .then((val) => {
        console.log(val) //6
        throw new Error('error!')
    })
    .catch((err) => {      //catch無法抓到上個promise的回傳值
        console.log(err.message)
        //這裡如果有回傳值，下一個then可以抓得到
        //return 100
    })
    .then((val) => console.log(val, 'done')) //val是undefined，回傳值消息
```

`then`方法中的兩個函式傳入參數，第1個onFulfilled函式是在promise物件有值的情況下才會執行，也就是進入到fulfilled(已實現)狀態。第2個onRejected函式則是在promise物件發生錯誤、失敗，才會執行。這兩個函式都可以寫出來，但為了方便進行多個不同程式碼的連鎖，通常在只使用`then`方法時，都只寫第1個函式傳入參數。

而錯誤處理通常交給另一個`catch`方法來作，`catch`只需要一個函式傳入參數，(出自[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)):

```js
p.catch(onRejected);

p.catch(function(reason) {
   // rejection
});
```

`catch`方法相當於`then(undefined, onRejected)`，也就是`then`方法的第一個函式傳入參數沒有給定值的情況，它算是個`then`方法的語法糖。`catch`方法正如其名，它就是要取代同步`try...catch`語句用的異步例外處理方式。

> 註: 不過也因為`catch`方法與`try...catch`中的`catch`同名，造成IE8以下的瀏覽器產生衝突與錯誤。有些函式庫會用`caught`這個名稱來取代它，或是乾脆用`then`方法就好。

### 執行流程與錯誤處理

#### throw與reject

在promise建構函式中，直接使用`throw`語句相當於用`reject`方法的作用，一個簡單的範例如下:

```js
// 用throw語句取代reject
const p1 = new Promise((resolve, reject) => {
    throw new Error('rejected!') // 用throw語句
    //相當於用以下的語句
    //reject(new Error('rejected!'))
})

p1.then((val) => {
        console.log(val)
        return val + 2
    }).then((val) => console.log(val))
    .catch((err) => console.log('error:', err.message))
    .then((val) => console.log('done'))

//最後結果:
//error: rejected!
//done
```

原先在錯誤處理章節中，對於`throw`語句的說明是，`throw`語句執行後會直接進到最近的`catch`方法的區塊中，執行區塊中程式碼後中斷之後程式碼。但這個理解是針對一般的同步程式結構，對異步的Promise結構並不適用。所以**並不會**中斷Promise結構的繼續執行，上面最後有個`then`中會輸出'done'字串，它依然會輸出。而且這裡的`throw`語句並不會瀏覽器的錯誤主控台出現錯誤訊息。

使用`throw`語句與用`reject`方法似乎是同樣的結果，都會導致promise物件的狀態變為rejected(已拒絕)，那麼這兩種方式有差異嗎？

有的，這兩種方式是有差異的。

首先，`throw`語句用於一般的程式碼中，它代表的意義是程式執行到某個時候發生錯誤，也就是`throw`語句會立即完成resolve(解決)，在then方法中按照規則，不論是onFulfilled函式或onRejected函式，只要丟出例外，就會導致新的promise物件的狀態直接變為Rejected(已拒絕)。

而`reject`則是一個一旦呼叫了就會讓Promise物件狀態變為Rejected(已拒絕)的方法，用起來像是一般的可呼叫的方法。

根據上面的解說，這兩種方式明顯是用在不同的場合的，實際上也無關優劣性。

其次，而在Promise中(建構函式或then)使用其他異步callback的API時會出現明顯的差異，這時候完全不能使用`throw`語句，以下的範例你可以試試:

```js
const p1 = new Promise(function(resolve, reject){
     setTimeout(function(){
         // 這裡如果用throw，是完全不會拒絕promise
         reject(new Error('error occur!'))
         //throw new Error('error occur!')
     }, 1000);
});

p1.then((val) => {
        console.log(val)
        return val + 2
    }).then((val) => console.log(val))
    .catch((err) => console.log('error:', err.message))
    .then((val) => console.log('done'))
```

### 執行順序

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

在這篇文章中[JavaScript Promises](http://www.html5rocks.com/en/tutorials/es6/promises/)的也有一個這個範例的流程圖。

我要建議的是不要單純用只有onFulfilled函式的`then`方法，以及`catch`方法來看整體流程，其實會有點混亂。從完整的`then`方法可以把流程看得更清楚，每個`then`方法(或`catch`方法)都會回傳一個完整的Promise物件，當新的Promise往下個`then`方法傳遞時，因原來其中程式碼執行的不同，狀態也不同。

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

> 註: catch方法通常會擺在整體流程的後面，這是因為擺前面會捕捉不到在它後面的then方法中的錯誤

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

我想第3步是不易理解的，實際上它需要根據errorHandler1()函式的回傳值來決定新的Promise是哪一種狀態。上面只是一般的情況，當errorHandler1()只是簡單的回傳值或沒回傳值時。以下解說回傳值的差異情況。

根據連鎖反應，rejected狀態的Promise物件，會一直往下找到有`onRejected`函式的`then`方法，或是`catch`方法才會進入2.2.7.1規則，也就是正常的Promise處理程序，那麼在正常的處理程序中，對於處理時發生的情況，又是怎麼決定回傳的新Promise物件？下面是大概的摘要。

2.2.7.1規則裡面的執行原則，主要是由回傳值`x`來決定的，這裡所謂的`x`是經過onFulfilled或onRejected其中有一個被執行後的回傳值`x`，有幾種情況:

- `x`不是函式或物件，直接用`x`實現(fulfill)promise物件(**注意: undefined也算，所以沒回傳值算這個規則**)
- `x`是promise，看`x`最後是實現(fulfill)或拒絕，就直接對應到promise物件，回傳值或理由也會一併跟著
- `x`是函式或物件，指定`then = x.then`然後執行`then`。其實它是在測試`x`是不是thenable物件，如果是thenable物件，會執行thenable物件中的then方法，一樣用類似then方法的兩選一來解析狀態，最後的狀態與值會影響到promise物件。一般情況就是如果不是thenable物件，也是直接用`x`作實現(fulfill)promise物件。

### 深入 then方法

有些內容其實上面都有說過了，算是大概再整理一下。

#### 一個promise物件經過then後，原本的promise物件的內容會改變？

不會。then方法會另外產生一個新的物件。以下為解說範例:

```js
const promise = new Promise(function(resolve, reject) {
    resolve(1)
})

const p1 = promise.then((value) => {
    return value + 1
})

const p2 = promise.then((value) => {
    return value + 1
})

//延時執行
setTimeout(
() => {
  console.log(promise)
  console.log(p1)
  console.log(p2)
  console.log(p1 === p2) //false
}, 10000)
```

#### `then`方法中函式回傳值(onFulfilled)

`then`方法中onFulfilled函式回傳值，或是Promise建構函式中的resolve中的參數值，這是已經進入正常的"執行Promise解析程序"中。相較於onRejected函式，它通常只會用reason(理由)來處理，理由通常是個Error物件，用於錯誤處理中。當然，如果你的onRejected函式有回傳值，它的規劃也會和onFulfilled回傳值一模一樣。

上面已經有提到關於回傳值的情況，以及需要遵守的規則，你可以對照看看。這個章節的內容是要討論onFulfilled函式，你可以自訂哪一些回傳值。

`then`方法中onFulfilled函式回傳值可以有三種不同類型:

- 值
- promise物件
- thenable物件

值的話，就一般在JavaScript的各種值，這沒什麼好講的，因為`then`方法最後會回傳Promise物件，所以這個回傳值會跟著這個新Promise物件到下一個連鎖方法去，這個回傳值可以用下個`then`方法的onFulfilled函式取到第1個傳入參數值中。

promise物件的話，當你不希望`then`幫你回傳promise物件，自己建立一個，這也是合情合理。通常是使用promise建構式new一個，或是直接用`Promise.resolve(value)`靜態方法，產生一個fulfilled(已實現)狀態的promise物件。

thenable物件是最特殊的，根據標準的定義為:

> `thenable` 是一個有定義then方法的物件或函式

按照標準上的解說，thenable是提供給沒有符合標準的其他實作函式庫或框架，利用合理的`then`方法來進行同化的的方式。以最簡單的情況說明，thenable物件是個單純物件，然後裡面有個then方法的定義而已，例如以下的範例:

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

```

#### then中的函式傳入參數值(onFulfilled)

在開始討論前，稍作整理一下`then`方法中的傳入參數如下。

```
promise.then(onFulfilled, onRejected)
```

then是promise物件中的方法，以onFulfilled與onRejected是作為兩個傳入參數，有幾個規則需要遵守:

- 當onFulfilled或onRejected不是函式時，忽略跳過
- 當promise是fulfilled時，執行onFulfilled函式，並帶有promise的value作為onFulfilled函式的傳入參數值
- 當promise是rejected時，執行onRejected函式，並帶有promise的reason作為onRejected函式的傳入參數值

then方法最後還要回傳另一個promise，也就是:

```
promise2 = promise1.then(onFulfilled, onRejected)
```

以下假設只使用then方法中的第1個onFulfilled傳入參數(onRejected也是一樣)，那麼它可以有四種傳入的情況。在這個範例中，我們會把焦點放在其中有關於值的回傳情況。仔細看下面的範例中的`doSomething2`函式:

```js
function doSomething1(){
  console.log('doSomething1 start')
  return new Promise(function(resolve, reject) {
      console.log('doSomething1 end')
      resolve(1)
  })
}

function doSomething2(){
  console.log('doSomething2')
  return 2
}

function finalThing(value){
  console.log('finalThing')
  console.log(value)
  return 0
}

//第1種傳入參數
doSomething1().then(doSomething2).then(finalThing)

//第2種傳入參數
doSomething1().then(doSomething2()).then(finalThing)

//第3種傳入參數
doSomething1().then(function(){doSomething2()}).then(finalThing)

//第3種傳入參數
doSomething1().then(function(){return doSomething2()}).then(finalThing)
```

第1種: 正常的函式傳入參數。最後的`finalThing`可以得到`doSomething2`回傳值`2`。

第2種: 雖然說`then`方法的規則，如果onFulfilled不是函式時會忽略，但這裡是執行`doSomething2()`函式，onFulfilled相當於`doSomething2()`的回傳值，JavaScript中的函式回傳值可以是個函式，沒執行過怎麼會知道它是不是回傳一個函式？所以會執行`doSomething2()`，但最後得到onFulfilled不是一個函式，所以忽略它。依照連鎖規則2.2.7.3(上面有說)，當onFulfilled不是函式，繼續用fulfilled狀態與帶值回傳新的Promise物件到下個then方法，最後的`finalThing`得到的值是`doSomething1`中的`1`。

第3種: 正常的函式傳入參數，因為在函式中執行`doSomething2()`，這個onFulfilled最後的回傳值其實是undefined，但算有回傳值，回傳的新Promise物件也是fulfilled狀態，不過值變成`undefined`。最後的`finalThing`得到`undefined`值。

第4種: 正常的函式傳入參數，`then`方法執行完onFulfilled最後的回傳值是`doSomething2()`的執行後的值也就是`2`，回傳的新Promise物件也是fulfilled狀態，最後的`finalThing`得到`2`值。

### then中的函式傳入參數(異步)(onFulfilled)

上面的概念如果在doSomething1、doSomething2與finalThing函式中，都有異步的callbacks時，這時除了值的情況，我們還會關心整體的執行順序。這個程式碼範例是來自[We have a problem with promises](https://pouchdb.com/2015/05/18/we-have-a-problem-with-promises.html)，其實這已經是稍微進階的討論議題了。

```js
function doSomething1(){
  console.log('doSomething1 start')
  return new Promise(function(resolve, reject) {
    setTimeout(function(){
      console.log('doSomething1 end')
      resolve(1)
    }, 1000)

  })
}

function doSomething2(){
  console.log('doSomething2 start')
  return new Promise(function(resolve, reject) {
    setTimeout(function(){
      console.log('doSomething2 end')
      resolve(2)
    }, 1000)

  })
}

function finalThing(value){
  console.log('finalThing start')
  return new Promise(function(resolve, reject) {
    setTimeout(function(){
      console.log('finalThing end')
      console.log(value)
      resolve(0)
    }, 1000)

  })
}

//第1種傳入參數，finalThing最後的值為2
doSomething1().then(doSomething2).then(finalThing)

//第2種傳入參數，finalThing最後的值為1
doSomething1().then(doSomething2()).then(finalThing)

//第3種傳入參數，finalThing最後的值為undefined
doSomething1().then(function(){doSomething2()}).then(finalThing)

//第4種傳入參數，finalThing最後的值為2
doSomething1().then(function(){return doSomething2()}).then(finalThing)
```

執行的結果會出乎想像，只有第1種與第4種，才是完整的Promise流程順序，也就是像下面的流程(在Chrome與Firefox瀏覽器測試結果相同):

```
doSomething1 start
doSomething1 end
doSomething2 start
doSomething2 end
finalThing start
finalThing end
```

第2種中的`then`方法裡onFulfilled傳入參數的`doSomething2()`執行語句，它是一個同步的語句，所以會在`doSomething1`還沒執行完成時，就先被執行，所以流程會變為:

```
doSomething1 start
doSomething2 start
doSomething1 end
finalThing start
doSomething2 end
finalThing end
```

第3種在`then`方法裡裡onFulfilled的`function(){doSomething2()}`裡面的`doSomething2()`執行語句，也是一個同步的語句，但外團的匿名函式卻是一個異步函式，因為這樣會在`doSomething1()`結束才開始執行，但是也是在`finalThing`開始後才會結束。不過流程也是怪異:

```
doSomething1 start
doSomething1 end
doSomething2 start
finalThing start
doSomething2 end
finalThing end
```

從這個範例中，我認為並不需要太深究其中的順序的原因是為何。其實是在告訴你不要亂用`then`方法中的傳入參數值，要不就是個堂堂正正的函式，要不然就寫好一個有回傳值的匿名函式。此外，如果你要在Promise結構中使用其他的異步API，更是要注意它們的執行順序，用想的還不如直接寫出來執行看看，有可能不見得是最後是你要的。

### 靜態方法 Promise.resolve與Promise.reject


### 靜態方法 Promise.all與Promise.race



### 執行順序

http://stackoverflow.com/questions/29111626/javascript-promise-then-ordering
http://stackoverflow.com/questions/36526639/es6-promise-execution-order
http://stackoverflow.com/questions/29853578/understanding-javascript-promises-stacks-and-chaining/29854205#29854205

### 最佳實踐

## 反樣式(anti-pattern)

## 資源參考

### 標準

- [Promises/A+](https://promisesaplus.com/)
- [ECMA-262](https://tc39.github.io/ecma262/#sec-promise-objects)
- [Writing Promise-Using Specifications(W3C)](https://www.w3.org/2001/tag/doc/promises-guide)

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
