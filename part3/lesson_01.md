# 變數、常數與命名

## 常數與變數(可變與不可變)

程式語言充份反應了人類社會的情況，因為它是人類所設計的，也是設計給人類所使用的。

程式語言中需要以一個標示，說明這個用於存放值(value)的代表名稱，在這個程式執行的生命週期中，它是可變或是不可變的。範例如下:

```js
//這個a是不可變的，指定它的值為10
const a = 10

//這個b是可變的，指定它的值為5
let b = 5
```

你可把常數和變數想像是一個盒子:

- 常數const/康死/: 裝入東西(值)之後就上鎖的盒子，之後不可以更動裡面的值
- 變數let/累/: 暫時存放值的盒子，盒子是打開的，可以更動裡面的值

尤其Javascript程式語言的程式設計師，在之前常會犯有不宣告就直接使用的壞習慣。我建議你最好在宣告常數或變數時，就給定一個值。變數並沒有要求一開始要給定值，只有常數才有這個要求。

因此，通常在程式碼撰寫中，我們使用const的情況會遠比let高。除非你很確定這個變數等會會需要被改變，或是它是一個在類別定義裡的變數，不然就用const就對了。另外還有一個常見的風格指引建議是"一行宣告一個變數或常數"，範例如下:

```js
//不好的宣告方式
const items = getItems(),
    goSportsTeam = true,
    dragonball = 'z';

//好的宣告方式
const items = getItems()
const goSportsTeam = true
const dragonball = 'z'
```

這大概是想要偷懶少打一些const，不過明顯這樣作是個壞習慣(雖然Javascript允許你可以這樣作)。你可能會問這樣作有什麼差異，好的宣告方式除了清楚明白外，最大的優點是它在程式除錯時，可以很清楚的理解是哪一行作什麼事。

> 常數指的"不能再次指定數值"(not re-assigned)，而不是"無法改變其中數值"(immutable)，是有差異的概念，物件類型的常數內容屬性是可以改變的，在這個情況會是一個常數指標。Javascript中並沒有immutable的內建特性。例如下面的程式碼是可以使用的，來自[Constant Reference, Not Value](https://strongloop.com/strongblog/es6-variable-declarations/):

```
const names = []
names.push( "Jordan" )
console.log( names )
```

> ES5以前都只會用"var"(肉<台>)作為標記，它是variable(買<台>里哦波)的縮寫。"let"與"const"是ES6的新加入特性，你應該開始使用它們，本書不使用var來宣告變數。不要混用兩種用法。關於這兩種宣告方式的差異請參考: [這裡的問答與範例](http://stackoverflow.com/questions/762011/let-keyword-vs-var-keyword)與[這篇文章的說明](https://strongloop.com/strongblog/es6-variable-declarations/)

### 英文解說

const/康死/ 是 constant/康死撐/ 的縮寫，中譯是"常數"的意思。

let/累/，中譯是"讓..."，
例如"let's go"大概是"大家一起走吧"或"please let me go"(讓我走吧...其實是男女朋友說"分手"的意思) 

### 風格指引

- (crockford)(airbnb 13.2)建議一行一個變數(或常數)宣告與註解，然後最好是按英文字母排列。
- (crockford)在函式中的最前面的敘述宣告變數。(註：不要在程式碼中間的敘述來宣告)
- (airbnb 13.3)把let宣告的放在一起，const宣告的放在一起。
- (google)總是使用var來宣告。(註：用於些處是說明，變數記得一定要宣告)
- (google)使用NAMES_LIKE_THIS這樣的命名方式，來命名常數。

## 命名 Naming

我們需要先為這些數值的代表名稱命名，稱為常數名稱(constant name)或變數名稱(variable name)。以下說明的命名規則，不只這些，還包含了函式的名稱，以及類別的名稱的命名。

### 命名規則

變數或常數名稱基本上要遵守以下的規則:

- 開頭字元需要是ASCII字元(英文大小寫字元)，或是下底線(_)。注意，數字不可用於開頭字元。
- 接下來的字元可以使用英文字元、數字或下底線(_)
- 名稱不可使用Javascript語言保留字詞

> 注意: 以下底線(_)為開頭的命名通常是有特別的用法涵意，它是用在類別中的私有變數、常數或函式(方法)。

### 好的變數(函式、類別)命名

#### 變數、函式、類別命名

變數與函式，都用小駝峰式(camelCase)的命名。類別用巴斯卡(PascalCase)或大駝峰式命名法(CamelCase)命名:

```js
let numberOfStudents
const numberOfLegs
function setBackgroundColor() 
class Student{}
```

#### 常數命名

在許多風格樣式指引中，會建議使用全大寫英文命名，字詞間使用下底線(_)連接:

```
const NAMES_LIKE_THIS='Hello'
```

不過，因為ES6中加入了const用於指示為常數後，其實這個規則並沒有那麼需要，程式檢查工具或執行時都會用常數的方式來檢查。所以你可以用一般對變數的命名規則就可以了:

```
const helloString='Hello'
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

#### 具時間意義或指示的字詞

- will 通常指即將發生但未發生
- did 已發生
- should 應該發生

### 風格指引

- (airbnb 22.1) 避免使用單個英文字元的命名
- (airbnb 22.4) 避免在命名的前後使用下底線(_)，例如`__firstName__`、`_firstName`、`firstName_`都是很糟的命名
 
## 註解 Comment

註解Comment(康門)的意思是要讓未來的自己或其他人很快的理解，這段程式碼是在作什麼用的。所以在重要的程式碼的地方(通常在上面)，加上註解是好的習慣。註解也常常用在暫時性的把某段程式碼先擋住，讓它不執行。好的註解習慣的養成建議:

- 不管你的英文有多破，用英文來進行註解是比較好的
- 註解上面需要加一行空白行
- 用`// FIXME:`與`// TODO:`來指示問題位置與未來要作的功能點
- 用註解來說明函式的參數(params)與回傳值(return)

> 提示: 英文很破為何還要逼你寫英文註解？是為了把順便英文學好的關係嗎？正確的解答是為了"加快與專注在程式碼的撰寫"。因為打中文註解還需要切換到中文輸入法，但是寫程式碼又要切回英文輸入法。專心寫程式就寫程式碼不是很好嗎？這只是一個建議的養成習慣而已。

單行註解用的是雙斜線(//)(double slash/打波 死內需/)符號。以下為範例:

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

#### 風格指引

- (airbnb 17.2)註解上面需要加一行空白行
- (airbnb 17.4)用`// FIXME:`來指示目前問題所在點
- (airbnb 17.4)用`// TODO:`來指示之後需要作的項目所在點

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
- (_)underscore

## 資料類型

### 數字

### 字串

### 布林

### NULL

### undefined

## 加減乘除
