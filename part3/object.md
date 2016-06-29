# 物件

物件(Object)類型，是將人類世界以可知可見物體的概念，進行數位資料化的一種資料類型。物件類型使用以屬性與方法為主要組成部份:

- 屬性: 物件的基本可被描述的量化資料。例如水果這個物件，有顏色、產地、大小、重量、甜度等等屬性。
- 方法: 物件的可被反應的動作或行為。例如車子這個物件，它的行為有加速、煞車、轉彎、打方向燈等等的行為或可作的動作。

## 類別(Class)

類別(Class)是物件產生的藍圖(blue-print)，我們在類別(Class)中定義好物件的組成內容，然後再用這個類別來產生一個個的物件實體。例如以下的最簡單的一個範例:

```js
class Point {
        constructor(x, y) {
            this.x = x;
            this.y = y;
        }
        toString() {
            return '(' + this.x + ', ' + this.y + ')';
        }
}
```

實際上類別定義是個函式類型

> 註: class關鍵字是ES6之後才有的一種新的作法。

## 物件字面(Object Literals)

- 物件是可變的(mutable)鍵集合(keyed collection)
- 陣列、函式、正規表述式都是物件
- 物件是屬性(properties)的容器(container)，屬性包含了一個名稱與數值，屬性的名稱可以是任何字串，也可以是空白字串；數值則可以是任何數值，除了`undefined`之外。
- 使用了`prototype`可連結特性讓物件繼承其他物件

使用花括號(curly braces)`{}`包含的空白區或name/value對。合法變數名稱不需要用括號(quote)，例如"first-name"就需要括號，而first_name就不需要。（註：合法變數名稱中也不能使用保留字） 

```javascript

var empty_object = {};

var stooge = {
         "first-name": "Jerome",
         "last-name": "Howard"
};

```

屬性值可以包含任何敘述，或是其他物件的語法

```javascript

var flight = {
         airline: "Oceanic",
         number: 815,
         departure: {
             IATA: "SYD",
             time: "2004-09-22 14:55",
             city: "Sydney"
         }, 
         arrival: {
             IATA: "LAX",
             time: "2004-09-23 10:42",
             city: "Los Angeles"
         }
};
```

### 物件的判斷

最常見的情況是，如果有個函式要求它的傳入參數之一需要為物件，而確定並不能是陣列。你要如何先判斷傳進來的值是真的一個物件？

## 英文解說

function 
method

variable/constant
properties

### 結語

JavaScript是個物件導向的程式語言，但它的物件導向特性，與一般流行的物件導向程式語言不同，也有很多設計上的問題。