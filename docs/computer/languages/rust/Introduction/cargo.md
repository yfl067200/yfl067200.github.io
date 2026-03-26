cargo 是 rust 提供的官方工具之一，他主要的工作在於編譯與包裝專案。這邊將說明如何使用 cargo 建立新專案。完整的 `cargo` 說明，請參考附錄 A 的 [cargo](docs/Computer/languages/rust/tools/cargo/readme.md) 章節

> [!Note]
> 一般來說，我們會在 HOME 目錄下建立一個目錄，用來處理專案。我們這邊用 `~/workspaces/rust` 目錄作為所有 rust 專案的主目錄。

# 建立專案

透過以下命令即可在當前目錄下，建立一個名為 {PROJECT} 的目錄；該目錄下包含一個名為 Cargo.toml 的檔案，與一個包含 main.rc 的目錄 (src)。

> $ cargo new {PROJECT}

我們這邊使用 hello_world 作為新專案的名稱
![[Screenshot 2026-01-06 at 5.01.32 PM.png]]


> [!Note]
> Rust 官方建議多個字之間使用底線  (`_`) 進行連接
## Cargo.toml

該檔案可以視為專案用的 Manifest 檔案，裡面包含了至少 package 與 dependencies 兩部分的資訊。完整的資訊，可以參考[官方文件](https://doc.rust-lang.org/cargo/reference/manifest.html)。

## src/main.rs

透過 cargo 建立的專案，程式碼會被置於 `src` 目錄下，而 top level 的目錄下只會存放 cargo.toml 與 README 檔案。以下為 main.rs 的內容

```
fn main() {
    println!("Hello, world!");
}
```

函式 main() 是特殊的函式，同 C 語言，程式的進入點就是 main() 函式。但是 rust 的 main() 函式與 C 的 main() 函式有以下不同：

1. rust 的 main 函式預設沒有返回值。如果需要返回值，該型態為 `Result<(), E>`，其中 E 為可定義的錯誤
2. rust 的 main 函式不需要參數處理使用者輸入的參數，rust 使用 `std::env::args` 函式處理使用者輸入的參數

rust 函式會在[函式章節]()有詳細的說明

# 編譯專案

編譯 rust 原始碼可以透過命令 `rustc 原始碼檔`。但是專案中會存在多個原始碼檔，還需要透過 `linker` 等工具對物件碼進行鏈結等，才能建立執行檔。比較簡單的方式是透過 `cargo build` 或是 `cargo build --release` 命令建立執行檔。

> [!Note]
> 測試檔案透過 `cargo build` 命令建立，執行檔會在 `target/debug` 目錄下；而正式版則是透過 `cargo build --release` 命令建立，執行檔位於 `target/release` 目錄下

## 建立並執行 debug 版本

一旦建立出執行檔，可以直接執行。另一個方式是透過 `cargo run` 命令，建立 debug 版本的執行檔，並執行該檔案。

## 檢查原始碼

`cargo` 提供一個編譯原始碼，但不建立執行檔的選項：`cargo check`。這個命令提供你更快的方式，確認你的程式沒有語法上的錯誤。