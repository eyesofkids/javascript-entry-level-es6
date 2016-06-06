# 寫作格式

本書使用了以下的風格作為撰寫風格:

## 專有名詞

對於專有名詞，例如函式庫中的定義、函式、方法、物件等專用名詞，保留原字詞不進行"取代式"的翻譯，而使用括號(())加註其後作為補充翻譯，例如:

```
state(狀態)
props(屬性)
```

## 符號

提供的符號，儘可能使用括號(())加註符號在字句中。例如:

```
單行註解使用雙斜線符號(//)。
```

## 中文字詞與符號

可以使用全形的逗號(，)、分號(、)與句號(。)，但儘量不使用用全形的括號(（）)、框號(「」)與其他符號等。而是使用半形符號。

適當的在中文字詞的前後加上空白字元，作為明顯的區隔出這個字詞。例如:

```
使用 擴充套件->管理 進入這個管理的頁面。
```

## 不使用分號作為每行程式碼的結尾

本書的所有程式碼，每行後面都"不"再使用分號(semicolon)(;)作為程式碼段落之用，程式碼只需要記得有分行就行了。

有許多現代新式的程式語言，也是不需要用分號(;)來作每行程式碼的結尾，例如Swift、Python或Ruby。而實際上，Javascript現在也可以不需要用分號(;)來分行，有許多知名的函式庫例如npm、jQuery都已經採用這個撰寫風格。所以，我們鼓勵所有的Javascript程式設計師，使用這種方式，讓程式碼看起來會更加簡潔。

更多參考資訊:

- [Semicolons in JavaScript are optional](http://mislav.net/2010/05/semicolons/)
- [An Open Letter to JavaScript Leaders Regarding Semicolons](http://blog.izs.me/post/2353458699/an-open-letter-to-javascript-leaders-regarding)

## 讀音(用中文拼音)

為了方便初學者能很容易的唸出英文字詞的讀音，本書使用"以中文讀音拼出英文字詞"的作法。這是參考日本使用五十音來拼英文字詞的作法，方法好不好或對不對見人見智。拼音只針對重要字詞，以前後斜線(/)符號含括中文拼音，例如:

```
const/康死/ 是 constant/康死撐/ 的縮寫，中譯是"常數"的意思。
```

## 英文字詞解說

針對重要的專業字詞，提供英文解說，並加入相關的字詞說明。如果可以的話，提供有趣的範例加深學習印象。

## 風格指引

這是參考目前網路上知名的數個Javascript程式碼撰寫風格指引，提供在每個段落相關的建議。不過有些風格指引是針對ES5的，僅作參考而已，還是會以章節內容為主。目前參考的風格樣式指引有以下幾個:

- [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
- [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwaldron/idiomatic.js/)
- [Google JavaScript Style Guide](https://google.github.io/styleguide/javascriptguide.xml)
- [Code Conventions for the JavaScript Programming Language](http://javascript.crockford.com/code.html)


## 其他參考

- [翻譯指引 Translation Guide](https://github.com/eyesofkids/javascript-style-guide-translate)
