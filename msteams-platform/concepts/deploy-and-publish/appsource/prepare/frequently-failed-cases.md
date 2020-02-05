---
title: ヒントとよく発生するケース
description: 送信に関するヒントと最も失敗したポリシーについて説明します。
keywords: teams の発行に関する faq が失敗することがよくあるケースヒントヒント
ms.openlocfilehash: ffb472c918a205bdcf921b967269a80fcaa93338
ms.sourcegitcommit: 4329a94918263c85d6c65ff401f571556b80307b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "41674962"
---
# <a name="tips-and-frequently-failed-cases"></a>ヒントとよく発生するケース 

この記事では、アプリが検証に失敗する最も一般的な理由のいくつかについて説明します。 アプリに関するすべての潜在的な問題を網羅したリストではありません。 ただし、このガイダンスに従うと、初回時の成功の可能性が大幅に向上します。 ポリシーの詳細な一覧については、「 [Appsource validation policies](https://dev.office.com/officestore/docs/validation-policies)」を参照してください。 AppSource の全体的な検証ポリシーのセクション14に記載されているポリシーは、Microsoft Teams アプリに固有のものです。

## <a name="tips-for-successful-app-submission"></a>アプリの送信を成功させるためのヒント

* Teams JavaScript SDK のバージョン1.4.1 以降を使用していることを確認してください。
* 検証の進行中にアプリを変更しないようにします。 これにより、アプリの完全な再検証が必要になります。
* アプリが応答を停止したり、予期せず終了したり、プログラミングエラーが発生したりしてはなりません。 問題がある場合は、メッセージをユーザーに転送するための有効な方法でエラーが発生する可能性があります。
* アプリは、ユーザーの環境に実行可能コードを自動的にダウンロード、インストール、または起動してはなりません。 ダウンロードでは、ユーザーから明示的なアクセス許可を検索する必要があります。
* 説明やサポートドキュメントなど、ユーザーの環境に関連付けられているマテリアルが正確である必要があります。 説明と資料では、正しいスペル、大文字と小文字の区別、句読点、および文法を使用してください。
* ヘルプとサポート: Teams アプリのヘルプ/FAQ リンクを用意し、このリンクを初回実行時のユーザー環境で提供することを強くお勧めします。 すべての個人用アプリについて、ユーザーの利便性を向上させるために、ヘルプページを個人タブとして提供することをお勧めします。

## <a name="policy-112-sign-up-sign-in-and-sign-out"></a>ポリシー 11.2: サインアップ、サインイン、サインアウト

説明: アプリでは、明確で簡単なサインイン/アウトと、(適切な場合) サインアップの機能を提供する必要があります。 アプリ内のすべての機能において、環境に到達可能である必要があります。

* ユーザーに対して明示的なサインインオプションが提供されている場合は、(アプリが SSO/サイレント認証を使用している場合でも) サインアウトオプションが存在する必要があります。
* サインアウトオプションは、Teams クライアントではなく、アプリの機能だけを使用してユーザーに署名する必要があります。
* サインインしているすべてのスコープにも、サインアウトが必要です。 サインアウトオプションでは、少なくとも、サインインオプションで署名されたのと同じ機能からユーザーをサインアウトする必要があります。 たとえば、サインインオプションでユーザーをメッセージング拡張機能とタブの両方に署名する場合、[サインアウト] オプションでは、メッセージの内線番号とタブからユーザーをサインアウトする必要があります。

* 次の動作 (または類似) を必ずしも逆転する方法があることを確認してください。
  * サインイン => サインアウト
  * アカウント/サービスのリンク => アカウント/サービスのリンク解除
  * アカウント/サービスを接続する => アカウント/サービスを切断する
  * アカウント/サービスを承認する => アカウント/サービスの承認を解除/承認解除する
  * アカウント/サービスを登録する => アカウント/サービスの登録を解除する
* アプリがアカウントまたはサービスを必要とする場合は、ユーザーがサインアップまたはサインアップを要求する方法を提供する必要があります。 アプリが "エンタープライズ" アプリカテゴリに収まる場合は、サインアッププロセスに対して例外を検索できます。
* サインイン/サインアウト機能は、モバイルクライアントで機能する必要があります。 Teams JavaScript SDK をバージョン1.4.1 以降にアップグレードしていることを確認してください。

認証の詳細については、次を参照してください。

* [認証のドキュメント](~/concepts/authentication/authentication.md)
* [ノード内の Bot 認証のサンプル](https://github.com/OfficeDev/microsoft-teams-sample-auth-node)
* [ノード内のタブ認証のサンプル](https://github.com/OfficeDev/microsoft-teams-sample-complete-node)
* [C#/.NET の Tab/bot 認証](https://github.com/OfficeDev/microsoft-teams-sample-complete-csharp)

## <a name="policy-145-microsoft-teams-apps-must-respond-in-a-reasonable-timeframe"></a>ポリシー 14.5: Microsoft Teams アプリは妥当な期間内に応答する必要があります。

* 14.5.1: タブの場合、アクションへの応答に3秒以上かかる場合は、読み込みメッセージまたは警告を指定する必要があります。
* 14.5.2: bot の場合、ユーザーコマンドへの応答は2秒以内に実行される必要があります。 より長い処理が必要な場合は、入力インジケーターを使用する必要があります。
* 14.5.3: 作成拡張機能の場合、ユーザーコマンドへの応答は5秒以内に実行される必要があります。

> [!TIP]
> アプリで時間がかかりすぎている場合は、必ず読み込みインジケーターを含めてください。

## <a name="policy-14153-content-in-a-tab-should-not-have-superfluousunnecessary-ui-aka-ui-chrome-or-layered-navigation"></a>ポリシー 14.15.3: タブ内のコンテンツは、不必要/不要な UI (別名: UI Chrome) またはレイヤードナビゲーションを持たないようにする必要があります。

タブは、フォーカスがあるコンテンツを提供し、このコンテンツに関連しない UI 要素を回避する必要があります。 一般的に、これは通常、不要なネスト化/レイヤードナビゲーション、コンテンツの横の関連性のない、または関連性のない UI、または、タブのコンテンツに関連しないコンテンツにユーザーが移動するリンクを指します。 たとえば、SharePoint はナビゲーションメニューを削除し、タブのメインコンテンツのみを紹介します。

![Sharepoint web ビュー](~/assets/images/faq/web-sp.png)
![sharepoint タブビュー](~/assets/images/faq/tab-sp.png)

複数のビューオプションがある場合は、ユーザーが選択できるタブ構成メニューがあることを検討してください。 たとえば、タブ内にメニューを埋め込む代わりに、幅の広いアイデアは構成ページにメニューを配置するので、実際のタブビューがクリーンでフォーカスされています。

![幅の広いアイデア構成ページ](~/assets/images/faq/wideidea.png)

## <a name="policy-14157-bots-must-respond-to-any-command-and-must-not-dead-end-the-user"></a>ポリシー 14.15.7: Bot は、すべてのコマンドに応答する必要がありますが、ユーザーの終了を停止してはなりません。

Bot は常に応答可能である必要があります。 お客様がユーザーにより高度に応答できるようにするためのヒントを以下に示します。

**コマンドリストを使用します。** 分析ユーザーの入力またはユーザーの意図を予測するのは困難です。 Bot が実行できることをユーザーに推測させるのではなく、bot が理解できるコマンドの一覧をユーザーに提供します。

![フローコマンドリスト](~/assets/images/faq/flow-bot.png)

**ヘルプコマンドを含めます。** ユーザーが失われたとき、または bot が期待した内容で応答しなかった場合には、"Help" と入力するのが最も一般的です。 すべての有効なコマンドと共に、価値の提案を提供するヘルプコマンドを含めます。

![フローヘルプコマンド](~/assets/images/faq/flow-help.png)

**Bot が失われたときに、ヘルプコンテンツまたはガイダンスを含めます。** ユーザーからの入力を理解できない場合は、代わりに実行できることをユーザーに伝えてください。 たとえば、「わかりません」というようになります。 詳細については、「help」と入力してください。 エラーメッセージに応答したり、単に "理解していない" というメッセージを表示したりしないでください。 この機会を使用して、ユーザーに教えることができます。

**両方のスコープで検討します。** 必要に応じて、ボットがチャネルで適切な応答を提供していること (@*botname*)、および個人の会話があるかどうかを確認します。 Bot が個人または teams のスコープ内で意味のあるコンテキストを提供していない場合は、マニフェストを使用してその範囲を無効にします。 (「 `bots` [Microsoft Teams のマニフェストスキーマリファレンス](~/resources/schema/manifest-schema.md#bots)」のブロックを参照してください)。

## <a name="policy-14159-bot-must-send-welcome-messages-on-the-first-launch"></a>ポリシー 14.15.9: Bot は最初の起動時にウェルカムメッセージを送信する必要があります。

[開始] メッセージは、トーンを設定するための最適な方法です。 これは、最初の相互作用ユーザーが bot との関係にあります。 良いウェルカムメッセージでは、ユーザーがアプリについての調査を続けることができますが、無効な場合には使用が混乱し、ユーザーがアプリの価値をすぐに見ることができない場合、ユーザーは関心を失う可能性があります。

### <a name="personal-scope"></a>パーソナルスコープ

Bot が初めて起動したときに、ユーザーはサインインする前にも、その bot からウェルカムメッセージを取得する必要があります。 ウェルカムメッセージを設計する際に考慮すべきいくつかのヒントを次に示します。

**メッセージを簡潔でわかりやすくします。** ユーザーがアプリに関して大きく異なるエクスペリエンスと知識を持っている場合があります。 他のプラットフォームでアプリを使用している場合や、アプリについて何も知らない場合があります。 すべての対象ユーザーに対してメッセージを調整したいとします。また、いくつかの文で、お客様の bot とその操作方法を説明します。 また、アプリの価値と、ユーザーがそれを使用する利点についても説明する必要があります。
![Cafe と Dinning bot](~/assets/images/faq/cafe-bot.png)

**メッセージをアクション可能にします。** アプリをインストールした後に、ユーザーが最初に行うことを検討してください。 試してみるべきすばらしいコマンドはありますか。 他に知っておくべき開始時の作業がありますか? サインインする必要があるかどうか アダプティブカードにアクションを追加したり、"お問い合わせください" などの特定の例を指定したりすることができます。

### <a name="team-scope"></a>チームスコープ

Bot が最初にチャネルに追加されたときには、多少異なる操作が行われます。 通常、チームの全員に1:1 メッセージを送信するのではなく、ボットがチャネルでウェルカムメッセージを送信することがあります。

## <a name="policy-141510-tab-configuration-ui-should-not-dead-end-the-experience-and-always-provide-a-way-for-a-user-to-continue"></a>ポリシー 14.15.10: タブ構成 UI は、環境を停止せず、常にユーザーに継続する方法を提供します。

ユーザーは、探しているコンテンツをすぐに見つけることができない場合でも、常に構成の作業を完了できる必要があります。 構成作業では、コンテンツを検索したり、URL の pin を指定したり、存在しない場合は新しいコンテンツを作成したりするためのオプションをユーザーに提供する必要があります。 ユーザーは、コンテンツを作成するための構成の作業を残しておく必要はありません。その後、チームに戻って固定する必要があります。

![OneNote では、メモが見つからない場合に OneNote リンクを貼り付けることができます。](~/assets/images/faq/tab-onenote-config.png)

![既存のプランがない場合、ユーザーはいつでもプランナーで新しいプランを作成できます。](~/assets/images/faq/tab-planner-config.png)

![SharePoint では、ユーザーが SharePoint リンクを直接貼り付けることもできます。](~/assets/images/faq/tab-sp-config.png)