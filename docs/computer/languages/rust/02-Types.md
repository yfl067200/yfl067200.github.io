# 型態 (Type)

Rust 的型態系統有以下特點：

- 強型態系統 (strong type system)
    - 在編譯期間，而非執行期間進行更多的型態檢查
- 靜態型態 (statically typed)
    - 變數的型態在編譯間就必須確定
        - 編譯器會依照我們賦予變數的值與使用的方式推導
		- 為了避免誤會，在宣告變數時最好加上資料型態

    - 當變數賦予值之後，將不能重新賦值
	- 預設變數的值是不可變 (**immutable**)



Rust 的型態受到 functional languages 如 Ocaml 與 Haskell 的 ADTs (Algebraic Data Types) 啟發，這些反映在 enums、struct、traits、與 error handling types (如 Option 與 Result )。


## 泛型 (Generics)

Rust 支援泛型程式設計。




