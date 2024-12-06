
Entity Manager 的 repository 位於 https://github.com/openbmc/entity-manager 。相關的 metadata 位於 meta-phosphor/configuration/entity-manager 目錄；在編譯專案時，使用的目錄為 xxx-openbmc-linux-yyy/entity-manager。

Entity Manager 的設計原則包含：

- 每個 Device 都使用一個 configuration file (JSON 檔案)
- Entity Manager 支援硬體偵測
    - Entity Manager 是透過 D-Bus 介面偵測硬體是否存在
        - 相關的 D-Bus 介面是由其他 Service 建立，實際偵測硬體的工作是由這些 Service 實作
    - 新增或是移除相關的服務
        - 要能在 D-Bus 上建立或是移除硬體相關介面，以便於其他程式與該硬體互動



01. [Definitions](01-Definitions.md) - 簡易的說明 Entity Manager 的功能，與其他互動的 Packages
02. [Configurations](02-Configurations.md) - 說明如何建立正確的 configuration files
03. [Topology](03-Topology.md)