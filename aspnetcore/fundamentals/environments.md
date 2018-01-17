---
title: "使用多個環境中 ASP.NET Core"
author: rick-anderson
description: "了解 ASP.NET Core 應用程式行為控制跨多個環境所提供的支援。"
keywords: "ASP.NET Core，環境設定，ASPNETCORE_ENVIRONMENT"
ms.author: riande
manager: wpickett
ms.date: 12/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 784d176145c3e4e44ddc0ea06b6702f70cd4b08c
ms.sourcegitcommit: 87168cdc409e7a7257f92a0f48f9c5ab320b5b28
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/17/2018
---
# <a name="working-with-multiple-environments"></a><span data-ttu-id="815c3-104">使用多個環境</span><span class="sxs-lookup"><span data-stu-id="815c3-104">Working with multiple environments</span></span>

<span data-ttu-id="815c3-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="815c3-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="815c3-106">ASP.NET Core 提供支援使用環境變數在執行階段設定應用程式行為。</span><span class="sxs-lookup"><span data-stu-id="815c3-106">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="815c3-107">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="815c3-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="815c3-108">環境</span><span class="sxs-lookup"><span data-stu-id="815c3-108">Environments</span></span>

<span data-ttu-id="815c3-109">ASP.NET Core 讀取環境變數`ASPNETCORE_ENVIRONMENT`應用程式啟動和存放區中的值在[IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)。</span><span class="sxs-lookup"><span data-stu-id="815c3-109">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="815c3-110">`ASPNETCORE_ENVIRONMENT`可以設定為任何值，但[三個值](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0)framework 所支援：[開發](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0)，[臨時](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0)，和[生產](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0)。</span><span class="sxs-lookup"><span data-stu-id="815c3-110">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="815c3-111">如果`ASPNETCORE_ENVIRONMENT`未設定，它會預設為`Production`。</span><span class="sxs-lookup"><span data-stu-id="815c3-111">If `ASPNETCORE_ENVIRONMENT` is not set, it will default to `Production`.</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="815c3-112">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="815c3-112">The preceding code:</span></span>

* <span data-ttu-id="815c3-113">呼叫[UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_)和[UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_)時`ASPNETCORE_ENVIRONMENT`設`Development`。</span><span class="sxs-lookup"><span data-stu-id="815c3-113">Calls [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="815c3-114">呼叫[UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_)時的值`ASPNETCORE_ENVIRONMENT`設定下列其中之一：</span><span class="sxs-lookup"><span data-stu-id="815c3-114">Calls [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="815c3-115">[環境標記協助程式](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)會使用值`IHostingEnvironment.EnvironmentName`来包含或排除的項目中的標記：</span><span class="sxs-lookup"><span data-stu-id="815c3-115">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="815c3-116">注意： 在 Windows 及 macOS，環境變數和值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="815c3-116">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="815c3-117">Linux 環境變數和值都是**區分大小寫**預設。</span><span class="sxs-lookup"><span data-stu-id="815c3-117">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="815c3-118">開發</span><span class="sxs-lookup"><span data-stu-id="815c3-118">Development</span></span>

<span data-ttu-id="815c3-119">在開發環境可啟用不應該在生產環境中公開的功能。</span><span class="sxs-lookup"><span data-stu-id="815c3-119">The development environment can enable features that should not be exposed in production.</span></span> <span data-ttu-id="815c3-120">例如，ASP.NET Core 範本啟用[開發人員例外狀況頁面](xref:fundamentals/error-handling#the-developer-exception-page)開發環境中。</span><span class="sxs-lookup"><span data-stu-id="815c3-120">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="815c3-121">適用於本機開發環境可以設定*Properties\launchSettings.json*專案檔案。</span><span class="sxs-lookup"><span data-stu-id="815c3-121">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="815c3-122">設定環境值*launchSettings.json*覆寫系統環境中設定的值。</span><span class="sxs-lookup"><span data-stu-id="815c3-122">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="815c3-123">下列 XML 顯示三個設定檔從*launchSettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="815c3-123">The following XML shows three profiles from a *launchSettings.json* file:</span></span>

[!code-xml[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

<span data-ttu-id="815c3-124">當應用程式會使用啟動`dotnet run`，第一個設定檔與`"commandName": "Project"`將使用。</span><span class="sxs-lookup"><span data-stu-id="815c3-124">When the application is launched with `dotnet run`, the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="815c3-125">值`commandName`指定要啟動的 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="815c3-125">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="815c3-126">`commandName`可以是下列之一：</span><span class="sxs-lookup"><span data-stu-id="815c3-126">`commandName` can be one of :</span></span>

* <span data-ttu-id="815c3-127">IIS Express</span><span class="sxs-lookup"><span data-stu-id="815c3-127">IIS Express</span></span>
* <span data-ttu-id="815c3-128">IIS</span><span class="sxs-lookup"><span data-stu-id="815c3-128">IIS</span></span>
* <span data-ttu-id="815c3-129">（這樣會啟動 Kestrel） 專案</span><span class="sxs-lookup"><span data-stu-id="815c3-129">Project (which launches Kestrel)</span></span>

<span data-ttu-id="815c3-130">當應用程式會使用啟動`dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="815c3-130">When an app is launched with `dotnet run`:</span></span>

* <span data-ttu-id="815c3-131">*launchSettings.json*讀取如果有的話。</span><span class="sxs-lookup"><span data-stu-id="815c3-131">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="815c3-132">`environmentVariables`中的設定*launchSettings.json*覆寫環境變數。</span><span class="sxs-lookup"><span data-stu-id="815c3-132">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="815c3-133">裝載環境隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="815c3-133">The hosting environment is displayed.</span></span>


<span data-ttu-id="815c3-134">下列的輸出會顯示應用程式入門`dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="815c3-134">The following output shows an app started with `dotnet run`:</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="815c3-135">Visual Studio**偵錯** 索引標籤提供的 GUI，若要編輯*launchSettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="815c3-135">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![專案屬性設定環境變數](environments/_static/project-properties-debug.png)

<span data-ttu-id="815c3-137">專案設定檔所做的變更可能不會生效，直到重新啟動 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="815c3-137">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="815c3-138">Kestrel 必須重新啟動它就會偵測到它的環境所做的變更。</span><span class="sxs-lookup"><span data-stu-id="815c3-138">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="815c3-139">*launchSettings.json*應該儲存機密資料。</span><span class="sxs-lookup"><span data-stu-id="815c3-139">*launchSettings.json* should not store secrets.</span></span> <span data-ttu-id="815c3-140">[密碼管理員工具](xref:security/app-secrets)可以用來儲存本機開發的密碼。</span><span class="sxs-lookup"><span data-stu-id="815c3-140">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="815c3-141">生產環境</span><span class="sxs-lookup"><span data-stu-id="815c3-141">Production</span></span>

<span data-ttu-id="815c3-142">在實際執行環境應該設定為最大化安全性、 效能及應用程式的健全性。</span><span class="sxs-lookup"><span data-stu-id="815c3-142">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="815c3-143">某些常見設定生產環境中可能會不同於開發包括：</span><span class="sxs-lookup"><span data-stu-id="815c3-143">Some common settings that a production environment might have that would differ from development include:</span></span>

* <span data-ttu-id="815c3-144">快取。</span><span class="sxs-lookup"><span data-stu-id="815c3-144">Caching.</span></span>
* <span data-ttu-id="815c3-145">用戶端的資源配套、 縮短，以及可能從 CDN 服務。</span><span class="sxs-lookup"><span data-stu-id="815c3-145">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="815c3-146">停用診斷錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="815c3-146">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="815c3-147">啟用易懂的錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="815c3-147">Friendly error pages enabled.</span></span>
* <span data-ttu-id="815c3-148">實際執行記錄，且啟用監視。</span><span class="sxs-lookup"><span data-stu-id="815c3-148">Production logging and monitoring enabled.</span></span> <span data-ttu-id="815c3-149">例如， [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/)。</span><span class="sxs-lookup"><span data-stu-id="815c3-149">For example, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="815c3-150">設定環境</span><span class="sxs-lookup"><span data-stu-id="815c3-150">Setting the environment</span></span>

<span data-ttu-id="815c3-151">通常會很有用來設定特定的環境進行測試。</span><span class="sxs-lookup"><span data-stu-id="815c3-151">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="815c3-152">如果未設定環境，它會預設為`Production`表示停用大部分的偵錯功能。</span><span class="sxs-lookup"><span data-stu-id="815c3-152">If the environment is not set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="815c3-153">設定環境的方法取決於作業系統。</span><span class="sxs-lookup"><span data-stu-id="815c3-153">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="815c3-154">Azure</span><span class="sxs-lookup"><span data-stu-id="815c3-154">Azure</span></span>

<span data-ttu-id="815c3-155">Azure 應用程式服務：</span><span class="sxs-lookup"><span data-stu-id="815c3-155">For Azure app service:</span></span>

* <span data-ttu-id="815c3-156">選取**應用程式設定**刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="815c3-156">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="815c3-157">將索引鍵和值**應用程式設定**。</span><span class="sxs-lookup"><span data-stu-id="815c3-157">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="815c3-158">Windows</span><span class="sxs-lookup"><span data-stu-id="815c3-158">Windows</span></span>
<span data-ttu-id="815c3-159">若要設定`ASPNETCORE_ENVIRONMENT`目前工作階段，如果應用程式會使用啟動`dotnet run`，使用下列命令</span><span class="sxs-lookup"><span data-stu-id="815c3-159">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using `dotnet run`, the following commands are used</span></span>

<span data-ttu-id="815c3-160">**命令列**</span><span class="sxs-lookup"><span data-stu-id="815c3-160">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="815c3-161">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="815c3-161">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="815c3-162">這些命令才會生效，僅針對目前的視窗。</span><span class="sxs-lookup"><span data-stu-id="815c3-162">These commands take effect only for the current window.</span></span> <span data-ttu-id="815c3-163">視窗關閉時，ASPNETCORE_ENVIRONMENT 設定會還原為預設值或 machine 值中。</span><span class="sxs-lookup"><span data-stu-id="815c3-163">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="815c3-164">若要開啟 Windows 全域設定的值**控制台** > **系統** > **進階系統設定**以及新增或編輯`ASPNETCORE_ENVIRONMENT`值。</span><span class="sxs-lookup"><span data-stu-id="815c3-164">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![進階屬性的系統](environments/_static/systemsetting_environment.png)

![ASPNET 核心環境變數](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="815c3-167">**web.config**</span><span class="sxs-lookup"><span data-stu-id="815c3-167">**web.config**</span></span>

<span data-ttu-id="815c3-168">請參閱*設定環境變數*區段[ASP.NET 核心模組的組態參考](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)主題。</span><span class="sxs-lookup"><span data-stu-id="815c3-168">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="815c3-169">**每個 IIS 應用程式集區**</span><span class="sxs-lookup"><span data-stu-id="815c3-169">**Per IIS Application Pool**</span></span>

<span data-ttu-id="815c3-170">若要設定個別執行的應用程式 （支援 IIS 10.0 +） 隔離的應用程式集區中的環境變數，請參閱*AppCmd.exe 命令*區段[環境變數\<environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe)主題。</span><span class="sxs-lookup"><span data-stu-id="815c3-170">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="815c3-171">macOS</span><span class="sxs-lookup"><span data-stu-id="815c3-171">macOS</span></span>
<span data-ttu-id="815c3-172">設定 macOS 的目前環境可以是內建作業時完成執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="815c3-172">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="815c3-173">或使用`export`設定之前執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="815c3-173">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="815c3-174">電腦層級環境變數中設定*.bashrc*或*.bash_profile*檔案。</span><span class="sxs-lookup"><span data-stu-id="815c3-174">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="815c3-175">編輯檔案使用任何文字編輯器，並加入下列陳述式。</span><span class="sxs-lookup"><span data-stu-id="815c3-175">Edit the file using any text editor and add the following statment.</span></span>

```
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="815c3-176">Linux</span><span class="sxs-lookup"><span data-stu-id="815c3-176">Linux</span></span>
<span data-ttu-id="815c3-177">針對 Linux 散發版本，請使用`export`工作階段以變數設定為基礎的命令在命令列和*bash_profile*機器層級的環境設定的檔案。</span><span class="sxs-lookup"><span data-stu-id="815c3-177">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="815c3-178">設定環境</span><span class="sxs-lookup"><span data-stu-id="815c3-178">Configuration by environment</span></span>

<span data-ttu-id="815c3-179">請參閱[環境組態](xref:fundamentals/configuration/index#configuration-by-environment)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="815c3-179">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="815c3-180">環境基礎啟動類別和方法</span><span class="sxs-lookup"><span data-stu-id="815c3-180">Environment based Startup class and methods</span></span>

<span data-ttu-id="815c3-181">ASP.NET Core 應用程式啟動時，[啟動類別](xref:fundamentals/startup)bootstraps 應用程式。</span><span class="sxs-lookup"><span data-stu-id="815c3-181">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="815c3-182">如果類別`Startup{EnvironmentName}`存在，將該呼叫類別`EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="815c3-182">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="815c3-183">注意： 呼叫[WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)覆寫組態區段。</span><span class="sxs-lookup"><span data-stu-id="815c3-183">Note: Calling [WebHostBuilder.UseStartup<TStartup>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="815c3-184">[設定](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_)和[ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0)支援環境特定版本的表單`Configure{EnvironmentName}`和`Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="815c3-184">[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="815c3-185">其他資源</span><span class="sxs-lookup"><span data-stu-id="815c3-185">Additional Resources</span></span>

* [<span data-ttu-id="815c3-186">應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="815c3-186">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="815c3-187">組態</span><span class="sxs-lookup"><span data-stu-id="815c3-187">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="815c3-188">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="815c3-188">IHostingEnvironment.EnvironmentName</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
