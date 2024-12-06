Bitbake 在解析 Metadata 時，有他自己定義的規範。以下將說明 Bitbake 如何

# 共享函式與變數

Metadata 中有兩種檔案是用來分享共用的函式與變數，這兩個檔案就是 Class 檔案與 Include 檔案。透過使用 `inherit`、 `include`、`require`、與 `INHERIT` 關鍵字，指定需要載入的 Class 檔案或是 Include 檔案，Bitbake 將會從變數 BBLAYERS 所列的目錄中，尋找符合檔名的檔案；當找到檔案之後，將會把完整的檔名塞進變數 BBPATH 中。如果有多個符合檔名的檔案存在變數 BBPATH　中，第一個被找到的檔案才會被載入。

| 關鍵字       | 搭配的檔案               | 特殊條件                                                                                                                           |
| :-------- | :------------------ | :----------------------------------------------------------------------------------------------------------------------------- |
| `include` | 任何檔案                | 一個 `include` 關鍵字只能搭配一個檔案，包含副檔名<br>載入的檔案沒有限制<br>檔案內容會被複製到檔案中 `include` 關鍵字處 <br>如果檔案不存在，不會有產生錯誤                                 |
| `inherit` | Class 檔案 (.bbclass) | 一個 `inherit` 關鍵字可以搭配多個檔名 (**不**須副檔名)<br>載入的檔案必須是 Class 檔案<br>只能使用在 Recipe 檔案或是 Class 檔案<br>可以透過[條件式命令]，從共享檔案中得到不同的變數值或是呼叫不同的函式 |
| `INHERIT` | Class 檔案 (.bbclass) | 與 `inherit` 相同<br>只使用於 Configuration 檔案                                                                                        |
| `require` | 任何檔案                | 與 `include` 相同<br>檔案不存在時，會產生錯誤                                                                                                 |
> [!IMPORTANT]
> 不同版本的 Recipe 檔案，可以將共用的函示與變數放進一個 Include 檔案。
> 不同版本的 Recipe 檔案可以透過 `include` 或是 `require` 關鍵字將該 Include 檔案的內容複製進來，使用共同的函式或是變數。
> 不能使用 `inherit` 原因是，共用的函式與變數是載入 Include 檔案，而非 Class 檔案。

> [!NOTE]
> 雖然 `INHERIT` 與 `inherit` 關鍵字都是載入 Class 檔案。但是因為 `INHERIT` 是在 Configuration 檔案載入，可以變成一個類似 Global 型態的 class 供所有 Recipe 檔案使用。故必較推薦使用 `INHERIT`

## 共享函式

使用 `INHERIT` 或是 `inherit` 關鍵字並不會複製 Class 檔案內容到 Recipe 檔案中，所以必須有個方法將 Class 檔案中的函式分享出來。這個方法在 Class 檔案中就是使用關鍵字 `EXPORT_FUNCTIONS`，將要分享的函式使用以下的語法分享出來。

> EXPORT_FUNCTIONS ${FUNCTION_NAME}

舉個例，假設我們要在 foo.bbclass 檔案中分享了一個函式叫做 do_bar()，我們需要在 foo.bbclass 檔案中加上命令

> EXPORT_FUNCTIONS do_bar

在 Recipe 檔案使用 `inherit` 繼承 foo 這個 Class 檔案之後，就可以呼叫 do_bar 這個檔案。不過因為是透過繼承，所以呼叫 do_bar() 函式會變成呼叫 Recipe 檔案中的 do_bar() 函式；要呼叫 foo 下的 do_bar，要呼叫 `foo_do_bar()`。

> [!NOTE]
> 呼叫繼承的函式，語法是 `${CLASS}_${FUNCTION}`

``` python
inhert foo

# Shell 函式
do_bar() {
	if [ SOME CONDITION ]; then
	    foo_do_bar()    # 執行 foo Class 下的 do_bar() 函式
	else
	    ...
	fi
}
```

## 定義函式

既然知道如何使用共享函式，就必須知道該如何定義共享函式。Bitbake 支援多種函式，包含以下 4 種。這些函式只能存在於 Class 檔案 (.bbclass) 或是 Recipe 檔案 (.bb) 中，且多為 Python 類函式

| 函式種類                    | 說明                                                                  |
| :---------------------- | :------------------------------------------------------------------ |
| Shell 函式                | Shell 函式                                                            |
| Bitbake-Style Python 函式 | 由 Bitbake 執行的 python 函式<br>可以在 Python 函式中透過 bb.build.exec_func() 執行 |
| Python 函式               | 透過 python 執行的 python 函式<br>唯一可以有參數與傳回值的函式                           |
| 無命名的 Python 函式          | 在解析階段自動執行的 python 函式                                                |

### Shell 函式

Bitbake 可以接受的 shell 函式定義如下：
``` bash
some_function() {
   ...     # Shell Script
}
```

當 Bitbake 呼叫 shell 函式時，他會透過 /bin/sh 執行。因為 /bin/sh 在不同平台上對應的 shell 不同，不可使用 shell 特殊的語法與功能。

> [!NOTE]
> Shell 函式的好處在於，可以額外定義 some_function:preped() 或是 some_function:append() 函式；這兩個函式會再呼叫 some_function() 函式之前與之後會被自動呼叫到

### Python 類函式

Bitbake 可以接受的 Bitbake-Style Python 函式定義如下，類似 Shell 函式的定義：
``` python
python some_python_function() {
   ...     # python script
}
```

 而 Python 函式的定義跟一般的 Python 函式相同：
``` python
def python_function( params ):
    ...     # python script
```

#### 兩者的相同點：

- Bitbake 會自動幫忙載入 bb 與 os 兩個模組，故 Python 類函式可自行使用該模組下的 API
- 兩種函式都無法進行變數擴展的動作，不可使用 `${X}`、`${@X()}` 或是 `${@"0" if True}`這些需要進行變數展開動作的程式碼

#### 兩者的差異點：

1. Bitbake 內建的資料倉儲 (datastore) 'd' 可供 Bitbake-Style Python 函式使用
    1-1. 變數 Flag，如 dirs、cleandirs 與 lockfiles，只能供 Bitbake-Style Python 函式使用
3. Python 函式可接受參數並回傳結果，而 Bitbake-Style Python 函式不可
    2-1. Bitbake 內建的資料倉儲 (datastore) 'd' 可以做為參數供 Python 函式使用
3. Bitbake-Style Python 函式同 Shell 函式，可以使用 some_python_function:prepend() 與 some_python_function:append()
4. 執行的方式不同，唯有 Bitbake-Style Python 函式可以被當作工作 (Task)

> [!IMPORTANT]
> 
> Bitbake-Style Python 函式與 Python 函式的最大差異在於如何執行
> 
> - Python 函式可以透過呼叫 python，或是透過[內部 Python 解析]執行。
> 
> - Bitbake-Stlye Python 函式也有兩種方式被呼叫
>     1. 在 Python 函式中，透過 bb.build.exec_func("some_python_function", d) 被執行
>         經由 bb.build.exec_func() 執行的可以是 Shell 函式與 Bitbake-Style Python 函式
>         當出現錯誤，可以透過 bb.build.FuncFailed 例外獲得額外資訊
 >    2. 由 Bitbake 自行呼叫
>         Bitbake 會自動產生一個 ${T}/run.some_python_function.pid 的 python script，經由這個去呼叫 Bitbake-Style Python 函式。
>         如果被呼叫的 Bitbake-Style Python 函式是工作 (Task)，則會產生一個 ${T}/log.some_python_function.pid 的log 檔案

### 無命名的 Python 函式

定義同 Bitbake-Style Bitbake 函式，只是沒有函式名稱而已。將函式名稱改為 "\_\_anonymous"，也會有相同的結果
``` python
python () {
    ...     # python script		   
}

python __anonymous() {
    ...     # python script		   
}
```

這種無命名的 Python 函式都是在解析 Metadata 的最後，自動被呼叫，不論是否有定義。如果一個 Recipe 檔案包含多個無命名 Python 函式，將會依照定義的順序，依序全部執行。

這類的函式是在 Recipe 對變數進行調整的最後一個步驟
``` python
FOO := "foo"
python () {
	d.setVar( "FOO", "foo from anonymous" )
}

# 變數 FOO 即便使用覆蓋式行為運算元，將變數 FOO 賦值成 "foo from outside"
FOO:append "from outside"
# 因為無命名的 Python 函式必定是最後執行的函式，變數 FOO 最後被賦值為 "foo from anonymous"
```
# 工作 (Tasks)

Bitbake 將編譯的流程劃分為幾個階段 (phase)。對應各階段有一個名稱為 "do_{TASK}" 的函式，執行這些 "do_{TASK}" 的函式被稱為工作 (task)；這些工作會被定義在 Class 檔案 (.bbclass) 或是 Recipe 檔案 (.bb) 中。在上述 4 種函式中，只有 Shell 函式跟 Bitbake-Style Python 函式可以成為 Task；而將函式提升為 Task 的方法就是透過命令 `addtask`，該命令的語法如下：

> addtask ${TASK} after {PHASE} before {PHASE}

> [!NOTE]
> 作為 Task 的函式，其名稱必須是 `do_${TASK}`；在使用 addtask 命令時，只需使用 `${TASK}` 作為函式名稱

既然有提升的命令，自然有移除工作的命令。那就是 deltask，語法如下：

> deltask ${TASK}

> [!Note]
> 需要注意的是，當移除工作時，工作間的相依性並不會自動修復。舉個例子來說，有 3 個工作 A, B, 與 C；其中 C 依賴 B，而 B 依賴 A。當工作 B 被移除之後，這 3 者的相依性就被打斷了。因此 C 可能會在 A 執行完之前被執行。
> 
> 如果要維持相依性，又想避開某個工作，可以透過調整工作變數名為`noexec` 的 Flag。詳情請參考下面的[Task 變數的 Flag]一節

> [!IMPORTANT]
> 想知道有 Recipe 中會執行的工作有那些，可以使用 Bitbake 命令
> 
> `bitbake ${RECIPE} -c listtasks`

## Task 變數的 Flag

Bitbake 使用變數的 Flag 來進行工作與其相依性的管理。具體的方法就是針對每一個工作，建立一個對應的變數，如變數 `do_configure` 就是用來管理 configure 這個工作的。這些變數的 Flag 主要都是在 Class 檔案或是 Recipe 檔案中被使用

| Flag 名稱            | 說明                                                                               |
| :----------------- | :------------------------------------------------------------------------------- |
| `cleandirs`        | 是否在 Task 開始前建立空的目錄 <br>目錄已存在會被移除，再重建                                             |
| `depends`          | 相依性                                                                              |
| `deptask`          | 相依性                                                                              |
| `dirs`             | 在 Task 開始前需要建立的目錄列表                                                              |
| `file-checksums`   | 工作需要的檔案，大部分都列在變數 `SRC_URI` 中 <br>但是，有時候會需要檔案被某些工作 Generated 出來，這些檔案就會使用本 Flag 紀錄 |
| `lockfiles`        | 用來作為 Lock 的檔案                                                                    |
| `network`          | 當值為 "1" 時，允許工作使用網路 <br>預設只有 do_fetch() 工作可以使用網路                                  |
| `noexec`           | 當值為 "1" 時，表示該工作不須執行 <br>用於處理工作間的相依性                                              |
| `nostamp`          | 當值為 "1" 時，表示 Bitbake 不需要建立 stamp 檔案，所以該工作必須每次都執行                                 |
| `number_threads`   | 與變數 `BB_NUMBER_THREADS` 類似 <br>變數是整個 Bitbake 的設定，這個 Flag 只針對該工作                  |
| `postfuncs`        | 執行本工作之後，需要呼叫的函式列表                                                                |
| `prefuncs`         | 執行本工作之前，需要呼叫的函式列表                                                                |
| `rdepends`         | 相依性                                                                              |
| `redeptask`        | 相依性                                                                              |
| `recideptask`      | 相依性                                                                              |
| `recrdeptask`      | 相依性                                                                              |
| `stamp-extra-info` | 在 stamp 檔案最後加上得額外資訊                                                              |
| `umask`            | 執行工作時，使用 umask 值                                                                 |

### 額外檔案檢查

如同 Flag `file_checksums` 中提到，工作會用到的檔案基本上都列在變數 `SRC_URI` 中。但是某些檔案需要額外處理，譬如說需要 Generated 出來。執行工作時會檢查這些檔案是否存在，並將檢查結果加入 Flag `file_checksums`。

> [!Note]
> Flag `file_checksums` 中的值是多個 "\${FILE_NAME}:\${EXISTED}" 的 pair。其中 "\${FILE_NAME}" 表示檔案或是目錄，而 "\${EXISTED}" 為 "True" 或是 "False" 表示是該檔案或是目錄否存在

### 相依性

Bitbake 可以同時編譯多個不同的 Recipe 檔案，而 Recipe 檔案中會指定進行某個工作前，需要先完成某些工作。藉由解析這些資訊，Bitbake 大量地使用 Flags，像是 `depends`、`deptask`、`rdepends`、`rdeptask`、`recideptask`、與 `recrdeptask`。

## 事件 (Events)

Bitbake 在編譯過程中，會有需多階段，各階段會產生出 Event (事件)。可以透過建立 Event Handler，對於各事件進行進一步的處理

| 事件                          | 說明                                                                                                          |
| :-------------------------- | :---------------------------------------------------------------------------------------------------------- |
| bb.event.ConfigParsed()     | 會多次觸發 <br>於 bitbake.conf、base.class、或是任何 `INHERIT` 敘述句被解析時觸發 <br>可以透過改變資料倉儲中變數 `BB_INVALIDCONF`，進行重新解析      |
| bb.event.ParseStart()       | 觸發於開始解析 Recipe 檔案 <br>事件屬性 `total` 用來說明總共有多少 Recipe 檔案                                                      |
| bb.event.ParseProgress()    | 觸發於解析 Recipe 檔案 <br>事件屬性 `current` 用來說明已解析多少 Recipe 檔案                                                      |
| bb.event.ParseCompleted()   | 觸發於開始解析 Recipe 檔案 <br>事件屬性 `cached`、`parsed` 、`skipped`、`virtuals`、`masked`、與 `errors` 用來說明解析 Recipe 檔案時的結果 |
| bb.event.BuildStart()       | Bitbake 可以設定多種 Configuration，每開始編譯一個 Configuration，就會觸發一次                                                   |
| bb.event.BuildCompleted()   | Bitbake 編譯完一次 Configuration，就會觸發一次                                                                          |
| bb.event.TaskStarted()      | 工作開始就會觸發一次<br>事件屬性 `taskfile` 說明 Recipe 檔案名稱、`taskname` 說明函數名稱、`logfile` 標示 output 檔案、`time` 標示開始執行的時間      |
| bb.event.TaskInvalid()      | 觸發於當工作找不到對應的函式                                                                                              |
| bb.event.TaskFailedSilent() | 觸發於當執行 setscene 工作失敗                                                                                        |
| bb.event.TaskFailed()       | 觸發於工作執行時出現錯誤                                                                                                |
| bb.event.TaskSuccessed()    | 觸發於工作成功執行完畢                                                                                                 |
| bb.event.HeartbeatEvent()   | 預設為每秒觸發一次 <br>可以透過變數 `BB_HEARTBEAT_EVENT` 調整觸發的時間                                                           |
| bb.cooker.CookerExit()      | 觸發於 Bitbake server/cooker 被 shut down                                                                       |

> [!NOTE]
> BuildStart() 與 BuildCompleted() 是一個特殊的事件，跟 Bitbake 支援 [多重 Configuration 檔案] 相關，詳情請參考 [[01-Overview]] 一章。當這兩個事件觸發時，Bitbake 尚未解析 Metadata。

### Event Handler

Event Handler 可以建立於 Class 檔案與 Recipe 檔案。下面案例說明如何建立一個 Event Handler。
```python
addhandler myclass_handler    # Register myclass_handler function as event handler

# Samples
python myclass_handler() {
	from bb.event import getName
	print("The name of the event is %s" % getName(e))
	print("The file we run for is %s" % d.getVar('FILE'))
}

# By using eventmask for calling myclass_handler function, when
# event bb.event.BuildStarted and bb.event.BuildCompleted triggered
myclass_handler[eventmask] = "bb.event.BuildStarted bb.event.BuildCompleted"
```

> [!IIMPORTANT]
> Event Handler 可以使用全域變數 `e` 來處理觸發的事件，亦可使用資料倉儲 `d` 存取變數。

### 特殊事件

| 事件                                      | 說明                                    |
| :-------------------------------------- | :------------------------------------ |
| bb.event.TreeDataPreparationStarted()   |                                       |
| bb.event.TreeDataPreparationProgress()  |                                       |
| bb.event.TreeDataPreparationCompleted() |                                       |
| bb.event.DepTreeGenerated()             |                                       |
| bb.event.CoreBaseFilesFound()           |                                       |
| bb.event.ConfigFilePathFound()          |                                       |
| bb.event.FilesMatchingFound()           |                                       |
| bb.event.ConfigFileFound()              |                                       |
| bb.event.TargetsTreeGenerated()         |                                       |

## 環境變數

在 [[02-Projects]] 一章中有提到，要編譯 Yocto 專案前，必須要先執行 `. ./setup ${PROJECT} ${DEST}` 命令。這是因為 Bitbake 會清除編譯時的環境變數，並套用自己所需要的環境變數。透過以下變數，Bitbake 得以做到控制環境變數：

- 變數 `BB_PRESERVE_ENV` - 決定是否要清除現有的環境變數
- 變數 `BB_ENV_PASSTHROUGH` 與 `BB_ENV_PASSTHROUGH_ADDITIONS` - 將 Bitbake 變數名稱放進這兩個變數中，可供 Bitbake 通過資料倉儲 (datastore) "d" 存取。

> [!NOTE]
> 透過變數 `BB_ENV_PASSTHROUGH` 與 `BB_ENV_PASSTHROUGH_ADDITIONS`，Bitbake 可以將編譯時所需的變數置於資料倉儲。但是要將這些變數變成環境變數，還是需要透過 `export ${VAR}` 命令，將變數變成環境變數供所有工作使用
> 
> 執行 `export ${VAR}` 命令的時機，應該是專案中的 conf/local.conf 檔案

> [!IMPORTANT]
> 
> 加入變數 `BB_ENV_PASSTHROUGH` 與 `BB_ENV_PASSTHROUGH_ADDITIONS` 的值會影響到 setscene checksum，導致某些工作必須重複的執行。需要透過設定
