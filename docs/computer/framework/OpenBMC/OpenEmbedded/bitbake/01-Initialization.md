# 簡介

編譯 openbmc 專案的流程可以分為兩大階段：

- 第一階段在載入 yocto 專案的環境變數

    - 在 openbmc 目錄下，執行 `. ./setup` 將列出所有可以編譯的 ${PROJECT} 列表

    - 在 openbmc 目錄下，執行 `. ./setup ${PROJECT} ${DESTINATION}` 命令

        - 執行完命令之後，會在 ${DESTINATION} 路徑下建立 ${PROJECT} 所需要的編譯設定檔；主要存在於 ${DESTINATION}/local 目錄下

        - 在該 session 下載入 yocto 專案的變數

            - 新的 session (新的 ssh 連線，或是重新登入帳號等)，都需要重新執行一次命令，載入 yocto 專案的變數

- 第二階段為使用 bitbake 工具編譯專案

    - 使用 bitbake 工具需要有 yocto 相關的環境設定

    - 可於 ${DESTINATION}/local 目錄下的設定調整專案

    - 透過命令 `bitbake obmc-phosphor-image`，編譯 openbmc 專案

調整 openbmc 專案的流程就放在[]() 描述，這邊就說明 bitbake 如何讀取參數與編譯專案。


## yocto 專案變數

在 openbmc 下的 setup 命令是一個 bash script。

### 列印出 openbmc 所有的專案

當使用者沒有加上 ${PROJECT} 參數，setup 就會去 openbmc 目錄下所有的 **meta-\*/conf/machine/** 與 **meta-\*/meta-\*/conf/machine/** 路徑下的 conf 檔案。略過對應路徑下，沒有 **bblayers.conf.sample** 檔案的路徑，剩下的就是符合 openbmc 標準的專案。

## Bitbake

工具 bitbake 位於 poky/bitbake/bin 目錄下，本身是一個 python 程式。執行 bitbake 命令之後，會藉由 poky/bitbake/lib/bb/main.py 中的 bitbake_main 物件進行 OpenBMC 的編譯動作。

```
├── bin                                <--    bitbake 執行檔
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
    ├── bb                            <--    bitbake 主要程式所在位置
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


## bitbake_main 類別

bitbake_main 類別定義位於 bb/main.py 檔案中，且 bitbake_main 物件繼承自兩個類別；第一個是 configParams 類別，第二個是 configuration 類別。在 butbake_main 物件中，可以透過 configParams 與 configuration 參數存取父類別

> def bitbake_main(configParams, configuration):

比對呼叫 bitbake 建立 openbmc project 的程式碼

> bitbake_main( BitBakeConfigParameters( sys.argv ), cookerdata.CookerConfiguration() )

我們可以知道，configParams 類別是 BitBakeConfigParameters 類別，而 configuration 類別是 cookerdata.CookerConfiguration 類別。

### BitBakeConfigParameters 類別

- 定義於 bb/main.py 檔案

> class BitBakeConfigParameters(cookerdata.ConfigParameters):

- 該類別繼承自 cookerdata.ConfigParameters 類別

    - cookerdata.ConfigParameters 類別定義在 bb/cookerdata.py 中

        > class ConfigParameters(object):

        - 將會呼叫 parseCommandLine() 函式解析命令列參數

        - 命令列參數將會變成物件的屬型 (attributes)

- 這個 BitbakeConfigParameters 物件內建一個 parser 物件

    - parser 物件繼承自 python argparse，用來解析命令列的參數。

        - 即 cookerdata.ConfigParameters 類別中呼叫 parseCommandLine() 函式解析命令列參數的物件

        - 命令列參數 (sys.argv) 解析後，將轉成 options 以及 options.targets

    - 該 parser 物件會建立六個[參數群]()，列出所有 bitbake 命令可以接受的參數


### cookerdata.CookerConfiguration 類別

- 定義於 bb/cookerdata.py 檔案

> class CookerConfiguration(object):

- 該物件用來儲存 bitbake 預設的編譯選項

## setup_bitbake

解析完 bitbake 命令列參數之後，將會呼叫 set_bitbake( configParams ) 函式。

- 加入 log handler

- 載入 ui_module 模組

> ui_module = import_extension_module(bb.ui, configParams.ui, 'main')

    - 預設是從 poky/bitbake/lib/bb/ui 路徑中，載入 knotty.py (configParams.ui 預設值)

        - 從模組中取得 "featureSet" 這個 attribute 的資料。knotty 的預設值是

        > featureSet = [bb.cooker.CookerFeatures.SEND_SANITYEVENTS, bb.cooker.CookerFeatures.BASEDATASTORE_TRACKING]

- 建立 bitbake server

    - 呼叫 lockBitbake()

	    - 定義在 poky/bitbake/lib/bb/main.py 檔案

		- 透過呼叫 bb.cookerdata.findTopdir()，取得存放 **bblayers.conf** 檔案的路徑

		    - 定義在 poky/bitbake/lib/bb/cookerdata.py 檔案

			- 呼叫 bb.data.init() 建立 dictionary 型態變數 d

			- 在呼叫 bb.cookerdata.findConfigFile()，取得存放 **bblayers.conf** 的路定

    - 建立 BitBakeServer 

	    - 定義在 poky/bitbake/lib/bb/server/process.py 檔案

		- 在初始化時，建立 deamon 執行 BitbakeServer._startServer()

		    > bb.daemonize.createDaemon(self._startServer, logfile)

			- 執行 poky/bitbake/bin/bitbake-server 

		- 


``` python3
topdir, lock, lockfile = lockBitbake()
sockname = topdir + "/bitbake.sock"

if lock:
    bb.event.ui_queue = []
    server = bb.server.process.BitBakeServer(lock, sockname, featureset, configParams.server_timeout, configParams.xmlrpcinterface, configParams.profile)
```


## 執行 ui_module.main( server_connection.connect, server_connect.events, configParams )

預設的 ui_module 是 knotty，定義於 poky/bitbake/lib/bb/ui/knotty.py 檔案。

> def main(server, eventHandler, params, tf = TerminalFilter):

- 建立 logconfig (JSON)

- 建立 BBUIHelper 物件

    - 定義於 poky/bitbake/lib/bb/ui/uihelper.py 檔案

- 建立 TerminalFilter 物件

    > termfilter = tf(main, helper, console_handlers, params.options.quiet)

    - 定義於 poky/bitbake/lib/bb/ui/knotty.py 檔案

    - 進入迴圈

        - 呼叫 termfilter.keepAlive( )


# 參考

## Bitbake 命令列參數

在 [BitBakeConfigParameters 物件]() 一節中有提到，解析命令列參數之後會建立 6 個參數群。各參數中包含多個參數，bitbake 物件可以透過 configParams.XXX 存取參數群中的參數 XXX。

以下為各參數群的說明：

### General Group

| 參數名稱 | 命令列參數 | action | 說明 |
|:---------|:-----------|:-------|:-----|
| `targets` | | nargs="\*", metavar="recipename/target" | bitbake 要執行的目標 (target) recipe 檔案 (.bb) |
| `show_environment` | `--environment` <br> `-e` | action="store_true", dest="show_environment" | 顯示 global 與各 recipes 使用到的環境變數 |
| `dot_graph` | `--graphviz` <br> `-g` | action="store_true", dest="dot_graph" | 建立並儲存執行目標 recipe 中的相依樹狀圖 |
| | `--help` <br> `-h` | action="help" | 顯示 help 訊息並結束 |
| | `--show-versions` <br> `-s` | action="store_true" | 顯示目前各 recipes 的版本 |
| | `--ui` <br> `-u` | default=os.environ.get( "BITBAKE_UI", "knotty" ) | |
| | `--version` | action="store_true" | 顯示 bitbake 的版本並結束 |

## Task Group

| 參數名稱 | 命令列參數 | action | 說明 |
|:---------|:-----------|:-------|:-----|
| | `--force` <br> '-f` | action="store_true" | 強制執行某一目標或是特定工作，無視 stamp 資料 |
| | `--cmd` <br> `-c` | | 強制執行某一個工作 (task)，譬如 compile <br> 會參考 stamp 資料 |
| `invalidate_stemp` | `--clear-stamp` <br> `-C` | dest="invalidate_stamp" | 無視於某工作的 stamp 檔案，執行某目標中對該工作的預設動作 |
| | `--runall` | action="append", default=[] | 將目標 (target) 與依賴該目標的其他目標，進行特定的工作 <br> 本動作完成後，會將被影響到的目標視為新的目標，重新進行相同的動作，直到沒有新的目標為止 |
| | `--runonly` | action="append" | 將目標 (target) 與相依於其的其他目標都執行某項工作 (task) <br> 與 `--runall` 的不同之處在於，`runonly` 不會進行新的一輪 |
| `nosetscene` | `--no-setscene` | action="store_true", dest="nosetscene" | 將略過所有 setscene 工作 <br> sstate 資訊將被無視，完全重新編譯目標 |
| `skipsetscene` | `--skip-setscene` | action="store_true", dest="skipsetscene" | 將略過所有 setscene 工作 <br> sstate 資訊會被最為參考 |
| `setscene` | `--setscene-only` | action="store_true", dest="setsceneonly" | 只進行 setscene 工作 |

## Exec Group 

| 參數名稱 | 命令列參數 | action | 說明 |
|:---------|:-----------|:-------|:-----|
| | `--buildfile` <br> `-b` | | 編譯某個目標，無視於相依性 |
| `halt` | `--continue` <br> `-k` | action="store_false", dest="halt" | 即使遇到錯誤，也盡可能跑完可以執行的工作 |
| | `--dry-run` <br> `-n` | action="store_true" | 依序執行，但是沒有真正的執行工作 |
| `SIGNATURE_HANDLER` | `--dump-signatures` <br> `-S` | action="store_true", default=[], metavar="SIGNATURE_HANDLER" |  |
| | `--parse-only` <br> `-p` | action="store_true" | 進行 parse BB 檔案的工作並結束 |
| | `--profile` <br> `-P` | action="store_true" | 建立相關 profile 與報表 |
| | `--revisions-changed` | action="store_true" | 指定結束的代碼。當確認 upstream 的原始碼改變便結束 |

## Logging Group

| 參數名稱 | 命令列參數 | action | 說明 |
|:---------|:-----------|:-------|:-----|
| | `--debug` <br> `-D` | action="count", default=0 | 依照 D 的數量，指定 debug level 的數字。<br> -DD 的 debug level 為 2，唯有 bb.debug( 1, ... ) 與 bb.debug( 2, ... ) 的訊息會顯示在 stdout <br> 如果 os.environ 中有 BBDEBUG 變數，將取 BBDEBUG 與 `--debug` 之間的最大值 |
| `debug_dommains` | `--log-domains` <br> `-l` | action="append", dest="debug_domains", default=[] | 在某些 logging domains 中顯示 debug 訊息 |
| | `--quiet` <br> `-q` | action="count", default=0 | 減少在 terminal 出現的除錯訊息數量 <br> 與 `--verbose` 參數不可同時出現 |
| | `--verbose` <br> `-v` | action="store_true" | 將 bb.note( ... ) 訊息顯示在 stdout <br> 與 `--quite` 參數不可同時出現 |
| `writeeventlog` | `--write-log` <br> `-w` | dest="writeeventlog", default=os.environ.get( "BBEVENTLOG" ) | 將事件紀錄在 bitbake event json 檔案 <p> 預設名稱為 bitbake_eventlog_${TIME}.json |

## Server Group

| 參數名稱 | 命令列參數 | action | 說明 |
|:---------|:-----------|:-------|:-----|
| | `--bind <br> -B | default=False | 指定 bitbake xmlrpc server 的 address 與 port，兩者間用 ":" 分隔 |
| `server_timeout` | `--idle-timeout <br> -T | type=float, dest="server_timeout", default=os.getenv( "BB_SERVER_TIMEOUT" ) | Idle 多久時間之後，與 bitbake server 中斷 |
| | `--kill-server` <br> `-m` | action="store_true" | 停止任何執行中的 bitbake server |
| | `--ovserve-only` | action="store_true" | 連上 bitbake server，僅進行觀測 |
| | `--remote-server` | default=os.environ.get( "BBSERVER" ) | 連上某一台 bitbake server <br> 與參數 `--server-only` 不可同時存在 |
| | `--server-only` | action="store_true" | 執行 bitbake 時會啟動 server，但是不啟動 UI <br> 與參數 `--remote-server` 不可同時存在 |
| | `--status-only` | action="store_true" | 查看遠端 bitbake server 的狀態 |
| `xmlkrpctoken` | `--token` | dest="xmlrpctoken", default=os.environ.get( "BBTOKEN" ) | 指定連結遠端 bitbake server 使用的 Token |

## Config Group

| 參數名稱 | 命令列參數 | action | 說明 |
|:---------|:-----------|:-------|:-----|
| `extra_assume_provided` | `--ignore--deps` <br> `-I` | action="append", dest="extra_assume_provided", default=[] | 假設相依性並不存在，或都提供 |
| `postfile` | `--postread` <br> `-R` | action="append", dest="postfile", default=[] | 在解析完 bitbake.conf 之後，額外解析指定的檔案 <br> 如果 os.environ 中有 BBPOSTCONF 資訊，將會自動加入 `postfile` 變數 |
| `prefile` | `--read` <br> `-r` | action="append", dest="prefile", default=[] | 在解析 bitbake.conf 之前，額外解析指定的檔案 <br> 如果 os.environ 中有 BBPPRECONF 資訊，將會自動加入 `prefile` 變數 |

