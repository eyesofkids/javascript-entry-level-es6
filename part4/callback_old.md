# callback 回調函式

## 名詞解釋

不論這個專有名詞用中文的"回呼"、"回叫"、"回調"，都是聽起來很怪異的講法，callback在英文是"call back"兩個單字的合體，你應該有聽過"call me back"的英文，大概是有客戶留電話給你，當你在忙碌時，請你等會有空時再"回電"給它，這裡的說法是指在電信公司裡的callback名詞的涵意。而用在軟體工程上，callback它的使用情境其實也是有類似的地方，[維基上的說明](http://en.wikipedia.org/wiki/Callback_(computer_programming))我就不說了，其實我也有看沒有懂。雖然在許多程式語言中都可以實現callback，但這裡專注的是在Javascript上的"異步(asynchronous) callback"機制。其他關於"同步callback機制"參考維基百科。

## 情境

專業一些來說，callback是什麼：

> callback是有個函式被當作另個函式的傳入參數，然後在某個事件觸發時才會呼叫。

白話一點的舉個例子：

> 早上出門上班上課時忘了帶手機，然後你就打市內電話回家給在家的媽媽，和她說如果有人打你的手機電話，幫忙說一聲然後留下姓名和電話，等你回來再打給他/她。

你媽總不可能接到你的要求後，就坐你的手機旁整天等電話響起，大概就是帶在身上，繼續上街買菜、洗衣服、看電視，真的有人打來就和他說一聲而已。

寫個簡單的程式說明一下：

```javascript
function callinFromSomeone(name, phoneNumber, doSomething){
    alert('電話響了!!! 顯示姓名是 ' + name + ' 與電話號碼是 '+phoneNumber);

    doSomething(name, phoneNumber);
}

function leaveMessage(name, phoneNumber){
    alert('和對方說一聲');
    console.log('姓名: '+ name+'\n 電話號碼: '+phoneNumber);
}

callinFromSomeone('施旖婕','0912345678', leaveMessage);
```

你可能會覺得上面這個程式碼，和下面不是一樣：

```javascript
function callinFromSomeone(name, phoneNumber){
    alert('電話響了!!! 顯示姓名是 ' + name + ' 與電話號碼是 '+phoneNumber);

    doSomething(name, phoneNumber);
}

function doSomething(name, phoneNumber){
    alert('和對方說一聲');
    console.log('姓名: '+ name+'\n 電話號碼: '+phoneNumber);
}
```

事實上媽媽可以自已定義自己的callback是怎麼樣的，這也是你經常會看到Javascript保留了callback彈性的地方：

```javascript
callinFromSomeone('未顯示來電號碼','0944444444', function(){
    alert('詐騙電話，罵它兩句!!!');
});

callinFromSomeone('施旖婕','0912345678', function(){
    alert('偷問一下她是誰...最近常聽說');
    leaveMessage('施旖婕', '0912345678');
});
```

callback也可以用`arguments`陣列獲得傳入的其它參數值。

```javascript
callinFromSomeone('施旖婕','0912345678', function(){
    alert('偷問一下她是誰...最近常聽說');
    console.log('姓名: '+ arguments[0]+'\n 電話號碼: '+arguments[1]);
});
```

從上面的例子可以看得出來，我們所設計出來的函式，的確是由某個"事件"觸發後(有人打電話進來)，才會最後呼叫這個所謂的callback函式。這稱為"事件驅動(Event-driven)"。異步callback可以讓程式執行"不會阻塞(non-blocking)"，這是Javascript在網頁上或是伺服器中的一大特點(賣點)。

http://www.2ality.com/2012/06/continuation-passing-style.html
