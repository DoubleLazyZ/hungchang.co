+++
author = "wyatt"
title = "I/O (2022/06/27 - 2022/07/01)"
date = "2022-06-27"
description = "I/O (2022/06/27 - 2022/07/01)"
tags = [
  "TypeScript",
  "React",
  "Data structure",
  "MongoDB"
]
categories = [
    "Daily",
]
series = ["Daily"]
+++
# I/O (2022/06/27 - 2022/07/01)
每天記錄每天學到什麼，主要是因為這樣子才能及時的輸出，發現要寫長文對自己來說太困難了。而且覺得效率也是一個問題，想要先累積出一些作品，實作比較重要，這樣發發每天的心得比較不會給自己有壓力。
## 2022/06/27
### Typescript
#### enum
enum 列舉，可以把變數範圍在某些範圍下進行存取，
```typescript=
enum directions {EAST, WEST, SOUTH, NORTH};
console.log(directions.EAST);

enum permission {ADMIN = 2, READ_ONLY, WRITE_ONLY}
console.log(permission.ADMIN); // 2
console.log(permission.READ_ONLY); // 3 會自動遞增加1
export {}
```
編譯出來的js程式碼如下
```javascript=
var directions;
(function (directions) {
    directions[directions["EAST"] = 0] = "EAST";
    directions[directions["WEST"] = 1] = "WEST";
    directions[directions["SOUTH"] = 2] = "SOUTH";
    directions[directions["NORTH"] = 3] = "NORTH";
})(directions || (directions = {}));
;
```
這裡有所謂的反射性，directions["EAST"] = 0，這行賦值回傳的也會是0，相當於directions[0] = "EAST"
#### Tuple
這個字念tu(ㄩ)ple的音，之前一直唸成a發音。tuple就像陣列，只是與陣列不同的是，他已經規定好長度以及每個索引值裡面的類別了。
```typescript=
let array:(number|string)[] // 這是陣列
let array: Array<string | number> // 這跟上面一樣是陣列
let role:[number, string]; // 這是元組
role = [0, "admin"];
```
我們可能定義了第一個當成某個角色的id，並且指定第二個索引值是角色名稱的字串。我們不能夠隨意的換成不符合的類型。
```typescript=
let role:[number, string];
role = [0, true]; // 不行
```
但要注意的事情是當tuple使用push時，他會檢測不出來不符合型別註記的情況出現。
```typescript=
let role:[number, string];
role = [0, true]; // 不行
role.push("a") // 不會報錯要小心
```
> [TypeScript 資料型別 - 元組(Tuple) & 列舉(Enum)-Kira](https://ithelp.ithome.com.tw/articles/10221546)
這篇文章講得很清楚。
### React
今天本來想跳戰線上課程的練習專案的，但是體力有點耗盡了，因此我只先創建了一個新的專案。明天再來繼續挑戰。
```bash=
npx create-react-app practice-project
```
## 2022/06/29
今天上班處理了關於時間的問題，date-fns不管怎麼樣使用format，時間都會相差8小時，後來未來即時解決問題我決定暫時先自行處理，使用Date物件拿到年、月、日、時、分、秒，再加上zero pad換成想要的格式。

## 2022/07/02
由於每次接到API資料的時候總覺得，自己重新產生想要的資料總是會重複做很多次一樣的步驟才達成目標，讓我有了想要學習資料結構的心，因此我開始學習了資料結構。

### Data structure
#### Linked List(鏈結串列)
一般來說，產生陣列必須要先指定陣列長度。
```java=
String[] myArr = new String[10];
```
記憶體會找一個地方，留10個連續的記憶體空間使用。

而Linked List在記憶體上不一定會是連續的位置。
Linked List只包含```head```和```length```兩個property。

* Linked List Advantage
* 可以不斷加入element，無限制次數。反之，array則會要調整長度
* 加入element的速度很快。
在js中如果指定一個空陣列，一開始給的長度會是1，接著根據加入array的元素才會不斷增加記憶體位置來儲存，從1開始2,4,8,16...，需要不斷的去更改記憶體分配。
Linked List要更改，例如刪除，只需要將指向下一個head的pointer跳過刪除的元素到下一個就完成了。而Array在刪除某個元素之後必須將之後的元素全部的都往前移。
##### javascript寫出Linked List
```javascript=
// 每一個Linked List裡的元素,會有元素的value，以及指向下一個元素。
class Node {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}

// 整個LinkedList只有兩個property，一個是head，一個是長度，一開始沒有長度，也沒有head
class LinkedList {
  constructor() {
    this.head = null;
    this.length = 0;
  }
    
 // 新增元素進入Linked List的最後面
  push(value) {
    let newNode = new Node(value);
    // Linked List起頭要是沒有東西的話則為鏈結串列的第一個元素
    if(this.head === null) {
      this.head = newNode;
    } else {
      // 現在的Node會等於第一個head
      let currentNode = this.head;
      //往下一個找 只要有指向另外一個元素
      while(currentNode.next !== null) {
        currentNode = currentNode.next;
      }
      // 找到最後一個只後currentNode.next === null給新的值
      currentNode.next = newNode;
    }
    this.length++;
  }

  printAll() {
    if(this.length === 0) {
      console.log('Nothing in this LinkedList!');
      return;
    }
    let currentNode = this.head;
    while(currentNode !== null) {
      console.log(currentNode.value);
      currentNode = currentNode.next;
    }
  }
}

let myLinkedList = new LinkedList();
myLinkedList.push("Wyatt");
myLinkedList.push("Wayne");
myLinkedList.push("Kuang");
myLinkedList.printAll();

console.log(myLinkedList);
```

### MongoDB
NoSQL的特性
* No data schema(資料表及欄位的設計)
* Fewer Data Realation

#### MongoDB的使用
```javascript=
npm install mongodb
```
#### 連接MongoDb
```javascript=
// database.js
const mongodb = require("mongodb");
const MongoClient = mongodb.MongoClient;

let _db; // 底線代表只想在這個文件內部讀取到

const mongoConnect = () => {
  MongoClient.connect(
    "mongodb+srv://<使用者名稱>:<使用者密碼>@cluster0.ykhk6.mongodb.net/shop?retryWrites=true&w=majority"
  )
    .then((client) => {
      _db = client.db(); // 可以跟database互動
    })
    .catch((err) => {
      throw err;
    })
}

const getDb = () => {
    if(_db) {
        return _db;
    }
    throw "No database Found!"
}

exports.mongoConnect = mongoConnect;
exports.getDb = getDb;
```
#### db的各種操作
1. 創建collections，新增一比資料。
```javascript=
const mongodb = require("mongodb");
const getDb = require("../util/database").getDb;
// 會自動新增products collection，新增一筆資料user資料
const db = getDb();
db.collection('products')
.insertOne({
  username: "wyatt",
  email: "test@gmail.com",
  id: new mongodb.ObjectId(id) // 主要是使用mongodb產生的id物件
})
```
2. 查找一筆資料
拿課程中部份程式做紀錄，使用find({key: 條件})，物件裡面第一個是找到的key，第二個是要寫符合的條件。

$in是特別用法會將所有我們提供javascript陣列的字串，查詢是否有符合的條件，並且使用toArray()，拿到所有的資料並轉換成陣列可以使用。

這裡要特別注意的是我們拿到的new ObjectId雖然是一個物件，雖然看上去一樣，但在使用===類型檢測上會不一樣，所以記得要使用.toString()
```javascript=
getCart() {
    const db = getDb();
    const productIds = this.cart.items.map((i) => {
      return i.productId;
    });
    return db
      .collection("products")
      .find({ _id: { $in: productIds } }) // 主要是這裡
      .toArray()
      .then((products) => {
        return products.map((p) => {
          return {
            ...p,
            quantity: this.cart.items.find((i) => {
              return i.productId.toString() === p._id.toString();
            }).quantity,
          };
        });
      });
  }
```
3. 更新一比資料
更新資料也很簡單，db.collection("users").updateOne({},{})，
第一個參數填入要改的資料，第二個使用$set可以指定要改的欄位的value。
```javascript=
const updatedCart = { items: updatedCartItems };
const db = getDb();
return db
  .collection("users")
  .updateOne(
    { _id: new ObjectId(this._id) },
    { $set: { cart: updatedCart } }
  );
```
4. 刪除一筆資料
可以使用update把資料清空。