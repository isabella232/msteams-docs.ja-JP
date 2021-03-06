---
title: タスクモジュール送信アクションに応答する
author: clearab
description: メッセージング拡張機能のアクションコマンドからタスクモジュール送信アクションに応答する方法について説明します。
ms.topic: conceptual
ms.author: anclear
ms.openlocfilehash: cc62bd6643fad9b3f2054d6595dd509b75c59680
ms.sourcegitcommit: d0ca6a4856ffd03d197d47338e633126723fa78a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/15/2020
ms.locfileid: "45137662"
---
# <a name="respond-to-the-task-module-submit-action"></a>タスクモジュール送信アクションに応答する

[!include[v4-to-v3-SDK-pointer](~/includes/v4-to-v3-pointer-me.md)]

ユーザーがタスクモジュールを送信すると、web サービスは `composeExtension/submitAction` コマンド id とパラメーター値が設定された invoke メッセージを受け取ります。 アプリには、呼び出しに応答するための5秒があります。それ以外の場合、ユーザーは "アプリに到達できません" というエラーメッセージを受け取り、呼び出しへの応答は Teams クライアントによって無視されます。

応答するには、次のオプションがあります。

* 応答なし-送信アクションを使用して外部システムでプロセスをトリガーし、ユーザーにフィードバックを提供しないようにすることができます。 これは長時間実行されているプロセスで便利な場合があり、別の方法でフィードバックを提供することもできます (たとえば、[予防的なメッセージ](~/bots/how-to/conversations/send-proactive-messages.md)を使用して)。
* [別のタスクモジュール](#respond-with-another-task-module)-複数ステップの操作の一部として、追加のタスクモジュールで応答できます。
* [カード応答](#respond-with-a-card-inserted-into-the-compose-message-area)-ユーザーがメッセージに操作したり、メッセージに挿入したりすることができるカードで応答できます。
* [Bot からのアダプティブカード](#bot-response-with-adaptive-card)。会話にアダプティブカードを直接挿入します。
* [ユーザーの認証を要求する](~/messaging-extensions/how-to/add-authentication.md)
* [ユーザーに追加の構成を提供するよう要求する](~/messaging-extensions/how-to/add-configuration-page.md)

次の表は、メッセージング拡張機能の呼び出し場所 () に基づいて、使用可能な応答の種類を示して `commandContext` います。 認証または構成の場合、ユーザーがフローを完了すると、元の呼び出しが web サービスに再送信されます。

|応答の種類 | hotmail | コマンド バー | message |
|--------------|:-------------:|:-------------:|:---------:|
|カード応答 | x | x | x |
|別のタスクモジュール | x | x | x |
|Adaptive Card 搭載ボット | x |  | x |
| 応答なし | x | x | x |

## <a name="the-submitaction-invoke-event"></a>SubmitAction invoke イベント

次に、呼び出しメッセージを受信する例を示します。

# <a name="cnet"></a>[C#/.NET](#tab/dotnet)

```csharp
protected override async Task<MessagingExtensionActionResponse> OnTeamsMessagingExtensionSubmitActionAsync(
  ITurnContext<IInvokeActivity> turnContext, MessagingExtensionAction action, CancellationToken cancellationToken) {
  //code to handle the submit action
}
```

# <a name="javascriptnodejs"></a>[JavaScript/Node.js](#tab/javascript)

```javascript
class TeamsMessagingExtensionsActionPreview extends TeamsActivityHandler {
  constructor() {
  handleTeamsMessagingExtensionSubmitAction(context, action) {
  
  //code to handle the submit action
    }
  }
}
```

# <a name="json"></a>[JSON](#tab/json)

受け取る JSON オブジェクトの例を次に示します。 パラメーターは、 `commandContext` メッセージング拡張機能がどこからトリガーされたかを示します。 オブジェクトには、 `data` パラメーターとしてフォーム上のフィールドと、ユーザーが送信した値が含まれています。 ここでは、JSON オブジェクトを短縮して、最も関連のあるフィールドを強調表示します。

```json
{
  "name": "composeExtension/submitAction",
  "imdisplayname": "Bob Smith",
  "serviceUrl": "https://smba.trafficmanager.net/amer/",
  "value": {
    "commandId": "giveKudos",
    "commandContext": "compose",
    "context": {
      "theme": "default"
    },
    "data": {
      "id": "submitButton",
      "formField1": "formField1_value",
      "formField2": "formField2_value",
      "formField3": "formField3_value"
    }
  },
  "conversation": {
    "id": "19:7705841b240044b297123ad7f9c99217@thread.skype"
  }
}
```

* * *

## <a name="respond-with-a-card-inserted-into-the-compose-message-area"></a>メッセージの作成領域に挿入されたカードで応答する

要求に応答する最も一般的な方法 `composeExtension/submitAction` は、[メッセージの作成] 領域に挿入されたカードです。 ユーザーは、このカードを会話に送信するかどうかを選択できます。 カードの使用の詳細については[、「カードおよびカードのアクション](~/task-modules-and-cards/cards/cards-actions.md)」を参照してください。

# <a name="cnet"></a>[C#/.NET](#tab/dotnet)

```csharp
protected override async Task<MessagingExtensionActionResponse> OnTeamsMessagingExtensionSubmitActionAsync(
  ITurnContext<IInvokeActivity> turnContext, MessagingExtensionAction action, CancellationToken cancellationToken)
{
    dynamic Data = JObject.Parse(action.Data.ToString());
    var response = new MessagingExtensionActionResponse
    {
        ComposeExtension = new MessagingExtensionResult
        {
            AttachmentLayout = "list",
            Type = "result",
        },
    };
    var card = new HeroCard
    {
        Title = Data["formField1"] as string,
        Subtitle = Data["formField2"]  as string,
        Text = Data["formField3"]  as string,
    };

    var attachments = new List<MessagingExtensionAttachment>();
    attachments.Add(new MessagingExtensionAttachment
    {
        Content = card,
        ContentType = HeroCard.ContentType,
        Preview = card.ToAttachment(),
    });

    response.ComposeExtension.Attachments = attachments;

    return response;

}
```

# <a name="javascriptnodejs"></a>[JavaScript/Node.js](#tab/javascript)

```javascript
class TeamsMessagingExtensionsActionPreview extends TeamsActivityHandler {
  handleTeamsMessagingExtensionSubmitAction(context, action) {
    const data = action.data;
    const heroCard = CardFactory.heroCard(data.title, data.text);
    heroCard.content.subtitle = data.subTitle;
    const attachment = { contentType: heroCard.contentType, content: heroCard.content, preview: heroCard };

    return {
      composeExtension: {
        type: 'result',
        attachmentLayout: 'list',
        attachments: [
          attachment
        ]
      }
    }
  }
}
```

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "composeExtension": {
    "attachmentLayout": "list",
    "type": "result",
    "attachments": [
      {
        "preview": {
          "contentType": "application/vnd.microsoft.card.hero",
          "content": {
            "title": "formField1_value",
            "subtitle": "formField2_value",
            "text": "formField3_value"
          }
        },
        "contentType": "application/vnd.microsoft.card.hero",
        "content": {
          "title": "formField1_value",
          "subtitle": "formField2_value",
          "text": "formField3_value"
        }
      }
    ]
  }
}
```

* * *

## <a name="respond-with-another-task-module"></a>別のタスクモジュールで応答する

追加のタスクモジュールを使用してイベントに応答することもでき `submitAction` ます。 これは、次のような場合に便利です。

* 大量の情報を収集する必要があります。
* ユーザーの入力に基づいて収集している情報を動的に変更する必要がある場合
* ユーザーによって送信された情報を検証する必要があり、何らかの問題が発生した場合に、フォームを再送信してエラーメッセージが表示される可能性がある場合。 

応答のメソッドは、[最初の `fetchTask` イベントに応答する](~/messaging-extensions/how-to/action-commands/create-task-module.md)のと同じです。 Bot フレームワーク SDK を使用している場合は、両方の送信アクションに対して同じイベントがトリガーされます。 これは、正しい応答を決定するロジックを追加する必要があることを意味します。

## <a name="bot-response-with-adaptive-card"></a>アダプティブカードを使用したボット応答

>[!Note]
>このフローでは、 `bot` アプリケーションマニフェストにオブジェクトを追加し、bot に対して必要なスコープが定義されている必要があります。 Bot のメッセージング拡張機能と同じ Id を使用します。

また、サブカード付きのメッセージをボット付きのチャネルに挿入することによって、送信アクションに応答することもできます。 ユーザーは、メッセージを送信する前にプレビューでき、必要に応じて編集や操作を行うこともできます。 これは、アダプティブカード応答を作成する前にユーザーから情報を収集する必要がある場合や、カードを操作した後にカードを更新する必要がある場合に、非常に便利です。 次のシナリオでは、アプリがこのフローをかなり使用して、チャネル会話に構成手順を含めずにポーリングを構成する方法を示します。

1. ユーザーが [メッセージング] 拡張をクリックして、タスクモジュールをトリガーします。
2. ユーザーは、タスクモジュールを使用してポーリングを構成します。
3. タスクモジュールを送信した後、アプリは提供された情報を使用して、投票をアダプティブカードとして作成し、クライアントへの応答として送信し `botMessagePreview` ます。
4. ユーザーは、カードをチャネルに挿入する前に、アダプティブカードメッセージをプレビューできます。 アプリがまだチャネルのメンバーではない場合は、をクリックするとそのアプリが `Send` 追加されます。
   1. ユーザーはメッセージを選択して `Edit` 、元のタスクモジュールに返すこともできます。
5. アダプティブカードとの対話は、メッセージを送信する前に変更します。
6. ユーザーがクリックすると、 `Send` メッセージがチャネルに投稿されます。

### <a name="respond-to-initial-submit-action"></a>最初の送信アクションに応答する

このフローを有効にするには、タスクモジュールは、 `composeExtension/submitAction` bot がチャネルに送信するカードのプレビューを使用して、最初のメッセージに応答する必要があります。 これにより、ユーザーは送信前にカードを確認することができます。また、インストールされていない場合は、お客様が会話に bot をインストールしようとします。

# <a name="cnet"></a>[C#/.NET](#tab/dotnet)

```csharp
protected override async Task<MessagingExtensionActionResponse> OnTeamsMessagingExtensionSubmitActionAsync(
  ITurnContext<IInvokeActivity> turnContext, MessagingExtensionAction action, CancellationToken cancellationToken)
{
  dynamic Data = JObject.Parse(action.Data.ToString());
  var response = new MessagingExtensionActionResponse
  {
    ComposeExtension = new MessagingExtensionResult
    {
      Type = "botMessagePreview",
      ActivityPreview = MessageFactory.Attachment(new Attachment
      {
        Content = new AdaptiveCard("1.0")
        {
          Body = new List<AdaptiveElement>()
          {
            new AdaptiveTextBlock() { Text = "FormField1 value was:", Size = AdaptiveTextSize.Large },
            new AdaptiveTextBlock() { Text = Data["FormField1"] as string }
          },
          Height = AdaptiveHeight.Auto,
          Actions = new List<AdaptiveAction>()
          {
            new AdaptiveSubmitAction
            {
              Type = AdaptiveSubmitAction.TypeName,
              Title = "Submit",
              Data = new JObject { { "submitLocation", "messagingExtensionFetchTask" } },
            },
          }
        },
        ContentType = AdaptiveCard.ContentType
      }) as Activity
    }
  };

  return response;
}
```

# <a name="javascriptnodejs"></a>[JavaScript/Node.js](#tab/javascript)

```javascript
class TeamsMessagingExtensionsActionPreview extends TeamsActivityHandler {
  handleTeamsMessagingExtensionSubmitAction(context, action) {
    const submittedData = action.data;
    const adaptiveCard = CardFactory.adaptiveCard({
      actions: [
        { type: 'Action.Submit', title: 'Submit', data: { submitLocation: 'messagingExtensionSubmit' } }
      ],
      body: [
          { text: 'Adaptive Card from Task Module', type: 'TextBlock', weight: 'bolder' },
          { text: `${ submittedData.Question }`, type: 'TextBlock', id: 'Question' },
          { id: 'Answer', placeholder: 'Answer here...', type: 'Input.Text' },
        {
          choices: [
            { title: submittedData.Option1, value: submittedData.Option1 },
            { title: submittedData.Option2, value: submittedData.Option2 },
            { title: submittedData.Option3, value: submittedData.Option3 }
          ],
          id: 'Choices',
          isMultiSelect: submittedData.MultiSelect,
          style: 'expanded',
          type: 'Input.ChoiceSet'
        }
      ],
      type: 'AdaptiveCard',
      version: '1.0'
    });
    return {
      composeExtension: {
        activityPreview: MessageFactory.attachment(adaptiveCard, null, null, InputHints.ExpectingInput),
        type: 'botMessagePreview'
      }
    };
  }
}
```

# <a name="json"></a>[JSON](#tab/json)

>[!Note]
>には、 `activityPreview` `message` 完全に1つのアダプティブカード添付ファイルがあるアクティビティが含まれている必要があります。 `<< Card Payload >>`値は、送信するカードのプレースホルダーです。

```json
{
  "composeExtension": {
    "type": "botMessagePreview",
    "activityPreview": {
      "type": "message",
      "attachments":  [
        {
          "contentType": "application/vnd.microsoft.card.adaptive",
          "content": << Card Payload >>
        }
      ]
    }
  }
}
```

* * *

### <a name="the-botmessagepreview-send-and-edit-events"></a>BotMessagePreview send および edit イベント

メッセージの内線番号は、次の2つの新しい種類の呼び出しに応答する必要があり `composeExtension/submitAction` `value.botMessagePreviewAction = "send"` `value.botMessagePreviewAction = "edit"` ます。

# <a name="cnet"></a>[C#/.NET](#tab/dotnet)

```csharp
protected override async Task<MessagingExtensionActionResponse> OnTeamsMessagingExtensionBotMessagePreviewEditAsync(
  ITurnContext<IInvokeActivity> turnContext, MessagingExtensionAction action, CancellationToken cancellationToken)
{
  //handle the event
}

protected override async Task<MessagingExtensionActionResponse> OnTeamsMessagingExtensionBotMessagePreviewSendAsync(
  ITurnContext<IInvokeActivity> turnContext, MessagingExtensionAction action, CancellationToken cancellationToken)
{
  //handle the event
}

```

# <a name="javascriptnodejs"></a>[JavaScript/Node.js](#tab/javascript)

```javascript
class TeamsMessagingExtensionsActionPreview extends TeamsActivityHandler {
  handleTeamsMessagingExtensionBotMessagePreviewEdit(context, action) {

    //handle the event
  }
  
  handleTeamsMessagingExtensionBotMessagePreviewSend(context, action) {

    //handle the event
  }
}

```

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "name": "composeExtension/submitAction",
  "type": "invoke",
  "conversation": { "id": "19:c366b75791784100b6e8b515fd55b063@thread.skype" },
  "imdisplayname": "Pranav Smith",
  ...
  "value": {
    "botMessagePreviewAction": "edit | send",
    "botActivityPreview": [
      {
        "type": "message/card",
        "attachments": [
          {
            "content":
              {
                "type": "AdaptiveCard",
                "body": [{<<card payload>>}]
              },
            "contentType" : "application/vnd.microsoft.card.adaptive"
          }
        ],
        "context": { "theme": "default" }
      }
    ],
  }
}
```

* * *

### <a name="respond-to-botmessagepreview-edit"></a>BotMessagePreview 編集に応答する

ユーザーが [**編集**] ボタンをクリックして送信する前にカードを編集する場合は、という呼び出しを受け取り `composeExtension/submitAction` `value.botMessagePreviewAction = edit` ます。 通常、対話を開始した最初の呼び出しに応答して送信したタスクモジュールを返すことによって応答する必要があり `composeExtension/fetchTask` ます。 これにより、ユーザーは元の情報を再入力することで、処理を開始することができます。 また、ユーザーがすべての情報を最初から入力していないように、タスクモジュールを事前入力するために、現在使用可能な情報を使用することも検討する必要があります。

「[最初の `fetchTask` イベントへの対応」を](~/messaging-extensions/how-to/action-commands/create-task-module.md)参照してください。

### <a name="respond-to-botmessagepreview-send"></a>BotMessagePreview send に応答する

ユーザーが [**送信**] ボタンをクリックすると、invoke with が表示され `composeExtension/submitAction` `value.botMessagePreviewAction = send` ます。 Web サービスは、アダプティブカードを使用した予防的なメッセージを作成して会話に送信する必要があります。また、呼び出しに応答することもできます。

# <a name="cnet"></a>[C#/.NET](#tab/dotnet)

```csharp
protected override async Task<MessagingExtensionActionResponse> OnTeamsMessagingExtensionBotMessagePreviewSendAsync(
  ITurnContext<IInvokeActivity> turnContext, MessagingExtensionAction action, CancellationToken cancellationToken)
{
  var activityPreview = action.BotActivityPreview[0];
  var attachmentContent = activityPreview.Attachments[0].Content;
  var previewedCard = JsonConvert.DeserializeObject<AdaptiveCard>(attachmentContent.ToString(),
          new JsonSerializerSettings { NullValueHandling = NullValueHandling.Ignore });
  
  previewedCard.Version = "1.0";

  var responseActivity = Activity.CreateMessageActivity();
  Attachment attachment = new Attachment()
  {
    ContentType = AdaptiveCard.ContentType,
    Content = previewedCard
  };
  responseActivity.Attachments.Add(attachment);
  
  // Attribute the message to the user on whose behalf the bot is posting
  responseActivity.ChannelData = new {
    OnBehalfOf = new []
    {
      new
      {
        ItemId = 0,
        MentionType = "person",
        Mri = turnContext.Activity.From.Id,
        DisplayName = turnContext.Activity.From.Name
      }  
    }
  };
  
  await turnContext.SendActivityAsync(responseActivity);

  return new MessagingExtensionActionResponse();
}
```

# <a name="javascriptnodejs"></a>[JavaScript/Node.js](#tab/javascript)

```javascript
class TeamsMessagingExtensionsActionPreview extends TeamsActivityHandler {
    async handleTeamsMessagingExtensionBotMessagePreviewSend(context, action) {
      // The data has been returned to the bot in the action structure.
      const activityPreview = action.botActivityPreview[0];
      const attachmentContent = activityPreview.attachments[0].content;
      const userText = attachmentContent.body[1].text;
      const choiceSet = attachmentContent.body[3];

      const submitData = {
        MultiSelect: choiceSet.isMultiSelect ? 'true' : 'false',
        Option1: choiceSet.choices[0].title,
        Option2: choiceSet.choices[1].title,
        Option3: choiceSet.choices[2].title,
        Question: userText
      };

      const adaptiveCard = CardFactory.adaptiveCard({
        actions: [
          { type: 'Action.Submit', title: 'Submit', data: { submitLocation: 'messagingExtensionSubmit' } }
        ],
        body: [
          { text: 'Adaptive Card from Task Module', type: 'TextBlock', weight: 'bolder' },
          { text: `${ submitData.Question }`, type: 'TextBlock', id: 'Question' },
          { id: 'Answer', placeholder: 'Answer here...', type: 'Input.Text' },
          {
            choices: [
                { title: submitData.Option1, value: submitData.Option1 },
                { title: submitData.Option2, value: submitData.Option2 },
                { title: submitData.Option3, value: submitData.Option3 }
            ],
            id: 'Choices',
            isMultiSelect: submitData.MultiSelect,
            style: 'expanded',
            type: 'Input.ChoiceSet'
          }
        ],
        type: 'AdaptiveCard',
        version: '1.0'
      });
      const responseActivity = { type: 'message', attachments: [adaptiveCard], channelData: {
          onBehalfOf: [
              { itemId: 0, mentionType: 'person', mri: context.activity.from.id, displayname: context.activity.from.name }
          ]
      }};

      await context.sendActivity(responseActivity);
    }
}
```

# <a name="json"></a>[JSON](#tab/json)

`composeExtension/submitAction`次のような新しいメッセージが表示されます。

```json
{
  "name": "composeExtension/submitAction",
  "type": "invoke",
  "conversation": { "id": "19:c366b75791784100b6e8b515fd55b063@thread.skype" },
  "imdisplayname": "Pranav Smith",
  ...
  "value": {
    "botMessagePreviewAction": "send",
    "botActivityPreview": [
      {
        "type": "message/card",
        "attachments": [
          {
            "content":
              {
                "type": "AdaptiveCard",
                "body": [{<<card payload>>}]
              },
            "contentType" : "application/vnd.microsoft.card.adaptive"
          }
        ],
        "context": { "theme": "default" }
      }
    ],
  }
}
```

* * *

### <a name="user-attribution-for-bots-messages"></a>Bot メッセージのユーザー属性 

Bot がユーザーに代わってメッセージを送信するシナリオでは、attributing は、そのユーザーにメッセージを送信することで、サービスを提供し、より自然な対話フローを実現できます。 この機能を使用すると、bot から送信されたユーザーにメッセージを属性化することができます。

下の図では、左側がユーザーの属性を*持たない*bot によって送信されるカードメッセージで、右側には、ユーザーの属性*を持つ*bot によって送信されたカードがあります。

![スクリーンショット](../../../assets/images/messaging-extension/user-attribution-bots.png)

Teams でユーザーの属性を使用するに `OnBehalfOf` `ChannelData` は、 `Activity` teams に送信されるペイロードで、メンションエンティティをに追加する必要があります。

# <a name="cnet"></a>[C#/.NET](#tab/dotnet-1)

```csharp
    OnBehalfOf = new []
    {
      new
      {
        ItemId = 0,
        MentionType = "person",
        Mri = turnContext.Activity.From.Id,
        DisplayName = turnContext.Activity.From.Name
      }  
    }

```

# <a name="json"></a>[JSON](#tab/json-1)

```json
{
    "text": "Hello World!",
    "ChannelData": {
        "OnBehalfOf": [{
            "itemid": 0,
            "mentionType": "person",
            "mri": "29:orgid:89e6508d-6c0f-4ffe-9f6a-b58416d965ae",
            "displayName": "Sowrabh N R S"
        }]
    }
}
```

* * *

配列のエンティティの説明を次に示し `OnBehalfOf` ます。

#### <a name="details-of--onbehalfof-entity-schema"></a>`OnBehalfOf`エンティティスキーマの詳細

|フィールド|種類|説明|
|:---|:---|:---|
|`itemId`|整数|0である必要があります。|
|`mentionType`|String|"Person" である必要があります。|
|`mri`|String|メッセージの送信先であるユーザーのメッセージリソース識別子 (MRI)。 メッセージの送信者名は "via" として表示され \<user\> \<bot name\> ます。|
|`displayName`|String|個人の名前。 名前解決ができないケースではフォールバックとして使用されます。|
  
## <a name="next-steps"></a>次の手順

検索コマンドを追加する

* [検索コマンドを定義する](~/messaging-extensions/how-to/search-commands/define-search-command.md)

[!include[messaging-extension-learn-more](~/includes/messaging-extensions/learn-more.md)]
