D-Bus 是 [freedesktop.org](https://www.freedesktop.org/) 旗下的專案之一，主要的功能是作為 IPC (Inter-Process Communication)，讓多個程式彼此進行資料交流。與其他 IPC 不同，D-Bus 像是一個獨立的 Service；這個設計讓程式可以不必使用多個 thread 等方式交換訊息，可以讓相關程式的設計更為簡潔

D-Bus 本身分為兩大部分：其中一個是屬於系統的 Daemon，又稱為 system bus；其生命週期與系統等長，主要是用來處理系統相關的運作，像是新硬體的加入等。另一個是屬於使用者的 Daemon，稱為 user bus 或是 session bus；其生命始於使用者登入，結束於使用者登出。當初 user bus 是供使用者桌面環境處理使用者輸入

D-Bus 本身支援 socket，除了本身機器之外，也可以透過 TCP 與其他機器上的 service 互動。但是因為 D-Bus 本身不支援加解密，跨機器互動最好要想考慮到資訊的安全等

以下章節將對 D-Bus 有更詳細的說明

- [01. Introduction](docs/computer/IPC/D-Bus/Introduction.md) - 說明 D-Bus 的核心用語，包含 Object Model、Signature 等
    - [01-1. Common Interfaces](Common_Interfaces.md) - 說明每個 D-Bus `Object` 必定包含的 `Interfaces` 與相關 `Method`

- [Appendix A Tools](Tools.md) - 目前在 Linux 上常見的 D-Bus 工具與如何使用
- [Appendix B. Low_Level_API](Low_Level_API.md) - 開發 D-Bus 使用的 C Library (libdbus)
    - [B-1. sd-bus](sd-bus.md) - 比較新的 D-Bus library，是 systemd 開發者建立

