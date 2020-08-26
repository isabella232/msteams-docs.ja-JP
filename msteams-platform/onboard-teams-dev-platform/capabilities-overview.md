---
title: Teams アプリの機能について
author: heath-hamilton
description: ''
ms.topic: overview
ms.author: heath-hamilton
ms.openlocfilehash: 580d75b648bde1caf0e85647c89635804c91fb70
ms.sourcegitcommit: 9fbc701a9a039ecdc360aefbe86df52b9c3593f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/12/2020
ms.locfileid: "46652092"
---
# <a name="understanding-teams-app-capabilities"></a>Teams アプリの機能について

*機能* は、Microsoft Teams プラットフォームでアプリを作成するための拡張点です。

Teams クライアントを拡張する方法は複数ありますが、すべてのアプリが一意であるため、一部のアプリは、1つの機能 (webhook など) しか持たないため、ユーザーのオプションを提供する方法がいくつかあります。 たとえば、アプリではデータを中央の場所 (タブ) に表示し、会話インターフェイス (bot) を通じて同じ情報を表示することができます。

Teams アプリには、次のコア機能のいずれかまたはすべてを含めることができます。

* [タブ](../tabs/what-are-tabs.md)
* [メッセージング拡張機能](../messaging-extensions/what-are-messaging-extensions.md)
* [Webhook とコネクタ](../webhooks-and-connectors/what-are-webhooks-and-connectors.md)
* [ボット](../bots/what-are-bots.md)

アプリでは、 [Microsoft GRAPH REST API](../graph-api/rsc/resource-specific-consent.md)などの高度な機能を利用することもできます。

アプリに必要な機能を提供する機能については、次の図を参照してください。

![Teams アプリの機能についてのマインドマップ](doc-links/images/capabilities-overview.png)

## <a name="doing-whats-best-for-your-users"></a>ユーザーにとって最適な方法

Teams アプリの開発について理解すると、その微妙な違いについて理解し始められます。 特定の機能 (ユーザー入力の収集など) を構築する方法は複数あります。 たとえば、を使用して、web ベースのフォームをタブに埋め込むことができ `<iframe>` ます。 また、タスクモジュールである Teams の UI 規則を使用して、ユーザーが望むようによりネイティブな操作を行うことができるタブで、これを行うこともできます。

機能と UI の規則、コントロール、および要素の適切な組み合わせを選択することによって、最初に [対象ユーザーのユースケースを理解](../concepts/design/understand-use-cases.md)することができます。

## <a name="learn-more"></a>詳細情報

* [アプリの計画を開始する](../concepts/extensibility-points.md)