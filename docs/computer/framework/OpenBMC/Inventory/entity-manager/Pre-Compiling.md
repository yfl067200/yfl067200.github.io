
Entity Manager 的 Metadata 位於 meta-phosphor/recipes-phosphor/configuration 目錄，其內部的檔案包含：
![[{16A09383-5940-4F0F-9243-E40599CFED9E}.png]]

# Recipe 檔案

## 編譯參數

有一個參數名為 `PACKAGECONFIG`，如果沒有設定該參數，預設值為 "ipmi-fru"

使用變數 Flag 的機制，可以將對應不同機制的編譯參數加入變數 `PACKAGECONFIG` 中。這個參數會在 Entity Manager 的編譯環境中的 meson.build 檔案中被使用，進而使用對應的編譯參數，或是進行不同檔案的編譯