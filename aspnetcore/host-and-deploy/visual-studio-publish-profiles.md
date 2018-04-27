---
title: Visual Studio 發行 ASP.NET Core 應用程式部署的設定檔
author: rick-anderson
description: 了解如何建立發行設定檔，Visual Studio 中，並將它們用於管理 ASP.NET Core 應用程式部署至各種目標。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3dc858793cd4ddb2630d05a5084f4b7caeaa30eb
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Visual Studio 發行 ASP.NET Core 應用程式部署的設定檔

作者：[Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) 及 [Rick Anderson](https://twitter.com/RickAndMSFT)

本文件著重於使用 Visual Studio 2017 建立和使用發行設定檔。 使用 Visual Studio 建立的發行設定檔，可從 MSBuild 和 Visual Studio 2017 執行。 如需發行到 Azure 的指示，請參閱[使用 Visual Studio 將 ASP.NET Core Web 應用程式發行到 Azure App Service](xref:tutorials/publish-to-azure-webapp-using-vs)。

使用命令建立下列專案檔`dotnet new mvc`:

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

`<Project>`項目的`Sdk`屬性將完成下列工作：

* 從該內容檔案匯入 *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props*開頭。
* 從結尾的 *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* 匯入目標檔案。

`MSBuildSDKsPath` (與 Visual Studio 2017 Enterprise) 的預設位置在 *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* 資料夾。

`Microsoft.NET.Sdk.Web` SDK 取決於：

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

這會導致下列的屬性和匯入的目標：

* *$ (MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*
* *$ (MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*
* *$ (MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*
* *$ (MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*

發行目標匯入設定的目標使用的發行方法為基礎的右邊。

當 MSBuild 或 Visual Studio 載入專案時，會發生下列高層級的動作：

* 建置專案
* 計算要發佈的檔案
* 將檔案發佈至目的地

## <a name="compute-project-items"></a>計算專案項目

載入專案時，會計算專案項目 (檔案)。 `item type` 屬性會決定如何處理檔案。 根據預設，*.cs* 檔案會包含在 `Compile` 項目清單中。 `Compile` 項目清單中的檔案會予以編譯。

`Content`項目清單會包含組建輸出除了已發行的檔案。 根據預設，檔案符合模式`wwwroot/**`隨附的`Content`項目。 `wwwroot/\*\*` [通用慣例模式](https://gruntjs.com/configuring-tasks#globbing-patterns)比對中的所有檔案*wwwroot*資料夾**和**子資料夾。 若要明確地將檔案加入至發行清單，加入檔案直接在 *.csproj*檔案中所示[Include 檔](#include-files)。

選取時**發行**按鈕在 Visual Studio 中，或從命令列發行：

* 會計算屬性/項目 (需要建置的檔案)。
* **只有 visual Studio**: NuGet 封裝還原。 (CLI 的使用者要明確還原。)
* 專案隨即建置。
* 會計算的發佈項目 (需要發佈的檔案)。
* 專案發行 （計算的檔案複製到發佈的目的地）。

當 ASP.NET Core 專案參考`Microsoft.NET.Sdk.Web`在專案檔中， *app_offline.htm*檔案會放在 web 應用程式目錄的根目錄。 當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。 如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)。

## <a name="basic-command-line-publishing"></a>基本命令列發行

命令列發行適用於所有.NET Core 支援的平台，而且不需要 Visual Studio。 下列範例中[dotnet 發行](/dotnet/core/tools/dotnet-publish)從專案目錄執行命令 (其中包含 *.csproj*檔案)。 如果不是在專案資料夾中，明確地傳入專案檔案路徑。 例如: 

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

[Dotnet 發行](/dotnet/core/tools/dotnet-publish)命令產生的輸出類似如下：

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

預設發佈資料夾是 `bin\$(Configuration)\netcoreapp<version>\publish`。 預設值為`$(Configuration)`是*偵錯*。 在上述範例中，`<TargetFramework>`是`netcoreapp2.0`。

`dotnet publish -h` 顯示發佈的說明資訊。

下列命令會指定 `Release` 組建和發佈目錄：

```console
dotnet publish -c Release -o C:\MyWebs\test
```

[Dotnet 發行](/dotnet/core/tools/dotnet-publish)命令呼叫 MSBuild，會叫用`Publish`目標。 任何參數傳遞至`dotnet publish`傳遞至 MSBuild。 `-c` 參數對應至 `Configuration` MSBuild 屬性。 `-o` 參數對應至 `OutputPath`。

MSBuild 屬性可以使用下列格式傳遞：

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

下列命令會將 `Release` 組建發佈到網路共用：

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

網路共用以正斜線 (*//r8/*) 指定，適用於所有 .NET Core 支援的平台。

確認部署的已發佈應用程式未在執行中。 當應用程式執行時，會鎖定 *publish* 資料夾中的檔案。 因為無法複製鎖定的檔案，所以無法執行部署。

## <a name="publish-profiles"></a>發行設定檔

本節會使用 Visual Studio 2017 建立發行設定檔。 一旦建立之後，從 Visual Studio 或命令列發行使用。

發行設定檔可簡化在發佈程序，並可以有任意數目的設定檔。 Visual Studio 中建立的發行設定檔，選擇其中一個下列路徑：

* 以滑鼠右鍵按一下方案總管 中的專案，然後選取**發行**。
* 選取**發行&lt;project_name&gt;** 從**建置**功能表。

**發行**容量頁面的 應用程式索引標籤會顯示。 如果專案沒有發行設定檔，會顯示下列頁面：

![應用程式的容量為頁面的 [發行] 索引標籤](visual-studio-publish-profiles/_static/az.png)

當**資料夾**已選取，指定的資料夾路徑來儲存已發行的資產。 預設資料夾是*bin\Release\PublishOutput*。 按一下**建立設定檔**按鈕，以完成。

一旦建立發行設定檔，**發行**索引標籤上的變更。 新建立的設定檔會出現在下拉式清單中。 按一下**建立新的設定檔**建立另一個新的設定檔。

![顯示 FolderProfile 應用程式的容量為頁面的 [發行] 索引標籤](visual-studio-publish-profiles/_static/create_new.png)

發佈精靈支援下列發佈目標：

* Azure App Service
* Azure 虛擬機器
* IIS、 FTP 等 （適用於任何 web 伺服器）
* 資料夾
* 匯入設定檔

如需詳細資訊，請參閱[哪些發佈選項是最適合我](/visualstudio/ide/not-in-toc/web-publish-options)。

使用 Visual Studio 中，建立的發行設定檔時*屬性/PublishProfiles/&lt;profile_name&gt;.pubxml*建立 MSBuild 檔案。 *.Pubxml*檔案是 MSBuild 檔案，而且包含發行組態設定。 自訂建置和發佈程序，可以變更這個檔案。 發佈程序會讀取這個檔案。 `<LastUsedBuildConfiguration>` 是特殊，因為它是全域屬性，且不應該在組建中匯入任何檔案中。 請參閱[MSBuild： 如何設定組態屬性](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx)如需詳細資訊。

當您發行至 Azure 的目標， *.pubxml*檔案包含您的 Azure 訂用帳戶識別碼。 與這個目標類型，不建議將此檔案加入至原始檔控制。 發行到非 Azure 目標時，您可以放心簽入 *.pubxml*檔案。

加密機密資訊 （例如發行密碼） 在每個使用者/電腦層級。 它會儲存在*屬性/PublishProfiles/&lt;profile_name&gt;。 pubxml.user*檔案。 因為這個檔案可以儲存敏感資訊，它不應該簽入原始檔控制中。

如需如何發行 ASP.NET core web 應用程式的概觀，請參閱[主機和部署](xref:host-and-deploy/index)。 MSBuild 工作和發行 ASP.NET Core 應用程式的必要的目標是開放原始碼在https://github.com/aspnet/websdk。

`dotnet publish` 可以使用 MSDeploy，資料夾和[Kudu](https://github.com/projectkudu/kudu/wiki)發行設定檔：

（適用於跨平台） 的資料夾：

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

MSDeploy （目前這僅適用於 Windows 因為 MSDeploy 並不跨平台）：

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

MSDeploy 封裝 （目前這僅適用於 Windows 因為 MSDeploy 並不跨平台）：

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

在上述範例中，**不**傳遞`deployonbuild`至`dotnet publish`。

如需詳細資訊，請參閱[Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)。

`dotnet publish` 支援從任何平台發行至 Azure 的 Kudu Api。 Visual Studio 發行的支援 Kudu 應用程式開發介面，但它會受到 WebSDK 跨平台發行至 Azure。

新增至發行設定檔*屬性/PublishProfiles*資料夾具有下列內容：

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

執行下列命令來壓縮發行內容，並將它發行到 Azure 中使用 Kudu Api:

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

使用發行設定檔時，請設定下列 MSBuild 屬性：

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

發佈與名為設定檔時*FolderProfile*，可以執行下列命令之一：

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

叫用時[dotnet 組建](/dotnet/core/tools/dotnet-build)，它會呼叫`msbuild`來執行建置和發佈程序。 呼叫`dotnet build`或`msbuild`相當資料夾設定檔中傳遞時。 在 Windows 上直接呼叫 MSBuild 時，會使用 MSBuild 的.NET Framework 版本。 MSDeploy 目前僅限於 Windows 電腦進行發佈。 在非資料夾設定檔上呼叫 `dotnet build` 會叫用 MSBuild，而 MSBuild 會在非資料夾設定檔上使用 MSDeploy。 在非資料夾設定檔上呼叫 `dotnet build`，會叫用 MSBuild (使用 MSDeploy) 並造成失敗 (即使是在 Windows 平台上執行)。 若要發佈非資料夾的設定檔，請直接呼叫 MSBuild。

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

請注意，`<LastUsedBuildConfiguration>` 設定為 `Release`。 從 Visual Studio 發佈時，會使用發佈程序開始時的值來設定 `<LastUsedBuildConfiguration>` 組態屬性值。 `<LastUsedBuildConfiguration>`組態屬性是特殊的且不應覆寫中的匯入的 MSBuild 檔案。 這個屬性可以覆寫從命令列。

使用.NET Core CLI:

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

使用 MSBuild：

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>從命令列發佈至 MSDeploy 端點

可以使用.NET 核心 CLI 或 MSBuild 完成發行。 `dotnet publish` 在 .NET Core 的內容中執行。 `msbuild`命令需要.NET Framework 中，這會限制 Windows 環境。

以 MSDeploy 發佈最簡單的方式，是先在 Visual Studio 2017 中建立發行設定檔，再從命令列使用設定檔。

在下列範例中，建立 ASP.NET Core web 應用程式 (使用`dotnet new mvc`)，並使用 Visual Studio 加入 Azure 發行設定檔。

執行`msbuild`從**VS 2017 的開發人員命令提示字元**。 開發人員命令提示字元中具有正確*msbuild.exe*在其路徑與一些 MSBuild 在設定變數中。

MSBuild 使用下列語法：

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

取得`Password`從*\<發行名稱 >。PublishSettings*檔案。 下載*。PublishSettings*從檔案：

* 方案總管: 以滑鼠右鍵按一下 Web 應用程式，然後選取**下載發行設定檔**。
* Azure 入口網站： 按一下**取得發行設定檔**上 Web 應用程式的**概觀**面板。

`Username` 可以在發行設定檔中找到。

下列範例會使用*Web11112-Web Deploy*發行設定檔：

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a>排除檔案

發佈 ASP.NET Core Web 應用程式時，會包含 *wwwroot* 資料夾的組建成品和內容。 `msbuild` 支援[通用慣例模式](https://gruntjs.com/configuring-tasks#globbing-patterns)。 例如，下列`<Content>`項目排除所有的文字 (*.txt*) 從檔案*wwwroot/content*資料夾及其所有子資料夾。

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

上述的標記可以加入至發行設定檔或 *.csproj*檔案。 當新增至 *.csproj* 檔案時，此規則會新增至專案中的所有發行設定檔。

下列`<MsDeploySkipRules>`項目排除所有的檔案從*wwwroot/content*資料夾：

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` 不會刪除*略過*從部署站台的目標。 `<Content>` 目標的檔案與資料夾會從部署網站刪除。 例如，假設已部署的 web 應用程式有下列檔案：

* *Views/Home/About1.cshtml*
* *Views/Home/About2.cshtml*
* *Views/Home/About3.cshtml*

如果下列`<MsDeploySkipRules>`加入的項目，這些檔案不會刪除部署站台上。

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

上述`<MsDeploySkipRules>`項目防止*略過*部署的檔案。 在部署之後，它不會刪除這些檔案。

下列`<Content>`項目刪除部署位置的目標的檔案：

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

使用命令列部署與前一個`<Content>`項目會產生下列輸出：

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

下列標記包含*映像*資料夾要在專案目錄外的*wwwroot/影像*發佈站台資料夾的：

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

標記可以新增至 *.csproj* 檔案或發行設定檔。 如果要加入至 *.csproj*檔案，它是否包含在專案中的每個發行設定檔中。

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

內建`BeforePublish`和`AfterPublish`目標之前或之後發行的目標，請執行目標。 將下列項目加入至記錄主控台訊息之前和之後發行的發行設定檔：

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a>發行至伺服器，使用不受信任的憑證

新增`<AllowUntrustedCertificate>`屬性值為`True`至發行設定檔：

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a>Kudu 服務

若要檢視 Azure App Service web 應用程式部署的檔案，使用[Kudu 服務](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service)。 附加`scm`web 應用程式名稱語彙基元。 例如: 

| URL                                    | 結果       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | Web 應用程式      |
| `http://mysite.scm.azurewebsites.net/` | Kudu 服務 |

選取[偵錯主控台](https://github.com/projectkudu/kudu/wiki/Kudu-console)功能表項目來檢視、 編輯、 刪除或加入檔案。

## <a name="additional-resources"></a>其他資源

* [Web 部署](https://www.iis.net/downloads/microsoft/web-deploy)(MSDeploy) 可簡化部署的 web 應用程式和網站的 IIS 伺服器。
* [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues)： 檔案問題，並要求部署的功能。
