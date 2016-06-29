# 陣列

陣列是一種有順序的複合式的資料結構，用於定義、存放複數的資料類型，在JavaScript的陣列中，並沒有規定它能放什麼資料進去，可以是原始的資料類型、其他陣列、函式等等。

陣列主要用於存放大量資料的結構，要如何有效地處理資料，需要更加注意。它的搭配方法與語法很多，也有很多作相同事情的不同方式，並不是每樣都要學，有些只需要用到再查詢相關用法即可。

另一個要學習的重點，是在於陣列相關函式的有無副作用部份，也就是延伸自純粹函式的內容，陣列的純粹函式對初學者來說會比較困難。主要原因在於JavaScript中的陣列與物件，搭配函式傳入參數使用時，雖然是傳值呼叫，但很容易就可變動其中包含的值。

> 註: 雖然陣列資料類型是屬於物件，但Array這個包裝物件的`typeof Array`也是回傳'function'

## 陣列定義

陣列定義有兩種方式，一種是使用陣列字面文字。

陣列的索引值(index)是從0開始的順序整數值，此外，陣列可以用方括號([])來取得成員的指定值，用這個方式也可以改變成員包含的值:

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

另一種是使用Array包裝物件的預先分配空間(Pre-allocating)的方式，但這種方式並不建議使用，經測試過它對效能並沒有太大幫助。而且這語法在分配後，一定會把長度值(成員個數)固定住，除非你百分之百確定陣列裡面的成員個數，不然千萬不要用。以下為範例:

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

注意初學者很容易犯的一個錯誤，就是用下面這種陣列定義語法，這語法並不會導致執行錯誤，這是很怪異的合法語法:

```js
const aArray = [10]
```

實際上這相當於:

```js
const aArray = []
aArray[0] = 10
```

#### 多維陣列

對JavaScript中的陣列結構來說，多維陣列指的是"陣列中的陣列"結構，例如像下面的範例:

```js
const magicMatrix = [
      [2, 9, 4],
      [7, 5, 3],
      [6, 1, 8]
]

magicMatrix[2][1] 
```

維數過多時在處理上會愈複雜，一般常見的只有二維。通常需要另外撰寫專屬的處理函式，或是搭配額外的函式庫在容易上會較為方便。

#### 關聯陣列

JavaScript並沒有關聯陣列(Associative Array)這種資料結構，關聯陣列指的是"鍵-值"的資料陣列，也常被稱為字典(dictionary)的資料陣列，有很多程式語言有這樣的資料結構。

在JavaScript中有複合類型只有物件和陣列，物件的屬性的確是"鍵-值"對應的，但也不單純是這樣，它也不是陣列，雖然在ES6中有特殊的物件結構，例如Set與Map，但目前的使用還不廣泛，其中支援的方法也少。

在大量複合資料處理時，陣列的處理效率明顯高出物件許多，陣列在JavaScript的用途相當廣泛，相對的需要因應不同的使用情況，也會複雜許多。

#### 儲存多種資料類型

雖然並沒有規定說，你只能在一個陣列中使用單一種資料類型。但是，在陣列中儲存多種資料類型，絕對是個不好的用法，在大的陣列中會嚴重影響處理效能，例如像下面這樣的例子。如果是多種資料類型，還不如先直接都用字串類型，需要取值時再作轉換。

```js
var arr = [1, '1', undefined, true, 'true']
```

另外你需要注意的是，雖然數字類型在JavaScript中並沒有分浮點數或整數，但實際上在瀏覽器的JavaScript引擎(例如Google Chrome的V8引擎)中，整數的陣列的處理效率高於浮點數，可見其實引擎辦別得出來各種不同的資料類型，而且會作最有效的儲存與運算，比你想像中聰明得很。

在ES6後加入了一種新式的進階資料結構，稱為型別陣列(Typed Arrays)，它是類似陣列的物件，但並非一般的陣列，也沒有大部份的陣列方法。這種資料結構是給特定的資料時使用的，主要是為了更有效率的處理二進位的資料(raw binary data)，例如檔案、圖片、聲音與影像等等。(註: [Typed Arrays標準](https://www.khronos.org/registry/typedarray/specs/latest/))

不過，就像上面一段說明的，聰明的JavaScript引擎的確會認得你在一般陣列中儲存的資料類型，然後作最有效率的運算處理，在某些情況下，型別陣列(Typed Arrays)在運算上仍然不見得會比一般陣列還有效率。

#### 多洞的陣列(Holey Arrays)或稀疏陣列(Sparse Arrays)

多洞的陣列代表你在定義陣列時，它的陣列值和索引(index)並沒有塞滿(Packed)，或是從一個完整的陣列刪除其中一個(留了個空位)。像下面這樣的陣列定義:

```js
const aArray = []
aArray[0] = 1
aArray[6] = 3

const bArray = [1, , 3]
```

或是陣列大部份的值都是不用的，有點像之前用`new Array(100)`先定義出一個很大的陣列，但實際上裡面的值很少，這叫作稀疏陣列(Sparse Arrays)，其實多洞的陣列(Holey Arrays)或稀疏陣列(Sparse Arrays)都差不多指的是這一類的陣列，稀疏陣列可以再重新設計讓它在程式上的效率更高。這種陣列在處理效能上都會有很大的影響，在大的陣列中要避免使用到這樣的情況。

### 拷貝(copy)陣列

把原有的陣列指定另一個變數(or常數)並不會讓它成為一個全新的陣列，這也是因為當指定值是陣列的情況，是指定到同一個參照，也就是同一個陣列，看下面的範例:

```js
const aArray = [1, 2, 3]
const bArray = aArray

aArray[0] = 100

console.log(bArray) //[100, 2, 3]
```

由範例可以看到，bArray與aArray是共享同樣這些值的，只是叫作不同名字的連體嬰，不論你在aArray或bArray中修改其中的值，增加或減少其中的值，都是對同一值作這件事。這種不叫拷貝陣列，可能應該是錯誤的寫法。

拷貝陣列並不是這樣作的，而且它是件複雜的事情，複雜的原因是陣列中包含的值，可以是各種不同的值，包含數字、字串、布林這些基本原始資料，也可以是其他陣列，物件、函式、其他特殊物件。而這件複雜的事情，同樣會出現在拷貝物件的時候。

拷貝陣列的情況可以區分為淺拷貝(shallow copy)與深拷貝(deep copy)兩種。淺拷貝只能拷貝到，只包含像數字或字串之類的基本原始資料類型值，而且是一維的平坦陣列，如果其中包含巢狀(多維)陣列、物件、函式、其他物件，要用深拷貝的方式才能真正複製出另一個完全獨立的陣列。

深拷貝一般就不用JavaScript內建的語法來達成，而是要用外部的函式庫例如jQuery、understore或lodash來作，它們會考慮效率，也要考慮各種情況。而且這些函式庫中的深拷貝不是只有支援陣列結構，同樣也支援物件結構。當然，如果你的陣列結構較為簡單，你也可以用for或while迴圈自己作這件事。

以下的方式主要是針對淺拷貝部份，一樣也有很多種方式可以作同樣這件事，以下列出四種:

#### 展開(spread)運算符

> 推薦使用

ES6後的新運算符，長得像之前在函式章節講到的其餘參數，使用的也是三個點的省略符號(ellipsis)(...)，語法相當簡單，現在很常被使用:

```js
const aArray = [1, 2, 3]
const copyArray = [...aArray]
```

它也可以用來組合陣列

```js
const aArray = [1, 2, 3]
const bArray = [5, 6, ...aArray, 8, 9]
```

> 註: 展開(spreads)運算符目前用babel轉換為ES5相容時是用concat方法

#### slice

slice(分割)原本是用在分割陣列為子陣列用的，當用0當參數或不加參數，相當於拷貝，這個方式是目前是效率較好的方式，語法也很簡單:

```js
const newArray = oldArray.slice(0)
const newArray = oldArray.slice()
```

#### concat

concat(合併)是用於合併多個陣列用的，把一個空的陣列和原先陣列合併，相當於拷貝的概念，這比較少人在用。在這裡寫出來是為了比較一下展開運算符:

```js
const newArray = [].concat(oldArray)
```

#### for/while迴圈語句

迴圈語句也可以作為淺拷貝，語句寫起來不難也很直覺，只是相較於其他方式，要打很多字，通常不會單純只用來作淺拷貝。以下為範例程式:

```js
const newArray = []

for (let i = 0, len = oldArray.length ; i < len ; i++){
    newArray[i]= oldArray[i]
}
```

```js
const newArray = []
let i = oldArray.length

while (i--){
    newArray[i]= oldArray[i]
}
```

### 判別是否為陣列

最常見的情況是，如果有個函式要求它的傳入參數之一需要為陣列，而確定並不能是物件。你要如何先判斷傳進來的值是真的一個陣列？因為直接使用`typeof`是沒辦法作這件事的，它只會直接回傳'object'。

在JavaScript中，有很多種方式可以作同一件事，這件事也不意外，不過每種方法都有一些些不同的細節或問題。以下的`variable`代表要被判斷的變數(或常數)值。

#### isArray

> 推薦使用

最簡單的判斷語法應該是這個，用的是內建Array物件中的`isArray`，它是個ES5標準方法:

```js
Array.isArray(variable)
```

#### constructor

下面這個是在Chrome瀏覽器中效能最佳的判斷方法，它是直接用物件的建構式來判斷:

```js
variable.constructor === Array
```

如果你是要判斷物件中的其中屬性是否為陣列，你可以先判斷這個屬性是否存在，像下面這樣(prop指的是物件屬性):

```js
variable.prop && variable.prop.constructor === Array
```

> 失效情況: 當變數繼承自陣列會失效

#### instanceof

這也是用物件的相關判別方法來判斷，`instanceof`是用於判斷是否為某個物件的實例，優點為語法簡潔清楚:

```js
variable instanceof Array
```

> 失效情況: 處理不同window或iframe時的變數會失效

#### toString.call

> 推薦使用

這也是用物件中的toString方法來判斷，這是幾乎所有情況都可以正確判斷的一種。它也是萬用方式，可以判斷陣列以外的其他特別物件，缺點是效率慢:

```js
Object.prototype.toString.call(variable) === '[object Array]'
```

> 註: jQuery、underscore函式庫中的判斷陣列的API是這種判斷方法

> 註: 在[http://shop.oreilly.com/product/9780596805531.do](http://shop.oreilly.com/product/9780596805531.do)這本書中有說到，`Array.isArray`其實就是這個方法的實作。

#### 方式選擇結論

這幾個方式的選擇，我的建議其實是只要學最後一種就行了，可以正確判斷並應用在各種情況，有時候比再快的效能更重要，更何況它其實是萬用的，除了陣列之外也可以用於其它的判斷情況。雖然它的語法對初學者來說，可能無法在此時完全理解，不過就先知道要這樣用就行了。

參考資料: [How do you check if a variable is an array in JavaScript?](http://stackoverflow.com/questions/767486/how-do-you-check-if-a-variable-is-an-array-in-javascript)

## 陣列屬性與方法

陣列方法多如牛毛，以下儘可以列出常用到的方法與屬性。

### 屬性

#### length長度(成員個數)

`length`用來回傳陣列的長度(成員個數)，這個屬性有時候是不可信的，就如同上面用`new Array(10)`的定義時，會被固定住為10，不論現在的裡面的值有多少個。多洞的陣列中也是與目前有值成員的個數不同:

```js
const aArray = [1, , undefined, '123']
console.log(aArray.length)
```

多維陣列說明過了，只是陣列中的陣列而已，它的`length`會像是一維陣列的:

```js
const magicMatrix = [
      [2, 9, 4],
      [7, 5, 3],
      [6, 1, 8]
]

console.log(magicMatrix.length) //3
```

`length`的整數值"竟然"是可以更動的，用來搭配for迴圈語句肯定會出錯:

```js
const bArray = [1, 2, 3]
console.log(bArray.length)

bArray.length = 4
console.log(bArray.length)
console.log(bArray)

bArray.length = 2
console.log(bArray.length)
console.log(bArray)
```

更動`length`其實是高效率的從陣列最後面"截短(truncate)"的密技語法，，它的效能是所有類似功能語法中最高的，而且比較其他類似語法是高到嚇人:

```js
const sArray = ['apple', 'banana', 'orange', 'mongo']

sArray.length = 3
console.log(sArray)

sArray.length = 2
console.log(sArray)
```

`length`指定為0也可以用於清空陣列，以下為各種清空陣列的範例程式碼，注意第一種不能使用`const`宣告，就意義上它不是真的把原來的陣列清空。第一種效率最好，第四種最差:

```js
let aArray = ['apple', 'banana', 'orange', 'mongo']

//第一種
aArray = []

//第二種
aArray.length = 0

//第三種
while(aArray.length > 0) {
    aArray.pop();
}

//第四種
aArray.splice(0, aArray.length)
```

### 方法

#### pop與push、shift與unshift

> 副作用方法

陣列的傳統處理方法，pop是"砰出"最後面一個值，然後把這個值從陣列移除掉。push是"塞入"一個值到陣列最後面。shift與pop類似，不過它是砰出最前面的值。unshift則與push類似，它是塞到陣列列前面。

`pop`的例子如下，它會回傳被砰出的值:

```js
const aArray = [1, 2, 3] 
const popValue = aArray.pop()

console.log(aArray) //[1,2]
console.log(popValue) //3
```

`push`的例子如下，它則是回傳新的長度值:

```js
const aArray = [1, 2, 3]
aArray.push(4)

console.log(aArray) //[1,2,3,4]

const pushValue = aArray.push(5)

console.log(aArray) //[1,2,3,4,5]
console.log(pushValue) //5
```

> 註: 簡單記法 - 有"p"的pop與push是針對陣列的"屁股"(最後面)。`pop-corn`是爆米花，所以pop用來爆出值的。有u的push與unshift是同一掛的。

#### concat



#### join與split，陣列與字串


#### 迭代

#### 排序

#### 搜尋與過濾

#### 其他



## 處理大量資料時


## 陣列處理純粹函式

push

```
function immutablePush(arr, newEntry){
  return [ ...arr, newEntry ]      
}
```

pop

```
function immutablePop(arr){
  return arr.slice(0, -1)     
}
```

shift

```
function immutableShift(arr){
  return arr.slice(1)     
}
```

unshift

```
function immutableUnshift(arr, newEntry){
  return [ newEntry, ...arr ]
}
```

splice

```
function immutableSplice(arr, start, deleteCount, ...items) {
  return [ ...arr.slice(0, start), ...items, ...arr.slice(start + deleteCount) ]
}
```

sort

```
function immutableSort(arr, compareFunction) {
  return [ ...arr ].sort(compareFunction)
}
```

reverse

```
function immutableReverse(arr) {
  return [ ...arr ].reverse()
}
```

刪除其中一個成員:

```js
function immutableDelete (arr, index) {
   return arr.slice(0,index).concat(arr.slice(index+1))
}
```

## 英文解說

## 常見問題

### 為什麼要那麼強調副作用？

副作用的概念已經存在於程式語言中很久了，但在最近幾年才在JavaScript界受到很大的重視。在過去，我們在寫這種腳本直譯式程式語言，最重視的其實是程式效率與相容性，同樣的功能，不同的寫法有時候效率會相差很多，也有可能會有不同瀏覽器品牌與版本上的差異。但現在的電腦硬體進步太多，執行環境的資源都很多，所謂的執行效率早就與10年前不同，效率已經不是單一的重點，而是要考量到更多其他的因素，例如程式碼的閱讀性與語法簡潔、易於維護與除錯、易於規模化等等。

純粹函式是一個新的想法，因為它有很多優點，雖然它大部份時候可能不是效率最佳的那種語法，而且為了要讓這些功能無副作用，還需要改寫它們。不過，純粹函式的確是未來的主流想法，當一個應用程式慢慢變大、變複雜時，純粹函式可以提供的好處會變成非常明顯，所以一開始學習這個概念是必要的。

## 參考

- http://stackoverflow.com/questions/7486085/copying-array-by-value-in-javascript
- https://gamealchemist.wordpress.com/2013/05/01/lets-get-those-javascript-arrays-to-work-fast/
- https://javascriptweblog.wordpress.com/2010/07/12/understanding-javascript-arrays/
- http://vincent.billey.me/pure-javascript-immutable-array
- https://gist.github.com/bendc/9b05735dfa6966859025
- https://www.smashingmagazine.com/2012/11/writing-fast-memory-efficient-javascript/
- https://drboolean.gitbooks.io/mostly-adequate-guide/content/ch3.html#oh-to-be-pure-again
- http://stackoverflow.com/questions/8423493/what-is-the-performance-of-objects-arrays-in-javascript-specifically-for-googl