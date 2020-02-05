## <a name="upload-your-tab-to-teams-with-app-studio"></a>アプリ Studio を使用して、タブを Teams にアップロードする

>[!NOTE]
> アプリ Studio を使用して、 **manifest.xml**ファイルを編集し、完成したパッケージを Teams にアップロードします。 必要に応じて、 **manifest**を手動で編集することもできます。 その場合は、もう一度ソリューションをビルドして、アップロードする**タブ .zip**ファイルを作成します。

- Microsoft Teams クライアントを開きます。 [Web ベースのバージョン](https://teams.microsoft.com)を使用する場合は、ブラウザーの[開発者ツール](~/tabs/how-to/developer-tools.md)を使用してフロントエンドコードを調べることができます。

- App studio を開き、[**マニフェストエディター** ] タブを選択します。

- [マニフェストエディター] の [**既存のアプリ**タイルをインポートする] を選択して、タブのアプリパッケージの更新を開始します。ソースコードには、部分的に完成したマニフェストがあります。 アプリパッケージの名前は、**タブ .zip**です。 この場所は、ここから入手できます。

```bash
/bin/Debug/netcoreapp2.2/Tab.zip
```

- **タブ .zip**を App Studio にアップロードします。

### <a name="update-your-app-package-with-manifest-editor"></a>マニフェストエディターを使用してアプリパッケージを更新する

アプリパッケージをアプリ Studio にアップロードしたら、構成を完了する必要があります。

- マニフェストエディターのウェルカムページの右パネルで、新しくインポートしたタブのタイルを選択します。

マニフェストエディターの左側と右側に、各手順の値を指定する必要があるプロパティの一覧が表示され、それらの手順の一覧が表示されます。 *マニフェスト*によって提供される情報の多くは、更新する必要があるフィールドがいくつかあります。

#### <a name="details-app-details"></a>詳細: アプリの詳細

[*アプリの詳細*] セクションで、次の手順を実行します。

- [*識別*] で [**生成**] を選択して、アプリの新しいアプリ Id を生成します。

- [*開発者向け情報*] で、 *ngrok* HTTPS url を使用して**web サイトの url**を更新します。

- *アプリ url*の下で、**プライバシー**に`https://<yourngrokurl>/privacy`関する声明と**使用条件**を> に`https://<yourngrokurl>/tou`更新します。

#### <a name="capabilities-tabs"></a>機能: タブ

[*タブ*] セクションで、次の手順を実行します。

- [*個人用タブの追加] で、* [***追加***] を選択します。 ポップアップダイアログウィンドウが表示されます。

- [*名前*] フィールドに入力します。

- [*エンティティ Id* ] フィールドに入力します。

- [*コンテンツの URL* ] フィールドを`https://<yourngrokurl>/personalTab`に更新します。

- [ *Web サイトの URL* ] フィールドは空白のままにします。

- [***保存***] を選択します。

#### <a name="finish-domains-and-permissions"></a>[完了]: ドメインとアクセス許可

[*ドメインと権限*] セクションの [*タブからのドメイン*] フィールドに、NGROK の URL が HTTPS プレフィックスなし`<yourngrokurl>.ngrok.io/`で含まれている必要があります。

##### <a name="finish-test-and-distribute"></a>完了: テストと配布

>[!IMPORTANT]
>右側の [**説明**] フィールドに、次の警告が表示されます。
>
>&#9888; "**validDomains ' 配列は、トンネリングサイトを含むことはできません...**"
>
>タブのテスト中は、この警告を無視できます。

[*テストと配布*] セクションで、次の手順を実行します。

- **[インストール]** を選択します。

- ポップアップウィンドウで、[*ユーザーに追加*する *] が [はい]* に設定されていることを確認し、[*チームに追加*] を [*いいえ*] に設定します。

- **[インストール]** を選択します。

- 次のポップアップウィンドウで [**開く**] を選択すると、タブが表示されます。

## <a name="view-your-personal-tab"></a>個人用タブの表示

- Teams アプリの左端にあるナビゲーションバーで、 `...`メニューを選択します。 個人用アプリの一覧が表示されます。

- 表示するリストからタブを選択します。