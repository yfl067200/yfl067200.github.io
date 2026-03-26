從 meson.build 檔案中可以得知，entity-manager 是 Entity Manager 必要的執行檔，一定會被編譯。這個執行檔是由以下檔案組成：

- entity_manager.cpp
- expression.cpp
- perform_scan.cpp
- perform_probe.cpp
- overlay.cpp
- topology.cpp
- utils.cpp


# 起始

## 建立 D-Bus 相關介面

| D-Bus     | 值                                    |
| :-------- | ------------------------------------ |
| Object    | `/xyz/openbmc_project/EntityManager` |
| Interface | `xyz.openbmc_project.EntityManager`  |


