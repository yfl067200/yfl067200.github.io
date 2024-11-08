
節錄自 Yacto Bitbake User Manual (https://docs.yoctoproject.org/bitbake/2.10/bitbake-user-manual/bitbake-user-manual-intro.html)。

# Introduction

Bitbake 是一個可以同步執行多個 shell script 與 python script 的架構，用於編譯 firmware image。將較於 GNU Make，bitbake 的優勢包含：

- 可以透過 metadata 對 bitbake 進行設定，用以改變 bitbake 的行為與最後的結果

  - metadata 包含 recipe (.bb) 檔案、recipe append (.bbappend) 檔案、class (.bbclass) 檔案、configuration (.conf) 檔案、與 include (.inc) 檔案

  - 其中 recipe、recipe append、與 class 檔案是針對單一程式，而 configuration 與 include 檔案是真對整個專案

  - recipe 包含編譯單一程式所需的所有動作，包含下載、patch、編譯、與打包等動作

- Bitbake 是 Client Server 架構

  - Server 將會進行排程，並指派單一 client 進行某個 recipe 中的某一個動作


## Metadata


### Recipe (.bb) 檔案與 Recipe Append (.bbappend) 檔案

Recipe 檔案是由 Yocto 或是 OpenEmbedded 專案維護，集中存放在某個特定的目錄；其中的內容為編譯該程式在各階段的動作。當專案需要在編譯流程中進行額外的動作 (打補釘之類)，就由專案的開發人員建立 Recipe Append 檔案；該檔案會放在專案所在的目錄下。

通常 Recipe 檔名會包含版本號，譬如說 busybox_1.21.3.bb 或是 busybox_1.21.5.bb 等。當建立 Recipe Append 檔案時，檔名必須要與 Recipe 檔案一致。

可以使用特殊符號 '%' 代表 wild character，但僅限於用在檔案名稱 '.bbappend' 之前。

```
譬如說 busybox_1.21.%.bbappend 檔案就可以通吃 busybox_1.21.3.bb 與 busybox_1.21.5.bb

而 busybox_1.%.bbappend 檔案可以通吃 buxybox_1.21.3.bb 與 busybox_1.1.bb 檔案
```

### Configuration (.conf) 檔案

相較於 Recipe 與 Recipe Append 檔案是針對一個程式，Configuration 檔案是針對一個專案。

在專案目錄下有一個 conf 子目錄，裡面主要有兩個檔案：

- bblayers.conf

- local.conf

其中 bblayers.conf 檔案列出 bitbake 在編譯該專案時，需要存取的檔案目錄；而 local.conf 則條列出編譯該專案時使用的變數，包含 MACHINE 與 DISTRO 這類用來說明編譯目標的變數，或是 DL_DIR、SSTATE_DIR、與 TMPDIR 指定編譯專案時使用到的檔案位置。

### bitbake 設定檔

另外，在 poky 目錄下也有很多 conf 子目錄。其中在 poky/meta/conf 目錄下的設定檔就是供 bitbake 使用的 configuration file。

### Class (.bbclass) 檔案與 Include (.include) 檔案

Class 檔案主要是集中在 poky/meta/classes 與 poky/meta/classes-recipe 兩個目錄。在 poky/meta/classes 目錄下，Class 檔案


## Layers

OpenEmbedded Project 開發 Bitbake 的同時，也設計了可以擴充與客製化的 Framework 搭配 Bitbake，這就是同名的 OpenEmbedded Framework。

OpenEmbedded Framework 的架構



# History

Bitbake 是從 Gentoo Linux 發行商使用的套件管理工具 Portage 得到啟發而製作出來的，用於 OpenEmbedded 專案中的編譯核心。而 OpenEmbedded 專案又與 Yocto 專案合作，建立 Poky Linux 的發行方式；而 OpenBMC 則是進一步使用 Yocto 專案的架構，建立了專門開發 BMC 的架構。

## Bitbake 設計重點

1. Cross-Compile 的支援

2. 處理編譯時的相依性

3. 將編譯的流程劃分為不同的動作





