
# 安裝所需的套件

在 [OpenBMC 的首頁](https://github.com/openbmc/openbmc) 有列出在 ubuntu 與 Fedora 環境下所需安裝的套件列表。正常情況下，只需要照著安裝即可。

但是在某些情況下，會遇即便都安裝好了，但是還是無法更新的情況。以下將我遇上的情況與解決方案列出

1. 缺乏某些 perl 模駔

若是在 fedora 的環境下，常會遇上某些 perl 模組沒有安裝的情況。以下為其中一例：


>>>
解決方案是透過 dnf 安裝缺少的模組。比較討厭的是，列出缺少模組的訊息跟 dnf 套件的名稱有些出入。以上例來說，需要安裝的 perl 模組是：

| 缺乏套件名稱 | 安裝套件名稱 |
|:-------------|:-------------|
| Thread::Queue | perl-Thread-Queue |
| File::Compare | perl-File-Compare |
| File::Copy | perl-File-Copy |
| FindBin | perl-FindBin |
>>>

