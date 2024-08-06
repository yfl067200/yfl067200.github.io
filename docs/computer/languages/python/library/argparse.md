##### Table of Contents  
- [Introduction]()
    - [使用方式](./argparse.html#%E4%BD%BF%E7%94%A8%E6%96%B9%E5%BC%8F)
- [建立 Parser]() 
    - [parents 參數](./argparse.html#parents)
	- [formatter_class 參數](./argparse.html#formatter_class)
	- [prefix_char 參數](./argparse.html#prefix_chars)
	- [fromfile_prefix_chars](./argparse.html#fromfile_prefix_chars)
	- [conflict_handler](./argparse.html#conflict_handler)
	- [exit_on_error](./argparse.html#exit_on_error)
- [客製化 Parsing 規則]()
    - [Name 與 Flags 參數](./argparse.html#name-%E8%88%87-flags)
    - [action 參數](./argparse.html#action)
    - [nargs 參數](./argparse.html#nargs)
    - [metaver 與 dest 參數](./argparse.html#metavar-%E8%88%87-dest)
- [Parsing 命令列]()
    - [解析的問題]()
    - [Optional 變數]()
	    - [精簡化命令列參數]()
		- [模擬兩可的 optional 變數縮寫]()
	- [解析結果 (argparse.Namespace)](./argparse.html#%E8%A7%A3%E6%9E%90%E7%B5%90%E6%9E%9C-argparsenamespace)
- [進階議題]()
    - [Group 變數]()
	- [客製化 action]()


# Introduction

argparse 模組是用來解析 (parse) 命令列參數。本模組的核心物件是 ArgumentParser，本物件提供幾個 API 供使用者進行客製化解析命令列，方便找出命令列中的 **變數** 與對應的 **值**。

命令列中的參數會被分成兩類 **變數**，一類是 optional 變數，另一類是 positional 變數：

| 變數類別 | 賦予方式 | 說明 |
|:---------|:---------|:-----|
| Optional 變數 | 比對名稱 | 比對命令列參數與變數名稱，相符的參數將只配給 Optional 變數 <br> 變數名稱多以 '-' 或是 '--' 起始。這個 prefix char 可以客製化 <br> 命令列中不需要有這些變數 |
| Positional 變數 | 依照順序 | 命令列中，不屬於 Optional 變數的參數，將依照加入客製化規則的順序指派為 Positinal 變數 <br> 所有 Positional 變數都應該在命令列中出現 |


## 使用方式

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


# 建立 Parser

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


## parents

透過將各 parsers 的實體放進 parents 參數中，即可將各 parsers 建立的規則納為己用。

Parents 的規則可能會有重複 key，導致解析的方式錯亂，需要搭配另一個參數 `conflict_handler` 處理相關的錯誤 


## formatter_class

設定顯示說明 (Help) 資訊的格式。有四種選項；

| 格式 | 說明 |
|:-----|:-----|
| argparse.RawDescriptionHelpFormatter | 預設的顯示方式會自動對 description 與 epilog 的文字進行縮排，使用者無法定義縮排方式。本模式將改用使用者定義的縮排方式 |
| argparse.RawTextHelpFormatter | 如同上面的 argparse.RawDescriptionHelpFormatter，本模式會保留使用者定義的空白等。但是會將多行空白行變成一行 |
| argparse.ArgumentDefaultsHelpFormatter | 類似預設模式，但是會將各變數的預設值列出 |
| argparse.MetavarTypeHelpFormatter | 類似預設模式，但是會將各變數的型態列出 |


## prefix_chars

大部分的 optional 變數都是使用 '-' 開頭，像是 '-h' 或是 '--help'。這個參數可以讓使用者決定變數的開頭字符，每一個字符都是獨立，且不計次數。以下舉例：

``` python3
>>> parser = argparse.ArgumentParser(prog='PROG', prefix_chars='-+')
```

命令列參數開頭為 '-'、'+'、'--'、'++'、或是 '-------' 等，都會被視為 **Optional 變數**；如果後面接的 key 是一樣的 (`-h` 與 `--h`)，會被視為相同的 **opentional 變數**。相反的，只要開頭不是 '-' 與 '+' 的資訊，都不會被視為 **Optional 變數**


## fromfile_prefix_chars

如果命令列參數太長，可以將其變成檔案。透過指定開頭字符，ArgumentParser 會將某些命令參數是為讀取檔案，並進行 parse 的動作。以下範例為讀取 args.txt 檔案，並解析之

``` python3
>>> parser = argparse.ArgumentParser(fromfile_prefix_chars='@')
>>> parser.add_argument('-f')
>>> parser.parse_args(['-f', 'foo', '@args.txt'])
```

**預設值為 None，表示不會讀取檔案**


## conflict_handler

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


## exit_on_error

如果解析的命令列有問題，預設將會中止程式 (exit_on_error = True)。

將此參數設為 False，將會跳出 argparse.ArgumentError 的例外。可供開發人員進行例外處理


# 客製化 Parsing 規則

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


## name 與 flags

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


## action

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


## nargs

在命令列中找到對應的 key 之後，其後的連續幾個參數都將被視為該變數的值。沒有指定 nargs 參數，不論是 `optional 變數` 或是 `positional 變數` 預設將命令列中下一個參數作為其變數的值。

nargs 後面接的值除了數字之外，還接受另外 3 種字符，分別是

- `'?'`：因為 parser 預期將下一個命令列參數視為值。假若無法將下一個參數作為值，包含沒有下一個參數或是下一個參數是另一個 opetional 參數的字符，ArgumentParser 會將 `const` 參數指定的值視為該 optional 變數的值 (不包含 positional 變數)；如果連 `const` 參數也沒指定，那就會出現 parse 錯誤

- `'*'`：當參數可以接受多個值，parser 會將變數之後的命令列參數都是為該變數的值，直到符合另一個符合變數的字符

- `'+'`：同 `'+'`，唯一的差異是命令列中至少要有一個值

使用上有個條件：Positional 變數無法指定多個值，除非先在 optional 變數中使用 nargs 參數


## metavar 與 dest

本函式的第一個參數 Name 與 Flags 會作為變數的名稱，詳細規則請參考第一個參數的說明。如果想要客製化變數名稱，可以使用 `dest` 參數。使用 `dest` 參數將會改變在 `argparse.Namespace` 物件中，變數的名稱

與 `dest` 參數不同，`metavar` 參數是用來表示變數值。用於說明文字中，幫助使用者提供易於理解的說明


# Parsing 命令列

當客製化 Parsing 的動作都完成之後，即可使用 ArgumentParser 物件進行命令列的解析。解析的函式 API 為

``` python3
argparse.Namespace = argparse.ArgumentParser.parse_args(
    args = None             #    需要解析的字串，預設來自於 sys.argv 變數
	namespace = None        #    解析完的資料所存放的
)
```


## 解析的問題

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


## Optional 變數

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


### 模擬兩可的 optional 變數縮寫

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


## 解析結果 (argparser.Namespace)

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


# 進階議題

## 階級化的命令

目前的趨勢是同一個命令下面有子命令，每個子命令又有各自的命令參數。像是 git 命令，他的子命令包含 `git clone`、`git pull` 等。為了處理這種狀況，argparse 模組提供了加入子 Parser 的功能。

``` python3
argparse.action = ArgumentParser.add_subparsers (
	[title]               #	子 Parser 群組的名稱。
	[, description]       #
	[, prog]              #
	[, parser_class]      #
	[, action]            #
	[, option_strings]    #
	[, dest]              #
	[, required]          #
	[, help]              #
	[, metavar]           #
)
```

## Group


## 客製化 Action



