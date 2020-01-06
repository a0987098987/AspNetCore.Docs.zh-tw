---
title: 在 ASP.NET Core 中使用多個環境
author: rick-anderson
description: 了解在 ASP.NET Core 應用程式中如何跨多個環境控制應用程式的行為。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/17/2019
uid: fundamentals/environments
ms.openlocfilehash: 30e2771c0a24fcbf6490d08c7028566314b6c011
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/25/2019
ms.locfileid: "75358717"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="0ea7a-103">在 ASP.NET Core 中使用多個環境</span><span class="sxs-lookup"><span data-stu-id="0ea7a-103">Use multiple environments in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0ea7a-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0ea7a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0ea7a-105">ASP.NET Core 會使用環境變數根據執行階段環境來設定應用程式行為。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="0ea7a-106">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0ea7a-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="0ea7a-107">環境</span><span class="sxs-lookup"><span data-stu-id="0ea7a-107">Environments</span></span>

<span data-ttu-id="0ea7a-108">ASP.NET Core 會在應用程式啟動時讀取環境變數 `ASPNETCORE_ENVIRONMENT`，並將值儲存在[IWebHostEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName)中。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IWebHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span></span> <span data-ttu-id="0ea7a-109">`ASPNETCORE_ENVIRONMENT` 可以設定為任何值，但架構會提供三個值：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.Extensions.Hosting.Environments.Development>
* <xref:Microsoft.Extensions.Hosting.Environments.Staging>
* <span data-ttu-id="0ea7a-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (預設值)</span><span class="sxs-lookup"><span data-stu-id="0ea7a-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="0ea7a-111">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-111">The preceding code:</span></span>

* <span data-ttu-id="0ea7a-112">當 `ASPNETCORE_ENVIRONMENT` 設為 `Development` 時，會呼叫 [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage)。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="0ea7a-113">當 `ASPNETCORE_ENVIRONMENT` 的值設為下列其中之一時，會呼叫 [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler)：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="0ea7a-114">[環境標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)會使用 `IHostingEnvironment.EnvironmentName` 的值來包含或排除項目中的標籤：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="0ea7a-115">在 Windows 及 macOS 中，環境變數和值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="0ea7a-116">Linux 的環境變數和值則預設為**區分大小寫**。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="0ea7a-117">開發</span><span class="sxs-lookup"><span data-stu-id="0ea7a-117">Development</span></span>

<span data-ttu-id="0ea7a-118">您可以在開發環境中啟用生產環境不應該公開的功能。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="0ea7a-119">例如，ASP.NET Core 範本會在開發環境中啟用[開發人員例外狀況頁面](xref:fundamentals/error-handling#developer-exception-page)。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-119">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="0ea7a-120">您可以在專案的 *Properties\launchSettings.json* 檔案設定適用於本機電腦開發的環境。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="0ea7a-121">在 *launchSettings.json* 中設定的環境值會覆寫系統環境的設定值。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="0ea7a-122">下列 JSON 顯示來自 *launchSettings.json* 檔案的三個設定檔：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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
> <span data-ttu-id="0ea7a-123">*launchSettings.json* 中的 `applicationUrl` 屬性可以指定伺服器 URL 的清單。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="0ea7a-124">請在清單的 URL 之間使用分號：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-124">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="0ea7a-125">以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動應用程式時，會使用具有 `"commandName": "Project"` 的第一個設定檔。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="0ea7a-126">`commandName` 的值可指定要啟動的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="0ea7a-127">`commandName` 可以是下列任何一個項目：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-127">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="0ea7a-128">`Project` (這會啟動 Kestrel)</span><span class="sxs-lookup"><span data-stu-id="0ea7a-128">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="0ea7a-129">以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動應用程式時：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-129">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="0ea7a-130">會讀取 *launchSettings.json* (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="0ea7a-131">*launchSettings.json* 中的 `environmentVariables` 設定會覆寫環境變數。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="0ea7a-132">主控環境隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-132">The hosting environment is displayed.</span></span>

<span data-ttu-id="0ea7a-133">下列輸出顯示應用程式是以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-133">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="0ea7a-134">Visual Studio 專案屬性的 [偵錯] 索引標籤提供 GUI，可編輯 *launchSettings.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-134">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![專案屬性設定環境變數](environments/_static/project-properties-debug.png)

<span data-ttu-id="0ea7a-136">您對專案設定檔所做的變更可能要等重新啟動網頁伺服器後才會生效。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="0ea7a-137">您必須重新啟動 Kestrel，它才會偵測到環境已進行的變更。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-137">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="0ea7a-138">*launchSettings.json* 不應儲存密碼。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-138">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="0ea7a-139">[密碼管理員工具](xref:security/app-secrets)可以用來儲存本機開發的密碼。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="0ea7a-140">使用 [Visual Studio Code](https://code.visualstudio.com/) 時，可以會在 *.vscode/launch.json* 檔案設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-140">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="0ea7a-141">下列範例會將環境設定為 `Development`：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-141">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="0ea7a-142">使用 `dotnet run` 啟動應用程式時，不會如同 *Properties/launchSettings.json* 一樣讀取專案中的 *.vscode/launch.json* 檔。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-142">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="0ea7a-143">開發時若啟動的應用程式沒有 *launchSettings.json* 檔，請使用環境變數設定環境，或是使用 `dotnet run` 命令的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-143">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="0ea7a-144">生產環境</span><span class="sxs-lookup"><span data-stu-id="0ea7a-144">Production</span></span>

<span data-ttu-id="0ea7a-145">若要將安全性、效能及應用程式加強性最大化，您應該設定生產環境。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-145">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="0ea7a-146">不同於開發的某些一般設定包括：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-146">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="0ea7a-147">快取。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-147">Caching.</span></span>
* <span data-ttu-id="0ea7a-148">用戶端的資源會經過配套、縮減，且可能由 CDN 提供。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-148">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="0ea7a-149">停用診斷錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-149">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="0ea7a-150">啟用易懂的錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-150">Friendly error pages enabled.</span></span>
* <span data-ttu-id="0ea7a-151">啟用生產記錄與監視。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-151">Production logging and monitoring enabled.</span></span> <span data-ttu-id="0ea7a-152">例如 [Application Insights](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-152">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="0ea7a-153">設定環境</span><span class="sxs-lookup"><span data-stu-id="0ea7a-153">Set the environment</span></span>

<span data-ttu-id="0ea7a-154">設定特定環境以使用環境變數或平臺設定進行測試時，通常會很有用。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-154">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="0ea7a-155">如果您未設定環境，它會預設為 `Production` 並停用大部分的偵錯功能。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-155">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="0ea7a-156">環境的設定方法取決於作業系統而定。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-156">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="0ea7a-157">建立主機時，應用程式所讀取的最後一個環境設定會決定應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-157">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="0ea7a-158">應用程式正在執行時，無法變更應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-158">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="0ea7a-159">環境變數或平臺設定</span><span class="sxs-lookup"><span data-stu-id="0ea7a-159">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="0ea7a-160">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0ea7a-160">Azure App Service</span></span>

<span data-ttu-id="0ea7a-161">若要在 [Azure App Service](https://azure.microsoft.com/services/app-service/) 設定環境，請執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-161">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="0ea7a-162">從 [應用程式服務] 刀鋒視窗選取應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-162">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="0ea7a-163">在 [**設定**] 群組中，**選取 [** 設定] 分頁。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-163">In the **Settings** group, select the **Configuration** blade.</span></span>
1. <span data-ttu-id="0ea7a-164">在 [**應用程式設定**] 索引標籤中，選取 [**新增應用程式設定**]。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-164">In the **Application settings** tab, select **New application setting**.</span></span>
1. <span data-ttu-id="0ea7a-165">在 [**新增/編輯應用程式設定**] 視窗中，提供**名稱**的 `ASPNETCORE_ENVIRONMENT`。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-165">In the **Add/Edit application setting** window, provide `ASPNETCORE_ENVIRONMENT` for the **Name**.</span></span> <span data-ttu-id="0ea7a-166">針對 [**值**]，提供環境（例如 `Staging`）。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-166">For **Value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="0ea7a-167">如果您想要在交換部署位置時，將環境設定維持在目前的位置，請選取 [**部署位置設定**] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-167">Select the **Deployment slot setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="0ea7a-168">如需詳細資訊，請參閱 Azure 檔[中的在 Azure App Service 中設定預備環境](/azure/app-service/web-sites-staged-publishing)。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-168">For more information, see [Set up staging environments in Azure App Service](/azure/app-service/web-sites-staged-publishing) in the Azure documentation.</span></span>
1. <span data-ttu-id="0ea7a-169">選取 **[確定]** 以關閉 [**新增/編輯應用程式設定**] 視窗。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-169">Select **OK** to close the **Add/Edit application setting** window.</span></span>
1. <span data-ttu-id="0ea7a-170">選取 [設定] 分頁頂端的 **[** **儲存**]。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-170">Select **Save** at the top of the **Configuration** blade.</span></span>

<span data-ttu-id="0ea7a-171">Azure App Service 會在新增、變更或刪除 Azure 入口網站的應用程式設定 (環境變數) 之後自動重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-171">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="0ea7a-172">Windows</span><span class="sxs-lookup"><span data-stu-id="0ea7a-172">Windows</span></span>

<span data-ttu-id="0ea7a-173">如果應用程式是使用 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動，請使用下列命令來設定目前工作階段的 `ASPNETCORE_ENVIRONMENT`：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-173">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="0ea7a-174">**命令提示字元**</span><span class="sxs-lookup"><span data-stu-id="0ea7a-174">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="0ea7a-175">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="0ea7a-175">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="0ea7a-176">這些命令僅針對目前的視窗才會生效。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-176">These commands only take effect for the current window.</span></span> <span data-ttu-id="0ea7a-177">視窗關閉時，`ASPNETCORE_ENVIRONMENT` 設定會還原為預設值或電腦值。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-177">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="0ea7a-178">若要在 Windows 中以全域的方式設定值，請使用下列其中一個方法：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-178">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="0ea7a-179">開啟 [**控制台**] >**系統**> [ **Advanced system settings** ]，然後新增或編輯 `ASPNETCORE_ENVIRONMENT` 值：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-179">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![系統進階屬性](environments/_static/systemsetting_environment.png)

  ![ASPNET Core 環境變數](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="0ea7a-182">開啟系統管理命令提示字元，然後使用 `setx` 命令，或開啟系統管理 PowerShell 命令提示字元，然後使用 `[Environment]::SetEnvironmentVariable`：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-182">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="0ea7a-183">**命令提示字元**</span><span class="sxs-lookup"><span data-stu-id="0ea7a-183">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="0ea7a-184">`/M` 參數表示將環境變數設定在系統層級。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-184">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="0ea7a-185">若未使用 `/M` 參數，則將環境變數設定為使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-185">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="0ea7a-186">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="0ea7a-186">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="0ea7a-187">`Machine` 選項值表示將環境變數設定在系統層級。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-187">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="0ea7a-188">若選項值變更為 `User`，則將環境變數設定為使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-188">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="0ea7a-189">當以全域的方式設定 `ASPNETCORE_ENVIRONMENT` 環境變數時，則在設定該值後開啟的任何命令視窗中，對 `dotnet run` 均有效。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-189">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="0ea7a-190">**web.config**</span><span class="sxs-lookup"><span data-stu-id="0ea7a-190">**web.config**</span></span>

<span data-ttu-id="0ea7a-191">若要使用 *web.config* 設定 `ASPNETCORE_ENVIRONMENT` 環境變數，請參閱 <xref:host-and-deploy/aspnet-core-module#setting-environment-variables> 的＜設定環境變數＞一節。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-191">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="0ea7a-192">**專案檔或發行設定檔**</span><span class="sxs-lookup"><span data-stu-id="0ea7a-192">**Project file or publish profile**</span></span>

<span data-ttu-id="0ea7a-193">**針對 WINDOWS IIS 部署：** 將 `<EnvironmentName>` 屬性包含在[發行設定檔（. .pubxml）](xref:host-and-deploy/visual-studio-publish-profiles)或專案檔中。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-193">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="0ea7a-194">此方法會在專案發行時於 *web.config* 中設定環境：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-194">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="0ea7a-195">**每個 IIS 應用程式集區**</span><span class="sxs-lookup"><span data-stu-id="0ea7a-195">**Per IIS Application Pool**</span></span>

<span data-ttu-id="0ea7a-196">若要為執行於隔離應用程式集區 (受 IIS 10.0 或更新版本支援) 的應用程式設定 `ASPNETCORE_ENVIRONMENT` 環境變數，請參閱[環境變數 &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的＜AppCmd.exe 命令＞一節。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-196">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="0ea7a-197">當 `ASPNETCORE_ENVIRONMENT` 環境變數設定為應用程式集區時，其值會覆寫系統層級的設定。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-197">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0ea7a-198">當在 IIS 中裝載應用程式，並新增或變更 `ASPNETCORE_ENVIRONMENT` 環境變數時，請使用下列任一種方法，讓應用程式挑選新的值：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-198">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="0ea7a-199">從命令提示字元執行後面接著 `net start w3svc` 的 `net stop was /y`。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-199">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="0ea7a-200">重新啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-200">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="0ea7a-201">macOS</span><span class="sxs-lookup"><span data-stu-id="0ea7a-201">macOS</span></span>

<span data-ttu-id="0ea7a-202">您可以在執行應用程式時，以內嵌方式設定 macOS 目前的環境：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-202">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="0ea7a-203">或者，在執行應用程式之前，使用 `export` 設定環境：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-203">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="0ea7a-204">您可以在 *.bashrc* 或 *.bash_profile* 檔案中設定電腦層級的環境變數。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-204">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="0ea7a-205">使用任何文字編輯器編輯檔案。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-205">Edit the file using any text editor.</span></span> <span data-ttu-id="0ea7a-206">新增下列陳述式：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-206">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="0ea7a-207">Linux</span><span class="sxs-lookup"><span data-stu-id="0ea7a-207">Linux</span></span>

<span data-ttu-id="0ea7a-208">針對 Linux 散發版本，請在命令提示字元使用 `export` 命令，進行以工作階段為基礎的變數設定，並使用 *bash_profile* 檔案，進行電腦層級的環境設定。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-208">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="0ea7a-209">在程式碼中設定環境</span><span class="sxs-lookup"><span data-stu-id="0ea7a-209">Set the environment in code</span></span>

<span data-ttu-id="0ea7a-210">在建立主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*>。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-210">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="0ea7a-211">請參閱 <xref:fundamentals/host/generic-host#environmentname>。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-211">See <xref:fundamentals/host/generic-host#environmentname>.</span></span>


### <a name="configuration-by-environment"></a><span data-ttu-id="0ea7a-212">取決於環境的組態</span><span class="sxs-lookup"><span data-stu-id="0ea7a-212">Configuration by environment</span></span>

<span data-ttu-id="0ea7a-213">若要依環境載入組態，建議使用：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-213">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="0ea7a-214">*appsettings* files （*appsettings. {環境}. json*）。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-214">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="0ea7a-215">請參閱 <xref:fundamentals/configuration/index#json-configuration-provider>。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-215">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="0ea7a-216">環境變數（在裝載應用程式的每個系統上設定）。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-216">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="0ea7a-217">請參閱<xref:fundamentals/host/generic-host#environmentname>和<xref:security/app-secrets#environment-variables>。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-217">See <xref:fundamentals/host/generic-host#environmentname> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="0ea7a-218">祕密管理員 (僅限開發環境)。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-218">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="0ea7a-219">請參閱 <xref:security/app-secrets>。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-219">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="0ea7a-220">以環境為基礎的 Startup 類別和方法</span><span class="sxs-lookup"><span data-stu-id="0ea7a-220">Environment-based Startup class and methods</span></span>

### <a name="inject-iwebhostenvironment-into-startupconfigure"></a><span data-ttu-id="0ea7a-221">將 IWebHostEnvironment 插入啟動。設定</span><span class="sxs-lookup"><span data-stu-id="0ea7a-221">Inject IWebHostEnvironment into Startup.Configure</span></span>

<span data-ttu-id="0ea7a-222">將 <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> 插入 `Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-222">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="0ea7a-223">當應用程式只需要調整幾個環境的 `Startup.Configure`，且每個環境的程式碼差異最低時，此方法就很有用。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-223">This approach is useful when the app only requires adjusting `Startup.Configure` for a few environments with minimal code differences per environment.</span></span>

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

### <a name="inject-iwebhostenvironment-into-the-startup-class"></a><span data-ttu-id="0ea7a-224">將 IWebHostEnvironment 插入 Startup 類別</span><span class="sxs-lookup"><span data-stu-id="0ea7a-224">Inject IWebHostEnvironment into the Startup class</span></span>

<span data-ttu-id="0ea7a-225">將 <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> 插入 `Startup` 的函式。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-225">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into the `Startup` constructor.</span></span> <span data-ttu-id="0ea7a-226">當應用程式需要針對少數環境設定 `Startup`，但每個環境的程式碼差異最低時，此方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-226">This approach is useful when the app requires configuring `Startup` for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="0ea7a-227">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-227">In the following example:</span></span>

* <span data-ttu-id="0ea7a-228">環境會保留在 [`_env`] 欄位中。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-228">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="0ea7a-229">`_env` 用於 `ConfigureServices` 和 `Configure`，以根據應用程式的環境套用啟動設定。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-229">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

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
### <a name="startup-class-conventions"></a><span data-ttu-id="0ea7a-230">Startup 類別慣例</span><span class="sxs-lookup"><span data-stu-id="0ea7a-230">Startup class conventions</span></span>

<span data-ttu-id="0ea7a-231">ASP.NET Core 應用程式啟動時，[Startup 類別](xref:fundamentals/startup)會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-231">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="0ea7a-232">應用程式可以針對不同的環境定義個別的 `Startup` 類別（例如，`StartupDevelopment`）。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-232">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="0ea7a-233">在執行時間選取適當的 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-233">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="0ea7a-234">將優先使用其名稱尾碼符合目前環境的類別。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-234">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="0ea7a-235">如果找不到相符的 `Startup{EnvironmentName}` 類別，會使用 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-235">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="0ea7a-236">當應用程式需要針對數個環境設定啟動，且每個環境有許多程式碼差異時，此方法很有用。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-236">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="0ea7a-237">若要實作以環境為基礎的 `Startup` 類別，請為每個使用中的環境建立 `Startup{EnvironmentName}` 類別和後援 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-237">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

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

<span data-ttu-id="0ea7a-238">使用接受組件名稱的 [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) 多載：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-238">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

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

### <a name="startup-method-conventions"></a><span data-ttu-id="0ea7a-239">Startup 方法慣例</span><span class="sxs-lookup"><span data-stu-id="0ea7a-239">Startup method conventions</span></span>

<span data-ttu-id="0ea7a-240">[設定](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure)和[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices)支援表單 `Configure<EnvironmentName>` 和 `Configure<EnvironmentName>Services`的環境特定版本。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-240">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="0ea7a-241">當應用程式需要針對數個環境設定啟動，且每個環境有許多程式碼差異時，此方法很有用。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-241">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="0ea7a-242">其他資源</span><span class="sxs-lookup"><span data-stu-id="0ea7a-242">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0ea7a-243">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0ea7a-243">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0ea7a-244">ASP.NET Core 會使用環境變數根據執行階段環境來設定應用程式行為。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-244">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="0ea7a-245">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0ea7a-245">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="0ea7a-246">環境</span><span class="sxs-lookup"><span data-stu-id="0ea7a-246">Environments</span></span>

<span data-ttu-id="0ea7a-247">ASP.NET Core 會在應用程式啟動時讀取 `ASPNETCORE_ENVIRONMENT` 環境變數，並將該值儲存在 [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName) 中。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-247">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span></span> <span data-ttu-id="0ea7a-248">`ASPNETCORE_ENVIRONMENT` 可以設定為任何值，但架構會提供三個值：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-248">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Staging>
* <span data-ttu-id="0ea7a-249"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (預設值)</span><span class="sxs-lookup"><span data-stu-id="0ea7a-249"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="0ea7a-250">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-250">The preceding code:</span></span>

* <span data-ttu-id="0ea7a-251">當 `ASPNETCORE_ENVIRONMENT` 設為 `Development` 時，會呼叫 [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage)。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-251">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="0ea7a-252">當 `ASPNETCORE_ENVIRONMENT` 的值設為下列其中之一時，會呼叫 [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler)：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-252">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="0ea7a-253">[環境標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)會使用 `IHostingEnvironment.EnvironmentName` 的值來包含或排除項目中的標籤：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-253">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="0ea7a-254">在 Windows 及 macOS 中，環境變數和值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-254">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="0ea7a-255">Linux 的環境變數和值則預設為**區分大小寫**。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-255">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="0ea7a-256">開發</span><span class="sxs-lookup"><span data-stu-id="0ea7a-256">Development</span></span>

<span data-ttu-id="0ea7a-257">您可以在開發環境中啟用生產環境不應該公開的功能。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-257">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="0ea7a-258">例如，ASP.NET Core 範本會在開發環境中啟用[開發人員例外狀況頁面](xref:fundamentals/error-handling#developer-exception-page)。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-258">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="0ea7a-259">您可以在專案的 *Properties\launchSettings.json* 檔案設定適用於本機電腦開發的環境。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-259">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="0ea7a-260">在 *launchSettings.json* 中設定的環境值會覆寫系統環境的設定值。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-260">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="0ea7a-261">下列 JSON 顯示來自 *launchSettings.json* 檔案的三個設定檔：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-261">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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
> <span data-ttu-id="0ea7a-262">*launchSettings.json* 中的 `applicationUrl` 屬性可以指定伺服器 URL 的清單。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-262">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="0ea7a-263">請在清單的 URL 之間使用分號：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-263">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="0ea7a-264">以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動應用程式時，會使用具有 `"commandName": "Project"` 的第一個設定檔。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-264">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="0ea7a-265">`commandName` 的值可指定要啟動的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-265">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="0ea7a-266">`commandName` 可以是下列任何一個項目：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-266">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="0ea7a-267">`Project` (這會啟動 Kestrel)</span><span class="sxs-lookup"><span data-stu-id="0ea7a-267">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="0ea7a-268">以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動應用程式時：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-268">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="0ea7a-269">會讀取 *launchSettings.json* (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-269">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="0ea7a-270">*launchSettings.json* 中的 `environmentVariables` 設定會覆寫環境變數。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-270">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="0ea7a-271">主控環境隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-271">The hosting environment is displayed.</span></span>

<span data-ttu-id="0ea7a-272">下列輸出顯示應用程式是以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-272">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="0ea7a-273">Visual Studio 專案屬性的 [偵錯] 索引標籤提供 GUI，可編輯 *launchSettings.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-273">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![專案屬性設定環境變數](environments/_static/project-properties-debug.png)

<span data-ttu-id="0ea7a-275">您對專案設定檔所做的變更可能要等重新啟動網頁伺服器後才會生效。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-275">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="0ea7a-276">您必須重新啟動 Kestrel，它才會偵測到環境已進行的變更。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-276">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="0ea7a-277">*launchSettings.json* 不應儲存密碼。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-277">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="0ea7a-278">[密碼管理員工具](xref:security/app-secrets)可以用來儲存本機開發的密碼。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-278">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="0ea7a-279">使用 [Visual Studio Code](https://code.visualstudio.com/) 時，可以會在 *.vscode/launch.json* 檔案設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-279">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="0ea7a-280">下列範例會將環境設定為 `Development`：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-280">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="0ea7a-281">使用 `dotnet run` 啟動應用程式時，不會如同 *Properties/launchSettings.json* 一樣讀取專案中的 *.vscode/launch.json* 檔。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-281">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="0ea7a-282">開發時若啟動的應用程式沒有 *launchSettings.json* 檔，請使用環境變數設定環境，或是使用 `dotnet run` 命令的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-282">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="0ea7a-283">生產環境</span><span class="sxs-lookup"><span data-stu-id="0ea7a-283">Production</span></span>

<span data-ttu-id="0ea7a-284">若要將安全性、效能及應用程式加強性最大化，您應該設定生產環境。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-284">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="0ea7a-285">不同於開發的某些一般設定包括：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-285">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="0ea7a-286">快取。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-286">Caching.</span></span>
* <span data-ttu-id="0ea7a-287">用戶端的資源會經過配套、縮減，且可能由 CDN 提供。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-287">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="0ea7a-288">停用診斷錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-288">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="0ea7a-289">啟用易懂的錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-289">Friendly error pages enabled.</span></span>
* <span data-ttu-id="0ea7a-290">啟用生產記錄與監視。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-290">Production logging and monitoring enabled.</span></span> <span data-ttu-id="0ea7a-291">例如 [Application Insights](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-291">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="0ea7a-292">設定環境</span><span class="sxs-lookup"><span data-stu-id="0ea7a-292">Set the environment</span></span>

<span data-ttu-id="0ea7a-293">設定特定環境以使用環境變數或平臺設定進行測試時，通常會很有用。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-293">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="0ea7a-294">如果您未設定環境，它會預設為 `Production` 並停用大部分的偵錯功能。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-294">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="0ea7a-295">環境的設定方法取決於作業系統而定。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-295">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="0ea7a-296">建立主機時，應用程式所讀取的最後一個環境設定會決定應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-296">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="0ea7a-297">應用程式正在執行時，無法變更應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-297">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="0ea7a-298">環境變數或平臺設定</span><span class="sxs-lookup"><span data-stu-id="0ea7a-298">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="0ea7a-299">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0ea7a-299">Azure App Service</span></span>

<span data-ttu-id="0ea7a-300">若要在 [Azure App Service](https://azure.microsoft.com/services/app-service/) 設定環境，請執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-300">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="0ea7a-301">從 [應用程式服務] 刀鋒視窗選取應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-301">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="0ea7a-302">在 [**設定**] 群組中，**選取 [** 設定] 分頁。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-302">In the **Settings** group, select the **Configuration** blade.</span></span>
1. <span data-ttu-id="0ea7a-303">在 [**應用程式設定**] 索引標籤中，選取 [**新增應用程式設定**]。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-303">In the **Application settings** tab, select **New application setting**.</span></span>
1. <span data-ttu-id="0ea7a-304">在 [**新增/編輯應用程式設定**] 視窗中，提供**名稱**的 `ASPNETCORE_ENVIRONMENT`。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-304">In the **Add/Edit application setting** window, provide `ASPNETCORE_ENVIRONMENT` for the **Name**.</span></span> <span data-ttu-id="0ea7a-305">針對 [**值**]，提供環境（例如 `Staging`）。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-305">For **Value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="0ea7a-306">如果您想要在交換部署位置時，將環境設定維持在目前的位置，請選取 [**部署位置設定**] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-306">Select the **Deployment slot setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="0ea7a-307">如需詳細資訊，請參閱 Azure 檔[中的在 Azure App Service 中設定預備環境](/azure/app-service/web-sites-staged-publishing)。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-307">For more information, see [Set up staging environments in Azure App Service](/azure/app-service/web-sites-staged-publishing) in the Azure documentation.</span></span>
1. <span data-ttu-id="0ea7a-308">選取 **[確定]** 以關閉 [**新增/編輯應用程式設定**] 視窗。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-308">Select **OK** to close the **Add/Edit application setting** window.</span></span>
1. <span data-ttu-id="0ea7a-309">選取 [設定] 分頁頂端的 **[** **儲存**]。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-309">Select **Save** at the top of the **Configuration** blade.</span></span>

<span data-ttu-id="0ea7a-310">Azure App Service 會在新增、變更或刪除 Azure 入口網站的應用程式設定 (環境變數) 之後自動重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-310">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="0ea7a-311">Windows</span><span class="sxs-lookup"><span data-stu-id="0ea7a-311">Windows</span></span>

<span data-ttu-id="0ea7a-312">如果應用程式是使用 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動，請使用下列命令來設定目前工作階段的 `ASPNETCORE_ENVIRONMENT`：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-312">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="0ea7a-313">**命令提示字元**</span><span class="sxs-lookup"><span data-stu-id="0ea7a-313">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="0ea7a-314">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="0ea7a-314">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="0ea7a-315">這些命令僅針對目前的視窗才會生效。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-315">These commands only take effect for the current window.</span></span> <span data-ttu-id="0ea7a-316">視窗關閉時，`ASPNETCORE_ENVIRONMENT` 設定會還原為預設值或電腦值。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-316">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="0ea7a-317">若要在 Windows 中以全域的方式設定值，請使用下列其中一個方法：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-317">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="0ea7a-318">開啟 [**控制台**] >**系統**> [ **Advanced system settings** ]，然後新增或編輯 `ASPNETCORE_ENVIRONMENT` 值：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-318">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![系統進階屬性](environments/_static/systemsetting_environment.png)

  ![ASPNET Core 環境變數](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="0ea7a-321">開啟系統管理命令提示字元，然後使用 `setx` 命令，或開啟系統管理 PowerShell 命令提示字元，然後使用 `[Environment]::SetEnvironmentVariable`：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-321">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="0ea7a-322">**命令提示字元**</span><span class="sxs-lookup"><span data-stu-id="0ea7a-322">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="0ea7a-323">`/M` 參數表示將環境變數設定在系統層級。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-323">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="0ea7a-324">若未使用 `/M` 參數，則將環境變數設定為使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-324">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="0ea7a-325">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="0ea7a-325">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="0ea7a-326">`Machine` 選項值表示將環境變數設定在系統層級。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-326">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="0ea7a-327">若選項值變更為 `User`，則將環境變數設定為使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-327">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="0ea7a-328">當以全域的方式設定 `ASPNETCORE_ENVIRONMENT` 環境變數時，則在設定該值後開啟的任何命令視窗中，對 `dotnet run` 均有效。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-328">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="0ea7a-329">**web.config**</span><span class="sxs-lookup"><span data-stu-id="0ea7a-329">**web.config**</span></span>

<span data-ttu-id="0ea7a-330">若要使用 *web.config* 設定 `ASPNETCORE_ENVIRONMENT` 環境變數，請參閱 <xref:host-and-deploy/aspnet-core-module#setting-environment-variables> 的＜設定環境變數＞一節。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-330">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="0ea7a-331">**專案檔或發行設定檔**</span><span class="sxs-lookup"><span data-stu-id="0ea7a-331">**Project file or publish profile**</span></span>

<span data-ttu-id="0ea7a-332">**針對 WINDOWS IIS 部署：** 將 `<EnvironmentName>` 屬性包含在[發行設定檔（. .pubxml）](xref:host-and-deploy/visual-studio-publish-profiles)或專案檔中。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-332">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="0ea7a-333">此方法會在專案發行時於 *web.config* 中設定環境：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-333">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="0ea7a-334">**每個 IIS 應用程式集區**</span><span class="sxs-lookup"><span data-stu-id="0ea7a-334">**Per IIS Application Pool**</span></span>

<span data-ttu-id="0ea7a-335">若要為執行於隔離應用程式集區 (受 IIS 10.0 或更新版本支援) 的應用程式設定 `ASPNETCORE_ENVIRONMENT` 環境變數，請參閱[環境變數 &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的＜AppCmd.exe 命令＞一節。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-335">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="0ea7a-336">當 `ASPNETCORE_ENVIRONMENT` 環境變數設定為應用程式集區時，其值會覆寫系統層級的設定。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-336">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0ea7a-337">當在 IIS 中裝載應用程式，並新增或變更 `ASPNETCORE_ENVIRONMENT` 環境變數時，請使用下列任一種方法，讓應用程式挑選新的值：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-337">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="0ea7a-338">從命令提示字元執行後面接著 `net start w3svc` 的 `net stop was /y`。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-338">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="0ea7a-339">重新啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-339">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="0ea7a-340">macOS</span><span class="sxs-lookup"><span data-stu-id="0ea7a-340">macOS</span></span>

<span data-ttu-id="0ea7a-341">您可以在執行應用程式時，以內嵌方式設定 macOS 目前的環境：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-341">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="0ea7a-342">或者，在執行應用程式之前，使用 `export` 設定環境：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-342">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="0ea7a-343">您可以在 *.bashrc* 或 *.bash_profile* 檔案中設定電腦層級的環境變數。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-343">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="0ea7a-344">使用任何文字編輯器編輯檔案。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-344">Edit the file using any text editor.</span></span> <span data-ttu-id="0ea7a-345">新增下列陳述式：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-345">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="0ea7a-346">Linux</span><span class="sxs-lookup"><span data-stu-id="0ea7a-346">Linux</span></span>

<span data-ttu-id="0ea7a-347">針對 Linux 散發版本，請在命令提示字元使用 `export` 命令，進行以工作階段為基礎的變數設定，並使用 *bash_profile* 檔案，進行電腦層級的環境設定。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-347">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="0ea7a-348">在程式碼中設定環境</span><span class="sxs-lookup"><span data-stu-id="0ea7a-348">Set the environment in code</span></span>

<span data-ttu-id="0ea7a-349">在建立主機時呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*>。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-349">Call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="0ea7a-350">請參閱 <xref:fundamentals/host/web-host#environment>。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-350">See <xref:fundamentals/host/web-host#environment>.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="0ea7a-351">取決於環境的組態</span><span class="sxs-lookup"><span data-stu-id="0ea7a-351">Configuration by environment</span></span>

<span data-ttu-id="0ea7a-352">若要依環境載入組態，建議使用：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-352">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="0ea7a-353">*appsettings* files （*appsettings. {環境}. json*）。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-353">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="0ea7a-354">請參閱 <xref:fundamentals/configuration/index#json-configuration-provider>。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-354">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="0ea7a-355">環境變數（在裝載應用程式的每個系統上設定）。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-355">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="0ea7a-356">請參閱<xref:fundamentals/host/web-host#environment>和<xref:security/app-secrets#environment-variables>。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-356">See <xref:fundamentals/host/web-host#environment> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="0ea7a-357">祕密管理員 (僅限開發環境)。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-357">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="0ea7a-358">請參閱 <xref:security/app-secrets>。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-358">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="0ea7a-359">以環境為基礎的 Startup 類別和方法</span><span class="sxs-lookup"><span data-stu-id="0ea7a-359">Environment-based Startup class and methods</span></span>

### <a name="inject-ihostingenvironment-into-startupconfigure"></a><span data-ttu-id="0ea7a-360">將 IHostingEnvironment 插入啟動。設定</span><span class="sxs-lookup"><span data-stu-id="0ea7a-360">Inject IHostingEnvironment into Startup.Configure</span></span>

<span data-ttu-id="0ea7a-361">將 <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> 插入 `Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-361">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="0ea7a-362">當應用程式只需要針對少數環境設定 `Startup.Configure`，且每個環境的程式碼差異最低時，這個方法就很有用。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-362">This approach is useful when the app only requires configuring `Startup.Configure` for only a few environments with minimal code differences per environment.</span></span>

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

### <a name="inject-ihostingenvironment-into-the-startup-class"></a><span data-ttu-id="0ea7a-363">將 IHostingEnvironment 插入 Startup 類別</span><span class="sxs-lookup"><span data-stu-id="0ea7a-363">Inject IHostingEnvironment into the Startup class</span></span>

<span data-ttu-id="0ea7a-364">將 <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> 插入 `Startup` 的函式，並將服務指派給欄位，以供整個 `Startup` 類別使用。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-364">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into the `Startup` constructor and assign the service to a field for use throughout the `Startup` class.</span></span> <span data-ttu-id="0ea7a-365">當應用程式需要針對少數環境設定啟動，但每個環境的程式碼差異最低時，這個方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-365">This approach is useful when the app requires configuring startup for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="0ea7a-366">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-366">In the following example:</span></span>

* <span data-ttu-id="0ea7a-367">環境會保留在 [`_env`] 欄位中。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-367">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="0ea7a-368">`_env` 用於 `ConfigureServices` 和 `Configure`，以根據應用程式的環境套用啟動設定。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-368">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

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

### <a name="startup-class-conventions"></a><span data-ttu-id="0ea7a-369">Startup 類別慣例</span><span class="sxs-lookup"><span data-stu-id="0ea7a-369">Startup class conventions</span></span>

<span data-ttu-id="0ea7a-370">ASP.NET Core 應用程式啟動時，[Startup 類別](xref:fundamentals/startup)會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-370">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="0ea7a-371">應用程式可以針對不同的環境定義個別的 `Startup` 類別（例如，`StartupDevelopment`）。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-371">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="0ea7a-372">在執行時間選取適當的 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-372">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="0ea7a-373">將優先使用其名稱尾碼符合目前環境的類別。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-373">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="0ea7a-374">如果找不到相符的 `Startup{EnvironmentName}` 類別，會使用 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-374">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="0ea7a-375">當應用程式需要針對數個環境設定啟動，且每個環境有許多程式碼差異時，此方法很有用。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-375">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="0ea7a-376">若要實作以環境為基礎的 `Startup` 類別，請為每個使用中的環境建立 `Startup{EnvironmentName}` 類別和後援 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-376">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

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

<span data-ttu-id="0ea7a-377">使用接受組件名稱的 [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) 多載：</span><span class="sxs-lookup"><span data-stu-id="0ea7a-377">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

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

### <a name="startup-method-conventions"></a><span data-ttu-id="0ea7a-378">Startup 方法慣例</span><span class="sxs-lookup"><span data-stu-id="0ea7a-378">Startup method conventions</span></span>

<span data-ttu-id="0ea7a-379">[設定](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure)和[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices)支援表單 `Configure<EnvironmentName>` 和 `Configure<EnvironmentName>Services`的環境特定版本。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-379">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="0ea7a-380">當應用程式需要針對數個環境設定啟動，且每個環境有許多程式碼差異時，此方法很有用。</span><span class="sxs-lookup"><span data-stu-id="0ea7a-380">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="0ea7a-381">其他資源</span><span class="sxs-lookup"><span data-stu-id="0ea7a-381">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>

::: moniker-end
