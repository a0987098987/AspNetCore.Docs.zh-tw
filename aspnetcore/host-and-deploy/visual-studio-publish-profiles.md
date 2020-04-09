---
title: 視覺化工作室發佈設定檔 (.pubxml) ASP.NET核心應用部署
author: rick-anderson
description: 了解如何在 Visual Studio 中建立發行設定檔，並使用這些設定檔來管理對各種目標的 ASP.NET Core 應用程式部署。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 274dd2cd528d3766aa07f69aac3470a131c79ffe
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78659373"
---
# <a name="visual-studio-publish-profiles-pubxml-for-aspnet-core-app-deployment"></a>視覺化工作室發佈設定檔 (.pubxml) ASP.NET核心應用部署

作者：[Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) 及 [Rick Anderson](https://twitter.com/RickAndMSFT)

本文件的重點在如何使用 Visual Studio 2019 或更新版本來建立及使用發行設定檔。 使用 Visual Studio 建立的發行設定檔，可搭配 MSBuild 和 Visual Studio 使用。 如需發佈至 Azure 的指示，請參閱<xref:tutorials/publish-to-azure-webapp-using-vs>。

這個`dotnet new mvc`指令產生項目檔案,其中包含以下根級[\<專案>元素](/visualstudio/msbuild/project-element-msbuild):

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
</Project>
```

上述 `<Project>` 元素的 `Sdk` 屬性會從 *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.props* 和 *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*，分別匯入 MSBuild[屬性](/visualstudio/msbuild/msbuild-properties)和[目標](/visualstudio/msbuild/msbuild-targets)。 `$(MSBuildSDKsPath)` (與 Visual Studio 2019 Enterprise) 的預設位置在 *%programfiles(x86)%\Microsoft Visual Studio\2019\Enterprise\MSBuild\Sdks* 資料夾。

`Microsoft.NET.Sdk.Web` (Web SDK) 相依於其他 SDK，包括 `Microsoft.NET.Sdk` (.NET Core SDK) 和 `Microsoft.NET.Sdk.Razor` ([Razor SDK](xref:razor-pages/sdk))。 系統會匯入與每個相依 SDK 相關聯的 MSBuild 屬性和目標。 發佈目標會根據所使用的發佈方法，匯入一組適當的目標。

當 MSBuild 或 Visual Studio 載入專案時，會執行下列高階動作：

* 建置專案
* 計算要發佈的檔案
* 將檔案發佈至目的地

## <a name="compute-project-items"></a>計算專案項目

載入專案時，會計算 [MSBuild 專案項目](/visualstudio/msbuild/common-msbuild-project-items) (檔案)。 項目類型會決定如何處理檔案。 根據預設，*.cs* 檔案會包含在 `Compile` 項目清單中。 `Compile` 項目清單中的檔案會予以編譯。

`Content` 項目清單包含除了組建輸出之外還會另行發佈的檔案。 符合 `wwwroot\**`、`**\*.config` 和 `**\*.json` 模式的檔案預設會包含在 `Content` 項目清單中。 例如，`wwwroot\**` [萬用字元模式](https://gruntjs.com/configuring-tasks#globbing-patterns)會比對 *wwwroot* 資料夾及其子資料夾中的所有檔案。

::: moniker range=">= aspnetcore-3.0"

Web SDK 會匯入[Razor SDK](xref:razor-pages/sdk)。 因此，符合 `**\*.cshtml` 和 `**\*.razor` 模式的檔案也會包含在 `Content` 項目清單中。

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Web SDK 會匯入[Razor SDK](xref:razor-pages/sdk)。 因此，符合 `**\*.cshtml` 模式的檔案也會包含在 `Content` 項目清單中。

::: moniker-end

若要明確地將檔案新增至發佈清單，請直接在 *.csproj* 檔案中新增檔案，如[包含檔案](#include-files)一節中所示。

在 Visual Studio 中選取 [發佈]**** 按鈕，或從命令列發佈時：

* 會計算屬性/項目 (需要建置的檔案)。
* **僅限可視化工作室**:恢復 NuGet 包。 (CLI 的使用者要明確還原。)
* 專案隨即建置。
* 會計算的發佈項目 (需要發佈的檔案)。
* 會發佈專案 (計算的檔案會複製到發佈目的地)。

當 ASP.NET Core 專案參考專案檔中的 `Microsoft.NET.Sdk.Web` 時，*app_offline.htm* 檔案會放在 Web 應用程式目錄的根目錄中。 當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。 如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)。

## <a name="basic-command-line-publishing"></a>基本命令列發佈

命令列發佈適用於所有 .NET Core 支援的平台，而不需要 Visual Studio。 在下列範例中，.NET Core CLI 的 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令是從專案目錄執行 (此目錄包含 *.csproj* 檔案)。 如果專案資料夾不是目前的工作目錄，請在專案檔案路徑中明確傳入。 例如：

```dotnetcli
dotnet publish C:\Webs\Web1
```

執行下列命令來建立及發佈 Web 應用程式：

```dotnetcli
dotnet new mvc
dotnet publish
```

`dotnet publish` 命令會產生下列輸出的變化：

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version {VERSION} for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 36.81 ms for C:\Webs\Web1\Web1.csproj.
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\Web1.Views.dll
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\publish\
```

預設發佈資料夾格式為 *bin\Debug\\{TARGET FRAMEWORK MONIKER}\publish\\*。 例如，*bin\Debug\netcoreapp2.2\publish\\*。

下列命令會指定 `Release` 組建和發佈目錄：

```dotnetcli
dotnet publish -c Release -o C:\MyWebs\test
```

`dotnet publish` 命令會呼叫 MSBuild，這會叫用 `Publish` 目標。 所有傳遞給 `dotnet publish` 的參數都會傳遞給 MSBuild。 `-c` 和 `-o` 參數會分別對應至 MSBuild 的 `Configuration` 和 `OutputPath` 屬性。

您可以使用下列兩種格式其中之一來傳遞 MSBuild 屬性：

* `p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

例如，下列命令會將 `Release` 組建發佈到網路共用。 網路共用以正斜線 (*//r8/*) 指定，適用於所有 .NET Core 支援的平台。

```dotnetcli
dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb
```

確認部署的已發佈應用程式未在執行中。 當應用程式執行時，會鎖定 *publish* 資料夾中的檔案。 因為無法複製鎖定的檔案，所以無法執行部署。

## <a name="publish-profiles"></a>發佈設定檔

本節會使用 Visual Studio 2019 或更新版本來建立發行設定檔。 建立設定檔之後，即可從 Visual Studio 或命令列進行發佈。 發行設定檔可以簡化發佈程序，且設定檔數目並無限制。

請在 Visual Studio 中選擇下列其中一個路徑來建立設定檔：

* 在 [方案總管]**** 中以滑鼠右鍵按一下專案，再選取 [發佈]****。
* 從 [建置]**** 功能表選取 [發佈 {專案名稱}]****。

應用程式功能頁面的 [發佈]**** 索引標籤隨即顯示。 如果專案沒有發行設定檔，則會顯示 [挑選發佈目標]**** 頁面。 系統會要求您選取下列發佈目標之一：

* Azure App Service
* Linux 上的 Azure App Service
* Azure 虛擬機器
* 資料夾
* IIS、FTP、Web Deploy (適用於任何網頁伺服器)
* 匯入設定檔

若要判斷最適合的發佈目標，請參閱[適合我的發佈選項為何](/visualstudio/ide/not-in-toc/web-publish-options)。

選取 [資料夾]**** 發佈目標時，請指定儲存發佈資產的資料夾路徑。 預設資料夾路徑為 *bin\\{PROJECT CONFIGURATION}\\{TARGET FRAMEWORK MONIKER}\publish\\*。 例如，*bin\Release\netcoreapp2.2\publish\\*。 選取 [建立設定檔]**** 按鈕來完成操作。

建立發行設定檔之後，[發佈]**** 索引標籤的內容就會變更。 新建立的設定檔會出現在下拉式清單中。 在以下的下拉式清單中，選取 [建立新設定檔]**** 建立另一個新的設定檔。

Visual Studio 的發佈工具會產生描述發行設定檔的 *Properties/PublishProfiles/{PROFILE NAME}.pubxml* MSBuild 檔案。 *.pubxml* 檔案：

* 包含發佈組態設定且供發佈程序使用。
* 可修改以自訂建置和發佈程序。

發佈到 Azure 目標時，*.pubxml* 檔案會包含您的 Azure 訂用帳戶識別碼。 使用該目標類型時，不建議將此檔案新增至原始檔控制。 發佈到非 Azure 目標時，可放心簽入 *.pubxml* 檔案。

機密資訊 (例如發佈密碼) 會依每個使用者/電腦加密。 其儲存位置是在 *Properties/PublishProfiles/{設定檔名稱}.pubxml.user* 檔案中。 由於此檔案可以儲存機密資訊，因此不應該將它簽入至原始檔控制。

如需如何發佈 ASP.NET Core Web 應用程式的概觀，請參閱<xref:host-and-deploy/index>。 MSBuild 發佈 ASP.NET核心 Web 應用所需的任務和目標在[aspnet/websdk 儲存庫](https://github.com/aspnet/websdk)中是開源的。

以下命令可以使用資料夾、MSDeploy 和[Kudu](https://github.com/projectkudu/kudu/wiki)發表配置檔。 因為 MSDeploy 缺少跨平台支援，所以只有 Windows 支援下列 MSDeploy 選項。

**資料夾 (可跨平台運作)：**

<!--

NOTE: Add back the following 'dotnet publish' folder publish example after https://github.com/aspnet/websdk/issues/888 is resolved.

```dotnetcli
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

-->

```dotnetcli
dotnet build WebApplication.csproj /p:DeployOnBuild=true /p:PublishProfile=<FolderProfileName>
```

**MSDeploy：**

```dotnetcli
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

```dotnetcli
dotnet build WebApplication.csproj /p:DeployOnBuild=true /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

**MSDeploy 套件：**

```dotnetcli
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

```dotnetcli
dotnet build WebApplication.csproj /p:DeployOnBuild=true /p:PublishProfile=<MsDeployPackageProfileName>
```

在前面的範例中:

* `dotnet publish`並支援`dotnet build`Kudu API 從任何平台發布到 Azure。 Visual Studio 發佈支援 Kudu API，但 WebSDK 也支援使用它來跨平台發佈到 Azure。
* 不要傳送`DeployOnBuild`指令`dotnet publish`。

有關詳細資訊,請參閱[Microsoft.NET.Sdk.發布](https://github.com/aspnet/websdk#microsoftnetsdkpublish)。

將含有下列內容的發行設定檔新增至專案的 *Properties/PublishProfiles* 資料夾：

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

## <a name="folder-publish-example"></a>資料夾宣告範例

使用名為*FolderProfile*的設定檔發佈時,請使用以下指令之一:

<!--

NOTE: Temporarily removed until https://github.com/aspnet/websdk/issues/888 is resolved.

* `dotnet publish /p:Configuration=Release /p:PublishProfile=FolderProfile`

-->

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

.NET Core CLI 的 [dotnet build](/dotnet/core/tools/dotnet-build) 命令會呼叫 `msbuild` 來執行建置和發佈程序。 傳入資料夾設定檔時，`dotnet build` 或 `msbuild` 命令是一樣的。 在 Windows 上直接呼叫 `msbuild` 時，會使用 .NET Framework 版本的 MSBuild。 在非資料夾設定檔上呼叫 `dotnet build`：

* 叫用 `msbuild`，這會使用 MSDeploy。
* 導致失敗 (即使是在 Windows 上執行)。 若要使用非資料夾設定檔發佈，請直接呼叫 `msbuild`。

下列資料夾發行設定檔是以 Visual Studio 建立並發佈至網路共用：

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

在上述範例中：

* `<ExcludeApp_Data>` 屬性的出現僅為滿足 XML 結構描述需求。 即使專案根目錄中有 *App_Data* 資料夾，`<ExcludeApp_Data>` 屬性對發佈程序也不具效用。 *App_Data* 資料夾不會受到和 ASP.NET 4.x 專案一樣的特殊處理。

<!--

NOTE: Temporarily removed from 'Using the .NET Core CLI' below until https://github.com/aspnet/websdk/issues/888 is resolved.

    ```dotnetcli
    dotnet publish /p:Configuration=Release /p:PublishProfile=FolderProfile
    ```

-->

* `<LastUsedBuildConfiguration>` 屬性設定為 `Release`。 從 Visual Studio 發佈時，會使用發佈程序開始時的值來設定 `<LastUsedBuildConfiguration>` 值。 `<LastUsedBuildConfiguration>` 很特殊，不應該在匯入的 MSBuild 檔案中被覆寫。 不過，這個屬性可以從命令列使用下列方法之一覆寫。
  * 使用 .NET Core CLI：

    ```dotnetcli
    dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
    ```

  * 使用 MSBuild：

    ```console
    msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
    ```

  如需詳細資訊，請參閱 [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (MSBuild：如何設定組態屬性)。

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>從命令列發佈至 MSDeploy 端點

下列範例使用由 Visual Studio 建立的 ASP.NET Core Web 應用程式，名為 *AzureWebApp*。 並使用 Visual Studio 新增了一個 Azure 應用程式發行設定檔。 如需如何建立設定檔的詳細資訊，請參閱[發行設定檔](#publish-profiles)一節。

若要使用發行設定檔部署應用程式，請從 **Visual Studio 開發人員命令提示字元**執行 `msbuild` 命令。 您可以從 Windows 工作列 [開始]**** 功能表的 [Visual Studio]** 資料夾中存取此命令提示字元。 為方便存取，您可以將命令提示字元新增至 Visual Studio 的 [工具]**** 功能表。 有關詳細資訊,請參閱[可視化工作室的開發人員命令提示](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio)。

MSBuild 使用下列命令語法：

```console
msbuild {PATH} 
    /p:DeployOnBuild=true 
    /p:PublishProfile={PROFILE} 
    /p:Username={USERNAME} 
    /p:Password={PASSWORD}
```

* {PATH} &ndash; 應用程式的專案檔路徑。
* {PROFILE} &ndash; 發行設定檔的名稱。
* {USERNAME} &ndash; MSDeploy 使用者名稱。 {USERNAME} 可以在發行設定檔中找到。
* {PASSWORD} &ndash; MSDeploy 密碼。 您可以從 *{PROFILE}.PublishSettings* 檔案取得 {PASSWORD}。 從下列其中一個位置下載 *.PublishSettings* 檔案：
  * **解決方案資源管理員**:選擇**檢視** > **雲資源管理員**。 連線到您的 Azure 訂用帳戶。 開啟 [應用程式服務]****。 以滑鼠右鍵按一下應用程式。 選取 [下載發行設定檔]****。
  * Azure 門戶:在 Web 應用的 **「概述」** 面板中選擇 **「獲取發佈設定檔**」。

下列範例使用名為 *AzureWebApp - Web Deploy* 的發行設定檔：

```console
msbuild "AzureWebApp.csproj" 
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

您也可以從 Windows 命令殼層透過 .NET Core CLI 的 [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) 命令來使用發行設定檔：

```dotnetcli
dotnet msbuild "AzureWebApp.csproj"
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

> [!IMPORTANT]
> `dotnet msbuild` 命令可跨平台使用，並可在 macOS 和 Linux 上編譯 ASP.NET Core 應用程式。 不過，macOS 和 Linux 上的 MSBuild 無法將應用程式部署至 Azure 或其他 MSDeploy 端點。

## <a name="set-the-environment"></a>設定環境

在發行設定檔 (*.pubxml*) 或專案檔中包括 `<EnvironmentName>` 屬性，以設定應用程式的[環境](xref:fundamentals/environments)：

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

如果您需要進行 *web.config* 轉換 (例如根據設定、設定檔或環境設定環境變數)，請參閱<xref:host-and-deploy/iis/transform-webconfig>。

## <a name="exclude-files"></a>排除檔案

發佈 ASP.NET Core Web 應用程式時，下列資產會包含在內：

* 組建成品
* 符合下列萬用字元模式的資料夾和檔案：
  * `**\*.config` (例如，*web.config*)
  * `**\*.json` (例如，*appsettings.json*)
  * `wwwroot\**`

MSBuild 支援[萬用字元模式](https://gruntjs.com/configuring-tasks#globbing-patterns)。 例如，下列 `<Content>` 元素會抑制在 *wwwroot/content* 資料夾及其子資料夾中複製文字檔 (*.txt*)：

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

您可以將上述標記新增至發行設定檔或 *.csproj* 檔案。 當新增至 *.csproj* 檔案時，此規則會新增至專案中的所有發行設定檔。

以下`<MsDeploySkipRules>`元素從*wwwroot_content 資料夾中*排除所有檔案:

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` 不會從部署網站刪除「略過」** 目標。 這會從部署網站刪除 `<Content>` 鎖定目標的檔案和資料夾。 例如，假設所部署的 Web 應用程式包含下列檔案：

* *Views/Home/About1.cshtml*
* *Views/Home/About2.cshtml*
* *Views/Home/About3.cshtml*

如果新增下列 `<MsDeploySkipRules>` 元素，就不會刪除部署網站上的這些檔案。

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

上述 `<MsDeploySkipRules>` 元素會防止部署「已略過」** 的檔案。 如果已部署這些檔案，它並不會將它們刪除。

下列 `<Content>` 元素會刪除部署網站上已鎖定目標的檔案：

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

使用命令列部署搭配上述 `<Content>` 元素會產生下列輸出變化：

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\{TARGET FRAMEWORK MONIKER}\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="include-files"></a>Include 檔案

下列各節會概述不同方法，以便在發佈階段包含檔案。 [包含一般檔案](#general-file-inclusion)一節使用 `DotNetPublishFiles` 項目，由 Web SDK 中的發佈目標檔案提供。 [包含選擇性檔案](#selective-file-inclusion)一節使用 `ResolvedFileToPublish` 項目，由 .NET Core SDK 中的發佈目標檔案提供。 因為 Web SDK 相依於.NET Core SDK，所以 ASP.NET Core 專案可使用兩個項目的任一個。

### <a name="general-file-inclusion"></a>包含一般檔案

下列範例的 `<ItemGroup>` 元素會示範將位在專案目錄外的資料夾複製到發佈網站的資料夾。 預設包含新增至下列標記 `<ItemGroup>` 中的任何檔案。

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotNetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotNetPublishFiles>
</ItemGroup>
```

上述標記：

* 可以新增至 *.csproj* 檔案或發行設定檔。 如果將它新增至 *.csproj* 檔案，它就會包含在專案的每個發行設定檔中。
* 宣告 `_CustomFiles` 項目來儲存符合 `Include` 屬性萬用字元模式的檔案。 模式中參考的 *images* 資料夾位於專案目錄外。 名為 `$(MSBuildProjectDirectory)` 的[保留屬性](/visualstudio/msbuild/msbuild-reserved-and-well-known-properties)，會解析成專案檔案的絕對路徑。
* 向 `DotNetPublishFiles` 項目提供檔案清單。 根據預設，項目的 `<DestinationRelativePath>` 元素為空白。 預設值會在標記中覆寫，並使用[已知的項目中繼資料](/visualstudio/msbuild/msbuild-well-known-item-metadata)，例如 `%(RecursiveDir)`。 內部文字代表發佈網站的 *wwwroot/images* 資料夾。

### <a name="selective-file-inclusion"></a>包含選擇性檔案

下列範例中的反白顯示標記會示範：

* 將位於專案外部的檔案複製到發佈網站的 *wwwroot* 資料夾。 維持 *ReadMe2.md* 的檔名。
* 排除 *wwwroot\Content* 資料夾。
* 排除 *Views\Home\About2.cshtml*。

[!code-xml[](visual-studio-publish-profiles/samples/Web1.pubxml?highlight=18-23)]

上述範例會使用 `ResolvedFileToPublish` 項目，其預設行為是一律將 `Include` 屬性提供的檔案複製到發佈的網站。 藉由包含附帶內部文字 `Never` 或 `PreserveNewest` 的 `<CopyToPublishDirectory>` 子元素，來覆寫預設行為。 例如：

```xml
<ResolvedFileToPublish Include="..\ReadMe2.md">
  <RelativePath>wwwroot\ReadMe2.md</RelativePath>
  <CopyToPublishDirectory>PreserveNewest</CopyToPublishDirectory>
</ResolvedFileToPublish>
```

如需其他部署範例，請參閱 [Web SDK 存放庫的讀我檔案](https://github.com/aspnet/websdk)。

## <a name="run-a-target-before-or-after-publishing"></a>在發佈之前或之後執行目標

內建的 `BeforePublish` 和 `AfterPublish` 目標可在發佈目標之前或之後執行目標。 將下列元素新增至發行設定檔，即可在發佈之前和之後都記錄主控台訊息：

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a>使用未受信任的憑證來發佈至伺服器

將 `<AllowUntrustedCertificate>` 屬性搭配 `True` 值新增至發行設定檔：

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a>Kudu 服務

若要檢視 Azure App Service Web 應用程式部署中的檔案，請使用 [Kudu 服務](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service)。 將 `scm` 權杖附加至 Web 應用程式名稱。 例如：

| URL                                    | 結果       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | Web 應用程式      |
| `http://mysite.scm.azurewebsites.net/` | Kudu 服務 |

選取[偵錯主控台](https://github.com/projectkudu/kudu/wiki/Kudu-console)功能表項目，以檢視、編輯、刪除或新增檔案。

## <a name="additional-resources"></a>其他資源

* [Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) 可簡化對 IIS 伺服器的 Web 應用程式和網站部署。
* [Web SDK GitHub 儲存庫](https://github.com/aspnet/websdk/issues):檔案問題和請求功能進行部署。
* [從 Visual Studio 將 ASP.NET Web 應用程式發行到 Azure VM](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
* <xref:host-and-deploy/iis/transform-webconfig>
