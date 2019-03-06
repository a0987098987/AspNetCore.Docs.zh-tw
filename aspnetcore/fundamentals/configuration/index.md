---
title: ASP.NET Core 的設定
author: guardrex
description: 了解如何使用組態 API 設定 ASP.NET Core 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 03/04/2019
uid: fundamentals/configuration/index
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="02249-103">ASP.NET Core 的設定</span><span class="sxs-lookup"><span data-stu-id="02249-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="02249-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="02249-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="02249-105">ASP.NET Core 中的應用程式設定是以由*設定提供者*所建立的機碼值組為基礎。</span><span class="sxs-lookup"><span data-stu-id="02249-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="02249-106">設定提供者會從各種設定來源將設定資料讀取到機碼值組中：</span><span class="sxs-lookup"><span data-stu-id="02249-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="02249-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="02249-107">Azure Key Vault</span></span>
* <span data-ttu-id="02249-108">命令列引數</span><span class="sxs-lookup"><span data-stu-id="02249-108">Command-line arguments</span></span>
* <span data-ttu-id="02249-109">自訂提供者 (已安裝或已建立)</span><span class="sxs-lookup"><span data-stu-id="02249-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="02249-110">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="02249-110">Directory files</span></span>
* <span data-ttu-id="02249-111">環境變數</span><span class="sxs-lookup"><span data-stu-id="02249-111">Environment variables</span></span>
* <span data-ttu-id="02249-112">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="02249-112">In-memory .NET objects</span></span>
* <span data-ttu-id="02249-113">設定檔</span><span class="sxs-lookup"><span data-stu-id="02249-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="02249-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="02249-114">Azure Key Vault</span></span>
* <span data-ttu-id="02249-115">命令列引數</span><span class="sxs-lookup"><span data-stu-id="02249-115">Command-line arguments</span></span>
* <span data-ttu-id="02249-116">自訂提供者 (已安裝或已建立)</span><span class="sxs-lookup"><span data-stu-id="02249-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="02249-117">環境變數</span><span class="sxs-lookup"><span data-stu-id="02249-117">Environment variables</span></span>
* <span data-ttu-id="02249-118">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="02249-118">In-memory .NET objects</span></span>
* <span data-ttu-id="02249-119">設定檔</span><span class="sxs-lookup"><span data-stu-id="02249-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="02249-120">命令列引數</span><span class="sxs-lookup"><span data-stu-id="02249-120">Command-line arguments</span></span>
* <span data-ttu-id="02249-121">自訂提供者 (已安裝或已建立)</span><span class="sxs-lookup"><span data-stu-id="02249-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="02249-122">環境變數</span><span class="sxs-lookup"><span data-stu-id="02249-122">Environment variables</span></span>
* <span data-ttu-id="02249-123">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="02249-123">In-memory .NET objects</span></span>
* <span data-ttu-id="02249-124">設定檔</span><span class="sxs-lookup"><span data-stu-id="02249-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="02249-125">*選項模式*是此主題中所述之設定概念的延伸。</span><span class="sxs-lookup"><span data-stu-id="02249-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="02249-126">選項使用類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="02249-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="02249-127">如需使用選項模式的詳細資訊，請參閱 <xref:fundamentals/configuration/options>。</span><span class="sxs-lookup"><span data-stu-id="02249-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="02249-128">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="02249-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="02249-129">這些套件均包含在 [Microsoft.AspNetCore.App j5/ 中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="02249-129">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="02249-130">這三個套件都包含在 [Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)中。</span><span class="sxs-lookup"><span data-stu-id="02249-130">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="02249-131">主機與應用程式設定的比較</span><span class="sxs-lookup"><span data-stu-id="02249-131">Host vs. app configuration</span></span>

<span data-ttu-id="02249-132">設定及啟動應用程式之前，會先設定及啟動「主機」。</span><span class="sxs-lookup"><span data-stu-id="02249-132">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="02249-133">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="02249-133">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="02249-134">應用程式與主機都是使用此主題中所述的設定提供者來設定的。</span><span class="sxs-lookup"><span data-stu-id="02249-134">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="02249-135">主機設定機碼值組會成為應用程式全域設定的一部分。</span><span class="sxs-lookup"><span data-stu-id="02249-135">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="02249-136">如需建置主機時如何使用組態提供者，以及組態來源如何影響主機組態的詳細資訊，請參閱[主機](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="02249-136">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see [The host](xref:fundamentals/index#host).</span></span>

## <a name="default-configuration"></a><span data-ttu-id="02249-137">預設的組態</span><span class="sxs-lookup"><span data-stu-id="02249-137">Default configuration</span></span>

<span data-ttu-id="02249-138">以 ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) 範本為基礎的 Web 應用程式，會在建置主機時呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>。</span><span class="sxs-lookup"><span data-stu-id="02249-138">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="02249-139">`CreateDefaultBuilder` 會提供應用程式的預設組態。</span><span class="sxs-lookup"><span data-stu-id="02249-139">`CreateDefaultBuilder` provides default configuration for the app.</span></span>

* <span data-ttu-id="02249-140">主機組態的提供來源：</span><span class="sxs-lookup"><span data-stu-id="02249-140">Host configuration is provided from:</span></span>
  * <span data-ttu-id="02249-141">使用[環境變數組態提供者](#environment-variables-configuration-provider)且以 `ASPNETCORE_` 為前置詞 (例如 `ASPNETCORE_ENVIRONMENT`) 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="02249-141">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="02249-142">使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="02249-142">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="02249-143">應用程式組態的提供來源 (根據以下順序)：</span><span class="sxs-lookup"><span data-stu-id="02249-143">App configuration is provided from (in the following order):</span></span>
  * <span data-ttu-id="02249-144">使用[檔案組態提供者](#file-configuration-provider)的 *appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="02249-144">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="02249-145">使用[檔案組態提供者](#file-configuration-provider)的 *appsettings.{Environment}.json*。</span><span class="sxs-lookup"><span data-stu-id="02249-145">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="02249-146">應用程式在使用項目組件之 `Development` 環境中執行時的[秘密管理員](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="02249-146">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="02249-147">使用[環境變數組態提供者](#environment-variables-configuration-provider)的環境變數。</span><span class="sxs-lookup"><span data-stu-id="02249-147">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="02249-148">使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="02249-148">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="02249-149">此主題稍後將說明組態提供者。</span><span class="sxs-lookup"><span data-stu-id="02249-149">The configuration providers are explained later in this topic.</span></span> <span data-ttu-id="02249-150">如需主機和 `CreateDefaultBuilder` 的詳細資訊，請參閱 <xref:fundamentals/host/web-host#set-up-a-host>。</span><span class="sxs-lookup"><span data-stu-id="02249-150">For more information on the host and `CreateDefaultBuilder`, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="security"></a><span data-ttu-id="02249-151">安全性</span><span class="sxs-lookup"><span data-stu-id="02249-151">Security</span></span>

<span data-ttu-id="02249-152">採用下列最佳做法：</span><span class="sxs-lookup"><span data-stu-id="02249-152">Adopt the following best practices:</span></span>

* <span data-ttu-id="02249-153">永遠不要將密碼或其他敏感性資料儲存在設定提供者程式碼或純文字設定檔中。</span><span class="sxs-lookup"><span data-stu-id="02249-153">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="02249-154">不要在開發或測試環境中使用生產環境祕密。</span><span class="sxs-lookup"><span data-stu-id="02249-154">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="02249-155">請在專案外部指定祕密，以防止其意外認可至開放原始碼存放庫。</span><span class="sxs-lookup"><span data-stu-id="02249-155">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="02249-156">深入了解[如何使用多個環境](xref:fundamentals/environments)及[使用祕密管理員管理開發中的應用程式祕密安全儲存體](xref:security/app-secrets) (包括有關使用環境變數來存放敏感性資料的建議)。</span><span class="sxs-lookup"><span data-stu-id="02249-156">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="02249-157">「祕密管理員」使用「檔案設定提供者」以 JSON 檔案在本機系統上存放使用者祕密。</span><span class="sxs-lookup"><span data-stu-id="02249-157">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="02249-158">此主題稍後將說明「檔案設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="02249-158">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="02249-159">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 是安全儲存應用程式祕密的一個選項。</span><span class="sxs-lookup"><span data-stu-id="02249-159">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="02249-160">如需詳細資訊，請參閱<xref:security/key-vault-configuration>。</span><span class="sxs-lookup"><span data-stu-id="02249-160">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="02249-161">階層式設定資料</span><span class="sxs-lookup"><span data-stu-id="02249-161">Hierarchical configuration data</span></span>

<span data-ttu-id="02249-162">設定 API 可在設定機碼中使用分隔符號來壓平合併階層式資料，以管理階層式設定資料。</span><span class="sxs-lookup"><span data-stu-id="02249-162">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="02249-163">在下列 JSON 檔案中，兩個區段的結構式階層中有四個機碼存在：</span><span class="sxs-lookup"><span data-stu-id="02249-163">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="02249-164">當該檔案被讀入設定之後，會建立唯一機碼來維護設定來源的原始階層式資料結構。</span><span class="sxs-lookup"><span data-stu-id="02249-164">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="02249-165">系統會使用冒號 (`:`) 將區段與機碼壓平合併以維護原始結構：</span><span class="sxs-lookup"><span data-stu-id="02249-165">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="02249-166">section0:key0</span><span class="sxs-lookup"><span data-stu-id="02249-166">section0:key0</span></span>
* <span data-ttu-id="02249-167">section0:key1</span><span class="sxs-lookup"><span data-stu-id="02249-167">section0:key1</span></span>
* <span data-ttu-id="02249-168">section1:key0</span><span class="sxs-lookup"><span data-stu-id="02249-168">section1:key0</span></span>
* <span data-ttu-id="02249-169">section1:key1</span><span class="sxs-lookup"><span data-stu-id="02249-169">section1:key1</span></span>

<span data-ttu-id="02249-170"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> 與 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> 方法可用來在設定資料中隔離區段與區段的子系。</span><span class="sxs-lookup"><span data-stu-id="02249-170"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="02249-171">[GetSection,、 GetChildren 與 Exists](#getsection-getchildren-and-exists) 中說明這些方法。</span><span class="sxs-lookup"><span data-stu-id="02249-171">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span> <span data-ttu-id="02249-172">`GetSection` 在 [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) 套件中，該套件在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)內。</span><span class="sxs-lookup"><span data-stu-id="02249-172">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="conventions"></a><span data-ttu-id="02249-173">慣例</span><span class="sxs-lookup"><span data-stu-id="02249-173">Conventions</span></span>

<span data-ttu-id="02249-174">在應用程式啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="02249-174">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="02249-175">「檔案設定提供者」可以在應用程式啟動之後且底層設定檔案變更時重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="02249-175">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="02249-176">此主題稍後將說明「檔案設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="02249-176">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="02249-177">您可以在應用程式的[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器中找到 <xref:Microsoft.Extensions.Configuration.IConfiguration>。</span><span class="sxs-lookup"><span data-stu-id="02249-177"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="02249-178"><xref:Microsoft.Extensions.Configuration.IConfiguration> 可插入 Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> 來取得類別的組態：</span><span class="sxs-lookup"><span data-stu-id="02249-178"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

```csharp
// using Microsoft.Extensions.Configuration;

public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }
        
    // The _config local variable is used to obtain configuration 
    // throughout the class.
}
```

<span data-ttu-id="02249-179">設定提供者無法使用 DI，因為當它們由主機設定時，它無法使用。</span><span class="sxs-lookup"><span data-stu-id="02249-179">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="02249-180">設定機碼會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="02249-180">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="02249-181">機碼區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="02249-181">Keys are case-insensitive.</span></span> <span data-ttu-id="02249-182">例如，`ConnectionString` 與 `connectionstring` 會被視為相等的機碼。</span><span class="sxs-lookup"><span data-stu-id="02249-182">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="02249-183">若相同機碼的值是由相同或不同設定提供者設定，在機碼上設定的最後一個值是所使用的值。</span><span class="sxs-lookup"><span data-stu-id="02249-183">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="02249-184">階層式機碼</span><span class="sxs-lookup"><span data-stu-id="02249-184">Hierarchical keys</span></span>
  * <span data-ttu-id="02249-185">在設定 API 內，冒號分隔字元 (`:`) 可在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="02249-185">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="02249-186">在環境變數中，冒號分隔字元可能無法在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="02249-186">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="02249-187">所有平台都支援雙底線 (`__`)，而且它會被轉換為冒號。</span><span class="sxs-lookup"><span data-stu-id="02249-187">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="02249-188">在 Azure Key Vault 中，階層式機碼使用 `--` (兩個破折號) 來做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="02249-188">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="02249-189">您必須提供程式碼，在祕密載入到應用程式的設定時將破折號取代為冒號。</span><span class="sxs-lookup"><span data-stu-id="02249-189">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="02249-190"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="02249-190">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="02249-191">[將陣列繫結到類別](#bind-an-array-to-a-class)一節說明陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="02249-191">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="02249-192">設定值會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="02249-192">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="02249-193">值是字串。</span><span class="sxs-lookup"><span data-stu-id="02249-193">Values are strings.</span></span>
* <span data-ttu-id="02249-194">Null 值無法存放在設定中或繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="02249-194">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="02249-195">提供者</span><span class="sxs-lookup"><span data-stu-id="02249-195">Providers</span></span>

<span data-ttu-id="02249-196">下表顯示可供 ASP.NET Core 應用程式使用的設定提供者。</span><span class="sxs-lookup"><span data-stu-id="02249-196">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="02249-197">提供者</span><span class="sxs-lookup"><span data-stu-id="02249-197">Provider</span></span> | <span data-ttu-id="02249-198">從&hellip;提供設定</span><span class="sxs-lookup"><span data-stu-id="02249-198">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="02249-199">[Azure Key Vault 設定提供者](xref:security/key-vault-configuration) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="02249-199">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="02249-200">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="02249-200">Azure Key Vault</span></span> |
| [<span data-ttu-id="02249-201">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-201">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="02249-202">命令列參數</span><span class="sxs-lookup"><span data-stu-id="02249-202">Command-line parameters</span></span> |
| [<span data-ttu-id="02249-203">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-203">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="02249-204">自訂來源</span><span class="sxs-lookup"><span data-stu-id="02249-204">Custom source</span></span> |
| [<span data-ttu-id="02249-205">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-205">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="02249-206">環境變數</span><span class="sxs-lookup"><span data-stu-id="02249-206">Environment variables</span></span> |
| [<span data-ttu-id="02249-207">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-207">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="02249-208">檔案 (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="02249-208">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="02249-209">每個檔案機碼的設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-209">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="02249-210">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="02249-210">Directory files</span></span> |
| [<span data-ttu-id="02249-211">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-211">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="02249-212">記憶體內集合</span><span class="sxs-lookup"><span data-stu-id="02249-212">In-memory collections</span></span> |
| <span data-ttu-id="02249-213">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="02249-213">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="02249-214">使用者設定檔目錄中的檔案</span><span class="sxs-lookup"><span data-stu-id="02249-214">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="02249-215">提供者</span><span class="sxs-lookup"><span data-stu-id="02249-215">Provider</span></span> | <span data-ttu-id="02249-216">從&hellip;提供設定</span><span class="sxs-lookup"><span data-stu-id="02249-216">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="02249-217">[Azure Key Vault 設定提供者](xref:security/key-vault-configuration) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="02249-217">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="02249-218">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="02249-218">Azure Key Vault</span></span> |
| [<span data-ttu-id="02249-219">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-219">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="02249-220">命令列參數</span><span class="sxs-lookup"><span data-stu-id="02249-220">Command-line parameters</span></span> |
| [<span data-ttu-id="02249-221">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-221">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="02249-222">自訂來源</span><span class="sxs-lookup"><span data-stu-id="02249-222">Custom source</span></span> |
| [<span data-ttu-id="02249-223">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-223">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="02249-224">環境變數</span><span class="sxs-lookup"><span data-stu-id="02249-224">Environment variables</span></span> |
| [<span data-ttu-id="02249-225">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-225">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="02249-226">檔案 (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="02249-226">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="02249-227">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-227">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="02249-228">記憶體內集合</span><span class="sxs-lookup"><span data-stu-id="02249-228">In-memory collections</span></span> |
| <span data-ttu-id="02249-229">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="02249-229">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="02249-230">使用者設定檔目錄中的檔案</span><span class="sxs-lookup"><span data-stu-id="02249-230">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="02249-231">提供者</span><span class="sxs-lookup"><span data-stu-id="02249-231">Provider</span></span> | <span data-ttu-id="02249-232">從&hellip;提供設定</span><span class="sxs-lookup"><span data-stu-id="02249-232">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="02249-233">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-233">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="02249-234">命令列參數</span><span class="sxs-lookup"><span data-stu-id="02249-234">Command-line parameters</span></span> |
| [<span data-ttu-id="02249-235">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-235">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="02249-236">自訂來源</span><span class="sxs-lookup"><span data-stu-id="02249-236">Custom source</span></span> |
| [<span data-ttu-id="02249-237">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-237">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="02249-238">環境變數</span><span class="sxs-lookup"><span data-stu-id="02249-238">Environment variables</span></span> |
| [<span data-ttu-id="02249-239">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-239">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="02249-240">檔案 (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="02249-240">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="02249-241">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-241">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="02249-242">記憶體內集合</span><span class="sxs-lookup"><span data-stu-id="02249-242">In-memory collections</span></span> |
| <span data-ttu-id="02249-243">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="02249-243">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="02249-244">使用者設定檔目錄中的檔案</span><span class="sxs-lookup"><span data-stu-id="02249-244">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="02249-245">在啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="02249-245">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="02249-246">此主題中所述的設定提供者是以字母順序描述，而非以您的程式碼安排它們的順序。</span><span class="sxs-lookup"><span data-stu-id="02249-246">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="02249-247">在您的程式碼中針對底層設定來源的優先順序，為設定提供者排序。</span><span class="sxs-lookup"><span data-stu-id="02249-247">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="02249-248">典型的設定提供者順序是：</span><span class="sxs-lookup"><span data-stu-id="02249-248">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="02249-249">檔案 (*appsettings.json*、*appsettings.{Environment}.json*，其中 `{Environment}` 是應用程式的目前裝載環境)</span><span class="sxs-lookup"><span data-stu-id="02249-249">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="02249-250">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="02249-250">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="02249-251">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (僅限開發環境)</span><span class="sxs-lookup"><span data-stu-id="02249-251">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="02249-252">環境變數</span><span class="sxs-lookup"><span data-stu-id="02249-252">Environment variables</span></span>
1. <span data-ttu-id="02249-253">命令列引數</span><span class="sxs-lookup"><span data-stu-id="02249-253">Command-line arguments</span></span>

<span data-ttu-id="02249-254">將命令列設定提供者放在提供者序列結尾是常見作法，因為這樣可以讓命令列引數覆寫由其它提供者所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="02249-254">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="02249-255">當您使用 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>初始化新的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 時，就會有這個提供者順序。</span><span class="sxs-lookup"><span data-stu-id="02249-255">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="02249-256">如需詳細資訊，請參閱 [Web 主機：設定主機](xref:fundamentals/host/web-host#set-up-a-host)。</span><span class="sxs-lookup"><span data-stu-id="02249-256">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="02249-257">您可以使用 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 並在 `Startup` <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> 方法，以為應用程式 (而非主機) 建立提供者順序：</span><span class="sxs-lookup"><span data-stu-id="02249-257">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

```csharp
public Startup(IHostingEnvironment env)
{
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
        .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
            reloadOnChange: true);

    var appAssembly = Assembly.Load(new AssemblyName(env.ApplicationName));

    if (appAssembly != null)
    {
        builder.AddUserSecrets(appAssembly, optional: true);
    }

    builder.AddEnvironmentVariables();

    Configuration = builder.Build();
}

public IConfiguration Configuration { get; }

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IConfiguration>(Configuration);
}
```

<span data-ttu-id="02249-258">在上面的範例中，環境名稱 (`env.EnvironmentName`) 與應用程式組件名稱 (`env.ApplicationName`) 是由 <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> 提供。</span><span class="sxs-lookup"><span data-stu-id="02249-258">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="02249-259">如需詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="02249-259">For more information, see <xref:fundamentals/environments>.</span></span> <span data-ttu-id="02249-260">透過 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 設定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="02249-260">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="02249-261">`SetBasePath` 在 [Microsoft.Extensions.Configuration FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) 套件中，該套件在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)內。</span><span class="sxs-lookup"><span data-stu-id="02249-261">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
<span data-ttu-id="02249-262">。</span><span class="sxs-lookup"><span data-stu-id="02249-262">.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="configureappconfiguration"></a><span data-ttu-id="02249-263">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="02249-263">ConfigureAppConfiguration</span></span>

<span data-ttu-id="02249-264">建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定提供者，以及由 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 自動新增的設定提供者：</span><span class="sxs-lookup"><span data-stu-id="02249-264">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

::: moniker-end

<span data-ttu-id="02249-265">應用程式啟動期間，可以使用 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 中為應用程式提供的組態，包括 `Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="02249-265">Configuration supplied to the app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="02249-266">如需詳細資訊，請參閱[在啟動期間存取組態](#access-configuration-during-startup)一節。</span><span class="sxs-lookup"><span data-stu-id="02249-266">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="02249-267">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-267">Command-line Configuration Provider</span></span>

<span data-ttu-id="02249-268"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> 會在執行階段從命令列引數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="02249-268">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="02249-269">為了啟用命令列設定，<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 延伸模組方法會在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫。</span><span class="sxs-lookup"><span data-stu-id="02249-269">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="02249-270">當您使用 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 初始化新的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 時，會自動呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="02249-270">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="02249-271">如需詳細資訊，請參閱 [Web 主機：設定主機](xref:fundamentals/host/web-host#set-up-a-host)。</span><span class="sxs-lookup"><span data-stu-id="02249-271">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="02249-272">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="02249-272">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="02249-273">從 *appsettings.json* 與 *appsettings.{Environment}.json* 載入其他選擇性設定。</span><span class="sxs-lookup"><span data-stu-id="02249-273">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="02249-274">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (在開發環境中)。</span><span class="sxs-lookup"><span data-stu-id="02249-274">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="02249-275">環境變數。</span><span class="sxs-lookup"><span data-stu-id="02249-275">Environment variables.</span></span>

<span data-ttu-id="02249-276">`CreateDefaultBuilder` 會最後才新增命令列設定提供者。</span><span class="sxs-lookup"><span data-stu-id="02249-276">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="02249-277">在執行階段傳遞的命令列引數會覆寫由其它提供者所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="02249-277">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="02249-278">`CreateDefaultBuilder` 會在建構主機時執行作業。</span><span class="sxs-lookup"><span data-stu-id="02249-278">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="02249-279">因此，由`CreateDefaultBuilder` 啟用的命令列設定可以影響主機的設定方式。</span><span class="sxs-lookup"><span data-stu-id="02249-279">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="02249-280">建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="02249-280">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="02249-281">`CreateDefaultBuilder` 已經呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="02249-281">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="02249-282">如果您需要提供應用程式設定並仍要能夠使用命令列引數覆寫該設定，請在 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 中呼叫應用程式的其他提供者，最後呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="02249-282">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call other providers here and call AddCommandLine last.
                config.AddCommandLine(args);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="02249-283">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="02249-283">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="02249-284">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>。</span><span class="sxs-lookup"><span data-stu-id="02249-284">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="02249-285">呼叫 `UseConfiguration` 時，`CreateDefaultBuilder` 已經呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="02249-285">`AddCommandLine` has already been called by `CreateDefaultBuilder` when `UseConfiguration` is called.</span></span> <span data-ttu-id="02249-286">如果您需要提供應用程式設定並仍要能夠使用命令列引數覆寫該設定，請在 `ConfigurationBuilder` 上呼叫應用程式的其他提供者，最後呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="02249-286">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers on a `ConfigurationBuilder` and call `AddCommandLine` last.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            // Call other providers here and call AddCommandLine last.
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="02249-287">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="02249-287">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="02249-288">若要啟用命令列設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="02249-288">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="02249-289">最後才呼叫提供者，以允許在執行階段傳遞的命令列引數覆寫由其他設定提供者所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="02249-289">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="02249-290">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="02249-290">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    // Call additional providers here as needed.
    // Call AddCommandLine last to allow arguments to override other configuration.
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="02249-291">**範例**</span><span class="sxs-lookup"><span data-stu-id="02249-291">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="02249-292">2.x 範例應用程式可發揮靜態方便方法 `CreateDefaultBuilder` 的優勢以建置主機，這包括對 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="02249-292">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="02249-293">1.x 範例應用程式會呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 上的 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>。</span><span class="sxs-lookup"><span data-stu-id="02249-293">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="02249-294">從專案目錄中開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="02249-294">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="02249-295">提供命令列引數給 `dotnet run` 命令 `dotnet run CommandLineKey=CommandLineValue`。</span><span class="sxs-lookup"><span data-stu-id="02249-295">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="02249-296">在應用程式執行之後，開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="02249-296">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="02249-297">觀察輸出是否包含提供給 `dotnet run` 之設定命令列引數的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="02249-297">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="02249-298">引數</span><span class="sxs-lookup"><span data-stu-id="02249-298">Arguments</span></span>

<span data-ttu-id="02249-299">當值後面接著空格時，值必須接著等號 (`=`)，或機碼必須有前置詞 (`--` 或 `/`)。</span><span class="sxs-lookup"><span data-stu-id="02249-299">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="02249-300">若使用等號 (例如 `CommandLineKey=`)，值可以是 Null。</span><span class="sxs-lookup"><span data-stu-id="02249-300">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="02249-301">索引鍵前置字元</span><span class="sxs-lookup"><span data-stu-id="02249-301">Key prefix</span></span>               | <span data-ttu-id="02249-302">範例</span><span class="sxs-lookup"><span data-stu-id="02249-302">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="02249-303">沒有前置字元</span><span class="sxs-lookup"><span data-stu-id="02249-303">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="02249-304">雙虛線 (`--`)</span><span class="sxs-lookup"><span data-stu-id="02249-304">Two dashes (`--`)</span></span>        | <span data-ttu-id="02249-305">`--CommandLineKey2=value2`、 `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="02249-305">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="02249-306">正斜線 (`/`)</span><span class="sxs-lookup"><span data-stu-id="02249-306">Forward slash (`/`)</span></span>      | <span data-ttu-id="02249-307">`/CommandLineKey3=value3`、 `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="02249-307">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="02249-308">在相同的命令中，請不要混合使用等號搭配使用空格之機碼值組的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="02249-308">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="02249-309">範例命令：</span><span class="sxs-lookup"><span data-stu-id="02249-309">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="02249-310">切換對應</span><span class="sxs-lookup"><span data-stu-id="02249-310">Switch mappings</span></span>

<span data-ttu-id="02249-311">參數對應允許索引鍵名稱取代邏輯。</span><span class="sxs-lookup"><span data-stu-id="02249-311">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="02249-312">當您使用 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 手動建置設定時，可以提供切換取代的字典給 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 方法。</span><span class="sxs-lookup"><span data-stu-id="02249-312">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="02249-313">使用切換對應字典時，會檢查字典中是否有任何索引鍵符合命令列引數所提供的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="02249-313">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="02249-314">如果在字典中找到命令列索引鍵，則會傳回字典值 (索引鍵取代) 以在應用程式的設定中設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="02249-314">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="02249-315">所有前面加上單虛線 (`-`) 的命令列索引鍵都需要切換對應。</span><span class="sxs-lookup"><span data-stu-id="02249-315">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="02249-316">切換對應字典索引鍵規則：</span><span class="sxs-lookup"><span data-stu-id="02249-316">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="02249-317">切換必須以單虛線 (`-`) 或雙虛線 (`--`) 開頭。</span><span class="sxs-lookup"><span data-stu-id="02249-317">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="02249-318">切換對應字典不能包含重複索引鍵。</span><span class="sxs-lookup"><span data-stu-id="02249-318">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="02249-319">建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="02249-319">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _switchMappings = 
        new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    // Do not pass the args to CreateDefaultBuilder
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder()
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddCommandLine(args, _switchMappings);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="02249-320">如上面的範例所示，當使用切換對應時，對 `CreateDefaultBuilder` 的呼叫不應該傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="02249-320">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="02249-321">`CreateDefaultBuilder` 方法的 `AddCommandLine` 呼叫不包括對應的切換，而且沒有任何方式可以將切換對應字典傳遞給 `CreateDefaultBuilder`。</span><span class="sxs-lookup"><span data-stu-id="02249-321">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="02249-322">若對應切換中包含引數且已傳遞給 `CreateDefaultBuilder`，其 `AddCommandLine` 提供者會無法使用 <xref:System.FormatException> 來初始化。</span><span class="sxs-lookup"><span data-stu-id="02249-322">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="02249-323">解決方案並非將引數傳遞給 `CreateDefaultBuilder`，而是允許 `ConfigurationBuilder` 方法的 `AddCommandLine` 方法同時處理引數與切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="02249-323">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var switchMappings = new Dictionary<string, string>
            {
                { "-CLKey1", "CommandLineKey1" },
                { "-CLKey2", "CommandLineKey2" }
            };

        var config = new ConfigurationBuilder()
            .AddCommandLine(args, switchMappings)
            .Build();

        // Do not pass the args to CreateDefaultBuilder
        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="02249-324">如上面的範例所示，當使用切換對應時，對 `CreateDefaultBuilder` 的呼叫不應該傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="02249-324">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="02249-325">`CreateDefaultBuilder` 方法的 `AddCommandLine` 呼叫不包括對應的切換，而且沒有任何方式可以將切換對應字典傳遞給 `CreateDefaultBuilder`。</span><span class="sxs-lookup"><span data-stu-id="02249-325">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="02249-326">若對應切換中包含引數且已傳遞給 `CreateDefaultBuilder`，其 `AddCommandLine` 提供者會無法使用 <xref:System.FormatException> 來初始化。</span><span class="sxs-lookup"><span data-stu-id="02249-326">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="02249-327">解決方案並非將引數傳遞給 `CreateDefaultBuilder`，而是允許 `ConfigurationBuilder` 方法的 `AddCommandLine` 方法同時處理引數與切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="02249-327">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public static void Main(string[] args)
{
    var switchMappings = new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    var config = new ConfigurationBuilder()
        .AddCommandLine(args, switchMappings)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .UseStartup<Startup>()
        .Start();

    using (host)
    {
        Console.ReadLine();
    }
}
```

::: moniker-end

<span data-ttu-id="02249-328">建立切換對應字典之後，它會包含下表中所示的資料。</span><span class="sxs-lookup"><span data-stu-id="02249-328">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="02249-329">Key</span><span class="sxs-lookup"><span data-stu-id="02249-329">Key</span></span>       | <span data-ttu-id="02249-330">值</span><span class="sxs-lookup"><span data-stu-id="02249-330">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="02249-331">若啟動應用程式時使用切換對應機碼，設定會接收由字典提供之機碼上的設定值：</span><span class="sxs-lookup"><span data-stu-id="02249-331">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="02249-332">執行上述命令之後，設定包含下表中顯示的值。</span><span class="sxs-lookup"><span data-stu-id="02249-332">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="02249-333">Key</span><span class="sxs-lookup"><span data-stu-id="02249-333">Key</span></span>               | <span data-ttu-id="02249-334">值</span><span class="sxs-lookup"><span data-stu-id="02249-334">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="02249-335">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-335">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="02249-336"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> 會在執行階段從環境變數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="02249-336">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="02249-337">若要啟用環境變數設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="02249-337">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="02249-338">在環境變數中搭配階層式機碼使用時，冒號分隔字元 (`:`) 可能無法在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="02249-338">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="02249-339">所有平台都支援雙底線 (`__`)，而且它會被冒號取代。</span><span class="sxs-lookup"><span data-stu-id="02249-339">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="02249-340">[Azure App Service](https://azure.microsoft.com/services/app-service/) 允許您在 Azure 入口網站中設定環境變數，以使用「環境變數設定提供者」覆寫應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="02249-340">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="02249-341">如需詳細資訊，請參閱 [Azure App：使用 Azure 入口網站覆寫應用程式設定](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)。</span><span class="sxs-lookup"><span data-stu-id="02249-341">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="02249-342">當您初始新的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 時，會為首碼為 `ASPNETCORE_` 的環境變數自動呼叫 `AddEnvironmentVariables`。</span><span class="sxs-lookup"><span data-stu-id="02249-342">`AddEnvironmentVariables` is automatically called for environment variables prefixed with `ASPNETCORE_` when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="02249-343">如需詳細資訊，請參閱 [Web 主機：設定主機](xref:fundamentals/host/web-host#set-up-a-host)。</span><span class="sxs-lookup"><span data-stu-id="02249-343">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="02249-344">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="02249-344">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="02249-345">來自無首碼之環境變數的應用程式組態 (在未提供首碼的情況下呼叫 `AddEnvironmentVariables`)。</span><span class="sxs-lookup"><span data-stu-id="02249-345">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="02249-346">從 *appsettings.json* 與 *appsettings.{Environment}.json* 載入其他選擇性設定。</span><span class="sxs-lookup"><span data-stu-id="02249-346">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="02249-347">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (在開發環境中)。</span><span class="sxs-lookup"><span data-stu-id="02249-347">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="02249-348">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="02249-348">Command-line arguments.</span></span>

<span data-ttu-id="02249-349">從使用者祕密與 *appsettings* 檔案建立設定之後，會呼叫「環境變數設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="02249-349">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="02249-350">在此位置呼叫提供者可讓系統在執行階段讀取環境變數，以覆寫由使用者祕密與 *appsettings* 檔案所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="02249-350">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="02249-351">建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="02249-351">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="02249-352">如果您需要從其他環境變數提供應用程式設定，請呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 中的應用程式其他提供者，並呼叫具有該前置詞的 `AddEnvironmentVariables`。</span><span class="sxs-lookup"><span data-stu-id="02249-352">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call additional providers here as needed.
                // Call AddEnvironmentVariables last if you need to allow environment
                // variables to override values from other providers.
                config.AddEnvironmentVariables(prefix: "PREFIX_");
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="02249-353">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="02249-353">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="02249-354">呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 執行個體上的 `AddEnvironmentVariables` 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="02249-354">Call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="02249-355">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>。</span><span class="sxs-lookup"><span data-stu-id="02249-355">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="02249-356">如果您需要從其他環境變數提供應用程式設定，請呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 中的應用程式其他提供者，並呼叫具有該前置詞的 `AddEnvironmentVariables`。</span><span class="sxs-lookup"><span data-stu-id="02249-356">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            // Call additional providers here as needed.
            // Call AddEnvironmentVariables last if you need to allow environment
            // variables to override values from other providers.
            .AddEnvironmentVariables(prefix: "PREFIX_")
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="02249-357">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="02249-357">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="02249-358">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="02249-358">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="02249-359">**範例**</span><span class="sxs-lookup"><span data-stu-id="02249-359">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="02249-360">2.x 範例應用程式可發揮靜態方便方法 `CreateDefaultBuilder` 的優勢以建置主機，這包括對 `AddEnvironmentVariables` 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="02249-360">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="02249-361">1.x 範例應用程式會呼叫 `ConfigurationBuilder` 上的 `AddEnvironmentVariables`。</span><span class="sxs-lookup"><span data-stu-id="02249-361">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="02249-362">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="02249-362">Run the sample app.</span></span> <span data-ttu-id="02249-363">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="02249-363">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="02249-364">觀察輸出是否包含環境變數 `ENVIRONMENT` 的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="02249-364">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="02249-365">值反映應用程式執行所在環境，在本機執行時，通常是 `Development`。</span><span class="sxs-lookup"><span data-stu-id="02249-365">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="02249-366">為縮短由應用程式轉譯的環境變數清單，應用程式會篩選開頭如下的環境變數：</span><span class="sxs-lookup"><span data-stu-id="02249-366">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="02249-367">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="02249-367">ASPNETCORE_</span></span>
* <span data-ttu-id="02249-368">urls</span><span class="sxs-lookup"><span data-stu-id="02249-368">urls</span></span>
* <span data-ttu-id="02249-369">記錄</span><span class="sxs-lookup"><span data-stu-id="02249-369">Logging</span></span>
* <span data-ttu-id="02249-370">環境</span><span class="sxs-lookup"><span data-stu-id="02249-370">ENVIRONMENT</span></span>
* <span data-ttu-id="02249-371">contentRoot</span><span class="sxs-lookup"><span data-stu-id="02249-371">contentRoot</span></span>
* <span data-ttu-id="02249-372">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="02249-372">AllowedHosts</span></span>
* <span data-ttu-id="02249-373">applicationName</span><span class="sxs-lookup"><span data-stu-id="02249-373">applicationName</span></span>
* <span data-ttu-id="02249-374">CommandLine</span><span class="sxs-lookup"><span data-stu-id="02249-374">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="02249-375">若想要將所有環境變數公開給應用程式使用，請將 *Pages/Index.cshtml.cs* 中的 `FilteredConfiguration` 變更為下面這樣：</span><span class="sxs-lookup"><span data-stu-id="02249-375">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="02249-376">若想要將所有環境變數公開給應用程式使用，請將 *Controllers/HomeController.cs* 中的 `FilteredConfiguration` 變更為下面這樣：</span><span class="sxs-lookup"><span data-stu-id="02249-376">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="02249-377">首碼</span><span class="sxs-lookup"><span data-stu-id="02249-377">Prefixes</span></span>

<span data-ttu-id="02249-378">當您將前置詞套用到 `AddEnvironmentVariables` 方法時，會篩選載入到應用程式設定中的環境變數。</span><span class="sxs-lookup"><span data-stu-id="02249-378">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="02249-379">例如，若要篩選前置詞為 `CUSTOM_` 的環境變數，請提供前置詞給設定提供者：</span><span class="sxs-lookup"><span data-stu-id="02249-379">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="02249-380">建立設定機碼值組時，會移除前置詞。</span><span class="sxs-lookup"><span data-stu-id="02249-380">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="02249-381">靜態方便方法 `CreateDefaultBuilder` 會建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 以建立應用程式的主機。</span><span class="sxs-lookup"><span data-stu-id="02249-381">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="02249-382">當 `WebHostBuilder` 建立時，它會在前置詞為 `ASPNETCORE_` 的環境變數中尋找其主機設定。</span><span class="sxs-lookup"><span data-stu-id="02249-382">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="02249-383">**連接字串前置詞**</span><span class="sxs-lookup"><span data-stu-id="02249-383">**Connection string prefixes**</span></span>

<span data-ttu-id="02249-384">設定 API 四個連接字串環境變數的特殊處理規則，這些這些環境變數牽涉到針對應用程式環境設定 Azure 連接字串。</span><span class="sxs-lookup"><span data-stu-id="02249-384">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="02249-385">若將前置詞提供給 `AddEnvironmentVariables`具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="02249-385">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="02249-386">連接字串前置詞</span><span class="sxs-lookup"><span data-stu-id="02249-386">Connection string prefix</span></span> | <span data-ttu-id="02249-387">提供者</span><span class="sxs-lookup"><span data-stu-id="02249-387">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="02249-388">自訂提供者</span><span class="sxs-lookup"><span data-stu-id="02249-388">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="02249-389">MySQL</span><span class="sxs-lookup"><span data-stu-id="02249-389">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="02249-390">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="02249-390">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="02249-391">SQL Server</span><span class="sxs-lookup"><span data-stu-id="02249-391">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="02249-392">當探索到具有下表所顯示之任何四個前置詞的環境變數並將其載入到設定中時：</span><span class="sxs-lookup"><span data-stu-id="02249-392">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="02249-393">會透過移除環境變數前置詞並新增設定機碼區段 (`ConnectionStrings`) 來移除具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="02249-393">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="02249-394">會建立新的設定機碼值組以代表資料庫連線提供者 (`CUSTOMCONNSTR_` 除外，這沒有所述提供者)。</span><span class="sxs-lookup"><span data-stu-id="02249-394">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="02249-395">環境變數機碼</span><span class="sxs-lookup"><span data-stu-id="02249-395">Environment variable key</span></span> | <span data-ttu-id="02249-396">已轉換的設定機碼</span><span class="sxs-lookup"><span data-stu-id="02249-396">Converted configuration key</span></span> | <span data-ttu-id="02249-397">提供者設定項目</span><span class="sxs-lookup"><span data-stu-id="02249-397">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="02249-398">設定項目未建立。</span><span class="sxs-lookup"><span data-stu-id="02249-398">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="02249-399">機碼：`ConnectionStrings:<KEY>_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="02249-399">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="02249-400">值：`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="02249-400">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="02249-401">機碼：`ConnectionStrings:<KEY>_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="02249-401">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="02249-402">值：`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="02249-402">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="02249-403">機碼：`ConnectionStrings:<KEY>_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="02249-403">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="02249-404">值：`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="02249-404">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="02249-405">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-405">File Configuration Provider</span></span>

<span data-ttu-id="02249-406"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> 是用於從檔案系統載入設定的基底類別。</span><span class="sxs-lookup"><span data-stu-id="02249-406"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="02249-407">下列設定提供者專用於特定檔案類型：</span><span class="sxs-lookup"><span data-stu-id="02249-407">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="02249-408">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-408">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="02249-409">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-409">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="02249-410">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-410">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="02249-411">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-411">INI Configuration Provider</span></span>

<span data-ttu-id="02249-412"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> 會在執行階段從 INI 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="02249-412">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="02249-413">若要啟用 INI 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="02249-413">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="02249-414">冒號可用來做為 INI 檔案設定中的區段分隔符號。</span><span class="sxs-lookup"><span data-stu-id="02249-414">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="02249-415">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="02249-415">Overloads permit specifying:</span></span>

* <span data-ttu-id="02249-416">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="02249-416">Whether the file is optional.</span></span>
* <span data-ttu-id="02249-417">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="02249-417">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="02249-418"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="02249-418">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="02249-419">建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="02249-419">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddIniFile("config.ini", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="02249-420">透過 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 設定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="02249-420">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="02249-421">`SetBasePath` 在 [Microsoft.Extensions.Configuration FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) 套件中，該套件在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)內。</span><span class="sxs-lookup"><span data-stu-id="02249-421">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="02249-422">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="02249-422">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="02249-423">呼叫 `CreateDefaultBuilder` 時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="02249-423">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddIniFile("config.ini", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="02249-424">透過 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 設定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="02249-424">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="02249-425">`SetBasePath` 在 [Microsoft.Extensions.Configuration FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) 套件中，該套件在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)內。</span><span class="sxs-lookup"><span data-stu-id="02249-425">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="02249-426">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="02249-426">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="02249-427">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="02249-427">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddIniFile("config.ini", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="02249-428">透過 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 設定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="02249-428">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="02249-429">`SetBasePath` 在 [Microsoft.Extensions.Configuration FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) 套件中，該套件在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)內。</span><span class="sxs-lookup"><span data-stu-id="02249-429">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="02249-430">INI 設定檔的一般範例：</span><span class="sxs-lookup"><span data-stu-id="02249-430">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="02249-431">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="02249-431">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="02249-432">section0:key0</span><span class="sxs-lookup"><span data-stu-id="02249-432">section0:key0</span></span>
* <span data-ttu-id="02249-433">section0:key1</span><span class="sxs-lookup"><span data-stu-id="02249-433">section0:key1</span></span>
* <span data-ttu-id="02249-434">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="02249-434">section1:subsection:key</span></span>
* <span data-ttu-id="02249-435">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="02249-435">section2:subsection0:key</span></span>
* <span data-ttu-id="02249-436">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="02249-436">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="02249-437">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-437">JSON Configuration Provider</span></span>

<span data-ttu-id="02249-438"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> 會在執行階段從 JSON 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="02249-438">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="02249-439">若要啟用 JSON 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="02249-439">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="02249-440">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="02249-440">Overloads permit specifying:</span></span>

* <span data-ttu-id="02249-441">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="02249-441">Whether the file is optional.</span></span>
* <span data-ttu-id="02249-442">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="02249-442">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="02249-443"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="02249-443">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="02249-444">當您使用 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 初始化新的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 時，會自動呼叫 `AddJsonFile` 兩次。</span><span class="sxs-lookup"><span data-stu-id="02249-444">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="02249-445">會呼叫此方法以從下列位置載入設定：</span><span class="sxs-lookup"><span data-stu-id="02249-445">The method is called to load configuration from:</span></span>

* <span data-ttu-id="02249-446">*appsettings.json* &ndash; 會先讀取此檔案。</span><span class="sxs-lookup"><span data-stu-id="02249-446">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="02249-447">檔案的環境版本可以覆寫由 *appsettings.json* 檔案提供的值。</span><span class="sxs-lookup"><span data-stu-id="02249-447">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="02249-448">*appsettings.{Environment}.json* &ndash; 檔案的環境版本是根據 [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*) 來載入的。</span><span class="sxs-lookup"><span data-stu-id="02249-448">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="02249-449">如需詳細資訊，請參閱 [Web 主機：設定主機](xref:fundamentals/host/web-host#set-up-a-host)。</span><span class="sxs-lookup"><span data-stu-id="02249-449">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="02249-450">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="02249-450">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="02249-451">環境變數。</span><span class="sxs-lookup"><span data-stu-id="02249-451">Environment variables.</span></span>
* <span data-ttu-id="02249-452">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (在開發環境中)。</span><span class="sxs-lookup"><span data-stu-id="02249-452">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="02249-453">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="02249-453">Command-line arguments.</span></span>

<span data-ttu-id="02249-454">會先建立 JSON 設定提供者。</span><span class="sxs-lookup"><span data-stu-id="02249-454">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="02249-455">因此，使用者祕密、環境變數與命令列引數會覆寫由 *appsettings* 檔案設定的設定。</span><span class="sxs-lookup"><span data-stu-id="02249-455">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="02249-456">在建置主機時，呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定檔案 (除了 *appsettings.json* 和  *appsettings.{Environment}.json* 以外) 的應用程式設定：</span><span class="sxs-lookup"><span data-stu-id="02249-456">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddJsonFile("config.json", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="02249-457">透過 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 設定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="02249-457">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="02249-458">`SetBasePath` 在 [Microsoft.Extensions.Configuration FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) 套件中，該套件在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)內。</span><span class="sxs-lookup"><span data-stu-id="02249-458">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="02249-459">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="02249-459">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="02249-460">您也可以直接呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 執行個體上的 `AddJsonFile` 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="02249-460">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="02249-461">呼叫 `CreateDefaultBuilder` 時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="02249-461">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("config.json", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="02249-462">透過 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 設定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="02249-462">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="02249-463">`SetBasePath` 在 [Microsoft.Extensions.Configuration FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) 套件中，該套件在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)內。</span><span class="sxs-lookup"><span data-stu-id="02249-463">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="02249-464">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="02249-464">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="02249-465">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="02249-465">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("config.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="02249-466">透過 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 設定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="02249-466">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="02249-467">`SetBasePath` 在 [Microsoft.Extensions.Configuration FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) 套件中，該套件在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)內。</span><span class="sxs-lookup"><span data-stu-id="02249-467">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="02249-468">**範例**</span><span class="sxs-lookup"><span data-stu-id="02249-468">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="02249-469">2.x 範例應用程式可發揮靜態方便方法 `CreateDefaultBuilder` 的優勢以建置主機，這包括對 `AddJsonFile` 的兩次呼叫。</span><span class="sxs-lookup"><span data-stu-id="02249-469">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="02249-470">從 *appsettings.json* 與 *appsettings.{Environment}.json* 載入設定。</span><span class="sxs-lookup"><span data-stu-id="02249-470">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="02249-471">1.x 範例應用程式會呼叫 `ConfigurationBuilder` 上的 `AddJsonFile` 兩次。</span><span class="sxs-lookup"><span data-stu-id="02249-471">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="02249-472">從 *appsettings.json* 與 *appsettings.{Environment}.json* 載入設定。</span><span class="sxs-lookup"><span data-stu-id="02249-472">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="02249-473">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="02249-473">Run the sample app.</span></span> <span data-ttu-id="02249-474">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="02249-474">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="02249-475">觀察輸出是否包含表格中所顯示之設定的機碼值組 (視環境而定)。</span><span class="sxs-lookup"><span data-stu-id="02249-475">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="02249-476">記錄設定機碼會使用冒號 (`:`) 做為階層式分隔符號。</span><span class="sxs-lookup"><span data-stu-id="02249-476">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="02249-477">Key</span><span class="sxs-lookup"><span data-stu-id="02249-477">Key</span></span>                        | <span data-ttu-id="02249-478">開發值</span><span class="sxs-lookup"><span data-stu-id="02249-478">Development Value</span></span> | <span data-ttu-id="02249-479">生產值</span><span class="sxs-lookup"><span data-stu-id="02249-479">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="02249-480">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="02249-480">Logging:LogLevel:System</span></span>    | <span data-ttu-id="02249-481">資訊</span><span class="sxs-lookup"><span data-stu-id="02249-481">Information</span></span>       | <span data-ttu-id="02249-482">資訊</span><span class="sxs-lookup"><span data-stu-id="02249-482">Information</span></span>      |
| <span data-ttu-id="02249-483">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="02249-483">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="02249-484">資訊</span><span class="sxs-lookup"><span data-stu-id="02249-484">Information</span></span>       | <span data-ttu-id="02249-485">資訊</span><span class="sxs-lookup"><span data-stu-id="02249-485">Information</span></span>      |
| <span data-ttu-id="02249-486">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="02249-486">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="02249-487">偵錯</span><span class="sxs-lookup"><span data-stu-id="02249-487">Debug</span></span>             | <span data-ttu-id="02249-488">錯誤</span><span class="sxs-lookup"><span data-stu-id="02249-488">Error</span></span>            |
| <span data-ttu-id="02249-489">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="02249-489">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="02249-490">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-490">XML Configuration Provider</span></span>

<span data-ttu-id="02249-491"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> 會在執行階段從 XML 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="02249-491">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="02249-492">若要啟用 XML 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="02249-492">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="02249-493">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="02249-493">Overloads permit specifying:</span></span>

* <span data-ttu-id="02249-494">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="02249-494">Whether the file is optional.</span></span>
* <span data-ttu-id="02249-495">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="02249-495">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="02249-496"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="02249-496">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="02249-497">建立設定機碼值組時，會忽略設定檔案的根節點。</span><span class="sxs-lookup"><span data-stu-id="02249-497">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="02249-498">請勿在檔案中指定文件類型定義 (DTD) 或命名空間。</span><span class="sxs-lookup"><span data-stu-id="02249-498">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="02249-499">建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="02249-499">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddXmlFile("config.xml", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="02249-500">透過 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 設定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="02249-500">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="02249-501">`SetBasePath` 在 [Microsoft.Extensions.Configuration FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) 套件中，該套件在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)內。</span><span class="sxs-lookup"><span data-stu-id="02249-501">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="02249-502">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="02249-502">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="02249-503">呼叫 `CreateDefaultBuilder` 時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="02249-503">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="02249-504">透過 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 設定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="02249-504">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="02249-505">`SetBasePath` 在 [Microsoft.Extensions.Configuration FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) 套件中，該套件在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)內。</span><span class="sxs-lookup"><span data-stu-id="02249-505">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="02249-506">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="02249-506">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="02249-507">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="02249-507">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="02249-508">透過 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 設定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="02249-508">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="02249-509">`SetBasePath` 在 [Microsoft.Extensions.Configuration FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) 套件中，該套件在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)內。</span><span class="sxs-lookup"><span data-stu-id="02249-509">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="02249-510">XML 設定檔可以針對重複的區段使用相異元素名稱：</span><span class="sxs-lookup"><span data-stu-id="02249-510">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="02249-511">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="02249-511">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="02249-512">section0:key0</span><span class="sxs-lookup"><span data-stu-id="02249-512">section0:key0</span></span>
* <span data-ttu-id="02249-513">section0:key1</span><span class="sxs-lookup"><span data-stu-id="02249-513">section0:key1</span></span>
* <span data-ttu-id="02249-514">section1:key0</span><span class="sxs-lookup"><span data-stu-id="02249-514">section1:key0</span></span>
* <span data-ttu-id="02249-515">section1:key1</span><span class="sxs-lookup"><span data-stu-id="02249-515">section1:key1</span></span>

<span data-ttu-id="02249-516">若 `name` 屬性是用來區別元素，則可以重複那些使用相同元素名稱的元素：</span><span class="sxs-lookup"><span data-stu-id="02249-516">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="02249-517">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="02249-517">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="02249-518">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="02249-518">section:section0:key:key0</span></span>
* <span data-ttu-id="02249-519">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="02249-519">section:section0:key:key1</span></span>
* <span data-ttu-id="02249-520">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="02249-520">section:section1:key:key0</span></span>
* <span data-ttu-id="02249-521">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="02249-521">section:section1:key:key1</span></span>

<span data-ttu-id="02249-522">屬性可用來提供值：</span><span class="sxs-lookup"><span data-stu-id="02249-522">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="02249-523">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="02249-523">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="02249-524">key:attribute</span><span class="sxs-lookup"><span data-stu-id="02249-524">key:attribute</span></span>
* <span data-ttu-id="02249-525">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="02249-525">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="02249-526">每個檔案機碼設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-526">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="02249-527"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> 使用目錄的檔案做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="02249-527">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="02249-528">機碼是檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="02249-528">The key is the file name.</span></span> <span data-ttu-id="02249-529">值包含檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="02249-529">The value contains the file's contents.</span></span> <span data-ttu-id="02249-530">每個檔案機碼設定提供者是在 Docker 主機案例中使用。</span><span class="sxs-lookup"><span data-stu-id="02249-530">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="02249-531">若要啟用每個檔案機碼設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="02249-531">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="02249-532">檔案的 `directoryPath` 必須是絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="02249-532">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="02249-533">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="02249-533">Overloads permit specifying:</span></span>

* <span data-ttu-id="02249-534">設定來源的 `Action<KeyPerFileConfigurationSource>` 委派。</span><span class="sxs-lookup"><span data-stu-id="02249-534">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="02249-535">目錄是否為選擇性與目錄的路徑。</span><span class="sxs-lookup"><span data-stu-id="02249-535">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="02249-536">雙底線 (`__`) 是做為檔案名稱中的設定金鑰分隔符號使用。</span><span class="sxs-lookup"><span data-stu-id="02249-536">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="02249-537">例如，檔案名稱 `Logging__LogLevel__System` 會產生設定金鑰 `Logging:LogLevel:System`。</span><span class="sxs-lookup"><span data-stu-id="02249-537">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="02249-538">建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="02249-538">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
                config.AddKeyPerFile(directoryPath: path, optional: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="02249-539">透過 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 設定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="02249-539">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="02249-540">`SetBasePath` 在 [Microsoft.Extensions.Configuration FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) 套件中，該套件在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)內。</span><span class="sxs-lookup"><span data-stu-id="02249-540">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="02249-541">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="02249-541">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
var config = new ConfigurationBuilder()
    .AddKeyPerFile(directoryPath: path, optional: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

::: moniker-end

## <a name="memory-configuration-provider"></a><span data-ttu-id="02249-542">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-542">Memory Configuration Provider</span></span>

<span data-ttu-id="02249-543"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> 使用記憶體內集合做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="02249-543">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="02249-544">若要啟用記憶體內集合設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="02249-544">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="02249-545">設定提供者可以使用 `IEnumerable<KeyValuePair<String,String>>` 來初始化。</span><span class="sxs-lookup"><span data-stu-id="02249-545">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="02249-546">建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="02249-546">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _dict = 
        new Dictionary<string, string>
        {
            {"MemoryCollectionKey1", "value1"},
            {"MemoryCollectionKey2", "value2"}
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddInMemoryCollection(_dict);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="02249-547">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="02249-547">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="02249-548">呼叫 `CreateDefaultBuilder` 時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="02249-548">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

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
}
```

<span data-ttu-id="02249-549">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="02249-549">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="02249-550">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="02249-550">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

var config = new ConfigurationBuilder()
    .AddInMemoryCollection(dict)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

## <a name="getvalue"></a><span data-ttu-id="02249-551">GetValue</span><span class="sxs-lookup"><span data-stu-id="02249-551">GetValue</span></span>

<span data-ttu-id="02249-552">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) 會從具有所指定機碼的設定擷取值，並將它轉換為指定的型別。</span><span class="sxs-lookup"><span data-stu-id="02249-552">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="02249-553">若找不到機碼，多載允許您提供預設值。</span><span class="sxs-lookup"><span data-stu-id="02249-553">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="02249-554">下列範例：</span><span class="sxs-lookup"><span data-stu-id="02249-554">The following example:</span></span>

* <span data-ttu-id="02249-555">從具有機碼 `NumberKey` 的組態擷取字串值。</span><span class="sxs-lookup"><span data-stu-id="02249-555">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="02249-556">若在組態機碼中找不到 `NumberKey`，則會使用預設值 `99`。</span><span class="sxs-lookup"><span data-stu-id="02249-556">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="02249-557">鍵入值為 `int`。</span><span class="sxs-lookup"><span data-stu-id="02249-557">Types the value as an `int`.</span></span>
* <span data-ttu-id="02249-558">在 `NumberConfig` 屬性中儲存值供頁面使用。</span><span class="sxs-lookup"><span data-stu-id="02249-558">Stores the value in the `NumberConfig` property for use by the page.</span></span>

```csharp
// using Microsoft.Extensions.Configuration;

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="02249-559">GetSection、GetChildren 與 Exists</span><span class="sxs-lookup"><span data-stu-id="02249-559">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="02249-560">針對下面的範例，請考慮下列 JSON 檔案。</span><span class="sxs-lookup"><span data-stu-id="02249-560">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="02249-561">在兩個區段中找到四個機碼，其中一個包括子區段組：</span><span class="sxs-lookup"><span data-stu-id="02249-561">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="02249-562">當檔案被讀入到設定之後，會建立下列唯一階層式機碼以存放設定值：</span><span class="sxs-lookup"><span data-stu-id="02249-562">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="02249-563">section0:key0</span><span class="sxs-lookup"><span data-stu-id="02249-563">section0:key0</span></span>
* <span data-ttu-id="02249-564">section0:key1</span><span class="sxs-lookup"><span data-stu-id="02249-564">section0:key1</span></span>
* <span data-ttu-id="02249-565">section1:key0</span><span class="sxs-lookup"><span data-stu-id="02249-565">section1:key0</span></span>
* <span data-ttu-id="02249-566">section1:key1</span><span class="sxs-lookup"><span data-stu-id="02249-566">section1:key1</span></span>
* <span data-ttu-id="02249-567">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="02249-567">section2:subsection0:key0</span></span>
* <span data-ttu-id="02249-568">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="02249-568">section2:subsection0:key1</span></span>
* <span data-ttu-id="02249-569">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="02249-569">section2:subsection1:key0</span></span>
* <span data-ttu-id="02249-570">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="02249-570">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="02249-571">GetSection</span><span class="sxs-lookup"><span data-stu-id="02249-571">GetSection</span></span>

<span data-ttu-id="02249-572">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) 會擷取具有所指定子區段機碼的設定子區段。</span><span class="sxs-lookup"><span data-stu-id="02249-572">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span> <span data-ttu-id="02249-573">`GetSection` 在 [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) 套件中，該套件在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)內。</span><span class="sxs-lookup"><span data-stu-id="02249-573">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="02249-574">若要傳回 `section1` 中只包含一個機碼值組的 <xref:Microsoft.Extensions.Configuration.IConfigurationSection>，請呼叫 `GetSection` 並提供區段名稱：</span><span class="sxs-lookup"><span data-stu-id="02249-574">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="02249-575">`configSection` 沒有值，只有索引鍵和路徑。</span><span class="sxs-lookup"><span data-stu-id="02249-575">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="02249-576">同樣地，若要取得 `section2:subsection0` 中之機碼的值，請呼叫 `GetSection` 並提供區段路徑：</span><span class="sxs-lookup"><span data-stu-id="02249-576">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="02249-577">`GetSection` 絕不會傳回 `null`。</span><span class="sxs-lookup"><span data-stu-id="02249-577">`GetSection` never returns `null`.</span></span> <span data-ttu-id="02249-578">若找不到相符的區段，會傳回空的 `IConfigurationSection`。</span><span class="sxs-lookup"><span data-stu-id="02249-578">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="02249-579">當 `GetSection` 傳回相符區段時，未填入 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value>。</span><span class="sxs-lookup"><span data-stu-id="02249-579">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="02249-580">當區段存在時，會傳回 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> 與 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path>。</span><span class="sxs-lookup"><span data-stu-id="02249-580">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="02249-581">GetChildren</span><span class="sxs-lookup"><span data-stu-id="02249-581">GetChildren</span></span>

<span data-ttu-id="02249-582">對 `section2` 上之 [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) 的呼叫會取得包括下列項目的 `IEnumerable<IConfigurationSection>`：</span><span class="sxs-lookup"><span data-stu-id="02249-582">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="02249-583">存在</span><span class="sxs-lookup"><span data-stu-id="02249-583">Exists</span></span>

<span data-ttu-id="02249-584">使用 [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) 來判斷設定區段是否存在：</span><span class="sxs-lookup"><span data-stu-id="02249-584">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="02249-585">以範例資料為例， `sectionExists` 是 `false`，因未設定資料中沒有 `section2:subsection2` 區段。</span><span class="sxs-lookup"><span data-stu-id="02249-585">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="02249-586">繫結到類別</span><span class="sxs-lookup"><span data-stu-id="02249-586">Bind to a class</span></span>

<span data-ttu-id="02249-587">設定可以繫結到類別，以使用*選項模式*代表相關設定的群組。</span><span class="sxs-lookup"><span data-stu-id="02249-587">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="02249-588">如需詳細資訊，請參閱<xref:fundamentals/configuration/options>。</span><span class="sxs-lookup"><span data-stu-id="02249-588">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="02249-589">設定值是以字串傳回，但是呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 會啟用 [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) 物件的建構。</span><span class="sxs-lookup"><span data-stu-id="02249-589">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="02249-590">`Bind` 在 [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) 套件中，該套件在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)內。</span><span class="sxs-lookup"><span data-stu-id="02249-590">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="02249-591">範例應用程式包含 `Starship` 模型 (*Models/Starship.cs*)：</span><span class="sxs-lookup"><span data-stu-id="02249-591">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="02249-592">當範例應用程式使用 JSON 設定提供者來載入設定時，*starship.json* 檔案的 `starship` 區段會建立設定：</span><span class="sxs-lookup"><span data-stu-id="02249-592">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="02249-593">會建立下列設定機碼值組：</span><span class="sxs-lookup"><span data-stu-id="02249-593">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="02249-594">Key</span><span class="sxs-lookup"><span data-stu-id="02249-594">Key</span></span>                   | <span data-ttu-id="02249-595">值</span><span class="sxs-lookup"><span data-stu-id="02249-595">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="02249-596">starship:name</span><span class="sxs-lookup"><span data-stu-id="02249-596">starship:name</span></span>         | <span data-ttu-id="02249-597">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="02249-597">USS Enterprise</span></span>                                    |
| <span data-ttu-id="02249-598">starship:registry</span><span class="sxs-lookup"><span data-stu-id="02249-598">starship:registry</span></span>     | <span data-ttu-id="02249-599">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="02249-599">NCC-1701</span></span>                                          |
| <span data-ttu-id="02249-600">starship:class</span><span class="sxs-lookup"><span data-stu-id="02249-600">starship:class</span></span>        | <span data-ttu-id="02249-601">Constitution</span><span class="sxs-lookup"><span data-stu-id="02249-601">Constitution</span></span>                                      |
| <span data-ttu-id="02249-602">starship:length</span><span class="sxs-lookup"><span data-stu-id="02249-602">starship:length</span></span>       | <span data-ttu-id="02249-603">304.8</span><span class="sxs-lookup"><span data-stu-id="02249-603">304.8</span></span>                                             |
| <span data-ttu-id="02249-604">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="02249-604">starship:commissioned</span></span> | <span data-ttu-id="02249-605">False</span><span class="sxs-lookup"><span data-stu-id="02249-605">False</span></span>                                             |
| <span data-ttu-id="02249-606">trademark</span><span class="sxs-lookup"><span data-stu-id="02249-606">trademark</span></span>             | <span data-ttu-id="02249-607">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="02249-607">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="02249-608">範例應用程式使用 `starship` 機碼呼叫 `GetSection`。</span><span class="sxs-lookup"><span data-stu-id="02249-608">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="02249-609">`starship` 機碼值組會被隔離。</span><span class="sxs-lookup"><span data-stu-id="02249-609">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="02249-610">會在子區段上呼叫 `Bind` 方法，並傳入 `Starship` 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="02249-610">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="02249-611">繫結執行個體值之後，執行個體會被指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="02249-611">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

<span data-ttu-id="02249-612">`GetSection` 在 [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) 套件中，該套件在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)內。</span><span class="sxs-lookup"><span data-stu-id="02249-612">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="02249-613">繫結至物件圖形</span><span class="sxs-lookup"><span data-stu-id="02249-613">Bind to an object graph</span></span>

<span data-ttu-id="02249-614"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 可以繫結整個 POCO 物件圖形。</span><span class="sxs-lookup"><span data-stu-id="02249-614"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="02249-615">`Bind` 在 [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) 套件中，該套件在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)內。</span><span class="sxs-lookup"><span data-stu-id="02249-615">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="02249-616">範例包含 `TvShow` 模型，其物件圖形包括 `Metadata` 與 `Actors` 類別 (*Models/TvShow.cs*)：</span><span class="sxs-lookup"><span data-stu-id="02249-616">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="02249-617">範例應用程式有 *tvshow.xml* 檔案，其中包含設定資料：</span><span class="sxs-lookup"><span data-stu-id="02249-617">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="02249-618">設定會被繫結到整個 `TvShow` 物件 (使用 `Bind` 方法)。</span><span class="sxs-lookup"><span data-stu-id="02249-618">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="02249-619">已繫結的執行個體會被指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="02249-619">The bound instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
viewModel.TvShow = tvShow;
```

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="02249-620">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 會繫結並傳回所指定型別。</span><span class="sxs-lookup"><span data-stu-id="02249-620">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="02249-621">`Get<T>` 比使用 `Bind` 更方便。</span><span class="sxs-lookup"><span data-stu-id="02249-621">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="02249-622">下列程式碼顯示如何根據上面的範例使用 `Get<T>`，這可讓您直接將已繫結的執行個體指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="02249-622">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

<span data-ttu-id="02249-623"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> 在 [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) 套件中，該套件在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)內。</span><span class="sxs-lookup"><span data-stu-id="02249-623"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="02249-624">ASP.NET Core 1.1 或更新版本中提供了 `Get<T>`。</span><span class="sxs-lookup"><span data-stu-id="02249-624">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="02249-625">`GetSection` 在 [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) 套件中，該套件在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)內。</span><span class="sxs-lookup"><span data-stu-id="02249-625">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="02249-626">將陣列繫結到類別</span><span class="sxs-lookup"><span data-stu-id="02249-626">Bind an array to a class</span></span>

<span data-ttu-id="02249-627">範例應用程式示範此節中解釋的概念。</span><span class="sxs-lookup"><span data-stu-id="02249-627">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="02249-628"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="02249-628">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="02249-629">任何公開數值機碼區段 (`:0:`、`:1:`、&hellip; `:{n}:`) 的陣列格式都能繫結到POCO 類別陣列。</span><span class="sxs-lookup"><span data-stu-id="02249-629">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span> <span data-ttu-id="02249-630">\`Bind\`\` 在 [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) 套件中，該套件在 [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) 中繼套件內。</span><span class="sxs-lookup"><span data-stu-id="02249-630">\`Bind\`\` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

> [!NOTE]
> <span data-ttu-id="02249-631">繫結是由慣例提供。</span><span class="sxs-lookup"><span data-stu-id="02249-631">Binding is provided by convention.</span></span> <span data-ttu-id="02249-632">自訂設定提供者不需要實作陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="02249-632">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="02249-633">**記憶體內陣列處理**</span><span class="sxs-lookup"><span data-stu-id="02249-633">**In-memory array processing**</span></span>

<span data-ttu-id="02249-634">考慮下表中顯示的設定機碼與值。</span><span class="sxs-lookup"><span data-stu-id="02249-634">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="02249-635">Key</span><span class="sxs-lookup"><span data-stu-id="02249-635">Key</span></span>             | <span data-ttu-id="02249-636">值</span><span class="sxs-lookup"><span data-stu-id="02249-636">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="02249-637">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="02249-637">array:entries:0</span></span> | <span data-ttu-id="02249-638">value0</span><span class="sxs-lookup"><span data-stu-id="02249-638">value0</span></span> |
| <span data-ttu-id="02249-639">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="02249-639">array:entries:1</span></span> | <span data-ttu-id="02249-640">value1</span><span class="sxs-lookup"><span data-stu-id="02249-640">value1</span></span> |
| <span data-ttu-id="02249-641">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="02249-641">array:entries:2</span></span> | <span data-ttu-id="02249-642">value2</span><span class="sxs-lookup"><span data-stu-id="02249-642">value2</span></span> |
| <span data-ttu-id="02249-643">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="02249-643">array:entries:4</span></span> | <span data-ttu-id="02249-644">value4</span><span class="sxs-lookup"><span data-stu-id="02249-644">value4</span></span> |
| <span data-ttu-id="02249-645">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="02249-645">array:entries:5</span></span> | <span data-ttu-id="02249-646">value5</span><span class="sxs-lookup"><span data-stu-id="02249-646">value5</span></span> |

<span data-ttu-id="02249-647">這些機碼與值是使用「記憶體設定提供者」在範例應用程式中載入：</span><span class="sxs-lookup"><span data-stu-id="02249-647">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="02249-648">陣列會跳過索引 &num;3 的值。</span><span class="sxs-lookup"><span data-stu-id="02249-648">The array skips a value for index &num;3.</span></span> <span data-ttu-id="02249-649">設定繫結程式無法繫結 Null 值或在已繫結的物件中建立 Null 項目，這在示範繫結此陣列的結果到某物件的時候已經很清楚。</span><span class="sxs-lookup"><span data-stu-id="02249-649">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="02249-650">在範例應用程式中，POCO 類別可用來存放繫結設定資料：</span><span class="sxs-lookup"><span data-stu-id="02249-650">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="02249-651">設定資料已繫結到物件：</span><span class="sxs-lookup"><span data-stu-id="02249-651">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="02249-652">`GetSection` 在 [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) 套件中，該套件在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)內。</span><span class="sxs-lookup"><span data-stu-id="02249-652">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="02249-653">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 語法也可以使用，這會產生更精簡的程式碼：</span><span class="sxs-lookup"><span data-stu-id="02249-653">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="02249-654">繫結物件 (`ArrayExample`的執行個體) 會從設定接收陣列資料。</span><span class="sxs-lookup"><span data-stu-id="02249-654">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="02249-655">`ArrayExample.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="02249-655">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="02249-656">`ArrayExample.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="02249-656">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="02249-657">0</span><span class="sxs-lookup"><span data-stu-id="02249-657">0</span></span>                            | <span data-ttu-id="02249-658">value0</span><span class="sxs-lookup"><span data-stu-id="02249-658">value0</span></span>                       |
| <span data-ttu-id="02249-659">1</span><span class="sxs-lookup"><span data-stu-id="02249-659">1</span></span>                            | <span data-ttu-id="02249-660">value1</span><span class="sxs-lookup"><span data-stu-id="02249-660">value1</span></span>                       |
| <span data-ttu-id="02249-661">2</span><span class="sxs-lookup"><span data-stu-id="02249-661">2</span></span>                            | <span data-ttu-id="02249-662">value2</span><span class="sxs-lookup"><span data-stu-id="02249-662">value2</span></span>                       |
| <span data-ttu-id="02249-663">3</span><span class="sxs-lookup"><span data-stu-id="02249-663">3</span></span>                            | <span data-ttu-id="02249-664">value4</span><span class="sxs-lookup"><span data-stu-id="02249-664">value4</span></span>                       |
| <span data-ttu-id="02249-665">4</span><span class="sxs-lookup"><span data-stu-id="02249-665">4</span></span>                            | <span data-ttu-id="02249-666">value5</span><span class="sxs-lookup"><span data-stu-id="02249-666">value5</span></span>                       |

<span data-ttu-id="02249-667">繫結物件中的索引 &num;3 存放 `array:4` 設定機碼與其 `value4` 的設定資料。</span><span class="sxs-lookup"><span data-stu-id="02249-667">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="02249-668">當繫結包含陣列的設定資料時，設定機碼中的陣列索引只會用來列舉設定資料 (當建立物件時)。</span><span class="sxs-lookup"><span data-stu-id="02249-668">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="02249-669">設定資料中不能保留 Null 值，當設定機碼中的陣列略過一或多個索引時，不會在繫結物件中建立 Null 值項目。</span><span class="sxs-lookup"><span data-stu-id="02249-669">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="02249-670">由會在設定中產生正確機碼值組的任何設定提供者繫結到 `ArrayExample` 執行個體之前，可以提供索引 &num;3 缺少的設定項目。</span><span class="sxs-lookup"><span data-stu-id="02249-670">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="02249-671">若範例包含具有缺少之機碼值組的額外 JSON 設定提供者，`ArrayExample.Entries` 會符合完整的設定陣列：</span><span class="sxs-lookup"><span data-stu-id="02249-671">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="02249-672">*missing_value.json*：</span><span class="sxs-lookup"><span data-stu-id="02249-672">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="02249-673">在 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>中：</span><span class="sxs-lookup"><span data-stu-id="02249-673">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="02249-674">在 `Startup` 建構函式中：</span><span class="sxs-lookup"><span data-stu-id="02249-674">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="02249-675">表格中顯示的機碼值組會載入到設定中。</span><span class="sxs-lookup"><span data-stu-id="02249-675">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="02249-676">Key</span><span class="sxs-lookup"><span data-stu-id="02249-676">Key</span></span>             | <span data-ttu-id="02249-677">值</span><span class="sxs-lookup"><span data-stu-id="02249-677">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="02249-678">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="02249-678">array:entries:3</span></span> | <span data-ttu-id="02249-679">value3</span><span class="sxs-lookup"><span data-stu-id="02249-679">value3</span></span> |

<span data-ttu-id="02249-680">在「JSON 設定提供者」包含索引 &num;3 的項目之後，若繫結 `ArrayExample` 類別執行個體，`ArrayExample.Entries` 陣列會包括這些值：</span><span class="sxs-lookup"><span data-stu-id="02249-680">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="02249-681">`ArrayExample.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="02249-681">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="02249-682">`ArrayExample.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="02249-682">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="02249-683">0</span><span class="sxs-lookup"><span data-stu-id="02249-683">0</span></span>                            | <span data-ttu-id="02249-684">value0</span><span class="sxs-lookup"><span data-stu-id="02249-684">value0</span></span>                       |
| <span data-ttu-id="02249-685">1</span><span class="sxs-lookup"><span data-stu-id="02249-685">1</span></span>                            | <span data-ttu-id="02249-686">value1</span><span class="sxs-lookup"><span data-stu-id="02249-686">value1</span></span>                       |
| <span data-ttu-id="02249-687">2</span><span class="sxs-lookup"><span data-stu-id="02249-687">2</span></span>                            | <span data-ttu-id="02249-688">value2</span><span class="sxs-lookup"><span data-stu-id="02249-688">value2</span></span>                       |
| <span data-ttu-id="02249-689">3</span><span class="sxs-lookup"><span data-stu-id="02249-689">3</span></span>                            | <span data-ttu-id="02249-690">value3</span><span class="sxs-lookup"><span data-stu-id="02249-690">value3</span></span>                       |
| <span data-ttu-id="02249-691">4</span><span class="sxs-lookup"><span data-stu-id="02249-691">4</span></span>                            | <span data-ttu-id="02249-692">value4</span><span class="sxs-lookup"><span data-stu-id="02249-692">value4</span></span>                       |
| <span data-ttu-id="02249-693">5</span><span class="sxs-lookup"><span data-stu-id="02249-693">5</span></span>                            | <span data-ttu-id="02249-694">value5</span><span class="sxs-lookup"><span data-stu-id="02249-694">value5</span></span>                       |

<span data-ttu-id="02249-695">**JSON 陣列處理**</span><span class="sxs-lookup"><span data-stu-id="02249-695">**JSON array processing**</span></span>

<span data-ttu-id="02249-696">若 JSON 檔案包含陣列，會為具有以零為基礎之區段索引的陣列元素建立設定機碼。</span><span class="sxs-lookup"><span data-stu-id="02249-696">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="02249-697">在下列設定檔中，`subsection` 是陣列：</span><span class="sxs-lookup"><span data-stu-id="02249-697">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="02249-698">「JSON 設定提供者」會將設定資料讀入到下列機碼值組：</span><span class="sxs-lookup"><span data-stu-id="02249-698">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="02249-699">Key</span><span class="sxs-lookup"><span data-stu-id="02249-699">Key</span></span>                     | <span data-ttu-id="02249-700">值</span><span class="sxs-lookup"><span data-stu-id="02249-700">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="02249-701">json_array:key</span><span class="sxs-lookup"><span data-stu-id="02249-701">json_array:key</span></span>          | <span data-ttu-id="02249-702">valueA</span><span class="sxs-lookup"><span data-stu-id="02249-702">valueA</span></span> |
| <span data-ttu-id="02249-703">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="02249-703">json_array:subsection:0</span></span> | <span data-ttu-id="02249-704">valueB</span><span class="sxs-lookup"><span data-stu-id="02249-704">valueB</span></span> |
| <span data-ttu-id="02249-705">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="02249-705">json_array:subsection:1</span></span> | <span data-ttu-id="02249-706">valueC</span><span class="sxs-lookup"><span data-stu-id="02249-706">valueC</span></span> |
| <span data-ttu-id="02249-707">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="02249-707">json_array:subsection:2</span></span> | <span data-ttu-id="02249-708">valueD</span><span class="sxs-lookup"><span data-stu-id="02249-708">valueD</span></span> |

<span data-ttu-id="02249-709">在範例應用程式中，下列 POCO 類別可用來繫結設定機碼值組：</span><span class="sxs-lookup"><span data-stu-id="02249-709">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="02249-710">在繫結之後，`JsonArrayExample.Key` 會存放值 `valueA`。</span><span class="sxs-lookup"><span data-stu-id="02249-710">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="02249-711">子區段值會存放在 POCO 陣列屬性 `Subsection` 中。</span><span class="sxs-lookup"><span data-stu-id="02249-711">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="02249-712">`JsonArrayExample.Subsection` 索引</span><span class="sxs-lookup"><span data-stu-id="02249-712">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="02249-713">`JsonArrayExample.Subsection` 值</span><span class="sxs-lookup"><span data-stu-id="02249-713">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="02249-714">0</span><span class="sxs-lookup"><span data-stu-id="02249-714">0</span></span>                                   | <span data-ttu-id="02249-715">valueB</span><span class="sxs-lookup"><span data-stu-id="02249-715">valueB</span></span>                              |
| <span data-ttu-id="02249-716">1</span><span class="sxs-lookup"><span data-stu-id="02249-716">1</span></span>                                   | <span data-ttu-id="02249-717">valueC</span><span class="sxs-lookup"><span data-stu-id="02249-717">valueC</span></span>                              |
| <span data-ttu-id="02249-718">2</span><span class="sxs-lookup"><span data-stu-id="02249-718">2</span></span>                                   | <span data-ttu-id="02249-719">valueD</span><span class="sxs-lookup"><span data-stu-id="02249-719">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="02249-720">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="02249-720">Custom configuration provider</span></span>

<span data-ttu-id="02249-721">範例應用程式示範如何建立會使用 [Entity Framework (EF)](/ef/core/) 從資料庫讀取設定機碼值組的基本設定提供者。</span><span class="sxs-lookup"><span data-stu-id="02249-721">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="02249-722">提供者具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="02249-722">The provider has the following characteristics:</span></span>

* <span data-ttu-id="02249-723">EF 記憶體內資料庫會用於示範用途。</span><span class="sxs-lookup"><span data-stu-id="02249-723">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="02249-724">若要使用需要連接字串的資料庫，請實作第二個 `ConfigurationBuilder` 以從另一個設定提供者提供連接字串。</span><span class="sxs-lookup"><span data-stu-id="02249-724">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="02249-725">啟動時，該提供者會將資料庫資料表讀入到設定中。</span><span class="sxs-lookup"><span data-stu-id="02249-725">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="02249-726">該提供者不會以個別機碼為基礎查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="02249-726">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="02249-727">未實作變更時重新載入，因此在應用程式啟動後更新資料庫對應用程式設定沒有影響。</span><span class="sxs-lookup"><span data-stu-id="02249-727">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="02249-728">定義 `EFConfigurationValue` 實體來在資料庫中存放設定值。</span><span class="sxs-lookup"><span data-stu-id="02249-728">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="02249-729">*Models/EFConfigurationValue.cs*：</span><span class="sxs-lookup"><span data-stu-id="02249-729">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="02249-730">新增 `EFConfigurationContext` 以存放及存取已設定的值。</span><span class="sxs-lookup"><span data-stu-id="02249-730">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="02249-731">*EFConfigurationProvider/EFConfigurationContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="02249-731">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="02249-732">建立會實作 <xref:Microsoft.Extensions.Configuration.IConfigurationSource> 的類別。</span><span class="sxs-lookup"><span data-stu-id="02249-732">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="02249-733">*EFConfigurationProvider/EFConfigurationSource.cs*：</span><span class="sxs-lookup"><span data-stu-id="02249-733">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="02249-734">透過繼承自 <xref:Microsoft.Extensions.Configuration.ConfigurationProvider> 來建立自訂設定提供者。</span><span class="sxs-lookup"><span data-stu-id="02249-734">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="02249-735">若資料庫是空的，設定提供者會初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="02249-735">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="02249-736">*EFConfigurationProvider/EFConfigurationProvider.cs*：</span><span class="sxs-lookup"><span data-stu-id="02249-736">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="02249-737">`AddEFConfiguration` 擴充方法允許新增設定來源到 `ConfigurationBuilder`。</span><span class="sxs-lookup"><span data-stu-id="02249-737">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="02249-738">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="02249-738">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="02249-739">下列程式碼顯示如何在 *Program.cs* 中使用自訂 `EFConfigurationProvider`：</span><span class="sxs-lookup"><span data-stu-id="02249-739">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="02249-740">在啟動期間存取組態</span><span class="sxs-lookup"><span data-stu-id="02249-740">Access configuration during startup</span></span>

<span data-ttu-id="02249-741">將 `IConfiguration` 插入到 `Startup` 建構函式，以存取 `Startup.ConfigureServices` 中的設定值。</span><span class="sxs-lookup"><span data-stu-id="02249-741">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="02249-742">若要存取 `Startup.Configure` 中的設定，請直接將 `IConfiguration` 插入到方法或從建構函式使用執行個體：</span><span class="sxs-lookup"><span data-stu-id="02249-742">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="02249-743">如需使用啟動方便方法來存取設定的範例，請參閱[應用程式啟動：便利的方法](xref:fundamentals/startup#convenience-methods)。</span><span class="sxs-lookup"><span data-stu-id="02249-743">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="02249-744">存取 Razor Pages 頁面或 MVC 檢視中的設定</span><span class="sxs-lookup"><span data-stu-id="02249-744">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="02249-745">若要在 [Razor Pages 頁面] 頁面或 MVC 檢視中存取組態設定，請針對 [Microsoft.Extensions.Configuration 命名空間](xref:Microsoft.Extensions.Configuration) [using 指示詞](xref:mvc/views/razor#using) ([C# 參考：using 指示詞](/dotnet/csharp/language-reference/keywords/using-directive))，並將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 插入到頁面或檢視中。</span><span class="sxs-lookup"><span data-stu-id="02249-745">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="02249-746">在 [Razor 頁面] 頁面中：</span><span class="sxs-lookup"><span data-stu-id="02249-746">In a Razor Pages page:</span></span>

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

<span data-ttu-id="02249-747">在 MVC 檢視中：</span><span class="sxs-lookup"><span data-stu-id="02249-747">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="02249-748">從外部組件新增設定</span><span class="sxs-lookup"><span data-stu-id="02249-748">Add configuration from an external assembly</span></span>

<span data-ttu-id="02249-749"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。</span><span class="sxs-lookup"><span data-stu-id="02249-749">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="02249-750">如需詳細資訊，請參閱<xref:fundamentals/configuration/platform-specific-configuration>。</span><span class="sxs-lookup"><span data-stu-id="02249-750">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="02249-751">其他資源</span><span class="sxs-lookup"><span data-stu-id="02249-751">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="02249-752">深入了解 Microsoft 設定</span><span class="sxs-lookup"><span data-stu-id="02249-752">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
