本章說明 D-Bus 的 **Object Model** 與 **Signature**，用來說明 D-Bus 

# Object Model

D-Bus 的架構採用類似物件導向的設計。每個物件 (`Object`) 有開放的屬性 (`Property`)與函式 (`Method`)，而屬性與函式將依照功能集中於不同的介面 (`Interface`)下方便管理。

各程式將向 D-Bus 註冊一個 Bus 代表自己，並在其 Bus 下建立 Object 表示分享的物件，最後在 Object 下建立不同的 Interface 提供該物件分享的功能。最後在 Interface 下建立 Property、Method、與 Signal 等，作為開放給其他程式 (稱為 Client) 使用各功能開放的介面。

| 名詞          | 說明                                                                                                                                      |
| :---------- | :-------------------------------------------------------------------------------------------------------------------------------------- |
| `Bus`       | 獨立的 IPC daemon，分為 system bus 與 user bus 兩種<br>每個程序跟 D-Bus 註冊一個代表自己的 Bus，且註冊的 Bus 不可重複                                                   |
| `Object`    | 又稱 `Object Path`，用來表是程序下分享的物件 <br>不限制每個程序只能註冊一個 `Object`                                                                                |
| `Interface` | 每個程序開放的功能，將依附於 `Object` 下 <br>每一個 `Interface` 代表一個開放的功能                                                                                 |
| `Property`  | 為 `Interface` 開放的變數，可供 Clients 修改或是讀取該變數的值                                                                                              |
| `Method`    | 即 `Interface` 開放的函式                                                                                                                     |
| `Signal`    | 不像 `Property` 與 `Method` 之於物件，`Signal` 是一個一對多的通知<br>多個 Clients 可以向一個 `Interface` 註冊關心的 `Signal`。當 `Interface` 符合某些條件，將會發送訊息給註冊的 Clients |

# Signature

D-Bus 最終是透過 `Interface` 下的 `Property`、`Method`、與 `Signal` 三種介面，達成程序間互動。與 `Property` 或是 `Method` 互動的前提是，我們必須知道 `Property` 的型態；或是 `Method` 接收的參數型態與數量，及其返回值的型態。D-Bus 透過 **Signature** 來說明這些資訊

`Signature` 基本上就是一個字串，使用不同的符號代表不同的形態；型態包含單一型態與 Container 型態，其中 Container 型態包含 Variant、Array、Struct、與 Dictionary

| 字符        | 說明                                     |
| :-------- | :------------------------------------- |
| `y`       | 8-bit unsigned integer                 |
| `n`       | 16-bit signed integer                  |
| `q`       | 16-bit unsigned integer                |
| `i`       | 32-bit signed integer                  |
| `u`       | 32-bit unsigned integer                |
| `x`       | 64-bit signed integer                  |
| `t`       | 64-bit unsigned integer                |
| `d`       | Double-precision Float Point (IEEE754) |
| `b`       | Boolean                                |
| `s`       | UTF-8 string                           |
| `o`       | D-Bus Object Path string               |
| `g`       | D-Bus Signature string                 |
| `h`       | Unix File Descriptor                   |
| `v`       | Variant Type                           |
| `a`       | Array                                  |
| `(` 與 `)` | Struct                                 |
| `{` 與 `}` | Dictionary                             |

## Container Signature

大部分的 Container Signature 都很直覺，除了 `Variant` 與 `Dictionary` 之外。其中 `Dictionary` 只是單存的 Key->Value pair，必須搭配 Array 才能成為一般所認知的 `Dictionary` 型態。而 `Variant` 將會是一連串 `${TYPE}:${VALUE}` 的字串組合

### Examples

| `Signature` | 說明                                                               |
| :---------- | :--------------------------------------------------------------- |
| (ii)        | 包含兩個 32-bit signed integer 的 Struct                              |
| ((ii)s)     | 包含另一個 Struct 與字串的 Struct，內部的 Struct 的內容為兩個 32-bit signed integer |
| ai          | 一個陣列，其內容為 32-bit signed integer                                  |
| a(ii)       | 一個陣列，其內容為包含兩個 32-bit signed integer 的 Struct                     |
| aai         | 一個陣列，其內容為包含 32-bit signed integer 的陣列                            |
| a{ss}       | 一個其內容為一個 string -> string 的字典                                    |
| a{is}       | 一個內容為一個 32-bit signed integer -> string 的字典                      |
| a{s(ii)}    | 一個內容為一個 string --> (ii) 的字典                                      |
| a{sa{ss}}   | 一個內容為一個 string --> 另一個字典的字典，內部字典為 string->string 型態              |




