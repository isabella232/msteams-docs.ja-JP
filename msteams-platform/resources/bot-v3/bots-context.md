---
title: Bot のコンテキストを取得する
description: Microsoft Teams でボットのコンテキストを取得する方法について説明します。
keywords: teams の bot コンテキスト
ms.date: 05/20/2019
ms.openlocfilehash: 2dea6fd51e7274fa899d9ae882441a21618d7e09
ms.sourcegitcommit: 4329a94918263c85d6c65ff401f571556b80307b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "41675110"
---
# <a name="get-context-for-your-microsoft-teams-bot"></a>Microsoft Teams の bot のコンテキストを取得する

[!include[v3-to-v4-SDK-pointer](~/includes/v3-to-v4-pointer-bots.md)]

Bot は、ユーザープロファイルなどのチームまたはチャットに関する追加のコンテキストにアクセスできます。 この情報は、ボットの機能を強化し、よりパーソナライズされた環境を提供するために使用できます。

> [!NOTE]
> これらの Microsoft&ndash;Teams 固有の bot api には、BOT ビルダー SDK の拡張機能を通じてアクセスすることをお勧めします。 C#/.NET については、「 [Microsoft の Bot](https://www.nuget.org/packages/Microsoft.Bot.Connector.Teams) NuGet パッケージをダウンロードしてください。 Node.js を開発する場合は、 [botbuilder](https://www.npmjs.com/package/botbuilder-teams)の npm パッケージをインストールできます。 両方の Sdk がターゲットボットビルダー v3。

## <a name="fetching-the-team-roster"></a>チーム名簿を取得する

Bot は、チームメンバーの一覧とその基本プロファイルを照会できます。これには、Teams のユーザー Id と、名前や objectId などの Azure Active Directory (Azure AD) の情報が含まれます。 この情報を使用して、ユーザー id を関連付けることができます。たとえば、Azure AD 資格情報を介してタブにログインしているユーザーが、チームのメンバーであるかどうかを確認します。

### <a name="rest-api-example"></a>REST API の例

エンドポイントとしての[`/conversations/{teamId}/members/`](/bot-framework/rest-api/bot-framework-rest-connector-api-reference#get-conversation-members) `serviceUrl`値を使用して、GET 要求を直接発行することができます。

は`teamId` 、次のシナリオで`channeldata` 、ボットが受け取るアクティビティペイロードのオブジェクトにあります。
* ユーザーがチームコンテキストの bot との間でメッセージを送信したり、対話したりする場合 ([メッセージの受信](~/resources/bot-v3/bot-conversations/bots-conversations.md#receiving-messages)を参照)
* 新しいユーザーまたは bot がチームに追加されたとき (「[チームに追加された bot またはユーザー」を](~/resources/bot-v3/bots-notifications.md#bot-or-user-added-to-a-team)参照)

> [!NOTE]
>* Api を呼び出すときにチーム id を必ず使用してください。
>* の`serviceUrl`値は安定していますが、変更することができます。 新しいメッセージが到着すると、ボットはに格納されて`serviceUrl`いる値を確認する必要があります。

```json
GET /v3/conversations/19:ja0cu120i1jod12j@skype.net/members

Response body
[{
    "id": "29:1GcS4EyB_oSI8A88XmWBN7NJFyMqe3QGnJdgLfFGkJnVelzRGos0bPbpsfJjcbAD22bmKc4GMbrY2g4JDrrA8vM06X1-cHHle4zOE6U4ttcc",
    "objectId": "9d3e08f9-a7ae-43aa-a4d3-de3f319a8a9c",
    "givenName": "Larry",
    "surname": "Brown",
    "email": "Larry.Brown@fabrikam.com",
    "userPrincipalName": "labrown@fabrikam.com"
}, {
    "id": "29:1bSnHZ7Js2STWrgk6ScEErLk1Lp2zQuD5H2qQ960rtvstKp8tKLl-3r8b6DoW0QxZimuTxk_kupZ1DBMpvIQQUAZL-PNj0EORDvRZXy8kvWk",
    "objectId": "76b0b09f-d410-48fd-993e-84da521a597b",
    "givenName": "John",
    "surname": "Patterson",
    "email": "johnp@fabrikam.com",
    "userPrincipalName": "johnp@fabrikam.com"
}, {
    "id": "29:1URzNQM1x1PNMr1D7L5_lFe6qF6gEfAbkdG8_BUxOW2mTKryQqEZtBTqDt10-MghkzjYDuUj4KG6nvg5lFAyjOLiGJ4jzhb99WrnI7XKriCs",
    "objectId": "6b7b3b2a-2c4b-4175-8582-41c9e685c1b5",
    "givenName": "Rick",
    "surname": "Stevens",
    "email": "Rick.Stevens@fabrikam.com",
    "userPrincipalName": "rstevens@fabrikam.com"
}]
```

### <a name="net-example"></a>.NET の例

を`GetConversationMembersAsync`使用`Team.Id`して、ユーザー id のリストを返します。

```csharp
// Fetch the members in the current conversation
var connector = new ConnectorClient(new Uri(context.Activity.ServiceUrl));
var teamId = context.Activity.GetChannelData<TeamsChannelData>().Team.Id;
var members = await connector.Conversations.GetConversationMembersAsync(teamId);

// Concatenate information about all members into a string
var sb = new StringBuilder();
foreach (var member in members.AsTeamsChannelAccounts())
{
    sb.AppendFormat(
        "GivenName = {0}, TeamsMemberId = {1}",
        member.Name, member.Id);

    sb.AppendLine();
}

// Post the member info back into the conversation
await context.PostAsync($"People in this conversation: {sb.ToString()}");
```

### <a name="nodejstypescript-example"></a>Node.js/TypeScript の例

次の例では、 [node.js の Bot ビルダー SDK の Microsoft Teams 拡張機能](https://www.npmjs.com/package/botbuilder-teams)を使用しています。

```typescript

[...]
import * as builder from "botbuilder";
[...]

var teamId = session.message.sourceEvent.team.id;
connector.fetchMembers(
  (<builder.IChatConnectorAddress>session.message.address).serviceUrl,
  teamId,
  (err, result) => {
    if (err) {
      session.endDialog('There is some error');
    }
    else {
      session.endDialog('%s', JSON.stringify(result));
    }
  }
);
```

## <a name="fetching-user-profile-or-roster-in-personal-or-group-chat"></a>個人またはグループチャットでのユーザープロファイルまたは名簿の取得

また、任意の個人チャットに対して同じ API 呼び出しを行うことにより、ユーザーが bot とチャットする際のプロファイル情報を取得することもできます。

API call および SDK のメソッドは、応答オブジェクトと同様に、チーム名簿を取得するのと同じです。 唯一の違いは、では`conversationId`なくを渡し`teamId`ます。

## <a name="fetching-the-list-of-channels-in-a-team"></a>チーム内のチャネルのリストの取得

Bot は、チーム内のチャネルの一覧を照会できます。

> [!NOTE]
>
>* 既定の一般的なチャネルの名前は、ローカリゼーション`null`を許可するためにとして返されます。
>* 一般チャネルのチャネル ID は、常にチーム ID と一致します。

### <a name="rest-api-example"></a>REST API の例

エンドポイントとしての`/teams/{teamId}/conversations/` `serviceUrl`値を使用して、GET 要求を直接発行することができます。

の唯一の`teamId`ソースは、チームコンテキストからのメッセージです。これは、ユーザーからのメッセージ、または、チームに追加されたときにボットが受信するメッセージのどちらかです (「[チームに追加されたボットまたはユーザー」を](~/resources/bot-v3/bots-notifications.md#team-member-or-bot-addition)参照)。

> [!NOTE]
> の`serviceUrl`値は安定していますが、変更することができます。 新しいメッセージが到着すると、ボットはに格納されて`serviceUrl`いる値を確認する必要があります。

```json
GET /v3/teams/19%3A033451497ea84fcc83d17ed7fb08a1b6%40thread.skype/conversations

Response body
{
    "conversations": [{
        "id": "19:033451497ea84fcc83d17ed7fb08a1b6@thread.skype",
        "name": null
    }, {
        "id": "19:cc25e4aae50746ecbb11473bba24c70a@thread.skype",
        "name": "Materials"
    }, {
        "id": "19:b7b84cba410c406ba671dbbf5e0a3519@thread.skype",
        "name": "Design"
    }, {
        "id": "19:fc5db2aed489454e8f8c06829ed6c986@thread.skype",
        "name": "Marketing"
    }]
}
```

#### <a name="net-example"></a>.NET の例

次の例では`FetchChannelList` 、[ボット Builder SDK For .Net の Microsoft Teams 拡張機能](https://www.nuget.org/packages/Microsoft.Bot.Connector.Teams)からの呼び出しを使用しています。

```csharp
ConversationList channels = client.GetTeamsConnectorClient().Teams.FetchChannelList(activity.GetChannelData<TeamsChannelData>().Team.Id);
```

#### <a name="nodejs-example"></a>Node.js の例

次の例で`fetchChannelList`は、 [Node.js の Bot ビルダー SDK の Microsoft Teams 拡張機能](https://www.npmjs.com/package/botbuilder-teams)からの呼び出しを使用しています。

```javascript
var teamId = session.message.sourceEvent.team.id;
connector.fetchChannelList(
  (session.message.address).serviceUrl,
  teamId,
  (err, result) => {
    if (err) {
      session.endDialog('There is an error');
    }
    else {
      session.endDialog('%s', JSON.stringify(result));
    }
  }
);
```