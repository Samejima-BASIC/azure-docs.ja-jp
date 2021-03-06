---
title: "セルフサービスによるパスワード リセット レポート - Azure Active Directory"
description: "Azure AD のセルフサービスによるパスワード リセット イベントのレポート"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro;seohack1
ms.openlocfilehash: ae9475218141f4fdf08ef6b39fb85f3b18711e61
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/17/2018
---
# <a name="reporting-options-for-azure-ad-password-management"></a>Azure AD のパスワード管理に関するレポート オプション

多くの組織は、デプロイ後に、セルフサービスによるパスワードのリセット (SSPR) が本当に使用されているかどうかや、どのように使用されているかを把握することを望んでいます。 Azure Active Directory (Azure AD) で提供されるレポート機能によって、お客様は質問に対する答えをあらかじめ用意されたレポートから得ることができます。 適切にライセンスを付与されている場合は、カスタム クエリを作成することもできます。

![レポート][Reporting]

次の質問に対する答えは、[Azure Portal] (https://portal.azure.com/) に用意されているレポートから得られます。

> [!NOTE]
> ユーザーは[グローバル管理者](active-directory-assign-admin-roles-azure-portal.md)であること、および組織の代表としてこのデータを収集できるようにオプトインすることが必要です。 オプトインするには、**[レポート]** タブまたは監査ログに少なくとも 1 回アクセスする必要があります。 それまでは、ご自分の組織のデータが収集されることはありません。
>

* パスワード リセットを登録した人数
* パスワード リセットを登録したユーザー
* ユーザーが登録しているデータ
* 過去 7 日間で自分のパスワードをリセットしたユーザー数
* パスワードをリセットするためにユーザーまたは管理者がもっとも使用する方法
* パスワード リセットを試みる場合に、ユーザーまたは管理者が直面する一般的な問題
* 自らのパスワードを頻繁にリセットしている管理者
* パスワード リセットに関する不審なアクティビティの有無

## <a name="power-bi-content-pack"></a>Power BI コンテンツ パック

Power BI ユーザーには、使いやすい SSPR レポートを含む Azure AD 用のコンテンツ パックが用意されています。 コンテンツ パックを使用およびデプロイする方法の詳細については、「[Azure Active Directory Power BI コンテンツ パックの使用方法](active-directory-reporting-power-bi-content-pack-how-to.md)」をご覧ください。 コンテンツ パックを使用すると、独自のダッシュボードを作成して組織内で共有できます。

## <a name="how-to-view-password-management-reports-in-the-azure-portal"></a>Azure Portal でパスワード管理レポートを表示する方法

Azure Portal エクスペリエンスでは、パスワード リセットおよびパスワード リセット登録アクティビティの表示方法が改良されています。 パスワード リセットおよびパスワード リセット登録イベントを表示するには、次の手順に従います。

1. [Azure ポータル](https://portal.azure.com)にアクセスします。
2. 左側のウィンドウにある **[すべてのサービス]** を選択します。
3. サービスの一覧で、**[Azure Active Directory]** を探して選択します。
4. **[ユーザーとグループ]** を選択します。
5. **[ユーザーとグループ]** メニューから **[監査ログ]** を選択します。 ディレクトリ内のすべてのユーザーに対して発生したすべての監査イベントが表示されます。 このビューをフィルター処理して、すべてのパスワード関連イベントを表示できます。
6. パスワード リセット関連イベントのみが表示されるようにこのビューをフィルター処理するには、ウィンドウの上部にある **[フィルター]** ボタンを選択します。
7. **[フィルター]** メニューから **[カテゴリ]** ボックスを選択し、カテゴリの種類を **[Self-service Password Management]\(セルフサービスのパスワード管理\)** に変更します。
8. 必要な場合は、関心のある特定の **[アクティビティ]** を選択して、リストをさらにフィルター処理します。

## <a name="how-to-retrieve-password-management-events-from-the-azure-ad-reports-and-events-api"></a>Azure AD レポートおよびイベント API からパスワード管理イベントを取得する方法

Azure AD レポートおよびイベント API では、パスワード リセットとパスワード リセット登録のレポートに含まれるすべての情報の取得がサポートされています。 この API を使用すると、パスワード リセットとパスワード リセット登録の個々のイベントをダウンロードして任意のレポート テクノロジと統合できます。

> [!IMPORTANT]
> 現時点では、Azure AD レポートおよびイベント API で取得できるのは、[SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent) タイプと [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent) タイプの最大 *75,000 個のイベント*です。 この API は、過去 30 日間が対象となります。
> 
> この期間を超えてデータを取得および保存する必要がある場合は、この API を使用してデータを外部データベースに保存し、クエリを実行して差分を得ることをお勧めします。 ご自分の組織で初めて SSPR を使用するときに、このデータの取得を開始することをお勧めします。 それを外部に保存したら、その時点から差分を追跡します。
>

### <a name="how-to-get-started-with-the-reporting-api"></a>レポート API の使用を開始する方法

このデータにアクセスするには、Microsoft のサーバーからそのデータを取得するための、小規模なアプリケーションまたはスクリプトを作成する必要があります。 詳細については、[Azure AD レポート API の概要](active-directory-reporting-api-getting-started.md)に関する記事をご覧ください。

使用できるスクリプトの準備ができたら、取得可能なパスワードのリセットおよび登録イベントがお客様のシナリオに合っているか確認するとよいでしょう。

* [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent): パスワード リセット イベントで利用可能な列を一覧表示します。
* [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent): パスワード リセット登録イベントで利用可能な列を一覧表示します。

## <a name="description-of-the-report-columns-in-the-azure-portal"></a>Azure Portal でのレポートの列の説明

次の一覧では、Azure Portal でのレポートの各列について詳細に説明します。

* **ユーザー**: パスワード リセット登録操作を試みたユーザー。
* **ロール**: ディレクトリ内のユーザーの役割。
* **日付と時刻**: 試行の日付と時刻。
* **登録データ**: パスワード リセット登録中にユーザーが入力した認証データ。

## <a name="description-of-the-report-values-in-the-azure-portal"></a>Azure Portal でのレポートの値の説明

次の表では、Azure Portal 内の各列に設定できる、さまざまな値について説明します。

| 分割 | 使用できる値とその意味 |
| --- | --- |
| 登録データ |**連絡用メール アドレス**: ユーザーが、認証のために連絡用メールまたは認証用メールを使用しました。<p><p>**会社電話**: ユーザーが、認証のために会社電話を使用しました。<p>**携帯電話**: ユーザーが、認証のために携帯電話または認証用の電話を使用しました。<p>**秘密の質問**: ユーザーが、認証のために秘密の質問を使用しました。<p>**前述の方法の任意の組み合わせ (連絡用メール アドレス + 携帯電話など)**: 2 ゲート ポリシーを指定した場合に発生し、パスワード リセット要求を認証するためにユーザーがどの方法を使用したかを示します。 |

## <a name="self-service-password-management-activity-types"></a>[Self-Service Password Management]\(セルフサービスのパスワード管理\) のアクティビティの種類

**[Self-Service Password Management]\(セルフサービスのパスワード管理\)** 監査イベント カテゴリには、次のアクティビティの種類が表示されます。

* [Blocked from self-service password reset (セルフサービスによるパスワードのリセットのブロック)](#activity-type-blocked-from-self-service-password-reset): ユーザーが 24 時間以内に合計 5 回を超えて、パスワードのリセット、特定のゲートの使用、または電話番号の確認を試行したことを示します。
* [Change password (self-service) (パスワードの変更 (セルフサービス))](#activity-type-change-password-self-service): ユーザーが自発的または (期限切れにより) 強制的にパスワードの変更を実行したことを示します。
* [Reset password (by admin) (パスワードのリセット (管理者))](#activity-type-reset-password-by-admin): 管理者が Azure Portal からユーザーに代わってパスワードのリセットを実行したことを示します。
* [Reset password (self-service) (パスワードのリセット (セルフサービス))](#activity-type-reset-password-self-service): ユーザーが [Azure AD のパスワード リセット ポータル](https://passwordreset.microsoftonline.com)から自分のパスワードのリセットに成功したことを示します。
* [Self-service password reset flow activity progress (セルフサービスのパスワード リセット フロー アクティビティの進行状況)](#activity-type-self-serve-password-reset-flow-activity-progress): ユーザーがパスワード リセット プロセスの一環として実行する個々の特定の手順 (特定のパスワード リセット認証ゲートの通過など) を示します。
* [Unlock user account (self-service) (ユーザー アカウントのロック解除 (セルフサービス))](#activity-type-unlock-user-account-self-service): ユーザーが [Azure AD のパスワード リセット ポータル](https://passwordreset.microsoftonline.com)から、リセットなしのアカウント ロック解除の Active Directory 機能を使用して、自分のパスワードをリセットせずに自分の Active Directory アカウントのロック解除に成功したことを示します。
* [User registered for self-service password reset (セルフサービスによるパスワードのリセットを登録したユーザー)](#activity-type-user-registered-for-self-service-password-reset): ユーザーが、現在指定されているテナント パスワード リセット ポリシーに従って、自分のパスワードをリセットするために必要なすべての情報を登録したことを示します。

### <a name="activity-type-blocked-from-self-service-password-reset"></a>アクティビティの種類: Blocked from self-service password reset (セルフサービスのパスワード リセットのブロック)

このアクティビティの詳しい説明は次のとおりです。

* **アクティビティの説明**: ユーザーが 24 時間以内に合計 5 回を超えて、パスワードのリセット、特定のゲートの使用、または電話番号の確認を試行したことを示します。
* **アクティビティのアクター**: 追加のリセット操作の実行を禁止されたユーザー。 このユーザーは、エンド ユーザーまたは管理者である可能性があります。
* **アクティビティのターゲット**: 追加のリセット操作の実行を禁止されたユーザー。 このユーザーは、エンド ユーザーまたは管理者である可能性があります。
* **アクティビティの状態**:
  * _成功_: ユーザーが今後 24 時間の間、追加のリセットの実行、追加の認証方法の試行、または追加の電話番号の確認を禁止されたことを示します。
* **アクティビティの状態が失敗になった理由**: 該当なし。

### <a name="activity-type-change-password-self-service"></a>アクティビティの種類: Change password (self-service) (パスワードの変更 (セルフサービス))

このアクティビティの詳しい説明は次のとおりです。

* **アクティビティの説明**: ユーザーが自発的または (期限切れにより) 強制的にパスワードの変更を実行したことを示します。
* **アクティビティのアクター**: 自分のパスワードを変更したユーザー。 このユーザーは、エンド ユーザーまたは管理者である可能性があります。
* **アクティビティのターゲット**: 自分のパスワードを変更したユーザー。 このユーザーは、エンド ユーザーまたは管理者である可能性があります。
* **アクティビティの状態**:
  * _成功_: ユーザーが自分のパスワードの変更に成功したことを示します。
  * _失敗_: ユーザーが自分のパスワードの変更に失敗したことを示します。 この行を選択すると、**[Activity Status Reason]\(アクティビティの状態の理由\)** カテゴリが表示され、失敗の原因について詳しく知ることができます。
* **アクティビティの状態が失敗になった理由**: 
  * _FuzzyPolicyViolationInvalidPassword_: ユーザーが選択したパスワードが、Microsoft 禁止パスワード検出機能によって一般的すぎるパスワードまたは特に脆弱なパスワードと見なされたため、自動的に禁止されました。

### <a name="activity-type-reset-password-by-admin"></a>アクティビティの種類: Reset password (by admin) (パスワードのリセット (管理者))

このアクティビティの詳しい説明は次のとおりです。

* **アクティビティの説明**: 管理者が Azure Portal からユーザーに代わってパスワードのリセットを実行したことを示します。
* **アクティビティのアクター**: 別のエンド ユーザーまたは管理者に代わってパスワードのリセットを実行した管理者。 グローバル管理者、パスワード管理者、ユーザー管理者、ヘルプデスク管理者のいずれかである可能性があります。
* **アクティビティのターゲット**: 自分のパスワードがリセットされたユーザー。 このユーザーは、エンド ユーザーまたは別の管理者である可能性があります。
* **アクティビティの状態**:
  * _成功_: 管理者がユーザーのパスワードのリセットに成功したことを示します。
  * _失敗_: 管理者がユーザーのパスワードの変更に失敗したことを示します。 この行を選択すると、**[Activity Status Reason]\(アクティビティの状態の理由\)** カテゴリが表示され、失敗の原因について詳しく知ることができます。

### <a name="activity-type-reset-password-self-service"></a>アクティビティの種類: Reset password (self-service) (パスワードのリセット (セルフサービス))

このアクティビティの詳しい説明は次のとおりです。

* **アクティビティの説明**: ユーザーが[Azure AD のパスワード リセット ポータル](https://passwordreset.microsoftonline.com)から自分のパスワードのリセットに成功したことを示します。
* **アクティビティのアクター**: 自分のパスワードをリセットしたユーザー。 このユーザーは、エンド ユーザーまたは管理者である可能性があります。
* **アクティビティのターゲット**: 自分のパスワードをリセットしたユーザー。 このユーザーは、エンド ユーザーまたは管理者である可能性があります。
* **アクティビティの状態**:
  * _成功_: ユーザーが自分のパスワードのリセットに成功したことを示します。
  * _失敗_: ユーザーが自分のパスワードのリセットに失敗したことを示します。 この行を選択すると、**[Activity Status Reason]\(アクティビティの状態の理由\)** カテゴリが表示され、失敗の原因について詳しく知ることができます。
* **アクティビティの状態が失敗になった理由**: 
  * _FuzzyPolicyViolationInvalidPassword_: 管理者が選択したパスワードが、Microsoft 禁止パスワード検出機能によって一般的すぎるパスワードまたは特に脆弱なパスワードと見なされたため、自動的に禁止されました。

### <a name="activity-type-self-serve-password-reset-flow-activity-progress"></a>アクティビティの種類: Self serve password reset flow activity progress (セルフサービスのパスワード リセット フロー アクティビティの進行状況)

このアクティビティの詳しい説明は次のとおりです。

* **アクティビティの説明**: ユーザーがパスワード リセット プロセスの一環として実行する個々の特定の手順 (特定のパスワード リセット認証ゲートの通過など) を示します。
* **アクティビティのアクター**: パスワード リセット フローの一部を実行したユーザー。 このユーザーは、エンド ユーザーまたは管理者である可能性があります。
* **アクティビティのターゲット**: パスワード リセット フローの一部を実行したユーザー。 このユーザーは、エンド ユーザーまたは管理者である可能性があります。
* **アクティビティの状態**:
  * _成功_: ユーザーがパスワード リセット フローの特定の手順に成功したことを示します。
  * _失敗_: パスワード リセット フローの特定の手順が失敗したことを示します。 この行を選択すると、**[Activity Status Reason]\(アクティビティの状態の理由\)** カテゴリが表示され、失敗の原因について詳しく知ることができます。
* **アクティビティの状態の理由**: [使用できるすべてのリセット アクティビティ状態の理由](#allowed-values-for-details-column)については、次の表をご覧ください。

### <a name="activity-type-unlock-a-user-account-self-service"></a>アクティビティの種類: Unlock user account (self-service) (ユーザー アカウントのロック解除 (セルフサービス))

このアクティビティの詳しい説明は次のとおりです。

* **アクティビティの説明**: ユーザーが [Azure AD のパスワード リセット ポータル](https://passwordreset.microsoftonline.com)から、リセットなしのアカウント ロック解除の Active Directory 機能を使用して、自分のパスワードをリセットせずに自分の Active Directory アカウントのロック解除に成功したことを示します。
* **アクティビティのアクター**: 自分のパスワードをリセットせずに自分のアカウントをロック解除したユーザー。 このユーザーは、エンド ユーザーまたは管理者である可能性があります。
* **アクティビティのターゲット**: 自分のパスワードをリセットせずに自分のアカウントをロック解除したユーザー。 このユーザーは、エンド ユーザーまたは管理者である可能性があります。
* **使用可能なアクティビティ状態**:
  * _成功_: ユーザーが自分のアカウントのロック解除に成功したことを示します。
  * _失敗_: ユーザーが自分のアカウントのロック解除に失敗したことを示します。 この行を選択すると、**[Activity Status Reason]\(アクティビティの状態の理由\)** カテゴリが表示され、失敗の原因について詳しく知ることができます。

### <a name="activity-type-user-registered-for-self-service-password-reset"></a>アクティビティの種類: User registered for self-service password reset (セルフサービスのパスワード リセットを登録したユーザー)

このアクティビティの詳しい説明は次のとおりです。

* **アクティビティの説明**: ユーザーが、現在指定されているテナント パスワード リセット ポリシーに従って、自分のパスワードをリセットするために必要なすべての情報を登録したことを示します。 
* **アクティビティのアクター**: パスワード リセットに登録したユーザー。 このユーザーは、エンド ユーザーまたは管理者である可能性があります。
* **アクティビティのターゲット**: パスワード リセットに登録したユーザー。 このユーザーは、エンド ユーザーまたは管理者である可能性があります。
* **使用可能なアクティビティ状態**:
  * _成功_: ユーザーが現在のポリシーに従ってパスワード リセットの登録に成功したことを示します。 
  * _失敗_: ユーザーがパスワード リセットの登録に失敗したことを示します。 この行を選択すると、**[Activity Status Reason]\(アクティビティの状態の理由\)** カテゴリが表示され、失敗の原因について詳しく知ることができます。 

     >[!NOTE]
     >失敗は、ユーザーが自分のパスワードをリセットできないことを意味するわけではありません。 登録プロセスを完了していないことを意味します。 ユーザーのアカウントに未確認の正しいデータ (未確認の電話番号など) がある場合、そのデータを確認していなくても、パスワードのリセットに使用できます。 詳細については、[ユーザーの登録時に発生すること](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-happens-when-a-user-registers)に関するページをご覧ください。
     >

## <a name="next-steps"></a>次の手順

* [SSPR のロールアウトを正常に完了する方法](active-directory-passwords-best-practices.md)
* [パスワードのリセットと変更。](active-directory-passwords-update-your-own-password.md)
* [セルフサービスによるパスワード リセットの登録。](active-directory-passwords-reset-register.md)
* [ライセンスに関する質問](active-directory-passwords-licensing.md)
* [SSPR が使用するデータと、ユーザー用に事前設定が必要なデータ。](active-directory-passwords-data.md)
* [ユーザーが使用できる認証方法。](active-directory-passwords-how-it-works.md#authentication-methods)
* [SSPR のポリシー オプション。](active-directory-passwords-policy.md)
* [パスワード ライトバックの概要とその必要性。](active-directory-passwords-writeback.md)
* [SSPR のすべてのオプションとその意味。](active-directory-passwords-how-it-works.md)
* [不具合が発生していると思われる場合のSSPR のトラブルシューティング方法。](active-directory-passwords-troubleshoot.md)
* [質問したい内容に関する説明がどこにもない。](active-directory-passwords-faq.md)

[Reporting]: ./media/active-directory-passwords-reporting/sspr-reporting.png "Azure AD の SSPR アクティビティ監査ログの例"
