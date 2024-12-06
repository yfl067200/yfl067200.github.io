Bitbake 在解析 Metadata 時，有他自己定義的規則。以下將說明 Bitbake 如何處理變數

# 變數展開 (Variable Expansion)

基本的語法如下。當變數的值是使用 ${X} 時，Bitbake 會將 ${X} 改變成另一個變數 X 的值，這個動作被稱為變數展開。基本上 Bitbake 讀取到變數展開的描述時，並不會馬上進行變數展開的動作，而是==延遲==到該變數被使用時才進行。而變數每被使用一次，都會在進行變數展開的動作。

> [!Note]
> 變數展開的運算符為 `${X}`，其中 `X` 為需要進行變數展開的變數或是 Python 程式 。
> 
> 當需要變數展開的變數只使用 `$` 字符，而缺乏 `{}` 字符時，該變數的展開將透過 shell，而非 Bitbake

> [!IMPORTANT]
> Metadata 使用字串作為變數值的**唯一型態**。因此在處理變數的時候，會有許多字串處理的動作。其中包含了以下 3 點特性：
> 1. 使用空格作為分隔符號。藉由空格，一個變數可以存入多筆資料
> 2. 使用 "\\" 進行行的結合。如同 shell 一般，當資料過長時，可以將資料變得比較易讀
> 3. 使用字符 "${}" 表示需要進行資料展開

## 立刻進行變數展開

在 Bitbake 實際使用到變數之前，變數展開的動作並不會發生；==因為每次使用變數時，都會進行變數展開，所以變數的值會不一樣==。如果要避免差異，可以使用 ":=" 或是 "??=" 語法取代 "=" 或是 "?="，立刻進行變數展開的動作。
``` python
X = "123"
#    一般變數擴展
VAR1 = "${X}"           # 尚未進行變數擴展，VAR1 的值預計是 "123"。
VAR2 := "${X}"          # 已進行變數擴展，VAR2 的值為 "123"

#    唯有變數未賦值，才會進行變數擴展
VAR3 ?= "${X}"          # 尚未進行變數擴展，假設 VAR3 並未賦值，則 VAR3 預計是 "123"
VAR4 ??= "${X}"         # 假設 VAR4 尚未賦值，進行變數擴展且其值為 "123"

X = "456"               # VAR1 與 VAR3 的值已隨著變數 X，被改變成 "456"
```

> [!NOTE]
> 立刻進行變數展開的動作，導致變數已經賦值。所以之後使用變數時，就不需要再次進行變數展開的動作。
## 內部 Python 解析

使用 `${X}` 的方式進行變數展開，X 除了是另一個變數之外，還可以是一個 Python 敘述句。當呼叫 Python 來執行 Python 敘述句已進行變數展開，就被稱為內部 Python 解析 (Inline Python Variable Expansion)。

語法如下，透過 `@`字符讓 Python 執行

> {VAR} = ${@X()}                          # 透過 Python 執行 `X()` 命令
> {VAR} = ${@'0' if True else '1'}    # 透過 Python 執行 `'0' if True else '1'` 命令

## 變數值調整

因為變數的值為字串型態，所以 Bitbake 定義了多種調整字串的語法。另外因為空格作為分隔符號，故使用調整語法時需要注意是否有自動加上空白格。

透過運算元 "+="、".="、"=+"、與 "=." 進行變數值的調整，會立刻進行變數展開的動作
``` python
#    擴充在後
{VAR} += ${VALUE}          # 在變數 VAR 的值之後，加上 VALUE。中間有空格分隔
{VAR} .= ${VALUE}          # 在變數 VAR 的值之後，加上 VALUE。中間無空格分隔
{VAR}:append = ${VALUE}    # 覆寫式行為運算元之一，結果同 ".="

#    擴充在前
{VAR} =+ ${VALUE}          # 在變數 VAR 的值之前，加上 VALUE。中間有空格分隔
{VAR} =. ${VALUE}          # 在變數 VAR 的值之前，加上 VALUE。中間無空格分隔
{VAR}:prepend = ${VALUE}   # 覆寫式行為運算元之一，結果同 "=."

#    移除變數 VAR 中，所有完全符合 VALUE 的值
{VAR}:remove = ${VALUE}    # 覆寫式行為運算元之一

#    移除變數 VAR
unset {VAR}
```

> [!NOTE]
> 只要是使用變數展開的語法，不論是不是使用內部 Python 解析；一旦該變數被使用，就會進行變數展開的動作。除非是使用立刻進行變數展開的運算元，包含 ":="、"??="、"+="、".="、"=+"、"=." 等
 
### 覆寫式行為 (Override Style Operation)

進行變數調整時，有兩者不同的執行順序。使用 "+="、".="、"=+"、或是 "=." 等運算符，是依照程式碼的順序，在被呼叫時立刻進行變數展開。

而另一種使用 ":append"、":preappend"、與 ":remove" 則是變數被使用到時，才會進行變數展開的動作；且進行變數展開時，依照 ":append" > ":prepend" > ":remove" 的順序執行。這一類的運算元稱為覆寫式行為運算元。

> [!IMPORTANT]
> 因為可以透過加入新的 Layer 擴充 OpenEmbbed-Core 的架構。為了建立正確的執行順序，底層的 Metadata 會使用覆寫式行為運算元，讓上層先對變數進行賦值。

> [!IMPORTANT]
> 以下範例可以看出這種加上覆寫式行為之後的變數值有多難預測
> ``` python
> #    範例一
> A = "1"           # 變數 A 的值為 1
> A:append = "2"    # 使用覆寫式行為運算元，變數 A 的值延遲展開
> A:append = "3"    # 使用覆寫式行為運算元，變數 A 的值延遲展開
> A += "4"          # 變數 A 的擴充為 "1 4"
> A .= "5"          # 變數 A 的值擴充為 "1 45"
>                   # 變數 A 的值因為覆寫式行為，進行變數展開而成為 "1 4523"
>
> #    範例二
> NONEED = "123 456 789"
> # 因為變數 FOO 尚未被使用，remove 這個動作的參數並未進行變數展開
> FOO:remove( "${NONEED}" )
> # 最後 NONEED 變數的值改變了，變數 FOO 進行 remove 參數也跟著變了
> NONEED = "123 789"         
> ```

## 變數的 Flag (Variable Flag)

Bitbake 可以替變數加上 Flag，使用的方式是透過 `${VAR}[${FLAG}]` 運算符存
``` python
# 
FOO[a] = "abc"    # 變數 FOO 的 Flag "a" 的值是 "abc"
FOO[b] = "123"    # 變數 FOO 的 Flag "b" 的值是 "123"
FOO[a] += "456"   # 變數 FOO 的 Flag "a" 的值是 "abc 456"


```

> [!NOTE]
> 對於 flag 而言，之前變數擴展語法都可使用；只有覆蓋式行為運算符 ":append"、":prepend"、與 ":remove" 不能使用
> 
> 另一個需要注意的事項是，Flag 的名稱最好不要用 "\_" 開頭。原因是 Bitbake 會透過 `d.getVarFlags( "VAR" )` 的方式取得資料倉儲 "d" 的 Flag，但是這個方式會忽略 "\_"。

## 追蹤變數值的變化

因為導入的變數展開與覆寫式行為運算元，導致使用變數時，他的值很難掌握。為了方便追蹤變數值的變化，可以使用命令列出變數值被改變的時間點。

> bitbake -e

# 條件式擴展

Bitbake 使用 `OVERRIDES` 這一個變數儲存條件。當變數的後贅詞符合任一條件時，則進行變數擴展。`OVERRIDES` 使用字串，且用分號 (":") 作為分隔符號，可以包含多個條件字串；而每一個條件字串只能包含==小寫字元==、數字、與分號。
> [!NOTE]
> 在舊版的 Bitbake (version 1.52)，OVERRIDES 使用底線 "\_" 作為分隔符號，而非分號 ":"。

在 OpenEmbedded-Core 中，常常會使用==變數:${CONDITION}== 的方式給予變數在不同條件下的值；以達到因時制宜。以下面為例。正常情況下 KBRANCH 的值就會是 "standard/base"；但是如果我們在客製化的專案加上 `OVERRIDES = "qemuarm"`，KBRANCH 的值就會改變為 "standard/arm-versatile-926ejs"
``` Python
KBRANCH = "standard/base"
KBRANCH:qemuarm = "standard/arm-versatile-926ejs"
KBRANCH:qemumips = "standard/mti-malta32"
KBRANCH:qemuppc = "standard/qemuppc"
KBRANCH:qemux86 = "standard/common-pc/base"
KBRANCH:qemux86-64 = "standard/commn-pc-64/base"
KBRANCH:qemumips64 = "standard/mti-malta64"
```

## Key 賦值

條件式擴展讓人困擾的地方在於，他需要進行所謂的 "Key 賦值"；而 "Key 賦值" 發生在所有 Metadata 都解析之後，這導致變數的值更加難以預料。我們以下面的例子進行說明
``` python
A${B} = "X"
B = 2
A2 = "Y"        # 變數 A2 的值為 "Y"
                # 進行 Key 賦值之後，變數 A${B} 變成變數 A2。
                # 原本的變數 A2 被取代，最後 A2 的值為 "X"
```

### 調整變數值

解釋完 "Key 賦值" 之後，就可以說明條件式擴展搭配調整變數值運算元的複雜程度。
``` python
OVERRIDE = "foo"
A = "X"
A:foo:append = "Z"      # 本敘述句應該是 (A):(foo):(append) = "Z"
                        # 先透過變數擴展，敘述句成為 (A):(foo) = "Z"
                        # 再經過條件式擴展，敘述句成為 (A) = "Z"
                        # 原本的變數 A 被取代，最後變數 A 的值為 "Z"
B = "Y"
B:append:foo = "Z"      # 本敘述句應該是 (B):(append):(foo) = "Z"
                        # 進行條件式擴展，敘述句成為 (B):(append) = "Z"
                        # 再經過變數擴展，變數 B 的值為 "YZ"
```

# 資料倉儲 (datastore) 

之前已經提到所謂的資料倉儲 (datastore)，他是 Bitbake 用來存放變數的地方。這個資料倉儲是個全域變數，名稱是 "d"。

可以透過以下的函式從存取儲存在資料倉儲中變數與資料倉儲本身的 Flag：
``` python
# 存取變數 "${VAR" 的值
Value = d.getVar( "${VAR}" )
d.setVar( "${VAR}", "${VALUE}" )

# 存取變數 "${VAR" 的 Flag
Value = d.getVarFlags( "${VAR}" )
self.d.setVarFlags( "${VARFLAGS}", { "${KEY}": "${VALUE}" } )
```

## 存取資料倉儲

Bitbake 的全域變數 "d" 是資料倉儲，必須知道如何存取 "d" 內部的變數與變數的 flag。

| 函式                                                  | 說明                                                                                      |
| :-------------------------------------------------- | :-------------------------------------------------------------------------------------- |
| `d.getVar( "${VAR}", expand )`                      | 取得變數 "\${VAR}" 的值，如果該變數不存在，傳回 "None" <br>第二個參數 `expand` 表示是否需要進行變數展開的動作；例 `expand=True` |
| `d.setVar( "${VAR}", "${VALUE}" )`                  | 設定變數 "\${VAR}" 的值為 "\${VALUE}"                                                          |
| `d.appendVar( "${VAR}", "${VALUE}" )`               | 擴充變數 "\${VAR}"，將 "\${VALUE}" 加入最後面                                                      |
| `d.prependVar( "${VAR}", "${VALUE}" )`              | 擴充變數 "\${VAR}"，將 "\${VALUE}" 加入最前面                                                      |
| `d.delVar( "${VAR}" )`                              | 移除變數 "\${VAR}"                                                                          |
| `d.renameVar( "${VAR}", "${VAR2}" )`                | 變數 "\${VAR}" 改名為 "\${VAR2}"                                                             |
| `d.getVarFlag( "${VAR}", ${FLAG}, expand )`         | 取得變數 "\${VAR}" 中名為 "\${FLAG}" Flag 的值                                                   |
| `d.getVarFlag( "${VAR}" )`                          | 取得變數 "\${VAR}" 所有 Flag 的值<br>回傳的型態為 flagsdict (Python Dictionary)                       |
| `d.setVarFlag( "${VAR}", ${FLAG}, "${VALUE}" )`     | 設定變數 "\${VAR}" Flag "\${FLAG}" 的值為 "\{VALUE}"                                           |
| `d.setVarFlag( "${VAR}", flagsdict )`               | 等同 d.addpendVarFlag()                                                                   |
| `d.appendVarFlag( "${VAR}", ${FLAG}, "${VALUE}" )`  | 擴充變數 "\${VAR}" Flag "\${FLAG}" 的值，將 "\${VALUE}" 加入最後面                                   |
| `d.prependVarFlag( "${VAR}", ${FLAG}, "${VALUE}" )` | 擴充變數 "\${VAR}" Flag "\${FLAG}" 的值，將 "\${VALUE}" 加入最前面                                   |
| `d.delVarFlag( "${VAR}", ${FLAG} )`                 | 移除變數 "\${VAR}" Flag "\${FLAG}"                                                          |
| `d.delVarFlag( "${VAR}" )`                          | 移除變數 "\${VAR}" 所有 Flag                                                                  |
| `d.expand( ${expression} )`                         |                                                                                         |


> [!Note]
> 存取 "d" 儲存變數的 Flag 有兩種 API。一是該變數指定某一 Flag，像是 `d.getVarFlag( "${VAR}", ${FLAG}, expand )`；另一種是針對該變數所有的 Flags。
> 
> 後者透過資料型態為 flagsdict，就是 Python Dictionary 型態。


