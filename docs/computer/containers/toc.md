# Container (容器) 簡介

Container 是一個形成，使用 kernel 上 cgroups 與 namespace 功能將自己的環境與其他 kernel 分隔。

## Kernel 上的實作

### namespace

Linux 從 2006 開始擴展 namespace，進行不同資源的分隔。

- Processes

- Network

- Users

- IPC

- Mounts

- Unix Time-Sharing (UTS)


### cgroups


t
