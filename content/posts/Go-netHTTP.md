+++
author = "wyatt"
title = "Go-net/HTTP創建伺服器"
date = "2022-05-29"
description = "Go-net/HTTP創建伺服器"
tags = [
    "Go",
]
categories = [
    "Go",
]
series = ["Go"]
+++
# 創建一個Https伺服器
首先我們要先產生一個憑證
```go
openssl req -newkey rsa:2048 -nodes -keyout server.key -x509 -days 365 -out server.crt
```
接著打上以下的程式碼

```go
// https.go
package main

import (
	"log"
	"net/http"
)

func main() {
	srv := &http.Server{Addr: ":9001", Handler: http.HandlerFunc(handle)}
	log.Printf("Serving on http://0.0.0.0:9001")
	log.Fatal(srv.ListenAndServeTLS("server.crt", "server.key"))
}

func handle(w http.ResponseWriter, r *http.Request) {
	log.Printf("Got connection: %s", r.Proto)
	w.Write([]byte("Hello, this is a HTTP 2 message"));
}
```
在瀏覽器打上https://0.0.0.0:9001便可以看到"Hello, this is a HTTP 2 message"