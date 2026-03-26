
Neovim 的設定檔位於 ${XDG_CONFIG_HOME}。可以透過內建命令 `:echo stdpath('config')`，得到其值；預設值為 `~/.config/`

| 檔案                                        | 說明                                         |
| :---------------------------------------- | :----------------------------------------- |
| `${XDG_CONFIG_HOME}/nvim'                 | Neovim 存放設定檔的目錄，等同於 vim 的 .vim             |
| `${XDG_CONFIG_HOME}/nvim/init.vim`        | Neovim 設定檔，等同於 vim 的 .vimrc                |
| `${XDG_STATE_HOME}/nvim/shada/main.shada` | 儲存 Neovim 的 session 資訊，等同於 vim 的 .vimijnfo |

# 客製化

