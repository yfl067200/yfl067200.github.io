如同 vim 一般，Neovim 也提供選項調整 Editor。各 options 能接受的值包含

- boolean - `True` 或是 `False`
- number - 數字
- string - 字串，需用 `""` 包夾

# 命令

| 命令 | 說明 
|:-----|:-----
| `se[t][!]` | 列出與預設值不同的 options <br> 如果加上 "!"，每行只列出一個 option |
| `se[t][!] all` | 列出所有的 options <br> 如果加上 "!"，每行只列出一個 option |
| `se[t] {option}` | 顯示 option 目前的設定值 |
