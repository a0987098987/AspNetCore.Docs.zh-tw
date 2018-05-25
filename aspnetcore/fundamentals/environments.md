---
title: 在 ASP.NET Core 中使用多個環境
author: rick-anderson
description: 了解 ASP.NET Core 如何跨多個環境支援應用程式的行為控制。
manager: wpickett
ms.author: riande
ms.date: 12/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/environments
ms.openlocfilehash: 2c8441db527203aeea516073dae3bc335c335565
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/07/2018
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="14c9e-103">在 ASP.NET Core 中使用多個環境</span><span class="sxs-lookup"><span data-stu-id="14c9e-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="14c9e-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="14c9e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="14c9e-105">ASP.NET Core 可支援使用環境變數，以在執行階段設定應用程式的行為。</span><span class="sxs-lookup"><span data-stu-id="14c9e-105">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="14c9e-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="14c9e-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="14c9e-107">環境</span><span class="sxs-lookup"><span data-stu-id="14c9e-107">Environments</span></span>

<span data-ttu-id="14c9e-108">ASP.NET Core 會在應用程式啟動時讀取 `ASPNETCORE_ENVIRONMENT` 環境變數，並將該值儲存在 [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName) 中。</span><span class="sxs-lookup"><span data-stu-id="14c9e-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="14c9e-109">您可將 `ASPNETCORE_ENVIRONMENT` 設定為任何值，但架構支援下列[三個值](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0)：[Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0)、[Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0) 和 [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0)。</span><span class="sxs-lookup"><span data-stu-id="14c9e-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="14c9e-110">如果未設定 `ASPNETCORE_ENVIRONMENT`，則會預設為 `Production`。</span><span class="sxs-lookup"><span data-stu-id="14c9e-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it will default to `Production`.</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="14c9e-111">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="14c9e-111">The preceding code:</span></span>

* <span data-ttu-id="14c9e-112">當 `ASPNETCORE_ENVIRONMENT` 設為 `Development` 時，會呼叫 [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 和 [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_)。</span><span class="sxs-lookup"><span data-stu-id="14c9e-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="14c9e-113">當 `ASPNETCORE_ENVIRONMENT` 的值設為下列其中之一時，會呼叫 [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_)：</span><span class="sxs-lookup"><span data-stu-id="14c9e-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="14c9e-114">[環境標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)會使用 `IHostingEnvironment.EnvironmentName` 的值來包含或排除項目中的標籤：</span><span class="sxs-lookup"><span data-stu-id="14c9e-114">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="14c9e-115">注意：在 Windows 及 macOS 中，環境變數和值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="14c9e-115">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="14c9e-116">Linux 的環境變數和值則預設為**區分大小寫**。</span><span class="sxs-lookup"><span data-stu-id="14c9e-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="14c9e-117">開發</span><span class="sxs-lookup"><span data-stu-id="14c9e-117">Development</span></span>

<span data-ttu-id="14c9e-118">您可以在開發環境中啟用生產環境不應該公開的功能。</span><span class="sxs-lookup"><span data-stu-id="14c9e-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="14c9e-119">例如，ASP.NET Core 範本會在開發環境中啟用[開發人員例外狀況頁面](xref:fundamentals/error-handling#the-developer-exception-page)。</span><span class="sxs-lookup"><span data-stu-id="14c9e-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="14c9e-120">您可以在專案的 *Properties\launchSettings.json* 檔案設定適用於本機電腦開發的環境。</span><span class="sxs-lookup"><span data-stu-id="14c9e-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="14c9e-121">在 *launchSettings.json* 中設定的環境值會覆寫系統環境的設定值。</span><span class="sxs-lookup"><span data-stu-id="14c9e-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="14c9e-122">下列 JSON 顯示來自 *launchSettings.json* 檔案的三個設定檔：</span><span class="sxs-lookup"><span data-stu-id="14c9e-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

::: moniker range=">= aspnetcore-2.1"
> [!NOTE]
> <span data-ttu-id="14c9e-123">*launchSettings.json* 中的 `applicationUrl` 屬性可以指定伺服器 URL 的清單。</span><span class="sxs-lookup"><span data-stu-id="14c9e-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="14c9e-124">請在清單的 URL 之間使用分號：</span><span class="sxs-lookup"><span data-stu-id="14c9e-124">Use a semicolon between the URLs in the list:</span></span>
>
> ```json
> "WebApplication1": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```
::: moniker-end

<span data-ttu-id="14c9e-125">以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動應用程式時，會使用含有 `"commandName": "Project"` 的第一個設定檔。</span><span class="sxs-lookup"><span data-stu-id="14c9e-125">When the application is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="14c9e-126">`commandName` 的值可指定要啟動的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="14c9e-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="14c9e-127">`commandName` 可以是下列之一：</span><span class="sxs-lookup"><span data-stu-id="14c9e-127">`commandName` can be one of :</span></span>

* <span data-ttu-id="14c9e-128">IIS Express</span><span class="sxs-lookup"><span data-stu-id="14c9e-128">IIS Express</span></span>
* <span data-ttu-id="14c9e-129">IIS</span><span class="sxs-lookup"><span data-stu-id="14c9e-129">IIS</span></span>
* <span data-ttu-id="14c9e-130">啟動 Kestrel 的專案</span><span class="sxs-lookup"><span data-stu-id="14c9e-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="14c9e-131">以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動應用程式時：</span><span class="sxs-lookup"><span data-stu-id="14c9e-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="14c9e-132">會讀取 *launchSettings.json* (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="14c9e-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="14c9e-133">*launchSettings.json* 中的 `environmentVariables` 設定會覆寫環境變數。</span><span class="sxs-lookup"><span data-stu-id="14c9e-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="14c9e-134">主控環境隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="14c9e-134">The hosting environment is displayed.</span></span>


<span data-ttu-id="14c9e-135">下列輸出顯示應用程式是以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動：</span><span class="sxs-lookup"><span data-stu-id="14c9e-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="14c9e-136">Visual Studio 的 [偵錯] 索引標籤提供的 GUI 可用來編輯 *launchSettings.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="14c9e-136">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![專案屬性設定環境變數](environments/_static/project-properties-debug.png)

<span data-ttu-id="14c9e-138">您對專案設定檔所做的變更可能要等重新啟動網頁伺服器後才會生效。</span><span class="sxs-lookup"><span data-stu-id="14c9e-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="14c9e-139">您必須重新啟動 Kestrel，它才會偵測到環境已有的變更。</span><span class="sxs-lookup"><span data-stu-id="14c9e-139">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="14c9e-140">*launchSettings.json* 不應儲存密碼。</span><span class="sxs-lookup"><span data-stu-id="14c9e-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="14c9e-141">[密碼管理員工具](xref:security/app-secrets)可以用來儲存本機開發的密碼。</span><span class="sxs-lookup"><span data-stu-id="14c9e-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="14c9e-142">生產環境</span><span class="sxs-lookup"><span data-stu-id="14c9e-142">Production</span></span>

<span data-ttu-id="14c9e-143">若要將安全性、效能及應用程式的健全性最大化，您應該設定生產環境。</span><span class="sxs-lookup"><span data-stu-id="14c9e-143">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="14c9e-144">不同於開發的某些一般設定包括：</span><span class="sxs-lookup"><span data-stu-id="14c9e-144">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="14c9e-145">快取。</span><span class="sxs-lookup"><span data-stu-id="14c9e-145">Caching.</span></span>
* <span data-ttu-id="14c9e-146">用戶端的資源會經過配套、縮減，且可能由 CDN 提供。</span><span class="sxs-lookup"><span data-stu-id="14c9e-146">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="14c9e-147">停用診斷錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="14c9e-147">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="14c9e-148">啟用易懂的錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="14c9e-148">Friendly error pages enabled.</span></span>
* <span data-ttu-id="14c9e-149">啟用生產記錄與監視。</span><span class="sxs-lookup"><span data-stu-id="14c9e-149">Production logging and monitoring enabled.</span></span> <span data-ttu-id="14c9e-150">例如 [Application Insights](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="14c9e-150">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="14c9e-151">設定環境</span><span class="sxs-lookup"><span data-stu-id="14c9e-151">Setting the environment</span></span>

<span data-ttu-id="14c9e-152">一般來說，設定特定環境以進行測試，是很實用的方法。</span><span class="sxs-lookup"><span data-stu-id="14c9e-152">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="14c9e-153">如果您未設定環境，它會預設為 `Production` 並停用大部分的偵錯功能。</span><span class="sxs-lookup"><span data-stu-id="14c9e-153">If the environment isn't set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="14c9e-154">環境的設定方法取決於作業系統而定。</span><span class="sxs-lookup"><span data-stu-id="14c9e-154">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="14c9e-155">Azure</span><span class="sxs-lookup"><span data-stu-id="14c9e-155">Azure</span></span>

<span data-ttu-id="14c9e-156">若是 Azure App Service：</span><span class="sxs-lookup"><span data-stu-id="14c9e-156">For Azure app service:</span></span>

* <span data-ttu-id="14c9e-157">選取 [應用程式設定] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="14c9e-157">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="14c9e-158">將索引鍵和值新增至 [應用程式設定]。</span><span class="sxs-lookup"><span data-stu-id="14c9e-158">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="14c9e-159">Windows</span><span class="sxs-lookup"><span data-stu-id="14c9e-159">Windows</span></span>
<span data-ttu-id="14c9e-160">如果應用程式是使用 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動，請使用下列命令來設定目前工作階段的 `ASPNETCORE_ENVIRONMENT`：</span><span class="sxs-lookup"><span data-stu-id="14c9e-160">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used</span></span>

<span data-ttu-id="14c9e-161">**命令列**</span><span class="sxs-lookup"><span data-stu-id="14c9e-161">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="14c9e-162">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="14c9e-162">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="14c9e-163">這些命令僅針對目前的視窗才會生效。</span><span class="sxs-lookup"><span data-stu-id="14c9e-163">These commands take effect only for the current window.</span></span> <span data-ttu-id="14c9e-164">視窗關閉時，ASPNETCORE_ENVIRONMENT 設定會還原為預設值或電腦值。</span><span class="sxs-lookup"><span data-stu-id="14c9e-164">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="14c9e-165">若要全域設定 Windows 的值，請開啟 [控制台] > [系統] > [進階系統設定]，然後新增或編輯 `ASPNETCORE_ENVIRONMENT` 的值。</span><span class="sxs-lookup"><span data-stu-id="14c9e-165">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![系統進階屬性](environments/_static/systemsetting_environment.png)

![ASPNET Core 環境變數](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="14c9e-168">**web.config**</span><span class="sxs-lookup"><span data-stu-id="14c9e-168">**web.config**</span></span>

<span data-ttu-id="14c9e-169">請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)主題的＜設定環境變數＞一節。</span><span class="sxs-lookup"><span data-stu-id="14c9e-169">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="14c9e-170">**每個 IIS 應用程式集區**</span><span class="sxs-lookup"><span data-stu-id="14c9e-170">**Per IIS Application Pool**</span></span>

<span data-ttu-id="14c9e-171">若要為執行於隔離應用程式集區 (IIS 10.0+ 支援)　的個別應用程式設定環境變數，請參閱[環境變數 \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的＜AppCmd.exe 命令＞一節。</span><span class="sxs-lookup"><span data-stu-id="14c9e-171">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="14c9e-172">macOS</span><span class="sxs-lookup"><span data-stu-id="14c9e-172">macOS</span></span>
<span data-ttu-id="14c9e-173">您可以在執行應用程式時，以內嵌方式設定 macOS 目前的環境，</span><span class="sxs-lookup"><span data-stu-id="14c9e-173">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="14c9e-174">或在執行應用程式之前，使用 `export` 來設定。</span><span class="sxs-lookup"><span data-stu-id="14c9e-174">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="14c9e-175">您可以在 *.bashrc* 或 *.bash_profile* 檔案中設定電腦層級的環境變數。</span><span class="sxs-lookup"><span data-stu-id="14c9e-175">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="14c9e-176">使用任何文字編輯器編輯檔案，並新增下列陳述式。</span><span class="sxs-lookup"><span data-stu-id="14c9e-176">Edit the file using any text editor and add the following statment.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="14c9e-177">Linux</span><span class="sxs-lookup"><span data-stu-id="14c9e-177">Linux</span></span>
<span data-ttu-id="14c9e-178">若是 Linux 散發版本，請在命令列使用 `export` 命令，進行以工作階段為基礎的變數設定，並使用 *bash_profile* 檔案，進行電腦層級的環境設定。</span><span class="sxs-lookup"><span data-stu-id="14c9e-178">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="14c9e-179">取決於環境的組態</span><span class="sxs-lookup"><span data-stu-id="14c9e-179">Configuration by environment</span></span>

<span data-ttu-id="14c9e-180">如需詳細資訊，請參閱[取決於環境的組態](xref:fundamentals/configuration/index#configuration-by-environment)。</span><span class="sxs-lookup"><span data-stu-id="14c9e-180">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="14c9e-181">以環境為基礎的 Startup 類別和方法</span><span class="sxs-lookup"><span data-stu-id="14c9e-181">Environment based Startup class and methods</span></span>

<span data-ttu-id="14c9e-182">ASP.NET Core 應用程式啟動時，[Startup 類別](xref:fundamentals/startup)會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="14c9e-182">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="14c9e-183">如果 `Startup{EnvironmentName}` 類別存在，就會針對該 `EnvironmentName` 呼叫此類別：</span><span class="sxs-lookup"><span data-stu-id="14c9e-183">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="14c9e-184">注意：呼叫 [WebHostBuilder.UseStartup<TStartup> ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) 時會覆寫組態區段。</span><span class="sxs-lookup"><span data-stu-id="14c9e-184">Note: Calling [WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="14c9e-185">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 和 [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) 支援表單 `Configure{EnvironmentName}` 和 `Configure{EnvironmentName}Services` 的環境特定版本：</span><span class="sxs-lookup"><span data-stu-id="14c9e-185">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="14c9e-186">其他資源</span><span class="sxs-lookup"><span data-stu-id="14c9e-186">Additional resources</span></span>

* [<span data-ttu-id="14c9e-187">應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="14c9e-187">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="14c9e-188">組態</span><span class="sxs-lookup"><span data-stu-id="14c9e-188">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="14c9e-189">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="14c9e-189">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
