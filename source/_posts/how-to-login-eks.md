---
title: how to login eks
date: 2022-05-19 13:31:23
categories: aws
tags: aws login eks k8s
---
這邊記錄一下，假設你已經有AWS的EKS叢集，若你想要在不同的地方進行登入時，所需要進行的步驟：

- 確認當前的開發機器上，AWS CLI與eksctl 均已安裝，並且是最新的，而AWS CLI與kubectl的版本有點關係，可以看看這[issue](https://github.com/aws/aws-cli/issues/6920) ， 主要解法就是更新一下CLI並重新 [set config]( https://stackoverflow.com/questions/72126048/error-exec-plugin-invalid-apiversion-client-authentication-k8s-io-v1alpha1-c) 
。

<br>

- 如果你是叢集的建立者
1. 若要查看您的 AWS CLI 使用者或角色的組態，請執行以下命令：
```shell
$ aws sts get-caller-identity
{
    "UserId": "XXXXXXXXXXXXXXXXXXXXX",
    "Account": "XXXXXXXXXXXX",
    "Arn": "arn:aws:iam::XXXXXXXXXXXX:user/testuser"
}
```

<br>

2. 確認你的arn相符

<br>

3. set你的kubeconfig，注意，需要用正確的IAM身份執行
```shell
# 將 eks-cluster-name 取代為您的叢集名稱。將 aws-region 取代為您的 AWS 區域
$ aws eks update-kubeconfig --name eks-cluster-name --region aws-region

# 或者連同ARN一起設定
$ aws eks update-kubeconfig --name eks-cluster-name --region aws-region --role-arn arn:aws:iam::XXXXXXXXXXXX:role/testrole
```

<br>

4. 確認kubeconfig是否已經更新
```shell
$ kubectl config view --minify
```

<br>

5. 若要確認您的 IAM 使用者或角色是否經驗證
```shell
$ kubectl get svc
```

<br>

---

<br>

- 如果你不是叢集建立者
1. 查看你的角色，取得你的arn
```shell
$ aws sts get-caller-identity
```

<br>

2. 交給你的管理員，透過管理員編輯 aws-auth ConfigMap
```shell
$ kubectl edit configmap aws-auth -n kube-system
```
<br>

3. 管理員將新arn填入，格式如下
```shell
# 將 testuser 取代為您的使用者名稱
# arn 取代為拿到的arn
mapUsers: |
  - userarn: arn:aws:iam::XXXXXXXXXXXX:user/testuser
    username: testuser
    groups:
      - system:masters
      
# 將 IAM 角色新增到 mapRoles
# 將 testrole 取代為您的角色
# arn 取代為拿到的arn
mapRoles: |
  - rolearn: arn:aws:iam::XXXXXXXXXXXX:role/testrole
    username: testrole
    groups:
      - system:masters
      
# mapRoles 部分中 username 的值僅接受小寫字元。 映射該 IAM 角色時應該略去路徑，因為 rolearn 不支援路徑。
# system:masters 群組允許進階使用者存取對任何資源執行任何動作。
```

<br>

打完收工～應該就可以登入了～


# 參考連結
[aws login](https://aws.amazon.com/tw/premiumsupport/knowledge-center/eks-api-server-unauthorized-error/)
