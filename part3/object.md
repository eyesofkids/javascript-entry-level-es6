# 物件

物件(Object)類型是電腦程式的一種資料類型，用抽象化概念比喻為人類現實世界中的物體，它是高階程式語言的特性之一。

對於JavaScript語言來說，除了原始的資料類型例如數字、字串、布林等等之外，所有的資料類型都是物件。JavaScript中的物件與其他目前流行的物件導向程式語言的設計不同，它一開始是使用原型基礎(prototype-based)的設計，而其他的物件導向程式語言，大部份都是使用類別基礎(class-based)的設計。不過，ES6之後加入了類似於類別為基礎的替代語法(是Prototype-based的語法糖)，可以用於建立物件與作繼承之用，雖然它仍然只是很基礎類別語法，但也讓開發者多了另一種選擇。

物件在JavaScript語言中可分為兩種應用層面來看:

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

以如果你已經對陣列有一些理解的基礎下，物件的情況相當類似，首先在定義與獲取值上:

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

> 註: 存取物件中的屬性或方法，使用的是句點(.)符號，這已經在書中的很多內建方法的使用時都有用到，相信你應該不陌生。

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

物件字面定義方式，通常單純只用於物件類型的資料描述，也就是只用於定義單純的"鍵-值"對應的資料，在裡面不會定義函式(方法)。而基於物件字面定義，發展出JSON(JavaScript Object Notation)的資料定義格式，這是現今在網路上作為資料交換使用的一種常見的格式，在特性篇會再對JSON格式作更多的說明。

> 注意: 不要任意擴充物件內的屬性，雖然語法上可以這樣作，但實際上這是不好的用法。

### 類別(Class)

類別(Class)是先裡面定義好物件的整體結構藍圖(blue-print)，然後再用這個類別來產生相同結構的多個的物件實例。例如以下的簡單範例:

> 註: 至少到ES6標準時，現在的JavaScript中的物件導向特性並不是真的是以類別為基礎(class-based)的，這是包裹著以原型為基礎(prototype-based)的語法糖。

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
console.log(aStudent.firstName)

const bStudent = new Student(222, 'Inori', 'Egoist')
console.log(bStudent.toString())
```

> 註: 注意類別名稱命名時要使用大駝峰(ClassName)的寫法 

我們在下面分別說明一些這個例子中用到的語法與關鍵字的重要概念。

#### this

在這個物件的類別定義中，我們第一次真正見到`this`關鍵字的用法，`this`簡單的說來，是物件實體專屬的指向變數，`this`指的就是"這個物件實體"，以上面的例子來說，也就是當物件真正實體化時，`this`變數會指向這個物件實體。`this`是怎麼知道要設定到那一個物件實體？是因為`new`運算符造成的結果。

`this`變數是JavaScript不容易理解的一個特性，它也是隱藏的內部變數之一，當函式呼叫或物件實體化時，這個`this`變數都會存在於它的影響。

還記得我們在函式的章節中，使用作用範圍(Scope)來說明以函式為基礎的檢視角度，在函式區塊中可見的變數與函式的領域的概念。另外也有一種執行上下文(Execution context)的概念，就是對於`this`的影響範圍所稱的，它是以物件為基礎的的檢視角度。

`this`也就是執行上下文可以簡單用三個情況來區分:

1. 函式呼叫: 在一般情況下的函式呼叫，`this`通常都指向global物件。這也是預設情況。
2. 建構式(constructor)呼叫: 透過`new`運算符建立物件實體，等於呼叫類型的建構式，`this`會指向新建立的物件實例
3. 物件中的方法呼叫: `this`指向呼叫這個方法的物件實體

所以當建構式呼叫時，也就是使用`new`運算符建立物件時，`this`會指向新建立的物件，也就是下面這段程式碼:

```js
const aStudent = new Student(111, 'Eddy', 'Chang')
```

因此在建構式中的指定值的語句，裡面的`this`值就會指向是這個新建立的物件，也就是`aStudent`:

```js
constructor(id, firstName, lastName) {
        this.id = id
        this.firstName = firstName
        this.lastName = lastName
    }
```

也就是說在建立物件後，經建構式的執行語句，這個`aStudent`的三個屬性值就會被指定完成，所以可以用像下面的語法來存取屬性:

```js
aStudent.id
aStudent.firstName
aStudent.lastName
```

第3種情況是呼叫物件中的方法，也就是像下面的程式碼中，`this`會指向這個呼叫toString方法的物件，也就是`aStudent`:

```
aStudent.toString()
```

對於`this`的說明大致上就是這樣而已，這裡都是很直覺的說明。`this`當然也不只是這麼簡單，在特性篇中有獨立的一個章節來說明`this`的一些特性與應用情況。

#### 建構式(constructor)

建構式是特別的物件方法，它必會在物件建立時被呼叫一次，通常用於建構新物件中的屬性，以及呼叫上層父母類別(如果有繼承的話)之用。用類別(class)的定義時，物件的屬性都只能在建構式中定義，這與用物件字面的定義方式不同，這一點是要特別注意的。

#### 私有成員

JavaScript截至ES6標準為止，在類別中並沒有像其他程式語言中的私有(private)、保護(protected)、公開(public)這種成員存取控制的修飾關鍵字詞，基本上所有的類別中的成員都是公開的。

目前比較簡單常見的區分方式，就是在私有成員(或方法)的名稱前面，加上下底線符號(\_)前綴字，用於區分這是私有的(private)成員，這只是由程式開發者自己作撰寫上的區分差別，與語言本身特性無關，對JavaScript語言來說，名稱前有沒有有下底線符號(\_)的，都是一樣的變數。雖然也有其他"模擬"出私有成員的方式，不過它們都是複雜的語法，這裡就不說明了。以下為簡單範例:

```js
class Student {
    constructor(id, firstName, lastName) {
        this._id = id
        this._firstName = firstName
        this._lastName = lastName
    }

    toString() {
        return 'id is '+this._id+' his/her name is '+this.firstName+' '+this.lastName
    }
}
```

> 註: 基本的原則是，如果是私有成員，就不能直接在外部存取，要用getter與setter來實作取得與修改值的方法。私有方法也不能在外部呼叫，只能在類別內部使用。

#### getter與setter

在類別定義中可以使用`get`與`set`關鍵字，作為類別方法的修飾字，可以代表getter與setter。

```js
class MyClass {
        get prop() {
            return 'getter';
        }
        set prop(value) {
            console.log('setter: '+value);
        }
    }
```

#### 靜態成員

#### 繼承

繼承的範例如下，在建構式中會多呼叫一個`super`方法，用於

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
    
class ColorPoint extends Point {
        constructor(x, y, color) {
            super(x, y); // (A)
            this.color = color;
        }
        toString() {
            return super.toString() + ' in ' + this.color; // (B)
        }
    }
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

## 參考

- http://exploringjs.com/es6/ch_oop-besides-classes.html
- https://www.sitepoint.com/object-oriented-javascript-deep-dive-es6-classes/
- https://scotch.io/tutorials/better-javascript-with-es6-pt-ii-a-deep-dive-into-classes
- https://strongloop.com/strongblog/an-introduction-to-javascript-es6-classes/
- http://book.mixu.net/node/ch6.html
- http://www.phpied.com/3-ways-to-define-a-javascript-class/