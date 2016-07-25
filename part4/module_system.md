# 模組系統

JavaScript語言長期以來並未內建支援模組系統，社群上發展了兩套知名的模組系統，但它們並不相容:

- CommonJS Modules
- Asynchronous Module Definition (AMD)

ES6中加入了模組系統的支援，它採用了CommonJS與AMD的優點，成了未來JavaScript語言中重要的特性。

## 模組系統是什麼？

當程式碼愈寫愈多，應用程式的規模愈來愈大時，我們需要一個用於組織與管理程式碼的方式，這個需求相當明確，或許不只是應用程式發展到一定程度才會考慮這些，而是應該在開發程式之前的規劃就需要考量進來。

JavaScript語言是一個沒有命名空間設計的程式語言，也沒有支援類似的組織與程式碼分離的設計。有些人認為使用物件定義的字面文字，可以定義出物件的方法與屬性，但如果你看過"物件"、"this"與"原型物件導向"的章節內容，就知道物件中並沒有區分私有、公開成員或方法的特性，這個組織法頂多只是把方法或屬性整理集中而已。

而在很早之前(2003)在社群上發展出一個稱之為模組樣式(module pattern)，以及之後的變型如 暴露模組樣式(Revealing Module Pattern)，就是第一期的程式碼組織方式。模組樣式實作相當簡單，有許多早期開始發展的函式庫或框架採用這個樣式，甚至到今天也可以看到它的使用身影。一個簡單的範例如下(以下範例來自[jQuery](https://learn.jquery.com/code-organization/concepts/)):

```js
// The module pattern
var feature = (function() {
 
    // Private variables and functions
    var privateThing = "secret";
    var publicThing = "not secret";
 
    var changePrivateThing = function() {
        privateThing = "super secret";
    };
 
    var sayPrivateThing = function() {
        console.log( privateThing );
        changePrivateThing();
    };
 
    // Public API
    return {
        publicThing: publicThing,
        sayPrivateThing: sayPrivateThing
    };
})();
 
feature.publicThing; // "not secret"
 
// Logs "secret" and changes the value of privateThing
feature.sayPrivateThing();
```

它使用了IIFE函式的特性，區分出作用域，不過它並沒有辦法徹底解決問題，它在小型的應用程式可以用得很好，但在複雜的程式中仍然有很大的問題，例如以下的問題:

- 沒辦法在程式中作模組載入
- 模組之間的相依性不易管理
- 異步載入模組
- 除錯與測試都不容易
- 在大型專案中不易管理

模組樣式似乎是一個暫時性的解決方案，但不得不說它的確是上一代很重要的程式碼組織方式，第二代的模組系統，是在2009年之後的CommonJS與AMD計劃，它們實作出真正完整的模組系統，CommonJS是專門設計給伺服器端的Node.js使用的，而AMD的目標對象則是瀏覽器端。當然它們兩者的設計有所不同，使用時也可能需要搭配載入工具來一併使用，不過這個階段的模組系統已經是較前一代完善許多，在相依性與模組輸出與輸入，都有相對的解決方式，程式碼的管理與組織方便了許多。

CommonJS與AMD並不會在這裡討論，我們的重點是是ES6中的模組系統，它是一個語言內建的模組系統，而且它可以使用於瀏覽器與伺服器端，這是一個重大的新特性，可以讓你的開發日子更輕鬆許多。


http://exploringjs.com/es6/ch_modules.html

http://www.2ality.com/2014/09/es6-modules-final.html

https://addyosmani.com/resources/essentialjsdesignpatterns/book/#modularjavascript

https://www.nczonline.net/blog/2016/04/es6-module-loading-more-complicated-than-you-think/