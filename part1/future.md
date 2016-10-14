# 發展

## ES6標準的未來

ES6標準在2015年確立後，雖然ES6中的新語法與API非常多，在今年(2016)已經有很多桌上電腦的瀏覽器品牌實作完幾乎所有的標準規格。相信在很快的將來，2017年幾乎是所有的平台上的瀏覽器，包含行動裝置、伺服器平台等等，都會實作完成。這代表你今天學習這些新式的語法與API，很快就可以使用在瀏覽器或伺服器平台上。

在ES6標準之前，有很多好用的方法或語法，都是要透過外部的函式庫，或是編譯工具，才能直接執行在瀏覽器上。當ES6標準正式上路後，你可以不需要再使用那些工具或函式庫了，更棒的是效能上因為是瀏覽器的JavaScript引擎直接支援，效能會更好。

ES6也因為它是個真正的JavaScript標準，不管如何，很多大公司或開發週邊的廠商，是一定要支持的，畢竟這是他們其中很多人參與共同討論出來的結果。對於學習者來說，學習標準有很多好處，尤其像現在各家函式庫或框架，發展很快而且各自競爭不相容得很嚴重，有些函式庫或框架還會使用很多更進階的超集語言，例如TypeScript、CoffeeScript等等，學習門檻拉得更高。整體來說，我認為學習ES6標準的一些好處如下:

- 不需要依靠外部函式庫(尤其是工具型的函式庫)，就可以作一樣的事
- 新式的語法有許多非常棒的功能，甚至是其他函式庫與框架沒辦法作或作得不夠的，例如模組系統、箭頭函式、類別等等
- 未來有很多新的機會，例如把舊的程式碼重新改寫為ES6標準的

## 行動裝置

要談到JavaScript語言用於行動裝置開發的發展，就不得不先說目前的兩大生態圈，一是以Facebook為首的ReactJS陣營，ReactJS只是一套視圖用的函式庫，但加入相當創新的想法，使用類似的語法可以使用React Native專案開發手機App，是可以跨iOS與Android平台，React Native開發出來的App是Native(原生)的，它目前是新式的一種開發手機App方式。

另一個是以Google為主的[AngularJS](https://angular.io/)陣營，它是一個完整的應用程式框架，另有一個專案是ionic，也是用來開發跨平台手機App，它還基於另一個專案[Apache Cordova](https://cordova.apache.org/)，這個專案原本就是使用HTML、CSS與JS來開發跨平台手機App的工具專案，這種開發出來的App稱為Hybrid(混合) Apps。

使用Hybrid(混合) Apps的開發工具或專案還有很多，例如jQuery Mobile、Framework 7、Sencha Touch等等，各自有各自使用的框架或函式庫。因為React Native的出現，有許多函式庫開始往原生的路線發展，目前原生的App大多使用[JavaScriptCore](https://developer.apple.com/reference/javascriptcore)作為執行的核心技術。

## 桌面應用程式

[Electron](http://electron.atom.io/)是由Github公司主導的，桌面應用程式開發框架的開放原始碼專案，基於Chromium與Node.js。它原本是用來要開發Atom程式碼編輯工具所使用的專案，原名稱為Atom Shell，後來成為獨立的專案，歷經兩年左右的開發，現在已有很多廠商使用它來開發桌面軟體。

Electron除了也是使用JavaScript語言與HTML作為基礎程式語言，其優點主要是開發出來的桌面應用可以跨作業系統，能在Mac OS X、Windows、Linux上運行，開發的門檻會低很多。不過，缺點是應用程式的檔案大小會比原生的應用大很多，而且執行效能比原生的應用程式差，對電腦資源如CPU與記憶體的需求會較高，目前這些缺點仍然是需要改進的重點。

此外也有類似的如[NW.js](http://nwjs.io/)專案，也是基於Chromium與Node.js的另一個開發平台。

## 伺服器端

Node.js的核心是使用高速的V8 JavaScript引擎，可以運行於一般常見的各種作業系統平台之上。Node.js有效的利用JavaScript語言中非阻塞I/O的優勢，在相同的伺服器資源下，可以提供承載高並行的運算處理。從2009年發展至今，已經逐漸成熟，近年來也發展在多核或多執行緒執行環境下的解決方案。目前已有許多大型的網站採用這個執行環境，例如IBM、Microsoft、Yahoo!、Walmart、Groupon、SAP、LinkedIn、Rakuten、PayPal等等公司。
