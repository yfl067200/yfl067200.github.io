## 變數 (Variables) 與常數 (Constants)

在形態一節，我們有提到 Rust 預設將變數視為不可變 (immutable)；這種行為好像是將變數當成常數，但是 Rust 對待這兩者之間還是有些許差異。

- 宣告時的差異
    - 變數宣告時，可以不必指定型態或是值
    - 常數宣告時，必須指定可在編譯期計算出的數值 (型態也一併決定)
    - 宣告變數時，可使用關鍵字 **mut** 表示該變數是可變 (mutable)
	    - 即便為可變變數，其資料型態在賦值之後便固定了
	- 宣告常數時，不可搭配關鍵字 **mut**
- 兩者的生命週期不同
    - 變數在其所屬的區塊 (scope) 結束之後，也隨之消失
    - 常數的生命週期是等同於該程式，與宣告時所在區塊無關

以下是宣告變數與常數的語法
```rust
//    variable declaration
let [mut] ${變數}[: ${型態}] [= ${值}]

//    constant declaration
const ${常數}[: ${型態}] = ${值}
```

### 變數遮蔽 (Shadowing)

程式就像積木一樣，是由一塊塊區塊 (scope) 所組成的；而每一個區塊中，有可以包含多個子區塊。在不同的區塊中宣告相同名稱的變數，原本的變數依舊存在，但是在該區塊中就只能存取新宣告的變數；這種新變數遮蔽舊變數的行為，舊被稱為變數遮蔽 (Shadowing)。

變數遮蔽大多發生於子區塊變數遮蔽父區塊的變數；但是因為 Rust 預設將變數視為不可重複賦值的機制，變數遮蔽也發生在同一個區塊中。以下的範例顯示第二個宣告的變數 spaces (integer 型態) 遮蔽同名的變數 (string 型態)；發生遮蔽現象之後，被遮蔽的變數將無法在該區塊中被存取

```rust
//    String type variable named spaces
let spaces = "Ker Ker";

//    Integer type variable named spaces will shodowing string type variable
let spaces = spaces.len();
```


