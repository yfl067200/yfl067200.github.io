# 資料型態

Python 是強型態的語言，亦即不能將不同型態的資料結合在一起；譬如說，字串跟整數就不能結合。但是 Python 是弱綁定，亦即你可以隨時將不同型態的資料綁定在同一個變數。再加上 Python 在宣告變數時，不需要指定資料型態。當賦予值的時候，Python 會自動綁定變數的資料型態。當不確定變數的資料型態為何，可以呼叫 type 函式，即可獲得變數當前的資料型態

``` python
#    declaring variable
${VARIABLE}[ = ${VALUE}]

#    data type of variable
type( ${VARIABLE} )
```

## 變數

[Python Tutor](https://pythontutor.com/visualize.html#mode=edit) 網站提供了視覺化的方式說明 Python 程式碼在記憶體上的運作。

Python 在記憶體上建立 Object 儲存資料，而變數就想是 C 的指標，指向 Object 的位置。藉由這種方式，Python 才能實踐強型態與弱綁定的機制。對於 Python 而言，任何數值都會被當成 Object 儲存在記憶體中；每一個 Object 都有一個 ID，而變數儲存的是 Object 的位置。當改變變數的值時，Python 不是跟其他語言一樣，將變數指向位置的內容改變；Python 是將變數儲存新資料 (Object) 所在的位置。Python 提供一個名為 id 的函式，可以查看變數指向的 Object ID。

``` python
def same( var1, var2 ):
    if var1 == var2 and var1 is var2:
        print( f"same value and same object" )
    elif var1 == var2 and not var1 is var2:
        print( f"same value but different objects: var1 = {id(var1)}, var2 = {id(var2)}" )
    elif not var1 == var2 and var1 is var2:
        print( f"Different values ({var1}/{var2})but same object" )
    else:
        print( f"Different values ({var1}/{var2}) and different objects: var1 = {id(var1)}, var2 = {id(var2)}" )

T0 = 42
T1 = 42
Ref = T0
same( T0, T1 )
same( T0, Ref )
same( T1, Ref )

T0 = 43
same( T0, T1 )
same( T0, Ref )
same( T1, Ref )
```

![image](https://github.com/yfl067200/yfl067200.github.io/assets/159564672/92513ae3-2358-4d2c-9575-3abb8610631e)

Figure 2-1. Python 變數、值、與記憶體的關係

Python 在 Object 中使用 reference count 管理記憶體。當 Object 中的 reference count 歸零之後，就會被清除。


### Immutable 與 Mutable

在 Python 中所謂的 Mutable 是指可以修改記憶體中 Object 所儲存的值，而 Immutable 是指需要建立一個新的 object 才能改變變數的值。屬於 Immutable 的資料型態包含整數 (Integer)、浮點數 (Float)、字串 (String)、與元 (tuple)，而 mutable 的型態包含列 (List)、集合 (Set)、與目錄 (Dictionary) 等。

簡而言之，任何變數指向的 Object 內容都不能改，但是 Object 另外指向的 Object 內容是可以更改的；例外是元，元儲存的 Object 的位置。而該 Object 又指向實際值儲存的位置。但是 Python 不讓人修改元裡面的內容。下圖中的 Frames 中的變數與值可以視為變數指向儲存值 (object) 的位置，可以從 output 的 ID 判斷

![Immutable](https://github.com/yfl067200/yfl067200.github.io/assets/159564672/9075ea26-9d83-4aae-a55d-0d1e2932ae65)

Figure 2-2. Immutable 型態

![Mutable](https://github.com/yfl067200/yfl067200.github.io/assets/159564672/7277465d-265e-4dfc-9fc5-084706b906c2)

Figure 2-3. Mutable 型態


### Shadow Copy 與 Deep Copy

因為 Python 的 Immutable 的機制，透過 copy constructor 建立物件需要特別注意，不然有可能是指向相同的 object。這在呼叫函式或是建立物件特別需要注意，不然可能會導致原本的變數的值被函式或是物件改變。

![variables](https://github.com/yfl067200/yfl067200.github.io/assets/159564672/5db1efc7-1679-4d54-8e3e-38dba0f596f3)

Figure 2-4. 本想建立一個 3*3 的陣列；結果只有 3 * 1 的陣列，其他兩行只是 reference 到第一行

透過 copy module 中的 copy() 函式，可以將變數 (shadow copy) 進行複製，如此一來就可以確保原本的變數不會意外被修改；但是 shadow copy 的問題是，他只能複製一層 (variable -> object)。當有多層 (variable -> object -> object) 的資料，shadow copy 就無能為力。這時需要透過 copy module 中的 deepcopy() 函式進行 deep copy，確保多層的資料都有複製。


### 遮蔽 Shadowing

其他語言的全域變數可以被任何子 scope 存取，但是對 Python 而言，子 scope 並不認識全域變數；要讓子 scope 存取全域變數，必須先在子 scope 中宣告全域變數

``` python
gVar = 100
gTest = 0

def func():
    global gVar                      #    Need to declare global variable before accessing it
	print( f'gVar = {gVar}' )    #    If you don't declare global before accessing it, you will get an error
    gTest = 100                      #    gTest is local variable, and it will be destroyed after exiting this scope


func()
print( f'gTest = {gTest}' )          #    Result still be 0
```

Python 允許在函數中包含子函數。在子函數如果要存取父函數中的變數，亦要先宣告；因為父函數中的變數不是全域變數，所以 keyword 不再是 global 而是 nonlocal。

``` python
def func():
    var1 = 100;

	def subfunc(): 
	    nonlocal var1            #    declaring variable outside sub-funcion

		var1 *= 10

    print( f'var1 = {var1}' )        #    Result will be 1000
```

Python 搜尋變數的規則是 local --> nonlocal --> global --> build-in。幫變數取名需要注意，避免遮蔽了要使用的變數。另外，當存取 global 或是 nonlocal 變數都需要注意，可能會導致其他程式碼的行為錯誤；建議是改用 class 的方式

