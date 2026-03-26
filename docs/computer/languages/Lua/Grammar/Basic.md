
# Chunk

`Lua` 程式的基本區塊被稱為 "chunk"。它可以是一個檔案，也可以是直譯器中的一行程式碼；一個比較明確的說法，一個 chunk 就是連續的程式碼。它可以是一個函式，或是單一程式碼。

與 `C/C++` 語言不同之處在於，`Lua` 不需要程式碼的分隔符號。`C/C++` 需要分號 (";") 標註每一行程式碼的結束，但是 `Lua` 並不需要。 因此，以下四個 chunk 在 `Lua` 中都是合法：

``` Lua
a = 1
b = a*2

a = 1;
b = a*2;

a = 1; b = a*2

a = 1 b = a*2
```

# Reserved words

以下為 `Lua` 語言的保留字。`Lua` 會分辨大小寫，因此 `And` 與 `AND` 會被視為不同的物件。
``` lua
and break do else elseif end false for function if in local nil not or
repeat return then true until while
```

## Comment

單行註解可以使用雙連字符 ("--")
多行註解可以置於 `--[[` 與 `]]--` 所包圍的區塊中

## Variables

不意外的，`Lua` 與 `Python` 一樣都是動態型別的語言。變數將隨著賦予的值，而改變其型態。`Lua` 內建的型態包含：

| Type     | Description                                                |
| :------- | ---------------------------------------------------------- |
| nil      | 任何未初始化或是未宣告的變數，其型態皆被視為為 nil<br>程式若使用到未初始或是未宣告的變數，並不會 crash |
| boolean  |                                                            |
| number   |                                                            |
| string   |                                                            |
| userdata |                                                            |
| function |                                                            |
| thread   |                                                            |
| table    |                                                            |

### Global 與 Local

