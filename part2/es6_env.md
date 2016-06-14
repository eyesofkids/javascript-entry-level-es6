#ES6環境說明

## 瀏覽器支援

根據[ECMAScript相容表](https://kangax.github.io/compat-table/es6/)，在這本書開始撰寫的同時，2016年6月發佈的Google Chrome v51(Mac平台，桌上型電腦)，在ES6功能的相容性已經高99%。也就是說幾乎完全不需要額外的函式庫或套件，可以直接在瀏覽器上使用與測試ES6功能。不過，還需要一些時間，因為其他的瀏覽器品牌，伺服器端或是行動裝置使用的瀏覽器軟體，目前還有很大一部份的功能沒有實作完成，不過，這也是時間的問題而已。

## babel

[babel](https://babeljs.io/)是一個專為未來性JavaScript語法所設計的編譯工具，目前我們可以先在這段時間中，使用它來將ES6或更新的程式碼，編譯為現行大部份瀏覽器品牌都可執行的語法。這種作法目前大量的在JavaScript界被使用，也就是說我們撰寫程式碼時，可以使用ES6+的新功能和語法，至於測試或發行時，再用babel來編譯即可。

由於babel只編譯(或變換)語法，對於一些新的API或方法是不足的(例如Promise)，所以在使用新的API時，還需要使用babel-polyfill進行補充，對這些新的API提供支援。在使用到這些API的程式碼最上方加入:

```js
import "babel-polyfill"
```

在webpack中只需要把babel-polyfill加入webpack.config.js的entry(進入點)陣列中即可:

```json
module.exports = {
   entry: ['babel-polyfill', './app/js']
};
```
