Bitbake 是 OpenEmbedded Project 所開發，一個可以同步執行多個 shell script 與 python script 的架構，用於編譯 firmware image。將較於 GNU Make，bitbake 的優勢包含：

- 可以透過 metadata 對 bitbake 進行設定，用以改變 bitbake 的行為與最後的結果

  - metadata 包含 recipe (.bb) 檔案、recipe append (.bbappend) 檔案、class (.bbclass) 檔案、configuration (.conf) 檔案、與 include (.inc) 檔案

  - 其中 recipe、recipe append、與 class 檔案是針對單一程式，而 configuration 與 include 檔案是真對整個專案

  - recipe 包含編譯單一程式所需的所有動作，包含下載、patch、編譯、與打包等動作

- Bitbake 是 Client Server 架構

  - Server 將會進行排程，並指派單一 client 進行某個 recipe 中的某一個動作
# Bitbake User Manual

This came from [bitbake 2.10 user manual](https://docs.yoctoproject.org/bitbake/2.10/bitbake-user-manual/)

- [01. Overview](./01-Overview.md)
- [02. 命令](02-Command.md)
