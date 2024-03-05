# 資料型態 (Data Type)

Rust 的型態系統有以下特點：

- 強型態系統 (strong type system)
    - 在編譯期間，而非執行期間進行更多的型態檢查
- 靜態型態 (statically typed)
    - 變數的型態在編譯間就必須確定
        - 編譯器會依照我們賦予變數的值與使用的方式推導
        - 為了避免誤會，在宣告變數時最好加上資料型態
- 預設變數的值是**不可變** (**immutable**)
    - 當變數賦予值之後，將不能重新賦值

建立資料變數的方式如下。因為 Rust 預設所有變數的值都是不可改變的，需要改變的變數必須加上 mut 關鍵字

``` rust
let [mut] ${NAME}[: ${TYPE}][ = ${VALUE}]
```

Rust 支援許多資料型態，除了數值 (整數與浮點數)、布林 (Boolean)、與字元 (Char) 這些簡易的資料型態，也包含元 (Tuple) 與陣列 (Array) 這種複合型態。這些資料型態的共同之處在於資料長度是固定的，且大小可以輕易的塞進 Stack；即便是元或是陣列，依舊可以將各欄位或是各筆紀錄視為單一資料塞入 Stack。除了這些基本資料型態之外，Rust 也支援客製化的資料型態，像是結構 (struct)、字串 (string)、等長度不等的

## Stack 與 Heap

程式語言將記憶體分為兩個部分，一個是 stack，另一個是 Heap；兩者的差異在於資料的管理方式。Stack 藉由 pipe 管理，資料都是**固定大小**且**後進先出**；而 Heap 則是一個沒有預設管理方式的龐大記憶體區塊，當需要記憶體空間時，將所需記憶體大小給記憶體管理模組，然後記憶體模組會給你一個記憶體位置或是 NULL。一旦您的程式收到記憶體位置，程式就像是跟記憶體模組簽約租借該位置與其後符合需求大小的記憶體；除了租借跟歸還之外，您的程式需要**自行管理**記憶體，沒有預設的管理或是維護方式。

## Reference 與 deference

類似 C/C++ 中的指標，使用 & 建立 reference，使用 * 進行 dereference。

``` rust
fn by_ref( x: &i32 ) -> i32 {
    *x + 1
}

fn main() {
    let i = 10;
    let res1 = by_ref( &i );
    let res2 = by_ref( &41 );
    println!( "{} {}", res1, res2 );
}

output = 11 42
```



Rust 的型態受到 functional languages 如 Ocaml 與 Haskell 的 ADTs (Algebraic Data Types) 啟發，這些反映在 enums、struct、traits、與 error handling types (如 Option 與 Result )。


