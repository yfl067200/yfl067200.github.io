
# A 開頭

| 變數名稱              | 說明                                                                                                                   |
| :---------------- | :------------------------------------------------------------------------------------------------------------------- |
| `ASSUME_PROVIDED` | Recipe 名稱 (即變數 PN) 列表 <br>Bitbake 將會認為這些 Recipe 已經編譯完成，不會進行編譯 <br>在 OpenEmbedded-Core 中，這個變數多半是編譯使用的工具；如 git-native  |
| `AZ_SAS`          | 這是 Azure Storage Shared Access Signature <br>供 OpenEmbedded-Core 中的 "Azure Storage Fetcher" 從 Azure 下載程式碼時，用來驗證的 Key |

>[!NOTE]
>這邊是 Microsoft Azure Storage 的[官方文件]([https://docs.microsoft.com/en-us/azure/storage/common/storage-sas-overview](https://docs.microsoft.com/en-us/azure/storage/common/storage-sas-overview))。如對 Azure 有興趣，可以查詢。目前 `AZ_SAS` 的內容大概是長這樣，不確定為何使用==兩個==雙引號
>
> `AZ_SAS = ""se=2021-01-01&sp=r&sv=2018-11-09&sr=c&skoid=<skoid>&sig=<signature>""`

# B 開頭

| 變數名稱         | 說明                                 |
| :----------- | :--------------------------------- |
| `B`          | Bitbake 在編譯過程中，執行函式所在的目錄           |
| `BITBAKE_UI` | 執行 Bitbake 的 UI 模組                 |
| `BUILDNAME`  | 本次編譯的名稱，預設為開始執行編譯時的 datetime stamp |
| `BZRDIR`     | 下載相關變數                             |
## BB_ 開頭

數量龐大，與 `BB` 開頭的變數不同，比較像是 Bitbake 設定的雜項設定項目。

| 變數名稱                           | 說明                                                                                                                                                                |
| :----------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `BB_ALLOWED_NETWORKS`          | 下載相關變數                                                                                                                                                            |
| `BB_BASEHASH_IGNORE_VARS`      | 請查閱 [[Projects]] 一章中，名為 Signatures 的流程                                                                                                                         |
| `BB_CACHEDIR`                  | Bitbake 設定相關變數，與 Bitbake 可用資源相關                                                                                                                                   |
| `BB_CHECK_SSL_CERTS`           | Fetcher 下載時，是否需要檢查 SSL vertifications <br>預設值為 '1'，改為 '0' 將不檢查 SSL Certification 資料                                                                               |
| `BB_CONSOLELOG`                | Bitbake 輸出的 log 檔案路徑                                                                                                                                              |
| `BB_CURRENTTASK`               | Bitbake 目前執行的工作名稱，不包含 "do_" 函式前置語                                                                                                                                 |
| `BB_DANGLINGAPPENDS_WARNONLY`  | 當 Recipe Append 檔案不能找到對應的 Recipe 檔案時，是否只報 Warning                                                                                                                 |
| `BB_DEFAULT_TASK`              | 當執行 Bitbake 並未使用 "-c" 參數時，預設的工作 <br>不包含 "do_" 函式前置語                                                                                                               |
| `BB_DEFAULT_UMASK`             | 執行工作時，預設的 umask 值                                                                                                                                                 |
| `BB_DISKMON_DIRS`              | 系統資源相關變數，Disk Monitor 相關                                                                                                                                          |
| `BB_DISKMON_WARNINTERVAL`      | 系統資源相關變數，Disk Monitor 相關                                                                                                                                          |
| `BB_ENV_PASSTHROUGH`           | 編譯環境相關變數                                                                                                                                                          |
| `BB_ENV_PASSTHROUGH_ADDITIONS` | 編譯環境相關變數                                                                                                                                                          |
| `BB_FETCH_PREMIRRORONLY`       | 下載相關變數                                                                                                                                                            |
| `BB_FILENAME`                  | 目前工作使用的 Recipe 檔案名稱，為完整路徑                                                                                                                                         |
| `BB_GERERATE_MIRROR_TARBALLS`  | 將值設定為 '1'，會把 git repositories 的資料，包含 Git metadata，下載到變數 `DL_DIR` 路徑下                                                                                              |
| `BB_GENERATE_SHALLOW_TARBALLS` | 類似變數 `BB_GENERATE_MIRROT_TARBALLS`，但是只下載部分資料 <br>由 2 變數 `BB_GERERATE_MIRROR_TARBALLS` 與 `BB_GIT_SHALLOW`，決定本變數是否能執行 <br>而變數 `BB_GITSHALLOW_DEPTH` 決定下載 commit 的數量 |
| `BB_GIT_SHALLOW`               | 是否支援部分下載                                                                                                                                                          |
| `BB_GIT_SHALLOW_DEPTH`         | 指定下載多少筆更新紀錄，從 SRCREV 為基礎                                                                                                                                          |
| `BB_GLOBAL_PYMODULES`          | 決定那些 Python Modules 會被載入自 global space                                                                                                                            |
| `BB_HASH_CODEPARSER_VALS`      | 傳遞給 Bitbake 用於 "Signatures" 流程的變數清單                                                                                                                               |
| `BB_HASHCHECK_FUNCTION`        | 傳遞給 Bitbake 用於 "setscene" 流程的函式名稱，該函式將回傳在 "setscene" 流程中被呼叫的函式清單                                                                                                  |
| `BB_HASHCHECK_IGNORE_VARS`     | 傳遞給 Bitbake 用於 "Signatures" 流程的變數清單，這些變數將不會被用於計算 Checksum                                                                                                         |
| `BB_HASHSERVE`                 |                                                                                                                                                                   |
| `BB_HASHSERVE_UPSTREAM`        |                                                                                                                                                                   |
| `BB_INVALIDCONF`               | 在 ConfigParsed 事件中使用，可以觸發重新解析 Metadata 的動作                                                                                                                        |
| `BB_LOADFACTOR_MAX`            | Bitbake 設定相關變數，與 Bitbake 可用資源相關                                                                                                                                   |
| `BB_LOGCONFIG`                 | 指定包含 logging 設定的 Configuration 檔案                                                                                                                                 |
| `BB_LOGFMT`                    | 指定 log 檔案的名稱，預設為 `log.${TASK}.${PID}`<br>可於某個 Configuration 檔案設定                                                                                                  |
| `BB_MULTI_PROVIDER_ALLOWED`    |                                                                                                                                                                   |
| `BB_NICE_LEVEL`                | Bitbake 設定相關變數，與 Bitbake 可用資源相關                                                                                                                                   |
| `BB_NO_NETWORK`                | Bitbake 設定相關變數，與 Bitbake 可用資源相關                                                                                                                                   |
| `BB_NUMBER_PARSE_THREADS`      | Bitbake 設定相關變數，與 Bitbake 可用資源相關                                                                                                                                   |
| `BB_NUMBER_THREADS`            | Bitbake 設定相關變數，與 Bitbake 可用資源相關                                                                                                                                   |
| `BB_ORIGENV`                   | 編譯環境相關變數                                                                                                                                                          |
| `BB_PRESERVE_ENV`              | 編譯環境相關變數<br>                                                                                                                                                      |
| `BB_PRESSURE_MAX_CPU`          | Bitbake 設定相關變數，與 Bitbake 可用資源相關<br>                                                                                                                               |
| `BB_PRESSURE_MAX_IO`           | Bitbake 設定相關變數，與 Bitbake 可用資源相關                                                                                                                                   |
| `BB_PRESSURE_MAX_MEMORY`       | Bitbake 設定相關變數，與 Bitbake 可用資源相關                                                                                                                                   |
| `BB_RUNFMT`                    | Bitbake 產生的工作 Script 檔案名稱，預設為 `run.${TASK}.${PID}`                                                                                                                |
| `BB_RUNTASK`                   | 目前執行的工作名稱，包含 "do_" Prefix                                                                                                                                         |
| `BB_SCHEDULER`                 |                                                                                                                                                                   |
| `BB_SCHEDULERS`                |                                                                                                                                                                   |
| `BB_SETSCENE_DEPVALID`         |                                                                                                                                                                   |
| `BB_SIGNATURE_EXCLUDE_FLAGS`   |                                                                                                                                                                   |
| `BB_SIGNATURE_HANDLER`         |                                                                                                                                                                   |
| `BB_SRCREV_POLICY`             | 定義 Fetcher 是否可以不使用 Source Control System：<br>- `cache` ：不使用，使用目前的檔案<br>- `clear` ：使用，此為預設值                                                                        |
| `BB_STRICT_CHECKSUM`           | 確保遠端下載需要 1 個以上的 Checksum 檢查                                                                                                                                       |
| `BB_TASK_IONICE_LEVEL`         | Bitbake 設定相關變數，與 Bitbake 可用資源相關                                                                                                                                   |
| `BB_TASK_NICE_LEVEL`           | Bitbake 設定相關變數，與 Bitbake 可用資源相關                                                                                                                                   |
| `BB_TASKHASH`                  | 該變數保留目前工作透過 Signature Generator 產生的值                                                                                                                              |
| `BB_VERBOSE_LOGS`              | Shell Scripts 的 `echo` 命令或是 output 是否會顯示在 stdout                                                                                                                  |
| `BB_WORKERCONTEXT`             | 檢查 Thread 是否在執行工作<br>"1" 表示是，其他可能是在處理 Parsing 或是 Event                                                                                                            |

## BB 開頭

與 `LAYER` 開頭的變數用來設定 Bitbake 如何處理 layers 與其下的 Class 檔案與 Recipe 檔案。請參考[[Variables]]下的 "layer.conf 檔案" 一節

| 變數                    | 說明                                                                                   |
| :-------------------- | ------------------------------------------------------------------------------------ |
| `BBCLASSEXTEND`       | 這個變數是使用在 ==Recipe 檔案==中，用來說明額外資訊<br>                                                 |
| `BBDEBUG`             | 設定 Bitbake debug level。可以透過 Bitbake 命令參數 "-D" 來增加                                    |
| `BBINCLUDED`          | 使用 `include` 或是 `required` 關鍵字，所引用的檔案清單                                              |
| `BBINCLIDELOGS`       | 如果有設定此變數，將會建立 Log 紀錄執行失敗的 Task                                                       |
| `BBINCLUDELOGS_LINES` | 如果變數 `BBINCLIDELOGS` 有設定，這邊將設定 Log 檔案的大小 <br>如果沒有設定此變數，將會進行完整記錄                      |
| `BBLAYERS`            | 只存在於 bblayer.conf 檔案，用來標記專案使用到的目錄清單                                                  |
| `BBLAYERS_FETCH_DIR`  |                                                                                      |
| `BBMASK`              | 列出不須編譯的條件，任何符合條件的 Recipe 與 Recipe Append 檔案都不會被編譯                                    |
| `BBMULTICONFIG`       | 因為 OpenEmbedded-Core 支援多重 Configuration<br>在 local.conf 檔案中指定所支援的多重 Configuration 清單 |
| `BBPATH`              | Layer 變數相關變數                                                                         |
| `BBSERVER`            |                                                                                      |
| `BBTARGETS`           | Bitbake 指令指定的 target 之外，使用此變數增加 target                                               |

## BBFILE_ 開頭

以下 "BBFILE_" 開頭的變數，內容都來自各 Layers 下 conf/layer.conf 檔案的同名變數。請參考[[Variables]]下的 "layer.conf 檔案" 一節

# C 開頭

| 變數名稱     | 說明                             |
| :------- | :----------------------------- |
| `CACHE`  | Bitbake 設定相關變數，與 Bitbake 可用資源相 |
| `CVSDIR` | 下載相關變數                         |

# D 開頭

| 變數名稱                 | 說明                                                                                                                      |
| :------------------- | :---------------------------------------------------------------------------------------------------------------------- |
| `DEFAULT_PREFERENCE` | 同一個程式會有多種不同版本，而各版本會有不同的 Recipe 檔案 <br>本變數是用來標示哪一個版本的 Recipe 應該被執行，數字愈高愈好<br>通常在開發版本的 Recipe 檔案中，會將該變數設為 "-1"，避免被一版使用者使用 |
| `DEPENDS`            | Recipe 檔案 (.bb) 中的同名變數，用來說明該 Recipe 在==編譯時==的相依性 <br>與另一個類似的變數 `RDEPENDS` 不同，該變數是指==執行時==的相依性                           |
| `DESCRIPTION`        | Recipe 檔案 (.bb) 中的同名變數                                                                                                  |
| `DL_DIR`             | 來自於專案使用的 local.conf 檔案 <br>用來儲存下載資料的目錄，但不包括 git repository                                                              |

# E 開頭

| 變數名稱                 | 說明                                                      |
| :------------------- | :------------------------------------------------------ |
| `EXCLUDE_FROM_WORLD` | 將本變數設為 "1" 表示本 Recipe 不能成為 Bitbake 的 Target，設為 '0' 表示可以 |
> [!NOTE]
> 所謂的 "World Builds" 就是指 Bitbake 會從 bblayers.conf 這個 Configuration 檔案中，定位、解析、與編譯 Layers 下的==所有  Recipe 檔案==。

# F 開頭

| 變數名稱              | 說明                                            |
| :---------------- | :-------------------------------------------- |
| `FAKEROOT`        | 已被其他 `FAKEROOT` 變數取代                          |
| `FAKEROOTBASEENV` | 在使用變數 `FAKEROOTCMD` 切換環境之後，需要建立的環境變數          |
| `FAKEROOTCMD`     | Bitbake 切換到 FAKEROOT 環境的命令                    |
| `FAKEROOTDIRS`    | 在執行 FAKEROOT 環境所需的目錄列表                        |
| `FAKEROOTENV`     | 在 FAKEROOT 的環境變數                              |
| `FAKEROOTNOENV`   | 非 FAKEROOT 的環境變數                              |
| `FETCHCMD`        | Bitbake 的 fetcher 使用的命令                       |
| `FILE`            | 進行 Parsing 與執行 Task 時，該 Recipe 檔案的名稱          |
| `FILE_LAYERNAME`  | 進行 Parsing 與執行 Task 時，該 Recipe 檔案所在的 Layer 名稱 |
| `FILESPATH`       | Bitbake 用來搜尋 patch 與其他檔案                      |

# G 開頭

| 變數名稱     | 說明     |
| :------- | :----- |
| `GITDIR` | 下載相關變數 |

# H 開頭

| 變數名稱       | 說明             |
| :--------- | :------------- |
| `HGDIR`    | 下載相關變數         |
| `HOMEPAGE` | Recipe 檔案中同名變數 |

# I 開頭

| 變數名稱 | 說明 |
|:-----------|:------|
| `INHERIT` |  使用關鍵字 `inherit` 或是 `INHERIT` 載入的共用 Class 檔案 |

# L 開頭

與 `BB` 開頭的變數用來設定 Bitbake 如何處理 layers 與其下的 Class 檔案與 Recipe 檔案，請參考[[Variables]]下的 "layer.conf 檔案" 一節

# M 開頭

| 變數名稱     | 說明     |
| :------- | :----- |
| `MIRROR` | 下載相關變數 |

# O 開頭

| 變數名稱        | 說明           |
| :---------- | :----------- |
| `OVERRIDES` | 用來處理條件式擴展的變數 |

# P 開頭

| 變數名稱                  | 說明                                           |
| :-------------------- | :------------------------------------------- |
| `P4DIR`               | 下載相關變數                                       |
| `PACKAGES`            | 本 Recipe 所建立的 Package 清單                     |
| `PACKAGES_DYNAMIC`    | Recipe 中對外宣稱會提供的 Package，供其他 Package 在執行期間使用 |
| `PE`                  | Package 相關變數                                 |
| `PERSISTEND_DIR`      | Bitbake 在編譯期間，所建立的非暫存檔案所在的目錄                 |
| `PF`                  | Package 相關變數                                 |
| `PN`                  | Package 相關變數                                 |
| `PR`                  | Package 相關變數                                 |
| `PREFERRED_PROVIDER`  | Package 相關變數，別名相關                            |
| `PREFFERED_PROVIDERS` | Package 相關變數，別名相關                            |
| `PREFFERED_VERSION`   | Package 相關變數，版本相關                            |
| `PREMIRRORS`          | 下載相關變數，尋找順序相關                                |
| `PROVIDES`            | Package 相關變數，別名相關                            |
| `PRSERV_HOST`         |                                              |
| `PV`                  | Package 相關變數                                 |

# R 開頭

| 變數名稱               | 說明                       |
| :----------------- | :----------------------- |
| `RDEPENDS`         | 類似變數 `DEPENDS`，列出執行時的相依性 |
| `REPODIR`          | 本地端存放 google-repo 目錄     |
| `REQUIRED_VERSION` | 針對 Class 指定必須使用哪一個版本     |
| `RPROVIDES`        | Package 相關變數，別名相關        |
| `RRECOMMENDS`      | Package 相關變數，別名相關        |

# S 開頭

| 變數名稱            | 說明                                                            |
| :-------------- | :------------------------------------------------------------ |
| `SECTION`       |                                                               |
| `SRC_URI`       | 於 Recipe 檔案中建立<br>下載相關變數，Fetcher 相關                           |
| `SRCDATE`       | 只有從 Source Code Manager (SCM) 系統下載程式碼時會用到，指定程式碼的日期            |
| `SRCREV`        | 從 Subversion、Git、Mercurial、與 Bazaar 等 CVS 系統中，下載程式碼的 Revision |
| `SRCREV_FORMAT` | 用來產生變數 `SRCREV` 的格式                                           |
| `STAMP`         | Bitbake 建立 STAMP 檔案所在目錄的基本名稱                                  |
| `STAMPCLEAN`    |                                                               |
| `SUMMARY`       | Recipe 檔案中同名變數 <br>長度限制為 72 個字元                               |
| `SVNDIR`        | 下載相關變數，Fetcher 相關                                             |

## SRC_URI 的額外選項

透過 SRC_URI 指定下載的工具與位置之外，尚有其他選項可供 Bitbake 執行特殊行動

| 選項                 | 說明                                                      |
| :----------------- | :------------------------------------------------------ |
| `downloadfilename` | 將下載的個別程式檔案改名                                            |
| `name`             | 給予下載的個別程式碼一個專屬的代號，用來搭配變數 `SRCREV` 或是 `SRC_URI` checksum |
| `subdir`           | 下載的個別程式碼存放的子目錄                                          |
| `subpath`          |                                                         |
| `unpack`           | 用來指定是否要解壓縮下載的檔案。預設是要                                    |
|                    |                                                         |
``` python
#    針對不同位置下載的程式碼，給予他們不同的代號
SRC_URI = "git://example.com/foo.git;branch=main;name=first \
           git://example.com/bar.git;branch=main;name=second \
           http://example.com/file.tar.gz;name=third"

#    透過代號，指定對應的 checksum
SRCREV_first = "f1d2d2f924e986ac86fdf7b36c94bcdf32beec15"
SRCREV_second = "e242ed3bffccdf271b7fbaf34ed72d089537b42f"
SRC_URI[third.sha256sum] = "13550350a8681c84c861aac2e5b440161c2b33a3e4f302ac680ca5b686de48de"
```

## SRC_URI 支援的 Protocol

| Protocol 名稱 | 說明 |
|:----------------|:-----|
| `az://` |  使用 Azure Storage fetcher |
| `bzr://` | 使用 Bazaar fetcher |
| `ccrc://` | 使用 ClearCase fetcher |
| `crate://` | 使用 Crate fetcher |
| `cvs://` | 使用 CVS fetcher |
| `file://` | 使用 Local File fetcher 從本地端取得檔案，該檔案多半與 Metadata 一起獲得 |
| `ftp://` | 使用 Wget fetcher，透過 wget 工具從遠端下載檔案 |
| `git://` | 使用 Git fetcher |
| `gitannex://` | 使用 Git Annex fetcher |
| `gitsm://` | 使用 Git SubModule fetcher |
| `gs://` | 使用 Google Cloud Storage fetcher |
| `hg://` | 使用 Mercurial fetcher |
| `http://` | 使用 Wget fetcher，透過 wget 工具從遠端下載檔案 |
| `https://` | 使用 Wget fetcher，透過 wget 工具從遠端下載檔案 |
| `npm://` | 使用 NPM fetcher |
| `npmsw://` | 使用 NPM ShinkWrap fetcher |
| `osc://` | 使用 OSC fetcher |
| `p4://` | 使用 Perforce fetcher |
| `repo://` | 使用 Repo fetcher |
| `s3://` | 使用 S3 fetcher |
| `sftp://` | 使用 Secure FTP fetcher |
| `ssh://` | 使用 Secure Shell fetcher |
| `svn://` | 使用 SVN fetcher |

# T 開頭

| 變數名稱     | 說明                                         |
| :------- | :----------------------------------------- |
| `T`      | Bitbake 編譯某個 Recipe 時，用來存放 Log 或是自動產生檔案的目錄 |
| `TOPDIR` | Bitbake 在編譯時使用的目錄 (Build Directory)        |
