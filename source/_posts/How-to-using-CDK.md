---
title: How to using CDK
date: 2022-09-20 16:53:13
categories: aws
tags: cdk, aws, node, cloudWatch, eventBridge, lambda
---
# 什麼是ＣＤＫ
CDK 是 AWS Cloud Development Kit (AWS CDK) 是一套開源軟體開發架構，使用熟悉的程式設計語言定義您的雲端應用程式資源。

早期佈建雲端應用程式可能是具有挑戰性的過程，需要透過手動操作、撰寫自訂指令碼、維護範本或學習特定領域的語言。AWS CDK 使用程式設計語言的熟悉性和表達能力，幫助應用程式進行模型分析。

它提供的高階元件 (稱為建構) 可利用經過驗證的預設值預先設定雲端資源，因此，您可以輕鬆建置雲端應用程式。AWS CDK 透過 AWS CloudFormation，以安全、可重複的方式佈建您的資源。它也讓您能夠編寫和分享自己的自訂建構，以整合組織的需求，協助您加快新專案。

# CDK 支援哪些語言
目前CDK已經出到V2的版本，透過doc能夠知道，CDK支援了Python、.Net、Go、Java、TypeScript，從這點來看，比較起Terraform 的自定義語言，CDK提供了不同的撰寫語言，讓開發者使用自己所熟悉的語言撰寫IaC，這點真是大大的降低學習成本，那使用Terraform難道就活該倒楣嗎？不～針對偏好使用 Terraform 的客戶，cdktf 提供以 TypeScript 和 Python 定義 Terraform HCL 狀態檔案的 CDK 建構。

# CDK8s
對於 Kubernetes 使用者，cdk8s 專案可讓您使用 CDK 建構在 TypeScript、Python 和 Java 中定義 Kubernetes 組態。超級方便～

# 一切都從官網開始
第一次學習CDK的人可以使用以下兩個官網，稍微看看，了解CDK能夠做到哪些事情
[cdk doc](https://docs.aws.amazon.com/cdk/api/v2/)
[cdkf](https://github.com/hashicorp/terraform-cdk)

# 需求
小編這次因內部需求所需，需將AWS Support 所開立的CASE同步到自己的內部系統上，因此使用CDK作為IaC的主要工具，並以golang作為lambda的語言，以下為本次實作架構，另外，小編使用的電腦為MAC，如果是Windows就抱歉拉，如果需要Windows，歡迎敲碗～

![架構](https://storage.googleapis.com/andy.gaiatechs.info/1.png)

# 建立CDK專案前的準備
1. 安裝 Node 和 NPM, 若你的安裝或升級過程遇到權限問題, 可能會用到sudo, 需要鍵入 superuser password
   `$ brew install node`
   `$ npm install -g aws-cdk`

2. 安裝 golang
   `$ brew install go `
3. 安裝AWS CLI
   [下載CLI V2](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

4. 生成Access Key ID & Secret Access Key，建議大家可以建立一個IAM 的 group，然後把新創建的user加入群組

    ```
    #! /bin/bash

    #VARs
    username=$1
    group="admin-group"
    password=`openssl rand -base64 32`

    #create user with password then add user into a group and create ak and sk
    echo "User: $username Password: $password"
    aws iam create-user --user-name $username --tags Key=Org,Value=Support Key=CreatedBy,Value=awscli
    aws iam create-login-profile --user-name $1 --password $password --password-reset-required
    aws iam add-user-to-group --user-name $username --group-name $group
    aws iam create-access-key --user-name $username
    ```

5. 設定 AWS CLI
   `$ aws configure`

   ``` AWS Access Key ID [None]: ANOTREALACCESSKEYID
    AWS Secret Access Key [None]: ANOTREALSECRETACCESSKEY
    Default region name [None]: eu-east-1
    Default output format [None]: json
    ```

6. 進行CDK初始化, 需要先得知"Account"跟"Region"的資訊, 從上一個步驟你知道了"Region", Account的資訊由下面的`aws sts get-caller-identity`指令得到, Account 的數值是由一串數字組成.
   `$ aws sts get-caller-identity`
   `$ cdk bootstrap aws://[ACCOUNT-NUMBER]/[REGION]`


    ```
    ⏳ Bootstrapping environment aws://123456789012/us-east-1...
    CDKToolkit: creating CloudFormation changeset...

    ✅  Environment aws://123456789012/us-east-1 bootstrapped.
    ```


> '---cloudformation-execution-policies' 是允許cloudformation可以執行時使用的IAM policy, 根據the priciple of least privilege, AWS 建議用戶可以另外指定, 上述的做法會以目前用戶擁有的權限給cloudformation 使用.

> 記得替換參數，指令下完後，你會看到正在初始化專案～

> 上方的軟體安裝均可以使用Docker，不過如果你是初學，建議直接安裝於MAC身上，看這人習慣，這邊就不贅述，小編我是直接安裝MAC上，方便開發。AWS的key也可以手動生成，看個人習慣。


# 安裝建立CDK專案
準備好以上就可以透過指令產生CDK預設專案拉～

`$ mkdir cdk-demo`
`$ cd cdk-demo`
`$ cdk init --language typescript`

    ├── bin
    │   └── cdk-demo.ts
    ├── cdk.json
    ├── jest.config.js
    ├── lib
    │   └── cdk-demo-stack.ts
    ├── package.json
    ├── package-lock.json
    ├── README.md
    ├── test
    │   └── cdk-demo.test.ts
    └── tsconfig.json


> 以下是一些重要檔案及其用途：
* bin/cdk-project.ts - 這是進入 CDK 應用程式的途徑。此檔案將會載入/建立我們在 lib/* 底下定義的所有堆疊

* lib/cdk-project-stack.ts - 這是主要的 CDK 應用程式堆疊的定義之處。資源及其屬性可存放於此處。

* package.json - 您會在此處定義專案相依性，以及一些額外資訊和建置指令碼 (npm build、npm test、npm watch)。
* cdk.json - 此檔案會向工具組指出如何執行你的應用程式，以及與 CDK 和你的專案相關的一些額外設定和參數。

我們將著重於 lib/cdk-demo-stack.ts 和 bin/cdk-demo.ts 檔案，以建立我們的基礎設施。我們將新增一段程式碼。


# 修改 bin/cdk-demo.ts
使其顯示如下。請務必將您的 AWS 帳戶 ID 取代為正確的號碼，並選擇正確的區域。

```javascript=
import 'source-map-support/register';
import * as cdk from '@aws-cdk/core';
import { CdkDemoStack } from '../lib/cdk-demo-stack';

const app = new cdk.App();
new CdkDemoStack(app, 'CdkDemoStack', {
  env: { account: '123456789012', region: 'eu-west-1' },
});
```



# 接著修改 lib/cdk-demo-stack.ts

```javascript=
import * as cdk from '@aws-cdk/core';
import * as lambda from '@aws-cdk/aws-lambda';
import * as iam from '@aws-cdk/aws-iam';
import * as events from '@aws-cdk/aws-events';
import * as logs from '@aws-cdk/aws-logs';
import * as targets from '@aws-cdk/aws-events-targets';
import * as path from 'path';
import * as assets from '@aws-cdk/aws-s3-assets';

export class CdkDemoStack extends cdk.Stack {
  constructor(scope: cdk.Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    // Policy
    const supportPolicy = new iam.PolicyStatement({
      actions: ['support:*'],
      resources: ['*'],
    })

    // lambda
    const demoLambdaAsset = new assets.Asset(this, "GoServerLambdaFnZip", {
      path: path.join(__dirname, "../lambda"),
    })

    // seting lambda and env 
    const demoLambda = new lambda.Function(this, 'msp', {
      runtime: lambda.Runtime.GO_1_X,
      timeout: cdk.Duration.seconds(300),
      handler: "main",
      code: lambda.Code.fromBucket(
        demoLambdaAsset.bucket,
        demoLambdaAsset.s3ObjectKey
      ),
      environment: {
        region: cdk.Stack.of(this).region,
        zones: JSON.stringify(cdk.Stack.of(this).availabilityZones),
        endpoint: 'endpoint',
      },
    })

    // add policy
    mspLambda.role?.attachInlinePolicy(
      new iam.Policy(this, 'demoSupportPolicy', {
        statements: [supportPolicy],
      }),
    )

    // cloudwatch log group
    const logGroup = new logs.LogGroup(this, 'demoLogGroup', {
      logGroupName: 'demoLogGroup',
    })

    // add eventBridge
    const demoEventRule = new events.Rule(this, 'demoRule', {
      eventPattern: {
        source: ["aws.support"],
      },
    })

    // add lambda for eventBridge's target
    demoEventRule.addTarget(new targets.CloudWatchLogGroup(logGroup))
    demoEventRule.addTarget(new targets.LambdaFunction(demoLambda))
  }
}
```

> 如果遇到import時找不到套件，記得安裝一下，例如 $ npm install @aws-cdk/aws-events

# 撰寫lambda
1. 建立一個資料夾
   $ mkdir lambda

2. 建立main.go
   $ touch main.go

3. 寫code


```golang=

package main

import (
 "context"
 "fmt"
 "log"
 "os"

 "github.com/aws/aws-lambda-go/events"
 "github.com/aws/aws-lambda-go/lambda"
 "github.com/joho/godotenv"
)

func main() {
 lambda.Start(HandleRequest)
}

func HandleRequest(_ context.Context, event events.CloudWatchEvent) error {
 err := godotenv.Load()
 if err != nil {
  log.Fatal("Error loading .env file")
 }
 region := os.Getenv("region")
 zones := os.Getenv("zones")
 iam_endpoint := os.Getenv("iam_endpoint")

 fmt.Println("env region:", region)
 fmt.Println("env zones:", zones)
 fmt.Println("env endpoint:", iam_endpoint)

 // 接收到值後，寫你想要做的事情
 fmt.Printf("PRINT: %#v\n", event)
 return nil
}
```


4. 一開始需要進行初始化並且載入相關套件
   `$ go mod init main && go mod tidy`

> 這邊golang的code單純進行print cloudwatch的值，且該lambda已經獲得aws.support的權限了，至於要做怎樣的操作，可自由發揮

# 開始部署嚕
- 確認您的程式碼是否有效
  `$ npm run build`

- 確認最終產生的CloudFormation是否正確
  `$ cdk sync`

  > 沒意外你會看到一堆很噁心的CloudFormation，你就會知道CDK其實只是幫你產生CloudFormation的yaml來執行而已～

- 部署
  `$ cdk deploy`

    ```CdkDemoStack: deploying...
    CdkDemoStack: creating CloudFormation changeset...
    [··························································] (0/13)
    
    3:50:17 PM | CREATE_IN_PROGRESS   | AWS::CloudFormation::Stack            | CdkDemoStack
    3:50:21 PM | CREATE_IN_PROGRESS   | AWS::EC2::InternetGateway             | MainVpc/IGW
    3:50:21 PM | CREATE_IN_PROGRESS   | AWS::EC2::VPC                         | MainVpc
    3:50:22 PM | CREATE_IN_PROGRESS   | AWS::CDK::Metadata                    | CDKMetadata/Default
    ```

  > 礙於小編公司內部資訊不能透露，這邊顯示示意圖～ 只要看到 ✅  就對了


幾分鐘後，您應該會看到一個綠色的勾號，以及新建 CloudFormation 堆疊的 ARN (Amazon 資源名稱)。

# 測試一下是否成功了
如果有購買AWS Support就可以去Support建立一張CASE，屆時你就會在cloudwatch中看到log，而由於EventBridge的Target是lambda，因此會觸發你所設置的lambda，也會看到lambda中的log唷。

![](https://i.imgur.com/v3duvIo.png)



# 將資源刪除
這就不多廢話，試完了，如果不想要保留，當然要刪除它，直接 $ cdk destroy

    Are you sure you want to delete: CdkEcsInfraStack (y/n)? y
    CdkEcsInfraStack: destroying...
    
    ✅  CdkEcsInfraStack: destroyed


# 結語
CDK真的很方便，可以讓工程們使用自己擅長的語言進行Iac的開發，將CDK納入自己的技能樹，絕對有很大的幫助。

# 參考文獻
[aws cdk](https://docs.aws.amazon.com/cdk/v2/guide/getting_started.html)
[aws cdk doc](https://docs.aws.amazon.com/cdk/api/v2/)
[create-login-profile](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iam/create-login-profile.html)
[create-user](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iam/create-user.html)
[add-user-to-group](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iam/add-user-to-group.html)
