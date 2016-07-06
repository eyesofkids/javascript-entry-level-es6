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

再來是`__proto__`這個隱藏屬性，它是一個存取器(accessor)屬性，意思是用getter和setter函式合成出來的屬性，我們可以用來更加深入理解整個原型的樣貌。`__proto__`是每一個JavaScript中物件都有的內部屬性，代表該物件繼承而來的源頭，也就是指向該物件的原型(prototype)，它會用來架構出原型鏈，或用原型繼承結構來理解。

對於一個函式而言，它本身也是一個物件，它的原型就是最上層的`Function.prototype`，你可以說這是所有函式的原產地。所以`Player`函式本身的`__proto__`指向`Function Prototype`，這應該可以理解。

那麼，`Player.prototype`的`__proto__`指向哪裡？`Player.prototype`本身也是個物件，它指向的就是所有JavaScript中最後的物件起源，也就是`Object.prototype`。由此也可推知，`Function.prototype`也同樣指向`Object.prototype`。以下面的程式就可以看到這個結果:

```js
function Player() { }

console.log(Player.__proto__)
console.log(Player.prototype.__proto__)

console.log(Player.__proto__ === Function.prototype)  //true
console.log(Player.prototype.__proto__ === Object.prototype) //true
console.log(Function.prototype.__proto__ === Object.prototype) //true
```

為了更容易理解，以下是一個簡單的關係圖，在圖片中綠色的虛線即是`__proto__`的指向，原本的`prototype`為紅色的實線:

![Prototype Image 02](https://raw.githubusercontent.com/eyesofkids/javascript-entry-level-es6/master/assets/prototype_2.png)


> 註: `__proto__`屬性的實作一直是有爭議性的，在一些瀏覽器品牌(例如IE)中不能使用，在這裡只是為了說明之用的，請不要用在真正的應用程式上。不過，在ES6中已經正式納入標準之中，未來所有的瀏覽器品牌與新版本都應該會支援它。


## 原型鏈

對於一個物件來說，我們需要有如何定義它的方式，用物件字面文字的定義是很直覺得，這在物件篇中也有提過了。例如以下的例子來說:

```js

```

