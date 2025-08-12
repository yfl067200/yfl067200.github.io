這個 class 的功能就是透過 D-Bus 搜尋任何有管理 emmc 裝置的服務，並從該服務中得到 emmc 裝置的資訊。

# 搜尋 emmc 裝置

在 OpenBMC 中




``` plantuml



abstract class std::enable_shared_from_this

class GetStorageConfiguation {
  respData: ManagedStorageType
  GetStorageConfiguration(): void
  GetStorageInfo(path: const std::string&, owner: const std::string&, retries = 5: size_t): void
  ~GetStorageConfiguration()
}


boost::container::flat_map <|.. StorageData

std::enable_shared_from_this <|.. GetStorageConfiguation
```


