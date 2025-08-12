# 流程控管

跟其他語言一樣，Python 的流程控管也是透過 `if ... else ...`、 `for ...` 、與 `while ...`  三種決策機制。

## if statement

基本上跟其他語言一樣，就省略說明；三種 `Statements_A`、`Statements_B`、與 `Statements_C`中只會有一種被執行，取決於哪一個 `Condition` 成立
``` Python
if condiftion_A:
    Statements_A
elif condition_B:
    Statements_B
else:
    Statements_C
```

如果判斷式只有 True 與 False 兩種，可以使用更精簡的 `if statement`
``` Python
Statements_A if condition_A else Statement_B
```

> [NOTE]
> 判斷的結果是布林型態，只有 `True` 與 `False` 兩種。對於 0、0.0、或是 None 都會被視為 False，在使用判斷式時要注意資料的型態 

## for loop

基本上的 Iterating 機制就是透過 `for loop`：比較有趣的地方在於，可以透過 `enumberate()` 獲得資料的 index，或是透過 `zip()` 同時處理多個不同的資料集。 

> [IMPORTANT]
> Python 定義的 Iterable 特性有兩種特色：
> 1.  能將資料中的資料，依序逐一列出
> 2. 任何實作出 `__iter__()` 或是 `__getitem__()` 函式的 `class`

> [NOTES]
> zip([]) 將從左至右逐一列出
> zip(\*[]) 將從上至下逐一列出

``` Python
for iterator in Range:       #    Range 可以是任何一種擁有 iterable 特性的資料 
	Statement                #    每一次進入 for 迴圈，變數 iterator 的值會依序改變

#    可以透過 enumberate() 從 Range 中取得順序 (index) 與資料 (iterator)
for index, iterator in enumberate(Range):
	Statement

#    這邊的 iterator 是 Tuple 型態，包含 index 與 Data
for iterator in enumberate(Range):
	Statement

#    可以透過 zip() 將多個 iterable 的資料集中的資料逐一取出
#    迴圈數等同於最少的資料集數量
for iterator_A, iterator_B in zip(Range_A, Range_B):
	Statement

#    這邊的 iterator 是 Tuple 型態，包含三個資料集中的資料
for iterator in zip(Range_A, Range_B, Range_C):
	Statement
```

### Class iterator

在 Python 的官方文件中有提到，可以透過 `iter()` 建立與擁有 `iterable` 特性的資料互動的物件，這個物件就是 `iterator`。`class iterator` 提供 `next()` 函式可以逐一取得資料集中的資料。資料集需實作出 `__iter__()` 與 `__next__()` 兩組 API，才能建立 `class iteractor`。 

當 iterator 無法取得下一筆資料，將會觸發 `StopIteration exception`。

``` Python
mytuple = ("apple", "banana", "cherry")  
myit = iter(mytuple)  #    myit 即是 mytuple 的 iterator
  
print(next(myit))  
print(next(myit))  
print(next(myit))
```

### else

`for loop` 最終會因為跑完所有的資料而離開 loop。但是，如果是透過 `for loop` 進行資料的尋找，當找到資料的時候可以使用 `break` 中斷迴圈。Python 提供了一個獨特的機制，用來處理完整跑完 `for loop` 之後的行為

``` Python
for iterator in Range:
    Statement
#    如果沒有在 Statement 中呼叫 break 離開 for loop，最後會執行 else block 中的程式
else:
	Statement_Complete
```

## while loop

可以透過 `continue` 與 `break` 重啟或是離開 `while loop`
``` Python
while Condition_A:
    Statement_A   #    需要在 Statement_A 中改變 Condition_A 的狀態，不然會永久困在迴圈
```

## Looping using list 

Python 本身支援的型態包含 list 等支援了 iterator 功能，因此這些 class 常會搭配以上各種 loop 機制；但是 Python 透過這個機制，建制了一個特殊的 loop 機制。語法如下

> newlist = [_expression_ for _item_ in _iterable_ if _condition_ == True]
>
> 其中 {_expression_} 為原本 loop 中應該執行的命令，上述的命令等同如下的命令
> 
> newlist = for _item_ in _iterable_:
>             if _condition_ == True:
>                  _expression_

``` python
thislist = ["apple", 2, (3, True)] 
[print(x) for x in thislist]
```

比較複雜的案例如計算[矩陣乘績](https://zh.wikipedia.org/zh-hant/%E7%9F%A9%E9%99%A3%E4%B9%98%E6%B3%95)
![[Screenshot 2025-03-26 at 1.10.24 PM.png]]

``` python
A = [ (1, 2), (3, 4) ]
B = [ (5, 1), (2, 1) ]
result = [ [ sum(i * j for i, j in zip(C,D)) for D in zip(*B) ] for C in A]

'''
for C in A] --> (1, 2) 與 (3, 4)
for D in zip(*B)] --> (5, 2) 與 (1, 1)

for i, j in zip(C, D)) for D in zip(*B)] for C in A] = 
for C in A:                       (1, 2), (3, 4)
    for D in zip(*B):             (5, 2), (1, 1) 從上到下 zip(*)
        for i, j in zip(C, D):                   從左到右 zip()
            1, 5    <-- (1, 2) (5, 2) first element 
            2, 2    <-- (1, 2) (5, 2) second element
            1, 1    <-- (1, 2) (1, 1) first element
            2, 1    <-- (1, 2) (1, 1) second element
            3, 5
            4, 2
            3, 1
            4, 1
	        sum(i*j)
		        5 + 4 = 9
		        1 + 2 = 3
		        15 + 8 = 23
		        3 + 4 = 7
'''
```