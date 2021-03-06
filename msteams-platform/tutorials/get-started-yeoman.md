---
title: Microsoft Teams 用のごみ箱のジェネレーターを使い始める
description: Microsoft Teams 用のごみ箱のジェネレーターでのすぐれたアプリの構築を開始する
keywords: nodejs node.js の概要
ms.topic: tutorial
ms.custom: scenarios:getting-started
ms.openlocfilehash: f9b3f165d3b5387f8e7d30563134ed4889920ca5
ms.sourcegitcommit: f9a2f5cedc9d30ef7a9cf78a47d01cfd277e150d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/23/2020
ms.locfileid: "48237994"
---
# <a name="build-your-first-microsoft-teams-app"></a>最初の Microsoft Teams アプリを構築する

>[!Note]
>このチュートリアルは、 [Teams wiki 用のごみ箱のジェネレーター](https://github.com/OfficeDev/generator-teams/wiki/Build-Your-First-Microsoft-Teams-App)からのものです。

このチュートリアルでは、Microsoft Teams を使用して初めて Microsoft Teams アプリを作成する方法について説明します。 [Microsoft Teams アプリのサイドローディングが有効になっ](~/concepts/build-and-test/prepare-your-o365-tenant.md)ていることを前提としています。

![ごみ箱のジェネレーター git](~/assets/yeoman-demo.gif)

## <a name="setup-and-prepare-your-machine"></a>コンピューターをセットアップして準備する

Teams ジェネレーターの使用を開始する前に、次のものをコンピューターにインストールする必要があります。

### <a name="install-node"></a>インストールノード

NodeJS がコンピューターにインストールされている必要があります。 最新の [LTS バージョン](https://nodejs.org)を使用する必要があります。

### <a name="install-a-code-editor"></a>コード エディターのインストール

コードエディターも必要ですが、任意のテキストエディターを自由に使用できます。 ただし、このドキュメントとスクリーンショットのほとんどは、 [Visual Studio Code](https://code.visualstudio.com)の使用方法を示しています。

### <a name="install-yeoman-and-gulp-cli"></a>Gulp CLI をインストールする

Teams ジェネレーターを使用してプロジェクトをスキャフォールディングできるようにするには、Gulp CLI タスクマネージャーに加えて、その他のツールをインストールする必要があります。

コマンドプロンプトを開き、次のように入力します。

```bash
npm install yo gulp-cli --global
```

## <a name="install-the-microsoft-teams-apps-generator---yo-teams"></a>Microsoft Teams Apps generator-Yo Teams をインストールする

Microsoft Teams アプリ用のごみ箱のジェネレーターは、次のコマンドを使用してインストールされます。

```bash
npm install generator-teams --global
```

#### <a name="install-preview-versions"></a>プレビューバージョンをインストールする

次のコマンドを使用して Teams ジェネレーターのプレビューバージョンをインストールする場合は、次のようにします。

```bash
npm install generator-teams@preview --global
```

## <a name="generate-your-project"></a>プロジェクトを生成する

コマンドプロンプトを開き、プロジェクトを作成する新しいディレクトリを作成し、そのディレクトリにコマンドを入力し `yo teams` ます。 これにより Teams アプリジェネレーターが開始され、一連の質問が寄せられます。

![yo teams](~/assets/yeoman-images/teams-first-app-1.png)

最初の質問はプロジェクト名についてのもので、enter キーを押してそのままにしておくことができます。 次の質問は、新しいディレクトリを作成するか、現在のディレクトリを使用するかを尋ねられます。 必要なディレクトリにあるように、enter キーを押します。

次の手順では、プロジェクトのタイトルを求めます。このタイトルは、アプリのマニフェストと説明で使用されます。 その後、会社名の入力を求められます。これはマニフェストでも使用されます。

5番目の質問では、使用するマニフェストのバージョンについて確認します。 このチュートリアルで `v1.5` は、[現在使用可能な汎用スキーマ] を選択します。

その後、ジェネレーターはプロジェクトに追加するアイテムを要求します。 1つまたは複数のアイテムの組み合わせを選択できます。 ここでは、 *タブを選択する*だけです。

![アイテムの選択](~/assets/yeoman-images/teams-first-app-2.png)

選択したアイテムに基づいて、一連のフォローアップ質問が表示されます。

ここで、ソリューションをホストする場所の URL を入力する必要があります。 任意の URL を指定できますが、既定では、ジェネレーターは Azure Web サイトの URL を提案します。

ジェネレーターには、オプトインまたはオプトアウトできる高度な機能が多数組み込まれています。 URL の質問に従って、ソリューションの単体テストを含めるかどうかを尋ねられます。既定値は [はい] です。 このオプションを選択すると、生成されたプロジェクトには、スキャフォールディングされているさまざまなアイテムについて、単体テストフレームワークと既定の単体テストがあります。 このチュートリアルでは、テストフレームワークを含めないことを選択します。

ログ記録を簡単にするために、ログに Azure Application Insights を使用するかどうかを確認するメッセージも表示されます。 [はい] を選択した場合は、Azure Application Insights キーを指定する必要があります。 このチュートリアルでは、Application Insights の使用のオプトアウトを行います。

次の一連の質問は、以前に選択したアイテムを基にしています。 タブの場合は、名前を指定するだけで、必要に応じて、このアプリを SharePoint Online web パーツとして使用できるようにするかどうかを選択できます。 この名前を入力すると、ジェネレーターによってプロジェクトが生成され、すべての依存関係がインストールされます。 これには 1 ~ 2 分かかります。

## <a name="add-some-code-to-your-tab"></a>タブにコードを追加する

ジェネレーターが完了すると、任意のコードエディターでソリューションを開くことができます。 時間をかけて、コードがどのように整理されているかを理解し、 [プロジェクト構造](https://github.com/OfficeDev/generator-teams/wiki/Project-Structure) のドキュメントで詳細を確認できます。

タブはファイル内に配置され `./src/app/scripts/myFirstAppTab/MyFirstAppTab.tsx` ます。 これは、タブの TypeScript に対応した基本クラスです。メソッドを見つけ、 `render()` コントロール内にコードの行を追加して、次のようにします。 `<PanelBody>`

``` TypeScript
<PanelBody>
    <div style={styles.section}>
    Hello World! Yo Teams rocks!
    </div>
</PanelBody>
```

ファイルを保存し、コマンドプロンプトに戻ります。

## <a name="build-your-app"></a>アプリを作成する

これで、プロジェクトをビルドできるようになります。 これは、2つの手順 (または、次の手順を参照) で行われます。

最初に Teams アプリのマニフェストファイルを作成する必要があります。これにより、Microsoft Teams にサイドロードをアップロードします。 これは、Gulp タスクによって実行され `gulp manifest` ます。 これにより、マニフェストが検証され、ディレクトリに zip ファイルが作成され `./package` ます。

ソリューションを構築するには、このコマンドを使用し `gulp build` ます。 これにより、ソリューションがフォルダーに transpile され `./dist` ます。 

## <a name="run-your-app"></a>アプリを実行する

アプリを実行するには、このコマンドを使用し `gulp serve` ます。 これにより、アプリをテストするためのローカル web サーバーが構築および開始されます。 また、プロジェクトでファイルを保存するたびに、アプリケーションが再構築されます。 

これで `http://localhost:3007/myFirstAppTab/` 、tab がレンダリングされていることを確認できるようになります。 ただし、Microsoft Teams ではまだありません。

![ブラウザーでサイトを表示する](~/assets/yeoman-images/teams-first-app-3.png)

## <a name="run-your-app-in-microsoft-teams"></a>Microsoft Teams でアプリを実行する

Microsoft Teams では、アプリを localhost でホストすることはできません。したがって、公開 URL に公開するか、ngrok などのプロキシを使用する必要があります。

朗報として、スキャフォールディングプロジェクトにはこの組み込みがあります。 を実行すると、 `gulp ngrok-serve` ngrok サービスはバックグラウンドで開始され、一意の DNS エントリとパブリック DNS エントリが作成されます。さらに、この一意の URL を使用してマニフェストをパッケージ化して、とまったく同じことを行い `gulp serve` ます。

を実行した後 `gulp ngrok-serve` 、新しい Microsoft Teams チームを作成して作成したら、チーム名をクリックして、Teams の設定に移動し、[ *アプリ*] を選択します。 右下隅に [ *カスタムアプリをアップロード*する] というリンクが表示されたら、それを選択して、という名前のプロジェクトフォルダーとサブフォルダーを参照し `package` ます。 そのフォルダー内の zip ファイルを選択し、[開く] を選択します。 これで、アプリが Microsoft Teams にサイドロードされました。

![サイドロードアプリ](~/assets/yeoman-images/teams-first-app-4.png)

*一般的な*チャネルに戻り、 *+* 新しいタブを追加するには、を選択します。タブの一覧にタブが表示されます。

![[構成] タブ](~/assets/yeoman-images/teams-first-app-5.png)

タブを選択し、指示に従って追加します。 ソースを編集できるカスタム構成ダイアログがあることに注意してください。 [ *保存* ] を選択して、タブをチャネルに追加します。 完了すると、Microsoft Teams 内でタブが読み込まれます。

![teams での実行中のタブ](~/assets/yeoman-images/teams-first-app-6.png)

**Congrats!最初の Microsoft Teams アプリを構築して展開した**
