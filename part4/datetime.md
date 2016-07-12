# 日期與時間

Date在JavaScript中是一個內建的特殊物件，Date(日期)的資料型態應該是指Datetime(日期時間)而言，所以不只包含我們所認知的年月日而已，它也包含時間中的時、分、秒與微秒。

Date物件複雜的地方不只是它的進位並非十進位而已，年月日的進位還涉及閏年的問題，星期幾的問題，本地化格式的問題，以及時區的等等各種問題。

所以Date物件有時是個很難處理的東西，在複雜的應用情況時，我們需要藉助其他的工具函式庫來協助，例如這一套很常使用的函式庫 - [Moment.js](http://momentjs.com/)，不過它並不在本書的範圍之中，有需要可以再參考。

Date物件中包含了許多對日期時間處理方法，大致上可以分成以下幾類:

- getter: 獲取某種時間格式，例如getFullYear是回傳西元年
- setter: 設定某種時間格式，例如setFullYear設定西元年
- 格式化的getter: 通常是要獲取不同地區的日期(或時間)格式，以及轉換成各種資料格式

> 註: 微秒(Millisecond, ms)是千分之一秒的意思

## Date物件的建構式

Date物件一定只能使用new運算符來建立，這是JavaScript中不變的規則，有一些內建的物件一定要使用建構式進行實體化。

Date物件有四種可在建構式傳入參數的類型，如以下的語法說明，其中第三種的dateString指的是遵守國際日期與時間標準[RFC2822](https://tools.ietf.org/html/rfc2822#section-3.3)或[ISO 8601](https://zh.wikipedia.org/wiki/ISO_8601)兩種的字串格式，後面再說明其中的內容。

```js
const date = new Date()
const date = new Date(milliseconds)
const date = new Date(dateString)
const date = new Date(year, month, day, hours, minutes, seconds, milliseconds)
```

以下就每種建立Date物件的應用情況分別說明，從範例中來理解它的用法。

### 取得目前日期時間

`new Date()`不加任何參數，即使用目前瀏覽器獲得的日期時間格式為參數，也就是目前的日期時間，輸出時會使用國際日期標準的格式輸出，相等於呼叫`toString()`方法，注意它仍然是個物件類型，只是看不出來它的裡面的結構是什麼而已:

```js
const datetime = new Date()
console.log(datetime) //Tue Jul 12 2016 11:35:48 GMT+0800 (CST)
console.log(datetime.toString()) //Tue Jul 12 2016 11:35:48 GMT+0800 (CST)
console.log(datetime.toUTCString()) //Tue, 12 Jul 2016 03:35:48 GMT
console.log(datetime.toTimeString()) //11:35:48 GMT+0800 (CST)
```

另一種要用於計算時間用的格式，則是把Date物件轉成只用微秒作數值的格式，它是一個由`1 January 1970 00:00:00 UTC`開始計算，到目前日期時間的微秒值。這個時間稱之為[UNIX時間](https://zh.wikipedia.org/wiki/UNIX%E6%97%B6%E9%97%B4)，原本是UNIX系統使用來表示時間的方式，現在很常用於程式設計中計算時間之用。有兩種方式可以獲得這個數字值:

```js
//常用，正號(+)是強制轉換為數字的語法
const timeInMs1 = +new Date()

//靜態方法，不常見
const timeInMs2 = Date.now()
```

> 註: 使用Date物件來測量JavaScript應用程式(或語句)的執行效率並不夠可靠與精確，你可以使用[performance.now](https://developer.mozilla.org/en-US/docs/Web/API/Performance/now)或[benchmark.js](https://github.com/bestiejs/benchmark.js)函式庫。

### 取得日期時間其中某個值

### 日期時間字串

### 格式化與本地化

### 日期比較

## 實例

### 時鐘

### 日曆

http://www.eion.com.tw/Blogger/?Pid=1148
https://www.sitepoint.com/beginners-guide-to-javascript-date-and-time/
http://www.w3school.com.cn/js/js_obj_date.asp
http://stackoverflow.com/questions/3552461/how-to-format-a-javascript-date

