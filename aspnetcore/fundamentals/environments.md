---
title: 在 ASP.NET Core 中使用多個環境
author: rick-anderson
description: 了解在 ASP.NET Core 應用程式中如何跨多個環境控制應用程式的行為。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 7/1/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/environments
ms.openlocfilehash: 977d222ed61fa914bd4ffb70e192ef19d4da5c33
ms.sourcegitcommit: 6fb27ea41a92f6d0e91dfd0eba905d2ac1a707f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/15/2020
ms.locfileid: "87330610"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="10075-103">在 ASP.NET Core 中使用多個環境</span><span class="sxs-lookup"><span data-stu-id="10075-103">Use multiple environments in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="10075-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Kirk Larkin](https://twitter.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="10075-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Kirk Larkin](https://twitter.com/serpent5)</span></span>

<span data-ttu-id="10075-105">ASP.NET Core 會使用環境變數根據執行階段環境來設定應用程式行為。</span><span class="sxs-lookup"><span data-stu-id="10075-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="10075-106">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="10075-106">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="10075-107">環境</span><span class="sxs-lookup"><span data-stu-id="10075-107">Environments</span></span>

<span data-ttu-id="10075-108">若要判斷執行時間環境，ASP.NET Core 從下列環境變數讀取：</span><span class="sxs-lookup"><span data-stu-id="10075-108">To determine the runtime environment, ASP.NET Core reads from the following environment variables:</span></span>

1. [<span data-ttu-id="10075-109">DOTNET_ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="10075-109">DOTNET_ENVIRONMENT</span></span>](xref:fundamentals/configuration/index#default-host-configuration)
1. <span data-ttu-id="10075-110">`ASPNETCORE_ENVIRONMENT`當 <xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults*> 呼叫時。</span><span class="sxs-lookup"><span data-stu-id="10075-110">`ASPNETCORE_ENVIRONMENT` when <xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults*> is called.</span></span> <span data-ttu-id="10075-111">預設 ASP.NET Core web 應用程式範本呼叫 `ConfigureWebHostDefaults` 。</span><span class="sxs-lookup"><span data-stu-id="10075-111">The default ASP.NET Core web app templates call `ConfigureWebHostDefaults`.</span></span> <span data-ttu-id="10075-112">`ASPNETCORE_ENVIRONMENT`值會覆寫 `DOTNET_ENVIRONMENT` 。</span><span class="sxs-lookup"><span data-stu-id="10075-112">The `ASPNETCORE_ENVIRONMENT` value overrides `DOTNET_ENVIRONMENT`.</span></span>

<span data-ttu-id="10075-113">`IHostEnvironment.EnvironmentName`可以設定為任何值，但下列值是由架構所提供：</span><span class="sxs-lookup"><span data-stu-id="10075-113">`IHostEnvironment.EnvironmentName` can be set to any value, but the following values are provided by the framework:</span></span>

* <span data-ttu-id="10075-114"><xref:Microsoft.Extensions.Hosting.Environments.Development>：在本機電腦上，將檔案[上的launchSettings.js](#lsj)設定 `ASPNETCORE_ENVIRONMENT` 為 `Development` 。</span><span class="sxs-lookup"><span data-stu-id="10075-114"><xref:Microsoft.Extensions.Hosting.Environments.Development> : The [launchSettings.json](#lsj) file sets `ASPNETCORE_ENVIRONMENT` to `Development` on the local machine.</span></span>
* <xref:Microsoft.Extensions.Hosting.Environments.Staging>
* <span data-ttu-id="10075-115"><xref:Microsoft.Extensions.Hosting.Environments.Production>：如果 `DOTNET_ENVIRONMENT` 未設定和，則為預設值 `ASPNETCORE_ENVIRONMENT` 。</span><span class="sxs-lookup"><span data-stu-id="10075-115"><xref:Microsoft.Extensions.Hosting.Environments.Production> : The default if `DOTNET_ENVIRONMENT` and `ASPNETCORE_ENVIRONMENT` have not been set.</span></span>

<span data-ttu-id="10075-116">下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="10075-116">The following code:</span></span>

* <span data-ttu-id="10075-117">當 `ASPNETCORE_ENVIRONMENT` 設為 `Development` 時，會呼叫 [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage)。</span><span class="sxs-lookup"><span data-stu-id="10075-117">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="10075-118">當[UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler)的值 `ASPNETCORE_ENVIRONMENT` 設定為、或時，會呼叫 UseExceptionHandler `Staging` `Production` `Staging_2` 。</span><span class="sxs-lookup"><span data-stu-id="10075-118">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set to `Staging`, `Production`, or  `Staging_2`.</span></span>
* <span data-ttu-id="10075-119">將插入 <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> 中 `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="10075-119">Injects <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="10075-120">當應用程式只需要調整 `Startup.Configure` 少數環境，且每個環境的程式碼差異最低時，這個方法就很有用。</span><span class="sxs-lookup"><span data-stu-id="10075-120">This approach is useful when the app only requires adjusting `Startup.Configure` for a few environments with minimal code differences per environment.</span></span>
* <span data-ttu-id="10075-121">類似于 ASP.NET Core 範本所產生的程式碼。</span><span class="sxs-lookup"><span data-stu-id="10075-121">Is similar to the code generated by the ASP.NET Core templates.</span></span>

[!code-csharp[](environments/3.1sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="10075-122">[環境標記](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)協助程式會使用[IHostEnvironment](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName)的值來包含或排除元素中的標記：</span><span class="sxs-lookup"><span data-stu-id="10075-122">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName) to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="10075-123">[範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample)中的 [[關於] 頁面](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample/EnvironmentsSample/Pages/About.cshtml)包含前述標記，並顯示的值 `IWebHostEnvironment.EnvironmentName` 。</span><span class="sxs-lookup"><span data-stu-id="10075-123">The [About page](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample/EnvironmentsSample/Pages/About.cshtml) from the [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample) includes the preceding markup and displays the value of `IWebHostEnvironment.EnvironmentName`.</span></span>

<span data-ttu-id="10075-124">在 Windows 和 macOS 上，環境變數和值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="10075-124">On Windows and macOS, environment variables and values aren't case-sensitive.</span></span> <span data-ttu-id="10075-125">根據預設，Linux 環境變數和值會區分**大小寫**。</span><span class="sxs-lookup"><span data-stu-id="10075-125">Linux environment variables and values are **case-sensitive** by default.</span></span>

### <a name="create-environmentssample"></a><span data-ttu-id="10075-126">建立 EnvironmentsSample</span><span class="sxs-lookup"><span data-stu-id="10075-126">Create EnvironmentsSample</span></span>

<span data-ttu-id="10075-127">本檔中使用的[範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample)是以 Razor 名為*EnvironmentsSample*的頁面專案為基礎。</span><span class="sxs-lookup"><span data-stu-id="10075-127">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample) used in this document is based on a Razor Pages project named *EnvironmentsSample*.</span></span>

<span data-ttu-id="10075-128">下列程式碼會建立並執行名為*EnvironmentsSample*的 web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="10075-128">The following code creates and runs a web app named *EnvironmentsSample*:</span></span>

```bash
dotnet new webapp -o EnvironmentsSample
cd EnvironmentsSample
dotnet run --verbosity normal
```

<span data-ttu-id="10075-129">當應用程式執行時，它會顯示下列部分輸出：</span><span class="sxs-lookup"><span data-stu-id="10075-129">When the app runs, it displays some of the following output:</span></span>

```bash
Using launch settings from c:\tmp\EnvironmentsSample\Properties\launchSettings.json
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: c:\tmp\EnvironmentsSample
```

<a name="lsj"></a>

### <a name="development-and-launchsettingsjson"></a><span data-ttu-id="10075-130">開發與 launchSettings.js</span><span class="sxs-lookup"><span data-stu-id="10075-130">Development and launchSettings.json</span></span>

<span data-ttu-id="10075-131">您可以在開發環境中啟用生產環境不應該公開的功能。</span><span class="sxs-lookup"><span data-stu-id="10075-131">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="10075-132">例如，ASP.NET Core 範本會在開發環境中啟用[開發人員例外狀況頁面](xref:fundamentals/error-handling#developer-exception-page)。</span><span class="sxs-lookup"><span data-stu-id="10075-132">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="10075-133">您可以在專案的 *Properties\launchSettings.json* 檔案設定適用於本機電腦開發的環境。</span><span class="sxs-lookup"><span data-stu-id="10075-133">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="10075-134">在 *launchSettings.json* 中設定的環境值會覆寫系統環境的設定值。</span><span class="sxs-lookup"><span data-stu-id="10075-134">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="10075-135">檔案*上的launchSettings.js* ：</span><span class="sxs-lookup"><span data-stu-id="10075-135">The *launchSettings.json* file:</span></span>

* <span data-ttu-id="10075-136">僅用於本機開發電腦。</span><span class="sxs-lookup"><span data-stu-id="10075-136">Is only used on the local development machine.</span></span>
* <span data-ttu-id="10075-137">未部署。</span><span class="sxs-lookup"><span data-stu-id="10075-137">Is not deployed.</span></span>
* <span data-ttu-id="10075-138">包含設定檔設定。</span><span class="sxs-lookup"><span data-stu-id="10075-138">contains profile settings.</span></span>

<span data-ttu-id="10075-139">下列 JSON 會針對以 Visual Studio 或建立的 ASP.NET Core 網路計畫，顯示名為*EnvironmentsSample*的檔案*launchSettings.js*檔案 `dotnet new` ：</span><span class="sxs-lookup"><span data-stu-id="10075-139">The following JSON shows the *launchSettings.json* file for an ASP.NET Core web projected named *EnvironmentsSample* created with Visual Studio or `dotnet new`:</span></span>

[!code-json[](environments/3.1sample/EnvironmentsSample/Properties/launchSettingsCopy.json)]

<span data-ttu-id="10075-140">上述標記包含兩個設定檔：</span><span class="sxs-lookup"><span data-stu-id="10075-140">The preceding markup contains two profiles:</span></span>

* <span data-ttu-id="10075-141">`IIS Express`：從 Visual Studio 啟動應用程式時所使用的預設設定檔。</span><span class="sxs-lookup"><span data-stu-id="10075-141">`IIS Express`: The default profile used when launching the app from Visual Studio.</span></span> <span data-ttu-id="10075-142">此索引 `"commandName"` 鍵的值 `"IISExpress"` 為，因此， [IISExpress](/iis/extensions/introduction-to-iis-express/iis-express-overview)是 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="10075-142">The `"commandName"` key has the value `"IISExpress"`, therefore, [IISExpress](/iis/extensions/introduction-to-iis-express/iis-express-overview) is the web server.</span></span> <span data-ttu-id="10075-143">您可以將啟動設定檔設定為專案或包含的任何其他設定檔。</span><span class="sxs-lookup"><span data-stu-id="10075-143">You can set the launch profile to the project or any other profile included.</span></span> <span data-ttu-id="10075-144">例如，在下圖中，選取專案名稱會啟動[Kestrel web 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="10075-144">For example, in the image below, selecting the project name launches the [Kestrel web server](xref:fundamentals/servers/kestrel).</span></span>

  ![IIS Express 在功能表上啟動](environments/_static/iisx2.png)
* <span data-ttu-id="10075-146">`EnvironmentsSample`：設定檔名稱是專案名稱。</span><span class="sxs-lookup"><span data-stu-id="10075-146">`EnvironmentsSample`: The profile name is the project name.</span></span> <span data-ttu-id="10075-147">當使用啟動應用程式時，預設會使用此設定檔 `dotnet run` 。</span><span class="sxs-lookup"><span data-stu-id="10075-147">This profile is used by default when launching the app with `dotnet run`.</span></span>  <span data-ttu-id="10075-148">`"commandName"`金鑰具有值 `"Project"` ，因此會啟動[Kestrel web 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="10075-148">The `"commandName"` key has the value `"Project"`, therefore, the [Kestrel web server](xref:fundamentals/servers/kestrel) is launched.</span></span>

<span data-ttu-id="10075-149">的值 `commandName` 可指定要啟動的 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="10075-149">The value of `commandName` can specify the web server to launch.</span></span> <span data-ttu-id="10075-150">`commandName` 可以是下列任何一個項目：</span><span class="sxs-lookup"><span data-stu-id="10075-150">`commandName` can be any one of the following:</span></span>

* <span data-ttu-id="10075-151">`IISExpress`：啟動 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="10075-151">`IISExpress` : Launches IIS Express.</span></span>
* <span data-ttu-id="10075-152">`IIS`：未啟動任何 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="10075-152">`IIS` : No web server launched.</span></span> <span data-ttu-id="10075-153">IIS 應該可以使用。</span><span class="sxs-lookup"><span data-stu-id="10075-153">IIS is expected to be available.</span></span>
* <span data-ttu-id="10075-154">`Project`：啟動 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="10075-154">`Project` : Launches Kestrel.</span></span>

<span data-ttu-id="10075-155">[Visual Studio 專案屬性] [**調試**] 索引標籤會提供 GUI 來編輯檔案*上的launchSettings.js* 。</span><span class="sxs-lookup"><span data-stu-id="10075-155">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file.</span></span> <span data-ttu-id="10075-156">您對專案設定檔所做的變更可能要等重新啟動網頁伺服器後才會生效。</span><span class="sxs-lookup"><span data-stu-id="10075-156">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="10075-157">您必須重新啟動 Kestrel，它才會偵測到環境已進行的變更。</span><span class="sxs-lookup"><span data-stu-id="10075-157">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

![專案屬性設定環境變數](environments/_static/project-properties-debug.png)

<span data-ttu-id="10075-159">下列*launchSettings.js*檔案包含多個設定檔：</span><span class="sxs-lookup"><span data-stu-id="10075-159">The following *launchSettings.json* file contains multiple profiles:</span></span>

[!code-json[](environments/3.1sample/EnvironmentsSample/Properties/launchSettings.json)]

<span data-ttu-id="10075-160">可以選取設定檔：</span><span class="sxs-lookup"><span data-stu-id="10075-160">Profiles can be selected:</span></span>

* <span data-ttu-id="10075-161">從 [Visual Studio] UI。</span><span class="sxs-lookup"><span data-stu-id="10075-161">From the Visual Studio UI.</span></span>
* <span data-ttu-id="10075-162">[`dotnet run`](/dotnet/core/tools/dotnet-run)在命令 shell 中使用命令，並將 `--launch-profile` 選項設定為設定檔的名稱。</span><span class="sxs-lookup"><span data-stu-id="10075-162">Using the [`dotnet run`](/dotnet/core/tools/dotnet-run) command in a command shell with the `--launch-profile` option set to the profile's name.</span></span> <span data-ttu-id="10075-163">*這個方法只支援 Kestrel 設定檔。*</span><span class="sxs-lookup"><span data-stu-id="10075-163">*This approach only supports Kestrel profiles.*</span></span>

  ```dotnetcli
  dotnet run --launch-profile "SampleApp"
  ```

> [!WARNING]
> <span data-ttu-id="10075-164">*launchSettings.json* 不應儲存密碼。</span><span class="sxs-lookup"><span data-stu-id="10075-164">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="10075-165">[密碼管理員工具](xref:security/app-secrets)可以用來儲存本機開發的密碼。</span><span class="sxs-lookup"><span data-stu-id="10075-165">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="10075-166">使用 [Visual Studio Code](https://code.visualstudio.com/) 時，可以會在 *.vscode/launch.json* 檔案設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="10075-166">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="10075-167">下列範例會設定數個[主機配置值環境變數](xref:fundamentals/host/web-host#host-configuration-values)：</span><span class="sxs-lookup"><span data-stu-id="10075-167">The following example sets several [Host configuration values environment variables](xref:fundamentals/host/web-host#host-configuration-values):</span></span>

[!code-json[](environments/3.1sample/EnvironmentsSample/.vscode/launch.json?range=4-10,32-38)]

<span data-ttu-id="10075-168">檔案的*vscode/launch.js*僅供 Visual Studio Code 使用。</span><span class="sxs-lookup"><span data-stu-id="10075-168">The *.vscode/launch.json* file is only used by Visual Studio Code.</span></span>

### <a name="production"></a><span data-ttu-id="10075-169">生產</span><span class="sxs-lookup"><span data-stu-id="10075-169">Production</span></span>

<span data-ttu-id="10075-170">應設定生產環境，以最大化安全性、[效能](xref:performance/performance-best-practices)和應用程式的穩定性。</span><span class="sxs-lookup"><span data-stu-id="10075-170">The production environment should be configured to maximize security, [performance](xref:performance/performance-best-practices), and application robustness.</span></span> <span data-ttu-id="10075-171">不同於開發的某些一般設定包括：</span><span class="sxs-lookup"><span data-stu-id="10075-171">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="10075-172">快[取。](xref:performance/caching/memory)</span><span class="sxs-lookup"><span data-stu-id="10075-172">[Caching](xref:performance/caching/memory).</span></span>
* <span data-ttu-id="10075-173">用戶端的資源會經過配套、縮減，且可能由 CDN 提供。</span><span class="sxs-lookup"><span data-stu-id="10075-173">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="10075-174">停用診斷錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="10075-174">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="10075-175">啟用易懂的錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="10075-175">Friendly error pages enabled.</span></span>
* <span data-ttu-id="10075-176">已啟用生產[記錄](xref:fundamentals/logging/index)和監視。</span><span class="sxs-lookup"><span data-stu-id="10075-176">Production [logging](xref:fundamentals/logging/index) and monitoring enabled.</span></span> <span data-ttu-id="10075-177">例如，使用[Application Insights](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="10075-177">For example, using [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="10075-178">設定環境</span><span class="sxs-lookup"><span data-stu-id="10075-178">Set the environment</span></span>

<span data-ttu-id="10075-179">設定特定環境以使用環境變數或平臺設定進行測試時，通常會很有用。</span><span class="sxs-lookup"><span data-stu-id="10075-179">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="10075-180">如果您未設定環境，它會預設為 `Production` 並停用大部分的偵錯功能。</span><span class="sxs-lookup"><span data-stu-id="10075-180">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="10075-181">環境的設定方法取決於作業系統而定。</span><span class="sxs-lookup"><span data-stu-id="10075-181">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="10075-182">建立主機時，應用程式所讀取的最後一個環境設定會決定應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="10075-182">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="10075-183">應用程式正在執行時，無法變更應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="10075-183">The app's environment can't be changed while the app is running.</span></span>

<span data-ttu-id="10075-184">[範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample)中的 [[關於] 頁面](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample/EnvironmentsSample/Pages/About.cshtml)會顯示的值 `IWebHostEnvironment.EnvironmentName` 。</span><span class="sxs-lookup"><span data-stu-id="10075-184">The [About page](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample/EnvironmentsSample/Pages/About.cshtml) from the [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample) displays the value of `IWebHostEnvironment.EnvironmentName`.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="10075-185">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="10075-185">Azure App Service</span></span>

<span data-ttu-id="10075-186">若要在 [Azure App Service](https://azure.microsoft.com/services/app-service/) 設定環境，請執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="10075-186">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="10075-187">從 [應用程式服務]\*\*\*\* 刀鋒視窗選取應用程式。</span><span class="sxs-lookup"><span data-stu-id="10075-187">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="10075-188">在 [**設定**] 群組中，**選取 [** 設定] 分頁。</span><span class="sxs-lookup"><span data-stu-id="10075-188">In the **Settings** group, select the **Configuration** blade.</span></span>
1. <span data-ttu-id="10075-189">在 [**應用程式設定**] 索引標籤中，選取 [**新增應用程式設定**]。</span><span class="sxs-lookup"><span data-stu-id="10075-189">In the **Application settings** tab, select **New application setting**.</span></span>
1. <span data-ttu-id="10075-190">在 [**新增/編輯應用程式設定**] 視窗中，提供 `ASPNETCORE_ENVIRONMENT` **名稱**的。</span><span class="sxs-lookup"><span data-stu-id="10075-190">In the **Add/Edit application setting** window, provide `ASPNETCORE_ENVIRONMENT` for the **Name**.</span></span> <span data-ttu-id="10075-191">針對 [**值**]，提供環境（例如 `Staging` ）。</span><span class="sxs-lookup"><span data-stu-id="10075-191">For **Value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="10075-192">如果您想要在交換部署位置時，將環境設定維持在目前的位置，請選取 [**部署位置設定**] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="10075-192">Select the **Deployment slot setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="10075-193">如需詳細資訊，請參閱 Azure 檔[中的在 Azure App Service 中設定預備環境](/azure/app-service/web-sites-staged-publishing)。</span><span class="sxs-lookup"><span data-stu-id="10075-193">For more information, see [Set up staging environments in Azure App Service](/azure/app-service/web-sites-staged-publishing) in the Azure documentation.</span></span>
1. <span data-ttu-id="10075-194">選取 **[確定]** 以關閉 [**新增/編輯應用程式設定**] 視窗。</span><span class="sxs-lookup"><span data-stu-id="10075-194">Select **OK** to close the **Add/Edit application setting** window.</span></span>
1. <span data-ttu-id="10075-195">選取 [設定] 分頁頂端的 **[** **儲存**]。</span><span class="sxs-lookup"><span data-stu-id="10075-195">Select **Save** at the top of the **Configuration** blade.</span></span>

<span data-ttu-id="10075-196">Azure App Service 在 Azure 入口網站中新增、變更或刪除應用程式設定後，自動重新開機應用程式。</span><span class="sxs-lookup"><span data-stu-id="10075-196">Azure App Service automatically restarts the app after an app setting is added, changed, or deleted in the Azure portal.</span></span>

### <a name="windows"></a><span data-ttu-id="10075-197">Windows</span><span class="sxs-lookup"><span data-stu-id="10075-197">Windows</span></span>

<span data-ttu-id="10075-198">*launchSettings.js*中的環境值會覆寫系統內容中所設定的覆寫值。</span><span class="sxs-lookup"><span data-stu-id="10075-198">Environment values in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="10075-199">如果應用程式是使用 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動，請使用下列命令來設定目前工作階段的 `ASPNETCORE_ENVIRONMENT`：</span><span class="sxs-lookup"><span data-stu-id="10075-199">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="10075-200">**命令提示字元**</span><span class="sxs-lookup"><span data-stu-id="10075-200">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Staging
dotnet run --no-launch-profile
```

<span data-ttu-id="10075-201">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="10075-201">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Staging"
dotnet run --no-launch-profile
```

<span data-ttu-id="10075-202">上述命令 `ASPNETCORE_ENVIRONMENT` 只會設定從該命令視窗啟動的進程。</span><span class="sxs-lookup"><span data-stu-id="10075-202">The preceding command sets `ASPNETCORE_ENVIRONMENT` only for processes launched from that command window.</span></span>

<span data-ttu-id="10075-203">若要在 Windows 中以全域的方式設定值，請使用下列其中一個方法：</span><span class="sxs-lookup"><span data-stu-id="10075-203">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="10075-204">開啟 [控制台]**[系統]** > **[進階系統設定]** > \*\*\*\*，然後新增或編輯 `ASPNETCORE_ENVIRONMENT` 值：</span><span class="sxs-lookup"><span data-stu-id="10075-204">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![系統進階屬性](environments/_static/systemsetting_environment.png)

  ![ASPNET Core 環境變數](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="10075-207">開啟系統管理命令提示字元，然後使用 `setx` 命令，或開啟系統管理 PowerShell 命令提示字元，然後使用 `[Environment]::SetEnvironmentVariable`：</span><span class="sxs-lookup"><span data-stu-id="10075-207">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="10075-208">**命令提示字元**</span><span class="sxs-lookup"><span data-stu-id="10075-208">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Staging /M
  ```

  <span data-ttu-id="10075-209">`/M` 參數表示將環境變數設定在系統層級。</span><span class="sxs-lookup"><span data-stu-id="10075-209">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="10075-210">若未使用 `/M` 參數，則將環境變數設定為使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="10075-210">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="10075-211">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="10075-211">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Staging", "Machine")
  ```

  <span data-ttu-id="10075-212">`Machine` 選項值表示將環境變數設定在系統層級。</span><span class="sxs-lookup"><span data-stu-id="10075-212">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="10075-213">若選項值變更為 `User`，則將環境變數設定為使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="10075-213">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="10075-214">當以全域的方式設定 `ASPNETCORE_ENVIRONMENT` 環境變數時，則在設定該值後開啟的任何命令視窗中，對 `dotnet run` 均有效。</span><span class="sxs-lookup"><span data-stu-id="10075-214">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span> <span data-ttu-id="10075-215">*launchSettings.js*中的環境值會覆寫系統內容中所設定的覆寫值。</span><span class="sxs-lookup"><span data-stu-id="10075-215">Environment values in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="10075-216">**web.config**</span><span class="sxs-lookup"><span data-stu-id="10075-216">**web.config**</span></span>

<span data-ttu-id="10075-217">若要使用 *web.config* 設定 `ASPNETCORE_ENVIRONMENT` 環境變數，請參閱 <xref:host-and-deploy/aspnet-core-module#setting-environment-variables> 的＜設定環境變數＞\*\* 一節。</span><span class="sxs-lookup"><span data-stu-id="10075-217">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="10075-218">**專案檔或發行設定檔**</span><span class="sxs-lookup"><span data-stu-id="10075-218">**Project file or publish profile**</span></span>

<span data-ttu-id="10075-219">**針對 WINDOWS IIS 部署：**`<EnvironmentName>`將屬性包含在[發行設定檔（. .pubxml）](xref:host-and-deploy/visual-studio-publish-profiles)或專案檔中。</span><span class="sxs-lookup"><span data-stu-id="10075-219">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="10075-220">此方法會在專案發行時於 *web.config* 中設定環境：</span><span class="sxs-lookup"><span data-stu-id="10075-220">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="10075-221">**每個 IIS 應用程式集區**</span><span class="sxs-lookup"><span data-stu-id="10075-221">**Per IIS Application Pool**</span></span>

<span data-ttu-id="10075-222">若要為執行於隔離應用程式集區 (受 IIS 10.0 或更新版本支援) 的應用程式設定 `ASPNETCORE_ENVIRONMENT` 環境變數，請參閱[環境變數 &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的＜AppCmd.exe 命令＞\*\* 一節。</span><span class="sxs-lookup"><span data-stu-id="10075-222">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="10075-223">當 `ASPNETCORE_ENVIRONMENT` 環境變數設定為應用程式集區時，其值會覆寫系統層級的設定。</span><span class="sxs-lookup"><span data-stu-id="10075-223">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

<span data-ttu-id="10075-224">當在 IIS 中裝載應用程式，並新增或變更 `ASPNETCORE_ENVIRONMENT` 環境變數時，請使用下列任一種方法，讓應用程式挑選新的值：</span><span class="sxs-lookup"><span data-stu-id="10075-224">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>

* <span data-ttu-id="10075-225">從命令提示字元執行後面接著 `net start w3svc` 的 `net stop was /y`。</span><span class="sxs-lookup"><span data-stu-id="10075-225">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
* <span data-ttu-id="10075-226">重新啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="10075-226">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="10075-227">macOS</span><span class="sxs-lookup"><span data-stu-id="10075-227">macOS</span></span>

<span data-ttu-id="10075-228">您可以在執行應用程式時，以內嵌方式設定 macOS 目前的環境：</span><span class="sxs-lookup"><span data-stu-id="10075-228">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Staging dotnet run
```

<span data-ttu-id="10075-229">或者，在執行應用程式之前，使用 `export` 設定環境：</span><span class="sxs-lookup"><span data-stu-id="10075-229">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Staging
```

<span data-ttu-id="10075-230">您可以在 *.bashrc* 或 *.bash_profile* 檔案中設定電腦層級的環境變數。</span><span class="sxs-lookup"><span data-stu-id="10075-230">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="10075-231">使用任何文字編輯器編輯檔案。</span><span class="sxs-lookup"><span data-stu-id="10075-231">Edit the file using any text editor.</span></span> <span data-ttu-id="10075-232">新增下列陳述式：</span><span class="sxs-lookup"><span data-stu-id="10075-232">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Staging
```

#### <a name="linux"></a><span data-ttu-id="10075-233">Linux</span><span class="sxs-lookup"><span data-stu-id="10075-233">Linux</span></span>

<span data-ttu-id="10075-234">針對 Linux 散發套件，請 `export` 在命令提示字元中使用命令來進行會話型變數設定，並針對電腦層級的環境設定*bash_profile*檔案。</span><span class="sxs-lookup"><span data-stu-id="10075-234">For Linux distributions, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="10075-235">在程式碼中設定環境</span><span class="sxs-lookup"><span data-stu-id="10075-235">Set the environment in code</span></span>

<span data-ttu-id="10075-236"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*>在建立主機時呼叫。</span><span class="sxs-lookup"><span data-stu-id="10075-236">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="10075-237">請參閱＜ <xref:fundamentals/host/generic-host#environmentname> ＞。</span><span class="sxs-lookup"><span data-stu-id="10075-237">See <xref:fundamentals/host/generic-host#environmentname>.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="10075-238">取決於環境的組態</span><span class="sxs-lookup"><span data-stu-id="10075-238">Configuration by environment</span></span>

<span data-ttu-id="10075-239">若要依環境載入設定，請參閱 <xref:fundamentals/configuration/index#json-configuration-provider> 。</span><span class="sxs-lookup"><span data-stu-id="10075-239">To load configuration by environment, see <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="10075-240">以環境為基礎的 Startup 類別和方法</span><span class="sxs-lookup"><span data-stu-id="10075-240">Environment-based Startup class and methods</span></span>

### <a name="inject-iwebhostenvironment-into-the-startup-class"></a><span data-ttu-id="10075-241">將 IWebHostEnvironment 插入 Startup 類別</span><span class="sxs-lookup"><span data-stu-id="10075-241">Inject IWebHostEnvironment into the Startup class</span></span>

<span data-ttu-id="10075-242">插入 <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> 至此函式 `Startup` 。</span><span class="sxs-lookup"><span data-stu-id="10075-242">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into the `Startup` constructor.</span></span> <span data-ttu-id="10075-243">當應用程式 `Startup` 只需要針對少數環境進行設定，而且每個環境的程式碼差異最低時，這個方法就很有用。</span><span class="sxs-lookup"><span data-stu-id="10075-243">This approach is useful when the app requires configuring `Startup` for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="10075-244">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="10075-244">In the following example:</span></span>

* <span data-ttu-id="10075-245">環境會保留在欄位中 `_env` 。</span><span class="sxs-lookup"><span data-stu-id="10075-245">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="10075-246">`_env`會在和中使用 `ConfigureServices` ， `Configure` 以根據應用程式的環境來套用啟動設定。</span><span class="sxs-lookup"><span data-stu-id="10075-246">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

[!code-csharp[](environments/3.1sample/EnvironmentsSample/StartupInject.cs?name=snippet&highlight=3-11)]

### <a name="startup-class-conventions"></a><span data-ttu-id="10075-247">Startup 類別慣例</span><span class="sxs-lookup"><span data-stu-id="10075-247">Startup class conventions</span></span>

<span data-ttu-id="10075-248">ASP.NET Core 應用程式啟動時，[Startup 類別](xref:fundamentals/startup)會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="10075-248">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="10075-249">應用程式可以 `Startup` 針對不同的環境定義多個類別。</span><span class="sxs-lookup"><span data-stu-id="10075-249">The app can define multiple `Startup` classes for different environments.</span></span> <span data-ttu-id="10075-250">在 `Startup` 執行時間選取適當的類別。</span><span class="sxs-lookup"><span data-stu-id="10075-250">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="10075-251">將優先使用其名稱尾碼符合目前環境的類別。</span><span class="sxs-lookup"><span data-stu-id="10075-251">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="10075-252">如果找不到相符的 `Startup{EnvironmentName}` 類別，會使用 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="10075-252">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="10075-253">當應用程式需要針對數個環境設定啟動，且每個環境有許多程式碼差異時，此方法很有用。</span><span class="sxs-lookup"><span data-stu-id="10075-253">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span> <span data-ttu-id="10075-254">一般的應用程式不需要這種方法。</span><span class="sxs-lookup"><span data-stu-id="10075-254">Typical apps will not need this approach.</span></span>

<span data-ttu-id="10075-255">若要執行以環境為基礎的 `Startup` 類別，請建立 `Startup{EnvironmentName}` 類別和 fallback `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="10075-255">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` classes and a fallback `Startup` class:</span></span>

[!code-csharp[](environments/3.1sample/EnvironmentsSample/StartupClassConventions.cs?name=snippet)]

<span data-ttu-id="10075-256">使用接受組件名稱的 [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) 多載：</span><span class="sxs-lookup"><span data-stu-id="10075-256">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

[!code-csharp[](environments/3.1sample/EnvironmentsSample/Program.cs?name=snippet)]

### <a name="startup-method-conventions"></a><span data-ttu-id="10075-257">Startup 方法慣例</span><span class="sxs-lookup"><span data-stu-id="10075-257">Startup method conventions</span></span>

<span data-ttu-id="10075-258">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure)和[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices)支援表單和的環境特定版本 `Configure<EnvironmentName>` `Configure<EnvironmentName>Services` 。</span><span class="sxs-lookup"><span data-stu-id="10075-258">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="10075-259">當應用程式需要針對數個環境設定啟動，且每個環境有許多程式碼差異時，這個方法會很有用：</span><span class="sxs-lookup"><span data-stu-id="10075-259">This approach is useful when the app requires configuring startup for several environments with many code differences per environment:</span></span>

[!code-csharp[](environments/3.1sample/EnvironmentsSample/StartupMethodConventions.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="10075-260">其他資源</span><span class="sxs-lookup"><span data-stu-id="10075-260">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
* <xref:blazor/fundamentals/environments>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="10075-261">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="10075-261">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="10075-262">ASP.NET Core 會使用環境變數根據執行階段環境來設定應用程式行為。</span><span class="sxs-lookup"><span data-stu-id="10075-262">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="10075-263">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="10075-263">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="10075-264">環境</span><span class="sxs-lookup"><span data-stu-id="10075-264">Environments</span></span>

<span data-ttu-id="10075-265">ASP.NET Core 會在應用程式啟動時讀取 `ASPNETCORE_ENVIRONMENT` 環境變數，並將該值儲存在 [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName) 中。</span><span class="sxs-lookup"><span data-stu-id="10075-265">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span></span> <span data-ttu-id="10075-266">`ASPNETCORE_ENVIRONMENT`可以設定為任何值，但架構會提供三個值：</span><span class="sxs-lookup"><span data-stu-id="10075-266">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Staging>
* <span data-ttu-id="10075-267"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (預設值)</span><span class="sxs-lookup"><span data-stu-id="10075-267"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="10075-268">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="10075-268">The preceding code:</span></span>

* <span data-ttu-id="10075-269">當 `ASPNETCORE_ENVIRONMENT` 設為 `Development` 時，會呼叫 [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage)。</span><span class="sxs-lookup"><span data-stu-id="10075-269">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="10075-270">當 `ASPNETCORE_ENVIRONMENT` 的值設為下列其中之一時，會呼叫 [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler)：</span><span class="sxs-lookup"><span data-stu-id="10075-270">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="10075-271">[環境標記](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)協助程式會使用的值 `IHostingEnvironment.EnvironmentName` 來包含或排除元素中的標記：</span><span class="sxs-lookup"><span data-stu-id="10075-271">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="10075-272">在 Windows 和 macOS 上，環境變數和值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="10075-272">On Windows and macOS, environment variables and values aren't case-sensitive.</span></span> <span data-ttu-id="10075-273">根據預設，Linux 環境變數和值會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="10075-273">Linux environment variables and values are case-sensitive by default.</span></span>

### <a name="development"></a><span data-ttu-id="10075-274">部署</span><span class="sxs-lookup"><span data-stu-id="10075-274">Development</span></span>

<span data-ttu-id="10075-275">您可以在開發環境中啟用生產環境不應該公開的功能。</span><span class="sxs-lookup"><span data-stu-id="10075-275">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="10075-276">例如，ASP.NET Core 範本會在開發環境中啟用[開發人員例外狀況頁面](xref:fundamentals/error-handling#developer-exception-page)。</span><span class="sxs-lookup"><span data-stu-id="10075-276">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="10075-277">您可以在專案的 *Properties\launchSettings.json* 檔案設定適用於本機電腦開發的環境。</span><span class="sxs-lookup"><span data-stu-id="10075-277">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="10075-278">在 *launchSettings.json* 中設定的環境值會覆寫系統環境的設定值。</span><span class="sxs-lookup"><span data-stu-id="10075-278">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="10075-279">下列 JSON 顯示來自 *launchSettings.json* 檔案的三個設定檔：</span><span class="sxs-lookup"><span data-stu-id="10075-279">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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
> <span data-ttu-id="10075-280">*launchSettings.json* 中的 `applicationUrl` 屬性可以指定伺服器 URL 的清單。</span><span class="sxs-lookup"><span data-stu-id="10075-280">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="10075-281">請在清單的 URL 之間使用分號：</span><span class="sxs-lookup"><span data-stu-id="10075-281">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="10075-282">以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動應用程式時，會使用具有 `"commandName": "Project"` 的第一個設定檔。</span><span class="sxs-lookup"><span data-stu-id="10075-282">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="10075-283">`commandName` 的值可指定要啟動的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="10075-283">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="10075-284">`commandName` 可以是下列任何一個項目：</span><span class="sxs-lookup"><span data-stu-id="10075-284">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="10075-285">`Project` (這會啟動 Kestrel)</span><span class="sxs-lookup"><span data-stu-id="10075-285">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="10075-286">以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動應用程式時：</span><span class="sxs-lookup"><span data-stu-id="10075-286">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="10075-287">會讀取 *launchSettings.json* (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="10075-287">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="10075-288">*launchSettings.json* 中的 `environmentVariables` 設定會覆寫環境變數。</span><span class="sxs-lookup"><span data-stu-id="10075-288">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="10075-289">主控環境隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="10075-289">The hosting environment is displayed.</span></span>

<span data-ttu-id="10075-290">下列輸出顯示應用程式是以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動：</span><span class="sxs-lookup"><span data-stu-id="10075-290">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="10075-291">Visual Studio 專案屬性的 [偵錯]\*\*\*\* 索引標籤提供 GUI，可編輯 *launchSettings.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="10075-291">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![專案屬性設定環境變數](environments/_static/project-properties-debug.png)

<span data-ttu-id="10075-293">您對專案設定檔所做的變更可能要等重新啟動網頁伺服器後才會生效。</span><span class="sxs-lookup"><span data-stu-id="10075-293">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="10075-294">您必須重新啟動 Kestrel，它才會偵測到環境已進行的變更。</span><span class="sxs-lookup"><span data-stu-id="10075-294">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="10075-295">*launchSettings.json* 不應儲存密碼。</span><span class="sxs-lookup"><span data-stu-id="10075-295">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="10075-296">[密碼管理員工具](xref:security/app-secrets)可以用來儲存本機開發的密碼。</span><span class="sxs-lookup"><span data-stu-id="10075-296">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="10075-297">使用 [Visual Studio Code](https://code.visualstudio.com/) 時，可以會在 *.vscode/launch.json* 檔案設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="10075-297">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="10075-298">下列範例會將環境設定為 `Development`：</span><span class="sxs-lookup"><span data-stu-id="10075-298">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="10075-299">使用 `dotnet run` 啟動應用程式時，不會如同 *Properties/launchSettings.json* 一樣讀取專案中的 *.vscode/launch.json* 檔。</span><span class="sxs-lookup"><span data-stu-id="10075-299">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="10075-300">開發時若啟動的應用程式沒有 *launchSettings.json* 檔，請使用環境變數設定環境，或是使用 `dotnet run` 命令的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="10075-300">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="10075-301">生產</span><span class="sxs-lookup"><span data-stu-id="10075-301">Production</span></span>

<span data-ttu-id="10075-302">若要將安全性、效能及應用程式加強性最大化，您應該設定生產環境。</span><span class="sxs-lookup"><span data-stu-id="10075-302">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="10075-303">不同於開發的某些一般設定包括：</span><span class="sxs-lookup"><span data-stu-id="10075-303">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="10075-304">快取。</span><span class="sxs-lookup"><span data-stu-id="10075-304">Caching.</span></span>
* <span data-ttu-id="10075-305">用戶端的資源會經過配套、縮減，且可能由 CDN 提供。</span><span class="sxs-lookup"><span data-stu-id="10075-305">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="10075-306">停用診斷錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="10075-306">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="10075-307">啟用易懂的錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="10075-307">Friendly error pages enabled.</span></span>
* <span data-ttu-id="10075-308">啟用生產記錄與監視。</span><span class="sxs-lookup"><span data-stu-id="10075-308">Production logging and monitoring enabled.</span></span> <span data-ttu-id="10075-309">例如， [Application Insights](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="10075-309">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="10075-310">設定環境</span><span class="sxs-lookup"><span data-stu-id="10075-310">Set the environment</span></span>

<span data-ttu-id="10075-311">設定特定環境以使用環境變數或平臺設定進行測試時，通常會很有用。</span><span class="sxs-lookup"><span data-stu-id="10075-311">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="10075-312">如果您未設定環境，它會預設為 `Production` 並停用大部分的偵錯功能。</span><span class="sxs-lookup"><span data-stu-id="10075-312">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="10075-313">環境的設定方法取決於作業系統而定。</span><span class="sxs-lookup"><span data-stu-id="10075-313">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="10075-314">建立主機時，應用程式所讀取的最後一個環境設定會決定應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="10075-314">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="10075-315">應用程式正在執行時，無法變更應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="10075-315">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="10075-316">環境變數或平臺設定</span><span class="sxs-lookup"><span data-stu-id="10075-316">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="10075-317">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="10075-317">Azure App Service</span></span>

<span data-ttu-id="10075-318">若要在 [Azure App Service](https://azure.microsoft.com/services/app-service/) 設定環境，請執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="10075-318">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="10075-319">從 [應用程式服務]\*\*\*\* 刀鋒視窗選取應用程式。</span><span class="sxs-lookup"><span data-stu-id="10075-319">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="10075-320">在 [**設定**] 群組中，**選取 [** 設定] 分頁。</span><span class="sxs-lookup"><span data-stu-id="10075-320">In the **Settings** group, select the **Configuration** blade.</span></span>
1. <span data-ttu-id="10075-321">在 [**應用程式設定**] 索引標籤中，選取 [**新增應用程式設定**]。</span><span class="sxs-lookup"><span data-stu-id="10075-321">In the **Application settings** tab, select **New application setting**.</span></span>
1. <span data-ttu-id="10075-322">在 [**新增/編輯應用程式設定**] 視窗中，提供 `ASPNETCORE_ENVIRONMENT` **名稱**的。</span><span class="sxs-lookup"><span data-stu-id="10075-322">In the **Add/Edit application setting** window, provide `ASPNETCORE_ENVIRONMENT` for the **Name**.</span></span> <span data-ttu-id="10075-323">針對 [**值**]，提供環境（例如 `Staging` ）。</span><span class="sxs-lookup"><span data-stu-id="10075-323">For **Value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="10075-324">如果您想要在交換部署位置時，將環境設定維持在目前的位置，請選取 [**部署位置設定**] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="10075-324">Select the **Deployment slot setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="10075-325">如需詳細資訊，請參閱 Azure 檔[中的在 Azure App Service 中設定預備環境](/azure/app-service/web-sites-staged-publishing)。</span><span class="sxs-lookup"><span data-stu-id="10075-325">For more information, see [Set up staging environments in Azure App Service](/azure/app-service/web-sites-staged-publishing) in the Azure documentation.</span></span>
1. <span data-ttu-id="10075-326">選取 **[確定]** 以關閉 [**新增/編輯應用程式設定**] 視窗。</span><span class="sxs-lookup"><span data-stu-id="10075-326">Select **OK** to close the **Add/Edit application setting** window.</span></span>
1. <span data-ttu-id="10075-327">選取 [設定] 分頁頂端的 **[** **儲存**]。</span><span class="sxs-lookup"><span data-stu-id="10075-327">Select **Save** at the top of the **Configuration** blade.</span></span>

<span data-ttu-id="10075-328">Azure App Service 會在新增、變更或刪除 Azure 入口網站的應用程式設定 (環境變數) 之後自動重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="10075-328">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="10075-329">Windows</span><span class="sxs-lookup"><span data-stu-id="10075-329">Windows</span></span>

<span data-ttu-id="10075-330">如果應用程式是使用 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動，請使用下列命令來設定目前工作階段的 `ASPNETCORE_ENVIRONMENT`：</span><span class="sxs-lookup"><span data-stu-id="10075-330">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="10075-331">**命令提示字元**</span><span class="sxs-lookup"><span data-stu-id="10075-331">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="10075-332">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="10075-332">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="10075-333">這些命令僅針對目前的視窗才會生效。</span><span class="sxs-lookup"><span data-stu-id="10075-333">These commands only take effect for the current window.</span></span> <span data-ttu-id="10075-334">視窗關閉時，`ASPNETCORE_ENVIRONMENT` 設定會還原為預設值或電腦值。</span><span class="sxs-lookup"><span data-stu-id="10075-334">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="10075-335">若要在 Windows 中以全域的方式設定值，請使用下列其中一個方法：</span><span class="sxs-lookup"><span data-stu-id="10075-335">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="10075-336">開啟 [控制台]**[系統]** > **[進階系統設定]** > \*\*\*\*，然後新增或編輯 `ASPNETCORE_ENVIRONMENT` 值：</span><span class="sxs-lookup"><span data-stu-id="10075-336">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![系統進階屬性](environments/_static/systemsetting_environment.png)

  ![ASPNET Core 環境變數](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="10075-339">開啟系統管理命令提示字元，然後使用 `setx` 命令，或開啟系統管理 PowerShell 命令提示字元，然後使用 `[Environment]::SetEnvironmentVariable`：</span><span class="sxs-lookup"><span data-stu-id="10075-339">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="10075-340">**命令提示字元**</span><span class="sxs-lookup"><span data-stu-id="10075-340">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="10075-341">`/M` 參數表示將環境變數設定在系統層級。</span><span class="sxs-lookup"><span data-stu-id="10075-341">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="10075-342">若未使用 `/M` 參數，則將環境變數設定為使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="10075-342">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="10075-343">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="10075-343">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="10075-344">`Machine` 選項值表示將環境變數設定在系統層級。</span><span class="sxs-lookup"><span data-stu-id="10075-344">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="10075-345">若選項值變更為 `User`，則將環境變數設定為使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="10075-345">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="10075-346">當以全域的方式設定 `ASPNETCORE_ENVIRONMENT` 環境變數時，則在設定該值後開啟的任何命令視窗中，對 `dotnet run` 均有效。</span><span class="sxs-lookup"><span data-stu-id="10075-346">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="10075-347">**web.config**</span><span class="sxs-lookup"><span data-stu-id="10075-347">**web.config**</span></span>

<span data-ttu-id="10075-348">若要使用 *web.config* 設定 `ASPNETCORE_ENVIRONMENT` 環境變數，請參閱 <xref:host-and-deploy/aspnet-core-module#setting-environment-variables> 的＜設定環境變數＞\*\* 一節。</span><span class="sxs-lookup"><span data-stu-id="10075-348">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="10075-349">**專案檔或發行設定檔**</span><span class="sxs-lookup"><span data-stu-id="10075-349">**Project file or publish profile**</span></span>

<span data-ttu-id="10075-350">**針對 WINDOWS IIS 部署：**`<EnvironmentName>`將屬性包含在[發行設定檔（. .pubxml）](xref:host-and-deploy/visual-studio-publish-profiles)或專案檔中。</span><span class="sxs-lookup"><span data-stu-id="10075-350">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="10075-351">此方法會在專案發行時於 *web.config* 中設定環境：</span><span class="sxs-lookup"><span data-stu-id="10075-351">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="10075-352">**每個 IIS 應用程式集區**</span><span class="sxs-lookup"><span data-stu-id="10075-352">**Per IIS Application Pool**</span></span>

<span data-ttu-id="10075-353">若要為執行於隔離應用程式集區 (受 IIS 10.0 或更新版本支援) 的應用程式設定 `ASPNETCORE_ENVIRONMENT` 環境變數，請參閱[環境變數 &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的＜AppCmd.exe 命令＞\*\* 一節。</span><span class="sxs-lookup"><span data-stu-id="10075-353">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="10075-354">當 `ASPNETCORE_ENVIRONMENT` 環境變數設定為應用程式集區時，其值會覆寫系統層級的設定。</span><span class="sxs-lookup"><span data-stu-id="10075-354">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="10075-355">當在 IIS 中裝載應用程式，並新增或變更 `ASPNETCORE_ENVIRONMENT` 環境變數時，請使用下列任一種方法，讓應用程式挑選新的值：</span><span class="sxs-lookup"><span data-stu-id="10075-355">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="10075-356">從命令提示字元執行後面接著 `net start w3svc` 的 `net stop was /y`。</span><span class="sxs-lookup"><span data-stu-id="10075-356">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="10075-357">重新啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="10075-357">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="10075-358">macOS</span><span class="sxs-lookup"><span data-stu-id="10075-358">macOS</span></span>

<span data-ttu-id="10075-359">您可以在執行應用程式時，以內嵌方式設定 macOS 目前的環境：</span><span class="sxs-lookup"><span data-stu-id="10075-359">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="10075-360">或者，在執行應用程式之前，使用 `export` 設定環境：</span><span class="sxs-lookup"><span data-stu-id="10075-360">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="10075-361">您可以在 *.bashrc* 或 *.bash_profile* 檔案中設定電腦層級的環境變數。</span><span class="sxs-lookup"><span data-stu-id="10075-361">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="10075-362">使用任何文字編輯器編輯檔案。</span><span class="sxs-lookup"><span data-stu-id="10075-362">Edit the file using any text editor.</span></span> <span data-ttu-id="10075-363">新增下列陳述式：</span><span class="sxs-lookup"><span data-stu-id="10075-363">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="10075-364">Linux</span><span class="sxs-lookup"><span data-stu-id="10075-364">Linux</span></span>

<span data-ttu-id="10075-365">針對 Linux 散發套件，請 `export` 在命令提示字元中使用命令來進行會話型變數設定，並針對電腦層級的環境設定*bash_profile*檔案。</span><span class="sxs-lookup"><span data-stu-id="10075-365">For Linux distributions, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="10075-366">在程式碼中設定環境</span><span class="sxs-lookup"><span data-stu-id="10075-366">Set the environment in code</span></span>

<span data-ttu-id="10075-367"><xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*>在建立主機時呼叫。</span><span class="sxs-lookup"><span data-stu-id="10075-367">Call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="10075-368">請參閱＜ <xref:fundamentals/host/web-host#environment> ＞。</span><span class="sxs-lookup"><span data-stu-id="10075-368">See <xref:fundamentals/host/web-host#environment>.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="10075-369">取決於環境的組態</span><span class="sxs-lookup"><span data-stu-id="10075-369">Configuration by environment</span></span>

<span data-ttu-id="10075-370">若要依環境載入組態，建議使用：</span><span class="sxs-lookup"><span data-stu-id="10075-370">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="10075-371">*appsettings* files （*appsettings. {環境}. json*）。</span><span class="sxs-lookup"><span data-stu-id="10075-371">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="10075-372">請參閱＜ <xref:fundamentals/configuration/index#json-configuration-provider> ＞。</span><span class="sxs-lookup"><span data-stu-id="10075-372">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="10075-373">環境變數（在裝載應用程式的每個系統上設定）。</span><span class="sxs-lookup"><span data-stu-id="10075-373">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="10075-374">請參閱 <xref:fundamentals/host/web-host#environment> 和 <xref:security/app-secrets#environment-variables>。</span><span class="sxs-lookup"><span data-stu-id="10075-374">See <xref:fundamentals/host/web-host#environment> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="10075-375">祕密管理員 (僅限開發環境)。</span><span class="sxs-lookup"><span data-stu-id="10075-375">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="10075-376">請參閱＜ <xref:security/app-secrets> ＞。</span><span class="sxs-lookup"><span data-stu-id="10075-376">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="10075-377">以環境為基礎的 Startup 類別和方法</span><span class="sxs-lookup"><span data-stu-id="10075-377">Environment-based Startup class and methods</span></span>

### <a name="inject-ihostingenvironment-into-startupconfigure"></a><span data-ttu-id="10075-378">將 IHostingEnvironment 插入 Startup.Configu</span><span class="sxs-lookup"><span data-stu-id="10075-378">Inject IHostingEnvironment into Startup.Configure</span></span>

<span data-ttu-id="10075-379">插入 <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="10075-379">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="10075-380">當應用程式只需要 `Startup.Configure` 針對每個環境具有最低程式碼差異的幾個環境進行設定時，這個方法就很有用。</span><span class="sxs-lookup"><span data-stu-id="10075-380">This approach is useful when the app only requires configuring `Startup.Configure` for only a few environments with minimal code differences per environment.</span></span>

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

### <a name="inject-ihostingenvironment-into-the-startup-class"></a><span data-ttu-id="10075-381">將 IHostingEnvironment 插入 Startup 類別</span><span class="sxs-lookup"><span data-stu-id="10075-381">Inject IHostingEnvironment into the Startup class</span></span>

<span data-ttu-id="10075-382">將插入 <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> 至此函式 `Startup` ，並將服務指派給欄位，以便在整個類別中使用 `Startup` 。</span><span class="sxs-lookup"><span data-stu-id="10075-382">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into the `Startup` constructor and assign the service to a field for use throughout the `Startup` class.</span></span> <span data-ttu-id="10075-383">當應用程式需要針對少數環境設定啟動，但每個環境的程式碼差異最低時，這個方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="10075-383">This approach is useful when the app requires configuring startup for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="10075-384">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="10075-384">In the following example:</span></span>

* <span data-ttu-id="10075-385">環境會保留在欄位中 `_env` 。</span><span class="sxs-lookup"><span data-stu-id="10075-385">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="10075-386">`_env`會在和中使用 `ConfigureServices` ， `Configure` 以根據應用程式的環境來套用啟動設定。</span><span class="sxs-lookup"><span data-stu-id="10075-386">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

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

### <a name="startup-class-conventions"></a><span data-ttu-id="10075-387">Startup 類別慣例</span><span class="sxs-lookup"><span data-stu-id="10075-387">Startup class conventions</span></span>

<span data-ttu-id="10075-388">ASP.NET Core 應用程式啟動時，[Startup 類別](xref:fundamentals/startup)會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="10075-388">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="10075-389">應用程式可以 `Startup` 針對不同的環境定義個別的類別（例如， `StartupDevelopment` ）。</span><span class="sxs-lookup"><span data-stu-id="10075-389">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="10075-390">在 `Startup` 執行時間選取適當的類別。</span><span class="sxs-lookup"><span data-stu-id="10075-390">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="10075-391">將優先使用其名稱尾碼符合目前環境的類別。</span><span class="sxs-lookup"><span data-stu-id="10075-391">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="10075-392">如果找不到相符的 `Startup{EnvironmentName}` 類別，會使用 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="10075-392">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="10075-393">當應用程式需要針對數個環境設定啟動，且每個環境有許多程式碼差異時，此方法很有用。</span><span class="sxs-lookup"><span data-stu-id="10075-393">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="10075-394">若要實作以環境為基礎的 `Startup` 類別，請為每個使用中的環境建立 `Startup{EnvironmentName}` 類別和後援 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="10075-394">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

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

<span data-ttu-id="10075-395">使用接受組件名稱的 [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) 多載：</span><span class="sxs-lookup"><span data-stu-id="10075-395">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

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

### <a name="startup-method-conventions"></a><span data-ttu-id="10075-396">Startup 方法慣例</span><span class="sxs-lookup"><span data-stu-id="10075-396">Startup method conventions</span></span>

<span data-ttu-id="10075-397">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure)和[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices)支援表單和的環境特定版本 `Configure<EnvironmentName>` `Configure<EnvironmentName>Services` 。</span><span class="sxs-lookup"><span data-stu-id="10075-397">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="10075-398">當應用程式需要針對數個環境設定啟動，且每個環境有許多程式碼差異時，此方法很有用。</span><span class="sxs-lookup"><span data-stu-id="10075-398">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="10075-399">其他資源</span><span class="sxs-lookup"><span data-stu-id="10075-399">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>

::: moniker-end
