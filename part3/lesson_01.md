# 值與符號

## 可變與不可變

程式語言充份反應了人類社會的情況，因為它是人類所設計的，也是設計給人類所使用的。

程式語言中需要以一個標示，代表這個用於存放值(value)的代表名稱，在這個程式執行的生命週期中，它是可變或是不可變的。範例如下:

```js
//這是不變的
const a=10

//這是可變的
let b=5
```
你可把常數和變數想像是一個盒子:

- 常數const(卡死): 裝入東西(值)之後就封起來的盒子
- 變數let(累): 暫時存放值的盒子，盒子是打開的，可以更動裡面的值

尤其Javascript程式語言的程式設計師，在之前常會犯有不宣告就直接使用的壞習慣。我建議你最好在宣告常數或變數時，就給定一個值。雖然變數並沒有要求一開始要給定值，常數才有這個要求。

> "不能再次指定數值"(not re-assigned)，與 "無法改變其中數值"(immutable)，是有差異的概念。常數指的是前面那種意思。Javascript中並沒有immutable的內建特性。
> ES5以前都只會用"var"(肉<台>)作為標記，它是variable(買<台>里哦波)的縮寫。"let"與"const"是ES6的新加入特性，你應該開始使用它們。

> 本書的所有程式碼，每行後面都"不"再使用分號(semicolon)(;)作為程式碼段落之用，只需要記得分行就行了。現在的Javascript可以不需要用分號來分行。

### 英文解說

const(卡死)是constant(卡死撐)的縮寫，中譯是"常數"的意思。

let(累)，中譯是"讓..."，
例如"let's go"大概是"大家一起走吧"或"please let me go"(讓我走吧...其實是常用在男女朋友"分手"的意思) 

### 風格指引

- (crockford)(airbnb 13.2)建議一行一個變數(或常數)宣告與註解，然後最好是按英文字母排列。
- (crockford)在函式中的最前面的敘述宣告變數。(註：不要在程式碼中間的敘述來宣告)
- (airbnb 13.3)把let宣告的放在一起，const宣告的放在一起。
- (google)總是使用var來宣告。(註：變數記得要宣告)
- (google)使用NAMES_LIKE_THIS這樣的命名方式，來命名常數。

## 命名

我們需要先為這些數值的代表名稱命名，稱為常數名稱(constant name)或變數名稱(variable name)。

### 命名規則

- The first character must be an ASCII letter (either uppercase or lowercase), or an underscore (_) character. Note that a number cannot be used as the first character.
- Subsequent characters must be letters, numbers, or underscores (_).
- The variable name must not be a reserved word.

### 好的變數（函式、類別）命名

#### 變數與函式，都用小駝峰式的命名。類別用大駝峰式命名。 

```js
let numberOfStudents
const numberOfLegs
function setBackgroundColor() 
class Student{}
```

#### 少用簡寫或自行發明縮寫

- 不好的命名：setBgColor / 好的命合：setBackgroundColor
- 不好的命名：userAdr / 好的命名：userAddress
- 不好的命名：fName, lName / 好的命名：firstName, lastName

#### 語意不明或對象不明

- 不好的命名：insert() 這是要插入什麼？/ 好的命名：insertDiv()
- 不好的命名：isOk() 什麼東西ok不ok？ / 好的命名：isConnected 連上線了嗎？

#### 不要拼錯英文單字

- 錯誤的拼字：memuItem / 正確的拼字：menuItem
- 錯誤的拼字：pueryString / 正確的拼字：queryString

#### 英文單複數

陣列之類的集合結構，有數量很多的意思，大部份都用「複數」或會加上資料的類型來分別，例如：
```
studentArray students
```
執行的動作（函式或方法），如果針對單一個變數的行為，用單數：
```
addItem()
```
如果針對多個數的行為，用複數：
```
addItems()
```
針對全體的行為，會用「All」：
```
removeAll()
```

#### 常見的英文計量字詞：

count numberOf amountOf price cost length width height speed

#### 常見的布林值開頭字詞：

isEmpty hasBasket

#### 常見的字串值開頭字詞：

string name description label text

#### 常用的動作詞（函數用）開頭

- make take 作某…事
- move 移動
- add 加上、相加   
- delete/remove 移除
- insert 單體 splice 複合體
- extend append 展開
- set 設定
- get 獲得
- print 印出
- list 列出
- reset 重置
- link 連至
- repeat 重覆
- replace 取代
- find search 尋找
- xxxxTo 到xxx

#### 具時間意義的英文字詞

- will 通常指即將發生但未發生
- did 已發生
- should 應該發生

## 註解 Comment

註解Comment(康門)的意思是要讓未來的自己或其他人很快的理解，這段程式碼是在作什麼用的。所以在重要的程式碼的地方(通常在上面)，加上註解是好的習慣。註解也常常用在暫時性的把某段程式碼先擋住，讓它不執行。不管你的英文有多破，用英文註解是比較好的。


單行註解用的是雙斜線(//)(double slash，打波 死內需)符號。以下為範例:

```js
//我是一個單行註解
```

多行註解用的是`/*...*/`符號，這也稱為註解區塊(comment block)。以下為範例:

```js
/*
我是一個多行註解區塊
這是第二行註解
這是第三行註解
*/
```

### 英文解說

Comment(康門)是很常見的網路用語，它除了用在程式開撰寫裡當"註解"外，也有"評論"、"留言"的意思。部落格通常下面有個留言的功能，它的英文就是這個單字。

slash(死內需)有砍，鞭打的意思，大概是右撇子英文，因為右手拿斧頭砍下去畫出來的動作線就像這樣(/)。

反斜線(\\)的英文是backslash(背可死內需)

## 符號

我們在程式碼中會用到很多符號，除了你剛上面已經看到的幾個，還有常見的幾個:

- 等號(=)
- 括號(())
- 中括號([])
- 大括號({})
- 逗號(,)
- (.)
- (!)
- (?)
- (")
- (')

## 資料類型

### 數字

### 字串

### 布林

### NULL

### undefined

## 加減乘除
