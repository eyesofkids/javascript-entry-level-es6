# 物件

## 物件(Objects)

- 物件是可變的(mutable)鍵集合(keyed collection)
- 陣列、函式、正規表述式都是物件
- 物件是屬性(properties)的容器(container)，屬性包含了一個名稱與數值，屬性的名稱可以是任何字串，也可以是空白字串；數值則可以是任何數值，除了`undefined`之外。
- 使用了`prototype`可連結特性讓物件繼承其他物件


## 物件字符(Object Literals)

使用花括號(curly braces)`{}`包含的空白區或name/value對。合法變數名稱不需要用括號(quote)，例如"first-name"就需要括號，而first_name就不需要。（註：合法變數名稱中也不能使用保留字） 

```javascript

var empty_object = {};

var stooge = {
         "first-name": "Jerome",
         "last-name": "Howard"
};

```

屬性值可以包含任何敘述，或是其他物件的語法

```javascript

var flight = {
         airline: "Oceanic",
         number: 815,
         departure: {
             IATA: "SYD",
             time: "2004-09-22 14:55",
             city: "Sydney"
         }, 
         arrival: {
             IATA: "LAX",
             time: "2004-09-23 10:42",
             city: "Los Angeles"
         }
};
```
