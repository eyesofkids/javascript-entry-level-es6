# 錯誤與例外處理

例外處理的思維是一種"求敗"的程式設計哲學，Exception(例外)明確來說有"異常"、"非預期"的意思。程式設計師應該考量許多異常或非預期的情況，在這些情況發生時，能夠加以控制或管理，這種處理稱之為例外處理。

在程式設計學術領域中，實際上只有"例外處理(Exception Handling)"的說法，而沒有"錯誤處理(Error handling)"，因為錯誤在定義上是無法處理的，例如在Java程式語言中，就有明確區分這兩種不同的錯誤與例外物件。

不過，在JavaScript語言中，Error(錯誤)與Exception(例外)基本上是同一種意思。這是因為JavaScript一直以來是在一個資源受限的環境下執行(瀏覽器)，對它來說錯誤是在一個有限的範圍發生，所以在設計上也只有一個專門處理例外用的Error物件，在ECMAScript標準中並沒有很明確的區別。

但是，如果是在伺服器端的JavaScript(Node.js)程式，它的執行環境就不是使用有限的環境，在錯誤處理上必須考量更多情況，在整體的設計也會與客戶端程式有所分別，這部份會比較複雜。

> 註: [例外處理(Exception Handling) 維基百科](https://en.wikipedia.org/wiki/Exception_handling)

## 錯誤何時會發生

錯誤何時會發生？在JavaScript語言中有很多種不同的分類法，基本上我們可以把錯誤可分為兩大類:

- 程式開發者的錯誤(Programmer errors): 其實就是程式臭蟲(bugs)，錯誤是程式開發者自己在撰寫程式碼中造成的，有很多狀況是開發者應避免而未避免，這種錯誤難以處理，需要開發者進一步修正。例如像以下的錯誤:
    - 嘗試讀取某個"undefined"的屬性
    - 用"string"類型傳入某個應該為"object"的參數

- 運算的錯誤(Operational errors): 在執行階段發生的錯誤，在程式本身並沒有臭蟲(bugs)的情況，錯誤的發生源是來自系統本身，大部份是發生於程式在執行時與外部環境互動產生的異常情況，例如使用者輸入不合法、網路連線失敗、檔案讀取失敗或是記憶體問題等等。例如像以下的錯誤:
    - 不合法的使用者輸入
    - 伺服器無法連線
    - 記憶體超出限制

由於我們只能處理"運算的錯誤"而無法處理"程式開發者的錯誤"，大致的處理方式有幾個重點:

- 直接處理錯誤: 有些錯誤是可以直接處理的，例如第一次開啟檔案時，有可能檔案並不存在，這時候你在偵測到這個錯誤時，可以建立一個新的檔案。
- 傳播錯誤給你的客戶端: 如果你不知道如何處理這個錯誤情況，可以直接把這個錯誤傳播給你的客戶端。
- 嘗試重新操作: 有些錯誤是因為網路連線造成的錯誤，通常可以在一段時間後，使用嘗試重新操作來進行。
- 記錄錯誤，不作任何處理: 某些錯誤並沒有辦法立即處理，或是根本也不是能處理的情況，所以用這個方式先記錄來追蹤。

> 註: 以上說明來自[Error Handling in Node.js](https://www.joyent.com/node-js/production/design/errors)的說法

## Error物件

Error物件是JavaScript中內建的用於處理錯誤的物件，其中共分成6個種類，也就延伸出來6種不同的物件，每個不同的種類名稱即是這些物件的`name`屬性，可以用於不同的的錯誤情況，以下為這些種類的說明:

- EvalError: 配合`eval()`方法使用所產生的錯誤，但因`eval`是一個不建議使用的全域方法，幾乎不會用到。
- SyntaxError: 這個也是配合`eval()`方法使用所產生的錯誤，幾乎不會用到。其他的語法錯誤將由瀏覽器直接回報。
- RangeError: 超出範圍所產生的的錯誤
- ReferenceError: 參照錯誤
- TypeError: 資料型態錯誤
- URIError: 配合encodeURI()或decodeURI()方法使用的錯誤

自訂的Error物件可以使用Error物件的建構式實體化，它只需要一個訊息字串作為傳入參數，Error物件也是一個必定要使用`new`運算符進行實體化的內建物件，通常會配合`throw`語句使用，例如以下的範例:

```js
try {
  throw new Error('Whoops!');
} catch (e) {
  console.log(e.name + ': ' + e.message);
}
```

使用Error物件有一些優點，有些瀏覽器品牌會針對Error物件作額外的擴充屬性或功能，讓它在除錯上更佳方便，例如`stack`(堆疊)這個屬性，它可以對目前Error物件發生的程式碼，列出呼叫的堆疊追蹤。在複雜的應用程式中，可以很快找出是位於某個程式碼中的呼叫所造成的錯誤。

不過，這些都算是Error物件中非標準的屬性，在每種瀏覽器品牌的實作情況的不一定，除了在開發階段使用，不建議使用非標準的屬性在正式的應用程式中。如果要使用相容各瀏覽器版本品牌的`stack`屬性，可以考慮使用外部的函式庫，例如[TraceKit](https://github.com/csnover/TraceKit/)或[stacktrace.js](https://github.com/stacktracejs/stacktrace.js)。

## 捕捉例外的try...catch語句

`try...catch`語句也是一種控制流程的語句。意思是如果所有位於`try`區塊中的語句都沒問題的話，就執行`try`其中的語句，當發生例外時會將控制權轉到`catch`區塊，執行`catch`區塊中的語句。`catch`可以在例外發生時，自動捕捉所有的例外情況，這在程式撰寫時相當方便。簡單的範例如下:

```js
try{
    document.getElementById('test').innerHTML  = 'test'
} catch(e) {
    console.log(e) //TypeError: Cannot set property 'innerHTML' of null(…)
}
```

可以用if...else語句來類似的測試語句，不過這個時候不會自動捕捉錯誤，程式設計師要自己決定:

```js
if(document.getElementById('test')){
    document.getElementById('test').innerHTML  = 'test'
} else {
    console.log(new TypeError('Cannot set property \'innerHTML\''))
}
```

`try...catch`語句最後還可以額外加上`finally`區塊，它是不論如何都會執行的語句區塊，例如你在try語句中開啟了一個檔案要進行處理，不論有沒有發生例外，最後需要把這個檔案進行關閉，這就是寫在`finally`區塊中。

`try...catch`語句聽起來似乎不錯，但在使用上的確需要再三考慮，尤其是在伺服器端的Node.js上，只能在必要的時候才會使用。在瀏覽器上的應用程式則是不需要考量那麼多。有幾個明顯的原因:

- 它是高消費的語句: 在有重大效能考量或迴圈的語句，不建議使用`try...catch`語句。
- 它是同步的語句: 如果是重度使用callback(回調)或promise樣式的異步程式，不需要使用它。此外，Promise語句可以完全取代它，而且是異步的語句。
- 不需要: 如果可以使用`if...else`的簡單程式碼中，將不會看到它的存在。另外，在一些對外取得資源的功能例如Ajax，我們一般都會使用額外函式庫來協助處理，這些函式庫都有考慮到比你想得到還完整的各種例外情況，所以也不需要由你親自來作例外處理。

那麼`try...catch`語句會使用在什麼情況下？通常會搭配會在例外發生時，直接丟出例外的方法上，這些方法有可能是JavaScript內建的，也可能是程式設計師自己設計的。最常見的是`JSON.parse`這個內建的用於解析JSON格式字串為對應物件的方法，範例如下:

```js
function validateData(jsonData){
    let data
    try{
      data = JSON.parse(jsonData);
    } catch(e) {
      console.log('Error data', e)
      data = null
    }
    return data
}

console.log(validateData('[1, 5, "false"]'))
console.log(validateData('1, 11'))
```

另外用於使用者輸入檢查的情況，這也是會用到`try...catch`語句的常見情況，這種通常稱之為錯誤中心(Error Center)的方式，用於集中很多不同的例外為一個語句中。以下為範例程式:

```js
function checkUserEntry()
{
    const userEntry = document.getElementById('email').value

    if (userEntry.length === 0) {
        throw new Error('請輸入字串')
    } else if (userEntry.indexOf('@') === -1) {
        throw new Error('請提供合法的Email住址')
    }

    document.getElementById('console').innerHTML =
    '<h3 style="color:yellow">你的Email是: </h3>' + userEntry
}

function validateEntry()
{
    try {
        checkUserEntry()
    } catch(e) {
        document.getElementById('console').innerHTML =
        '<h3 style="color:red"> 警告: </h3>' + e.message
    }
}

document.getElementById('clickme').addEventListener('click', validateEntry)
```

這是這個範例的html檔案內容:

```html
<input type="text" id="email" />
<button id="clickme">點我</button>
<div id="console" style="height:200px; width:300px; background-color: green"></div>
```

## 丟出例外的throw

`throw`語句會"丟出"程式設計師自訂的例外，`throw`語句會中斷執行，所以在下面的語句並不會再被執行，然後把控制權轉交給呼叫堆疊(call stack)中第一個的`catch`區塊，如果沒有`catch`區塊則會立即停止應用程式。

`throw`語句後面雖然是可以使用任何的表達式，但一般使用上都會直接使用Error物件。以下為簡單的範例:

```js
if( x === 0){
  throw new Error('x equals zero')
}
```

有些內建的JavaScript方法，在發生錯誤時就會自動"丟出"對應的例外，例如常見的`JSON.parse`方法，你也可以在自己撰寫的函式裡這樣作，例如上一節中的範例，這也是很常見的一種作法。

## window.onerror

瀏覽器中全域的事件處理器(event handler)，它會在錯誤發生時被觸發，這是另一種利用事件作為錯誤處理的機制。以下有兩種會觸發錯誤事件的情況:

- JavaScript執行階段錯誤(runtime error): 各種錯誤，包含語法錯誤均會觸發ErrorEvent介面，然後呼叫window.onerror方法。
- 當資源無法載入時: 例如`<img>`或`<script>`無法正確載入時，會觸發該元素的Event介面，以及呼叫該元素的onerror()方法。

window.onerror是針對JavaScript執行階段錯誤所設計的錯誤事件呼叫的方法，它的語法如下:

```js
window.onerror = function(message, filename, lineNo, columnNo, errorObj) { ... }
```

> 註: errorObj參數指的就是Error物件

而針對每個元素所設計的onerror方法，傳入的參數並不同，以此作為區別。它的語法如下:

```js
element.onerror = function(event) { ... }
```

我們的重點會放在`window.onerror`方法上，它算是所有執行階段錯誤的集中處，也是瀏覽器最後可以捕捉到錯誤的方法，通常會用於記錄錯誤之用。但這個方法並不是很完美的方法，它有一些明顯的缺陷。

首先，它在各種瀏覽器品牌與版本上，一直沒有完整的標準，例如以最後傳入的Error物件參數為例，目前只有在Firefox、Chrome、IE 11上才有支援，在最新的Microsoft Edge或是iOS、Safari瀏覽器上，都沒有這個傳入參數，也可以靠`try/catch`補強的方法來讓所有瀏覽器都可以有這個傳入值。

另外，有一個問題會發生在如果使用的JavaScript程式檔案是來自於CDN時，在Firefox與Chrome瀏覽器會因為安全機制，導致`window.onerror`方法會完全失效，目前已有解決的方式。

由此看來，這個`window.onerror`方法在基礎標準上並未完善，如果你要使用的話，需要參考一些補強或是相關的文件。

> 註: 瀏覽器相容的資料來自這篇文章[Capture and report JavaScript errors with window.onerror](https://blog.getsentry.com/2016/01/04/client-javascript-reporting-window-onerror.html)

> 註: CDN失效的解決方式請參考這篇文章[How to catch JavaScript Errors with window.onerror (even on Chrome and Firefox)](https://danlimerick.wordpress.com/2014/01/18/how-to-catch-javascript-errors-with-window-onerror-even-on-chrome-and-firefox/)

## 結語

錯誤處理基本上只是一個概念與設計哲學，上面所說明的各種語句例如`try/catch`、`throw`都是一些工具，方便讓程式設計師對錯誤處理的程式碼使用而已。

錯誤處理在新式的JavaScript中，可以使用Promise語法，取代原有的`try/catch`語句，詳細的內容請參考Promise的說明。
