# 資料類型(值)

資料是所有電腦運算的基礎，對於電腦來說，所有的資料都只是0與1的訊號。但對於人類來說，資料類型的區分就複雜得多了，需要因應各種不同的情況來使用。

JavaScript中有6種原始的原始資料類型(Primitive Data Type)，分別是數字(Number)、字串(String)與布林(Boolean)，以及空(null)與未定義(undefined)兩種特殊值，ES6後加入了符號(Symbol)。

另外還有另一個非常重要的第7種資料類型 - Object(物件)。

我們常常會把屬於物件的陣列(Array)、日期時間(Date)、函式(Function)、正規表述式(RegExp)等等獨立出來說明，因為這些物件都有各自獨立的作用，算是特別的物件類型。尤其是函式(Function)，JavaScript在把函式視為'function'的一種獨立類型(使用typeof的回傳值)。

總之，我們的分類是像下面這樣，本章節也是以這個分類開始來介紹，而陣列與物件將會在另一章節說明。至於新的資料類型symbol(符號)屬於進階的資料類型，本書中並不會介紹。

主要的原始資料類型:

- 數字(Number/難波/)
- 字串(String/斯翠/)
- 布林(Boolean/布林/)

特殊的原始資料類型:

- 空(null/怒偶/)
- 未定義(undefined/昴滴犯/)
- 符號(Symbol/辛波/)

複合型(composite)或參考型(reference)的資料類型:

- 陣列(Array/額瑞/)
- 物件(Object/阿捷特/)

> 註: 所謂的"原始資料類型"，指的是在程式語言中，最低階的一種資料類型，不屬於物件也沒有方法。它也具有不可改變的(immutable)特性。不過，JavaScript語言中也存在名稱為String、Number、Boolean的物件，用來對應原始資料類型，它們是一種包裝物件(wrapper object)，提供了原始資料類型可以使用的一些延申的屬性與方法。

> 註: JavaScript的確是一個物件導向的程式語言。不過，JavaScript中的物件導向特性與其他目前所流行的物件導向語言例如Java、C++等有很大的不同。

## 鬆散資料類型

JavaScript是一個鬆散資料類型(loosely typed)的程式語言，或稱之為"動態的程式語言(dynamic language)"。意思是說你不需要為變數/常數在定義時，就規定要使用哪一種的資料類型。取而代之的是，只需要指定它的值，JavaScript會依照你所指定的值決定變數/常數的資料類型。

```js
let foo = 42    // foo現在是Number資料類型
let foo = 'bar' // foo現在是String資料類型
let foo = true // foo現在是Boolean資料類型
```

## 判斷資料類型 - typeof

那麼要如何判斷目前變數/常數的資料類型？最簡單的方式就是使用這個運算符`typeof`，它是個類似正負號(+-)的運算符，是個一元的運算符，它是加在運算元的前面。`typeof`最後會回傳一個所屬資料類型的字串，範例如下:

```js
console.log(typeof 37) //'number'
console.log(typeof NaN) //'number'
console.log(typeof '') //'string'
console.log(typeof (typeof 1)) //'string'
console.log(typeof true) //'boolean'
console.log(typeof null) //'object'
console.log(typeof function(){}) //'function'
```

> 註: 為何null資料類型的的`typeof`結果是'object'(物件)，而不是'null'呢？依據[ECMAScript的標準章節11.4.3條](http://www.ecma-international.org/ecma-262/5.1/#sec-11.4.3)對`typeof`的規定就是回傳'object'。不過目前有一些反對的聲音，認為null是原始資料類型，理應當回傳自己本身的資料類型。也有認為現在修改這個太晚了，一經改動的話會造成很多舊程式被影響。截至ES6標準這一規定仍然沒有更動。

> 註: typeof的回傳值還有一個特例，就是函式的回傳值是'function'，因為在JavaScript覺得它太特別了，所以另外給它獨有的回傳值。

## 數字(Number)

JavaScript的數字(Number/難波/)類型是64位元的浮點數，類似於其他程式語言(如Java)的`double`/打波/ 資料類型。但在JavaScript中數字類型沒有如其他程式語言中，有獨立的整數(int)或浮點數(float)類型。所以對JavaScript來說，`1`與`1.0`指的是相同的資料類型與值。另外，數字可以使用算術運算符(+-\*/%)等來進行運算。以下是幾個宣告的範例:

```js
const intValue = 123
const floatValue = 10.01
const negValue= -5.5
```

> 註: 浮點數(float, FP)指的是帶有小數的數值，使用64位元儲存一個浮點數的稱為雙精度浮點數(double)

> 註: 雖然對JavaScript來說，浮點數與整數都屬於同一種資料類型。但瀏覽器內部的JavaScript引擎中是有分別的，它在執行時其實會把這兩種不同的數字類型區分出來，整數的運算理論上都會比浮點數快很多。

電腦中的程式語言在數學上的運算，總會有超出儲存的極限與無法處理的情況。JavaScript使用三個特殊的記號，來代表在數字上處理的極限值或非數字的情況，但它們都屬於數字資料類型:

- +Infinity/硬飛了踢/: 正無限值(相當於Infinity)
- -Infinity: 負無限值
- NaN: 代表不是數字

```js
console.log(1/0) //Infinity
console.log(1/-0) //-Infinity，0有分+0與-0，這有點算陷阱
console.log(Infinity - Infinity) //NaN
```

在JavaScript中數字的最大與最小值，可以使用`Number.MAX_VALUE`與`Number.MIN_VALUE`獲得。

```js
console.log(Number.MAX_VALUE)
console.log(Number.MIN_VALUE)
```

### 整數的進位

以整數來說，除了最常使用的10進位，還有2、8、16進位幾種。

2進位(Binary/敗了綠/)在ES6之後可以使用`0b`或`0B`開頭來定義:

```js
const FLT_SIGNBIT  = 0b10000000000000000000000000000000 // 2147483648
const FLT_MANTISSA = 0B00000000011111111111111111111111 // 8388607
```

8進位(Octal/阿多/)在ES6之後可以使用`0o`或`0O`開頭來定義:

```js
const n = 0O755 // 493
const m = 0o644 // 420
```

16進位(Hexadecimal/海斯達斯摸/)中直接可以用`0x`開頭來定義:

```js
const x = 0xFF
const y = 0xAA33BC
```

在ES6之前，對於2或8進位並沒有內建的直接可定義方式，需要透過一個字串轉數字(整數)的方法`parseInt(string, radix)`，將一個2進位或8進位的數字字串轉換，這方式也可以轉換16進位的數字字串:

```js
//8進位
const octalNumber = parseInt('071', 8)

//2進位
const binaryNumber = parseInt('0111', 2)

//16進位
const hexNumber = parseInt('0xFF', 16)
```

> 註: `parseInt`雖然主要是傳入字串，轉換為整數。但也有傳入浮點數，轉換為整數的功能。

如果是ES6之後的2、8進位定義格式的字串，需要用`Number()`方法才能轉為10進位，`parseInt`是無法正確轉換為整數的。範例如下:

```js
const binaryNumber = Number('0b11') // 3
const octalNumber = Number('0o11') // 9
const hexNumber = Number('0x11')  // 17
```

> 註: 不論是2、8、16進位的數字，直接輸出到HTML碼中必定會自動轉成10進位的數字字串輸出。

另一個情況是要將10進位數字以不同的進位基數(radix)轉為字串，通常是用在輸出時，這時要使用數字的方法 - `toString([radix])`。不過輸出後的字串格式與上面所說的格式不同，請見以下的範例:

```js
const decimalNumber = 125
console.log(decimalNumber.toString()) //'125'
console.log(decimalNumber.toString(2)) //'1111101'
console.log(decimalNumber.toString(8)) //'175'
console.log(decimalNumber.toString(16)) //'7d'
```

### 浮點數的轉換

#### 字串轉浮點數

對照上面的字串轉數字(整數)的`parseInt`方法，字串中還有另一個`parseFloat(string)`可以轉換數字字串為浮點數，不過就像最上面所說明的，數字1.0相當於1，小數點後面如果是0都會自動消失不見。以下是範例:

```js
const aNumber = parseFloat("10")  //10
const bNumber = parseFloat("10.00") //10
const cNumber = parseFloat("10.33") //10.33
const dNumber = parseFloat("34 45 66") //34
const eNumber = parseFloat("40 years") //40
const fNumber = parseFloat("He was 40")  //NaN
```

####  浮點數轉整數

至於浮點數要轉換為整數，需要使用`Math`物件中的幾個方法來轉換，因為浮點數要轉換有幾種不同的情況，看是要轉換為直接進位，還是四捨五入，還是直接捨去小數點，就看你的需要，以下為範例:

```js
const floatValue = 10.55
const intValueOne = Math.floor( floatValue ) //地板值 10
const intValueTwo = Math.ceil( floatValue ) //天花板值 11
const intValueThree = Math.round( floatValue ) //四捨五入值 11
```

> 註: Math是專門用於數學運算使用的JavaScript語言內建工具物件，裡面有很多好用的數學方法，請參考[Math(MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)與[JavaScript Math Object](http://www.w3schools.com/js/js_math.asp)

#### 整數轉浮點數

再強調一下，對於JavaScript來說，數字就是數字類型，沒有什麼浮點數或整數的類型。3.00與3的數字在JavaScript中都是一樣的，直接轉出數字3.00依然會用3來顯示。

唯一可能作的事情是調整輸出的格式，在輸出的時候都會變成字串類型。這時可以用數字物件中的`toFixed([digits])`方法來達成，以下為範例:

```js
const myString = (3).toFixed(2) //string, 3.00

const numObj = 12345.6789
const numObjString = numObj.toFixed() //string, 12346
```

> 註: toFixed方法如果不加傳入參數，會視為0位小數點，而且會作四捨五入。

### 其他類型轉換為整數

#### 雙位元反相運算(Double Bitwise NOT)(\~\~)

波浪符號(Tilde)(~)，在JavaScript語言中的運算符名稱為 "位元反相運算(Bitwise NOT)"，這是長得像波浪或毛毛蟲樣子的符號。根據它的功能文件說明，它會把"數字 x 轉換為 -(x + 1)"。也就是說像下面這樣的例子:

```js
const a = ~10 //a is -11
```

那如果是兩條毛毛蟲(Double Bitwise NOT)(\~\~)呢？上面的例子會變成如何:

```js
const b = ~~10 //b is 10
```

兩條毛毛蟲看起來沒什麼用，只是回復原本的數字值而已。不過它的真正功用是轉換其他類型為整數，而且它有相當於`parseInt`的功能，但並不是百分之百相等。在"正"數值情況下，由浮點數轉為整數時，也相當於`Math.floor()`。但重點在於它的效能在某些瀏覽器非常快，所以有很多程式設計師會使用這樣的寫法。以下為範例:

```js
console.log(~~'-1')  // -1
console.log(~~'0')   // 0
console.log(~~'1')   // 1
console.log(~~true)  // 1
console.log(~~false) // 0
console.log(~~null) // 0
console.log(~~undefined) // 0
```

> 註: `parseInt`雖然主要是傳入字串轉換為整數。但它也有傳入浮點數，轉換為整數的功能。

> 註: 位元操作符(Bitwise operators)是用於數字類型的位元運算使用的，本書並沒有特別提及，請參考[Bitwise operators(MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators)。

#### 正號(+)

正號(Unary Plus)(+)也是一個一元的運算符，它也有轉換其他類型為數字的能力，不過它的功能接近`parseFloat`，但並非完全相等。負號(-)也有同樣的功用，但很少被使用。以下為範例:

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

在電腦中的數字並非是永無極限的，有最大的數值上限也有最小的下限。另一種情況是，當在進行運算時，有時候會出現不如你想像的結果，尤其是在浮點數的運算上，這些大部份與浮點數的精確度(precision)有關，以下是幾個典型的範例，這些已經算是陷阱題目:

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

因此，在處理浮點數時，要格外小心。如果你遇到了不可預期的結果，有可能是精確度出了問題。如果你需要處理浮點數的運算，可以尋找合適的函式庫，單靠JavaScript語言本身可能會有所不足。例如[BigDecimal.js](https://github.com/dtrebbien/BigDecimal.js)或[decimal.js](https://github.com/MikeMcl/decimal.js/)

## 字串(String)

字串類型用於描述一般的字串值，是使用相當廣泛的值。JavaScript常與HTML搭配使用，而從HTML取得或輸入的值，以及輸出到HTML的值，都是字串類型的值。在JavaScript中，字串是使用Unicode作為編碼(UCS-2/UTF-16)。以下為簡單的宣告範例：

```js
const aString = '你好'
const bString = 'Hello'
```

對於JavaScript語言來說，使用雙引號標記("")與單引號標記('')來定義字串值，結果都是完全一樣的。在本書中，會推薦優先使用單引號標記('')。主要原因是JavaScript經常需要與HTML碼搭配使用，而HTML碼中也會使用引號來作為標記屬性值的定義，所以讓HTML屬性使用雙引號標記("")，而Javascript中的字串使用單引號標記('')，這變成一個約定俗成的撰寫習慣。

> 註: 有些程式語言(例如PHP)，對於使用雙引號標記("")與單引號標記('')定義兩種字串值，其特性並不完全相同。不過在JavaScript語言中是完全相同。

> 註: 優先使用單引號標記('')只是推薦的撰寫習慣，但不見得每個JavaScript函式庫或框架都這樣作，像Google, Airbnb, Node, npm都是這麼作。而推薦優先使用雙引號標記("")，經常被提到的是jQuery函式庫。

### 字串與字元

存取一個字串中的單一個字元，可以用類似陣列的存取方式，或是用`charAt()`方法。不過，JavaScript中並沒有所謂的字元(Character)類型，所以它依然是個字串(String)資料類型，只是只有一個字元而已。JavaScript中隱喻的將字串設計作為字元陣列，所以你可能會在字串類型中看到很多與陣列同名的方法，以及類似的使用情況，但這並不代表字串是一種真正的陣列物件。

```js
const a = 'cat'.charAt(1)   //  'a'
const b = 'cat'[1]   // 'a'

console.log(typeof a) //string
```

> 注意: 使用陣列的提取字串中的字元這種方式為不安全(unsafe)的方式，尤其在某些舊的瀏覽器品牌中不支援。

### 跳脫字元(Escape characters)

當需要在雙引號記號("")中使用雙引號(")，或在單引號記號('')中使用單引號(')時，或在字串中使用一些特殊用途的字元。使用反斜線符號blackslash(\\)來進行跳脫，這稱為跳脫字元或跳脫記號。例如：

```js
const aString = 'It\'s ok'
const bString = 'This is a blackslash \\'
```

> 註: 在JavaScript中，跳脫字元在雙引號記號("")與單引號記號('')中均可使用，這一點可能與其他程式語言有些不同。

### 樣版字串(Template strings)

ES6中新的字串寫法，稱為樣版字串(Template strings)，使用的是重音符號backtick(\`\`)來敘述字串。樣版字串可以多行、加入字串變數，還有其他延伸的用法，這在一些新式的函式庫或框架中(例如Angular)被大量使用。範例如下：

```js
const aString = `hello world`
const aString = `hello!

 world!`
```

在樣版字串中可以嵌入變數/常數，也可以作運算，嵌入的符號是使用錢號與花括號的組合(${}):

```js
const firstName = 'Eddy'
console.log(`Hello ${firstName}!
Do you want some
rabbits tonight?`)


const x = 5
console.log(`5 + 3= ${x + 3}`)

```

### 字串運算

字串中有很多可以進行處理字串使用的函式與符號，以下以不同的處理情況來說明。不過，因為字串的搜尋、取代等等，需要配合正規表述式的樣式，在這裡的內容就先不討論。

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

#### 與其他類型作加號(+)運算

不過，當你看到加號(+)時，可能會有個疑問，不是在數字的運算中也有加號(+)，像下面這個程式碼，結果應該是什麼，到底會是101還是11？

```js
const a = '10' + 1
console.log(a)

const b = 10 + '1'
console.log(b)
```

依據[ECMAScript的標準章節11.6.1條](http://www.ecma-international.org/ecma-262/5.1/#sec-11.6.1)對加號(+)運算的規定，簡單說明如下:

> 當運算式的左邊運算元**或**右邊運算元，是字串類型時，則使用字串連接(concatenating)的運算，非字串類型的運算元則使用ToString(轉為字串)的轉換。

所以上面的兩個加號(+)運算的結果將會是101。那更複雜的運算像下面的程式碼又會是什麼結果呢？你可以試看看:

```js
console.log( 3 + 4 + '5' )
console.log( 4 + 3 + '5' + 3 )
```

所以，當你想要轉換某個類型為字串時，直接與一個空白字串作加號(+)運算，就會變成字串類型了。這也是當用來把其他類型轉為字串的最簡便的語法。當然這是因為每個原始的資料類型，都有內建的toString方法，用加號(+)也是類似的效果，不過這對複合型的資料類型是沒有用的，例如陣列或物件。

```js
const a = 6 + ''
const b = true + ''
```

> 註: 除了加號(+)運算外，其他的數字運算符號(-/\*)都會將運算元轉換為數字。

#### 字串長度

`length`是字串的屬性值，可以讀取出字串目前的長度(字元個數)，中文字每個字算1個長度:

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

`trim`方法，可以清除字串左右(前後)空白字元:

```js
const aString = ' Hello World!    '
const bString = aString.trim()
```

#### 子字串搜尋(索引值)

`indexOf`可以回傳目前在字串中尋找的子字串的索引值，索引值的順序是從左邊由0開始計算，一直到字串的總長度減1。如果尋找不到該子字串，則會回傳`-1`，它還有第二個傳入參數是可以指定開始尋找的索引值。以下為範例:

```js
const aString = 'Apple Mongo Banana'

console.log( aString.indexOf('Apple') ) // 0
console.log( aString.indexOf('Mongo') ) // 6
console.log( aString.indexOf('Banana') ) // 12
console.log( aString.indexOf('Honey') ) // -1
```

#### 取出子字串

JavaScript中對於子字串的取出，有三種方法可以使用`substr`、`substring`、`slice`，其傳入參數並不相同。`substr`與`substring`這兩個英文字詞，在字義上是一模一樣，差異只在於簡寫。而`slice`的字義是"切割"的意思。另外，在陣列(Array)類型中，也有一個同名方法`slice`，這代表在JavaScript中對於子字串提取，與陣列(Array)中的`slice`方法類似。從上面的內容看來，字串資料類型，本身結構也類似由多個單字串組成的陣列。實際上`slice`方法，與`substring`十分類似，僅有一些行為上的差異。

##### substring

`substring`是ECMAScript 5.1標準定義的方法，使用兩個參數，一個是要取出的子字串的開頭索引值，另一個是要取出的子字串的結束索引值，子字串將不會包含結束索引值的那個字元。如同之前`indexOf`中說明的，字串的索引值從字串的開頭是以`0`開始計算。

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

`substring`方法有很多特殊的情況，例如超出索引值、開頭索引值大於結束索引值，以及索引值出現負數情況等等。其中最特別的是，當"開頭索引值大於結束索引值"時，它會自動對調兩者的參數位置，也就是開頭索引值會變結束索引值，這個作法經常會出現出乎意料的結果。

另外，從上面的例子來看，substring會直接把負數的參數當作`0`看待，而超過索引值大小就當作最大索引值。

> 註: 由於`substring`的自動對調索引值、負數參數自動變為0之類的古怪特性，它不建議被使用。而是要改用`slice`方法。

##### slice

`slice`扮演的是`substring`的替代救援方法，它的大部份的行為和substring類似，傳入參數值也是一樣的。看以下的範例和`substring`的範例比較一下，大概就知道在很多特殊情況下，slice是不會有值(只有空字串)回傳的。整體來說，它的回傳結果比`substring`容易預測，而且也較為合理。

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

- 當開頭索引值大於結束索引值時，它**不會**互換兩者的位置
- 當索引值有負值的情況下，它是從最後面倒過來計算位置的(最後一個索引值相當於`-1`)
- 只要遵守"開頭索引值"比"結束索引值"的位置更靠左，就有回傳值。其他都是空字串。

#### substr

`substr`基本上並不屬於在ECMAScript標準中所定義的方法，它是歸類在附錄章節中瀏覽器相容方法。也就是說，它是因為原本有某些瀏覽器品牌自己製作出來的一個方法，後來把它放在附錄中，作為其他瀏覽器品牌的參考之用。因此它有可能在不同的瀏覽器品牌中有不同的結果。例如在IE8以前的版本，它在特殊情況(開頭索引值為負數時)，結果就和IE9或其他瀏覽器不同。所以建議你儘量不要用這個方法，尤其是用在負數索引值的情況。除此之外，它的方法名稱實在和`substring`也太像了，很容易搞混兩個的傳入參數差異。

> 語法: str.substr(start[, length])

```js
const aString = '0123456789'
console.log(aString.substr(2,4)) //2345
console.log(aString.substr(0,8)) //01234567
```

> 註: 口訣"截長補短"，所以縮短字詞的方法"substr"用的是長度(length)。

總之，在字串提取子字串的功能，優先使用`slice`方法。

> 注意: 再強調一次，IE瀏覽器不能支援負數的開頭參數。我建議你不要用`substr`方法，因為它不是一個ECMAScript標準的方法。會放上這段的內容，主要是要讓你了解有這個方法，而且能看懂其他的教學文章或其他設計師所寫的程式碼。

### 風格指引

- (Airbnb 6.1) 雖然使用雙引號(")與單引號(')都是一樣的宣告方式，但建議使用單引號(')
- (Airbnb 6.2/6.3) 字串中的長度超過100字元時，需分成多個字串，然後使用字串的串接符號(+)
- (Airbnb 6.4) 當需要在程式中建立字串時，優先採用樣版字串的方式，可以提高可閱讀性與精確。
- (Airbnb 6.6) 避免在字串中使用不必要的跳脫字元
- (Google) 單引號(')優先使用於雙引號(")。這有助於建立包含HTML的字串。
- (Google) 定義多行字串時，分成一行行的字串，再使用串接符號(+)進行串接。

## 布林(Boolean)

布林(Boolean/布林/)或簡寫為 Bool/布爾/，它是由發明的科學家George Boole命名。是一種使用絕對兩分法的值(黑白/陰陽/真假)，在JavaScript中以關鍵字`true`與`false`來作為布林的兩種可用的值。布林值通常用於判斷式中，與比較運算符有關，常用於流程控制的結構中。 例如以下的範例:

```js
const a = true
const b = false

console.log(typeof a) //boolean
console.log(1=='1') //true
console.log(typeof (1=='1')) //boolean
console.log(b!=a) //true
```

> 注意: 所有的JavaScript關鍵字(保留字)都是小寫的英文，像`true`的話，如果寫成True、 TRUE、TrUe，都是不對的寫法。

布林(Boolean)直接在HTML格式中輸出時，`true`會輸出'true'字串，`false`輸出'false'字串。不過因為HTML輸入時(或是在HTML上抓取數值時)，有可能需要將'true'字串或'false'字串轉成布林值的情況，這時候要用像下面的轉換方式:

```js
const aString = 'true'
const bString = 'false'
const aBool = (aString === 'true')
const bBool = (bString === 'true')

console.log(aBool, typeof aBool) //true "boolean"
console.log(bBool, typeof bBool) //false "boolean"
```

驚嘆號(!)之前有說過是個邏輯運算符，名稱為"Logic NOT"，用在布林值上具有反轉(Inverted)的功能，雙驚嘆號(!!)就等於"**反轉再反轉**"，等於轉回原本的布林值:

```js
const aBool = true
const bBool = !aBool //false
const cBool = !!aBool //true
```

雙驚嘆號(!!)並不單純是在這樣用的，它是為了要轉換一些可以形成布林值的情況值，列出如下:

- `false`: 0, -0, null, false, NaN, undefined, 空白字串('')
- `true`: 不是false的其他情況

```js
const aBool = !!0 //false
const bBool = !!'false' //true
const cBool = !!NaN //false
```

> 註: 你也可以用Boolean物件來作轉換其他資料類型為布林的這件事。

### falsy與短路求值

Douglas Crockford是在JavaScript界相當知名的大師級人物，他主張使用"**truthy**"與"**falsy**"來描述資料類型的值。也就是說，像上面說講的那些會轉換為布林值的false值的資料類型的值，通稱為"falsy"(字典裡是沒這個字詞，意思是"false的")，你可以把它當成是"false家族成員"。

> "falsy"包含了0, -0, null, NaN, undefined, 空白字串('')，當然也一定包含了false值

"falsy"的概念在JavaScript的邏輯運算，以及布林值中都是很重要的概念。之前已經看到一個邏輯運算符 - 邏輯反相(Logic NOT)(!)的運用，還有兩個邏輯運算符，也很常用到:

- 邏輯與(Logical AND)(&&)
- 邏輯或(Logical OR)(||)

邏輯與(&&)與邏輯或(||)的運算，正常的使用情況下，並沒有什麼太大的問題，範例如下:

```js
console.log(true && false) //false
console.log(true || false) //true
```

只不過，因為JavaScript採用了與其他程式語言不同的邏輯運算的回傳值設計，我們把這兩個運算符稱為"**短路求值(Short-circuit)**"的運算符。實際上JavaScript中，在經過邏輯與(&&)與邏輯或(||)的運算後，它的回傳值是 - **最後的值(Last value)**，並不是像在常見的Java、C++、PHP程式語言中的是布林值。

因此，短路求值運算變成JavaScript中一種常被使用的特性，尤其是邏輯或(||)。那這與之前所說的"falsy"特性有何關係？關於邏輯或(||)運算在JavaScript語言中的可以這樣說明:

> 邏輯或(Logical OR)(||)運算符在運算時，如果當第1個運算子為"falsy"時，則回傳第2個運算子。否則，將會回傳第1個運算子。

也就是說像下面這樣的程式碼，基本上它的回傳值都不是布林值，而是其他的資料類型的值:

```js
console.log('foo' || 'bar') // 'foo'
console.log(false || 'bar') // 'bar'
```

"falsy"擴大了這種運算的範圍，像下面的程式碼的回傳情況:

```js
console.log( 0 || '' || 5 || 'bar') //5
console.log(false || null || '' || 0 || NaN || 'Hello' || undefined) //'Hello'
```

而出現了一種常見的在程式碼中簡短寫法，這稱為"**指定預設值**"的寫法，就是使用邏輯或(Logical OR)(||)運算符，範例如下:

```js
let a = value || 5 //5是預設值
```

不過，這樣的指定預設值的寫法，並不能完全精確判斷value屬於哪一種資料類型與值的方式，在某些情況下會出現意外，例如你可能認為`value=0`也是合法的值，但`value=0`時，a會被短路求值成5。所以，只要當value是"falsy"時，變數a就會被指定為預設值5。所以在使用時還是需要特別注意，不然你會得到出乎意料的結果。

> 註: 每種程式語言都有短路求值的一些設計方式，請參考[Short-circuit evaluation](https://en.wikipedia.org/wiki/Short-circuit_evaluation)

### 風格指引

- (Airbnb 23.3) 如果屬性(變數/常數)或方法(函式)是一個布林值，使用像isVal()或hasVal()的命名。

## 空(null)與未定義(undefined)

比較其它程式語言的設計，一般都只有一種類似Null的空值原始資料類型。而JavaScript多了一種未定義(undefined)類型，會這樣設計是有它的歷史背景與原因。

依照ECMAScript的定義`null`的說明([出處](https://tc39.es/ecma262/#sec-null-value))：

> primitive value that represents the intentional absence of any object value

> 中譯：一種原始值，代表刻意缺少(有意短少)的任何物件值

依照ECMAScript的定義`undefined`的說明([出處](https://tc39.es/ecma262/#sec-undefined-value))：

> primitive value used when a variable has not been assigned a value

> 中譯：一種原始值，當變數沒有被指定一個值時而使用

簡單說明上述的想法：

> 空(null)就是空值，代表的是沒有值的意思。(有隱含不存在的物件值，並非空白物件)

> 未定義(undefined)即是尚未被定義類型，或也有可能是根本不存在，完全不知道是什麼。

> 註：有一說是兩個空值的設計，是自一開始就是錯誤設計(由創作者Brendan Eich說明過)，但JavaScript長久以來一向有向下版本的相容性設計，所以現今不可能更動這個設計。

### 轉為數字(或進行算術運算)

這兩者在進行轉換為數字或算術運算時的行為也不太一樣。

```js
+null // 0
+undefined // NaN

1 - null //1
1 - undefined //NaN
```

### 比較運算

null與undefined作比較運算時，在值的比較(==)是相等的，而在值與類型比較(===)時，是不相等的，這是能夠比較出兩個差異的地方。

```js
null  == undefined // true
null === undefined // false
```

而在`typeof`運算符回傳的類型時，目前null的類型仍然是`object`，而不是`null`。關於這一點要特別注意，有人或許它是個bug，但目前仍然沒有更動。

```js
typeof null        // object
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

### 用途

經常的用途是，null會用來當作一種運算後的特殊情況回傳值。而未定義(undefined)則常使用的是加上`typeof`運算符的判斷方式。這裡只是讓你能大概理解這兩個原始資料類型的涵意。

雖然JavaScript並沒有明確規範這兩個資料類型的用途，但對JavaScript程式設計師而言，應儘量使用空值(null)來進行運算後判斷，而未定義(undefined)就留給JavaScript系統用。

> 註: 要更深入理解請參考[Exploring the Eternal Abyss of Null and Undefined](http://ryanmorr.com/exploring-the-eternal-abyss-of-null-and-undefined/))

## 家庭作業

### 作業一

[Math.random()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random)方法是在JavaScript中產生亂數的一個方法。不論你要作野球拳遊戲，還是線上博杯拿大獎的應用程式，都可以用它來產生亂數。`Math.random`會隨時產生一個0到1的浮點數，它的基本用法如下:

```js
Math.floor(Math.random() * 6) + 1
```

- 上面的語句代表要產生1到6的整數亂數
- 1代表開始數字
- 6代表可能情況有6種

所以如果是以最大值max，最小值min來寫這個語句，這也是產生整數的亂數，會像下面這樣:

```js
Math.floor(Math.random() * (max - min + 1)) + min
```

現在要利用這個產生亂數的方法，作一個線上抽獎的活動，客戶說有下面幾個獎品:

1. 50吋液晶電視 1台
2. PS4遊戲機 3台
3. 手機行動充電器 10台
4. 7-11的100元購物券 100張

預計參與抽獎的人數有1萬人，要如何來寫這個程式的抽獎過程的應用程式？

### 作業二

有一家日本的房地產公司來台灣設立新的分公司，因為日本的房地產用的計算單位與台灣的不同，所以要委託你寫一個程式，這個程式是希望能轉換平方公尺為坪數。要如何來寫這個轉換的公式？
