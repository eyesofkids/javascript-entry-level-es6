# 事件處理

JavaScript是一個以事件驅動(Event-driven)的程式語言。事件驅動程式設計的主要流程，是由圖形化使用者操作介面(UI)的互動事件為主要核心，藉由事件的觸發動作(滑鼠點按、鍵盤輸入等等)或是感應器的訊息，來啟動整體的程式流程。

依據"異步程式設計與事件迴圈"一章的內容中的說明，在JavaScript中有一個不斷偵測事件發生的事件迴圈(Event Loop)，所有在網頁上的DOM元素註冊的事件，都會進入一個佇列(queue)中，等待被觸發事件，然後作出相對的回傳送還給使用者。

我們在這裡所稱的事件(Event)，通常我們把它稱為瀏覽器事件(browser events)，並不是在ECMAScript標準中所制定的，而是網頁中的DOM(Document Object Model)事件，它的制定者是W3C組織，也有一些特定的事件是由瀏覽器品牌自行制定。

最後，對於近年來實現了伺服器端的JavaScript語言執行環境的Node.js，它並沒有這一系列直接可以使用的事件，內建的事件模型是Node.js自行設計的，不過它採用了相似的程式樣式，使用一個EventEmitter類別的物件實體來作事件處理工作，在底層也使用事件迴圈(Event Loop)的設計來達成。不過，你如果要進入伺服器端的事件處理領域，建議先從熟悉瀏覽器端的事件先著手，你會發覺它們有很多相似的設計與樣式。

## DOM事件

DOM(Document Object Model, 文件物件模型)是由W3C組織所制定的跨平台與跨程式語言的應用程式介面，它是把HTML、XHTML、XML文件的視為樹狀的結構，每個節點都是代表文件的一個物件。DOM的標準大戰可以從第一次瀏覽器大戰，由Netscape與微軟IE開始算起，到2015年已經是第4級(Level 4)的標準。

DOM事件則是可以註冊各種事件處理器/監聽器(event handlers/listeners)在DOM的節點元素上，在DOM標準的第2級(Level 2)時，W3C標準化了DOM中的事件模型，統一不同瀏覽器中的事件模型差異。這個標準也就是我們現在在瀏覽器上使用的事件模型基準。

JavaScript程式語言一直以來對瀏覽器中的物件模型(object model)與其事件模型，有非常高的支援性，雖然DOM標準從來就不是制定只專門給JavaScript一種語言使用的，不過因為JavaScript的使用率與支援程度太高了，現在也差不多就是制定給它用的。

那麼，什麼樣的DOM元素中會對應什麼樣的DOM事件，或是什麼樣的操作方式會觸發怎麼樣的事件？

這都由DOM中的標準來制定，例如像這個[DOM事件列表](http://www.w3schools.com/jsref/dom_obj_event.asp)非常的長，從滑鼠、鍵盤輸入到HTML表單…等等，近年來由於觸碰式的行動裝置很流行，W3C也開始制定[觸碰事件(Touch Events)](https://dvcs.w3.org/hg/webevents/raw-file/tip/touchevents.html)的標準。不過，有幾個瀏覽器品牌自己實作了不少非標準的事件，有些事件是只用於該瀏覽器的特殊用途。

註: [DOM events (維基百科)](https://en.wikipedia.org/wiki/DOM_events)

## Event物件(介面)

The Event object provides a lot of information about a particular event, including information about target element, key pressed, mouse button pressed, mouse position, etc. Unfortunately, there are very serious browser incompatibilities in this area. Hence only the W3C Event object is discussed in this article.

## EventTarget物件(介面)

The Event object provides a lot of information about a particular event, including information about target element, key pressed, mouse button pressed, mouse position, etc. Unfortunately, there are very serious browser incompatibilities in this area. Hence only the W3C Event object is discussed in this article.

## 事件處理模型

現行的事件處理模型通常會使用"監聽"(listen)的方式，作為事件處理的樣式，因為標準制定歷史版本不同，實際上有好幾種不同的方法可以作事件處理。以下分述這幾級的差異，其中第一種與第二種是舊的方式，不建議使用。

### 內聯模型

這種方式是最簡單的，也稱為內聯模型(Inline model)。它直接在HTML裡DOM元素中標記中使用，每個元素都會實作對應的可使用事件，名稱都會是像"onxxxxx"這樣的全小寫字詞，例如按鈕會實作onclick的事件屬性(attribute)，就在這裡面寫上事件處理的程式碼:

```html
<button onclick="console.log('hello!');"> Say Hello! </button>
```

JavaScript引擎中會產生一個對應的匿名函式，包含在onclick中的語句。這個方式也是最不建議的方式，它完全不像個用JavaScript語言寫的應用程式，也因為需要直接寫在HTML中，完全沒有彈性可言。

### 傳統模型

傳統模型(Traditional model)這個方式提供了分離HTML與JavaScript程式碼的語法，它比之前內聯模型的方式好得多了。不過它依然有個大問題，就是它只能在一個元素上使用一個事件。所以也不建議使用。

```js
document.getElementById('myButton').onclick = function(){
    console.log('hello!')
}
```

### DOM Level 2

這個方式才是稱為事件監聽(Event Listen)的方式，使用callback(回調)函式，作為事件的監聽者(或稱之為事件處理函式)。

```js
const el = document.getElementById('myButton')

el.addEventListener( 'click', function(){
     console.log('hello!')
}, false)
```

事件監聽的方式可以對一個元件附加多個事件處理函式，而且以標準來說基本有定義三種方法可使用:

- addEventListener: 在事件對象上加入事件監聽者
- removeEventListener: 從事件對象移除事件監聽者
- dispatchEvent: 送出事件給所有有訂閱的監聽者

> 註: 微軟IE瀏覽器在舊版本中使用自己定義的事件處理方法，所以要與舊版本相容時需要特別注意。IE9之後就能使用上述的事件監聽方式。

## 同步事件

http://javascript.info/tutorial/events-and-timing-depth#synchronous-events

## 事件觸發的順序

在同一個元素上註冊多個不同的事件監聽者(事件處理函式)，它的在事件觸發時的執行順序是怎麼樣子的？是按照程式碼所寫的順序嗎？

由於W3C的DOM第2代標準中並沒有明確的說明順序這件事，所以當如果有多個事件處理函式在同一元素中註冊時，它們的執行順序將會由瀏覽器來決定，有些舊的瀏覽器並不是按照程式碼中的順序執行。

在DOM的第3代標準也已經明定順序由註冊到事件目標的監聽者順序決定，而且現在常見的瀏覽器品牌與新版本，都會依照程式碼中的順序為執行的順序。

## 事件的泡泡、捕捉

上面所說的這個順序還會涉及到更複雜的一種，就是DOM元素的樹狀結構，它是有父母-子女關係(parent-child)，那麼如果在父母層的事件監聽，在子女層也會被同時監聽嗎？或是反過來也可以作得到嗎？這個時候的事件處理函式的順序又是如何？

http://www.quirksmode.org/js/events_order.html

http://stackoverflow.com/questions/9512551/the-order-of-multiple-event-listeners

http://www.quirksmode.org/js/events_advanced.html

## 事件監聽者的this


https://www.w3.org/TR/DOM-Level-3-Events/#event-flow

https://www.w3.org/TR/DOM-Level-3-Events/#sync-async

http://stackoverflow.com/questions/7398290/unable-to-understand-usecapture-attribute-in-addeventlistener

http://javascript.info/tutorial/bubbling-and-capturing

http://blog.bitovi.com/a-crash-course-in-how-dom-events-work/

https://w3c.github.io/uievents/#ui-events-intro

http://www.quirksmode.org/js/this.html
http://d.hatena.ne.jp/ynakajima/20090207/p1