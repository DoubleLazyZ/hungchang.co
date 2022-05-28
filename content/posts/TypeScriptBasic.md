+++
author = "wyatt"
title = "TypeScript"
date = "2022-05-28"
description = "原本想用Go學習後端，但是大家建議先學好TS並且加上Node.js練習，等到熟悉了再學習Go，加上工作剛好未來有需求要使用TypeScript，因此我來學習TypeScript了。"
tags = [
    "TypeScript",
]
categories = [
    "TypeScript",
]
series = ["TypeScript"]
+++
# 初學TypeScript
原本想用Go學習後端，但是大家建議先學好TS並且加上Node.js練習，等到熟悉了再學習Go，加上工作剛好

未來有需求要使用TypeScript，因此我來學習TypeScript了。網路上找到覺得最好的教學就是由Maxwell

寫的文章，寫得相當詳細，我還買了他的書來詳讀。本篇文章都是拜讀書籍的內容，強烈建議大家購買。

附上連結[讓 TypeScript 成為你全端開發的 ACE！系列](https://ithelp.ithome.com.tw/users/20120614/ironman/2685)，作者：Maxwell


## TS的原始型別 Primitive Types
js的原始型別有number, string, boolean, undefined, null, void
```js
let num = 123;
let message = 'Hello world';
let truthy = true;
let meansEmpty = undefined; // any
let meansNothing = null; // any
```
這裡直接賦值undefined和null不是好事情因此我們必須型別註記一下。
```js
let meansEmpty: undefined = undefined;
let meansNothing: null = null;
```

## 遲滯性指派Delayed Initialization
當我們一開始宣告一個變數，卻沒有賦予值的時候，TypeScript也會把變數當成any。
```js
let num; // any
console.log(num); // undefined
num = 123;
```
上面的例子並不會報錯，因為num被判別成any的緣故，這顯然很糟糕。因此我們必須一開始型別註記避免發生
沒有預期的錯誤。
```js
let num:number;
console.log(num); // 報錯
```
```js
let num:number | undefined;
console.log(num) // 不會報錯
```

## JSON物件的型別
物件的型別註記長得像下面一樣
```js
let info : {
  name: string;
  age: number;
  interest: string[];
} = {
  name: 'wyatt',
  age: 24,
  interest: ['drawing', 'programming']
}
```
只是上面看起來實在是太複雜了，因此我們把物件結構抽出來。這裡可以使用"型別化名"簡潔程式，型別化名
可以抽象化。
```js
type PersonalInfo = {
  name: string;
  age: number;
  interest: string[];
}

let info: PersonalInfo = {
  name: 'Wyatt',
  age: 24,
  interest: ['programming', 'play game']
}
```


### 物件的完整性
物件不可以隨便新增及刪除，並且更改值必須符合物件結構的型別。
```js
info.email = 'test@gmail.com' // 報錯
delete info.name; // 也會報錯了
info.name = 'Wayne' // 這種複寫因為型別一樣可以
```
全部複寫是可以的，但還是一樣要符合型別
```js
info = {
  name: "Wayne",
  age: info.age,
  interest: info.interest,
}

// ES7(Rest-Spread Operator)寫法
info = {...info, name: 'Wayne'}

info = {...info, name: 123} // 報錯，string不可以賦值number
```

### 唯獨屬性
我們有時候會希望物件不可以被覆寫，可以使用readonly。
```js
type PersonalInfo = {
  name: string,
  readonly age: number;
  interest: string[];
}

let info: PersonalInfo = {
  name: "wyatt",
  age: 24,
  interest: ["programming"]
}

info.age = 25; // 報錯，因為readonly
```
有時候我們是引用別人寫的程式，為了避免型別化名的自由度降低，但避免物件太過自由的方法可以用下面的
方法。
```js
type PersonalInfo = {
  name: string;
  age: number;
  interest: string[];
}

let info: Readonly<PersonalInfo> = {
  name: "wyatt",
  age: 24,
  interest: ["programming"]
}

info.age = 25; // 也會報錯
```

## 函式的型別
大多函數的參數都會被判別成any，因此涵式的輸入都必須要進行註記的動作

1. 宣告變數，將函數當成物件值
```js
let addition1: (a: number, b: number) => number = function(a, b) {
  return a + b;
}
```

2. 宣告變數，將函數當成物件值，在韓式宣告輸入輸出時，一併註記。
```js
let addition2 = function (a: number, b: number): number {
  return a + b;
}
```


3. 函數宣告，輸入、輸出時註記
```js
function addition3(a: number, b: number): number {
  return a + b
}
```

4. 函數物件值屬於表達式，使用斷言註記法
```js
let addition4 = function(a, b) {
  return a + b
} as (a: number, b: number) => number;
```

### 選用參數 - 只能在參數的尾端
```js
// ok
function sumFrom3Nums(input1: number, input2?: number, input3?: number) {
  return input1 + (input2 ? input2 : 0) + (input3 ? input3 : 0);
}

// 不ok
function sumFrom3Nums(input1: number, input2?: number, input3: number) {
  return input1 + (input2 ? input2 : 0) + (input3 ? input3 : 0);
}
```
### 預設參數
使用空值連結操作符(Nullish Coalescing Operator)
```js
function increment(input1: number, input2?:number) {
  const value = input2 ?? 1;
  return input1 + value
}
```
通常我們會寫成下面這樣
```js
function increment(input1: number, input2: number = 1) {
  return input1 + input2
}
```

### 陣列型別
二維陣列
```js
const points = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
]
// 型別判別是int[][]
```
如果想在陣列裡放速不同型態的可以寫成下面的樣子(要注意括號要加)
```js
const arr1: (number | string)[] = [1, 2, 3];
const arr2: number | string[] = [1, 2, 3]; // 這個意思是數字或是字串型別陣列，會報錯
```

## 明文類別
使用let宣告變數並指派原始型別的值，被推論的不是明文型別，而是明文代表的型別。

使用const則選告變數是指派原始型別值， 並且被推論的是明文型別。

以下是純值的型別註記
```js
let jsonObj: { foo: 123, bar: "Hello" } = { foo: 123, bar: "Hello" };
jsonObj.foo = 123;
jsonObj.foo = 456; 不行，會報錯
```
