Entity Manager 將會讀取所有相關的 JSON 檔案，並將對應硬體的資訊呈現在 D-Bus 上。這些 JSON 檔案並不會至於專案中，而是統一存放在 Entity Manager 的 repository 下 [configurations 目錄](https://github.com/openbmc/entity-manager/tree/master/configurations)。OpenBMC 提供了對應的 Schema 檔案，用於驗證各 configuration files 是否合法。

為了符合 OpenBMC 程式碼的 guidelines，Entity Manager 的 Configuration 與 Schema 檔案的名稱將會是 lower_snake_case 形式。

> [!NOTE] 
> lower_snake_case 形式：每一個單字的起始字母為小寫 (或是數字)，每個單字間使用底線 ("\_") 相連

# Schema 檔案

Entity Manager 定義了 JSON 檔案的結構，可以在 Entity Manager 的 repository 下 [schema 目錄](https://github.com/openbmc/entity-manager/tree/master/schemas)找到完整的資訊。 我們以 [global.json 檔案](https://github.com/openbmc/entity-manager/blob/master/schemas/global.json)來說明
![[./figures/{D1CA4811-2A8A-4BCF-967C-1A5B8128DCF3}.png]]

一個正確的 configuration 檔案，必須包含一個 EMConfig 或是一個包含多個 EMConfig 的 JSON Array。

## EMConfig

這是 Entity Manager 使用的 Configuration 檔案核心之一，用來說明某個硬體。必須包含四個屬性
![[./figures/{B6AFDD18-3059-4B69-A5BD-D2C9086B4020}.png]]

| 屬性        | 說明                                                                                         |
| :-------- | :----------------------------------------------------------------------------------------- |
| `Type`    | 目前支援 5 種型態："Board"、"Chassis"、"NVMe"、"PowerSupply"、與 "CPU"                                  |
| `Name`    | 該 EMConfig 的名稱                                                                             |
| `Probe`   | Entity Manager 會在 D-Bus 上搜尋符合此字串或是字串陣列 D-Bus Property<br>一旦相符的 D-Bus Property 有符合的值，表示硬體存在 |
| `Exposes` | 一個包含多個 EMExposeElement 的陣列                                                                 |

> [!IMPORTANT]
> 其中 `Type` 與 `Name` 兩個屬性會被 Entity Manager 用於建立對應的 D-Bus Object
> /xyz/openbmc_project/inventory/system/\${Type}/\${Name}

除了這 4 個主要參數，其他跟 OpenBMC 相關的屬性則依需求自行決定使否需要建立

## EMExposeElement 

這是 Entity Manager 使用的 Configuration 檔案核心之一，用來說明某個硬體提供的存取功能
![[./figures/{66B94C5C-D54F-47F7-B41C-D2925F116AE2}.png]]

# D-Bus

以下為使用 Entity Manager 的服務，所應該使用的 D-Bus 規範
## Objects

對於與硬體溝通的服務，其 D-Bus 介面

> `/xyz/openbmc_project/Inventory/Item/{Entity Type}/{Entity Name}`

從上述可見，搭配 Entity Manager 的服務會依照 {Entity Type} 歸類為不同的形態，
## Interfaces




# Example

我們使用 meta-ampere 下的 mtjade 為範例，其 Entity Manager 的 Configuration 檔案[在此](https://github.com/openbmc/entity-manager/blob/master/configurations/mtjade.json)。其中包含兩個 EMConfig，我們就已 `Name` 為 "Mt.Jade" 的 Object 為主。

