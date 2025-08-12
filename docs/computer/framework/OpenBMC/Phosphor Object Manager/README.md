本服務存在的目的就是作為一個 Hub，讓其他 OpenBMC 的服務方便找到所需要的資料。

# 協尋 D-Bus 相關訊息

透過下方的 D-Bus 介面，可以獲得其他服務的資訊。前提是，該服務有透過 Object Manager 註冊自己的資訊。

| 項目             | 值                                  |
| :------------- | ---------------------------------- |
| 服務 (Service)   | xyz.openbmc_project.ObjectMapper   |
| 路徑 (Path)      | /xyz/openbmc_project/object_mapper |
| 介面 (Interface) | xyz.openbmc_project.ObjectMapper   |

## 函式

本服務提供以下函式，讓其他程序可以查詢到所尋找的 D-Bus 相關資料

| 函式 | 功能 | 參數 | 回傳值
|:-----|:-----|:-----|:------
| GetAncestors |
| GetAssociatedSubPath |
| GetAssociatedSubTreePaths |
| GetObject | 列出提供相同 Path 與 Interface 的服務 | sas <br>第一個 string 為 Path，而後面的 array of string 是 Interface (Optional) | a{sas} <br>第一個 string 為 Service，後面的 array of string 是該 Path 下的 Interface |
| GetSubTree |
| GetSubTreePaths |

### GetAncestors

本函式的功能在於列出 Path 上，往前尋找符合 Interface 名稱的 Service。回傳值不包含完整的 Path 

- 輸入參數如下，型態為 sas
    - Path 名稱，型態為 string
    - Interface 名稱，此項目為 Optional，型態為 array of string
- 輸出資料如下，型態為 a{sa{sas}}
    - Path 名稱集合，型態為 array of string 
    - Service 名稱集合，型態為 array of string
    - Interface 名稱集合，型態為 array of string

#### 範例

譬如說，我們想知道從 Path "/xyz/openbmc_project/sensors/voltage/BMC_P12V_STBY" 往上找，有提供 Interface 為 "org.freedesktop.DBus.ObjectManager" 的 Service

```bash
dbus-send --system --print-reply \
--dest=xyz.openbmc_project.ObjectMapper \
/xyz/openbmc_project/object_mapper \
xyz.openbmc_project.ObjectMapper.GetAncestors \
string:"/xyz/openbmc_project/sensors/voltage/BMC_P12V_STBY" \
array:string:"org.freedesktop.DBus.ObjectManager"



```


### GetAssociatedSubTree


### GetAssociatedSubTreePaths


### GetObject

本函式的功能在於列出擁有符合 Path 名稱的 Service，與該 Path 下所有的 Interface

- 輸入參數如下，型態為 sas
    - Path 名稱，型態為 string
    - Interface 名稱，此項目為 Optional，型態為 array of string
- 輸出資料如下，型態為 a{sas}
    - Service 名稱集合，型態為 array of string
    - Interface 名稱集合，型態為 array of string

#### 範例

譬如說，我們想知道哪一個服務有一個 path 為 "/xyz/openbmc_project/sensors/voltage/BMC_P12V_STBY"

``` bash
dbus-send --system --print-reply \
--dest=xyz.openbmc_project.ObjectMapper \
/xyz/openbmc_project/object_mapper \
xyz.openbmc_project.ObjectMapper.GetObject \
string:"/xyz/openbmc_project/sensors/voltage/BMC_P12V_STBY" array:string:

   array [
      dict entry(
         string "xyz.openbmc_project.ObjectMapper"
         array [
            string "org.freedesktop.DBus.Introspectable"
            string "org.freedesktop.DBus.Peer"
            string "org.freedesktop.DBus.Properties"
         ]
      )
      dict entry(
         string "xyz.openbmc_project.PSUSensor"
         array [
            string "xyz.openbmc_project.Association.Definitions"
            string "xyz.openbmc_project.Sensor.Threshold.Critical"
            string "xyz.openbmc_project.Sensor.Threshold.Warning"
            string "xyz.openbmc_project.Sensor.Value"
            string "xyz.openbmc_project.State.Decorator.Availability"
            string "xyz.openbmc_project.State.Decorator.OperationalStatus"
         ]
      )
   ]

busctl call xyzopenbmc_project.ObjectMapper \
/xyz/openbmc_project/object_mapper \ 
xyz.openbmc_project.ObjectMapper \
GetObject sas "/xyz/openbmc_project/sensors/voltage/BMC_P12V_STBY" 0

a{sas} 2 "xyz.openbmc_project.ObjectMapper"
         3 "org.freedesktop.DBus.Introspectable"
           "org.freedesktop.DBus.Peer"
           "org.freedesktop.DBus.Properties"
         "xyz.openbmc_project.PSUSensor"
         6 "xyz.openbmc_project.Association.Definitions"
           "xyz.openbmc_project.Sensor.Threshold.Critical"
           "xyz.openbmc_project.Sensor.Threshold.Warning"
           "xyz.openbmc_project.Sensor.Value"
           "xyz.openbmc_project.State.Decorator.Availability"
           "xyz.openbmc_project.State.Decorator.OperationalStatus"

```

如同以上條件，加上 Interface 名稱為 "xyz.openbmc_project.Sensor.Value"
``` bash
dbus-send --system --print-reply \
--dest=xyz.openbmc_project.ObjectMapper \
/xyz/openbmc_project/object_mapper \
xyz.openbmc_project.ObjectMapper.GetObject \
string:"/xyz/openbmc_project/sensors/voltage/BMC_P12V_STBY" \
array:string: "xyz.openbmc_project.Sensor.Value"

   array [
      dict entry(
         string "xyz.openbmc_project.PSUSensor"
         array [
            string "xyz.openbmc_project.Association.Definitions"
            string "xyz.openbmc_project.Sensor.Threshold.Critical"
            string "xyz.openbmc_project.Sensor.Threshold.Warning"
            string "xyz.openbmc_project.Sensor.Value"
            string "xyz.openbmc_project.State.Decorator.Availability"
            string "xyz.openbmc_project.State.Decorator.OperationalStatus"
         ]
      )
   ]

busctl call xyzopenbmc_project.ObjectMapper \
/xyz/openbmc_project/object_mapper \ 
xyz.openbmc_project.ObjectMapper \
GetObject sas "/xyz/openbmc_project/sensors/voltage/BMC_P12V_STBY" \
              1 "xyz.openbmc_project.Sensor.Value"

a{sas} 1 "xyz.openbmc_project.PSUSensor"
         6 "xyz.openbmc_project.Association.Definitions"
           "xyz.openbmc_project.Sensor.Threshold.Critical"
           "xyz.openbmc_project.Sensor.Threshold.Warning"
           "xyz.openbmc_project.Sensor.Value"
           "xyz.openbmc_project.State.Decorator.Availability"
           "xyz.openbmc_project.State.Decorator.OperationalStatus"

```

### GetSubTree

本服務類似 GetObject，差別在於輸入的參數為 Interface 的名稱，而非 Path 的名稱

- 輸入參數如下，型態為 sias
    - SubTree (Object) 的起始位置，型態為 string。如果使用 "/" 則會從頭開始尋找
    - Deepth，型態為 int32。用此數字限制尋找的深度，從 SubTree 起始位置開始算
        - 使用 0 表示沒有限制
    - Interface 名稱列表，型態為 array of string。
        - 只要某一個 Interface 符合，即會返回對應的 D-Bus 資訊
        - 返回值不會包含重複的 D-Bus 資訊
        - 賦予空集合，即會返回 SubTree 以下的所有資訊
- 輸出資料如下，型態為 a{sa{sas}}
    - Path 名稱集合，型態為 array of string
    - Service 名稱集合，型態為 array of string
    - Interface 名稱集合，型態為 array of string
        - Service 名稱集合與 Interface 名稱集合等同於 GetObject 的返回值
#### 範例

譬如說，我們想知道在 path 為 "/xyz/openbmc_project/sensors/current" 底下，有哪些 Service 提供 Interface 名為 "xyz.openbmc_project.Sensor.Threshold.Warning"。以下兩命令將得到相同的結果

``` Bash
dbus-send --system --print-reply \
--dest=xyz.openbmc_project.ObjectMapper \
/xyz/openbmc_project/object_mapper \
xyz.openbmc_project.ObjectMapper.GetSubTree \
string:"/xyz/openbmc_project/sensors/current" int32:0 \
array:string:"xyz.openbmc_project.Sensor.Threshold.Warning"


busctl call xyzopenbmc_project.ObjectMapper \
/xyz/openbmc_project/object_mapper \ 
xyz.openbmc_project.ObjectMapper \
GetSubTree sias "/xyz/openbmc_project/sensors/current" 0 \
                1 "xyz.openbmc_project.Sensor.Threshold.Warning"
```

### GetSubTreePaths

本服務類似 GetSubTree，但是只回覆 Path 名稱

- 輸入參數如下，型態為 sias
    - SubTree (Object) 的起始位置，型態為 string。如果使用 "/" 則會從頭開始尋找
    - Deepth，型態為 int32。用此數字限制尋找的深度，從 SubTree 起始位置開始算
        - 使用 0 表示沒有限制
    - Interface 名稱列表，型態為 array of string。
        - 只要某一個 Interface 符合，即會返回對應的 D-Bus 資訊
        - 返回值不會包含重複的 D-Bus 資訊
        - 賦予空集合，即會返回 SubTree 以下的所有資訊
- 輸出資料如下，型態為 as
    - Path 名稱集合，型態為 array of string
    
# Reference

[官方說明文件](https://github.com/openbmc/docs/blob/master/architecture/object-mapper.md)