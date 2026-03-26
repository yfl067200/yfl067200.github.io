Ansible 是一個專案管理工具；不同於 git 等 CVS 專注於專案版本的管理，ansible 專注於專案的發布。

下面為一個 ansible 專案的檔案。其中的 `{Playbooks}.yml` 為多個 YAML 格式的檔案，內容將列出該專案所需的差異性 (版本或是虛擬化等)，所進行的發布動作；而 `inventory` 路徑下放置該專案的機器列表。透過呼叫 `ansible-playbook -i inventory/{Host List}.ini {Playbook}.yml` 進行專案的發布
```
--- ${PROJECT}
 |
 |- inventory
 |      |
 |      |-  {Host List}.ini
 |
 |- {Playbooks}.yml
```


- [Playbooks](docs/Computer/tools/ansible/playbooks/README.md)
- [Hosts](docs/Computer/tools/ansible/hosts/README.md)
- Virtualization
  - [Python](./virtualization/python/README)
- Apprendix
  - Commands
    - [ansible](ansible.md)
    - [ansible-playbook](ansible-playbook.md)