## <a name="establish-a-secure-tunnel-to-your-tab"></a>タブへのセキュリティで保護されたトンネルの確立

Microsoft Teams は完全なクラウドベースの製品であり、HTTPS エンドポイントを使用してクラウドからタブのコンテンツを利用できるようにする必要があります。 Teams はローカルホスティングを許可しません。 タブを公開 URL に公開するか、またはインターネットに直接接続された URL にローカルポートを公開するプロキシを使用する必要があります。

タブをテストするには、 [ngrok](https://ngrok.com/docs)を使用します。 ローカルコンピューター上で ngrok を実行している間は、サーバーの web エンドポイントを使用できます。 Ngrok を閉じると、次回の開始時に Url が異なるものになります。