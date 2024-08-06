##### Table of Contents  
- [Introduction]()
    - [使用流程](./argparse.html#%E4%BD%BF%E7%94%A8%E6%96%B9%E5%BC%8F)
- [建立 Parser]() 
    - [parents 參數](./argparse.html#parents)
	- [formatter_class 參數](./argparse.html#formatter_class)
	- [prefix_char 參數](./argparse.html#prefix_chars)
	- [fromfile_prefix_chars](./argparse.html#fromfile_prefix_chars)
	    - [客製化檔案解析機制]()
	- [conflict_handler](./argparse.html#conflict_handler)
	- [exit_on_error](./argparse.html#exit_on_error)
- [客製化 Parsing 規則]()
    - [Name 與 Flags 參數](./argparse.html#name-%E8%88%87-flags)
    - [action 參數](./argparse.html#action)
    - [nargs 參數](./argparse.html#nargs)
    - [metaver 與 dest 參數](./argparse.html#metavar-%E8%88%87-dest)
- [Parsing 命令列]()
    - [解析的問題](./argparse.html#%E8%A7%A3%E6%9E%90%E7%9A%84%E5%95%8F%E9%A1%8C)
    - [Optional 變數](./argparse.html#optional-%E8%AE%8A%E6%95%B8)
        - [精簡化命令列參數](./argparse.html#%E7%B2%BE%E7%B0%A1%E5%8C%96%E5%91%BD%E4%BB%A4%E5%88%97%E5%8F%83%E6%95%B8)
	    - [模擬兩可的 optional 變數縮寫](./argparse.html#%E6%A8%A1%E6%93%AC%E5%85%A9%E5%8F%AF%E7%9A%84-optional-%E8%AE%8A%E6%95%B8%E7%B8%AE%E5%AF%AB)
	- [解析結果 (argparse.Namespace)](./argparse.html#%E8%A7%A3%E6%9E%90%E7%B5%90%E6%9E%9C-argparsenamespace)
- [進階議題]()
    - [子命令](/argparse.html#%E9%9A%8E%E7%B4%9A%E5%8C%96%E7%9A%84%E5%91%BD%E4%BB%A4)
	- [檔案物件]()
    - [變數 group](./argparse.html#group)
	    - [互斥]()
	- [Parser 的預設值]()
	- [混用式命令列參數]()


## Introduction

argparse 模組是用來解析 (parse) 命令列參數。本模組的核心物件是 ArgumentParser，本物件提供幾個 API 供使用者進行客製化解析命令列，方便找出命令列中的 **變數** 與對應的 **值**。

命令列中的參數會被分成兩類 **變數**，一類是 optional 變數，另一類是 positional 變數：

| 變數類別 | 賦予方式 | 說明 |
|:---------|:---------|:-----|
| Optional 變數 | 比對名稱 | 比對命令列參數與變數名稱，相符的參數將只配給 Optional 變數 <br> 變數名稱多以 '-' 或是 '--' 起始。這個 prefix char 可以客製化 <br> 命令列中不需要有這些變數 |
| Positional 變數 | 依照順序 | 命令列中，不屬於 Optional 變數的參數，將依照加入客製化規則的順序指派為 Positinal 變數 <br> 所有 Positional 變數都應該在命令列中出現 |


### 使用流程

以下為使用 argparse 模駔進行解析命令列的方法；

``` python3
import argparse

#    建立 Parser
parser = argparse.ArgumentParser( ... )

#    客製化 Parsing 規則
parser.add_argument( ... )

#    Parsing 命令列
data = parser.parse_args( ... )			#    data 為 argparse.Namespace 物件
```


## 建立 Parser

ArgumentParser 的 constructor 宣告為
``` python3
class argparse.ArgumentParser(
	prog = None,                                   #	執行的程式名稱，預設是 os.path.basename( sys.argv[ 0 ] )
	usage = None,                                  #	說明如何使用，預設將從客製化規則自動生成
	description = None,                            #	會被置於說明中的一個部分，通常會出現在 `options` 項目之前
	epilog = None,                                 #	會被置於說明中的最後一個部分
	parents = [],                                  #	將其他 ArgumentParser 的設定變成自己的設定
	formatter_class = argparse.HelpFormatter,      #
	prefix_chars = '-',                            #
	fromfile_prefix_chars = None,                  #
	argument_default = None,                       #	
	conflict_handler = 'error',                    #
	add_help = True,                               #    是否加上 -h/--help 功能
	allow_abbrev = True,                           #	在命令列中能否使用縮寫。建議設為 False，避免[模擬兩可的解析問題]()
	exit_on_error = True                           #	是否在發生錯誤時
)
```

原則上不需要額外調整參數，直接建立的 ArgumentParser 實體就可以正常運作。以下為需要額外說明的參數：


### parents

透過將各 parsers 的實體放進 parents 參數中，即可將各 parsers 建立的規則納為己用。

Parents 的規則可能會有重複 key，導致解析的方式錯亂，需要搭配另一個參數 `conflict_handler` 處理相關的錯誤 


### formatter_class

設定顯示說明 (Help) 資訊的格式。有四種選項；

| 格式 | 說明 |
|:-----|:-----|
| argparse.RawDescriptionHelpFormatter | 預設的顯示方式會自動對 description 與 epilog 的文字進行縮排，使用者無法定義縮排方式。本模式將改用使用者定義的縮排方式 |
| argparse.RawTextHelpFormatter | 如同上面的 argparse.RawDescriptionHelpFormatter，本模式會保留使用者定義的空白等。但是會將多行空白行變成一行 |
| argparse.ArgumentDefaultsHelpFormatter | 類似預設模式，但是會將各變數的預設值列出 |
| argparse.MetavarTypeHelpFormatter | 類似預設模式，但是會將各變數的型態列出 |


### prefix_chars

大部分的 optional 變數都是使用 '-' 開頭，像是 '-h' 或是 '--help'。這個參數可以讓使用者決定變數的開頭字符，每一個字符都是獨立，且不計次數。以下舉例：

``` python3
>>> parser = argparse.ArgumentParser(prog='PROG', prefix_chars='-+')
```

命令列參數開頭為 '-'、'+'、'--'、'++'、或是 '-------' 等，都會被視為 **Optional 變數**；如果後面接的 key 是一樣的 (`-h` 與 `--h`)，會被視為相同的 **opentional 變數**。相反的，只要開頭不是 '-' 與 '+' 的資訊，都不會被視為 **Optional 變數**


### fromfile_prefix_chars

如果命令列參數太長，可以將其寫入檔案。透過指定開頭字符，ArgumentParser 會將某些命令參數視為命令列檔案，將開啟該檔案並進行解析的動作。以下範例為讀取 args.txt 檔案，並解析之

``` python3
>>> parser = argparse.ArgumentParser(fromfile_prefix_chars='@')
>>> parser.add_argument('-f')
>>> parser.parse_args(['-f', 'foo', '@args.txt'])
```

**預設值為 None，表示不會讀取檔案**


#### 客製化檔案解析機制

如果要客製化檔案解析的動作，譬如說進行跨行處理，使用特殊分隔符號，甚至於是特殊的檔案格式。可以建立一個繼承自 argparse.ArgumentParser 的物件，然後改寫其 convert_arg_line_to_args( self, arg_line ) 函式

當 ArgumentParser 進行檔案解析時，他預設會一行一行進行解析動作。所以 convert_arg_line_to_args() 函數中的參數 arg_line，即為檔案中的每一行資料

``` python3
>>> class MyArgumentParser(argparse.ArgumentParser):
>>>    def convert_arg_line_to_args(self, arg_line):
>>>        ...
```

### conflict_handler

ArgumentParser 不能接受相同名稱有多種處理方式。當使用 parent 參數，將多個 ArgumentParser 的規則加入時，很容易導致衝突。

解決方法是在建立 ArgumentParser 時，加上參數 **conflict_handler='resolve'**；這樣後加上的參數規則將會覆蓋掉舊的參數規則。以下為範例，會將 help 訊息一併處理

``` python3
>>> parser = argparse.ArgumentParser(prog='PROG', conflict_handler='resolve')
>>> parser.add_argument('-f', '--foo', help='old foo help')
>>> parser.add_argument('--foo', help='new foo help')
>>> parser.print_help()

usage: PROG [-h] [-f FOO] [--foo FOO]

options:
 -h, --help  show this help message and exit
 -f FOO      old foo help
 --foo FOO   new foo help
```


### exit_on_error

如果解析的命令列有問題，預設將會中止程式 (exit_on_error = True)。將此參數設為 False，將會跳出 argparse.ArgumentError 的例外。可供開發人員進行例外處理

#### 錯誤處理機制

``` python3
class ArgumentParser
    def exit( self, status = 0, message = None ):    #	處理離開的機制。將回傳 status 的值，並列印出 message 的訊息
	    if status:
		    raise Exception( f'Exiting because of an error: {message}' )
		exit( status )

    def error( message ):                            #	將 status 設為 2，並觸發上述離開機制
```


## 客製化 Parsing 規則

此為 ArgumentParser 的核心，用來處理每一個 **變數** 的規則

``` python3
ArgumentParser.add_argument(
	name or flags               #	變數的名稱。譬如說 foo 或是 -f, --foo
	[, action]                  #
	[, nargs]                   #	const 或是 default 參數需搭配此參數才能產生效果
	[, const]                   #	在命令列中找到變數卻沒有找到對應的值，將用此數值作為該變數的值 (預設是 None)
	[, default]                 #	在命令列中沒有找到變數，將會自行建立該變數，且賦予此值 (預設是 None)
	[, type]                    #
	[, choices]                 #	列出變數可接受的值，可使用 list 等 sequences 結構，包含 range()
	[, required]                #	用於表示 Optional 變數必須在命令列中
	[, help]                    #	該參數的說明，會自動加入說明描述
	[, metavar]                 #	在說明文件中表示變數值的文字
	[, dest]                    #
)
```


### name 與 flags

必須是該函式第一個參數。如果有 `prefix char` 開頭 (optional 變數)，可以列出多個參數 (flags)；這些參數中，剔除掉 `prefix char` 之後，最長的名稱將成為 optional 變數的名稱；Optional 變數的名稱中的 '-' 字符將被換成 '_' 字符

如果沒有 `prefix char`，則只會將第一個參數視為 positional 變數的名稱。第二個之後的參數會被視 add_argument() 函式中其他參數。

``` python3
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument( '-f', '--foo' )        #    有 prefix char 的參數，通通會被視為第一個參數
>>> parser.add_argument( 'bar' )                #    沒有 prefix char 的參數，只有第一個參數會被接受
>>> parser.add_argument( "foo", "bar" )         #    不接受多個無 prefix char 的參數
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/lib/python3.12/argparse.py", line 1467, in add_argument
    chars = self.prefix_chars
            ^^^^^^^^^^^^^^^^^
```


### action

當找到對應的字符 (即 name or flags) 時的處理方式。目前支援的 actions 包含以下幾種：

| action | 搭配參數 | 說明 |
|:-------|:---------|:-----|
| action='store' | | 預設行為，將變數與值存在 arguparse.Namespace 中 |
| action='store_const' | `const` | 類似	`store`，變數值為 `const` 參數的值 <br> 不需在命令列中提供值 |
| action='store_true' <br> action='store_false' | | 類似 `store`，但是變數的值不是 True 就是 False <br> 不需在命令列中提供值 |
| action='append' | | 當命令列中出現重複的 key 時，只會保留最後一個。本行為會將所有符合變數的值都記錄下來，存放在 list 物件中 |
| action='append_const' | `const` | 類似 `append`，只是 `const` 參數的值可以為 list 型態 <br> 不需在命令列中提供值 |
| action='count' | `default` | 用來記錄命令列中出現幾次符合字符，'-vvv' 會被視為出現 3 次 '-v' <br> 搭配 `default` 變數可調整預期的數值 | 
| action='help' | | 印出 ArgumentParser 中所有客製化參數的說明 |
| action='version' | | 印出使用 ArgumentParser 程式的版本 |
| action='extend' | | 類似 `append` |
| action=argparse.BooleanOptionalAction | | 使用者可以使用 --foo 表示 foo 變數為 True，或是 --no-foo 表示 False |


### nargs

在命令列中找到對應的 key 之後，其後的連續幾個參數都將被視為該變數的值。沒有指定 nargs 參數，不論是 `optional 變數` 或是 `positional 變數` 預設將命令列中下一個參數作為其變數的值。

nargs 後面接的值除了數字之外，還接受另外 3 種字符，分別是

- `'?'`：因為 parser 預期將下一個命令列參數視為值。假若無法將下一個參數作為值，包含沒有下一個參數或是下一個參數是另一個 opetional 參數的字符，ArgumentParser 會將 `const` 參數指定的值視為該 optional 變數的值 (不包含 positional 變數)；如果連 `const` 參數也沒指定，那就會出現 parse 錯誤

- `'*'`：當參數可以接受多個值，parser 會將變數之後的命令列參數都是為該變數的值，直到符合另一個符合變數的字符

- `'+'`：同 `'+'`，唯一的差異是命令列中至少要有一個值

使用上有個條件：Positional 變數無法指定多個值，除非先在 optional 變數中使用 nargs 參數


### metavar 與 dest

本函式的第一個參數 Name 與 Flags 會作為變數的名稱，詳細規則請參考第一個參數的說明。如果想要客製化變數名稱，可以使用 `dest` 參數。使用 `dest` 參數將會改變在 `argparse.Namespace` 物件中，變數的名稱

與 `dest` 參數不同，`metavar` 參數是用來表示變數值。用於說明文字中，幫助使用者提供易於理解的說明


## Parsing 命令列

當客製化 Parsing 的動作都完成之後，即可使用 ArgumentParser 物件進行命令列的解析。解析的函式 APIs 為

``` python3
argparse.Namespace = argparse.ArgumentParser.parse_args (
    args = None             #    需要解析的字串，預設來自於 sys.argv 變數
	namespace = None        #    解析完的資料所存放的
)

argparse.Namespace = argparse.ArgumentParser.parse_known_args (
    args = None, 
	namespace = None
)
```

其中 parse_args() 函式會處理所有命令列的參數，如果遇到未定義的資料 (某些命令列的參數無法歸類到變數中)，將會出現錯誤。而 parse_known_args() 函式則會正常返回一個有兩個 tuple 的 Namespace 物件；第一個 tuple 放置變數與對應的值，而第二個 tuple 放置無法歸類的命令列參數。

### 解析的問題

呼叫本函式之後，ArgumentParser 會進行解析的動作；包含了 positional 變數的數量是否正確，Optional 變數的解析是否沒有爭議等。解析的過程可能會出乎意料之外。以下為可能發生的爭議情況

``` python3
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument( "-x" )                #	Optional 變數，預設值是 None
>>> parser.add_argument( "foo", nargs="?" )    #	Positional 變數，值會存放在 list 中。預設值為 None

#	"-1" 可能是 Optional 變數的值，也可能是 Positional 變數的值
>>> parser.parse_args( [ "-x", "-1" ] )

#	"-1" 跟 "-5" 都不在 Optional 變數列表。故 "-1" 可能是 Optional 變數的值，而 "-5" 則是 Positional 變數的值
>>> parser.parse_args( [ "-x", "-1", "-5" ] )
```

為了避免可能的爭議，在命令列中可以加上一個 **"--" 參數**。在 **"--" 參數**之後的所有參數都將被視為 **Positional 變數**


### Optional 變數

以下為 Optional 變數的相關議題


### 精簡化命令列參數

一般來說，變數跟值是分開的兩個命令列參數。對於 optional 變數來說，對應 key word 參數的下一個參數就是該變數的值；對於 positional 變數來說，非 optional 變數的命令列參數，將依序賦值。對於 optional 變數來說，可以用同一個命令列參數表達參數與值。以下為相關的規則：

對於長度大於 1 的變數名稱，可以搭配 `--XX=值` 的方式將變數與值放在同一個參數中；長度為 1 的變數名稱，直接將值放在變數之後即可 (-xX = 變數 x 的值為 X)。甚至可以將多個長度為 1 的變數放在同一個命令列參數

``` python3
>>> parser = argparse.ArgumentParser(prog='PROG')
>>> parser.add_argument('-x', action='store_true')
>>> parser.add_argument('-y', action='store_true')
>>> parser.add_argument('-z')
>>> parser.parse_args(['-xyzZ'])
Namespace(x=True, y=True, z='Z')
```


#### 模擬兩可的 optional 變數縮寫

ArgumentParser 再比對 optional 變數時，可以接受使用縮寫。舉個例子來說，在命令列中可以使用參數 `-bac` 表示 `bacon` 這個 optional 變數

``` python3
>>> parser.add_argument('-bacon')
```

雖然縮寫可以方便使用者，但是可能會造成解析時發生模擬兩可的狀況，導致解析出錯。為了避免這類的錯誤，在建立 Parser 時，可以提供參數 `allow_abbrev=False`

``` python3
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('-bacon')
>>> parser.add_argument('-badger')
>>> parser.parse_args('-bac MMM'.split())     #	解析正常，變數 `bacon` 的值是 'MMM'
>>> parser.parse_args('-bad Wood'.split())    #	解析正常，變數 `bagger` 的值是 'Wood'
>>> parser.parse_args('-ba BA'.split())       #	解析失敗，無法判斷是變數 `bacon` 或是變數 `badger`
```


### 解析結果 (argparser.Namespace)

當解析完命令列之後，ArgumentParser 會返回一個 argparse.Namespace 的物件。該物件會包含可讀的文字描述，資訊會保留在該物件的 __dict__ 中。可以透過 args.foo 的方式讀取 args 這個 argparse.Namespace 物件中，名為 foo 變數的值。要取得在 argparse.Namespace 物件中所有的 Key，可以使用 vars() 函式將 argparse.Namespace 傳換成 dict-like 的資料型態

``` python3
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('--foo')
>>> args = parser.parse_args(['--foo', 'BAR'])
>>> print( f'foo = {args.__dict__[ "foo" ]}' )
foo = BAR

>>> print( f'foo = {args.foo}' )
foo = BAR

>>> data = vars(args)					#	Dictionary
>>> print( f'data = {data}' )
data = { 'foo', 'BAR' }

>>> print( f'foo = {data[ "foo" ]}' )
foo = BAR

>>> for i in data:
>>>    print( f'data = {i}' )
data = foo

>>> value = vars( args ).values()		#	Only Values
>>> for i in value:
>>>     print( f'value = {i}' )
value = BAR
```

另一種作法是解析的結果置於一個以建立的物件，再透過該物件獲得各變數的值

``` python3
>>> class C:
>>>    pass

>>> c = C()
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('--foo')
>>> parser.parse_args(args=['--foo', 'BAR'], namespace=c)
>>> c.foo
'BAR'
```


## 進階議題

### 子命令

目前的趨勢是同一個命令下面有子命令，每個子命令又有各自的命令參數。像是 git 命令，他的子命令包含 `git clone`、`git pull` 等，每個子命令有自己的參數結構。為了處理這種狀況，argparse 模組提供了加入子 Parser 的功能。如下，透過 ArgumentParser.add_subparser() 函式，可以獲得一個特殊的 action object，而這個 action object 只有一個函式，就是 add_parser()，建立一個新的 ArgumentParser 物件，處理子命令列的參數

``` python3
argparse.action = ArgumentParser.add_subparsers (
	[title]               #	子 Parser 的名稱。當 description 參數有定義，預設是 "subcommands"；不然是 positional 變數的名稱
	[, description]       #	子 Parser 的說明，預設是 None
	[, prog]              #	預設是程式名稱
	[, parser_class]      #	預設是 ArgumentParser，亦可為自訂 class
	[, action]            #	
	[, option_strings]    #
	[, dest]              #	可以用來表示哪一個子命令被呼叫
	[, required]          #
	[, help]              #
	[, metavar]           #
)

ArgumentParser = argparse.action.add_parser (
	...                   #	應該接受所有建立 ArgumentParser 的參數
	aliases=[]            #	定義子命令可以接受的所有 key words
)    
```

這個架構，就像是下圖。當處理命令列參數時，父 (根) parser 會先處理命令列參數。如果有找到子 parser 對應的命令，就會交由子 parser 處理；應該子命令就像是一個[互斥群組]()，只會有一個子 parser 處理對應的命令列。

```
ArgumentParser /--
                |
				|- 變數    #	父 Parser 處理的命令列參數
				|
				|- argparse.action /--    <-- 來自於 ArgumentParser.add_subparser()
				                    |
									|- ArgumentParser /--         <-- 來自於 argparser.action.add_parser()
									                   |
												       |- 變數    #	子 Parser 處理的命令列參數
```

但是事實上，父 (根) parser 下可以同時存在多個子 parser，只是同一個命令列只能有一個子 parser 幫忙處理。例外是 --help 參數，當列印 說明文件時，如果沒有指定子 parser 的命令，就會印出父 parser 下所有子 parser 支援的命令。


### 檔案物件


### 變數的 Group 

在 ArgumentParser 中預設變數為兩個 group，一個是 Optional 變數，另一個是 Positional 變數。這種客製化的機制也開放給使用者，即為下面的 API：

``` python3
group = ArgumentParser.add_argument_group (
	title = None,         #	變數 group 的名稱
	description = None    #	變數 group 的說明
)
```

我們可以使用回傳的 group 物件，進行客製化 parse 的動作，將不同的變數歸類到不同的群組，方便管理。

``` python3
>>> parser = argparse.ArgumentParser(prog='PROG', add_help=False)

>>> group1 = parser.add_argument_group('group1', 'group1 description')
>>> group1.add_argument('foo', help='foo help')

>>> group2 = parser.add_argument_group('group2', 'group2 description')
>>> group2.add_argument('--bar', help='bar help')

>>> parser.print_help()
usage: PROG [--bar BAR] foo

group1:
  group1 description

  foo    foo help

group2:
  group2 description

  --bar BAR  bar help
```


#### 互斥群組

有些變數可能造成互斥的狀態，因此 ArgumentParser 提供了建立互斥群組的 API。任何在互斥群組中的變數，只能有一個在命令列中出現。以下為其 API：

``` python3
group = ArgumentParser.add_mutually_exclusive_group (
		required = False    #	群組中的變數是否一定要有一個出現在命令列中
)

>>> parser = argparse.ArgumentParser(prog='PROG')
>>> group = parser.add_argument_group('Group title', 'Group description')
>>> exclusive_group = group.add_mutually_exclusive_group(required=True)    #    亦可以在變數群組中加上子群組
>>> exclusive_group.add_argument('--foo', help='foo help')
>>> exclusive_group.add_argument('--bar', help='bar help')
>>> parser.print_help()
usage: PROG [-h] (--foo FOO | --bar BAR)

options:
  -h, --help  show this help message and exit

Group title:
  Group description

  --foo FOO   foo help
  --bar BAR   bar help 
```


### Parser 的預設值

我們可以在[客製化 parse 規則]() 中制訂某些變數的預設值，即便在命令列中沒有找到對應的參數，該變數還是有預設值可以使用。

ArgumentParser 提供了另一個 API 讓我們能一次制定多個變數的預設值

``` python3
ArgumentParser.set_defaults( **kwargs )

>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('foo', type=int)
>>> parser.set_defaults(bar=42, baz='badger')    #	定義多個變數的預設值
>>> parser.parse_args(['736'])
Namespace(bar=42, baz='badger', foo=736)
```


### 混用式命令列參數

有些 Unix 命令允許使用者混用 Optional 變數與 Positional 變數。簡單的說，只要符合 parse 規則，即便命令列參數不會因為 Optional 變數打斷，而將命令列參數分給多個 positional 變數。

對於這種命令列，ArgumentParser 提供兩組名稱中帶有 **intermixed** 字樣的 API 處理：

``` python3
argparse.Namespace = argparse.ArgumentParser.parse_intermixed_args(
	args = None,
	namespace = None
)

argparse.Namespace = argparse.ArgumentParser.parse_known_args(
	args = None,
	namespace = None
)

argparse.Namespace = argparse.ArgumentParser.parse_known_intermixed_args(
	args = None,
	namespace = None
)
```

這兩個 APIs 並未支援前述 ArgumentParser 的所有功能；當使用者使用這兩個 APIs， 可能會因為使用到未支援的功能而跳出例外。基本上，[子命令]() 與 [互斥]() 兩項功能不論是在 Optional 變數或是 Positional 變數上，都不支援

另外，parse_Known_args() 或是 parse_known_intermixed_args() 函式回傳的 argparse.Namespace 會包含兩個 namespace，第二個 namespace 將存放 **未定義** 變數。而 parse_intermixed_args() 與 parse_known_intermixed_args() 函數會將被 optional 變數打斷的 positional 變數 '2' 跟 '3' 歸類到 positinal 變數 rest 中

``` python3
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument( '--foo' )
>>> parser.add_argument( 'cmd' )
>>> parser.add_argument( 'rest', nargs='*', type=int )

#	命令列參數 (2 跟 3) 與 1 不屬於同一個變數，是因為兩者中間隔了一個 optional 變數，且這個 API 沒有包含 **_intermixed_** 字樣
#	因為本 API 包含 **_known_** 字樣，故返回的 namespace 有兩組；而為定義的命令列參數 2 跟 3 就被放置在第二個 namespace 中
>>> parser.parse_known_args('doit 1 --foo bar 2 3'.split())
(Namespace(cmd='doit', foo='bar', rest=[1]), ['2', '3'])

#	這邊命令列參數 (2 跟 3) 與 1 都被歸類為變數 rest 的值，不受到中間有 optional 變數影響。因為這個 API 有包含 **_intermixed_** 字樣
#	因為 API 並未包含 _known_ 字串，故返回的 namespace 只有一個
>>> parser.parse_intermixed_args('doit 1 --foo bar 2 3'.split())
Namespace(cmd='doit', foo='bar', rest=[1, 2, 3])

#	這邊命令列參數 (2 跟 3) 與 1 都被歸類為變數 rest 的值，不受到中間有 optional 變數影響。因為這個 API 有包含 **_intermixed_** 字樣
#	因為本 API 包含 **_known_** 字樣，故返回的 namespace 有兩組；第二個 namespace 將放置為定義的命令列參數
>>> parser.parse_known_intermixed_args('doit 1 --foo bar 2 3'.split())
(Namespace(cmd='doit', foo='bar', rest=[1, 2, 3]), [])
```



