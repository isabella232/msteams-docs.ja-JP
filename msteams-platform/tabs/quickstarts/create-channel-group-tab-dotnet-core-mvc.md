---
title: ASP.NET コア MVC を使用して [チャネルとグループ] タブを作成する
author: laujan
description: ASP.NET コア MVC を使用してカスタムチャネルおよびグループタブを作成するためのクイックスタートガイド。
ms.topic: quickstart
ms.author: laujan
ms.openlocfilehash: 57c22d10414eb8ec93249584219488397f0b6b33
ms.sourcegitcommit: 4329a94918263c85d6c65ff401f571556b80307b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "41674840"
---
# <a name="create-a-custom-channel-and-group-tab-with-aspnet-core-mvc"></a>ASP.NET コア MVC を使用してカスタムのチャネルとグループタブを作成する

このクイックスタートでは、C# および ASP.Net コア MVC を使用して、カスタムのチャネル/グループタブを作成する手順を説明します。 また、 [Microsoft teams 用のアプリ Studio](~/concepts/build-and-test/app-studio-overview.md)を使用して、アプリマニフェストを完成させ、タブを teams に展開します。

[!INCLUDE [dotnet-core-prereq](~/includes/tabs/dotnet-core-prereq.md)]

## <a name="get-the-source-code"></a>ソースコードを取得する

コマンドプロンプトを開き、タブプロジェクト用の新しいディレクトリを作成します。 開始するための簡単な[チャネルグループタブ](https://github.com/OfficeDev/microsoft-teams-sample-tabs/ChannelGroupTabMVC)プロジェクトを提供しています。 ソースコードを取得するには、zip フォルダーをダウンロードして、ファイルを抽出するか、またはサンプルリポジトリを新しいディレクトリに複製します。

```bash
git clone https://github.com/OfficeDev/microsoft-teams-sample-tabs.git
```

ソースコードを取得したら、Visual Studio を開き、[**プロジェクトまたはソリューションを開く**] を選択します。 Tab アプリケーションディレクトリに移動し、[ **Channelgrouptabmvc .sln**] を開きます。

アプリケーションをビルドして実行するには、 **F5**キーを押すか、[**デバッグ**] メニューの [**デバッグ開始**] を選択します。 ブラウザーで以下の Url に移動し、アプリケーションが正しく読み込まれていることを確認します。

- `http://localhost:44360`
- `http://localhost:44360/privacy`
- `http://localhost:44360/tou`

## <a name="review-the-source-code"></a>ソースコードを確認する

### <a name="startupcs"></a>Startup.cs

このプロジェクトは、セットアップ時に選択されている *[HTTPS の詳細構成*] チェックボックスがオンになっている ASP.NET Core 2.2 Web アプリケーションの空のテンプレートから作成されました。 MVC サービスは、依存関係の挿入フレームワークの`ConfigureServices()`メソッドによって登録されます。 また、空のテンプレートでは、既定で静的コンテンツの提供が有効になっていないため`Configure()` 、このメソッドには静的ファイルミドルウェアが追加されます。

```csharp
public void ConfigureServices(IServiceCollection services)
  {
    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
  }
public void Configure(IApplicationBuilder app)
  {
    app.UseStaticFiles();
    app.UseMvc();
  }
```

### <a name="wwwroot-folder"></a>wwwroot フォルダー

ASP.NET Core で、web ルートフォルダーはアプリケーションが静的ファイルを検索する場所です。

### <a name="appmanifest-folder"></a>Appmanifest.xml フォルダー

このフォルダーには、次の必要なアプリパッケージファイルが含まれています。

- 192 x 192 ピクセルを測定する**フルカラーアイコン**。
- 32 x 32 ピクセルを測定する**透明のアウトラインアイコン**。
- アプリの属性を指定する**manifest.xml**ファイル。

これらのファイルは、タブを Teams にアップロードする際に使用するために、アプリパッケージに圧縮する必要があります。

### <a name="csproj"></a>.csproj

Visual Studio の [ソリューションエクスプローラー] ウィンドウで、プロジェクトを右クリックし、[**プロジェクトファイルの編集**] を選択します。 ファイルの末尾に、アプリケーションのビルド時に zip フォルダーを作成して更新するコードが表示されます。

```xml
<PropertyGroup>
    <PostBuildEvent>powershell.exe Compress-Archive -Path \"$(ProjectDir)AppManifest\*\" -DestinationPath \"$(TargetDir)tab.zip\" -Force</PostBuildEvent>
  </PropertyGroup>

  <ItemGroup>
    <EmbeddedResource Include="AppManifest\icon-outline.png">
      <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </EmbeddedResource>
    <EmbeddedResource Include="AppManifest\icon-color.png">
      <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </EmbeddedResource>
    <EmbeddedResource Include="AppManifest\manifest.json">
      <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </EmbeddedResource>
  </ItemGroup>
```

### <a name="models"></a>モデル

*ChannelGroup.cs*は、構成中にコントローラーから呼び出されるメッセージオブジェクトとメソッドを示しています。

### <a name="views"></a>ビュー

#### <a name="home"></a>ホーム

ASP.NET Core は、 *Index*というファイルをサイトの既定またはホームページとして扱います。 ブラウザーの URL がサイトのルートを指している場合は、アプリケーションのホームページとして、 **cshtml**が表示されます。

#### <a name="shared"></a>Shared

部分的なビューのマークアップ *_Layout*には、アプリケーションの全体的なページ構造と共有のビジュアル要素が含まれています。 Teams ライブラリを参照することもできます。

### <a name="controllers"></a>コントローラー

コントローラーは、ViewBag プロパティを使用して、ビューに値を動的に転送します。

[!INCLUDE [dotnet-ngrok-intro](~/includes/tabs/dotnet-ngrok-intro.md)]

- プロジェクトディレクトリのルートでコマンドプロンプトを開き、次のコマンドを実行します。

```bash
ngrok http https://localhost:443560 -host-header="localhost:44360"
```

- Ngrok は、インターネットからの要求をリッスンし、ポート44355上で実行されている場合にそれらをアプリケーションにルーティングします。  Y8rCgT2b は、 `https://y8rCgT2b.ngrok.io/` ngrok ** ALPHA の数字の HTTPS URL に置き換えられる場所に似ているはずです。

- Ngrok を実行していることを確認し、URL をメモするためには、後で必要になるように、コマンドプロンプトを常に使用してください。

## <a name="update-your-application"></a>アプリケーションを更新する

**Tab. cshtml**アプリケーションは、赤いアイコンまたは灰色のアイコンでタブを表示するための2つのオプションボタンをユーザーに提供します。 **[灰色]** または **[赤**の選択`saveGray()` ] `saveRed()`ボタンを選択する`settings.setValidityState(true)`と、[構成] ページの [**保存**] ボタンが設定され、有効になります。 このコードを使用すると、構成の要件を満たしていることを Teams で認識し、インストールを続行することができます。 Save では、の`settings.setSettings`パラメーターが設定されます。 最後に`saveEvent.notifySuccess()` 、は、コンテンツ URL が正常に解決されたことを示すためにを呼び出します。

[!INCLUDE [dotnet-update-app](~/includes/tabs/dotnet-update-chan-grp-app.md)]

[!INCLUDE [dotnet-upload-to-teams](~/includes/tabs/dotnet-upload-to-teams.md)]