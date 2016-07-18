# 錯誤與例外處理

實際上在程式設計領域中，只有"例外處理(Exception Handling)"的說法，而沒有"錯誤處理(Error handling)"。而在其他語言中是有區分的，例如在Java語言中，就有明確區分這兩種不同的物件，它的分別大概是以下這樣:

- Error(錯誤):指的是無法回復的錯誤，經常是因為執行環境造成的錯誤
- Exception(例外):可以用try-catch語句或throw語句捕抓到例外，可以從例外中回復，經常是因為應用程式本身造成的例外

不過，在JavaScript中，Error(錯誤)與Exception(例外)基本上是同一種意思，因為JavaScript的設計中，它一直以來是在一個受限的執行環境下執行(瀏覽器)，加上只有一個內建的專門作為處理例外用的Error物件，在API中也有一些以error作為名稱的方法。而且，在ECMAScript標準中也沒有很明確的區別，所以常常用錯誤處理作為說明的標題。但是，在伺服器端的JavaScript(Node.js)就不是這樣的執行方式，所以錯誤處理如果在伺服器端就不是那麼簡單的一件事，它必須考量更多情況，在使用上也與客戶端程式有所分別。

> 註: [例外處理(Exception Handling) 維基百科](https://en.wikipedia.org/wiki/Exception_handling)

例外處理的思維是一種"求敗"的程式設計哲學，Exception(例外)明確來說有"異常"、"並非預期"的意思。程式設計師應該考量許多異常或非預期的情況，在這些情況發生時，能夠加以控制或管理，這種處理稱之為例外處理。

## 錯誤何時會發生

錯誤何時會發生？在JavaScript語言中有很多種不同的分類法，我採用的是來自這篇[Error Handling in Node.js](https://www.joyent.com/node-js/production/design/errors)的講法，雖然它主要是針對伺服器端的JavaScript程式，但其中的概念會比較符合目前的程式寫法。

基本上錯誤可分為兩大類:

- 程式開發者的錯誤(Programmer errors): 其實就是程式臭蟲(bugs)，錯誤是程式開發者自己在撰寫程式碼中造成的，有很多狀況是應避免而未避免，這種錯誤無法處理。例如像以下的錯誤:
    - 嘗式讀取某個"undefined"的屬性
    - 用"string"類型傳入某個應該為"object"的參數

- 運算的錯誤(Operational errors): 在執行階段發生的錯誤，程式本身並沒有臭蟲(bugs)的情況，錯誤的發生源是來自系統本身，多半是發生於程式在執行時與外部環境互動產生的異常情況，例如使用者輸入不合法、網路連線失敗、檔案讀取失敗或是記憶體問題。例如像以下的錯誤:
    - 不合法的使用者輸入
    - 伺服器無法連線
    - 記憶體超出限制

由於我們只能處理"運算的錯誤"而無法處理"程式開發者的錯誤"，我們的重點會放在如何處理"運算的錯誤"上，大致的處理方式有幾個重點:

- 直接處理錯誤: 有些錯誤是可以直接處理的，例如第一次開啟檔案時，有可能檔案並不存在，這時候你在偵測到這個錯誤時，可以建立一個新的檔案。
- 傳播(Propagate)錯誤給你的客戶端: 如果你不知道如何處理這個錯誤情況，可以直接把這個錯誤傳播給你的客戶端。
- 嘗試重新操作: 有些錯誤是因為網路連線造成的錯誤，通常可以在一段時間後，使用嘗試重新操作來進行。
- 記錄錯誤，不作任何處理: 某些錯誤並沒有辦法立即處理，或是根本也不是你能處理的情況，所以用這個方式先記錄來追蹤。

## Error物件

Error物件是JavaScript中內建的用於處理錯誤的物件，其中共分成6個種類，用於不同的的錯誤情況:

- EvalError: 配合`eval()`方法使用的錯誤，但因`eval`是一個很糟的全域方法，所以這個錯誤種類幾乎不會用到。
- SyntaxError: 這個也是配合`eval()`方法使用的錯誤，幾乎不會用到。其他的語法錯誤將由瀏覽器直接回報。
- RangeError: 超出範圍的錯誤
- ReferenceError: 參照錯誤
- TypeError: 型態錯誤
- URIError: 配合encodeURI()或decodeURI()方法使用的錯誤

自訂的Error物件可以使用Error物件的建構式實體化，它只需要一個訊息字串作為傳入參數，Error物件也是一個必定要使用`new`運算符進行實體化的內建物件，通常會配合`throw`語句使用，例如以下的範例:

```js
try {
  throw new Error('Whoops!');
} catch (e) {
  console.log(e.name + ': ' + e.message);
}
```

使用Error物件有一些優點，有些瀏覽器品牌會針對Error物件作額外的擴充屬性或功能，讓它在除錯上更佳方便，例如stack(堆疊)這個屬性，它可以對目前Error物件發生的程式碼，列出呼叫的堆疊進行追蹤。在複雜的應用程式中，可以很快找出是位於某個程式碼中的呼叫所造成的錯誤。

不過，這些都算是Error物件中非標準的屬性，在每種瀏覽器品牌的實作情況的不一定，除了在開發階段使用，不建議你使用這些非標準的屬性在正式的應用程式中。如果你要使用與相容各版本品牌的瀏覽器的stack屬性，可以考慮使用外部的函式庫，例如[TraceKit](https://github.com/csnover/TraceKit/)或[stacktrace.js](https://github.com/stacktracejs/stacktrace.js)。

## 捕抓例外的try...catch語句

`try...catch`語句也是一種控制流程的語句。意思是如果所有位於`try`區塊中的語句都沒問題的話，就執行`try`其中的語句，當發生例外時會將控制權轉到`catch`區塊，執行`catch`區塊中的語句。簡單的範例如下:

```js
try{
    document.getElementById('test').innerHTML  = 'test'
} catch(e) {
    console.log(e) //TypeError: Cannot set property 'innerHTML' of null(…)
}
```

可以用if...else語句來類似的測試語句，不過這個時候不會自動捕抓錯誤，程式設計師要自己決定:

```js
if(document.getElementById('test')){
    document.getElementById('test').innerHTML  = 'test'
} else {
    console.log(new TypeError('Cannot set property \'innerHTML\''))
}
```

`try...catch`語句最後還可以額外加上`finally`區塊，它是不論如何都會執行的語句區塊，例如你在try語句中開啟了一個檔案要進行處理，不論有沒有發生例外，最後需要把這個檔案進行關閉，這就是寫在`finally`區塊中。

`try...catch`語句聽起來似乎不錯，但在使用上的確需要再三考慮，尤其是在伺服器端的Node.js上，只能在非不得已的時候才使用，瀏覽器端則是還不需要考量那麼多。有幾個明顯的原因:

- 它是高消費的語句: 在有重大效能考量或迴圈的語句，不建議使用`try...catch`語句。
- 它是同步的語句: 如果是重度使用callback(回調)或promise樣式的異步程式，不需要使用它。此外，Promise語句可以完全取代它，而且是異步的語句。
- 不需要: 如果可以使用`if...else`的簡單程式碼中，將不會看到它的存在。另外，在一些複雜的對外取得資源的功能，例如Ajax，我們一般都會使用額外的函式庫來協助處理，這些函式庫都有考量到比你想得到還完整的各種例外情況，所以也不需要由你親自動手來作例外處理。

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

`throw`語句會"丟出"程式設計師自訂的例外，`throw`語句會使接下來的任何語句都中斷執行，然後把控制權轉交給呼叫堆疊(call stack)中第一個的`catch`區塊，如果沒有`catch`區塊就會停止應用程式。`throw`語句後面雖然是可以使用任何的表達式，但一般使用上都會使用Error物件。以下為簡單的範例:

```js
if( x === 0){
  throw new Error('x equals zero')
}
```

有些內建的JavaScript方法，在發生錯誤時就會自動"丟出"對應的例外，例如常見的`JSON.parse`方法，你也可以在自己撰寫的函式裡這樣作，例如上一節中的範例，這也是很常見的一種作法。

## window.onerror

## 最佳實踐



http://speakingjs.com/es5/ch14.html

https://www.nczonline.net/blog/2009/04/28/javascript-error-handling-anti-pattern/

http://stackoverflow.com/questions/6484528/what-are-the-best-practices-for-javascript-error-handling

http://www.slideshare.net/nzakas/enterprise-javascript-error-handling-presentation

http://stackoverflow.com/questions/7310521/node-js-best-practice-exception-handling/15935021#15935021

http://ruben.verborgh.org/blog/2012/12/31/asynchronous-error-handling-in-javascript/

https://www.joyent.com/node-js/production/design/errors

http://stackoverflow.com/questions/9156176/what-is-the-difference-between-throw-new-error-and-throw-someobject-in-javas

https://github.com/RubenVerborgh/JavaScript-Error-Handling

http://javascript.info/tutorial/exceptions#the-throw-statement

http://www.techrepublic.com/blog/australian-technology/error-handling-in-javascript-rarely-done-often-needed/