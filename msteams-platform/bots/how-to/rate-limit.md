---
title: レート制限
description: Microsoft Teams でのレートの制限とベストプラクティス
keywords: teams のボットレート制限
ms.openlocfilehash: 4e9efab539ec7817d259fd6c149c438ba02e3ce5
ms.sourcegitcommit: 4329a94918263c85d6c65ff401f571556b80307b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "41674750"
---
# <a name="optimize-your-bot-rate-limiting-and-best-practices-in-microsoft-teams"></a>Bot を最適化する: Microsoft Teams でのレートの制限とベストプラクティス

一般的な原則として、アプリケーションでは、個々のチャットまたはチャネル会話に投稿するメッセージ数を制限する必要があります。 これにより、エンドユーザーに "spammy" を感じない最適な操作性が実現されます。

Microsoft Teams とそのユーザーを保護するために、bot Api rate は受信要求を制限します。 この制限を超えるアプリは、エラー `HTTP 429 Too Many Requests`状態を受信します。 すべての要求は、メッセージの送信、チャネルの列挙、名簿の取得など、同じ料金を制限するポリシーに従います。

レート制限の正確な値は変更される可能性があるため、API が戻る`HTTP 429 Too Many Requests`ときに、アプリケーションで適切なバックオフ動作を実装することをお勧めします。

## <a name="handling-rate-limits"></a>処理率の制限

Bot ビルダー SDK 操作を発行するときに、状態`Microsoft.Rest.HttpOperationException`コードを処理および確認できます。

```csharp
try
{
    // Perform Bot Framework operation
    // for example, await connector.Conversations.UpdateActivityAsync(reply);
}
catch (HttpOperationException ex)
{
    if (ex.Response != null && (uint)ex.Response.StatusCode ==  429)
    {
        //Perform retry of the above operation/Action method
    }
}
```

## <a name="best-practices"></a>ベスト プラクティス

通常、応答を受信`HTTP 429`しないようにするには、簡単な予防措置を講じる必要があります。 たとえば、同じ個人用またはチャネル会話に対して複数の要求を発行しないようにします。 代わりに、API 要求のバッチ処理を検討してください。

ランダムな変位による指数バックオフを使用することは、429s を処理するために推奨される方法です。 これにより、複数の要求で再試行時に競合が発生しなくなります。

## <a name="example-detecting-transient-exceptions"></a>例: 一時的な例外を検出する

ここでは、一時的な障害処理アプリケーションブロックによる指数バックオフを使用する例を示します。

[一時的な障害処理ライブラリ](/previous-versions/msp-n-p/hh680901(v=pandp.50))を使用して、バックオフと再試行を実行できます。 NuGet パッケージを入手してインストールするためのガイドラインについては、「[ソリューションに一時的なエラー処理アプリケーションブロックを追加](/previous-versions/msp-n-p/hh680891(v=pandp.50))する」を参照してください。

```csharp
public class BotSdkTransientExceptionDetectionStrategy : ITransientErrorDetectionStrategy
{
    // List of error codes to retry on
    List<int> transientErrorStatusCodes = new List<int>() { 429 };

    public bool IsTransient(Exception ex)
    {
        var httpOperationException = ex as HttpOperationException;
        if (httpOperationException != null)
        {
            return httpOperationException.Response != null &&
                    transientErrorStatusCodes.Contains((int) httpOperationException.Response.StatusCode);
        }

        return false;
    }
}
```

## <a name="example-backoff"></a>例: バックオフ

速度制限の検出に加えて、指数バックオフを実行することもできます。

```csharp
/**
* The first parameter specifies the number of retries before failing the operation.
* The second parameter specifies the minimum and maximum backoff time respectively.
* The last parameter is used to add a randomized  +/- 20% delta to avoid numerous clients retrying simultaneously.
*/
var exponentialBackoffRetryStrategy = new ExponentialBackoff(3, TimeSpan.FromSeconds(2),
                        TimeSpan.FromSeconds(20), TimeSpan.FromSeconds(1));


// Define the Retry Policy
var retryPolicy = new RetryPolicy(new BotSdkTransientExceptionDetectionStrategy(), fixedIntervalRetryStrategy);

//Execute any bot sdk action
await retryPolicy.ExecuteAsync(() => connector.Conversations.ReplyToActivityAsync((Activity)reply)).ConfigureAwait(false);
```

前に説明した`System.Action`再試行ポリシーを使用して、メソッドを実行することもできます。 参照されるライブラリでは、固定の間隔または線形バックオフメカニズムを指定することもできます。

値と戦略を構成ファイルに格納して、実行時に値を微調整および調整することをお勧めします。

詳細については、この便利な「再試行[パターン](/azure/architecture/patterns/retry)」を参照してください。

## <a name="per-bot-per-thread-limit"></a>スレッドの制限ごとに1つの bot

>[!NOTE]
>サービスレベルでのメッセージ分割は、予想される1秒あたりの要求数 (RPS) を超える結果になります。 制限への接近を懸念している場合は、前述のバックオフ戦略を実装する必要があります。 以下の値は、見積のみを対象としています。

この制限は、bot が1回の会話で生成できるトラフィックを制御します。 ここでの会話は、ボットとユーザー、グループチャット、またはチーム内のチャネルの間で1:1 です。

| **シナリオ** | **期間 (秒)** | **許可される最大操作数** |
| --- | --- | --- |
| NewMessage | 1  | 7  |
| NewMessage | 2  | 8  |
| NewMessage | 31 | 60 |
| NewMessage | 3600 | 1800 |
| 次 | 1  | 7  |
| 次 | 2  | 8  |
| 次 | 31 | 60 |
| 次 | 3600 | 1800 |
| NewThread | 1  | 7  |
| NewThread | 2  | 8  |
| NewThread | 31 | 60 |
| NewThread | 3600 | 1800 |
| GetThreadMembers | 1  | 14  |
| GetThreadMembers | 2  | 16  |
| GetThreadMembers | 31 | 120 |
| GetThreadMembers | 3600 | 3600 |
| GetThread | 1  | 14  |
| GetThread | 2  | 16  |
| GetThread | 31 | 120 |
| GetThread | 3600 | 3600 |

## <a name="per-thread-limit-for-all-bots"></a>すべてのボットに対するスレッド数の制限

この制限は、すべてのボットが1つの会話で生成できるトラフィックを制御します。 ここでの会話は、ボットとユーザー、グループチャット、またはチーム内のチャネルの間で1:1 です。

| **シナリオ** | **期間 (秒)** | **許可される最大操作数** |
| --- | --- | --- |
| NewMessage | 1  | 14  |
| NewMessage | 2  | 16  |
| 次 | 1  | 14  |
| 次 | 2  | 16  |
| NewThread | 1  | 14  |
| NewThread | 2  | 16  |
| GetThreadMembers | 1  | 個 |
| GetThreadMembers | 2  | 32 |
| GetThread | 1  | 個 |
| GetThread | 2  | 32 |

## <a name="bot-per-data-center-limit"></a>データセンターあたりの Bot 数の制限

この制限は、1つのデータセンター (複数のテナント) 内のすべてのスレッド間でボットが生成できるトラフィックを制御します。

|**期間 (秒)** | **許可される最大操作数** |
| --- | --- |
| 1  | 1280 |
| 1800 | 8000 |
| 3600 | 15000 |