---
title: 適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔
author: rick-anderson
description: 了解如何在 Visual Studio 中建立發行設定檔，並使用這些設定檔來管理對各種目標的 ASP.NET Core 應用程式部署。
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 280599ab4b4f0a70d154cc4408e7232aaf766d8e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279554"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔

作者：[Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) 及 [Rick Anderson](https://twitter.com/RickAndMSFT)

本文將焦點放在使用 Visual Studio 2017 來建立及使用發行設定檔。 使用 Visual Studio 建立的發行設定檔，可從 MSBuild 和 Visual Studio 2017 執行。 如需發行到 Azure 的指示，請參閱[使用 Visual Studio 將 ASP.NET Core Web 應用程式發行到 Azure App Service](xref:tutorials/publish-to-azure-webapp-using-vs)。

以下是使用 `dotnet new mvc`命令來建立的專案檔：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.5" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.6" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

---

`<Project>` 元素的 `Sdk` 屬性會完成下列工作：

* 一開始會從 *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* 匯入屬性檔案。
* 從結尾的 *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* 匯入目標檔案。

`MSBuildSDKsPath` (與 Visual Studio 2017 Enterprise) 的預設位置在 *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* 資料夾。

`Microsoft.NET.Sdk.Web` SDK 依存於：

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

這會導致匯入下列屬性和目標：

* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*

發佈目標會根據所使用的發佈方法，匯入一組正確的目標。

當 MSBuild 或 Visual Studio 載入專案時，會執行下列高階動作：

* 建置專案
* 計算要發佈的檔案
* 將檔案發佈至目的地

## <a name="compute-project-items"></a>計算專案項目

載入專案時，會計算專案項目 (檔案)。 `item type` 屬性會決定如何處理檔案。 根據預設，*.cs* 檔案會包含在 `Compile` 項目清單中。 `Compile` 項目清單中的檔案會予以編譯。

`Content` 項目清單包含除了組建輸出之外還會另行發佈的檔案。 符合 `wwwroot/**` 模式的檔案預設會包含在 `Content` 項目中。 `wwwroot/\*\*` [萬用字元模式](https://gruntjs.com/configuring-tasks#globbing-patterns)會比對 [wwwroot] 資料夾**和**子資料夾中的所有檔案。 若要明確地將檔案新增至發佈清單，請直接在 *.csproj* 檔案中新增檔案，如[包含檔案](#include-files)中所示。

在 Visual Studio 中選取 [發佈] 按鈕，或從命令列發佈時：

* 會計算屬性/項目 (需要建置的檔案)。
* **僅限 Visual Studio**：會還原 NuGet 套件。 (CLI 的使用者要明確還原。)
* 專案隨即建置。
* 會計算的發佈項目 (需要發佈的檔案)。
* 會發佈專案 (計算的檔案會複製到發佈目的地)。

當 ASP.NET Core 專案參考專案檔中的 `Microsoft.NET.Sdk.Web` 時，*app_offline.htm* 檔案會放在 Web 應用程式目錄的根目錄中。 當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。 如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)。

## <a name="basic-command-line-publishing"></a>基本命令列發佈

命令列發佈適用於所有 .NET Core 支援的平台，而不需要 Visual Studio。 在下列範例中，是從專案目錄 (其中包含 *.csproj* 檔案) 執行 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令。 如果不在專案資料夾中，請以明確方式傳入專案檔路徑。 例如: 

```console
dotnet publish C:\Webs\Web1
```

執行下列命令來建立及發佈 Web 應用程式：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

[dotnet publish](/dotnet/core/tools/dotnet-publish) 命令會產生類似以下的輸出：

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

預設發佈資料夾是 `bin\$(Configuration)\netcoreapp<version>\publish`。 `$(Configuration)` 的預設值是 *Debug*。 在上述範例中，`<TargetFramework>` 是 `netcoreapp2.0`。

`dotnet publish -h` 顯示發佈的說明資訊。

下列命令會指定 `Release` 組建和發佈目錄：

```console
dotnet publish -c Release -o C:\MyWebs\test
```

[dotnet publish](/dotnet/core/tools/dotnet-publish) 命令會呼叫 MSBuild 來叫用 `Publish` 目標。 所有傳遞給 `dotnet publish` 的參數都會傳遞給 MSBuild。 `-c` 參數對應至 `Configuration` MSBuild 屬性。 `-o` 參數對應至 `OutputPath`。

您可以使用下列兩種格式其中之一來傳遞 MSBuild 屬性：

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

下列命令會將 `Release` 組建發佈到網路共用：

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

網路共用以正斜線 (*//r8/*) 指定，適用於所有 .NET Core 支援的平台。

確認部署的已發佈應用程式未在執行中。 當應用程式執行時，會鎖定 *publish* 資料夾中的檔案。 因為無法複製鎖定的檔案，所以無法執行部署。

## <a name="publish-profiles"></a>發行設定檔

本節會使用 Visual Studio 2017 來建立發行設定檔。 建立之後，即可從 Visual Studio 或命令列進行發佈。

發行設定檔可以簡化發佈程序，且設定檔數目並無限制。 請在 Visual Studio 中選擇下列其中一個路徑來建立設定檔：

* 在 [方案總管] 中的專案上按一下滑鼠右鍵，然後選取 [發佈]。
* 從 [建置] 功能表中選取 [發佈 &lt;project_name&gt;] 。

應用程式容量頁面的 [發佈] 索引標籤隨即顯示。 如果專案缺少發行設定檔，則會顯示下列頁面：

![應用程式容量頁面的 [發佈] 索引標籤](visual-studio-publish-profiles/_static/az.png)

選取 [資料夾] 時，請指定要用來儲存所發佈資產的資料夾路徑。 預設的資料夾是 *bin\Release\PublishOutput*。 請按一下 [建立設定檔] 按鈕來完成操作。

建立發行設定檔之後，[發佈] 索引標籤就會變更。 新建立的設定檔會出現在下拉式清單中。 按一下 [建立新設定檔] 即可建立另一個新的設定檔。

![顯示 FolderProfile 的應用程式容量頁面 [發佈] 索引標籤](visual-studio-publish-profiles/_static/create_new.png)

發佈精靈支援下列發佈目標：

* Azure App Service
* Azure 虛擬機器
* IIS、FTP 等等 (適用於任何網頁伺服器)
* 資料夾
* 匯入設定檔

如需詳細資訊，請參閱[哪些發佈選項適合我？](/visualstudio/ide/not-in-toc/web-publish-options)。

使用 Visual Studio 來建立發行設定檔時，會建立 *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml* MSBuild 檔案。 此 *.pubxml* 檔案是一個 MSBuild 檔案，而且包含發佈組態設定。 您可以變更這個檔案以自訂建置和發佈程序。 發佈程序會讀取這個檔案。 `<LastUsedBuildConfiguration>` 很特殊，因為它是全域屬性，不應該在組建中所匯入的任何檔案中。 如需詳細資訊，請參閱 [MSBuild：如何設定組態屬性](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) \(英文\)。

發佈到 Azure 目標時，*.pubxml* 檔案會包含您的 Azure 訂用帳戶識別碼。 使用該目標類型時，不建議將此檔案新增至原始檔控制。 發佈到非 Azure 目標時，可放心簽入 *.pubxml* 檔案。

機密資訊 (例如發佈密碼) 會依每個使用者/電腦加密。 其儲存位置是在 *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml.user* 檔案中。 由於此檔案可以儲存機密資訊，因此不應該將它簽入至原始檔控制。

如需如何在 ASP.NET Core 上發佈 Web 應用程式的概觀，請參閱[裝載及部署](xref:host-and-deploy/index)。 發佈 ASP.NET Core 應用程式所需的 MSBuild 工作和目標是可從 https://github.com/aspnet/websdk 取得的開放原始碼。

`dotnet publish` 可以使用資料夾、MSDeploy 及 [Kudu](https://github.com/projectkudu/kudu/wiki) 發行設定檔：

資料夾 (可跨平台運作)：

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

MSDeploy (目前僅適用於 Windows，因為 MSDeploy 無法跨平台運作)：

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

MSDeploy 套件 (目前僅適用於 Windows，因為 MSDeploy 無法跨平台運作)：

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

在先前的範例中，請「不要」將 `deployonbuild` 傳遞給 `dotnet publish`。

如需詳細資訊，請參閱 [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)\(英文\)。

`dotnet publish` 支援使用 Kudu API 從任何平台發佈到 Azure。 Visual Studio 發佈支援 Kudu API，但 WebSDK 也支援使用它來跨平台發佈到 Azure。

將含有下列內容的發行設定檔新增至 *Properties/PublishProfiles* 資料夾：

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

執行下列命令以壓縮發佈內容，然後使用 Kudu API 將它發佈到 Azure：

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

使用發行設定檔時，請設定下列 MSBuild 屬性：

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

使用名為 *FolderProfile* 的設定檔來進行發佈時，可以執行下列兩個命令其中之一：

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

叫用 [dotnet build](/dotnet/core/tools/dotnet-build) 時，它會呼叫 `msbuild` 來執行建置和發佈程序。 傳入資料夾設定檔時，呼叫 `dotnet build` 或 `msbuild` 是一樣的。 直接在 Windows 上呼叫 MSBuild 時，會使用 .NET Framework 版本的 MSBuild。 MSDeploy 目前僅限於 Windows 電腦進行發佈。 在非資料夾設定檔上呼叫 `dotnet build` 會叫用 MSBuild，而 MSBuild 會在非資料夾設定檔上使用 MSDeploy。 在非資料夾設定檔上呼叫 `dotnet build`，會叫用 MSBuild (使用 MSDeploy) 並造成失敗 (即使是在 Windows 平台上執行)。 若要發佈非資料夾的設定檔，請直接呼叫 MSBuild。

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

請注意，`<LastUsedBuildConfiguration>` 設定為 `Release`。 從 Visual Studio 發佈時，會使用發佈程序開始時的值來設定 `<LastUsedBuildConfiguration>` 組態屬性值。 `<LastUsedBuildConfiguration>` 組態屬性很特殊，不應該在匯入的 MSBuild 檔案中被覆寫。 您可以從命令列覆寫此屬性。

使用 .NET Core CLI：

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

使用 MSBuild：

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>從命令列發佈至 MSDeploy 端點

您可以使用 .NET Core CLI 或 MSBuild 來完成發佈。 `dotnet publish` 在 .NET Core 的內容中執行。 `msbuild` 命令需要 .NET Framework，因此會將它限制在 Windows 環境。

以 MSDeploy 發佈最簡單的方式，是先在 Visual Studio 2017 中建立發行設定檔，再從命令列使用設定檔。

下列範例會建立 ASP.NET Core Web 應用程式 (使用 `dotnet new mvc`)，並使用 Visual Studio 來新增 Azure 發行設定檔。

請從「VS 2017 的開發人員命令提示字元」執行 `msbuild`。 「開發人員命令提示字元」的路徑中會有已設定一些 MSBuild 變數的正確 *msbuild.exe*。

MSBuild 使用下列語法：

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

請從 *\<發佈名稱>.PublishSettings* 檔案取得 `Password`。 從下列其中一個位置下載 *.PublishSettings* 檔案：

* 方案總管：在 Web 應用程式上按一下滑鼠右鍵，然後選取 [下載發行設定檔]。
* Azure 入口網站：按一下 Web 應用程式 [概觀] 面板上的 [取得發行設定檔]。

`Username` 可以在發行設定檔中找到。

下列範例使用 *Web11112 - Web Deploy*發行設定檔：

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a>排除檔案

發佈 ASP.NET Core Web 應用程式時，會包含 *wwwroot* 資料夾的組建成品和內容。 `msbuild` 支援[通用慣例模式](https://gruntjs.com/configuring-tasks#globbing-patterns)。 例如，下列 `<Content>` 元素會排除 *wwwroot/content* 資料夾及其所有子資料夾中的所有文字檔 (*.txt*)。

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

您可以將上述標記新增至發行設定檔或 *.csproj* 檔案。 當新增至 *.csproj* 檔案時，此規則會新增至專案中的所有發行設定檔。

下列 `<MsDeploySkipRules>` 元素會排除 *wwwroot/content* 資料夾中的所有檔案：

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` 不會從部署網站刪除「略過」目標。 這會從部署網站刪除 `<Content>` 鎖定目標的檔案和資料夾。 例如，假設所部署的 Web 應用程式包含下列檔案：

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

上述 `<MsDeploySkipRules>` 元素會防止部署「已略過」的檔案。 如果已部署這些檔案，它並不會將它們刪除。

下列 `<Content>` 元素會刪除部署網站上已鎖定目標的檔案：

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

使用命令列部署搭配上述 `<Content>` 元素會產生下列輸出：

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
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

下列標記會將專案目錄外的 [images] 資料夾包 含在發佈網站的 *wwwroot/images* 資料夾中：

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

標記可以新增至 *.csproj* 檔案或發行設定檔。 如果將它新增至 *.csproj* 檔案，它就會包含在專案的每個發行設定檔中。

下列反白顯示的標記會示範如何：

* 將專案外的檔案複製到 *wwwroot* 資料夾。
* 排除 *wwwroot\Content* 資料夾。
* 排除 *Views\Home\About2.cshtml*。

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
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

如需更多的部署範例，請參閱 [WebSDK 讀我檔案](https://github.com/aspnet/websdk)。

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

若要檢視 Azure App Service Web 應用程式部署中的檔案，請使用 [Kudu 服務](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service)。 將 `scm` 權杖附加至 Web 應用程式名稱。 例如: 

| URL                                    | 結果       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | Web 應用程式      |
| `http://mysite.scm.azurewebsites.net/` | Kudu 服務 |

選取[偵錯主控台](https://github.com/projectkudu/kudu/wiki/Kudu-console)功能表項目，以檢視、編輯、刪除或新增檔案。

## <a name="additional-resources"></a>其他資源

* [Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) 可簡化對 IIS 伺服器的 Web 應用程式和網站部署。
* [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues)：針對部署提出問題及要求功能。
