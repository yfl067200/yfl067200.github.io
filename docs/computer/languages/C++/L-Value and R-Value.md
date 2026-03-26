http://thbecker.net/articles/rvalue_references/section_01.html

R-Value 是從 C++ 11 開始加入 C++ 標準函式庫。這說明這個功能之前，我們先解釋在 C 語言中定義的 L-Value 與 R-Value﹔

> [!IMPORTANT]
> L-Value 是一個表達式 (expression, e)，可置於等號 (=) 左右側；而 R-Value 則是一個只能置於等號右側的表達式。依照此定義，R-Value 將會是某一種臨時產生，由 C++ Runtime 管理而非使用者程式管理的資料；譬如説某個寫死的數字 43。而 L-Value 則是使用者程式，透過變數管理執行時所產生的資料。
> 
> C++ 亦有一種被稱為 Reference，他其實就是存放 L-Value 資料所在的記憶體位置。因為 Reference 依賴於 L-Value 的存在，故 Reference 在宣告時就必須指定一個已存在的 L-Value，且無法更改。

這個功能主要是用於解決兩個問題：

1. move 語法，與
2. 完美的 forwarding 功能


