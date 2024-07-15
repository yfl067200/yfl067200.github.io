http://thbecker.net/articles/rvalue_references/section_01.html

R-Value 是從 C++ 11 開始加入 C++ 標準函式庫。這說明這個功能之前，我們先解釋在 C 語言中定義的 L-Value 與 R-Value﹔

>
L-Value 是一個表達式 (expression, e)，可置於等號 (=) 左右側；而 R-Value 則是一個只能置於等號右側的表達式。
>



這個功能主要是用於解決兩個問題：

1. move 語法，與
2. 完美的 forwarding 功能


