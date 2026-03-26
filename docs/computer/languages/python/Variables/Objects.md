
在 Python 中，==物件 (Objects)== 是資料的抽象描述，任何 Python 中的資料都是以物件的方式呈現。所有的物件都擁有以下三個欄位：

| Field    | Description                                                                                                                                                                |
| :------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Identify | 當物件創始時便建立，其值永不會改變。物件間的 Identify 將是獨一無二，不會重複<p>可視為 C/C++ 中的記憶體位置，可以透過 Identify，存取物件<p>Python 中的 is operator 即比較物件的 Identify 是否一致，而另一個內建函式 ==id()== 則會回覆一個表示物件 Identify 的整數值 |
| Type     | 該欄位說明了物件所儲存的 Value 型態，也用來控制如何與物件互動的機制<p>可以視為 C++ 的類別 (class) <p>本欄位在物件創造時即建立，其值永不改變<p>內建函式 ==type()== 將會型態                                                                 |
| Value    | 物件所儲存的值<p>依據型態而分為不可改變 (Immutable) 與可以改變 (Mutable) 兩種                                                                                                                       |
Python 使用物件來儲存數值，而變數將透過物件的 identify 去存取物件。鑑於物件中的 Type 是不可改變的，且大部分物件中的 Value 也是 Immutable；在 Python 程式中改變變數的值，其實就是建立一個新的物件，然後將新物件的 Identify 重新指派給變數。

> [!Note]
> 在 Python 中建立一個數值，譬如說 `i = 3`。
> Python 會在記憶體中建立一個物件，其 `Type` 欄位的值為 "int" 且 `Value` 欄位的值為數字 3，而欄位 `Identify` 的值為 `Value` 在記憶體中的位置。然後



Python 雖然支援 Garbage Collection 機制，確保物件在不被需要的情況下會被自動清除。但是許多與系統資源相關的變數，如 file descriptor 或是 socket，即便物件消失這些資源依舊被系統視為存在；除非該物件能在被清除前，與系統進行式放資源的動作。在不確定物件實作者是否有相關的對應動作，最好不要相信 Python 的垃圾清除機制，盡量使用手動的方式處理系統資源的釋放。

> [!Note]
Python 建議使用 [try ... finally ...]() 或是 [with]() 語法，進行系統資源的管理。

Python 物件中的 Value，可能並非儲存實際的值，而是其他物件的 Reference 或是 Identify；這種形式的物件多為 Mutable，譬如說 list 或是 dictionary，但是也會出現在 Immutable 物件，如 tuple。是否使用 Reference 指向多個物件，純粹是該物件實作者的決定。

> [!Note]
> Python 不像 C/C++ 一樣，在 list 或是 dictionary 中使用相同型態的資料；因此可能會有第一個元素指向 tuple，但是下一個元素卻指向 string 的狀況。因此在處理變數前，先識別其型態可以有效避免程式崩潰的情況。

# 客製化

在 Python 定義==類別 (class)== 時，可以藉由重新定義特定函式 (使用相同名稱)，以達到 operator overloaing 的機制。

> [!Note]
> Overloading 為程式語言的一個術語，用來說明使用多個函式使用相同函數名稱，但是其參數型態與數量不同的狀況。其好處在於，使用同一個名稱不會讓程式碼過於雜亂，且方便開發人員記住；但又因為參數的差異，可供編譯器在 link 函式時，不至於鏈結到錯誤的函式。進而創造出一個單一函數名稱，但是實作不同的 API 供開發人員使用

由於 Python 物件的特性，導致客製化的函式不一定能正常運作。舉個簡單的例子，對 None 物件開發 `__iter__()` 函式就是一個沒有意義的事情。這邊列出了 [Python 物件的特殊函式列表](https://docs.python.org/3/reference/datamodel.html#special-method-names)，提供了開發新物件所需的所有資料。以下為簡易版本的說明

## Basic 

| 函式                                                                                                                                                                                           | 說明                                                                                                                                                                                                                                                                                                          |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `object.__new__(cls[, ...])`                                                                                                                                                                 | 為 static 函式，建立一個 class 型態的 instance<p>第一個參數為 instance 的 class，其他參數將被傳送為該 class 的 constructor 函式。<p>本函式被呼叫之後，會==自動呼叫== `object.__init__(cls[, ...])` 函式，建立該類別的實體並回傳<p>如果不回傳該類別的實體，則不會呼叫 `object.__init__(cls[, ...])` 函式<p>實作內容通常是呼叫 `super().__new__(cls[, ...])`，建立父 instance 之後，在結束回傳之前，對新的 instance 進行調整 |
| `object.__init__(cls[, ...]`)                                                                                                                                                                | 無回傳值。回傳任何非 None 的值將引發 TypeError 例外<p>`object.__new__(cls[, ...])` 與 `object.__init__(cls[, ...])` 兩個函式搭配，成為 constructor。前者建立 instance，後者客製化 instance                                                                                                                                                        |
| `object.__del__(self)`                                                                                                                                                                       | 物件的 destructor。當父代的 destructor 被呼叫，其子代也要被呼叫<p>當 Python 直譯器停止之後，==沒有保證==物件的 destructor 會被呼叫<p>由於物件大多採用 Reference Count 機制，唯有 Reference Count 歸零之後，才會去呼叫本 destructor                                                                                                                                          |
| `object.__repr__(self)`<br>`object.__str__(self)`<br>`object.__bytes__(self)`                                                                                                                | 這些都是回傳出 string 型態，用來說明物件的資訊<p>其中 `object.__str__(self)` 函式的值可以透過 `object.__format__(self, format_spec)` 決定輸出格<p>除了 `object.__bytes__(self)` 函式並未實作在 Object 類別，其他兩者都有內建                                                                                                                                      |
| `object.__lt__(self, other)`<br>`object.__le__(self, other)`<br>`object.__eq__(self, other)`<br>`object.__ne__(self, other)`<br>`object.__ge__(self, other)`<br>`object.__gt__(self, other)` | 對應於運算元 $\lt$、$\le$、$\equiv$、$\ne$、$\ge$、與 $\gt$<p>除了 $\equiv$ 與 $\ne$，其他回傳值都是 bool。型態不同，將觸發 TypeError 例外<p>$\equiv$ 預設使用 is operator 進行比對。相同返回 True，不同將返回 `NotImplemented`<p>$\ne$ 則是呼叫 $\equiv$，再將結果顛倒                                                                                                     |

## Attributions



| 函式                                                                        | 說明                                                                                                                                                                                                                                                                                                |
| :------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `object.__getattr__(self, name)`<br>`object.__getattribute__(self, name)` |                                                                                                                                                                                                                                                                                                   |
| `object.__setattr__(self, name, value)`                                   |                                                                                                                                                                                                                                                                                                   |
| `object.__delattr__(self, name)`                                          |                                                                                                                                                                                                                                                                                                   |
| `object.__dir__(self)`                                                    | 當 `object.dir()` 被呼叫時，會執行本函式。<p>返回值是 list 型態，內容應包含該物件可供執行的函式列表<p>當呼叫內建函式 dir(x) 時，會先檢查 x 這個物件是否有實作 `__dir__()` 函式；有實作則直接呼叫該函式。<p>若是沒有實作，預設的動作將會列出<br>1. 物件 `__dict__` attributes 下的資料，然後是<br>2. class 下 `__dict__` attributes 下的資料，接下來是<br>3. 父代 class 下 `__dict__` attributes 下的資料，直到最源頭的 class。 |
