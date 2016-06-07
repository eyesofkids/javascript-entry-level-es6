# 資料類型(值) Data Type

Javascript有6種原始的原始資料類型(Primary/Primitive)，分別是字串、數字與布林值，以及兩種特殊情況的null(空)與undefined(未定義)，ES6標準中加入了符號(Symbol)。

Javascript中還有另一個非常重要的第7種資料類型 - Object(物件)。

我們常常會把屬於物件的Array(陣列)、Data(日期時間)、Function(函式)、RegExp(正規表述式)獨立出來說明，因為這些物件都有特殊的功能和作用，算是特別的物件類型。

總之，我們大致上的分類是像下面這樣，本章節也是以這個分類開始來介紹，陣列與物件將會再獨立為另一章節說明:

主要的原始資料類型:

- 數字(Number)
- 字串(String)
- 布林值(Boolean)

特殊的原始資料類型:

- null(空)
- undefined(未定義)
- symbol(符號)

複合型(composite)或參考型(reference)的資料類型:

- array(陣列)
- object(物件)

> 註: 所謂的"原始資料類型"，指的是在程式語言中，最低階的一種資料類型，不屬於物件也沒有方法。它也具有不可改變的(immutable)特性。不過，Javascript語言中也存在名稱為String、Number、Boolean對應原始資料類型的物件，它們是包裝物件(wrapper object)，只用於特殊的情況。

> 註: 是的，Javascript的確是一個物件導向的程式語言。不過，Javascript的物件導向特性與其他語言例如Java、C++等有很大的不同。

## 數字(Number)

JavaScript的數字類型必定是64位元的浮點數，類似於Java語言的`double`資料類型，並不存在如其他程式語言有分離的整數(int)或浮點數(float)類型。在Javascript中，`1`與`1.0`指的是相同的類型與值。數字可以使用算術運算符(+-*/%)等來進行運算。

以下是幾個宣告的範例:

```
const intNumber = 123
const floatNumber = 10.01
const negNumber= -5.5
```

電腦中的程式語言在數學上的運算，總會有處理上的極限與無法處理的情況。Javascript使用三個特殊的記號，來代表在數字上處理的極限值或非數字的情況:

- +Infinity: 正無限值
- -Infinity: 負無限值
- NaN: 不是數字

```
const aNumber = 1/0
const bNumber = -1/0
```


### 數字的精確問題

1. 數字並非是永無極限的，有最大的數值上限也有最小的下限。

範例來自[這裡](http://javascript.info/tutorial/number-math#permissive-conversion-parseint-and-parsefloat)與[這裡](http://www.w3schools.com/js/js_number_methods.asp)

問題一：

```
alert(999999999999999);
alert(9999999999999999);
```

問題二：

```
var i = 0
while(i != 10) { 
  i += 0.2
}
```

問題三：

```
var x = 0.2 + 0.1; 
```

## 字串(String)

### 範例

用於描述一般的字串。例如：

- '你好'
- 'string'

### 字串與字元

存取一個字串中的字元，可以用類似陣列的存取方式，或是用`charAt()`函式。

```
'cat'.charAt(1); // returns "a"
'cat'[1]; // returns "a"
```

### 跳脫字元(escape characters)

當需要在雙引號符號(")中使用雙引號(")，或在單引號(')中使用單引號(')時，或在字串中使用一些特殊用途的字元。使用反斜線符號`\`來進行跳脫，這稱為跳脫字元或跳脫記號。常見用於在英文縮寫，例如：

```
'It\'s alright';
'This is a blackslash \\'
```

### 樣版字串(Template strings)

ES6中新式的寫法，稱為樣版字串(Template strings)，使用的是重音符號(``)來敘述字串。樣版字串可以多行、加入字串變數，以及其他延申的用法。範例如下：

```
`hello world`
`hello!
 world!`
`hello ${who}`
escape `<a>${who}</a>`
```

### 字串運算

字串中有很多運算用的函式與記號，以下是常用的幾個：

1. 字串連接

使用加號(+)是最直覺的方式，效能也比`concat()`好，這兩種方式都是同樣的結果，範例如下：

```

```


### 風格指引

- (Airbnb 6.2) 雖然使用雙引號(")與單引號(')都是一樣的宣告方式，但建議使用單引號(')
- (Airbnb 6.3) 字串中的長度超過100字元時，需分成多個字串，然後使用字串的連接符號(+)
- (Airbnb 6.6) 避免在字串中使用不必要的跳脫字元


## 資料類型 - 布林值（boolean）

## 特殊資料類型 - Null, Undefined
