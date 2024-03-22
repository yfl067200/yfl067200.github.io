# 函式

## 參數

在[變數](./02-Types-Variables.html)一節，我們有解釋 Python 的變數是儲存值所在的記憶體位置，而參數也如同變數一般。簡單的說，函式中的參數是類似 call by reference 的方式傳遞的。但是因為 python 的變數有 [immutable 與 mutable](./02-Types-Variables.html#immutable-%E8%88%87-mutable) 的差異，即便是 call by reference，也有可能導致參數與原本變數的值不同。



