Python 是==強型態==的語言，亦即不能將不同型態的資料結合在一起；譬如說，字串跟整數就不能結合。

但是 Python 同時也具有==弱綁定==的特性，亦即你可以隨時將不同型態的資料綁定在同一個變數：Python 在宣告變數時，不需要指定資料型態。當賦予值的時候，Python 會自動綁定變數的資料型態。

``` python
#    declaring variable
${VARIABLE}[ = ${VALUE}]

#    當不確定變數的資料型態，可呼叫 type( 變數 ) 函式獲得變數當前的資料型態
type( ${VARIABLE} )

#    亦可透過 isinstance( 變數, 型態 ) 函式確認變數是否是某種型態
isinstance( ${VARIABLE}, ${TYPE} )
```

# Object

Python 將 Object 視為唯一的抽象資料集合，變數不是 `Object`，就是 `Reference to Objects`。透過這種處理資料的方式，Python 可以輕易的實作弱綁定的特性 － 藉由指向不同的 `Oject`，變數可以隨時換成不同型態的資料。 

Object 本身被 Garbage Collection 機制管理，一旦 Object 沒有被 Reference 就會被 GC 收集並摧毀；使用 try ... except 機制有可能將 Object 的生命延長。另外 Object 內部存放的外部資源，譬如說是 file descriptor 等，無法透過 GC 釋放。

> [NOTE]
> 有一個[網站](http://pythontutor.com/)可以讓您寫入 Python Script，執行時會用視覺化的方式顯示變數的內容

Python 的 Object 有 3 種主要的屬性：

- Identity
- Type
- Value
## Identity

其中 Identity 是當 Object 建立時便賦值，這個屬性的值並不會改變；Identity 之於 Python 就如同 Address 之於 C。運算元 `is` 就是用來比對兩個變數的 Identity 是否一致，可透過 `id()` 獲得某 Object 的 Identity；這也是用來確認變數指向的 `Object` 是否為同一個個方式。

## Type

Type 對於 Object 的影響至大，甚至於影響到變數的行為。

我們以數值與 list 為例，初始化變數時的行為會因為不同 Type 而有不同的行為：
``` Python
def same( a, b ):
    if id(a) == id(b):
        print('Both variables are reference to same object')
        return

    print(f'Variables are reference to different object {id(a)}/{id(b)}')

    if a == b:
        print('Variables have same value')
        return

    print('Varables have different values')

a = 1
b = 1
same(a,b)

list_a = []
list_b = []
same(list_a, list_b)

list_c = list_d = []
same(list_c, list_d)

a = 2
same(a,b)
```
上述程式的結果如下：
``` python
# 因為變數 a 與 b 賦予相同的值，故 Python 建立一個 Object 並與變數 a 與 b 進行綁定
Both variables are reference to same object

# list_a 與 list_b 雖然被賦予相同的空 list，但是 Python 並不回重複綁定同一個 Object 到 list 型態
Variables are reference to different object 4299666944/4299507840
Variables have same value

# 可以透過強制綁定的方式，讓 list 綁定到同一個 Object
Both variables are reference to same object

# 因為 a 重新賦值，Python 建立另一個 Object 並與變數 a 綁定
Variables are reference to different object 4312162528/4312162496
Varables have different values
```
## Value

Python 將 Object 或是 reference to Object 作為儲存變數的唯一方法。

> [IMPORT]
> 出乎人意料之外，Python Object 的 Type 與 Value 也是不可改變的：每當你對變數進行任何運算動作，導致形態或是值改變，Python 的處理方式是敨過建立新的 Object 並重新將變數與新的 Object 進行綁定。

Python 的變數依照能否重新指派 Object 中的 Value 屬性，分成 mutable 與 immutable 兩種不同型態：簡單的型態，像是 number、 string、與 tuple 為 immutable Object；而 dictionary 與 list 等 container，則為 mutable Object。就實作的方式來說，前者就是變數指向存放值的 `Object`：而後者可能是使用類似 double linker 的機制傳連多個存放值的 `Object`，藉由改變 linker 指向的 `Object`，達到改變內含資料的方式。

> [mutable 變數]
> Container 型態的資料，包含 tuple 型態，在其 Value 欄位存放的並非值，而是另一個 Object 的 Reference。
> 
> 如果 Reference 的 Object 是一個 mutable 型態，可以透過改變被 reference 的 Object 的值，改變 Container 的值。以 tuple 為例，雖然 tuple 是 immutable Object，但是如果賦值時 reference 的是某個 list 的某些 element，可以藉由改變 list 的值進而改變 tuple 的值。

目前已知的 `mutable` 型態包含：

- Bytes Array
- Dictionary
- List
- Set
### Shadow Copy 與 Deep Copy

Python 的 `Object` 機制可能在初始化或是函式呼叫時，採用不同的方式傳遞參數。比較簡單的判斷方式是，如果參數是 `mutable`，那就會使用 `call by reference`；而 `Immutable` 參數，則是採用 `call by value` 的機制。

``` python
def modify_list(mylist):
    print(f'Lenght of list is {len(mylist)}')
    mylist.append('ADDED')

def modify_string(mystring):
	mystring = "Modified string"
	print(f'mystring is {mystring}')

foo=[ 1, 'A', True]
print(foo)    # [1, 'A', True]

modify_list(foo)
print(foo).   # [1, 'A', True, 'ADDED']

bar="Hello World"
print(bar)    # Hello World
modify_string(bar)
print(bar)    # Hello World
```



因為 Python 的 Immutable 的機制，透過 copy constructor 建立物件需要特別注意，不然有可能是指向相同的 object。這在呼叫函式或是建立物件特別需要注意，不然可能會導致原本的變數的值被函式或是物件改變。

![variables](https://github.com/yfl067200/yfl067200.github.io/assets/159564672/5db1efc7-1679-4d54-8e3e-38dba0f596f3)

Figure 2-4. 本想建立一個 `3*3` 的陣列；結果只有 `3*1` 的陣列，其他兩行只是 reference 到第一行

透過 copy module 中的 copy() 函式，可以將變數 (shadow copy) 進行複製，如此一來就可以確保原本的變數不會意外被修改；但是 shadow copy 的問題是，他只能複製一層 (variable -> object)。當有多層 (variable -> object -> object) 的資料，shadow copy 就無能為力。這時需要透過 copy module 中的 deepcopy() 函式進行 deep copy，確保多層的資料都有複製。

# Standary Type Heirarchy

Python 內建的資料型態包含以下

## 特殊類

| Type             | 說明                                                                  |     |
| :--------------- | :------------------------------------------------------------------ | --- |
| `None`           | 程式中的 `None` 即為此 Object <br> 函式若沒有指定回傳值，將回傳此 Object <br> 其值等同於 False |     |
| `NotImplemented` | 當呼叫未實作的函式將會回傳此 Object                                               |     |
| `Ellipsis`       | 程式中的 `...` 或是 `Ellipsis` 即為此 Object <br> 其值等同於 True                 |     |

## 數值類 Object (`numbers.Number`)

| Type               | 說明                                                                                                                                                                                                                      |
| :----------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `numbers.Integral` | 用來處理正負數整數與 Boolean 值 <br> 整數的範圍是無限的，且為了 shift 等運算，採用 binary 形式保存 <br> 布林值算是整數的子集，其值為 `False` (0) 與 `True` (1) 兩種<br>     -> 任何非 0 的值都被視為 True，但是 True 的值永遠被視為 1<br> 除法分為真除法  (`/`) 與整數除法 (`//`) 兩種，整數除法只回傳商，不進行進位等額外的處理 |
| `numbers.Real`     | 浮點數，採用 Double-Precision Floating-Point Value 的規範<br>     -> 因為近似問題，導致使用 `Real` 型態進行計算可能會導致問題。為此 python 提供 `Decimal` 型態，用來處理非整數的計算                                                                                       |
| `number.Complex`   | 採用一對浮點數用來表示複數 (z = a + bx) 的實部與虛部的值，可以透過 z.real (a) 與 z.imag (b) 屬性存取                                                                                                                                                   |
| `number.Fractions` | 擁有兩個屬性，`numberator` (分子) 與 `denominator` (分母)                                                                                                                                                                           |
| `number.Decimal`   |                                                                                                                                                                                                                         |

## 序列類 Object (`Sequences`)

序列類的 Object 擁有一個有限，且可透過正整數 (起始為 0) 的 Index 定位的資料集合。

Python 內建一個名為 len() 的 API 說明序列內資料的長度；假設序列 Object `A` 的長度是 `N`，可以透過 `A[n]` 存取 `A` 內部的資料；而 `n` 的範圍為 0 ~ N-1。

部分序列類的 Object 支援更多功能的 `[]` 運算元。包含：

1. 使用負數反向存取資料。`A[-1]` 即取得最後一個資料，`A[-2]`即取得倒數第二個資料
2. 重複取樣，

> [Note]
> 重複取樣的格式為 `A[i:j:k]`；其中 `i` 與 `j` 為取樣的範圍，而 `k` 為取樣的間隔。
> 
> 舉個例子， `A` 是一個連續 100 個正整數的 `list`，第一個值是 1。如果我們透過 `A[10:20:2]` 所取樣到的資料就是 11、13、15、17、與 19。這是因為 `A[10:20]` 的集合是 11、12、13、14、15、16、17、18、19、與 20，而取樣的間隔為 2，所以取樣的 Index 就會是 0、2、4、6、與 8。
 > 
> 其中 `i` 與 `j` 可以省略，表示取樣的範圍開始於第一個，或是結束於最後一個。如果省略 `k`，表示取樣範圍內的所有資料都要。

### Mutable Seqences

| Type          | 說明                                                                                                     |
| :------------ | :----------------------------------------------------------------------------------------------------- |
| `Lists`       | `List` 儲存的任一元素都是一個獨立的 `Object`，可以視為透過 `Reference to Object` 的方式，以一種類似 `Table` 的方式將獨立的 `Object` 集合成一個資料 |
| `Byte Arrays` | `Byte Array` 就是一個儲存多個 `Byte` 的陣列 <br> 可以透過 `decode()` 將 `ByteArray` 轉換成 `String`                       |
### Immutable Sequences

| Type      | 說明                                                                                                                                                                                                                                |
| :-------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Bytes`   | `Byte` 的儲存長度是 8-bits <br> 可以透過 `decode()`將 `Byte` 轉換成 `String`                                                                                                                                                                    |
| `Strings` | 資料格式是 Unicode，其範圍為 `U+0000` ~ `U+10FFFF` <br> 可以透過 `ord()` 將長度為 1 的 `string` 轉成 `Integer` (0~0x10FFFF)，也可透過 `char()`  將 `Integer` ( 0~0x10FFFF) 轉成一個長度為 1 的 `String`。<br> 將 `String` 轉換成 `Byte` 或是 `Byte Array` 的方式為透過 `encode()` |
| `Tuples`  | `Tuple` 儲存的任一元素都是一個獨立的 `Object`，可以視為透過 `Reference to Object` 的方式，以一種類似 `Table` 的方式將獨立的 `Object` 集合成一個資料 <br> Tuple 可以視為 Container 的一種，但是依照其內部設計，讓他成為 Immutable                                                                    |

## 集合類 Object (`Set`)

與序列類的 Object 不同之處在於，集合類的資料是獨一無二，不可重複且亂序。注意獨一無二這一個特性，對於 `1` 或是 `1.0` 這兩個值因為比對會被判斷是同一個值，所以只會存在一個。
 
因為是亂序，因此透過 Index 存取資料的方式不適合 (每次執行時，資料的位置可能不一樣)；因此透過 `len()` 取得總數，並依序逐一存取資料是推薦的方式。

`Set` 主要用於從 `Sequence` 中剔除重複的資料，或是進行數學中 `insert`、 `union`、或是 `difference` 等運算

| Type          | 說明                                                                                        |
| :------------ | :---------------------------------------------------------------------------------------- |
| `Sets`        | 是 Container 之一，為 mutable                                                                  |
| `Frozen Sets` | 使用 `hash` 方式建立 `Set` 內部元素的順序，使其成為 Immutable <br> 可以成為其他 `set` 的元素，或是 `Dictionary` 的 `Key` |
## 對照類 Object (`Mapping`)

類似序列類 Object，可以透過 `Index` 存取某一元素。但是與 `Sequence` 不同之處在於，其 `Index` 並非正整數，而是對應的 `Key`。可以透過 `len()` 取得元素的總數量，並透過 iteration 的方式逐一取得 `Key`。

| Type           | 說明                       |
| :------------- | :----------------------- |
| `Dictionaries` | 是 Container 之一，為 mutable |













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

## 內建型態

### 數字

透過 type( 變數 ) 函式可以得到 int 型態 (整數) 或是 float 型態 (非整數)。

不管是何種型態，數字類的變數都可以使用四則運算符號 (+-*/)；除法使用 **/** 運算符一定會得到 float 型態的值，可以改用 **//** 運算符得到 int 型態，但是是無條件去到小數位的值。

| 運算符號 | 說明 |
|:---------|:-----|
| + | 加法 |
| - | 減法 |
| * | 乘法 |
| ** | 倍數 |
| / | 除法，得到包含小數位的商 (float 型態) |
| // | 除法，只得到整數值的商，小數位無條件捨去 (int 型態) |
| % | 除法，得到餘數 |

### 字串

透過 type( 變數 ) 函式可以得到 str 型態。

Python 可以使用單引號 **'...'** 或是雙引號 **"..."** 表示字串；兩者沒有差別，只是如果資料中單引號或是雙引號時，可以使用另一種標示字串，省去在內容中使用 **\'** 或是 **\"**。如果不想讓內容中的 **\** 被當作特殊符號，可以在單引號或是雙引號前加上 **r**

``` python
>>> print( 'c:\test' )
c:	est

>>> print( r'c:\test' )
c:\test
```

如果內容要多行呈現，可以使用 **\n** 表示換行；但是 python 也提供使用 3 個單引號 **'''...'''** 或是 3 個雙引號 **"""..."""** 的方式處理多行。另外，在程式碼中也可以用這種方式進行多行註解。

``` python
print("""\
Usage: thingy [OPTIONS]
     -h                        Display this usage message
     -H hostname               Hostname to connect to
""")
```

| 運算符號 | 說明 |
|:---------|:-----|
| + | 兩字串結合 |
| * | 字串進行複製 |
| [M] | 存取字串中第 N 個字元，從 0 開始 </p> 如果 N 是負數，則是反向第 N 個字元 |
| [M:N] | 存取字串中第 M 個字元開使的 N 個字元 </p> 如果 M 為空，表示從頭開始 </p> 如果 N 為空，表示到底 <p> 如果 M 為負值，則是反向第 M 個字元開始，往字尾 N 個字元 |
| [M:N:O] | 規則同上，只是間隔 O 個字元取樣一次 |

使用 **[M:N:O]** 運算符時，如果超出字串內容，會從頭繼續算，不會有超出範圍的問題。此外，字串在 python 是 immutable 的，所以不可使用 **[]** 運算元修改某個字元

### List 

Python 有支援幾種結合資料的型態，其中一個就是 list。建立 list 的方式就是使用中括弧 **[]**，各資料間使用逗號分隔；每一個資料的型態，不必相同。如下例

``` python
>>> squares = [ 1, 3, 5, 7, 9, "EOL" ]

#    兩個 list 合併
>>> squares + [ 2, 4, 6, 8 ]

#    Item 數量
>>> len( squares )
10

#    多重 list 
>>> multi-lists = [ [ 1, 3, 5, 7, 9 ], [ 2, 4, 6, 8 ], [ 'A', 'a' ] ]

>>> mulit-lists.append( "EOL" )			#    在 list 中加上新的 item
```

List 跟字串一樣，可以透過 **[M:N:O]** 運算符存取內涵的資料。跟字串不同的地方在於，list 是 mutable，所以可以透過 **[M:N:O]**修改某一個/群 item 的值。

指派 list 給另一個變數，就類似 C 中的指派位置給另一個指標，兩個 list 變數都指向同一組 object；當透過某一個變數修改 list 中的某一個值，可以透過另一個變數看到。Pyhton 對 list 進行深度複製的方式是透過 **[M:N:O]** 運算符

``` python
>>> rgb = [ "Green", "Red", "Blue" ]
>>> rgba = rgb
>>> rgba.append( "Alph" )
>>> print( rgb )
[ "Green", "Red", "Blue", "Alph" ]

>>> rgbb = rgb[:]		#    深層複製
>>> rgbb[3] = "Alpha"
>>> print( rgb )
[ "Green", "Red", "Blue", "Alph" ]

>>> print( rgbb )
[ "Green", "Red", "Blue", "Alpha" ]
```
