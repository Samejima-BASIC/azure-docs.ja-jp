---
title: "Azure CLI のサンプル スクリプト - プレフィックスでコンテナーを削除する | Microsoft Docs"
description: "コンテナー名のプレフィックスに基づいて Azure Storage Blob コンテナーを削除します。"
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 06/22/2017
ms.author: tamram
ms.openlocfilehash: 3eab1878c2a5f6b1d031ef3208e30a4df19dc41e
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2018
---
# <a name="delete-containers-based-on-container-name-prefix"></a>コンテナー名のプレフィックスに基づいたコンテナーの削除

このスクリプトでは、まず Azure Blob ストレージにいくつかのサンプルのコンテナーを作成してから、コンテナー名のプレフィックスに基づいてコンテナーの一部を削除します。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>サンプル スクリプト

[!code-azurecli-interactive[main](../../../cli_scripts/storage/delete-containers-by-prefix/delete-containers-by-prefix.sh?highlight=2-3 "Delete containers by prefix")]

## <a name="clean-up-deployment"></a>デプロイのクリーンアップ 

次のコマンドを実行して、リソース グループ、残りのコンテナー、すべての関連リソースを削除します。

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>スクリプトの説明

このスクリプトでは、次のコマンドを使用して、コンテナー名のプレフィックスに基づいてコンテナーを削除します。 表内の各項目は、コマンドごとのドキュメントにリンクされています。

| コマンド | メモ |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | すべてのリソースを格納するリソース グループを作成します。 |
| [az storage account create](/cli/azure/storage/account#az_storage_account_create) | 特定のリソース グループに Azure Storage アカウントを作成します。 |
| [az storage container create](/cli/azure/storage/container#az_storage_container_create) | Azure Blob ストレージにコンテナーを作成します。 |
| [az storage container list](/cli/azure/storage/container#az_storage_container_list) | Azure Storage アカウントのコンテナーを一覧表示します。 |
| [az storage container delete](/cli/azure/storage/container#az_storage_container_delete) | Azure Storage アカウントのコンテナーを削除します。 |

## <a name="next-steps"></a>次の手順

Azure CLI の詳細については、[Azure CLI のドキュメント](/cli/azure/overview)のページをご覧ください。

その他のストレージ CLI サンプル スクリプトは、[Azure Storage 用 Azure CLI サンプル](../blobs/storage-samples-blobs-cli.md)のページにあります。
