---
title: 在 Windows 服務上裝載 ASP.NET Core
author: guardrex
description: 了解如何在 Windows 服務上裝載 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/04/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: ec3a37fd859df7592fa0d6d9cc0109942a570e7a
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086994"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="8a949-103">在 Windows 服務上裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8a949-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="8a949-104">作者：[Luke Latham](https://github.com/guardrex) 和 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8a949-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="8a949-105">ASP.NET Core 應用程式可以裝載在 Windows 上作為 [Windows 服務](/dotnet/framework/windows-services/introduction-to-windows-service-applications)，不需要使用 IIS。</span><span class="sxs-lookup"><span data-stu-id="8a949-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="8a949-106">當裝載為 Windows 服務時，應用程式將會在重新開機後自動啟動。</span><span class="sxs-lookup"><span data-stu-id="8a949-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="8a949-107">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8a949-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8a949-108">必要條件</span><span class="sxs-lookup"><span data-stu-id="8a949-108">Prerequisites</span></span>

* [<span data-ttu-id="8a949-109">PowerShell 6.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="8a949-109">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

> [!NOTE]
> <span data-ttu-id="8a949-110">針對早於 Windows 10 2018 年 10 月更新 (1809 版/組建 10.0.17763) 的 Windows OS，必須使用 [WindowsCompatibility module](https://github.com/PowerShell/WindowsCompatibility) \(英文\) 來匯入 [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts) 模組，以存取用於[建立使用者帳戶](#create-a-user-account)一節中的 [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) Cmdlet：</span><span class="sxs-lookup"><span data-stu-id="8a949-110">For Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763), the [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts) module must be imported with the [WindowsCompatibility module](https://github.com/PowerShell/WindowsCompatibility) to gain access to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet used in the [Create a user account](#create-a-user-account) section:</span></span>
>
> ```powershell
> Install-Module WindowsCompatibility -Scope CurrentUser
> Import-WinModule Microsoft.PowerShell.LocalAccounts
> ```

## <a name="deployment-type"></a><span data-ttu-id="8a949-111">部署類型</span><span class="sxs-lookup"><span data-stu-id="8a949-111">Deployment type</span></span>

<span data-ttu-id="8a949-112">您可以建立架構相依或自封式 Windows 服務部署。</span><span class="sxs-lookup"><span data-stu-id="8a949-112">You can create either a framework-dependent or self-contained Windows Service deployment.</span></span> <span data-ttu-id="8a949-113">如需詳細資訊與部署案例建議，請參閱 [.NET Core 應用程式部署](/dotnet/core/deploying/)。</span><span class="sxs-lookup"><span data-stu-id="8a949-113">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment"></a><span data-ttu-id="8a949-114">與 Framework 相依的部署</span><span class="sxs-lookup"><span data-stu-id="8a949-114">Framework-dependent deployment</span></span>

<span data-ttu-id="8a949-115">架構相依部署 (FDD) 仰賴存在於目標系統上的全系統共用 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="8a949-115">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="8a949-116">搭配 ASP.NET Core Windows 服務應用程式使用 FDD 案例時，SDK 會產生可執行檔 (*\*.exe*)，稱為架構相依可執行檔。</span><span class="sxs-lookup"><span data-stu-id="8a949-116">When the FDD scenario is used with an ASP.NET Core Windows Service app, the SDK produces an executable (*\*.exe*), called a *framework-dependent executable*.</span></span>

### <a name="self-contained-deployment"></a><span data-ttu-id="8a949-117">自封式部署</span><span class="sxs-lookup"><span data-stu-id="8a949-117">Self-contained deployment</span></span>

<span data-ttu-id="8a949-118">自封式部署 (SCD) 不仰賴任何存在於目標系統上的共用元件。</span><span class="sxs-lookup"><span data-stu-id="8a949-118">Self-contained deployment (SCD) doesn't rely on the presence of shared components on the target system.</span></span> <span data-ttu-id="8a949-119">執行階段與應用程式的相依性會隨著應用程式部署到裝載系統。</span><span class="sxs-lookup"><span data-stu-id="8a949-119">The runtime and the app's dependencies are deployed with the app to the hosting system.</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="8a949-120">將專案轉換成 Windows 服務</span><span class="sxs-lookup"><span data-stu-id="8a949-120">Convert a project into a Windows Service</span></span>

<span data-ttu-id="8a949-121">對現有的 ASP.NET Core 專進行下列變更，以服務形式執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="8a949-121">Make the following changes to an existing ASP.NET Core project to run the app as a service:</span></span>

### <a name="project-file-updates"></a><span data-ttu-id="8a949-122">專案檔更新</span><span class="sxs-lookup"><span data-stu-id="8a949-122">Project file updates</span></span>

<span data-ttu-id="8a949-123">視您選擇的[部署類型](#deployment-type)而定，更新專案檔：</span><span class="sxs-lookup"><span data-stu-id="8a949-123">Based on your choice of [deployment type](#deployment-type), update the project file:</span></span>

#### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="8a949-124">架構相依部署 (FDD)</span><span class="sxs-lookup"><span data-stu-id="8a949-124">Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="8a949-125">將 Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog) 新增至包含目標 Framework 的 `<PropertyGroup>`。</span><span class="sxs-lookup"><span data-stu-id="8a949-125">Add a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="8a949-126">在下列範例中，RID 已設定為 `win7-x64`。</span><span class="sxs-lookup"><span data-stu-id="8a949-126">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="8a949-127">新增 `<SelfContained>` 屬性集到 `false`。</span><span class="sxs-lookup"><span data-stu-id="8a949-127">Add the `<SelfContained>` property set to `false`.</span></span> <span data-ttu-id="8a949-128">這些屬性會指示 SDK 產生適用於 Windows 的可執行檔 (*.exe*)。</span><span class="sxs-lookup"><span data-stu-id="8a949-128">These properties instruct the SDK to generate an executable (*.exe*) file for Windows.</span></span>

<span data-ttu-id="8a949-129">針對 Windows Services 應用程式，不需要 *web.config* 檔案 (發行 ASP.NET Core 應用程式時通常會產生此檔案)。</span><span class="sxs-lookup"><span data-stu-id="8a949-129">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="8a949-130">若要停用 *web.config* 檔案的建立，請新增 `<IsTransformWebConfigDisabled>` 屬性集到 `true`。</span><span class="sxs-lookup"><span data-stu-id="8a949-130">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="8a949-131">新增 `<UseAppHost>` 屬性集到 `true`。</span><span class="sxs-lookup"><span data-stu-id="8a949-131">Add the `<UseAppHost>` property set to `true`.</span></span> <span data-ttu-id="8a949-132">此屬性為服務提供 FDD 的啟用路徑 (可執行檔 *.exe*)。</span><span class="sxs-lookup"><span data-stu-id="8a949-132">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.1</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

#### <a name="self-contained-deployment-scd"></a><span data-ttu-id="8a949-133">自封式部署 (SCD)</span><span class="sxs-lookup"><span data-stu-id="8a949-133">Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="8a949-134">確認有 Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog)，或將 RID 新增至包含目標 Framework 的 `<PropertyGroup>`。</span><span class="sxs-lookup"><span data-stu-id="8a949-134">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add a RID to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="8a949-135">透過新增 `<IsTransformWebConfigDisabled>` 屬性集到 `true`，以停用 *web.config* 檔案的建立。</span><span class="sxs-lookup"><span data-stu-id="8a949-135">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="8a949-136">發行多個 RID：</span><span class="sxs-lookup"><span data-stu-id="8a949-136">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="8a949-137">以分號分隔的清單提供 RID。</span><span class="sxs-lookup"><span data-stu-id="8a949-137">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="8a949-138">使用屬性名稱 `<RuntimeIdentifiers>` (複數)。</span><span class="sxs-lookup"><span data-stu-id="8a949-138">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

  <span data-ttu-id="8a949-139">如需詳細資訊，請參閱 [.NET Core RID 目錄](/dotnet/core/rid-catalog)。</span><span class="sxs-lookup"><span data-stu-id="8a949-139">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="8a949-140">新增 [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="8a949-140">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

<span data-ttu-id="8a949-141">若要啟用 Windows 事件記錄的記錄功能，請加入對 [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="8a949-141">To enable Windows Event Log logging, add a package reference for [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="8a949-142">如需詳細資訊，請參閱[處理開始與停止事件](#handle-starting-and-stopping-events)一節。</span><span class="sxs-lookup"><span data-stu-id="8a949-142">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

### <a name="programmain-updates"></a><span data-ttu-id="8a949-143">Program.Main 更新</span><span class="sxs-lookup"><span data-stu-id="8a949-143">Program.Main updates</span></span>

<span data-ttu-id="8a949-144">在 `Program.Main` 中進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="8a949-144">Make the following changes in `Program.Main`:</span></span>

* <span data-ttu-id="8a949-145">若要在於服務外執行時測試及偵錯，請新增程式碼以判斷應用程式是以服務形式執行或以主控台應用程式形式執行。</span><span class="sxs-lookup"><span data-stu-id="8a949-145">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="8a949-146">檢查偵錯工具是否已附加或 `--console` 命令列引數是否存在。</span><span class="sxs-lookup"><span data-stu-id="8a949-146">Inspect if the debugger is attached or a `--console` command-line argument is present.</span></span>

  <span data-ttu-id="8a949-147">若任一條件為真 (應用程式不是以服務形式執行)，請呼叫 Web 主機上的 <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>。</span><span class="sxs-lookup"><span data-stu-id="8a949-147">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> on the Web Host.</span></span>

  <span data-ttu-id="8a949-148">若條件為偽 (應用程式是以服務形式執行)：</span><span class="sxs-lookup"><span data-stu-id="8a949-148">If the conditions are false (the app is run as a service):</span></span>

  * <span data-ttu-id="8a949-149">呼叫 <xref:System.IO.Directory.SetCurrentDirectory*> 並使用應用程式發佈位置的路徑。</span><span class="sxs-lookup"><span data-stu-id="8a949-149">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="8a949-150">請勿呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 來取得路徑，因為當呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 時，Windows 服務應用程式會傳回 *C:\\WINDOWS\\system32* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="8a949-150">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="8a949-151">如需詳細資訊，請參閱[目前目錄與內容根目錄](#current-directory-and-content-root)一節。</span><span class="sxs-lookup"><span data-stu-id="8a949-151">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
  * <span data-ttu-id="8a949-152">呼叫 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> 以服務形式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="8a949-152">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

  <span data-ttu-id="8a949-153">因為[命令列設定提供者](xref:fundamentals/configuration/index#command-line-configuration-provider) 需要命令列引數的名稱值組，在 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 收到引數之前，`--console` 切換參數就會從引數移除。</span><span class="sxs-lookup"><span data-stu-id="8a949-153">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives them.</span></span>

* <span data-ttu-id="8a949-154">若要寫入 Windows 事件記錄檔，請新增 EventLog 提供者到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>。</span><span class="sxs-lookup"><span data-stu-id="8a949-154">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="8a949-155">使用 *appsettings.Production.json* 檔案中的 `Logging:LogLevel:Default` 機碼設定記錄層級。</span><span class="sxs-lookup"><span data-stu-id="8a949-155">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span> <span data-ttu-id="8a949-156">針對示範與測試用途，範例應用程式的生產設定檔案會將記錄層級設定為 `Information`。</span><span class="sxs-lookup"><span data-stu-id="8a949-156">For demonstration and testing purposes, the sample app's Production settings file sets the logging level to `Information`.</span></span> <span data-ttu-id="8a949-157">在生產環境中，該值一般會設定為 `Error`。</span><span class="sxs-lookup"><span data-stu-id="8a949-157">In production, the value is typically set to `Error`.</span></span> <span data-ttu-id="8a949-158">如需詳細資訊，請參閱<xref:fundamentals/logging/index#windows-eventlog-provider>。</span><span class="sxs-lookup"><span data-stu-id="8a949-158">For more information, see <xref:fundamentals/logging/index#windows-eventlog-provider>.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

## <a name="publish-the-app"></a><span data-ttu-id="8a949-159">發行應用程式</span><span class="sxs-lookup"><span data-stu-id="8a949-159">Publish the app</span></span>

<span data-ttu-id="8a949-160">使用 [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) (這是一個 [Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles)) 或 Visual Studio Code 來發行應用程式。</span><span class="sxs-lookup"><span data-stu-id="8a949-160">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="8a949-161">使用 Visual Studio 時，請選取 [FolderProfile] 並設定 [目標位置]，再選取 [發行] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="8a949-161">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

<span data-ttu-id="8a949-162">若要使用命令列介面 (CLI) 工具來發佈範例應用程式，請從專案資料夾在 Windows 命令殼層中執行 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令，同時搭配將發行設定傳遞至 [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) 選項。</span><span class="sxs-lookup"><span data-stu-id="8a949-162">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command in a Windows command shell from the project folder with a Release configuration passed to the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option.</span></span> <span data-ttu-id="8a949-163">搭配路徑使用 [-o|--output](/dotnet/core/tools/dotnet-publish#options) 選項以發行到應用程式以外的資料夾。</span><span class="sxs-lookup"><span data-stu-id="8a949-163">Use the [-o|--output](/dotnet/core/tools/dotnet-publish#options) option with a path to publish to a folder outside of the app.</span></span>

### <a name="publish-a-framework-dependent-deployment-fdd"></a><span data-ttu-id="8a949-164">發行架構相依部署 (FDD)</span><span class="sxs-lookup"><span data-stu-id="8a949-164">Publish a Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="8a949-165">在下列範例中，應用程式是發行到 *c:\\svc* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="8a949-165">In the following example, the app is published to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --output c:\svc
```

### <a name="publish-a-self-contained-deployment-scd"></a><span data-ttu-id="8a949-166">發行自封式部署 (SCD)</span><span class="sxs-lookup"><span data-stu-id="8a949-166">Publish a Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="8a949-167">必須在 `<RuntimeIdenfifier>` (或 `<RuntimeIdentifiers>`) 中指定 RID 專案檔的屬性。</span><span class="sxs-lookup"><span data-stu-id="8a949-167">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="8a949-168">提供執行階段給 `dotnet publish` 命令的 [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) 選項。</span><span class="sxs-lookup"><span data-stu-id="8a949-168">Supply the runtime to the [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) option of the `dotnet publish` command.</span></span>

<span data-ttu-id="8a949-169">在下列範例中，應用程式是針對 `win7-x64` 執行階段發行到 *c:\\svc* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="8a949-169">In the following example, the app is published for the `win7-x64` runtime to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

## <a name="create-a-user-account"></a><span data-ttu-id="8a949-170">建立使用者帳戶</span><span class="sxs-lookup"><span data-stu-id="8a949-170">Create a user account</span></span>

<span data-ttu-id="8a949-171">從系統管理 PowerShell 6 命令殼層使用 [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) Cmdlet，為服務建立使用者帳戶：</span><span class="sxs-lookup"><span data-stu-id="8a949-171">Create a user account for the service using the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell:</span></span>

```powershell
New-LocalUser -Name {NAME}
```

<span data-ttu-id="8a949-172">在系統提示時提供[強式密碼](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements)。</span><span class="sxs-lookup"><span data-stu-id="8a949-172">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="8a949-173">針對範例應用程式，建立名為 `ServiceUser` 的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="8a949-173">For the sample app, create a user account with the name `ServiceUser`.</span></span>

```powershell
New-LocalUser -Name ServiceUser
```

<span data-ttu-id="8a949-174">除非搭配過期 <xref:System.DateTime> 將 `-AccountExpires` 參數提供給 [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) Cmdlet，否則該帳戶將不會過期。</span><span class="sxs-lookup"><span data-stu-id="8a949-174">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="8a949-175">如需詳細資訊，請參閱 [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) 和[服務使用者帳戶](/windows/desktop/services/service-user-accounts)。</span><span class="sxs-lookup"><span data-stu-id="8a949-175">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="8a949-176">使用 Active Directory 時有一個管理使用者的替代方法，就是使用「受控服務帳戶」。</span><span class="sxs-lookup"><span data-stu-id="8a949-176">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="8a949-177">如需詳細資訊，請參閱[群組受控服務帳戶概觀](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)。</span><span class="sxs-lookup"><span data-stu-id="8a949-177">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="set-permission-log-on-as-a-service"></a><span data-ttu-id="8a949-178">設定權限：以服務方式登入</span><span class="sxs-lookup"><span data-stu-id="8a949-178">Set permission: Log on as a service</span></span>

<span data-ttu-id="8a949-179">在系統管理 PowerShell 6 命令殼層中使用 [icacls](/windows-server/administration/windows-commands/icacls) 命令，授與對應用程式資料夾的寫入/讀取/執行存取權。</span><span class="sxs-lookup"><span data-stu-id="8a949-179">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command an administrative PowerShell 6 command shell.</span></span>

```powershell
icacls "{PATH}" /grant "{USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS}" /t
```

* <span data-ttu-id="8a949-180">`{PATH}` &ndash; 應用程式資料夾的路徑。</span><span class="sxs-lookup"><span data-stu-id="8a949-180">`{PATH}` &ndash; Path to the app's folder.</span></span>
* <span data-ttu-id="8a949-181">`{USER ACCOUNT}` &ndash; 使用者帳戶 (SID)。</span><span class="sxs-lookup"><span data-stu-id="8a949-181">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
* <span data-ttu-id="8a949-182">`(OI)` &ndash;「物件繼承旗標會將權限傳播到次級檔案。</span><span class="sxs-lookup"><span data-stu-id="8a949-182">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
* <span data-ttu-id="8a949-183">`(CI)` &ndash;「物件繼承旗標會將權限傳播到次級檔案。</span><span class="sxs-lookup"><span data-stu-id="8a949-183">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
* <span data-ttu-id="8a949-184">`{PERMISSION FLAGS}` &ndash; 設定應用程式的存取權限。</span><span class="sxs-lookup"><span data-stu-id="8a949-184">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
  * <span data-ttu-id="8a949-185">寫入 (`W`)</span><span class="sxs-lookup"><span data-stu-id="8a949-185">Write (`W`)</span></span>
  * <span data-ttu-id="8a949-186">讀取 (`R`)</span><span class="sxs-lookup"><span data-stu-id="8a949-186">Read (`R`)</span></span>
  * <span data-ttu-id="8a949-187">執行 (`X`)</span><span class="sxs-lookup"><span data-stu-id="8a949-187">Execute (`X`)</span></span>
  * <span data-ttu-id="8a949-188">完整 (`F`)</span><span class="sxs-lookup"><span data-stu-id="8a949-188">Full (`F`)</span></span>
  * <span data-ttu-id="8a949-189">修改 (`M`)</span><span class="sxs-lookup"><span data-stu-id="8a949-189">Modify (`M`)</span></span>
* <span data-ttu-id="8a949-190">`/t` &ndash; 套用遞迴到現有的次級資料夾與檔案。</span><span class="sxs-lookup"><span data-stu-id="8a949-190">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

<span data-ttu-id="8a949-191">針對發行到 *c:\\svc* 資料夾的範例應用程式，以及具有寫入/讀取/執行權限的 `ServiceUser` 帳戶，請在系統管理 PowerShell 6 命令殼層中使用下列命令。</span><span class="sxs-lookup"><span data-stu-id="8a949-191">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command an administrative PowerShell 6 command shell.</span></span>

```powershell
icacls "c:\svc" /grant "ServiceUser:(OI)(CI)WRX" /t
```

<span data-ttu-id="8a949-192">如需詳細資訊，請參閱 [icacls](/windows-server/administration/windows-commands/icacls)。</span><span class="sxs-lookup"><span data-stu-id="8a949-192">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

## <a name="create-the-service"></a><span data-ttu-id="8a949-193">建立服務</span><span class="sxs-lookup"><span data-stu-id="8a949-193">Create the service</span></span>

<span data-ttu-id="8a949-194">使用 [RegisterService.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) 的 PowerShell 指令碼註冊服務。</span><span class="sxs-lookup"><span data-stu-id="8a949-194">Use the [RegisterService.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) PowerShell script to register the service.</span></span> <span data-ttu-id="8a949-195">從系統管理 PowerShell 6 命令殼層，搭配下列命令執行指令碼：</span><span class="sxs-lookup"><span data-stu-id="8a949-195">From an administrative PowerShell 6 command shell, execute the script with the following command:</span></span>

```powershell
.\RegisterService.ps1 
    -Name {NAME} 
    -DisplayName "{DISPLAY NAME}" 
    -Description "{DESCRIPTION}" 
    -Exe "{PATH TO EXE}\{ASSEMBLY NAME}.exe" 
    -User {DOMAIN\USER}
```

<span data-ttu-id="8a949-196">在範例應用程式的下列範例中：</span><span class="sxs-lookup"><span data-stu-id="8a949-196">In the following example for the sample app:</span></span>

* <span data-ttu-id="8a949-197">服務的名稱是 **MyService**。</span><span class="sxs-lookup"><span data-stu-id="8a949-197">The service is named **MyService**.</span></span>
* <span data-ttu-id="8a949-198">已發行的服務位於 *c:\\svc* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="8a949-198">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="8a949-199">應用程式可執行檔名稱是 *SampleApp.exe*。</span><span class="sxs-lookup"><span data-stu-id="8a949-199">The app executable is named *SampleApp.exe*.</span></span>
* <span data-ttu-id="8a949-200">服務是以 `ServiceUser` 帳戶執行。</span><span class="sxs-lookup"><span data-stu-id="8a949-200">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="8a949-201">下列範例命令中，本機電腦名稱為 `Desktop-PC`。</span><span class="sxs-lookup"><span data-stu-id="8a949-201">In the following example command, the local machine name is `Desktop-PC`.</span></span> <span data-ttu-id="8a949-202">將 `Desktop-PC` 取代為您系統的電腦名稱或網域。</span><span class="sxs-lookup"><span data-stu-id="8a949-202">Replace `Desktop-PC` with the computer name or domain for your system.</span></span>

```powershell
.\RegisterService.ps1 
    -Name MyService 
    -DisplayName "My Cool Service" 
    -Description "This is the Sample App service." 
    -Exe "c:\svc\SampleApp.exe" 
    -User Desktop-PC\ServiceUser
```

## <a name="manage-the-service"></a><span data-ttu-id="8a949-203">管理服務</span><span class="sxs-lookup"><span data-stu-id="8a949-203">Manage the service</span></span>

### <a name="start-the-service"></a><span data-ttu-id="8a949-204">啟動服務</span><span class="sxs-lookup"><span data-stu-id="8a949-204">Start the service</span></span>

<span data-ttu-id="8a949-205">以 `Start-Service -Name {NAME}` 的 PowerShell 6 命令啟動服務。</span><span class="sxs-lookup"><span data-stu-id="8a949-205">Start the service with the `Start-Service -Name {NAME}` PowerShell 6 command.</span></span>

<span data-ttu-id="8a949-206">請使用下列命令啟動範例應用程式服務：</span><span class="sxs-lookup"><span data-stu-id="8a949-206">To start the sample app service, use the following command:</span></span>

```powershell
Start-Service -Name MyService
```

<span data-ttu-id="8a949-207">此命令需要幾秒鐘啓動服務。</span><span class="sxs-lookup"><span data-stu-id="8a949-207">The command takes a few seconds to start the service.</span></span>

### <a name="determine-the-service-status"></a><span data-ttu-id="8a949-208">判斷服務狀態</span><span class="sxs-lookup"><span data-stu-id="8a949-208">Determine the service status</span></span>

<span data-ttu-id="8a949-209">若要檢查服務狀態，請使用 `Get-Service -Name {NAME}` 的 PowerShell 6 命令。</span><span class="sxs-lookup"><span data-stu-id="8a949-209">To check the status of the service, use the `Get-Service -Name {NAME}` PowerShell 6 command.</span></span> <span data-ttu-id="8a949-210">狀態會回報為下列值之一：</span><span class="sxs-lookup"><span data-stu-id="8a949-210">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

<span data-ttu-id="8a949-211">使用下列命令來檢查範例應用程式服務的狀態：</span><span class="sxs-lookup"><span data-stu-id="8a949-211">Use the following command to check the status of the sample app service:</span></span>

```powershell
Get-Service -Name MyService
```

### <a name="browse-a-web-app-service"></a><span data-ttu-id="8a949-212">瀏覽 Web 應用程式服務</span><span class="sxs-lookup"><span data-stu-id="8a949-212">Browse a web app service</span></span>

<span data-ttu-id="8a949-213">當服務處於 `RUNNING` 狀態，且若服務是 Web 應用程式時，請在其路徑瀏覽應用程式 (預設為 `http://localhost:5000`，使用 [HTTPS 重新導向中介軟體](xref:security/enforcing-ssl)時會重新導向至 `https://localhost:5001`)。</span><span class="sxs-lookup"><span data-stu-id="8a949-213">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

<span data-ttu-id="8a949-214">範例應用程式服務請瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="8a949-214">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

### <a name="stop-the-service"></a><span data-ttu-id="8a949-215">停止服務</span><span class="sxs-lookup"><span data-stu-id="8a949-215">Stop the service</span></span>

<span data-ttu-id="8a949-216">以 `Stop-Service -Name {NAME}` 的 PowerShell 6 命令停止服務。</span><span class="sxs-lookup"><span data-stu-id="8a949-216">Stop the service with the `Stop-Service -Name {NAME}` Powershell 6 command.</span></span>

<span data-ttu-id="8a949-217">下列命令會停止範例應用程式服務：</span><span class="sxs-lookup"><span data-stu-id="8a949-217">The following command stops the sample app service:</span></span>

```powershell
Stop-Service -Name MyService
```

### <a name="remove-the-service"></a><span data-ttu-id="8a949-218">移除服務</span><span class="sxs-lookup"><span data-stu-id="8a949-218">Remove the service</span></span>

<span data-ttu-id="8a949-219">在停止服務的短暫延遲之後，請以 `Remove-Service -Name {NAME}` 的 Powershell 6 命令移除服務。</span><span class="sxs-lookup"><span data-stu-id="8a949-219">After a short delay to stop a service, remove the service with the `Remove-Service -Name {NAME}` Powershell 6 command.</span></span>

<span data-ttu-id="8a949-220">下列命令會移除範例應用程式服務：</span><span class="sxs-lookup"><span data-stu-id="8a949-220">The following command removes the sample app service:</span></span>

```powershell
Remove-Service -Name MyService
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="8a949-221">處理開始與停止事件</span><span class="sxs-lookup"><span data-stu-id="8a949-221">Handle starting and stopping events</span></span>

<span data-ttu-id="8a949-222">若要處理 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> 與 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> 事件，請進行下列額外變更：</span><span class="sxs-lookup"><span data-stu-id="8a949-222">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events, perform the following additional changes:</span></span>

1. <span data-ttu-id="8a949-223">使用 `OnStarting`、`OnStarted` 與 `OnStopping` 方法建立衍生自 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> 的類別：</span><span class="sxs-lookup"><span data-stu-id="8a949-223">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="8a949-224">為 <xref:Microsoft.AspNetCore.Hosting.IWebHost> 建立一個會將 `CustomWebHostService` 傳遞給 <xref:System.ServiceProcess.ServiceBase.Run*> 的擴充方法：</span><span class="sxs-lookup"><span data-stu-id="8a949-224">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="8a949-225">在 `Program.Main` 中，呼叫 `RunAsCustomService` 擴充方法，而不是呼叫 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>：</span><span class="sxs-lookup"><span data-stu-id="8a949-225">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="8a949-226">若要查看 `Program.Main` 中的 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> 位置，請參閱[將專案轉換為 Windows 服務](#convert-a-project-into-a-windows-service)一節中的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="8a949-226">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Convert a project into a Windows Service](#convert-a-project-into-a-windows-service) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="8a949-227">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="8a949-227">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="8a949-228">服務如果會與來自網際網路或公司網路的要求進行互動，並且位於 Proxy 或負載平衡器後方，可能會需要額外的設定。</span><span class="sxs-lookup"><span data-stu-id="8a949-228">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="8a949-229">如需詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="8a949-229">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="8a949-230">設定 HTTPS</span><span class="sxs-lookup"><span data-stu-id="8a949-230">Configure HTTPS</span></span>

<span data-ttu-id="8a949-231">使用安全端點來設定服務：</span><span class="sxs-lookup"><span data-stu-id="8a949-231">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="8a949-232">使用您平台的憑證取得與部署機制來建立將用於裝載系統的 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="8a949-232">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="8a949-233">指定 [Kestrel 伺服器 HTTPS 端點設定](xref:fundamentals/servers/kestrel#endpoint-configuration)以使用憑證。</span><span class="sxs-lookup"><span data-stu-id="8a949-233">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="8a949-234">不支援使用 ASP.NET Core HTTPS 開發憑證來保護服務端點。</span><span class="sxs-lookup"><span data-stu-id="8a949-234">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="8a949-235">目前目錄和內容根目錄</span><span class="sxs-lookup"><span data-stu-id="8a949-235">Current directory and content root</span></span>

<span data-ttu-id="8a949-236">針對 Windows 服務呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 所傳回的目前工作目錄為 *C:\\WINDOWS\\system32* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="8a949-236">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="8a949-237">*System32* 資料夾不是儲存服務檔案 (例如，設定檔) 的合適位置。</span><span class="sxs-lookup"><span data-stu-id="8a949-237">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="8a949-238">使用下列其中一個方式來維護及存取服務的資產與設定檔。</span><span class="sxs-lookup"><span data-stu-id="8a949-238">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="8a949-239">將內容根目錄路徑設定到應用程式的資料夾</span><span class="sxs-lookup"><span data-stu-id="8a949-239">Set the content root path to the app's folder</span></span>

<span data-ttu-id="8a949-240"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> 與建立服務時提供給 `binPath` 引數的路徑相同。</span><span class="sxs-lookup"><span data-stu-id="8a949-240">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="8a949-241">請搭配應用程式內容根目錄的路徑呼叫 <xref:System.IO.Directory.SetCurrentDirectory*>，而不要呼叫 `GetCurrentDirectory` 來建立設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="8a949-241">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="8a949-242">在 `Program.Main` 中，判斷服務可執行檔資料夾的路徑，然後使用該路徑來建立應用程式的內容根目錄：</span><span class="sxs-lookup"><span data-stu-id="8a949-242">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="8a949-243">將服務的檔案儲存在磁碟上的適當位置</span><span class="sxs-lookup"><span data-stu-id="8a949-243">Store the service's files in a suitable location on disk</span></span>

<span data-ttu-id="8a949-244">使用包含檔案的 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> 資料夾，使用 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 來指定絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="8a949-244">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8a949-245">其他資源</span><span class="sxs-lookup"><span data-stu-id="8a949-245">Additional resources</span></span>

* <span data-ttu-id="8a949-246">[Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration) (包括 HTTPS 組態與 SNI 支援)</span><span class="sxs-lookup"><span data-stu-id="8a949-246">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
