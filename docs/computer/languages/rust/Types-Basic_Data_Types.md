## 基本資料型態 (Data Types)

Rust 支援的資料型態可能分成單一型態 (Scalar) 與複合型態 (Compound) 兩種，其特色在於資料的長度固定，且可輕易的塞進 Stack。

### 單一型態 

Rust 支援 4 種單一型態，包含 integer (整數)、Float-Point Number (浮點數)、Booleans (布林值)、與 Character (字元)。

#### Integer

整數是一種不包含小數的數值型態。Rust 的整數型態可以依據數值大小與是否需要包含正負符號 (signed)，有多種型態可供選擇。其中架構選項會依據處理器支援 32 位元或 64 位元，預設為 32-bit 或是 64-bit。進階的工程師可以依照資料大小選擇適合的整數型態，不確定的就選擇架構選項

| 長度 | 包含負值 (signed) | 不包含負值 (unisgned) | 
|:-----|:------------------|:----------------------|
| 8-bit | i8 (-128 ~ 127) | u8 (max 155) |
| 16-bit | i16 (-32_768 ~ 32_767) | u16 (max 65_535) |
| 32-bit | i32 (-2_147_483_648 ~ 2_147_483_647) | u32 (max 4_294_967_296) |
| 64-bit | i64 | u64 |
| 128-bit | i128 | u128 |
| 架構 | isize | usize |

Rust 與其他語言對待 Integer 不同之處，在於 Rust 本身支援數值描述符號；包含使用底線 (_) 分隔數字，方便閱讀。也包含了加上特殊字首符號，標示進位法。

| 進位法 | 字首符號 | 範例 |
|:-------|:---------|:-----|
| 2 進位 | 0b | 0b1111_0000 |
| 8 進位 | 0o | 0o77 |
| Byte (u8 only) | b | b'A' | 
| 10 進位 | | 98_222 |
| 16 | 0x | 0xff | 

##### 溢位問題 (Overflow)

當整數型態的資料比整數變數可接受的數值大的情況，就會發生溢位的問題。舉一個例子，當型態為 u8 的整數變數 (最大值為 255) 被指派一個 256 的值。Rust 針對編譯的方式，對溢位問題有不同的處理方式。

| 編譯方式 | 處理方式 |
|:---------|:---------|
| 除錯 | 在 runtime 觸發 Panic 處理 |
| 正式 | 在 runtime 時忽略溢位的資料，並不會觸發 Panic 處理 |

處理溢位問題，官方建議是採用以下方式：

- 使用 wrapping_* 函式，忽略溢位的值
- 透過 check_* 函式，當發生溢位時回傳 None
- 使用 overflowing_* 函式，取得最後的數值與是否溢位
- 使用 saturating_* 函式，設定變數的上下限


#### Float-Point Number

Rust 支援 2 種浮點數型態資料 (f32 與 f64)，都只支援 10 進位與正負數 (signed)。浮點數型態依據 IEEE-754 規範，f32 為單精度浮點數 (single-precision float)，而 f64 為雙精度浮點數 (double-precision float)

#### Booleans

布林值在 Rust 中的資料型態為 bool，只接受兩個值： true 或是 false

#### Character

字元型態在 Rust 中為 char，使用單引號 (') 指派單一字元；如果使用雙引號 (")，則指派字串 (string)。

與其他程式語言不一樣的地方在於，Rust 使用 4 個 bytes 儲存字元資料，可以處理 Unicode 字元。


### 複合型態 

複合型態就是將多種型態包裝在一起，透過同一個變數存取各個欄位或是記錄；Rust 支援的複合型態包含 2 種： Tuple (元) 與 Array (陣列)

#### Tuple

在 Rust 中宣告 tuple 的方式是使用小括號，並使用逗號 (,) 分隔各欄位。Tuple 型態在建立之後會固定長度，無法新增或是移除內建的型態；但是透過 mut 關鍵字，我們可以修改各欄位的資料。

存取 tuple 內的欄位，有兩種方式。我們以下列的範例說明：

- 使用小數點加上 index
    - Index 從 0 開始
	- 可存取單一欄位
- 使用 destructure
    - 存取所有欄位
        - 使用 destructure 的方式，必須指派相同數量的變數

``` rust
let mut color: (u8, u8, u8) = (32, 64, 128);

println!( "color = {}, {}, {}", color.0, color.1, color.2 );     //    Accessing Fields with . + index

color.2 = 255;
println!( "Point = {}, {}, {}", color.0, color.1, color.2 );

let (x, y, z) = color;                                           //    Accessing ALL Fields with Destructure
println!( "x = {}, y = {}, z = {}", x, y, z );

println!( "color = {}, {}, {}", color.0, color.1, color.2 );
```

一個 Tuple 型態的變數，如果沒有任何的值，被稱為單元 (unit)。

### Array

Tuple 是將多個資料型態包裝成一個資料型態，而 Array 則是將多個相同資料型態的變數包裝成一個。與 Tuple 一樣，一旦建立之後其資料大小就固定：無法新增、或是移除紀錄。但是加上 mut 關鍵字，可以修改紀錄中資料。

在 rust 中宣告 array 的方式是使用中括號 ([])，並使用逗號 (,) 分隔各筆資料。宣告 array 的方式與其他變數有些不同，array 可以在宣告時指定內涵幾筆資料，在形態後面加上 '; ${len}' 即可。另外再指派資料時，亦可使用 '; ${num}' 的方式，建立 ${num} 個相同數值的紀錄。

``` rust
//    declaring an arry for ${type} [with ${len} elements]
let [mut] ${name}: [ ${type}[; ${len}] ];

//    implementing an array contained ${num} records with same value ${value}
let [mut] ${name} = [ ${value}[, ${value}][; ${num}] ]
```

要存取 array 內部的資料，可以透過 [] 加上 index 的方式; index 也是從 0 開始
``` rust
let mut a: [u8; 3] = [ 1, 2, 3 ];

println!( "array a = {}, {}, {}", a[0], a[1], a[2] );

a[0] = 123;
println!( "array a = {}, {}, {}", a[0], a[1], a[2] );

//    creatinr an array with same value
let b = [ 6; 5 ];
println!( "array b = {}, {}, {}, {}, {}", b[0], b[1], b[2], b[3], b[4] );
```

##### 非法存取

rust 不像 c 可以存取 out of bounds 的資料。如果你存取的紀錄不存在 array 中，將會觸發 panic 處理機制


