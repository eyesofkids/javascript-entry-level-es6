# 控制流程

## Expression(表達式)與Statements(語句)

Expression(表達式)是各種值(運算元)與運算符的組合體，組合起來進行運算然後產生值。

> Expression即任何合法的可計算產出值的程式碼單位

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

Statements(語句)可以以不同情況的使用進行分類，以下列出:

- 控制流程(Control flow)
- 定義(Declarations)
- 函式與類別(Functions and classes)
- 迭代、迴圈(Iterations)
- 其他(Others)

> 註: 此分類參考[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements)的分類方式，正式的ECMAScript分類並不是這樣。

> 註: Iterations這個字詞，中文一般是翻譯為"迭代"這個字，這個翻譯過於專業，對初學者來說是有看沒有懂。比較好的理解是它有"重覆作某件事"的意思，也就是和"迴圈"、"重覆"的意思相近。

### Side Effect(副作用)

你如果有去醫院看過醫生拿過藥，在藥包上都會註明這個藥物的可能副作用。Side Effect(副作用)這個字詞，經常會出現在很多進階使用的JavaScript框架或函式庫之中，它基本上算是一種概念，在表達式中有使用這個概念來分類。Expression(表達式)就上面所說的，是用來求出(產生)值的，但一般的Expression(表達式)是在指定值的運算，在指定原本的變數/常數的Expression(表達式)值時，如果會變動原本的變數/常數，就稱為是"**具有Side Effect(副作用)**"的Expression(表達式)，例如像最典型的具有副作用的表達式的範例:

```js
counter++
x += 3
y = "Hello " + name
```

沒有副作用的表達式的範例，它們大概都只是字面文字(literal)或變數/常數名稱而已:

```js
3 + 5
true
1.9
x
'Hello World'
```

Side Effect(副作用)除了會在表達式中看到，我們在函式中也會再見到它。這裡就先大概了解哪些是有副作用，而哪些是正常的求出值的表達式。至於為什麼要這樣分別，之後再說明它的原因了。

## 控制流程

控制流程(flow control)語句是一種特殊的語句，它與程式的執行流程有關，會因為其中的判斷結果不同，導致不同的執行結果。

### 區塊(block)語句

區塊(block)語句可以組合多行語句，或用於分隔不同語句，區塊(block)語句經常使用於控制流程的語句之中。使用花括號(curly brackets)標記({})，將多行語句包含在其中:

```js
{
  statement_1;
  statement_2;
  ...
  statement_n;
}
```

### if...else語句

`if...else`是常見的控制流程的語句，在中文的意思就是"如果...要不然"。所以整體的結構就像是中文的意思"如果aaa命題為真情況下，就xxx，要不然就ooo"。這個aaa就是一個判斷情況(condition)，判斷情況使用的都是比較運算符(Comparison operators)與邏輯運算符(Logical Operators)，而結果則是會用布林的`true`與`false`值來判斷，但要特別注意之前說明的"falsy"情況，這些在判斷情況中會自動轉成布林的`false`。

一個簡單的`if...else`語句的範例如下:

```js
const x = 10

if (x > 100) {
    console.log('x > 100')
} else {   
    console.log('x < 100')
}
```

> 註: `if...else`並不是寫了`if`就一定要有`else`，也可以單獨只用`if`語句，但`else`不能單獨使用。

`if...else`語句有幾個常見的延申結構，例如多個判斷情況時可以再其中加入`else if`。不過當你在寫這些判斷情況時，最好是讓它有真的會出現的情況，以免寫出根本不會有的判斷情況，成為程式碼的多餘部份:

```js
const x = 10

if (x > 100) {
    console.log('x>100')
} else if( x < 50){
    console.log('x<100 but x>50')
} else {   
    console.log('x < 50')
}
```

另一種是巢狀的語法結構，也就是進入某個判斷情況下，可以在裡面再寫出另一個`if...else`語句，例如:

```js
const x = 10

if(x > 100) {
    if(x > 500){
        console.log('x > 500')
    }else{
        console.log('x > 100 but x < 500')
    }
} else if( x < 50){
    console.log('x < 100 but x > 50')
} else {   
    console.log('x < 50')
}
```

在判斷後的結果語句，如果是只有一行，或是很簡單的幾個字元的語句，可以不需要配合區塊語句({})。而單獨使用`if`時，有些設計師會用空一格然後把判斷情況與結果語句寫在同一行，不過要注意不要寫太長的語句，雖然沒有明確的規定一行只能用幾個字元。例如:

```js
const x = 10

//去除block statement
if(x > 100) 
    console.log('x > 100')
else 
    console.log('x < 50')

//寫成一行
if(x === 10) console.log('x is 10')
```

而在使用`if`加上`else`時，為了簡化語句，使用三元運算符(Conditional (ternary) Operator)，來讓程式碼更簡潔，注意這也是用於簡單的判斷情況與語句時:

```js
const x = 10

(x > 100) ? console.log('x > 100') : console.log('x < 50')
```

> 注意: 建議使用三元運算符在單一行的語句上，雖然它不受這個限制，事實上可以使用在多行與巢狀結構，但這是個不建議的壞習慣。

由於三元運算符的語法很簡單，它也常被配合用在指定值的語句中，例如:

```js
const foo = value1 > value2 ? 'baz' : null
```

這個有點像我們之前教過的，用邏輯或運算符(||)來指定變數/常數預設值的語句。不過如果是單純的判斷某個值是否存在，然後設定它為預設值，用邏輯或運算符(||)是比較推薦的，例如:

```js
//相當於 const foo = a ? a : b
const foo = a || b
```

> 心得提示: 因為人生還很長，而我們需要寫的程式碼還有很多，所以你會本書上看到有很多感覺上像是偷懶或密技的簡短語法，或許它並不是出現在標準中，也不是課堂上會教的"正規"寫法，但的確是廣泛被使用的一種語法。

#### 比較運算符

比較運算符我們在前面的課堂上已經有介紹過一部份，以及相等比較的概念。以下將它大略分成幾個分類來說明:

- 相等比較運算: 相等(Equality)(==)，完全一致(Identity)(===)。不相等(Inequality)(!=)，不完全一致(Non-identity)(!==)。
- 關係比較運算: 大於(>)，小於(<)，大於等於(>=)，小於等於(<=)

相等比較運算時，需要遵守標準中所定義的"抽象相等比較演算法"(也就是(==)與(!=)符號的比較)與"嚴格相等比較演算法"(也就是(===)與(!==)符號的比較)的規定。相等的(equal)運算符(==)，它只能比較值的部份，而無法比較資料類型，也就是說`'1'==1`的結果為`true`這就不足為奇了。

抽象相等比較演算法(也就是(==)與(!=)符號的比較)，來說它的規則如下，x是左邊的運算子，而y是右邊的運算子:

- Type(x)與Type(y)的類型相同時，進行嚴格相等比較
- 數字與字串比較時，字串會自動轉成數字再進行比較
- 其中有布林值時，true轉為1，false轉為+0
- 當物件與數字或字串比較時，物件會先轉為預設值以及轉成原始資料類型的值，再作比較

至於嚴格相等比較演算法(也就是(===)與(!==)符號的比較)，由於一定要比較原始資料類型的類型，只要類型不同就一定是回傳為false。除了類型相同，值也要相等，才會有回傳`true`的情況。

> 註: 物件的情況會比較特別，這個我們會在專門說明物件類型的章節再詳細說明。

你如果之前已經有"falsy"與"truthy"的概念，就知道很多比較運算的結果都會以這個為基礎轉變為布林的`false`或`true`值。在`if`中的判斷情況可以直接運算單個值，這就是依照"falsy"與"truthy"的概念，或是搭配 邏輯反相Logical NOT符號(!)來回傳反轉的布林值，例如:

```js

if(undefined) console.log('true') //false

if(null) console.log('true') //false

if(+0) console.log('true') //false

if(!'') console.log('!\'\' true') //true

if(123) console.log('123 true') //true

//下面的還沒教到，是空物件與空陣列
const a = {}
const b = []

if(a) console.log(a, 'true')
if(b) console.log(b, 'true')
```

#### 邏輯運算符

邏輯運算符實際上在前面的課程都已經介紹過了，只有三個而已:

- 邏輯與Logical AND(&&)
- 邏輯或Logical OR(||)
- 邏輯反相Logical NOT (!)

邏輯與Logical AND(&&) 與 邏輯或Logical OR(||) 兩個符號可以組合多個不同的比較運算，然後以邏輯運算的"與"與"或"來作最後的布林值的運算。不過要注意的是，它們的運算回傳值，在JavaScript中並非布林值，而是**最終的值**，轉換為布林值是判斷情況運算的作用。

這兩個運算符通常會搭配群組運算符(())，也就是左右括號標記(())，這相當於在數學運算上作為優先運算的區分，或是作為區隔不同比較運算。以下為範例:

```js
const x = 50
const y = 60
const z = 100

if((x > 10) && (x < 100)) console.log(true)
if(( ((x + y) > 10) || (x === 50) ) && (z == 100)) console.log(true)
```

> 註:(&)符號英文為And或Amphersand。(|)符號的英文在電腦上通常稱為Pipe，管道的意思。(!)在英文中稱為Exclamation mark，驚嘆號的意思。

#### 風格指引

- (Airbnb 15.1) 
- (Airbnb 16.1)

### switch語句

switch語句有一說法，是可相當於多個`if...else`的組合語句，也就是用於判斷情況有很多種不同的回傳值的組合情況。但實際上，它最常被使用的是在完全一致相等值(===)比較的情況下，很少用在不是單純相關比較運算的情況下。基本上它的語法結構如下:

```js
switch (expression) {
  case value1:
    //符合運算得到value1的執行語句
    break;
  case value2:
    //符合運算得到value2的執行語句
    break;
  ...
  case valueN:
    //符合運算得到valueN的執行語句
    break;
  default:
    //符合運算得到其他值的執行語句
    break;
}
```

舉一個簡單的範例來說，像下面這樣的`if...else`語句:

```js
const x = 10

if(x > 100) 
    console.log('x > 100')
else if(x < 100 && x >50) 
    console.log('x < 100 && x >50')
else 
    console.log('x < 50')
```

轉換為switch語句會變成像下面這樣，`switch(true)`代表當`case`中的比較運算需要為布林值的`true`時，才能滿足而執行其中包含的語句:

```js
const x = 10

switch(true){
    case (x >100):{
        console.log('x > 100')
        break
    }
    case (x < 100 && x >50):{
        console.log('x < 100 && x >50')
        break
    }
    default:{
        console.log('x < 50')
        break
    }
}
```

而`switch`語句最常被使用的情況，是用於判斷相等確定值的情況下，在這時候`case`語句裡的比較結果的相等於完全一致(Identity)(===)的比較結果，在滿足的情況下才會執行包含在`case`其中的語句，例如:

```js
const x = 10

switch(x){
    case 100 :{
        console.log('x is 100')
        break
    }
    case 50:{
        console.log('x is 50')
        break
    }
    case 10:{
        console.log('x is 10')
        break
    }
    default:{
        console.log(x)
        break
    }
}
```

> 注意: 因為case語句中的值會以一致相等運算來比較，所以資料類型也要完全相等才行，上面的範例如果把`case 10:`改為`case '10':`，就會輸出`10`，而不是`x is 10`

`break`關鍵字雖然在語法上是可選的，但`case`語句沒有搭配`break`會一直執行到出現`break`或是`switch`整個語句的結尾，這通常會出現不正確的結果，像下面這個範例:

```js
const x = 50

switch(x){
    case 100 :{
        console.log('x is 100')
        break
    }
    case 50:{
            console.log('x is 50')
            //這邊少一個break
    }
    case 10:{
            console.log('x is 10')
            break
    }
    default:{
        console.log(x)
        break
    }
}
```

最後的結果會是:

```
x is 50
x is 10
```

> 註: 所以不管如何，只要case語句中有包含其它需要執行的語句，一定需要以break作為結尾。

判斷時有多個`case`情況而執行同一個語句時，會使用像下面這個範例的語法，這個語法結構也是很常見的用法，例如:

```js
const fruit = '芒果'

switch (fruit)
{
   case '芭樂':
   case '香蕉': {
       console.log(fruit, '是四季都出產的水果')
       break
   }
   case '西瓜':
   case '荔枝':
   case '芒果':
   default:{
       console.log(fruit, '是只有夏季出產的水果')
       break
   }
}
```

#### 風格指引

- (Airbnb 15.5) Use braces to create blocks in case and default clauses that contain lexical declarations (e.g. let, const, function, and class).
- `switch`語句的`default`語句的`break`功能上並不需要，只是讓程式碼的撰寫風格延續統一而已。`defalt`語句通常也是放在`switch`語句的最後一組。

## 英文解說

表達式(expression)

Statements

context

literal


## 暫存

http://stackoverflow.com/questions/27509/detecting-an-undefined-object-property?page=1&tab=votes#tab-top

http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html