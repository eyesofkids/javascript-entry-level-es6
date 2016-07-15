# 錯誤與例外處理

實際上在程式設計領域中，只有"例外處理(Exception Handling)"的說法，而沒有"錯誤處理(Error handling)"。而在其他語言中是有區分的，例如在Java語言中，就有明確區分這兩種不同的物件，它的分別大概是以下這樣:

- Error(錯誤):指的是無法回復的錯誤，經常是因為執行環境造成的錯誤
- Exception(例外):可以用try-catch語句或throw語句捕抓到例外，可以從例外中回復，經常是因為應用程式本身造成的例外

不過，在JavaScript中，Error(錯誤)與Exception(例外)基本上是同一種意思，因為JavaScript的設計中，它以來是在一個受限的執行環境下執行(瀏覽器)，加上只有一個內建的專門作為處理例外用的Error物件，在API中也有一些以error作為名稱的方法。而且，在ECMAScript標準中也沒有很明確的區別，所以常常用錯誤處理作為說明的標題。

> 註: [例外處理(Exception Handling) 維基百科](https://en.wikipedia.org/wiki/Exception_handling)

例外處理的思維是一種"求敗"的程式設計哲學，Exception明確一點來說有"異常"、"並非預期"的意思。程式設計師應該考量許多異常或非預期的情況，在這些情況發生時，能夠加以控制或管理，這種處理稱之為例外處理。

## Error物件

## window.onerror

## 捕抓例外的try...catch語句

## 丟出例外的throw

## 最佳實踐



http://speakingjs.com/es5/ch14.html

https://www.nczonline.net/blog/2009/04/28/javascript-error-handling-anti-pattern/

http://stackoverflow.com/questions/6484528/what-are-the-best-practices-for-javascript-error-handling

http://www.slideshare.net/nzakas/enterprise-javascript-error-handling-presentation

http://stackoverflow.com/questions/7310521/node-js-best-practice-exception-handling/15935021#15935021

http://ruben.verborgh.org/blog/2012/12/31/asynchronous-error-handling-in-javascript/

https://www.joyent.com/node-js/production/design/errors
