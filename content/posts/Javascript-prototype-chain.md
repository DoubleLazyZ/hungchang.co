+++
author = "wyatt"
title = "Javascript-原型鏈"
date = "2022-05-31"
description = "Javascript-原型鏈(Prototype chain)"
tags = [
    "Javascript",
]
categories = [
    "Javascript",
]
series = ["JavaScript"]
+++
# Javascript-原型鏈
Javascript中除了純值之外，剩下的類型其實都是物件，每個物件除了頂層的物件中都有原型，在ECMAScript標準裡面是寫成someObj.\[[Prototype]]。

在瀏覽器中我們常會用.__proto__取得當前物件的原型物件，要注意的是.__proto__是瀏覽器實做訪問原型的方便寫法，正式的讀寫應該使用 **Object.getPrototypeOf(someObj)** 以及 **Object.setPrototypeOf()** 這兩個訪問器。但是為了理解，這裡講都會使用.__proto__來做筆記。

### 訪問原型鏈
當我們對於一個物件查找屬性找不到時，JS會往原型物件找。
```javascript
var person = {
  name: "ZZ",
  age: 24
}

person.__proto__.height = 1.65
console.log(person.height) // 1.65
```
原本應該沒有height屬性，但在person物件的原型物件上加上屬性後便會從person往上找person.__proto__找有沒有height屬性，將結果輸出。

然而


###### 參考連結
[MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)


