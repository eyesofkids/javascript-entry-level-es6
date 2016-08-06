# callback回調函式

## callback是什麼？

中文翻譯字詞會用"回呼""、"回叫"、"回調"，都是聽起來很怪異的講法，callback在英文是"call back"兩個單字的合體，你應該有聽過"call me back"的英文，實際情況大概是有客戶打來電話給你，可是你正在電話中，客戶會留話說請你等會有空時再"回電"給它，這裡的說法是指在電信公司裡的callback的涵意。而在程式開發上上，callback它的使用情境其實也是有類似的地方。

## CPS風格與直接風格

延續傳遞風格(Continuation-passing style, CPS)，它的對比是"直接風格(Direct style)"，這兩種是程式開發時所使用的風格，CPS早在1970年代就已經被提出來了。CPS用的是明確地移轉控制權到下一個函式中，也就是使用"延續函式"，一般稱它為"回調函式"或"回調(callback)"。這是一個可以作為傳入參數的函式，用於在目前的函式呼叫執行最後移交控制權，而不使用回傳值的方法。直接風格的控制權移交是不明確的，它是用回傳值的方式，然後進行到下一行程式碼或呼叫接下來的函式。下面以範例來說明會比較容易。

直接風格的範例如下，其實就是一般函式呼叫的方式，或是用回傳的方式:

```js
//直接風格
function func(x) {
  return x
}
```

CPS風格就不是這樣，它會用另一個函式作為函式中的傳入參數的樣式來撰寫程式，然後將本來應該要回傳的值(不一定只有一個)，傳給下一個延續函式，繼續下個函式的執行:

```js
//CPS風格
function func(x, cb) {
  cb(x)
}
```

以明確的程式流程的例子來說，假設現在要從資料庫獲取某個會員的資料，然後把裡面的大頭照片輸出。

用直接風格的寫法:

```js
function getAvatar(user){
   //...一些程式碼
   return avatar
}

function display(avatar){
  console.log(avatar)
}

const avatar = getAvatar('eddy')
display(avatar)   
```

用CPS風格的寫法，像下面這樣:

```js
function getAvatar(user, cb){
  //...一些程式碼
   cb(avatar)
}

function display(avatar){
  console.log(avatar)
}

getAvatar('eddy', display)
```
長久以來，直接風格的程式碼是最常見的，因為它容易被學習者與開發者理解，一個步驟接著一個步驟，伺服器端的設計通常也是這樣設計，主要是因為伺服器端使用多執行緒的執行方式，不論是網站伺服器還是資料庫伺服器，都是多執行緒的。CPS風格反而不易被理解，並沒有明顯的誘因讓程式設計師一定要用CPS風格，而且也不是所有的程式語言都能使用CPS風格。

CPS風格還有一些明顯的缺點:

- 相較於直接風格，在愈複雜的情況時，程式碼愈不易撰寫與理解
- 錯誤處理較為困難

JavaScript中會大量使用CPS風格，除了它本身可以使用這種風格外，其實是有原因:

- 以CPS風格來設計事件異步處理的模型
- 只有單執行緒，在瀏覽器端只有一個使用者，但事件或網路要求(AJAX)要求不能阻塞其他程式的進行，但這也僅限在這些特殊的情況。不過在伺服器端的執行情況都很嚴峻，要能同時讓多人連線使用，必需要達到不能阻塞I/O，才能與以多執行緒執行的伺服器一樣的執行

> 要有高階函式(High Order Function)的特性才能使用CPS風格，也就是可以把某個函式當作另一函式的傳入參數，也可以回傳函式。除了JavaScript語言外，常見的語言具有高階函式特性的有Python、Java、Ruby、Swift等。

## 複雜在何處

callback運用在瀏覽器端似乎並沒有想像中複雜，一個事件的處理範例大概會像下面這樣:

```js
const el = document.getElementById('myButton')

el.addEventListener( 'click', function(){
     console.log('hello!')
}, false)
```

你也可以把callback寫成另一個函式定義:

```js
function callback(){
     console.log('hello!')
}

const el = document.getElementById('myButton')

el.addEventListener('click', callback, false)
```

AJAX是另一個常使用的情況，內建的`XMLHttpRequest`物件的行為類似於事件監聽，而且都打包好了。實際上`XMLHttpRequest.onreadystatechange`這個屬性，就是`XMLHttpRequest`物件在處理事件的callback(回調)函式。只是從程式碼中你看不到`XMLHttpRequest`內部是怎麼運作的:

```js
var xhr = new XMLHttpRequest();

xhr.onreadystatechange = function() {
    if (xhr.readyState == XMLHttpRequest.DONE ) {
       if (xhr.status == 200) {
           document.getElementById('myDiv').innerHTML = xhr.responseText
       }
       else if (xhr.status == 400) {
          console.log('There was an error 400')
       }
       else {
           console.log('something else other than 200 was returned')
       }
    }
}

xhr.open('GET', 'ajax_info.txt', true)
xhr.send()
```

### 匿名函式、函式定義與函式呼叫的混合

第一個我認為它會複雜的原因是來自匿名函式、函式定義與函式呼叫的混合寫法。所以當在看程式碼時，你的腦袋很容易打結。

```js
function func(x, cb){
  cb(x)
}

func(123456, function(value){
  console.log(value)
})
```

這例子很簡單，要分作幾個部份來看:

這是一個完整的函式呼叫，也就是說它是一個完整的被執行的語句結構:

```js
func(123456, function(value){
  console.log(value)
})
```

但這個是什麼，這個是一個函式定義，而且是匿名函式，它是一個callback(回調)函式的定義，它代表了函式執行完後要作的下一件事:

```js
function(value){
  console.log(value)
}
```

所以整個語法是代表"**在函式呼叫時，要寫出下一個要執行的函式定義**"，這就是常見回調函式的語法樣式。當然，你可以另外用一個函式來寫得更清楚:

```js
function func(x, cb){
  cb(x)
}

function callback(value){
  console.log(value)
}

func(123456, callback)
```

不過，你可以發現幾件事情:

- callback的函式名稱不重要，可以用匿名函式取代
- callback因為是函式的定義，所以傳入參數的名稱叫什麼其實都可以
- callback其實有closure(閉包)的特性，它可以獲取到`func`中的傳入參數，以及裡面的定義的值

callback的最大優點，它給了程式開發者很大的彈性，允許可以自訂下一個要執行函式的內容，等於說它可以提供函式的擴充與重覆使用。

### 回調地獄(Callback Hell)

複雜的情況是在於CPS風格使用callback來移往下一個函式執行，當你開始撰寫一個接著一個執行的流程，也就是一個函式呼叫後接下一個時，就會看到"回調中回調"，也就是所謂的"回調地獄"的結構，像下面這樣的例子:

```js
step1(x, function(value1){
  //do something...
  step2(y, function(value2){
    //do something...
    step3(z, function(value3){
        //do something...
    })
  })
})
```

它的執行順序應該是`step1 -> step2 -> step3`沒錯，所以真正是這樣嗎？你可能忘了匿名函式(callback)也是一個函式。

1. `step1`執行後，value1已經有值，移往`function(value1)`
2. `function(value1)`執行到`step2`，`step2`執行到最後，value2已經有值，移往`function(value2)`
3. `function(value2)`執行到`step3`，`step3`執行到最後，value3已經有值，移往`function(value3)`
4. `function(value3)`執行完成

```
step1 -> function(value1) -> step2 -> function(value2) -> step3 -> function(value3)
```

那為何為不使用直接風格？而一定要用這麼複雜的程式流程結構。上面已經有講為什麼JavaScript中會大量的使用CPS的原因:

> 因為有些I/O或事件類的函式，用直接風格會造成阻塞

你可能會認為阻塞有這麼容易發生嗎？是的，在JavaScript中要"阻塞"太容易了，它是單執行緖執行的設計。一個比較長時間的程序執行就會造成阻塞，下面的`for`迴圈就會讓你的按鈕按下去沒反應，而且幾個訊息都要一段時間執行完才會顯示出來:

```js
const el = document.getElementById('myButton')

el.addEventListener( 'click', function(){
     alert('hello!')
}, false)

const aArray = []
for(let i=0; i< 100000000;i++){
  aArray[i] = i+10
}

console.log('aArray done!')

const bArray = []
for(let i=0; i< 100000000;i++){
  bArray[i] = i*10
}

console.log('bArray done!')
```

在主緒行緒阻塞是一個嚴重的情況，異步執行的回調函式並不代表異步執行後就不會阻塞，也有可能從異步執行的佇列回到主緒行緒後，因為需要CPU密集型的運算，仍然會阻塞到緒行緒的進行。異步執行的回調函式，是JavaScript為了在只有單執行緒的情況，用來達成並行(concurrency)模型的一種設計。

> 如果要配合JavaScript的異步處理流程，也就是非阻塞的I/O處理，只有CPS可以，直接風格會造成阻塞

Node.js使用了CPS作為主要的I/O處理語法，老實說是一個不得已的選擇，它的樣式是使用error-first(以錯誤為主)的CPS風格，因為callback要處理錯誤不容易，所以要優先處理錯誤，它的主要原則如下:

- callback的第一個參數保留給Error(錯誤)物件，當錯誤發生時，它將會以第一個參數回傳。
- callback的第2個參數保留給成功回應的資料。當沒有錯誤發生時，error(即第一個參數)會設定為null，然後將成功回應的資料傳入第二個參數。

```js
var fs = require('fs');

fs.readFile('foo.txt', 'utf8', function(err, data) {
   if(err) {
    console.log('Unknown Error');
    return;
  }
  console.log(data);
});
```

Node.js使用CPS風格很容易出現回調地獄的問題，這是因為在伺服器端的各種I/O處理相當頻繁而且流程愈來愈複雜。相反的，過去很少人會討論在瀏覽器(客戶端)的回調地獄的問題。所幸現在已經有很多協助處理的方式，回調地獄可以用例如Promise、generator、async/await之類的語法結構，或是[Async](https://github.com/caolan/async)、[co](https://github.com/tj/co)外部函式庫等等，來協助在流程處理上更容易。

此外，隨著技術不斷的進步，現在的JavaScript已經有可以讓它使用其他執行緒的技術，有[Web Worker](https://developer.mozilla.org/zh-TW/docs/Web/API/Web_Workers_API/Using_web_workers)，或是專門給Node.js使用的child_process模組與cluster(叢集)模組。

## 異步回調函式

我需要再次強調，並非所有的使用callbacks(回調)函式的API都是異步執行的。在JavaScript中，除了DOM事件處理中的回調函式9成9都是異步執行的，只有少部份的API的的回調函式是異步執行的，要讓callbacks(回調)的執行轉變為異步，有以下幾種方式:

- 使用計時器(timer)函式: `setTimeout`, `setInterval`
- 特殊的函式: `nextTick`, `setImmediate`
- 執行I/O: 監聽網路、資料庫查詢或讀寫外部資源
- 訂閱事件

針對callbacks(回調)函式來說，異步與同步的執行到底是差在那裡？你可能會產生疑惑。下面用個簡單的例子來說明。

```js
function aFunc(value, callback){
  callback(value)
}

function bFunc(value, callback){
  setTimeout(callback, 0, value)
}

function cb1(value){ console.log(value) }
function cb2(value){ console.log(value) }
function cb3(value){ console.log(value) }
function cb4(value){ console.log(value) }

aFunc(1, cb1)
bFunc(2, cb2)
aFunc(3, cb3)
bFunc(4, cb4)
```

`aFunc`是一個簡單的回調結構，`callback`回調函式被傳入後最後以`value`作為傳入參數執行。

`bFunc`函式則是包裹了一個`setTimeout`內建方法，它可以在一定時間(第二個參數)內執行第一個參數，也就是`setTimeout`會執行的回調函式，第三個參數是要加入到回調函式的傳入參數值。

`aFunc`一般的回調函式，只是傳入到函式中當作參數，然後最後執行而已，這種是同步執行的回調函式。

`bFunc`中使用了計時器API`setTimeout`會把傳入的回調函式進行異步執行，也就是先移到工作佇列中，等執行主執行緒的呼叫堆疊空了，在某個時間回到主執行緒再執行。所以即使它的時間設定為0秒，裡面的回調函式並不是立即執行，而是會暫緩執行。

最後的執行結果是`1 -> 3 -> 2 -> 4`，也就是說，所有的同步回調函式都執行完成了，才會開始依順序執行異步的回調函式。如果你在瀏覽器上測試這個程式，應該會明顯感受到，2與4的輸出時，會有點延遲的現象，這並不是你的瀏覽器或電腦的問題，這是因為不論你設定的`setTimeout`為0，它要回到主執行緒上執行仍然需要按照內部事件迴圈所設定的時間差，在某個時間點才會回來執行。

這個程式執行的流程，可以看[這個在loupe網站的流程模擬](http://latentflip.com/loupe/?code=ZnVuY3Rpb24gYUZ1bmModmFsdWUsIGNiKXsKICBjYih2YWx1ZSk7Cn0KCmZ1bmN0aW9uIGJGdW5jKHZhbHVlLCBjYil7CiAgc2V0VGltZW91dChjYiwgMCwgdmFsdWUpOwp9CgpmdW5jdGlvbiBjYjEodmFsdWUpeyBjb25zb2xlLmxvZyh2YWx1ZSk7IH0KZnVuY3Rpb24gY2IyKHZhbHVlKXsgY29uc29sZS5sb2codmFsdWUpOyB9CmZ1bmN0aW9uIGNiMyh2YWx1ZSl7IGNvbnNvbGUubG9nKHZhbHVlKTsgfQpmdW5jdGlvbiBjYjQodmFsdWUpeyBjb25zb2xlLmxvZyh2YWx1ZSk7IH0KCmFGdW5jKDEsIGNiMSk7CmJGdW5jKDIsIGNiMik7CmFGdW5jKDMsIGNiMyk7CmJGdW5jKDQsIGNiNCk7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)

由這個範例中，可以看到異步的回調程式執行並沒有比同步的回調程式快，反而是更慢了點，它只是一種因應特別情況所採用的函式執行方式，例如需要與外部資源存取(I/O)、DOM事件的處理或是計時器的情況。

> 關於函式的異步執行與事件迴圈資料，請參考[異步執行與事件迴圈]的章節

## 參考資源

http://matt.might.net/articles/by-example-continuation-passing-style/
http://www.2ality.com/2012/06/continuation-passing-style.html
http://stackoverflow.com/questions/14019341/whats-the-difference-between-a-continuation-and-a-callback
https://en.wikipedia.org/wiki/Continuation-passing_style
