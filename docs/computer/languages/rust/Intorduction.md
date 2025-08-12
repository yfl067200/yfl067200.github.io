
# 安裝

Rust 語言自帶一個安裝工具，名為 `rustup`；該安裝工具可由 [Rust 官網](https://www.rust-lang.org/tools/install)下載。Linux 與 Mac 平台可以透過命令 `curl --proto '=https' --tlsv1.3 https://sh.rustup.rs -sSf | sh` 安裝，或是使用套件管理工具安裝。

安裝之後，可以透過執行命令 `rustup --version` 檢視是否安裝成功；另一個方法是，查看系統變數 `PATH` 是否包含 `rustup` 所在的目錄位置。

## C Linker

Rust 編譯器編譯好的 Object Code 需要 Linker 將其結合，所以你的編譯環境還需要一個 C Linker 才能建立 Rust 編譯環境。

> Note
> 
> 在 Linux 環境，可以透過套件管理工具安裝 GCC 或是 Clang
> 在 MAC 環境，可以透過指令 `xcode-select --install` 安裝 C Compiler
> 在 Windows 環境，需要安裝 Visual Studio，並安裝以下套件：
>    “Desktop Development with C++”
>    The Windows 10 or 11 SDK

## rustup

`rustup` 這個工具並非只用來安裝 `rust` 編譯器，還能透過以下命令進行升級與移除的動作：

``` bash
rustup update
rustup self uninstall
```

經由 `rustup`，將會在系統上安裝以下工具

| 工具 | 說明 |
|:-----|:-----|
| `cargo` | 這是`rust` 語言的包裝工具 <br> 功能包山包海，包含下載編譯所需的 library，打包程式上傳到 cargo.io 等 | 
| `clippy` | `rust` 語言的 lint 工具，用來檢查原始碼的錯誤，並提供建議 |
| `rust-docs` | `rust` 語言的文件 |
| `rust-std` | `rust` 語言的 standard library |
| `rustc` | `rust` 編譯器 <br> 基本上都會透過 `cargo` 間接呼叫 `rustc` |
| `rustfmt` | 美化 `rust` 原始碼的工具 <br> 可以透過 `cargo rustfmt` 進行原始碼的美化 |
## 建立新專案

建立專案的流程大約是：

1. 建立專案目錄
2. 編寫原始碼
3. 進行編譯與除錯
4. Release

`rust` 建議透過 `cargo` 來管理專案，這邊就先說明如何透過 `cargo` 建立專案

### 透過 cargo

使用命令 `cargo new ${PROJECT}`，會在目前路徑下建立 ${PROJECT} 這個目錄，並在目錄下建立一個 `Cargo.toml` 與 `src/main.rc` 檔案，甚至於還有 git 相關的資料。其中 `Cargo.toml` 為專案的 metadata，而 `src/main.rc` 為一個空白的原始碼檔案

> [IMPORT]

> Cargo.toml 檔案包含了兩個部分：
> - package
>      該專案的資料，包含名稱、與版本等
> - dependencies
>      編譯該專案所需要的額外 library
>      在 rust 環境中，這些 library 被稱為 crates (箱子)

可以透過以下命令，進行編譯等動作

| 命令 | 說明 |
|:-----|:-----|
| `cargo build` | 編譯原始碼，可以加上參數 `--release` 建立正式的執行檔 |
| `cargo check` | 檢查原始碼，並提供修改原始碼的建議 |
| `cargo run` | 執行編譯好的執行檔 |
