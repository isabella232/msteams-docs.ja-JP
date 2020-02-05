---
title: 設計ガイドラインのリファレンス
description: 個人用アプリの設計ガイドラインについて説明します。
keywords: teams の設計ガイドラインリファレンスフレームワーク個人用アプリ
ms.openlocfilehash: 6a07b618d78a3ad79850713052c88ef178c1ecc1
ms.sourcegitcommit: 4329a94918263c85d6c65ff401f571556b80307b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "41674697"
---
# <a name="personal-apps"></a>個人用アプリ

> [!Important]
> モバイルクライアントでのタブの完全なサポートは、近日中に予定されています。 この変更を準備するには、タブを作成するときに[[モバイル] のタブのガイダンス](~/tabs/design/tabs-mobile.md)に従ってください。 個人用アプリ (静的タブ) は現在、[開発者向けプレビュー](~/resources/dev-preview/developer-preview-intro.md)で利用できます。
>
> タブの完全なサポートがリリースされると、次のようになります。
>
> * すべてのタブがモバイルで常に使用できるようになります。
> * `contentUrl` **は、モバイル Teams クライアントにロードされ**ます。
> * [チャネル/グループ] タブでは、ユーザーは自分`websiteUrl`のを経由して別のブラウザーで`contentUrl`タブを開くことができます。ただし、は最初に読み込まれます。

個人用アプリは、個人のスコープを持つアプリです。 アプリ開発者は、個々のユーザー向けにビルドされたバージョンのアプリを提供するオプションを使用できます。 このバージョンでは、タブのコレクション (および bot が含まれている場合は bot) は、その人物向けに設計されています。 これにより、ユーザーとの1対1の操作を作成することができます。

個人用アプリは、誰かが表示されているすべてのアイテム、およびアプリで最近表示したすべてのアイテムを表示できます。 すべてが1つの場所に配置されます。 次のスクリーンショットでは、Contoso は個人用アプリのポップアップにある個人用アプリです。

![アプリのオーバーフローメニューのイメージ](~/assets/images/Personal-apps-App-flyout.png)

---

## <a name="guidelines"></a>ガイドライン

個人用アプリには通常、次のタブが含まれています。

### <a name="your-tab"></a>タブ

これで、ユーザーにすべてのアイテムが表示されます。 個人のスペースです。 このタブは、リスト、グリッド、列、または1つのキャンバスとして配置できます。アプリケーションにとって最適なものは何か。 有効なタブの設計の詳細については、次を参照してください。 [タブデザイン (~/tabs/design/tabs.md)] を参照してください。

このタブには複数のチャネルのアイテムが表示されることがあるので、各アイテムには自分のチーム、チャネル、およびタブが表示され、ユーザーは自分の元の場所を簡単に確認できます。

![[個人用タスク] タブ](~/assets/images/Personal-apps-MY-tab.png)

### <a name="recent"></a>最新

[**最近使っ**た項目] タブを使用すると、アプリで最近表示したすべての情報を他のユーザーが参照できます。 これは、時系列 (最も新しいものから最も古いものまで) に一覧表示されます。 このリスト内のアイテムをクリックすると、そのアイテムのチャネルとタブに移動します。

![[最近使用したタブ]](~/assets/images/Personal-apps-Recent-tab.png)

### <a name="all"></a>すべて

これは、ユーザーの組織内のすべてのタブ (アクセスできるもの) の一覧です。 つまり、アプリが使用されているすべての場所を表示します。 [**最近使っ**た項目] タブと同様に、リスト内で何かを選択すると、ユーザーは関連するチャネルとタブに直接移動します。

### <a name="bot"></a>Bot

Bot は必須ではありませんが、ユーザーと直接やり取りするための最適な方法です。 通知は個人アプリの最も重要な機能の1つで、直接的なコミュニケーションと比べてより良い通知方法ですか。

Bot は、メッセージをカード形式で配信します。これにより、特定の情報 (新しいコンテンツが利用可能であることを示す通知など) や広範な更新 (毎日のタスクリストなど) を提供できます。 効果的なボットの設計に関する追加情報については、以下を参照してください。 [Bot 設計] (~/bots/design/bots.md)。

![Bot 案内応答](~/assets/images/Personal-apps-Bot.png)

### <a name="help-and-settings"></a>ヘルプと設定

ヘルプコンテンツは、ユーザーがアプリの微妙な違いを見つけられるようにします。 [**設定**] タブを追加して、さらにカスタマイズできるようにします。

### <a name="about"></a>概要

バージョン番号、機能、プライバシー、アクセス許可のリンクなどの情報を提供するための [**概要**] タブを含めます。

## <a name="best-practices"></a>ベスト プラクティス

### <a name="communicate-directly-with-your-users"></a>ユーザーと直接通信する

Bot を使用して、ユーザーに変更と新機能があることを通知します。

### <a name="customize-your-tabs"></a>タブをカスタマイズする...

ユーザーが特定のタスクを実行するのに役立つその他のタブを自由に追加できます。

### <a name="and-make-them-relevant-to-every-user"></a>...すべてのユーザーに関連するようにします。

アプリマニフェストで宣言したすべてのタブは、すべてのユーザーに表示されます。 たとえば、個人アプリが、マネージャーと従業員の両方で使用される経費報告ツールである場合、[**承認**] タブで、両方の役割にとって意味のあるコンテンツを提供する必要があります。