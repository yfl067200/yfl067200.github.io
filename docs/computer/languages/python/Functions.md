# 函式

Python 支援 Functional programming、Procedural programming、與 Object-oriented programming。建立函式的實作方法就是透過以下的格式，千萬別忘了冒號 (:)，那是 Python 用來建立 scope 的方式。

``` python
def ${NAME}( [${VARIABLE} [= ${DEFAULT VALUE} ] ] ):
    ${CODE}
```


## Functional programming

Functional programming 跟 Procedural programming 的差異在於 Functional programming 的每一個函式都是獨立的。雖然 Procedural programming 也是使用函式架構出一個問題的解決方案，Functional programming 的函式只會依照輸入的參數進行運作，並輸出對應的結果；也就是說，當參數不變，結果也不變。

而 Procedural programming 或是 Object-oriented programming 的函式除了輸入的參數之外，還會使用外部的變數或是 State；這導致輸入的參數是一樣的，但是可能因為其他變數的影響，導致輸出不一樣。

最後，因為 Functional programming 每個函式都自成一個世界，這表示 Functional programming 不會改變函式以外的狀態。這種獨立的好處在於，隨時都可以更改 Functional programming 的函式的實作內容。最後，以下為 Functional programming 的幾個規則：

- 函式之間不應該有 State 的順序
    - 使用 Recursion 取代 loop
- 每個函式只做一件事
- 每個函式都不會影響外部的狀態
    - 函式能改變的只有內部的變數


### Recursion

在 Python 中的 Recurion 是有限度的；如果深度超過 997，Python 會停下來，並發出 RecursionError。如果需要增加 Recursion 的深度，可以透過 sys.setrecursionlimit( N ) 函式調整。但是，當 Recusion 的深度過深，很可能導致 Stack 的資料覆蓋掉 Heap 的資料，導致系統崩潰。



## 參數的傳遞

在[變數](./02-Types.html)一節，我們有解釋 Python 的變數是儲存值所在的記憶體位置，而參數也如同變數一般。簡單的說，函式中的參數是類似 call by reference 的方式傳遞的。但是因為 python 的變數有 [immutable 與 mutable](./02-Types-Variables.html#immutable-%E8%88%87-mutable) 的差異，即便是 call by reference，也有可能導致參數與原本變數的值不同。



