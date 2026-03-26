OpenStack Network Service (Neutron) 的架構如下。網路設定儲存在 Database 中，而 Message Queue 除了是使用者 API 命令的暫存區之外，也是 OpenStack Network Service 與各 Agents 互動的訊息池。 

![[Screenshot 2025-12-22 at 3.00.53 PM.png]]

# OpenStack Network 支援功能

## [Switching]()

OpenStack Network Service 支援多種 Virtual Switching 機制，包含 bridge 與 Open vSwitch。

> [!Note]
> Virtual Switching 為一個用來處理 L2 封包的軟體虛擬裝置

> [!Note]
> Open vSwitch，又被稱為 OVS，是一個 Open Source 的 Virtual Switch。

## [Routing]()

OpenStack Network Service 支援 routing 與 NAT 等，支援 IP Forwarding、iptables、甚至 namespaces

## [Load Balancing]()

首度在 OpenStack 版本 Grizzly 面世，Load Balancing as a Service (LBaaS v2) 可供使用者將需求平均分散到各個服務。OpenStack Network Service 本身提供的 plugins 為 HAProxy，但是也支援第三方模組。

## [Firewalling]()

OpenStack Network Service 包含 2 個部分：
- Security Groups - 同一 Group 中的程序會使用相同的功能與規則
- Firewall as a Service (FWaaS)

> [!Note]
> Security Groups 來自於 Nova-Network，為 OpenStack Compute Service 中使用的 networking stack，源自於 Amazon EC2 Security Groups。

## [Virtual Private Networks]()

OpenStack Network Service 提供 IPSec-based VPN

# Network Functions Virtualization


# 