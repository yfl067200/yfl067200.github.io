# Introduction


## 安裝

在 Linux 環境下，可以透過套件管理工具安裝 Python；常見的就是 Debian 家族的 apt 或是 redhat 家族的 dnf。而在 Windows 下，可以透過 Microsoft Store 安裝 Python。理論上透過上述方式，就可以直接執行 python。

### 環境變數

| 名稱 | 說明 |
|:-----|:-----|
| PYTHONHOME | Python 用來尋找 library 的路徑，預設是 ${prefix}/lib/python${version}，${prefix} 多半是 /usr/local |
| PYTHONPATH | Python 用來尋找 module 檔案的路徑。預設同 PYTHONHOME 變數 </p> 透過分隔號，可以包含多個路徑 |

### 路徑

在 Linux 環境下，應該可以在 /usr/bin 路徑或是 /usr/local/bin 路徑下找到 python 這個檔案；雖然說 python 可能是 link 到另外一個實體檔案的捷徑。可以透過在 /usr/bin 或是 /usr/local/bin 路徑下建立 python{X}.{Y} 這種捷徑，鏈結到其他版本的 Python 執行檔，方便執行不同版本的 python。

而 Windows 環境下，可能需要自行將 Python 的路徑加入 PATH 環境參數；比較建議建立一個空的目錄，將該目錄加入 PATH 環境變數。如此一來就可以在該目錄下，透過建立捷徑鏈結到不同版本的 python 檔。

## Interpreter

安裝完 Python 之後，就可以執行命令 python 進入 python interpreter 中進行簡易的 python 程式碼測試。

離開 python interpreter 的命令是 *quit()*

## Virtual Machine



