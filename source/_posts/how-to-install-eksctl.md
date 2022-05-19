---
title: how to install eksctl
date: 2022-05-19 12:37:40
categories: aws
tags: aws eksctl
---

# what is eksctl
命令列工具，適用於使用 EKS 叢集，可自動執行許多個別任務。

# How to Use
還沒安裝 Homebrew的，請先安裝
```shell
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

<br>

安裝 Weaveworks Homebrew tap。
```shell
$ brew tap weaveworks/tap
```
<br>

安裝或升級eksctl
```shell
# 未安裝
$ brew install weaveworks/tap/eksctl

#已安裝
$ brew upgrade eksctl && brew link --overwrite eksctl
```

確認是否安裝成功
```shell
$ eksctl version
0.97.0
```

# 參考連結
[install eksctl](https://docs.aws.amazon.com/zh_tw/eks/latest/userguide/eksctl.html)
