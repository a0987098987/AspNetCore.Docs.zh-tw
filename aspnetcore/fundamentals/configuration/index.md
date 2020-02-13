---
title: ASP.NET Core 的設定
author: guardrex
description: 了解如何使用組態 API 設定 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/10/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: d0ef670aa0ac4960318f86ea7888b9eab71f17fd
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2020
ms.locfileid: "77171891"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="942d5-103">ASP.NET Core 的設定</span><span class="sxs-lookup"><span data-stu-id="942d5-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="942d5-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="942d5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="942d5-105">ASP.NET Core 中的應用程式設定是以由*設定提供者*所建立的機碼值組為基礎。</span><span class="sxs-lookup"><span data-stu-id="942d5-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="942d5-106">設定提供者會從各種設定來源將設定資料讀取到機碼值組中：</span><span class="sxs-lookup"><span data-stu-id="942d5-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="942d5-107">Azure 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="942d5-107">Azure Key Vault</span></span>
* <span data-ttu-id="942d5-108">Azure 應用程式組態</span><span class="sxs-lookup"><span data-stu-id="942d5-108">Azure App Configuration</span></span>
* <span data-ttu-id="942d5-109">命令列引數</span><span class="sxs-lookup"><span data-stu-id="942d5-109">Command-line arguments</span></span>
* <span data-ttu-id="942d5-110">自訂提供者 (已安裝或已建立)</span><span class="sxs-lookup"><span data-stu-id="942d5-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="942d5-111">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="942d5-111">Directory files</span></span>
* <span data-ttu-id="942d5-112">環境變數</span><span class="sxs-lookup"><span data-stu-id="942d5-112">Environment variables</span></span>
* <span data-ttu-id="942d5-113">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="942d5-113">In-memory .NET objects</span></span>
* <span data-ttu-id="942d5-114">設定檔</span><span class="sxs-lookup"><span data-stu-id="942d5-114">Settings files</span></span>

<span data-ttu-id="942d5-115">一般組態提供者案例 ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) 的組態套件會以隱含方式包含在架構中。</span><span class="sxs-lookup"><span data-stu-id="942d5-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

<span data-ttu-id="942d5-116">下列程式碼範例與範例應用程式中的程式碼範例使用 <xref:Microsoft.Extensions.Configuration> 命名空間：</span><span class="sxs-lookup"><span data-stu-id="942d5-116">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="942d5-117">*選項模式*是此主題中所述之設定概念的延伸。</span><span class="sxs-lookup"><span data-stu-id="942d5-117">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="942d5-118">選項使用類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-118">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="942d5-119">如需詳細資訊，請參閱 <xref:fundamentals/configuration/options>。</span><span class="sxs-lookup"><span data-stu-id="942d5-119">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="942d5-120">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="942d5-120">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="942d5-121">主機與應用程式組態的比較</span><span class="sxs-lookup"><span data-stu-id="942d5-121">Host versus app configuration</span></span>

<span data-ttu-id="942d5-122">設定及啟動應用程式之前，會先設定及啟動「主機」。</span><span class="sxs-lookup"><span data-stu-id="942d5-122">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="942d5-123">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="942d5-123">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="942d5-124">應用程式與主機都是使用此主題中所述的設定提供者來設定的。</span><span class="sxs-lookup"><span data-stu-id="942d5-124">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="942d5-125">主機組態機碼/值組也會包含在應用程式的組態中。</span><span class="sxs-lookup"><span data-stu-id="942d5-125">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="942d5-126">如需有關當建置主機時如何使用設定提供者的詳細資訊，以及設定來源如何影響主機設定的詳細資訊，請參閱 <xref:fundamentals/index#host>。</span><span class="sxs-lookup"><span data-stu-id="942d5-126">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="942d5-127">其他設定</span><span class="sxs-lookup"><span data-stu-id="942d5-127">Other configuration</span></span>

<span data-ttu-id="942d5-128">本主題僅適用于*應用程式*設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-128">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="942d5-129">執行和裝載 ASP.NET Core 應用程式的其他層面，是使用本主題未涵蓋的設定檔來設定：</span><span class="sxs-lookup"><span data-stu-id="942d5-129">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="942d5-130">/*launchsettings.json* *，json 是*開發環境的工具設定檔，如下所述：</span><span class="sxs-lookup"><span data-stu-id="942d5-130">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="942d5-131">在 <xref:fundamentals/environments#development>。</span><span class="sxs-lookup"><span data-stu-id="942d5-131">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="942d5-132">在檔集內，用來為開發案例設定 ASP.NET Core 應用程式的檔案。</span><span class="sxs-lookup"><span data-stu-id="942d5-132">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="942d5-133">*web.config*是伺服器設定檔，如下列主題所述：</span><span class="sxs-lookup"><span data-stu-id="942d5-133">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="942d5-134">如需從舊版 ASP.NET 遷移應用程式設定的詳細資訊，請參閱 <xref:migration/proper-to-2x/index#store-configurations>。</span><span class="sxs-lookup"><span data-stu-id="942d5-134">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="942d5-135">預設組態</span><span class="sxs-lookup"><span data-stu-id="942d5-135">Default configuration</span></span>

<span data-ttu-id="942d5-136">以 ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) 範本為基礎的 Web 應用程式，會在建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>。</span><span class="sxs-lookup"><span data-stu-id="942d5-136">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="942d5-137">`CreateDefaultBuilder` 會以下列順序提供應用程式的預設組態：</span><span class="sxs-lookup"><span data-stu-id="942d5-137">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="942d5-138">下列項目適用於使用[一般主機](xref:fundamentals/host/generic-host)的應用程式。</span><span class="sxs-lookup"><span data-stu-id="942d5-138">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="942d5-139">如需使用 [Web 主機](xref:fundamentals/host/web-host)時預設組態的詳細資料，請參閱[本主題的 ASP.NET Core 2.2 版本](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2)。</span><span class="sxs-lookup"><span data-stu-id="942d5-139">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="942d5-140">主機組態的提供來源：</span><span class="sxs-lookup"><span data-stu-id="942d5-140">Host configuration is provided from:</span></span>
  * <span data-ttu-id="942d5-141">使用`DOTNET_`環境變數組態提供者`DOTNET_ENVIRONMENT`且以 [ 為前置詞 (例如 ](#environment-variables-configuration-provider)) 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="942d5-141">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="942d5-142">載入設定機碼值組時，會移除前置詞 (`DOTNET_`)。</span><span class="sxs-lookup"><span data-stu-id="942d5-142">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="942d5-143">使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="942d5-143">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="942d5-144">已建立 Web 主機預設組態 (`ConfigureWebHostDefaults`)：</span><span class="sxs-lookup"><span data-stu-id="942d5-144">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="942d5-145">Kestrel 會用作為網頁伺服器，並使用應用程式的組態提供者來設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-145">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="942d5-146">新增主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="942d5-146">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="942d5-147">如果 `ASPNETCORE_FORWARDEDHEADERS_ENABLED` 環境變數設定為 `true`，則會新增轉接的標頭中介軟體。</span><span class="sxs-lookup"><span data-stu-id="942d5-147">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="942d5-148">啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="942d5-148">Enable IIS integration.</span></span>
* <span data-ttu-id="942d5-149">應用程式設定的提供來源：</span><span class="sxs-lookup"><span data-stu-id="942d5-149">App configuration is provided from:</span></span>
  * <span data-ttu-id="942d5-150">使用*檔案組態提供者*的 [appsettings.json](#file-configuration-provider)。</span><span class="sxs-lookup"><span data-stu-id="942d5-150">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="942d5-151">使用*檔案組態提供者*的 [appsettings.{Environment}.json](#file-configuration-provider)。</span><span class="sxs-lookup"><span data-stu-id="942d5-151">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="942d5-152">應用程式在使用項目組件之 [ 環境中執行時的](xref:security/app-secrets)秘密管理員`Development`。</span><span class="sxs-lookup"><span data-stu-id="942d5-152">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="942d5-153">使用[環境變數組態提供者](#environment-variables-configuration-provider)的環境變數。</span><span class="sxs-lookup"><span data-stu-id="942d5-153">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="942d5-154">使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="942d5-154">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

## <a name="security"></a><span data-ttu-id="942d5-155">安全性</span><span class="sxs-lookup"><span data-stu-id="942d5-155">Security</span></span>

<span data-ttu-id="942d5-156">採用下列做法來保護敏感性組態資料：</span><span class="sxs-lookup"><span data-stu-id="942d5-156">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="942d5-157">永遠不要將密碼或其他敏感性資料儲存在設定提供者程式碼或純文字設定檔中。</span><span class="sxs-lookup"><span data-stu-id="942d5-157">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="942d5-158">不要在開發或測試環境中使用生產環境祕密。</span><span class="sxs-lookup"><span data-stu-id="942d5-158">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="942d5-159">請在專案外部指定祕密，以防止其意外認可至開放原始碼存放庫。</span><span class="sxs-lookup"><span data-stu-id="942d5-159">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="942d5-160">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="942d5-160">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="942d5-161"><xref:security/app-secrets> &ndash; 包含有關使用環境變數來儲存敏感性資料的建議。</span><span class="sxs-lookup"><span data-stu-id="942d5-161"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="942d5-162">「祕密管理員」使用「檔案設定提供者」以 JSON 檔案在本機系統上存放使用者祕密。</span><span class="sxs-lookup"><span data-stu-id="942d5-162">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="942d5-163">此主題稍後將說明「檔案設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="942d5-163">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="942d5-164">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 可安全地儲存 ASP.NET Core 應用程式的應用程式祕密。</span><span class="sxs-lookup"><span data-stu-id="942d5-164">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="942d5-165">如需詳細資訊，請參閱 <xref:security/key-vault-configuration>。</span><span class="sxs-lookup"><span data-stu-id="942d5-165">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="942d5-166">階層式設定資料</span><span class="sxs-lookup"><span data-stu-id="942d5-166">Hierarchical configuration data</span></span>

<span data-ttu-id="942d5-167">設定 API 可在設定機碼中使用分隔符號來壓平合併階層式資料，以管理階層式設定資料。</span><span class="sxs-lookup"><span data-stu-id="942d5-167">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="942d5-168">在下列 JSON 檔案中，兩個區段的結構式階層中有四個機碼存在：</span><span class="sxs-lookup"><span data-stu-id="942d5-168">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

<span data-ttu-id="942d5-169">當該檔案被讀入設定之後，會建立唯一機碼來維護設定來源的原始階層式資料結構。</span><span class="sxs-lookup"><span data-stu-id="942d5-169">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="942d5-170">系統會使用冒號 (`:`) 將區段與機碼壓平合併以維護原始結構：</span><span class="sxs-lookup"><span data-stu-id="942d5-170">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="942d5-171">section0:key0</span><span class="sxs-lookup"><span data-stu-id="942d5-171">section0:key0</span></span>
* <span data-ttu-id="942d5-172">section0:key1</span><span class="sxs-lookup"><span data-stu-id="942d5-172">section0:key1</span></span>
* <span data-ttu-id="942d5-173">section1:key0</span><span class="sxs-lookup"><span data-stu-id="942d5-173">section1:key0</span></span>
* <span data-ttu-id="942d5-174">section1:key1</span><span class="sxs-lookup"><span data-stu-id="942d5-174">section1:key1</span></span>

<span data-ttu-id="942d5-175"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> 與 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> 方法可用來在設定資料中隔離區段與區段的子系。</span><span class="sxs-lookup"><span data-stu-id="942d5-175"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="942d5-176">[GetSection,、 GetChildren 與 Exists](#getsection-getchildren-and-exists) 中說明這些方法。</span><span class="sxs-lookup"><span data-stu-id="942d5-176">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="942d5-177">慣例</span><span class="sxs-lookup"><span data-stu-id="942d5-177">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="942d5-178">來源和提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-178">Sources and providers</span></span>

<span data-ttu-id="942d5-179">在應用程式啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="942d5-179">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="942d5-180">實作變更偵測的組態提供者能夠在基礎設定變更時重新載入組態。</span><span class="sxs-lookup"><span data-stu-id="942d5-180">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="942d5-181">例如，檔案組態提供者 (將於本主題稍後討論) 和 [Azure Key Vault 組態提供者](xref:security/key-vault-configuration)均會實作變更偵測。</span><span class="sxs-lookup"><span data-stu-id="942d5-181">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="942d5-182">您可以在應用程式的<xref:Microsoft.Extensions.Configuration.IConfiguration>相依性插入 (DI)[ 容器中找到 ](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="942d5-182"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="942d5-183"><xref:Microsoft.Extensions.Configuration.IConfiguration> 可以插入至 Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> 或 MVC <xref:Microsoft.AspNetCore.Mvc.Controller>，以取得類別的設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-183"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="942d5-184">在下列範例中，`_config` 欄位是用來存取設定值：</span><span class="sxs-lookup"><span data-stu-id="942d5-184">In the following examples, the `_config` field is used to access configuration values:</span></span>

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }
}
```

```csharp
public class HomeController : Controller
{
    private readonly IConfiguration _config;

    public HomeController(IConfiguration config)
    {
        _config = config;
    }
}
```

<span data-ttu-id="942d5-185">設定提供者無法使用 DI，因為當它們由主機設定時，它無法使用。</span><span class="sxs-lookup"><span data-stu-id="942d5-185">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="942d5-186">索引鍵</span><span class="sxs-lookup"><span data-stu-id="942d5-186">Keys</span></span>

<span data-ttu-id="942d5-187">設定機碼會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="942d5-187">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="942d5-188">機碼區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="942d5-188">Keys are case-insensitive.</span></span> <span data-ttu-id="942d5-189">例如，`ConnectionString` 與 `connectionstring` 會被視為相等的機碼。</span><span class="sxs-lookup"><span data-stu-id="942d5-189">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="942d5-190">若相同機碼的值是由相同或不同設定提供者設定，在機碼上設定的最後一個值是所使用的值。</span><span class="sxs-lookup"><span data-stu-id="942d5-190">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="942d5-191">階層式機碼</span><span class="sxs-lookup"><span data-stu-id="942d5-191">Hierarchical keys</span></span>
  * <span data-ttu-id="942d5-192">在設定 API 內，冒號分隔字元 (`:`) 可在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="942d5-192">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="942d5-193">在環境變數中，冒號分隔字元可能無法在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="942d5-193">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="942d5-194">所有平台都支援雙底線 (`__`)，且會自動轉換為冒號。</span><span class="sxs-lookup"><span data-stu-id="942d5-194">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="942d5-195">在 Azure Key Vault 中，階層式機碼使用 `--` (兩個破折號) 來做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="942d5-195">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="942d5-196">撰寫程式碼，以在將密碼載入應用程式的設定時，以冒號取代破折號。</span><span class="sxs-lookup"><span data-stu-id="942d5-196">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="942d5-197"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="942d5-197">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="942d5-198">[將陣列繫結到類別](#bind-an-array-to-a-class)一節說明陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="942d5-198">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="942d5-199">值</span><span class="sxs-lookup"><span data-stu-id="942d5-199">Values</span></span>

<span data-ttu-id="942d5-200">設定值會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="942d5-200">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="942d5-201">值是字串。</span><span class="sxs-lookup"><span data-stu-id="942d5-201">Values are strings.</span></span>
* <span data-ttu-id="942d5-202">Null 值無法存放在設定中或繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="942d5-202">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="942d5-203">提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-203">Providers</span></span>

<span data-ttu-id="942d5-204">下表顯示可供 ASP.NET Core 應用程式使用的設定提供者。</span><span class="sxs-lookup"><span data-stu-id="942d5-204">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="942d5-205">提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-205">Provider</span></span> | <span data-ttu-id="942d5-206">從&hellip;提供設定</span><span class="sxs-lookup"><span data-stu-id="942d5-206">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="942d5-207">[Azure Key Vault 設定提供者](xref:security/key-vault-configuration) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="942d5-207">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="942d5-208">Azure 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="942d5-208">Azure Key Vault</span></span> |
| <span data-ttu-id="942d5-209">[Azure 應用程式組態提供者](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure 文件)</span><span class="sxs-lookup"><span data-stu-id="942d5-209">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="942d5-210">Azure 應用程式組態</span><span class="sxs-lookup"><span data-stu-id="942d5-210">Azure App Configuration</span></span> |
| [<span data-ttu-id="942d5-211">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="942d5-212">命令列參數</span><span class="sxs-lookup"><span data-stu-id="942d5-212">Command-line parameters</span></span> |
| [<span data-ttu-id="942d5-213">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="942d5-214">自訂來源</span><span class="sxs-lookup"><span data-stu-id="942d5-214">Custom source</span></span> |
| [<span data-ttu-id="942d5-215">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-215">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="942d5-216">環境變數</span><span class="sxs-lookup"><span data-stu-id="942d5-216">Environment variables</span></span> |
| [<span data-ttu-id="942d5-217">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="942d5-218">檔案 (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="942d5-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="942d5-219">每個檔案機碼的設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-219">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="942d5-220">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="942d5-220">Directory files</span></span> |
| [<span data-ttu-id="942d5-221">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-221">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="942d5-222">記憶體內集合</span><span class="sxs-lookup"><span data-stu-id="942d5-222">In-memory collections</span></span> |
| <span data-ttu-id="942d5-223">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="942d5-223">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="942d5-224">使用者設定檔目錄中的檔案</span><span class="sxs-lookup"><span data-stu-id="942d5-224">File in the user profile directory</span></span> |

<span data-ttu-id="942d5-225">在啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="942d5-225">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="942d5-226">本主題所描述的設定提供者會依字母順序描述，而不是程式碼排列它們的順序。</span><span class="sxs-lookup"><span data-stu-id="942d5-226">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="942d5-227">請在程式碼中訂購設定提供者，以符合應用程式所需之基礎設定來源的優先順序。</span><span class="sxs-lookup"><span data-stu-id="942d5-227">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="942d5-228">典型的設定提供者順序是：</span><span class="sxs-lookup"><span data-stu-id="942d5-228">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="942d5-229">檔案 (*appsettings.json*、*appsettings.{Environment}.json*，其中 `{Environment}` 是應用程式的目前裝載環境)</span><span class="sxs-lookup"><span data-stu-id="942d5-229">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="942d5-230">Azure 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="942d5-230">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="942d5-231">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (僅限開發環境)</span><span class="sxs-lookup"><span data-stu-id="942d5-231">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="942d5-232">環境變數</span><span class="sxs-lookup"><span data-stu-id="942d5-232">Environment variables</span></span>
1. <span data-ttu-id="942d5-233">命令列引數</span><span class="sxs-lookup"><span data-stu-id="942d5-233">Command-line arguments</span></span>

<span data-ttu-id="942d5-234">將命令列組態提供者放在提供者序列結尾是常見做法，因為這樣可以讓命令列引數覆寫由其他提供者所設定的組態。</span><span class="sxs-lookup"><span data-stu-id="942d5-234">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="942d5-235">當使用 `CreateDefaultBuilder`初始化新的主機產生器時，會使用上述的提供者序列。</span><span class="sxs-lookup"><span data-stu-id="942d5-235">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="942d5-236">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="942d5-236">For more information, see the [Default configuration](#default-configuration) section.</span></span>

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="942d5-237">使用 ConfigureHostConfiguration 設定主機建立器</span><span class="sxs-lookup"><span data-stu-id="942d5-237">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="942d5-238">若要設定主機建立器，請呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> 並提供組態。</span><span class="sxs-lookup"><span data-stu-id="942d5-238">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="942d5-239">`ConfigureHostConfiguration` 用於初始化 <xref:Microsoft.Extensions.Hosting.IHostEnvironment>，以便稍後在建置程序中使用。</span><span class="sxs-lookup"><span data-stu-id="942d5-239">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="942d5-240">`ConfigureHostConfiguration` 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="942d5-240">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureHostConfiguration(config =>
        {
            var dict = new Dictionary<string, string>
            {
                {"MemoryCollectionKey1", "value1"},
                {"MemoryCollectionKey2", "value2"}
            };

            config.AddInMemoryCollection(dict);
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

## <a name="configureappconfiguration"></a><span data-ttu-id="942d5-241">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="942d5-241">ConfigureAppConfiguration</span></span>

<span data-ttu-id="942d5-242">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定提供者，以及由 `CreateDefaultBuilder` 自動新增的設定提供者：</span><span class="sxs-lookup"><span data-stu-id="942d5-242">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="942d5-243">使用命令列引數覆寫先前的組態</span><span class="sxs-lookup"><span data-stu-id="942d5-243">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="942d5-244">若要提供可使用命令列引數覆寫的應用程式組態，請最後呼叫 `AddCommandLine`：</span><span class="sxs-lookup"><span data-stu-id="942d5-244">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="942d5-245">移除 CreateDefaultBuilder 新增的提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-245">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="942d5-246">若要移除 `CreateDefaultBuilder`新增的提供者，請先在[IConfigurationBuilder](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources)上呼叫[Clear](/dotnet/api/system.collections.generic.icollection-1.clear) ：</span><span class="sxs-lookup"><span data-stu-id="942d5-246">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="942d5-247">在應用程式啟動期間使用組態</span><span class="sxs-lookup"><span data-stu-id="942d5-247">Consume configuration during app startup</span></span>

<span data-ttu-id="942d5-248">應用程式啟動期間，可以使用 `ConfigureAppConfiguration` 中為應用程式提供的組態，包括 `Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="942d5-248">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="942d5-249">如需詳細資訊，請參閱[在啟動期間存取組態](#access-configuration-during-startup)一節。</span><span class="sxs-lookup"><span data-stu-id="942d5-249">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="942d5-250">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-250">Command-line Configuration Provider</span></span>

<span data-ttu-id="942d5-251"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> 會在執行階段從命令列引數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-251">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="942d5-252">為了啟用命令列設定，<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 延伸模組方法會在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫。</span><span class="sxs-lookup"><span data-stu-id="942d5-252">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="942d5-253">呼叫 `AddCommandLine` 時，會自動呼叫 `CreateDefaultBuilder(string [])`。</span><span class="sxs-lookup"><span data-stu-id="942d5-253">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="942d5-254">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="942d5-254">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="942d5-255">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="942d5-255">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="942d5-256">從 *appsettings.json* 與 *appsettings.{Environment}.json* 檔案載入其他選擇性組態。</span><span class="sxs-lookup"><span data-stu-id="942d5-256">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="942d5-257">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="942d5-257">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="942d5-258">環境變數。</span><span class="sxs-lookup"><span data-stu-id="942d5-258">Environment variables.</span></span>

<span data-ttu-id="942d5-259">`CreateDefaultBuilder` 會最後才新增命令列設定提供者。</span><span class="sxs-lookup"><span data-stu-id="942d5-259">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="942d5-260">在執行階段傳遞的命令列引數會覆寫由其它提供者所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-260">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="942d5-261">`CreateDefaultBuilder` 會在建構主機時執行作業。</span><span class="sxs-lookup"><span data-stu-id="942d5-261">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="942d5-262">因此，由`CreateDefaultBuilder` 啟用的命令列設定可以影響主機的設定方式。</span><span class="sxs-lookup"><span data-stu-id="942d5-262">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="942d5-263">針對以 ASP.NET Core 範本為基礎的應用程式，`AddCommandLine` 已由 `CreateDefaultBuilder` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="942d5-263">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="942d5-264">若要新增其他組態提供者並維持能夠使用命令列引數覆寫這些提供者組態的能力，請在 `ConfigureAppConfiguration` 中呼叫應用程式的其他提供者，且最後呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="942d5-264">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="942d5-265">**範例**</span><span class="sxs-lookup"><span data-stu-id="942d5-265">**Example**</span></span>

<span data-ttu-id="942d5-266">範例應用程式利用靜態方便方法 `CreateDefaultBuilder` 的優勢來建置主機，這包括對 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="942d5-266">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="942d5-267">從專案目錄中開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="942d5-267">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="942d5-268">提供命令列引數給 `dotnet run` 命令 `dotnet run CommandLineKey=CommandLineValue`。</span><span class="sxs-lookup"><span data-stu-id="942d5-268">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="942d5-269">在應用程式執行之後，開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="942d5-269">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="942d5-270">觀察輸出是否包含提供給 `dotnet run` 之設定命令列引數的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="942d5-270">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="942d5-271">引數</span><span class="sxs-lookup"><span data-stu-id="942d5-271">Arguments</span></span>

<span data-ttu-id="942d5-272">當值後面接著空格時，值必須接著等號 (`=`)，或機碼必須有前置詞 (`--` 或 `/`)。</span><span class="sxs-lookup"><span data-stu-id="942d5-272">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="942d5-273">如果使用等號 (例如 `CommandLineKey=`)，則不需要此值。</span><span class="sxs-lookup"><span data-stu-id="942d5-273">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="942d5-274">索引鍵前置字元</span><span class="sxs-lookup"><span data-stu-id="942d5-274">Key prefix</span></span>               | <span data-ttu-id="942d5-275">範例</span><span class="sxs-lookup"><span data-stu-id="942d5-275">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="942d5-276">沒有前置字元</span><span class="sxs-lookup"><span data-stu-id="942d5-276">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="942d5-277">雙虛線 (`--`)</span><span class="sxs-lookup"><span data-stu-id="942d5-277">Two dashes (`--`)</span></span>        | <span data-ttu-id="942d5-278">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="942d5-278">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="942d5-279">正斜線 (`/`)</span><span class="sxs-lookup"><span data-stu-id="942d5-279">Forward slash (`/`)</span></span>      | <span data-ttu-id="942d5-280">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="942d5-280">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="942d5-281">在相同的命令中，請不要混合使用等號搭配使用空格之機碼值組的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="942d5-281">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="942d5-282">範例命令：</span><span class="sxs-lookup"><span data-stu-id="942d5-282">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="942d5-283">切換對應</span><span class="sxs-lookup"><span data-stu-id="942d5-283">Switch mappings</span></span>

<span data-ttu-id="942d5-284">參數對應允許索引鍵名稱取代邏輯。</span><span class="sxs-lookup"><span data-stu-id="942d5-284">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="942d5-285">以 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>手動建立設定時，請將參數取代的字典提供給 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 方法。</span><span class="sxs-lookup"><span data-stu-id="942d5-285">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="942d5-286">使用切換對應字典時，會檢查字典中是否有任何索引鍵符合命令列引數所提供的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="942d5-286">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="942d5-287">如果在字典中找到命令列索引鍵，則會傳回字典值 (索引鍵取代) 以在應用程式的設定中設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="942d5-287">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="942d5-288">所有前面加上單虛線 (`-`) 的命令列索引鍵都需要切換對應。</span><span class="sxs-lookup"><span data-stu-id="942d5-288">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="942d5-289">切換對應字典索引鍵規則：</span><span class="sxs-lookup"><span data-stu-id="942d5-289">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="942d5-290">切換必須以單虛線 (`-`) 或雙虛線 (`--`) 開頭。</span><span class="sxs-lookup"><span data-stu-id="942d5-290">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="942d5-291">切換對應字典不能包含重複索引鍵。</span><span class="sxs-lookup"><span data-stu-id="942d5-291">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="942d5-292">建立切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="942d5-292">Create a switch mappings dictionary.</span></span> <span data-ttu-id="942d5-293">下列範例會建立兩個切換對應：</span><span class="sxs-lookup"><span data-stu-id="942d5-293">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="942d5-294">建立主機時，請使用切換對應字典呼叫 `AddCommandLine`：</span><span class="sxs-lookup"><span data-stu-id="942d5-294">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="942d5-295">針對使用切換對應的應用程式，呼叫 `CreateDefaultBuilder` 不應傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="942d5-295">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="942d5-296">`CreateDefaultBuilder` 方法的 `AddCommandLine` 呼叫不包括對應的切換，且沒有任何方式可以將切換對應字典傳遞給 `CreateDefaultBuilder`。</span><span class="sxs-lookup"><span data-stu-id="942d5-296">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="942d5-297">解決方案並非將引數傳遞給 `CreateDefaultBuilder`，而是允許 `ConfigurationBuilder` 方法的 `AddCommandLine` 方法同時處理引數與切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="942d5-297">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="942d5-298">建立切換對應字典之後，它會包含下表中所示的資料。</span><span class="sxs-lookup"><span data-stu-id="942d5-298">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="942d5-299">Key</span><span class="sxs-lookup"><span data-stu-id="942d5-299">Key</span></span>       | <span data-ttu-id="942d5-300">值</span><span class="sxs-lookup"><span data-stu-id="942d5-300">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="942d5-301">若啟動應用程式時使用切換對應機碼，設定會接收由字典提供之機碼上的設定值：</span><span class="sxs-lookup"><span data-stu-id="942d5-301">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="942d5-302">執行上述命令之後，設定包含下表中顯示的值。</span><span class="sxs-lookup"><span data-stu-id="942d5-302">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="942d5-303">Key</span><span class="sxs-lookup"><span data-stu-id="942d5-303">Key</span></span>               | <span data-ttu-id="942d5-304">值</span><span class="sxs-lookup"><span data-stu-id="942d5-304">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="942d5-305">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-305">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="942d5-306"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> 會在執行階段從環境變數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-306">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="942d5-307">若要啟用環境變數設定，請在 <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="942d5-307">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="942d5-308">[Azure App Service](https://azure.microsoft.com/services/app-service/)允許在 Azure 入口網站中設定環境變數，以使用環境變數設定提供者來覆寫應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-308">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="942d5-309">如需詳細資訊，請參閱 [Azure App：使用 Azure 入口網站覆寫應用程式設定](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)。</span><span class="sxs-lookup"><span data-stu-id="942d5-309">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="942d5-310">使用`AddEnvironmentVariables`一般主機`DOTNET_`初始化新的主機建立器並呼叫 [ 時，可使用 ](#host-versus-app-configuration) 為[主機組態](xref:fundamentals/host/generic-host)載入字首為 `CreateDefaultBuilder` 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="942d5-310">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="942d5-311">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="942d5-311">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="942d5-312">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="942d5-312">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="942d5-313">來自無首碼之環境變數的應用程式組態 (在未提供首碼的情況下呼叫 `AddEnvironmentVariables`)。</span><span class="sxs-lookup"><span data-stu-id="942d5-313">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="942d5-314">從 *appsettings.json* 與 *appsettings.{Environment}.json* 檔案載入其他選擇性組態。</span><span class="sxs-lookup"><span data-stu-id="942d5-314">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="942d5-315">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="942d5-315">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="942d5-316">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="942d5-316">Command-line arguments.</span></span>

<span data-ttu-id="942d5-317">從使用者祕密與 *appsettings* 檔案建立設定之後，會呼叫「環境變數設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="942d5-317">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="942d5-318">在此位置呼叫提供者可讓系統在執行階段讀取環境變數，以覆寫由使用者祕密與 *appsettings* 檔案所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-318">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="942d5-319">若要從其他環境變數提供應用程式設定，請在 `ConfigureAppConfiguration` 中呼叫應用程式的其他提供者，並使用前置詞呼叫 `AddEnvironmentVariables`：</span><span class="sxs-lookup"><span data-stu-id="942d5-319">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="942d5-320">呼叫 last `AddEnvironmentVariables`，以允許具有指定前置詞的環境變數覆寫其他提供者的值。</span><span class="sxs-lookup"><span data-stu-id="942d5-320">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="942d5-321">**範例**</span><span class="sxs-lookup"><span data-stu-id="942d5-321">**Example**</span></span>

<span data-ttu-id="942d5-322">範例應用程式利用靜態方便方法 `CreateDefaultBuilder` 的優勢來建置主機，這包括對 `AddEnvironmentVariables` 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="942d5-322">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="942d5-323">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="942d5-323">Run the sample app.</span></span> <span data-ttu-id="942d5-324">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="942d5-324">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="942d5-325">觀察輸出是否包含環境變數 `ENVIRONMENT` 的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="942d5-325">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="942d5-326">值反映應用程式執行所在環境，在本機執行時，通常是 `Development`。</span><span class="sxs-lookup"><span data-stu-id="942d5-326">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="942d5-327">為縮短由應用程式轉譯的環境變數清單，應用程式會篩選環境變數。</span><span class="sxs-lookup"><span data-stu-id="942d5-327">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="942d5-328">請參閱範例應用程式的 *Pages/Index.cshtml.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="942d5-328">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="942d5-329">若要公開應用程式可用的所有環境變數，請將*Pages/Index. cshtml*中的 `FilteredConfiguration` 變更為下列內容：</span><span class="sxs-lookup"><span data-stu-id="942d5-329">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="942d5-330">首碼</span><span class="sxs-lookup"><span data-stu-id="942d5-330">Prefixes</span></span>

<span data-ttu-id="942d5-331">在 `AddEnvironmentVariables` 方法中提供前置詞時，會篩選載入應用程式設定中的環境變數。</span><span class="sxs-lookup"><span data-stu-id="942d5-331">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="942d5-332">例如，若要篩選前置詞為 `CUSTOM_` 的環境變數，請提供前置詞給設定提供者：</span><span class="sxs-lookup"><span data-stu-id="942d5-332">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="942d5-333">建立設定機碼值組時，會移除前置詞。</span><span class="sxs-lookup"><span data-stu-id="942d5-333">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="942d5-334">建立主機建立器時，主機組態由環境變數提供。</span><span class="sxs-lookup"><span data-stu-id="942d5-334">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="942d5-335">如需這些環境變數所用前置詞的詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="942d5-335">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="942d5-336">**連接字串前置詞**</span><span class="sxs-lookup"><span data-stu-id="942d5-336">**Connection string prefixes**</span></span>

<span data-ttu-id="942d5-337">設定 API 四個連接字串環境變數的特殊處理規則，這些這些環境變數牽涉到針對應用程式環境設定 Azure 連接字串。</span><span class="sxs-lookup"><span data-stu-id="942d5-337">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="942d5-338">若將前置詞提供給 `AddEnvironmentVariables`具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="942d5-338">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="942d5-339">連接字串前置詞</span><span class="sxs-lookup"><span data-stu-id="942d5-339">Connection string prefix</span></span> | <span data-ttu-id="942d5-340">提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-340">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="942d5-341">自訂提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-341">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="942d5-342">MySQL</span><span class="sxs-lookup"><span data-stu-id="942d5-342">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="942d5-343">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="942d5-343">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="942d5-344">SQL Server</span><span class="sxs-lookup"><span data-stu-id="942d5-344">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="942d5-345">當探索到具有下表所顯示之任何四個前置詞的環境變數並將其載入到設定中時：</span><span class="sxs-lookup"><span data-stu-id="942d5-345">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="942d5-346">會透過移除環境變數前置詞並新增設定機碼區段 (`ConnectionStrings`) 來移除具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="942d5-346">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="942d5-347">會建立新的設定機碼值組以代表資料庫連線提供者 (`CUSTOMCONNSTR_` 除外，這沒有所述提供者)。</span><span class="sxs-lookup"><span data-stu-id="942d5-347">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="942d5-348">環境變數機碼</span><span class="sxs-lookup"><span data-stu-id="942d5-348">Environment variable key</span></span> | <span data-ttu-id="942d5-349">已轉換的設定機碼</span><span class="sxs-lookup"><span data-stu-id="942d5-349">Converted configuration key</span></span> | <span data-ttu-id="942d5-350">提供者設定項目</span><span class="sxs-lookup"><span data-stu-id="942d5-350">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="942d5-351">設定項目未建立。</span><span class="sxs-lookup"><span data-stu-id="942d5-351">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="942d5-352">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="942d5-352">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="942d5-353">值: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="942d5-353">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="942d5-354">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="942d5-354">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="942d5-355">值: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="942d5-355">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="942d5-356">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="942d5-356">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="942d5-357">值: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="942d5-357">Value: `System.Data.SqlClient`</span></span>  |

<span data-ttu-id="942d5-358">**範例**</span><span class="sxs-lookup"><span data-stu-id="942d5-358">**Example**</span></span>

<span data-ttu-id="942d5-359">在伺服器上建立自訂連接字串環境變數：</span><span class="sxs-lookup"><span data-stu-id="942d5-359">A custom connection string environment variable is created on the server:</span></span>

* <span data-ttu-id="942d5-360">名稱 &ndash; `CUSTOMCONNSTR_ReleaseDB`</span><span class="sxs-lookup"><span data-stu-id="942d5-360">Name &ndash; `CUSTOMCONNSTR_ReleaseDB`</span></span>
* <span data-ttu-id="942d5-361">值 &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span><span class="sxs-lookup"><span data-stu-id="942d5-361">Value &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span></span>

<span data-ttu-id="942d5-362">如果 `IConfiguration` 插入，並將其指派給名為 `_config`的欄位，請閱讀值：</span><span class="sxs-lookup"><span data-stu-id="942d5-362">If `IConfiguration` is injected and assigned to a field named `_config`, read the value:</span></span>

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a><span data-ttu-id="942d5-363">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-363">File Configuration Provider</span></span>

<span data-ttu-id="942d5-364"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> 是用於從檔案系統載入設定的基底類別。</span><span class="sxs-lookup"><span data-stu-id="942d5-364"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="942d5-365">下列設定提供者專用於特定檔案類型：</span><span class="sxs-lookup"><span data-stu-id="942d5-365">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="942d5-366">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-366">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="942d5-367">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-367">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="942d5-368">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-368">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="942d5-369">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-369">INI Configuration Provider</span></span>

<span data-ttu-id="942d5-370"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> 會在執行階段從 INI 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-370">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="942d5-371">若要啟用 INI 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="942d5-371">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="942d5-372">冒號可用來做為 INI 檔案設定中的區段分隔符號。</span><span class="sxs-lookup"><span data-stu-id="942d5-372">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="942d5-373">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="942d5-373">Overloads permit specifying:</span></span>

* <span data-ttu-id="942d5-374">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="942d5-374">Whether the file is optional.</span></span>
* <span data-ttu-id="942d5-375">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-375">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="942d5-376"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="942d5-376">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="942d5-377">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="942d5-377">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="942d5-378">INI 設定檔的一般範例：</span><span class="sxs-lookup"><span data-stu-id="942d5-378">A generic example of an INI configuration file:</span></span>

```ini
[section0]
key0=value
key1=value

[section1]
subsection:key=value

[section2:subsection0]
key=value

[section2:subsection1]
key=value
```

<span data-ttu-id="942d5-379">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="942d5-379">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="942d5-380">section0:key0</span><span class="sxs-lookup"><span data-stu-id="942d5-380">section0:key0</span></span>
* <span data-ttu-id="942d5-381">section0:key1</span><span class="sxs-lookup"><span data-stu-id="942d5-381">section0:key1</span></span>
* <span data-ttu-id="942d5-382">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="942d5-382">section1:subsection:key</span></span>
* <span data-ttu-id="942d5-383">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="942d5-383">section2:subsection0:key</span></span>
* <span data-ttu-id="942d5-384">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="942d5-384">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="942d5-385">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-385">JSON Configuration Provider</span></span>

<span data-ttu-id="942d5-386"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> 會在執行階段從 JSON 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-386">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="942d5-387">若要啟用 JSON 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="942d5-387">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="942d5-388">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="942d5-388">Overloads permit specifying:</span></span>

* <span data-ttu-id="942d5-389">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="942d5-389">Whether the file is optional.</span></span>
* <span data-ttu-id="942d5-390">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-390">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="942d5-391"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="942d5-391">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="942d5-392">當使用 `CreateDefaultBuilder`初始化新的主機產生器時，會自動呼叫 `AddJsonFile` 兩次。</span><span class="sxs-lookup"><span data-stu-id="942d5-392">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="942d5-393">會呼叫此方法以從下列位置載入設定：</span><span class="sxs-lookup"><span data-stu-id="942d5-393">The method is called to load configuration from:</span></span>

* <span data-ttu-id="942d5-394">*appsettings*會先讀取此檔案 &ndash;。</span><span class="sxs-lookup"><span data-stu-id="942d5-394">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="942d5-395">檔案的環境版本可以覆寫由 *appsettings.json* 檔案提供的值。</span><span class="sxs-lookup"><span data-stu-id="942d5-395">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="942d5-396">*appsettings。{環境}. json* &ndash; 檔案的環境版本是根據[IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)載入。</span><span class="sxs-lookup"><span data-stu-id="942d5-396">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="942d5-397">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="942d5-397">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="942d5-398">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="942d5-398">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="942d5-399">環境變數。</span><span class="sxs-lookup"><span data-stu-id="942d5-399">Environment variables.</span></span>
* <span data-ttu-id="942d5-400">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="942d5-400">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="942d5-401">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="942d5-401">Command-line arguments.</span></span>

<span data-ttu-id="942d5-402">會先建立 JSON 設定提供者。</span><span class="sxs-lookup"><span data-stu-id="942d5-402">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="942d5-403">因此，使用者祕密、環境變數與命令列引數會覆寫由 *appsettings* 檔案設定的設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-403">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="942d5-404">在建置主機時，呼叫 `ConfigureAppConfiguration` 以指定檔案 (除了 *appsettings.json* 和  *appsettings.{Environment}.json* 以外) 的應用程式設定：</span><span class="sxs-lookup"><span data-stu-id="942d5-404">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="942d5-405">**範例**</span><span class="sxs-lookup"><span data-stu-id="942d5-405">**Example**</span></span>

<span data-ttu-id="942d5-406">範例應用程式會利用靜態便利方法 `CreateDefaultBuilder` 來建立主機，其中包括兩個 `AddJsonFile`呼叫：</span><span class="sxs-lookup"><span data-stu-id="942d5-406">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

* <span data-ttu-id="942d5-407">第一次呼叫 `AddJsonFile` 會從*appsettings*載入設定：</span><span class="sxs-lookup"><span data-stu-id="942d5-407">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="942d5-408">第二次呼叫 `AddJsonFile` 會從 appsettings 載入設定 *。 {環境}. json*。</span><span class="sxs-lookup"><span data-stu-id="942d5-408">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="942d5-409">適用于*appsettings。* 範例應用程式中的開發 json 會載入下列檔案：</span><span class="sxs-lookup"><span data-stu-id="942d5-409">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="942d5-410">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="942d5-410">Run the sample app.</span></span> <span data-ttu-id="942d5-411">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="942d5-411">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="942d5-412">輸出包含以應用程式環境為基礎之設定的索引鍵/值組。</span><span class="sxs-lookup"><span data-stu-id="942d5-412">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="942d5-413">在開發環境中執行應用程式時，會 `Debug` 金鑰 `Logging:LogLevel:Default` 的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="942d5-413">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="942d5-414">在生產環境中再次執行範例應用程式：</span><span class="sxs-lookup"><span data-stu-id="942d5-414">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="942d5-415">開啟*Properties/launchsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="942d5-415">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="942d5-416">在 `ConfigurationSample` 設定檔中，將 `ASPNETCORE_ENVIRONMENT` 環境變數的值變更為 [`Production`]。</span><span class="sxs-lookup"><span data-stu-id="942d5-416">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="942d5-417">儲存檔案，並使用命令 shell 中的 `dotnet run` 來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="942d5-417">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="942d5-418">Appsettings 中的設定 *。* 在*appsettings*中，不會再覆寫 json 中的設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-418">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="942d5-419">`Information`金鑰 `Logging:LogLevel:Default` 的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="942d5-419">The log level for the key `Logging:LogLevel:Default` is `Information`.</span></span>

### <a name="xml-configuration-provider"></a><span data-ttu-id="942d5-420">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-420">XML Configuration Provider</span></span>

<span data-ttu-id="942d5-421"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> 會在執行階段從 XML 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-421">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="942d5-422">若要啟用 XML 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="942d5-422">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="942d5-423">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="942d5-423">Overloads permit specifying:</span></span>

* <span data-ttu-id="942d5-424">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="942d5-424">Whether the file is optional.</span></span>
* <span data-ttu-id="942d5-425">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-425">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="942d5-426"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="942d5-426">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="942d5-427">建立設定機碼值組時，會忽略設定檔案的根節點。</span><span class="sxs-lookup"><span data-stu-id="942d5-427">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="942d5-428">請勿在檔案中指定文件類型定義 (DTD) 或命名空間。</span><span class="sxs-lookup"><span data-stu-id="942d5-428">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="942d5-429">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="942d5-429">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="942d5-430">XML 設定檔可以針對重複的區段使用相異元素名稱：</span><span class="sxs-lookup"><span data-stu-id="942d5-430">XML configuration files can use distinct element names for repeating sections:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section0>
    <key0>value</key0>
    <key1>value</key1>
  </section0>
  <section1>
    <key0>value</key0>
    <key1>value</key1>
  </section1>
</configuration>
```

<span data-ttu-id="942d5-431">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="942d5-431">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="942d5-432">section0:key0</span><span class="sxs-lookup"><span data-stu-id="942d5-432">section0:key0</span></span>
* <span data-ttu-id="942d5-433">section0:key1</span><span class="sxs-lookup"><span data-stu-id="942d5-433">section0:key1</span></span>
* <span data-ttu-id="942d5-434">section1:key0</span><span class="sxs-lookup"><span data-stu-id="942d5-434">section1:key0</span></span>
* <span data-ttu-id="942d5-435">section1:key1</span><span class="sxs-lookup"><span data-stu-id="942d5-435">section1:key1</span></span>

<span data-ttu-id="942d5-436">若 `name` 屬性是用來區別元素，則可以重複那些使用相同元素名稱的元素：</span><span class="sxs-lookup"><span data-stu-id="942d5-436">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section name="section0">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
  <section name="section1">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
</configuration>
```

<span data-ttu-id="942d5-437">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="942d5-437">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="942d5-438">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="942d5-438">section:section0:key:key0</span></span>
* <span data-ttu-id="942d5-439">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="942d5-439">section:section0:key:key1</span></span>
* <span data-ttu-id="942d5-440">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="942d5-440">section:section1:key:key0</span></span>
* <span data-ttu-id="942d5-441">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="942d5-441">section:section1:key:key1</span></span>

<span data-ttu-id="942d5-442">屬性可用來提供值：</span><span class="sxs-lookup"><span data-stu-id="942d5-442">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="942d5-443">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="942d5-443">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="942d5-444">key:attribute</span><span class="sxs-lookup"><span data-stu-id="942d5-444">key:attribute</span></span>
* <span data-ttu-id="942d5-445">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="942d5-445">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="942d5-446">每個檔案機碼設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-446">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="942d5-447"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> 使用目錄的檔案做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="942d5-447">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="942d5-448">機碼是檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="942d5-448">The key is the file name.</span></span> <span data-ttu-id="942d5-449">值包含檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="942d5-449">The value contains the file's contents.</span></span> <span data-ttu-id="942d5-450">每個檔案機碼設定提供者是在 Docker 主機案例中使用。</span><span class="sxs-lookup"><span data-stu-id="942d5-450">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="942d5-451">若要啟用每個檔案機碼設定，請在 <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="942d5-451">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="942d5-452">檔案的 `directoryPath` 必須是絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="942d5-452">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="942d5-453">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="942d5-453">Overloads permit specifying:</span></span>

* <span data-ttu-id="942d5-454">設定來源的 `Action<KeyPerFileConfigurationSource>` 委派。</span><span class="sxs-lookup"><span data-stu-id="942d5-454">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="942d5-455">目錄是否為選擇性與目錄的路徑。</span><span class="sxs-lookup"><span data-stu-id="942d5-455">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="942d5-456">雙底線 (`__`) 是做為檔案名稱中的設定金鑰分隔符號使用。</span><span class="sxs-lookup"><span data-stu-id="942d5-456">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="942d5-457">例如，檔案名稱 `Logging__LogLevel__System` 會產生設定金鑰 `Logging:LogLevel:System`。</span><span class="sxs-lookup"><span data-stu-id="942d5-457">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="942d5-458">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="942d5-458">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="942d5-459">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-459">Memory Configuration Provider</span></span>

<span data-ttu-id="942d5-460"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> 使用記憶體內集合做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="942d5-460">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="942d5-461">若要啟用記憶體內集合設定，請在 <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="942d5-461">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="942d5-462">設定提供者可以使用 `IEnumerable<KeyValuePair<String,String>>` 來初始化。</span><span class="sxs-lookup"><span data-stu-id="942d5-462">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="942d5-463">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-463">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="942d5-464">下列範例會建立組態字典：</span><span class="sxs-lookup"><span data-stu-id="942d5-464">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="942d5-465">字典會與 `AddInMemoryCollection` 的呼叫搭配使用以提供組態：</span><span class="sxs-lookup"><span data-stu-id="942d5-465">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="942d5-466">GetValue</span><span class="sxs-lookup"><span data-stu-id="942d5-466">GetValue</span></span>

<span data-ttu-id="942d5-467">[ConfigurationBinder\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*)會使用指定的索引鍵從設定中解壓縮單一值，並將它轉換成指定的 noncollection 類型。</span><span class="sxs-lookup"><span data-stu-id="942d5-467">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="942d5-468">多載會接受預設值。</span><span class="sxs-lookup"><span data-stu-id="942d5-468">An overload accepts a default value.</span></span>

<span data-ttu-id="942d5-469">下列範例︰</span><span class="sxs-lookup"><span data-stu-id="942d5-469">The following example:</span></span>

* <span data-ttu-id="942d5-470">從具有機碼 `NumberKey` 的組態擷取字串值。</span><span class="sxs-lookup"><span data-stu-id="942d5-470">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="942d5-471">若在組態機碼中找不到 `NumberKey`，則會使用預設值 `99`。</span><span class="sxs-lookup"><span data-stu-id="942d5-471">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="942d5-472">鍵入值為 `int`。</span><span class="sxs-lookup"><span data-stu-id="942d5-472">Types the value as an `int`.</span></span>
* <span data-ttu-id="942d5-473">在 `NumberConfig` 屬性中儲存值供頁面使用。</span><span class="sxs-lookup"><span data-stu-id="942d5-473">Stores the value in the `NumberConfig` property for use by the page.</span></span>

```csharp
public class IndexModel : PageModel
{
    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    public int NumberConfig { get; private set; }

    public void OnGet()
    {
        NumberConfig = _config.GetValue<int>("NumberKey", 99);
    }
}
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="942d5-474">GetSection、GetChildren 與 Exists</span><span class="sxs-lookup"><span data-stu-id="942d5-474">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="942d5-475">針對下面的範例，請考慮下列 JSON 檔案。</span><span class="sxs-lookup"><span data-stu-id="942d5-475">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="942d5-476">在兩個區段中找到四個機碼，其中一個包括子區段組：</span><span class="sxs-lookup"><span data-stu-id="942d5-476">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  },
  "section2": {
    "subsection0" : {
      "key0": "value",
      "key1": "value"
    },
    "subsection1" : {
      "key0": "value",
      "key1": "value"
    }
  }
}
```

<span data-ttu-id="942d5-477">當檔案被讀入到設定之後，會建立下列唯一階層式機碼以存放設定值：</span><span class="sxs-lookup"><span data-stu-id="942d5-477">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="942d5-478">section0:key0</span><span class="sxs-lookup"><span data-stu-id="942d5-478">section0:key0</span></span>
* <span data-ttu-id="942d5-479">section0:key1</span><span class="sxs-lookup"><span data-stu-id="942d5-479">section0:key1</span></span>
* <span data-ttu-id="942d5-480">section1:key0</span><span class="sxs-lookup"><span data-stu-id="942d5-480">section1:key0</span></span>
* <span data-ttu-id="942d5-481">section1:key1</span><span class="sxs-lookup"><span data-stu-id="942d5-481">section1:key1</span></span>
* <span data-ttu-id="942d5-482">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="942d5-482">section2:subsection0:key0</span></span>
* <span data-ttu-id="942d5-483">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="942d5-483">section2:subsection0:key1</span></span>
* <span data-ttu-id="942d5-484">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="942d5-484">section2:subsection1:key0</span></span>
* <span data-ttu-id="942d5-485">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="942d5-485">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="942d5-486">GetSection</span><span class="sxs-lookup"><span data-stu-id="942d5-486">GetSection</span></span>

<span data-ttu-id="942d5-487">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) 會擷取具有所指定子區段機碼的設定子區段。</span><span class="sxs-lookup"><span data-stu-id="942d5-487">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="942d5-488">若要傳回 <xref:Microsoft.Extensions.Configuration.IConfigurationSection> 中只包含一個機碼值組的 `section1`，請呼叫 `GetSection` 並提供區段名稱：</span><span class="sxs-lookup"><span data-stu-id="942d5-488">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="942d5-489">`configSection` 沒有值，只有索引鍵和路徑。</span><span class="sxs-lookup"><span data-stu-id="942d5-489">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="942d5-490">同樣地，若要取得 `section2:subsection0` 中之機碼的值，請呼叫 `GetSection` 並提供區段路徑：</span><span class="sxs-lookup"><span data-stu-id="942d5-490">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="942d5-491">`GetSection` 絕不會傳回 `null`。</span><span class="sxs-lookup"><span data-stu-id="942d5-491">`GetSection` never returns `null`.</span></span> <span data-ttu-id="942d5-492">若找不到相符的區段，會傳回空的 `IConfigurationSection`。</span><span class="sxs-lookup"><span data-stu-id="942d5-492">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="942d5-493">當 `GetSection` 傳回相符區段時，未填入 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value>。</span><span class="sxs-lookup"><span data-stu-id="942d5-493">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="942d5-494">當區段存在時，會傳回 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> 與 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path>。</span><span class="sxs-lookup"><span data-stu-id="942d5-494">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="942d5-495">GetChildren</span><span class="sxs-lookup"><span data-stu-id="942d5-495">GetChildren</span></span>

<span data-ttu-id="942d5-496">對 [ 上之 ](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*)IConfiguration.GetChildren`section2` 的呼叫會取得包括下列項目的 `IEnumerable<IConfigurationSection>`：</span><span class="sxs-lookup"><span data-stu-id="942d5-496">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="942d5-497">Exists</span><span class="sxs-lookup"><span data-stu-id="942d5-497">Exists</span></span>

<span data-ttu-id="942d5-498">使用 [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) 來判斷設定區段是否存在：</span><span class="sxs-lookup"><span data-stu-id="942d5-498">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="942d5-499">以範例資料為例， `sectionExists` 是 `false`，因未設定資料中沒有 `section2:subsection2` 區段。</span><span class="sxs-lookup"><span data-stu-id="942d5-499">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="942d5-500">繫結到類別</span><span class="sxs-lookup"><span data-stu-id="942d5-500">Bind to a class</span></span>

<span data-ttu-id="942d5-501">設定可以繫結到類別，以使用*選項模式*代表相關設定的群組。</span><span class="sxs-lookup"><span data-stu-id="942d5-501">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="942d5-502">如需詳細資訊，請參閱 <xref:fundamentals/configuration/options>。</span><span class="sxs-lookup"><span data-stu-id="942d5-502">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="942d5-503">設定值是以字串傳回，但是呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 會啟用 [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) 物件的建構。</span><span class="sxs-lookup"><span data-stu-id="942d5-503">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="942d5-504">系結器會將值系結至提供之類型的所有公用讀取/寫入屬性。</span><span class="sxs-lookup"><span data-stu-id="942d5-504">The binder binds values to all of the public read/write properties of the type provided.</span></span> <span data-ttu-id="942d5-505">欄位**未**系結。</span><span class="sxs-lookup"><span data-stu-id="942d5-505">Fields are **not** bound.</span></span>

<span data-ttu-id="942d5-506">範例應用程式包含 `Starship` 模型 (*Models/Starship.cs*)：</span><span class="sxs-lookup"><span data-stu-id="942d5-506">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="942d5-507">當範例應用程式使用 JSON 設定提供者來載入設定時，`starship`starship.json*檔案的* 區段會建立設定：</span><span class="sxs-lookup"><span data-stu-id="942d5-507">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

<span data-ttu-id="942d5-508">會建立下列設定機碼值組：</span><span class="sxs-lookup"><span data-stu-id="942d5-508">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="942d5-509">Key</span><span class="sxs-lookup"><span data-stu-id="942d5-509">Key</span></span>                   | <span data-ttu-id="942d5-510">值</span><span class="sxs-lookup"><span data-stu-id="942d5-510">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="942d5-511">starship:name</span><span class="sxs-lookup"><span data-stu-id="942d5-511">starship:name</span></span>         | <span data-ttu-id="942d5-512">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="942d5-512">USS Enterprise</span></span>                                    |
| <span data-ttu-id="942d5-513">starship:registry</span><span class="sxs-lookup"><span data-stu-id="942d5-513">starship:registry</span></span>     | <span data-ttu-id="942d5-514">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="942d5-514">NCC-1701</span></span>                                          |
| <span data-ttu-id="942d5-515">starship:class</span><span class="sxs-lookup"><span data-stu-id="942d5-515">starship:class</span></span>        | <span data-ttu-id="942d5-516">Constitution</span><span class="sxs-lookup"><span data-stu-id="942d5-516">Constitution</span></span>                                      |
| <span data-ttu-id="942d5-517">starship:length</span><span class="sxs-lookup"><span data-stu-id="942d5-517">starship:length</span></span>       | <span data-ttu-id="942d5-518">304.8</span><span class="sxs-lookup"><span data-stu-id="942d5-518">304.8</span></span>                                             |
| <span data-ttu-id="942d5-519">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="942d5-519">starship:commissioned</span></span> | <span data-ttu-id="942d5-520">False</span><span class="sxs-lookup"><span data-stu-id="942d5-520">False</span></span>                                             |
| <span data-ttu-id="942d5-521">trademark</span><span class="sxs-lookup"><span data-stu-id="942d5-521">trademark</span></span>             | <span data-ttu-id="942d5-522">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="942d5-522">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="942d5-523">範例應用程式使用 `GetSection` 機碼呼叫 `starship`。</span><span class="sxs-lookup"><span data-stu-id="942d5-523">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="942d5-524">`starship` 機碼值組會被隔離。</span><span class="sxs-lookup"><span data-stu-id="942d5-524">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="942d5-525">會在子區段上呼叫 `Bind` 方法，並傳入 `Starship` 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="942d5-525">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="942d5-526">繫結執行個體值之後，執行個體會被指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="942d5-526">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="942d5-527">繫結至物件圖形</span><span class="sxs-lookup"><span data-stu-id="942d5-527">Bind to an object graph</span></span>

<span data-ttu-id="942d5-528"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 可以繫結整個 POCO 物件圖形。</span><span class="sxs-lookup"><span data-stu-id="942d5-528"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="942d5-529">如同系結簡單的物件，只會系結公用讀取/寫入屬性。</span><span class="sxs-lookup"><span data-stu-id="942d5-529">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="942d5-530">範例包含 `TvShow` 模型，其物件圖形包括 `Metadata` 與 `Actors` 類別 (*Models/TvShow.cs*)：</span><span class="sxs-lookup"><span data-stu-id="942d5-530">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="942d5-531">範例應用程式有 *tvshow.xml* 檔案，其中包含設定資料：</span><span class="sxs-lookup"><span data-stu-id="942d5-531">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="942d5-532">設定會被繫結到整個 `TvShow` 物件 (使用 `Bind` 方法)。</span><span class="sxs-lookup"><span data-stu-id="942d5-532">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="942d5-533">已繫結的執行個體會被指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="942d5-533">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="942d5-534">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 會繫結並傳回所指定型別。</span><span class="sxs-lookup"><span data-stu-id="942d5-534">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="942d5-535">`Get<T>` 比使用 `Bind` 更方便。</span><span class="sxs-lookup"><span data-stu-id="942d5-535">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="942d5-536">下列程式碼顯示如何根據上面的範例使用 `Get<T>`，這可讓您直接將已繫結的執行個體指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="942d5-536">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="942d5-537">將陣列繫結到類別</span><span class="sxs-lookup"><span data-stu-id="942d5-537">Bind an array to a class</span></span>

<span data-ttu-id="942d5-538">範例應用程式示範此節中解釋的概念。</span><span class="sxs-lookup"><span data-stu-id="942d5-538">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="942d5-539"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="942d5-539">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="942d5-540">公開數值索引鍵區段（`:0:`、`:1:`&hellip; `:{n}:`）的任何陣列格式，都能夠系結至 POCO 類別陣列。</span><span class="sxs-lookup"><span data-stu-id="942d5-540">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="942d5-541">繫結是由慣例提供。</span><span class="sxs-lookup"><span data-stu-id="942d5-541">Binding is provided by convention.</span></span> <span data-ttu-id="942d5-542">自訂設定提供者不需要實作陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="942d5-542">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="942d5-543">**記憶體內陣列處理**</span><span class="sxs-lookup"><span data-stu-id="942d5-543">**In-memory array processing**</span></span>

<span data-ttu-id="942d5-544">考慮下表中顯示的設定機碼與值。</span><span class="sxs-lookup"><span data-stu-id="942d5-544">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="942d5-545">Key</span><span class="sxs-lookup"><span data-stu-id="942d5-545">Key</span></span>             | <span data-ttu-id="942d5-546">值</span><span class="sxs-lookup"><span data-stu-id="942d5-546">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="942d5-547">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="942d5-547">array:entries:0</span></span> | <span data-ttu-id="942d5-548">value0</span><span class="sxs-lookup"><span data-stu-id="942d5-548">value0</span></span> |
| <span data-ttu-id="942d5-549">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="942d5-549">array:entries:1</span></span> | <span data-ttu-id="942d5-550">value1</span><span class="sxs-lookup"><span data-stu-id="942d5-550">value1</span></span> |
| <span data-ttu-id="942d5-551">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="942d5-551">array:entries:2</span></span> | <span data-ttu-id="942d5-552">value2</span><span class="sxs-lookup"><span data-stu-id="942d5-552">value2</span></span> |
| <span data-ttu-id="942d5-553">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="942d5-553">array:entries:4</span></span> | <span data-ttu-id="942d5-554">value4</span><span class="sxs-lookup"><span data-stu-id="942d5-554">value4</span></span> |
| <span data-ttu-id="942d5-555">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="942d5-555">array:entries:5</span></span> | <span data-ttu-id="942d5-556">value5</span><span class="sxs-lookup"><span data-stu-id="942d5-556">value5</span></span> |

<span data-ttu-id="942d5-557">這些機碼與值是使用「記憶體設定提供者」在範例應用程式中載入：</span><span class="sxs-lookup"><span data-stu-id="942d5-557">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

<span data-ttu-id="942d5-558">陣列會跳過索引 &num;3 的值。</span><span class="sxs-lookup"><span data-stu-id="942d5-558">The array skips a value for index &num;3.</span></span> <span data-ttu-id="942d5-559">設定繫結程式無法繫結 Null 值或在已繫結的物件中建立 Null 項目，這在示範繫結此陣列的結果到某物件的時候已經很清楚。</span><span class="sxs-lookup"><span data-stu-id="942d5-559">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="942d5-560">在範例應用程式中，POCO 類別可用來存放繫結設定資料：</span><span class="sxs-lookup"><span data-stu-id="942d5-560">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="942d5-561">設定資料已繫結到物件：</span><span class="sxs-lookup"><span data-stu-id="942d5-561">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="942d5-562">也可以使用 [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 語法，其會產生更精簡的程式碼：</span><span class="sxs-lookup"><span data-stu-id="942d5-562">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="942d5-563">繫結物件 (`ArrayExample`的執行個體) 會從設定接收陣列資料。</span><span class="sxs-lookup"><span data-stu-id="942d5-563">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="942d5-564">`ArrayExample.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="942d5-564">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="942d5-565">`ArrayExample.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="942d5-565">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="942d5-566">0</span><span class="sxs-lookup"><span data-stu-id="942d5-566">0</span></span>                            | <span data-ttu-id="942d5-567">value0</span><span class="sxs-lookup"><span data-stu-id="942d5-567">value0</span></span>                       |
| <span data-ttu-id="942d5-568">1</span><span class="sxs-lookup"><span data-stu-id="942d5-568">1</span></span>                            | <span data-ttu-id="942d5-569">value1</span><span class="sxs-lookup"><span data-stu-id="942d5-569">value1</span></span>                       |
| <span data-ttu-id="942d5-570">2</span><span class="sxs-lookup"><span data-stu-id="942d5-570">2</span></span>                            | <span data-ttu-id="942d5-571">value2</span><span class="sxs-lookup"><span data-stu-id="942d5-571">value2</span></span>                       |
| <span data-ttu-id="942d5-572">3</span><span class="sxs-lookup"><span data-stu-id="942d5-572">3</span></span>                            | <span data-ttu-id="942d5-573">value4</span><span class="sxs-lookup"><span data-stu-id="942d5-573">value4</span></span>                       |
| <span data-ttu-id="942d5-574">4</span><span class="sxs-lookup"><span data-stu-id="942d5-574">4</span></span>                            | <span data-ttu-id="942d5-575">value5</span><span class="sxs-lookup"><span data-stu-id="942d5-575">value5</span></span>                       |

<span data-ttu-id="942d5-576">繫結物件中的索引 &num;3 存放 `array:4` 設定機碼與其 `value4` 的設定資料。</span><span class="sxs-lookup"><span data-stu-id="942d5-576">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="942d5-577">當繫結包含陣列的設定資料時，設定機碼中的陣列索引只會用來列舉設定資料 (當建立物件時)。</span><span class="sxs-lookup"><span data-stu-id="942d5-577">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="942d5-578">設定資料中不能保留 Null 值，當設定機碼中的陣列略過一或多個索引時，不會在繫結物件中建立 Null 值項目。</span><span class="sxs-lookup"><span data-stu-id="942d5-578">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="942d5-579">由會在設定中產生正確機碼值組的任何設定提供者繫結到 &num; 執行個體之前，可以提供索引 `ArrayExample`3 缺少的設定項目。</span><span class="sxs-lookup"><span data-stu-id="942d5-579">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="942d5-580">若範例包含具有缺少之機碼值組的額外 JSON 設定提供者，`ArrayExample.Entries` 會符合完整的設定陣列：</span><span class="sxs-lookup"><span data-stu-id="942d5-580">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="942d5-581">*missing_value.json*：</span><span class="sxs-lookup"><span data-stu-id="942d5-581">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="942d5-582">在 `ConfigureAppConfiguration` 中：</span><span class="sxs-lookup"><span data-stu-id="942d5-582">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="942d5-583">表格中顯示的機碼值組會載入到設定中。</span><span class="sxs-lookup"><span data-stu-id="942d5-583">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="942d5-584">Key</span><span class="sxs-lookup"><span data-stu-id="942d5-584">Key</span></span>             | <span data-ttu-id="942d5-585">值</span><span class="sxs-lookup"><span data-stu-id="942d5-585">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="942d5-586">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="942d5-586">array:entries:3</span></span> | <span data-ttu-id="942d5-587">value3</span><span class="sxs-lookup"><span data-stu-id="942d5-587">value3</span></span> |

<span data-ttu-id="942d5-588">在「JSON 設定提供者」包含索引 `ArrayExample`3 的項目之後，若繫結 &num; 類別執行個體，`ArrayExample.Entries` 陣列會包括這些值：</span><span class="sxs-lookup"><span data-stu-id="942d5-588">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="942d5-589">`ArrayExample.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="942d5-589">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="942d5-590">`ArrayExample.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="942d5-590">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="942d5-591">0</span><span class="sxs-lookup"><span data-stu-id="942d5-591">0</span></span>                            | <span data-ttu-id="942d5-592">value0</span><span class="sxs-lookup"><span data-stu-id="942d5-592">value0</span></span>                       |
| <span data-ttu-id="942d5-593">1</span><span class="sxs-lookup"><span data-stu-id="942d5-593">1</span></span>                            | <span data-ttu-id="942d5-594">value1</span><span class="sxs-lookup"><span data-stu-id="942d5-594">value1</span></span>                       |
| <span data-ttu-id="942d5-595">2</span><span class="sxs-lookup"><span data-stu-id="942d5-595">2</span></span>                            | <span data-ttu-id="942d5-596">value2</span><span class="sxs-lookup"><span data-stu-id="942d5-596">value2</span></span>                       |
| <span data-ttu-id="942d5-597">3</span><span class="sxs-lookup"><span data-stu-id="942d5-597">3</span></span>                            | <span data-ttu-id="942d5-598">value3</span><span class="sxs-lookup"><span data-stu-id="942d5-598">value3</span></span>                       |
| <span data-ttu-id="942d5-599">4</span><span class="sxs-lookup"><span data-stu-id="942d5-599">4</span></span>                            | <span data-ttu-id="942d5-600">value4</span><span class="sxs-lookup"><span data-stu-id="942d5-600">value4</span></span>                       |
| <span data-ttu-id="942d5-601">5</span><span class="sxs-lookup"><span data-stu-id="942d5-601">5</span></span>                            | <span data-ttu-id="942d5-602">value5</span><span class="sxs-lookup"><span data-stu-id="942d5-602">value5</span></span>                       |

<span data-ttu-id="942d5-603">**JSON 陣列處理**</span><span class="sxs-lookup"><span data-stu-id="942d5-603">**JSON array processing**</span></span>

<span data-ttu-id="942d5-604">若 JSON 檔案包含陣列，會為具有以零為基礎之區段索引的陣列元素建立設定機碼。</span><span class="sxs-lookup"><span data-stu-id="942d5-604">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="942d5-605">在下列設定檔中，`subsection` 是陣列：</span><span class="sxs-lookup"><span data-stu-id="942d5-605">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="942d5-606">「JSON 設定提供者」會將設定資料讀入到下列機碼值組：</span><span class="sxs-lookup"><span data-stu-id="942d5-606">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="942d5-607">Key</span><span class="sxs-lookup"><span data-stu-id="942d5-607">Key</span></span>                     | <span data-ttu-id="942d5-608">值</span><span class="sxs-lookup"><span data-stu-id="942d5-608">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="942d5-609">json_array:key</span><span class="sxs-lookup"><span data-stu-id="942d5-609">json_array:key</span></span>          | <span data-ttu-id="942d5-610">valueA</span><span class="sxs-lookup"><span data-stu-id="942d5-610">valueA</span></span> |
| <span data-ttu-id="942d5-611">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="942d5-611">json_array:subsection:0</span></span> | <span data-ttu-id="942d5-612">valueB</span><span class="sxs-lookup"><span data-stu-id="942d5-612">valueB</span></span> |
| <span data-ttu-id="942d5-613">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="942d5-613">json_array:subsection:1</span></span> | <span data-ttu-id="942d5-614">valueC</span><span class="sxs-lookup"><span data-stu-id="942d5-614">valueC</span></span> |
| <span data-ttu-id="942d5-615">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="942d5-615">json_array:subsection:2</span></span> | <span data-ttu-id="942d5-616">valueD</span><span class="sxs-lookup"><span data-stu-id="942d5-616">valueD</span></span> |

<span data-ttu-id="942d5-617">在範例應用程式中，下列 POCO 類別可用來繫結設定機碼值組：</span><span class="sxs-lookup"><span data-stu-id="942d5-617">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="942d5-618">在繫結之後，`JsonArrayExample.Key` 會存放值 `valueA`。</span><span class="sxs-lookup"><span data-stu-id="942d5-618">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="942d5-619">子區段值會存放在 POCO 陣列屬性 `Subsection` 中。</span><span class="sxs-lookup"><span data-stu-id="942d5-619">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="942d5-620">`JsonArrayExample.Subsection` 索引</span><span class="sxs-lookup"><span data-stu-id="942d5-620">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="942d5-621">`JsonArrayExample.Subsection` 值</span><span class="sxs-lookup"><span data-stu-id="942d5-621">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="942d5-622">0</span><span class="sxs-lookup"><span data-stu-id="942d5-622">0</span></span>                                   | <span data-ttu-id="942d5-623">valueB</span><span class="sxs-lookup"><span data-stu-id="942d5-623">valueB</span></span>                              |
| <span data-ttu-id="942d5-624">1</span><span class="sxs-lookup"><span data-stu-id="942d5-624">1</span></span>                                   | <span data-ttu-id="942d5-625">valueC</span><span class="sxs-lookup"><span data-stu-id="942d5-625">valueC</span></span>                              |
| <span data-ttu-id="942d5-626">2</span><span class="sxs-lookup"><span data-stu-id="942d5-626">2</span></span>                                   | <span data-ttu-id="942d5-627">valueD</span><span class="sxs-lookup"><span data-stu-id="942d5-627">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="942d5-628">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-628">Custom configuration provider</span></span>

<span data-ttu-id="942d5-629">範例應用程式示範如何建立會使用 [Entity Framework (EF)](/ef/core/) 從資料庫讀取設定機碼值組的基本設定提供者。</span><span class="sxs-lookup"><span data-stu-id="942d5-629">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="942d5-630">提供者具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="942d5-630">The provider has the following characteristics:</span></span>

* <span data-ttu-id="942d5-631">EF 記憶體內資料庫會用於示範用途。</span><span class="sxs-lookup"><span data-stu-id="942d5-631">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="942d5-632">若要使用需要連接字串的資料庫，請實作第二個 `ConfigurationBuilder` 以從另一個設定提供者提供連接字串。</span><span class="sxs-lookup"><span data-stu-id="942d5-632">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="942d5-633">啟動時，該提供者會將資料庫資料表讀入到設定中。</span><span class="sxs-lookup"><span data-stu-id="942d5-633">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="942d5-634">該提供者不會以個別機碼為基礎查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="942d5-634">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="942d5-635">未實作變更時重新載入，因此在應用程式啟動後更新資料庫對應用程式設定沒有影響。</span><span class="sxs-lookup"><span data-stu-id="942d5-635">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="942d5-636">定義 `EFConfigurationValue` 實體來在資料庫中存放設定值。</span><span class="sxs-lookup"><span data-stu-id="942d5-636">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="942d5-637">*Models/EFConfigurationValue.cs*：</span><span class="sxs-lookup"><span data-stu-id="942d5-637">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="942d5-638">新增 `EFConfigurationContext` 以存放及存取已設定的值。</span><span class="sxs-lookup"><span data-stu-id="942d5-638">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="942d5-639">*EFConfigurationProvider/EFConfigurationContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="942d5-639">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="942d5-640">建立會實作 <xref:Microsoft.Extensions.Configuration.IConfigurationSource> 的類別。</span><span class="sxs-lookup"><span data-stu-id="942d5-640">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="942d5-641">*EFConfigurationProvider/EFConfigurationSource.cs*：</span><span class="sxs-lookup"><span data-stu-id="942d5-641">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="942d5-642">透過繼承自 <xref:Microsoft.Extensions.Configuration.ConfigurationProvider> 來建立自訂設定提供者。</span><span class="sxs-lookup"><span data-stu-id="942d5-642">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="942d5-643">若資料庫是空的，設定提供者會初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="942d5-643">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="942d5-644">由於設定索引[鍵不區分大小寫](#keys)，用來初始化資料庫的字典會以不區分大小寫的比較子（[StringComparer. OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)）來建立。</span><span class="sxs-lookup"><span data-stu-id="942d5-644">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="942d5-645">*EFConfigurationProvider/EFConfigurationProvider.cs*：</span><span class="sxs-lookup"><span data-stu-id="942d5-645">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="942d5-646">`AddEFConfiguration` 擴充方法允許新增設定來源到 `ConfigurationBuilder`。</span><span class="sxs-lookup"><span data-stu-id="942d5-646">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="942d5-647">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="942d5-647">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="942d5-648">下列程式碼顯示如何在 `EFConfigurationProvider`Program.cs*中使用自訂*：</span><span class="sxs-lookup"><span data-stu-id="942d5-648">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="942d5-649">在啟動期間存取組態</span><span class="sxs-lookup"><span data-stu-id="942d5-649">Access configuration during startup</span></span>

<span data-ttu-id="942d5-650">將 `IConfiguration` 插入到 `Startup` 建構函式，以存取 `Startup.ConfigureServices` 中的設定值。</span><span class="sxs-lookup"><span data-stu-id="942d5-650">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="942d5-651">若要存取 `Startup.Configure` 中的設定，請直接將 `IConfiguration` 插入到方法或從建構函式使用執行個體：</span><span class="sxs-lookup"><span data-stu-id="942d5-651">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

```csharp
public class Startup
{
    private readonly IConfiguration _config;

    public Startup(IConfiguration config)
    {
        _config = config;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        var value = _config["key"];
    }

    public void Configure(IApplicationBuilder app, IConfiguration config)
    {
        var value = config["key"];
    }
}
```

<span data-ttu-id="942d5-652">如需使用啟動方便方法來存取設定的範例，請參閱[應用程式啟動：方便方法](xref:fundamentals/startup#convenience-methods)。</span><span class="sxs-lookup"><span data-stu-id="942d5-652">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="942d5-653">存取 Razor Pages 頁面或 MVC 檢視中的設定</span><span class="sxs-lookup"><span data-stu-id="942d5-653">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="942d5-654">若要在 [Razor Pages 頁面] 頁面或 MVC 檢視中存取組態設定，請針對 [Microsoft.Extensions.Configuration 命名空間](xref:mvc/views/razor#using)[using 指示詞](/dotnet/csharp/language-reference/keywords/using-directive) ([C# 參考：using 指示詞](xref:Microsoft.Extensions.Configuration))，並將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 插入到頁面或檢視中。</span><span class="sxs-lookup"><span data-stu-id="942d5-654">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="942d5-655">在 [Razor 頁面] 頁面中：</span><span class="sxs-lookup"><span data-stu-id="942d5-655">In a Razor Pages page:</span></span>

```cshtml
@page
@model IndexModel
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

<span data-ttu-id="942d5-656">在 MVC 檢視中：</span><span class="sxs-lookup"><span data-stu-id="942d5-656">In an MVC view:</span></span>

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="942d5-657">從外部組件新增設定</span><span class="sxs-lookup"><span data-stu-id="942d5-657">Add configuration from an external assembly</span></span>

<span data-ttu-id="942d5-658"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。</span><span class="sxs-lookup"><span data-stu-id="942d5-658">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="942d5-659">如需詳細資訊，請參閱 <xref:fundamentals/configuration/platform-specific-configuration>。</span><span class="sxs-lookup"><span data-stu-id="942d5-659">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="942d5-660">其他資源</span><span class="sxs-lookup"><span data-stu-id="942d5-660">Additional resources</span></span>

* <xref:fundamentals/configuration/options>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="942d5-661">ASP.NET Core 中的應用程式設定是以由*設定提供者*所建立的機碼值組為基礎。</span><span class="sxs-lookup"><span data-stu-id="942d5-661">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="942d5-662">設定提供者會從各種設定來源將設定資料讀取到機碼值組中：</span><span class="sxs-lookup"><span data-stu-id="942d5-662">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="942d5-663">Azure 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="942d5-663">Azure Key Vault</span></span>
* <span data-ttu-id="942d5-664">Azure 應用程式組態</span><span class="sxs-lookup"><span data-stu-id="942d5-664">Azure App Configuration</span></span>
* <span data-ttu-id="942d5-665">命令列引數</span><span class="sxs-lookup"><span data-stu-id="942d5-665">Command-line arguments</span></span>
* <span data-ttu-id="942d5-666">自訂提供者 (已安裝或已建立)</span><span class="sxs-lookup"><span data-stu-id="942d5-666">Custom providers (installed or created)</span></span>
* <span data-ttu-id="942d5-667">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="942d5-667">Directory files</span></span>
* <span data-ttu-id="942d5-668">環境變數</span><span class="sxs-lookup"><span data-stu-id="942d5-668">Environment variables</span></span>
* <span data-ttu-id="942d5-669">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="942d5-669">In-memory .NET objects</span></span>
* <span data-ttu-id="942d5-670">設定檔</span><span class="sxs-lookup"><span data-stu-id="942d5-670">Settings files</span></span>

<span data-ttu-id="942d5-671">一般組態提供者案例 ([microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) 的組態套件包含在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="942d5-671">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="942d5-672">下列程式碼範例與範例應用程式中的程式碼範例使用 <xref:Microsoft.Extensions.Configuration> 命名空間：</span><span class="sxs-lookup"><span data-stu-id="942d5-672">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="942d5-673">*選項模式*是此主題中所述之設定概念的延伸。</span><span class="sxs-lookup"><span data-stu-id="942d5-673">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="942d5-674">選項使用類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-674">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="942d5-675">如需詳細資訊，請參閱 <xref:fundamentals/configuration/options>。</span><span class="sxs-lookup"><span data-stu-id="942d5-675">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="942d5-676">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="942d5-676">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="942d5-677">主機與應用程式組態的比較</span><span class="sxs-lookup"><span data-stu-id="942d5-677">Host versus app configuration</span></span>

<span data-ttu-id="942d5-678">設定及啟動應用程式之前，會先設定及啟動「主機」。</span><span class="sxs-lookup"><span data-stu-id="942d5-678">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="942d5-679">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="942d5-679">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="942d5-680">應用程式與主機都是使用此主題中所述的設定提供者來設定的。</span><span class="sxs-lookup"><span data-stu-id="942d5-680">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="942d5-681">主機組態機碼/值組也會包含在應用程式的組態中。</span><span class="sxs-lookup"><span data-stu-id="942d5-681">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="942d5-682">如需有關當建置主機時如何使用設定提供者的詳細資訊，以及設定來源如何影響主機設定的詳細資訊，請參閱 <xref:fundamentals/index#host>。</span><span class="sxs-lookup"><span data-stu-id="942d5-682">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="942d5-683">其他設定</span><span class="sxs-lookup"><span data-stu-id="942d5-683">Other configuration</span></span>

<span data-ttu-id="942d5-684">本主題僅適用于*應用程式*設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-684">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="942d5-685">執行和裝載 ASP.NET Core 應用程式的其他層面，是使用本主題未涵蓋的設定檔來設定：</span><span class="sxs-lookup"><span data-stu-id="942d5-685">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="942d5-686">/*launchsettings.json* *，json 是*開發環境的工具設定檔，如下所述：</span><span class="sxs-lookup"><span data-stu-id="942d5-686">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="942d5-687">在 <xref:fundamentals/environments#development>。</span><span class="sxs-lookup"><span data-stu-id="942d5-687">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="942d5-688">在檔集內，用來為開發案例設定 ASP.NET Core 應用程式的檔案。</span><span class="sxs-lookup"><span data-stu-id="942d5-688">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="942d5-689">*web.config*是伺服器設定檔，如下列主題所述：</span><span class="sxs-lookup"><span data-stu-id="942d5-689">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="942d5-690">如需從舊版 ASP.NET 遷移應用程式設定的詳細資訊，請參閱 <xref:migration/proper-to-2x/index#store-configurations>。</span><span class="sxs-lookup"><span data-stu-id="942d5-690">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="942d5-691">預設組態</span><span class="sxs-lookup"><span data-stu-id="942d5-691">Default configuration</span></span>

<span data-ttu-id="942d5-692">以 ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) 範本為基礎的 Web 應用程式，會在建置主機時呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>。</span><span class="sxs-lookup"><span data-stu-id="942d5-692">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="942d5-693">`CreateDefaultBuilder` 會以下列順序提供應用程式的預設組態：</span><span class="sxs-lookup"><span data-stu-id="942d5-693">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="942d5-694">下列項目適用於使用 [Web 主機](xref:fundamentals/host/web-host)的應用程式。</span><span class="sxs-lookup"><span data-stu-id="942d5-694">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="942d5-695">如需使用[一般主機](xref:fundamentals/host/generic-host)時預設組態的詳細資料，請參閱[本主題的最新版本](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="942d5-695">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="942d5-696">主機組態的提供來源：</span><span class="sxs-lookup"><span data-stu-id="942d5-696">Host configuration is provided from:</span></span>
  * <span data-ttu-id="942d5-697">使用`ASPNETCORE_`環境變數組態提供者`ASPNETCORE_ENVIRONMENT`且以 [ 為前置詞 (例如 ](#environment-variables-configuration-provider)) 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="942d5-697">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="942d5-698">載入設定機碼值組時，會移除前置詞 (`ASPNETCORE_`)。</span><span class="sxs-lookup"><span data-stu-id="942d5-698">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="942d5-699">使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="942d5-699">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="942d5-700">應用程式設定的提供來源：</span><span class="sxs-lookup"><span data-stu-id="942d5-700">App configuration is provided from:</span></span>
  * <span data-ttu-id="942d5-701">使用*檔案組態提供者*的 [appsettings.json](#file-configuration-provider)。</span><span class="sxs-lookup"><span data-stu-id="942d5-701">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="942d5-702">使用*檔案組態提供者*的 [appsettings.{Environment}.json](#file-configuration-provider)。</span><span class="sxs-lookup"><span data-stu-id="942d5-702">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="942d5-703">應用程式在使用項目組件之 [ 環境中執行時的](xref:security/app-secrets)秘密管理員`Development`。</span><span class="sxs-lookup"><span data-stu-id="942d5-703">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="942d5-704">使用[環境變數組態提供者](#environment-variables-configuration-provider)的環境變數。</span><span class="sxs-lookup"><span data-stu-id="942d5-704">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="942d5-705">使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="942d5-705">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

## <a name="security"></a><span data-ttu-id="942d5-706">安全性</span><span class="sxs-lookup"><span data-stu-id="942d5-706">Security</span></span>

<span data-ttu-id="942d5-707">採用下列做法來保護敏感性組態資料：</span><span class="sxs-lookup"><span data-stu-id="942d5-707">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="942d5-708">永遠不要將密碼或其他敏感性資料儲存在設定提供者程式碼或純文字設定檔中。</span><span class="sxs-lookup"><span data-stu-id="942d5-708">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="942d5-709">不要在開發或測試環境中使用生產環境祕密。</span><span class="sxs-lookup"><span data-stu-id="942d5-709">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="942d5-710">請在專案外部指定祕密，以防止其意外認可至開放原始碼存放庫。</span><span class="sxs-lookup"><span data-stu-id="942d5-710">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="942d5-711">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="942d5-711">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="942d5-712"><xref:security/app-secrets> &ndash; 包含有關使用環境變數來儲存敏感性資料的建議。</span><span class="sxs-lookup"><span data-stu-id="942d5-712"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="942d5-713">「祕密管理員」使用「檔案設定提供者」以 JSON 檔案在本機系統上存放使用者祕密。</span><span class="sxs-lookup"><span data-stu-id="942d5-713">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="942d5-714">此主題稍後將說明「檔案設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="942d5-714">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="942d5-715">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 可安全地儲存 ASP.NET Core 應用程式的應用程式祕密。</span><span class="sxs-lookup"><span data-stu-id="942d5-715">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="942d5-716">如需詳細資訊，請參閱 <xref:security/key-vault-configuration>。</span><span class="sxs-lookup"><span data-stu-id="942d5-716">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="942d5-717">階層式設定資料</span><span class="sxs-lookup"><span data-stu-id="942d5-717">Hierarchical configuration data</span></span>

<span data-ttu-id="942d5-718">設定 API 可在設定機碼中使用分隔符號來壓平合併階層式資料，以管理階層式設定資料。</span><span class="sxs-lookup"><span data-stu-id="942d5-718">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="942d5-719">在下列 JSON 檔案中，兩個區段的結構式階層中有四個機碼存在：</span><span class="sxs-lookup"><span data-stu-id="942d5-719">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

<span data-ttu-id="942d5-720">當該檔案被讀入設定之後，會建立唯一機碼來維護設定來源的原始階層式資料結構。</span><span class="sxs-lookup"><span data-stu-id="942d5-720">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="942d5-721">系統會使用冒號 (`:`) 將區段與機碼壓平合併以維護原始結構：</span><span class="sxs-lookup"><span data-stu-id="942d5-721">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="942d5-722">section0:key0</span><span class="sxs-lookup"><span data-stu-id="942d5-722">section0:key0</span></span>
* <span data-ttu-id="942d5-723">section0:key1</span><span class="sxs-lookup"><span data-stu-id="942d5-723">section0:key1</span></span>
* <span data-ttu-id="942d5-724">section1:key0</span><span class="sxs-lookup"><span data-stu-id="942d5-724">section1:key0</span></span>
* <span data-ttu-id="942d5-725">section1:key1</span><span class="sxs-lookup"><span data-stu-id="942d5-725">section1:key1</span></span>

<span data-ttu-id="942d5-726"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> 與 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> 方法可用來在設定資料中隔離區段與區段的子系。</span><span class="sxs-lookup"><span data-stu-id="942d5-726"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="942d5-727">[GetSection,、 GetChildren 與 Exists](#getsection-getchildren-and-exists) 中說明這些方法。</span><span class="sxs-lookup"><span data-stu-id="942d5-727">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="942d5-728">慣例</span><span class="sxs-lookup"><span data-stu-id="942d5-728">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="942d5-729">來源和提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-729">Sources and providers</span></span>

<span data-ttu-id="942d5-730">在應用程式啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="942d5-730">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="942d5-731">實作變更偵測的組態提供者能夠在基礎設定變更時重新載入組態。</span><span class="sxs-lookup"><span data-stu-id="942d5-731">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="942d5-732">例如，檔案組態提供者 (將於本主題稍後討論) 和 [Azure Key Vault 組態提供者](xref:security/key-vault-configuration)均會實作變更偵測。</span><span class="sxs-lookup"><span data-stu-id="942d5-732">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="942d5-733">您可以在應用程式的<xref:Microsoft.Extensions.Configuration.IConfiguration>相依性插入 (DI)[ 容器中找到 ](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="942d5-733"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="942d5-734"><xref:Microsoft.Extensions.Configuration.IConfiguration> 可以插入至 Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> 或 MVC <xref:Microsoft.AspNetCore.Mvc.Controller>，以取得類別的設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-734"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="942d5-735">在下列範例中，`_config` 欄位是用來存取設定值：</span><span class="sxs-lookup"><span data-stu-id="942d5-735">In the following examples, the `_config` field is used to access configuration values:</span></span>

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }
}
```

```csharp
public class HomeController : Controller
{
    private readonly IConfiguration _config;

    public HomeController(IConfiguration config)
    {
        _config = config;
    }
}
```

<span data-ttu-id="942d5-736">設定提供者無法使用 DI，因為當它們由主機設定時，它無法使用。</span><span class="sxs-lookup"><span data-stu-id="942d5-736">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="942d5-737">索引鍵</span><span class="sxs-lookup"><span data-stu-id="942d5-737">Keys</span></span>

<span data-ttu-id="942d5-738">設定機碼會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="942d5-738">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="942d5-739">機碼區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="942d5-739">Keys are case-insensitive.</span></span> <span data-ttu-id="942d5-740">例如，`ConnectionString` 與 `connectionstring` 會被視為相等的機碼。</span><span class="sxs-lookup"><span data-stu-id="942d5-740">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="942d5-741">若相同機碼的值是由相同或不同設定提供者設定，在機碼上設定的最後一個值是所使用的值。</span><span class="sxs-lookup"><span data-stu-id="942d5-741">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="942d5-742">階層式機碼</span><span class="sxs-lookup"><span data-stu-id="942d5-742">Hierarchical keys</span></span>
  * <span data-ttu-id="942d5-743">在設定 API 內，冒號分隔字元 (`:`) 可在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="942d5-743">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="942d5-744">在環境變數中，冒號分隔字元可能無法在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="942d5-744">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="942d5-745">所有平台都支援雙底線 (`__`)，且會自動轉換為冒號。</span><span class="sxs-lookup"><span data-stu-id="942d5-745">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="942d5-746">在 Azure Key Vault 中，階層式機碼使用 `--` (兩個破折號) 來做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="942d5-746">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="942d5-747">撰寫程式碼，以在將密碼載入應用程式的設定時，以冒號取代破折號。</span><span class="sxs-lookup"><span data-stu-id="942d5-747">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="942d5-748"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="942d5-748">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="942d5-749">[將陣列繫結到類別](#bind-an-array-to-a-class)一節說明陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="942d5-749">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="942d5-750">值</span><span class="sxs-lookup"><span data-stu-id="942d5-750">Values</span></span>

<span data-ttu-id="942d5-751">設定值會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="942d5-751">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="942d5-752">值是字串。</span><span class="sxs-lookup"><span data-stu-id="942d5-752">Values are strings.</span></span>
* <span data-ttu-id="942d5-753">Null 值無法存放在設定中或繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="942d5-753">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="942d5-754">提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-754">Providers</span></span>

<span data-ttu-id="942d5-755">下表顯示可供 ASP.NET Core 應用程式使用的設定提供者。</span><span class="sxs-lookup"><span data-stu-id="942d5-755">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="942d5-756">提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-756">Provider</span></span> | <span data-ttu-id="942d5-757">從&hellip;提供設定</span><span class="sxs-lookup"><span data-stu-id="942d5-757">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="942d5-758">[Azure Key Vault 設定提供者](xref:security/key-vault-configuration) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="942d5-758">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="942d5-759">Azure 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="942d5-759">Azure Key Vault</span></span> |
| <span data-ttu-id="942d5-760">[Azure 應用程式組態提供者](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure 文件)</span><span class="sxs-lookup"><span data-stu-id="942d5-760">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="942d5-761">Azure 應用程式組態</span><span class="sxs-lookup"><span data-stu-id="942d5-761">Azure App Configuration</span></span> |
| [<span data-ttu-id="942d5-762">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-762">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="942d5-763">命令列參數</span><span class="sxs-lookup"><span data-stu-id="942d5-763">Command-line parameters</span></span> |
| [<span data-ttu-id="942d5-764">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-764">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="942d5-765">自訂來源</span><span class="sxs-lookup"><span data-stu-id="942d5-765">Custom source</span></span> |
| [<span data-ttu-id="942d5-766">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-766">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="942d5-767">環境變數</span><span class="sxs-lookup"><span data-stu-id="942d5-767">Environment variables</span></span> |
| [<span data-ttu-id="942d5-768">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-768">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="942d5-769">檔案 (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="942d5-769">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="942d5-770">每個檔案機碼的設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-770">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="942d5-771">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="942d5-771">Directory files</span></span> |
| [<span data-ttu-id="942d5-772">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-772">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="942d5-773">記憶體內集合</span><span class="sxs-lookup"><span data-stu-id="942d5-773">In-memory collections</span></span> |
| <span data-ttu-id="942d5-774">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="942d5-774">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="942d5-775">使用者設定檔目錄中的檔案</span><span class="sxs-lookup"><span data-stu-id="942d5-775">File in the user profile directory</span></span> |

<span data-ttu-id="942d5-776">在啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="942d5-776">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="942d5-777">本主題所描述的設定提供者會依字母順序描述，而不是程式碼排列它們的順序。</span><span class="sxs-lookup"><span data-stu-id="942d5-777">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="942d5-778">請在程式碼中訂購設定提供者，以符合應用程式所需之基礎設定來源的優先順序。</span><span class="sxs-lookup"><span data-stu-id="942d5-778">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="942d5-779">典型的設定提供者順序是：</span><span class="sxs-lookup"><span data-stu-id="942d5-779">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="942d5-780">檔案 (*appsettings.json*、*appsettings.{Environment}.json*，其中 `{Environment}` 是應用程式的目前裝載環境)</span><span class="sxs-lookup"><span data-stu-id="942d5-780">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="942d5-781">Azure 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="942d5-781">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="942d5-782">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (僅限開發環境)</span><span class="sxs-lookup"><span data-stu-id="942d5-782">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="942d5-783">環境變數</span><span class="sxs-lookup"><span data-stu-id="942d5-783">Environment variables</span></span>
1. <span data-ttu-id="942d5-784">命令列引數</span><span class="sxs-lookup"><span data-stu-id="942d5-784">Command-line arguments</span></span>

<span data-ttu-id="942d5-785">將命令列組態提供者放在提供者序列結尾是常見做法，因為這樣可以讓命令列引數覆寫由其他提供者所設定的組態。</span><span class="sxs-lookup"><span data-stu-id="942d5-785">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="942d5-786">當使用 `CreateDefaultBuilder`初始化新的主機產生器時，會使用上述的提供者序列。</span><span class="sxs-lookup"><span data-stu-id="942d5-786">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="942d5-787">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="942d5-787">For more information, see the [Default configuration](#default-configuration) section.</span></span>

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="942d5-788">使用 UseConfiguration 設定主機建立器</span><span class="sxs-lookup"><span data-stu-id="942d5-788">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="942d5-789">若要設定主機建立器，請使用該組態在主機建立器上呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>。</span><span class="sxs-lookup"><span data-stu-id="942d5-789">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

    var config = new ConfigurationBuilder()
        .AddInMemoryCollection(dict)
        .Build();

    return WebHost.CreateDefaultBuilder(args)
        .UseConfiguration(config)
        .UseStartup<Startup>();
}
```

## <a name="configureappconfiguration"></a><span data-ttu-id="942d5-790">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="942d5-790">ConfigureAppConfiguration</span></span>

<span data-ttu-id="942d5-791">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定提供者，以及由 `CreateDefaultBuilder` 自動新增的設定提供者：</span><span class="sxs-lookup"><span data-stu-id="942d5-791">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="942d5-792">使用命令列引數覆寫先前的組態</span><span class="sxs-lookup"><span data-stu-id="942d5-792">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="942d5-793">若要提供可使用命令列引數覆寫的應用程式組態，請最後呼叫 `AddCommandLine`：</span><span class="sxs-lookup"><span data-stu-id="942d5-793">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="942d5-794">移除 CreateDefaultBuilder 新增的提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-794">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="942d5-795">若要移除 `CreateDefaultBuilder`新增的提供者，請先在[IConfigurationBuilder](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources)上呼叫[Clear](/dotnet/api/system.collections.generic.icollection-1.clear) ：</span><span class="sxs-lookup"><span data-stu-id="942d5-795">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="942d5-796">在應用程式啟動期間使用組態</span><span class="sxs-lookup"><span data-stu-id="942d5-796">Consume configuration during app startup</span></span>

<span data-ttu-id="942d5-797">應用程式啟動期間，可以使用 `ConfigureAppConfiguration` 中為應用程式提供的組態，包括 `Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="942d5-797">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="942d5-798">如需詳細資訊，請參閱[在啟動期間存取組態](#access-configuration-during-startup)一節。</span><span class="sxs-lookup"><span data-stu-id="942d5-798">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="942d5-799">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-799">Command-line Configuration Provider</span></span>

<span data-ttu-id="942d5-800"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> 會在執行階段從命令列引數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-800">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="942d5-801">為了啟用命令列設定，<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 延伸模組方法會在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫。</span><span class="sxs-lookup"><span data-stu-id="942d5-801">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="942d5-802">呼叫 `AddCommandLine` 時，會自動呼叫 `CreateDefaultBuilder(string [])`。</span><span class="sxs-lookup"><span data-stu-id="942d5-802">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="942d5-803">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="942d5-803">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="942d5-804">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="942d5-804">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="942d5-805">從 *appsettings.json* 與 *appsettings.{Environment}.json* 檔案載入其他選擇性組態。</span><span class="sxs-lookup"><span data-stu-id="942d5-805">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="942d5-806">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="942d5-806">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="942d5-807">環境變數。</span><span class="sxs-lookup"><span data-stu-id="942d5-807">Environment variables.</span></span>

<span data-ttu-id="942d5-808">`CreateDefaultBuilder` 會最後才新增命令列設定提供者。</span><span class="sxs-lookup"><span data-stu-id="942d5-808">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="942d5-809">在執行階段傳遞的命令列引數會覆寫由其它提供者所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-809">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="942d5-810">`CreateDefaultBuilder` 會在建構主機時執行作業。</span><span class="sxs-lookup"><span data-stu-id="942d5-810">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="942d5-811">因此，由`CreateDefaultBuilder` 啟用的命令列設定可以影響主機的設定方式。</span><span class="sxs-lookup"><span data-stu-id="942d5-811">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="942d5-812">針對以 ASP.NET Core 範本為基礎的應用程式，`AddCommandLine` 已由 `CreateDefaultBuilder` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="942d5-812">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="942d5-813">若要新增其他組態提供者並維持能夠使用命令列引數覆寫這些提供者組態的能力，請在 `ConfigureAppConfiguration` 中呼叫應用程式的其他提供者，且最後呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="942d5-813">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="942d5-814">**範例**</span><span class="sxs-lookup"><span data-stu-id="942d5-814">**Example**</span></span>

<span data-ttu-id="942d5-815">範例應用程式利用靜態方便方法 `CreateDefaultBuilder` 的優勢來建置主機，這包括對 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="942d5-815">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="942d5-816">從專案目錄中開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="942d5-816">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="942d5-817">提供命令列引數給 `dotnet run` 命令 `dotnet run CommandLineKey=CommandLineValue`。</span><span class="sxs-lookup"><span data-stu-id="942d5-817">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="942d5-818">在應用程式執行之後，開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="942d5-818">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="942d5-819">觀察輸出是否包含提供給 `dotnet run` 之設定命令列引數的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="942d5-819">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="942d5-820">引數</span><span class="sxs-lookup"><span data-stu-id="942d5-820">Arguments</span></span>

<span data-ttu-id="942d5-821">當值後面接著空格時，值必須接著等號 (`=`)，或機碼必須有前置詞 (`--` 或 `/`)。</span><span class="sxs-lookup"><span data-stu-id="942d5-821">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="942d5-822">如果使用等號 (例如 `CommandLineKey=`)，則不需要此值。</span><span class="sxs-lookup"><span data-stu-id="942d5-822">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="942d5-823">索引鍵前置字元</span><span class="sxs-lookup"><span data-stu-id="942d5-823">Key prefix</span></span>               | <span data-ttu-id="942d5-824">範例</span><span class="sxs-lookup"><span data-stu-id="942d5-824">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="942d5-825">沒有前置字元</span><span class="sxs-lookup"><span data-stu-id="942d5-825">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="942d5-826">雙虛線 (`--`)</span><span class="sxs-lookup"><span data-stu-id="942d5-826">Two dashes (`--`)</span></span>        | <span data-ttu-id="942d5-827">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="942d5-827">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="942d5-828">正斜線 (`/`)</span><span class="sxs-lookup"><span data-stu-id="942d5-828">Forward slash (`/`)</span></span>      | <span data-ttu-id="942d5-829">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="942d5-829">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="942d5-830">在相同的命令中，請不要混合使用等號搭配使用空格之機碼值組的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="942d5-830">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="942d5-831">範例命令：</span><span class="sxs-lookup"><span data-stu-id="942d5-831">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="942d5-832">切換對應</span><span class="sxs-lookup"><span data-stu-id="942d5-832">Switch mappings</span></span>

<span data-ttu-id="942d5-833">參數對應允許索引鍵名稱取代邏輯。</span><span class="sxs-lookup"><span data-stu-id="942d5-833">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="942d5-834">以 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>手動建立設定時，請將參數取代的字典提供給 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 方法。</span><span class="sxs-lookup"><span data-stu-id="942d5-834">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="942d5-835">使用切換對應字典時，會檢查字典中是否有任何索引鍵符合命令列引數所提供的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="942d5-835">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="942d5-836">如果在字典中找到命令列索引鍵，則會傳回字典值 (索引鍵取代) 以在應用程式的設定中設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="942d5-836">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="942d5-837">所有前面加上單虛線 (`-`) 的命令列索引鍵都需要切換對應。</span><span class="sxs-lookup"><span data-stu-id="942d5-837">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="942d5-838">切換對應字典索引鍵規則：</span><span class="sxs-lookup"><span data-stu-id="942d5-838">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="942d5-839">切換必須以單虛線 (`-`) 或雙虛線 (`--`) 開頭。</span><span class="sxs-lookup"><span data-stu-id="942d5-839">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="942d5-840">切換對應字典不能包含重複索引鍵。</span><span class="sxs-lookup"><span data-stu-id="942d5-840">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="942d5-841">建立切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="942d5-841">Create a switch mappings dictionary.</span></span> <span data-ttu-id="942d5-842">下列範例會建立兩個切換對應：</span><span class="sxs-lookup"><span data-stu-id="942d5-842">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="942d5-843">建立主機時，請使用切換對應字典呼叫 `AddCommandLine`：</span><span class="sxs-lookup"><span data-stu-id="942d5-843">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="942d5-844">針對使用切換對應的應用程式，呼叫 `CreateDefaultBuilder` 不應傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="942d5-844">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="942d5-845">`CreateDefaultBuilder` 方法的 `AddCommandLine` 呼叫不包括對應的切換，且沒有任何方式可以將切換對應字典傳遞給 `CreateDefaultBuilder`。</span><span class="sxs-lookup"><span data-stu-id="942d5-845">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="942d5-846">解決方案並非將引數傳遞給 `CreateDefaultBuilder`，而是允許 `ConfigurationBuilder` 方法的 `AddCommandLine` 方法同時處理引數與切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="942d5-846">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="942d5-847">建立切換對應字典之後，它會包含下表中所示的資料。</span><span class="sxs-lookup"><span data-stu-id="942d5-847">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="942d5-848">Key</span><span class="sxs-lookup"><span data-stu-id="942d5-848">Key</span></span>       | <span data-ttu-id="942d5-849">值</span><span class="sxs-lookup"><span data-stu-id="942d5-849">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="942d5-850">若啟動應用程式時使用切換對應機碼，設定會接收由字典提供之機碼上的設定值：</span><span class="sxs-lookup"><span data-stu-id="942d5-850">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="942d5-851">執行上述命令之後，設定包含下表中顯示的值。</span><span class="sxs-lookup"><span data-stu-id="942d5-851">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="942d5-852">Key</span><span class="sxs-lookup"><span data-stu-id="942d5-852">Key</span></span>               | <span data-ttu-id="942d5-853">值</span><span class="sxs-lookup"><span data-stu-id="942d5-853">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="942d5-854">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-854">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="942d5-855"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> 會在執行階段從環境變數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-855">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="942d5-856">若要啟用環境變數設定，請在 <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="942d5-856">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="942d5-857">[Azure App Service](https://azure.microsoft.com/services/app-service/)允許在 Azure 入口網站中設定環境變數，以使用環境變數設定提供者來覆寫應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-857">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="942d5-858">如需詳細資訊，請參閱 [Azure App：使用 Azure 入口網站覆寫應用程式設定](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)。</span><span class="sxs-lookup"><span data-stu-id="942d5-858">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="942d5-859">使用 `AddEnvironmentVariables`Web 主機`ASPNETCORE_`初始化新的主機建立器並呼叫 [ 時，可使用 ](#host-versus-app-configuration) 為[主機組態](xref:fundamentals/host/web-host)載入字首為 `CreateDefaultBuilder` 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="942d5-859">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="942d5-860">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="942d5-860">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="942d5-861">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="942d5-861">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="942d5-862">來自無首碼之環境變數的應用程式組態 (在未提供首碼的情況下呼叫 `AddEnvironmentVariables`)。</span><span class="sxs-lookup"><span data-stu-id="942d5-862">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="942d5-863">從 *appsettings.json* 與 *appsettings.{Environment}.json* 檔案載入其他選擇性組態。</span><span class="sxs-lookup"><span data-stu-id="942d5-863">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="942d5-864">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="942d5-864">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="942d5-865">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="942d5-865">Command-line arguments.</span></span>

<span data-ttu-id="942d5-866">從使用者祕密與 *appsettings* 檔案建立設定之後，會呼叫「環境變數設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="942d5-866">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="942d5-867">在此位置呼叫提供者可讓系統在執行階段讀取環境變數，以覆寫由使用者祕密與 *appsettings* 檔案所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-867">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="942d5-868">若要從其他環境變數提供應用程式設定，請在 `ConfigureAppConfiguration` 中呼叫應用程式的其他提供者，並使用前置詞呼叫 `AddEnvironmentVariables`：</span><span class="sxs-lookup"><span data-stu-id="942d5-868">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="942d5-869">呼叫 last `AddEnvironmentVariables`，以允許具有指定前置詞的環境變數覆寫其他提供者的值。</span><span class="sxs-lookup"><span data-stu-id="942d5-869">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="942d5-870">**範例**</span><span class="sxs-lookup"><span data-stu-id="942d5-870">**Example**</span></span>

<span data-ttu-id="942d5-871">範例應用程式利用靜態方便方法 `CreateDefaultBuilder` 的優勢來建置主機，這包括對 `AddEnvironmentVariables` 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="942d5-871">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="942d5-872">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="942d5-872">Run the sample app.</span></span> <span data-ttu-id="942d5-873">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="942d5-873">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="942d5-874">觀察輸出是否包含環境變數 `ENVIRONMENT` 的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="942d5-874">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="942d5-875">值反映應用程式執行所在環境，在本機執行時，通常是 `Development`。</span><span class="sxs-lookup"><span data-stu-id="942d5-875">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="942d5-876">為縮短由應用程式轉譯的環境變數清單，應用程式會篩選環境變數。</span><span class="sxs-lookup"><span data-stu-id="942d5-876">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="942d5-877">請參閱範例應用程式的 *Pages/Index.cshtml.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="942d5-877">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="942d5-878">若要公開應用程式可用的所有環境變數，請將*Pages/Index. cshtml*中的 `FilteredConfiguration` 變更為下列內容：</span><span class="sxs-lookup"><span data-stu-id="942d5-878">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="942d5-879">首碼</span><span class="sxs-lookup"><span data-stu-id="942d5-879">Prefixes</span></span>

<span data-ttu-id="942d5-880">在 `AddEnvironmentVariables` 方法中提供前置詞時，會篩選載入應用程式設定中的環境變數。</span><span class="sxs-lookup"><span data-stu-id="942d5-880">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="942d5-881">例如，若要篩選前置詞為 `CUSTOM_` 的環境變數，請提供前置詞給設定提供者：</span><span class="sxs-lookup"><span data-stu-id="942d5-881">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="942d5-882">建立設定機碼值組時，會移除前置詞。</span><span class="sxs-lookup"><span data-stu-id="942d5-882">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="942d5-883">建立主機建立器時，主機組態由環境變數提供。</span><span class="sxs-lookup"><span data-stu-id="942d5-883">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="942d5-884">如需這些環境變數所用前置詞的詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="942d5-884">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="942d5-885">**連接字串前置詞**</span><span class="sxs-lookup"><span data-stu-id="942d5-885">**Connection string prefixes**</span></span>

<span data-ttu-id="942d5-886">設定 API 四個連接字串環境變數的特殊處理規則，這些這些環境變數牽涉到針對應用程式環境設定 Azure 連接字串。</span><span class="sxs-lookup"><span data-stu-id="942d5-886">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="942d5-887">若將前置詞提供給 `AddEnvironmentVariables`具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="942d5-887">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="942d5-888">連接字串前置詞</span><span class="sxs-lookup"><span data-stu-id="942d5-888">Connection string prefix</span></span> | <span data-ttu-id="942d5-889">提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-889">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="942d5-890">自訂提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-890">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="942d5-891">MySQL</span><span class="sxs-lookup"><span data-stu-id="942d5-891">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="942d5-892">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="942d5-892">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="942d5-893">SQL Server</span><span class="sxs-lookup"><span data-stu-id="942d5-893">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="942d5-894">當探索到具有下表所顯示之任何四個前置詞的環境變數並將其載入到設定中時：</span><span class="sxs-lookup"><span data-stu-id="942d5-894">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="942d5-895">會透過移除環境變數前置詞並新增設定機碼區段 (`ConnectionStrings`) 來移除具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="942d5-895">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="942d5-896">會建立新的設定機碼值組以代表資料庫連線提供者 (`CUSTOMCONNSTR_` 除外，這沒有所述提供者)。</span><span class="sxs-lookup"><span data-stu-id="942d5-896">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="942d5-897">環境變數機碼</span><span class="sxs-lookup"><span data-stu-id="942d5-897">Environment variable key</span></span> | <span data-ttu-id="942d5-898">已轉換的設定機碼</span><span class="sxs-lookup"><span data-stu-id="942d5-898">Converted configuration key</span></span> | <span data-ttu-id="942d5-899">提供者設定項目</span><span class="sxs-lookup"><span data-stu-id="942d5-899">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="942d5-900">設定項目未建立。</span><span class="sxs-lookup"><span data-stu-id="942d5-900">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="942d5-901">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="942d5-901">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="942d5-902">值: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="942d5-902">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="942d5-903">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="942d5-903">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="942d5-904">值: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="942d5-904">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="942d5-905">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="942d5-905">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="942d5-906">值: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="942d5-906">Value: `System.Data.SqlClient`</span></span>  |

<span data-ttu-id="942d5-907">**範例**</span><span class="sxs-lookup"><span data-stu-id="942d5-907">**Example**</span></span>

<span data-ttu-id="942d5-908">在伺服器上建立自訂連接字串環境變數：</span><span class="sxs-lookup"><span data-stu-id="942d5-908">A custom connection string environment variable is created on the server:</span></span>

* <span data-ttu-id="942d5-909">名稱 &ndash; `CUSTOMCONNSTR_ReleaseDB`</span><span class="sxs-lookup"><span data-stu-id="942d5-909">Name &ndash; `CUSTOMCONNSTR_ReleaseDB`</span></span>
* <span data-ttu-id="942d5-910">值 &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span><span class="sxs-lookup"><span data-stu-id="942d5-910">Value &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span></span>

<span data-ttu-id="942d5-911">如果 `IConfiguration` 插入，並將其指派給名為 `_config`的欄位，請閱讀值：</span><span class="sxs-lookup"><span data-stu-id="942d5-911">If `IConfiguration` is injected and assigned to a field named `_config`, read the value:</span></span>

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a><span data-ttu-id="942d5-912">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-912">File Configuration Provider</span></span>

<span data-ttu-id="942d5-913"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> 是用於從檔案系統載入設定的基底類別。</span><span class="sxs-lookup"><span data-stu-id="942d5-913"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="942d5-914">下列設定提供者專用於特定檔案類型：</span><span class="sxs-lookup"><span data-stu-id="942d5-914">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="942d5-915">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-915">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="942d5-916">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-916">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="942d5-917">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-917">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="942d5-918">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-918">INI Configuration Provider</span></span>

<span data-ttu-id="942d5-919"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> 會在執行階段從 INI 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-919">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="942d5-920">若要啟用 INI 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="942d5-920">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="942d5-921">冒號可用來做為 INI 檔案設定中的區段分隔符號。</span><span class="sxs-lookup"><span data-stu-id="942d5-921">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="942d5-922">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="942d5-922">Overloads permit specifying:</span></span>

* <span data-ttu-id="942d5-923">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="942d5-923">Whether the file is optional.</span></span>
* <span data-ttu-id="942d5-924">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-924">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="942d5-925"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="942d5-925">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="942d5-926">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="942d5-926">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="942d5-927">INI 設定檔的一般範例：</span><span class="sxs-lookup"><span data-stu-id="942d5-927">A generic example of an INI configuration file:</span></span>

```ini
[section0]
key0=value
key1=value

[section1]
subsection:key=value

[section2:subsection0]
key=value

[section2:subsection1]
key=value
```

<span data-ttu-id="942d5-928">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="942d5-928">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="942d5-929">section0:key0</span><span class="sxs-lookup"><span data-stu-id="942d5-929">section0:key0</span></span>
* <span data-ttu-id="942d5-930">section0:key1</span><span class="sxs-lookup"><span data-stu-id="942d5-930">section0:key1</span></span>
* <span data-ttu-id="942d5-931">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="942d5-931">section1:subsection:key</span></span>
* <span data-ttu-id="942d5-932">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="942d5-932">section2:subsection0:key</span></span>
* <span data-ttu-id="942d5-933">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="942d5-933">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="942d5-934">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-934">JSON Configuration Provider</span></span>

<span data-ttu-id="942d5-935"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> 會在執行階段從 JSON 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-935">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="942d5-936">若要啟用 JSON 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="942d5-936">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="942d5-937">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="942d5-937">Overloads permit specifying:</span></span>

* <span data-ttu-id="942d5-938">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="942d5-938">Whether the file is optional.</span></span>
* <span data-ttu-id="942d5-939">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-939">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="942d5-940"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="942d5-940">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="942d5-941">當使用 `CreateDefaultBuilder`初始化新的主機產生器時，會自動呼叫 `AddJsonFile` 兩次。</span><span class="sxs-lookup"><span data-stu-id="942d5-941">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="942d5-942">會呼叫此方法以從下列位置載入設定：</span><span class="sxs-lookup"><span data-stu-id="942d5-942">The method is called to load configuration from:</span></span>

* <span data-ttu-id="942d5-943">*appsettings*會先讀取此檔案 &ndash;。</span><span class="sxs-lookup"><span data-stu-id="942d5-943">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="942d5-944">檔案的環境版本可以覆寫由 *appsettings.json* 檔案提供的值。</span><span class="sxs-lookup"><span data-stu-id="942d5-944">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="942d5-945">*appsettings。{環境}. json* &ndash; 檔案的環境版本是根據[IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)載入。</span><span class="sxs-lookup"><span data-stu-id="942d5-945">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="942d5-946">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="942d5-946">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="942d5-947">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="942d5-947">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="942d5-948">環境變數。</span><span class="sxs-lookup"><span data-stu-id="942d5-948">Environment variables.</span></span>
* <span data-ttu-id="942d5-949">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="942d5-949">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="942d5-950">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="942d5-950">Command-line arguments.</span></span>

<span data-ttu-id="942d5-951">會先建立 JSON 設定提供者。</span><span class="sxs-lookup"><span data-stu-id="942d5-951">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="942d5-952">因此，使用者祕密、環境變數與命令列引數會覆寫由 *appsettings* 檔案設定的設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-952">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="942d5-953">在建置主機時，呼叫 `ConfigureAppConfiguration` 以指定檔案 (除了 *appsettings.json* 和  *appsettings.{Environment}.json* 以外) 的應用程式設定：</span><span class="sxs-lookup"><span data-stu-id="942d5-953">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="942d5-954">**範例**</span><span class="sxs-lookup"><span data-stu-id="942d5-954">**Example**</span></span>

<span data-ttu-id="942d5-955">範例應用程式會利用靜態便利方法 `CreateDefaultBuilder` 來建立主機，其中包括兩個 `AddJsonFile`呼叫：</span><span class="sxs-lookup"><span data-stu-id="942d5-955">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

* <span data-ttu-id="942d5-956">第一次呼叫 `AddJsonFile` 會從*appsettings*載入設定：</span><span class="sxs-lookup"><span data-stu-id="942d5-956">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="942d5-957">第二次呼叫 `AddJsonFile` 會從 appsettings 載入設定 *。 {環境}. json*。</span><span class="sxs-lookup"><span data-stu-id="942d5-957">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="942d5-958">適用于*appsettings。* 範例應用程式中的開發 json 會載入下列檔案：</span><span class="sxs-lookup"><span data-stu-id="942d5-958">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="942d5-959">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="942d5-959">Run the sample app.</span></span> <span data-ttu-id="942d5-960">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="942d5-960">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="942d5-961">輸出包含以應用程式環境為基礎之設定的索引鍵/值組。</span><span class="sxs-lookup"><span data-stu-id="942d5-961">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="942d5-962">在開發環境中執行應用程式時，會 `Debug` 金鑰 `Logging:LogLevel:Default` 的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="942d5-962">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="942d5-963">在生產環境中再次執行範例應用程式：</span><span class="sxs-lookup"><span data-stu-id="942d5-963">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="942d5-964">開啟*Properties/launchsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="942d5-964">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="942d5-965">在 `ConfigurationSample` 設定檔中，將 `ASPNETCORE_ENVIRONMENT` 環境變數的值變更為 [`Production`]。</span><span class="sxs-lookup"><span data-stu-id="942d5-965">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="942d5-966">儲存檔案，並使用命令 shell 中的 `dotnet run` 來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="942d5-966">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="942d5-967">Appsettings 中的設定 *。* 在*appsettings*中，不會再覆寫 json 中的設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-967">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="942d5-968">`Warning`金鑰 `Logging:LogLevel:Default` 的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="942d5-968">The log level for the key `Logging:LogLevel:Default` is `Warning`.</span></span>

### <a name="xml-configuration-provider"></a><span data-ttu-id="942d5-969">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-969">XML Configuration Provider</span></span>

<span data-ttu-id="942d5-970"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> 會在執行階段從 XML 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-970">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="942d5-971">若要啟用 XML 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="942d5-971">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="942d5-972">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="942d5-972">Overloads permit specifying:</span></span>

* <span data-ttu-id="942d5-973">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="942d5-973">Whether the file is optional.</span></span>
* <span data-ttu-id="942d5-974">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-974">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="942d5-975"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="942d5-975">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="942d5-976">建立設定機碼值組時，會忽略設定檔案的根節點。</span><span class="sxs-lookup"><span data-stu-id="942d5-976">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="942d5-977">請勿在檔案中指定文件類型定義 (DTD) 或命名空間。</span><span class="sxs-lookup"><span data-stu-id="942d5-977">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="942d5-978">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="942d5-978">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="942d5-979">XML 設定檔可以針對重複的區段使用相異元素名稱：</span><span class="sxs-lookup"><span data-stu-id="942d5-979">XML configuration files can use distinct element names for repeating sections:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section0>
    <key0>value</key0>
    <key1>value</key1>
  </section0>
  <section1>
    <key0>value</key0>
    <key1>value</key1>
  </section1>
</configuration>
```

<span data-ttu-id="942d5-980">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="942d5-980">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="942d5-981">section0:key0</span><span class="sxs-lookup"><span data-stu-id="942d5-981">section0:key0</span></span>
* <span data-ttu-id="942d5-982">section0:key1</span><span class="sxs-lookup"><span data-stu-id="942d5-982">section0:key1</span></span>
* <span data-ttu-id="942d5-983">section1:key0</span><span class="sxs-lookup"><span data-stu-id="942d5-983">section1:key0</span></span>
* <span data-ttu-id="942d5-984">section1:key1</span><span class="sxs-lookup"><span data-stu-id="942d5-984">section1:key1</span></span>

<span data-ttu-id="942d5-985">若 `name` 屬性是用來區別元素，則可以重複那些使用相同元素名稱的元素：</span><span class="sxs-lookup"><span data-stu-id="942d5-985">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section name="section0">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
  <section name="section1">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
</configuration>
```

<span data-ttu-id="942d5-986">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="942d5-986">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="942d5-987">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="942d5-987">section:section0:key:key0</span></span>
* <span data-ttu-id="942d5-988">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="942d5-988">section:section0:key:key1</span></span>
* <span data-ttu-id="942d5-989">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="942d5-989">section:section1:key:key0</span></span>
* <span data-ttu-id="942d5-990">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="942d5-990">section:section1:key:key1</span></span>

<span data-ttu-id="942d5-991">屬性可用來提供值：</span><span class="sxs-lookup"><span data-stu-id="942d5-991">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="942d5-992">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="942d5-992">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="942d5-993">key:attribute</span><span class="sxs-lookup"><span data-stu-id="942d5-993">key:attribute</span></span>
* <span data-ttu-id="942d5-994">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="942d5-994">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="942d5-995">每個檔案機碼設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-995">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="942d5-996"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> 使用目錄的檔案做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="942d5-996">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="942d5-997">機碼是檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="942d5-997">The key is the file name.</span></span> <span data-ttu-id="942d5-998">值包含檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="942d5-998">The value contains the file's contents.</span></span> <span data-ttu-id="942d5-999">每個檔案機碼設定提供者是在 Docker 主機案例中使用。</span><span class="sxs-lookup"><span data-stu-id="942d5-999">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="942d5-1000">若要啟用每個檔案機碼設定，請在 <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="942d5-1000">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="942d5-1001">檔案的 `directoryPath` 必須是絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="942d5-1001">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="942d5-1002">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="942d5-1002">Overloads permit specifying:</span></span>

* <span data-ttu-id="942d5-1003">設定來源的 `Action<KeyPerFileConfigurationSource>` 委派。</span><span class="sxs-lookup"><span data-stu-id="942d5-1003">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="942d5-1004">目錄是否為選擇性與目錄的路徑。</span><span class="sxs-lookup"><span data-stu-id="942d5-1004">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="942d5-1005">雙底線 (`__`) 是做為檔案名稱中的設定金鑰分隔符號使用。</span><span class="sxs-lookup"><span data-stu-id="942d5-1005">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="942d5-1006">例如，檔案名稱 `Logging__LogLevel__System` 會產生設定金鑰 `Logging:LogLevel:System`。</span><span class="sxs-lookup"><span data-stu-id="942d5-1006">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="942d5-1007">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="942d5-1007">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="942d5-1008">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-1008">Memory Configuration Provider</span></span>

<span data-ttu-id="942d5-1009"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> 使用記憶體內集合做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="942d5-1009">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="942d5-1010">若要啟用記憶體內集合設定，請在 <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="942d5-1010">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="942d5-1011">設定提供者可以使用 `IEnumerable<KeyValuePair<String,String>>` 來初始化。</span><span class="sxs-lookup"><span data-stu-id="942d5-1011">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="942d5-1012">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="942d5-1012">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="942d5-1013">下列範例會建立組態字典：</span><span class="sxs-lookup"><span data-stu-id="942d5-1013">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="942d5-1014">字典會與 `AddInMemoryCollection` 的呼叫搭配使用以提供組態：</span><span class="sxs-lookup"><span data-stu-id="942d5-1014">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="942d5-1015">GetValue</span><span class="sxs-lookup"><span data-stu-id="942d5-1015">GetValue</span></span>

<span data-ttu-id="942d5-1016">[ConfigurationBinder\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*)會使用指定的索引鍵從設定中解壓縮單一值，並將它轉換成指定的 noncollection 類型。</span><span class="sxs-lookup"><span data-stu-id="942d5-1016">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="942d5-1017">多載會接受預設值。</span><span class="sxs-lookup"><span data-stu-id="942d5-1017">An overload accepts a default value.</span></span>

<span data-ttu-id="942d5-1018">下列範例︰</span><span class="sxs-lookup"><span data-stu-id="942d5-1018">The following example:</span></span>

* <span data-ttu-id="942d5-1019">從具有機碼 `NumberKey` 的組態擷取字串值。</span><span class="sxs-lookup"><span data-stu-id="942d5-1019">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="942d5-1020">若在組態機碼中找不到 `NumberKey`，則會使用預設值 `99`。</span><span class="sxs-lookup"><span data-stu-id="942d5-1020">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="942d5-1021">鍵入值為 `int`。</span><span class="sxs-lookup"><span data-stu-id="942d5-1021">Types the value as an `int`.</span></span>
* <span data-ttu-id="942d5-1022">在 `NumberConfig` 屬性中儲存值供頁面使用。</span><span class="sxs-lookup"><span data-stu-id="942d5-1022">Stores the value in the `NumberConfig` property for use by the page.</span></span>

```csharp
public class IndexModel : PageModel
{
    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    public int NumberConfig { get; private set; }

    public void OnGet()
    {
        NumberConfig = _config.GetValue<int>("NumberKey", 99);
    }
}
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="942d5-1023">GetSection、GetChildren 與 Exists</span><span class="sxs-lookup"><span data-stu-id="942d5-1023">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="942d5-1024">針對下面的範例，請考慮下列 JSON 檔案。</span><span class="sxs-lookup"><span data-stu-id="942d5-1024">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="942d5-1025">在兩個區段中找到四個機碼，其中一個包括子區段組：</span><span class="sxs-lookup"><span data-stu-id="942d5-1025">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  },
  "section2": {
    "subsection0" : {
      "key0": "value",
      "key1": "value"
    },
    "subsection1" : {
      "key0": "value",
      "key1": "value"
    }
  }
}
```

<span data-ttu-id="942d5-1026">當檔案被讀入到設定之後，會建立下列唯一階層式機碼以存放設定值：</span><span class="sxs-lookup"><span data-stu-id="942d5-1026">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="942d5-1027">section0:key0</span><span class="sxs-lookup"><span data-stu-id="942d5-1027">section0:key0</span></span>
* <span data-ttu-id="942d5-1028">section0:key1</span><span class="sxs-lookup"><span data-stu-id="942d5-1028">section0:key1</span></span>
* <span data-ttu-id="942d5-1029">section1:key0</span><span class="sxs-lookup"><span data-stu-id="942d5-1029">section1:key0</span></span>
* <span data-ttu-id="942d5-1030">section1:key1</span><span class="sxs-lookup"><span data-stu-id="942d5-1030">section1:key1</span></span>
* <span data-ttu-id="942d5-1031">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="942d5-1031">section2:subsection0:key0</span></span>
* <span data-ttu-id="942d5-1032">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="942d5-1032">section2:subsection0:key1</span></span>
* <span data-ttu-id="942d5-1033">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="942d5-1033">section2:subsection1:key0</span></span>
* <span data-ttu-id="942d5-1034">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="942d5-1034">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="942d5-1035">GetSection</span><span class="sxs-lookup"><span data-stu-id="942d5-1035">GetSection</span></span>

<span data-ttu-id="942d5-1036">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) 會擷取具有所指定子區段機碼的設定子區段。</span><span class="sxs-lookup"><span data-stu-id="942d5-1036">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="942d5-1037">若要傳回 <xref:Microsoft.Extensions.Configuration.IConfigurationSection> 中只包含一個機碼值組的 `section1`，請呼叫 `GetSection` 並提供區段名稱：</span><span class="sxs-lookup"><span data-stu-id="942d5-1037">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="942d5-1038">`configSection` 沒有值，只有索引鍵和路徑。</span><span class="sxs-lookup"><span data-stu-id="942d5-1038">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="942d5-1039">同樣地，若要取得 `section2:subsection0` 中之機碼的值，請呼叫 `GetSection` 並提供區段路徑：</span><span class="sxs-lookup"><span data-stu-id="942d5-1039">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="942d5-1040">`GetSection` 絕不會傳回 `null`。</span><span class="sxs-lookup"><span data-stu-id="942d5-1040">`GetSection` never returns `null`.</span></span> <span data-ttu-id="942d5-1041">若找不到相符的區段，會傳回空的 `IConfigurationSection`。</span><span class="sxs-lookup"><span data-stu-id="942d5-1041">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="942d5-1042">當 `GetSection` 傳回相符區段時，未填入 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value>。</span><span class="sxs-lookup"><span data-stu-id="942d5-1042">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="942d5-1043">當區段存在時，會傳回 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> 與 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path>。</span><span class="sxs-lookup"><span data-stu-id="942d5-1043">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="942d5-1044">GetChildren</span><span class="sxs-lookup"><span data-stu-id="942d5-1044">GetChildren</span></span>

<span data-ttu-id="942d5-1045">對 [ 上之 ](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*)IConfiguration.GetChildren`section2` 的呼叫會取得包括下列項目的 `IEnumerable<IConfigurationSection>`：</span><span class="sxs-lookup"><span data-stu-id="942d5-1045">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="942d5-1046">Exists</span><span class="sxs-lookup"><span data-stu-id="942d5-1046">Exists</span></span>

<span data-ttu-id="942d5-1047">使用 [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) 來判斷設定區段是否存在：</span><span class="sxs-lookup"><span data-stu-id="942d5-1047">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="942d5-1048">以範例資料為例， `sectionExists` 是 `false`，因未設定資料中沒有 `section2:subsection2` 區段。</span><span class="sxs-lookup"><span data-stu-id="942d5-1048">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="942d5-1049">繫結到類別</span><span class="sxs-lookup"><span data-stu-id="942d5-1049">Bind to a class</span></span>

<span data-ttu-id="942d5-1050">設定可以繫結到類別，以使用*選項模式*代表相關設定的群組。</span><span class="sxs-lookup"><span data-stu-id="942d5-1050">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="942d5-1051">如需詳細資訊，請參閱 <xref:fundamentals/configuration/options>。</span><span class="sxs-lookup"><span data-stu-id="942d5-1051">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="942d5-1052">設定值是以字串傳回，但是呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 會啟用 [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) 物件的建構。</span><span class="sxs-lookup"><span data-stu-id="942d5-1052">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="942d5-1053">系結器會將值系結至提供之類型的所有公用讀取/寫入屬性。</span><span class="sxs-lookup"><span data-stu-id="942d5-1053">The binder binds values to all of the public read/write properties of the type provided.</span></span> <span data-ttu-id="942d5-1054">欄位**未**系結。</span><span class="sxs-lookup"><span data-stu-id="942d5-1054">Fields are **not** bound.</span></span>

<span data-ttu-id="942d5-1055">範例應用程式包含 `Starship` 模型 (*Models/Starship.cs*)：</span><span class="sxs-lookup"><span data-stu-id="942d5-1055">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="942d5-1056">當範例應用程式使用 JSON 設定提供者來載入設定時，`starship`starship.json*檔案的* 區段會建立設定：</span><span class="sxs-lookup"><span data-stu-id="942d5-1056">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

<span data-ttu-id="942d5-1057">會建立下列設定機碼值組：</span><span class="sxs-lookup"><span data-stu-id="942d5-1057">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="942d5-1058">Key</span><span class="sxs-lookup"><span data-stu-id="942d5-1058">Key</span></span>                   | <span data-ttu-id="942d5-1059">值</span><span class="sxs-lookup"><span data-stu-id="942d5-1059">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="942d5-1060">starship:name</span><span class="sxs-lookup"><span data-stu-id="942d5-1060">starship:name</span></span>         | <span data-ttu-id="942d5-1061">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="942d5-1061">USS Enterprise</span></span>                                    |
| <span data-ttu-id="942d5-1062">starship:registry</span><span class="sxs-lookup"><span data-stu-id="942d5-1062">starship:registry</span></span>     | <span data-ttu-id="942d5-1063">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="942d5-1063">NCC-1701</span></span>                                          |
| <span data-ttu-id="942d5-1064">starship:class</span><span class="sxs-lookup"><span data-stu-id="942d5-1064">starship:class</span></span>        | <span data-ttu-id="942d5-1065">Constitution</span><span class="sxs-lookup"><span data-stu-id="942d5-1065">Constitution</span></span>                                      |
| <span data-ttu-id="942d5-1066">starship:length</span><span class="sxs-lookup"><span data-stu-id="942d5-1066">starship:length</span></span>       | <span data-ttu-id="942d5-1067">304.8</span><span class="sxs-lookup"><span data-stu-id="942d5-1067">304.8</span></span>                                             |
| <span data-ttu-id="942d5-1068">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="942d5-1068">starship:commissioned</span></span> | <span data-ttu-id="942d5-1069">False</span><span class="sxs-lookup"><span data-stu-id="942d5-1069">False</span></span>                                             |
| <span data-ttu-id="942d5-1070">trademark</span><span class="sxs-lookup"><span data-stu-id="942d5-1070">trademark</span></span>             | <span data-ttu-id="942d5-1071">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="942d5-1071">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="942d5-1072">範例應用程式使用 `GetSection` 機碼呼叫 `starship`。</span><span class="sxs-lookup"><span data-stu-id="942d5-1072">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="942d5-1073">`starship` 機碼值組會被隔離。</span><span class="sxs-lookup"><span data-stu-id="942d5-1073">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="942d5-1074">會在子區段上呼叫 `Bind` 方法，並傳入 `Starship` 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="942d5-1074">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="942d5-1075">繫結執行個體值之後，執行個體會被指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="942d5-1075">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="942d5-1076">繫結至物件圖形</span><span class="sxs-lookup"><span data-stu-id="942d5-1076">Bind to an object graph</span></span>

<span data-ttu-id="942d5-1077"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 可以繫結整個 POCO 物件圖形。</span><span class="sxs-lookup"><span data-stu-id="942d5-1077"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="942d5-1078">如同系結簡單的物件，只會系結公用讀取/寫入屬性。</span><span class="sxs-lookup"><span data-stu-id="942d5-1078">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="942d5-1079">範例包含 `TvShow` 模型，其物件圖形包括 `Metadata` 與 `Actors` 類別 (*Models/TvShow.cs*)：</span><span class="sxs-lookup"><span data-stu-id="942d5-1079">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="942d5-1080">範例應用程式有 *tvshow.xml* 檔案，其中包含設定資料：</span><span class="sxs-lookup"><span data-stu-id="942d5-1080">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="942d5-1081">設定會被繫結到整個 `TvShow` 物件 (使用 `Bind` 方法)。</span><span class="sxs-lookup"><span data-stu-id="942d5-1081">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="942d5-1082">已繫結的執行個體會被指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="942d5-1082">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="942d5-1083">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 會繫結並傳回所指定型別。</span><span class="sxs-lookup"><span data-stu-id="942d5-1083">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="942d5-1084">`Get<T>` 比使用 `Bind` 更方便。</span><span class="sxs-lookup"><span data-stu-id="942d5-1084">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="942d5-1085">下列程式碼顯示如何根據上面的範例使用 `Get<T>`，這可讓您直接將已繫結的執行個體指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="942d5-1085">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="942d5-1086">將陣列繫結到類別</span><span class="sxs-lookup"><span data-stu-id="942d5-1086">Bind an array to a class</span></span>

<span data-ttu-id="942d5-1087">範例應用程式示範此節中解釋的概念。</span><span class="sxs-lookup"><span data-stu-id="942d5-1087">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="942d5-1088"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="942d5-1088">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="942d5-1089">公開數值索引鍵區段（`:0:`、`:1:`&hellip; `:{n}:`）的任何陣列格式，都能夠系結至 POCO 類別陣列。</span><span class="sxs-lookup"><span data-stu-id="942d5-1089">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="942d5-1090">繫結是由慣例提供。</span><span class="sxs-lookup"><span data-stu-id="942d5-1090">Binding is provided by convention.</span></span> <span data-ttu-id="942d5-1091">自訂設定提供者不需要實作陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="942d5-1091">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="942d5-1092">**記憶體內陣列處理**</span><span class="sxs-lookup"><span data-stu-id="942d5-1092">**In-memory array processing**</span></span>

<span data-ttu-id="942d5-1093">考慮下表中顯示的設定機碼與值。</span><span class="sxs-lookup"><span data-stu-id="942d5-1093">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="942d5-1094">Key</span><span class="sxs-lookup"><span data-stu-id="942d5-1094">Key</span></span>             | <span data-ttu-id="942d5-1095">值</span><span class="sxs-lookup"><span data-stu-id="942d5-1095">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="942d5-1096">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="942d5-1096">array:entries:0</span></span> | <span data-ttu-id="942d5-1097">value0</span><span class="sxs-lookup"><span data-stu-id="942d5-1097">value0</span></span> |
| <span data-ttu-id="942d5-1098">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="942d5-1098">array:entries:1</span></span> | <span data-ttu-id="942d5-1099">value1</span><span class="sxs-lookup"><span data-stu-id="942d5-1099">value1</span></span> |
| <span data-ttu-id="942d5-1100">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="942d5-1100">array:entries:2</span></span> | <span data-ttu-id="942d5-1101">value2</span><span class="sxs-lookup"><span data-stu-id="942d5-1101">value2</span></span> |
| <span data-ttu-id="942d5-1102">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="942d5-1102">array:entries:4</span></span> | <span data-ttu-id="942d5-1103">value4</span><span class="sxs-lookup"><span data-stu-id="942d5-1103">value4</span></span> |
| <span data-ttu-id="942d5-1104">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="942d5-1104">array:entries:5</span></span> | <span data-ttu-id="942d5-1105">value5</span><span class="sxs-lookup"><span data-stu-id="942d5-1105">value5</span></span> |

<span data-ttu-id="942d5-1106">這些機碼與值是使用「記憶體設定提供者」在範例應用程式中載入：</span><span class="sxs-lookup"><span data-stu-id="942d5-1106">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

<span data-ttu-id="942d5-1107">陣列會跳過索引 &num;3 的值。</span><span class="sxs-lookup"><span data-stu-id="942d5-1107">The array skips a value for index &num;3.</span></span> <span data-ttu-id="942d5-1108">設定繫結程式無法繫結 Null 值或在已繫結的物件中建立 Null 項目，這在示範繫結此陣列的結果到某物件的時候已經很清楚。</span><span class="sxs-lookup"><span data-stu-id="942d5-1108">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="942d5-1109">在範例應用程式中，POCO 類別可用來存放繫結設定資料：</span><span class="sxs-lookup"><span data-stu-id="942d5-1109">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="942d5-1110">設定資料已繫結到物件：</span><span class="sxs-lookup"><span data-stu-id="942d5-1110">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="942d5-1111">也可以使用 [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 語法，其會產生更精簡的程式碼：</span><span class="sxs-lookup"><span data-stu-id="942d5-1111">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="942d5-1112">繫結物件 (`ArrayExample`的執行個體) 會從設定接收陣列資料。</span><span class="sxs-lookup"><span data-stu-id="942d5-1112">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="942d5-1113">`ArrayExample.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="942d5-1113">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="942d5-1114">`ArrayExample.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="942d5-1114">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="942d5-1115">0</span><span class="sxs-lookup"><span data-stu-id="942d5-1115">0</span></span>                            | <span data-ttu-id="942d5-1116">value0</span><span class="sxs-lookup"><span data-stu-id="942d5-1116">value0</span></span>                       |
| <span data-ttu-id="942d5-1117">1</span><span class="sxs-lookup"><span data-stu-id="942d5-1117">1</span></span>                            | <span data-ttu-id="942d5-1118">value1</span><span class="sxs-lookup"><span data-stu-id="942d5-1118">value1</span></span>                       |
| <span data-ttu-id="942d5-1119">2</span><span class="sxs-lookup"><span data-stu-id="942d5-1119">2</span></span>                            | <span data-ttu-id="942d5-1120">value2</span><span class="sxs-lookup"><span data-stu-id="942d5-1120">value2</span></span>                       |
| <span data-ttu-id="942d5-1121">3</span><span class="sxs-lookup"><span data-stu-id="942d5-1121">3</span></span>                            | <span data-ttu-id="942d5-1122">value4</span><span class="sxs-lookup"><span data-stu-id="942d5-1122">value4</span></span>                       |
| <span data-ttu-id="942d5-1123">4</span><span class="sxs-lookup"><span data-stu-id="942d5-1123">4</span></span>                            | <span data-ttu-id="942d5-1124">value5</span><span class="sxs-lookup"><span data-stu-id="942d5-1124">value5</span></span>                       |

<span data-ttu-id="942d5-1125">繫結物件中的索引 &num;3 存放 `array:4` 設定機碼與其 `value4` 的設定資料。</span><span class="sxs-lookup"><span data-stu-id="942d5-1125">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="942d5-1126">當繫結包含陣列的設定資料時，設定機碼中的陣列索引只會用來列舉設定資料 (當建立物件時)。</span><span class="sxs-lookup"><span data-stu-id="942d5-1126">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="942d5-1127">設定資料中不能保留 Null 值，當設定機碼中的陣列略過一或多個索引時，不會在繫結物件中建立 Null 值項目。</span><span class="sxs-lookup"><span data-stu-id="942d5-1127">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="942d5-1128">由會在設定中產生正確機碼值組的任何設定提供者繫結到 &num; 執行個體之前，可以提供索引 `ArrayExample`3 缺少的設定項目。</span><span class="sxs-lookup"><span data-stu-id="942d5-1128">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="942d5-1129">若範例包含具有缺少之機碼值組的額外 JSON 設定提供者，`ArrayExample.Entries` 會符合完整的設定陣列：</span><span class="sxs-lookup"><span data-stu-id="942d5-1129">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="942d5-1130">*missing_value.json*：</span><span class="sxs-lookup"><span data-stu-id="942d5-1130">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="942d5-1131">在 `ConfigureAppConfiguration` 中：</span><span class="sxs-lookup"><span data-stu-id="942d5-1131">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="942d5-1132">表格中顯示的機碼值組會載入到設定中。</span><span class="sxs-lookup"><span data-stu-id="942d5-1132">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="942d5-1133">Key</span><span class="sxs-lookup"><span data-stu-id="942d5-1133">Key</span></span>             | <span data-ttu-id="942d5-1134">值</span><span class="sxs-lookup"><span data-stu-id="942d5-1134">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="942d5-1135">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="942d5-1135">array:entries:3</span></span> | <span data-ttu-id="942d5-1136">value3</span><span class="sxs-lookup"><span data-stu-id="942d5-1136">value3</span></span> |

<span data-ttu-id="942d5-1137">在「JSON 設定提供者」包含索引 `ArrayExample`3 的項目之後，若繫結 &num; 類別執行個體，`ArrayExample.Entries` 陣列會包括這些值：</span><span class="sxs-lookup"><span data-stu-id="942d5-1137">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="942d5-1138">`ArrayExample.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="942d5-1138">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="942d5-1139">`ArrayExample.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="942d5-1139">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="942d5-1140">0</span><span class="sxs-lookup"><span data-stu-id="942d5-1140">0</span></span>                            | <span data-ttu-id="942d5-1141">value0</span><span class="sxs-lookup"><span data-stu-id="942d5-1141">value0</span></span>                       |
| <span data-ttu-id="942d5-1142">1</span><span class="sxs-lookup"><span data-stu-id="942d5-1142">1</span></span>                            | <span data-ttu-id="942d5-1143">value1</span><span class="sxs-lookup"><span data-stu-id="942d5-1143">value1</span></span>                       |
| <span data-ttu-id="942d5-1144">2</span><span class="sxs-lookup"><span data-stu-id="942d5-1144">2</span></span>                            | <span data-ttu-id="942d5-1145">value2</span><span class="sxs-lookup"><span data-stu-id="942d5-1145">value2</span></span>                       |
| <span data-ttu-id="942d5-1146">3</span><span class="sxs-lookup"><span data-stu-id="942d5-1146">3</span></span>                            | <span data-ttu-id="942d5-1147">value3</span><span class="sxs-lookup"><span data-stu-id="942d5-1147">value3</span></span>                       |
| <span data-ttu-id="942d5-1148">4</span><span class="sxs-lookup"><span data-stu-id="942d5-1148">4</span></span>                            | <span data-ttu-id="942d5-1149">value4</span><span class="sxs-lookup"><span data-stu-id="942d5-1149">value4</span></span>                       |
| <span data-ttu-id="942d5-1150">5</span><span class="sxs-lookup"><span data-stu-id="942d5-1150">5</span></span>                            | <span data-ttu-id="942d5-1151">value5</span><span class="sxs-lookup"><span data-stu-id="942d5-1151">value5</span></span>                       |

<span data-ttu-id="942d5-1152">**JSON 陣列處理**</span><span class="sxs-lookup"><span data-stu-id="942d5-1152">**JSON array processing**</span></span>

<span data-ttu-id="942d5-1153">若 JSON 檔案包含陣列，會為具有以零為基礎之區段索引的陣列元素建立設定機碼。</span><span class="sxs-lookup"><span data-stu-id="942d5-1153">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="942d5-1154">在下列設定檔中，`subsection` 是陣列：</span><span class="sxs-lookup"><span data-stu-id="942d5-1154">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="942d5-1155">「JSON 設定提供者」會將設定資料讀入到下列機碼值組：</span><span class="sxs-lookup"><span data-stu-id="942d5-1155">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="942d5-1156">Key</span><span class="sxs-lookup"><span data-stu-id="942d5-1156">Key</span></span>                     | <span data-ttu-id="942d5-1157">值</span><span class="sxs-lookup"><span data-stu-id="942d5-1157">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="942d5-1158">json_array:key</span><span class="sxs-lookup"><span data-stu-id="942d5-1158">json_array:key</span></span>          | <span data-ttu-id="942d5-1159">valueA</span><span class="sxs-lookup"><span data-stu-id="942d5-1159">valueA</span></span> |
| <span data-ttu-id="942d5-1160">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="942d5-1160">json_array:subsection:0</span></span> | <span data-ttu-id="942d5-1161">valueB</span><span class="sxs-lookup"><span data-stu-id="942d5-1161">valueB</span></span> |
| <span data-ttu-id="942d5-1162">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="942d5-1162">json_array:subsection:1</span></span> | <span data-ttu-id="942d5-1163">valueC</span><span class="sxs-lookup"><span data-stu-id="942d5-1163">valueC</span></span> |
| <span data-ttu-id="942d5-1164">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="942d5-1164">json_array:subsection:2</span></span> | <span data-ttu-id="942d5-1165">valueD</span><span class="sxs-lookup"><span data-stu-id="942d5-1165">valueD</span></span> |

<span data-ttu-id="942d5-1166">在範例應用程式中，下列 POCO 類別可用來繫結設定機碼值組：</span><span class="sxs-lookup"><span data-stu-id="942d5-1166">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="942d5-1167">在繫結之後，`JsonArrayExample.Key` 會存放值 `valueA`。</span><span class="sxs-lookup"><span data-stu-id="942d5-1167">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="942d5-1168">子區段值會存放在 POCO 陣列屬性 `Subsection` 中。</span><span class="sxs-lookup"><span data-stu-id="942d5-1168">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="942d5-1169">`JsonArrayExample.Subsection` 索引</span><span class="sxs-lookup"><span data-stu-id="942d5-1169">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="942d5-1170">`JsonArrayExample.Subsection` 值</span><span class="sxs-lookup"><span data-stu-id="942d5-1170">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="942d5-1171">0</span><span class="sxs-lookup"><span data-stu-id="942d5-1171">0</span></span>                                   | <span data-ttu-id="942d5-1172">valueB</span><span class="sxs-lookup"><span data-stu-id="942d5-1172">valueB</span></span>                              |
| <span data-ttu-id="942d5-1173">1</span><span class="sxs-lookup"><span data-stu-id="942d5-1173">1</span></span>                                   | <span data-ttu-id="942d5-1174">valueC</span><span class="sxs-lookup"><span data-stu-id="942d5-1174">valueC</span></span>                              |
| <span data-ttu-id="942d5-1175">2</span><span class="sxs-lookup"><span data-stu-id="942d5-1175">2</span></span>                                   | <span data-ttu-id="942d5-1176">valueD</span><span class="sxs-lookup"><span data-stu-id="942d5-1176">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="942d5-1177">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="942d5-1177">Custom configuration provider</span></span>

<span data-ttu-id="942d5-1178">範例應用程式示範如何建立會使用 [Entity Framework (EF)](/ef/core/) 從資料庫讀取設定機碼值組的基本設定提供者。</span><span class="sxs-lookup"><span data-stu-id="942d5-1178">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="942d5-1179">提供者具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="942d5-1179">The provider has the following characteristics:</span></span>

* <span data-ttu-id="942d5-1180">EF 記憶體內資料庫會用於示範用途。</span><span class="sxs-lookup"><span data-stu-id="942d5-1180">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="942d5-1181">若要使用需要連接字串的資料庫，請實作第二個 `ConfigurationBuilder` 以從另一個設定提供者提供連接字串。</span><span class="sxs-lookup"><span data-stu-id="942d5-1181">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="942d5-1182">啟動時，該提供者會將資料庫資料表讀入到設定中。</span><span class="sxs-lookup"><span data-stu-id="942d5-1182">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="942d5-1183">該提供者不會以個別機碼為基礎查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="942d5-1183">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="942d5-1184">未實作變更時重新載入，因此在應用程式啟動後更新資料庫對應用程式設定沒有影響。</span><span class="sxs-lookup"><span data-stu-id="942d5-1184">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="942d5-1185">定義 `EFConfigurationValue` 實體來在資料庫中存放設定值。</span><span class="sxs-lookup"><span data-stu-id="942d5-1185">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="942d5-1186">*Models/EFConfigurationValue.cs*：</span><span class="sxs-lookup"><span data-stu-id="942d5-1186">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="942d5-1187">新增 `EFConfigurationContext` 以存放及存取已設定的值。</span><span class="sxs-lookup"><span data-stu-id="942d5-1187">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="942d5-1188">*EFConfigurationProvider/EFConfigurationContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="942d5-1188">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="942d5-1189">建立會實作 <xref:Microsoft.Extensions.Configuration.IConfigurationSource> 的類別。</span><span class="sxs-lookup"><span data-stu-id="942d5-1189">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="942d5-1190">*EFConfigurationProvider/EFConfigurationSource.cs*：</span><span class="sxs-lookup"><span data-stu-id="942d5-1190">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="942d5-1191">透過繼承自 <xref:Microsoft.Extensions.Configuration.ConfigurationProvider> 來建立自訂設定提供者。</span><span class="sxs-lookup"><span data-stu-id="942d5-1191">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="942d5-1192">若資料庫是空的，設定提供者會初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="942d5-1192">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="942d5-1193">*EFConfigurationProvider/EFConfigurationProvider.cs*：</span><span class="sxs-lookup"><span data-stu-id="942d5-1193">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="942d5-1194">`AddEFConfiguration` 擴充方法允許新增設定來源到 `ConfigurationBuilder`。</span><span class="sxs-lookup"><span data-stu-id="942d5-1194">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="942d5-1195">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="942d5-1195">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="942d5-1196">下列程式碼顯示如何在 `EFConfigurationProvider`Program.cs*中使用自訂*：</span><span class="sxs-lookup"><span data-stu-id="942d5-1196">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="942d5-1197">在啟動期間存取組態</span><span class="sxs-lookup"><span data-stu-id="942d5-1197">Access configuration during startup</span></span>

<span data-ttu-id="942d5-1198">將 `IConfiguration` 插入到 `Startup` 建構函式，以存取 `Startup.ConfigureServices` 中的設定值。</span><span class="sxs-lookup"><span data-stu-id="942d5-1198">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="942d5-1199">若要存取 `Startup.Configure` 中的設定，請直接將 `IConfiguration` 插入到方法或從建構函式使用執行個體：</span><span class="sxs-lookup"><span data-stu-id="942d5-1199">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

```csharp
public class Startup
{
    private readonly IConfiguration _config;

    public Startup(IConfiguration config)
    {
        _config = config;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        var value = _config["key"];
    }

    public void Configure(IApplicationBuilder app, IConfiguration config)
    {
        var value = config["key"];
    }
}
```

<span data-ttu-id="942d5-1200">如需使用啟動方便方法來存取設定的範例，請參閱[應用程式啟動：方便方法](xref:fundamentals/startup#convenience-methods)。</span><span class="sxs-lookup"><span data-stu-id="942d5-1200">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="942d5-1201">存取 Razor Pages 頁面或 MVC 檢視中的設定</span><span class="sxs-lookup"><span data-stu-id="942d5-1201">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="942d5-1202">若要在 [Razor Pages 頁面] 頁面或 MVC 檢視中存取組態設定，請針對 [Microsoft.Extensions.Configuration 命名空間](xref:mvc/views/razor#using)[using 指示詞](/dotnet/csharp/language-reference/keywords/using-directive) ([C# 參考：using 指示詞](xref:Microsoft.Extensions.Configuration))，並將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 插入到頁面或檢視中。</span><span class="sxs-lookup"><span data-stu-id="942d5-1202">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="942d5-1203">在 [Razor 頁面] 頁面中：</span><span class="sxs-lookup"><span data-stu-id="942d5-1203">In a Razor Pages page:</span></span>

```cshtml
@page
@model IndexModel
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

<span data-ttu-id="942d5-1204">在 MVC 檢視中：</span><span class="sxs-lookup"><span data-stu-id="942d5-1204">In an MVC view:</span></span>

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="942d5-1205">從外部組件新增設定</span><span class="sxs-lookup"><span data-stu-id="942d5-1205">Add configuration from an external assembly</span></span>

<span data-ttu-id="942d5-1206"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。</span><span class="sxs-lookup"><span data-stu-id="942d5-1206">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="942d5-1207">如需詳細資訊，請參閱 <xref:fundamentals/configuration/platform-specific-configuration>。</span><span class="sxs-lookup"><span data-stu-id="942d5-1207">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="942d5-1208">其他資源</span><span class="sxs-lookup"><span data-stu-id="942d5-1208">Additional resources</span></span>

* <xref:fundamentals/configuration/options>

::: moniker-end
