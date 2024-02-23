# 型態 (Type)

Rust 的型態系統有以下特點：

- 強型態系統 (strong type system)
    - 在編譯期間，而非執行期間進行更多的型態檢查
- 靜態 (static)
    - 當變數賦予值之後，將不能重新賦值
	- 預設變數的值是不可變 (**immutable**)


## 變數 (Variables) 與常數 (Constants)

雖然說 Rust 將變數賦予值之後，預設是不可重新賦值；這種行為好像是將變數當成常數，但是 Rust 對待這兩者之間還是有些許差異。

- 變數在賦值之前，可以不設定型態；常數必須指定型態與值
    - 宣告變數時，可以加上關鍵字 **mut** 表示這個變數可以重複賦值。常數不可搭配 **mut** 關鍵字
- 常數的值必須在編譯時期即可計算出來
- 變數在其所屬的區塊 (scope) 結束之後，也隨之消失；常數的生命週期是等同於該程式，與宣告時所在區塊無關

以下是宣告變數與常數的語法
```rust
//    variable declaration
let [mut] ${變數}[: ${型態}] [= ${值}]

//    constant declaration
const ${常數}: ${型態} = ${值}
```

### 變數遮蔽 (Shadowing)

程式就像積木一樣，是由一塊塊區塊 (scope) 所組成的；而每一個區塊中，有可以包含多個子區塊。在不同的區塊中宣告相同名稱的變數，原本的變數依舊存在，但是在該區塊中就只能存取新宣告的變數；這種新變數遮蔽舊變數的行為，舊被稱為變數遮蔽 (Shadowing)。

變數遮蔽大多發生於子區塊變數遮蔽父區塊的變數；但是因為 Rust 預設將變數視為不可重複賦值的機制，變數遮蔽也發生在同一個區塊中。以下的範例顯示第二個宣告的變數 spaces (integer 型態) 遮蔽同名的變數 (string 型態)

```rust
//    String type variable named spaces
let spaces = "Ker Ker";

//    Integer type variable named spaces will shodowing string type variable
let spaces = spaces.len();
```



Rust 的型態受到 functional languages 如 Ocaml 與 Haskell 的 ADTs (Algebraic Data Types) 啟發，這些反映在 enums、struct、traits、與 error handling types (如 Option 與 Result )。


## 泛型 (Generics)

Rust 支援泛型程式設計。




