---
title: カスタムタブを使用して Teams アプリを拡張する
author: laujan
description: タブを作成するためのガイド
keywords: 構成可能な teams タブグループチャネル
ms.topic: conceptual
ms.author: ''
ms.openlocfilehash: 3f3b0ac8bc141672f25d9db2470cb71a856e0ed8
ms.sourcegitcommit: 4329a94918263c85d6c65ff401f571556b80307b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "41674884"
---
# <a name="extend-your-teams-app-with-a-custom-tab"></a>カスタムタブを使用して Teams アプリを拡張する

カスタムタブを使用すると、チャネル、グループチャット、および個人ユーザーに対してホストする web コンテンツを提供できます。 高レベルでは、次の手順を実行してタブを作成する必要があります。

1. 開発環境を準備します。
1. ページを作成します。
1. アプリサービスをホストします。
1. アプリパッケージを作成し、Microsoft Teams にアップロードします。

[!include[prepare environment](~/includes/prepare-environment.md)]

## <a name="create-your-pages"></a>ページを作成する

個人またはチャネル/グループのスコープ内にタブを表示するかどうかにかかわらず、ホストする1つ以上の HTML ページから構成されます。 個人のスコープを持つタブは単一のコンテンツページで構成され、チャネルまたはグループのスコープを持つタブでは、インストール時のユーザー入力に基づいてコンテンツページの URL を設定する構成ページが必要になります。

タブページには3つの種類があります。 作成の詳細については、対応するドキュメントページを参照してください。

1. [コンテンツページ](~/tabs/how-to/create-tab-pages/content-page.md)(タブに表示されるページ)。
1. [[構成] ページ](~/tabs/how-to/create-tab-pages/configuration-page.md)。コンテンツページの URL を設定または更新し、[チャネル/グループ] タブに追加するために使用されます。
1. [削除ページ](~/tabs/how-to/create-tab-pages/removal-page.md)。 [チャネル/グループ] タブが削除されたときに表示されるオプションのページです。

### <a name="tab-requirements"></a>タブの要件

ページの種類に関係なく、タブは次の要件を満たす必要があります。

* X フレームオプションまたはコンテンツセキュリティポリシーの HTTP 応答ヘッダーを使用して、IFrame でページを処理できるようにする必要があります。

* 通常、クリック-jacking に対する保護手段として、ログインページは Iframe では表示されません。 そのため、認証ロジックでは、リダイレクト以外のメソッドを使用する必要があります (たとえば、トークンベースまたは cookie ベースの認証を使用します)。

> [!NOTE]
> Chrome 80 (初期2020でリリースが予定されています)。新しい cookie 値を紹介し、既定で cookie ポリシーを設定します。 既定のブラウザーの動作に依存するのではなく、cookie に対して使用する目的を設定することをお勧めします。 [SameSite cookie 属性 (2020 update)](../../resources/samesite-cookie-update.md)を*参照してください*。

* ブラウザーは、web ページを提供するドメインとは別のドメインに対する要求を web ページから実行できないようにする、同一生成元ポリシー制限に従います。 ただし、別のドメインまたはサブドメインに、構成ページまたはコンテンツページをリダイレクトする必要がある場合があります。 クロスドメインナビゲーションロジックにより、チームクライアントは、タブを読み込んだり通信したりするときに、アプリマニフェストの静的な validDomains リストに対して生成元を検証できるようにする必要があります。

* シームレスな環境を作成するには、Teams クライアントのテーマ、設計、および目的に基づいて、タブのスタイルを指定する必要があります。 通常、タブは、特定のニーズに対応するように構築され、タブのチャネルの場所に関連する一連の小さなタスクまたはデータのサブセットに焦点を当てるように構築されている場合に最適に機能します。

* コンテンツページ内で、script タグを使用して[Microsoft Teams JavaScript クライアント SDK](/javascript/api/overview/msteams-client)への参照を追加します。 ページの読み込みの後、に`microsoftTeams.initialize()`電話をかけます。 使用しない場合、ページは表示されません。

## <a name="host-your-app-service"></a>アプリサービスをホストする

コンテンツは HTTPS を使用して利用可能な公開 URL でホストされている必要があります。 テストのために、 [ngrok](https://ngrok.com/)などのリバースプロキシを使用して、ローカルポートをインターネットに接続する URL に公開することができます。

## <a name="create-your-app-package-with-app-studio"></a>アプリ Studio を使用してアプリパッケージを作成する

アプリのマニフェストを作成するために、Microsoft Teams クライアント内からアプリ Studio アプリを使用することができます。 Teams にアプリ studio がインストールされていない場合は、teams](/microsoftteams/platform/assets/images/tab-images/storeApp.png)アプリの左下隅にある [**アプリ** ![ストアアプリ] を選択して、アプリ studio を検索します。 タイルを見つけたら、それを選択して、[ポップアップウィンドウ] ダイアログボックスの [インストール] を選択します。

1. Microsoft Teams クライアントを開く- [web ベースのバージョン](https://teams.microsoft.com)を使用すると、ブラウザーの[開発者ツール](~/tabs/how-to/developer-tools.md)を使用してフロントエンドコードを検査できます。
1. App Studio を開き、[**マニフェストエディター** ] タブを選択します。
1. [**新しいアプリタイルを作成する**] を選択します。
1. アプリの詳細を追加します (各フィールドの詳細については、「[マニフェストスキーマ定義](~/resources/schema/manifest-schema.md)」を参照してください)。
1. [機能] セクションで、[**タブ**] を選択します。
    * [個人] タブで、[*個人用タブの追加]* を選択し、[**追加**] を選択します。 ポップアップダイアログウィンドウが表示され、タブの詳細を追加できます。
    * [チャネル/グループ] タブの [*チーム*] タブで、[**追加**] を選択し、[チーム] タブのポップアップウィンドウにあるタブの詳細フィールドに入力します。 が*構成を更新できることを確認するチーム*および*グループのチャット*ボックスがチェックされ、[**保存**] を選択します。
1. [*ドメインと権限*] セクションで、[*タブのドメイン*] フィールドに、HTTPS プレフィックスのないホストまたはリバースプロキシの URL が含まれている必要があります。
1. **[** => **テストと配布**の終了] タブでは、アプリパッケージを**ダウンロード**するか、パッケージをチームに**インストール**するか、Teams アプリストアに**提出**して承認を受けることができます。 *リバースプロキシを使用している場合は、右側の [**説明**] フィールドに警告が表示されます。タブのテスト中は、この警告を無視でき*ます。

## <a name="create-your-app-package-manually"></a>アプリパッケージを手動で作成する

ボットおよびメッセージング拡張機能と同様に、タブのプロパティを含むようにアプリの[アプリマニフェスト](~/resources/schema/manifest-schema.md)を更新します。 これらのプロパティは、タブが使用可能な範囲、使用する Url、およびその他のさまざまなプロパティを制御します。

### <a name="personal-tabs"></a>個人用タブ

個人用タブに表示されるコンテンツは、すべてのユーザーに対して同じで`staticTabs`あり、配列に構成されています。 アプリで最大16個の個人用タブを宣言することができます。

|名前| 型| 最大サイズ | 必須 | 説明|
|---|---|---|---|---|
|`entityId`|String|64文字|✔|タブに表示されるエンティティの一意識別子。|
|`name`|String|128文字|✔|チャネルインターフェイスのタブの表示名。|
|`contentUrl`|String|2048 文字|✔|Teams キャンバスに表示されるエンティティ UI をポイントする https://URL。|
|`websiteUrl`|String|2048 文字||ユーザーがブラウザーで表示をポイントしたかどうかを示す https://URL。|
|`scopes`|列挙型の配列|1 |✔|静的タブでは、 `personal`スコープのみがサポートされます。つまり、個人用アプリの一部としてのみプロビジョニングできます。|

#### <a name="simple-personal-tab-manifest-example"></a>シンプルな個人用タブマニフェストの例

次の例は、アプリ`staticTabs`マニフェストからの配列のみを示しています。

```json
...
"staticTabs": [
    {
      "entityId": "idForPage",
      "name": "Display name for tab",
      "contentUrl": "https:// yourURL.com/content ",
      "websiteUrl": "https://yourURL.com/website",
      "scopes": [ "personal" ]
    }
...
```

### <a name="channelgroup-tabs"></a>チャネル/グループタブ

チャネルまたはグループのタブが`configurableTabs`配列に追加されます。 配列には、 `configurableTabs` [チャネル/グループ] タブを1つだけ宣言できます。

|名前| 型| 最大サイズ | 必須 | 説明|
|---|---|---|---|---|
|`configurationUrl`|String|2048 文字|✔|Https://URL を構成するページ。|
|`canUpdateConfiguration`|Boolean|||作成後にタブの構成のインスタンスをユーザーが更新できるかどうかを示す値。 限り`true`|
|`scopes`|列挙型の配列|1 |✔|構成可能なタブで`team`は`groupchat` 、および範囲のみがサポートされています。 |

#### <a name="simple-channelgroup-tab-manifest-example"></a>シンプルなチャネル/グループタブマニフェストの例

次の例は、アプリ`configurableTabs`マニフェストからの配列のみを示しています。

```json
...
"configurableTabs": [
    {
      "configurationUrl": "https://yourURL.com/configure",
      "canUpdateConfiguration": true,
      "scopes": [ "team", "groupchat" ]
    }
...
```

2つの必須アイコン`manifest.json`と共に、zip フォルダーでバンドルを完了した後。

### <a name="upload-app-package-directly-to-a-team"></a>アプリパッケージを直接チームにアップロードする

1. Microsoft Teams クライアントを開きます。 [Web ベースのバージョン](https://teams.microsoft.com)を使用する場合は、ブラウザーの[開発者ツール](~/tabs/how-to/developer-tools.md)を使用してフロントエンドコードを調べることができます。
1. 左側の [*チーム*] パネルで、使用して`...`いるチームの横のメニューを選択して、タブのテストを行い、[**チームの管理**] を選択します。
1. メインパネルで、タブバーから [**アプリ**] を選択し、ページの右下隅にある [**カスタムアプリのアップロード**] を選択します。
1. プロジェクトディレクトリを開き、 **./パッケージ**フォルダーを参照して、アプリパッケージの zip フォルダーを選択し、[**開く**] を選択します。 タブが Teams にアップロードされます。

### <a name="view-your-tab-in-teams"></a>Teams でタブを表示する

1. [個人用] タブを表示します。
    * Teams クライアントの左端にあるナビゲーションバーで、 `...`メニューを選択し、リストからアプリを選択します。

1. [チャネル/グループ] タブを表示します。
    * チームに戻り、タブを表示するチャネルを選択し、タブバーから [➕] を選択して、ギャラリーからタブを選択します。
    * タブを追加する手順に従います。 [チャネル/グループ] タブには、カスタム構成ダイアログがあることに注意してください。 [**保存**] を選択すると、タブがチャネルのタブバーに追加されます。

## <a name="learn-more"></a>詳細情報

* [タブのコンテンツページを作成する](~/tabs/how-to/create-tab-pages/content-page.md)
* [タブの構成ページを作成する](~/tabs/how-to/create-tab-pages/configuration-page.md)
* [タブを更新または削除する](~/tabs/how-to/create-tab-pages/removal-page.md)