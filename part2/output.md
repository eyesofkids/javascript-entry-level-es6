# 輸出

JavaScript是搭配HTML使用的，最終輸出(呈現結果)的方式，可以用HTML的輸出格式作為輸出。不過在一開始學習時，或是在開發時簡單測試時，我們可以使用以下的語法在瀏覽器的除錯主控台中輸出數值:

```js
console.log('Hello')
console.log('變數abc的值', abc)
```

> 注意: console.log並非涵蓋所有瀏覽器版本與品牌的標準方法，有些比較舊版本的瀏覽器中(尤其是Microsoft Internet Explorer6、7)是不能使用的，會造成程式錯誤。

> 註: 更多資訊請參考Google Chrome中的[console API](https://developer.chrome.com/devtools/docs/console-api#consolelogobject-object)

## 其他的輸出方式

除了上述的`console.log`方式，還有以下的幾種輸出方式，以下簡單說明。

### alert

`alert`是一個存在已久的全域方法(屬於window物件)，它可以在很舊的瀏覽器品牌與版本中使用。用它可以跳一個極醜的警告視窗:

```js
//alert屬於window物件
window.alert('Hello')
//直接用alert也可以
alert('Hello')
```

除非你真的有需要在如Internet Explorer6、7上作相容性的測試或開發，不然這個`alert`可以略過不要用，它有很多缺點，建議不要再使用它。

> 注意: 在之前早期的年代，這個`alert`有很多濫用的情況，很容易造成瀏覽器當掉。請不要作一個偷懶的工程師，用這個來當跳出的提示或錯誤視窗。現在已經有很多取代的跳出視窗方案，不但美觀而且問題少。

### document.write

`document.write`是屬於`document`物件的一個寫入方法，它可以在HTML網頁上寫出輸出的結果。

```js
document.write('Hello')
```

它同樣不是一個好方法，也是一個存在已久的輸出方法。表面上雖然看起來就這樣用，但實際上並不是那麼簡單。它只建議用在測試時，真正使用上千萬不要使用。

> `document`相當於`window.document`，它是在`window`物件下的內容物件。

### innerHTML

`innerHTML`需要配合`document.getElementById(id)`方法使用，它是一個DOM節點的屬性值，可以動態進行指定，例如:

```js
document.getElementById("demo").innerHTML = 'Hello'
```

其中的`demo`要求是HTML中已經存在的DOM節點，例如下面這個HTML碼:

```html
<div id="demo"></div>
```

> 註: 這個方式是推薦的輸出方式，也是現今很多JavaScript函式庫用來輸出結果的方式。

### createTextNode

`createTextNode`是一個`document`物件中的方法，看它的名稱就知道用途是"建立文字的節點"。不過你要建立節點需要與其他的方法搭配才行，不是獨立使用的方法。例如:

```js
const newText = document.createTextNode('Hello')
const demo = document.getElementById("demo")
demo.appendChild(newText)
```

其中的`demo`要求是HTML中已經存在的DOM節點。不過，這個方法比`innerHTML`複雜些，而且效能更低，只會用在特別的情況，例如有需要作節點的處理時。

## 結語

不論是`innerHTML`、`alert`、`createTextNode`、`document.write`，因為輸出到網頁上的HTML碼中，所以必定會轉變為字串值。也就是不論這轉出的值原本是數字的2進位、8進位、16進位，就會變成10進位，而其他的類型的值也會自動轉成字串值。在開發時並不會用這樣的輸出方式，所以還是只有`console.log`這方式可用。