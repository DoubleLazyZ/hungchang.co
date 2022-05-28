+++
author = "wyatt"
title = "Node學習-Server、Stream/Buffer"
date = "2022-05-27"
description = "Node學習-Server、Stream/Buffer"
tags = [
    "Node",
]
categories = [
    "Node",
]
series = ["Node.js"]
+++
# 初學Node

## HTTP,HTTPS

超文本傳輸協定，瀏覽器與伺服器交換資料的協定。

- Http: Hyper Text Transfer Protocol
- Https: Hyper Text Transfer Protocol Secure。
 - HTTP + Data Encryption (during Transmission)。

## Node創建一個簡單的伺服器
app.js
```javascript
const http = require('http')

function requestHandler(req, res, next) => {
  console.log(req.url, req.method, req.headers);
  // process.exit() // 這樣寫會結束程式
  next(); // 這裡的next是一個函數，代表讓城市繼續進行下去，前往下一個中間件。
}

const server = http.createServer(requestHandler) // 這裡NodeJS會一直監聽有沒有request
server.listen(3000); // server監聽port3000
```

當我們node.js app.js後，會開始解析程式，編譯之後註冊變數和函數，接著js引擎提供event loop，會持續

等待event listener註冊，直到行程結束(這裡是指app.js程式結束才會結束)。

## 分離路徑
通常網頁會有不同的請求，通通把所有request判斷都寫在app.js會相當複雜不易閱讀，因此我們通常會根據不同功能拆分路徑。

在根目錄下底下創建 routes.js，把我們剛才的requestHandler分離出去。
```javascript
const fs = require("fs"); // node的資料夾裡的file system

const requestHandler = (req, res) => {
  // * sending response
  const url = req.url; // 我們取得了使用者輸入的url
  const method = req.method; // 使用者使用的請求方式(GET? POST?...)
  if (url === "/") {
    res.write("<html>");
    res.write("<head><title>Enter Message</title></head>");
    res.write(
      '<body><form action="/message" method="POST"><input type="text" name="message"><button type="submit">Send</button></form></body>'
    );
    res.write("</html>");
    return res.end(); //結束傳送資料給客戶端，不繼續執行下去，後面不會繼續執行下去，因此我們在這裡return
  }

  if (url === "/message" && method === "POST") {
    const body = [];
    req.on("data", (chunk) => {
      console.log(chunk);
      body.push(chunk);
    });

    return req.on("end", () => {
      const parsedBody = Buffer.concat(body).toString();
      const message = parsedBody.split("=")[1];
      console.log(parsedBody);
      fs.writeFile("message.txt", message, (err) => {
        res.statusCode = 302;
        res.setHeader("Location", "/");
        return res.end();
      });
    });
  }
  res.setHeader("Content-Type", "text/html");
  res.write("<html>");
  res.write("<head><title>My First Page</title></head>");
  res.write("<body><h1>Hello from my Node.js Server!</h1></body>");
  res.write("</html>");
  res.end(); //結束傳送資料給客戶端
};

module.exports = requestHandler

exports.handler = requestHandler;
exports.someText = 'Some hard coded text'
```
上面的程式碼分段解釋
```javascript
const url = req.url; // 我們取得了使用者輸入的url
const method = req.method; // 使用者使用的請求方式(GET? POST?...)
if (url === "/") {
  res.write("<html>");
  res.write("<head><title>Enter Message</title></head>");
  res.write(
    '<body><form action="/message" method="POST"><input type="text" name="message"><button type="submit">Send</button></form></body>'
  );
  res.write("</html>");
  return res.end(); //結束傳送資料給客戶端，不繼續執行下去，後面不會繼續執行下去，因此我們在這裡return
}
```
這裡當我們輸入http://localhost:3000/，會得到上面回傳的表單，裡面可以輸入資料並且POST到路徑/message。
```javascript
if (url === "/message" && method === "POST") {
    const body = [];
    req.on("data", (chunk) => {
      console.log(chunk);
      body.push(chunk);
    });

    return req.on("end", () => {
      const parsedBody = Buffer.concat(body).toString();
      const message = parsedBody.split("=")[1];
      console.log(parsedBody);
      fs.writeFile("message.txt", message, (err) => {
        res.statusCode = 302;
        res.setHeader("Location", "/");
        return res.end();
      });
    });
  }
```
當我們POST到/message後便會符合路徑url === 'message'和 method === 'POST'，

這裡我們寫上req.on('data')代表監聽傳來的資料，這裡有一個要補充的知識就是Buffer，
在接受傳輸的資料時候，伺服器會將一整個資料流(stream)分成好幾塊(chunk)接受，完成後再拼接一起轉換成可讀的資料。
![stream和Buffer](https://upload.wikimedia.org/wikipedia/commons/0/08/CachePrefetching_StreamBuffers.png?20161019235817)
> 這裡這張圖並不是Node.js運行的樣子，只是為了說明資料會像這樣分成好幾塊。

等到所有的資料都接收到，便會呼叫req.on("end",()=> {/**執行這裡的程式**/})

我們便可以調用Buffer.concat()的方法把資料串起來，並且轉換成字串。

最後我們在使用fs.writeFile來寫入檔案，並且給http狀態碼302(轉址)，將頁面轉回原來的表單頁面。

## 結論
我們可以發現因為js引擎是single thread也就是說一次只做一件事，如果今天有一個檔案很大，某個使用者請求了，

那樣程式就會在原地等待讀取檔案結束才繼續進行下去。那樣另外一個使用者也發起同樣的請求，他便會需要等到前面的

使用者請求完成才可以繼續處理該名使用者的請求，這相當不合理。因此我們的function就必須要是非同步的，

當我們有請求的時候，將處理交給額外的worker pool，等到處理完的時候，便通知event loop裡面註冊的listener，

去執行裡面的回調函數來處理接下來的步驟。



