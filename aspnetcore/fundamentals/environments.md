---
title: "使用多個環境"
author: ardalis
description: 
keywords: "ASP.NET Core，環境設定，ASPNETCORE_ENVIRONMENT"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b5bba985-be12-4464-9a01-df3599b2a6f1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 8f8612fd9c92370d9b0a86572dca654a6a034916
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="working-with-multiple-environments"></a><span data-ttu-id="0efaa-103">使用多個環境</span><span class="sxs-lookup"><span data-stu-id="0efaa-103">Working with multiple environments</span></span>

<span data-ttu-id="0efaa-104">由[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="0efaa-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="0efaa-105">ASP.NET Core 提供支援跨多個環境，例如開發、 預備及生產環境中控制應用程式行為。</span><span class="sxs-lookup"><span data-stu-id="0efaa-105">ASP.NET Core provides support for controlling app behavior across multiple environments, such as development, staging, and production.</span></span> <span data-ttu-id="0efaa-106">表示執行階段環境中，允許應用程式設定為該環境使用環境變數。</span><span class="sxs-lookup"><span data-stu-id="0efaa-106">Environment variables are used to indicate the runtime environment, allowing the app to be configured for that environment.</span></span>

[<span data-ttu-id="0efaa-107">檢視或下載範例程式碼</span><span class="sxs-lookup"><span data-stu-id="0efaa-107">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample)

## <a name="development-staging-production"></a><span data-ttu-id="0efaa-108">開發、 暫存、 生產環境</span><span class="sxs-lookup"><span data-stu-id="0efaa-108">Development, Staging, Production</span></span>

<span data-ttu-id="0efaa-109">ASP.NET Core 參考特定[環境變數](https://github.com/aspnet/Home/wiki)，`ASPNETCORE_ENVIRONMENT`來描述應用程式目前執行中的環境。</span><span class="sxs-lookup"><span data-stu-id="0efaa-109">ASP.NET Core references a particular [environment variable](https://github.com/aspnet/Home/wiki), `ASPNETCORE_ENVIRONMENT` to describe the environment the application is currently running in.</span></span> <span data-ttu-id="0efaa-110">此變數可以設定任何值，但是慣例會使用三個值： `Development`， `Staging`，和`Production`。</span><span class="sxs-lookup"><span data-stu-id="0efaa-110">This variable can be set to any value you like, but three values are used by convention: `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="0efaa-111">您會發現這些範例中所使用的值和 ASP.NET Core 提供的範本。</span><span class="sxs-lookup"><span data-stu-id="0efaa-111">You will find these values used in the samples and templates provided with ASP.NET Core.</span></span>

<span data-ttu-id="0efaa-112">目前的環境設定可以偵測到以程式設計方式從您的應用程式內。</span><span class="sxs-lookup"><span data-stu-id="0efaa-112">The current environment setting can be detected programmatically from within your application.</span></span> <span data-ttu-id="0efaa-113">此外，您可以使用環境[標記協助程式](../mvc/views/tag-helpers/index.md)中的特定區段中加入您[檢視](../mvc/views/index.md)根據目前的應用程式環境。</span><span class="sxs-lookup"><span data-stu-id="0efaa-113">In addition, you can use the Environment [tag helper](../mvc/views/tag-helpers/index.md) to include certain sections in your [view](../mvc/views/index.md) based on the current application environment.</span></span>

<span data-ttu-id="0efaa-114">注意： 在 Windows 及 macOS，指定的環境名稱是不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="0efaa-114">Note: On Windows and macOS, the specified environment name is case insensitive.</span></span> <span data-ttu-id="0efaa-115">是否將變數設`Development`或`development`或`DEVELOPMENT`結果將會相同。</span><span class="sxs-lookup"><span data-stu-id="0efaa-115">Whether you set the variable to `Development` or `development` or `DEVELOPMENT` the results will be the same.</span></span> <span data-ttu-id="0efaa-116">不過，Linux 是**區分大小寫**預設的作業系統。</span><span class="sxs-lookup"><span data-stu-id="0efaa-116">However, Linux is a **case sensitive** OS by default.</span></span> <span data-ttu-id="0efaa-117">環境變數、 檔案名稱和設定需要區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="0efaa-117">Environment variables, file names and settings require case sensitivity.</span></span>

### <a name="development"></a><span data-ttu-id="0efaa-118">開發</span><span class="sxs-lookup"><span data-stu-id="0efaa-118">Development</span></span>

<span data-ttu-id="0efaa-119">這應該是開發應用程式時使用的環境。</span><span class="sxs-lookup"><span data-stu-id="0efaa-119">This should be the environment used when developing an application.</span></span> <span data-ttu-id="0efaa-120">它通常用來啟用功能，您不想要可供使用，當應用程式執行於生產環境，例如[開發人員例外狀況頁面](xref:fundamentals/error-handling#the-developer-exception-page)。</span><span class="sxs-lookup"><span data-stu-id="0efaa-120">It is typically used to enable features that you wouldn't want to be available when the app runs in production, such as the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page).</span></span>

<span data-ttu-id="0efaa-121">如果您使用 Visual Studio，可以在專案的偵錯設定檔中設定環境。</span><span class="sxs-lookup"><span data-stu-id="0efaa-121">If you're using Visual Studio, the environment can be configured in your project's debug profiles.</span></span> <span data-ttu-id="0efaa-122">偵錯設定檔指定[伺服器](xref:fundamentals/servers/index)来設定啟動應用程式和任何環境變數時使用。</span><span class="sxs-lookup"><span data-stu-id="0efaa-122">Debug profiles specify the [server](xref:fundamentals/servers/index) to use when launching the application and any environment variables to be set.</span></span> <span data-ttu-id="0efaa-123">您的專案可以有多個以不同方式設定環境變數的偵錯設定檔。</span><span class="sxs-lookup"><span data-stu-id="0efaa-123">Your project can have multiple debug profiles that set environment variables differently.</span></span> <span data-ttu-id="0efaa-124">使用管理這些設定檔**偵錯**] 索引標籤的 [web 應用程式專案的**屬性**功能表。</span><span class="sxs-lookup"><span data-stu-id="0efaa-124">You manage these profiles by using the **Debug** tab of your web application project's **Properties** menu.</span></span> <span data-ttu-id="0efaa-125">您在專案屬性中設定的值會保存在*launchSettings.json*檔案，而且您也可以設定設定檔藉由直接編輯該檔案。</span><span class="sxs-lookup"><span data-stu-id="0efaa-125">The values you set in project properties are persisted in the *launchSettings.json* file, and you can also configure profiles by editing that file directly.</span></span>

<span data-ttu-id="0efaa-126">IIS Express 的設定檔如下所示：</span><span class="sxs-lookup"><span data-stu-id="0efaa-126">The profile for IIS Express is shown here:</span></span>

![專案屬性設定環境變數](environments/_static/project-properties-debug.png)

<span data-ttu-id="0efaa-128">以下是`launchSettings.json`檔案，其中包含的設定檔`Development`和`Staging`:</span><span class="sxs-lookup"><span data-stu-id="0efaa-128">Here is a `launchSettings.json` file that includes profiles for `Development` and `Staging`:</span></span>

<span data-ttu-id="0efaa-129">[!code-json[Main](../fundamentals/environments/sample/src/Environments/Properties/launchSettings.json?highlight=15,22)]</span><span class="sxs-lookup"><span data-stu-id="0efaa-129">[!code-json[Main](../fundamentals/environments/sample/src/Environments/Properties/launchSettings.json?highlight=15,22)]</span></span>

<span data-ttu-id="0efaa-130">專案設定檔所做的變更可能不會生效，直到重新啟動使用網頁伺服器 （特別是，Kestrel 必須重新啟動之前它會偵測到它的環境所做的變更）。</span><span class="sxs-lookup"><span data-stu-id="0efaa-130">Changes made to project profiles may not take effect until the web server used is restarted (in particular, Kestrel must be restarted before it will detect changes made to its environment).</span></span>

>[!WARNING]
> <span data-ttu-id="0efaa-131">環境變數會儲存在*launchSettings.json*並未受到保護以任何方式，並將原始程式碼儲存機制在專案的一部分，如果您使用其中一個。</span><span class="sxs-lookup"><span data-stu-id="0efaa-131">Environment variables stored in *launchSettings.json* are not secured in any way and will be part of the source code repository for your project, if you use one.</span></span> <span data-ttu-id="0efaa-132">**絕對不要儲存這個檔案中的認證或其他機密資料。**</span><span class="sxs-lookup"><span data-stu-id="0efaa-132">**Never store credentials or other secret data in this file.**</span></span> <span data-ttu-id="0efaa-133">如果您需要儲存這類資料的位置，使用*密碼管理員*中所述的工具[安全存放應用程式密碼，在開發期間](../security/app-secrets.md#security-app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="0efaa-133">If you need a place to store such data, use the *Secret Manager* tool described in [Safe storage of app secrets during development](../security/app-secrets.md#security-app-secrets).</span></span>

### <a name="staging"></a><span data-ttu-id="0efaa-134">預備環境</span><span class="sxs-lookup"><span data-stu-id="0efaa-134">Staging</span></span>

<span data-ttu-id="0efaa-135">依照慣例，`Staging`環境是用於部署至生產環境前進行最終測試進入生產階段前環境。</span><span class="sxs-lookup"><span data-stu-id="0efaa-135">By convention, a `Staging` environment is a pre-production environment used for final testing before deployment to production.</span></span> <span data-ttu-id="0efaa-136">在理想情況下，其實體特性應該會鏡像的生產環境中，，以便在生產環境中可能會發生任何問題進行第一次在臨時環境中，而不會影響使用者來處理。</span><span class="sxs-lookup"><span data-stu-id="0efaa-136">Ideally, its physical characteristics should mirror that of production, so that any issues that may arise in production occur first in the staging environment, where they can be addressed without impact to users.</span></span>

### <a name="production"></a><span data-ttu-id="0efaa-137">生產環境</span><span class="sxs-lookup"><span data-stu-id="0efaa-137">Production</span></span>

<span data-ttu-id="0efaa-138">`Production`環境是即時時，執行應用程式的環境，使用者正在使用。</span><span class="sxs-lookup"><span data-stu-id="0efaa-138">The `Production` environment is the environment in which the application runs when it is live and being used by end users.</span></span> <span data-ttu-id="0efaa-139">此環境應該設定為最大化安全性、 效能及應用程式的健全性。</span><span class="sxs-lookup"><span data-stu-id="0efaa-139">This environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="0efaa-140">某些常見設定生產環境中可能會不同於開發包括：</span><span class="sxs-lookup"><span data-stu-id="0efaa-140">Some common settings that a production environment might have that would differ from development include:</span></span>

* <span data-ttu-id="0efaa-141">開啟快取</span><span class="sxs-lookup"><span data-stu-id="0efaa-141">Turn on caching</span></span>

* <span data-ttu-id="0efaa-142">請確認所有用戶端資源配套、 縮短，而且可能由 CDN</span><span class="sxs-lookup"><span data-stu-id="0efaa-142">Ensure all client-side resources are bundled, minified, and potentially served from a CDN</span></span>

* <span data-ttu-id="0efaa-143">關閉診斷 ErrorPages</span><span class="sxs-lookup"><span data-stu-id="0efaa-143">Turn off diagnostic ErrorPages</span></span>

* <span data-ttu-id="0efaa-144">開啟易懂的錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="0efaa-144">Turn on friendly error pages</span></span>

* <span data-ttu-id="0efaa-145">啟用記錄和監視的生產環境 (例如， [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/))</span><span class="sxs-lookup"><span data-stu-id="0efaa-145">Enable production logging and monitoring (for example, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/))</span></span>

<span data-ttu-id="0efaa-146">這是不是當做完整清單。</span><span class="sxs-lookup"><span data-stu-id="0efaa-146">This is by no means meant to be a complete list.</span></span> <span data-ttu-id="0efaa-147">您最好避免均勻環境檢查在您的應用程式的許多部分。</span><span class="sxs-lookup"><span data-stu-id="0efaa-147">It's best to avoid scattering environment checks in many parts of your application.</span></span> <span data-ttu-id="0efaa-148">相反地，建議的方法是執行中應用程式的這類檢查`Startup`類別可能的情況下</span><span class="sxs-lookup"><span data-stu-id="0efaa-148">Instead, the recommended approach is to perform such checks within the application's `Startup` class(es) wherever possible</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="0efaa-149">設定環境</span><span class="sxs-lookup"><span data-stu-id="0efaa-149">Setting the environment</span></span>

<span data-ttu-id="0efaa-150">設定環境的方法取決於作業系統。</span><span class="sxs-lookup"><span data-stu-id="0efaa-150">The method for setting the environment depends on the operating system.</span></span>

### <a name="windows"></a><span data-ttu-id="0efaa-151">Windows</span><span class="sxs-lookup"><span data-stu-id="0efaa-151">Windows</span></span>
<span data-ttu-id="0efaa-152">若要設定`ASPNETCORE_ENVIRONMENT`目前工作階段，如果應用程式會使用啟動`dotnet run`，使用下列命令</span><span class="sxs-lookup"><span data-stu-id="0efaa-152">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using `dotnet run`, the following commands are used</span></span>

<span data-ttu-id="0efaa-153">**命令列**</span><span class="sxs-lookup"><span data-stu-id="0efaa-153">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="0efaa-154">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="0efaa-154">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="0efaa-155">這些命令才會生效，僅針對目前的視窗。</span><span class="sxs-lookup"><span data-stu-id="0efaa-155">These commands take effect only for the current window.</span></span> <span data-ttu-id="0efaa-156">視窗關閉時，ASPNETCORE_ENVIRONMENT 設定會還原為預設值或 machine 值中。</span><span class="sxs-lookup"><span data-stu-id="0efaa-156">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="0efaa-157">若要開啟 Windows 全域設定的值**控制台** > **系統** > **進階系統設定**以及新增或編輯`ASPNETCORE_ENVIRONMENT`值。</span><span class="sxs-lookup"><span data-stu-id="0efaa-157">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![進階屬性的系統](environments/_static/systemsetting_environment.png)

![ASPNET 核心環境變數](environments/_static/windows_aspnetcore_environment.png) 

<span data-ttu-id="0efaa-160">**web.config**</span><span class="sxs-lookup"><span data-stu-id="0efaa-160">**web.config**</span></span>

<span data-ttu-id="0efaa-161">請參閱*設定環境變數*區段[ASP.NET 核心模組的組態參考](xref:hosting/aspnet-core-module#setting-environment-variables)主題。</span><span class="sxs-lookup"><span data-stu-id="0efaa-161">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="0efaa-162">**每個 IIS 應用程式集區**</span><span class="sxs-lookup"><span data-stu-id="0efaa-162">**Per IIS Application Pool**</span></span>

<span data-ttu-id="0efaa-163">如果您需要設定環境變數，個別執行的應用程式中 （支援 IIS 10.0 +） 隔離的應用程式集區，請參閱*AppCmd.exe 命令*區段[環境變數\<environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe)在 IIS 中的主題參考文件。</span><span class="sxs-lookup"><span data-stu-id="0efaa-163">If you need to set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

### <a name="macos"></a><span data-ttu-id="0efaa-164">MacOS</span><span class="sxs-lookup"><span data-stu-id="0efaa-164">macOS</span></span>
<span data-ttu-id="0efaa-165">設定 macOS 的目前環境可以是內建作業時完成執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="0efaa-165">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="0efaa-166">或使用`export`設定之前執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="0efaa-166">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
``` 
<span data-ttu-id="0efaa-167">電腦層級環境變數中設定*.bashrc*或*.bash_profile*檔案。</span><span class="sxs-lookup"><span data-stu-id="0efaa-167">Machine level environment variables are set in the *.bashrc*  or *.bash_profile* file.</span></span> <span data-ttu-id="0efaa-168">編輯檔案使用任何文字編輯器，並加入下列陳述式。</span><span class="sxs-lookup"><span data-stu-id="0efaa-168">Edit the file using any text editor and add the following statment.</span></span>

```
export ASPNETCORE_ENVIRONMENT=Development
```  

### <a name="linux"></a><span data-ttu-id="0efaa-169">Linux</span><span class="sxs-lookup"><span data-stu-id="0efaa-169">Linux</span></span>
<span data-ttu-id="0efaa-170">針對 Linux 散發版本，請使用`export`工作階段以變數設定為基礎的命令在命令列和*bash_profile*機器層級的環境設定的檔案。</span><span class="sxs-lookup"><span data-stu-id="0efaa-170">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

## <a name="determining-the-environment-at-runtime"></a><span data-ttu-id="0efaa-171">判斷在執行階段環境</span><span class="sxs-lookup"><span data-stu-id="0efaa-171">Determining the environment at runtime</span></span>

<span data-ttu-id="0efaa-172">`IHostingEnvironment`服務所提供的核心概念與環境搭配使用。</span><span class="sxs-lookup"><span data-stu-id="0efaa-172">The `IHostingEnvironment` service provides the core abstraction for working with environments.</span></span> <span data-ttu-id="0efaa-173">這個服務是由 ASP.NET 裝載圖層，而且可以插入您的啟動邏輯透過[相依性插入](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="0efaa-173">This service is provided by the ASP.NET hosting layer, and can be injected into your startup logic via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="0efaa-174">ASP.NET Core web 網站範本在 Visual Studio 中的使用這個方法載入特定環境的組態檔 （如果有的話），以及自訂應用程式的錯誤處理設定。</span><span class="sxs-lookup"><span data-stu-id="0efaa-174">The ASP.NET Core web site template in Visual Studio uses this approach to load environment-specific configuration files (if present) and to customize the app's error handling settings.</span></span> <span data-ttu-id="0efaa-175">在這兩種情況下，此行為藉由呼叫目前指定的環境參考來達成`EnvironmentName`或`IsEnvironment`的執行個體上`IHostingEnvironment`傳遞至適當的方法。</span><span class="sxs-lookup"><span data-stu-id="0efaa-175">In both cases, this behavior is achieved by referring to the currently specified environment by calling `EnvironmentName` or `IsEnvironment` on the instance of `IHostingEnvironment` passed into the appropriate method.</span></span>

> [!NOTE]
> <span data-ttu-id="0efaa-176">如果您要檢查應用程式是否正在執行中的特定環境使用`env.IsEnvironment("environmentname")`因為正確，就會忽略大小寫 (而不是檢查`env.EnvironmentName == "Development"`例如)。</span><span class="sxs-lookup"><span data-stu-id="0efaa-176">If you need to check whether the application is running in a particular environment, use `env.IsEnvironment("environmentname")` since it will correctly ignore case (instead of checking if `env.EnvironmentName == "Development"` for example).</span></span>

<span data-ttu-id="0efaa-177">例如，您可以使用下列程式碼在您設定的方法設定環境特定的錯誤處理：</span><span class="sxs-lookup"><span data-stu-id="0efaa-177">For example, you can use the following code in your Configure method to setup environment specific error handling:</span></span>

<span data-ttu-id="0efaa-178">[!code-csharp[Main](environments/sample/src/Environments/Startup.cs?range=19-30)]</span><span class="sxs-lookup"><span data-stu-id="0efaa-178">[!code-csharp[Main](environments/sample/src/Environments/Startup.cs?range=19-30)]</span></span>

<span data-ttu-id="0efaa-179">如果應用程式在執行`Development`環境中，則可讓使用 Visual Studio，開發特定的錯誤網頁 （這通常不應在生產環境中） 和特殊的資料庫錯誤中的"BrowserLink 」 功能所需的執行階段支援頁面 （其提供一個方式來套用移轉，因此應該只用於開發工作）。</span><span class="sxs-lookup"><span data-stu-id="0efaa-179">If the app is running in a `Development` environment, then it enables the runtime support necessary to use the "BrowserLink" feature in Visual Studio, development-specific error pages (which typically should not be run in production) and special database error pages (which provide a way to apply migrations and should therefore only be used in development).</span></span> <span data-ttu-id="0efaa-180">否則，如果未在開發環境中執行應用程式，標準錯誤事件處理常式設定為會顯示在任何未處理的例外狀況的回應。</span><span class="sxs-lookup"><span data-stu-id="0efaa-180">Otherwise, if the app is not running in a development environment, a standard error handling page is configured to be displayed in response to any unhandled exceptions.</span></span>

<span data-ttu-id="0efaa-181">若要判斷要傳送給用戶端在執行階段，根據目前的環境的內容。</span><span class="sxs-lookup"><span data-stu-id="0efaa-181">You may need to determine which content to send to the client at runtime, depending on the current environment.</span></span> <span data-ttu-id="0efaa-182">例如，在開發環境中您通常做非最小化指令碼和樣式表，可簡化偵錯更容易。</span><span class="sxs-lookup"><span data-stu-id="0efaa-182">For example, in a development environment you generally serve non-minimized scripts and style sheets, which makes debugging easier.</span></span> <span data-ttu-id="0efaa-183">生產和測試環境應該用於縮短的版本，通常是從 CDN。</span><span class="sxs-lookup"><span data-stu-id="0efaa-183">Production and test environments should serve the minified versions and generally from a CDN.</span></span> <span data-ttu-id="0efaa-184">您可以使用環境[標記協助程式](../mvc/views/tag-helpers/intro.md)。</span><span class="sxs-lookup"><span data-stu-id="0efaa-184">You can do this using the Environment [tag helper](../mvc/views/tag-helpers/intro.md).</span></span> <span data-ttu-id="0efaa-185">環境標記協助程式只會呈現其內容，如果目前的環境符合使用指定的環境的其中一個`names`屬性。</span><span class="sxs-lookup"><span data-stu-id="0efaa-185">The Environment tag helper will only render its contents if the current environment matches one of the environments specified using the `names` attribute.</span></span>

<span data-ttu-id="0efaa-186">[!code-html[Main](environments/sample/src/Environments/Views/Shared/_Layout.cshtml?range=13-22)]</span><span class="sxs-lookup"><span data-stu-id="0efaa-186">[!code-html[Main](environments/sample/src/Environments/Views/Shared/_Layout.cshtml?range=13-22)]</span></span>

<span data-ttu-id="0efaa-187">若要開始使用您的應用程式請參閱使用標記 helper[標記協助程式簡介](../mvc/views/tag-helpers/intro.md)。</span><span class="sxs-lookup"><span data-stu-id="0efaa-187">To get started with using tag helpers in your application see [Introduction to Tag Helpers](../mvc/views/tag-helpers/intro.md).</span></span>

## <a name="startup-conventions"></a><span data-ttu-id="0efaa-188">啟動慣例</span><span class="sxs-lookup"><span data-stu-id="0efaa-188">Startup conventions</span></span>

<span data-ttu-id="0efaa-189">ASP.NET Core 支援以慣例為基礎的方式來設定應用程式的啟動根據目前的環境。</span><span class="sxs-lookup"><span data-stu-id="0efaa-189">ASP.NET Core supports a convention-based approach to configuring an application's startup based on the current environment.</span></span> <span data-ttu-id="0efaa-190">您也以程式設計方式可以控制您的應用程式的行為方式根據要何種環境處於，可讓您建立及管理自己的慣例。</span><span class="sxs-lookup"><span data-stu-id="0efaa-190">You can also programmatically control how your application behaves according to which environment it is in, allowing you to create and manage your own conventions.</span></span>

<span data-ttu-id="0efaa-191">ASP.NET Core 應用程式啟動時，`Startup`類別用來啟動載入應用程式時，載入其組態設定等 ([深入了解 ASP.NET 啟動](startup.md))。</span><span class="sxs-lookup"><span data-stu-id="0efaa-191">When an ASP.NET Core application starts, the `Startup` class is used to bootstrap the application, load its configuration settings, etc. ([learn more about ASP.NET startup](startup.md)).</span></span> <span data-ttu-id="0efaa-192">不過，如果存在的類別命名為`Startup{EnvironmentName}`(例如`StartupDevelopment`)，而`ASPNETCORE_ENVIRONMENT`環境變數符合該名稱，然後，`Startup`改為使用類別。</span><span class="sxs-lookup"><span data-stu-id="0efaa-192">However, if a class exists named `Startup{EnvironmentName}` (for example `StartupDevelopment`), and the `ASPNETCORE_ENVIRONMENT` environment variable matches that name, then that `Startup` class is used instead.</span></span> <span data-ttu-id="0efaa-193">因此，您可以設定`Startup`進行開發，但有不同的`StartupProduction`，會在生產環境中執行應用程式時使用。</span><span class="sxs-lookup"><span data-stu-id="0efaa-193">Thus, you could configure `Startup` for development, but have a separate `StartupProduction` that would be used when the app is run in production.</span></span> <span data-ttu-id="0efaa-194">反之亦然。</span><span class="sxs-lookup"><span data-stu-id="0efaa-194">Or vice versa.</span></span>

> [!NOTE]
> <span data-ttu-id="0efaa-195">呼叫`WebHostBuilder.UseStartup<TStartup>()`覆寫組態區段。</span><span class="sxs-lookup"><span data-stu-id="0efaa-195">Calling `WebHostBuilder.UseStartup<TStartup>()` overrides configuration sections.</span></span>

<span data-ttu-id="0efaa-196">除了使用完全不同`Startup`類別根據目前的環境中，您也可以進行調整應用程式內的設定方式`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="0efaa-196">In addition to using an entirely separate `Startup` class based on the current environment, you can also make adjustments to how the application is configured within a `Startup` class.</span></span> <span data-ttu-id="0efaa-197">`Configure()`和`ConfigureServices()`方法支援類似於的環境特定版本`Startup`類別本身的表單`Configure{EnvironmentName}()`和`Configure{EnvironmentName}Services()`。</span><span class="sxs-lookup"><span data-stu-id="0efaa-197">The `Configure()` and `ConfigureServices()` methods support environment-specific versions similar to the `Startup` class itself, of the form `Configure{EnvironmentName}()` and `Configure{EnvironmentName}Services()`.</span></span> <span data-ttu-id="0efaa-198">如果定義方法`ConfigureDevelopment()`將呼叫而不是`Configure()`當環境設定為開發。</span><span class="sxs-lookup"><span data-stu-id="0efaa-198">If you define a method `ConfigureDevelopment()` it will be called instead of `Configure()` when the environment is set to development.</span></span> <span data-ttu-id="0efaa-199">同樣地，`ConfigureDevelopmentServices()`而不是呼叫`ConfigureServices()`相同環境中。</span><span class="sxs-lookup"><span data-stu-id="0efaa-199">Likewise, `ConfigureDevelopmentServices()` would be called instead of `ConfigureServices()` in the same environment.</span></span>

## <a name="summary"></a><span data-ttu-id="0efaa-200">總結</span><span class="sxs-lookup"><span data-stu-id="0efaa-200">Summary</span></span>

<span data-ttu-id="0efaa-201">ASP.NET Core 提供數種功能可讓開發人員輕鬆地控制其應用程式在不同環境中的行為方式的慣例。</span><span class="sxs-lookup"><span data-stu-id="0efaa-201">ASP.NET Core provides a number of features and conventions that allow developers to easily control how their applications behave in different environments.</span></span> <span data-ttu-id="0efaa-202">當發行應用程式從開發到預備環境到實際執行環境，環境變數集適當地環境允許偵錯、 測試或生產環境使用，視需要的應用程式的最佳化。</span><span class="sxs-lookup"><span data-stu-id="0efaa-202">When publishing an application from development to staging to production, environment variables set appropriately for the environment allow for optimization of the application for debugging, testing, or production use, as appropriate.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0efaa-203">其他資源</span><span class="sxs-lookup"><span data-stu-id="0efaa-203">Additional Resources</span></span>

* [<span data-ttu-id="0efaa-204">組態</span><span class="sxs-lookup"><span data-stu-id="0efaa-204">Configuration</span></span>](configuration.md)

* [<span data-ttu-id="0efaa-205">標記協助程式簡介</span><span class="sxs-lookup"><span data-stu-id="0efaa-205">Introduction to Tag Helpers</span></span>](../mvc/views/tag-helpers/intro.md)
