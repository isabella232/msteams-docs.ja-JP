## <a name="add-a-messaging-extension-to-your-app"></a>メッセージング拡張機能をアプリに追加する

メッセージング拡張機能は、ユーザーの要求を待機し、[カード](~/task-modules-and-cards/what-are-cards.md)などの構造化データを使用して応答する、クラウドでホストされるサービスです。 Bot フレームワーク`Activity`オブジェクトを使用して、サービスを Microsoft Teams と統合します。 Bot ビルダー SDK の .NET および node.js 拡張子は、メッセージング拡張機能をアプリに追加するのに役立ちます。

![メッセージング拡張機能のメッセージフローの図](~/assets/images/compose-extensions/ceflow.png)

### <a name="register-in-the-bot-framework"></a>Bot フレームワークに登録する

まだお持ちでない場合は、最初に bot を Microsoft Bot フレームワークに登録する必要があります。 Bot の Microsoft app ID とコールバックエンドポイントは、そこに定義されているように、メッセージング拡張機能でユーザーの要求を受信して応答するために使用されます。 Bot の Microsoft Teams チャネルを必ず有効にしてください。

Bot アプリ ID とアプリパスワードを記録するには、アプリのマニフェストにアプリ ID を指定する必要があります。

### <a name="update-your-app-manifest"></a>アプリのマニフェストを更新する

ボットやタブの場合と同様に、メッセージング拡張機能のプロパティを含むようにアプリの[マニフェスト](~/resources/schema/manifest-schema.md#composeextensions)を更新します。 これらのプロパティは、Microsoft Teams クライアントでメッセージング拡張機能がどのように表示され、動作するかを制御します。 メッセージング内線番号は、マニフェストの v 1.0 以降でサポートされています。

#### <a name="declare-your-messaging-extension"></a>メッセージング拡張機能を宣言する

メッセージング拡張機能を追加するには、 `composeExtensions`プロパティを使用して、マニフェストに新しいトップレベルの JSON 構造を含めます。 現時点では、アプリに対して単一のメッセージング拡張機能の作成が制限されています。

> [!NOTE]
> マニフェストは、として`composeExtensions`メッセージング拡張機能を参照します。 これは、下位互換性を維持するためのものです。

拡張機能の定義は、次の構造を持つオブジェクトです。

| プロパティ名 | 用途 | 必須 |
|---|---|---|
| `botId` | Bot フレームワークに登録された bot の一意の Microsoft アプリ ID。 これは通常、Teams アプリ全体の ID と同じである必要があります。 | はい |
| `scopes` | この内線番号をまたは`personal` `team`範囲に追加できるか、またはその両方を追加できるかを宣言する配列。 | はい |
| `canUpdateConfiguration` | **設定**メニュー項目を有効にします。 | いいえ |
| `commands` | このメッセージング拡張機能がサポートするコマンドの配列です。 コマンドは10個に制限されています。 | はい |

#### <a name="define-commands"></a>コマンドを定義する

メッセージング拡張機能では、ユーザーが [新規作成] ボックスの [**その他のオプション**(**&#8943;**)] ボタンからアプリを選択したときに表示されるコマンドを1つ宣言する必要があります。

![Teams でのメッセージング拡張機能のリストのスクリーンショット](~/assets/images/compose-extensions/compose-extension-list.png)

アプリマニフェストのコマンド項目は、次の構造を持つオブジェクトです。

| プロパティ名 | 用途 | 必須 | マニフェストの最小バージョン |
|---|---|---|---|
| `id` | このコマンドに割り当てる一意の ID です。 ユーザー要求には、この ID が含まれます。 | はい | 1.0 |
| `title` | コマンド名を指定します。 この値は UI に表示されます。 | はい | 1.0 |
| `description` | このコマンドの機能を示すヘルプテキスト。 この値は UI に表示されます。 | はい | 1.0 |
| `type` | コマンドの種類を設定します。 使用可能な値は、`query`、`action` です。 指定しない場合、既定値はに設定されます。`query` | いいえ | 1.4 |
| `initialRun` | 省略可能なパラメーターで`query` 、コマンドと共に使用します。 **True**に設定すると、ユーザーが UI でこのコマンドを選択するとすぐにこのコマンドが実行されることを示します。 | いいえ | 1.0 |
| `fetchTask` | 省略可能なパラメーターで`action` 、コマンドと共に使用します。 **True**に設定すると、[タスクモジュール](~/task-modules-and-cards/what-are-task-modules.md)内に表示するアダプティブカードまたは web url を取得します。 これは、 `action`コマンドへの入力が、パラメーターの静的なセットではなく動的になる場合に使用されます。 **True**に設定すると、コマンドの静的パラメータリストは無視されることに注意してください。 | いいえ | 1.4 |
| `parameters` | コマンドのパラメーターの静的リスト。 | はい | 1.0 |
| `parameter.name` | パラメーターの名前です。 これは、ユーザーの要求でサービスに送信されます。 | はい | 1.0 |
| `parameter.description` | このパラメーターの目的または提供する必要がある値の例について説明します。 この値は UI に表示されます。 | はい | 1.0 |
| `parameter.title` | 簡単なユーザーフレンドリパラメーターのタイトルまたはラベル。 | はい | 1.0 |
| `parameter.inputType` | 必要な入力の種類を設定します。 可能な値`text`は`textarea`、 `number` `date` `time`、、、 `toggle`、です。 既定値はに設定されています`text` | いいえ | 1.4 |
| `context` | メッセージアクションが使用可能なコンテキストを定義する値のオプションの配列です。 可能な値`message`は`compose`、、 `commandBox`、です。 既定値は `["compose", "commandBox"]` です。 | いいえ | 1.5 |
