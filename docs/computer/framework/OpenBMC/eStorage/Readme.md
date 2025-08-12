eStoraged 是 OpenBMC 上的一項服務，用來處理加密的檔案裝置。由於這個服務是在 BMC 啟動之後才會被執行，故無法用於存放 firmware 的檔案裝置。

本服務使用 cryptsetup 工具，透過 kernel 的 dm-crypt 子系統，提供對存儲裝置進行加密與 mapping 的功能。而 cryptsetup 工具則提供 LUKS ([Linux Unified Key Setup](https://en.wikipedia.org/wiki/Linux_Unified_Key_Setup))的能力，提供密碼管理等，再變更密碼之後，也不需要重新加密裝置。

本服務主要用於 emmc 裝置，並提供 

# 設計

## 服務介面

| D-Bus   |                               |
| :------ | ----------------------------- |
| Service | xyz.openbmc_project.eStoraged |

本服務的 Object Path 是透過查詢另一個服務而建立的，經由透過 ObjectMapper 服務，找尋所有 EmmcDevice 的介面。以下為範例
``` D-Bus
busctl call xyz.openbmc_project.ObjectMapper /xyz/openbmc_project/object_mapper xyz.openbmc_project.ObjectMapper GetSubTree sias "/" 0 1 "xyz.openbmc_project.Configuration.EmmcDevice"

a{sa{sas}} 1 "/xyz/openbmc_project/inventory/system/board/AgoraV3AD/agora_emmc"
           1 "xyz.openbmc_project.EntityManager"
           1 "xyz.openbmc_project.Configuration.EmmcDevice"
```

我們拿到資料型態是 a{sa{sas}}，第一個 “s” 是 Object Path，其後的兩個 ”s“ 分別是 Service 與查詢的裝置名稱；由於回覆的型別是 Array 中包著另一個 Array，因此我們預期將收到多筆資訊。上例我們找到在 EntityManager 這個服務中，有一個 emmc 裝置。

一旦找到 EmmcDevice 的資訊之后，本服務將逐一查詢相關服務，並透過 Intrerface org.freedesktop.DBus.Properties 下的 Method “GetAll” 已取得 emmc 裝置的資訊。我們試著從 EntityManager 下取得 emmc 的資料：

``` D-Bus
busctl call xyz.openbmc_project.EntityManager /xyz/openbmc_project/inventory/system/board/AgoraV3AD/agora_emmc org.freedesktop.DBus.Properties GetAll s 'xyz.openbmc_project.Configuration.EmmcDevice'

a{sv} 4 "EraseMaxGeometry" t 15634268169
        "EraseMinGeometry" t 9380560896
        "Name" s "agora_emmc"
        "Type" s "EmmcDevice"
```

``` plantuml
state Init {
  state Start
  Start: Creating estoraged::Estoraged Object
  
  state GetStorageConfiguration
  GetStorageConfiguration: Creating estoraged::GetStorageConfiguration Object

  state Searching_EMMC_Interface
  Searching_EMMC_Interface: Checking data via D-Bus service \n"xyz.openbmc_project.ObjectMapper" on path \n"xyz/openbmc_project/object_mapper" with interface \n"xyz.openbmc_project.ObjectMapper", and calling method \n"GetSubTree" with parameter \n"/" 0 1 "xyz.openbmc_project.Configuration.EmmcDevice"

  Start --> GetStorageConfiguration
  GetStorageConfiguration --> Searching_EMMC_Interface
}
```

## 目標

本服務預計提供以下服務：

- 在存儲裝置上建立 LUKS 加密檔案系統
- 能秘密清除裝置上的資料，並確保資料會被清除
- 鎖住與釋放存儲裝置
- 提供修改密碼的功能

