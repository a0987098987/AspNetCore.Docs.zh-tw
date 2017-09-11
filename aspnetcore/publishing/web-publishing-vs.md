---
title: "建立 Visual Studio 和 MSBuild 的發行設定檔"
author: rick-anderson
description: "說明 Visual Studio 中的 Web 發佈。"
keywords: "ASP.NET Core, Web 發佈, 發佈, msbuild, Web 部署, Dotnet 發佈, Visual Studio 2017"
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.assetid: 0377a02d-8fda-47a5-929a-24a16e1d2c93
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/web-publishing-vs
ms.openlocfilehash: 872e8c99734a0913770281d9d87b1e30c250ab0a
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/25/2017
---
# <a name="create-publish-profiles-for-visual-studio-and-msbuild-to-deploy-aspnet-core-apps"></a>建立 Visual Studio 和 MSBuild 的發行設定檔，以部署 ASP.NET Core 應用程式。

作者：[Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) 及 [Rick Anderson](https://twitter.com/RickAndMSFT)

本文著重在使用 Visual Studio 2017 建立發行設定檔。 使用 Visual Studio 建立的發行設定檔，可從 MSBuild 和 Visual Studio 2017 執行。

下列 *.csproj* 檔案是使用命令 `dotnet new mvc` 建立：

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
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.1" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.2" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.1" />
  </ItemGroup>

</Project>
```

---

上述標記的 `<Project>` 項目 (在第一行) 的 `Sdk` 屬性會進行下列作業：

* 從開始的 *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* 匯入 `props` 檔案。
* 從結尾的 *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* 匯入目標檔案。

`MSBuildSDKsPath` (與 Visual Studio 2017 Enterprise) 的預設位置在 *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* 資料夾。

`Microsoft.NET.Sdk.Web` 相依於：

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

這會導致匯入下列 `props` 和 `targets`：

* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets

發佈目標會再次根據所使用的發佈方法，匯入正確的目標集。

當 MSBuild 或 Visual Studio 載入專案時，會執行下列高階動作：

* 建置專案
* 計算要發佈的檔案
* 將檔案發佈至目的地

### <a name="compute-project-items"></a>計算專案項目

載入專案時，會計算專案項目 (檔案)。 `item type` 屬性會決定如何處理檔案。 根據預設，*.cs* 檔案會包含在 `Compile` 項目清單中。 `Compile` 項目清單中的檔案會予以編譯。

`Content` 項目清單包含的檔案還會另行發佈到組建輸出。 根據預設，符合模式 wwwroot/** 的檔案會包含在 `Content` 項目中。 [wwwroot/** 是通用慣例模式](https://gruntjs.com/configuring-tasks#globbing-patterns)，指定 *wwwroot* 資料夾**和**子資料夾中的所有檔案。 如果您需要明確地將檔案新增至發佈清單，您可以直接在 *.csproj* 檔案中新增檔案，如[包括檔案](#including-files)中所示。

當您在 Visual Studio 中選取 [發佈] 按鈕，或當您從命令列發佈時：

- 會計算屬性/項目 (需要建置的檔案)。
- 僅限 Visual Studio：還原 NuGet 套件。  (CLI 的使用者要明確還原。)
- 專案隨即建置。
- 會計算的發佈項目 (需要發佈的檔案)。
- 專案已發佈。 (計算的檔案會複製到發佈目的地。)

## <a name="simple-command-line-publishing"></a>簡單的命令列發佈

本節適用於所有 .NET Core 支援的平台，不需要 Visual Studio。 在下列範例中，`dotnet publish` 命令是從專案目錄執行 (其包含 *.csproj* 檔案)。 如果您不在專案資料夾中，您可以明確方式在專案檔案路徑中傳送。 例如: 

```console
dotnet publish  c:/webs/web1
```

執行下列命令來建立及發佈 Web 應用程式：

```console
dotnet new mvc
dotnet restore
dotnet publish
```

`dotnet publish` 會產生與類似下列內容的輸出：

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.1.548.43366
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp1.1\Web1.dll
```

預設發佈資料夾是 `bin\$(Configuration)\netcoreapp<version>\publish`。 `$(Configuration)` 預設為偵錯。 在上例中，`<TargetFramework>` 是 `netcoreapp1.1`。 上例中的實際路徑是 *bin\Debug\netcoreapp1.1\publish*。

`dotnet publish -h` 顯示發佈的說明資訊。

下列命令會指定 `Release` 組建和發佈目錄：

```console
dotnet publish -c Release -o C:/MyWebs/test
```

`dotnet publish` 命令會呼叫 `MSBuild`，後者叫用 `Publish` 目標。 任何傳遞至 `dotnet publish` 的參數都會傳遞至 `MSBuild`。 `-c` 參數對應至 `Configuration` MSBuild 屬性。 `-o` 參數對應至 `OutputPath`。

您可以使用下列兩種格式之一傳遞 MSBuild 屬性：

- ` p:<NAME>=<VALUE>`
- `/p:<NAME>=<VALUE>`

下列命令會將 `Release` 組建發佈到網路共用：

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

網路共用以正斜線 (*//r8/*) 指定，適用於所有 .NET Core 支援的平台。

確認部署的已發佈應用程式未在執行中。 當應用程式執行時，會鎖定 *publish* 資料夾中的檔案。 因為無法複製鎖定的檔案，所以無法執行部署。

## <a name="publish-profiles"></a>發行設定檔

本節會使用 Visual Studio 2017 及更高版本建立發行設定檔。 建立之後，您就可以從 Visual Studio 或命令列發佈。

發行設定檔可簡化發佈程序。 您可以有多個發行設定檔。 若要在 Visual Studio 中建立發行設定檔，以滑鼠右鍵按一下方案總管中的專案，然後選取 [發佈]。 或者，您可以從 [建置] 功能表選取 [發佈 \<專案名稱>] 。 隨即顯示應用程式容量頁面的 [發佈] 索引標籤。 如果專案不包含發行設定檔，則會顯示下列頁面：

![應用程式容量頁面的 [發佈]**** 索引標籤，顯示選取了 Azure、IIS、FTB，資料夾與 Azure。 也會顯示 [create new] (新建) 和 [Select Exiting] (選取結束) 選項按鈕](web-publishing-vs/_static/az.png)

選取 [資料夾] 時，[發佈] 按鈕會建立並發佈發行設定檔資料夾。

![應用程式容量頁面的 [發佈]**** 索引標籤，顯示 Azure、IIS、FTB、資料夾。](web-publishing-vs/_static/pub1.png)

一旦建立發行設定檔，[發佈] 索引標籤即會變更，而您要選取 [建立新的設定檔] 來建立新的設定檔。

![應用程式容量頁面 [發佈]**** 索引標籤：顯示在最後一個步驟中建立的 FolderProfile，以及建立新的設定檔連結。](web-publishing-vs/_static/create_new.png)

發佈精靈支援下列發佈目標：

- Microsoft Azure App Service
- IIS、FTP 等等 (適用於任何網頁伺服器)
- 資料夾
- 匯入設定檔 (可讓您匯入設定檔)。
- Microsoft Azure 虛擬機器

如需詳細資訊，請參閱[哪些發佈選項適合我？](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options)

當您使用 Visual Studio 建立發行設定檔時，會建立 *Properties/PublishProfiles/\<發佈名稱>.pubxml* 的 MSBuild 檔案。 這個 *.pubxml* 檔案是 MSBuild 檔案，而且包含發佈組態設定。 您可以變更這個檔案以自訂建置和發佈處理序。 發佈程序會讀取這個檔案。 `<LastUsedBuildConfiguration>` 很特殊，因為它是全域屬性，不應該在組建匯入的任何檔案中。 如需詳細資訊，請參閱 [MSBuild：如何設定組態屬性](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx)。
*.pubxml* 檔案不應該簽入原始檔控制，因為它會隨 *.user* 檔案而定。 *.user* 檔案絕對不能簽入原始檔控制，因為它可以包含機密資訊，而且只對一位使用者和一部電腦有效。

機密資訊 (例如發佈密碼) 是依每個使用者/電腦加密，且儲存在 *Properties/PublishProfiles/*\<發佈名稱>.pubxml.user 檔案中。 因為這個檔案可以包含機密資訊，所以它「不」應該簽入原始檔控制。

如需如何在 ASP.NET Core 上發佈 Web 應用程式的概觀，請參閱[發佈和部署](index.md)。 [發佈和部署](index.md)是開放原始碼專案，位在 https://github.com/aspnet/websdk。

目前 `dotnet publish` 不能使用發行設定檔。 若要使用發行設定檔，請使用 `dotnet build`。 `dotnet build` 在專案上叫用 MSBuild。 或者，直接呼叫 `msbuild`。

使用發行設定檔時，請設定下列 MSBuild 屬性：

- `DeployOnBuild=true`
- `PublishProfile=<Publish profile name>`

例如，當發行名為 *FolderProfile* 的設定檔時，您可以執行下列命令之一。

- `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
- `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

當您叫用 `dotnet build` 時，它會呼叫 `msbuild` 來執行建置和發佈程序。 當您傳入資料夾設定檔時，呼叫 `dotnet build` 或 `msbuild` 基本上是一樣的。 直接在 Windows 上呼叫 MSBuild 時，您會取得 .NET Framework 版本的 MSBuild。  MSDeploy 目前僅限於 Windows 電腦進行發佈。 在非資料夾設定檔上呼叫 `dotnet build` 會叫用 MSBuild，而 MSBuild 會在非資料夾設定檔上使用 MSDeploy。 在非資料夾設定檔上呼叫 `dotnet build`，會叫用 MSBuild (使用 MSDeploy) 並造成失敗 (即使是在 Windows 平台上執行)。 若要發佈非資料夾的設定檔，請直接呼叫 MSBuild。

下列資料夾發行設定檔是以 Visual Studio 建立並發佈至網路共用：

[!code-xml[Main](web-publishing-vs/sample/FolderProfile.pubxml?highlight=5,9,11,18)]

請注意，`<LastUsedBuildConfiguration>` 設定為 `Release`。  從 Visual Studio 發佈時，會使用發佈程序開始時的值來設定 `<LastUsedBuildConfiguration>` 組態屬性值。 `<LastUsedBuildConfiguration>` 組態屬性很特殊，不應在匯入的 MSBuild 檔案中覆寫。 您可以從命令列覆寫這個屬性。 例如: 

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

使用 MSBuild：

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>從命令列發佈至 MSDeploy 端點

如先前所述，您可以使用 `dotnet publish` 或 `msbuild` 命令發佈。 `dotnet publish` 在 .NET Core 的內容中執行。 `msbuild` 需要 .NET framework，因此限於 Windows 環境。

以 MSDeploy 發佈最簡單的方式，是先在 Visual Studio 2017 中建立發行設定檔，再從命令列使用設定檔。

在下列範例中，我建立了 ASP.NET Core Web 應用程式 (使用 `dotnet new mvc`)，並使用 Visual Studio 新增了 Azure 發行設定檔。

您從 **VS 2017 開發人員命令提示字元**執行 `msbuild`。 開發人員命令提示字元在其路徑中會有正確的 *msbuild.exe*，並設定某些 MSBuild 變數。

MSBuild 使用下列語法：

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile>  /p:Username=<USERNAME> /p:Password=<PASSWORD>`

您可以從 \<發佈名稱>.PublishSettings 檔案取得 `Password`。 *.PublishSettings* 檔案可從以下位置下載：

- 方案總管：以滑鼠右鍵按一下 Web 應用程式，然後選取 [下載發行設定檔]。
- Azure 管理入口網站：從 [Web 應用程式] 刀鋒視窗選取 [取得發行設定檔]。

`Username` 可以在發行設定檔中找到。

下列範例使用「Web11112 - Web 部署」發行設定檔：

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a>排除檔案

發佈 ASP.NET Core Web 應用程式時，會包含 *wwwroot* 資料夾的組建成品和內容。 `msbuild` 支援[通用慣例模式](https://gruntjs.com/configuring-tasks#globbing-patterns)。 例如，下列 `<Content>` 項目標記會排除 *wwwroot/content* 資料夾及其所有子資料夾中的所有文字檔 (*.txt*)。

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

上述標記可以新增至發行設定檔或 *.csproj* 檔案。 當新增至 *.csproj* 檔案時，此規則會新增至專案中的所有發行設定檔。

下列 `<MsDeploySkipRules>` 項目標記會排除 *wwwroot/content* 資料夾中的所有檔案：

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` 不會刪除部署網站「略過」的目標。 會從部署網站刪除 `<Content>` 鎖定目標的檔案和資料夾。 例如，假設您曾使用下列檔案部署 Web 應用程式：

- *Views/Home/About1.cshtml*
- *Views/Home/About2.cshtml*
- *Views/Home/About3.cshtml*

如已新增下列 `<MsDeploySkipRules>` 標記，部署網站就不會刪除這些檔案。

``` xml
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

上例中顯示的 `<MsDeploySkipRules>` 標記會防止部署「略過的」檔案，但這些部署一旦部署之後，就不會被刪除。

下列 `<Content>` 標記可能會刪除部署網站已鎖定目標的檔案：

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

使用含上述 `<Content>` 標記的命令列部署，可能會產生類似下面內容的輸出：

``` console
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

## <a name="including-files"></a>包括檔案

下列標記可用來包含發佈網站 *wwwroot/images* 資料夾之專案目錄外的 *images* 資料夾。

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

標記可以新增至 *.csproj* 檔案或發行設定檔。 如果它新增至 *.csproj* 檔案，會包含在專案的每個發行設定檔中。

下列反白顯示的標記會示範如何：

* 將專案外的檔案複製到 *wwwroot* 資料夾。
* 排除 *wwwroot\Content* 資料夾。
* 排除 *Views\Home\About2.cshtml*。

[!code-xml[Main](web-publishing-vs/sample/FolderProfile2.pubxml?highlight=21-29)]

如需更多的部署範例，請參閱 [WebSDK 讀我檔案](https://github.com/aspnet/websdk)。

### <a name="run-a-target-before-or-after-publishing"></a>在發佈之前或之後執行目標

內建的 `BeforePublish` 和 `AfterPublish` 目標可以用來在發佈目標之前或之後執行目標。 下列標記可以新增至發行設定檔，先將訊息記錄至主控台輸出再發佈：

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a>Kudu 服務

若要檢視 Azure Web 應用程式上的檔案，請使用 [kudu 服務](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service)。 將 `scm` 權杖附加在名稱或 Web 應用程式之後。 例如: 

| URL               | 結果|
| ----------------- | ------------ |
| `http://mysite.azurewebsites.net/` | Web 應用程式 |
| `http://mysite.scm.azurewebsites.net/` | Kudu 服務 |

選取[偵錯主控台](https://github.com/projectkudu/kudu/wiki/Kudu-console)功能表項目以檢視/編輯/刪除/新增檔案。

## <a name="additional-resources"></a>其他資源

- [Web 部署](https://www.iis.net/downloads/microsoft/web-deploy) (msdeploy) 可簡化 IIS 伺服器的 Web 應用程式和網站部署。

- [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues)：部署的檔案問題及要求功能。
