安裝 rust 的方式有很多，我們這邊介紹官方推薦的工具：`rustup`。
# rustup

透過 `rustup` 安裝，只會安裝在該使用者的 `~/.cargo/` 路徑下，非全系統安裝。 進入該路徑之後，會發現所有的工具都指向 `rustup`。各工具的說明，請參考
![[Screenshot 2026-01-06 at 5.02.39 PM.png]]
## 安裝

### Linux 與 MAC 環境

在 Linux 與 MAC 環境下，執行下列指令將下載並安裝 rust 相關工具與程序庫

> curl --proto '=https' --tlsv1.3 https://sh.rustup.rs -sSf | sh

依照輸出的結果，你需要重新登入 Linux 環境讓 PATH 變數包含 rust 工具與程序庫所在。

> [!Note]
> 除了 rust 工具之外，系統還需要 `linker` 將編譯後的物件檔連結在一起，以建立執行檔。如果編譯之後出現 linker errors，請安裝 `linker` 工具 (通常包含在 C 編譯工具)
> 
> 在 MAC 環境，可以執行下面命令安裝 `linker`
> xcode-select --install

### Windows 環境

在 Windows 環境安裝會比較麻煩。你需要先到[官網](https://www.rust-lang.org/tools/install)下載安裝工具，再進行安裝。安裝過程中會要求你安裝編譯工具與 SDK 等，請參考[微軟相關文件](https://learn.microsoft.com/en-us/windows/dev-environment/rust/setup)。

### 安裝檢查

安裝完之後，應該可以使用命令 `which rustup` 或是 `which rustc` 找到相關的工具，不然也可以執行命令 `rustc --version` 得到版本資訊

## 其他功能

rustup 除了用來安裝之外，還提供以下功能
### 文件

透過 rustup 安裝會在本地端安裝 rust 相關文件。可以執行命令 `rustup doc` 開啟瀏覽器進行閱讀

### 更新與移除

安裝之外，還可以進行 rust 的更新與移除

```
# 更新
$ rustup update

# 移除 rustup 與所有安裝的 rust 工具
$ rustup self uninstall
```
