# this

在其他以類別為基礎的程式語言中，`this`指的是目前使用類別進行實體化的物件。而JavaScript語言中因為沒有類別這種東西，設計上也不一樣，`this`的指向的是目前呼叫函式或方法的擁有者(owner)物件，也就是說它與函式如何被調用有關，雖然是同一函式的呼叫，也有可能是不同的this值。

有幾個事實要先說明，你可能是一直誤解的幾個部份:

- 所有的函式在呼叫時，其實都有一個擁有者物件來進行呼叫。在瀏覽器的全域中是由window物件來進行呼叫。

## scope VS context

Scope refers to variables within a function, and context refers to the object within which a function is executed.

## scope chain

This is because JavaScript uses static scope; that is, the scope chain of a function is defined at the moment that function is created and never changes; the scope chain is a property of the function.

## Execution context

Execution context is different and separate from the scope chain in that it is constructed at the time a function is invoked (whether directly – func(); – or as the result of a browser invocation, such as a timeout expiring). The execution context is composed of the activation object (the function's parameters and local variables), a reference to the scope chain, and the value of this.

## call stack

Each time a function is called, its parameters and this value are stored in a new 'object' on the stack.

## 什麼是this

this是一個特別的識別用關鍵字，當每個函式被呼叫執行時，會自動產生一個this值，指向呼叫這個函式的擁有者(owner)物件


## 參考

http://themihirchronicles.tumblr.com/post/129081364525/scope-vs-context-in-javascript

http://davidshariff.com/blog/what-is-the-execution-context-in-javascript/
http://davidshariff.com/blog/javascript-scope-chain-and-closures/

http://stackoverflow.com/questions/7721200/using-javascript-closures-in-settimeout/7722057#7722057

http://www.digital-web.com/articles/scope_in_javascript/
http://stackoverflow.com/questions/3127429/how-does-the-this-keyword-work
http://unschooled.org/2012/03/understanding-javascript-this/
https://www.sitepoint.com/mastering-javascripts-this-keyword/

https://www.sitepoint.com/inner-workings-javascripts-this-keyword/

https://www.sitepoint.com/mastering-javascripts-this-keyword/