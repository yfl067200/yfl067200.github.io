# 機器列表 (Invoentory)

建議將機器列表與 playbook 放置於同一路徑下，可以針對不同需求建立不同的機器列表。`ansivle-playbook` 命令也接受參數 `-i` 與 `--inventory` 指定 inventory 檔案 (可以使用 `,` 分隔多個檔案) 或是其路徑 (單一路徑)

> [!Note]
> Ansible 將機器列表稱為清單 (Inventory)

> [!Note]
> Ansible 預設機器列表位於 /etc/ansible/hosts 檔