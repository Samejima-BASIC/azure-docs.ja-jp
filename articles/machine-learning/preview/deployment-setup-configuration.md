---
title: "Azure Machine Learning モデル管理のセットアップと構成 | Microsoft Docs"
description: "このドキュメントでは、Azure Machine Learning でのモデル管理の設定と構成に関連する手順および概念について説明します。"
services: machine-learning
author: raymondlaghaeian
ms.author: raymondl
manager: hjerez
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 12/6/2017
ms.openlocfilehash: 391c02c5c76a3be3f5338790a81bca0311fb301b
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/23/2018
---
# <a name="model-management-setup"></a>モデル管理のセットアップ

このドキュメントでは、Azure ML モデル管理を使用して Machine Learning モデルを Web サービスとしてデプロイおよび管理する方法について説明します。 

Azure ML モデル管理を使用すると、SparkML、Keras、TensorFlow、Microsoft Cognitive Toolkit、Python などの、いくつかのフレームワークを使用して構築された Machine Learning モデルを効率的にデプロイおよび管理できます。 

このドキュメントの最後までには、モデル管理の環境をセットアップし、Machine Learning モデルをデプロイする準備ができるようになります。

## <a name="what-you-need-to-get-started"></a>はじめにやるべきこと
このガイドを最大限に活用するには、モデルをデプロイできる Azure サブスクリプションまたはリソース グループに対する共同作成者アクセス権が必要です。
Azure Machine Learning Workbench および [Azure DSVM](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-virtual-machine-overview) には CLI がプレインストールされています。

## <a name="using-the-cli"></a>CLI の使用
ワークベンチからコマンド ライン インターフェイス (CLI) を使用するには、**[ファイル]** -> **[Open Command Prompt] \(コマンド プロンプトを開く)** をクリックします。 

データ サイエンス仮想マシン上で、接続してコマンド プロンプトを開きます。 「`az ml -h`」と入力してオプションを表示します。 コマンドの詳細については、--help フラグを使用します。

その他のすべてのシステムでは、CLI をインストールする必要があります。

### <a name="installing-or-updating-on-windows"></a>Windows へのインストール (または更新)

https://www.python.org/ から Python をインストールします。 pip のインストールを選択したことを確認してください。

[管理者として実行] を使用してコマンド プロンプトを開き、次のコマンドを実行します。

```cmd
pip install -r https://aka.ms/az-ml-o16n-cli-requirements-file
```

### <a name="installing-or-updating-on-linux"></a>Linux へのインストール (または更新)
コマンド ラインから次のコマンドを実行し、プロンプトに従います。

```bash
sudo -i
pip install -r https://aka.ms/az-ml-o16n-cli-requirements-file
```

### <a name="configuring-docker-on-linux"></a>Linux での Docker の構成
root 以外のユーザーが使用する Linux 上の Docker を構成するには、「[Post-installation steps for Linux (Linux でのインストール後の手順)](https://docs.docker.com/engine/installation/linux/linux-postinstall/)」にある手順に従います。

>[!NOTE]
> Linux DSVM では、下のスクリプトを実行して Docker を正しく構成できます。 **このスクリプトを実行した後、ログアウトしてログインし直すことを忘れないでください。**
>```
>sudo /opt/microsoft/azureml/initial_setup.sh
>```

## <a name="deploying-your-model"></a>モデルをデプロイする
モデルを Web サービスとしてデプロイするには、CLI を使用します。 Web サービスはローカルに、またはクラスターにデプロイできます。

ローカル デプロイから始め、そのモデルとコードの動作を検証してから、運用スケールの使用のためにクラスターにデプロイします。

開始するには、デプロイメント環境をセットアップする必要があります。 環境のセットアップは 1 回限りのタスクです。 セットアップが完了したら、以降のデプロイのためにその環境を再利用できます。 詳細については、次のセクションを参照してください。

環境のセットアップを完了した場合:
- Azure にサインインするよう求められます。 サインインするには、Web ブラウザーを使用してページ https://aka.ms/devicelogin を開き、認証のための提供されたコードを入力します。
- 認証プロセス中に、認証するためのアカウントを入力するよう求められます。 重要: 有効な Azure サブスクリプションと、アカウントにリソースを作成するための十分なアクセス許可を持つアカウントを選択してください。 ログインが完了したら、サブスクリプション情報が表示され、選択されたアカウントで続行するかどうかを確認するメッセージが表示されます。

### <a name="environment-setup"></a>環境のセットアップ
セットアップ プロセスを開始するには、次のコマンドを入力していくつかの環境プロバイダーを登録する必要があります。

```azurecli
az provider register -n Microsoft.MachineLearningCompute
az provider register -n Microsoft.ContainerRegistry
az provider register -n Microsoft.ContainerService
```
#### <a name="local-deployment"></a>ローカル デプロイ
ローカル コンピューター上で Web サービスをデプロイおよびテストするには、次のコマンドを使用してローカル環境をセットアップします。 リソース グループ名は省略可能です。

```azurecli
az ml env setup -l [Azure Region, e.g. eastus2] -n [your environment name] [-g [existing resource group]]
```
>[!NOTE] 
>ローカル Web サービスのデプロイでは、ローカル コンピューターに Docker をインストールする必要があります。 
>

ローカル環境のセットアップ コマンドは、サブスクリプション内に次のリソースを作成します。
- リソース グループ (指定されていない場合、または指定された名前が存在しない場合)
- ストレージ アカウント
- Azure Container Registry (ACR)
- Application Insights アカウント

セットアップが正常に完了したら、次のコマンドを使用して、使用される環境を設定します。

```azurecli
az ml env set -n [environment name] -g [resource group]
```

#### <a name="cluster-deployment"></a>クラスター デプロイ
高スケールの運用シナリオには、クラスター デプロイを使用します。 これは、Kubernetes を含む ACS クラスターをオーケストレーターとして設定します。 ACS クラスターは、Web サービス呼び出しのためのより高いスループットを処理するようにスケールアウトできます。

Web サービスを実稼働環境にデプロイするには、まず次のコマンドを使用して環境をセットアップします。

```azurecli
az ml env setup --cluster -n [your environment name] -l [Azure region e.g. eastus2] [-g [resource group]]
```

クラスター環境のセットアップ コマンドは、サブスクリプション内に次のリソースを作成します。
- リソース グループ (指定されていない場合、または指定された名前が存在しない場合)
- ストレージ アカウント
- Azure Container Registry (ACR)
- Azure Container Service (ACS) クラスター上の Kubernetes デプロイ
- Application Insights アカウント

>[!IMPORTANT]
> クラスター環境を正常に作成するには、Azure サブスクリプションまたはリソース グループに対する共同作成者アクセス権を持っている必要があります。

リソース グループ、ストレージ アカウント、および ACR はすぐに作成されます。 ACS のデプロイには、最大 20 分かかる場合があります。 

進行中のクラスター プロビジョニングの状態を確認するには、次のコマンドを使用します。

```azurecli
az ml env show -n [environment name] -g [resource group]
```

セットアップが完了したら、このデプロイに使用される環境を設定する必要があります。 次のコマンドを使用します。

```azurecli
az ml env set -n [environment name] -g [resource group]
```

>[!NOTE] 
> 環境が作成された後、以降のデプロイでは、上の設定コマンドを使用してそれを再利用するだけで済みます。
>

### <a name="create-a-model-management-account"></a>モデル管理アカウントを作成する
モデルをデプロイするには、モデル管理アカウントが必要です。 これをサブスクリプションごとに 1 回実行する必要があり、複数のデプロイで同じアカウントを再利用できます。

新しいアカウントを作成するには、次のコマンドを使用します。

```azurecli
az ml account modelmanagement create -l [Azure region, e.g. eastus2] -n [your account name] -g [resource group name] --sku-instances [number of instances, e.g. 1] --sku-name [Pricing tier for example S1]
```

既存のアカウントを使用するには、次のコマンドを使用します。
```azurecli
az ml account modelmanagement set -n [your account name] -g [resource group it was created in]
```

### <a name="deploy-your-model"></a>モデルをデプロイする
これで、保存されたモデルを Web サービスとしてデプロイする準備ができました。 

```azurecli
az ml service create realtime --model-file [model file/folder path] -f [scoring file e.g. score.py] -n [your service name] -s [schema file e.g. service_schema.json] -r [runtime for the Docker container e.g. spark-py or python] -c [conda dependencies file for additional python packages]
```

## <a name="next-steps"></a>次の手順
ギャラリーにある多数のサンプルのうちの 1 つを試してください。
