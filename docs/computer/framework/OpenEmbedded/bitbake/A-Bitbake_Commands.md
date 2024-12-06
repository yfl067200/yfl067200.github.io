
# 命令參數

Bitbake 內建許多變數，可以劃分為 6 大類。這些變數將影響 bitbake 命令的動作，而使用者可以透過命令列參數對這些變數進行調整。

> [!NOTE]
> 相關的程式碼在 poky/bitbake/lib/bb/main.py 中的 create_bitbake_parser() 中進行處理
> 相關的程式碼透過 [python argparse 模組](https://docs.python.org/3/library/argparse.html)處理。

若是參數的 action 並未指定變數名稱 (dest參數)，變數名稱則為最長的參數名稱移除 "--"。用 General Group 中的 -e 與 -s 為例，因為 -e 有指定變數名稱 "show_environment"，故變數名稱為 "show_environment"；而 -s 並未指定變數名稱，預設將使用 "show-versions" (最長的參數名稱)。

## General Group

| 命令列參數                           | action                                           | 說明                                      |
| :------------------------------ | :----------------------------------------------- | :-------------------------------------- |
|                                 | nargs="\*", metavar="recipename/target"          | bitbake 要執行的目標 (target) recipe 檔案 (.bb) |
| `-e`, <br>`--environment`       | action="store_true", dest="show_environment"     | 顯示 global 與各 recipes 使用到的環境變數           |
| `-g`,<br>`--graphviz` <br>      | action="store_true", dest="dot_graph"            | 建立並儲存執行目標 recipe 中的相依樹狀圖                |
| `-h`,<br>`--help` <br>          | action="help"                                    | 顯示 help 訊息並結束                           |
| `-s`,<br>`--show-versions` <br> | action="store_true"                              | 顯示目前各 recipes 的版本                       |
| `-u`,<br>`--ui` <br>            | default=os.environ.get( "BITBAKE_UI", "knotty" ) |                                         |
| `--version`                     | action="store_true"                              | 顯示 bitbake 的版本並結束                       |

## Task Group

| 命令列參數                         | action                                   | 說明                                                                                 |
| :---------------------------- | :--------------------------------------- | :--------------------------------------------------------------------------------- |
| `-f`,<br>`--force` <br>       | action="store_true"                      | 強制執行某一目標或是特定工作，無視 stamp 資料                                                         |
| `-c`,<br>`--cmd` <br>         |                                          | 強制執行某一個工作 (task)，譬如 compile <br>會參考 stamp 資料                                       |
| `-C`,<br>`--clear-stamp` <br> | dest="invalidate_stamp"                  | 無視於某工作的 stamp 檔案，執行某目標中對該工作的預設動作                                                   |
| `--runall`                    | action="append", default=[]              | 將目標 (target) 與依賴該目標的其他目標，進行特定的工作 <br> 本動作完成後，會將被影響到的目標視為新的目標，重新進行相同的動作，直到沒有新的目標為止  |
| `--runonly`                   | action="append"                          | 將目標 (target) 與相依於其的其他目標都執行某項工作 (task) <br> 與 `--runall` 的不同之處在於，`runonly` 不會進行新的一輪 |
| `--no-setscene`               | action="store_true", dest="nosetscene"   | 將略過所有 setscene 工作 <br> sstate 資訊將被無視，完全重新編譯目標                                      |
| `--skip-setscene`             | action="store_true", dest="skipsetscene" | 將略過所有 setscene 工作 <br> sstate 資訊會被最為參考                                             |
| `--setscene-only`             | action="store_true", dest="setsceneonly" | 只進行 setscene 工作                                                                    |

## Exec Group 

| 命令列參數                             | action                                                       | 說明                             |
| :-------------------------------- | :----------------------------------------------------------- | :----------------------------- |
| `-b`, <br>`--buildfile` <br>      |                                                              | 編譯某個目標，無視於相依性                  |
| `-k`,<br>`--continue` <br><br>    | action="store_false", dest="halt"                            | 即使遇到錯誤，也盡可能跑完可以執行的工作           |
| `-n`,<br>`--dry-run` <br>         | action="store_true"                                          | 依序執行，但是沒有真正的執行工作               |
| `-S`,<br>`--dump-signatures` <br> | action="store_true", default=[], metavar="SIGNATURE_HANDLER" |                                |
| `-p`,<br>`--parse-only` <br>      | action="store_true"                                          | 進行 parse BB 檔案的工作並結束           |
| `-P`,<br>`--profile`              | action="store_true"                                          | 建立相關 profile 與報表               |
| `--revisions-changed`             | action="store_true"                                          | 指定結束的代碼。當確認 upstream 的原始碼改變便結束 |

## Logging Group

| 命令列參數                     | action                                                       | 說明                                                                                                                                                                              |
| :------------------------ | :----------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `-D`,<br>`--debug`        | action="count", default=0                                    | 依照 D 的數量，指定 debug level 的數字。<br> -DD 的 debug level 為 2，唯有 bb.debug( 1, ... ) 與 bb.debug( 2, ... ) 的訊息會顯示在 stdout <br> 如果 os.environ 中有 BBDEBUG 變數，將取 BBDEBUG 與 `--debug` 之間的最大值 |
| `-l`,<br>`--log-domains`  | action="append", dest="debug_domains", default=[]            | 在某些 logging domains 中顯示 debug 訊息                                                                                                                                                |
| `-q`<br>`--quiet` <br>    | action="count", default=0                                    | 減少在 terminal 出現的除錯訊息數量 <br> 與 `--verbose` 參數不可同時出現                                                                                                                              |
| `-v`,<br>`--verbose` <br> | action="store_true"                                          | 將 bb.note( ... ) 訊息顯示在 stdout <br> 與 `--quite` 參數不可同時出現                                                                                                                         |
| `-w`,<br>`--write-log`    | dest="writeeventlog", default=os.environ.get( "BBEVENTLOG" ) | 將事件紀錄在 bitbake event json 檔案 <p> 預設名稱為 bitbake_eventlog_${TIME}.json                                                                                                            |

## Server Group

| 命令列參數                         | action                                                                      | 說明                                                                |
| :---------------------------- | :-------------------------------------------------------------------------- | :---------------------------------------------------------------- |
| `-B`, <br>`--bind <br>        | default=False                                                               | 指定 bitbake xmlrpc server 的 address 與 port，兩者間用 ":" 分隔             |
| `-T`,<br>`--idle-timeout <br> | type=float, dest="server_timeout", default=os.getenv( "BB_SERVER_TIMEOUT" ) | Idle 多久時間之後，與 bitbake server 中斷                                   |
| `-m`,<br>`--kill-server` <br> | action="store_true"                                                         | 停止任何執行中的 bitbake server                                           |
| `--ovserve-only`              | action="store_true"                                                         | 連上 bitbake server，僅進行觀測                                           |
| `--remote-server`             | default=os.environ.get( "BBSERVER" )                                        | 連上某一台 bitbake server <br> 與參數 `--server-only` 不可同時存在              |
| `--server-only`               | action="store_true"                                                         | 執行 bitbake 時會啟動 server，但是不啟動 UI <br> 與參數 `--remote-server` 不可同時存在 |
| `--status-only`               | action="store_true"                                                         | 查看遠端 bitbake server 的狀態                                           |
| `--token`                     | dest="xmlrpctoken", default=os.environ.get( "BBTOKEN" )                     | 指定連結遠端 bitbake server 使用的 Token                                   |

## Config Group

| 命令列參數                          | action                                                    | 說明                                                                                      |
| :----------------------------- | :-------------------------------------------------------- | :-------------------------------------------------------------------------------------- |
| `-I`,<br>`--ignore--deps` <br> | action="append", dest="extra_assume_provided", default=[] | 假設相依性並不存在，或都提供                                                                          |
| `-R`,<br>`--postread` <br>     | action="append", dest="postfile", default=[]              | 在解析完 bitbake.conf 之後，額外解析指定的檔案 <br> 如果 os.environ 中有 BBPOSTCONF 資訊，將會自動加入 `postfile` 變數 |
| `-r`,<br>`--read` <br>         | action="append", dest="prefile", default=[]               | 在解析 bitbake.conf 之前，額外解析指定的檔案 <br> 如果 os.environ 中有 BBPPRECONF 資訊，將會自動加入 `prefile` 變數   |




