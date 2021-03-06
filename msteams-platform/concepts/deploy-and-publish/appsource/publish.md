---
title: Microsoft Teams アプリ承認提出プロセスガイダンス
description: Microsoft Teams アプリストアに公開されたアプリを取得するための送信承認プロセスについて説明します。
keywords: teams 発行ストア office publishing 発行 AppSource partner center アカウント検証アプリアカウントが対象として公開されていません
ms.openlocfilehash: 2879717aebd9d295cdf37cc0371b68f5f695b86b
ms.sourcegitcommit: 3fc7ad33e2693f07170c3cb1a0d396261fc5c619
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2020
ms.locfileid: "48796310"
---
# <a name="submit-your-app-to-appsource"></a>アプリを AppSource に提出する

## <a name="teams-app-submission"></a>Teams アプリの提出

アプリを [Appsource](https://appsource.microsoft.com) に発行すると、Teams アプリカタログと web 上で使用できるようになります。 高レベルでは、アプリを AppSource に提出するプロセスは次のようになります。

1. [デザインガイドライン](~/concepts/design/understand-use-cases.md)に従ってアプリを開発します。 タブは、 [タブデザインガイドライン](~/tabs/design/tabs.md)に準拠している必要があります。 Bot は [bot 設計ガイドライン](~/bots/design/bots.md)に従う必要があります。
1. アプリが Microsoft Teams のアプリの [検証ポリシー](/legal/marketplace/certification-policies) を満たしていることを確認します。 
1. [マニフェスト検証ツール](https://docs.microsoft.com/microsoftteams/platform/concepts/deploy-and-publish/appsource/prepare/submission-checklist#teams-app-validation-tool)を使用してアプリを自己テストします。
1. [パートナーセンター](https://support.microsoft.com/help/4499930/partner-center-overview)で[開発者アカウントをセットアップ](/office/dev/store/open-a-developer-account)します。 下記の FAQ セクションで、 [パートナーセンターアカウントを作成する方法](#how-do-i-create-a-partner-center-account)に *ついても参照して* ください。
1. [提出のチェックリスト](~/concepts/deploy-and-publish/appsource/prepare/submission-checklist.md)に従って、アプリを提出する準備をします。
1. [アプリの送信を成功させるためのヒントを](~/concepts/deploy-and-publish/appsource/prepare/frequently-failed-cases.md)確認します。
1. [パートナーセンターを使用して、Appsource](/office/dev/store/use-partner-center-to-submit-to-appsource)にパッケージを提出します。
1. パートナーセンターダッシュボードで承認プロセスを追跡します。 *「* [Partner Center の概要](https://support.microsoft.com/help/4499930/partner-center-overview)」を参照してください。
1. 投稿後: 公開された [アプリの管理とサポート](~/concepts/deploy-and-publish/appsource/post-publish/overview.md)に関するガイダンスを確認してください。

>[!NOTE]
>
>- Teams アプリに bot が含まれている場合は、ボット開発者フレームワーク [の倫理](https://aka.ms/bf-conduct)規定に準拠する必要があります。
>- アプリに Office 365 コネクタが含まれている場合は、追加の用語が適用されることがあります。 *「* [コネクタ開発者ダッシュボード](https://aka.ms/connectorsdashboard) と [アプリ開発者アグリーメント](https://sellerdashboard.microsoft.com/Assets/Content/Agreements/Office_Store_Seller_Agreement_20120927.htm)」を参照してください。
>- アプリを GCC ユーザーが使用できるようにし、ストア内のアプリリストが重複しないようにするには、認証プロセス/フローでは、ユーザーを識別し、GCC ユーザーの指定されたコンテンツ URL または予想されるコンテンツ URL にルーティングする必要があります。

## <a name="faqs--teams-apps-and-partner-account-verification-process-in-partner-center"></a>よくあるご質問-Teams アプリとパートナーアカウント検証プロセスパートナーセンター

## <a name="how-do-i-create-a-partner-center-account"></a>パートナーセンターアカウントを作成するにはどうすればよいですか?

パートナーセンターアカウントを作成するには、次の2つの方法があります。

- パートナーセンターを初めて使用していて、Microsoft ネットワーク内にアカウントがない場合は、 [パートナーセンターの登録ページを使用してアカウントを作成](/office/dev/store/open-a-developer-account#create-an-account-using-an-existing-partner-center-enrollment)します。
- パートナーネットワークに既に登録されている場合は、 [既存の登録を使用してパートナーセンターで直接アカウントを作成](/office/dev/store/)します。

## <a name="what-if-i-cannot-find-my-office-store-account-in-partner-center"></a>パートナーセンターで Office ストアアカウントが見つからない場合はどうすればよいですか?

[パートナーセンターのサポートチケット](https://partner.microsoft.com/support/v2/?stage=1)を開いて、ドロップダウンメニューから次のものを選択してください。

| メニュー | オプション |
| -------   | -------  |
|カテゴリ| コマーシャル市場|
| トピック | 一般的な Marketplace のヘルプと操作方法に関する質問 |
| サブ| Office アドイン |

## <a name="where-can-i-get-support-for-my-partner-center-account-issues"></a>パートナーセンターのアカウントの問題をサポートするには、どうすればよいですか?

「 [Publishers support」ページ](https://aka.ms/marketplacepublishersupport) にアクセスして、問題のトピックを検索し、ガイダンスを見つけてください。 提供されているガイダンスが役に立たない場合は、 [パートナーセンターのサポートチケットを開き](/azure/marketplace/partner-center-portal/support#how-to-open-a-support-ticket)ます。

## <a name="how-do-i-manage-my-office-store-account-in-partner-center"></a>パートナーセンターで Office ストアアカウントを管理するにはどうすればよいですか?

パートナーセンターで Office ストアアカウントを管理するためのガイダンスについては、「  [Partner center で Office ストアアカウントを管理する」](/office/dev/store/manage-account-settings-and-profile) を参照してください。

## <a name="how-do-i-add-my-phone-number-to-the-partner-profile-contact-section"></a>パートナープロファイルの連絡先セクションに電話番号を追加するにはどうすればよいですか?

電話番号には、国番号、市外局番、電話番号の3つのパーツがあります。 電話番号に市外局番が含まれていない場合は、2番目のボックスを空のままにして、3番目のボックスに入力します。

## <a name="how-do-i-manage-my-account-settings-and-partner-profile-in-partner-center"></a>パートナーセンターでアカウントの設定とパートナーのプロファイルを管理するにはどうすればよいですか?

パートナーセンターのアカウント設定を管理するためのガイダンスについては、「 [Manage account settings and profile info](/windows/uwp/publish/manage-account-settings-and-profile#additional-settings-and-info) page」を参照してください。

## <a name="why-do-i-receive-the-message-this-account-is-not-publish-eligible-when-i-try-to-submit-my-add-in-through-partner-center"></a>パートナーセンターからアドインを送信しようとすると、"このアカウントは公開されていません" というメッセージが表示されるのはなぜですか?

[アカウントの確認状態](/partner-center/verification-responses)が保留中の場合、上記のエラーメッセージが表示されます。 パートナーセンターの [ダッシュボード](https://partner.microsoft.com/dashboard)で [ **設定** ] オプション (歯車アイコン) を選択し、[ **開発者設定** ] [  =>  **アカウント**   =>  **アカウント設定** ] を選択することによって、アカウントの確認状態を確認できます。

![パートナーセンターのアカウント設定ページ](../../../assets/images/partner-center-accts-page.png)

![パートナーセンターの確認状態](../../../assets/images/partner-center-verification-status.png)

アカウント検証プロセスの間に、各必要な手順の状態 (電子メールの所有権、雇用の確認、およびビジネスの確認) が表示されます。 検証プロセスが正常に完了すると、プロファイルページの登録の検証状態が "保留" から "承認済み" に変わり、プロセスの手順は表示されなくなります。

![パートナーセンターの検証エラー](../../../assets/images/partner-center-acct-verification-error.png)

## <a name="what-is-verified-in-partner-center-account-verification-process-and-how-to-respond"></a>パートナーセンターのアカウント検証プロセスと応答の確認方法
3つの検証領域には、電子メールの所有権、雇用、ビジネスがあります。 主な連絡先 (グローバル管理者またはアカウント管理者) で [ある場合は](/partner-center/verification-responses#what-is-verified-and-how-to-respond) 、パートナープロファイルに移動して検証の状態を監視し、進行状況を追跡することをお勧めします。

検証プロセスが完了すると、プロファイルページの登録の検証状態が [ *保留中* ] から [ *承認済み* ] に変更され、そのページに表示されている状態のプロセスステップが非表示になります。 プライマリ連絡先は、確認が完了した後、数営業日以内に Microsoft からの電子メールを受け取ります。

## <a name="my-account-verification-status-has-not-advanced-beyond-email-ownership-in-partner-center-how-should-i-proceed"></a>自分のアカウントの確認状態は、パートナーセンターの電子メールの所有権を超えていません。 続行するにはどうすればよいですか?

**電子メールの所有権** の確認プロセスでは、確認メールがプライマリ連絡先の電子メールアドレスに送信されます。 プライマリ連絡先の受信トレイで **maccount@ <span>microsoft</span>.com** からの電子メールが必要です。そのためには、「件名を付けてください。」というアクションを必要とします。電子メール *アカウントが microsoft によって確認* され、電子メールの確認プロセスが完了することを要求します。 確認メールは、パートナーセンターの [アカウント設定] ページに表示されている電子メールアドレスに送信されます。

> [!NOTE]
 >[電子メールの確認のリンクは、7日間のみ有効です。 パートナープロファイルページにアクセスして、[ **再送信の確認** ] の電子メールリンクを選択することによって、電子メールを送信するように要求することができます。 電子メールが受信されることを確認するには、microsoft.com からのセキュリティで保護されたドメインとして電子メールのセーフリストを作成し、迷惑メールフォルダーをチェックします。

## <a name="how-i-do-get-further-support-for-my-account-related-issues"></a>自分のアカウント関連の問題について、さらにサポートを受けるにはどうすればよいですか?

サポートチケットを作成するためのガイダンスと手順については、「パートナーセンター」のページの「 [コマーシャル市場プログラムのサポート」](/azure/marketplace/partner-center-portal/support) を参照してください。

## <a name="ive-checked-my-mail-folders-and-havent-received-the-verification-email-what--should-i-do-next"></a>メールフォルダーをチェックしていて、確認の電子メールを受信していません。 次の手順を実行します。

次のことを試してください。

1. 迷惑メールフォルダーを確認してください。
1. ブラウザーのキャッシュをクリアし、パートナーセンターのアカウントダッシュボードに移動し、[ **再送信確認** ] 電子メールリンクを選択して、確認の電子メールを電子メールアドレスに送信します。
1. 別のブラウザーから [  **再送信の確認の電子メール** へのアクセスを試行してください。
1. IT 部門と協力して、電子メールサーバーによって検証メールがブロックされないようにします。
1. サーバーのスパムフィルターを調整して、maccount@microsoft からのすべての電子メールを許可/セーフリストにし **ます。 <span></span>com** 。

## <a name="how-long-does-the-employment-verification-process-usually-take"></a>雇用確認プロセスには、通常どのくらいの時間がかかりますか?

送信されたすべての詳細が正しい場合、雇用の確認は 1 ~ 2 時間で完了します。

## <a name="how-long-does-the-business-verification-process-usually-take"></a>"ビジネス検証" プロセスは、通常どのくらいの時間で実行されますか。

必要なすべてのドキュメントが送信された場合、ビジネスの検証は完了するまでに 1 ~ 2 営業日かかります。

## <a name="if-i-reach-out-to-the-support-team-will-my-ticket-be-expedited"></a>サポートチームに連絡した場合、チケットは優先されますか。

サポートチケットは、週の時間内に解決されます。 サポートチケットの発生時に提供されたメールに送信される更新プログラムを探してください。

## <a name="my-issue-is-not-listed-here--are-there-other-pages-i-can-reference-for-partner-center-issues"></a>この一覧には表示されません。  パートナーセンターの問題を参照できる他のページはありますか?

詳細については、「 [市販のマーケットプレース](/azure/marketplace/) 」のドキュメントを参照してください。

## <a name="ive-created-a-support-ticket-it-has-been-7-business-days-and-i-havent-received-an-update-where-can-i-get-additional-help"></a>サポートチケットを作成したが、それは7営業日でしたが、更新を受信していません。 追加のヘルプはどこで入手できますか?

次の情報を使用して、にメールを送信してください **<teamsubm@microsoft.com>** 。

1. **件名行** *<App_Name>のパートナーセンターのアカウントの問題* (アプリの名前を指定してください)。
1. **メール本文:**
    * サポートチケット番号:
    * 販売者 ID:
    * 問題のスクリーンショット (可能な場合):

>
> [!div class="nextstepaction"]
> [Microsoft Teams のアプリの検証ポリシーの詳細情報](/legal/marketplace/certification-policies)
