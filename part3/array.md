# 陣列

陣列是一種有順序的複合式的資料結構，用於定義、存放複數的資料類型，在JavaScript的陣列中，並沒有規定它能放什麼資料進去，可以是原始的資料類型、其他陣列、函式等等。

由於陣列是會用於存放大量資料的結構，要如何有效地處理資料上，需要更加注意，尤其像JavaScript程式語言，對於每個不同的瀏覽器品牌與版本，以及相同功能的不同作法，都有實作上的差異。

另一個要學習的重點，是在於陣列相關函式的有無副作用部份，也就是純粹函式與副作用的內容，在這個章節我們會再配合陣列加以說明。

> 註: 雖然陣列資料類型是屬於物件，但Array包裝物件的`typeof Array`也是回傳'function'

## 陣列定義

陣列定義有兩種方式，一種是使用陣列字面文字，陣列是從0開始的有序複合值，可以用方括號([])來作其中成員的指定值或獲取值:

```js
const aArray = [] 
const bArray = [1, 2, 3]

console.log(aArray.length) //0

aArray[0] = 1
aArray[1] = 2
aArray[2] = 3
aArray[2] = 5

console.log(typeof aArray) // object
console.log(aArray) // [1,2,5]
console.log(aArray.length) //3
console.log(aArray[3]) //undefined
```

> 註: 陣列為參照的資料類型，其中包含的值是可以再更改的，這與const的常數宣告無關。請參考資料類型的章節內容。

另一種是使用Array物件的預先分配空間(Pre-allocating)的方式，但這種方式不建議使用，經測試過它對效能並沒有太大幫助。而且這一定會把長度一定固定住，除非你很確定陣列裡面的成員個數，不然千萬不要用。以下為範例:

```js
const aArray = new Array(10)
const bArray = new Array(1, 2, 3)
console.log(aArray.length) //10

aArray[0] = 1
aArray[1] = 2
aArray[2] = 3
aArray[2] = 5

console.log(typeof aArray) // object
console.log(aArray) // [1,2,5]
console.log(aArray.length) //10
console.log(aArray[3]) //undefined
```

> 註: JavaScript的內建物件都不建議`new`作初始定義的。

### 陣列定義注意事項

#### 一開始就搞混的語法

注意初學者很容易犯的一個錯誤，就是用下面這種陣列定義語法:

```js
const aArray = [10]
```

實際上這相當於:

```js
const aArray = []
aArray[0] = 10
```

#### 關聯陣列

JavaScript並沒有關聯陣列(Associative Array)這種資料結構，它是"鍵-值"的資料陣列。常用的複合類型只有物件和陣列，物件的屬性的確是"鍵-值"對應的，但也不單純是這樣，它也不是陣列。陣列的處理效率高出物件許多。

> 註: ES6中有Map資料結構，是一種進階的物件，但是目前看起來用得還不廣泛，支援方法也少。

#### 儲存多種資料類型

雖然並沒有規定說，你只能在一個陣列中使用單一種資料類型。但是，在陣列中儲存多種資料類型，絕對是個不好的習慣，在大的陣列中相當影響處理效能，例如像下面這樣的例子:

```js
var arr = [1, '1', undefined, true, 'true']
```

如果是多種資料類型，還不如先直接都用字串類型，需要取值時再作轉換。

另外，雖然數字類型在JavaScript中並沒有分浮點數或整數，但實際上在瀏覽器的JavaScript引擎(例如Google Chrome的V8引擎)中，整數的陣列的處理效率高於浮點數，可見其實引擎辦別得出來。

#### 多洞的陣列(Holey Arrays)或稀疏陣列(Sparse Arrays)

多洞的陣列代表你在定義陣列時，它的陣列值和索引(index)並沒有塞滿(Packed)，或是從一個完整的陣列刪除其中一個(留了個空位)。像下面這樣的陣列定義:

```js
const aArray = []
aArray[0] = 1
aArray[6] = 3

const bArray = [1, , 3]
```

或是陣列大部份的值都是不用的，有點像之前用`new Array(100)`先定義出一個很大的陣列，但實際上裡面的值很少，這叫作稀疏陣列(Sparse Arrays)，其實多洞的陣列(Holey Arrays)或稀疏陣列(Sparse Arrays)都差不多指得是同一種陣列，稀疏陣列可以重新設計讓它在程式上的效率更高。這種陣列在處理效能上都會有很大的影響，在大的陣列中要避免使用到這樣的情況。

## 陣列屬性與方法

陣列方法多如牛毛，

### 常用屬性

#### 長度(成員個數)

`length`用來回傳陣列的長度(成員個數)，這個屬性有時候是不可信的，就如同上面用`new Array(10)`的定義時，會被固定住為10，不論現在的裡面的值有多少個。多洞的陣列中也是與目前值的個數不同:

```js
const aArray = [1, , undefined, '123']
console.log(aArray.length)
```


## 處理大量資料時

## 陣列處理純粹函式

## 參考

- http://stackoverflow.com/questions/7486085/copying-array-by-value-in-javascript
- https://gamealchemist.wordpress.com/2013/05/01/lets-get-those-javascript-arrays-to-work-fast/
- https://javascriptweblog.wordpress.com/2010/07/12/understanding-javascript-arrays/
- http://vincent.billey.me/pure-javascript-immutable-array
- https://gist.github.com/bendc/9b05735dfa6966859025
- https://www.smashingmagazine.com/2012/11/writing-fast-memory-efficient-javascript/
- http://stackoverflow.com/questions/8423493/what-is-the-performance-of-objects-arrays-in-javascript-specifically-for-googl