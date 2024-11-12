OpenBMC 是一個提供客製化 BMC 系統的環境。基於 Yocto Project，透過建立自己的 meta- layer，可以客製化基於 Linux 的 BMC 系統。

# Yocto Project

Yocto project 是一個 [Linux Foundation](https://www.linuxfoundation.org/) 下的專案，主要的目標是提供一個名為 poky distribution 的架構，發行 Linux 套件。Poky 是採用類似 [Gentoo Linux](https://www.gentoo.org/) 發行商使用的 Portage 工具，提供使用者自行編譯 Linux 核心與工具套件；而 Poky 使用的架構來自於 [OpenEmbedded Project](https://www.openembedded.org/)，後者於 2011 年 3 月整合進入 Yocto Project；由 OpenEmbedded Project 所開發的 [[OpenEmbedded 架構]]，也成為 Yocto Project 的主幹

> [!NOTE]
> OpenEmbedded Project 起源於將 Linux 移植到 sharp 一個名為 Zaurus 的手持裝置。經過多年的開發之後，OpenEmbedded Project 建立了一個被稱為 OpenEmbedded 的架構，透過自行開發的 bitbake 工具，提供使用者客製化 Linux 的編譯環境。
> 
> 在 2010 年，OpenEmbedded 重新整理了OpenEmbedded 的架構。原本的架構被稱為 OpenEmbedded-Class，新的架構被稱為 OpenEmbedded-Core；新的 OpenEmbedded-Core 支援多個 Layer，提供更容易客製化 Linux 的環境。
> 
> 以下為 OpenEmbedded Project 的 [git tree](https://git.openembedded.org/?s=name)。其中 openembedded 為 OpenEmbedded-Class，而 openembedded-core 為 OpenEmbbed-Core。比對兩者，您可以發現 OpenEmbedded-Core 提供多個 meta- 目錄，類似的工具會被歸類進同一個 meta- 目錄。
> ![[{C0C3A6FA-A30C-49BE-869F-E6C1A96A0983}.png]]

## Layers

OpenBedded Project 除了在 openbedded-core 目錄下有建立 meta- 目錄之外，也允許使用者建立自己的 meta- 目錄。以下為 OpenEmbedded Project 所列出的 [meta- 目錄](https://layers.openembedded.org/layerindex/branch/master/layers/)，絕大多數是針對不同硬體所建立的 meta- 目錄，這表示 OpenEmbedded Project 的建構已經開始支援多平台 (編譯)。
![[{753ED932-13C4-46DC-83E4-ED382B3DA9C4}.png]]

Yocto Project 與 OpenEmbedded Project 整合之後，將 OpenBedded-Core 作為核心，並將主要的 meta- 目錄一併整合進 Poky Distribution。藉由增加 meta- 目錄的方式，其他廠商也將各自的 drivers 、系統核心、或是工具整合進 poky Distribution；而使用者也可以透過加入或是移除 meta- 目錄的方式，客製化自己想要的 poky Distribution。
![[{40CC85BC-AFEB-4E30-84F6-0F982E133E65}.png]]

> [!IMPORTANT]
> 因為 OpenBedded Project 稱這些 meta- 目錄為 layer，故我們之後將以 layer 替代 meta- 目錄。

# OpenEmbedded 架構

在了解  Yocto Project 與 OpenEmbedded Project 的關係之後，要開始對其架構進行更進一步了說明。唯有了解 OpenBedded Framework，才能理解如何客製化 Linux 環境。這邊用一個簡單圖片來說明相關的關係
![[{04704057-531D-4647-82C8-B6579FF7AAC5}.png]]

以下為 OpenEmbedded 架構的核心：

| Item                                        | Description                                                         |
| :------------------------------------------ | ------------------------------------------------------------------- |
| [Bitbake](./OpenEmbedded/bitbake/Readme.md) | OpenEmbedded Project 所開發的編譯工具。搭配 OpenEmbedded 架構，建立可客製化的 Linux 編譯環境 |
# OpenBMC

 藉由 [D-Bus](https://www.freedesktop.org/wiki/Software/dbus/) 作為訊息中心，所建立的 BMC 系統。將相關的東西整合進 meta-phosphor 目錄，搭被 Yocto Project，讓開發人員可以建立客製化的 BMC 系統

以下為 OpenBMC 架構的核心：

| Item  | Description                                 |
| :---- | ------------------------------------------- |
| D-Bus | 由 [Gnome](https://www.gnome.org/) 所開發出的訊息系統 |

