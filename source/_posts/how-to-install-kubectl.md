---
title: how to install kubectl
date: 2022-05-19 17:00:20
categories: kubectl
tags: kubectl
---

# what is kubectl
命令列工具，適用於使用 Kubernetes 叢集，是針對k8s下指令的工具，client要創建或刪除東西，都是透過該工具下指令。

# How to Use
基本上如果你的k8s架設在雲端上，在mac上均需要安裝kubectl，再透過CLI去存取你的k8s集群～

1. 透過 Homebrew 安裝
```shell
$ brew install kubectl 
```

<br>

2. 確認是否安裝完成
```shell
$ kubectl cluster-info 
```

<br>
補充： 
為了讓kubectl能夠發現並訪問Kubernetes集群，你需要一個kubeconfig文件， 
該文件在創建集群時，會自動產生。 通常，kubectl 的配置文件存放在 

``` ~/.kube/config ``` 

<br>

3. 開啟 shell 自動補全功能
```shell
$ echo "source <(kubectl completion zsh)" >> ~/.zshrc
$ source ~/.zshrc
```


# 參考連結
[install kubectl](https://kubernetes.io/zh/docs/tasks/tools/install-kubectl-macos/#install-with-homebrew-on-macos)
