---
title: 在 ASP.NET Core 中使用多個環境
author: rick-anderson
description: 了解在 ASP.NET Core 應用程式中如何跨多個環境控制應用程式的行為。
ms.author: riande
ms.date: 07/03/2018
uid: fundamentals/environments
ms.openlocfilehash: 8983a0ce81beb16d68c799d30bfbfce6e7b693b1
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433944"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="c959f-103">在 ASP.NET Core 中使用多個環境</span><span class="sxs-lookup"><span data-stu-id="c959f-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="c959f-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c959f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c959f-105">ASP.NET Core 會使用環境變數根據執行階段環境來設定應用程式行為。</span><span class="sxs-lookup"><span data-stu-id="c959f-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="c959f-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c959f-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="c959f-107">環境</span><span class="sxs-lookup"><span data-stu-id="c959f-107">Environments</span></span>

<span data-ttu-id="c959f-108">ASP.NET Core 會在應用程式啟動時讀取 `ASPNETCORE_ENVIRONMENT` 環境變數，並將該值儲存在 [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) 中。</span><span class="sxs-lookup"><span data-stu-id="c959f-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span></span> <span data-ttu-id="c959f-109">您可將 `ASPNETCORE_ENVIRONMENT` 設定為任何值，但架構支援下列[三個值](/dotnet/api/microsoft.aspnetcore.hosting.environmentname)：[Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development)、[Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging) 和 [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production)。</span><span class="sxs-lookup"><span data-stu-id="c959f-109">You can set `ASPNETCORE_ENVIRONMENT` to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span></span> <span data-ttu-id="c959f-110">如果未設定 `ASPNETCORE_ENVIRONMENT`，則預設為 `Production`。</span><span class="sxs-lookup"><span data-stu-id="c959f-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it defaults to `Production`.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="c959f-111">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="c959f-111">The preceding code:</span></span>

* <span data-ttu-id="c959f-112">當 `ASPNETCORE_ENVIRONMENT` 設為 `Development` 時，會呼叫 [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) 和 [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink)。</span><span class="sxs-lookup"><span data-stu-id="c959f-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) and [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="c959f-113">當 `ASPNETCORE_ENVIRONMENT` 的值設為下列其中之一時，會呼叫 [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler)：</span><span class="sxs-lookup"><span data-stu-id="c959f-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="c959f-114">[環境標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)會使用 `IHostingEnvironment.EnvironmentName` 的值來包含或排除項目中的標籤：</span><span class="sxs-lookup"><span data-stu-id="c959f-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="c959f-115">在 Windows 及 macOS 中，環境變數和值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="c959f-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="c959f-116">Linux 的環境變數和值則預設為**區分大小寫**。</span><span class="sxs-lookup"><span data-stu-id="c959f-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="c959f-117">開發</span><span class="sxs-lookup"><span data-stu-id="c959f-117">Development</span></span>

<span data-ttu-id="c959f-118">您可以在開發環境中啟用生產環境不應該公開的功能。</span><span class="sxs-lookup"><span data-stu-id="c959f-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="c959f-119">例如，ASP.NET Core 範本會在開發環境中啟用[開發人員例外狀況頁面](xref:fundamentals/error-handling#the-developer-exception-page)。</span><span class="sxs-lookup"><span data-stu-id="c959f-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="c959f-120">您可以在專案的 *Properties\launchSettings.json* 檔案設定適用於本機電腦開發的環境。</span><span class="sxs-lookup"><span data-stu-id="c959f-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="c959f-121">在 *launchSettings.json* 中設定的環境值會覆寫系統環境的設定值。</span><span class="sxs-lookup"><span data-stu-id="c959f-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="c959f-122">下列 JSON 顯示來自 *launchSettings.json* 檔案的三個設定檔：</span><span class="sxs-lookup"><span data-stu-id="c959f-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:54339/",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      }
    },
    "EnvironmentsSample": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:54340/"
    },
    "Kestrel Staging": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:51997/"
    }
  }
}
```

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="c959f-123">*launchSettings.json* 中的 `applicationUrl` 屬性可以指定伺服器 URL 的清單。</span><span class="sxs-lookup"><span data-stu-id="c959f-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="c959f-124">請在清單的 URL 之間使用分號：</span><span class="sxs-lookup"><span data-stu-id="c959f-124">Use a semicolon between the URLs in the list:</span></span>
>
> ```json
> "EnvironmentsSample": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```

::: moniker-end

<span data-ttu-id="c959f-125">以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動應用程式時，會使用具有 `"commandName": "Project"` 的第一個設定檔。</span><span class="sxs-lookup"><span data-stu-id="c959f-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="c959f-126">`commandName` 的值可指定要啟動的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="c959f-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="c959f-127">`commandName` 可以是下列任何一個項目：</span><span class="sxs-lookup"><span data-stu-id="c959f-127">`commandName` can be any one of the following:</span></span>

* <span data-ttu-id="c959f-128">IIS Express</span><span class="sxs-lookup"><span data-stu-id="c959f-128">IIS Express</span></span>
* <span data-ttu-id="c959f-129">IIS</span><span class="sxs-lookup"><span data-stu-id="c959f-129">IIS</span></span>
* <span data-ttu-id="c959f-130">啟動 Kestrel 的專案</span><span class="sxs-lookup"><span data-stu-id="c959f-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="c959f-131">以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動應用程式時：</span><span class="sxs-lookup"><span data-stu-id="c959f-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="c959f-132">會讀取 *launchSettings.json* (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="c959f-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="c959f-133">*launchSettings.json* 中的 `environmentVariables` 設定會覆寫環境變數。</span><span class="sxs-lookup"><span data-stu-id="c959f-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="c959f-134">主控環境隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="c959f-134">The hosting environment is displayed.</span></span>

<span data-ttu-id="c959f-135">下列輸出顯示應用程式是以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動：</span><span class="sxs-lookup"><span data-stu-id="c959f-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="c959f-136">Visual Studio 專案屬性的 [偵錯] 索引標籤提供 GUI，可編輯 *launchSettings.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="c959f-136">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![專案屬性設定環境變數](environments/_static/project-properties-debug.png)

<span data-ttu-id="c959f-138">您對專案設定檔所做的變更可能要等重新啟動網頁伺服器後才會生效。</span><span class="sxs-lookup"><span data-stu-id="c959f-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="c959f-139">您必須重新啟動 Kestrel，它才會偵測到環境已進行的變更。</span><span class="sxs-lookup"><span data-stu-id="c959f-139">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="c959f-140">*launchSettings.json* 不應儲存密碼。</span><span class="sxs-lookup"><span data-stu-id="c959f-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="c959f-141">[密碼管理員工具](xref:security/app-secrets)可以用來儲存本機開發的密碼。</span><span class="sxs-lookup"><span data-stu-id="c959f-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="c959f-142">使用 [Visual Studio Code](https://code.visualstudio.com/) 時，可以會在 *.vscode/launch.json* 檔案設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="c959f-142">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="c959f-143">下列範例會將環境設定為 `Development`：</span><span class="sxs-lookup"><span data-stu-id="c959f-143">The following example sets the environment to `Development`:</span></span>

```json
{
   "version": "0.2.0",
   "configurations": [
        {
            "name": ".NET Core Launch (web)",

            ... additional VS Code configuration settings ...

            "env": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    ]
}
```

<span data-ttu-id="c959f-144">使用 `dotnet run` 啟動應用程式時，不會如同 *Properties/launchSettings.json* 一樣讀取專案中的 *.vscode/launch.json* 檔。</span><span class="sxs-lookup"><span data-stu-id="c959f-144">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="c959f-145">開發時若啟動的應用程式沒有 *launchSettings.json* 檔，請使用環境變數設定環境，或是使用 `dotnet run` 命令的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="c959f-145">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="c959f-146">生產環境</span><span class="sxs-lookup"><span data-stu-id="c959f-146">Production</span></span>

<span data-ttu-id="c959f-147">若要將安全性、效能及應用程式加強性最大化，您應該設定生產環境。</span><span class="sxs-lookup"><span data-stu-id="c959f-147">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="c959f-148">不同於開發的某些一般設定包括：</span><span class="sxs-lookup"><span data-stu-id="c959f-148">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="c959f-149">快取。</span><span class="sxs-lookup"><span data-stu-id="c959f-149">Caching.</span></span>
* <span data-ttu-id="c959f-150">用戶端的資源會經過配套、縮減，且可能由 CDN 提供。</span><span class="sxs-lookup"><span data-stu-id="c959f-150">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="c959f-151">停用診斷錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="c959f-151">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="c959f-152">啟用易懂的錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="c959f-152">Friendly error pages enabled.</span></span>
* <span data-ttu-id="c959f-153">啟用生產記錄與監視。</span><span class="sxs-lookup"><span data-stu-id="c959f-153">Production logging and monitoring enabled.</span></span> <span data-ttu-id="c959f-154">例如 [Application Insights](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="c959f-154">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="c959f-155">設定環境</span><span class="sxs-lookup"><span data-stu-id="c959f-155">Set the environment</span></span>

<span data-ttu-id="c959f-156">一般來說，設定特定環境以進行測試，是很實用的方法。</span><span class="sxs-lookup"><span data-stu-id="c959f-156">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="c959f-157">如果您未設定環境，它會預設為 `Production` 並停用大部分的偵錯功能。</span><span class="sxs-lookup"><span data-stu-id="c959f-157">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="c959f-158">環境的設定方法取決於作業系統而定。</span><span class="sxs-lookup"><span data-stu-id="c959f-158">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="c959f-159">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c959f-159">Azure App Service</span></span>

<span data-ttu-id="c959f-160">若要在 [Azure App Service](https://azure.microsoft.com/services/app-service/) 設定環境，請執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="c959f-160">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="c959f-161">從 [應用程式服務] 刀鋒視窗選取應用程式。</span><span class="sxs-lookup"><span data-stu-id="c959f-161">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="c959f-162">在 [設定] 群組中，選取 [應用程式設定] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="c959f-162">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="c959f-163">在 [應用程式設定] 區域中，選取 [新增設定]。</span><span class="sxs-lookup"><span data-stu-id="c959f-163">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="c959f-164">針對 [輸入名稱]，提供 `ASPNETCORE_ENVIRONMENT`。</span><span class="sxs-lookup"><span data-stu-id="c959f-164">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="c959f-165">針對 [輸入值]，提供環境 (例如 `Staging`)。</span><span class="sxs-lookup"><span data-stu-id="c959f-165">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="c959f-166">如果您想要在交換部署位置時，環境設定保留目前的位置，請選取 [位置設定] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="c959f-166">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="c959f-167">如需詳細資訊，請參閱 [Azure 文件：交換哪些設定？](/azure/app-service/web-sites-staged-publishing)。</span><span class="sxs-lookup"><span data-stu-id="c959f-167">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="c959f-168">選取刀鋒視窗頂端的 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="c959f-168">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="c959f-169">Azure App Service 會在新增、變更或刪除 Azure 入口網站的應用程式設定 (環境變數) 之後自動重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="c959f-169">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

### <a name="windows"></a><span data-ttu-id="c959f-170">Windows</span><span class="sxs-lookup"><span data-stu-id="c959f-170">Windows</span></span>

<span data-ttu-id="c959f-171">如果應用程式是使用 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動，請使用下列命令來設定目前工作階段的 `ASPNETCORE_ENVIRONMENT`：</span><span class="sxs-lookup"><span data-stu-id="c959f-171">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="c959f-172">**命令提示字元**</span><span class="sxs-lookup"><span data-stu-id="c959f-172">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="c959f-173">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="c959f-173">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="c959f-174">這些命令僅針對目前的視窗才會生效。</span><span class="sxs-lookup"><span data-stu-id="c959f-174">These commands only take effect for the current window.</span></span> <span data-ttu-id="c959f-175">視窗關閉時，`ASPNETCORE_ENVIRONMENT` 設定會還原為預設值或電腦值。</span><span class="sxs-lookup"><span data-stu-id="c959f-175">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="c959f-176">若要全域設定 Windows 的值，請開啟 [控制台] > [系統] > [進階系統設定]，然後新增或編輯 `ASPNETCORE_ENVIRONMENT` 的值：</span><span class="sxs-lookup"><span data-stu-id="c959f-176">To set the value globally in Windows, open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

![系統進階屬性](environments/_static/systemsetting_environment.png)

![ASPNET Core 環境變數](environments/_static/windows_aspnetcore_environment.png)

<span data-ttu-id="c959f-179">**web.config**</span><span class="sxs-lookup"><span data-stu-id="c959f-179">**web.config**</span></span>

<span data-ttu-id="c959f-180">請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)主題的＜設定環境變數＞一節。</span><span class="sxs-lookup"><span data-stu-id="c959f-180">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="c959f-181">**每個 IIS 應用程式集區**</span><span class="sxs-lookup"><span data-stu-id="c959f-181">**Per IIS Application Pool**</span></span>

<span data-ttu-id="c959f-182">若要為執行於隔離應用程式集區 (IIS 10.0+ 支援)　的個別應用程式設定環境變數，請參閱[環境變數 &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的＜AppCmd.exe 命令＞一節。</span><span class="sxs-lookup"><span data-stu-id="c959f-182">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="c959f-183">macOS</span><span class="sxs-lookup"><span data-stu-id="c959f-183">macOS</span></span>

<span data-ttu-id="c959f-184">您可以在執行應用程式時，以內嵌方式設定 macOS 目前的環境：</span><span class="sxs-lookup"><span data-stu-id="c959f-184">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="c959f-185">或者，在執行應用程式之前，使用 `export` 設定環境：</span><span class="sxs-lookup"><span data-stu-id="c959f-185">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="c959f-186">您可以在 *.bashrc* 或 *.bash_profile* 檔案中設定電腦層級的環境變數。</span><span class="sxs-lookup"><span data-stu-id="c959f-186">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="c959f-187">使用任何文字編輯器編輯檔案。</span><span class="sxs-lookup"><span data-stu-id="c959f-187">Edit the file using any text editor.</span></span> <span data-ttu-id="c959f-188">新增下列陳述式：</span><span class="sxs-lookup"><span data-stu-id="c959f-188">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="c959f-189">Linux</span><span class="sxs-lookup"><span data-stu-id="c959f-189">Linux</span></span>

<span data-ttu-id="c959f-190">針對 Linux 散發版本，請在命令提示字元使用 `export` 命令，進行以工作階段為基礎的變數設定，並使用 *bash_profile* 檔案，進行電腦層級的環境設定。</span><span class="sxs-lookup"><span data-stu-id="c959f-190">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="c959f-191">取決於環境的組態</span><span class="sxs-lookup"><span data-stu-id="c959f-191">Configuration by environment</span></span>

<span data-ttu-id="c959f-192">如需詳細資訊，請參閱[取決於環境的組態](xref:fundamentals/configuration/index#configuration-by-environment)。</span><span class="sxs-lookup"><span data-stu-id="c959f-192">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="c959f-193">以環境為基礎的 Startup 類別和方法</span><span class="sxs-lookup"><span data-stu-id="c959f-193">Environment-based Startup class and methods</span></span>

<span data-ttu-id="c959f-194">ASP.NET Core 應用程式啟動時，[Startup 類別](xref:fundamentals/startup)會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="c959f-194">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="c959f-195">如果 `Startup{EnvironmentName}` 類別存在，就會針對該 `EnvironmentName` 呼叫此類別：</span><span class="sxs-lookup"><span data-stu-id="c959f-195">If a `Startup{EnvironmentName}` class exists, the class is called for that `EnvironmentName`:</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="c959f-196">[WebHostBuilder.UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) 會覆寫組態區段。</span><span class="sxs-lookup"><span data-stu-id="c959f-196">[WebHostBuilder.UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="c959f-197">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) 和 [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) 支援表單 `Configure{EnvironmentName}` 和 `Configure{EnvironmentName}Services` 的環境特定版本：</span><span class="sxs-lookup"><span data-stu-id="c959f-197">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,51)]

## <a name="additional-resources"></a><span data-ttu-id="c959f-198">其他資源</span><span class="sxs-lookup"><span data-stu-id="c959f-198">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="c959f-199">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="c959f-199">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
