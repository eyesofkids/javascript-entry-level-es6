## 前言

展開運算子(Spread Operator)與其餘參數(Rest parameters)是ECMAScript 6中的其中一種新特性。也是懶人必學的Javascript新語法之一。這兩種特性的語法是一樣的，都是(...)三個點，我們常常在文字聊天時，這個(...)常用來代表了"無言"、"無窮的想像"或"後面其他的"的意思。

簡單摘要一下這個語法的內容：

- 符號是三個點(...)
- 都與陣列有關
- 一個是用在函式的參數，一個是用在運算中

## 其餘參數(Rest parameters)

> 就像在電影葉問中的台詞："我要打十個"，其餘參數可以讓你一次打剩下的全部，不過葉問會變成一個陣列。

既然是一個參數的語法，當然就是用在函式中的參數。其餘參數(Rest parameters)的語法代表是"不確定的傳入參數值"，例如最典型的就是下面這個加總的範例：

```js
function sum(…numbers) {
  var result = 0;
  numbers.forEach(function (number) {
    result += number;
  });
  return result;
}
console.log(sum(1)); // 1
console.log(sum(1, 2, 3, 4, 5)); // 15
```

numbers參數，會把傳給它的"其餘"的所有值，轉換成一個數值陣列。

實際上，在ES5中就有一個隱藏的特性`arguments`可以用，也是可以達到同樣的結果，下面是範例程式：

```js
function sum() {
   var numbers = Array.prototype.slice.call(arguments),
       result = 0;
   numbers.forEach(function (number) {
       result += number;
   });
   return result;
}
console.log(sum(1)); // 1
console.log(sum(1, 2, 3, 4, 5)); // 15
```

不過你可能會想知道這其中的差異。`arguments`包含了所有的函式傳入參數，它是一個類似陣列的物件，並不是純粹的陣列。而使用其餘參數(Rest parameters)語法的參數，則是個把傳入數值的陣列。

其餘參數(Rest parameters)有一個限制，就是這個參數一定是函式的"最後一個"。你如果放在其餘的參數前，就會產生錯誤。

ES6採用了其餘參數(Rest parameters)的特性，一方面是為了改善在這之前，使用`arguments`的語法，很容易造成誤解或混亂，另一方面如果不仔細的看文件說明或程式碼中的使用，會常常眼花不知道在傳入什麼或在用什麼，畢竟`arguments`是一個隱性的屬性值。

## 展開運算子(Spread Operator)

> 展開運算子是"把一個陣列展開(expand)成個別數值"的速寫語法

與上面的其餘參數(Rest parameters)會使用同樣的符號，不過它是個"運算子(Operator)"，常見是用在運算中或陣列中。其餘參數(Rest parameters)是集合(collect)個別的參數成為數值陣列，而展開運算子(Spread Operator)則是把一個陣列展開(expand)成個別的數值。以下是一個簡單的範例：

```js
var params = [ "hello", true, 7 ]
var other = [ 1, 2, ...params ] // [ 1, 2, "hello", true, 7 ]

var str = "foo"
var chars = [ ...str ] // [ "f", "o", "o" ]
```

你也可以用來把陣列展開，傳入函式之中，例如下面這個一個加總函式的範例：

```js
function sum(a, b, c) {
  return a + b + c;
}
var args = [1, 2, 3];
console.log(sum(…args)); // 6
```

對照ES5中的相容語法，則是用`apply`函式，以下是用ES5的同樣結果的範例程式：

```js
function sum(a, b, c) {
  return a + b + c;
}
var args = [1, 2, 3];
console.log(sum.apply(undefined, args)); // 6
```

展開運算子(Spread Operator)可以讓程式碼更容易閱讀，[這篇文章](https://ponyfoo.com/articles/es6-spread-and-butter-in-depth)的作者作了一張比較表把各種常見的使用情況、ES5、ES6列出來，我加了一個字串分割。表格如下：

| 情況 | ES5 | ES6 |
|---|---|---|
| Concatenation(連結) | [1, 2].concat(more) | [1, 2, ...more] |
| String Split | chars = str.split("") | chars = [ ...str ] |
| Push onto list | list.push.apply(list, [3, 4]) | list.push(...[3, 4]) |
| Destructuring | a = list[0], rest = list.slice(1) | [a, ...rest] = list |
| new + apply | new (Date.bind.apply(Date, [null,2015,31,8])) | new Date(...[2015,31,8]) |

展開運算子(Spread Operator)就沒有什麼特別的限制，它的用法就這麼簡單，像是讓你少打一些程式碼的簡寫語法而已。

最後，有可能你會看到用在物件上的類似語法，但它們是還在制定中的ES7，稱為其餘屬性(Rest Properties)與展開屬性(Spread Properties)。像下面這樣的程式碼，[程式碼來源](https://github.com/sebmarkbage/ecmascript-rest-spread)：

```js
// Rest Properties
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
x; // 1
y; // 2
z; // { a: 3, b: 4 }

// Spread Properties
let n = { x, y, ...z };
n; // { x: 1, y: 2, a: 3, b: 4 }
```

> 這種用法目前大量的使用在新式的框架或工具中，例如Redux、React-Native，使用時要注意所需的轉換工具

## 結語

按照最新的[ES6相容表](http://kangax.github.io/compat-table/es6/)看來，目前比較新的瀏覽器和程式執行環境(Node.js等)，都有支援展開運算子(Spread Operator)了，但其餘參數(Rest parameters)的支援度沒有那麼足夠。不過，你現在如果要用這個語法，大概也是要作預先編譯，不然大部份的現行瀏覽器是根本不支援的。

## 參考資源

- [ES6 — default + rest + spread](https://medium.com/ecmascript-2015/default-rest-spread-f3ab0d2e0a5e#.pmsbw2tdb)
- [Features Of ES6 Part 5: The Spread](http://odetocode.com/blogs/scott/archive/2014/09/02/features-of-es6-part-5-the-spread.aspx)
- [Features of ES6 Part 4: Rest Parameters](http://odetocode.com/blogs/scott/archive/2014/08/18/features-of-es6-part-4-rest-parameters.aspx)
- [ES6 Spread and Butter in Depth](https://ponyfoo.com/articles/es6-spread-and-butter-in-depth)
```
var callMe(fn, ...args) {
    return fn.apply(args);
}

callMe(Math.max, 4, 7, 6);
```
