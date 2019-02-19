---
title: 在 Windows 服務上裝載 ASP.NET Core
author: guardrex
description: 了解如何在 Windows 服務上裝載 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/13/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 081a631c9c3e74c01e15f4b0b272d650c162bd20
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248247"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="5e26d-103">在 Windows 服務上裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e26d-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="5e26d-104">作者：[Luke Latham](https://github.com/guardrex) 和 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="5e26d-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="5e26d-105">ASP.NET Core 應用程式可以裝載在 Windows 上作為 [Windows 服務](/dotnet/framework/windows-services/introduction-to-windows-service-applications)，不需要使用 IIS。</span><span class="sxs-lookup"><span data-stu-id="5e26d-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="5e26d-106">當裝載為 Windows 服務時，應用程式將會在重新開機後自動啟動。</span><span class="sxs-lookup"><span data-stu-id="5e26d-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="5e26d-107">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5e26d-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="deployment-type"></a><span data-ttu-id="5e26d-108">部署類型</span><span class="sxs-lookup"><span data-stu-id="5e26d-108">Deployment type</span></span>

<span data-ttu-id="5e26d-109">您可以建立架構相依或自封式 Windows 服務部署。</span><span class="sxs-lookup"><span data-stu-id="5e26d-109">You can create either a framework-dependent or self-contained Windows Service deployment.</span></span> <span data-ttu-id="5e26d-110">如需詳細資訊與部署案例建議，請參閱 [.NET Core 應用程式部署](/dotnet/core/deploying/)。</span><span class="sxs-lookup"><span data-stu-id="5e26d-110">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment"></a><span data-ttu-id="5e26d-111">與 Framework 相依的部署</span><span class="sxs-lookup"><span data-stu-id="5e26d-111">Framework-dependent deployment</span></span>

<span data-ttu-id="5e26d-112">架構相依部署 (FDD) 仰賴存在於目標系統上的全系統共用 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="5e26d-112">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="5e26d-113">搭配 ASP.NET Core Windows 服務應用程式使用 FDD 案例時，SDK 會產生可執行檔 (*\*.exe*)，稱為架構相依可執行檔。</span><span class="sxs-lookup"><span data-stu-id="5e26d-113">When the FDD scenario is used with an ASP.NET Core Windows Service app, the SDK produces an executable (*\*.exe*), called a *framework-dependent executable*.</span></span>

### <a name="self-contained-deployment"></a><span data-ttu-id="5e26d-114">自封式部署</span><span class="sxs-lookup"><span data-stu-id="5e26d-114">Self-contained deployment</span></span>

<span data-ttu-id="5e26d-115">自封式部署 (SCD) 不仰賴任何存在於目標系統上的共用元件。</span><span class="sxs-lookup"><span data-stu-id="5e26d-115">Self-contained deployment (SCD) doesn't rely on the presence of shared components on the target system.</span></span> <span data-ttu-id="5e26d-116">執行階段與應用程式的相依性會隨著應用程式部署到裝載系統。</span><span class="sxs-lookup"><span data-stu-id="5e26d-116">The runtime and the app's dependencies are deployed with the app to the hosting system.</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="5e26d-117">將專案轉換成 Windows 服務</span><span class="sxs-lookup"><span data-stu-id="5e26d-117">Convert a project into a Windows Service</span></span>

<span data-ttu-id="5e26d-118">對現有的 ASP.NET Core 專進行下列變更，以服務形式執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="5e26d-118">Make the following changes to an existing ASP.NET Core project to run the app as a service:</span></span>

### <a name="project-file-updates"></a><span data-ttu-id="5e26d-119">專案檔更新</span><span class="sxs-lookup"><span data-stu-id="5e26d-119">Project file updates</span></span>

<span data-ttu-id="5e26d-120">視您選擇的[部署類型](#deployment-type)而定，更新專案檔：</span><span class="sxs-lookup"><span data-stu-id="5e26d-120">Based on your choice of [deployment type](#deployment-type), update the project file:</span></span>

#### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="5e26d-121">架構相依部署 (FDD)</span><span class="sxs-lookup"><span data-stu-id="5e26d-121">Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="5e26d-122">將 Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog) 新增至包含目標 Framework 的 `<PropertyGroup>`。</span><span class="sxs-lookup"><span data-stu-id="5e26d-122">Add a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="5e26d-123">在下列範例中，RID 已設定為 `win7-x64`。</span><span class="sxs-lookup"><span data-stu-id="5e26d-123">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="5e26d-124">新增 `<SelfContained>` 屬性集到 `false`。</span><span class="sxs-lookup"><span data-stu-id="5e26d-124">Add the `<SelfContained>` property set to `false`.</span></span> <span data-ttu-id="5e26d-125">這些屬性會指示 SDK 產生適用於 Windows 的可執行檔 (*.exe*)。</span><span class="sxs-lookup"><span data-stu-id="5e26d-125">These properties instruct the SDK to generate an executable (*.exe*) file for Windows.</span></span>

<span data-ttu-id="5e26d-126">針對 Windows Services 應用程式，不需要 *web.config* 檔案 (發行 ASP.NET Core 應用程式時通常會產生此檔案)。</span><span class="sxs-lookup"><span data-stu-id="5e26d-126">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="5e26d-127">若要停用 *web.config* 檔案的建立，請新增 `<IsTransformWebConfigDisabled>` 屬性集到 `true`。</span><span class="sxs-lookup"><span data-stu-id="5e26d-127">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="5e26d-128">新增 `<UseAppHost>` 屬性集到 `true`。</span><span class="sxs-lookup"><span data-stu-id="5e26d-128">Add the `<UseAppHost>` property set to `true`.</span></span> <span data-ttu-id="5e26d-129">此屬性為服務提供 FDD 的啟用路徑 (可執行檔 *.exe*)。</span><span class="sxs-lookup"><span data-stu-id="5e26d-129">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

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

#### <a name="self-contained-deployment-scd"></a><span data-ttu-id="5e26d-130">自封式部署 (SCD)</span><span class="sxs-lookup"><span data-stu-id="5e26d-130">Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="5e26d-131">確認有 Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog)，或將 RID 新增至包含目標 Framework 的 `<PropertyGroup>`。</span><span class="sxs-lookup"><span data-stu-id="5e26d-131">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add a RID to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="5e26d-132">透過新增 `<IsTransformWebConfigDisabled>` 屬性集到 `true`，以停用 *web.config* 檔案的建立。</span><span class="sxs-lookup"><span data-stu-id="5e26d-132">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="5e26d-133">發行多個 RID：</span><span class="sxs-lookup"><span data-stu-id="5e26d-133">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="5e26d-134">以分號分隔的清單提供 RID。</span><span class="sxs-lookup"><span data-stu-id="5e26d-134">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="5e26d-135">使用屬性名稱 `<RuntimeIdentifiers>` (複數)。</span><span class="sxs-lookup"><span data-stu-id="5e26d-135">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

  <span data-ttu-id="5e26d-136">如需詳細資訊，請參閱 [.NET Core RID 目錄](/dotnet/core/rid-catalog)。</span><span class="sxs-lookup"><span data-stu-id="5e26d-136">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="5e26d-137">新增 [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="5e26d-137">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

<span data-ttu-id="5e26d-138">若要啟用 Windows 事件記錄的記錄功能，請加入對 [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="5e26d-138">To enable Windows Event Log logging, add a package reference for [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="5e26d-139">如需詳細資訊，請參閱[處理開始與停止事件](#handle-starting-and-stopping-events)一節。</span><span class="sxs-lookup"><span data-stu-id="5e26d-139">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

### <a name="programmain-updates"></a><span data-ttu-id="5e26d-140">Program.Main 更新</span><span class="sxs-lookup"><span data-stu-id="5e26d-140">Program.Main updates</span></span>

<span data-ttu-id="5e26d-141">在 `Program.Main` 中進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="5e26d-141">Make the following changes in `Program.Main`:</span></span>

* <span data-ttu-id="5e26d-142">若要在於服務外執行時測試及偵錯，請新增程式碼以判斷應用程式是以服務形式執行或以主控台應用程式形式執行。</span><span class="sxs-lookup"><span data-stu-id="5e26d-142">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="5e26d-143">檢查偵錯工具是否已附加或 `--console` 命令列引數是否存在。</span><span class="sxs-lookup"><span data-stu-id="5e26d-143">Inspect if the debugger is attached or a `--console` command-line argument is present.</span></span>

  <span data-ttu-id="5e26d-144">若任一條件為真 (應用程式不是以服務形式執行)，請呼叫 Web 主機上的 <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>。</span><span class="sxs-lookup"><span data-stu-id="5e26d-144">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> on the Web Host.</span></span>

  <span data-ttu-id="5e26d-145">若條件為偽 (應用程式是以服務形式執行)：</span><span class="sxs-lookup"><span data-stu-id="5e26d-145">If the conditions are false (the app is run as a service):</span></span>

  * <span data-ttu-id="5e26d-146">呼叫 <xref:System.IO.Directory.SetCurrentDirectory*> 並使用應用程式發佈位置的路徑。</span><span class="sxs-lookup"><span data-stu-id="5e26d-146">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="5e26d-147">請勿呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 來取得路徑，因為當呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 時，Windows 服務應用程式會傳回 *C:\\WINDOWS\\system32* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="5e26d-147">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="5e26d-148">如需詳細資訊，請參閱[目前目錄與內容根目錄](#current-directory-and-content-root)一節。</span><span class="sxs-lookup"><span data-stu-id="5e26d-148">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
  * <span data-ttu-id="5e26d-149">呼叫 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> 以服務形式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="5e26d-149">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

  <span data-ttu-id="5e26d-150">因為[命令列設定提供者](xref:fundamentals/configuration/index#command-line-configuration-provider) 需要命令列引數的名稱值組，在 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 收到引數之前，`--console` 切換參數就會從引數移除。</span><span class="sxs-lookup"><span data-stu-id="5e26d-150">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives them.</span></span>

* <span data-ttu-id="5e26d-151">若要寫入 Windows 事件記錄檔，請新增 EventLog 提供者到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>。</span><span class="sxs-lookup"><span data-stu-id="5e26d-151">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="5e26d-152">使用 *appsettings.Production.json* 檔案中的 `Logging:LogLevel:Default` 機碼設定記錄層級。</span><span class="sxs-lookup"><span data-stu-id="5e26d-152">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span> <span data-ttu-id="5e26d-153">針對示範與測試用途，範例應用程式的生產設定檔案會將記錄層級設定為 `Information`。</span><span class="sxs-lookup"><span data-stu-id="5e26d-153">For demonstration and testing purposes, the sample app's Production settings file sets the logging level to `Information`.</span></span> <span data-ttu-id="5e26d-154">在生產環境中，該值一般會設定為 `Error`。</span><span class="sxs-lookup"><span data-stu-id="5e26d-154">In production, the value is typically set to `Error`.</span></span> <span data-ttu-id="5e26d-155">如需詳細資訊，請參閱<xref:fundamentals/logging/index#windows-eventlog-provider>。</span><span class="sxs-lookup"><span data-stu-id="5e26d-155">For more information, see <xref:fundamentals/logging/index#windows-eventlog-provider>.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

### <a name="publish-the-app"></a><span data-ttu-id="5e26d-156">發行應用程式</span><span class="sxs-lookup"><span data-stu-id="5e26d-156">Publish the app</span></span>

<span data-ttu-id="5e26d-157">使用 [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) (這是一個 [Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles)) 或 Visual Studio Code 來發行應用程式。</span><span class="sxs-lookup"><span data-stu-id="5e26d-157">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="5e26d-158">使用 Visual Studio 時，請選取 [FolderProfile] 並設定 [目標位置]，再選取 [發行] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="5e26d-158">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

<span data-ttu-id="5e26d-159">若要使用命令列介面 (CLI) 工具發佈應用程式，請在將發行設定傳遞到 [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) 選項的情況下從專案資料夾的命令提示字元中執行 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令。</span><span class="sxs-lookup"><span data-stu-id="5e26d-159">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder with a Release configuration passed to the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option.</span></span> <span data-ttu-id="5e26d-160">搭配路徑使用 [-o|--output](/dotnet/core/tools/dotnet-publish#options) 選項以發行到應用程式以外的資料夾。</span><span class="sxs-lookup"><span data-stu-id="5e26d-160">Use the [-o|--output](/dotnet/core/tools/dotnet-publish#options) option with a path to publish to a folder outside of the app.</span></span>

#### <a name="publish-a-framework-dependent-deployment-fdd"></a><span data-ttu-id="5e26d-161">發行架構相依部署 (FDD)</span><span class="sxs-lookup"><span data-stu-id="5e26d-161">Publish a Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="5e26d-162">在下列範例中，應用程式是發行到 *c:\\svc* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="5e26d-162">In the following example, the app is published to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --output c:\svc
```

#### <a name="publish-a-self-contained-deployment-scd"></a><span data-ttu-id="5e26d-163">發行自封式部署 (SCD)</span><span class="sxs-lookup"><span data-stu-id="5e26d-163">Publish a Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="5e26d-164">必須在 `<RuntimeIdenfifier>` (或 `<RuntimeIdentifiers>`) 中指定 RID 專案檔的屬性。</span><span class="sxs-lookup"><span data-stu-id="5e26d-164">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="5e26d-165">提供執行階段給 `dotnet publish` 命令的 [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) 選項。</span><span class="sxs-lookup"><span data-stu-id="5e26d-165">Supply the runtime to the [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) option of the `dotnet publish` command.</span></span>

<span data-ttu-id="5e26d-166">在下列範例中，應用程式是針對 `win7-x64` 執行階段發行到 *c:\\svc* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="5e26d-166">In the following example, the app is published for the `win7-x64` runtime to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

### <a name="create-a-user-account"></a><span data-ttu-id="5e26d-167">建立使用者帳戶</span><span class="sxs-lookup"><span data-stu-id="5e26d-167">Create a user account</span></span>

<span data-ttu-id="5e26d-168">從系統管理命令殼層使用 `net user` 命令為服務建立使用者帳戶：</span><span class="sxs-lookup"><span data-stu-id="5e26d-168">Create a user account for the service using the `net user` command from an administrative command shell:</span></span>

```console
net user {USER ACCOUNT} {PASSWORD} /add
```

<span data-ttu-id="5e26d-169">預設的密碼到期期限是六週。</span><span class="sxs-lookup"><span data-stu-id="5e26d-169">The default password expiration is six weeks.</span></span>

<span data-ttu-id="5e26d-170">針對範例應用程式，建立名為 `ServiceUser` 的使用者帳戶與密碼。</span><span class="sxs-lookup"><span data-stu-id="5e26d-170">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="5e26d-171">在下列命令中，將 `{PASSWORD}` 取代為[強式密碼](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements)。</span><span class="sxs-lookup"><span data-stu-id="5e26d-171">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

```console
net user ServiceUser {PASSWORD} /add
```

<span data-ttu-id="5e26d-172">若需要將使用者新增到某個群組，請使用 `net localgroup` 命令，其中 `{GROUP}` 是群組的名稱：</span><span class="sxs-lookup"><span data-stu-id="5e26d-172">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

```console
net localgroup {GROUP} {USER ACCOUNT} /add
```

<span data-ttu-id="5e26d-173">如需詳細資訊，請參閱[服務使用者帳戶](/windows/desktop/services/service-user-accounts)。</span><span class="sxs-lookup"><span data-stu-id="5e26d-173">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="5e26d-174">使用 Active Directory 時有一個管理使用者的替代方法，就是使用「受控服務帳戶」。</span><span class="sxs-lookup"><span data-stu-id="5e26d-174">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="5e26d-175">如需詳細資訊，請參閱[群組受控服務帳戶概觀](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)。</span><span class="sxs-lookup"><span data-stu-id="5e26d-175">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

### <a name="set-permissions"></a><span data-ttu-id="5e26d-176">設定權限</span><span class="sxs-lookup"><span data-stu-id="5e26d-176">Set permissions</span></span>

#### <a name="access-to-the-app-folder"></a><span data-ttu-id="5e26d-177">存取應用程式資料夾</span><span class="sxs-lookup"><span data-stu-id="5e26d-177">Access to the app folder</span></span>

<span data-ttu-id="5e26d-178">從系統管理命令殼層使用 [icacls](/windows-server/administration/windows-commands/icacls) 命令，授與對應用程式資料夾的寫入/讀取/執行存取權：</span><span class="sxs-lookup"><span data-stu-id="5e26d-178">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command from an administrative command shell:</span></span>

```console
icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
```

* <span data-ttu-id="5e26d-179">`{PATH}` &ndash; 應用程式資料夾的路徑。</span><span class="sxs-lookup"><span data-stu-id="5e26d-179">`{PATH}` &ndash; Path to the app's folder.</span></span>
* <span data-ttu-id="5e26d-180">`{USER ACCOUNT}` &ndash; 使用者帳戶 (SID)。</span><span class="sxs-lookup"><span data-stu-id="5e26d-180">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
* <span data-ttu-id="5e26d-181">`(OI)` &ndash;「物件繼承旗標會將權限傳播到次級檔案。</span><span class="sxs-lookup"><span data-stu-id="5e26d-181">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
* <span data-ttu-id="5e26d-182">`(CI)` &ndash;「物件繼承旗標會將權限傳播到次級檔案。</span><span class="sxs-lookup"><span data-stu-id="5e26d-182">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
* <span data-ttu-id="5e26d-183">`{PERMISSION FLAGS}` &ndash; 設定應用程式的存取權限。</span><span class="sxs-lookup"><span data-stu-id="5e26d-183">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
  * <span data-ttu-id="5e26d-184">寫入 (`W`)</span><span class="sxs-lookup"><span data-stu-id="5e26d-184">Write (`W`)</span></span>
  * <span data-ttu-id="5e26d-185">讀取 (`R`)</span><span class="sxs-lookup"><span data-stu-id="5e26d-185">Read (`R`)</span></span>
  * <span data-ttu-id="5e26d-186">執行 (`X`)</span><span class="sxs-lookup"><span data-stu-id="5e26d-186">Execute (`X`)</span></span>
  * <span data-ttu-id="5e26d-187">完整 (`F`)</span><span class="sxs-lookup"><span data-stu-id="5e26d-187">Full (`F`)</span></span>
  * <span data-ttu-id="5e26d-188">修改 (`M`)</span><span class="sxs-lookup"><span data-stu-id="5e26d-188">Modify (`M`)</span></span>
* <span data-ttu-id="5e26d-189">`/t` &ndash; 套用遞迴到現有的次級資料夾與檔案。</span><span class="sxs-lookup"><span data-stu-id="5e26d-189">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

<span data-ttu-id="5e26d-190">針對發行到 *c:\\svc* 資料夾的範例應用程式與具有寫入/讀取/執行權限的 `ServiceUser` 帳戶，請使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="5e26d-190">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

```console
icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
```

<span data-ttu-id="5e26d-191">如需詳細資訊，請參閱 [icacls](/windows-server/administration/windows-commands/icacls)。</span><span class="sxs-lookup"><span data-stu-id="5e26d-191">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

#### <a name="log-on-as-a-service"></a><span data-ttu-id="5e26d-192">以服務方式登入</span><span class="sxs-lookup"><span data-stu-id="5e26d-192">Log on as a service</span></span>

<span data-ttu-id="5e26d-193">將[以服務方式登入](/windows/security/threat-protection/security-policy-settings/log-on-as-a-service)權限授與使用者帳戶：</span><span class="sxs-lookup"><span data-stu-id="5e26d-193">To grant the [Log on as a service](/windows/security/threat-protection/security-policy-settings/log-on-as-a-service) privilege to the user account:</span></span>

1. <span data-ttu-id="5e26d-194">在 [本機安全性原則] 主控台或 [本機群組原則編輯器] 主控台中，找出 [使用者權限指派] 原則。</span><span class="sxs-lookup"><span data-stu-id="5e26d-194">Locate the **User Rights Assignment** policies in either the Local Security Policy console or Local Group Policy Editor console.</span></span> <span data-ttu-id="5e26d-195">如需指示，請參閱：[設定安全性原則設定](/windows/security/threat-protection/security-policy-settings/how-to-configure-security-policy-settings)。</span><span class="sxs-lookup"><span data-stu-id="5e26d-195">For instructions, see: [Configure security policy settings](/windows/security/threat-protection/security-policy-settings/how-to-configure-security-policy-settings).</span></span>
1. <span data-ttu-id="5e26d-196">找出 `Log on as a service` 原則。</span><span class="sxs-lookup"><span data-stu-id="5e26d-196">Locate the `Log on as a service` policy.</span></span> <span data-ttu-id="5e26d-197">按兩下該原則以開啟它。</span><span class="sxs-lookup"><span data-stu-id="5e26d-197">Double-click the policy to open it.</span></span>
1. <span data-ttu-id="5e26d-198">選取 [新增使用者或群組]。</span><span class="sxs-lookup"><span data-stu-id="5e26d-198">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="5e26d-199">選取 [進階]，然後選取 [立即尋找]。</span><span class="sxs-lookup"><span data-stu-id="5e26d-199">Select **Advanced** and select **Find Now**.</span></span>
1. <span data-ttu-id="5e26d-200">選取稍早在[建立使用者帳戶](#create-a-user-account)一節中建立的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="5e26d-200">Select the user account created in the [Create a user account](#create-a-user-account) section earlier.</span></span> <span data-ttu-id="5e26d-201">選取 [確定] 以接受該選取項目。</span><span class="sxs-lookup"><span data-stu-id="5e26d-201">Select **OK** to accept the selection.</span></span>
1. <span data-ttu-id="5e26d-202">確定物件名稱正確之後，選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="5e26d-202">Select **OK** after confirming that the object name is correct.</span></span>
1. <span data-ttu-id="5e26d-203">選取 [套用]。</span><span class="sxs-lookup"><span data-stu-id="5e26d-203">Select **Apply**.</span></span> <span data-ttu-id="5e26d-204">選取 [確定] 以關閉原則視窗。</span><span class="sxs-lookup"><span data-stu-id="5e26d-204">Select **OK** to close the policy window.</span></span>

## <a name="manage-the-service"></a><span data-ttu-id="5e26d-205">管理服務</span><span class="sxs-lookup"><span data-stu-id="5e26d-205">Manage the service</span></span>

### <a name="create-the-service"></a><span data-ttu-id="5e26d-206">建立服務</span><span class="sxs-lookup"><span data-stu-id="5e26d-206">Create the service</span></span>

<span data-ttu-id="5e26d-207">從系統管理命令殼層使用 [sc.exe](https://technet.microsoft.com/library/bb490995) 命令列工具來建立服務。</span><span class="sxs-lookup"><span data-stu-id="5e26d-207">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service from an administrative command shell.</span></span> <span data-ttu-id="5e26d-208">`binPath` 值是應用程式可執行檔的路徑，其中包括可執行檔的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="5e26d-208">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="5e26d-209">**等號與每個參數與值的引號字元之間必須有空格。**</span><span class="sxs-lookup"><span data-stu-id="5e26d-209">**The space between the equal sign and the quote character of each parameter and value is required.**</span></span>

```console
sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
```

* <span data-ttu-id="5e26d-210">`{SERVICE NAME}` &ndash; 要指派給[服務控制管理員](/windows/desktop/services/service-control-manager)中之服務的名稱。</span><span class="sxs-lookup"><span data-stu-id="5e26d-210">`{SERVICE NAME}` &ndash; The name to assign to the service in [Service Control Manager](/windows/desktop/services/service-control-manager).</span></span>
* <span data-ttu-id="5e26d-211">`{PATH}` &ndash; 服務可執行檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="5e26d-211">`{PATH}` &ndash; The path to the service executable.</span></span>
* <span data-ttu-id="5e26d-212">`{DOMAIN}` &ndash; 已加入網域之機器的網域。</span><span class="sxs-lookup"><span data-stu-id="5e26d-212">`{DOMAIN}` &ndash; The domain of a domain-joined machine.</span></span> <span data-ttu-id="5e26d-213">若機器未加入網域，請使用本機電腦名稱。</span><span class="sxs-lookup"><span data-stu-id="5e26d-213">If the machine isn't domain-joined, use the local machine name.</span></span>
* <span data-ttu-id="5e26d-214">`{USER ACCOUNT}` &ndash; 用於執行服務的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="5e26d-214">`{USER ACCOUNT}` &ndash; The user account under which the service runs.</span></span>
* <span data-ttu-id="5e26d-215">`{PASSWORD}` &ndash; 使用者帳戶密碼。</span><span class="sxs-lookup"><span data-stu-id="5e26d-215">`{PASSWORD}` &ndash; The user account password.</span></span>

> [!WARNING]
> <span data-ttu-id="5e26d-216">請**勿**省略 `obj` 參數。</span><span class="sxs-lookup"><span data-stu-id="5e26d-216">Do **not** omit the `obj` parameter.</span></span> <span data-ttu-id="5e26d-217">`obj` 的預設值是 [LocalSystem 帳戶](/windows/desktop/services/localsystem-account)帳戶。</span><span class="sxs-lookup"><span data-stu-id="5e26d-217">The default value for `obj` is the [LocalSystem account](/windows/desktop/services/localsystem-account) account.</span></span> <span data-ttu-id="5e26d-218">以 `LocalSystem` 帳戶執行服務會帶來重大安全性風險。</span><span class="sxs-lookup"><span data-stu-id="5e26d-218">Running a service under the `LocalSystem` account presents a significant security risk.</span></span> <span data-ttu-id="5e26d-219">一律使用具有受限權限的使用者帳戶來執行服務。</span><span class="sxs-lookup"><span data-stu-id="5e26d-219">Always run a service with a user account that has restricted privileges.</span></span>

<span data-ttu-id="5e26d-220">在範例應用程式的下列範例中：</span><span class="sxs-lookup"><span data-stu-id="5e26d-220">In the following example for the sample app:</span></span>

* <span data-ttu-id="5e26d-221">服務的名稱是 **MyService**。</span><span class="sxs-lookup"><span data-stu-id="5e26d-221">The service is named **MyService**.</span></span>
* <span data-ttu-id="5e26d-222">已發行的服務位於 *c:\\svc* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="5e26d-222">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="5e26d-223">應用程式可執行檔名稱是 *SampleApp.exe*。</span><span class="sxs-lookup"><span data-stu-id="5e26d-223">The app executable is named *SampleApp.exe*.</span></span> <span data-ttu-id="5e26d-224">以雙引號 (") 括住 `binPath` 值。</span><span class="sxs-lookup"><span data-stu-id="5e26d-224">Enclose the `binPath` value in double quotation marks (").</span></span>
* <span data-ttu-id="5e26d-225">服務是以 `ServiceUser` 帳戶執行。</span><span class="sxs-lookup"><span data-stu-id="5e26d-225">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="5e26d-226">將 `{DOMAIN}` 取代為使用者帳戶的網域或本機電腦名稱。</span><span class="sxs-lookup"><span data-stu-id="5e26d-226">Replace `{DOMAIN}` with the user account's domain or local machine name.</span></span> <span data-ttu-id="5e26d-227">以雙引號 (") 括住 `obj` 值。</span><span class="sxs-lookup"><span data-stu-id="5e26d-227">Enclose the `obj` value in double quotation marks (").</span></span> <span data-ttu-id="5e26d-228">範例：若裝載系統是名為 `MairaPC` 的本機電腦，請將 `obj` 設定為 `"MairaPC\ServiceUser"`。</span><span class="sxs-lookup"><span data-stu-id="5e26d-228">Example: If the hosting system is a local machine named `MairaPC`, set `obj` to `"MairaPC\ServiceUser"`.</span></span>
* <span data-ttu-id="5e26d-229">將 `{PASSWORD}` 取代為使用者帳戶的密碼。</span><span class="sxs-lookup"><span data-stu-id="5e26d-229">Replace `{PASSWORD}` with the user account's password.</span></span> <span data-ttu-id="5e26d-230">以雙引號 (") 括住 `password` 值。</span><span class="sxs-lookup"><span data-stu-id="5e26d-230">Enclose the `password` value in double quotation marks (").</span></span>

```console
sc create MyService binPath= "c:\svc\sampleapp.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
```

> [!IMPORTANT]
> <span data-ttu-id="5e26d-231">確定參數的等號與參數的值之間有空格。</span><span class="sxs-lookup"><span data-stu-id="5e26d-231">Make sure that the spaces between the parameters' equal signs and the parameters' values are present.</span></span>

### <a name="start-the-service"></a><span data-ttu-id="5e26d-232">啟動服務</span><span class="sxs-lookup"><span data-stu-id="5e26d-232">Start the service</span></span>

<span data-ttu-id="5e26d-233">以 `sc start {SERVICE NAME}` 命令啟動服務。</span><span class="sxs-lookup"><span data-stu-id="5e26d-233">Start the service with the `sc start {SERVICE NAME}` command.</span></span>

<span data-ttu-id="5e26d-234">請使用下列命令啟動範例應用程式服務：</span><span class="sxs-lookup"><span data-stu-id="5e26d-234">To start the sample app service, use the following command:</span></span>

```console
sc start MyService
```

<span data-ttu-id="5e26d-235">此命令需要幾秒鐘啓動服務。</span><span class="sxs-lookup"><span data-stu-id="5e26d-235">The command takes a few seconds to start the service.</span></span>

### <a name="determine-the-service-status"></a><span data-ttu-id="5e26d-236">判斷服務狀態</span><span class="sxs-lookup"><span data-stu-id="5e26d-236">Determine the service status</span></span>

<span data-ttu-id="5e26d-237">若要檢查服務的狀態，請使用 `sc query {SERVICE NAME}` 命令。</span><span class="sxs-lookup"><span data-stu-id="5e26d-237">To check the status of the service, use the `sc query {SERVICE NAME}` command.</span></span> <span data-ttu-id="5e26d-238">狀態會回報為下列值之一：</span><span class="sxs-lookup"><span data-stu-id="5e26d-238">The status is reported as one of the following values:</span></span>

* `START_PENDING`
* `RUNNING`
* `STOP_PENDING`
* `STOPPED`

<span data-ttu-id="5e26d-239">使用下列命令來檢查範例應用程式服務的狀態：</span><span class="sxs-lookup"><span data-stu-id="5e26d-239">Use the following command to check the status of the sample app service:</span></span>

```console
sc query MyService
```

### <a name="browse-a-web-app-service"></a><span data-ttu-id="5e26d-240">瀏覽 Web 應用程式服務</span><span class="sxs-lookup"><span data-stu-id="5e26d-240">Browse a web app service</span></span>

<span data-ttu-id="5e26d-241">當服務處於 `RUNNING` 狀態，且若服務是 Web 應用程式時，請在其路徑瀏覽應用程式 (預設為 `http://localhost:5000`，使用 [HTTPS 重新導向中介軟體](xref:security/enforcing-ssl)時會重新導向至 `https://localhost:5001`)。</span><span class="sxs-lookup"><span data-stu-id="5e26d-241">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

<span data-ttu-id="5e26d-242">範例應用程式服務請瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="5e26d-242">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

### <a name="stop-the-service"></a><span data-ttu-id="5e26d-243">停止服務</span><span class="sxs-lookup"><span data-stu-id="5e26d-243">Stop the service</span></span>

<span data-ttu-id="5e26d-244">以 `sc stop {SERVICE NAME}` 命令停止服務。</span><span class="sxs-lookup"><span data-stu-id="5e26d-244">Stop the service with the `sc stop {SERVICE NAME}` command.</span></span>

<span data-ttu-id="5e26d-245">下列命令會停止範例應用程式服務：</span><span class="sxs-lookup"><span data-stu-id="5e26d-245">The following command stops the sample app service:</span></span>

```console
sc stop MyService
```

### <a name="delete-the-service"></a><span data-ttu-id="5e26d-246">刪除服務</span><span class="sxs-lookup"><span data-stu-id="5e26d-246">Delete the service</span></span>

<span data-ttu-id="5e26d-247">在停止服務的短暫延遲之後，請以 `sc delete {SERVICE NAME}` 命令解除安裝服務。</span><span class="sxs-lookup"><span data-stu-id="5e26d-247">After a short delay to stop a service, uninstall the service with the `sc delete {SERVICE NAME}` command.</span></span>

<span data-ttu-id="5e26d-248">檢查範例應用程式服務的狀態：</span><span class="sxs-lookup"><span data-stu-id="5e26d-248">Check the status of the sample app service:</span></span>

```console
sc query MyService
```

<span data-ttu-id="5e26d-249">當範例應用程式服務處於 `STOPPED` 狀態時，請使用下列命令來解除安裝範例應用程式服務：</span><span class="sxs-lookup"><span data-stu-id="5e26d-249">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

```console
sc delete MyService
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="5e26d-250">處理開始與停止事件</span><span class="sxs-lookup"><span data-stu-id="5e26d-250">Handle starting and stopping events</span></span>

<span data-ttu-id="5e26d-251">若要處理 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> 與 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> 事件，請進行下列額外變更：</span><span class="sxs-lookup"><span data-stu-id="5e26d-251">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events, perform the following additional changes:</span></span>

1. <span data-ttu-id="5e26d-252">使用 `OnStarting`、`OnStarted` 與 `OnStopping` 方法建立衍生自 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> 的類別：</span><span class="sxs-lookup"><span data-stu-id="5e26d-252">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="5e26d-253">為 <xref:Microsoft.AspNetCore.Hosting.IWebHost> 建立一個會將 `CustomWebHostService` 傳遞給 <xref:System.ServiceProcess.ServiceBase.Run*> 的擴充方法：</span><span class="sxs-lookup"><span data-stu-id="5e26d-253">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="5e26d-254">在 `Program.Main` 中，呼叫 `RunAsCustomService` 擴充方法，而不是呼叫 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>：</span><span class="sxs-lookup"><span data-stu-id="5e26d-254">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="5e26d-255">若要查看 `Program.Main` 中的 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> 位置，請參閱[將專案轉換為 Windows 服務](#convert-a-project-into-a-windows-service)一節中的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="5e26d-255">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Convert a project into a Windows Service](#convert-a-project-into-a-windows-service) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="5e26d-256">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="5e26d-256">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="5e26d-257">服務如果會與來自網際網路或公司網路的要求進行互動，並且位於 Proxy 或負載平衡器後方，可能會需要額外的設定。</span><span class="sxs-lookup"><span data-stu-id="5e26d-257">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="5e26d-258">如需詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="5e26d-258">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="5e26d-259">設定 HTTPS</span><span class="sxs-lookup"><span data-stu-id="5e26d-259">Configure HTTPS</span></span>

<span data-ttu-id="5e26d-260">使用安全端點來設定服務：</span><span class="sxs-lookup"><span data-stu-id="5e26d-260">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="5e26d-261">使用您平台的憑證取得與部署機制來建立將用於裝載系統的 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="5e26d-261">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="5e26d-262">指定 [Kestrel 伺服器 HTTPS 端點設定](xref:fundamentals/servers/kestrel#endpoint-configuration)以使用憑證。</span><span class="sxs-lookup"><span data-stu-id="5e26d-262">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="5e26d-263">不支援使用 ASP.NET Core HTTPS 開發憑證來保護服務端點。</span><span class="sxs-lookup"><span data-stu-id="5e26d-263">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="5e26d-264">目前目錄和內容根目錄</span><span class="sxs-lookup"><span data-stu-id="5e26d-264">Current directory and content root</span></span>

<span data-ttu-id="5e26d-265">針對 Windows 服務呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 所傳回的目前工作目錄為 *C:\\WINDOWS\\system32* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="5e26d-265">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="5e26d-266">*System32* 資料夾不是儲存服務檔案 (例如，設定檔) 的合適位置。</span><span class="sxs-lookup"><span data-stu-id="5e26d-266">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="5e26d-267">使用下列其中一個方式來維護及存取服務的資產與設定檔。</span><span class="sxs-lookup"><span data-stu-id="5e26d-267">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="5e26d-268">將內容根目錄路徑設定到應用程式的資料夾</span><span class="sxs-lookup"><span data-stu-id="5e26d-268">Set the content root path to the app's folder</span></span>

<span data-ttu-id="5e26d-269"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> 與建立服務時提供給 `binPath` 引數的路徑相同。</span><span class="sxs-lookup"><span data-stu-id="5e26d-269">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="5e26d-270">請搭配應用程式內容根目錄的路徑呼叫 <xref:System.IO.Directory.SetCurrentDirectory*>，而不要呼叫 `GetCurrentDirectory` 來建立設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="5e26d-270">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="5e26d-271">在 `Program.Main` 中，判斷服務可執行檔資料夾的路徑，然後使用該路徑來建立應用程式的內容根目錄：</span><span class="sxs-lookup"><span data-stu-id="5e26d-271">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="5e26d-272">將服務的檔案儲存在磁碟上的適當位置</span><span class="sxs-lookup"><span data-stu-id="5e26d-272">Store the service's files in a suitable location on disk</span></span>

<span data-ttu-id="5e26d-273">使用包含檔案的 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> 資料夾，使用 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 來指定絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="5e26d-273">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5e26d-274">其他資源</span><span class="sxs-lookup"><span data-stu-id="5e26d-274">Additional resources</span></span>

* <span data-ttu-id="5e26d-275">[Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration) (包括 HTTPS 組態與 SNI 支援)</span><span class="sxs-lookup"><span data-stu-id="5e26d-275">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
