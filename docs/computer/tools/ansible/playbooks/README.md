
Playbook 是使用 YAML 格式的文檔，內容可以包含多種 task；每一個 task 都始於下束格式，並說明哪些機器需要執行該 task

> - name: XXXXX

Ansible 會逐行產生出對應的 python script，然後將該 python script 傳送到對應的機器上執行。產生的 python script 會根據 YAML 檔案的順序 (由上而下) 產生，故執行的 task 順序不會被改變。

> [!Note]
> Ansible 會同時在多台機器上進行相關的 task。但是為了方便，唯有當所有機器都完成目前的 task 之後，ansible 才會同時讓多台機器進行下一個 task

Ansible 本身已經建立多個 python module 處理大多數使用者會用到的功能，譬如說安裝套件等；但是還是可以讓你可以自行進行客製化，像是建立檔案或是路徑等。

