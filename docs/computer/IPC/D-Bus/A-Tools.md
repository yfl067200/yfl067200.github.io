目前在 Linux 上最常見的 D-Bus 工具包含 dbus-send 與 busctl

# busctl

這是目前最常被使用的工具，其相關語法為

```
busctl [OPTIONS] [COMMAND]

COMMAND:
    list                  # List all ${BUS}
    status ${BUS}
    monitor ${BUS}        # 
    capture ${BUS}
    tree ${BUS}           # List all OBJECT for ${BUS}

	# List all ${INTERFACE} for ${OBJECT} on ${BUS}
    introspect ${BUS} ${OBJECT} [${INTERFACE}]

    # Calling `Method` under ${INTERFACE} for ${OBJECT} on ${BUS}
    call ${BUS} ${OBJECT} ${INTERFACE} ${METHOD} [${SIGNATURE} ${VALUE}]

    # Accessing `Property` under ${INTERFACE} for ${OBJECT} on ${BUS}
    get-property ${BUS} ${OBJECT} ${INTERFACE} ${PROPERTY}
    set-property ${BUS} ${OBJECT} ${INTERFACE} ${PROPERTY} ${SIGNATURE} ${VALUE}

    # Triggering `Signal` under ${INTERFACE} for ${OBJECT} on ${BUS}
    emit ${OBJECT} ${INTERFACE} ${SIGNAL} [${SIGNATURE} ${VALUE}]

OPTIONS:
    -H. --host=[${USER@}HOST]
    -M, --machine=CONTAINER 
```

## Examples

我們用 `Bus` "org.freedesktop.hostname1" 為例

### Tree

列出該 `Bus` "org.freedesktop.hostname1" 下所有的 `Object`
![[{0C7E3C98-0628-4A1F-8224-1621565A3711}.png]]

### Introspect

列出該 `Bus` 下某一 `Object` 的所有 `Interface` 詳情；我們使用  `Bus` "org.freedesktop.hostname1" 下 `Object` "/org/freedesktop/hostname1" 為例
![[{359DC9D3-E1F4-4E49-9780-F0F979D88B38}.png]]

順帶附上透過 `Interface` org.freedesktop.DBus.Introspectable 的 `Method` Introspect() 得到的資料[JSON 檔案](docs/computer/IPC/D-Bus/attachments/org.freedesktop.hostname1.json)
### Method Call

我們用 `Bus` "org.freedesktop.hostname1" 下 `Object` "/org/freedesktop/hostname1" 的 `Interface` "org.freedesktop.hostname1" 為例

#### 無 Signaure

試著呼叫 "Describe" 這個 `Method`；因為這個 `Method` 不需要參數，所以就不用填寫 ${SIGNATURE}

>  busctl call org.freedesktop.hostname1 /org/freedesktop/hostname1 org.freedesktop.hostname1 Describe
![[{F1B85788-6CEA-48D1-BE6B-D66227CE3A5A}.png]]

回覆包含兩項，第一個是回覆的 `Signature`，其餘為回覆的值。將回覆的值整理之後，得到以下的結果
![[{7AD90F68-36E0-44FE-BD10-FDF63B27341A}.png]]

#### 有 Signature

試著呼叫 "GetProductUUID" 這個 `Method`；因為這個 `Method` 需要一個參數，其型態為 Boolean，所以我們需要加上 `b true` 這些參數

> sudo busctl call org.freedesktop.hostname1 /org/freedesktop/hostname1 org.freedesktop.hostname1 GetProductUUID b true
![[{0D867014-6DDC-42A7-875C-C626BE6A140A}.png]]

回覆的 `Signature` 為 ay，所以其後的值就是 byte 陣列。

### Property

busctl 有兩個功能可以對 `Property` 進行存取與更新，這兩個功能就像是 wrapper function，其實是透過 `Interface` "org.freedesktop.DBus.Properties" 下的 `Get` 與 `Set` 這兩個 `Method` 存取 `Property`

`get-property` - 存取 `Property` 
`set-property` - 更新 `Property` 

但是需要先進行 introspect 動作，某些 Property 是唯獨 (access="read")。這邊就已讀取 "FirmwareVendor" 為例

> busctl get-property org.freedesktop.hostname1 /org/freedesktop/hostname1 org.freedesktop.hostname1 FirmwareVendor
```
s "American Megatrends Inc."
```



# dbus-send

這是 freedesktop.org 提供與 D-Bus 互動的工具，其相關語法為

```
Exit status 0 indicates success, non-zero exit status indicated any failure.

dbus-send [--system|--session|--bus=${ADDRESS}|--peer=${ADDRESS}] 
          [--sender=${NAME}] 
          [--dest=${NAME}]                  # Bus Name
          [--print-reply [=${LITERAL}]]     # Print responses
          [--reply-timeout=${MSEC}] 
          [--type=${TYPE}] 
          ${OBJECT_PATH}                    # Object
          ${INTERFACE}.${MEMBER}            # Interface
          [${SIGNATURE}...]

${MEMBER} ::= Name of `Property`、`Method`、or `Signal`
${SIGNATURE} ::= ${ITEM} | ${CONTAINER} [ ${ITEM} | ${CONTAINER} ]
${ITEM} ::= ${TYPE}:${VALUE}
${TYPE} ::= byte | int16 | uint16 | int32 | uint32 | int64 | uint64 | double | boolean | string |objpath
${CONTAINER} ::= ${ARRAY} | ${VARIANT} | ${DICTIONARY}
${ARRAY} ::= array:${TYPE}:${VALUE}[, ${VALUE}]
${VARIANT} ::= ${VARIANT}:${TYPE}:${VALUE}
${DICTIONARY} ::= dict:${TYPE}:${TYPE}:${KEY}:${VALUE}[, ${KEY}:${VALUE}]
```

與相對方便的 busctl 命令相比，使用 dbus-send 命令必須透過 [[01.1-Common_Interfaces]] 描述的共用介面與該物件互動
## Example 

### Introspect

我們還是用 `Bus` "org.freedesktop.hostname1" 下的 `Object` "/org/freedesktop/hostname1" 為範例。從 [[01.1-Common_Interfaces]] 可以知道，我們可以透過呼叫 `Interface` org.freedesktop.DBus.Introspectable 下的 `Method` Introspect() 得到跟 busctl introspect 相同的結果。按照上述 dbus-send 的語法說明，我們可以使用下面的命令

> dbus-send --system --print-reply --dest=org.freedesktop.hostname1  /org/freedesktop/hostname1 org.freedesktop.DBus.Introspectable.Introspect

得到的結果 [JSON 檔案](docs/computer/IPC/D-Bus/attachments/org.freedesktop.hostname1_2.json) 
### Method Call

這邊使用的範例跟 busctl 一致
#### 無 Signaure

> dbus-send --system --print-reply --dest=org.freedesktop.hostname1 /org/freedesktop/hostname1 org.freedesktop.hostname1.Describe
![[{205A6D9E-6B7E-462F-A4A3-5577E6E64381}.png]]
#### 有 Signaure

>  dbus-send --system --print-reply --dest=org.freedesktop.hostname1 /org/freedesktop/hostname1 org.freedesktop.hostname1.GetProductUUID boolean:true
![[{EFA41EDE-FDF4-47E6-8B89-60E4A797CF2B}.png]]

### Property 

這邊使用的範例跟 busctl 一致。但是因為 dbus-send 不像 busctl 一樣有 wrapper 功能 `get-property` 與 `set-property`，所以需要透過 `Interface` "org.freedesktop.DBus.Properties" 下的 `Method` "Get" 與 "Set" 來存取。而 `Method` "Get" 需要兩個 string 參數，所以我們加上 "string:org.freedesktop.hostname1 string:FirmwareVendor"，說明我們要獲得 `Interface` "org.freedesktop.hostname1" 下 `Property` "FirmwareVendor" 的值

> dbus-send --system --print-reply --dest=org.freedesktop.hostname1 /org/freedesktop/hostname1 org.freedesktop.DBus.Properties.Get string:org.freedesktop.hostname1 string:FirmwareVendor
![[{D2979795-7D92-4DAD-B48B-C65460F5F834}.png]]