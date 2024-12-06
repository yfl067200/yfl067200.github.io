Bitbake 是 OpenEmbedded Project 所開發，一個可以同步執行多個 shell script 與 python script 的架構，用於編譯 firmware image。將較於 GNU Make，bitbake 的優勢包含：

- 可以透過 metadata 對 bitbake 進行設定，用以改變 bitbake 的行為與最後的結果

  - metadata 包含 recipe (.bb) 檔案、recipe append (.bbappend) 檔案、class (.bbclass) 檔案、configuration (.conf) 檔案、與 include (.inc) 檔案

  - 其中 recipe、recipe append、與 class 檔案是針對單一程式，而 configuration 與 include 檔案是真對整個專案

  - recipe 包含編譯單一程式所需的所有動作，包含下載、patch、編譯、與打包等動作

- Bitbake 是 Client Server 架構

  - Server 將會進行排程，並指派單一 client 進行某個 recipe 中的某一個動作
# Bitbake User Manual

This came from [bitbake 2.10 user manual](https://docs.yoctoproject.org/bitbake/2.10/bitbake-user-manual/)

- [01. Overview](01-Overview.md) - 簡易的說明與術語解釋
- [02. 專案](02-Projects.md) - 如何編譯專案與如何增加支援的專案
- [03. 建立新專案](03-New_Projects.md) - 如何建立新的專案

# Bitbake 進階議題

透過 Bitbake User Manual，我們知道 Bitbake 與 OpenEmbedded-Core 的關係，以及如何使用他們建立或是製作專案。在這，我們進一步理解 Bitbake 如何分析 Metadata，以及從 Metadata 中獲得的變數將如何改變 Bitbake 的行為

- [04. 語法-變數類](04-Syntax-Variables.md) - 在 Bitbake 中如何處理變數
- [05. 語法-函式類](05-Syntax-Functions.md) - 在 Bitbake 中如何處理變數

- [Appendix A. Bitbake 命令參數](A-Bitbake_Commands.md) - Bitbake 命令使用的參數
- [Appendix B. 工作管理](B-Task-Management.md) - Bitbake 如何管理工作
- [Appendix C. 變數說明](C_1-Variables.md) - 說明 Bitbake 如何使用變數
    - [Appendix C-1. 變數列表](C.1-Variables-Glossary.md) - Bitbake 使用的變數列表，依照名稱排序
