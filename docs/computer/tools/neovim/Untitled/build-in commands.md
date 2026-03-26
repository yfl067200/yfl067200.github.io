目前 neovim 支援的命令如下：

# abs(expr)

當參數 (expr) 為數值型態，將返回參數的絕對值；不然將返回 -1，並給予錯誤訊息。命令格式如下：

``` 範例
: echo abs(-9)
9

: echo abs(p)
E121: Undefined variable: p                                            
E116: Invalid arguments for function abs                                        
Press ENTER or type command to continue
```
# acos(expr)



# add(object, items) ＊

將參數 (items) 加入 `list` 或是 `blob` 型態的物件 (object) 中。與命令 `extend()` 不同之處在於，當參數是也是 `list` 型態，命令 `add()` 會將參數視為一個元素塞進物件中；而命令  `extend()` 則會將兩個 `list` 合併

``` 範例
: let mylist = add( [1, 2, 3], 4 )
: echo mylist
[1, 2, 3, 4]

: let mylist2 = add( mylist, [5, 6])
: echo mylist2
[1, 2, 3, 4, [5, 6]]
```

# and(expr, expr)

將兩個參數 (expr) 轉化成數值型態，再進行 `AND` 運算。倘若參數為 `list`、 `dict`、或是 `float` 型態，將會

``` 範例
: echo add(123, 0x77)
115

: echo add(1.3, 0x77)
E805: Using a Float as a Number
119
```

# api_info()

Neovim 在編譯時，會將 **/src/nvim/api/*** 路徑下的標頭檔進行解析，並建立一個被稱為 **api-metadata** 的 `dict` 型態資料。本命令會把完整的 **api-metadata** 的資料，以 JSON 格式顯示

# append(lnum, items)

如果參數 (items) 是一個 `list` 型態的資料，其每一個元素都將被視為一行文字，逐一塞進行數 (lnum) 之後：不然將被視為同一行的文字，塞進行數 (lnum) 之後。成功塞入文字，將返回 0；失敗將返回 1。

