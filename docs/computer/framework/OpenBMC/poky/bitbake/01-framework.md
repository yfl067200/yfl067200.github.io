# 檔案架構

```
├── bin								<--	bitbake 執行檔
├── contrib
│   ├── hashserv
│   ├── prserv
│   └── vim
├── doc
│   ├── _templates
│   ├── bitbake-user-manual
│   ├── sphinx-static
│   └── template
└── lib
    ├── __pycache__
    ├── bb							<--	bitbake 主要程式
	├── bblayers
    ├── bs4
    ├── hashserv
    ├── layerindexlib
    ├── ply
    ├── progressbar
    ├── prserv
    ├── simplediff
    └── toaster
```

執行 bitbake obmc-phosphpr-image 命令，會呼叫 bb/main.py 中的 bitbake_main 物件進行 OpenBMC 的編譯動作。

## bitbake_main 物件

bitbake_main 物件原始檔位於 bb/main.py 中，建立該物件需要兩個參數 - configParams 與 configuration；i

其中 configParams 是 BitbakeConfigParams 物件，繼承自 cookerdata.ConfigParameters (位於 bb/cookerdata.py)；

而 configuration 是


### create_bitbake_parser()

本函式會建立一個 argparse.ArgumentParser() 的 instance，並用他來處理呼叫 bitbake 傳入的參數。當 parser 建立之後，bitbake 會在該 parser 中建立以下 6 個參數 groups 儲存 bitbake 命令用到的相關參數

| Group | argument | 特殊動作 | 說明 |
|:------|:---------|:---------|:-----|
| General | `targets` | nargs="\*", metavar="recipename/target" | bitbake 要執行的目標 (target) recipe 檔案 (.bb) |
| | `--environment` <br> `-e` | action="store_true" | 顯示 global 與各 recipes 使用到的變數 |
| | `--graphviz` <br> `-g` | action="store_true", dest="dot_graph" | 建立並儲存執行目標 recipe 中的相依樹狀圖 |
| | `--help` <br> `-h` | action="help" | 顯示 help 訊息並結束 |
| | `--show-versions` <br> `-s` | action="store_true", dest="show_environment" | 顯示目前各 recipes 的版本 |
| | `--ui` <br> `-u` | default=os.environ.get( "BITBAKE_UI", "knotty" ) |
| | `--version` | action="store_true" | 顯示 bitbake 的版本並結束 |
| Task | `--force` <br> `-f` | action="store_true" | 強制執行某個 target |
| | `--cmd` <br> `-c` | | 強制執行某個 target 的某一個工作 (task) <p> 譬如 compile |
| | `--clear-stamp` <br> `-C` | dest="invalidate_stamp" |
| | `--runall` | action="append", default=[] | 將目標 (target) 包含的所有子目標都執行某項工作 (task) 與相依於該工作的工作 |
| | `--runonly` | action="append" | 將目標 (target) 包含的所有子目標都執行某項工作 (task) |
| | `--no-setscene` | action="store_true", dest="nosetscene" | 將略過所有 setscene 工作。sstate 資訊將被無視，完全重新編譯目標 |
| | `--setscene-only` | action="store_true", dest="setsceneonly" | 略過 setscene 工作，但是會從 sstate 中取得個目標的狀態 |
| | `--skip-setscene` | action="store_true", dest="skipsetscene" | 只跑 setscene 工作建立 sstate 資訊 |
| Exec | `--buildfile` <br> `-b` | | 編譯某個目標，無視於相依性 |
| | `--continue` <br> `-k` | action="store_true", dest="halt" | 即使遇到錯誤，也盡可能跑完可以執行的工作 |
| | `--dry-run` <br> `-n` | action="store_true" | 沒有執行真正的工作，但是試著依序執行 |
| | `--dump-signatures` <br> `-S` | action="store_true", default=[], metavar="SIGNATURE_HANDLER" |  |
| | `--parse-only` <br> `-p` | action="store_true" | 進行 parse BB 檔案的工作並結束 |
| | `--profile` <br> `-P` | action="store_true" | 建立相關 profile 與報表 |
| | `--revisions-changed` | action="store_true" | 指定結束的代碼。當確認 upstream 的原始碼改變便結束 |
| Logging | `--debug` <br> `-D` | action="count", default=0 | 依照 D 的數量，指定 debug level 的數字。<p> -DD 的 debug level 為 2，唯有 bb.debug( 1, ... ) 與 bb.debug( 2, ... ) 的訊息會顯示在 stdout |
| | `--log-domains` | action="append", dest="debug_domains", default=[] | 在某些 logging domains 中顯示 debug 訊息 |
| | `--quiet` <br> `-q` | action="count", default=0 | |
| | `--verbose` <br> `-v` | action="store_true" | 將 bb.note( ... ) 訊息顯示在 stdout |
| | `--write-log` <br> `-w` | dest="writeeventlog", default=os.environ.get( "BBEVENTLOG" ) | 將事件紀錄在 bitbake event json 檔案 |
| Server | `--bind <br> -B | default=False | 指定 bitbake xmlrpc server 的 name 與 address |
| | `--idle-timeout <br> -T | type=float, dest="server_timeout", default=os.getenv( "BB_SERVER_TIMEOUT" ) | Idle 多久時間之後，與 bitbake server 中斷 |
| | `--kill-server` <br> `-m` | action="store_true" | 停止任何執行中的 bitbake server |
| | `--ovserve-only` | action="store_true" | 連上 bitbake server，僅進行觀測 |
| | `--remote-server` | default=os.environ.get( "BBSERVER" ) | 連上某一台 bitbake server |
| | `--server-only` | action="store_true" | 執行 bitbake 時會啟動 server，但是不啟動 UI |
| | `--status-only` | action="store_true" | 查看遠端 bitbake server 的狀態 |
| | `--token` | dest="xmlrpctoken", default=os.environ.get( "BBTOKEN" ) | 指定連結遠端 bitbake server 使用的 Token |
| Config | `--ignore--deps` <br> `-I` | action="append", dest="extra_assume_provided", default=[] | 假設相依性並不存在，或都提供 |
| | `--postread` <br> `-R` | action="append", dest="postfile", default=[] | 在解析完 bitbake.conf 之後，額外解析指定的檔案 |
| | `--read` <br> `-r` | action="append", dest="prefile", default=[] | 在解析 bitbake.conf 之前，額外解析指定的檔案 |

