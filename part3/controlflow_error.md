# 控制流程

Expression(表達式)是各種值(運算元)與運算符的組合體，組合起來進行運算然後產生值。

> Expression代表任何合法的可解出值的程式碼單位

簡單的表達式如下，這看起有點像某行程式碼的一部份:

```
x = 7
3 + 5
```

Statements(語句、陳述)是在程式語言中，一小段功能性的程式碼，語句中包含了關鍵字與合法的語法(Syntax)。在JavaScript語言中，傳統是以半形分號(;)作為代表結束與分隔其他的語句，但同樣也可以以分行來區分。撰寫一支程式，就如同在寫一篇文章時，其中會包含了各種描述語句。

```js
//Comment is a statement

//assign value is a statement
const x = 10

//block statement
{
  statement_1;
  statement_2;
  ...
  statement_n;
}

//function statment
function name() {
   [statements]
}
```

> Statements(語句)可以視為在JavaScript的最小獨立執行程式碼組合

在JavaScript語言中，Expression(表達式)主要用來產生"值"，因為它的功用很特殊，通常會獨立出來說明。而一般的Statements(語句)主要功能是執行動作或定義某種行為，例如之前說過的註解(Comment)就是一種語句。

另外為了組合多行的語句，採用了區塊的

控制流程(flow control)敘述是一種特殊的敘述，它與程式的執行流程有關，會因為其中的判斷結果不同，導致不同的執行結果。

### (Side Effect)副作用


## if / else敘述

### 邏輯運算符

## switch敘述

## 英文解說

表達式(expression)

context




## 暫存

http://stackoverflow.com/questions/27509/detecting-an-undefined-object-property?page=1&tab=votes#tab-top

http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html