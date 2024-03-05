## RAID

Linux 本身提供了軟體的 RAID 功能

### 建立 RAID 的問題

Linux Raid kernel list 中有明確的說明，2019 年之後的 WS 紅標與 2020 之後的桌面 HDD 都無法正常的使用 Linux 軟體 RAID。這是因為 Timeout mismatch 的問題

### Timeout Mismatch

HDD timout 是指 HDD 無法處理 OS 的要求而返回錯誤的時間。不幸的是，這個時間並沒有統一的標準；有的廠商提供設定 HDD timeout 的功能，有的沒有。Linux Kernel 中設定 HDD timeout 的功能稱為 ERC (Error Recovery Control)，但是各家硬體廠商使用不同的名稱；WD 稱為 TLER (Time-Limited Error Recovery)，而 Samsumg/Hitachi 稱為 CCTL (Command Completion Time Limit)。大部分的硬體都沒有支援 ERC 的功能，可以透過 smartctl 命令檢查 HDD 是否支援：

``` bash
smartctl -l scterc /dev/${DEV}
```

如果硬體不支援 ERC，硬體大約需要 2 分鐘或是更久才會觸發 HDD timeout。但是對 Linux Kernel 而言，timeout 的時間只有 30 秒左右。當 Linux Kernel 觸發 timeout，Linux 中的 RAID 會重新計算並發送命令給硬體；但是因為硬體尚未觸發 timeout，導致 RAID 會誤認硬體已經死亡，並將其踢出 Array。這就導致了 Linux RAID 很容易崩潰的問題。

Linux RAID 建議，如果硬體有支援 ERC，最好是將 HDD timeout 時間設定為 7 秒。不然只能將 Kernel timeout 的時間拉長到 3 分鐘或是更長，要比硬體的 HDD timeout 時間更長。Kernel RAID 有提供一位 Brad Campbell 提出的方案：

``` bash
#!/bin/bash
for i in /dev/sd? ; do
    if smartctl -l scterc,70,70 $i > /dev/null ; then
        echo -n $i " is good "
    else
        echo 180 > /sys/block/${i/\/dev\/}/device/timeout
        echo -n $i " is  bad "
    fi;
    smartctl -i $i | egrep "(Device Model|Product:)"
    blockdev --setra 1024 $i
done
```

如果透過 smartctl scterc 命令設定 ERC 時間為 7 秒失敗，則將 Kernel Timeout 時間改為 3 分鐘。但是不是每一個 Block Driver 都支援 scterc 命令，有些 Driver 直接回覆 0 導致這個 script 在 2010 前的硬體與 2019 之後的 SMR 裝置無法解決 Timeout mismatch 的問題。

### Shingled Magnetic Recording (SMR) 

在 2019 之後，新的硬體技術 Shingled Magnetic Recording (SMR) 成為市場主流。SMR 在重新整理資料的時候，可能長達 10 分鐘無法回應。Kernel RAID 建議避免使用 SMR，改為使用 CMR (Conventional Magnetic Recording) 硬體建立 RAID。

