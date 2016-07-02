# 物件

物件(Object)類型，電腦程式的一種資料類型，比喻為是將人類現實世界物體的抽象化概念，它是高階程式語言的特性之一。

對於JavaScript語言來說，除了原始的資料類型例如數字、字串、布林等等之外，所有的資料類型都是物件。JavaScript中的物件與其他目前流行的物件導向程式語言的設計不同，它一開始是使用原型基礎(Prototype-based)的設計，而目前流行的其他物件導向程式語言，大部份都是使用類別基礎(class-based)的設計。ES6之後加入了類似於類別為基礎的替代語法(是Prototype-based的語法糖)，可以用於建立物件與作繼承之用，雖然它仍然是很基礎的類別設計，但也讓開發者多了另一種選擇。

物件在JavaScript語言中可分為兩種層面來看:

- 主要用於資料的描述，它扮演類似關連陣列的資料結構，儲存"鍵-值"的成對資料。
- 則是用於物件導向的程式設計，可以設計出各種的物件，其中包含各種方法，就像已經介紹過的各種包裝物件，例如字串、陣列等等的包裝物件。

物件類型使用以屬性與方法為主要組成部份:

- 屬性: 物件的基本可被描述的量化資料。例如水果這個物件，有顏色、產地、大小、重量、甜度等等屬性。
- 方法: 物件的可被反應的動作或行為。例如車子這個物件，它的行為有加速、煞車、轉彎、打方向燈等等的行為或可作的動作。

## 物件定義方式

### 物件字面(Object Literals)

用於資料描述的物件定義，使用花括號(curly braces)`{}`作為區塊宣告，其中加入無順序的"鍵-值"成對值，屬性的值可以是任何合法的值，包含函式或其他物件。

而在物件定義中的"鍵-值"，如果是一般的值的情況，稱為"屬性(property, prpo)"，如果是一個函式，稱之為"方法(method)"。

> 註：屬性名稱(鍵)中也不能使用保留字 

```javascript

const emptyObject = {}

const student = {
    firstName: 'Jerome',
    lastName: 'Howard'
}
```

以如果你已經對陣列有一些理解的基礎下，物件的情況相當類似，首先在定義上與獲取值上:

```js
const aArray = []
const aObject = {}

const bArray = ['foo', 'bar']
const bObject = {
    firstKey: 'foo',
    secondKey: 'bar'
}

bArray[2] = 'yes'
bObject.thirdKey = 'yes'

console.log(bArray[2]) //yes
console.log(bObject.thirdKey) //yes
```

> 註: 相較於陣列中不建議使用的`new Array()`語法，也有`new Object()`的語法，不過也不需要使用它。

不過，對於陣列的有順序索引值，而且只有索引值的情況，我們會更加關心"鍵"的存在，上面的程式碼雖然在`thirdKey`不存在時，會自動進行擴充，這通常不是經常的用途，物件的定義是在使用前就會定義好的，而物件的擴充是在於對現有的JavaScript語言內建物件，或是函式庫的擴充之用。

`in`這個運算符可以讓你判斷這個屬性名稱(鍵)是否存在於物件之中，然後決定是否改變其中的值:

```js
const bObject = {
    firstKey: 'foo',
    secondKey: 'bar'
}

if ('thirdKey' in bObject){
    bObject.thirdKey = 'yes'
}

console.log(bObject)
```

這種定義物件的方式，稱之為單例(singleton)的物件，也就是在程式碼中只能有唯一一個物件實例，就是你定義的這個物件。當你需要產生同樣的多個物件時，那又該怎麼作？那就是要另一種定義方式了。

> 注意: 不要任意擴充物件內的屬性，這是不好的習慣

### 類別(Class)

類別(Class)是先裡面定義好物件的整體結構藍圖(blue-print)，然後再用這個類別來產生相同結構的多個的物件實例。例如以下的簡單範例:

```js
class Student {
    constructor(id, firstName, lastName) {
        this.id = id
        this.firstName = firstName
        this.lastName = lastName
    }

    toString() {
        return 'id is '+this.id+' his/her name is '+this.firstName+' '+this.lastName
    }
}

const aStudent = new Student(111, 'Eddy', 'Chang')
console.log(aStudent.toString())
const bStudent = new Student(222, 'Inori', 'Egoist')
console.log(bStudent.toString())
```

有了這個例子，我們在下面分別說明一些這其中用到的語法，以及關鍵字的重要概念。

#### this

在這個物件的類別定義中，我們第一次真正見到`this`關鍵字的用法，`this`簡單的說來，是物件實體專屬的指向變數，`this`指的就是"這個物件實體"，以上面的例子來說，也就是當物件真正實體化時，`this`變數會指向這個物件實體。`this`是怎麼知道要設定到那一個物件實體？是因為`new`運算符造成的結果。

`this`變數是JavaScript不容易理解的一個特性，它也是隱藏的內部變數之一，當函式呼叫或物件實體化時，這個`this`變數都會存在於它的影響。

還記得我們在函式的章節中，使用作用範圍(Scope)來說明以函式為基礎的檢視角度，在函式區塊中可見的變數與函式的領域的概念。另外也有一種執行上下文(Context)的概念，就是對於`this`的影響範圍所稱的，它是以物件為基礎的的檢視角度。

http://www.digital-web.com/articles/scope_in_javascript/
http://stackoverflow.com/questions/3127429/how-does-the-this-keyword-work
http://unschooled.org/2012/03/understanding-javascript-this/
https://www.sitepoint.com/mastering-javascripts-this-keyword/


### 物件的判斷

最常見的情況是，如果有個函式要求它的傳入參數之一需要為物件，而確定並不能是陣列。你要如何先判斷傳進來的值是真的一個物件？

## 英文解說

function 
method

variable/constant
properties

### 結語

JavaScript是個物件導向的程式語言，但它的物件導向特性，與一般流行的物件導向程式語言不同，也有很多設計上的問題。

## 參考

- http://exploringjs.com/es6/ch_oop-besides-classes.html
- https://www.sitepoint.com/object-oriented-javascript-deep-dive-es6-classes/
- https://scotch.io/tutorials/better-javascript-with-es6-pt-ii-a-deep-dive-into-classes
- https://strongloop.com/strongblog/an-introduction-to-javascript-es6-classes/
- http://book.mixu.net/node/ch6.html
- http://www.phpied.com/3-ways-to-define-a-javascript-class/