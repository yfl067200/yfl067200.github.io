
每一個註冊的 Object 都會有這些 Interfaces。這些 `Interface` 與其下的 `Property` 與 `Method` 提供 D-Bus 進行搜尋。為了方便，我們使用 `bus` "org.freedesktop.hostname1" 下的 `object` "/org/freedesktop/hostname1" 作為範例

# org.freedesktop.DBus.Peer

| 型態       | 名稱               | 參數 `Signature` | 回傳值 `Signature` | 說明                        |
| :------- | :--------------- | :------------- | --------------- | ------------------------- |
| `Method` | `Ping()`         | -              | -               | 用來判斷對應的程式是否還活著            |
| `Method` | `GetMachineId()` | -              | s               | 返回該 `Object` 所在機器的 `UUID` |

## org.freedesktop.DBus.Introspectable

| 型態       | 名稱             | 參數 `Signature` | 回傳值 `Signature` | 說明                    |
| :------- | :------------- | :------------- | --------------- | --------------------- |
| `Method` | `Introspect()` | -              | s               | 回傳一個 `XML` 格式的字串，用來說明 |

### Introspect() --> string

提供該 `Object` 下所有 `Interfaces` 等資訊，得到的資料跟透過 busctl introspect 一致。這邊我們用 `Bus` "org.freedesktop.hostname1" 下的 `Object` "/org/freedesktop/hostname1" 為例，結果如這個 [JSON 檔案](./attachments/org.freedesktop.hostname1.json)

# org.freedesktop.DBus.Properties

| 型態       | 名稱                    | 參數 `Signature` | 回傳值 `Signature` | 說明                                                                                                      |
| :------- | :-------------------- | :------------- | --------------- | ------------------------------------------------------------------------------------------------------- |
| `Method` | `Get()`               | `ss`           | `v`             | s - `Interface` 名稱<br>s - `Property` 名稱<br>讀取某個 `Interface` 下，某一個 `Property` 的值                         |
| `Method` | `GetAll()`            | `s`            | `a{sv}`         | s - `Interface` 名稱<br>讀取某個 `Interface` 下，所有 `Porperty` 的值                                               |
| `Method` | `Set()`               | `ssv`          | -               | s - `Interface` 名稱<br>s - `Property` 名稱<br>v - 該 `Property` 的新值<br>更新某個 `Interface` 下，某一個 `Property` 的值 |
| `Signal` | `PropertiesChanged()` | `sa{sv}as`     | -               |                                                                                                         |
透過 Introspect，我們可以得到更詳細的資訊，包含 `Method` 所需的參數與型態。以下為本 `Interface` 下 `Method` 與 `Signal` 的詳細資訊
![[./figures/{5FA8DB95-A411-4038-870E-64AEE41E3D8F}.png]]

相較於 `Method` 的易懂，`Signal` PropertiesChanged 比較複雜。該 `Signal` 將會送出 3 組資訊：

"s" - `Property` 發生變動的 `Interface` 名稱
"a{sv}" - 一個陣列，內含該 `Interface` 發生變動的 `Property` 名稱與更新後的值
"as" - 一個陣列，內含該 `Interface` 未發生變動的 `Property` 名稱


