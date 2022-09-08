---
title: how to install aws cli
date: 2022-05-19 11:57:24
categories: aws
tags: aws-cli 
---
要使用aws公有雲，aws cli是一個很方便的工句，跟之前有介紹gcp的cli一樣，可以幫助你自動化很多的建置，今天以mac為例子，教大家如何安裝aws-cli，之後再透過aws-cli建立k8s集群。

# what is AWS CLI
AWS Command Line Interface (AWS CLI) 是開放原始碼工具，可讓你在命令列 Shell 中使用命令來與 AWS 服務互動。
只需最少的組態，AWS CLI 就可以讓你在終端程式中，從命令提示字元開始執行可實作相當於瀏覽器型 AWS Management Console 所提供功能的命令。


# why is AWS CLI
AWS CLI 能夠直接存取 AWS 服務的公有 API。 你可以利用 AWS CLI 探索服務的功能，並開發 Shell 指令碼來管理資源。
除了低階的 API 同等命令，有幾個 AWS 服務還提供 AWS CLI 自訂功能。自訂功能可能包括較高階的命令，可簡化具有複雜 API 的服務使用。
所以使用起來會更方便建立自動化流程。

# How to Use
先下載 [aws-cli](https://awscli.amazonaws.com/AWSCLIV2.pkg) ，直接點選開啟安裝。

接著就不管3721，就直接選取預設選項
![label](how-to-install-aws-cli/1.png)

<br>

![label](how-to-install-aws-cli/2.png)

<br>

![label](how-to-install-aws-cli/3.png)

<br>

![label](how-to-install-aws-cli/4.png)

確認一下安裝位置與是否安裝成功

```shell
$ which aws
/usr/local/bin/aws
```

```shell
$ aws --version
aws-cli/2.2.44 Python/3.8.8 Darwin/20.5.0 exe/x86_64 prompt/off
```
<br>

成功拉～
接下來如果要建立k8s集群，還需要kubectl和eksctl，下一篇繼續。
記得配一下config~

```bash 
$ aws configure
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-west-2
Default output format [None]: json
```

# 參考連結
[aws cli doc](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
