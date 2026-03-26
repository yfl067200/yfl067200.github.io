
# 安裝 Language Server 實作

在 Microsoft 的 Github 上有 Language Server Protocol (LSP) 相關的 Repository：[Language Server Protocol](https://microsoft.github.io/language-server-protocol/)。除了 Protocol 相關的資訊之外，也提供了各種語言相關的實作；我們這邊以 C++ 其中一個實作 (clangd) 為例，可以透過 apt 等套件管理工具進行安裝

安裝好之後，需要透過 `which clangd` 命令找到該實作執行程式所在的位置。
# 設定 neovim

neovim 與 LSP 進行通訊的流程可以分成兩個階段：

- 啟動 LSP daemon
    - 建立 LSP Client 與 LSP daemon 進行初始化
- 透過 LSP Client


## 建立 LSP Client

neovim 透過 Lua 語言的 `vim.lsp.start()` 函式建立 LSP Client。當 LSP Client 建立的過程中，會檢查 LSP Daemon 是否啟動；若 LSP Daemon 未啟動，則會啟動相關 LSP Daemon。

而這個自定義的 `vim.lsp.start()`的內容如下
``` Lua

```


# Reference

[Neovim LSP](https://neovim.io/doc/user/lsp.html)