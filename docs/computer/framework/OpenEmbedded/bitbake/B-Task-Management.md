
工作管理

Bitbake 會使用 varflags 進行對工作與相依性的管理。Bitbake 針對某個工作所建立的 varflags，其 varflags 的名稱為 "do_${TASK}"，與工作使用的函式同名。存取某個工作的 varflags，就是使用以下命令，以工作 do_configure 為例：

> do_configure[ ${X} ]    # 取得 do_configure 工作中的 varflags 中，Key 為 "${X}" 的值

以下為內建於 Bitbake 中，用來管理 Task 的 varflags：

| Key 名稱             | 說明                                                                                  |
| :----------------- | :---------------------------------------------------------------------------------- |
| `cleandirs`        | 在執行工作前，必須建立的空白目錄列表 <br>假設該目錄已存在，將會先移除再重新建立                                          |
| `depends`          |                                                                                     |
| `deptask`          |                                                                                     |
| `dirs`             | 在執行工作前，必須建立的目錄列表 <br>假設該目錄已存在，保持該目錄的現狀 <br>最後一個目錄將會作為該工作的 current working directory |
| `file-checksums`   |                                                                                     |
| `lockfiles`        | 用來作為鎖的檔案列表                                                                          |
| `network`          | 當其值為 "1" 的時候，該工作可以使用網路 <br>預設只有 do_fetch() 工作可以使用網路                                 |
| `noexec`           | 當其值為 "1" 的時候，該工作不須執行                                                                |
| `nostamp`          | 當其值為 "1" 的時候，Bitbake 執行該工作時不產生 stamp 檔案，故該工作必須每次都執行                                 |
| `number_threads`   | 該工作可以同時執行多少個 CPU Thread <br>該值等同於變數 BB_NUMBER_THREAD                                |
| `postfuncs`        | 該工作執行之後，應該呼叫的函式列表 <br>類似 do_${TASK}:append                                          |
| `prefuncs`         | 該工作執行之前，應該呼叫的函式列表 <br>類似 do_${TASK}:prepend                                         |
| `rdepends`         |                                                                                     |
| `rdeptask`         |                                                                                     |
| `recideptask`      |                                                                                     |
| `recrdeptask`      |                                                                                     |
| `stamp-extra-info` | 該工作的 stamp 檔案中，額外加入的資訊                                                              |
| `umask`            | 執行該工作時，所設定的 umask 值                                                                 |

除了上述的 Key 之外，尚有以下用來進行 Checksum 或是 Signatures 使用的 Key

| Key 名稱               | 說明                                                    |
| :------------------- | :---------------------------------------------------- |
| `vardeps`            | 變數列表，使用空白作為分隔符號 <br>Bitbake 將會計算這些變數的 Signature       |
| `vardepsexclude`     | 變數列表，使用空白作為分隔符號 <br>列出在 `vardeps` 中不須計算 Signature 的變數 |
| `vardepvalue`        | 如果有設定，將使用該值用來用來計算變數的 Signature                        |
| `vardepvalueexclude` | 數值列表，使用 "\|" 作為分隔符號 <br>計算變數的 Signature 時不使用這些數值      |




