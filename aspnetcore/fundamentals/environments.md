---
title: 在 ASP.NET Core 中使用多個環境
author: rick-anderson
description: 了解在 ASP.NET Core 應用程式中如何跨多個環境控制應用程式的行為。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: fundamentals/environments
ms.openlocfilehash: affbb95273c91fe5bf452e0e1ebefa669297304c
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944317"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="aef5f-103">在 ASP.NET Core 中使用多個環境</span><span class="sxs-lookup"><span data-stu-id="aef5f-103">Use multiple environments in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="aef5f-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="aef5f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="aef5f-105">ASP.NET Core 會使用環境變數根據執行階段環境來設定應用程式行為。</span><span class="sxs-lookup"><span data-stu-id="aef5f-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="aef5f-106">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="aef5f-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="aef5f-107">環境</span><span class="sxs-lookup"><span data-stu-id="aef5f-107">Environments</span></span>

<span data-ttu-id="aef5f-108">ASP.NET Core 會在應用程式啟動時讀取環境變數 `ASPNETCORE_ENVIRONMENT`，並將值儲存在[IWebHostEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName)中。</span><span class="sxs-lookup"><span data-stu-id="aef5f-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IWebHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span></span> <span data-ttu-id="aef5f-109">`ASPNETCORE_ENVIRONMENT` 可以設定為任何值，但架構會提供三個值：</span><span class="sxs-lookup"><span data-stu-id="aef5f-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.Extensions.Hosting.Environments.Development>
* <xref:Microsoft.Extensions.Hosting.Environments.Staging>
* <span data-ttu-id="aef5f-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (預設值)</span><span class="sxs-lookup"><span data-stu-id="aef5f-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="aef5f-111">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="aef5f-111">The preceding code:</span></span>

* <span data-ttu-id="aef5f-112">當 `ASPNETCORE_ENVIRONMENT` 設為 `Development` 時，會呼叫 [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage)。</span><span class="sxs-lookup"><span data-stu-id="aef5f-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="aef5f-113">當 `ASPNETCORE_ENVIRONMENT` 的值設為下列其中之一時，會呼叫 [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler)：</span><span class="sxs-lookup"><span data-stu-id="aef5f-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="aef5f-114">[環境標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)會使用 `IHostingEnvironment.EnvironmentName` 的值來包含或排除項目中的標籤：</span><span class="sxs-lookup"><span data-stu-id="aef5f-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="aef5f-115">在 Windows 及 macOS 中，環境變數和值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="aef5f-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="aef5f-116">Linux 的環境變數和值則預設為**區分大小寫**。</span><span class="sxs-lookup"><span data-stu-id="aef5f-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="aef5f-117">開發</span><span class="sxs-lookup"><span data-stu-id="aef5f-117">Development</span></span>

<span data-ttu-id="aef5f-118">您可以在開發環境中啟用生產環境不應該公開的功能。</span><span class="sxs-lookup"><span data-stu-id="aef5f-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="aef5f-119">例如，ASP.NET Core 範本會在開發環境中啟用[開發人員例外狀況頁面](xref:fundamentals/error-handling#developer-exception-page)。</span><span class="sxs-lookup"><span data-stu-id="aef5f-119">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="aef5f-120">您可以在專案的 *Properties\launchSettings.json* 檔案設定適用於本機電腦開發的環境。</span><span class="sxs-lookup"><span data-stu-id="aef5f-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="aef5f-121">在 *launchSettings.json* 中設定的環境值會覆寫系統環境的設定值。</span><span class="sxs-lookup"><span data-stu-id="aef5f-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="aef5f-122">下列 JSON 顯示來自 *launchSettings.json* 檔案的三個設定檔：</span><span class="sxs-lookup"><span data-stu-id="aef5f-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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

> [!NOTE]
> <span data-ttu-id="aef5f-123">*launchSettings.json* 中的 `applicationUrl` 屬性可以指定伺服器 URL 的清單。</span><span class="sxs-lookup"><span data-stu-id="aef5f-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="aef5f-124">請在清單的 URL 之間使用分號：</span><span class="sxs-lookup"><span data-stu-id="aef5f-124">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="aef5f-125">以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動應用程式時，會使用具有 `"commandName": "Project"` 的第一個設定檔。</span><span class="sxs-lookup"><span data-stu-id="aef5f-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="aef5f-126">`commandName` 的值可指定要啟動的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="aef5f-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="aef5f-127">`commandName` 可以是下列任何一個項目：</span><span class="sxs-lookup"><span data-stu-id="aef5f-127">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="aef5f-128">`Project` (這會啟動 Kestrel)</span><span class="sxs-lookup"><span data-stu-id="aef5f-128">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="aef5f-129">以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動應用程式時：</span><span class="sxs-lookup"><span data-stu-id="aef5f-129">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="aef5f-130">會讀取 *launchSettings.json* (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="aef5f-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="aef5f-131">*launchSettings.json* 中的 `environmentVariables` 設定會覆寫環境變數。</span><span class="sxs-lookup"><span data-stu-id="aef5f-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="aef5f-132">主控環境隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="aef5f-132">The hosting environment is displayed.</span></span>

<span data-ttu-id="aef5f-133">下列輸出顯示應用程式是以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動：</span><span class="sxs-lookup"><span data-stu-id="aef5f-133">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="aef5f-134">Visual Studio 專案屬性的 [偵錯] 索引標籤提供 GUI，可編輯 *launchSettings.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="aef5f-134">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![專案屬性設定環境變數](environments/_static/project-properties-debug.png)

<span data-ttu-id="aef5f-136">您對專案設定檔所做的變更可能要等重新啟動網頁伺服器後才會生效。</span><span class="sxs-lookup"><span data-stu-id="aef5f-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="aef5f-137">您必須重新啟動 Kestrel，它才會偵測到環境已進行的變更。</span><span class="sxs-lookup"><span data-stu-id="aef5f-137">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="aef5f-138">*launchSettings.json* 不應儲存密碼。</span><span class="sxs-lookup"><span data-stu-id="aef5f-138">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="aef5f-139">[密碼管理員工具](xref:security/app-secrets)可以用來儲存本機開發的密碼。</span><span class="sxs-lookup"><span data-stu-id="aef5f-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="aef5f-140">使用 [Visual Studio Code](https://code.visualstudio.com/) 時，可以會在 *.vscode/launch.json* 檔案設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="aef5f-140">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="aef5f-141">下列範例會將環境設定為 `Development`：</span><span class="sxs-lookup"><span data-stu-id="aef5f-141">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="aef5f-142">使用 `dotnet run` 啟動應用程式時，不會如同 *Properties/launchSettings.json* 一樣讀取專案中的 *.vscode/launch.json* 檔。</span><span class="sxs-lookup"><span data-stu-id="aef5f-142">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="aef5f-143">開發時若啟動的應用程式沒有 *launchSettings.json* 檔，請使用環境變數設定環境，或是使用 `dotnet run` 命令的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="aef5f-143">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="aef5f-144">生產環境</span><span class="sxs-lookup"><span data-stu-id="aef5f-144">Production</span></span>

<span data-ttu-id="aef5f-145">若要將安全性、效能及應用程式加強性最大化，您應該設定生產環境。</span><span class="sxs-lookup"><span data-stu-id="aef5f-145">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="aef5f-146">不同於開發的某些一般設定包括：</span><span class="sxs-lookup"><span data-stu-id="aef5f-146">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="aef5f-147">快取。</span><span class="sxs-lookup"><span data-stu-id="aef5f-147">Caching.</span></span>
* <span data-ttu-id="aef5f-148">用戶端的資源會經過配套、縮減，且可能由 CDN 提供。</span><span class="sxs-lookup"><span data-stu-id="aef5f-148">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="aef5f-149">停用診斷錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="aef5f-149">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="aef5f-150">啟用易懂的錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="aef5f-150">Friendly error pages enabled.</span></span>
* <span data-ttu-id="aef5f-151">啟用生產記錄與監視。</span><span class="sxs-lookup"><span data-stu-id="aef5f-151">Production logging and monitoring enabled.</span></span> <span data-ttu-id="aef5f-152">例如 [Application Insights](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="aef5f-152">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="aef5f-153">設定環境</span><span class="sxs-lookup"><span data-stu-id="aef5f-153">Set the environment</span></span>

<span data-ttu-id="aef5f-154">設定特定環境以使用環境變數或平臺設定進行測試時，通常會很有用。</span><span class="sxs-lookup"><span data-stu-id="aef5f-154">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="aef5f-155">如果您未設定環境，它會預設為 `Production` 並停用大部分的偵錯功能。</span><span class="sxs-lookup"><span data-stu-id="aef5f-155">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="aef5f-156">環境的設定方法取決於作業系統而定。</span><span class="sxs-lookup"><span data-stu-id="aef5f-156">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="aef5f-157">建立主機時，應用程式所讀取的最後一個環境設定會決定應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="aef5f-157">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="aef5f-158">應用程式正在執行時，無法變更應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="aef5f-158">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="aef5f-159">環境變數或平臺設定</span><span class="sxs-lookup"><span data-stu-id="aef5f-159">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="aef5f-160">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="aef5f-160">Azure App Service</span></span>

<span data-ttu-id="aef5f-161">若要在 [Azure App Service](https://azure.microsoft.com/services/app-service/) 設定環境，請執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="aef5f-161">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="aef5f-162">從 [應用程式服務] 刀鋒視窗選取應用程式。</span><span class="sxs-lookup"><span data-stu-id="aef5f-162">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="aef5f-163">在 [設定] 群組中，選取 [應用程式設定] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="aef5f-163">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="aef5f-164">在 [應用程式設定] 區域中，選取 [新增設定]。</span><span class="sxs-lookup"><span data-stu-id="aef5f-164">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="aef5f-165">針對 [輸入名稱]，提供 `ASPNETCORE_ENVIRONMENT`。</span><span class="sxs-lookup"><span data-stu-id="aef5f-165">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="aef5f-166">針對 [輸入值]，提供環境 (例如 `Staging`)。</span><span class="sxs-lookup"><span data-stu-id="aef5f-166">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="aef5f-167">如果您想要在交換部署位置時，環境設定保留目前的位置，請選取 [位置設定] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="aef5f-167">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="aef5f-168">如需詳細資訊，請參閱 [Azure 文件：交換哪些設定？](/azure/app-service/web-sites-staged-publishing)。</span><span class="sxs-lookup"><span data-stu-id="aef5f-168">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="aef5f-169">選取刀鋒視窗頂端的 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="aef5f-169">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="aef5f-170">Azure App Service 會在新增、變更或刪除 Azure 入口網站的應用程式設定 (環境變數) 之後自動重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="aef5f-170">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="aef5f-171">Windows</span><span class="sxs-lookup"><span data-stu-id="aef5f-171">Windows</span></span>

<span data-ttu-id="aef5f-172">如果應用程式是使用 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動，請使用下列命令來設定目前工作階段的 `ASPNETCORE_ENVIRONMENT`：</span><span class="sxs-lookup"><span data-stu-id="aef5f-172">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="aef5f-173">**命令提示字元**</span><span class="sxs-lookup"><span data-stu-id="aef5f-173">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="aef5f-174">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="aef5f-174">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="aef5f-175">這些命令僅針對目前的視窗才會生效。</span><span class="sxs-lookup"><span data-stu-id="aef5f-175">These commands only take effect for the current window.</span></span> <span data-ttu-id="aef5f-176">視窗關閉時，`ASPNETCORE_ENVIRONMENT` 設定會還原為預設值或電腦值。</span><span class="sxs-lookup"><span data-stu-id="aef5f-176">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="aef5f-177">若要在 Windows 中以全域的方式設定值，請使用下列其中一個方法：</span><span class="sxs-lookup"><span data-stu-id="aef5f-177">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="aef5f-178">開啟 [**控制台**] >**系統**> [ **Advanced system settings** ]，然後新增或編輯 `ASPNETCORE_ENVIRONMENT` 值：</span><span class="sxs-lookup"><span data-stu-id="aef5f-178">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![系統進階屬性](environments/_static/systemsetting_environment.png)

  ![ASPNET Core 環境變數](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="aef5f-181">開啟系統管理命令提示字元，然後使用 `setx` 命令，或開啟系統管理 PowerShell 命令提示字元，然後使用 `[Environment]::SetEnvironmentVariable`：</span><span class="sxs-lookup"><span data-stu-id="aef5f-181">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="aef5f-182">**命令提示字元**</span><span class="sxs-lookup"><span data-stu-id="aef5f-182">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="aef5f-183">`/M` 參數表示將環境變數設定在系統層級。</span><span class="sxs-lookup"><span data-stu-id="aef5f-183">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="aef5f-184">若未使用 `/M` 參數，則將環境變數設定為使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="aef5f-184">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="aef5f-185">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="aef5f-185">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="aef5f-186">`Machine` 選項值表示將環境變數設定在系統層級。</span><span class="sxs-lookup"><span data-stu-id="aef5f-186">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="aef5f-187">若選項值變更為 `User`，則將環境變數設定為使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="aef5f-187">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="aef5f-188">當以全域的方式設定 `ASPNETCORE_ENVIRONMENT` 環境變數時，則在設定該值後開啟的任何命令視窗中，對 `dotnet run` 均有效。</span><span class="sxs-lookup"><span data-stu-id="aef5f-188">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="aef5f-189">**web.config**</span><span class="sxs-lookup"><span data-stu-id="aef5f-189">**web.config**</span></span>

<span data-ttu-id="aef5f-190">若要使用 *web.config* 設定 `ASPNETCORE_ENVIRONMENT` 環境變數，請參閱 <xref:host-and-deploy/aspnet-core-module#setting-environment-variables> 的＜設定環境變數＞一節。</span><span class="sxs-lookup"><span data-stu-id="aef5f-190">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="aef5f-191">**專案檔或發行設定檔**</span><span class="sxs-lookup"><span data-stu-id="aef5f-191">**Project file or publish profile**</span></span>

<span data-ttu-id="aef5f-192">**針對 WINDOWS IIS 部署：** 將 `<EnvironmentName>` 屬性包含在[發行設定檔（. .pubxml）](xref:host-and-deploy/visual-studio-publish-profiles)或專案檔中。</span><span class="sxs-lookup"><span data-stu-id="aef5f-192">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="aef5f-193">此方法會在專案發行時於 *web.config* 中設定環境：</span><span class="sxs-lookup"><span data-stu-id="aef5f-193">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="aef5f-194">**每個 IIS 應用程式集區**</span><span class="sxs-lookup"><span data-stu-id="aef5f-194">**Per IIS Application Pool**</span></span>

<span data-ttu-id="aef5f-195">若要為執行於隔離應用程式集區 (受 IIS 10.0 或更新版本支援) 的應用程式設定 `ASPNETCORE_ENVIRONMENT` 環境變數，請參閱[環境變數 &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的＜AppCmd.exe 命令＞一節。</span><span class="sxs-lookup"><span data-stu-id="aef5f-195">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="aef5f-196">當 `ASPNETCORE_ENVIRONMENT` 環境變數設定為應用程式集區時，其值會覆寫系統層級的設定。</span><span class="sxs-lookup"><span data-stu-id="aef5f-196">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="aef5f-197">當在 IIS 中裝載應用程式，並新增或變更 `ASPNETCORE_ENVIRONMENT` 環境變數時，請使用下列任一種方法，讓應用程式挑選新的值：</span><span class="sxs-lookup"><span data-stu-id="aef5f-197">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="aef5f-198">從命令提示字元執行後面接著 `net start w3svc` 的 `net stop was /y`。</span><span class="sxs-lookup"><span data-stu-id="aef5f-198">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="aef5f-199">重新啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="aef5f-199">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="aef5f-200">macOS</span><span class="sxs-lookup"><span data-stu-id="aef5f-200">macOS</span></span>

<span data-ttu-id="aef5f-201">您可以在執行應用程式時，以內嵌方式設定 macOS 目前的環境：</span><span class="sxs-lookup"><span data-stu-id="aef5f-201">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="aef5f-202">或者，在執行應用程式之前，使用 `export` 設定環境：</span><span class="sxs-lookup"><span data-stu-id="aef5f-202">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="aef5f-203">您可以在 *.bashrc* 或 *.bash_profile* 檔案中設定電腦層級的環境變數。</span><span class="sxs-lookup"><span data-stu-id="aef5f-203">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="aef5f-204">使用任何文字編輯器編輯檔案。</span><span class="sxs-lookup"><span data-stu-id="aef5f-204">Edit the file using any text editor.</span></span> <span data-ttu-id="aef5f-205">新增下列陳述式：</span><span class="sxs-lookup"><span data-stu-id="aef5f-205">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="aef5f-206">Linux</span><span class="sxs-lookup"><span data-stu-id="aef5f-206">Linux</span></span>

<span data-ttu-id="aef5f-207">針對 Linux 散發版本，請在命令提示字元使用 `export` 命令，進行以工作階段為基礎的變數設定，並使用 *bash_profile* 檔案，進行電腦層級的環境設定。</span><span class="sxs-lookup"><span data-stu-id="aef5f-207">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="aef5f-208">在程式碼中設定環境</span><span class="sxs-lookup"><span data-stu-id="aef5f-208">Set the environment in code</span></span>

<span data-ttu-id="aef5f-209">在建立主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*>。</span><span class="sxs-lookup"><span data-stu-id="aef5f-209">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="aef5f-210">請參閱<xref:fundamentals/host/generic-host#environmentname>.</span><span class="sxs-lookup"><span data-stu-id="aef5f-210">See <xref:fundamentals/host/generic-host#environmentname>.</span></span>


### <a name="configuration-by-environment"></a><span data-ttu-id="aef5f-211">取決於環境的組態</span><span class="sxs-lookup"><span data-stu-id="aef5f-211">Configuration by environment</span></span>

<span data-ttu-id="aef5f-212">若要依環境載入組態，建議使用：</span><span class="sxs-lookup"><span data-stu-id="aef5f-212">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="aef5f-213">*appsettings* files （*appsettings. {環境}. json*）。</span><span class="sxs-lookup"><span data-stu-id="aef5f-213">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="aef5f-214">請參閱<xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="aef5f-214">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="aef5f-215">環境變數（在裝載應用程式的每個系統上設定）。</span><span class="sxs-lookup"><span data-stu-id="aef5f-215">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="aef5f-216">請參閱<xref:fundamentals/host/generic-host#environmentname>和<xref:security/app-secrets#environment-variables>。</span><span class="sxs-lookup"><span data-stu-id="aef5f-216">See <xref:fundamentals/host/generic-host#environmentname> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="aef5f-217">祕密管理員 (僅限開發環境)。</span><span class="sxs-lookup"><span data-stu-id="aef5f-217">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="aef5f-218">請參閱<xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="aef5f-218">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="aef5f-219">以環境為基礎的 Startup 類別和方法</span><span class="sxs-lookup"><span data-stu-id="aef5f-219">Environment-based Startup class and methods</span></span>

### <a name="inject-iwebhostenvironment-into-startupconfigure"></a><span data-ttu-id="aef5f-220">將 IWebHostEnvironment 插入啟動。設定</span><span class="sxs-lookup"><span data-stu-id="aef5f-220">Inject IWebHostEnvironment into Startup.Configure</span></span>

<span data-ttu-id="aef5f-221">將 <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> 插入 `Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="aef5f-221">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="aef5f-222">當應用程式只需要調整幾個環境的 `Startup.Configure`，且每個環境的程式碼差異最低時，此方法就很有用。</span><span class="sxs-lookup"><span data-stu-id="aef5f-222">This approach is useful when the app only requires adjusting `Startup.Configure` for a few environments with minimal code differences per environment.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Development environment code
    }
    else
    {
        // Code for all other environments
    }
}
```

### <a name="inject-iwebhostenvironment-into-the-startup-class"></a><span data-ttu-id="aef5f-223">將 IWebHostEnvironment 插入 Startup 類別</span><span class="sxs-lookup"><span data-stu-id="aef5f-223">Inject IWebHostEnvironment into the Startup class</span></span>

<span data-ttu-id="aef5f-224">將 <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> 插入 `Startup` 的函式。</span><span class="sxs-lookup"><span data-stu-id="aef5f-224">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into the `Startup` constructor.</span></span> <span data-ttu-id="aef5f-225">當應用程式需要針對少數環境設定 `Startup`，但每個環境的程式碼差異最低時，此方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="aef5f-225">This approach is useful when the app requires configuring `Startup` for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="aef5f-226">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="aef5f-226">In the following example:</span></span>

* <span data-ttu-id="aef5f-227">環境會保留在 [`_env`] 欄位中。</span><span class="sxs-lookup"><span data-stu-id="aef5f-227">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="aef5f-228">`_env` 用於 `ConfigureServices` 和 `Configure`，以根據應用程式的環境套用啟動設定。</span><span class="sxs-lookup"><span data-stu-id="aef5f-228">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

```csharp
public class Startup
{
    private readonly IWebHostEnvironment _env;

    public Startup(IWebHostEnvironment env)
    {
        _env = env;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else if (_env.IsStaging())
        {
            // Staging environment code
        }
        else
        {
            // Code for all other environments
        }
    }

    public void Configure(IApplicationBuilder app)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else
        {
            // Code for all other environments
        }
    }
}
```
### <a name="startup-class-conventions"></a><span data-ttu-id="aef5f-229">Startup 類別慣例</span><span class="sxs-lookup"><span data-stu-id="aef5f-229">Startup class conventions</span></span>

<span data-ttu-id="aef5f-230">ASP.NET Core 應用程式啟動時，[Startup 類別](xref:fundamentals/startup)會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="aef5f-230">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="aef5f-231">應用程式可以針對不同的環境定義個別的 `Startup` 類別（例如，`StartupDevelopment`）。</span><span class="sxs-lookup"><span data-stu-id="aef5f-231">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="aef5f-232">在執行時間選取適當的 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="aef5f-232">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="aef5f-233">將優先使用其名稱尾碼符合目前環境的類別。</span><span class="sxs-lookup"><span data-stu-id="aef5f-233">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="aef5f-234">如果找不到相符的 `Startup{EnvironmentName}` 類別，會使用 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="aef5f-234">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="aef5f-235">當應用程式需要針對數個環境設定啟動，且每個環境有許多程式碼差異時，此方法很有用。</span><span class="sxs-lookup"><span data-stu-id="aef5f-235">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="aef5f-236">若要實作以環境為基礎的 `Startup` 類別，請為每個使用中的環境建立 `Startup{EnvironmentName}` 類別和後援 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="aef5f-236">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}
```

<span data-ttu-id="aef5f-237">使用接受組件名稱的 [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) 多載：</span><span class="sxs-lookup"><span data-stu-id="aef5f-237">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

    return WebHost.CreateDefaultBuilder(args)
        .UseStartup(assemblyName);
}
```

### <a name="startup-method-conventions"></a><span data-ttu-id="aef5f-238">Startup 方法慣例</span><span class="sxs-lookup"><span data-stu-id="aef5f-238">Startup method conventions</span></span>

<span data-ttu-id="aef5f-239">[設定](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure)和[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices)支援表單 `Configure<EnvironmentName>` 和 `Configure<EnvironmentName>Services`的環境特定版本。</span><span class="sxs-lookup"><span data-stu-id="aef5f-239">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="aef5f-240">當應用程式需要針對數個環境設定啟動，且每個環境有許多程式碼差異時，此方法很有用。</span><span class="sxs-lookup"><span data-stu-id="aef5f-240">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="aef5f-241">其他資源</span><span class="sxs-lookup"><span data-stu-id="aef5f-241">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="aef5f-242">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="aef5f-242">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="aef5f-243">ASP.NET Core 會使用環境變數根據執行階段環境來設定應用程式行為。</span><span class="sxs-lookup"><span data-stu-id="aef5f-243">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="aef5f-244">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="aef5f-244">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="aef5f-245">環境</span><span class="sxs-lookup"><span data-stu-id="aef5f-245">Environments</span></span>

<span data-ttu-id="aef5f-246">ASP.NET Core 會在應用程式啟動時讀取 `ASPNETCORE_ENVIRONMENT` 環境變數，並將該值儲存在 [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName) 中。</span><span class="sxs-lookup"><span data-stu-id="aef5f-246">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span></span> <span data-ttu-id="aef5f-247">`ASPNETCORE_ENVIRONMENT` 可以設定為任何值，但架構會提供三個值：</span><span class="sxs-lookup"><span data-stu-id="aef5f-247">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Staging>
* <span data-ttu-id="aef5f-248"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (預設值)</span><span class="sxs-lookup"><span data-stu-id="aef5f-248"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="aef5f-249">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="aef5f-249">The preceding code:</span></span>

* <span data-ttu-id="aef5f-250">當 `ASPNETCORE_ENVIRONMENT` 設為 `Development` 時，會呼叫 [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage)。</span><span class="sxs-lookup"><span data-stu-id="aef5f-250">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="aef5f-251">當 `ASPNETCORE_ENVIRONMENT` 的值設為下列其中之一時，會呼叫 [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler)：</span><span class="sxs-lookup"><span data-stu-id="aef5f-251">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="aef5f-252">[環境標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)會使用 `IHostingEnvironment.EnvironmentName` 的值來包含或排除項目中的標籤：</span><span class="sxs-lookup"><span data-stu-id="aef5f-252">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="aef5f-253">在 Windows 及 macOS 中，環境變數和值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="aef5f-253">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="aef5f-254">Linux 的環境變數和值則預設為**區分大小寫**。</span><span class="sxs-lookup"><span data-stu-id="aef5f-254">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="aef5f-255">開發</span><span class="sxs-lookup"><span data-stu-id="aef5f-255">Development</span></span>

<span data-ttu-id="aef5f-256">您可以在開發環境中啟用生產環境不應該公開的功能。</span><span class="sxs-lookup"><span data-stu-id="aef5f-256">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="aef5f-257">例如，ASP.NET Core 範本會在開發環境中啟用[開發人員例外狀況頁面](xref:fundamentals/error-handling#developer-exception-page)。</span><span class="sxs-lookup"><span data-stu-id="aef5f-257">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="aef5f-258">您可以在專案的 *Properties\launchSettings.json* 檔案設定適用於本機電腦開發的環境。</span><span class="sxs-lookup"><span data-stu-id="aef5f-258">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="aef5f-259">在 *launchSettings.json* 中設定的環境值會覆寫系統環境的設定值。</span><span class="sxs-lookup"><span data-stu-id="aef5f-259">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="aef5f-260">下列 JSON 顯示來自 *launchSettings.json* 檔案的三個設定檔：</span><span class="sxs-lookup"><span data-stu-id="aef5f-260">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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

> [!NOTE]
> <span data-ttu-id="aef5f-261">*launchSettings.json* 中的 `applicationUrl` 屬性可以指定伺服器 URL 的清單。</span><span class="sxs-lookup"><span data-stu-id="aef5f-261">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="aef5f-262">請在清單的 URL 之間使用分號：</span><span class="sxs-lookup"><span data-stu-id="aef5f-262">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="aef5f-263">以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動應用程式時，會使用具有 `"commandName": "Project"` 的第一個設定檔。</span><span class="sxs-lookup"><span data-stu-id="aef5f-263">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="aef5f-264">`commandName` 的值可指定要啟動的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="aef5f-264">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="aef5f-265">`commandName` 可以是下列任何一個項目：</span><span class="sxs-lookup"><span data-stu-id="aef5f-265">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="aef5f-266">`Project` (這會啟動 Kestrel)</span><span class="sxs-lookup"><span data-stu-id="aef5f-266">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="aef5f-267">以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動應用程式時：</span><span class="sxs-lookup"><span data-stu-id="aef5f-267">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="aef5f-268">會讀取 *launchSettings.json* (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="aef5f-268">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="aef5f-269">*launchSettings.json* 中的 `environmentVariables` 設定會覆寫環境變數。</span><span class="sxs-lookup"><span data-stu-id="aef5f-269">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="aef5f-270">主控環境隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="aef5f-270">The hosting environment is displayed.</span></span>

<span data-ttu-id="aef5f-271">下列輸出顯示應用程式是以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動：</span><span class="sxs-lookup"><span data-stu-id="aef5f-271">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="aef5f-272">Visual Studio 專案屬性的 [偵錯] 索引標籤提供 GUI，可編輯 *launchSettings.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="aef5f-272">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![專案屬性設定環境變數](environments/_static/project-properties-debug.png)

<span data-ttu-id="aef5f-274">您對專案設定檔所做的變更可能要等重新啟動網頁伺服器後才會生效。</span><span class="sxs-lookup"><span data-stu-id="aef5f-274">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="aef5f-275">您必須重新啟動 Kestrel，它才會偵測到環境已進行的變更。</span><span class="sxs-lookup"><span data-stu-id="aef5f-275">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="aef5f-276">*launchSettings.json* 不應儲存密碼。</span><span class="sxs-lookup"><span data-stu-id="aef5f-276">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="aef5f-277">[密碼管理員工具](xref:security/app-secrets)可以用來儲存本機開發的密碼。</span><span class="sxs-lookup"><span data-stu-id="aef5f-277">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="aef5f-278">使用 [Visual Studio Code](https://code.visualstudio.com/) 時，可以會在 *.vscode/launch.json* 檔案設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="aef5f-278">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="aef5f-279">下列範例會將環境設定為 `Development`：</span><span class="sxs-lookup"><span data-stu-id="aef5f-279">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="aef5f-280">使用 `dotnet run` 啟動應用程式時，不會如同 *Properties/launchSettings.json* 一樣讀取專案中的 *.vscode/launch.json* 檔。</span><span class="sxs-lookup"><span data-stu-id="aef5f-280">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="aef5f-281">開發時若啟動的應用程式沒有 *launchSettings.json* 檔，請使用環境變數設定環境，或是使用 `dotnet run` 命令的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="aef5f-281">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="aef5f-282">生產環境</span><span class="sxs-lookup"><span data-stu-id="aef5f-282">Production</span></span>

<span data-ttu-id="aef5f-283">若要將安全性、效能及應用程式加強性最大化，您應該設定生產環境。</span><span class="sxs-lookup"><span data-stu-id="aef5f-283">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="aef5f-284">不同於開發的某些一般設定包括：</span><span class="sxs-lookup"><span data-stu-id="aef5f-284">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="aef5f-285">快取。</span><span class="sxs-lookup"><span data-stu-id="aef5f-285">Caching.</span></span>
* <span data-ttu-id="aef5f-286">用戶端的資源會經過配套、縮減，且可能由 CDN 提供。</span><span class="sxs-lookup"><span data-stu-id="aef5f-286">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="aef5f-287">停用診斷錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="aef5f-287">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="aef5f-288">啟用易懂的錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="aef5f-288">Friendly error pages enabled.</span></span>
* <span data-ttu-id="aef5f-289">啟用生產記錄與監視。</span><span class="sxs-lookup"><span data-stu-id="aef5f-289">Production logging and monitoring enabled.</span></span> <span data-ttu-id="aef5f-290">例如 [Application Insights](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="aef5f-290">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="aef5f-291">設定環境</span><span class="sxs-lookup"><span data-stu-id="aef5f-291">Set the environment</span></span>

<span data-ttu-id="aef5f-292">設定特定環境以使用環境變數或平臺設定進行測試時，通常會很有用。</span><span class="sxs-lookup"><span data-stu-id="aef5f-292">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="aef5f-293">如果您未設定環境，它會預設為 `Production` 並停用大部分的偵錯功能。</span><span class="sxs-lookup"><span data-stu-id="aef5f-293">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="aef5f-294">環境的設定方法取決於作業系統而定。</span><span class="sxs-lookup"><span data-stu-id="aef5f-294">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="aef5f-295">建立主機時，應用程式所讀取的最後一個環境設定會決定應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="aef5f-295">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="aef5f-296">應用程式正在執行時，無法變更應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="aef5f-296">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="aef5f-297">環境變數或平臺設定</span><span class="sxs-lookup"><span data-stu-id="aef5f-297">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="aef5f-298">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="aef5f-298">Azure App Service</span></span>

<span data-ttu-id="aef5f-299">若要在 [Azure App Service](https://azure.microsoft.com/services/app-service/) 設定環境，請執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="aef5f-299">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="aef5f-300">從 [應用程式服務] 刀鋒視窗選取應用程式。</span><span class="sxs-lookup"><span data-stu-id="aef5f-300">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="aef5f-301">在 [設定] 群組中，選取 [應用程式設定] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="aef5f-301">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="aef5f-302">在 [應用程式設定] 區域中，選取 [新增設定]。</span><span class="sxs-lookup"><span data-stu-id="aef5f-302">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="aef5f-303">針對 [輸入名稱]，提供 `ASPNETCORE_ENVIRONMENT`。</span><span class="sxs-lookup"><span data-stu-id="aef5f-303">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="aef5f-304">針對 [輸入值]，提供環境 (例如 `Staging`)。</span><span class="sxs-lookup"><span data-stu-id="aef5f-304">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="aef5f-305">如果您想要在交換部署位置時，環境設定保留目前的位置，請選取 [位置設定] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="aef5f-305">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="aef5f-306">如需詳細資訊，請參閱 [Azure 文件：交換哪些設定？](/azure/app-service/web-sites-staged-publishing)。</span><span class="sxs-lookup"><span data-stu-id="aef5f-306">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="aef5f-307">選取刀鋒視窗頂端的 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="aef5f-307">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="aef5f-308">Azure App Service 會在新增、變更或刪除 Azure 入口網站的應用程式設定 (環境變數) 之後自動重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="aef5f-308">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="aef5f-309">Windows</span><span class="sxs-lookup"><span data-stu-id="aef5f-309">Windows</span></span>

<span data-ttu-id="aef5f-310">如果應用程式是使用 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動，請使用下列命令來設定目前工作階段的 `ASPNETCORE_ENVIRONMENT`：</span><span class="sxs-lookup"><span data-stu-id="aef5f-310">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="aef5f-311">**命令提示字元**</span><span class="sxs-lookup"><span data-stu-id="aef5f-311">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="aef5f-312">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="aef5f-312">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="aef5f-313">這些命令僅針對目前的視窗才會生效。</span><span class="sxs-lookup"><span data-stu-id="aef5f-313">These commands only take effect for the current window.</span></span> <span data-ttu-id="aef5f-314">視窗關閉時，`ASPNETCORE_ENVIRONMENT` 設定會還原為預設值或電腦值。</span><span class="sxs-lookup"><span data-stu-id="aef5f-314">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="aef5f-315">若要在 Windows 中以全域的方式設定值，請使用下列其中一個方法：</span><span class="sxs-lookup"><span data-stu-id="aef5f-315">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="aef5f-316">開啟 [**控制台**] >**系統**> [ **Advanced system settings** ]，然後新增或編輯 `ASPNETCORE_ENVIRONMENT` 值：</span><span class="sxs-lookup"><span data-stu-id="aef5f-316">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![系統進階屬性](environments/_static/systemsetting_environment.png)

  ![ASPNET Core 環境變數](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="aef5f-319">開啟系統管理命令提示字元，然後使用 `setx` 命令，或開啟系統管理 PowerShell 命令提示字元，然後使用 `[Environment]::SetEnvironmentVariable`：</span><span class="sxs-lookup"><span data-stu-id="aef5f-319">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="aef5f-320">**命令提示字元**</span><span class="sxs-lookup"><span data-stu-id="aef5f-320">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="aef5f-321">`/M` 參數表示將環境變數設定在系統層級。</span><span class="sxs-lookup"><span data-stu-id="aef5f-321">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="aef5f-322">若未使用 `/M` 參數，則將環境變數設定為使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="aef5f-322">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="aef5f-323">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="aef5f-323">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="aef5f-324">`Machine` 選項值表示將環境變數設定在系統層級。</span><span class="sxs-lookup"><span data-stu-id="aef5f-324">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="aef5f-325">若選項值變更為 `User`，則將環境變數設定為使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="aef5f-325">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="aef5f-326">當以全域的方式設定 `ASPNETCORE_ENVIRONMENT` 環境變數時，則在設定該值後開啟的任何命令視窗中，對 `dotnet run` 均有效。</span><span class="sxs-lookup"><span data-stu-id="aef5f-326">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="aef5f-327">**web.config**</span><span class="sxs-lookup"><span data-stu-id="aef5f-327">**web.config**</span></span>

<span data-ttu-id="aef5f-328">若要使用 *web.config* 設定 `ASPNETCORE_ENVIRONMENT` 環境變數，請參閱 <xref:host-and-deploy/aspnet-core-module#setting-environment-variables> 的＜設定環境變數＞一節。</span><span class="sxs-lookup"><span data-stu-id="aef5f-328">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="aef5f-329">**專案檔或發行設定檔**</span><span class="sxs-lookup"><span data-stu-id="aef5f-329">**Project file or publish profile**</span></span>

<span data-ttu-id="aef5f-330">**針對 WINDOWS IIS 部署：** 將 `<EnvironmentName>` 屬性包含在[發行設定檔（. .pubxml）](xref:host-and-deploy/visual-studio-publish-profiles)或專案檔中。</span><span class="sxs-lookup"><span data-stu-id="aef5f-330">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="aef5f-331">此方法會在專案發行時於 *web.config* 中設定環境：</span><span class="sxs-lookup"><span data-stu-id="aef5f-331">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="aef5f-332">**每個 IIS 應用程式集區**</span><span class="sxs-lookup"><span data-stu-id="aef5f-332">**Per IIS Application Pool**</span></span>

<span data-ttu-id="aef5f-333">若要為執行於隔離應用程式集區 (受 IIS 10.0 或更新版本支援) 的應用程式設定 `ASPNETCORE_ENVIRONMENT` 環境變數，請參閱[環境變數 &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的＜AppCmd.exe 命令＞一節。</span><span class="sxs-lookup"><span data-stu-id="aef5f-333">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="aef5f-334">當 `ASPNETCORE_ENVIRONMENT` 環境變數設定為應用程式集區時，其值會覆寫系統層級的設定。</span><span class="sxs-lookup"><span data-stu-id="aef5f-334">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="aef5f-335">當在 IIS 中裝載應用程式，並新增或變更 `ASPNETCORE_ENVIRONMENT` 環境變數時，請使用下列任一種方法，讓應用程式挑選新的值：</span><span class="sxs-lookup"><span data-stu-id="aef5f-335">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="aef5f-336">從命令提示字元執行後面接著 `net start w3svc` 的 `net stop was /y`。</span><span class="sxs-lookup"><span data-stu-id="aef5f-336">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="aef5f-337">重新啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="aef5f-337">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="aef5f-338">macOS</span><span class="sxs-lookup"><span data-stu-id="aef5f-338">macOS</span></span>

<span data-ttu-id="aef5f-339">您可以在執行應用程式時，以內嵌方式設定 macOS 目前的環境：</span><span class="sxs-lookup"><span data-stu-id="aef5f-339">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="aef5f-340">或者，在執行應用程式之前，使用 `export` 設定環境：</span><span class="sxs-lookup"><span data-stu-id="aef5f-340">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="aef5f-341">您可以在 *.bashrc* 或 *.bash_profile* 檔案中設定電腦層級的環境變數。</span><span class="sxs-lookup"><span data-stu-id="aef5f-341">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="aef5f-342">使用任何文字編輯器編輯檔案。</span><span class="sxs-lookup"><span data-stu-id="aef5f-342">Edit the file using any text editor.</span></span> <span data-ttu-id="aef5f-343">新增下列陳述式：</span><span class="sxs-lookup"><span data-stu-id="aef5f-343">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="aef5f-344">Linux</span><span class="sxs-lookup"><span data-stu-id="aef5f-344">Linux</span></span>

<span data-ttu-id="aef5f-345">針對 Linux 散發版本，請在命令提示字元使用 `export` 命令，進行以工作階段為基礎的變數設定，並使用 *bash_profile* 檔案，進行電腦層級的環境設定。</span><span class="sxs-lookup"><span data-stu-id="aef5f-345">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="aef5f-346">在程式碼中設定環境</span><span class="sxs-lookup"><span data-stu-id="aef5f-346">Set the environment in code</span></span>

<span data-ttu-id="aef5f-347">在建立主機時呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*>。</span><span class="sxs-lookup"><span data-stu-id="aef5f-347">Call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="aef5f-348">請參閱<xref:fundamentals/host/web-host#environment>.</span><span class="sxs-lookup"><span data-stu-id="aef5f-348">See <xref:fundamentals/host/web-host#environment>.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="aef5f-349">取決於環境的組態</span><span class="sxs-lookup"><span data-stu-id="aef5f-349">Configuration by environment</span></span>

<span data-ttu-id="aef5f-350">若要依環境載入組態，建議使用：</span><span class="sxs-lookup"><span data-stu-id="aef5f-350">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="aef5f-351">*appsettings* files （*appsettings. {環境}. json*）。</span><span class="sxs-lookup"><span data-stu-id="aef5f-351">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="aef5f-352">請參閱<xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="aef5f-352">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="aef5f-353">環境變數（在裝載應用程式的每個系統上設定）。</span><span class="sxs-lookup"><span data-stu-id="aef5f-353">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="aef5f-354">請參閱<xref:fundamentals/host/web-host#environment>和<xref:security/app-secrets#environment-variables>。</span><span class="sxs-lookup"><span data-stu-id="aef5f-354">See <xref:fundamentals/host/web-host#environment> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="aef5f-355">祕密管理員 (僅限開發環境)。</span><span class="sxs-lookup"><span data-stu-id="aef5f-355">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="aef5f-356">請參閱<xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="aef5f-356">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="aef5f-357">以環境為基礎的 Startup 類別和方法</span><span class="sxs-lookup"><span data-stu-id="aef5f-357">Environment-based Startup class and methods</span></span>

### <a name="inject-ihostingenvironment-into-startupconfigure"></a><span data-ttu-id="aef5f-358">將 IHostingEnvironment 插入啟動。設定</span><span class="sxs-lookup"><span data-stu-id="aef5f-358">Inject IHostingEnvironment into Startup.Configure</span></span>

<span data-ttu-id="aef5f-359">將 <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> 插入 `Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="aef5f-359">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="aef5f-360">當應用程式只需要針對少數環境設定 `Startup.Configure`，且每個環境的程式碼差異最低時，這個方法就很有用。</span><span class="sxs-lookup"><span data-stu-id="aef5f-360">This approach is useful when the app only requires configuring `Startup.Configure` for only a few environments with minimal code differences per environment.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Development environment code
    }
    else
    {
        // Code for all other environments
    }
}
```

### <a name="inject-ihostingenvironment-into-the-startup-class"></a><span data-ttu-id="aef5f-361">將 IHostingEnvironment 插入 Startup 類別</span><span class="sxs-lookup"><span data-stu-id="aef5f-361">Inject IHostingEnvironment into the Startup class</span></span>

<span data-ttu-id="aef5f-362">將 <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> 插入 `Startup` 的函式，並將服務指派給欄位，以供整個 `Startup` 類別使用。</span><span class="sxs-lookup"><span data-stu-id="aef5f-362">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into the `Startup` constructor and assign the service to a field for use throughout the `Startup` class.</span></span> <span data-ttu-id="aef5f-363">當應用程式需要針對少數環境設定啟動，但每個環境的程式碼差異最低時，這個方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="aef5f-363">This approach is useful when the app requires configuring startup for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="aef5f-364">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="aef5f-364">In the following example:</span></span>

* <span data-ttu-id="aef5f-365">環境會保留在 [`_env`] 欄位中。</span><span class="sxs-lookup"><span data-stu-id="aef5f-365">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="aef5f-366">`_env` 用於 `ConfigureServices` 和 `Configure`，以根據應用程式的環境套用啟動設定。</span><span class="sxs-lookup"><span data-stu-id="aef5f-366">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

```csharp
public class Startup
{
    private readonly IHostingEnvironment _env;

    public Startup(IHostingEnvironment env)
    {
        _env = env;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else if (_env.IsStaging())
        {
            // Staging environment code
        }
        else
        {
            // Code for all other environments
        }
    }

    public void Configure(IApplicationBuilder app)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else
        {
            // Code for all other environments
        }
    }
}
```

### <a name="startup-class-conventions"></a><span data-ttu-id="aef5f-367">Startup 類別慣例</span><span class="sxs-lookup"><span data-stu-id="aef5f-367">Startup class conventions</span></span>

<span data-ttu-id="aef5f-368">ASP.NET Core 應用程式啟動時，[Startup 類別](xref:fundamentals/startup)會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="aef5f-368">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="aef5f-369">應用程式可以針對不同的環境定義個別的 `Startup` 類別（例如，`StartupDevelopment`）。</span><span class="sxs-lookup"><span data-stu-id="aef5f-369">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="aef5f-370">在執行時間選取適當的 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="aef5f-370">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="aef5f-371">將優先使用其名稱尾碼符合目前環境的類別。</span><span class="sxs-lookup"><span data-stu-id="aef5f-371">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="aef5f-372">如果找不到相符的 `Startup{EnvironmentName}` 類別，會使用 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="aef5f-372">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="aef5f-373">當應用程式需要針對數個環境設定啟動，且每個環境有許多程式碼差異時，此方法很有用。</span><span class="sxs-lookup"><span data-stu-id="aef5f-373">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="aef5f-374">若要實作以環境為基礎的 `Startup` 類別，請為每個使用中的環境建立 `Startup{EnvironmentName}` 類別和後援 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="aef5f-374">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}
```

<span data-ttu-id="aef5f-375">使用接受組件名稱的 [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) 多載：</span><span class="sxs-lookup"><span data-stu-id="aef5f-375">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

    return WebHost.CreateDefaultBuilder(args)
        .UseStartup(assemblyName);
}
```

### <a name="startup-method-conventions"></a><span data-ttu-id="aef5f-376">Startup 方法慣例</span><span class="sxs-lookup"><span data-stu-id="aef5f-376">Startup method conventions</span></span>

<span data-ttu-id="aef5f-377">[設定](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure)和[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices)支援表單 `Configure<EnvironmentName>` 和 `Configure<EnvironmentName>Services`的環境特定版本。</span><span class="sxs-lookup"><span data-stu-id="aef5f-377">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="aef5f-378">當應用程式需要針對數個環境設定啟動，且每個環境有許多程式碼差異時，此方法很有用。</span><span class="sxs-lookup"><span data-stu-id="aef5f-378">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="aef5f-379">其他資源</span><span class="sxs-lookup"><span data-stu-id="aef5f-379">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>

::: moniker-end