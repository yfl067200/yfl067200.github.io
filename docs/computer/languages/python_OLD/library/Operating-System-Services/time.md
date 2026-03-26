##### Table of Contents  
- [Introduction](./time.html#introduction)
    - [常見字彙](./time.html#)
- [資料結構](./time.html#) 
    - [class time.struct_time](time.html#parents)
	- [seconds since the epoch](./time.html#formatter_class)
- [APIs](./time.html#) 
    - [單位轉換函式](time.html#parents)


## Introduction

time 模組提供與時間相關的 APIs，使用本模組可能會連帶使用到資料結構 [datatime]() 與 [calendar]() 等模組。

本模組雖然永遠存在，但是提供的 APIs 不一定能在平台上執行；本模組的 APIs 會呼叫 C library 上對應的 APIs，請自行參考平台上 C library 相關的文件，確保執行結果不會出錯。

- 本模組的功能完全取決於 C Library，故無法處理 epoch 之前與超過 32-bits 系統可以儲存的未來時間 (約在 2038 年)。

- 時間的精準度會有誤差。大部分的 Unix 系統上的一個 ticks 大約是 1/50 ~ 1/100 秒

    - time() 與 sleep() 的精準度會優於 Unix 系統所提供的資料。

	- time() 使用 gettimeofday()，而 sleep() 會使用 Unix select() 方式實作

- 使用 strptime() 解析 2 位元的年度時間 (%y) 會依據目前 POSIX 與 ISO C 的規範

    - 69 ~ 99 將被視為 1969 ~ 1999

	- 00 ~ 68 將被視為 2000 ~ 2068

- 時間的單位分為兩種，一是 seconds since epoch，另一種是 struct time

### 常見字彙

| 字彙 | 說明 |
|:-----|:-----|
| epoch | 電腦系統上的起始時間 <br> 可以透過 time.gmttime( 0 ) 取得，為 1970 年 1 月 1 號 00:00:00 (UTC) |
| seconds since the epoch | 某時間與 epoch 之間的秒數差異 |
| UTC | Universal Time (格林威治時間，又稱 GMT) |
| DST | Daylight Saving Time <br> 陽光節約時間的規定主要取決於當地政府，沒有甚麼共通邏輯 <br> 本模組的處理方式完全取決於 C Library 上的實作方法 |
| Monotonic 時間 | 系統上的時間無法倒退 |


## 資料結構

### class time.struct_time

可以透過 time.gmttime()、time.localtime()、與 time.strptime() 獲得。本結構為一擁有 [named tuple]() 介面的物件，可以透過 index 或是名稱取得各屬性的值。當屬性的值有問題，將觸發 TypeError 例外

| Index | 名稱 | 說明 |
|:------|:-----|:-----|
| 0 | tm_year | 4 位數年代，譬如 1999 |
| 1 | tm_mon | 月份，range[ 1, 12 ] <br> 注意，與 C library 提供的不同 (range[ 0, 11 ]) |
| 2 | tm_mday | 月日期，range [ 1, 31 ] |
| 3 | tm_hour | 小時，range [ 0, 23 ] |
| 4 | tm_min | 分，range [ 0, 59 ] |
| 5 | tm_sec | 秒，range [ 0, 61 ] |
| 6 | tm_wday | 週日期，range [ 0, 6 ]，0 是周一 |
| 7 | tm_yday | 年日期，range [ 1, 366 ] |
| 8 | tm_isdst | 是否支援夏季日光節約時間 <br> 0: 支援 <br> 1: 不支援 <br> -1:不詳 |


### seconds since the epoch

| API | 說明 |
|:----|:-----|
| time.time() | seconds since the epoch <br> float 型態 | 
| time.time_ns() | nanoseconds since the epoch <br> int 型態 | 
| time.monotonic() | seconds since the epoch <br> float 型態 <br> 使用 monotonic 時間 | 
| time.monotonic_ns() | nanoseconds since the epoch <br> int 型態 <br> 使用 monotonic 時間 | 
| time.perf_counter() | seconds since the epoch <br> float 型態 <br>  | 
| time.perf_counter_ns() | nanoseconds since the epoch <br> int 型態 <br> | 
| time.process_time() | seconds since the epoch <br> float 型態 <br> 返回本程式使用的 system 與 user CPU 時間 | 
| time.process_time_ns() | nanoseconds since the epoch <br> int 型態 <br> 返回本程式使用的 system 與 user CPU 時間 | 
| time.thread_time() | seconds since the epoch <br> float 型態 <br> 返回本 thread 使用的 system 與 user CPU 時間 <br> 僅支援 Linux、Unix (需支援 CLOCK_THREAD_CPUTIME_ID)、與 Windows 平台 | 
| time.thread_time_ns() | nanoseconds since the epoch <br> int 型態 <br> 返回本 thread 使用的 system 與 user CPU 時間 <br> 僅支援 Linux、Unix (需支援 CLOCK_THREAD_CPUTIME_ID)、與 Windows 平台 | 


## APIs


### 單位轉換函式

| API | 參數結構 | 輸出 | 說明 |
|:----|:---------|:-----|:-----|
| time.asctime( [T] ) | class time.struct_time | 字串，類似 'Sun Jun 20 23:21:05 1993' | 當參數是 None 時，使用 time.localtime() 返回的值 |
| time.ctime( [S] ) | seconds since the epoch | 字串，類似 'Sun Jun 20 23:21:05 1993' | 當參數是 None 時，使用 time.time() 返回的值 |
| time.gmttime( [S] ) | seconds since the epoch | UTC 時間的 class time.struct_time | 當參數是 None 時，使用 time.time() 返回的值 |
| time.localtime( [S] ) | seconds since the epoch | 當地時間的 class time.struct_time | 當參數是 None 時，使用 time.time() 返回的值 <br> 呼叫本函式可能會觸發 OverflowError 例外或是 OSError 例外。為避免此類錯誤，年代限制於 1970 ~ 2038 之間 |
| time.mktime( T ) | class time.struct_time | seconds since the epoch | 與 time.localtime() 是相反的函式 <br> 有可能觸發 OverflowError 例外或是 ValueError 例外 |
| time.strftime( format[, t] ) | class time.struct_time | 字串 | 當參數是 None 時，使用 time.localtime() 返回的值 <br> 可能會觸發 ValueError 例外 |
| time.strptime( string[, format ] ) | 表示時間的字串 | class time.struct_time | 預設的 format 是 "%a %b %d %H:%ML%S %Y" <br> 可能會觸發 ValueError 例外 |

以下為 strftime() 與 strptime() 支援的 format
| 參數 | 說明 | 其他 |
|:-----|:-----|:-----|
| %a | 當地週日期 | |
| %A | 當地完整週日期 | |
| %b | 當地月份 | |
| %B | 當地完整月份 | |
| %c | 當地的日期與時間 | |
| %d | 月日期， range[ 1, 31] | |
| %f | 用 10 進位表示 Microseconds | |
| %H | 24 小時制的小時，range[ 0, 23 ] | |
| %I | 12 小時制的小時，range[ 0, 11 ] | |
| %j | 年日期，range[ 1, 366 ] | |
| %m | 10 進位表示月份，range[ 1, 12 ] | |
| %M | 10 進位表示分，range[ 0, 59 ] | |
| %p | 當地時間的上午或是下午 | |
| %S | 10 進位表示秒，range[ 0, 59 ] | |
| %U | 10 進位表示年週期，range[ 0, 53 ] <br> 使用週日作為一週的第一天，week 0 是從新年的第一個週日算起 | |
| %w | 10 進位表示周日期，range[ 0, 6 ] <br> 0 為週日 | |
| %W | 10 進位表示年週期，range[ 0, 53 ] <br> 使用週一作為一週的第一天，week 0 是從新年的第一個週一算起 | |
| %x | 當地日期 | |
| %X | 當地時間 | | 
| %y | 2 位元的年，range[ 0, 99 ] | |
| %Y | 4 位元的年 | |
| %z | 與 GMT 的時差，range[ -23:59, +23:59 ] | |
| %Z | 時區的名稱 | |
| %% | 用來表是 % 字符 | |


### 系統時鐘函式





