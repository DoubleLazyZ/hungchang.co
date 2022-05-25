+++
author = "wyatt"
title = "struct"
date = "2022-05-23"
description = "Golang的struct學習記錄。"
tags = [
    "Go",
]
categories = [
    "Go",
]
series = ["Go"]
+++
# struct
## struct類型
struct就很像是類的概念
```go
package main

import "fmt"

func main() {
	type game struct {
		name string
		stars int
		category []string
	}

	fallguys := game{name: "fallguys"}
	// 第一種賦值方法
	fallguys = game{
		name: "糖豆人",
		stars: 5.0,
		category: []string{"funny"},
	}
	// 第二種賦值方法
	fallguys = game{"糖豆人", 5.0, []string{"funny"}}
	fmt.Println(fallguys) //{糖豆人 5 [funny]}
	fmt.Println(fallguys.stars) // 5
	fmt.Println(fallguys.category) // [funny]
	fallguys.stars ++ 
	fmt.Println(fallguys.stars) // 6
	fallguys.category = append(fallguys.category, "children") // [funny children]
	fmt.Println(fallguys.category)

	type platform struct {
		name string
		game game
	}

	steam := platform{
		name: "steam",
		game: fallguys,
	}

	fmt.Println(steam) // {steam {糖豆人 6 [funny children]}}
	fmt.Println(steam.game.stars) // 6
	steam.game.stars = 4
	fmt.Println(steam.game.stars) // 4 
	fmt.Println(fallguys.stars) // 6 不會改變，因為steam.game.stars這裡給的是copy的值

	type addressSteam struct {
		name string
		game *game // 這裡是一個傳入一個參考的值，等等創建struct就會直接傳入參考struct的位置，而非在複製一分資料。
	}

	addressed := addressSteam{"Addressed Steam", &fallguys} // 這裡給的就是指向fallguys的位置
	fmt.Println(addressed)
	fmt.Println(*addressed.game)
	addressed.game.stars = 4
	fmt.Println(addressed.game.stars) //4 這裡的game指向 fallguys
	fmt.Println(fallguys.stars) // 4

	LOL := new(game)
	fmt.Println(LOL) // &{ 0 []}
	fmt.Println(*LOL) // { 0 []}
	(*LOL).name = "LOL" // 實際上看起來是這樣，但是golang幫忙解決了
	LOL.category = []string{"dota"}
	LOL.stars= 4

	intel := cpu{name: "intel", speed: 100}
	intel.getPrice()
	intel.getSpeed()
}

type cpu struct {
	name string
	speed float64
}

func (cpu) getPrice() {
	fmt.Println("get total price")
}

// 像是往cpu這個類別加了一個方法

func (c cpu) getSpeed() {
	fmt.Println(c.speed)
}

// 第一個參數是receiver的概念，在Go中稱之為method
```