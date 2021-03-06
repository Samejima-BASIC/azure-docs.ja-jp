---
title: "Azure Cosmos DB でのアクセス制御の設定 | Microsoft Docs"
description: "Azure Cosmos DB でアクセス制御を設定する方法について説明します。"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: fae3af3f-4d6e-46d8-9d9b-b80a4218add9
ms.service: cosmos-db
ms.topic: article
ms.date: 02/06/2018
ms.author: mimig
ms.openlocfilehash: 5ef4a12c8229d8801a5d9749514a69c2c1e19499
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/21/2018
---
# <a name="access-control-in-azure-cosmos-db"></a>Azure Cosmos DB でのアクセス制御

ユーザー アカウントに Azure Cosmos DB アカウントの閲覧者アクセス権を追加するには、サブスクリプションの所有者が Azure Portal で以下の手順を実行します。

1. Azure Portal を開き、Azure Cosmos DB アカウントを選択します。
2. **[アクセス制御 (IAM)]** をクリックし、**[+ 追加]** をクリックします。
3. **[アクセス許可の追加]** ウィンドウの **[ロール]** ボックスで、**[Cosmos DB アカウントの閲覧者ロール]** を選択します。
4. **[アクセスの割り当て先]** ボックスで、**[Azure AD のユーザー、グループ、またはアプリケーション]** を選択します。
5. ディレクトリで、アクセス権を付与するユーザー、グループ、またはアプリケーションを選択します。  ディレクトリは、表示名、電子メール アドレス、およびオブジェクト識別子を使用して検索できます。
    選択したメンバー、グループ、またはアプリケーションが、選択したメンバー一覧に表示されます。
6. **[保存]** をクリックします。

そのエンティティが Azure Cosmos DB リソースを閲覧できるようになります。
