
# 常用名詞

| 名詞        | 說明                                |
| :-------- | :-------------------------------- |
| `Entity`  | 可以偵測，且可任意加入或是移除自 BMC 系統的硬體        |
| `Exposes` | `Entity` 提供的功能                    |
| `Probe`   | 提供 `Entity` 是否存在的方法，主要是 D-Bus 的介面 |

# 相關功能

從常見名詞可見，entiry manager 有 3 個主要的功能，包含了偵測、管理、與跟硬體互動。
## 偵測

Entity Manager 不包含偵測硬體的功能。透過其他程式建立 D-Bus，觸發 Entity Manager 知道某裝置存在與否。目前有 3 種主要的 Packages 在處理偵測硬體的動作：

- i2c
- peci-pcie - 透過跟 Intel Processor 互動，取得 PCIe 裝置
- smbios-mdr - 從 BIOS 提供的 SMBIOS 資訊，建立系統硬體的訊息

## 管理

在 entity-manager 的 repository 下的 configurations 目錄，有許多的裝置設定檔。裝置設定檔是一個 JSON 型態的檔案；每個 json object 都表示一個裝置，且提供一個 `Probe` 關鍵字說明如何偵測該裝置是否存在 (大多數是使用 D-Bus)。

Entity Manager 會透過檢查 D-Bus，與各裝置設定檔上的 `Probe` 關鍵字進行比對。如果比對成功，表示該裝置存在；Entity Manager 接著會將該裝置 `Exposes` 的資訊，建立在 D0Bus 上。

## 互動

Entity Manager 不會有與硬體互動的程式碼。相反的，Entity Manager 透過建立與移除 D-Bus 的資訊，提供使用者與其他程式跟管理硬體的 Daemon 互動。

除此之外，Entity Manager 也提供了介面，供其他程式取得或是建立硬體資訊。

-  `bmcweb` - 透過 Entity Manager 取得 Inventory 的資訊，用以建立 Redfish 相關的 API
-  `intel-ipmi-oem` - 提供 Intel 環境下的 SDR、FRU、與 Storage 資訊