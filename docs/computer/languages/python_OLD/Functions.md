# 函式

建立函式的實作方法就是透過以下的格式，千萬別忘了==冒號 (:)==，那是 Python 用來建立 scope 的方式。

``` python
def ${NAME}( [${VARIABLE} [= ${DEFAULT VALUE} ] ] ):
    ${CODE}
```

## Scope

函式自身是一個獨立的 scope，即便是子函式亦無法修改父函式的變數。以下程式碼為例，結果是兩個函式彼此擁有各自的同名的變數 

``` Python
def Outter():
	test = 1
	def Inner():
		test = 2
		print(f'Inner test = {test}')

	Inner()
	print(f'Outter test = {test}')

Outter()
```

> 執行結果為
> Inner test = 2
> Outter test = 1

###  nonlocal 與 global 宣告

要讓不同 scope 之間共用同一個變數，在 C/C++ 可以使用指針作為函式的參數，實踐 `call by reference` 的機制；這樣其他的函式也可以修改共用變數的值。但是，Python 並沒有指針，因此 Python 提供兩種方式：`nonlocal` 與 `global` 宣告：其中 `nolocal` 宣告用於讓子函式能夠存取父函式的變數，而 `global` 宣告讓函式可以存取全域變數。

``` Python
def Outter():
	test = 1
	def Inner():
		nonlocal test  #    宣布變數 test 並非本 scope 的變數，Python 會使用父函式變數
		test = 2
		print(f'Inner test = {test}')

	Inner()
	print(f'Outter test = {test}')

Outter()
```

> 執行結果為
> Inner test = 2
> Outter test = 2

``` Python
def Inner():
        global test  #  這邊改用 nonlocal 會出現 SyntaxError: invalid syntax 錯誤
        test = 3
        print(f'Inner test = {test}')

def Outter():
        test = 2
        Inner()
        print(f'Outter test = {test}')

test = 1
Outter()
print(f'Global test = {test}')
```

> 執行結果為
> Inner test = 3
> Outter test = 2
> Global test = 3

### 參數傳遞

除了 global 與 nonlocal 之外，要實踐 Call by Reference 的方式就是傳遞 mutable 變數。

#### 搭配 * 與 ** 傳遞參數

使用 `*` 與 `**` 傳遞參數，並非 Ｃ 語言一樣是傳遞位置；在 Python 中，是用來解析 `tuple` 與 `dict` 的型態。







Python 支援 Functional programming、Procedural programming、與 Object-oriented programming。
# Functional programming

Functional programming 跟 Procedural programming 的差異在於 Functional programming 的每一個函式都是獨立的。雖然 Procedural programming 也是透過使用多個函式架構出一個問題的解決方案，Functional programming 的函式只會參考輸入的參數，任何非參數的資訊都不會改變函式的輸出。

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



