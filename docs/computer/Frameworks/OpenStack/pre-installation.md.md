安裝 OpenStack 的官方文件為 [OpenStack Installation Guide](https://docs.openstack.org/2025.2/install/)。 除了 OpenStack 之外，還有基於 OpenStack 的雲架構，譬如說

- [OpenStack-Ansible](https:/ / docs. openstack. org/ openstack- ansible/)
- [Red Hat RDO](http:/ / openstack. redhat. com/)
- [Kolla](https:/ / docs. openstack. org/ kolla- ansible/ latest/)

# 安裝前的需求

## 處理器

OpenStack Compute Service 的處理器必須支援==虛擬化==，譬如說 Intel VT-x 或是 AMD 的 AMD-V

## 網路

每台跑 OpenStack 的機器需要 2 個以上的 1 GB 網路口；其中一個專門作為 OpenStack 內部資訊互動的網路口被稱為 ==Management Port==，另一個即與==網際網路 (Internet)== 互動。下圖如一個範例，各 Node 的 eth0 作為 ==Management Port==，而 eth2 則是各 Node 與==網際網路==互動。至於額外的 eth1，在這作為各 Node 間互動。
![[Screenshot 2025-12-22 at 4.42.10 PM.png]]

## 服務

OpenStack 需要使用以下的服務：時間同步 (NTP)、RDBS (MySQL)、MessageQuene (RabbitMQ)、分散式 Cache (MemCached)
### NTP

執行 OpenStack 的機器必須執行 Network Time Protocol (NTP)，如此一來才能讓所有機器都與 Controller 進行同步。在 Controller 上執行 NTP 服務，並接受來自 eth0 的需求。為了讓各裝置可以進行時間同步，記得要在防火墻上打開 NTP 

#### 安裝

這邊用 chrony 為例 

> sudo apt install chrony

#### 設定

另外在 ==Controller== 的 chrony 設定檔 (/etc/chrony/chrony.conf) 中，請加入下述設定

> allow ${Management IP} / ${Management Mask}

在其他 Nodes 的 chrony 設定檔 (/etc/chrony/chrony.conf) 中，請加入下述設定

> server ${CONTROLLER HOSTNAME} iburst

設定好之後，再重啟 Chrony 服務

> sudo systemctl restart chrony
### MySQL

執行 OpenStack 的機器 (Controller) 需要安裝 MySQL 服務與相關的 Python 模組，用來儲存設定。

#### 安裝

需要安裝 MySQL 的 python client 端 (python3-pymysql)

> sudo apt install mariadb-server python3-pymysql

#### 設定

另外在 ==Controller== 建立 Openstack 的 MySQL 設定檔 (/etc/my.cnf.d/99-openstack.cnf) 中，請加入下述設定
```
[mysqld]
bind-address = 10.10.0.100            # management IP

default-storage-engine = innodb
innodb_file_per_table = on
max_connections = 4096
collation-server = utf8_general_ci
character-set-server = utf8
```

重啟 MySQL 之後，需要執行命令初始化 MySQL 如下。請將密碼設定為 ==openstack==，其他設定為 "Y"

> sudo systemctl restart mysql
> sudo mysql_secure_installation

### Messaging Queue

OpenStack 支援眾多 Message Queue Services，包括 RabbitMQ 與 Qpid 等。這邊用 rabbitMQ 為例

#### 安裝
> sudo apt install rabbitmq-server

#### 設定

在 ==Controller== 上設定 RabbitMQ 的帳號

> rabbitmqctl add_user openstack RABBIT_PASS              # 加入 openstack 賬號
> rabbitmqctl set_permissions openstack ".\*" ".\*" ".\*".      # 設定 Configure、Read、與 write 權限

### Memcached

OpenStack 也使用分散式高速緩衝記憶體系統 (memcached)，供多個 nodes 可以共用高速緩衝；譬如 OpenStack Identity Server 的認證機制使用 Memcached 來存儲 Tokens，可以避免各 Nodes 重複與某些服務互動，取得相同的資訊。

> [!Note]
> 因為 memcached 並未支援認證與安全管制，所以 memcached 必須放在 firewall 後方

#### 安裝

需要安裝 memcached 的 python client 端 (python3-memcache)

> sudo apt install memcached python3-memcache

#### 設定

在 memcached 的設定檔 (/etc/memcached.conf)，將 default listener address (127.0.0.1) 改為 Controller 的 ==Management IP==。設定好之後，重啟 memcached

> sudo systemctl restart memcached


# 安裝 OpenStack

OpenStack 包含多種功能，我們在 ==Controller== 上安裝幾個核心功能：
- [Compute (Nova)](nova.md)、
- [Network (Neutron)](neutron.md.md)、
- [Identity (Keystone)](keystone.md.md)、
- [Images (Glance)](./glance.md)、與 
- [Web Interface (Horizon)](./horizon.md)。
![[Screenshot 2025-12-29 at 11.01.26 AM.png]]
## OpenStack Client

### 安裝

在 ==Controller== 端，安裝 OpenStack 的命令列工具

> sudo apt install python-openstackclient

