# AJAX與fetch

## AJAX與XMLHttpRequest

[AJAX](http://adaptivepath.org/ideas/ajax-new-approach-web-applications/)這個技術名詞的出現大約是在十年前，實際上這個技術的實現是更早之前(2000年之前)。2006年XMLHttpRequest被列入W3C標準草案之中，現在已被所有的瀏覽器品牌與新版本所支援。關於AJAX的說明就不再多說，網路上有很詳細的說明。

AJAX技術在JavaScript中的實作即是以[XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest)物件(簡稱為XHR)為主要核心，正如它的名稱，它是用於客戶端對伺服器端送出httpRequest(要求)的物件，流程即是建立一個XMLHttpRequest(XHR)物件，打開網址，然後送出要求，最後處理由伺服器傳回的Response(回應)。整體的流程並不困難，但經過這麼長久的使用時間經歷(11年)，它在使用上產生不少令人頭痛的問題，例如:

- API設計得過於簡單，所有的輸出與輸入、狀態，都只能與這個XHR物件溝通取得，狀態也能用事件來追蹤
- XHR是使用以事件為基礎(event-based)模組來進行異步程式設計
- 跨網站的HTTP要求(cross-site HTTP request)與CORS(Cross-Origin Resource Sharing)不易實作
- 非文字類型的資料處理上不易實作
- 除錯不易

XHR在簡單的使用上都是像下面的範例程式碼這樣，其實你可以把它視作一種事件處理的結構，大小事都是依靠XHR物件來作，這也顯示它的使用語法並沒有把事情分隔得很清楚，而比較像是擠在一團:

```js
function reqListener() {  
  var data = JSON.parse(this.responseText);  
  console.log(data);  
}

function reqError(err) {  
  console.log('Fetch Error :-S', err);  
}

var oReq = new XMLHttpRequest();  
oReq.onload = reqListener;  
oReq.onerror = reqError;  
oReq.open('get', './api/some.json', true);  
oReq.send();
```

XHR在今天瀏覽器功能相當強大的時代，早就已經不敷使用，它在架構上明顯的有太多的問題，在很多應用情況，程式碼顯得複雜而且不易維護。除非你是有一定要使用原生JavaScript的強迫症，要不然要作AJAX程式並不會使用單純的原生XHR來撰寫。而且，一個AJAX的程式碼，並不是單純只有XHR的要求與回應這麼簡單，例如你要求一個伺服器的資料，得到資料後需要再進一步作對應的處理的流程，利用callbacks(回調)可以簡單的處理，但如果是複雜流程，就會涉及到異步程式設計的方式，原生XHR都沒有提供可用的方式，要自己想辦法。

### XHR Level 2(第2級)

XHR並不是沒有在努力進步，在約5年前已經有制定XHR的第2級新標準，但它仍然與原有XHR向下相容，所以整體的模型架構並沒有重大的改變，只是針對問題加以補強，或是提供擴充的方式而已。目前XHR第2級在9成以上的瀏覽器品牌新版本都已經支援全部的功能，除了IE系列要版本10之後才支援，與Opera Mini瀏覽器完全不支援，但仍然有一小部份功能在不同瀏覽器上實作細節的問題。XHR第2級(5年前)相較於原有的XHR(11年前)多加了以下的功能，這也是現在我們已經可以使用到的XHR的新特性:

- 指定回應格式
- 上傳文件與blob格式檔案
- 使用FormData傳送表單
- 跨來源資源共享(CORS)
- 監視傳輸的進程

不過，XHR在原本上的設計就是這樣，常被批評的是它的語法結構在使用與設定都相當的零亂。再多的補強或擴充都還是跳脫不了基本的結構，現今是HTM5、CSS3與ES6的時代，有許多新的技術正在蓬勃發展，說句實在話，就是XHR技術已經舊掉了，當時的設計不符合現在時代需求了，這也無關對或錯。

### jQuery

外部函式庫例如jQuery很早就看到XHR物件中在使用的問題，使用像jQuery的函式庫來撰寫AJAX相關功能，不光是在解決不同瀏覽器中XHR物件名稱與方法的不相容問題，或是提供簡化語法的方法這麼簡單而已。像jQuery它擴充了原有的XHR物件為jQuery XMLHttpRequest(jqXHR)，並加入類似於Promise的介面與Deferred Object(延遲物件)的設計。

為何要加入類似Promise的介面？可以看看它的說明中，是為了什麼而加入的？

> 這些方法可以使用一個以上的函式傳入參數，當`$.ajax()`的要求結束時呼叫它們。這可以讓你在單一個(request)要求中指定多個callbacks(回調)，甚至可以在要求完成後指定多個callbacks(回調)。 ~譯自jQuery官網[The jqXHR Object](http://api.jquery.com/jQuery.ajax/#jqXHR)

原生的XHR根本就沒有這種結構，Promise的結構基本上除了是一種異步程式設計的架構，它也可以包含錯誤處理的流程。簡單地來說，這外部函式庫的目標並不是只是簡化語法或瀏覽器相容性，這樣太low了，它們的目標是要"**整個取代掉以原生的XHR物件的AJAX結構**"。

jQuery作得相當成功，在Promise還沒那麼流行的前些年，裡面就已經有類似概念的設計。它的語法結構相當清楚，可閱讀性與設定彈性相當高，加上現在的新版本(3.0)已經支援正式的Promise標準，如果要使用原生的XHR，無疑是要拿石頭砸自己的腳。以下是jQuery中ajax方法的範例:

```js
// Using the core $.ajax() method
$.ajax({

    // The URL for the request
    url: "post.php",

    // The data to send (will be converted to a query string)
    data: {
        id: 123
    },

    // Whether this is a POST or GET request
    type: "GET",

    // The type of data we expect back
    dataType : "json",
})
  // Code to run if the request succeeds (is done);
  // The response is passed to the function
  .done(function( json ) {
     $( "<h1>" ).text( json.title ).appendTo( "body" );
     $( "<div class=\"content\">").html( json.html ).appendTo( "body" );
  })
  // Code to run if the request fails; the raw request and
  // status codes are passed to the function
  .fail(function( xhr, status, errorThrown ) {
    alert( "Sorry, there was a problem!" );
    console.log( "Error: " + errorThrown );
    console.log( "Status: " + status );
    console.dir( xhr );
  })
  // Code to run regardless of success or failure;
  .always(function( xhr, status ) {
    alert( "The request is complete!" );
  });
```

把原生的XHR用Promise包裹住，的確是一個好作法，有很多其他的函式庫也是使用類似的作法，例如[axios](https://github.com/mzabriskie/axios)與[SuperAgent](https://github.com/visionmedia/superagent)，不過這些是專門只使用於AJAX的函式庫，這些函式庫也可以用在伺服器端，也是有它一定的使用族群。

## Fetch

Fetch是近年來號稱要取代XHR的新技術標準，它首先由Mozilla與Google公司在2015年3月發佈實作消息，目前也只有Firefox與Chrome、Opera瀏覽器在新版本中原生支援，微軟的新瀏覽器Edge也在最近宣佈支援([新聞連結](https://blogs.windows.com/msedgedev/2016/05/24/fetch-and-xhr-limitations/))(應該是Edge 14)，其他瀏覽器目前可以使用[polyfill](https://github.com/github/fetch)來作填充，提供暫時解決相容性的方案。Fetch同樣要使用ES6 Promise的新特性，這代表如果瀏覽器沒有Promise特性，一樣也需要使用[es6-promise](https://github.com/stefanpenner/es6-promise)來作填充。

Fetch並不是單純一個XHR的加強版或改進版本，它是一個用不同角度思考的設計，雖然是可以作類似的事情，但它與XHR的設計完全不同，Fetch還是基於Promise語法結構的設計，而且它的設計足夠低階，這表示它可以依照實際需求進行更多彈性的設定。相對於XHR的功能來說，Fetch已經有足夠的相對功能來取代它，但Fetch並不止於此，它還提供更有效率與更多擴充性的作法。

### Fetch基本語法

`fetch()`方法是一個位於全域window物件的方法，它會被用來執行發出Request(要求)的工作，如果成功得到回應的話，它會回傳一個解決(resolves)為Response物件的Promise。`fetch()`的語法完全是Promise的語法，十分清爽容易閱讀，它看起來也很類似jQuery的語法:

```js
fetch('http://abc.com/url', {method: 'get'})
.then(function(response) {
    //handle response
}).catch(function(err) {
	// Error :(
})
```

但要注意的是fetch在只要有回應都視為Promise已實現的狀態(網路連線正常，伺服器有回應)，這其中也包含狀態碼為錯誤碼(404, 500...)的時候，所以一般的用法還需要加一下檢查:

```js
fetch(request).then(response => {
  //ok is trun when status in the range 200-299
  if (!response.ok) throw new Error(response.statusText)
  return response.json()
}).catch(function(err) {
	// Error :(
})
```

或是先用另一個函式先用Promise.resolve與Promise.reject包裝為為Promise物件:

```js
var processStatus = function (response) {
    // status "0" to handle local files fetching (e.g. Cordova/Phonegap etc.)
    if (response.status === 200 || response.status === 0) {
        return Promise.resolve(response)
    } else {
        return Promise.reject(new Error(response.statusText))
    }
};

fetch(url)
    .then(processStatus)
    // the following code added for example only
    .then()
    .catch();
```

### Fetch相關介面說明

fetch的核心由GlobalFetch、Request、Response與Headers四個介面(物件)與一個Body Mixin(混合)。大概的內容說明如下:

- GlobalFetch: 提供全域的fetch方法
- Request: 要求，其中包含method、url、headers、context、body等眾多屬性與方法
- Response: 回應，其中包含headers、ok、status、statusText、type、body等眾多屬性與方法
- Headers: 主要是讓開發者執行Request與Response中所包含的headers的各種動作，例如取回、增加、移除、檢查等等。設計這個介面的原因有一部份是為了安全性。
- Body: 同時在Request與Response中實作，包含主體內容的物件

與XHR有很大的明顯不同，每個XHR物件都是一個獨立的物件，麻煩的是每次作不同的Request(要求)或要處理不同的Response(回應)時，就得再重新實體化一個新的XHR物件，然後再設定一次。而fetch中則是可以明確地設定不同的Request(要求)或Response(回應)物件，提供了更細部設定的彈性，這些設定過的物件都可以重覆再使用。Request(要求)物件可以直接作為fetch方法的傳入參數，例如下面的這個範例:

```js
const req = new Request(URL, {method: 'GET', cache: 'reload'})

fetch(req).then(function(response) {
  //handle response
}).catch(function(err) {
	// Error :(
})
```

另一個很棒的功能是你可以用原有的Request(要求)物件，當作其他要新增的Request(要求)物件的基本樣版，像下面範例中的新的`postReq`即是把原有的`req`物件的method改為'POST'而已，這可以很容易重覆使用原先設定好的Request(要求)物件。

```js
const postReq = new Request(req, {method: 'POST'})
```

以下摘要Request(要求)物件中可以包含的屬性值，可以看到設定值相當的多選擇，可以依情況設定到很細:

- method: `GET`, `POST`, `PUT`, `DELETE`, `HEAD`。
- url: 要求的網址
- headers: 與要求相關的Headers物件
- referrer - `no-referrer`, `client`或一個網址。預設為`client`。
- mode - `cors`, `no-cors`, `same-origin`, `navigate`。預設為`cors`。Chrome(v47~)目前的預設值是`same-origin`。
- credentials - `omit`, `same-origin`, `include`。預設為`omit`。Chrome(v47~)目前的預設值是`include`。
- redirect - `follow`, `error`, `manual`。Chrome(v47~)目前的預設值是`。manual`。
- integrity - Subresource Integrity(子資源完整性, SRI)的值
- cache - `default`, `no-store`, `reload`, `no-cache`, 或 `force-cache`
- body: 要加到要求中的內容。使用method為`GET`或`HEAD`時不使用這個值。

Request(要求)物件中可以包含headers屬性，它是一個以Headers()建構式進行實體化的物件，實體化後可以再使用其中的方法進行設定。例如以下的範例:

```js
const httpHeaders = { 'Content-Type' : 'image/jpeg', 'Accept-Charset' : 'utf-8', 'X-My-Custom-Header' : 'fetch are cool' }
const myHeaders = new Headers(httpHeaders)

const req = new Request(URL, {headers: headers})
```

```js
const headers = new Headers()
headers.append('Accept', 'application/json')

const req = new Request(URL, {headers: headers})
```

> 註: Headers()建構式的傳入參數可以是其他的Headers物件，或是內含符合[HTTP headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)的位元組字串的物件。

> 註: Headers物件中還有一個很特殊的屬性guard，它與安全性有關，請參考[Basic_concepts#Guard](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Basic_concepts#Guard)

當然fetch方法也可以不需要一定得要傳入Request(要求)實體物件，它可以直接使用相同結構的物件字面當作傳入參數，例如以下的範例:

```js
fetch('http://abc.com/url', {
	method: 'POST',
	mode: 'cors',
	redirect: 'follow',
	headers: new Headers({
		'Content-Type': 'text/plain'
	})
}).then(function() { /* handle response */ })
```

fetch的語法連鎖下一個`.then`方法會得到一個Response(回應)物件。雖然在fetch API中也允許你自己建立一個Response(回應)物件實體，不過，Response(回應)物件通常都是從外部資源要求得到，要自訂Response(回應)物件大概只會在特殊的情況，在此就不討論。

Response(回應)物件中包含的屬性摘要如下:

- type: basic, cors
- url: 回應網址
- useFinalURL: 布林值，代表這個網址是否為最後的網址(也可能是重新導向的網址)
- status: 狀態碼 (例如: 200, 404, 500...)
- ok: 代表成功的狀態碼 (狀態碼介於200-299)
- statusText: 狀態碼的文字 (例如: OK)
- headers: 與回應相關的Headers物件

由於Response(回應)實作了Body介面(物件)，可以由Body的方法來取得回應回來的內容，依照不同的內容資料類型都有對應的方法，其中最常使用的是json與text方法:

- arrayBuffer()
- blob()
- formData()
- json()
- text()

> 註: arrayBuffer請參考[ArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)
>
> 註: blob請參考[Blob](https://developer.mozilla.org/en-US/docs/Web/API/Blob)

不過要特別注意，Body實體的在Request(要求)與Response(回應)中的設計是"**只要讀取過就不能再使用**"，其中都有一個`bodyUsed`屬性在讀取過會變成`true`，代表不能再被重覆使用。所以，如果要重覆使用Body物件，必須在被讀取前(`bodyUsed`屬性被設定為`true`前)，先呼叫Request(要求)或Response(回應)物件中的clone方法。

以下分別由幾種不同的回應資料類型來撰寫的樣式。

#### 純文字/HTML格式文字

```js
fetch('/next/page')
  .then(function(response) {
    return response.text()
  }).then(function(text) {
  	console.log(text)
  }).catch(function(err) {
  	// Error :(
  })
```

#### json格式

json方法會回傳一個帶有包含JSON資料的物件值的Promise已實現物件。

```js
fetch('https://davidwalsh.name/demo/arsenal.json').then(function(response) {
	// 直接轉成JSON格式
	return response.json()
}).then(function(j) {
	// `j`會是一個JavaScript物件
	console.log(j)
}).catch(function(err) {
  // Error :(
})
```

#### blob(檔案的原始資料)

```js
fetch('https://davidwalsh.name/flowers.jpg')
	.then(function(response) {
	  return response.blob();
	})
	.then(function(imageBlob) {
	  document.querySelector('img').src = URL.createObjectURL(imageBlob);
	});
```

> 註: URL也是一個的Web API，在新式的瀏覽器上都有支援。請參考[URL.createObjectURL](https://developer.mozilla.org/en-US/docs/Web/API/URL/createObjectURL)

#### FormData

FormData大部份是使用在傳送表單資料時使用。以下為範例:

```js
fetch('https://davidwalsh.name/submit', {
	method: 'post',
	body: new FormData(document.getElementById('comment-form'))
});
```

也可以直接使用JSON格式的物件資料:

```js
fetch('https://davidwalsh.name/submit-json', {
	method: 'post',
	body: JSON.stringify({
		email: document.getElementById('email').value,
		answer: document.getElementById('answer').value
	})
});
```

> 註: FormData介面包含在XMLHttpRequest的新標準之中，目前只有Chrome與Firefox支援，請參考[FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData)

### 相較於jQuery.ajax

jQuery中的ajax方法與相關方法的設計，已經很接近fetch的語法結構，不過它的回傳值仍然只是XHR物件的擴充物件jqXHR，需要經過轉換才能成為ES6的Promise物件。除此之外，有兩個重要的不同之處需要注意，來自[window.fetch polyfill](https://github.com/github/fetch#caveats):

- `fetch`方法回傳的Promise物件不會在有收到Response(回應)，但是是在HTTP錯誤狀態碼(例如404、500)的時候變成已拒絕(rejected)狀態。也就是說，它只會在網路出現問題或是被阻止進行Request(要求)時，才會變成已拒絕(rejected)狀態，其他都是已實現(fulfilled)。

- `fetch`方法預設是不會傳送任何的証書(credentials)，例如cookie到伺服器上的，這有可能會造成有管理使用者連線階段(session)的伺服器因此視為未經認証的Request(要求)。要加上傳送cookie可以用`fetch(url, {credentials: 'include'})`的語法來設置。

## 問題點

### 要求中斷或是設定timeout

它沒有辦法像XHR可以中斷要求、或是設定timeout屬性，請參考[Add timeout option #20](https://github.com/whatwg/fetch/issues/20)的討論。這個部落格文章[JavaScript Fetch API in action](https://blog.hospodarets.com/fetch_in_action)有提供一個用Promise物包裝的暫時解決方式，部份程式碼如下:

```js
var MAX_WAITING_TIME = 5000;// in ms

var timeoutId = setTimeout(function () {
    wrappedFetch.reject(new Error('Load timeout for resource: ' + params.url));// reject on timeout
}, MAX_WAITING_TIME);

return wrappedFetch.promise// getting clear promise from wrapped
    .then(function (response) {
        clearTimeout(timeoutId);
        return response;
    });
```

### 進程事件(Progress events)

沒辦法觀察目前傳輸狀態。[Fetch標準](https://fetch.spec.whatwg.org/#fetch-api)中有提供一個簡單的範例，但並不是很好的作法，程式碼如下:

```js
function consume(reader) {
  var total = 0
  return pump()
  function pump() {
    return reader.read().then(({done, value}) => {
      if (done) {
        return
      }
      total += value.byteLength
      log(`received ${value.byteLength} bytes (${total} bytes in total)`)
      return pump()
    })
  }
}

fetch("/music/pk/altes-kamuffel.flac")
  .then(res => consume(res.body.getReader()))
  .then(() => log("consumed the entire body without keeping the whole thing in memory!"))
  .catch(e => log("something went wrong: " + e))
```

## 結論

Fetch在瀏覽器的實作與XHR不同，內容與API也相差很多，所以說包著糖衣式的XHR畢竟與Fetch的內容還是差太多了。而且，現在在Chrome瀏覽器中，在Service Worker技術中也提供了Fetch方法，但只限制在Service Worker中使用。

## 其他: AJAX與Fetch函式庫比較表

本表主要參考自[AJAX/HTTP Library Comparison](http://andrewhfarmer.com/ajax-libraries/)。

| 名稱 | 星  | 瀏覽器  | Node  | Promise  | 原生  | 最後更新 |
|---|---|---|---|---|---|---|---|
| [XMLHttpRequest]()  | -  | 全部  | - | - | 是  | - |  
| [Node HTTP]() | - | - | 是  | - | 是 | - |  
| [fetch]() |  - | [部份](http://caniuse.com/#feat=fetch) | - | 是 | 是* | - |  
| [window.fetch polyfill](https://github.com/github/fetch)  | 9269 | 全部 | - | 是 | - | 2016/5 |
| [node-fetch](https://github.com/bitinn/node-fetch) | 894 | - | 是 | 是 | -  | 2016/5 |
| [isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch)  | 2587  | 是 | 是  | 是  | -  | 2015/11 |
| [axios](https://github.com/mzabriskie/axios) | 2035  | 是 | 是  | 是  | -  | 2016/7  |
| [SuperAgent](https://github.com/visionmedia/superagent) | 8175 | 是 | 是 | 是 | -  | 2016/7  |
| [jQuery](https://github.com/jquery/jquery)  | 40718 | 是  | - | 是* | - | 2016/7 |

註記:
 1. 以上瀏覽器是指目前各瀏覽器品牌新版本而言。
 2. isomorphic-fetch是混合window.fetch polyfill(whatwg-fetch模組)與node-fetch的專案。
 3. Promise為ES6新特性，[瀏覽器支援一覽表](http://caniuse.com/#feat=promises)。需要另外填充時建議使用[es6-promise](https://github.com/stefanpenner/es6-promise)。
 4. jQuery並非專門用於AJAX的函式庫，3.0版本後支援Promise標準。

## 參考資料

### 標準

- [Fetch Standard](https://github.com/github/fetch)
- [Fetch Standard](https://fetch.spec.whatwg.org/#fetch-api)
- [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)

### 教學

- [Introduction to the Fetch API](https://www.sitepoint.com/introduction-to-the-fetch-api/)
- [深入浅出Fetch API](http://wwsun.github.io/posts/fetch-api-intro.html)
- [Basic Fetch Request](https://developers.google.com/web/updates/2015/03/introduction-to-fetch)
- [That's so fetch!](https://jakearchibald.com/2015/thats-so-fetch/)
- [fetch API](https://davidwalsh.name/fetch)
- [JavaScript Fetch API in action](https://blog.hospodarets.com/fetch_in_action)
- [Handling Failed HTTP Responses With fetch()](https://www.tjvantoll.com/2015/09/13/fetch-and-errors/)

### XHR&相關協定

- [What is an Internet Protocol?](https://iamgopikrishna.wordpress.com/2014/06/13/4/)
- [XMLHttpRequest](https://hpbn.co/xmlhttprequest/)
