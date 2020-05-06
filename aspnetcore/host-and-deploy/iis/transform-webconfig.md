---
title: 轉換 web.config
author: rick-anderson
description: 了解如何在發佈 ASP.NET Core 應用程式時轉換 web.config 檔案。
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: host-and-deploy/iis/transform-webconfig
ms.openlocfilehash: f7e1fb0adc669b4bffa02e6688231c8f1447bd98
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775930"
---
# <a name="transform-webconfig"></a><span data-ttu-id="19f89-103">轉換 web.config</span><span class="sxs-lookup"><span data-stu-id="19f89-103">Transform web.config</span></span>

<span data-ttu-id="19f89-104">依[Vijay Ramakrishnan](https://github.com/vijayrkn)</span><span class="sxs-lookup"><span data-stu-id="19f89-104">By [Vijay Ramakrishnan](https://github.com/vijayrkn)</span></span>

<span data-ttu-id="19f89-105">對 *web.config* 檔案的轉換可在發佈應用程式時，根據下列條件進行套用：</span><span class="sxs-lookup"><span data-stu-id="19f89-105">Transformations to the *web.config* file can be applied automatically when an app is published based on:</span></span>

* [<span data-ttu-id="19f89-106">建置組態</span><span class="sxs-lookup"><span data-stu-id="19f89-106">Build configuration</span></span>](#build-configuration)
* [<span data-ttu-id="19f89-107">設定檔</span><span class="sxs-lookup"><span data-stu-id="19f89-107">Profile</span></span>](#profile)
* [<span data-ttu-id="19f89-108">環境</span><span class="sxs-lookup"><span data-stu-id="19f89-108">Environment</span></span>](#environment)
* [<span data-ttu-id="19f89-109">Custom</span><span class="sxs-lookup"><span data-stu-id="19f89-109">Custom</span></span>](#custom)

<span data-ttu-id="19f89-110">這些轉換會針對下列任一 *web.config* 產生案例進行：</span><span class="sxs-lookup"><span data-stu-id="19f89-110">These transformations occur for either of the following *web.config* generation scenarios:</span></span>

* <span data-ttu-id="19f89-111">由 `Microsoft.NET.Sdk.Web` SDK 自動產生。</span><span class="sxs-lookup"><span data-stu-id="19f89-111">Generated automatically by the `Microsoft.NET.Sdk.Web` SDK.</span></span>
* <span data-ttu-id="19f89-112">由開發人員在應用程式的[內容根目錄](xref:fundamentals/index#content-root)中提供。</span><span class="sxs-lookup"><span data-stu-id="19f89-112">Provided by the developer in the [content root](xref:fundamentals/index#content-root) of the app.</span></span>

## <a name="build-configuration"></a><span data-ttu-id="19f89-113">建置組態</span><span class="sxs-lookup"><span data-stu-id="19f89-113">Build configuration</span></span>

<span data-ttu-id="19f89-114">組建組態會第一個執行。</span><span class="sxs-lookup"><span data-stu-id="19f89-114">Build configuration transforms are run first.</span></span>

<span data-ttu-id="19f89-115">請為每個需要進行 *web.config* 轉換的 [組建組態 (Debug|Release)](/dotnet/core/tools/dotnet-publish#options) 包含一個 *web.{CONFIGURATION}.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="19f89-115">Include a *web.{CONFIGURATION}.config* file for each [build configuration (Debug|Release)](/dotnet/core/tools/dotnet-publish#options) requiring a *web.config* transformation.</span></span>

<span data-ttu-id="19f89-116">在下列範例中，會在 *web.Release.config* 中設定一個組態特定的環境變數：</span><span class="sxs-lookup"><span data-stu-id="19f89-116">In the following example, a configuration-specific environment variable is set in *web.Release.config*:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Configuration_Specific" 
                               value="Configuration_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="19f89-117">當組態設定為 *Release* 時，就會套用轉換：</span><span class="sxs-lookup"><span data-stu-id="19f89-117">The transform is applied when the configuration is set to *Release*:</span></span>

```dotnetcli
dotnet publish --configuration Release
```

<span data-ttu-id="19f89-118">組態的 MSBuild 屬性為 `$(Configuration)`。</span><span class="sxs-lookup"><span data-stu-id="19f89-118">The MSBuild property for the configuration is `$(Configuration)`.</span></span>

## <a name="profile"></a><span data-ttu-id="19f89-119">設定檔</span><span class="sxs-lookup"><span data-stu-id="19f89-119">Profile</span></span>

<span data-ttu-id="19f89-120">設定檔轉換會第二個執行，亦即在[組建組態](#build-configuration)轉換之後執行。</span><span class="sxs-lookup"><span data-stu-id="19f89-120">Profile transformations are run second, after [Build configuration](#build-configuration) transforms.</span></span>

<span data-ttu-id="19f89-121">請為每個需要進行 *web.config* 轉換的設定檔組態包含一個 *web.{PROFILE}.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="19f89-121">Include a *web.{PROFILE}.config* file for each profile configuration requiring a *web.config* transformation.</span></span>

<span data-ttu-id="19f89-122">在下列範例中，會在資料夾發佈設定檔的 *web.FolderProfile.config* 中設定一個設定檔特定的環境變數：</span><span class="sxs-lookup"><span data-stu-id="19f89-122">In the following example, a profile-specific environment variable is set in *web.FolderProfile.config* for a folder publish profile:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Profile_Specific" 
                               value="Profile_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="19f89-123">當設定檔為 *FolderProfile* 時，就會套用轉換：</span><span class="sxs-lookup"><span data-stu-id="19f89-123">The transform is applied when the profile is *FolderProfile*:</span></span>

```dotnetcli
dotnet publish --configuration Release /p:PublishProfile=FolderProfile
```

<span data-ttu-id="19f89-124">設定檔名稱的 MSBuild 屬性為 `$(PublishProfile)`。</span><span class="sxs-lookup"><span data-stu-id="19f89-124">The MSBuild property for the profile name is `$(PublishProfile)`.</span></span>

<span data-ttu-id="19f89-125">如果未傳遞任何設定檔，則預設的設定檔名稱為 **FileSystem**，而如果檔案存在於應用程式的內容根目錄中，就會套用 *web.FileSystem.config*。</span><span class="sxs-lookup"><span data-stu-id="19f89-125">If no profile is passed, the default profile name is **FileSystem** and *web.FileSystem.config* is applied if the file is present in the app's content root.</span></span>

## <a name="environment"></a><span data-ttu-id="19f89-126">環境</span><span class="sxs-lookup"><span data-stu-id="19f89-126">Environment</span></span>

<span data-ttu-id="19f89-127">環境轉換會第三個執行，亦即在 [組建組態](#build-configuration)與[設定檔](#profile)轉換之後執行。</span><span class="sxs-lookup"><span data-stu-id="19f89-127">Environment transformations are run third, after [Build configuration](#build-configuration) and [Profile](#profile) transforms.</span></span>

<span data-ttu-id="19f89-128">請為每個需要進行 *web.config* 轉換的[環境](xref:fundamentals/environments)包含一個 *web.{ENVIRONMENT}.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="19f89-128">Include a *web.{ENVIRONMENT}.config* file for each [environment](xref:fundamentals/environments) requiring a *web.config* transformation.</span></span>

<span data-ttu-id="19f89-129">在下列範例中，會在生產環境的 *web.Production.config* 中設定一個環境特定的環境變數：</span><span class="sxs-lookup"><span data-stu-id="19f89-129">In the following example, a environment-specific environment variable is set in *web.Production.config* for the Production environment:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Environment_Specific" 
                               value="Environment_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="19f89-130">當環境為 *Production* 時，就會套用轉換：</span><span class="sxs-lookup"><span data-stu-id="19f89-130">The transform is applied when the environment is *Production*:</span></span>

```dotnetcli
dotnet publish --configuration Release /p:EnvironmentName=Production
```

<span data-ttu-id="19f89-131">環境的 MSBuild 屬性為 `$(EnvironmentName)`。</span><span class="sxs-lookup"><span data-stu-id="19f89-131">The MSBuild property for the environment is `$(EnvironmentName)`.</span></span>

<span data-ttu-id="19f89-132">從 Visual Studio 發佈並使用發行設定檔時，請參閱 <xref:host-and-deploy/visual-studio-publish-profiles#set-the-environment>。</span><span class="sxs-lookup"><span data-stu-id="19f89-132">When publishing from Visual Studio and using a publish profile, see <xref:host-and-deploy/visual-studio-publish-profiles#set-the-environment>.</span></span>

<span data-ttu-id="19f89-133">已指定環境名稱時，`ASPNETCORE_ENVIRONMENT` 環境變數會自動新增至 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="19f89-133">The `ASPNETCORE_ENVIRONMENT` environment variable is automatically added to the *web.config* file when the environment name is specified.</span></span>

## <a name="custom"></a><span data-ttu-id="19f89-134">自訂</span><span class="sxs-lookup"><span data-stu-id="19f89-134">Custom</span></span>

<span data-ttu-id="19f89-135">自訂轉換會第四個執行，亦即在 [組建組態](#build-configuration)[設定檔](#profile)及[環境](#environment)轉換之後執行。</span><span class="sxs-lookup"><span data-stu-id="19f89-135">Custom transformations are run last, after [Build configuration](#build-configuration), [Profile](#profile), and [Environment](#environment) transforms.</span></span>

<span data-ttu-id="19f89-136">請為每個需要進行 *web.config* 轉換的自訂組態包含一個 *{CUSTOM_NAME}.transform* 檔案。</span><span class="sxs-lookup"><span data-stu-id="19f89-136">Include a *{CUSTOM_NAME}.transform* file for each custom configuration requiring a *web.config* transformation.</span></span>

<span data-ttu-id="19f89-137">在下列範例中，會在 *custom.transform* 中設定一個自訂轉換環境變數：</span><span class="sxs-lookup"><span data-stu-id="19f89-137">In the following example, a custom transform environment variable is set in *custom.transform*:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Custom_Specific" 
                               value="Custom_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="19f89-138">將 `CustomTransformFileName` 屬性傳遞給 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令時，就會套用轉換：</span><span class="sxs-lookup"><span data-stu-id="19f89-138">The transform is applied when the `CustomTransformFileName` property is passed to the [dotnet publish](/dotnet/core/tools/dotnet-publish) command:</span></span>

```dotnetcli
dotnet publish --configuration Release /p:CustomTransformFileName=custom.transform
```

<span data-ttu-id="19f89-139">設定檔名稱的 MSBuild 屬性為 `$(CustomTransformFileName)`。</span><span class="sxs-lookup"><span data-stu-id="19f89-139">The MSBuild property for the profile name is `$(CustomTransformFileName)`.</span></span>

## <a name="prevent-webconfig-transformation"></a><span data-ttu-id="19f89-140">防止 web.config 轉換</span><span class="sxs-lookup"><span data-stu-id="19f89-140">Prevent web.config transformation</span></span>

<span data-ttu-id="19f89-141">若要防止轉換 *web.config* 檔案，請設定 `$(IsWebConfigTransformDisabled)` MSBuild 屬性：</span><span class="sxs-lookup"><span data-stu-id="19f89-141">To prevent transformations of the *web.config* file, set the MSBuild property `$(IsWebConfigTransformDisabled)`:</span></span>

```dotnetcli
dotnet publish /p:IsWebConfigTransformDisabled=true
```

## <a name="additional-resources"></a><span data-ttu-id="19f89-142">其他資源</span><span class="sxs-lookup"><span data-stu-id="19f89-142">Additional resources</span></span>

* <span data-ttu-id="19f89-143">[Web 應用程式專案部署的 Web.config 轉換語法](/previous-versions/dd465326(v=vs.100))</span><span class="sxs-lookup"><span data-stu-id="19f89-143">[Web.config Transformation Syntax for Web Application Project Deployment](/previous-versions/dd465326(v=vs.100))</span></span>
* <span data-ttu-id="19f89-144">[使用 Visual Studio 之 Web 專案部署的 Web.config 轉換語法](/previous-versions/aspnet/dd465326(v=vs.110)) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="19f89-144">[Web.config Transformation Syntax for Web Project Deployment Using Visual Studio](/previous-versions/aspnet/dd465326(v=vs.110))</span></span>
