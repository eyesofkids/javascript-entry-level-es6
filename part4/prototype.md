# 原型物件導向

JavaScript本身就是原型為基礎的物件導向設計，至今仍沒變動過。在物件的章節中所介紹的類別定義方式，只是原型物件導向語法的語法糖，骨子裡還是原型，不是真正的以類型為基礎的物件導向設計。理解JavaScript的原型，可以看到JavaScript中物件導向設計的原貌，有助於對很多程式碼與概念。

## 函式

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

此時在newPlayer物件中的`prototype`與`__proto__`又是如何？

```js
//不是函式，不會有prototype
console.log(newPlayer.prototype)  // undefined

console.log(newPlayer.__proto__)
console.log(newPlayer.__proto__ === Player.prototype) //true
```



