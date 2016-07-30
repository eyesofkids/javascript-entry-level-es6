# 解構賦值

## 前言

解構賦值(Destructuring assignment)是一個在ES6中新的特性語法，用於取出陣列或物件中的資料。它的解譯只有一小段英文：

> The destructuring assignment syntax is a JavaScript expression that makes it possible to extract data from arrays or objects using a syntax that mirrors the construction of array and object literals.

這句後面的`mirrors the construction of array and object literals`，代表這個語法的使用情況 - 如同鏡子一般，對映出陣列或物件記號上的結構出來。也就是樣式(pattern)式的語法。

解構指派(Destructuring assignment)很容易就可以學習，但也有很深入的用法，尤其是與ES6其他的特性配合時會顯得複雜不易理解。這篇文章只整理一些初步的說明而已。愈到後面就稍微複雜些，也會更佳挑戰你對Javascript的基礎知識。這個特性在其他的程式語言中應該是早就有了，學過來用的。

> Destructuring: 變性、破壞性。使用"解構"是對照`de-`字頭有"脫離"、"去除"的意思

> assignment: 賦值、指派。賦值通常指的是程式中使用等號(=)運算敘述。

基本的語法有三種情況，第一種是從陣列解構賦值，第二種是從物件解構或是從混用物件或陣列解構賦值，第三種是非物件或陣列解構賦值的情況。

## 從陣列解構賦值(Array destructuring)

從陣列解構賦值沒太多學問，唯一比較特別的是可以用其餘參數(rest parameters)的語法。看看幾個範例你就可以很容易的理解，下面是個簡單的幾個範例：

```js
var [a, b] = [1, 2]; //a=1, b=2

// 略過某些值(omitting certain values)
var [a, , b] = [1, 2, 3]; // a=1, b=3

// 其餘參數(Combine with rest)
var [a, ...b] = [1, 2, 3]; //a=1, b=[2,3]

// 失敗保護(Fail-safe)
var [, , , a, b] = [1, 2, 3]; // a=undefined, b=undefined

// 交換值(Swap)
var a = 1, b = 2;
[b, a] = [a, b]; //a=2, b=1

// 多維複雜陣列(Advance deep arrays)
var [a, [b, [c, d]]] = [1, [2, [[[3, 4], 5], 6]]];

// 字串
const str = "hello";
var [a, b, c, d, e] =str;
```

用法就是這麼簡單，賦值的(等號)左邊按照你寫的樣式，然後在右邊寫上對應數值樣式，就像之前說的"鏡子"般的對應。沒賦到值就會得到`undefined`。

## 從物件解構賦值(Object destructuring)

在Javascript中的物件除了有特別的記號({})外，還有屬性名稱，按照基本的原則，也是用像"鏡子"般的樣式對應，不過它會有兩種對映到的情況。一樣看範例就很容易理解：

```js
// 基本用法(basic)
var {user: x} = {user: 5}; // x=5

// 失敗保護(Fail-safe)
var {user: x} = {user2: 5}; //x=undefined

// 賦予新的變數名稱(Assign new variable names)
var {prop: x, prop2: y} = {prop: 5, prop2: 10}; // x=5, y=10

// 屬性賦值語法
var { prop: prop, prop2: prop2} = {prop: 5, prop2: 10}; //prop = 5, prop2=10

// 相當於上面簡短語法(Short-hand syntax)
var { prop, prop2} = {prop: 5, prop2: 10}; //prop = 5, prop2=10
```

下面的語法是個有陷阱的語法，這是錯誤的示範：

```js
// 錯誤的示範(This doesn't work):
var a, b;
{ a, b } = {a: 1, b: 2};
```

因為在Javascript語言中，雖然使用花括號符號({})是物件的宣告符號，但這個符號用在程式敘述中，也就是前面沒有let、const、var這些宣告字詞時，則是代表程式碼的區塊(block)。在外面再加上括號符號(())就可以改正，正確的寫法如下：

```js
var a, b;
({ a, b } = {a: 1, b: 2}); //a=1, b=2
```

複雜的物件或混合陣列到物件，如果你能記住之前說的鏡子樣式對映原則，其實也很容易就能理解：

```js
// 混用物件與陣列(Combine objects an arrays)
var {prop: x, prop2: [, y]} = {prop: 5, prop2: [10, 100]};
console.log(x, y); // => 5 100

// 複雜多層次的物件(Deep objects)
var {
  prop: x,
  prop2: {
    prop2: {
      nested: [ , , b]
    }
  }
} = { prop: "Hello", prop2: { prop2: { nested: ["a", "b", "c"]}}};
console.log(x, b); // => Hello c
```

## 從非陣列或非物件解構賦值(Destructuring Values that are not an Object or Array)

從其他的數值類型解構，首先是null或undefined這兩個，你會得到錯誤：

```js
var [a] = undefined;
var {b} = null;
//"TypeError: Invalid attempt to destructure non-iterable instance
```

如果是從其他的主要數值類型boolean、number、string等作物件解構，則會得到undefined值。

```js
var {a} = false;
var {b} = 10;
var {c} = "hello";

console.log(a, b, c); // undefined undefined undefined
```

作陣列解構的話，字串類型可以解出字元，其他也是得到undefined值：

```js
var [a] = false;
var [b] = 10;
var [c] = "hello"; //c="h"

console.log( a, b, c);
```

> string可以解構成為char，請看上面的陣列範例。字串類型只能用在陣列解構，物件解構是無法的。

會有如此的結果，我找到的解說是，當一個數值要被進行解構時，它會被轉成物件(或陣列)，因為null或undefined無法轉成物件(或陣列)，所以必定產生錯誤。而這個數值轉換的物件(或陣列)如果沒有帶對應的迭代器(Iterator)，就無法被解構，所以回傳undefined。

## 預設值

在等號左邊的樣式(pattern)中是可以給定預設值的，作為賦不到值時的預設數值。

```js
 var [missing = true] = [];
 console.log(missing);
 // true

 var { message: msg = "Something went wrong" } = {};
 console.log(msg);
 // "Something went wrong"

 var { x = 3 } = {};
 console.log(x);
 // 3
```

要作一個簡單的陷阱題滿簡單的，實作一下下面這個程式碼到底是賦到了什麼值：

```js
var {a="hello"} = "hello";
var [b="hello"] = "hello";

console.log( a, b);
```

## 在函式參數中使用

我一直認為Javascript中的函式，是個不太正常的東西，或許它的使用情況非常多，所以常常難以掌握最後的結果是什麼，尤其是用在物件裡面時。

一切是如此的簡單與美好，一個簡單的解構賦值用在函式的參數裡：

```js
function f({a, b}) {
  return a + b;
}

f({a: 1, b: 2}); // 3
```

當你用上了預設值的機制，世界開始變了調。前面的a有預設值，後面的b就沒有：

```js
function f({a = 3, b}) {
  return a + b;
}

f({a: 1, b: 2}); // 3
f({b: 2}); // 5
f({a: 1}); // null
f({}); // null
f(); // Cannot read property 'a' of undefined
```

當兩個都有預設值時，情況就會變得好多了。前面a和後面的b都有了預設值：

```js
function f({a = 3, b = 5}) {
  return a + b;
}

f({a: 1, b: 2}); // 3
f({a: 1}); // 6
f({b: 2}); // 5
f({}); // 8
f(); // Cannot read property 'a' of undefined
```

實際上函式它自己也可以加預設值，但這情況又和解構賦值中的預設值不大一樣，合著用有另一翻風情，至少怎麼用都會有結果：

```js
function f({a = 3, b = 5} = {}) {
  return a + b;
}

f({a: 1, b: 2}); // 3
f({a: 1}); // 6
f({b: 2}); // 5
f({}); // 8
f(); // 8
```

另一種情況是在函式的預設值中給了另一套數值的預設值，這只會在是在什麼都沒有的`f()`發揮它的作用：

```js
function f({a = 3, b = 5} = {a: 7, b: 11}) {
  return a + b;
}

f({a: 1, b: 2}); // 3
f({a: 1}); // 6
f({b: 2}); // 5
f({}); // 8
f(); // 18
```

你可以觀察一下，當對某個變數賦值時你給他null或void 0，到底是用預設值還是沒有值，這個範例的`g()`函式是個對照組：

```js
function f({a = 1, b = 2} = {a: 1, b: 2}) {
  return a + b;
}

f({a: 3, b: 5}); // 8
f({a: 3}); // 5
f({b: 5}); // 6
f({a: null}); // 2
f({b: null}); // 1
f({a: void 0}); // 3
f({b: void 0}); // 3
f({}); // 3
f(); // 3

function g(a = 1, b = 2) {
  return a + b;
}

g(3, 5); // 8
g(3); // 5
g(5); // 7
g(void 0, 5); // 6
g(null, 5); // 5
g(); // 3
```

> 註：所以在函式中作解構賦值時，給定變數null值時會導致預設值無用，請記住這一點

## 實例應用

這個範例用了for...of語法。出自[Destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)：

```js
var people = [
  {
    name: "Mike Smith",
    family: {
      mother: "Jane Smith",
      father: "Harry Smith",
      sister: "Samantha Smith"
    },
    age: 35
  },
  {
    name: "Tom Jones",
    family: {
      mother: "Norah Jones",
      father: "Richard Jones",
      brother: "Howard Jones"
    },
    age: 25
  }
];

for (var {name: n, family: { father: f } } of people) {
  console.log("Name: " + n + ", Father: " + f);
}

// "Name: Mike Smith, Father: Harry Smith"
// "Name: Tom Jones, Father: Richard Jones"
```


這個範例混用了一些ES6的語法，出自[Several demos and usages for ES6 destructuring.](https://gist.github.com/mikaelbr/9900818)：

```js
// Combine with other ES6 features.
var ajax = function ({ url = "localhost", port: p = 80}, ...data) {
  console.log("Url:", url, "Port:", p, "Rest:", data);
};

ajax({ url: "someHost" }, "additional", "data", "hello");
// => Url: someHost Port: 80 Rest: [ 'additional', 'data', 'hello' ]

ajax({ }, "additional", "data", "hello");
// => Url: localhost Port: 80 Rest: [ 'additional', 'data', 'hello' ]

var ajax = ({ url: url = "localhost", port: p = 80}, ...data) => {
  console.log("Url:", url, "Port:", p, "Rest:", data);
};
ajax({ }, "additional", "data", "hello");
```

這個是React Native的一個教學，裡面有用了解構賦值的語法，有些程式碼喜歡用這樣寫，一行一個賦值的變數值。出自[React Native Tutorial: Building Apps with JavaScript](http://www.raywenderlich.com/99473/introducing-react-native-building-apps-javascript)：

```js
'use strict';

var React = require('react-native');
var {
  StyleSheet,
  Text,
  TextInput,
  View,
  TouchableHighlight,
  ActivityIndicatorIOS,
  Image,
  Component
} = React;
```

## 參考資料

- [Several demos and usages for ES6 destructuring.](https://gist.github.com/mikaelbr/9900818)
- [Destructuring Assignment in ECMAScript 6](http://fitzgeraldnick.com/weblog/50/)
- [Destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
- [Destructuring assignmentのご利用は計画的に](http://qiita.com/armorik83/items/ec3210a1942471fba612)
