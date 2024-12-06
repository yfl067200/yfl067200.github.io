
在 [Definitions](01-Definitions.md) 一節中，有簡略的說明 Entity Manager 是如何與其他 Packages 互動，建立完整的硬體 topology。在本節中，將說明如何在 Entity Manager 中建立對應硬體的 Configuration 檔案

# Configuration 檔案

在 Entity Manager 的 Repository 中有一個名為 Configuration 的目錄，裡面有許多硬體設定檔。

當專案有某一個硬體，你需要建立一個 Daemon 與該硬體互動，透過 D-Bus 介面供使用者或是其他程式與之互動。而 Entity Manager 就是需要知道該 Daemon 建立的 D-Bus 介面，才能達到偵測的功能；當 Entity Manager 知道硬體存在或是移除，就必須建立或是移除對應該 Daemon 的 D-Bus 介面，讓使用者或是其他程式與該 Daemon 互動。

## Schema



