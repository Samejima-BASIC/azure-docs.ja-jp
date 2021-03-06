---
title: "Azure 診断ログの Log Analytics へのストリーミング | Microsoft Docs"
description: "Azure 診断ログを Log Analytics ワークスペースにストリーミングする方法について説明します。"
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: johnkem
ms.openlocfilehash: 9440bd7f872914887c1f6e50f08a3c273536fcf8
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/02/2017
---
# <a name="stream-azure-diagnostic-logs-to-log-analytics"></a>Azure 診断ログの Log Analytics へのストリーミング
Azure Log Analytics には、ポータル、PowerShell コマンドレット、Azure CLI のいずれかの方法を使い、**[Azure 診断ログ](monitoring-overview-of-diagnostic-logs.md)**をほぼリアルタイムでストリーミングすることができます。

## <a name="what-you-can-do-with-diagnostics-logs-in-log-analytics"></a>Log Analytics から診断ログを使ってできること

Azure Log Analytics は、ログの検索と分析に対応したフレキシブルなツールです。Azure リソースから生成された生のログ データから洞察を得ることができます。 いくつかの機能の例を次に示します。

* **ログ検索** - ログ データに対する高度なクエリを記述して、各種ソースからのログを相互に関連付けます。また、Azure ダッシュボードにピン留め可能なグラフを生成することもできます。
* **アラート** - 特定のクエリにイベントが一致したことを検出し、メールまたは webhook 呼び出しで通知します。
* **ソリューション** - 既製のビューとダッシュボードを使用して、すぐにログ データから洞察を得ることができます。
* **高度な分析** - 機械学習とパターン マッチング アルゴリズムを適用して、ログから潜在的な問題を特定します。

## <a name="enable-streaming-of-diagnostic-logs-to-log-analytics"></a>Log Analytics への診断ログのストリーミングを有効にする
診断ログのストリーミングは、プログラム、ポータル、または [Azure Monitor REST API](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings) を使用して有効にすることができます。 どの方法でも、Log Analytics ワークスペースと、そこに送信するログのカテゴリおよびメトリックを指定する診断設定を作成します。 診断**ログ カテゴリ**とは、リソースから得られるログの種類です。

設定を構成するユーザーが両方のサブスクリプションに対して適切な RBAC アクセスを持っている限り、Log Analytics ワークスペースはログを出力するリソースと同じサブスクリプションに属している必要はありません。

## <a name="stream-diagnostic-logs-using-the-portal"></a>ポータルを使用して診断ログをストリーミングする
1. ポータルで、Azure Monitor に移動し、**[診断設定]** をクリックします。

    ![Azure Monitor の [監視] セクション](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-blade.png)

2. 必要に応じて、リソース グループまたはリソースの種類で一覧をフィルタリングして、診断設定を行うリソースをクリックします。

3. 選択したリソースの設定が存在しない場合は、設定を作成するように求められます。 [診断を有効にする] をクリックします。

   ![診断設定の追加 - 既存の設定が存在しない](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-none.png)

   リソースに既存の設定が存在する場合は、このリソースで構成済みの設定の一覧が表示されます。 [診断設定の追加] をクリックします。

   ![診断設定の追加 - 既存の設定が存在する](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-multiple.png)

3. 設定に名前を付け、**[Log Analytics への送信]** チェック ボックスをオンにして、Log Analytics ワークスペースを選択します。
   
   ![診断設定の追加 - 既存の設定が存在する](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-configure.png)

4. **[ Save]** をクリックします。

しばらくすると、このリソースの設定一覧に新しい設定が表示され、新しいイベント データが生成されるとすぐに、診断ログがそのワークスペースにストリーミングされます。 イベントが生成されてから、それが Log Analytics に表示されるまでに 15 分ほどかかる場合があるので注意してください。

### <a name="via-powershell-cmdlets"></a>PowerShell コマンドレットの使用
[Azure PowerShell コマンドレット](insights-powershell-samples.md)を使用してストリーミングを有効にするには、次のパラメーターを指定して `Set-AzureRmDiagnosticSetting` コマンドレットを使用します。

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource ID] -WorkspaceID [resource ID of the Log Analytics workspace] -Categories [list of log categories] -Enabled $true
```

workspaceID プロパティに指定するのは、ワークスペースの完全な Azure リソース ID です。Log Analytics ポータルに表示されるワークスペース ID/キーではないので注意してください。

### <a name="via-azure-cli"></a>Azure CLI の使用
[Azure CLI](insights-cli-samples.md) を使用してストリーミングを有効にするには、次のように `insights diagnostic set` コマンドを使用します。

```azurecli
azure insights diagnostic set --resourceId <resourceID> --workspaceId <workspace resource ID> --categories <list of categories> --enabled true
```

workspaceId プロパティに指定するのは、ワークスペースの完全な Azure リソース ID です。Log Analytics ポータルに表示されるワークスペース ID/キーではないので注意してください。

## <a name="how-do-i-query-the-data-in-log-analytics"></a>Log Analytics 内のデータを照会する方法

ポータルの [ログ検索] ブレードまたは Log Analytics の機能である [高度な分析] から、Log Management ソリューションの範囲内の診断ログを AzureDiagnostics テーブルで照会することができます。 他にも、[Azure リソース向けのソリューションがいくつか](../log-analytics/log-analytics-add-solutions.md)存在します。こうしたソリューションをインストールすることで、Log Analytics に送信中のログ データからすぐに洞察を得ることができます。


## <a name="next-steps"></a>次のステップ
* [Azure 診断ログの詳細を確認する](monitoring-overview-of-diagnostic-logs.md)
