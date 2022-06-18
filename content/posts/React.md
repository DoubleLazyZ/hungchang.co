+++
author = "wyatt"
title = "React初體驗"
date = "2022-05-31"
description = "React初體驗"
tags = [
    "React",
]
categories = [
    "React",
]
series = ["React"]
+++
# React

### 下載
1. 下載node
2. 下載react
```bash=
npx create-react-app my-app
cd my-app
npm start
```

### 閱讀官方docs後的心得

#### 以資料結構想畫面，盡量讓資料結構跟畫面能夠結合在一起。
#### 操控資料要使用useState
```javascript=
import { useState } from 'react';
const [inputText, setInputText] = useState("init word")
```
useState會回傳一個陣列，陣列的第一個是變數，第二個則是操縱資料變化的函數，其命名方法會是一個名詞，另一個是 “set名詞” 。一開始useState裡要傳入初始化的數值當作參數。
> inputText 值只會被初始化一次。

#### React要做到雙向綁定(資料綁畫面，畫面改動寫回資料)需要手動調用useState的第二個函數。
操作資料一定要使用setInputText，不可以直接賦值變數。
#### 如果資料是由父傳子，想要改變狀態可以將setInputText()當成props傳下去。
並且傳下去的名稱會使用 on + 名詞 + 操作動作，如同onInputTextChange


### 細部探討 - Component
每一個物件都是一個function，並且回傳jsx。使用export可以導出元件，在其他文件導入來複用。

一個文件export default 只能有一個，但可以有多個export。導入方式根據是不是default有所不同。
```javascript=
// a.js
export default function Profile(){...}
export function Other1() {...}
export function Other2() {...}
// b.js
import Profile from './a.js'
import { Other1, Other2 } from './a.js'
```
> 要注意react的function名稱一定是要大寫開頭命名才會有作用。主要是因為要區分是html的tag還是是react的component.

### 細部探討 - export 和 import
在react的專案中，src/App.js是app的根。除非是用Next.js，它的每個頁面的跟才會不一樣。

在import的時候可以不加副檔名.js，這是因為es module的特性。

### 細部探討 - JSX
在撰寫JSX的時候有些地方與html不一樣。下面條列規則
1. 必須只有一個根包住整個結構，可以用<div></div>，也可以使用<></>
```jsx=
<div>
  <h1>JSX</h1>
  <img>
</div>
```
```jsx=
<>
  <h1>JSX</h1>
  <img>
</>
```
這個<></>特別的tag叫做React fragment。不能夠有兩個以上的root是因為jsx最終都會被轉成js物件，一個function裡不能同時兩個物件在沒有展開的情況下。


