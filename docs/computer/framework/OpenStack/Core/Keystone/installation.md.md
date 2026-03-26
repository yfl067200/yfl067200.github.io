> [!Note]
> 
> 請先檢查 [OpenStack 的需求](pre-installation.md.md)

# 安裝

本服務只需安裝在 ==Controller==。為了擴充性，本流程將說明如何讓 keystone 使用 Apache Server，並 藉由  Fernet tokens 進行驗證。

> [!Note]
> Fernet 加密是一種使用類似 AES-128 或是 HMAC-SHA256 同金鑰加密協定。該 token 中會包含加密過的 signed identity/authorization data，讓分散式系統可以直接檢驗 token，無需與資料庫等進行再確認。目前已知的內容包含版本 (version)、時間 (timestamp)、payload、與 VI。

Keystone 小組除了提供 Identity Service 之外，也提供以下的工具：

1. WSGI middleware、
2. Authentication Library、與
3. Python Client Library

因此需要安裝相關的服務： 

```
# Debain Based
sudo apt install keystone apache2 libapache2-mod-wsgi
```

> [!Note]
> Keystone 預設使用 Port 5000 與 35357，與 Apache 的 WSGI 的介面互動

# 設定


## 管理 MySQL

### 建立 MySQL 資料庫

Keystone 預設使用 MySQL 進行資料的管理，為此我們需要在 MySQL 中建立 keystone 使用的 database。其中 =='KEYSTONE-DBPASS'== 可以改為任意字串，作為使用者 keystone 的密碼。
```
${PROMPT}: mysql -u root -p                    # 與 MySQL Server 進行連線 (redhat)
${PROMPT}: mysql                               # 與 MySQL Server 進行連線 (debian)

MariaDB [(none)]> CREATE DATABASE keystone;    # 建立 database keystone

# MySQL 8 and 9
MariaDB [(none)]> CREATE USER 'keystone'@'localhost' IDENTIFIED BY 'KEYSTONE-DBPASS'; 
MariaDB [(none)]> GRANT ALL PRIVILEGESS ON keystone.* TO 'keystone'@'localhost';
MariaDB [(none)]> CREATE USER 'keystone'@'%' IDENTIFIED BY 'KEYSTONE-DBPASS'; 
MariaDB [(none)]> GRANT ALL PRIVILEGESS ON keystone.* TO 'keystone'@'%';

# MySQL 5.7
MariaDB [(none)]> GRANT ALL PRIVILEGESS ON keystone.* TO 'keystone'@'localhost' IDENTIFIED BY 'KEYSTONE-DBPASS';
MariaDB [(none)]> GRANT ALL PRIVILEGESS ON keystone.* TO 'keystone'@'%' IDENTIFIED BY 'KEYSTONE-DBPASS';

MariaDB [(none)]> quit;                        # 離開 MySQL Server 連線
```

### 設定 keystone 連結 MySQL

在 keystone 的設定檔 (/etc/keystone/keystone.conf) 中進行下述調整。說明如何與 MySQL 進行溝通，以及 token 的加密方式。
```
[databse]
# ...
#connection = mysql+pymysql://${USER}:${PASSWORD}@${HOSTNAME}/${DATABASE}
connection = mysql+pymysql://keystone:KEYSTONE-DBPASS@${HOSTNAME}/keystone

[token]
# ...
provider = fernet
```

設定好之後，讓 keystone 與 MySQL 進行同步
> su -s /bin/sh -c "keystone-manage db_sync" keystone
