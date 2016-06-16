# 資料類型(值) Data Type

資料是所有電腦運算的基礎，對於電腦來說，所有的資料都只是0與1的訊號，但對於人類來說，資料類型的區分就複雜得多了，需要因應各種不同的情況來使用。

JavaScript有6種原始的原始資料類型(Primitive)，分別是字串、數字與布林值，以及null(空)與undefined(未定義)兩種特殊值，ES6中加入了符號(symbol)。

JavaScript中還有另一個非常重要的第7種資料類型 - Object(物件)。

我們常常會把屬於物件的Array(陣列)、Data(日期時間)、Function(函式)、RegExp(正規表述式)獨立出來說明，因為這些物件都有特殊的功能和作用，算是特別的物件類型。

總之，我們大致上的分類是像下面這樣，本章節也是以這個分類開始來介紹，陣列與物件將會再獨立為另一章節說明。至於新的資料類型symbol(符號)，它的使用情況會需要先了很多基礎知識，本書中並不會介紹。

主要的原始資料類型:

- 數字(Number)
- 字串(String)
- 布林值(Boolean)

特殊的原始資料類型:

- null(空)
- undefined(未定義)
- Symbol(符號)

複合型(composite)或參考型(reference)的資料類型:

- Array(陣列)
- Object(物件)

> 註: 所謂的"原始資料類型"，指的是在程式語言中，最低階的一種資料類型，不屬於物件也沒有方法。它也具有不可改變的(immutable)特性。不過，JavaScript語言中也存在名稱為String、Number、Boolean的物件，用來對應原始資料類型，它們是一種包裝物件(wrapper object)，提供了原始資料類型使用的一些延申的屬性與方法，真正會用到這些物件只有在特殊情況下。

> 註: 是的，JavaScript的確是一個物件導向的程式語言。不過，JavaScript的物件導向特性與其他目前所流行的物件導向語言例如Java、C++等有很大的不同。

## 鬆散資料類型

JavaScript語言是一個鬆散資料類型(loosely typed)或稱為動態的程式語言(dynamic language)。這代表你不需要為變數or常數在宣告時，就規定它的資料類型，取而代之的是只需要指定它的值。JavaScript會依照指定的值決定變數or常數的資料類型。

```
let foo = 42    // foo現在是Number資料類型
let foo = 'bar' // foo現在是String資料類型
let foo = true // foo現在是Boolean資料類型
```

## 判斷資料類型 - typeof

對於要如何判斷目前變數or常數的資料類型，最簡便的方式就是使用這個運算符`typeof`，它是個類似正號(+)或負號(-)的運算符，是個一元的運算符，所以是加在運算元的前面。最後會回傳一個是屬於那一種資料類型的字串，範例如下:

```js
console.log(typeof 37) //'number'
console.log(typeof NaN) //'number'
console.log(typeof '') //'string'
console.log(typeof (typeof 1)) //'string'
console.log(typeof true) //'boolean'
console.log(typeof null) //'object'
```

> 註: 為何null資料類型的的typeof結果是'object'(物件)，而不是'null'呢？依據[ECMAScript的標準章節11.4.3條](http://www.ecma-international.org/ecma-262/5.1/#sec-11.4.3)對`typeof`結果的規定，就是回傳'object'。目前有一些反對的聲音，認為null是原始資料類型，理應當回傳自己本身的資料類型。也有人認為現在修改這個太晚了，一改動的話會造成很多舊的程式被影響。不過截至ES6標準這一規定仍然沒有更動。

## 數字(Number)

JavaScript的數字(Number/難波/)類型必定是64位元的浮點數，類似於其他程式語言(如Java)的`double`/打波/ 資料類型。數字類型沒有如其他程式語言中，有獨立的整數(int)或浮點數(float)類型。在JavaScript中，`1`與`1.0`指的是相同的類型與值。此外，數字可以使用算術運算符(+-*/%)等來進行運算。以下是幾個宣告的範例:

```js
const intValue = 123
const floatValue = 10.01
const negValue= -5.5
```

電腦中的程式語言在數學上的運算，總會有處理上的極限與無法處理的情況。JavaScript使用三個特殊的記號，來代表在數字上處理的極限值或非數字的情況:

- +Infinity: 正無限值(相當於Infinity)
- -Infinity: 負無限值
- NaN: 代表不是數字

```js
console.log(1/0) //Infinity
console.log(1/-0) //-Infinity，0有分+0與-0，這有點算陷阱
console.log(Infinity - Infinity) //NaN
```

數字的最大與最小值，可以使用`Number.MAX_VALUE`與`Number.MIN_VALUE`得到。

```js
console.log(Number.MAX_VALUE)
console.log(Number.MIN_VALUE)
```

### 整數進位

以整數來說，一般的整數進位有2、8、16，以及最常使用的10進位。

2進位(Binary)在ES6之後可以使用`0b`或`0B`開頭來定義:

```js
const FLT_SIGNBIT  = 0b10000000000000000000000000000000 // 2147483648
const FLT_MANTISSA = 0B00000000011111111111111111111111 // 8388607
```

8進位(Octal)在ES6之後可以使用`0o`或`0O`開頭來定義:

```js
const n = 0O755 // 493
const m = 0o644 // 420
```

16進位(Hexadecimal)中直接可以用`0x`開頭來定義:

```js
const x = 0xFF
const y = 0xAA33BC   
```

在ES6之前，對於2或8進位並沒有內建的直接可定義方式，需要透過一個字串的方法`parseInt(string, radix)`，將一個2進位或8進位的數字字串轉換，這方式也可以轉換16進位的數字字串:

```js
//8進位
const octalNumber = parseInt('071',8)

//2進位
const binaryNumber = parseInt('0111',2)

//16進位
const hexNumber = parseInt('0xFF',16)
```

如果是新式的2、8進位定義格式的字串，需要用Number()方法才能轉為10進位，範例如下:

```js
const binaryNumber = Number('0b11')    // 3
const octalNumber = Number('0o11')    // 9
const hexNumber = Number('0x11')    // 17
```

> 註: 不論是2、8、16進位的數字，直接輸出到HTML碼中必定會自動轉成10進位輸出。

另一個情況是要將10進位數字以不同的進位基數(radix)轉為字串，通常是用在輸出時，這時要使用數字的方法 - `toString([radix])`

```js
const decimalNumber = 125
console.log(decimalNumber.toString())
console.log(decimalNumber.toString(2))
console.log(decimalNumber.toString(8))
console.log(decimalNumber.toString(16))
```

### 浮點數的轉換

#### 字串轉浮點數

對照上面的`parseInt`方法，字串也有另一個`parseFloat(string)`可以轉換數字字串為浮點數，不過就像最上面所說明的，數字1.0相當於1，對於JavaScript來說，數字就是數字。以下是範例:

```js
const aNumber = parseFloat("10")  //10
const bNumber = parseFloat("10.00") //10
const cNumber = parseFloat("10.33") //10.33
const dNumber = parseFloat("34 45 66") //34
const eNumber = parseFloat("40 years") //40
const fNumber = parseFloat("He was 40")  //NaN
```

####  浮點數轉整數

至於浮點數要轉換為整數，需要使用Math物件中的幾個方法來轉換，因為浮點數是要轉換為直接進位，還是四捨五入，還是直接去掉小數點，就看你需要的情況:

```js
const floatValue = 10.55
const intValueOne = Math.floor( floatValue ) //地板值 10
const intValueTwo = Math.ceil( floatValue ) //天花板值 11
const intValueThree = Math.round( floatValue ) //四捨五入值 11
```

> 註: Math是專門用於數學運算使用的JavaScript語言內建物件，裡面有很多好用的數學方法，請參考[Math(MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)與[JavaScript Math Object](http://www.w3schools.com/js/js_math.asp)

#### 整數轉浮點數

再強調一下，對於JavaScript來說，數字就是數字，沒有什麼浮點數或整數的類型。如果你強烈想要3.00而不是3的數字，這在JavaScript中來說都是一樣的，直接轉出數字3.00依然會用3來顯示。

唯一可能的情況，是希望調整輸出的格式。這時可以用數字物件中的`toFixed([digits])`方法來達成，不過它會回傳成字串，這已經不是數字了:

```js
const myString = (3).toFixed(2) //string, 3.00

const numObj = 12345.6789 
const numObjString = numObj.toFixed() //string, 123456
```

### 其他類型轉換為整數

#### 雙位元反相運算(Double Bitwise NOT)(\~\~)

波浪符號(Tilde)(~)，在JavaScript語言中的運算符名稱為位元反相運算(Bitwise NOT)，這是長得像波浪或毛毛蟲樣子的符號。根據它的功能文件說明，它會把"數字 x 轉換為 -(x + 1)"。也就是說像下面這樣的例子:

```js
const a = ~10 //a is -11
```

那如果是兩條毛毛蟲(Double Bitwise NOT)(\~\~)呢？上面的例子會變成如何:

```js
const b = ~~10 //b is 10
```

兩條毛毛蟲看起來沒什麼用，只是回復原本的數字值而已。不過它的功用是轉換其他類型為整數，而且它有相當於`parseInt`的功能，但並不是百分之百相等。在"正"數值情況下，由浮點數轉為整數時，也相當於`Math.floor()`。但重點是它的效能在某些瀏覽器非常快。所以有很多程式設計師會使用這樣的寫法。

```js
console.log(~~'-1')  // -1
console.log(~~'0')   // 0
console.log(~~'1')   // 1
console.log(~~true)  // 1
console.log(~~false) // 0
console.log(~~null) // 0
console.log(~~undefined) // 0
```

> 註: `parseInt`雖然主要是傳入字串，轉換為整數。但也有傳入浮點數，轉換為整數的功能。

#### 正號(+)

正號(Unary Plus)(+)也是一個一元的運算符，它也有轉換其他類型為數字的能力，不過它的功能接近`parseFloat`，但並不完全相等。負號(-)也有同樣的功用，但很少被使用。

```js
console.log(+'2.3') //2.3
console.log(+'0') //0
console.log(+'foo') //NaN
console.log(+true) //1
console.log(+false) //0
console.log(+null) //0
```

> 註: 這份網路上的[Stackoverflow問答](http://stackoverflow.com/questions/17106681/parseint-vs-unary-plus-when-to-use-which)中有一張表，列出所有轉換的情況。

### 數字的精確問題

數字並非是永無極限的，有最大的數值上限也有最小的下限。上面已有說明它的最大與最小值怎麼獲得，但在運算時，有時候會出現不如你想像的情況，這些都與浮點數的精確度(precision)，有關以下是幾個典型的範例，這些都算是陷阱題目:

#### 範例一：

注意: 第二行的值失去精確，它會變成另一個值

```js
console.log(999999999999999)
console.log(9999999999999999)
```

#### 範例二

注意: x, y都不是你想的結果

```js
const x = 0.2 + 0.1
const y = 0.3 - 0.1
```

> 註: 範例來自[Number, Math](http://JavaScript.info/tutorial/number-math#permissive-conversion-parseint-and-parsefloat)

因此，在處理浮點數時，要格外小心。如果你遇到了不可預期的結果，有可能是精確出了問題。如果你遇到需要處理浮點數的運算，可以尋找合適的函式庫。例如[BigDecimal.js](https://github.com/dtrebbien/BigDecimal.js)或[decimal.js](https://github.com/MikeMcl/decimal.js/)

## 字串(String)

字串類型用於描述一般的字串值，是使用相當廣泛的值。在JavaScript語言中，字串是使用Unicode作為編碼。例如：

```js
const aString = '你好'
const bString = 'Hello'
```

對於JavaScript語言來說，使用雙引號標記("")與單引號標記('')來定義字串值，結果都是一樣的。但推薦使用**單引號標記('')**。原因是JavaScript經常需要與HTML碼搭配使用，而HTML碼中也會使用引號來作為標記屬性值的定義，所以讓HTML屬性使用雙引號標記("")，而Javascript中的字串使用單引號標記('')，這個也變成一個約定俗成的撰寫習慣。

### 字串與字元

存取一個字串中的字元，可以用類似陣列的存取方式，或是用`charAt()`方法。不過，JavaScript中並沒有所謂的字元(Character)類型，所以它依然是個字串(String)資料類型，只是只有一個字元而已。

```js
const a = 'cat'.charAt(1)   //  'a'
const b = 'cat'[1]   // 'a'

console.log(typeof a)
```

> 註: 陣列資料類型的內容，會在後面的章節再介紹。

> 注意: 使用陣列的提取字串中的字元這種方式為不安全(unsafe)的方式，尤其在某些舊的瀏覽器品牌中不支援。

### 跳脫字元(escape characters)

當需要在雙引號記號("")中使用雙引號(")，或在單引號記號('')中使用單引號(')時，或在字串中使用一些特殊用途的字元。使用反斜線符號`\`來進行跳脫，這稱為跳脫字元或跳脫記號。例如：

```js
const aString = 'It\'s ok'
const bString = 'This is a blackslash \\'
```

> 註: 在JavaScript中，跳脫字元在雙引號記號("")與單引號記號('')中均可使用，這一點與其他程式語言有些不同。

### 樣版字串(Template strings)

ES6中新的字串寫法，稱為樣版字串(Template strings)，使用的是重音符號backtick(``)來敘述字串。樣版字串可以多行、加入字串變數，以及其他延申的用法。範例如下：

```js
const aString = `hello world`
const aString = `hello!

 world!`
```

在樣版字串中可以嵌入變數or常數，也可以作運算，嵌入的符號是使用錢號與花括號的組合(${}):

```js
const firstName = 'Eddy'
console.log(`Hello ${firstName}!
Do you want some
rabbits tonight?`)


const x = 5
console.log(`5 + 3= ${x + 3}`)

```

### 字串運算

字串中有很多運算用的函式與記號，以下是常用的幾個：

#### 字串串接

使用加號(+)或加法指定(+=)是最直覺的方式，效能也比`concat()`好，這兩種方式都是同樣的結果，範例如下：

```js
//使用concat()串接
const aString = 'JavaScript'
const bString = aString.concat(' is a', ' script language')
console.log(bString)

//使用(+=)串接
let cString = 'JavaScript'
cString += ' is a'
cString += ' script language'
console.log(cString)
```

> 註: 實際上還有一個join()方法可以串接字串，不過它是陣列的方法。

#### 與其他類型作加號(+)運算

不過，當你看到加號(+)時，你如果看到數字中也有運算用的加號(+)，可能會有個疑問，像下面這個程式碼，結果應該是什麼，到底會是101還是11？

```js
const a = '10' + 1
console.log(a)

const b = 10 + '1'
console.log(b)
```

依據[ECMAScript的標準章節11.6.1條](http://www.ecma-international.org/ecma-262/5.1/#sec-11.6.1)對加號(+)運算的規定，簡單說明如下:

> 當運算式的左邊運算元**或**右邊運算元，是字串類型時，則使用字串連接(concatenating)的運算，非字串類型的運算元則使用ToString轉換

所以上面的兩個輸出的結果會是101，沒問題吧。那更複雜的運算像下面的程式碼又會是什麼結果呢？

```js
console.log( 3 + 4 + '5' ) //75
console.log( 4 + 3 + '5' + 3 ) //753
```

所以，當你想要轉換某個類型為字串時，直接與一個空白字串作加號(+)運算，就會變成字串類型了。這也是當用來把其他類型轉為字串的最簡便的語法。

```js
const a = 6 + ''
const b = true + ''
```

> 註: 除了加號(+)運算外，其他的數字運算符號(-/*)都會將運算元轉換為數字。

#### 字串長度

`length`是字串的屬性值，可以讀取出字串目前的長度:

```js
const aString = 'Hello World!'
const bString = '你好'
const aStringLength = aString.length //12
const bStringLength = bString.length //2
```

#### 大寫與小寫

`toUpperCase`與`toLowerCase`兩個方法，用於將英文字串轉變為大寫或小寫:

```js
const aString = 'Hello World!'       
const bString = aString.toUpperCase()
const cString = aString.toLowerCase()
```

#### 清除字串左右空白字元

`trim`方法，可以清除字串左右空白字元:

```js
const aString = ' Hello World!    ' 
const bString = aString.trim()
```

#### 子字串搜尋(索引值)

`indexOf`可以回傳目前在字串中尋找的子字串的索引值，索引值的順序是從0開始，一直到字串的總長度減1。如果尋找不到該子字串，則會回傳`-1`。以下為範例:

```js
const aString = 'Apple Mongo Banana'

console.log( aString.indexOf('Apple') ) // 0
console.log( aString.indexOf('Mongo') ) // 6
console.log( aString.indexOf('Banana') ) // 12
console.log( aString.indexOf('Honey') ) // -1
```

#### 子字串

JavaScript中對於子字串的取出，有三種方法可以使用substr、substring、slice。它們是因應不同使用情況下的方法。substr與substring的英文字詞，從字義上幾乎是一模一樣，差異只在於簡寫。而slice的字義是"切割"的意思，在Array(陣列)中也有一個同名方法slice。這也代表在JavaScript中對於子字串提取，與Array(陣列)類似，字串類型，結構也像是由多個單字串組成的陣列。實際上slice方法，與substring十分類似，僅有一些行為上的差異。

##### substring

substring是ECMAScript 5.1標準定義的方法，使用兩個參數，一個是要取出的子字串的開頭索引值，另一個是要取出的子字串的結束索引值。如同之前`indexOf`中說明的，字串的索引值從字串的開頭是以`0`開始計算。

> 語法: str.substring(start[, end])

範例:

```js
const aString = '0123456789'
console.log(aString.substring(0, 3)) //012
console.log(aString.substring(5, 8)) //567

//以下為特殊情況
console.log(aString.substring(4, 4)) //''
console.log(aString.substring(5)) //56789
console.log(aString.substring(5, 2)) //234
console.log(aString.substring(5, 20)) //56789
console.log(aString.substring(-5, 2)) //01
console.log(aString.substring(2, -5)) //01
console.log(aString.substring(-5, -5)) //''
```

substring方法有很多特殊的情況，例如超出索引值、開頭索引值大於結束索引值，以及索引值出現負數情況等等。其中最特別的是，當"**開頭索引值大於結束索引值**"時，它會自動對調兩者的參數位置，也就是開頭索引值會變結束索引值，這個作法經常會出現出乎意料的結果。

另外，從上面的例子來看，substring會直接把負數的參數當作`0`看待，而超過索引值大小就當作最大索引值。

##### slice

slice扮演的是substring的替代方法，它的大部份正常的行為和substring類似，傳入參數值也是一樣的。看以下的範例和substring的範例比較一下，大概就知道在很多特殊情況下，slice是不會有值(只有空字串)回傳的。總個來說，它的使用結果比較容易被預測。

> 語法: str.slice(start[, end])

```js
const aString = '0123456789'
console.log(aString.slice(0, 3)) //012
console.log(aString.slice(5, 8)) //567

//以下為特殊情況
console.log(aString.slice(4, 4)) //''
console.log(aString.slice(5)) //56789
console.log(aString.slice(5, 2)) //''
console.log(aString.slice(5, 20)) //56789
console.log(aString.slice(-5, 2)) //''
console.log(aString.slice(2, -5)) //234
console.log(aString.slice(-5, -5)) //''
```

如果真的認真比較slice與substring在特殊的情況下的不同:

- 當開頭索引值大於結束索引值時，它不會互換兩者的位置
- 當索引值有負值的情況下，它是從最後面倒過來計算位置的(最後一個索引值相當於`-1`)

基本上slice必須遵守"開頭索引值"比"結束索引值"的位置更靠左，才會有回傳值，這就合理多了。所以它會比substring更推薦使用。

#### substr

`substr`基本上並不屬於在ECMAScript標準中所定義的方法，它是歸類在瀏覽器相容方法的附錄章節中。也就是說，它是因為原本有某些瀏覽器品牌自己作出來的一個方法，後來才放在附錄中，因此它有可能在不同的瀏覽器品牌中有不同的結果。例如在IE8以前的版本，它在特殊情況(開頭索引值為負數時)，結果就和IE9或其他瀏覽器不同。所以建議你可以儘量不要用這個方法，尤其是用在負數索引值的情況。除此之外，它的方法名稱實在和`substring`也太像了，很容易搞混兩個的傳入參數差異。

> 語法: str.substr(start[, length])

```js
const aString = '0123456789'
console.log(aString.substr(2,4)) //2345
console.log(aString.substr(0,8)) //01234567
```

> 註: 口訣"截長補短"，所以縮短字詞的方法"substr"用的是長度(length)。

總之，在字串提取子字串的功能，優先使用`slice`方法。

> 注意: 再強調一次，IE瀏覽器不能支援負數的開頭參數。其實我也建議你不要用substr方法，因為它也不是一個標準中的方法。

### 風格指引

- (Airbnb 6.1) 雖然使用雙引號(")與單引號(')都是一樣的宣告方式，但建議使用單引號(')
- (Airbnb 6.2/6.3) 字串中的長度超過100字元時，需分成多個字串，然後使用字串的串接符號(+)
- (Airbnb 6.4) 當需要在程式中建立字串時，優先採用樣版字串的方式，可以提高可閱讀性與精確。
- (Airbnb 6.6) 避免在字串中使用不必要的跳脫字元
- (Google) 單引號(')優先使用於雙引號(")。這有助於建立包含HTML的字串。
- (Google) 定義多行字串時，分成一行行的字串，再使用串接符號(+)進行串接。

## 布林(Boolean)

布林(Boolean/布林/)或簡寫為 Bool/布爾/，它是由發明的科學家George Boole命名。是一種採用兩分法(黑白/陽陰/真假)的值，在JavaScript中以關鍵字`true`與`false`來作為布林的兩種可用值。布林值通常用於判斷式中，與比較運算符有關，常用於流程控制的結構中。 例如以下的範例:

```js
const a = true
const b = false

console.log(typeof a) //boolean
console.log(1=='1') //true
console.log(typeof (1=='1')) //boolean
console.log(b!=a) //true
```

> 記住: 所有的JavaScript關鍵字(保留字)都是小寫的英文，像`true`的話，如果寫成True、 TRUE、TrUe，都是不對的寫法。

布林(Boolean)直接在HTML格式中輸出時，true會輸出true字串，false輸出false字串。不過因為HTML輸入時(或是在HTML上抓取數值時)，有可能需要將true字串或false字串轉成布林值的情況，這時候要用像下面的轉換方式:

```js
const aString = 'true'
const bString = 'false'
const aBool = (aString === 'true')
const bBool = (bString === 'true')

console.log(aBool, typeof aBool) //true "boolean"
console.log(bBool, typeof bBool) //false "boolean"
```

驚嘆號(!)之前有說過是個邏輯運算符，名稱為"Logic NOT"，用在布林值上具有反轉(Inverted)的功能，雙驚嘆號(!!)就等於反轉再反轉，等於轉回原本的布林值:

```js
const aBool = true
const bBool = !aBool //false
const cBool = !!aBool //true
```

雙驚嘆號(!!)並不單純是在這樣用的，它是為了要轉換一些可以形成布林值的情況值，列出如下:

- false: 0, -0, null, false, NaN, undefined, 空白字串('')
- true: 不是false的其他情況

```js
const aBool = !!0 //false
const bBool = !!'false' //true
const cBool = !!NaN //false
```

> 註: 你也可以用Boolean物件來作轉換的這件事。

### 風格指引

- (Airbnb 23.3) If the property/method is a boolean, use isVal() or hasVal()

## 空(null)與未定義(undefined)

比較其它程式語言的設計，一般都只有一種類似Null或None的原始資料類型，而JavaScript多了一種未定義(undefined)類型，它有其歷史背景與原因，但這兩個值很容易被搞混與誤用。

> 空(null)就是空值，代表的是沒有值的意思。

> 未定義(undefined)即是尚未被定義類型，或也有可能是根本不存在，不知道是什麼。

以下是最簡單的一種解釋說法:

```js
let name //當name從來未被定義完成，不知道其類型
```

你問JavaScript: name是什麼?

JavaScript回答: name? 從來沒聽過? 我不知道你在說什麼。

```js
name = null 
```

你問JavaScript: name是什麼?

JavaScript回答: 我不知道

以上的回答來自: [Why is null an object and what's the difference between null and undefined?](http://stackoverflow.com/questions/801032/why-is-null-an-object-and-whats-the-difference-between-null-and-undefined)

### 比較運算

null與undefined作比較運算時，在值的比較(==)是相等的，而在值與類型比較(===)時，是不相等的，這是能夠比較出兩個差異的地方。

```js
null  == undefined // true
null === undefined // false
```

而在`typeof`運算符回傳的類型時，目前null的類型仍然是`object`，而不是`null`。關於這一點要特別注意，有人或許它是個bug，但目前仍然沒有更動。

```js
typeof null        // object (bug in ECMAScript, should be null)
typeof undefined   // undefined
```



未定義(undefined)與"真正的根本完全沒有定義"而會造成錯誤中斷的程式碼有區別，見以下的範例:

```js
let a
console.log(a) //undefined
console.log(typeof a) //undefined
console.log(typeof b) //undefined

//注意，此行會因錯誤中斷之後的程式碼
console.log(b) //Error: b is not defined
```

### 結語

經常的用途是，null會用來當作一種特殊情況的運算後的回傳值。而未定義(undefined)則常使用的是加上`typeof`運算符的判斷方式。判斷敘述的教學，我們會在後面的章節再說明，這邊大概能理解這兩個原始資料類型大致上的涵意。而雖然JavaScript並沒有明確規範這兩個資料類型的用途，但對JavaScript程式設計師而言，應儘量使用空值(null)來進行運算判斷，而未定義(undefined)就留給JavaScript系統用的。特別一提的是，把不再使用的變數(物件)設為空值(null)與記憶體管理中的垃圾回收(garbage collection)機制有關。(參考[Exploring the Eternal Abyss of Null and Undefined](http://ryanmorr.com/exploring-the-eternal-abyss-of-null-and-undefined/))

## 小結

## 作業


