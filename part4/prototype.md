# 原型物件導向

> If you don’t understand prototypes,
you don’t understand JavaScript.
> 如果你沒搞懂原型，你不算真的懂JavaScript

JavaScript本身就是原型為基礎的物件導向設計，至今仍沒變動過。在物件的章節中所介紹的類別定義方式，只是原型物件導向語法的語法糖，骨子裡還是原型，不是真正的以類型為基礎的物件導向設計。理解JavaScript的原型，可以看到JavaScript中物件導向設計的原貌，有助於對很多程式碼與概念。

## 函式 - 原型的起手式

所有JavaScript中的函式都有一個內建的`prototype`屬性，指向一個特殊的prototype物件，prototype物件中也有一個`contructor`屬性，指向原來的函式，以下的程式碼可以看出這個關係:

```js
function Player() { }

console.log(Player)
console.log(Player.prototype)
console.log(Player.prototype.constructor)
console.log(Player.prototype.constructor === Player) //true
```

為了更容易理解，以下是一個簡單的關係圖:

![Prototype Image 01](https://raw.githubusercontent.com/eyesofkids/javascript-entry-level-es6/master/assets/prototype_1.png)

再來是`__proto__`這個隱藏屬性，它是一個存取器(accessor)屬性，意思是用getter和setter函式合成出來的屬性，我們可以用來更加深入理解整個原型的樣貌。`__proto__`是每一個JavaScript中物件都有的內部屬性，代表該物件繼承而來的源頭，也就是指向該物件的原型(prototype)，它會用來連接出**原型鏈**，或可以理解為原型的繼承結構。

對於一個函式而言，它本身也是一個物件，它的原型就是最上層的`Function.prototype`，你可以說這是所有函式的原產地。所以`Player`函式本身的`__proto__`指向`Function Prototype`，這應該可以理解。

那麼，`Player.prototype`的`__proto__`指向哪裡？`Player.prototype`本身也是個物件，它指向的就是所有JavaScript中最後的物件起源，也就是`Object.prototype`。由此也可推知，`Function.prototype`也同樣指向`Object.prototype`。以下面的程式就可以看到這個結果:

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

> 註: `__proto__`注意是前後各有兩條下底線(_)，不是只有一條而已。這個屬性的實作一直爭議性的，在一些舊的瀏覽器品牌(例如IE)中不能使用，在這裡只是為了說明之用的，請不要用在真正的應用程式上。在ES6中已經正式納入標準之中，未來所有的瀏覽器品牌與新版本都應該會支援它。

當進一步使用Player函式作為建構函式，產生物件實體時，也就是使用new運算符的語句。像下面這樣簡單的例子，在建構函式中會用this.name的方式來指定傳入參數，this按照之前在物件篇的內容所說明，指向的是new運算符中指定的物件實體。

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

- 每個函式中都會有`prototype`屬性，指向一個`prototype`物件。例如MyFunc函式會有一個對應的MyFunc.prototype物件
- 每個函式的`prototype`物件，會有一個`constructor`屬性，指回到這個函式
- 每個物件都有一個`__proto__`內部屬性，指向它的繼承上游源頭`prototype`物件
- 由`__proto__`指向連接起來的結構，稱之為原型鏈(prototype chain)，也就是原型繼承的整個結構

## 原型真相 - 由問答之中理解

原型到底是什麼，從上面看到原型鏈、new運算符、建構式等等的構造，你可能會有所一些疑惑或誤解。以下是幾個實際重點，我們用問答的方式來說明。

### 原型是什麼？

一種讓別的物件繼承其中的屬性的物件。

### 任何物件都有其原型？

是的。只有一個的例外是Object.prototype(Object的原型)沒有原型，它是所有物件的最初始的源頭。

### 任何物件可以拿來作為原型？

是的。因物件必有其原型，從其原型必可實體化物件。

### 那為什麼上面的例子中，`newPlayer`物件沒有prototype屬性

用Object.getPrototypeOf方法或__proto__屬性，可以找到這個物件真正的原型是什麼，這個prototype屬性是留給建構函式用的。事實上，每個物件都一定有constructor(建構式)屬性，constructor(建構式)屬性會指向建立這個物件的函式，建構函式的屬性其中就有prototype屬性。

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

原型可以很容易的擴充屬性與方法，這也是JavaScript中常見的用來擴充內建物件的語法。當使用`Player.prototype`來進行擴充時，這些擴充出來的屬性與方法，是所有物件共享的，見以下的範例。

```js
function Player(name) {
  this.name = name
}

const newPlayer = new Player('Inori')
console.log(newPlayer.age)

Player.prototype.age = 11
console.log(newPlayer.age)

Player.prototype.toString = function() {
    return this.name
}

console.log(newPlayer.toString())
console.log(newPlayer) //觀察物件的內容
```

## 繼承

繼承是什麼？我們回歸本質上來思考"繼承的目的是什麼"，在程式開發時的目的通常是為了擴充原有的物件定義不足之處。繼承基本上可以依照不同的物件導向特性區分為:

- 類別的繼承(Classical inheritance)
- 原型為基礎的繼承(Prototype-based inheritance)

原型繼承是什麼，其實就是原型的擴充語法，上一節有說明過了。至於類別的繼承方式，在JavaScript可以用Object.create方法模擬出來，不過複雜多了。

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

相當於ES6中的類別定義方法:

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

總而言之，在很多真實的應用情況下，"以合成(或擴充)代替繼承(composition over inheritance)"才是正解，在JavaScript中合成比繼承容易得多了，彈性高應用也很廣，也比較符合語言本身的特性。思考的重點不同，才能撰寫出符合應用情況的程式碼，要不然只會在原有的類別框架中打轉而已。

## 原型鏈(prototype chain)

原型鏈的觀念如此重要，在於有很多物件的行為都是與它相關，在物件篇已經有介紹過物件中的一些方法，都是會遍歷整個物件的原型鏈，而不僅是物件本身而已。這與原型的物件實體化設計有關，因為物件的實體化過程，就是原型的繼承過程，也就是物件的實體化是由繼承其他物件而來的。

先看一下物件屬性的存取這件事，這裡用簡單的`__proto__`屬性指向，來製作出一個物件與它的子物件，

```js
const player = {}
const vipPlayer = {}
vipPlayer.__proto__ = player

console.log(vipPlayer.level); // undefined

player.level = '10'

console.log(vipPlayer.level); //'10'
```

## 參考

- http://aaditmshah.github.io/why-prototypal-inheritance-matters/#toc_9
- https://medium.com/javascript-scene/common-misconceptions-about-inheritance-in-javascript-d5d9bab29b0a#.64lu1pi2e
- https://javascriptweblog.wordpress.com/2010/06/07/understanding-javascript-prototypes/
- http://sporto.github.io/blog/2013/02/22/a-plain-english-guide-to-javascript-prototypes/
- http://www.w3schools.com/js/js_object_prototypes.asp
