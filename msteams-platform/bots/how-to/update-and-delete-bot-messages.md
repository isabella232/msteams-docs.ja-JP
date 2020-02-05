---
title: Bot から送信されたメッセージの更新と削除
author: WashingtonKayaker
description: Microsoft Teams bot から送信されたメッセージを更新および削除する方法
ms.topic: overview
ms.author: anclear
ms.openlocfilehash: 012a6edb77f75c43cff01c58fb94e03fd4f61a85
ms.sourcegitcommit: 4329a94918263c85d6c65ff401f571556b80307b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "41675009"
---
# <a name="update-and-delete-messages-sent-from-your-bot"></a>Bot から送信されたメッセージの更新と削除

[!INCLUDE [pre-release-label](~/includes/v4-to-v3-pointer-bots.md)]

## <a name="updating-messages"></a>メッセージを更新する

メッセージをデータの静的なスナップショットにするのではなく、bot がメッセージを送信した後にメッセージを動的に更新することができます。 動的メッセージ更新は、投票の更新、ボタンの押した後の使用可能なアクションの変更、またはその他の非同期状態の変更などのシナリオで使用できます。

新しいメッセージは、元の種類と一致する必要はありません。 たとえば、元のメッセージに添付ファイルが含まれていた場合、新しいメッセージは単純なテキストメッセージにすることができます。

# <a name="cnettabdotnet"></a>[C#/.NET](#tab/dotnet)

既存のメッセージを更新するには、 `Activity`既存のアクティビティ ID を持つ新しいオブジェクト`UpdateActivityAsync`を`TurnContext`クラスのメソッドに渡します。 [TurnContextClass](/dotnet/api/microsoft.bot.builder.turncontext?view=botbuilder-dotnet-stable)を*参照してください*。

```csharp
var newActivity = MessageFactory.Text("The new text for the activity");
newActivity.Id = activityId;
await turnContext.UpdateActivityAsync(newActivity, cancellationToken);
```

# <a name="typescriptnodejstabtypescript"></a>[TypeScript/node.js](#tab/typescript)

既存のメッセージを更新するには、 `Activity`既存のアクティビティ ID を持つ新しいオブジェクト`updateActivity`を`TurnContext`オブジェクトのメソッドに渡します。 *「* [Updateactivity](/javascript/api/botbuilder-core/turncontext?view=botbuilder-ts-latest#updateactivity-partial-activity--) 」を参照

```typescript
const newActivity = MessageFactory.text('The new text for the activity');
newActivity.id = activityId;
await turnContext.updateActivity(newActivity);
```

# <a name="pythontabpython"></a>[Python](#tab/python)

既存のメッセージを更新するには、 `Activity`既存のアクティビティ ID を持つ新しいオブジェクト`update_activity`を`TurnContext`クラスのメソッドに渡します。 [TurnContextClass](link to Python API ref docs)を参照してください。

```python

new_activity = MessageFactory.text("The new text for the activity")
new_activity.id = activity_id
update_result = await context.update_activity(new_activity)

```

---

## <a name="deleting-messages"></a>メッセージの削除

Bot フレームワークでは、すべてのメッセージに固有のアクティビティ識別子があります。
次に示すように、Bot フレームワークの`DeleteActivity`メソッドを使用してメッセージを削除できます。

# <a name="cnettabdotnet"></a>[C#/.NET](#tab/dotnet)

そのメッセージを削除するには、そのアクティビティの ID `DeleteActivityAsync`を`TurnContext`クラスのメソッドに渡します。 *「TurnContext」を参照してください* [。](/dotnet/api/microsoft.bot.builder.turncontext.deleteactivityasync?view=botbuilder-dotnet-stable)

```csharp
foreach (var activityId in _list)
{
    await turnContext.DeleteActivityAsync(activityId, cancellationToken);
}
```

# <a name="typescriptnodejstabtypescript"></a>[TypeScript/node.js](#tab/typescript)

そのメッセージを削除するには、そのアクティビティの ID `deleteActivity`を`TurnContext`オブジェクトのメソッドに渡します。 *「* [Deleteactivity](/javascript/api/botbuilder-core/turncontext?view=botbuilder-ts-latest#deleteactivity-string---partial-conversationreference--) 」を参照

```typescript
for (let i = 0; i < activityIds.length; i++) {
    await turnContext.deleteActivity(activityIds[i]);
}
```

# <a name="pythontabpython"></a>[Python](#tab/python)

そのメッセージを削除するには、そのアクティビティの ID `delete_activity`を`TurnContext`オブジェクトのメソッドに渡します。 [Delete_activity](link to Python API ref docs)を参照してください。

```python
for each activity_id in _list:
    await TurnContext.delete_activity(activity_id)
```

---
