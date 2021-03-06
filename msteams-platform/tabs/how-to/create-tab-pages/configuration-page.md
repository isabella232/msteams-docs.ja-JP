---
title: 構成ページを作成する
author: laujan
description: 構成ページを作成する方法
keywords: 構成可能な teams タブグループチャネル
ms.topic: conceptual
ms.author: lajanuar
ms.openlocfilehash: 591e1aa91bd33d1a61e9d70b35fd1561368fcda4
ms.sourcegitcommit: d3bb4bbcdff9545c9869647dcdbe563a2db868be
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2020
ms.locfileid: "47964607"
---
# <a name="create-a-configuration-page"></a>構成ページを作成する

構成ページは、ユーザーが Teams アプリのいくつかの側面を構成できる特別な種類の [コンテンツページ](content-page.md) です。 通常は、次の一部として使用されます。

* [チャネルまたはグループチャット] タブ-[構成] ページでは、ユーザーから情報を収集し、 `contentUrl` 表示するコンテンツページのを設定できます。
* [メッセージング拡張機能](~/messaging-extensions/what-are-messaging-extensions.md)
* [Office 365 コネクタ](~/webhooks-and-connectors/what-are-webhooks-and-connectors.md)

## <a name="configuring-a-channel-or-group-chat-tab"></a>チャネルまたはグループの [チャット] タブの構成

構成ページがコンテンツページに表示される方法を通知します。 アプリケーションは、 [Microsoft Teams の JavaScript クライアント SDK](/javascript/api/overview/msteams-client?view=msteams-client-js-latest&preserve-view=true) および呼び出しを参照する必要があり `microsoft.initialize()` ます。 さらに、Url は、セキュリティで保護された HTTPS エンドポイントであり、クラウドから入手可能である必要があります。 構成ページの例を次に示します。

```html
<head>
<script src='https://statics.teams.cdn.office.net/sdk/v1.6.0/js/MicrosoftTeams.min.js'></script>
</head>
    <body>
        <button onclick="(document.getElementById('icon').src = '/images/iconGray.png'); colorClickGray()">Select Gray</button>
        <img id="icon" src="/images/teamsIcon.png" alt="icon" style="width:100px" />
        <button onclick="(document.getElementById('icon').src = '/images/iconRed.png'); colorClickRed()">Select Red</button>

        <script>
            microsoftTeams.initialize();
            let saveGray = () => {
                microsoftTeams.settings.registerOnSaveHandler((saveEvent) => {
                    microsoftTeams.settings.setSettings({
                        websiteUrl: "https://yourWebsite.com",
                        contentUrl: "https://yourWebsite.com/gray",
                        entityId: "grayIconTab",
                        suggestedDisplayName: "MyNewTab"
                    });
                    saveEvent.notifySuccess();
                });
            }
            let saveRed = () => {
                microsoftTeams.settings.registerOnSaveHandler((saveEvent) => {
                    microsoftTeams.settings.setSettings({
                        websiteUrl: "https://yourWebsite.com",
                        contentUrl: "https://yourWebsite.com/red",
                        entityId: "redIconTab",
                        suggestedDisplayName: "MyNewTab"
                    });
                    saveEvent.notifySuccess();
                });
            }

            let gr = document.getElementById("gray").style;
            let rd = document.getElementById("red").style;

            const colorClickGray = () => {
                gr.display = "block";
                rd.display = "none";
                microsoftTeams.settings.setValidityState(true);
                saveGray()
            }

            const colorClickRed = () => {
                rd.display = "block";
                gr.display = "none";
                microsoftTeams.settings.setValidityState(true);
                saveRed();
            }
        </script>
    </body>
...
```

ここでは、ユーザーに2つのオプションボタンを表示し、[ **灰色** ] または **[赤** ] を選択してタブのコンテンツを赤または灰色のアイコンで表示します。 [相対] ボタンを選択する `saveGray()` `saveRed()` と、次のように起動されます。

1. `settings.setValidityState(true)`は true に設定されています。
1. `microsoftTeams.settings.registerOnSaveHandler()`イベントハンドラーがトリガーされます。
1. アプリの [構成] ページの [ **保存** ] ボタン (Teams にアップロードされたもの) は有効になっています。

このコードを使用すると、構成の要件が満たされており、インストールを続行できることがわかります。 **Save**では、のパラメーターは、 `settings.setSettings()` インターフェイスの定義に従って、現在のインスタンスに対して設定され `Settings` ます (「 [Settings interface](/javascript/api/@microsoft/teams-js/microsoftteams.settings.settings?view=msteams-client-js-latest&preserve-view=true) 」を参照してください)。 最後に、 `saveEvent.notifySuccess()` は、コンテンツ URL が正常に解決されたことを示すためにを呼び出します。

>[!NOTE]
>
>* を使用して保存ハンドラーを登録した場合 `microsoftTeams.settings.registerOnSaveHandler()` は、コールバックが起動する `saveEvent.notifySuccess()` か、 `saveEvent.notifyFailure()` 構成の結果を示す必要があります。
>* 保存ハンドラーが登録されていない場合は、 `saveEvent.notifySuccess()` ユーザーが [ **保存** ] ボタンを選択すると、その時点で通話が自動的に実行されます。

### <a name="get-context-data-for-your-tab-settings"></a>タブ設定のコンテキストデータを取得する

タブには、関連するコンテンツを表示するためにコンテキスト情報が必要な場合があります。 コンテキスト情報を使用すると、よりカスタマイズされたユーザー環境を提供することにより、タブの外観をさらに向上させることができます。

Teams [コンテキストインターフェイス](/javascript/api/@microsoft/teams-js/microsoftteams.context?view=msteams-client-js-latest&preserve-view=true) は、タブの構成に使用できるプロパティを定義します。 コンテキストデータ変数の値は、次の2つの方法で収集できます。

1. マニフェスト内に URL クエリ文字列プレースホルダーを挿入 `configurationURL` します。

1. [TEAMS SDK](/javascript/api/overview/msteams-client?view=msteams-client-js-latest&preserve-view=true)メソッドを使用し `microsoftTeams.getContext((context) =>{}` ます。

#### <a name="insert-placeholders-in-the-configurationurl"></a>にプレースホルダーを挿入します。 `configurationURL`

コンテキストインターフェイスプレースホルダーは、ベースに追加でき `configurationUrl` ます。 次に例を示します。

##### <a name="base-url"></a>ベース Url

```json
...
"configurationUrl": "https://yourWebsite/config",
...
```

#### <a name="base-url-with-query-strings"></a>クエリ文字列を含むベース URL

```json
...
"configurationUrl": "https://yourWebsite/config?team={teamId}&channel={channelId}&{locale}"
...
```

ページがアップロードされると、クエリ文字列のプレースホルダーが関連する値で Teams によって更新されます。 構成ページにロジックを含めて、それらの値を取得して使用することができます。 URL クエリ文字列の処理の詳細については、「MDN web ドキュメント」の「 [Urlsearchparams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) 」を参照してください。上記のプロパティから値を抽出する方法の例を次に示し `configurationURL` ます。

```html
<script>
   microsoftTeams.initialize();
   const getId = () => {
        let urlParams = new URLSearchParams(document.location.search.substring(1));
        let blueTeamId = urlParams.get('team');
        return blueTeamId
    }
//For testing, you can invoke the following to view the pertinent value:
document.write(getId());
</script>
```

### <a name="use-the-getcontext-function-to-retrieve-context"></a>関数を使用して `getContext()` コンテキストを取得する

呼び出されると、 `microsoftTeams.getContext((context) => {})` 関数は [コンテキストインターフェイス](/javascript/api/@microsoft/teams-js//microsoftteams.context?view=msteams-client-js-latest&preserve-view=true)を取得します。 この関数を構成ページに追加して、コンテキストの値を取得できます。

```html
<!-- `userPrincipalName` will render in the span with the id "user". -->

<span id="user"></span>
...
<script>
    microsoftTeams.getContext((context) =>{
        let userId = document.getElementById('user');
        userId.innerHTML = context.userPrincipalName;
    });
</script>
...
```

## <a name="context-and-authentication"></a>コンテキストと認証

ユーザーにアプリの構成を許可する前に認証が必要な場合や、独自の認証プロトコルを使用するソースがコンテンツに含まれている場合があります。 「 [Microsoft Teams でユーザーを認証する」タブ](~/tabs/how-to/authentication/auth-flow-tab.md) コンテキスト情報は、認証要求および承認ページの url を構築するのに役立ちます。
タブページで使用されているすべてのドメインが配列に含まれていることを確認してください `manifest.json` `validDomains` 。

## <a name="modify-or-remove-a-tab"></a>タブを変更または削除する

サポートされている削除オプションでは、ユーザーの作業をさらに絞り込むことができます。 ユーザーがマニフェストのプロパティをに設定して、グループ/チャネルタブの変更、再構成、または名前の変更を行うことができるようになり `canUpdateConfiguration` `true` ます。  また、アプリに [削除オプション] ページを含め、構成内のプロパティの値を設定することによって、タブが削除されたときにコンテンツに何が起こるかを指定でき `removeUrl`  `setSettings()` ます (以下を参照)。 個人用タブは変更できませんが、ユーザーがアンインストールすることはできます。 詳細については、「 [タブの削除ページを作成する](~/tabs/how-to/create-tab-pages/removal-page.md)」を参照してください。

## <a name="mobile-clients"></a>モバイル クライアント

Teams モバイル クライアントに [チャネル/グループ] タブを表示するように選択した場合は、`setSettings()` 構成には `websiteUrl` プロパティの値を設定する必要があります (下記参照)。 モバイル クライアントでのタブの完全なサポートは、間もなくリリースされる予定です。 更新プログラムの準備を行うには、タブの作成時に、[モバイルでのタブのガイダンス](~/tabs/design/tabs-mobile.md)に従ってください。

削除ページまたはモバイルクライアントの Microsoft Teams setSettings () 構成:

```javascript
microsoftTeams.settings.setSettings({
    contentUrl: "add content page URL here",
    entityId: "add unique name here",
    suggestedDisplayName: "add name to display on tab here",
    websiteUrl: "URL REQUIRED FOR MOBILE CLIENTS",
    removeUrl: "ADD REMOVAL PAGE URL HERE"
});
```
