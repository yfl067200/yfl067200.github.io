rust 有許多設計跟其根源 c/c++ 有所不同，其中之一在於[變數的可變性](Introduction/differences)與遮蔽；這部分在之前的章節提到過。這邊將列出跟資料結構相關的主題

# 靜態類型語言

rust 與其根源 c/c++ 一樣，都是靜態類型語言 (statically typed language)。導致於此，rust 語言在宣告、使用、或是賦值時，都會進行資料型態的檢查或是轉型等動作。

> [!Note]
> 靜態類型語言 (Statically Typed Language) 與動態類型語言 (Dynamically Typed Language) 的差異在於對型態的檢查強度：
> - 靜態語言在編譯時，會檢查個變數的型態
> - 靜態語言的變數在宣告時即已決定型態，無法在執行期改變型態
> - 有處理型態錯的機制

## 模擬兩可

因為是靜態型態語言，所以在編寫 rust 語言時需要注意資料的型態；不然會很容易出現資料型態的錯誤。其中最常見的錯誤之一是型態的模擬兩可。當編譯器發現變數的型態出現模擬兩可的情況，編譯器將給予錯誤警告。以下面的程式碼為範例：
``` rust
let i = `43`.parse().except("Not a number");
```

因為 string.parse() 能轉換的型態有太多種可能，因此編譯器將會回報型態錯誤
> Compiling no_type_annotations v0.1.0 (file:///projects/no_type_annotations) error[E0282]: type annotations needed

解決的方案有二。要不是指定變數 i 的型態，或是指定 parse() 返回的型態
``` rust
let i: u32 = "43".parse().except("Not a number");
let i = "43".parse::<u32>().except("Not a number");
```

## 純量類型 (scalar types)

所謂的純量型態 (scalar types) 表示該型態的變數值並非組合而成，每一個變數只單存的保存一個型態的值。在 rust 語言中，原生支援的標量型態有四：

### 整數 (integer)

rust 的整數型態有多種長度，不同場度的形態各異。以下為相關列表。

| Data Length | Signed | Un-Signed |
| :---------- | :----- | :-------- |
| 8 bits      | i8     | u8        |
| 16 bits     | i16    | u16       |
| 32 bits     | i32    | u32       |
| 64 bits     | i64    | u64       |
| 128 bits    | i128   | u128      |
| arch        | isize  | usize     |
其中 signed 與 unsigned 的差異在於 unsigned 型態的資料只存正整數。unsigned 整數的值區塊為 0 <= 值 <= $2^N$，其中 N 為 Data 的長度。而 signed 整數的值區塊為 $-2^(N-1)$ <= 值 <= 2^(N-1)$ 

### 浮點數 (float-point number)


### 布林 (boolean)


### 字元 (chacter)