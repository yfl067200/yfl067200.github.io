Bitbake 是搭配 OpenEmbedded Project 設計的 [[docs/computer/framework/OpenBMC/readme|OpenEmbedded-Core 架構]]，用來編譯 Linux 環境的工具。Bitbake 本身是使用 Python 開發，可以同時處理多個 python 或是 shell scripts；透過讀取使用者指定的 layers 下的 metadata，bitbake 將會對編譯依照相依性等進行排程。為了能更方便的了解 bitbake 的功能，以下就先對 bitbake 相關的術語 (Terminology) 進行說明

# 術語 (Terminology)

## Layers (即 meta- 目錄)

隨著 OpenEmbedded Project 重新整理 OpenEmbedded-Core 架構，原本的 OpenEmbedded-Class 下的各種工具、Library、或是 Linux kernel 等，都依分類收納到對應的 meta- 目錄下；在這些 meta- 目錄下若是有 conf 子目錄，則這些 meta- 目錄就被稱為 Layer。

客製化專案一定是某一個 Layer，其下的 conf 子目錄至少包含兩個檔案：

- bblayers.conf 與
- local.conf 

其中 bblayers.conf 用來設定 bitbake 在編譯該專案時，需要從那些 Layer 下面取得進一步的 metadata；而 local.conf 則是用來設定 bitbake 在編譯時的相關環境變數

## Metadata

所謂的 metadata 就是 OpenEmbedded-Core 架構中，用來說明編譯時的設定檔與變數。目前的 metadata 可以分為幾種：

| 種類               | 副檔名       | 說明                                                                                                             |
| :--------------- | --------- | -------------------------------------------------------------------------------------------------------------- |
| Recipe 檔案        | .bb       | 1. 本檔案是由 Yocto Project 維護<br>2. 內含下載、補丁、設定、編譯、與包裝等編譯某一元件流程時所用上的動作                                              |
| Recipe Append 檔案 | .bbappend | 1. 用於客製化的專案，由使用者維護<br>2. 內含調整 Recipe 檔案的動作                                                                     |
| Configuration 檔案 | .conf     | 設定編譯相關的變數，包含<br>1. 路徑 (主要存放在 bblayers.conf)<br>2, 檔案 (主要是指 Recipe 或是 Recipe Append 檔案，存放在layers.conf)<br>3. 其他 |
| Class 檔案         | .bbclass  | 1. 本檔案是由 Yocto Project 維護<br>2. 各種 python script，供 bitbake 用來執行編譯的動作                                           |
| Include 檔案       | .inc      |                                                                                                                |

# Metadata

在這邊簡單的說明各種 metadata 檔案

## Recipe (.bb) 檔案與 Recipe Append (.bbappend) 檔案

Recipe 檔案是由 Yocto Project 維護，集中存放在某個特定的目錄；其中的內容為編譯該程式在各階段的動作。當專案需要在編譯流程中進行額外的動作 (打補釘之類)，就由專案的開發人員建立 Recipe Append 檔案；該檔案會放在專案所在的目錄下。不論是 Recipe 檔案或是 Recipe Append 檔案，都集中在 Layer 路徑下的 recipe- 子目錄

通常 Recipe 檔名會包含版本號，譬如說 busybox_1.21.3.bb 或是 busybox_1.21.5.bb 等。當建立 Recipe Append 檔案時，檔名必須要與 Recipe 檔案一致。

可以使用特殊符號 '%' 代表 wild character，但僅限於用在檔案名稱 '.bbappend' 之前。

> [!IMPORTANT]
> 譬如說 busybox_1.21.%.bbappend 檔案就可以通吃 busybox_1.21.3.bb 與 busybox_1.21.5.bb
> 
> 而 busybox_1.%.bbappend 檔案可以通吃 buxybox_1.21.3.bb 與 busybox_1.1.bb 檔案


## Configuration (.conf) 檔案

相較於 Recipe 與 Recipe Append 檔案是針對一個程式，Configuration 檔案是針對一個 Layer 或是一個專案。以專案來說，通常是存放在專案 Layer 下 conf 子目錄內的 bblayers.conf 或是 layer.conf 檔案；前者用來說明編譯該專案時，需要參考那些 Layer 下面的 metadata。而後者是用來指定如何取得相關的 Recipe 或是 Recipe Append 檔案。

其他的 Configuration 檔案，則是用來記錄編譯用到的環境變數，或是用來新增或是移除相關的原件等。

### 多重 Configuration 檔案

Bitbake 亦支援多重目標 Firmware。以下圖為例，該專案有兩種不同的目標；不同的目標多半是因為硬體上的差異。
![[{0EF587CD-99D0-4959-9A6F-E37975DF7A5A}.png]]

為了支援多重 Configuration 檔案，在 local.conf 檔案中必須加上這一行：

> BBMULTICONFIG = "target1 target2"

## Class (.bbclass) 檔案

Class 檔案主要是集中在 poky/meta/classes 與 poky/meta/classes-recipe 兩個目錄，兩者都是提供 Bitbake 編譯時用的函式。

Class 檔案大多是包含了 Bitbake 共用的函式與變數，透過特殊字 `inherit`，可以將 Class 檔案的內容嵌入 Recipe 檔案。

##  Include (.inc) 檔案