+++
author = "wyatt"
title = "Node模組-path"
date = "2022-05-25"
description = "Node讀取資料夾及文件的路徑經常使用的。"
tags = [
    "Node",
]
categories = [
    "Node",
]
series = ["Node.js"]
+++
## Node模組-path
Node讀取資料夾及文件的路徑經常使用的。

## resolve使用: path.resolve

```javascript
const path = require('path');

const basePath = '\\User\\Wyatt';
const filename = 'test.txt';

// const filePath = basePath + '/' + filename; //使用拼接會有路徑問題

/* 註釋
  每個系統使用的路徑分隔符號不一樣
  Linux / Mac os /
  Window \, \\, /

  IEEE提出可疑值操作系統接口(Portable Operating System Interface，縮寫POSIX)
  Linux和Mac OS都實現了POSIX接口
  Windows電腦的話是部分實現POSIX接口
*/

const filePath = path.resolve(basePath, filename) // 這裡會幫我們處理basePath裡面路徑的分割符號
console.log(filePath) // D:\User\Wyatt\test.txt
```

## 獲得路徑的資訊: path.xxx

```javascript
const path = require('path');

// 1.獲取路徑訊息
const filepath = '/User/Wyatt/test.txt'

console.log(path.dirname(filepath)); //  /User/Wyatt
console.log(path.basename(filepath)); //  test.txt
console.log(path.extname(filepath)); //   .txt

```

## 路徑的拼接: join和resolve的不同

```javascript
// 2. join拼接
const basepath = '../User/Wyatt';
const filename = 'test.txt';
const filepath = path.join(basepath, filename);
console.log(filepath) // ..\User\Wyatt\test.txt


// 3. resolve拼接
// resolve會判斷拼接的路徑中，是否有以/或./或../開頭的路徑
const filepath2 = path.resolve(basepath, filename);
console.log(filepath2) //  D:\Node\05_常見的模組\User\Wyatt\test.txt
```