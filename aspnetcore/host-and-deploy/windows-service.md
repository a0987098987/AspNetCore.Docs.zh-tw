---
title: 在 Windows 服務上裝載 ASP.NET Core
author: guardrex
description: 了解如何在 Windows 服務上裝載 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
uid: host-and-deploy/windows-service
ms.openlocfilehash: d4b540de50f4153f517f871f037521347fb5eb84
ms.sourcegitcommit: 990a4c2e623c202a27f60bdf3902f250359c13be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/03/2020
ms.locfileid: "76971996"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="4c248-103">在 Windows 服務上裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4c248-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="4c248-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4c248-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4c248-105">ASP.NET Core 應用程式可以裝載在 Windows 上作為 [Windows 服務](/dotnet/framework/windows-services/introduction-to-windows-service-applications)，不需要使用 IIS。</span><span class="sxs-lookup"><span data-stu-id="4c248-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="4c248-106">當裝載為 Windows 服務時，應用程式將會在伺服器重新開機後自動啟動。</span><span class="sxs-lookup"><span data-stu-id="4c248-106">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="4c248-107">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4c248-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4c248-108">必要條件：</span><span class="sxs-lookup"><span data-stu-id="4c248-108">Prerequisites</span></span>

* [<span data-ttu-id="4c248-109">ASP.NET Core SDK 2.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="4c248-109">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="4c248-110">PowerShell 6.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="4c248-110">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a><span data-ttu-id="4c248-111">背景工作服務範本</span><span class="sxs-lookup"><span data-stu-id="4c248-111">Worker Service template</span></span>

<span data-ttu-id="4c248-112">ASP.NET Core 背景工作服務範本提供撰寫長期執行服務應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="4c248-112">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="4c248-113">使用範本作為 Windows 服務應用程式的基礎：</span><span class="sxs-lookup"><span data-stu-id="4c248-113">To use the template as a basis for a Windows Service app:</span></span>

1. <span data-ttu-id="4c248-114">從 .NET Core 範本建立背景工作服務應用程式。</span><span class="sxs-lookup"><span data-stu-id="4c248-114">Create a Worker Service app from the .NET Core template.</span></span>
1. <span data-ttu-id="4c248-115">請遵循[應用程式組態](#app-configuration)一節中的指導方針，更新背景工作服務應用程式，以便其執行為 Windows 服務。</span><span class="sxs-lookup"><span data-stu-id="4c248-115">Follow the guidance in the [App configuration](#app-configuration) section to update the Worker Service app so that it can run as a Windows Service.</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

::: moniker-end

## <a name="app-configuration"></a><span data-ttu-id="4c248-116">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="4c248-116">App configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4c248-117">應用程式需要[WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices)的套件參考。</span><span class="sxs-lookup"><span data-stu-id="4c248-117">The app requires a package reference for [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices).</span></span>

<span data-ttu-id="4c248-118">建立主機時，會呼叫 `IHostBuilder.UseWindowsService`。</span><span class="sxs-lookup"><span data-stu-id="4c248-118">`IHostBuilder.UseWindowsService` is called when building the host.</span></span> <span data-ttu-id="4c248-119">如果應用程式以 Windows 服務形式執行，則方法會：</span><span class="sxs-lookup"><span data-stu-id="4c248-119">If the app is running as a Windows Service, the method:</span></span>

* <span data-ttu-id="4c248-120">將主機存留期設定為 `WindowsServiceLifetime`。</span><span class="sxs-lookup"><span data-stu-id="4c248-120">Sets the host lifetime to `WindowsServiceLifetime`.</span></span>
* <span data-ttu-id="4c248-121">將[內容根目錄](xref:fundamentals/index#content-root)設定為[AppCoNtext. BaseDirectory](xref:System.AppContext.BaseDirectory)。</span><span class="sxs-lookup"><span data-stu-id="4c248-121">Sets the [content root](xref:fundamentals/index#content-root) to [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory).</span></span> <span data-ttu-id="4c248-122">如需詳細資訊，請參閱[目前目錄與內容根目錄](#current-directory-and-content-root)一節。</span><span class="sxs-lookup"><span data-stu-id="4c248-122">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
* <span data-ttu-id="4c248-123">啟用使用應用程式名稱作為預設來源名稱記錄至事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="4c248-123">Enables logging to the event log with the application name as the default source name.</span></span>
  * <span data-ttu-id="4c248-124">您可以使用 *appsettings.Production.json* 檔案中的 `Logging:LogLevel:Default` 機碼來設定記錄層級。</span><span class="sxs-lookup"><span data-stu-id="4c248-124">The log level can be configured using the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>
  * <span data-ttu-id="4c248-125">只有系統管理員才能建立新的事件來源。</span><span class="sxs-lookup"><span data-stu-id="4c248-125">Only administrators can create new event sources.</span></span> <span data-ttu-id="4c248-126">如果無法使用應用程式名稱建立事件來源，則會向「應用程式」來源記錄警告，並停用事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="4c248-126">When an event source can't be created using the application name, a warning is logged to the *Application* source and event logs are disabled.</span></span>

<span data-ttu-id="4c248-127">在*Program.cs*的 `CreateHostBuilder` 中：</span><span class="sxs-lookup"><span data-stu-id="4c248-127">In `CreateHostBuilder` of *Program.cs*:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseWindowsService()
    ...
```

<span data-ttu-id="4c248-128">本主題隨附的下列範例應用程式：</span><span class="sxs-lookup"><span data-stu-id="4c248-128">The following sample apps accompany this topic:</span></span>

* <span data-ttu-id="4c248-129">背景工作角色服務範例會根據背景工作使用[託管服務](xref:fundamentals/host/hosted-services)的[工作者服務範本](#worker-service-template)，&ndash; 非 web 應用程式範例。</span><span class="sxs-lookup"><span data-stu-id="4c248-129">Background Worker Service Sample &ndash; A non-web app sample based on the [Worker Service template](#worker-service-template) that uses [hosted services](xref:fundamentals/host/hosted-services) for background tasks.</span></span>
* <span data-ttu-id="4c248-130">Web App Service 範例 &ndash; Razor Pages web 應用程式範例，其以 Windows 服務的形式執行，並具有適用于背景工作的[託管服務](xref:fundamentals/host/hosted-services)。</span><span class="sxs-lookup"><span data-stu-id="4c248-130">Web App Service Sample &ndash; A Razor Pages web app sample that runs as a Windows Service with [hosted services](xref:fundamentals/host/hosted-services) for background tasks.</span></span>

<span data-ttu-id="4c248-131">如需 MVC 指引，請參閱 <xref:mvc/overview> 和 <xref:migration/22-to-30>底下的文章。</span><span class="sxs-lookup"><span data-stu-id="4c248-131">For MVC guidance, see the articles under <xref:mvc/overview> and <xref:migration/22-to-30>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="4c248-132">應用程式需要 [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) 和 [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="4c248-132">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="4c248-133">若要在於服務外執行時測試及偵錯，請新增程式碼以判斷應用程式是以服務形式執行或以主控台應用程式形式執行。</span><span class="sxs-lookup"><span data-stu-id="4c248-133">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="4c248-134">檢查偵錯工具是否已附加或 `--console` 切換參數是否存在。</span><span class="sxs-lookup"><span data-stu-id="4c248-134">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="4c248-135">若任一條件為真 (應用程式不是以服務形式執行)，請呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>。</span><span class="sxs-lookup"><span data-stu-id="4c248-135">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="4c248-136">若條件為偽 (應用程式是以服務形式執行)：</span><span class="sxs-lookup"><span data-stu-id="4c248-136">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="4c248-137">呼叫 <xref:System.IO.Directory.SetCurrentDirectory*> 並使用應用程式發佈位置的路徑。</span><span class="sxs-lookup"><span data-stu-id="4c248-137">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="4c248-138">請勿呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 來取得路徑，因為當呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 時，Windows 服務應用程式會傳回 *C:\\WINDOWS\\system32* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="4c248-138">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="4c248-139">如需詳細資訊，請參閱[目前目錄與內容根目錄](#current-directory-and-content-root)一節。</span><span class="sxs-lookup"><span data-stu-id="4c248-139">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="4c248-140">在 `CreateWebHostBuilder` 中設定應用程式之前執行此步驟。</span><span class="sxs-lookup"><span data-stu-id="4c248-140">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="4c248-141">呼叫 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> 以服務形式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="4c248-141">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="4c248-142">因為[命令列設定提供者](xref:fundamentals/configuration/index#command-line-configuration-provider)需要命令列引數的名稱值組，在 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 收到引數之前，`--console` 切換參數就會從引數中移除。</span><span class="sxs-lookup"><span data-stu-id="4c248-142">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="4c248-143">若要寫入 Windows 事件記錄檔，請新增 EventLog 提供者到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>。</span><span class="sxs-lookup"><span data-stu-id="4c248-143">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="4c248-144">使用 *appsettings.Production.json* 檔案中的 `Logging:LogLevel:Default` 機碼設定記錄層級。</span><span class="sxs-lookup"><span data-stu-id="4c248-144">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="4c248-145">在範例應用程式的下列範例中，為了處理應用程式內的存留期事件，會呼叫 `RunAsCustomService` 而不是 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>。</span><span class="sxs-lookup"><span data-stu-id="4c248-145">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="4c248-146">如需詳細資訊，請參閱[處理開始與停止事件](#handle-starting-and-stopping-events)一節。</span><span class="sxs-lookup"><span data-stu-id="4c248-146">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a><span data-ttu-id="4c248-147">部署類型</span><span class="sxs-lookup"><span data-stu-id="4c248-147">Deployment type</span></span>

<span data-ttu-id="4c248-148">如需詳細資訊與部署案例建議，請參閱 [.NET Core 應用程式部署](/dotnet/core/deploying/)。</span><span class="sxs-lookup"><span data-stu-id="4c248-148">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="sdk"></a><span data-ttu-id="4c248-149">SDK</span><span class="sxs-lookup"><span data-stu-id="4c248-149">SDK</span></span>

<span data-ttu-id="4c248-150">針對使用 Razor Pages 或 MVC 架構的 web 應用程式服務，請在專案檔中指定 Web SDK：</span><span class="sxs-lookup"><span data-stu-id="4c248-150">For a web app-based service that uses the Razor Pages or MVC frameworks, specify the Web SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="4c248-151">如果服務只執行背景工作（例如，[託管服務](xref:fundamentals/host/hosted-services)），請在專案檔中指定工作者 SDK：</span><span class="sxs-lookup"><span data-stu-id="4c248-151">If the service only executes background tasks (for example, [hosted services](xref:fundamentals/host/hosted-services)), specify the Worker SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="4c248-152">架構相依部署 (FDD)</span><span class="sxs-lookup"><span data-stu-id="4c248-152">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="4c248-153">架構相依部署 (FDD) 仰賴存在於目標系統上的全系統共用 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="4c248-153">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="4c248-154">依照此文章中的指導方針採用 FDD 案例時，SDK 會產生可執行檔 ( *.exe*)，稱為「架構相依可執行檔」。</span><span class="sxs-lookup"><span data-stu-id="4c248-154">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4c248-155">如果使用[WEB SDK](#sdk)，通常是在發佈 ASP.NET Core 應用程式時所*產生的 web.config*檔案，對於 Windows 服務應用程式而言是不必要的。</span><span class="sxs-lookup"><span data-stu-id="4c248-155">If using the [Web SDK](#sdk), a *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="4c248-156">若要停用 *web.config* 檔案的建立，請新增 `<IsTransformWebConfigDisabled>` 屬性集到 `true`。</span><span class="sxs-lookup"><span data-stu-id="4c248-156">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="4c248-157">Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) 包含目標 Framework。</span><span class="sxs-lookup"><span data-stu-id="4c248-157">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="4c248-158">在下列範例中，RID 已設定為 `win7-x64`。</span><span class="sxs-lookup"><span data-stu-id="4c248-158">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="4c248-159">`<SelfContained>` 屬性設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="4c248-159">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="4c248-160">這些屬性會指示 SDK，針對 Windows 和相依於共用 .NET Core framework 的應用程式產生可執行檔 ( *.exe*)。</span><span class="sxs-lookup"><span data-stu-id="4c248-160">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="4c248-161">針對 Windows Services 應用程式，不需要 *web.config* 檔案 (發行 ASP.NET Core 應用程式時通常會產生此檔案)。</span><span class="sxs-lookup"><span data-stu-id="4c248-161">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="4c248-162">若要停用 *web.config* 檔案的建立，請新增 `<IsTransformWebConfigDisabled>` 屬性集到 `true`。</span><span class="sxs-lookup"><span data-stu-id="4c248-162">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="4c248-163">Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) 包含目標 Framework。</span><span class="sxs-lookup"><span data-stu-id="4c248-163">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="4c248-164">在下列範例中，RID 已設定為 `win7-x64`。</span><span class="sxs-lookup"><span data-stu-id="4c248-164">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="4c248-165">`<SelfContained>` 屬性設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="4c248-165">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="4c248-166">這些屬性會指示 SDK，針對 Windows 和相依於共用 .NET Core framework 的應用程式產生可執行檔 ( *.exe*)。</span><span class="sxs-lookup"><span data-stu-id="4c248-166">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="4c248-167">`<UseAppHost>` 屬性設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="4c248-167">The `<UseAppHost>` property is set to `true`.</span></span> <span data-ttu-id="4c248-168">此屬性為服務提供 FDD 的啟用路徑 (可執行檔 *.exe*)。</span><span class="sxs-lookup"><span data-stu-id="4c248-168">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

<span data-ttu-id="4c248-169">針對 Windows Services 應用程式，不需要 *web.config* 檔案 (發行 ASP.NET Core 應用程式時通常會產生此檔案)。</span><span class="sxs-lookup"><span data-stu-id="4c248-169">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="4c248-170">若要停用 *web.config* 檔案的建立，請新增 `<IsTransformWebConfigDisabled>` 屬性集到 `true`。</span><span class="sxs-lookup"><span data-stu-id="4c248-170">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="4c248-171">自封式部署 (SCD)</span><span class="sxs-lookup"><span data-stu-id="4c248-171">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="4c248-172">自封式部署 (SCD) 不仰賴任何存在於主機系統上的共用架構。</span><span class="sxs-lookup"><span data-stu-id="4c248-172">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="4c248-173">執行階段與應用程式的相依性會隨著應用程式進行部署。</span><span class="sxs-lookup"><span data-stu-id="4c248-173">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="4c248-174">Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog) 會納入包含目標 Framework 的 `<PropertyGroup>` 中：</span><span class="sxs-lookup"><span data-stu-id="4c248-174">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="4c248-175">發行多個 RID：</span><span class="sxs-lookup"><span data-stu-id="4c248-175">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="4c248-176">以分號分隔的清單提供 RID。</span><span class="sxs-lookup"><span data-stu-id="4c248-176">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="4c248-177">使用屬性名稱 [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (複數)。</span><span class="sxs-lookup"><span data-stu-id="4c248-177">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="4c248-178">如需詳細資訊，請參閱 [.NET Core RID 目錄](/dotnet/core/rid-catalog)。</span><span class="sxs-lookup"><span data-stu-id="4c248-178">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="4c248-179">`<SelfContained>` 屬性設定為 `true`：</span><span class="sxs-lookup"><span data-stu-id="4c248-179">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a><span data-ttu-id="4c248-180">服務使用者帳戶</span><span class="sxs-lookup"><span data-stu-id="4c248-180">Service user account</span></span>

<span data-ttu-id="4c248-181">若要為服務建立使用者帳戶，請從系統管理 PowerShell 6 命令殼層使用 [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) Cmdlet。</span><span class="sxs-lookup"><span data-stu-id="4c248-181">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="4c248-182">Windows 10 2018 年 10 月更新 (版本 1809/組建 10.0.17763) 或更新版本：</span><span class="sxs-lookup"><span data-stu-id="4c248-182">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```PowerShell
New-LocalUser -Name {SERVICE NAME}
```

<span data-ttu-id="4c248-183">在 Windows 10 2018 年 10 月更新之前 (1809 版/組建 10.0.17763) 的 Windows OS 上：</span><span class="sxs-lookup"><span data-stu-id="4c248-183">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {SERVICE NAME}"
```

<span data-ttu-id="4c248-184">在系統提示時提供[強式密碼](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements)。</span><span class="sxs-lookup"><span data-stu-id="4c248-184">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="4c248-185">除非搭配過期 <xref:System.DateTime> 將 `-AccountExpires` 參數提供給 [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) Cmdlet，否則該帳戶將不會過期。</span><span class="sxs-lookup"><span data-stu-id="4c248-185">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="4c248-186">如需詳細資訊，請參閱 [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) 和[服務使用者帳戶](/windows/desktop/services/service-user-accounts)。</span><span class="sxs-lookup"><span data-stu-id="4c248-186">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="4c248-187">使用 Active Directory 時有一個管理使用者的替代方法，就是使用「受控服務帳戶」。</span><span class="sxs-lookup"><span data-stu-id="4c248-187">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="4c248-188">如需詳細資訊，請參閱[群組受控服務帳戶概觀](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)。</span><span class="sxs-lookup"><span data-stu-id="4c248-188">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="4c248-189">以服務方式登入權限</span><span class="sxs-lookup"><span data-stu-id="4c248-189">Log on as a service rights</span></span>

<span data-ttu-id="4c248-190">若要為服務使用者帳戶建立「以服務方式登入」權限：</span><span class="sxs-lookup"><span data-stu-id="4c248-190">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="4c248-191">執行 *secpol.msc* 來開啟 [本機安全性原則編輯器]。</span><span class="sxs-lookup"><span data-stu-id="4c248-191">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="4c248-192">展開 [本機原則] 節點，然後選取 [使用者權限指派]。</span><span class="sxs-lookup"><span data-stu-id="4c248-192">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="4c248-193">開啟 [以服務方式登入] 原則。</span><span class="sxs-lookup"><span data-stu-id="4c248-193">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="4c248-194">選取 [新增使用者或群組]。</span><span class="sxs-lookup"><span data-stu-id="4c248-194">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="4c248-195">使用下列其中一種方法提供物件名稱 (使用者帳戶)：</span><span class="sxs-lookup"><span data-stu-id="4c248-195">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="4c248-196">在物件名稱欄位中輸入使用者帳戶 (`{DOMAIN OR COMPUTER NAME\USER}`)，然後選取 [確定] 將使用者新增至原則。</span><span class="sxs-lookup"><span data-stu-id="4c248-196">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="4c248-197">選取 [進階]。</span><span class="sxs-lookup"><span data-stu-id="4c248-197">Select **Advanced**.</span></span> <span data-ttu-id="4c248-198">選取 [立即尋找]。</span><span class="sxs-lookup"><span data-stu-id="4c248-198">Select **Find Now**.</span></span> <span data-ttu-id="4c248-199">從清單中選取使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="4c248-199">Select the user account from the list.</span></span> <span data-ttu-id="4c248-200">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="4c248-200">Select **OK**.</span></span> <span data-ttu-id="4c248-201">再次選取 [確定] 將使用者新增至原則。</span><span class="sxs-lookup"><span data-stu-id="4c248-201">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="4c248-202">選取 [確定] 或 [套用] 以接受變更。</span><span class="sxs-lookup"><span data-stu-id="4c248-202">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="4c248-203">建立及管理 Windows 服務</span><span class="sxs-lookup"><span data-stu-id="4c248-203">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="4c248-204">建立服務</span><span class="sxs-lookup"><span data-stu-id="4c248-204">Create a service</span></span>

<span data-ttu-id="4c248-205">使用 PowerShell 命令來註冊服務。</span><span class="sxs-lookup"><span data-stu-id="4c248-205">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="4c248-206">透過系統管理 PowerShell 6 命令殼層，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="4c248-206">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {SERVICE NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="4c248-207">`{EXE PATH}` &ndash; 路徑指向主機上的應用程式資料夾（例如，`d:\myservice`）。</span><span class="sxs-lookup"><span data-stu-id="4c248-207">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="4c248-208">請勿包含路徑中應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="4c248-208">Don't include the app's executable in the path.</span></span> <span data-ttu-id="4c248-209">不需要結尾的斜線。</span><span class="sxs-lookup"><span data-stu-id="4c248-209">A trailing slash isn't required.</span></span>
* <span data-ttu-id="4c248-210">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; 服務使用者帳戶（例如，`Contoso\ServiceUser`）。</span><span class="sxs-lookup"><span data-stu-id="4c248-210">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="4c248-211">`{SERVICE NAME}` &ndash; 服務名稱（例如，`MyService`）。</span><span class="sxs-lookup"><span data-stu-id="4c248-211">`{SERVICE NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="4c248-212">`{EXE FILE PATH}` &ndash; 應用程式的可執行檔路徑（例如，`d:\myservice\myservice.exe`）。</span><span class="sxs-lookup"><span data-stu-id="4c248-212">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="4c248-213">包含可執行檔的檔案名稱 (包含副檔名)。</span><span class="sxs-lookup"><span data-stu-id="4c248-213">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="4c248-214">`{DESCRIPTION}` &ndash; 服務描述（例如，`My sample service`）。</span><span class="sxs-lookup"><span data-stu-id="4c248-214">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="4c248-215">`{DISPLAY NAME}` &ndash; 服務顯示名稱（例如，`My Service`）。</span><span class="sxs-lookup"><span data-stu-id="4c248-215">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="4c248-216">啟動服務</span><span class="sxs-lookup"><span data-stu-id="4c248-216">Start a service</span></span>

<span data-ttu-id="4c248-217">以下列 PowerShell 6 命令啟動服務：</span><span class="sxs-lookup"><span data-stu-id="4c248-217">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {SERVICE NAME}
```

<span data-ttu-id="4c248-218">此命令需要幾秒鐘啓動服務。</span><span class="sxs-lookup"><span data-stu-id="4c248-218">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="4c248-219">判斷服務的狀態</span><span class="sxs-lookup"><span data-stu-id="4c248-219">Determine a service's status</span></span>

<span data-ttu-id="4c248-220">若要檢查服務狀態，請使用下列 PowerShell 6 命令：</span><span class="sxs-lookup"><span data-stu-id="4c248-220">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {SERVICE NAME}
```

<span data-ttu-id="4c248-221">狀態會回報為下列值之一：</span><span class="sxs-lookup"><span data-stu-id="4c248-221">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="4c248-222">停止服務</span><span class="sxs-lookup"><span data-stu-id="4c248-222">Stop a service</span></span>

<span data-ttu-id="4c248-223">使用下列 Powershell 6 命令停止服務：</span><span class="sxs-lookup"><span data-stu-id="4c248-223">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {SERVICE NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="4c248-224">移除服務</span><span class="sxs-lookup"><span data-stu-id="4c248-224">Remove a service</span></span>

<span data-ttu-id="4c248-225">在停止服務的短暫延遲之後，請以下列 Powershell 6 命令移除服務：</span><span class="sxs-lookup"><span data-stu-id="4c248-225">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {SERVICE NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="4c248-226">處理開始與停止事件</span><span class="sxs-lookup"><span data-stu-id="4c248-226">Handle starting and stopping events</span></span>

<span data-ttu-id="4c248-227">若要處理 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> 與 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> 事件：</span><span class="sxs-lookup"><span data-stu-id="4c248-227">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="4c248-228">使用 `OnStarting`、`OnStarted` 與 `OnStopping` 方法建立衍生自 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> 的類別：</span><span class="sxs-lookup"><span data-stu-id="4c248-228">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="4c248-229">為 <xref:Microsoft.AspNetCore.Hosting.IWebHost> 建立一個會將 `CustomWebHostService` 傳遞給 <xref:System.ServiceProcess.ServiceBase.Run*> 的擴充方法：</span><span class="sxs-lookup"><span data-stu-id="4c248-229">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="4c248-230">在 `Program.Main` 中，呼叫 `RunAsCustomService` 擴充方法，而不是呼叫 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>：</span><span class="sxs-lookup"><span data-stu-id="4c248-230">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="4c248-231">若要查看 `Program.Main` 中的 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> 位置，請參閱[部署類型](#deployment-type)一節中的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="4c248-231">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="4c248-232">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="4c248-232">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="4c248-233">服務如果會與來自網際網路或公司網路的要求進行互動，並且位於 Proxy 或負載平衡器後方，可能會需要額外的設定。</span><span class="sxs-lookup"><span data-stu-id="4c248-233">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="4c248-234">如需詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="4c248-234">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-endpoints"></a><span data-ttu-id="4c248-235">設定端點</span><span class="sxs-lookup"><span data-stu-id="4c248-235">Configure endpoints</span></span>

<span data-ttu-id="4c248-236">ASP.NET Core 預設會繫結至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="4c248-236">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="4c248-237">設定 `ASPNETCORE_URLS` 環境變數，以設定 URL 和埠。</span><span class="sxs-lookup"><span data-stu-id="4c248-237">Configure the URL and port by setting the `ASPNETCORE_URLS` environment variable.</span></span>

<span data-ttu-id="4c248-238">如需其他 URL 和埠設定方法，請參閱相關的伺服器文章：</span><span class="sxs-lookup"><span data-stu-id="4c248-238">For additional URL and port configuration approaches, see the relevant server article:</span></span>

* <xref:fundamentals/servers/kestrel#endpoint-configuration>
* <xref:fundamentals/servers/httpsys#configure-windows-server>

<span data-ttu-id="4c248-239">前述指引涵蓋 HTTPS 端點的支援。</span><span class="sxs-lookup"><span data-stu-id="4c248-239">The preceding guidance covers support for HTTPS endpoints.</span></span> <span data-ttu-id="4c248-240">例如，當搭配 Windows 服務使用驗證時，請為 HTTPS 設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="4c248-240">For example, configure the app for HTTPS when authentication is used with a Windows Service.</span></span>

> [!NOTE]
> <span data-ttu-id="4c248-241">不支援使用 ASP.NET Core HTTPS 開發憑證來保護服務端點。</span><span class="sxs-lookup"><span data-stu-id="4c248-241">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="4c248-242">目前目錄和內容根目錄</span><span class="sxs-lookup"><span data-stu-id="4c248-242">Current directory and content root</span></span>

<span data-ttu-id="4c248-243">針對 Windows 服務呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 所傳回的目前工作目錄為 *C:\\WINDOWS\\system32* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="4c248-243">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="4c248-244">*System32* 資料夾不是儲存服務檔案 (例如，設定檔) 的合適位置。</span><span class="sxs-lookup"><span data-stu-id="4c248-244">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="4c248-245">使用下列其中一個方式來維護及存取服務的資產與設定檔。</span><span class="sxs-lookup"><span data-stu-id="4c248-245">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a><span data-ttu-id="4c248-246">使用 ContentRootPath 或 ContentRootFileProvider</span><span class="sxs-lookup"><span data-stu-id="4c248-246">Use ContentRootPath or ContentRootFileProvider</span></span>

<span data-ttu-id="4c248-247">使用 [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) 或 <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> 來找出應用程式的資源。</span><span class="sxs-lookup"><span data-stu-id="4c248-247">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) or <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> to locate an app's resources.</span></span>

<span data-ttu-id="4c248-248">當應用程式以服務的形式執行時，<xref:Microsoft.Extensions.Hosting.WindowsServiceLifetimeHostBuilderExtensions.UseWindowsService*> 會將 <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath> 設定為[AppCoNtext. BaseDirectory](xref:System.AppContext.BaseDirectory)。</span><span class="sxs-lookup"><span data-stu-id="4c248-248">When the app runs as a service, <xref:Microsoft.Extensions.Hosting.WindowsServiceLifetimeHostBuilderExtensions.UseWindowsService*> sets the <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath> to [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory).</span></span>

<span data-ttu-id="4c248-249">應用程式的預設設定檔案*appsettings. json*和*appsettings。 {環境}. json*會在[主機結構期間呼叫 CreateDefaultBuilder](xref:fundamentals/host/generic-host#set-up-a-host)，從應用程式的內容根目錄載入。</span><span class="sxs-lookup"><span data-stu-id="4c248-249">The app's default settings files, *appsettings.json* and *appsettings.{Environment}.json*, are loaded from the app's content root by calling [CreateDefaultBuilder during host construction](xref:fundamentals/host/generic-host#set-up-a-host).</span></span>

<span data-ttu-id="4c248-250">若為開發人員程式碼在 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>中載入的其他設定檔案，則不需要呼叫 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>。</span><span class="sxs-lookup"><span data-stu-id="4c248-250">For other settings files loaded by developer code in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, there's no need to call <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="4c248-251">在下列範例中， *custom_settings 的 json*檔案存在於應用程式的內容根目錄中，並在未明確設定基底路徑的情況下載入：</span><span class="sxs-lookup"><span data-stu-id="4c248-251">In the following example, the *custom_settings.json* file exists in the app's content root and is loaded without explicitly setting a base path:</span></span>

[!code-csharp[](windows-service/samples_snapshot/CustomSettingsExample.cs?highlight=13)]

<span data-ttu-id="4c248-252">請勿嘗試使用 <xref:System.IO.Directory.GetCurrentDirectory*> 來取得資源路徑，因為 Windows 服務應用程式會傳回*C：\\Windows\\system32*資料夾做為其目前目錄。</span><span class="sxs-lookup"><span data-stu-id="4c248-252">Don't attempt to use <xref:System.IO.Directory.GetCurrentDirectory*> to obtain a resource path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder as its current directory.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="4c248-253">將內容根目錄路徑設定到應用程式的資料夾</span><span class="sxs-lookup"><span data-stu-id="4c248-253">Set the content root path to the app's folder</span></span>

<span data-ttu-id="4c248-254"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> 與建立服務時提供給 `binPath` 引數的路徑相同。</span><span class="sxs-lookup"><span data-stu-id="4c248-254">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="4c248-255">請呼叫具有應用程式[內容根目錄](xref:fundamentals/index#content-root)路徑的 <xref:System.IO.Directory.SetCurrentDirectory*>，而不是呼叫 `GetCurrentDirectory` 建立設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="4c248-255">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's [content root](xref:fundamentals/index#content-root).</span></span>

<span data-ttu-id="4c248-256">在 `Program.Main` 中，判斷服務可執行檔資料夾的路徑，然後使用該路徑來建立應用程式的內容根目錄：</span><span class="sxs-lookup"><span data-stu-id="4c248-256">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="4c248-257">將服務的檔案儲存在磁碟上的適當位置</span><span class="sxs-lookup"><span data-stu-id="4c248-257">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="4c248-258">使用包含檔案的 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> 資料夾，使用 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 來指定絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="4c248-258">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="4c248-259">疑難排解</span><span class="sxs-lookup"><span data-stu-id="4c248-259">Troubleshoot</span></span>

<span data-ttu-id="4c248-260">若要疑難排解 Windows 服務應用程式，請參閱 <xref:test/troubleshoot>。</span><span class="sxs-lookup"><span data-stu-id="4c248-260">To troubleshoot a Windows Service app, see <xref:test/troubleshoot>.</span></span>

### <a name="common-errors"></a><span data-ttu-id="4c248-261">常見的錯誤</span><span class="sxs-lookup"><span data-stu-id="4c248-261">Common errors</span></span>

* <span data-ttu-id="4c248-262">舊版或發行前版本的 PowerShell 已在使用中。</span><span class="sxs-lookup"><span data-stu-id="4c248-262">An old or pre-release version of PowerShell is in use.</span></span>
* <span data-ttu-id="4c248-263">已註冊的服務不會使用來自[dotnet publish](/dotnet/core/tools/dotnet-publish)命令的應用程式**已發佈**輸出。</span><span class="sxs-lookup"><span data-stu-id="4c248-263">The registered service doesn't use the app's **published** output from the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="4c248-264">應用程式部署不支援[dotnet build](/dotnet/core/tools/dotnet-build)命令的輸出。</span><span class="sxs-lookup"><span data-stu-id="4c248-264">Output of the [dotnet build](/dotnet/core/tools/dotnet-build) command isn't supported for app deployment.</span></span> <span data-ttu-id="4c248-265">根據部署類型，可以在下列其中一個資料夾中找到已發佈的資產：</span><span class="sxs-lookup"><span data-stu-id="4c248-265">Published assets are found in either of the following folders depending on the deployment type:</span></span>
  * <span data-ttu-id="4c248-266">*bin/Release/{TARGET FRAMEWORK}/publish* （FDD）</span><span class="sxs-lookup"><span data-stu-id="4c248-266">*bin/Release/{TARGET FRAMEWORK}/publish* (FDD)</span></span>
  * <span data-ttu-id="4c248-267">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* （SCD）</span><span class="sxs-lookup"><span data-stu-id="4c248-267">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* (SCD)</span></span>
* <span data-ttu-id="4c248-268">服務不是處於執行中狀態。</span><span class="sxs-lookup"><span data-stu-id="4c248-268">The service isn't in the RUNNING state.</span></span>
* <span data-ttu-id="4c248-269">應用程式所使用資源的路徑（例如憑證）不正確。</span><span class="sxs-lookup"><span data-stu-id="4c248-269">The paths to resources that the app uses (for example, certificates) are incorrect.</span></span> <span data-ttu-id="4c248-270">Windows 服務的基底路徑是*c：\\windows\\System32*。</span><span class="sxs-lookup"><span data-stu-id="4c248-270">The base path of a Windows Service is *c:\\Windows\\System32*.</span></span>
* <span data-ttu-id="4c248-271">使用者不具有 [*以服務方式登*入] 許可權。</span><span class="sxs-lookup"><span data-stu-id="4c248-271">The user doesn't have *Log on as a service* rights.</span></span>
* <span data-ttu-id="4c248-272">執行 `New-Service` PowerShell 命令時，使用者的密碼已過期或傳遞錯誤。</span><span class="sxs-lookup"><span data-stu-id="4c248-272">The user's password is expired or incorrectly passed when executing the `New-Service` PowerShell command.</span></span>
* <span data-ttu-id="4c248-273">應用程式需要 ASP.NET Core 驗證，但未設定安全連線（HTTPS）。</span><span class="sxs-lookup"><span data-stu-id="4c248-273">The app requires ASP.NET Core authentication but isn't configured for secure connections (HTTPS).</span></span>
* <span data-ttu-id="4c248-274">要求 URL 埠不正確，或未在應用程式中正確設定。</span><span class="sxs-lookup"><span data-stu-id="4c248-274">The request URL port is incorrect or not configured correctly in the app.</span></span>

### <a name="system-and-application-event-logs"></a><span data-ttu-id="4c248-275">系統和應用程式事件記錄檔</span><span class="sxs-lookup"><span data-stu-id="4c248-275">System and Application Event Logs</span></span>

<span data-ttu-id="4c248-276">存取系統和應用程式事件記錄檔：</span><span class="sxs-lookup"><span data-stu-id="4c248-276">Access the System and Application Event Logs:</span></span>

1. <span data-ttu-id="4c248-277">開啟 [開始] 功能表，搜尋 [*事件檢視器*]，然後選取 [**事件檢視器**] 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4c248-277">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="4c248-278">在 [事件檢視器] 中，開啟 [Windows 記錄] 節點。</span><span class="sxs-lookup"><span data-stu-id="4c248-278">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="4c248-279">選取 [**系統**] 以開啟 [系統] 事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="4c248-279">Select **System** to open the System Event Log.</span></span> <span data-ttu-id="4c248-280">選取 [應用程式] 以開啟「應用程式事件記錄檔」。</span><span class="sxs-lookup"><span data-stu-id="4c248-280">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="4c248-281">搜尋與失敗應用程式相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="4c248-281">Search for errors associated with the failing app.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="4c248-282">在命令提示字元中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="4c248-282">Run the app at a command prompt</span></span>

<span data-ttu-id="4c248-283">許多啟動錯誤不會在事件記錄檔中產生有用的資訊。</span><span class="sxs-lookup"><span data-stu-id="4c248-283">Many startup errors don't produce useful information in the event logs.</span></span> <span data-ttu-id="4c248-284">您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="4c248-284">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span> <span data-ttu-id="4c248-285">若要從應用程式記錄其他詳細資料，請降低[記錄層級](xref:fundamentals/logging/index#log-level)，或在[開發環境](xref:fundamentals/environments)中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="4c248-285">To log additional detail from the app, lower the [log level](xref:fundamentals/logging/index#log-level) or run the app in the [Development environment](xref:fundamentals/environments).</span></span>

### <a name="clear-package-caches"></a><span data-ttu-id="4c248-286">清除套件快取</span><span class="sxs-lookup"><span data-stu-id="4c248-286">Clear package caches</span></span>

<span data-ttu-id="4c248-287">升級開發電腦上的 .NET Core SDK 或變更應用程式內的套件版本之後，正常運作的應用程式可能會立即失敗。</span><span class="sxs-lookup"><span data-stu-id="4c248-287">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="4c248-288">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="4c248-288">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="4c248-289">大多數這些問題都可依照下列指示來進行修正：</span><span class="sxs-lookup"><span data-stu-id="4c248-289">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="4c248-290">刪除 [bin] 和 [obj] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="4c248-290">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="4c248-291">從命令 shell 執行[dotnet nuget 區域變數 all--clear](/dotnet/core/tools/dotnet-nuget-locals) ，以清除套件快取。</span><span class="sxs-lookup"><span data-stu-id="4c248-291">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="4c248-292">您也可以使用[nuget.exe](https://www.nuget.org/downloads)工具來完成清除套件快取，並 `nuget locals all -clear`執行命令。</span><span class="sxs-lookup"><span data-stu-id="4c248-292">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="4c248-293">*nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。</span><span class="sxs-lookup"><span data-stu-id="4c248-293">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="4c248-294">還原並重建專案。</span><span class="sxs-lookup"><span data-stu-id="4c248-294">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="4c248-295">重新部署應用程式之前，請先刪除伺服器上 [部署] 資料夾中的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="4c248-295">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

### <a name="slow-or-hanging-app"></a><span data-ttu-id="4c248-296">回應緩慢或無回應的應用程式</span><span class="sxs-lookup"><span data-stu-id="4c248-296">Slow or hanging app</span></span>

<span data-ttu-id="4c248-297">損*毀*傾印是系統記憶體的快照集，有助於判斷應用程式損毀、啟動失敗或應用程式緩慢的原因。</span><span class="sxs-lookup"><span data-stu-id="4c248-297">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="4c248-298">應用程式損毀或發生例外狀況</span><span class="sxs-lookup"><span data-stu-id="4c248-298">App crashes or encounters an exception</span></span>

<span data-ttu-id="4c248-299">從 [Windows 錯誤報告 (WER)](/windows/desktop/wer/windows-error-reporting) 取得並分析傾印：</span><span class="sxs-lookup"><span data-stu-id="4c248-299">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="4c248-300">在 `c:\dumps` 中建立資料夾以保存損毀傾印檔案。</span><span class="sxs-lookup"><span data-stu-id="4c248-300">Create a folder to hold crash dump files at `c:\dumps`.</span></span>
1. <span data-ttu-id="4c248-301">以應用程式可執行檔名稱執行[EnableDumps PowerShell 腳本](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="4c248-301">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) with the application executable name:</span></span>

   ```console
   .\EnableDumps {APPLICATION EXE} c:\dumps
   ```

1. <span data-ttu-id="4c248-302">在會導致損毀的情況下，執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="4c248-302">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="4c248-303">發生損毀之後，請執行 [DisableDumps PowerShell 指令碼](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="4c248-303">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1):</span></span>

   ```console
   .\DisableDumps {APPLICATION EXE}
   ```

<span data-ttu-id="4c248-304">在應用程式損毀且傾印收集完成之後，即可正常結束應用程式。</span><span class="sxs-lookup"><span data-stu-id="4c248-304">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="4c248-305">PowerShell 指令碼會將 WER 設定為每個應用程式收集最多五個傾印。</span><span class="sxs-lookup"><span data-stu-id="4c248-305">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="4c248-306">損毀傾印可能會佔用大量磁碟空間 (每個高達好幾 GB)。</span><span class="sxs-lookup"><span data-stu-id="4c248-306">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="4c248-307">應用程式停止回應、在啟動期間失敗，或正常執行</span><span class="sxs-lookup"><span data-stu-id="4c248-307">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="4c248-308">當*應用程式*當機（停止回應但未損毀）、啟動期間失敗，或正常執行時，請參閱使用者模式傾印檔案[：選擇最適合的工具](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)來選取適當的工具以產生傾印。</span><span class="sxs-lookup"><span data-stu-id="4c248-308">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="4c248-309">分析傾印</span><span class="sxs-lookup"><span data-stu-id="4c248-309">Analyze the dump</span></span>

<span data-ttu-id="4c248-310">您可以使用數種方法來分析傾印。</span><span class="sxs-lookup"><span data-stu-id="4c248-310">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="4c248-311">如需詳細資訊，請參閱[分析使用者模式傾印檔案](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)。</span><span class="sxs-lookup"><span data-stu-id="4c248-311">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4c248-312">其他資源</span><span class="sxs-lookup"><span data-stu-id="4c248-312">Additional resources</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="4c248-313">[Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration) (包括 HTTPS 組態與 SNI 支援)</span><span class="sxs-lookup"><span data-stu-id="4c248-313">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="4c248-314">[Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration) (包括 HTTPS 組態與 SNI 支援)</span><span class="sxs-lookup"><span data-stu-id="4c248-314">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
