---
title: 在 ASP.NET Core 中使用多個環境
author: rick-anderson
description: 了解在 ASP.NET Core 應用程式中如何跨多個環境控制應用程式的行為。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/17/2019
uid: fundamentals/environments
ms.openlocfilehash: b0218b2c77c283c0849dca9491046534b88c5a77
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656216"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="efb6e-103">在 ASP.NET Core 中使用多個環境</span><span class="sxs-lookup"><span data-stu-id="efb6e-103">Use multiple environments in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="efb6e-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="efb6e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="efb6e-105">ASP.NET Core 會使用環境變數根據執行階段環境來設定應用程式行為。</span><span class="sxs-lookup"><span data-stu-id="efb6e-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="efb6e-106">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="efb6e-106">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="efb6e-107">環境</span><span class="sxs-lookup"><span data-stu-id="efb6e-107">Environments</span></span>

<span data-ttu-id="efb6e-108">ASP.NET Core 會在應用程式啟動時讀取環境變數 `ASPNETCORE_ENVIRONMENT`，並將值儲存在[IWebHostEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName)中。</span><span class="sxs-lookup"><span data-stu-id="efb6e-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IWebHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span></span> <span data-ttu-id="efb6e-109">`ASPNETCORE_ENVIRONMENT` 可以設定為任何值，但架構會提供三個值：</span><span class="sxs-lookup"><span data-stu-id="efb6e-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.Extensions.Hosting.Environments.Development>
* <xref:Microsoft.Extensions.Hosting.Environments.Staging>
* <span data-ttu-id="efb6e-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (預設值)</span><span class="sxs-lookup"><span data-stu-id="efb6e-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="efb6e-111">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="efb6e-111">The preceding code:</span></span>

* <span data-ttu-id="efb6e-112">當 [ 設為 ](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) 時，會呼叫 `ASPNETCORE_ENVIRONMENT`UseDeveloperExceptionPage`Development`。</span><span class="sxs-lookup"><span data-stu-id="efb6e-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="efb6e-113">當 [ 的值設為下列其中之一時，會呼叫 ](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler)UseExceptionHandler`ASPNETCORE_ENVIRONMENT`：</span><span class="sxs-lookup"><span data-stu-id="efb6e-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="efb6e-114">[環境標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)會使用 `IHostingEnvironment.EnvironmentName` 的值來包含或排除項目中的標籤：</span><span class="sxs-lookup"><span data-stu-id="efb6e-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="efb6e-115">在 Windows 及 macOS 中，環境變數和值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="efb6e-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="efb6e-116">Linux 的環境變數和值則預設為**區分大小寫**。</span><span class="sxs-lookup"><span data-stu-id="efb6e-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="efb6e-117">部署</span><span class="sxs-lookup"><span data-stu-id="efb6e-117">Development</span></span>

<span data-ttu-id="efb6e-118">您可以在開發環境中啟用生產環境不應該公開的功能。</span><span class="sxs-lookup"><span data-stu-id="efb6e-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="efb6e-119">例如，ASP.NET Core 範本會在開發環境中啟用[開發人員例外狀況頁面](xref:fundamentals/error-handling#developer-exception-page)。</span><span class="sxs-lookup"><span data-stu-id="efb6e-119">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="efb6e-120">您可以在專案的 *Properties\launchSettings.json* 檔案設定適用於本機電腦開發的環境。</span><span class="sxs-lookup"><span data-stu-id="efb6e-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="efb6e-121">在 *launchSettings.json* 中設定的環境值會覆寫系統環境的設定值。</span><span class="sxs-lookup"><span data-stu-id="efb6e-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="efb6e-122">下列 JSON 顯示來自 *launchSettings.json* 檔案的三個設定檔：</span><span class="sxs-lookup"><span data-stu-id="efb6e-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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
> <span data-ttu-id="efb6e-123">`applicationUrl`launchSettings.json*中的* 屬性可以指定伺服器 URL 的清單。</span><span class="sxs-lookup"><span data-stu-id="efb6e-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="efb6e-124">請在清單的 URL 之間使用分號：</span><span class="sxs-lookup"><span data-stu-id="efb6e-124">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="efb6e-125">以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動應用程式時，會使用具有 `"commandName": "Project"` 的第一個設定檔。</span><span class="sxs-lookup"><span data-stu-id="efb6e-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="efb6e-126">`commandName` 的值可指定要啟動的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="efb6e-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="efb6e-127">`commandName` 可以是下列任何一個項目：</span><span class="sxs-lookup"><span data-stu-id="efb6e-127">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="efb6e-128">`Project` (這會啟動 Kestrel)</span><span class="sxs-lookup"><span data-stu-id="efb6e-128">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="efb6e-129">以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動應用程式時：</span><span class="sxs-lookup"><span data-stu-id="efb6e-129">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="efb6e-130">會讀取 *launchSettings.json* (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="efb6e-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="efb6e-131">`environmentVariables`launchSettings.json*中的* 設定會覆寫環境變數。</span><span class="sxs-lookup"><span data-stu-id="efb6e-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="efb6e-132">主控環境隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="efb6e-132">The hosting environment is displayed.</span></span>

<span data-ttu-id="efb6e-133">下列輸出顯示應用程式是以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動：</span><span class="sxs-lookup"><span data-stu-id="efb6e-133">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="efb6e-134">Visual Studio 專案屬性的 [偵錯] 索引標籤提供 GUI，可編輯 *launchSettings.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="efb6e-134">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![專案屬性設定環境變數](environments/_static/project-properties-debug.png)

<span data-ttu-id="efb6e-136">您對專案設定檔所做的變更可能要等重新啟動網頁伺服器後才會生效。</span><span class="sxs-lookup"><span data-stu-id="efb6e-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="efb6e-137">您必須重新啟動 Kestrel，它才會偵測到環境已進行的變更。</span><span class="sxs-lookup"><span data-stu-id="efb6e-137">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="efb6e-138">*launchSettings.json* 不應儲存密碼。</span><span class="sxs-lookup"><span data-stu-id="efb6e-138">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="efb6e-139">[密碼管理員工具](xref:security/app-secrets)可以用來儲存本機開發的密碼。</span><span class="sxs-lookup"><span data-stu-id="efb6e-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="efb6e-140">使用 [Visual Studio Code](https://code.visualstudio.com/) 時，可以會在 *.vscode/launch.json* 檔案設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="efb6e-140">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="efb6e-141">下列範例會將環境設定為 `Development`：</span><span class="sxs-lookup"><span data-stu-id="efb6e-141">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="efb6e-142">使用 *啟動應用程式時，不會如同*Properties/launchSettings.json`dotnet run` 一樣讀取專案中的 *.vscode/launch.json* 檔。</span><span class="sxs-lookup"><span data-stu-id="efb6e-142">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="efb6e-143">開發時若啟動的應用程式沒有 *launchSettings.json* 檔，請使用環境變數設定環境，或是使用 `dotnet run` 命令的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="efb6e-143">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="efb6e-144">Production</span><span class="sxs-lookup"><span data-stu-id="efb6e-144">Production</span></span>

<span data-ttu-id="efb6e-145">若要將安全性、效能及應用程式加強性最大化，您應該設定生產環境。</span><span class="sxs-lookup"><span data-stu-id="efb6e-145">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="efb6e-146">不同於開發的某些一般設定包括：</span><span class="sxs-lookup"><span data-stu-id="efb6e-146">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="efb6e-147">快取。</span><span class="sxs-lookup"><span data-stu-id="efb6e-147">Caching.</span></span>
* <span data-ttu-id="efb6e-148">用戶端的資源會經過配套、縮減，且可能由 CDN 提供。</span><span class="sxs-lookup"><span data-stu-id="efb6e-148">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="efb6e-149">停用診斷錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="efb6e-149">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="efb6e-150">啟用易懂的錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="efb6e-150">Friendly error pages enabled.</span></span>
* <span data-ttu-id="efb6e-151">啟用生產記錄與監視。</span><span class="sxs-lookup"><span data-stu-id="efb6e-151">Production logging and monitoring enabled.</span></span> <span data-ttu-id="efb6e-152">例如 [Application Insights](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="efb6e-152">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="efb6e-153">設定環境</span><span class="sxs-lookup"><span data-stu-id="efb6e-153">Set the environment</span></span>

<span data-ttu-id="efb6e-154">設定特定環境以使用環境變數或平臺設定進行測試時，通常會很有用。</span><span class="sxs-lookup"><span data-stu-id="efb6e-154">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="efb6e-155">如果您未設定環境，它會預設為 `Production` 並停用大部分的偵錯功能。</span><span class="sxs-lookup"><span data-stu-id="efb6e-155">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="efb6e-156">環境的設定方法取決於作業系統而定。</span><span class="sxs-lookup"><span data-stu-id="efb6e-156">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="efb6e-157">建立主機時，應用程式所讀取的最後一個環境設定會決定應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="efb6e-157">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="efb6e-158">應用程式正在執行時，無法變更應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="efb6e-158">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="efb6e-159">環境變數或平臺設定</span><span class="sxs-lookup"><span data-stu-id="efb6e-159">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="efb6e-160">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="efb6e-160">Azure App Service</span></span>

<span data-ttu-id="efb6e-161">若要在 [Azure App Service](https://azure.microsoft.com/services/app-service/) 設定環境，請執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="efb6e-161">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="efb6e-162">從 [應用程式服務] 刀鋒視窗選取應用程式。</span><span class="sxs-lookup"><span data-stu-id="efb6e-162">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="efb6e-163">在 [**設定**] 群組中，**選取 [** 設定] 分頁。</span><span class="sxs-lookup"><span data-stu-id="efb6e-163">In the **Settings** group, select the **Configuration** blade.</span></span>
1. <span data-ttu-id="efb6e-164">在 [**應用程式設定**] 索引標籤中，選取 [**新增應用程式設定**]。</span><span class="sxs-lookup"><span data-stu-id="efb6e-164">In the **Application settings** tab, select **New application setting**.</span></span>
1. <span data-ttu-id="efb6e-165">在 [**新增/編輯應用程式設定**] 視窗中，提供**名稱**的 `ASPNETCORE_ENVIRONMENT`。</span><span class="sxs-lookup"><span data-stu-id="efb6e-165">In the **Add/Edit application setting** window, provide `ASPNETCORE_ENVIRONMENT` for the **Name**.</span></span> <span data-ttu-id="efb6e-166">針對 [**值**]，提供環境（例如 `Staging`）。</span><span class="sxs-lookup"><span data-stu-id="efb6e-166">For **Value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="efb6e-167">如果您想要在交換部署位置時，將環境設定維持在目前的位置，請選取 [**部署位置設定**] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="efb6e-167">Select the **Deployment slot setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="efb6e-168">如需詳細資訊，請參閱 Azure 檔[中的在 Azure App Service 中設定預備環境](/azure/app-service/web-sites-staged-publishing)。</span><span class="sxs-lookup"><span data-stu-id="efb6e-168">For more information, see [Set up staging environments in Azure App Service](/azure/app-service/web-sites-staged-publishing) in the Azure documentation.</span></span>
1. <span data-ttu-id="efb6e-169">選取 **[確定]** 以關閉 [**新增/編輯應用程式設定**] 視窗。</span><span class="sxs-lookup"><span data-stu-id="efb6e-169">Select **OK** to close the **Add/Edit application setting** window.</span></span>
1. <span data-ttu-id="efb6e-170">選取 [設定] 分頁頂端的 **[** **儲存**]。</span><span class="sxs-lookup"><span data-stu-id="efb6e-170">Select **Save** at the top of the **Configuration** blade.</span></span>

<span data-ttu-id="efb6e-171">Azure App Service 會在新增、變更或刪除 Azure 入口網站的應用程式設定 (環境變數) 之後自動重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="efb6e-171">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="efb6e-172">Windows</span><span class="sxs-lookup"><span data-stu-id="efb6e-172">Windows</span></span>

<span data-ttu-id="efb6e-173">如果應用程式是使用 `ASPNETCORE_ENVIRONMENT`dotnet run[ 啟動，請使用下列命令來設定目前工作階段的 ](/dotnet/core/tools/dotnet-run)：</span><span class="sxs-lookup"><span data-stu-id="efb6e-173">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="efb6e-174">**命令提示字元**</span><span class="sxs-lookup"><span data-stu-id="efb6e-174">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="efb6e-175">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="efb6e-175">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="efb6e-176">這些命令僅針對目前的視窗才會生效。</span><span class="sxs-lookup"><span data-stu-id="efb6e-176">These commands only take effect for the current window.</span></span> <span data-ttu-id="efb6e-177">視窗關閉時，`ASPNETCORE_ENVIRONMENT` 設定會還原為預設值或電腦值。</span><span class="sxs-lookup"><span data-stu-id="efb6e-177">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="efb6e-178">若要在 Windows 中以全域的方式設定值，請使用下列其中一個方法：</span><span class="sxs-lookup"><span data-stu-id="efb6e-178">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="efb6e-179">開啟 [**控制台**] >**系統**> [ **Advanced system settings** ]，然後新增或編輯 `ASPNETCORE_ENVIRONMENT` 值：</span><span class="sxs-lookup"><span data-stu-id="efb6e-179">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![系統進階屬性](environments/_static/systemsetting_environment.png)

  ![ASPNET Core 環境變數](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="efb6e-182">開啟系統管理命令提示字元，然後使用 `setx` 命令，或開啟系統管理 PowerShell 命令提示字元，然後使用 `[Environment]::SetEnvironmentVariable`：</span><span class="sxs-lookup"><span data-stu-id="efb6e-182">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="efb6e-183">**命令提示字元**</span><span class="sxs-lookup"><span data-stu-id="efb6e-183">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="efb6e-184">`/M` 參數表示將環境變數設定在系統層級。</span><span class="sxs-lookup"><span data-stu-id="efb6e-184">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="efb6e-185">若未使用 `/M` 參數，則將環境變數設定為使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="efb6e-185">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="efb6e-186">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="efb6e-186">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="efb6e-187">`Machine` 選項值表示將環境變數設定在系統層級。</span><span class="sxs-lookup"><span data-stu-id="efb6e-187">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="efb6e-188">若選項值變更為 `User`，則將環境變數設定為使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="efb6e-188">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="efb6e-189">當以全域的方式設定 `ASPNETCORE_ENVIRONMENT` 環境變數時，則在設定該值後開啟的任何命令視窗中，對 `dotnet run` 均有效。</span><span class="sxs-lookup"><span data-stu-id="efb6e-189">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="efb6e-190">**web.config**</span><span class="sxs-lookup"><span data-stu-id="efb6e-190">**web.config**</span></span>

<span data-ttu-id="efb6e-191">若要使用 `ASPNETCORE_ENVIRONMENT`web.config*設定* 環境變數，請參閱  *的＜設定環境變數＞*<xref:host-and-deploy/aspnet-core-module#setting-environment-variables>一節。</span><span class="sxs-lookup"><span data-stu-id="efb6e-191">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="efb6e-192">**專案檔或發行設定檔**</span><span class="sxs-lookup"><span data-stu-id="efb6e-192">**Project file or publish profile**</span></span>

<span data-ttu-id="efb6e-193">**針對 WINDOWS IIS 部署：** 將 `<EnvironmentName>` 屬性包含在[發行設定檔（. .pubxml）](xref:host-and-deploy/visual-studio-publish-profiles)或專案檔中。</span><span class="sxs-lookup"><span data-stu-id="efb6e-193">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="efb6e-194">此方法會在專案發行時於 *web.config* 中設定環境：</span><span class="sxs-lookup"><span data-stu-id="efb6e-194">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="efb6e-195">**每個 IIS 應用程式集區**</span><span class="sxs-lookup"><span data-stu-id="efb6e-195">**Per IIS Application Pool**</span></span>

<span data-ttu-id="efb6e-196">若要為執行於隔離應用程式集區 (受 IIS 10.0 或更新版本支援) 的應用程式設定 `ASPNETCORE_ENVIRONMENT` 環境變數，請參閱*環境變數* environmentVariables[&lt; 主題的＜AppCmd.exe 命令＞&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe)一節。</span><span class="sxs-lookup"><span data-stu-id="efb6e-196">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="efb6e-197">當 `ASPNETCORE_ENVIRONMENT` 環境變數設定為應用程式集區時，其值會覆寫系統層級的設定。</span><span class="sxs-lookup"><span data-stu-id="efb6e-197">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="efb6e-198">當在 IIS 中裝載應用程式，並新增或變更 `ASPNETCORE_ENVIRONMENT` 環境變數時，請使用下列任一種方法，讓應用程式挑選新的值：</span><span class="sxs-lookup"><span data-stu-id="efb6e-198">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="efb6e-199">從命令提示字元執行後面接著 `net stop was /y` 的 `net start w3svc`。</span><span class="sxs-lookup"><span data-stu-id="efb6e-199">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="efb6e-200">重新啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="efb6e-200">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="efb6e-201">macOS</span><span class="sxs-lookup"><span data-stu-id="efb6e-201">macOS</span></span>

<span data-ttu-id="efb6e-202">您可以在執行應用程式時，以內嵌方式設定 macOS 目前的環境：</span><span class="sxs-lookup"><span data-stu-id="efb6e-202">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="efb6e-203">或者，在執行應用程式之前，使用 `export` 設定環境：</span><span class="sxs-lookup"><span data-stu-id="efb6e-203">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="efb6e-204">您可以在 *.bashrc* 或 *.bash_profile* 檔案中設定電腦層級的環境變數。</span><span class="sxs-lookup"><span data-stu-id="efb6e-204">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="efb6e-205">使用任何文字編輯器編輯檔案。</span><span class="sxs-lookup"><span data-stu-id="efb6e-205">Edit the file using any text editor.</span></span> <span data-ttu-id="efb6e-206">新增下列陳述式：</span><span class="sxs-lookup"><span data-stu-id="efb6e-206">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="efb6e-207">Linux</span><span class="sxs-lookup"><span data-stu-id="efb6e-207">Linux</span></span>

<span data-ttu-id="efb6e-208">針對 Linux 散發版本，請在命令提示字元使用 `export` 命令，進行以工作階段為基礎的變數設定，並使用 *bash_profile* 檔案，進行電腦層級的環境設定。</span><span class="sxs-lookup"><span data-stu-id="efb6e-208">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="efb6e-209">在程式碼中設定環境</span><span class="sxs-lookup"><span data-stu-id="efb6e-209">Set the environment in code</span></span>

<span data-ttu-id="efb6e-210">在建立主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*>。</span><span class="sxs-lookup"><span data-stu-id="efb6e-210">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="efb6e-211">請參閱＜<xref:fundamentals/host/generic-host#environmentname>＞。</span><span class="sxs-lookup"><span data-stu-id="efb6e-211">See <xref:fundamentals/host/generic-host#environmentname>.</span></span>


### <a name="configuration-by-environment"></a><span data-ttu-id="efb6e-212">取決於環境的組態</span><span class="sxs-lookup"><span data-stu-id="efb6e-212">Configuration by environment</span></span>

<span data-ttu-id="efb6e-213">若要依環境載入組態，建議使用：</span><span class="sxs-lookup"><span data-stu-id="efb6e-213">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="efb6e-214">*appsettings* files （*appsettings. {環境}. json*）。</span><span class="sxs-lookup"><span data-stu-id="efb6e-214">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="efb6e-215">請參閱＜<xref:fundamentals/configuration/index#json-configuration-provider>＞。</span><span class="sxs-lookup"><span data-stu-id="efb6e-215">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="efb6e-216">環境變數（在裝載應用程式的每個系統上設定）。</span><span class="sxs-lookup"><span data-stu-id="efb6e-216">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="efb6e-217">請參閱 <xref:fundamentals/host/generic-host#environmentname> 和 <xref:security/app-secrets#environment-variables>。</span><span class="sxs-lookup"><span data-stu-id="efb6e-217">See <xref:fundamentals/host/generic-host#environmentname> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="efb6e-218">祕密管理員 (僅限開發環境)。</span><span class="sxs-lookup"><span data-stu-id="efb6e-218">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="efb6e-219">請參閱＜<xref:security/app-secrets>＞。</span><span class="sxs-lookup"><span data-stu-id="efb6e-219">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="efb6e-220">以環境為基礎的 Startup 類別和方法</span><span class="sxs-lookup"><span data-stu-id="efb6e-220">Environment-based Startup class and methods</span></span>

### <a name="inject-iwebhostenvironment-into-startupconfigure"></a><span data-ttu-id="efb6e-221">將 IWebHostEnvironment 插入啟動。設定</span><span class="sxs-lookup"><span data-stu-id="efb6e-221">Inject IWebHostEnvironment into Startup.Configure</span></span>

<span data-ttu-id="efb6e-222">將 <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> 插入 `Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="efb6e-222">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="efb6e-223">當應用程式只需要調整幾個環境的 `Startup.Configure`，且每個環境的程式碼差異最低時，此方法就很有用。</span><span class="sxs-lookup"><span data-stu-id="efb6e-223">This approach is useful when the app only requires adjusting `Startup.Configure` for a few environments with minimal code differences per environment.</span></span>

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

### <a name="inject-iwebhostenvironment-into-the-startup-class"></a><span data-ttu-id="efb6e-224">將 IWebHostEnvironment 插入 Startup 類別</span><span class="sxs-lookup"><span data-stu-id="efb6e-224">Inject IWebHostEnvironment into the Startup class</span></span>

<span data-ttu-id="efb6e-225">將 <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> 插入 `Startup` 的函式。</span><span class="sxs-lookup"><span data-stu-id="efb6e-225">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into the `Startup` constructor.</span></span> <span data-ttu-id="efb6e-226">當應用程式需要針對少數環境設定 `Startup`，但每個環境的程式碼差異最低時，此方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="efb6e-226">This approach is useful when the app requires configuring `Startup` for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="efb6e-227">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="efb6e-227">In the following example:</span></span>

* <span data-ttu-id="efb6e-228">環境會保留在 [`_env`] 欄位中。</span><span class="sxs-lookup"><span data-stu-id="efb6e-228">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="efb6e-229">`_env` 用於 `ConfigureServices` 和 `Configure`，以根據應用程式的環境套用啟動設定。</span><span class="sxs-lookup"><span data-stu-id="efb6e-229">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

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
### <a name="startup-class-conventions"></a><span data-ttu-id="efb6e-230">Startup 類別慣例</span><span class="sxs-lookup"><span data-stu-id="efb6e-230">Startup class conventions</span></span>

<span data-ttu-id="efb6e-231">ASP.NET Core 應用程式啟動時，[Startup 類別](xref:fundamentals/startup)會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="efb6e-231">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="efb6e-232">應用程式可以針對不同的環境定義個別的 `Startup` 類別（例如，`StartupDevelopment`）。</span><span class="sxs-lookup"><span data-stu-id="efb6e-232">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="efb6e-233">在執行時間選取適當的 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="efb6e-233">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="efb6e-234">將優先使用其名稱尾碼符合目前環境的類別。</span><span class="sxs-lookup"><span data-stu-id="efb6e-234">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="efb6e-235">如果找不到相符的 `Startup{EnvironmentName}` 類別，會使用 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="efb6e-235">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="efb6e-236">當應用程式需要針對數個環境設定啟動，且每個環境有許多程式碼差異時，此方法很有用。</span><span class="sxs-lookup"><span data-stu-id="efb6e-236">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="efb6e-237">若要實作以環境為基礎的 `Startup` 類別，請為每個使用中的環境建立 `Startup{EnvironmentName}` 類別和後援 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="efb6e-237">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

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

[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="efb6e-238">使用接受組件名稱的 [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) 多載：</span><span class="sxs-lookup"><span data-stu-id="efb6e-238">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

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

### <a name="startup-method-conventions"></a><span data-ttu-id="efb6e-239">Startup 方法慣例</span><span class="sxs-lookup"><span data-stu-id="efb6e-239">Startup method conventions</span></span>

<span data-ttu-id="efb6e-240">[設定](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure)和[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices)支援表單 `Configure<EnvironmentName>` 和 `Configure<EnvironmentName>Services`的環境特定版本。</span><span class="sxs-lookup"><span data-stu-id="efb6e-240">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="efb6e-241">當應用程式需要針對數個環境設定啟動，且每個環境有許多程式碼差異時，此方法很有用。</span><span class="sxs-lookup"><span data-stu-id="efb6e-241">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="efb6e-242">其他資源</span><span class="sxs-lookup"><span data-stu-id="efb6e-242">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="efb6e-243">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="efb6e-243">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="efb6e-244">ASP.NET Core 會使用環境變數根據執行階段環境來設定應用程式行為。</span><span class="sxs-lookup"><span data-stu-id="efb6e-244">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="efb6e-245">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="efb6e-245">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="efb6e-246">環境</span><span class="sxs-lookup"><span data-stu-id="efb6e-246">Environments</span></span>

<span data-ttu-id="efb6e-247">ASP.NET Core 會在應用程式啟動時讀取 `ASPNETCORE_ENVIRONMENT` 環境變數，並將該值儲存在 [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName) 中。</span><span class="sxs-lookup"><span data-stu-id="efb6e-247">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span></span> <span data-ttu-id="efb6e-248">`ASPNETCORE_ENVIRONMENT` 可以設定為任何值，但架構會提供三個值：</span><span class="sxs-lookup"><span data-stu-id="efb6e-248">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Staging>
* <span data-ttu-id="efb6e-249"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (預設值)</span><span class="sxs-lookup"><span data-stu-id="efb6e-249"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="efb6e-250">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="efb6e-250">The preceding code:</span></span>

* <span data-ttu-id="efb6e-251">當 [ 設為 ](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) 時，會呼叫 `ASPNETCORE_ENVIRONMENT`UseDeveloperExceptionPage`Development`。</span><span class="sxs-lookup"><span data-stu-id="efb6e-251">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="efb6e-252">當 [ 的值設為下列其中之一時，會呼叫 ](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler)UseExceptionHandler`ASPNETCORE_ENVIRONMENT`：</span><span class="sxs-lookup"><span data-stu-id="efb6e-252">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="efb6e-253">[環境標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)會使用 `IHostingEnvironment.EnvironmentName` 的值來包含或排除項目中的標籤：</span><span class="sxs-lookup"><span data-stu-id="efb6e-253">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="efb6e-254">在 Windows 及 macOS 中，環境變數和值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="efb6e-254">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="efb6e-255">Linux 的環境變數和值則預設為**區分大小寫**。</span><span class="sxs-lookup"><span data-stu-id="efb6e-255">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="efb6e-256">部署</span><span class="sxs-lookup"><span data-stu-id="efb6e-256">Development</span></span>

<span data-ttu-id="efb6e-257">您可以在開發環境中啟用生產環境不應該公開的功能。</span><span class="sxs-lookup"><span data-stu-id="efb6e-257">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="efb6e-258">例如，ASP.NET Core 範本會在開發環境中啟用[開發人員例外狀況頁面](xref:fundamentals/error-handling#developer-exception-page)。</span><span class="sxs-lookup"><span data-stu-id="efb6e-258">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="efb6e-259">您可以在專案的 *Properties\launchSettings.json* 檔案設定適用於本機電腦開發的環境。</span><span class="sxs-lookup"><span data-stu-id="efb6e-259">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="efb6e-260">在 *launchSettings.json* 中設定的環境值會覆寫系統環境的設定值。</span><span class="sxs-lookup"><span data-stu-id="efb6e-260">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="efb6e-261">下列 JSON 顯示來自 *launchSettings.json* 檔案的三個設定檔：</span><span class="sxs-lookup"><span data-stu-id="efb6e-261">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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
> <span data-ttu-id="efb6e-262">`applicationUrl`launchSettings.json*中的* 屬性可以指定伺服器 URL 的清單。</span><span class="sxs-lookup"><span data-stu-id="efb6e-262">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="efb6e-263">請在清單的 URL 之間使用分號：</span><span class="sxs-lookup"><span data-stu-id="efb6e-263">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="efb6e-264">以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動應用程式時，會使用具有 `"commandName": "Project"` 的第一個設定檔。</span><span class="sxs-lookup"><span data-stu-id="efb6e-264">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="efb6e-265">`commandName` 的值可指定要啟動的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="efb6e-265">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="efb6e-266">`commandName` 可以是下列任何一個項目：</span><span class="sxs-lookup"><span data-stu-id="efb6e-266">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="efb6e-267">`Project` (這會啟動 Kestrel)</span><span class="sxs-lookup"><span data-stu-id="efb6e-267">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="efb6e-268">以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動應用程式時：</span><span class="sxs-lookup"><span data-stu-id="efb6e-268">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="efb6e-269">會讀取 *launchSettings.json* (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="efb6e-269">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="efb6e-270">`environmentVariables`launchSettings.json*中的* 設定會覆寫環境變數。</span><span class="sxs-lookup"><span data-stu-id="efb6e-270">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="efb6e-271">主控環境隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="efb6e-271">The hosting environment is displayed.</span></span>

<span data-ttu-id="efb6e-272">下列輸出顯示應用程式是以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動：</span><span class="sxs-lookup"><span data-stu-id="efb6e-272">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="efb6e-273">Visual Studio 專案屬性的 [偵錯] 索引標籤提供 GUI，可編輯 *launchSettings.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="efb6e-273">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![專案屬性設定環境變數](environments/_static/project-properties-debug.png)

<span data-ttu-id="efb6e-275">您對專案設定檔所做的變更可能要等重新啟動網頁伺服器後才會生效。</span><span class="sxs-lookup"><span data-stu-id="efb6e-275">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="efb6e-276">您必須重新啟動 Kestrel，它才會偵測到環境已進行的變更。</span><span class="sxs-lookup"><span data-stu-id="efb6e-276">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="efb6e-277">*launchSettings.json* 不應儲存密碼。</span><span class="sxs-lookup"><span data-stu-id="efb6e-277">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="efb6e-278">[密碼管理員工具](xref:security/app-secrets)可以用來儲存本機開發的密碼。</span><span class="sxs-lookup"><span data-stu-id="efb6e-278">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="efb6e-279">使用 [Visual Studio Code](https://code.visualstudio.com/) 時，可以會在 *.vscode/launch.json* 檔案設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="efb6e-279">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="efb6e-280">下列範例會將環境設定為 `Development`：</span><span class="sxs-lookup"><span data-stu-id="efb6e-280">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="efb6e-281">使用 *啟動應用程式時，不會如同*Properties/launchSettings.json`dotnet run` 一樣讀取專案中的 *.vscode/launch.json* 檔。</span><span class="sxs-lookup"><span data-stu-id="efb6e-281">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="efb6e-282">開發時若啟動的應用程式沒有 *launchSettings.json* 檔，請使用環境變數設定環境，或是使用 `dotnet run` 命令的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="efb6e-282">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="efb6e-283">Production</span><span class="sxs-lookup"><span data-stu-id="efb6e-283">Production</span></span>

<span data-ttu-id="efb6e-284">若要將安全性、效能及應用程式加強性最大化，您應該設定生產環境。</span><span class="sxs-lookup"><span data-stu-id="efb6e-284">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="efb6e-285">不同於開發的某些一般設定包括：</span><span class="sxs-lookup"><span data-stu-id="efb6e-285">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="efb6e-286">快取。</span><span class="sxs-lookup"><span data-stu-id="efb6e-286">Caching.</span></span>
* <span data-ttu-id="efb6e-287">用戶端的資源會經過配套、縮減，且可能由 CDN 提供。</span><span class="sxs-lookup"><span data-stu-id="efb6e-287">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="efb6e-288">停用診斷錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="efb6e-288">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="efb6e-289">啟用易懂的錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="efb6e-289">Friendly error pages enabled.</span></span>
* <span data-ttu-id="efb6e-290">啟用生產記錄與監視。</span><span class="sxs-lookup"><span data-stu-id="efb6e-290">Production logging and monitoring enabled.</span></span> <span data-ttu-id="efb6e-291">例如 [Application Insights](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="efb6e-291">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="efb6e-292">設定環境</span><span class="sxs-lookup"><span data-stu-id="efb6e-292">Set the environment</span></span>

<span data-ttu-id="efb6e-293">設定特定環境以使用環境變數或平臺設定進行測試時，通常會很有用。</span><span class="sxs-lookup"><span data-stu-id="efb6e-293">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="efb6e-294">如果您未設定環境，它會預設為 `Production` 並停用大部分的偵錯功能。</span><span class="sxs-lookup"><span data-stu-id="efb6e-294">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="efb6e-295">環境的設定方法取決於作業系統而定。</span><span class="sxs-lookup"><span data-stu-id="efb6e-295">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="efb6e-296">建立主機時，應用程式所讀取的最後一個環境設定會決定應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="efb6e-296">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="efb6e-297">應用程式正在執行時，無法變更應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="efb6e-297">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="efb6e-298">環境變數或平臺設定</span><span class="sxs-lookup"><span data-stu-id="efb6e-298">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="efb6e-299">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="efb6e-299">Azure App Service</span></span>

<span data-ttu-id="efb6e-300">若要在 [Azure App Service](https://azure.microsoft.com/services/app-service/) 設定環境，請執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="efb6e-300">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="efb6e-301">從 [應用程式服務] 刀鋒視窗選取應用程式。</span><span class="sxs-lookup"><span data-stu-id="efb6e-301">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="efb6e-302">在 [**設定**] 群組中，**選取 [** 設定] 分頁。</span><span class="sxs-lookup"><span data-stu-id="efb6e-302">In the **Settings** group, select the **Configuration** blade.</span></span>
1. <span data-ttu-id="efb6e-303">在 [**應用程式設定**] 索引標籤中，選取 [**新增應用程式設定**]。</span><span class="sxs-lookup"><span data-stu-id="efb6e-303">In the **Application settings** tab, select **New application setting**.</span></span>
1. <span data-ttu-id="efb6e-304">在 [**新增/編輯應用程式設定**] 視窗中，提供**名稱**的 `ASPNETCORE_ENVIRONMENT`。</span><span class="sxs-lookup"><span data-stu-id="efb6e-304">In the **Add/Edit application setting** window, provide `ASPNETCORE_ENVIRONMENT` for the **Name**.</span></span> <span data-ttu-id="efb6e-305">針對 [**值**]，提供環境（例如 `Staging`）。</span><span class="sxs-lookup"><span data-stu-id="efb6e-305">For **Value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="efb6e-306">如果您想要在交換部署位置時，將環境設定維持在目前的位置，請選取 [**部署位置設定**] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="efb6e-306">Select the **Deployment slot setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="efb6e-307">如需詳細資訊，請參閱 Azure 檔[中的在 Azure App Service 中設定預備環境](/azure/app-service/web-sites-staged-publishing)。</span><span class="sxs-lookup"><span data-stu-id="efb6e-307">For more information, see [Set up staging environments in Azure App Service](/azure/app-service/web-sites-staged-publishing) in the Azure documentation.</span></span>
1. <span data-ttu-id="efb6e-308">選取 **[確定]** 以關閉 [**新增/編輯應用程式設定**] 視窗。</span><span class="sxs-lookup"><span data-stu-id="efb6e-308">Select **OK** to close the **Add/Edit application setting** window.</span></span>
1. <span data-ttu-id="efb6e-309">選取 [設定] 分頁頂端的 **[** **儲存**]。</span><span class="sxs-lookup"><span data-stu-id="efb6e-309">Select **Save** at the top of the **Configuration** blade.</span></span>

<span data-ttu-id="efb6e-310">Azure App Service 會在新增、變更或刪除 Azure 入口網站的應用程式設定 (環境變數) 之後自動重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="efb6e-310">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="efb6e-311">Windows</span><span class="sxs-lookup"><span data-stu-id="efb6e-311">Windows</span></span>

<span data-ttu-id="efb6e-312">如果應用程式是使用 `ASPNETCORE_ENVIRONMENT`dotnet run[ 啟動，請使用下列命令來設定目前工作階段的 ](/dotnet/core/tools/dotnet-run)：</span><span class="sxs-lookup"><span data-stu-id="efb6e-312">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="efb6e-313">**命令提示字元**</span><span class="sxs-lookup"><span data-stu-id="efb6e-313">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="efb6e-314">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="efb6e-314">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="efb6e-315">這些命令僅針對目前的視窗才會生效。</span><span class="sxs-lookup"><span data-stu-id="efb6e-315">These commands only take effect for the current window.</span></span> <span data-ttu-id="efb6e-316">視窗關閉時，`ASPNETCORE_ENVIRONMENT` 設定會還原為預設值或電腦值。</span><span class="sxs-lookup"><span data-stu-id="efb6e-316">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="efb6e-317">若要在 Windows 中以全域的方式設定值，請使用下列其中一個方法：</span><span class="sxs-lookup"><span data-stu-id="efb6e-317">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="efb6e-318">開啟 [**控制台**] >**系統**> [ **Advanced system settings** ]，然後新增或編輯 `ASPNETCORE_ENVIRONMENT` 值：</span><span class="sxs-lookup"><span data-stu-id="efb6e-318">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![系統進階屬性](environments/_static/systemsetting_environment.png)

  ![ASPNET Core 環境變數](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="efb6e-321">開啟系統管理命令提示字元，然後使用 `setx` 命令，或開啟系統管理 PowerShell 命令提示字元，然後使用 `[Environment]::SetEnvironmentVariable`：</span><span class="sxs-lookup"><span data-stu-id="efb6e-321">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="efb6e-322">**命令提示字元**</span><span class="sxs-lookup"><span data-stu-id="efb6e-322">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="efb6e-323">`/M` 參數表示將環境變數設定在系統層級。</span><span class="sxs-lookup"><span data-stu-id="efb6e-323">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="efb6e-324">若未使用 `/M` 參數，則將環境變數設定為使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="efb6e-324">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="efb6e-325">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="efb6e-325">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="efb6e-326">`Machine` 選項值表示將環境變數設定在系統層級。</span><span class="sxs-lookup"><span data-stu-id="efb6e-326">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="efb6e-327">若選項值變更為 `User`，則將環境變數設定為使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="efb6e-327">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="efb6e-328">當以全域的方式設定 `ASPNETCORE_ENVIRONMENT` 環境變數時，則在設定該值後開啟的任何命令視窗中，對 `dotnet run` 均有效。</span><span class="sxs-lookup"><span data-stu-id="efb6e-328">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="efb6e-329">**web.config**</span><span class="sxs-lookup"><span data-stu-id="efb6e-329">**web.config**</span></span>

<span data-ttu-id="efb6e-330">若要使用 `ASPNETCORE_ENVIRONMENT`web.config*設定* 環境變數，請參閱  *的＜設定環境變數＞*<xref:host-and-deploy/aspnet-core-module#setting-environment-variables>一節。</span><span class="sxs-lookup"><span data-stu-id="efb6e-330">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="efb6e-331">**專案檔或發行設定檔**</span><span class="sxs-lookup"><span data-stu-id="efb6e-331">**Project file or publish profile**</span></span>

<span data-ttu-id="efb6e-332">**針對 WINDOWS IIS 部署：** 將 `<EnvironmentName>` 屬性包含在[發行設定檔（. .pubxml）](xref:host-and-deploy/visual-studio-publish-profiles)或專案檔中。</span><span class="sxs-lookup"><span data-stu-id="efb6e-332">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="efb6e-333">此方法會在專案發行時於 *web.config* 中設定環境：</span><span class="sxs-lookup"><span data-stu-id="efb6e-333">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="efb6e-334">**每個 IIS 應用程式集區**</span><span class="sxs-lookup"><span data-stu-id="efb6e-334">**Per IIS Application Pool**</span></span>

<span data-ttu-id="efb6e-335">若要為執行於隔離應用程式集區 (受 IIS 10.0 或更新版本支援) 的應用程式設定 `ASPNETCORE_ENVIRONMENT` 環境變數，請參閱*環境變數* environmentVariables[&lt; 主題的＜AppCmd.exe 命令＞&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe)一節。</span><span class="sxs-lookup"><span data-stu-id="efb6e-335">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="efb6e-336">當 `ASPNETCORE_ENVIRONMENT` 環境變數設定為應用程式集區時，其值會覆寫系統層級的設定。</span><span class="sxs-lookup"><span data-stu-id="efb6e-336">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="efb6e-337">當在 IIS 中裝載應用程式，並新增或變更 `ASPNETCORE_ENVIRONMENT` 環境變數時，請使用下列任一種方法，讓應用程式挑選新的值：</span><span class="sxs-lookup"><span data-stu-id="efb6e-337">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="efb6e-338">從命令提示字元執行後面接著 `net stop was /y` 的 `net start w3svc`。</span><span class="sxs-lookup"><span data-stu-id="efb6e-338">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="efb6e-339">重新啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="efb6e-339">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="efb6e-340">macOS</span><span class="sxs-lookup"><span data-stu-id="efb6e-340">macOS</span></span>

<span data-ttu-id="efb6e-341">您可以在執行應用程式時，以內嵌方式設定 macOS 目前的環境：</span><span class="sxs-lookup"><span data-stu-id="efb6e-341">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="efb6e-342">或者，在執行應用程式之前，使用 `export` 設定環境：</span><span class="sxs-lookup"><span data-stu-id="efb6e-342">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="efb6e-343">您可以在 *.bashrc* 或 *.bash_profile* 檔案中設定電腦層級的環境變數。</span><span class="sxs-lookup"><span data-stu-id="efb6e-343">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="efb6e-344">使用任何文字編輯器編輯檔案。</span><span class="sxs-lookup"><span data-stu-id="efb6e-344">Edit the file using any text editor.</span></span> <span data-ttu-id="efb6e-345">新增下列陳述式：</span><span class="sxs-lookup"><span data-stu-id="efb6e-345">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="efb6e-346">Linux</span><span class="sxs-lookup"><span data-stu-id="efb6e-346">Linux</span></span>

<span data-ttu-id="efb6e-347">針對 Linux 散發版本，請在命令提示字元使用 `export` 命令，進行以工作階段為基礎的變數設定，並使用 *bash_profile* 檔案，進行電腦層級的環境設定。</span><span class="sxs-lookup"><span data-stu-id="efb6e-347">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="efb6e-348">在程式碼中設定環境</span><span class="sxs-lookup"><span data-stu-id="efb6e-348">Set the environment in code</span></span>

<span data-ttu-id="efb6e-349">在建立主機時呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*>。</span><span class="sxs-lookup"><span data-stu-id="efb6e-349">Call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="efb6e-350">請參閱＜<xref:fundamentals/host/web-host#environment>＞。</span><span class="sxs-lookup"><span data-stu-id="efb6e-350">See <xref:fundamentals/host/web-host#environment>.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="efb6e-351">取決於環境的組態</span><span class="sxs-lookup"><span data-stu-id="efb6e-351">Configuration by environment</span></span>

<span data-ttu-id="efb6e-352">若要依環境載入組態，建議使用：</span><span class="sxs-lookup"><span data-stu-id="efb6e-352">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="efb6e-353">*appsettings* files （*appsettings. {環境}. json*）。</span><span class="sxs-lookup"><span data-stu-id="efb6e-353">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="efb6e-354">請參閱＜<xref:fundamentals/configuration/index#json-configuration-provider>＞。</span><span class="sxs-lookup"><span data-stu-id="efb6e-354">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="efb6e-355">環境變數（在裝載應用程式的每個系統上設定）。</span><span class="sxs-lookup"><span data-stu-id="efb6e-355">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="efb6e-356">請參閱 <xref:fundamentals/host/web-host#environment> 和 <xref:security/app-secrets#environment-variables>。</span><span class="sxs-lookup"><span data-stu-id="efb6e-356">See <xref:fundamentals/host/web-host#environment> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="efb6e-357">祕密管理員 (僅限開發環境)。</span><span class="sxs-lookup"><span data-stu-id="efb6e-357">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="efb6e-358">請參閱＜<xref:security/app-secrets>＞。</span><span class="sxs-lookup"><span data-stu-id="efb6e-358">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="efb6e-359">以環境為基礎的 Startup 類別和方法</span><span class="sxs-lookup"><span data-stu-id="efb6e-359">Environment-based Startup class and methods</span></span>

### <a name="inject-ihostingenvironment-into-startupconfigure"></a><span data-ttu-id="efb6e-360">將 IHostingEnvironment 插入啟動。設定</span><span class="sxs-lookup"><span data-stu-id="efb6e-360">Inject IHostingEnvironment into Startup.Configure</span></span>

<span data-ttu-id="efb6e-361">將 <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> 插入 `Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="efb6e-361">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="efb6e-362">當應用程式只需要針對少數環境設定 `Startup.Configure`，且每個環境的程式碼差異最低時，這個方法就很有用。</span><span class="sxs-lookup"><span data-stu-id="efb6e-362">This approach is useful when the app only requires configuring `Startup.Configure` for only a few environments with minimal code differences per environment.</span></span>

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

### <a name="inject-ihostingenvironment-into-the-startup-class"></a><span data-ttu-id="efb6e-363">將 IHostingEnvironment 插入 Startup 類別</span><span class="sxs-lookup"><span data-stu-id="efb6e-363">Inject IHostingEnvironment into the Startup class</span></span>

<span data-ttu-id="efb6e-364">將 <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> 插入 `Startup` 的函式，並將服務指派給欄位，以供整個 `Startup` 類別使用。</span><span class="sxs-lookup"><span data-stu-id="efb6e-364">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into the `Startup` constructor and assign the service to a field for use throughout the `Startup` class.</span></span> <span data-ttu-id="efb6e-365">當應用程式需要針對少數環境設定啟動，但每個環境的程式碼差異最低時，這個方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="efb6e-365">This approach is useful when the app requires configuring startup for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="efb6e-366">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="efb6e-366">In the following example:</span></span>

* <span data-ttu-id="efb6e-367">環境會保留在 [`_env`] 欄位中。</span><span class="sxs-lookup"><span data-stu-id="efb6e-367">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="efb6e-368">`_env` 用於 `ConfigureServices` 和 `Configure`，以根據應用程式的環境套用啟動設定。</span><span class="sxs-lookup"><span data-stu-id="efb6e-368">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

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

### <a name="startup-class-conventions"></a><span data-ttu-id="efb6e-369">Startup 類別慣例</span><span class="sxs-lookup"><span data-stu-id="efb6e-369">Startup class conventions</span></span>

<span data-ttu-id="efb6e-370">ASP.NET Core 應用程式啟動時，[Startup 類別](xref:fundamentals/startup)會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="efb6e-370">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="efb6e-371">應用程式可以針對不同的環境定義個別的 `Startup` 類別（例如，`StartupDevelopment`）。</span><span class="sxs-lookup"><span data-stu-id="efb6e-371">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="efb6e-372">在執行時間選取適當的 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="efb6e-372">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="efb6e-373">將優先使用其名稱尾碼符合目前環境的類別。</span><span class="sxs-lookup"><span data-stu-id="efb6e-373">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="efb6e-374">如果找不到相符的 `Startup{EnvironmentName}` 類別，會使用 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="efb6e-374">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="efb6e-375">當應用程式需要針對數個環境設定啟動，且每個環境有許多程式碼差異時，此方法很有用。</span><span class="sxs-lookup"><span data-stu-id="efb6e-375">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="efb6e-376">若要實作以環境為基礎的 `Startup` 類別，請為每個使用中的環境建立 `Startup{EnvironmentName}` 類別和後援 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="efb6e-376">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

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

<span data-ttu-id="efb6e-377">使用接受組件名稱的 [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) 多載：</span><span class="sxs-lookup"><span data-stu-id="efb6e-377">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

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

### <a name="startup-method-conventions"></a><span data-ttu-id="efb6e-378">Startup 方法慣例</span><span class="sxs-lookup"><span data-stu-id="efb6e-378">Startup method conventions</span></span>

<span data-ttu-id="efb6e-379">[設定](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure)和[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices)支援表單 `Configure<EnvironmentName>` 和 `Configure<EnvironmentName>Services`的環境特定版本。</span><span class="sxs-lookup"><span data-stu-id="efb6e-379">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="efb6e-380">當應用程式需要針對數個環境設定啟動，且每個環境有許多程式碼差異時，此方法很有用。</span><span class="sxs-lookup"><span data-stu-id="efb6e-380">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="efb6e-381">其他資源</span><span class="sxs-lookup"><span data-stu-id="efb6e-381">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>

::: moniker-end
