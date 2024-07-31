# Introduction

argparse 模組是用來解析 (parse) 命令列參數。本模組的核心物件是 ArgumentParser，本物件提供幾個 API 供使用者進行客製化解析命令列的動作。

# 使用方式

以下為使用 argparse 模駔進行解析命令列的方法；

``` python3
import argparse

#    建立 Parser
parser = argparse.ArgumentParser( ... )

#    客製化 Parsing 規則
parser.add_argument( ... )

#    Parsing 命令列
data = parser.parse_args( ... )
```


## 建立 Parser

ArgumentParser 的 constructor 宣告為
``` python3
class argparse.ArgumentParser(
    prog = None,                                   #	執行的程式名稱，預設是 os.path.basename( sys.argv[ 0 ] )
	usage = None,                                  #	說明如何使用，預設將從客製化規則自動生成
	description = None,                            #	會被置於說明中的一個部分，通常會出現在 `options` 項目之前
	epilog = None,                                 #	會被置於說明中的最後一個部分
	parents = [],                                  #	將其他 ArgumentParser 的設定做為自己的一個部分
	formatter_class = argparse.HelpFormatter,      #
	prefix_chars = '-',                            #
	fromfile_prefix_chars = None,                  #
	argument_default = None,                       #	
	conflict_handler = 'error',                    #
	add_help = True,                               #    是否加上 -h/--help 功能
	allow_abbrev = True,                           #	在命令列中能否使用縮寫
	exit_on_error = True
)
```

基本上不需要額外調整參數，建立的 ArgumentParser 實體就可以正常運作。

### parents

### formatter_class


### prefix_chars


### fromfile_prefix_chars

### conflict_handler

### exit_on_error


## 客製化 Parsing 規則

ArgumentParser 

``` python3
ArgumentParser.add_argument(
	name or flags...			#
	[, action]					#
	[, nargs]					#	在命令列找到對應的字符後，其後 N 個參數為
	[, const]					#
	[, default]					#
	[, type]					#
	[, choices]					#
	[, required]				#
	[, help]					#
	[, metavar]					#
	[, dest]					#
)
```


## 
