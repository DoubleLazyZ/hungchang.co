+++
author = "wyatt"
title = "I/O (2022/07/04 - 2022/07/10)"
date = "2022-07-10"
description = "I/O (2022/07/04 - 2022/07/10)"
tags = [
  "JavaScript",
  "React",
  "Node.js",
]
categories = [
    "Daily",
]
series = ["Daily"]
+++
# I/O (2022/07/04 - 2022/07/10)
上禮拜還是沒有每天都寫紀錄，感覺進度差了一些，要持續的output很重要。

## 2022/07/04
每天都在處理圖片沒寫到程式感到疲憊。今天有遇到一個問題，很大很大的圖片在openlayers上顯示不出來。明明在測試的時候都可以，上了正式機卻不行，後來發現是正式機檢查的時候我用edge，然後我測了一下發現只有firefox可以，chrome也不行，看到討論串有說是瀏覽器限制，大概5000*5000以上就不行了，突然覺得firefox很猛，不過這麽大的圖片loading確實很重，希望廠商不要再覺得那麼大的圖片可以放上去了，下載再去下載原圖。
> https://github.com/openlayers/openlayers/issues/10252

### Javascript
今天心血來潮，看回了之前只看了一小段的Javascript奇怪的部分，發現真的很有趣幫忙很多，以前學都還不太清楚，過一陣子後回來看都知道他想表達什麼，很精彩。

今天簡單講解一下學到的內容
#### Syntax Parser、Execution contexts、lexical environments。
- Syntax Parser(語法解析器)
    - 編譯器或直譯器解析人寫出來的程式有沒有符合規範，才能轉換成機器懂的語言。
- Execution contexts(執行環境|執行脈絡|執行上下文)
    - 一個包裹(包住程式執行的空間)，幫助管理正在執行的程式。
    - Excution contexts除了自己寫的code和正在執行的code，還包括了很多其他的東西幫助程式執行。
- Lexical environments(詞彙環境)
    - 片段程式被寫在哪裡？以及周圍寫了哪些程式，因為編譯器會根據你寫的位置來判斷怎麼轉換成機器讀的語言。

#### Object
是由一個key/value所組成，在每一個執行上下文中，key是唯一的，不會重複的。

#### Excution context包含什麼？
* Creation Phase
執行上下文的創建階段會包含以下幾個部分。
1. global object -> 將會變成全域變數中的其中一個屬性，在執行全域函數或讀取全域變數時才會參照。
2. this 
3. outer environment -> 如果是function execution context會有外部的execution context。
4. your code -> 自己寫的程式。
* Excution Phase
#### Hoist
Javascript會有提升的概念，變數或是函數的提升。
提升並非是把程式移到最前面執行，而是Javascript的Syntax parser讀過程式後交給編譯器處理，會將我們的變數通通存進記憶體當中，並且賦予undefined的值，因此我們發現當我們試圖讀取寫在後面的全域函式或是變數的時候不會報錯。

```javascript=
console.log(myVariable); // undfined
Hello(); //Hello!

var myVariable = 'a';
function Hello() {
    console.log('Hello!');
}
```
#### undefined
undefined在JavaScript中是一個primitive(純值)，所有變數在hoist之後，我們沒有賦予值，Javascript引擎會給我們的值賦予undefined，這是一個"值"
當我們直接讀取一個沒有被創建在記憶的值時會顯示not defined，那跟undefined完全不同。

#### Single Thread & Synchronous
- Single Thread 
> one command at a time.

一次執行一個指令。但以瀏覽器來說並不是，這裡指的是javascript引擎本身。

- Synchronous
>one at time。依序一行一行讀下來。

程式是同步的，意味著每一步驟都必須完成才會繼續進行下去，當讀取到某個函數時，必須等到函式自己的execution context的code執行完，才會繼續往下讀取。

#### Function Invocation And The execution stack
- Invocation
> running a function. In JS,by using parenthesis()
#### Execution stack
全域會有一個execution context，每個函數也都會有自己的excution context，當遇到函數的時候，會形成一個excution context，依照先進後出的規則(這個資料型別我們稱作stack)，意味著要逐行讀，發現函式就放到堆中執行，如果該函式沒有另外函式待自己的程式跑完，將此函式丟出堆中，並且繼續執行呼叫該函式的函式。
```javascript=
function a() {
    b();
    console.log('after b')
}
function b() {
    console.log('b excution code run end.')
}

a();
console.log('c')

// 最後輸出是
// b excution code run end.
// after b
// 'c'
```
#### Function, Context, and Variable Environments
- Variable Environments
 > Where the variable live.And how they related to each other in memory

除了全域有global的scoped之外，剩下有scoped的就是function，相當於excution context，變數的使用只能在自己所屬的excution context。除了全域變數大家都能讀取使用。
#### scoped chain(範圍鏈)
當我們去執行一個函數或者是去讀取一個變數的時候，如果在自己的execution context裡面找不到的話，就會像自己的outer environment去尋找。
而參照的外部環境會是依據我們上述先前提到Lexical environments(詞彙環境)來不斷往外查找，對於javascript來說，詞彙環境也是很重要的，例如我們來看以下的例子。
#### Asynchronous非同步回呼
#### Types And Javascript
#### 範圍、ES6和let
#### Dynamic Typing
#### 函式就是個物件
* JS是First-Class Function
    * JS中函數在可以被視為其他的變數的時候就是一級函式。
    * 因為函式可以指派給其他變數。
    * 函式可以當作參數來傳遞。
    * 函式也可以被回傳。
* 函數都是物件
* 函數包含了name(選擇),code(invokable)。
* expression(表達式)和stament(敘述式)最大的差異是表達式會回傳值，而敘述式不會。

### HTML
```htmlembedded=
<!-- html5開頭 -->
<!DOCTYPE html>
<html>
    <head>
        <title>這是標題</title>
    </head>
    <body>
        <h1>
            標題
        </h1>
        <p>
            段落段落
        </p>
    </body>
</html>
```
```htmlembedded=
<!DOCTYPE html>
<html lang="en">
<head>
<!-- IE不寫會亂碼 -->
  <meta charset="UTF-8"> 
<!--   IE瀏覽器更新到最新 -->
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  
</body>
</html>
```

### React
#### React.Fragment
```jsx=
<React.Fragment>
</React.Fragment> 
```
```jsx=
<>
</>
```
#### Portal
渲染html在正確的位置，讓html sementic。
```htmlembedded=
<div id="backdrop-root"></div>
<div id="modal-root"></div>
<div id="root"></div>
```
```javascript=
const BackDrop = (props) => {
    return <div className={classes.backdrop} onClick={props.onConfirm}/>
}

const ModelOverlay = (props) => {
    return ...
}
const Modal = (props) => {
  return (
    <>
      {ReactDom.createPortal(<BackDrop onClick={props.onConfirm}/>, document.getElementById('backdrop-root'))}
    </>
  )Ï
}
```

#### useReducer
const [state, dispatchFn]


### Node.js

#### Cookie
瀏覽器用來存資料的地方(可以儲存較小的資料)，後端可以在身份驗證通過時，傳送cookie。

下禮拜再補上這禮拜沒寫完的部分。