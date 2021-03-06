# 原型基礎物件導向

> If you don’t understand prototypes, you don’t understand JavaScript.

> 如果你沒搞懂原型，你不算真的懂JavaScript

JavaScript本身就是原型為基礎的物件導向設計，至ES6標準制定後仍沒變動過。在物件的章節中所介紹的類別定義方式，只是原型物件導向語法的語法糖，骨子裡還是原型，並不是真正的以類型為基礎的物件導向設計。理解JavaScript的原型是很重要的，只是混亂得讓初學者難以理解。

> 註: 語法糖(Syntactic sugar)指的是在程式語言中添加的某些語法，這些語法對語言本身的功能並沒有影響，但是能更方便使用，可以讓程式碼更加簡潔，有更高可讀性。另外類似的術語還有"語法糖精"與"語法鹽"。

## 函式 - 原型的起手式

所有JavaScript中的函式都有一個內建的`prototype`屬性，指向一個特殊的prototype物件，prototype物件中也有一個`contructor`屬性，指向原來的函式，互相指來指去會讓你覺得有點怪異，但設計就是如此。

以下的程式碼可以看出這個關係:

```js
function Player() { }

console.log(Player)
console.log(Player.prototype)
console.log(Player.prototype.constructor)
console.log(Player.prototype.constructor === Player) //true
```

為了更容易理解，以下是一個簡單的關係圖:

![Prototype Image 01](https://raw.githubusercontent.com/eyesofkids/javascript-entry-level-es6/master/assets/prototype_1.png)

再來是`__proto__`這個內部屬性，它是一個存取器(accessor)屬性，意思是用`getter`和`setter`函式合成出來的屬性，我們可以用它來更加深入理解整個原型的樣貌。`__proto__`是每一個JavaScript中**物件**都有的內部屬性，代表該物件繼承而來的源頭，也就是指向該物件的原型(prototype)，它會用來連接出**原型鏈**，或可以理解為原型的繼承結構。

對於一個函式而言，它本身也是一個物件，它的原型就是最上層的`Function.prototype`，你可以說這是所有函式的發源地。所以`Player`函式本身的`__proto__`指向`Function Prototype`，這應該可以很容易理解。

那麼，`Player.prototype`的`__proto__`指向哪裡？`Player.prototype`本身也是個物件，它指向的就是所有JavaScript中最上層的物件起源，也就是`Object.prototype`。由此也可推知，`Function.prototype`也同樣指向`Object.prototype`。以下面的程式就可以看到這個結果:

```js
function Player() { }

console.log(Player.__proto__)
console.log(Player.prototype.__proto__)

//最上層的Object.prototype的__proto__是null值，它是一個特例
console.log(Object.prototype.__proto__)  //null

console.log(Player.__proto__ === Function.prototype)  //true
console.log(Player.prototype.__proto__ === Object.prototype) //true
console.log(Function.prototype.__proto__ === Object.prototype) //true
```

為了更容易理解，以下是一個簡單的關係圖，在圖片中綠色的虛線即是`__proto__`的指向，原本的`prototype`為紅色的實線:

![Prototype Image 02](https://raw.githubusercontent.com/eyesofkids/javascript-entry-level-es6/master/assets/prototype_2.png)

> 註: `__proto__`注意是前後各有兩條下底線(\_)，不是只有一條而已。

> 註: `__proto__`在一些舊的瀏覽器品牌(例如IE)中不能使用。雖然在ES6中已經正式納入標準之中，它是個危險的內部屬性，也不要用在真正的應用程式上。

當進一步使用Player函式作為建構函式，產生物件實體時，也就是使用`new`運算符的語句。像下面這樣簡單的例子，在建構函式中會用`this.name`的方式來指定傳入參數，`this`按照之前在物件篇的內容所說明，指向的是`new`運算符中指定的物件實體。

```js
function Player(name) {
  this.name = name
}

const newPlayer = new Player('Inori')
```

此時在`newPlayer`物件中的`prototype`與`__proto__`又是如何？由於`newPlayer`是一個物件，並不是函式，它不會有`prototype`這個屬性。`newPlayer`的`__proto__`則是指向`Player.prototype`，把物件的原型鏈整個串接起來。

```js
//不是函式，不會有prototype
console.log(newPlayer.prototype)  // undefined

console.log(newPlayer.__proto__)
console.log(newPlayer.__proto__ === Player.prototype) //true
```

以下是一個簡單的關係圖，黃色的代表剛剛實體化的`newPlayer`物件，在圖片中綠色的虛線即是`__proto__`的指向:

![Prototype Image 03](https://raw.githubusercontent.com/eyesofkids/javascript-entry-level-es6/master/assets/prototype_3.png)

最後總結以下的幾個摘要，讓這章節的內容更加清楚:

- 每個函式中都會有`prototype`屬性，指向一個`prototype`物件。例如MyFunc函式的`prototype`屬性，會指向對應的MyFunc.prototype物件。
- 每個函式的`prototype`物件，會有一個`constructor`屬性，指回到這個函式。例如MyFunc.prototype物件的`constructor`屬性，會指向MyFunc函式。
- 每個物件都有一個`__proto__`內部屬性，指向它的繼承而來的原型`prototype`物件
- 由`__proto__`指向連接起來的結構，稱之為原型鏈(prototype chain)，也就是原型繼承的整個連接結構

## 原型真相 - 由問答理解

原型到底是什麼，從上面看到原型鏈、new運算符、建構式等等的概念，你可能會有一些疑惑或誤解。以下是幾個重點，我們用問答的方式來說明。

### 原型是什麼？

一種讓別的物件繼承其中的屬性的物件。

### 任何物件都有其原型？

是的。唯一的例外是Object.prototype(Object的原型)沒有原型，它是所有物件的最上層的源頭。

### 任何物件可以拿來作為原型？

是的。

### 那為什麼上面的例子中，`newPlayer`物件沒有prototype屬性

用`Object.getPrototypeOf`方法或`__proto__`屬性，可以找到這個物件真正的原型是什麼，而這個prototype屬性是給建構函式用的。事實上，每個物件都一定有constructor(建構式)屬性，constructor(建構式)屬性會指向建立這個物件的函式，建構函式的屬性其中就有prototype屬性。

```js
const newPlayer = new Player('Inori')

console.log(Object.getPrototypeOf(newPlayer))
console.log(newPlayer.__proto__)

//只能用於不是使用Object.create建立的物件
console.log(newPlayer.constructor.prototype)
```

### 為什麼函式中一定會有prototype屬性，那建構函式與函式的區分又是什麼？

設計上的原則而已，JavaScript並沒有對你所建立的函式，區分建構函式與一般的函式，所以只要是函式，就一定會有prototype屬性(除了語言內建的函式不會有這個屬性)。而且，函式以外的類型，不會有這個屬性。

## 擴充

原型可以很容易的擴充屬性與方法，而且是動態的，可以在物件實體後繼續擴充其中的成員，這也是JavaScript中常用來擴充內建物件的方式。當使用`Player.prototype`來進行擴充時，這些擴充出來的屬性與方法，是所有物件共享的，見以下的範例。

```js
function Player(name) {
  this.name = name
}

const newPlayer = new Player('Inori')
console.log(newPlayer.age)

Player.prototype.age = 11
console.log(newPlayer.age)

Player.prototype.toString = function() {
    return 'Name: ' + this.name
}

console.log(newPlayer.toString())
console.log(newPlayer) //觀察物件的內容
```

> 註: 如果以類別為基礎的物件導向，在物件實體化後，物件或類別都是無法擴充其中的成員的。這一點是原型物件導向的彈性之處。

## 繼承

繼承是什麼？我們回歸本質上來思考"繼承的目的是什麼"，在程式開發時的目的通常是為了擴充原有的物件定義不足之處。繼承基本上可以依照不同程式語言的物件導向特性區分為:

- 類別的繼承(Classical inheritance)
- 原型為基礎的繼承(Prototype-based inheritance)

原型繼承是什麼，其實就是原型的擴充語法，上一節有說明過了。至於類別的繼承方式，在JavaScript可以用`Object.create`方法模擬出來，不過複雜多了。

```js
//superclass
function Player(name) {
    this.name = name
}

// 1. 呼叫上層的建構式
function VipPlayer(name, level) {
    Player.call(this, name)
    this.level = level
}

// 2. 使用Object.create建立prototype物件。
//    VipPlayer建構式的prototype.constructor為自己。
VipPlayer.prototype = Object.create(Player.prototype)
VipPlayer.prototype.constructor = VipPlayer


var inori = new VipPlayer('inori', 5)

console.log(inori instanceof Player) //true
console.log(inori instanceof VipPlayer) //true
```

相當於ES6中的類別定義方法，使用extends關鍵字來作類別的繼承:

```js
class Player{
  constructor (name){
    this.name = name
  }
}

class VipPlayer extends Player {
  constructor (name, level){
    super(name)
    this.level = level
  }
}
```

總而言之，在很多真實的應用情況下，"以合成(或擴充)代替繼承(composition over inheritance)"才是正解，在JavaScript中合成比繼承容易得多了，彈性高應用也很廣，也比較符合語言本身的特性。思考的重點不同，才能撰寫出符合應用情況的程式碼。

## 私有/公開成員

在十多年前Douglas Crockford的這篇[Private Members in JavaScript](http://javascript.crockford.com/private.html)就有提出關於私有成員的樣式，使用的是建構式樣式來模擬私有、公有與特權方法。網路上大部份的文章都是使用這個樣式來說明，或是進一步改良。

不過，私有或公開成員完全不是JavaScript語言中原本就有的設計，用這種方法會限制住只能使用"建構式樣式"的語法來作物件實體化，而且也與原型物件導向概念相去甚遠。原型物件導向對於封裝的概念，基本上是根本沒有。

目前比較簡單常見的區分方式，就是在私有成員(或方法)的名稱前面，加上下底線符號(\_)前綴字，用於區分這是私有的(private)成員，這只是由程式開發者撰寫上的區分差別，與語言本身特性無關，對JavaScript語言來說，成員名稱前有沒有有下底線符號(\_)的，都是視為一樣的變數，至於要如何保護這些私有成員，就靠程式設計者自己了。

## 靜態屬性/靜態方法

JavaScript語言中也沒這概念。不過，原型物件另外定義的屬性與方法，是所有以此原型物件實體化的物件共享的就是了:

```js
function Employee() {
}
Employee.prototype.count = 0;

var e = new Employee();
console.log(e.hasOwnProperty('count')); // logs "false"
e.count += 1;
console.log(e.hasOwnProperty('count')); // logs "true"

console.log(e.count);                   // logs "1"
console.log(Employee.prototype.count);  // logs "0"
var e = new Employee();

console.log(e.count);                   // logs "0"
++Employee.prototype.count;
console.log(e.count);                   // logs "1"

var e = new Employee();
console.log(e.count);    // Logs "0", because it's using `Employee.prototype.count`
++e.count;               // Now `e` has its own `count` property
console.log(e.count);    // Logs "1", `e`'s own `count`
delete e.count;          // Now `e` doesn't have a `count` property anymore
console.log(e.count);    // Logs "0", we're back to using `Employee.prototype.count`
```

另一種作法是用IIFE模擬出靜態變數的結構:

```js
var Employee = (function() {
    var sharedVariable = 0;

    function Employee() {
        ++sharedVariable;
        console.log("sharedVariable = " + sharedVariable);
    }

    return Employee;
})();

new Employee(); //1
new Employee(); //2
new Employee();  //3
new Employee();  //4
```

## 原型鏈(prototype chain)

原型鏈是JavaScript中以原型為基礎的物件導向特性，指的是使用原型來作物件實體化，會產生"原型鏈"的結構。原型鏈的觀念如此重要，在於有很多物件的行為都是與它相關，在物件篇已經有介紹過物件中的一些方法，都是會遍歷整個物件的原型鏈，而不僅是物件本身而已。這與原型的物件實體化設計有關，因為物件的實體化過程，就是原型的繼承過程，也就是物件的實體化是由繼承其他物件而來的。

先看一下物件屬性的存取這件事，下面的例子中，我們並沒有在`player`物件中定義`toString`方法，但它的確是存在的，這個`toString`方法是來自原型鏈上層的物件中，也就是繼承得來的。

```js
const player = {}
console.log(player.toString())
```

### instanceof

`instanceof`是一個運算符，主要是用來判斷一個物件在原型鏈中是否有存在某個建構式，如果存在回傳`true`，要不然就是`false`值。它的前面的運算子是物件，後面則是建構式，語法是像下面這樣:

> object instanceof constructor

`instanceof`常會拿來和存取物件實體的`constructor`屬性的方式作比較，由於`constructor`只會指向最接近的建構式，而`instanceof`會找遍整個原型鏈，當然結果會有所有不同，以下為範例:

```js
function Animal(){}

function Cat(){}
Cat.prototype = new Animal()
Cat.prototype.constructor = Cat

const kitty = new Cat()


console.log(kitty instanceof Cat) // true
console.log(kitty instanceof Animal) // true
console.log(kitty.constructor === Cat) // true
console.log(kitty.constructor === Animal) // false
```

`instanceof`有一些例外情況，它對於原始資料類型(數字、字串、布林、null、undefined)是無法判斷的，必定是回傳`false`，這和用`constructor`屬性判斷是不同的結果，例如以下的範例:

```js
console.log(3 instanceof Number) // false
console.log(true instanceof Boolean) // false
console.log('abc' instanceof Boolean) // false
console.log((3).constructor === Number) // true
console.log(true.constructor === Boolean) // true
console.log('abc'.constructor === String) // true
```

另外對在iframe、frame或著另開視窗中所產生的物件進行判斷時，它也會失效。

在不使用建構式的物件建立的情況，它也無法判斷，而且會產生錯誤，所以它並不能使用於像`Object.create`方法，或直接回傳物件的工廠樣式，只能用於建構式樣式。

### in與for...in

`in`運算符是用來判斷某個屬性是否存在於某個物件中，存在的話會回傳true，否則會回傳false。`in`一樣會尋遍整個原型鏈，對比`hasOwnProperty`則是只會在這物件當中，並不會往原型鏈尋找，`in`通常會搭配for使用`for...in`語句。以下這個語法是最常見到的:

```js
for(let key in obj){
    if (obj.hasOwnProperty(key)) {
        console.log(obj[key]);
    }
}
```

### 鴨子類型(Duck typing)

那麼我們要如何正確的判斷一個物件，就是我們需要的物件？首先，`typeof`運算符所能判斷的情況過少，只能用於判斷資料類型，在資料類型那個章節就有提及它的內容。對於物件、陣列、null來說，它都會回傳'object'。

`instaceof`只能用於以`new`實體化的物件，也就是有建構式的情況，而且它有一些失效的情況，`instaceof`有時會直接產生錯誤中斷執行，而不是回傳`false`。`instaceof`並不是不能使用，常見的使用情況是用於判斷語言內建的幾個物件，例如以下幾個:

```js
[1, 2, 3] instanceof Array // true
/abc/ instanceof RegExp // true
({}) instanceof Object // true
(function(){}) instanceof Function // true
```

那如果是在一個函式的傳入參數，要如何判斷這個傳入物件實體是我們要的？

答案就是使用"鴨子類型(Duck typing)"，也就是使用該物件的屬性或行為複合地來判斷它，類似像下面的說明:

> 當看到一隻鳥走起來像鴨子、游泳起來像鴨子、叫起來也像鴨子，那麼這隻鳥就可以被稱為鴨子。

所以當我們要判斷一個自訂的物件，可以用其中應該包含的屬性與方法來判斷，例如以下的判斷函式:

```js
function isPlayer(object) {
  return object != null && typeof object === 'object' &&
    'name' in object && 'age' in object && 'toString' in object
}
```

## new有害說與物件實體化

JavaScript長期以來就有反對使用new運算符用於實體化物件的言論，建議大家不要使用它來作物件的實體化，主要的理由是語言本身設計上的缺陷:

> 忘了在物件實體化時加上new運算符，函式並無明確區分建立物件用的建構式與一般的函式，如果你是要用來實體化物件，而忘了加上new關鍵字，雖然不會產生任何錯誤，但事情會很大條。

以下用幾個方向來討論如何正確的其他作法，以及如何避免其中可能的問題。

### 防止錯誤的語法樣式

這個樣式可以防止程式設計師少加了`new`運算子。一般的JavaScript函式庫並不鼓勵使用它的程式設計師使用`new`，反而會希望用它的程式設計師都使用函式的方式來建立物件實體，原因除了防止漏寫的錯誤外，在函式也有可能會隱藏對於物件實體的複雜的生成過程。以下為範例:

```js
function Player(name, age)
{
   if (this instanceof Player){
      this.name = name
      this.age = age
   }else{
      return new Player(name, age)
   }
}

const aPlayer = Player('Inori', 16)
const bPlayer = new Player('Gi', 16)
```

### Object.create

`Object.create`方法可以使用的是物件的原型來建立物件。這是一個ES5後加入的新方法，最早在10年前在這篇文章[Prototypal Inheritance in JavaScript](http://javascript.crockford.com/prototypal.html)提出的想法與實作。文章中提及`new`本身就是一個為了要讓JavaScript中的物件實體化，用起來像是類別為基礎的程式語言，才會設計的一個語法，但因此模糊了原型繼承的真正作法。

`Object.create`並不只是`new`運算符的取代方法這麼簡單，它提供了更多的彈性，把物件導向的語法結構變成原本的原型導向，它的基本語法如下:

> Object.create(proto[, propertiesObject])

必要的傳入參數是物件的原型，在下面的範例中可以看到。不過要先說明的是為何它把整個語法結構都轉變了。按照`new`運算符來作實體物件的工作，原本的步驟是像下面這樣的:

1. 先撰寫建構函式，定義好裡面的屬性與方法
2. 然後用`new`來建立物件實體

`new`會作的工作已經說明過很多次了，指向`this`到新建立的物件實體、執行建構函式，最後回傳物件實體。如果你比較一下ES6中的類別語法，這個流程與以類別為基礎的物件導向幾乎無異，差異只是在撰寫類別定義與撰寫建構函式定義上，類別定義現在只是個語法糖，還記得嗎？所以當然是一樣的。

那麼原型物件導向原來的流程應該是如何的？原型物件導向有幾個核心的概念:

- 物件繼承自其他物件: 並不是先有物件的藍圖(類別)，然後實體化它。而是先有要作為原型的物件，然後用由這個物件產生新的物件。
- 以合成(或擴充)代替繼承: 合成(或擴充)的方式才是重點，由原型物件開始實體一個新物件，可以很容易的合成與擴充。

`Object.create`方法相較於`new`運算符，在物件實體化過程中有一個非常明顯的差異:

> Object.create不會執行建構函式

`Object.create`的使用流程會是這樣的，這是簡化過的版本，實際上可能不只這樣:

1. 定義一個物件當作原型物件
2. 從這個原型物件建立另一個新的物件(過程可以加入其他的屬性)

以一個最簡單的範例來說，你可以看到`Object.create`完全不使用建構函式來設定新物件實體的屬性值，只是從原型物件產生一個新物件而已。

```js
const PlayerPrototype = {
  name: '',
  toString() {
    return 'Name: '+ this.name
  }
}

const inori = Object.create(PlayerPrototype)
inori.name = 'iNori'
console.log(inori.toString())
```

那麼我們如果要對物件實體進行初始化要怎麼作，其實就是呼叫原型物件裡一個自訂的初始化用的方法就行了，通常會用init來當這個方法的名稱:

```js
const PlayerPrototype = {
    init(name, age) {
        this.name   = name
        this.age  = age
    },
    toString() {
        return 'Name: '+ this.name +' Age:'+ this.age
    }
}

const inori = Object.create(PlayerPrototype)
inori.init('iNori', 16)
console.log(inori.toString())
```

> 註: 這個語法樣式，有個專有名稱叫作OLOO(objects linked to other objects)樣式

> 註: 因為沒有了建構式，所以有一些屬性(例如constructor)與方法(例如`instanceof`)不能使用，會有錯誤的情況。可以用`isPrototypeOf`方法來判斷原型的關係。

`Object.create`方法的第二個傳入參數，可以使用一種特殊的屬性物件(properties Object)，提供更多在物件建立時的彈性運用，例如以下的範例:

```js
const inori = Object.create(PlayerPrototype, {
                hairColor: {
                    value: 'pink',
                    writable: true
                }
              })
inori.init('iNori', 16)

console.log(inori)
```

> 註: 屬性物件是一種用來定義屬性的特殊物件，請參考[Object.defineProperties()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperties)

### 建立物件的語法比較

我們目前為止已經看到物件的建立有以下這三種方式:

```js

//物件字面定義，相等於new Object()
const newObject = {}

//使用Object.create方法
const newObject = Object.create( proto )

//ES6類別定義，或是建構函式。通常稱為建構式樣式。
const newObject = new ConstructorFunc()
const newObject = new ClassName()
```

在進入建立物件的主題前，我想先說明一下，在JavaScript中關於建立物件這件事，是有需要那麼常用到的嗎？或是真的會在單一個應用程式中，建立大量的物件的情況？

你應該了解，JavaScript的應用程式都是執行在瀏覽器的環境中，這是一個有受到限制的執行環境。而且是當一個使用者連到網頁時，JavaScript的應用程式才會透過網站傳遞到使用者電腦中的瀏覽器，然後才執行，這與一般的桌面或手機上的應用程式，先安裝後才執行完全不同。

一般常見程式設計師會在JavaScript應用程式裡，建立不重覆的自訂物件資料的應用情況有可能是以下幾種:

- 網頁上的UI小元件、行事曆、對話盒等等: 一個網頁上頂多是10-20個物件。
- 用來描述資料模型的物件: 用來作為最終的資料交換使用，描述資料的物件，頂多5~10個物件。
- 用於應用程式的物件: 例如一個遊戲中，對於怪物、玩家角色、NPC的這些物件，大概就是20-50個。

以效能來說，物件的建立這件事，不太可能像在網路上測試報告的情況，一次建立幾十萬個或百萬個物件。物件的建立與各種運算，本身就是高消費的，所以在物件的建立，反而效率並不是太重要的課題，而是它在撰寫時的高閱讀性、易於維護與擴充、使用的彈性等等。

以上面的三種物件建立的方式來說，效率最佳的是物件字面定義(花括號({})定義物件)，其次為建構函式加上new運算符這種，通常稱之為"建構式樣式(Constructor Pattern)"，最差的則是`Object.create`。

但物件字面定義語法有一些問題，它只會有一個物件實體。它沒辦法直接複製出其他的物件實體，所以如果是要指"可建立多個物件"的語法，這個並不是可以這樣使用的，它需要寫成一個像下面這樣的函式，才能達到需求，這稱之為工廠樣式(Factory Pattern)的語法:

```js
function PlayerFactory(name, age) {

  return {
    name : name,
    age: age,
    toString : function() {
        return 'Name: '+ this.name +' Age:'+ this.age
    }
  }
}

const inori = PlayerFactory('Inori', 16)
console.log(inori.toString())

const ayase = PlayerFactory('ayase', 17)
console.log(ayase.toString())
```

單純使用物件字面定義的工廠樣式在效率上是吊車尾的，而且由於所有的物件實體都類似於物件字面所定義出來的，它們的原型都是`Object.prototype`。工廠模式也可以用的`Object.create`方法來建立物件實體，搭配物件字面定義出來的物件，建立新的物件實體，上面已經有範例，要用哪一種，都是要視應用的情況而定的。

### 必要的情況(內建物件)

有些JavaScript語言中的內建物件，在使用時一定要用new運算符進行實體化，例如以下幾個:

```js
new Date()
new XMLHttpRequest()
new Error
```

除了這些之外，JavaScript語言中內建的包裝物件幾乎都不使用new作物件實體化，也不建議使用。

### 建構式樣式 vs 工廠樣式

`new`在真實的應用情況也很少會用到，不過我認為主因應該是語法樣式，而非單純只有`new`本身的問題，重點應該放在，對於"建構式樣式"與"工廠樣式"的比較。至少到目前為止的所看到的，"建構式樣式"教得人多，但用得人很少，"工廠樣式"的使用頻率是遠遠勝過"建構式樣式"。

"建構式樣式"是以建構函式式為主的語法樣式，使用new作為物件實體化的唯一方式，最後回傳物件實體。而只能把物件的定義內容寫在建構式之中，只會回傳物件實體，限制住很多能使用的情況。建構函式原本就是一個JavaScript中十分怪異的設計，除了初學者一定很容易和一般的函式搞混，它有一些隱含的機制也很奇特。

"工廠樣式"的語法提供了更多的彈性，相較於"建構式樣式"只能回傳物件，"工廠樣式"最後直接回傳物件實體，但"工廠樣式"也可以多了很多彈性，可以視情況提供各種物件實體的應對程式碼，最後可以回傳以物件字面定義的物件、使用new或Object.create方法。此外，工廠樣式可以對物件資料進行更好的封裝(encapsulation)與資料隱藏(data hiding)，這一點在建構式樣式中完全是個無法比得上的。

### 更多的樣式

#### 原型鏈共享的工廠樣式

工廠樣式提供了更多的彈性，因為`Object.create`直接由一個單純的物件來建立物件，失去了原型鏈的擴充彈性，你可以用下面的樣式來調整:

```js
function Player(){}

Player.prototype.toString = function(){
  return this.name
}

function PlayerFactory(name){
  const obj = Object.create(Player.prototype)
  obj.name = name

  return obj
}
```

實際上Player函式與PlayerFactory函式兩者可以合併，像下面這樣:

```js
function PlayerFactory(name){
  const obj = Object.create(PlayerFactory.prototype)
  obj.name = name

  return obj
}

PlayerFactory.prototype.toString = function(){
  return this.name
}

const inori = PlayerFactory('Inori')
console.log(inori.toString())
console.log(inori)
```

#### 多重繼承(複合)樣式

這個樣式可以建立繼承自多個物件的新物件，用的是`Object.assign`加上`Object.create`方法的語法，這方式可以一次增加多個新物件的屬性與方法，此外`Object.assign`並沒有限定第二個參數之後只能加一個物件進來合併，所以可以加很多物件來合併成為一個新的物件，類似多重繼承的結果。以下為範例:

```js
let player = {
  name: 'player',
  toString() {
   return this.name
  }
}

function PlayerFactory(name, age) {
  return Object.assign(Object.create(player), {
    name: name,
    age: age,
    toString(){
      return 'Name: '+ this.name + ' Age:'+ this.age
    }
  })
}

const inori = PlayerFactory('inori', 16)
console.log(inori.toString())
console.log(inori)
```

#### extend(擴充)樣式

某些時候對於簡單的物件，像是設定值之類的物件，如果都要用到物件實體化或各種語法樣式，實在太過沉重。這個時候用extend(擴充)的方式是快速簡便的，並不一定要額外進行物件實體化，extend(擴充)樣式是一種Mixins(混合)樣式，它並不是繼承或物件實體化的樣式。簡單的extend函式就只是個迴圈語句而已，範例如下(出自[SweetAlert](https://github.com/t4t5/sweetalert)):

```js
var extend = function extend(a, b) {
  for (var key in b) {
    if (b.hasOwnProperty(key)) {
      a[key] = b[key];
    }
  }
  return a;
};
```

> 註: 許多JavaScript函式庫例如jQuery、underscore、lodash都有提供extend(擴充)的API，其他也有像clone(複製)、merge(合併)、assign(指定)之類的用於物件的API

## 結語

JavaScript中原型物件導向設計其實並不難理解，難的是它裡面混雜的太多奇奇怪怪的設計，而且又常要與類別為基礎的物件導向設計相比較。本章除了提供原型鏈的基礎知識說明外，也加入了很多你可能會在實際使用時遇到的樣式，工廠樣式與建構式樣式，這兩個基本的樣式你應該要先熟悉。
