+++
author = "wyatt"
title = "array"
date = "2022-05-24"
description = "Golang的array是一個固定長度的資料且並非指標的值，在Go中array只能裝同樣類型的資料。"
tags = [
    "Go",
]
categories = [
    "Go",
]
series = ["Go"]
+++
> 全文皆來自自學Youtube的教學影片[Learn Go Programming - Golang Tutorial for Beginners](https://youtu.be/YS4e4q9oBaU)，講得相當好，大家可以點進去學習。
# Array
array是一個固定長度的資料且並非指標的值，在Go中array只能裝同樣類型的資料。
1. Array各種聲明方法，要注意只能裝同個類型。
```go
package main

import (
  "fmt"
)

func main() {
  // 三種聲明array的方式
  grades := [3]{12, 55, 77}
  points := [...]{23, 55, 77} // 會自動偵測元素有多少個來決定長度
  var students [3]string
  // 我們可以更換array的值
  students[0] = "Wyatt"
  students[1] = "Wayne"
  students[2] = "Peter"

  fmt.Printf("Grades: %v\n", grades)
  fmt.Printf("Students: %v", students)
  
  // 我們可以用len()來取得
  fmt.Printf("Numbers of Student:", len(students))
}
```
2. 可以創建一個多維的陣列
```go
package main

import (
  "fmt"
)
func main() {
  var matrix [3][3]int = [3][3]{[3]int{1, 0, 0}, [3]int{0, 1, 0}, [3]int{0, 0, 1}}

  // 上面很複雜，可以用下面的方法比較清楚
  var identityMatrix [3][3]int
  identityMatrix[0] = [3]int{1, 0, 0}
  identityMatrix[1] = [3]int{0, 1, 0}
  identityMatrix[2] = [3]int{0, 0, 1}
  fmt.Println(identityMatrix)
  // [[1 0 0] [0 1 0] [0 0 1]]
}
```

3. 以下例子可以證明陣列Array並非指標
```go
a := [...]int{1, 2, 3}
b := a
b[1] = 5
c := &a // 表示我要指向a的記憶體值
fmt.Println(a) [1, 2, 3]
fmt.Println(b) [1, 5, 3]
fmt.Println(c) &[1, 2, 3]
```
由上述例子可以知道b是直接複製a的值到新的記憶體空間，並非複製a指向記憶體的指針。