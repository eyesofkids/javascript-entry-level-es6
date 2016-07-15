# 日期與時間

Date在JavaScript中是一個內建的特殊物件，Date(日期)的資料型態應該是指Datetime(日期時間)而言，所以不只包含我們所認知的年月日而已，它也包含時間中的時、分、秒與微秒。

Date物件複雜的地方不只是它的進位並非十進位而已，年月日的進位還涉及閏年的問題，星期幾的問題，本地化格式的問題，以及時區的等等各種問題。

所以Date物件有時是個很難處理的東西，在複雜的應用情況時，我們需要藉助其他的工具函式庫來協助，例如這一套很常使用的函式庫 - [Moment.js](http://momentjs.com/)，不過它並不在本書的範圍之中，有需要可以再參考。

Date物件中包含了許多對日期時間處理方法，大致上可以分成以下幾類:

- getter: 獲取某種時間格式，例如getFullYear是回傳西元年
- setter: 設定某種時間格式，例如setFullYear設定西元年
- 格式化的getter: 通常是要獲取不同地區的日期(或時間)格式，以及轉換成各種資料格式

> 註: 微秒(Millisecond, ms)是千分之一秒

## Date物件的建構式

Date物件一定只能使用`new`運算符來建立，這是JavaScript中的規則，有一些內建的物件一定要使用建構式進行實體化。

Date物件有四種可在建構式傳入參數的類型，如以下的語法說明，其中第三種的dateString指的是遵守通用的日期時間字串、國際標準[RFC2822](https://tools.ietf.org/html/rfc2822#section-3.3)或[ISO 8601](https://zh.wikipedia.org/wiki/ISO_8601)兩種的字串格式，後面再說明其中的內容。

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

另一種要用於計算時間用的格式，則是把Date物件轉成只用微秒作數值的格式，它是一個由`1 January 1970 00:00:00 UTC`開始計算，到目前日期時間的微秒數字值。這個時間稱之為[UNIX時間](https://zh.wikipedia.org/wiki/UNIX%E6%97%B6%E9%97%B4)，原本是UNIX系統使用來表示時間的方式，不過UNIX時間只有計算到秒，這個數字值是計算到微秒，現在很常用於程式設計中計算時間之用。有兩種方式可以獲得這個數字值:

```js
//常用，正號(+)是強制轉換為數字的語法
const timeInMs1 = +new Date()

//靜態方法，不常見
const timeInMs2 = Date.now()
```

> 註: 使用Date物件來測量JavaScript應用程式(或語句)的執行效率並不夠可靠與精確，你可以使用[performance.now](https://developer.mozilla.org/en-US/docs/Web/API/Performance/now)或[benchmark.js](https://github.com/bestiejs/benchmark.js)函式庫。

### 取得日期時間其中某個值

在實體化Date物件後，可以對Date物件取得包含在其中的某個值，例如年、月、日、星期、時分秒等等。我們先來看年月日與星期怎麼取得的範例:

```js
const dateObject = new Date() //Fri Jul 15 2016 16:23:49 GMT+0800 (CST)

const date = dateObject.getDate() //15
const day = dateObject.getDay()  //5
const month = dateObject.getMonth()  //6
const year = dateObject.getFullYear()  //2016
```

JavaScript中的Date物件的取值方法，說實在從名稱上會有些看不清楚，先看比較直覺而沒問題的部份:

- getDate: 取得日期的值，範圍為1-31
- getFullYear: 取得年(西元年)，為4位數數字

那麼我所說的不清楚部份是下面兩個:

- getDay: 取得星期幾的值，範例為0-6
- getgetMonth: 取得月份的值，範例為0-11

這兩個實際上是陣列索引值的概念，也就是說如果你要轉成真正的月份與星期的值，需要有一個對比的陣列，然後用這個索引值去轉換出來，會這樣設計也是因為這兩個值通常在每個地區或語言都有不同的字詞，所以保留給設計師自行處理。以下是月份的範例:

```js
const monthNamesEn = [
  'January', 'February', 'March', 'April', 'May', 'June',
  'July', 'August', 'September', 'October', 'November', 'December'
]

const monthNamesZh = [
  '一月', '二月', '三月', '四月', '五月', '六月',
  '七月', '八月', '九月', '十月', '十一月', '十二月'
]

console.log(monthNamesEn[month]) //July
console.log(monthNamesZh[month]) //七月
```

以下則是星期的範例，注意陣列的第一個(索引值為0)是指"星期日":

```js
const dayNamesEn = [ 'Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']
const dayNamesZh = [ '星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六']

console.log(dayNamesEn[day]) //Friday
console.log(dayNamesZh[day]) //星期五
```

時分秒與微秒的取得就很標準，以下為範例，後面註解說明它的回傳數字的範圍:

```js
const hour = dateObject.getHours() //0-24
const minute = dateObject.getMinutes() //0-59
const second = dateObject.getSeconds() //0-59
const ms = dateObject.getMilliseconds() //0-999
```

> 註: 取得年月日星期的方法為單數英文字詞，取得時分秒微秒的是複數英文字詞。

另外，Date物件有數個名稱中有UTC的取得方法，都是在獲取UTC(標準時間)的值。例如台灣的時間是UTC+8的時區，所有用這類方法取得的時間，有可能在年月日、小時，會有所不同。

至於取得時區的方法是`getTimezoneOffset`，它是用分鐘計算的值，而且是以目前的時區為基準，所以例如以下的範例會回傳`-480`:

```js
const timezoneByMins = dateObject.getTimezoneOffset() //-480

//除以60後正負相轉才是時區
const timezone = -(dateObject.getTimezoneOffset()/60)
```

> 註: 時區處理有可能很複雜，上面說到的[Moment.js](http://momentjs.com/)也有另一套專門處理時區用的工具函式庫。

### 設定日期時間 與 日期時間字串

在Date物件實體化時，給定傳入參數值可以直接設定這個Date物件內的內容，除了不給定內容的建構式會自動抓取目前的日期時間。其它兩種建構式就不多作說明了，本節的重點在於日期時間字串，

JavaScript中的日期格式有可能是個頭痛的問題，目前雖然已經是標準的一員。但這標準可不是一開始就有的，而是在ES5後訂定的，所以有些舊版的瀏覽器中並不一定可以這樣用，也有可能在這個瀏覽器可以這樣定義，到別的瀏覽器就不能用了。這裡有一份參考的[時間日期字串格式相容表](http://dygraphs.com/date-formats.html)。

#### 通用的格式

> 建議使用

這也不算什麼標準，只是這樣定義的話，在每種瀏覽器品牌或版本都可以相容使用。除非你有非不得已要用其他格式的理由，通常都建議只使用這種定義方式:

```js
2009/07/12
2009/7/12
2009/07/12 12:34
2009/07/12 12:34:56
```

把年放在最後面也是可以，不過這方式也是英文語言使用者常用的方式，不要搞混月份與日期的位置，第一個位置是"月份":

```js
07/02/2012
7/2/2012
7/2/2012 12:34
```

而直接在字串中加上微秒定義，會出現嚴重的不相容性，只有Chrome瀏覽器認得。所以解決方式就拆開所有的值，改用使用第四種的Date物件實體化，或是用設定值的方法`setMilliseconds`直接設定才行。

#### ISO 8601格式

> 注意: 舊版瀏覽器不相容

EMCAScript標準中定義的格式，實際上它就是ISO 8601標準，格式如下:

```
YYYY-MM-DDTHH:mm:ss.sssZ
```

YMD代表年月日，Hms代表是時分秒，這兩部份比較沒什麼太大的問題。

那麼`T`代表什麼？`T`只是分隔年月日與時分秒而已。

那麼`Z`代表什麼？`Z`是時區的意思，單純用Z代表是UTC標準時間，如果是其他時區的時間可以用`+`或`-`，再加上`HH:mm`作為時區的表達式，所以像下面的幾個日期時間字串是合於這個規定的:

```
2016-01-01 //年月日可以只有年、年月
2016-07-15T09:00  //時間的部份至少要有時與分
2016-07-15T09:26:23.216Z
2016-07-15T09:20:37.788+08:00
```

#### RFC 2822格式

另一種格式則是RFC2822格式，它並不是一種專門用於定義日期時間格式的標準，定義的標題名稱是"Internet Message Format"(網際網路訊息格式)，主要是用於定義Email的相關格式的標準，而其中有一段是日期時間格式的章節。這種日期時間格式，主要的使用群是英文使用者，原因它使用了英文中的月份與星期縮寫在字串上，以下為格式範例:

```
ddd, DD MMM YYYY HH:mm:ss ZZ
MMM DD, YYYY
DD MMM, YYYY
```

以下是幾個範例:

```
Mon, 15 Aug 2005 15:52:01 +0000
Thu, 18 Feb 2016 15:33:10 +0200
Jan 1, 2015
1 Jan, 2015
```

由於月份與星期都已經字串化(不是數字)，所以你可能會看到有很多種變型的寫法。RFC2822標準並沒有明確指出格式的限制，程式語言都可以很容易實作與解析這種日期字串。在MSDN中的說明，把它歸類為其他格式之中，與其他的日期格式使用相同的規則來作解析處理。

> 注意: 這段的內容只是說明RFC2822與ISO 8601格式定義日期時間的用法，一般情況下你只需要使用第一種的定義方式即可。

> 註: 你可能不太明白為何在設定日期時間時，還要加上星期幾的意義何在。實際上這個格式通常是用來轉換到其他的格式之用。

#### 設定方法



### 格式化與本地化

### 日期比較

## 實例

### 時鐘

### 日曆

http://chitanda.me/2015/08/21/the-trivia-of-js-date-function/
http://www.eion.com.tw/Blogger/?Pid=1148
https://www.sitepoint.com/beginners-guide-to-javascript-date-and-time/
http://www.w3school.com.cn/js/js_obj_date.asp
http://stackoverflow.com/questions/3552461/how-to-format-a-javascript-date

