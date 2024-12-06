
# 編譯環境

為了能做到在各種環境下都能透過 Bitbake 編譯，Bitbake 會將目前的環境變數清除，然後載入自帶的環境變數。透過以下變數，Bitbake 得以做到改變環境變數：

-  變數 `BB_ORIGENV` - 用來儲存原本的環境變數資料，存放在資料倉儲 "d" 中
-  變數 `BB_PRESERVE_ENV` - 是否將原本環境變數，導入 Bitbake 環境
-  變數 `BB_ENV_PASSTHOUGH` 與 `BB_ENV_PASSTHROUGH_ADDITIONS` - 將環境變數加入 Bitbake 的資料倉儲中，必須於外部先建立相關的變數，再透過這兩個變數將外部變數加入 Bitbake 中。

其中的變數 `BB_ORIGENV` 會置於資料倉儲 (datastore) "d" 中。可以透過命令 `getVar( ${VAR}, False)` 取得變數。
``` Python
orienv = d.getVar("BB_ORIGENV", False)    # 從資料倉儲 "d" 中，取得原本的環境變數
bar = orienv.getVar("BAR", False)         # 變數 "BB_ORIGENV" 的型態是變數 Flag
```

## 系統資源相關

與系統資源相關的變數包含

- `BB_DISKMON_DIRS` - 這類檢查硬碟空間是否足夠，或是 
- `BB_DISKMON_WARNINTERVAL` - 
- `BB_NUMBER_THREADS` - 標示編譯可以使用的 Processor 數量。

### Disk Monitor 相關

Bitbake 在進行編譯時，會透過變數 `BB_DISKMON_DIRS` 監控硬碟空間與 inode 使用數量。變數的結構是：

```
ITEM = "${ACTION},${PATH},${THRESHOLD}"
BB_DISKMON_DIRS = "${ITEM} [${ITEM} ...]"

其中 ACTION 用來表示當空間或是 inode 數量不足時的處理方式，包含：
- HALT - 立刻停止編譯動作
- STOPTASKS - 等執行中的工作結束之後，停止編譯
- WARN - 顯示警告訊息，但繼續進行編譯。其後的 ${THRESHOLD} 參數將不被使用，而是由另一個變數 `BB_DISKMON_WARNINTERVAL` 提供。

PATH 為路徑

THRESHOLD 是最小剩餘空間與最小剩餘 inode 數量，兩者間用逗號 ',' 分隔；
          使用的單位可以是 "G"、"M"、或是 "K"，預設為 "K"
```

變數 `BB_DISKMON_WARNINTERVAL` 只有在變數 `BB_DISKMON_DIRS` 中有設定 WARN 這項 action 才會有用，其值為將會取代採用 WARN 這項 action 的 ${THRESHOLD} 變數的值。若是忘了設定 `BB_DISKMON_WARNINTERVAL` 變數，將使用預設值 "50M,50K"。

# Bitbake 設定相關

Bitbake 內部使用 2 個變數 `B` 與 `T`，用來說明 Bitbake 進行編譯時，編譯所用的目錄 (B)，以及存放暫存檔案的目錄 (T)。這兩個變數會因執行的 Recipe 檔案不同而改變。

變數 `TOPDIR` 為 Bitbake 編譯專案時，使用的 build 目錄路徑。

## FAKEROOT 相關


## 雜項設定

使用 `BB_` 開頭的變數多半是用來設定 Bitbake 可使用的資源

### Bitbake 可用資源

| 變數                        | 說明                                                                                             |
| :------------------------ | ---------------------------------------------------------------------------------------------- |
| `BB_CACHEDIR`             | Bitbake Parser 所用的暫存檔案目錄，預設值是 `${TOPDIR}/cache`                                                |
| `BB_LOADFACTOR_MAX`       | Bitbake 會以本變數的值 \* BB_NUMBER_THREADS 作為基準，一旦系統的 loading 超過該基準，就暫時不會啟動新的工作<br>預設值為 1.5          |
| `BB_NO_NETWORK`           | Bitbake Fetcher 不能使用網路下載檔案                                                                     |
| `BB_NICE_LEVEL`           | 調整 Bitbake 本身的 nice value，將會影響系統執行 bitbake 的優先順序<br>nice value 的值介於 -20 ~ 19 之間，19 表示優先順序最低    |
| `BB_NUMBER_PARSE_THREADS` | Bitbake 可以使用多少 processor 進行 Parsing                                                            |
| `BB_NUMBER_THREADS`       | Bitbake 可以==同時==執行多少工作                                                                         |
| `BB_PRESSURE_MAX_CPU`     | 監控 CPU 的壓力 (/proc/pressure)，介於 1.0 ~ 1000000 之間<br>如果高於本變數，Bitbake 不會建立新的工作<br>未設定本變數，就不會監控    |
| `BB_PRESSURE_MAX_IO`      | 監控 I/O 的壓力 (/proc/pressure)，介於 1.0 ~ 1000000 之間<br>如果高於本變數，Bitbake 不會建立新的工作<br>未設定本變數，就不會監控    |
| `BB_PRESSURE_MAX_MEMORY`  | 監控 Memory 的壓力 (/proc/pressure)，介於 1.0 ~ 1000000 之間<br>如果高於本變數，Bitbake 不會建立新的工作<br>未設定本變數，就不會監控 |
| `BB_TASK_IONICE_LEVEL`    | 用來調整 Bitbake I/O 的優先等級                                                                         |
| `BB_TASK_NICE_LEVEL`      | 用來調整 Bitbake 工作的優先等級                                                                           |
| `CACHE`                   | Bitbake 解析完 Metadata 後，將結果儲存的目錄<br>Bitbake 不需要每次都重新解析 Metadata                                 |
| `PERSISTENT_DIR`          | Bitbake 在編譯期間，所建立的非暫存檔案所在的目錄<br>如果要能多個專案共享以編譯好的檔案，可以將此變數設定為同一個                                 |

# Layers 相關

變數名稱使用 `LAYER` 開頭的，用來說明 Bitbake 如何處理 layers；而變數使用 `BB` 開頭，用來說明 Bitbake 如何處理 layers 與檔案。這一類的變數大多存在於 layer.conf 與 bblayers.conf 檔案

## layer.conf 檔案

下為 meta-aspeed/conf/layer.conf 檔案的內容
![[{FD780ED2-A727-4D82-9B11-0AE43CBD4141}.png]]

Layer.conf 檔案中，除了說明路徑的變數之外，還有以下用來說明本 layer 的資訊。這些變數大多會使用 `${VARIABLE}_${BBFILE_COLLECTIONS}` 的形式，用來說明 layer `${BBFILE_COLLECTIONS}` 的相關資訊

| 變數名稱                 | 說明                                                                                                                                              |
| :------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------- |
| `BBFILE_COLLECTIONS` | Layer 的名稱，提供給以下 `BBFILE_`開頭變數使用的 key word                                                                                                       |
| `BBFILE_PATTERN`     | 對應本 Layer 的 regular expression                                                                                                                  |
| `BBFILE_PRIORITY`    | 當 layer 中有相同 package 的 Recipe 檔案時，在Recipe 檔案中使用本變數決定哪一個 Recipe 檔案應該被使用<br>即便各 Recipe 有自己的版本 (`PV`)，使用本變數可以微調該被使用的 Recipe 檔案<br>數字愈高，越容易被選中      |
| `BBFILES`            | 所有 Recipe 與 Recipe Append 檔案所在的路徑                                                                                                               |
| `BBFILES_DYNAMIC`    |                                                                                                                                                 |
| `BBPATH`             | Bitbake 是使用到的路徑，即變數 `LAYERDIR` 的總和                                                                                                              |
| `LAYERDEPENDS`       | 用來指定相依 Recipe 檔案 (Package，或是變數 `PN`) 所在的目錄清單，空格作為分隔符號。譬如："LAYERDEPS\_${PN}" = "XXX YYY"<br>可以搭配版本，譬如："LAYERDEPENDS\_\${PN}:\${VERSION}" = "XXX" |
| `LAYERDIR`           | conf/layer.conf 所在的父目錄清單，只用來解析 conf/layer.conf 檔案                                                                                               |
| `LAYERDIR_RE`        | 只用在 conf/layer.conf 檔案中，用來表示當前 layer                                                                                                            |
| `LAYERSERIES_COMPAT` | 只用在 conf/layer.conf 檔案中<br>用來說明該 Layer 相容於 OpenEmbedded-Core 的某個版本清單                                                                            |
| `LAYERVERSION`       | 在 layer.conf 檔案中定義相關 Layer 的版本<br>用來與變數 `LAYERDEPENDS` 中指定的版本進行比較                                                                               |
| `LICENSE`            | Recipe 檔案中同名變數                                                                                                                                  |

# Package 相關 (Recipe 檔案)

Bitbake 會將 Recipe 檔案視為一個 Package，提供多個變數來記錄 Package 的屬性：

| 變數名稱 | 完整名稱                | 說明                                                        |
| :--- | :------------------ | :-------------------------------------------------------- |
| `PE` | Package Epoch       | Recipe 檔案的里程碑 <br>本變數基本上不會用到，只有不能與舊版相容時，才會設定              |
| `PF` | FULL Package Name   | Recipe 的完整名稱，包含版本與 Revision 號碼 <br>應該是 `PN`-`PV`-`PR` 的結合 |
| `PN` | Package Name        | Recipe 的名稱                                                |
| `PR` | Revision of Package | Recipe 的 Revision 號碼                                      |
| `PV` | Package Version     | Recipe 的版本號碼                                              |

在 Recipe 檔案或是 Class 檔案中使用變數時，多半會加上變數 `PN` 用來說明該變數是針對哪一個 Recipe 的。其用法如下：

- `${VARIABLE}`==:==`${PN}` - 使用條件式擴展
- `${VARIABLE}_${PN}` - 使用 BBCLASSEXTEND

## 別名 (PROVIDES)

PN 是每個 Package 預設的對外名稱。藉由變數 `PROVIDES` 與 `RPROVIDES`，讓 Package 可以提供額外的別名。

變數 `PROVIDES` 在 Bitbake 解析時，提供別名滿足其他 Package 編譯時的相依性。除了滿足相依性之外，變數 `PROVIDES` 亦提供了虛擬目標 (virtual target) 的選項。

變數 `RPROVIDES` 在建立檔案時，會依清單建立 link 檔案供其他執行檔使用。

> [!IMPORTANT]
> 虛擬目標在 OpenEmbedded-Core 架構中，扮演很重要的角色。為了滿足能在多種不同的機器上運行 Linux，Linux 的 Kernel 依照硬體設定被劃分為許多不同的 Package；某些服務也不只有一種套件，譬如說 http server 就有多種不同的套件。
> 
> 對於這種有多種方案的需求，OpenEmbedded-Core 建立了所謂的虛擬目標機制。各種方案的提供者，在各自的 Recipe 中加上以下宣告，表示自己提供了虛擬目標
> 
> > PROVIDES = "virtual/${TARGET}"
>
> 而需要這些虛擬目標的 Package，也會在自已的 Recipe 中加上以下指示，表示偏好使用哪一個服務等
> 
> > PREFERRED_PROVIDER_virtual/\${TARGET} ?= "${X}"

> [!NOTE]
> 與別名相關的另一個變數 `ASSUME_PROVIDED` 用來列出相依但是不需要重新編譯的 Recipe 檔案。可以透過本參數，減少重新編譯其他 Recipe 檔案的機會

## 版本

在 OpenEmbedded-Core 的架構下，同一個 Package 可以有多個不同版本的 Recipe 檔案。因此在編譯時，需要透過變數告知 Bitbake 應該使用哪一個版本。目前使用的變數有二：

- `PREFERRED_VERSION`
- `REQUIRED_VERSION`

可以使用 `%` 作為萬用字符，作為指多個版本。語法如下

> PREFERRED_VERSION_${CLASS} = "\${VERSION}%"
> REQUIRED_VERSION_${CLASS} = "\${VERSION}%"

範例如下
```Bitbake
# 建議使用的 linux-yocto class 的版本為 4.12*
PREFERRED_VERSION_linux-yocto = "4.12%"

# 必須使用 python 2.7.3 版
RQUIRED_VERSION_python = "2.7.3"
```

因為 `REQUIRED_VERSION` 的比 `PREFERRED_VERSION` 更加嚴謹，當兩者的值不同時，以 `REQUIRED_VERSION` 為主。當指定版本對應的檔案不存在時，使用 `REQUIRED_VERSION` 變數的 Recipe 會報錯誤；而使用 `PREFERRED_VERSION` 變數只會出現警告。

# 下載相關

Bitbake 除了在下載工作之外，都自動設定為不可下載。可以透過變數 `BB_NO_NETWORK` 針對 Fetcher，設定不從遠端下載檔案。
## 尋找順序

Bitbake 在尋找檔案時，優先會尋找本地端下載的位置。如果沒有找到，先依照以下順序，試著進行下載

- 本地端的下載位置 (變數 `CACHE`、`DL_DIR`)
- 變數 `PREMIRRORS` 指定的位置 (變數 `PREMIRRORS`) 
- Upstream source (變數 `SRC_URI`)
- 變數 `MIRROR` 指定的位置 (變數 `MIRROR`)

## 下載限制

- 變數 `BB_NO_NETWORK` 強迫限制 Fetcher 使用本地端檔案
- 變數 `BB_FETCH_PREMIRRORONLY` 強迫限制 Fetcher 只從變數 `PREMIRRORS` 指定位置下載
- 變數 `BB_ALLOWED_NETWORKS` 限制了 Fetcher 能下載的 Server 清單

> [!NOTE]
> 變數 `BB_ALLOWED_NETWORKS` 使用空格作為分隔符號，列出可下載檔案的 的 Server；本變數唯有在變數 `BB_NO_NETWORK` 沒有設定，或是設定為 "0" 才有效
> 
> 可以使用 '\*' 作為 wildcard 字元，但只限於 Server 名稱的起始處；如 '\*.gnu.org' 等

> [IMPORTANT]
> 開發環境有時候會對網路進行限制。針對此項，可以建立一個特殊的 Server，定期從 Upstream 下載檔案；而這個 Server，就可以設定進變數 `PREMIRROR` 中。再把專案的 local.conf 設定中加入變數 `BB_FETCH_PREMIRRORONLY`，即可限制開發人員從該 Server 中下載檔案

# 相依性

在 Bitbake 中用來描述相依性的變數有 3 個

- `DEPENDS` - 用來說明編譯期間的相依性
- `PACKAGES_DYNAMIC` - Recipe 中堆外宣稱會提供的 Package，供其他 Package 在執行期間使用
- `RDEPENDS` - 用來說明執行期間的相依性
- `RRECOMMENDS` - 類似變數 `DEPENDS`，但是並非一定要有

對於相依性，可以透過運算符 (Operator) "="、"<"、">"、"<="、與 ">=" 進一步說明版本號碼。每個 Recipe 會說明自己的相依性，語法如下

> RDEPENDS:\${PN} = "\${PACKAGE} (\${OPERATOR} \${Version})"
``` Bitbake
# 來自 nghttp2_1.63.0.bb
# nghttp2 執行時需要 nghttp2-proxy，且版本必須高於 1.63.0 (nghttp2 目前版本)
RDEPENDS:${PN} = "${PN}-proxy (>= ${PV})"    
```

## 編譯期的相依性

使用變數 `DEPENDS` 列出 Recipe A 在編譯期間，相依於其他 Recipe 的清單。可以在 Recipe A 各工作變數 `do_${TASK}` 的 Flag `deptask` 列舉工作清單，而 Bitbake 會先完成變數 `DEPENDS` 中所有 Recipe 檔案中，所有列在 Flag `deptask` 的工作都完成後，再進行 Recipe A 的 ${TASK}

## 執行期的相依性

Recipe 建立的程式可以被包裝成多個 package，而進行包裝時工作就跟編譯期一樣；可以透過變數 `RDEPENDS` 變數列出相依的 Recipe 清單，並於 Flag `rdeptask` 中列舉工作。

## Inter-Task 相依性

變數 Flag `depends` 與 `rdepends` 也可以達成類似編譯期與執行期相依性。

與變數 `deptask` 不同，變數 `depends` 內的清單必須包含 Recipe 名稱與工作名稱，且不會參考變數 `DEPENDS` 所列的 Recipe 清單。

## Recursive 相依性

變數 Flag `recrdeptask` 指定所有 Recipe 都必須完成該 Flag 所列的工作之後，才會執行 Recipe A 的工作。