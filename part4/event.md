# 事件處理

JavaScript是一個以事件驅動(Event-driven)的程式語言。事件驅動程式設計的主要流程，是由圖形化使用者操作介面(UI)的互動事件為主要核心，藉由事件的觸發動作(滑鼠點按、鍵盤輸入等等)，來決定整體的程式流程。

依據"異步程式設計與事件迴圈"一章的內容中的說明，在JavaScript中有一個不斷偵測事件發生的事件迴圈(Event Loop)，所有在網頁上的DOM元素註冊的事件，都會進入一個佇列(queue)中，等待被觸發事件，然後作出相對的回傳送還給使用者。

https://w3c.github.io/uievents/#ui-events-intro

http://www.quirksmode.org/js/this.html
http://d.hatena.ne.jp/ynakajima/20090207/p1