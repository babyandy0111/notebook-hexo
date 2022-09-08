---
title: how to install azure cli
date: 2022-05-23 21:02:29
categories: azure
tags: azure-cli
---

# what is Azure CLI
Azure 命令列介面 (CLI) 是跨平台命令列工具，可連線到 Azure 並在 Azure 資源上執行系統管理命令。 其允許透過終端機使用互動式命令列提示或指令碼來執行命令。
若要進行互動式使用，你必須先在 Windows 上啟動 cmd.exe 之類的殼層，或在 Linux 或 macOS 上啟動 Bash，然後在殼層提示字元發出命令。 若要自動化重複的工作，你可以使用所選殼層的腳本語法，將 CLI 命令組合成殼層腳本，然後執行腳本。

# How to Use
在mac上安裝，如果沒有Homebrew就去安裝
<br>

```shell
$ brew update && brew install azure-cli
# 如果在M1上 brew要改成 arch -arm64 brew
```
<br>

安裝完之後，執行 
<br>

```shell
$ az login
```

會跳出瀏覽起進行驗證登入，就直接在瀏覽器中操作後，再回到console，會看到驗證資訊了～


# 參考連結
[az cli install](https://docs.microsoft.com/zh-tw/cli/azure/install-azure-cli)
<br>

[az cli 命令](https://docs.microsoft.com/zh-tw/cli/azure/get-started-with-azure-cli)
