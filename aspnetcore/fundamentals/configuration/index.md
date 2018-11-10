---
title: ASP.NET Core 的設定
author: guardrex
description: 了解如何使用組態 API 設定 ASP.NET Core 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 10/09/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 6dd478770d4eae4d497da576c17fbe7d2c133b89
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021738"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="8fa60-103">ASP.NET Core 的設定</span><span class="sxs-lookup"><span data-stu-id="8fa60-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="8fa60-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8fa60-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="8fa60-105">ASP.NET Core 中的應用程式設定是以由*設定提供者*所建立的機碼值組為基礎。</span><span class="sxs-lookup"><span data-stu-id="8fa60-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="8fa60-106">設定提供者會從各種設定來源將設定資料讀取到機碼值組中：</span><span class="sxs-lookup"><span data-stu-id="8fa60-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="8fa60-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="8fa60-107">Azure Key Vault</span></span>
* <span data-ttu-id="8fa60-108">命令列引數</span><span class="sxs-lookup"><span data-stu-id="8fa60-108">Command-line arguments</span></span>
* <span data-ttu-id="8fa60-109">自訂提供者 (已安裝或已建立)</span><span class="sxs-lookup"><span data-stu-id="8fa60-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="8fa60-110">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="8fa60-110">Directory files</span></span>
* <span data-ttu-id="8fa60-111">環境變數</span><span class="sxs-lookup"><span data-stu-id="8fa60-111">Environment variables</span></span>
* <span data-ttu-id="8fa60-112">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="8fa60-112">In-memory .NET objects</span></span>
* <span data-ttu-id="8fa60-113">設定檔</span><span class="sxs-lookup"><span data-stu-id="8fa60-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="8fa60-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="8fa60-114">Azure Key Vault</span></span>
* <span data-ttu-id="8fa60-115">命令列引數</span><span class="sxs-lookup"><span data-stu-id="8fa60-115">Command-line arguments</span></span>
* <span data-ttu-id="8fa60-116">自訂提供者 (已安裝或已建立)</span><span class="sxs-lookup"><span data-stu-id="8fa60-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="8fa60-117">環境變數</span><span class="sxs-lookup"><span data-stu-id="8fa60-117">Environment variables</span></span>
* <span data-ttu-id="8fa60-118">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="8fa60-118">In-memory .NET objects</span></span>
* <span data-ttu-id="8fa60-119">設定檔</span><span class="sxs-lookup"><span data-stu-id="8fa60-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="8fa60-120">命令列引數</span><span class="sxs-lookup"><span data-stu-id="8fa60-120">Command-line arguments</span></span>
* <span data-ttu-id="8fa60-121">自訂提供者 (已安裝或已建立)</span><span class="sxs-lookup"><span data-stu-id="8fa60-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="8fa60-122">環境變數</span><span class="sxs-lookup"><span data-stu-id="8fa60-122">Environment variables</span></span>
* <span data-ttu-id="8fa60-123">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="8fa60-123">In-memory .NET objects</span></span>
* <span data-ttu-id="8fa60-124">設定檔</span><span class="sxs-lookup"><span data-stu-id="8fa60-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="8fa60-125">*選項模式*是此主題中所述之設定概念的延伸。</span><span class="sxs-lookup"><span data-stu-id="8fa60-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="8fa60-126">選項使用類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="8fa60-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="8fa60-127">如需使用選項模式的詳細資訊，請參閱 <xref:fundamentals/configuration/options>。</span><span class="sxs-lookup"><span data-stu-id="8fa60-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="8fa60-128">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8fa60-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8fa60-129">此主題中提供的範例需要：</span><span class="sxs-lookup"><span data-stu-id="8fa60-129">The examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="8fa60-130">使用 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 設定應用程式的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="8fa60-130">Setting the base path of the app with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="8fa60-131">參考 [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) 套件即可讓應用程式使用 `SetBasePath`。</span><span class="sxs-lookup"><span data-stu-id="8fa60-131">`SetBasePath` is made available to an app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="8fa60-132">使用 <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> 解析設定檔的區段。</span><span class="sxs-lookup"><span data-stu-id="8fa60-132">Resolving sections of configuration files with <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span></span> <span data-ttu-id="8fa60-133">參考 [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) 套件即可讓應用程式使用 `GetSection`。</span><span class="sxs-lookup"><span data-stu-id="8fa60-133">`GetSection` is made available to an app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="8fa60-134">使用 <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 與 [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 將設定繫結到 .NET 類別。</span><span class="sxs-lookup"><span data-stu-id="8fa60-134">Binding configuration to .NET classes with <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> and [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span></span> <span data-ttu-id="8fa60-135">參考 [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) 套件即可讓應用程式使用 `Bind` 與 `Get<T>`。</span><span class="sxs-lookup"><span data-stu-id="8fa60-135">`Bind` and `Get<T>` are made available to an app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span> <span data-ttu-id="8fa60-136">ASP.NET Core 1.1 或更新版本中提供了 `Get<T>`。</span><span class="sxs-lookup"><span data-stu-id="8fa60-136">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8fa60-137">這些套件均包含在 [Microsoft.AspNetCore.App j5/ 中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="8fa60-137">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8fa60-138">這三個套件都包含在 [Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)中。</span><span class="sxs-lookup"><span data-stu-id="8fa60-138">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="8fa60-139">主機與應用程式設定的比較</span><span class="sxs-lookup"><span data-stu-id="8fa60-139">Host vs. app configuration</span></span>

<span data-ttu-id="8fa60-140">設定及啟動應用程式之前，會先設定及啟動「主機」。</span><span class="sxs-lookup"><span data-stu-id="8fa60-140">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="8fa60-141">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="8fa60-141">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="8fa60-142">應用程式與主機都是使用此主題中所述的設定提供者來設定的。</span><span class="sxs-lookup"><span data-stu-id="8fa60-142">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="8fa60-143">主機設定機碼值組會成為應用程式全域設定的一部分。</span><span class="sxs-lookup"><span data-stu-id="8fa60-143">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="8fa60-144">如需有關當建置主機時如何使用設定提供者的詳細資訊，以及設定來源如何影響主機設定的詳細資訊，請參閱 <xref:fundamentals/host/index>。</span><span class="sxs-lookup"><span data-stu-id="8fa60-144">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="security"></a><span data-ttu-id="8fa60-145">安全性</span><span class="sxs-lookup"><span data-stu-id="8fa60-145">Security</span></span>

<span data-ttu-id="8fa60-146">採用下列最佳做法：</span><span class="sxs-lookup"><span data-stu-id="8fa60-146">Adopt the following best practices:</span></span>

* <span data-ttu-id="8fa60-147">永遠不要將密碼或其他敏感性資料儲存在設定提供者程式碼或純文字設定檔中。</span><span class="sxs-lookup"><span data-stu-id="8fa60-147">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="8fa60-148">不要在開發或測試環境中使用生產環境祕密。</span><span class="sxs-lookup"><span data-stu-id="8fa60-148">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="8fa60-149">請在專案外部指定祕密，以防止其意外認可至開放原始碼存放庫。</span><span class="sxs-lookup"><span data-stu-id="8fa60-149">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="8fa60-150">深入了解[如何使用多個環境](xref:fundamentals/environments)及[使用祕密管理員管理開發中的應用程式祕密安全儲存體](xref:security/app-secrets) (包括有關使用環境變數來存放敏感性資料的建議)。</span><span class="sxs-lookup"><span data-stu-id="8fa60-150">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="8fa60-151">「祕密管理員」使用「檔案設定提供者」以 JSON 檔案在本機系統上存放使用者祕密。</span><span class="sxs-lookup"><span data-stu-id="8fa60-151">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="8fa60-152">此主題稍後將說明「檔案設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="8fa60-152">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="8fa60-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 是安全儲存應用程式祕密的一個選項。</span><span class="sxs-lookup"><span data-stu-id="8fa60-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="8fa60-154">如需詳細資訊，請參閱<xref:security/key-vault-configuration>。</span><span class="sxs-lookup"><span data-stu-id="8fa60-154">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="8fa60-155">階層式設定資料</span><span class="sxs-lookup"><span data-stu-id="8fa60-155">Hierarchical configuration data</span></span>

<span data-ttu-id="8fa60-156">設定 API 可在設定機碼中使用分隔符號來壓平合併階層式資料，以管理階層式設定資料。</span><span class="sxs-lookup"><span data-stu-id="8fa60-156">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="8fa60-157">在下列 JSON 檔案中，兩個區段的結構式階層中有四個機碼存在：</span><span class="sxs-lookup"><span data-stu-id="8fa60-157">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="8fa60-158">當該檔案被讀入設定之後，會建立唯一機碼來維護設定來源的原始階層式資料結構。</span><span class="sxs-lookup"><span data-stu-id="8fa60-158">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="8fa60-159">系統會使用冒號 (`:`) 將區段與機碼壓平合併以維護原始結構：</span><span class="sxs-lookup"><span data-stu-id="8fa60-159">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="8fa60-160">section0:key0</span><span class="sxs-lookup"><span data-stu-id="8fa60-160">section0:key0</span></span>
* <span data-ttu-id="8fa60-161">section0:key1</span><span class="sxs-lookup"><span data-stu-id="8fa60-161">section0:key1</span></span>
* <span data-ttu-id="8fa60-162">section1:key0</span><span class="sxs-lookup"><span data-stu-id="8fa60-162">section1:key0</span></span>
* <span data-ttu-id="8fa60-163">section1:key1</span><span class="sxs-lookup"><span data-stu-id="8fa60-163">section1:key1</span></span>

<span data-ttu-id="8fa60-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> 與 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> 方法可用來在設定資料中隔離區段與區段的子系。</span><span class="sxs-lookup"><span data-stu-id="8fa60-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="8fa60-165">[GetSection,、 GetChildren 與 Exists](#getsection-getchildren-and-exists) 中說明這些方法。</span><span class="sxs-lookup"><span data-stu-id="8fa60-165">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="8fa60-166">慣例</span><span class="sxs-lookup"><span data-stu-id="8fa60-166">Conventions</span></span>

<span data-ttu-id="8fa60-167">在應用程式啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="8fa60-167">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="8fa60-168">「檔案設定提供者」可以在應用程式啟動之後且底層設定檔案變更時重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="8fa60-168">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="8fa60-169">此主題稍後將說明「檔案設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="8fa60-169">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="8fa60-170">您可以在應用程式的[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器中找到 <xref:Microsoft.Extensions.Configuration.IConfiguration>。</span><span class="sxs-lookup"><span data-stu-id="8fa60-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="8fa60-171">設定提供者無法使用 DI，因為當它們由主機設定時，它無法使用。</span><span class="sxs-lookup"><span data-stu-id="8fa60-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="8fa60-172">設定機碼會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="8fa60-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="8fa60-173">機碼區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="8fa60-173">Keys are case-insensitive.</span></span> <span data-ttu-id="8fa60-174">例如，`ConnectionString` 與 `connectionstring` 會被視為相等的機碼。</span><span class="sxs-lookup"><span data-stu-id="8fa60-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="8fa60-175">若相同機碼的值是由相同或不同設定提供者設定，在機碼上設定的最後一個值是所使用的值。</span><span class="sxs-lookup"><span data-stu-id="8fa60-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="8fa60-176">階層式機碼</span><span class="sxs-lookup"><span data-stu-id="8fa60-176">Hierarchical keys</span></span>
  * <span data-ttu-id="8fa60-177">在設定 API 內，冒號分隔字元 (`:`) 可在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="8fa60-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="8fa60-178">在環境變數中，冒號分隔字元可能無法在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="8fa60-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="8fa60-179">所有平台都支援雙底線 (`__`)，而且它會被轉換為冒號。</span><span class="sxs-lookup"><span data-stu-id="8fa60-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="8fa60-180">在 Azure Key Vault 中，階層式機碼使用 `--` (兩個破折號) 來做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="8fa60-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="8fa60-181">您必須提供程式碼，在祕密載入到應用程式的設定時將破折號取代為冒號。</span><span class="sxs-lookup"><span data-stu-id="8fa60-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="8fa60-182"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="8fa60-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="8fa60-183">[將陣列繫結到類別](#bind-an-array-to-a-class)一節說明陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="8fa60-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="8fa60-184">設定值會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="8fa60-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="8fa60-185">值是字串。</span><span class="sxs-lookup"><span data-stu-id="8fa60-185">Values are strings.</span></span>
* <span data-ttu-id="8fa60-186">Null 值無法存放在設定中或繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="8fa60-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="8fa60-187">提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-187">Providers</span></span>

<span data-ttu-id="8fa60-188">下表顯示可供 ASP.NET Core 應用程式使用的設定提供者。</span><span class="sxs-lookup"><span data-stu-id="8fa60-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="8fa60-189">提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-189">Provider</span></span> | <span data-ttu-id="8fa60-190">從&hellip;提供設定</span><span class="sxs-lookup"><span data-stu-id="8fa60-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="8fa60-191">[Azure Key Vault 設定提供者](xref:security/key-vault-configuration) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="8fa60-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="8fa60-192">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="8fa60-192">Azure Key Vault</span></span> |
| [<span data-ttu-id="8fa60-193">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-193">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="8fa60-194">命令列參數</span><span class="sxs-lookup"><span data-stu-id="8fa60-194">Command-line parameters</span></span> |
| [<span data-ttu-id="8fa60-195">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-195">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="8fa60-196">自訂來源</span><span class="sxs-lookup"><span data-stu-id="8fa60-196">Custom source</span></span> |
| [<span data-ttu-id="8fa60-197">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-197">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="8fa60-198">環境變數</span><span class="sxs-lookup"><span data-stu-id="8fa60-198">Environment variables</span></span> |
| [<span data-ttu-id="8fa60-199">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-199">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="8fa60-200">檔案 (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="8fa60-200">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="8fa60-201">每個檔案機碼的設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-201">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="8fa60-202">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="8fa60-202">Directory files</span></span> |
| [<span data-ttu-id="8fa60-203">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-203">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="8fa60-204">記憶體內集合</span><span class="sxs-lookup"><span data-stu-id="8fa60-204">In-memory collections</span></span> |
| <span data-ttu-id="8fa60-205">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="8fa60-205">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="8fa60-206">使用者設定檔目錄中的檔案</span><span class="sxs-lookup"><span data-stu-id="8fa60-206">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="8fa60-207">提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-207">Provider</span></span> | <span data-ttu-id="8fa60-208">從&hellip;提供設定</span><span class="sxs-lookup"><span data-stu-id="8fa60-208">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="8fa60-209">[Azure Key Vault 設定提供者](xref:security/key-vault-configuration) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="8fa60-209">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="8fa60-210">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="8fa60-210">Azure Key Vault</span></span> |
| [<span data-ttu-id="8fa60-211">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="8fa60-212">命令列參數</span><span class="sxs-lookup"><span data-stu-id="8fa60-212">Command-line parameters</span></span> |
| [<span data-ttu-id="8fa60-213">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="8fa60-214">自訂來源</span><span class="sxs-lookup"><span data-stu-id="8fa60-214">Custom source</span></span> |
| [<span data-ttu-id="8fa60-215">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-215">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="8fa60-216">環境變數</span><span class="sxs-lookup"><span data-stu-id="8fa60-216">Environment variables</span></span> |
| [<span data-ttu-id="8fa60-217">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="8fa60-218">檔案 (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="8fa60-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="8fa60-219">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-219">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="8fa60-220">記憶體內集合</span><span class="sxs-lookup"><span data-stu-id="8fa60-220">In-memory collections</span></span> |
| <span data-ttu-id="8fa60-221">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="8fa60-221">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="8fa60-222">使用者設定檔目錄中的檔案</span><span class="sxs-lookup"><span data-stu-id="8fa60-222">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="8fa60-223">提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-223">Provider</span></span> | <span data-ttu-id="8fa60-224">從&hellip;提供設定</span><span class="sxs-lookup"><span data-stu-id="8fa60-224">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="8fa60-225">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-225">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="8fa60-226">命令列參數</span><span class="sxs-lookup"><span data-stu-id="8fa60-226">Command-line parameters</span></span> |
| [<span data-ttu-id="8fa60-227">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-227">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="8fa60-228">自訂來源</span><span class="sxs-lookup"><span data-stu-id="8fa60-228">Custom source</span></span> |
| [<span data-ttu-id="8fa60-229">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-229">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="8fa60-230">環境變數</span><span class="sxs-lookup"><span data-stu-id="8fa60-230">Environment variables</span></span> |
| [<span data-ttu-id="8fa60-231">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-231">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="8fa60-232">檔案 (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="8fa60-232">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="8fa60-233">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-233">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="8fa60-234">記憶體內集合</span><span class="sxs-lookup"><span data-stu-id="8fa60-234">In-memory collections</span></span> |
| <span data-ttu-id="8fa60-235">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="8fa60-235">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="8fa60-236">使用者設定檔目錄中的檔案</span><span class="sxs-lookup"><span data-stu-id="8fa60-236">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="8fa60-237">在啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="8fa60-237">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="8fa60-238">此主題中所述的設定提供者是以字母順序描述，而非以您的程式碼安排它們的順序。</span><span class="sxs-lookup"><span data-stu-id="8fa60-238">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="8fa60-239">在您的程式碼中針對底層設定來源的優先順序，為設定提供者排序。</span><span class="sxs-lookup"><span data-stu-id="8fa60-239">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="8fa60-240">典型的設定提供者順序是：</span><span class="sxs-lookup"><span data-stu-id="8fa60-240">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="8fa60-241">檔案 (*appsettings.json*、*appsettings.{Environment}.json*，其中 `{Environment}` 是應用程式的目前裝載環境)</span><span class="sxs-lookup"><span data-stu-id="8fa60-241">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="8fa60-242">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="8fa60-242">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="8fa60-243">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (僅限開發環境)</span><span class="sxs-lookup"><span data-stu-id="8fa60-243">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="8fa60-244">環境變數</span><span class="sxs-lookup"><span data-stu-id="8fa60-244">Environment variables</span></span>
1. <span data-ttu-id="8fa60-245">命令列引數</span><span class="sxs-lookup"><span data-stu-id="8fa60-245">Command-line arguments</span></span>

<span data-ttu-id="8fa60-246">將命令列設定提供者放在提供者序列結尾是常見作法，因為這樣可以讓命令列引數覆寫由其它提供者所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="8fa60-246">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8fa60-247">當您使用 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>初始化新的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 時，就會有這個提供者順序。</span><span class="sxs-lookup"><span data-stu-id="8fa60-247">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="8fa60-248">如需詳細資訊，請參閱 [Web主機：設定主機](xref:fundamentals/host/web-host#set-up-a-host)。</span><span class="sxs-lookup"><span data-stu-id="8fa60-248">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8fa60-249">您可以使用 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 並在 `Startup` <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> 方法，以為應用程式 (而非主機) 建立提供者順序：</span><span class="sxs-lookup"><span data-stu-id="8fa60-249">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

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

<span data-ttu-id="8fa60-250">在上面的範例中，環境名稱 (`env.EnvironmentName`) 與應用程式組件名稱 (`env.ApplicationName`) 是由 <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> 提供。</span><span class="sxs-lookup"><span data-stu-id="8fa60-250">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="8fa60-251">如需詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="8fa60-251">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="configureappconfiguration"></a><span data-ttu-id="8fa60-252">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="8fa60-252">ConfigureAppConfiguration</span></span>

<span data-ttu-id="8fa60-253">建置 Web 主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定提供者，以及由 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 自動新增的設定提供者：</span><span class="sxs-lookup"><span data-stu-id="8fa60-253">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the Web Host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="8fa60-254">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-254">Command-line Configuration Provider</span></span>

<span data-ttu-id="8fa60-255"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> 會在執行階段從命令列引數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="8fa60-255">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="8fa60-256">為了啟用命令列設定，<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 延伸模組方法會在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫。</span><span class="sxs-lookup"><span data-stu-id="8fa60-256">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8fa60-257">當您使用 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 初始化新的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 時，會自動呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="8fa60-257">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="8fa60-258">如需詳細資訊，請參閱 [Web主機：設定主機](xref:fundamentals/host/web-host#set-up-a-host)。</span><span class="sxs-lookup"><span data-stu-id="8fa60-258">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="8fa60-259">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="8fa60-259">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="8fa60-260">從 *appsettings.json* 與 *appsettings.{Environment}.json* 載入其他選擇性設定。</span><span class="sxs-lookup"><span data-stu-id="8fa60-260">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="8fa60-261">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (在開發環境中)。</span><span class="sxs-lookup"><span data-stu-id="8fa60-261">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="8fa60-262">環境變數。</span><span class="sxs-lookup"><span data-stu-id="8fa60-262">Environment variables.</span></span>

<span data-ttu-id="8fa60-263">`CreateDefaultBuilder` 會最後才新增命令列設定提供者。</span><span class="sxs-lookup"><span data-stu-id="8fa60-263">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="8fa60-264">在執行階段傳遞的命令列引數會覆寫由其它提供者所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="8fa60-264">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="8fa60-265">`CreateDefaultBuilder` 會在建構主機時執行作業。</span><span class="sxs-lookup"><span data-stu-id="8fa60-265">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="8fa60-266">因此，由`CreateDefaultBuilder` 啟用的命令列設定可以影響主機的設定方式。</span><span class="sxs-lookup"><span data-stu-id="8fa60-266">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8fa60-267">建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="8fa60-267">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="8fa60-268">`CreateDefaultBuilder` 已經呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="8fa60-268">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="8fa60-269">如果您需要提供應用程式設定並仍要能夠使用命令列引數覆寫該設定，請在 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 中呼叫應用程式的其他提供者，最後呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="8fa60-269">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="8fa60-270">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="8fa60-270">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8fa60-271">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>。</span><span class="sxs-lookup"><span data-stu-id="8fa60-271">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="8fa60-272">呼叫 `UseConfiguration` 時，`CreateDefaultBuilder` 已經呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="8fa60-272">`AddCommandLine` has already been called by `CreateDefaultBuilder` when `UseConfiguration` is called.</span></span> <span data-ttu-id="8fa60-273">如果您需要提供應用程式設定並仍要能夠使用命令列引數覆寫該設定，請在 `ConfigurationBuilder` 上呼叫應用程式的其他提供者，最後呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="8fa60-273">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers on a `ConfigurationBuilder` and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="8fa60-274">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="8fa60-274">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8fa60-275">若要啟用命令列設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="8fa60-275">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="8fa60-276">最後才呼叫提供者，以允許在執行階段傳遞的命令列引數覆寫由其他設定提供者所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="8fa60-276">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="8fa60-277">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="8fa60-277">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="8fa60-278">**範例**</span><span class="sxs-lookup"><span data-stu-id="8fa60-278">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8fa60-279">2.x 範例應用程式可發揮靜態方便方法 `CreateDefaultBuilder` 的優勢以建置主機，這包括對 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="8fa60-279">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8fa60-280">1.x 範例應用程式會呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 上的 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>。</span><span class="sxs-lookup"><span data-stu-id="8fa60-280">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="8fa60-281">從專案目錄中開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="8fa60-281">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="8fa60-282">提供命令列引數給 `dotnet run` 命令 `dotnet run CommandLineKey=CommandLineValue`。</span><span class="sxs-lookup"><span data-stu-id="8fa60-282">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="8fa60-283">在應用程式執行之後，開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="8fa60-283">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="8fa60-284">觀察輸出是否包含提供給 `dotnet run` 之設定命令列引數的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="8fa60-284">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="8fa60-285">引數</span><span class="sxs-lookup"><span data-stu-id="8fa60-285">Arguments</span></span>

<span data-ttu-id="8fa60-286">當值後面接著空格時，值必須接著等號 (`=`)，或機碼必須有前置詞 (`--` 或 `/`)。</span><span class="sxs-lookup"><span data-stu-id="8fa60-286">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="8fa60-287">若使用等號 (例如 `CommandLineKey=`)，值可以是 Null。</span><span class="sxs-lookup"><span data-stu-id="8fa60-287">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="8fa60-288">索引鍵前置字元</span><span class="sxs-lookup"><span data-stu-id="8fa60-288">Key prefix</span></span>               | <span data-ttu-id="8fa60-289">範例</span><span class="sxs-lookup"><span data-stu-id="8fa60-289">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="8fa60-290">沒有前置字元</span><span class="sxs-lookup"><span data-stu-id="8fa60-290">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="8fa60-291">雙虛線 (`--`)</span><span class="sxs-lookup"><span data-stu-id="8fa60-291">Two dashes (`--`)</span></span>        | <span data-ttu-id="8fa60-292">`--CommandLineKey2=value2`、 `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="8fa60-292">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="8fa60-293">正斜線 (`/`)</span><span class="sxs-lookup"><span data-stu-id="8fa60-293">Forward slash (`/`)</span></span>      | <span data-ttu-id="8fa60-294">`/CommandLineKey3=value3`、 `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="8fa60-294">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="8fa60-295">在相同的命令中，請不要混合使用等號搭配使用空格之機碼值組的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="8fa60-295">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="8fa60-296">範例命令：</span><span class="sxs-lookup"><span data-stu-id="8fa60-296">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="8fa60-297">切換對應</span><span class="sxs-lookup"><span data-stu-id="8fa60-297">Switch mappings</span></span>

<span data-ttu-id="8fa60-298">參數對應允許索引鍵名稱取代邏輯。</span><span class="sxs-lookup"><span data-stu-id="8fa60-298">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="8fa60-299">當您使用 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 手動建置設定時，可以提供切換取代的字典給 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 方法。</span><span class="sxs-lookup"><span data-stu-id="8fa60-299">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="8fa60-300">使用切換對應字典時，會檢查字典中是否有任何索引鍵符合命令列引數所提供的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="8fa60-300">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="8fa60-301">如果在字典中找到命令列索引鍵，則會傳回字典值 (索引鍵取代) 以在應用程式的設定中設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="8fa60-301">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="8fa60-302">所有前面加上單虛線 (`-`) 的命令列索引鍵都需要切換對應。</span><span class="sxs-lookup"><span data-stu-id="8fa60-302">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="8fa60-303">切換對應字典索引鍵規則：</span><span class="sxs-lookup"><span data-stu-id="8fa60-303">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="8fa60-304">切換必須以單虛線 (`-`) 或雙虛線 (`--`) 開頭。</span><span class="sxs-lookup"><span data-stu-id="8fa60-304">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="8fa60-305">切換對應字典不能包含重複索引鍵。</span><span class="sxs-lookup"><span data-stu-id="8fa60-305">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8fa60-306">建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="8fa60-306">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="8fa60-307">如上面的範例所示，當使用切換對應時，對 `CreateDefaultBuilder` 的呼叫不應該傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="8fa60-307">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="8fa60-308">`CreateDefaultBuilder` 方法的 `AddCommandLine` 呼叫不包括對應的切換，而且沒有任何方式可以將切換對應字典傳遞給 `CreateDefaultBuilder`。</span><span class="sxs-lookup"><span data-stu-id="8fa60-308">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="8fa60-309">若對應切換中包含引數且已傳遞給 `CreateDefaultBuilder`，其 `AddCommandLine` 提供者會無法使用 <xref:System.FormatException> 來初始化。</span><span class="sxs-lookup"><span data-stu-id="8fa60-309">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="8fa60-310">解決方案並非將引數傳遞給 `CreateDefaultBuilder`，而是允許 `ConfigurationBuilder` 方法的 `AddCommandLine` 方法同時處理引數與切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="8fa60-310">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="8fa60-311">如上面的範例所示，當使用切換對應時，對 `CreateDefaultBuilder` 的呼叫不應該傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="8fa60-311">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="8fa60-312">`CreateDefaultBuilder` 方法的 `AddCommandLine` 呼叫不包括對應的切換，而且沒有任何方式可以將切換對應字典傳遞給 `CreateDefaultBuilder`。</span><span class="sxs-lookup"><span data-stu-id="8fa60-312">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="8fa60-313">若對應切換中包含引數且已傳遞給 `CreateDefaultBuilder`，其 `AddCommandLine` 提供者會無法使用 <xref:System.FormatException> 來初始化。</span><span class="sxs-lookup"><span data-stu-id="8fa60-313">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="8fa60-314">解決方案並非將引數傳遞給 `CreateDefaultBuilder`，而是允許 `ConfigurationBuilder` 方法的 `AddCommandLine` 方法同時處理引數與切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="8fa60-314">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="8fa60-315">建立切換對應字典之後，它會包含下表中所示的資料。</span><span class="sxs-lookup"><span data-stu-id="8fa60-315">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="8fa60-316">Key</span><span class="sxs-lookup"><span data-stu-id="8fa60-316">Key</span></span>       | <span data-ttu-id="8fa60-317">值</span><span class="sxs-lookup"><span data-stu-id="8fa60-317">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="8fa60-318">若啟動應用程式時使用切換對應機碼，設定會接收由字典提供之機碼上的設定值：</span><span class="sxs-lookup"><span data-stu-id="8fa60-318">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="8fa60-319">執行上述命令之後，設定包含下表中顯示的值。</span><span class="sxs-lookup"><span data-stu-id="8fa60-319">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="8fa60-320">Key</span><span class="sxs-lookup"><span data-stu-id="8fa60-320">Key</span></span>               | <span data-ttu-id="8fa60-321">值</span><span class="sxs-lookup"><span data-stu-id="8fa60-321">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="8fa60-322">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-322">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="8fa60-323"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> 會在執行階段從環境變數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="8fa60-323">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="8fa60-324">若要啟用環境變數設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="8fa60-324">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="8fa60-325">在環境變數中搭配階層式機碼使用時，冒號分隔字元 (`:`) 可能無法在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="8fa60-325">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="8fa60-326">所有平台都支援雙底線 (`__`)，而且它會被冒號取代。</span><span class="sxs-lookup"><span data-stu-id="8fa60-326">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="8fa60-327">[Azure App Service](https://azure.microsoft.com/services/app-service/) 允許您在 Azure 入口網站中設定環境變數，以使用「環境變數設定提供者」覆寫應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="8fa60-327">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="8fa60-328">如需詳細資訊，請參閱 [Azure App：使用 Azure 入口網站覆寫應用程式設定](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)。</span><span class="sxs-lookup"><span data-stu-id="8fa60-328">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8fa60-329">當您使用 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 初始化新的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 時，會自動呼叫 `AddEnvironmentVariables`。</span><span class="sxs-lookup"><span data-stu-id="8fa60-329">`AddEnvironmentVariables` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="8fa60-330">如需詳細資訊，請參閱 [Web主機：設定主機](xref:fundamentals/host/web-host#set-up-a-host)。</span><span class="sxs-lookup"><span data-stu-id="8fa60-330">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="8fa60-331">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="8fa60-331">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="8fa60-332">從 *appsettings.json* 與 *appsettings.{Environment}.json* 載入其他選擇性設定。</span><span class="sxs-lookup"><span data-stu-id="8fa60-332">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="8fa60-333">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (在開發環境中)。</span><span class="sxs-lookup"><span data-stu-id="8fa60-333">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="8fa60-334">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="8fa60-334">Command-line arguments.</span></span>

<span data-ttu-id="8fa60-335">從使用者祕密與 *appsettings* 檔案建立設定之後，會呼叫「環境變數設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="8fa60-335">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="8fa60-336">在此位置呼叫提供者可讓系統在執行階段讀取環境變數，以覆寫由使用者祕密與 *appsettings* 檔案所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="8fa60-336">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8fa60-337">建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="8fa60-337">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="8fa60-338">具有前置詞 `ASPNETCORE_` 之環境變數的 `AddEnvironmentVariables`，已由 `CreateDefaultBuilder` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="8fa60-338">`AddEnvironmentVariables` for environment variables prefixed with `ASPNETCORE_` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="8fa60-339">如果您需要從其他環境變數提供應用程式設定，請呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 中的應用程式其他提供者，並呼叫具有該前置詞的 `AddEnvironmentVariables`。</span><span class="sxs-lookup"><span data-stu-id="8fa60-339">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="8fa60-340">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="8fa60-340">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8fa60-341">呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 執行個體上的 `AddEnvironmentVariables` 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="8fa60-341">Call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="8fa60-342">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>。</span><span class="sxs-lookup"><span data-stu-id="8fa60-342">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="8fa60-343">具有前置詞 `ASPNETCORE_` 之環境變數的 `AddEnvironmentVariables`，已由 `CreateDefaultBuilder` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="8fa60-343">`AddEnvironmentVariables` for environment variables prefixed with `ASPNETCORE_` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="8fa60-344">如果您需要從其他環境變數提供應用程式設定，請呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 中的應用程式其他提供者，並呼叫具有該前置詞的 `AddEnvironmentVariables`。</span><span class="sxs-lookup"><span data-stu-id="8fa60-344">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="8fa60-345">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="8fa60-345">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8fa60-346">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="8fa60-346">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="8fa60-347">**範例**</span><span class="sxs-lookup"><span data-stu-id="8fa60-347">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8fa60-348">2.x 範例應用程式可發揮靜態方便方法 `CreateDefaultBuilder` 的優勢以建置主機，這包括對 `AddEnvironmentVariables` 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="8fa60-348">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8fa60-349">1.x 範例應用程式會呼叫 `ConfigurationBuilder` 上的 `AddEnvironmentVariables`。</span><span class="sxs-lookup"><span data-stu-id="8fa60-349">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="8fa60-350">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="8fa60-350">Run the sample app.</span></span> <span data-ttu-id="8fa60-351">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="8fa60-351">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="8fa60-352">觀察輸出是否包含環境變數 `ENVIRONMENT` 的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="8fa60-352">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="8fa60-353">值反映應用程式執行所在環境，在本機執行時，通常是 `Development`。</span><span class="sxs-lookup"><span data-stu-id="8fa60-353">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="8fa60-354">為縮短由應用程式轉譯的環境變數清單，應用程式會篩選開頭如下的環境變數：</span><span class="sxs-lookup"><span data-stu-id="8fa60-354">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="8fa60-355">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="8fa60-355">ASPNETCORE_</span></span>
* <span data-ttu-id="8fa60-356">urls</span><span class="sxs-lookup"><span data-stu-id="8fa60-356">urls</span></span>
* <span data-ttu-id="8fa60-357">記錄</span><span class="sxs-lookup"><span data-stu-id="8fa60-357">Logging</span></span>
* <span data-ttu-id="8fa60-358">環境</span><span class="sxs-lookup"><span data-stu-id="8fa60-358">ENVIRONMENT</span></span>
* <span data-ttu-id="8fa60-359">contentRoot</span><span class="sxs-lookup"><span data-stu-id="8fa60-359">contentRoot</span></span>
* <span data-ttu-id="8fa60-360">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="8fa60-360">AllowedHosts</span></span>
* <span data-ttu-id="8fa60-361">applicationName</span><span class="sxs-lookup"><span data-stu-id="8fa60-361">applicationName</span></span>
* <span data-ttu-id="8fa60-362">CommandLine</span><span class="sxs-lookup"><span data-stu-id="8fa60-362">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8fa60-363">若想要將所有環境變數公開給應用程式使用，請將 *Pages/Index.cshtml.cs* 中的 `FilteredConfiguration` 變更為下面這樣：</span><span class="sxs-lookup"><span data-stu-id="8fa60-363">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8fa60-364">若想要將所有環境變數公開給應用程式使用，請將 *Controllers/HomeController.cs* 中的 `FilteredConfiguration` 變更為下面這樣：</span><span class="sxs-lookup"><span data-stu-id="8fa60-364">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="8fa60-365">首碼</span><span class="sxs-lookup"><span data-stu-id="8fa60-365">Prefixes</span></span>

<span data-ttu-id="8fa60-366">當您將前置詞套用到 `AddEnvironmentVariables` 方法時，會篩選載入到應用程式設定中的環境變數。</span><span class="sxs-lookup"><span data-stu-id="8fa60-366">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="8fa60-367">例如，若要篩選前置詞為 `CUSTOM_` 的環境變數，請提供前置詞給設定提供者：</span><span class="sxs-lookup"><span data-stu-id="8fa60-367">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="8fa60-368">建立設定機碼值組時，會移除前置詞。</span><span class="sxs-lookup"><span data-stu-id="8fa60-368">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8fa60-369">靜態方便方法 `CreateDefaultBuilder` 會建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 以建立應用程式的主機。</span><span class="sxs-lookup"><span data-stu-id="8fa60-369">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="8fa60-370">當 `WebHostBuilder` 建立時，它會在前置詞為 `ASPNETCORE_` 的環境變數中尋找其主機設定。</span><span class="sxs-lookup"><span data-stu-id="8fa60-370">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="8fa60-371">**連接字串前置詞**</span><span class="sxs-lookup"><span data-stu-id="8fa60-371">**Connection string prefixes**</span></span>

<span data-ttu-id="8fa60-372">設定 API 四個連接字串環境變數的特殊處理規則，這些這些環境變數牽涉到針對應用程式環境設定 Azure 連接字串。</span><span class="sxs-lookup"><span data-stu-id="8fa60-372">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="8fa60-373">若將前置詞提供給 `AddEnvironmentVariables`具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="8fa60-373">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="8fa60-374">連接字串前置詞</span><span class="sxs-lookup"><span data-stu-id="8fa60-374">Connection string prefix</span></span> | <span data-ttu-id="8fa60-375">提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-375">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="8fa60-376">自訂提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-376">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="8fa60-377">MySQL</span><span class="sxs-lookup"><span data-stu-id="8fa60-377">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="8fa60-378">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="8fa60-378">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="8fa60-379">SQL Server</span><span class="sxs-lookup"><span data-stu-id="8fa60-379">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="8fa60-380">當探索到具有下表所顯示之任何四個前置詞的環境變數並將其載入到設定中時：</span><span class="sxs-lookup"><span data-stu-id="8fa60-380">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="8fa60-381">會透過移除環境變數前置詞並新增設定機碼區段 (`ConnectionStrings`) 來移除具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="8fa60-381">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="8fa60-382">會建立新的設定機碼值組以代表資料庫連線提供者 (`CUSTOMCONNSTR_` 除外，這沒有所述提供者)。</span><span class="sxs-lookup"><span data-stu-id="8fa60-382">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="8fa60-383">環境變數機碼</span><span class="sxs-lookup"><span data-stu-id="8fa60-383">Environment variable key</span></span> | <span data-ttu-id="8fa60-384">已轉換的設定機碼</span><span class="sxs-lookup"><span data-stu-id="8fa60-384">Converted configuration key</span></span> | <span data-ttu-id="8fa60-385">提供者設定項目</span><span class="sxs-lookup"><span data-stu-id="8fa60-385">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="8fa60-386">設定項目未建立。</span><span class="sxs-lookup"><span data-stu-id="8fa60-386">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="8fa60-387">機碼：`ConnectionStrings:<KEY>_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="8fa60-387">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="8fa60-388">值：`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="8fa60-388">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="8fa60-389">機碼：`ConnectionStrings:<KEY>_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="8fa60-389">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="8fa60-390">值：`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="8fa60-390">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="8fa60-391">機碼：`ConnectionStrings:<KEY>_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="8fa60-391">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="8fa60-392">值：`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="8fa60-392">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="8fa60-393">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-393">File Configuration Provider</span></span>

<span data-ttu-id="8fa60-394"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> 是用於從檔案系統載入設定的基底類別。</span><span class="sxs-lookup"><span data-stu-id="8fa60-394"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="8fa60-395">下列設定提供者專用於特定檔案類型：</span><span class="sxs-lookup"><span data-stu-id="8fa60-395">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="8fa60-396">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-396">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="8fa60-397">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-397">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="8fa60-398">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-398">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="8fa60-399">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-399">INI Configuration Provider</span></span>

<span data-ttu-id="8fa60-400"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> 會在執行階段從 INI 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="8fa60-400">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="8fa60-401">若要啟用 INI 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="8fa60-401">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="8fa60-402">冒號可用來做為 INI 檔案設定中的區段分隔符號。</span><span class="sxs-lookup"><span data-stu-id="8fa60-402">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="8fa60-403">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="8fa60-403">Overloads permit specifying:</span></span>

* <span data-ttu-id="8fa60-404">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="8fa60-404">Whether the file is optional.</span></span>
* <span data-ttu-id="8fa60-405">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="8fa60-405">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="8fa60-406"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="8fa60-406">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8fa60-407">建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="8fa60-407">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="8fa60-408">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="8fa60-408">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8fa60-409">呼叫 `CreateDefaultBuilder` 時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="8fa60-409">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="8fa60-410">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="8fa60-410">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8fa60-411">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="8fa60-411">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="8fa60-412">INI 設定檔的一般範例：</span><span class="sxs-lookup"><span data-stu-id="8fa60-412">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="8fa60-413">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="8fa60-413">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="8fa60-414">section0:key0</span><span class="sxs-lookup"><span data-stu-id="8fa60-414">section0:key0</span></span>
* <span data-ttu-id="8fa60-415">section0:key1</span><span class="sxs-lookup"><span data-stu-id="8fa60-415">section0:key1</span></span>
* <span data-ttu-id="8fa60-416">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="8fa60-416">section1:subsection:key</span></span>
* <span data-ttu-id="8fa60-417">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="8fa60-417">section2:subsection0:key</span></span>
* <span data-ttu-id="8fa60-418">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="8fa60-418">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="8fa60-419">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-419">JSON Configuration Provider</span></span>

<span data-ttu-id="8fa60-420"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> 會在執行階段從 JSON 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="8fa60-420">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="8fa60-421">若要啟用 JSON 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="8fa60-421">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="8fa60-422">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="8fa60-422">Overloads permit specifying:</span></span>

* <span data-ttu-id="8fa60-423">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="8fa60-423">Whether the file is optional.</span></span>
* <span data-ttu-id="8fa60-424">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="8fa60-424">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="8fa60-425"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="8fa60-425">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8fa60-426">當您使用 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 初始化新的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 時，會自動呼叫 `AddJsonFile` 兩次。</span><span class="sxs-lookup"><span data-stu-id="8fa60-426">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="8fa60-427">會呼叫此方法以從下列位置載入設定：</span><span class="sxs-lookup"><span data-stu-id="8fa60-427">The method is called to load configuration from:</span></span>

* <span data-ttu-id="8fa60-428">*appsettings.json* &ndash; 會先讀取此檔案。</span><span class="sxs-lookup"><span data-stu-id="8fa60-428">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="8fa60-429">檔案的環境版本可以覆寫由 *appsettings.json* 檔案提供的值。</span><span class="sxs-lookup"><span data-stu-id="8fa60-429">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="8fa60-430">*appsettings.{Environment}.json* &ndash; 檔案的環境版本是根據 [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*) 來載入的。</span><span class="sxs-lookup"><span data-stu-id="8fa60-430">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="8fa60-431">如需詳細資訊，請參閱 [Web主機：設定主機](xref:fundamentals/host/web-host#set-up-a-host)。</span><span class="sxs-lookup"><span data-stu-id="8fa60-431">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="8fa60-432">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="8fa60-432">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="8fa60-433">環境變數。</span><span class="sxs-lookup"><span data-stu-id="8fa60-433">Environment variables.</span></span>
* <span data-ttu-id="8fa60-434">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (在開發環境中)。</span><span class="sxs-lookup"><span data-stu-id="8fa60-434">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="8fa60-435">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="8fa60-435">Command-line arguments.</span></span>

<span data-ttu-id="8fa60-436">會先建立 JSON 設定提供者。</span><span class="sxs-lookup"><span data-stu-id="8fa60-436">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="8fa60-437">因此，使用者祕密、環境變數與命令列引數會覆寫由 *appsettings* 檔案設定的設定。</span><span class="sxs-lookup"><span data-stu-id="8fa60-437">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8fa60-438">在建置主機時，呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定檔案 (除了 *appsettings.json* 和  *appsettings.{Environment}.json* 以外) 的應用程式設定：</span><span class="sxs-lookup"><span data-stu-id="8fa60-438">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

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

<span data-ttu-id="8fa60-439">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="8fa60-439">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8fa60-440">您也可以直接呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 執行個體上的 `AddJsonFile` 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="8fa60-440">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="8fa60-441">呼叫 `CreateDefaultBuilder` 時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="8fa60-441">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="8fa60-442">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="8fa60-442">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8fa60-443">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="8fa60-443">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="8fa60-444">**範例**</span><span class="sxs-lookup"><span data-stu-id="8fa60-444">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8fa60-445">2.x 範例應用程式可發揮靜態方便方法 `CreateDefaultBuilder` 的優勢以建置主機，這包括對 `AddJsonFile` 的兩次呼叫。</span><span class="sxs-lookup"><span data-stu-id="8fa60-445">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="8fa60-446">從 *appsettings.json* 與 *appsettings.{Environment}.json* 載入設定。</span><span class="sxs-lookup"><span data-stu-id="8fa60-446">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8fa60-447">1.x 範例應用程式會呼叫 `ConfigurationBuilder` 上的 `AddJsonFile` 兩次。</span><span class="sxs-lookup"><span data-stu-id="8fa60-447">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="8fa60-448">從 *appsettings.json* 與 *appsettings.{Environment}.json* 載入設定。</span><span class="sxs-lookup"><span data-stu-id="8fa60-448">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="8fa60-449">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="8fa60-449">Run the sample app.</span></span> <span data-ttu-id="8fa60-450">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="8fa60-450">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="8fa60-451">觀察輸出是否包含表格中所顯示之設定的機碼值組 (視環境而定)。</span><span class="sxs-lookup"><span data-stu-id="8fa60-451">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="8fa60-452">記錄設定機碼會使用冒號 (`:`) 做為階層式分隔符號。</span><span class="sxs-lookup"><span data-stu-id="8fa60-452">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="8fa60-453">Key</span><span class="sxs-lookup"><span data-stu-id="8fa60-453">Key</span></span>                        | <span data-ttu-id="8fa60-454">開發值</span><span class="sxs-lookup"><span data-stu-id="8fa60-454">Development Value</span></span> | <span data-ttu-id="8fa60-455">生產值</span><span class="sxs-lookup"><span data-stu-id="8fa60-455">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="8fa60-456">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="8fa60-456">Logging:LogLevel:System</span></span>    | <span data-ttu-id="8fa60-457">資訊</span><span class="sxs-lookup"><span data-stu-id="8fa60-457">Information</span></span>       | <span data-ttu-id="8fa60-458">資訊</span><span class="sxs-lookup"><span data-stu-id="8fa60-458">Information</span></span>      |
| <span data-ttu-id="8fa60-459">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="8fa60-459">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="8fa60-460">資訊</span><span class="sxs-lookup"><span data-stu-id="8fa60-460">Information</span></span>       | <span data-ttu-id="8fa60-461">資訊</span><span class="sxs-lookup"><span data-stu-id="8fa60-461">Information</span></span>      |
| <span data-ttu-id="8fa60-462">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="8fa60-462">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="8fa60-463">偵錯</span><span class="sxs-lookup"><span data-stu-id="8fa60-463">Debug</span></span>             | <span data-ttu-id="8fa60-464">錯誤</span><span class="sxs-lookup"><span data-stu-id="8fa60-464">Error</span></span>            |
| <span data-ttu-id="8fa60-465">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="8fa60-465">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="8fa60-466">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-466">XML Configuration Provider</span></span>

<span data-ttu-id="8fa60-467"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> 會在執行階段從 XML 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="8fa60-467">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="8fa60-468">若要啟用 XML 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="8fa60-468">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="8fa60-469">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="8fa60-469">Overloads permit specifying:</span></span>

* <span data-ttu-id="8fa60-470">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="8fa60-470">Whether the file is optional.</span></span>
* <span data-ttu-id="8fa60-471">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="8fa60-471">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="8fa60-472"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="8fa60-472">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="8fa60-473">建立設定機碼值組時，會忽略設定檔案的根節點。</span><span class="sxs-lookup"><span data-stu-id="8fa60-473">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="8fa60-474">請勿在檔案中指定文件類型定義 (DTD) 或命名空間。</span><span class="sxs-lookup"><span data-stu-id="8fa60-474">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8fa60-475">建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="8fa60-475">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="8fa60-476">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="8fa60-476">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8fa60-477">呼叫 `CreateDefaultBuilder` 時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="8fa60-477">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="8fa60-478">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="8fa60-478">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8fa60-479">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="8fa60-479">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="8fa60-480">XML 設定檔可以針對重複的區段使用相異元素名稱：</span><span class="sxs-lookup"><span data-stu-id="8fa60-480">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="8fa60-481">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="8fa60-481">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="8fa60-482">section0:key0</span><span class="sxs-lookup"><span data-stu-id="8fa60-482">section0:key0</span></span>
* <span data-ttu-id="8fa60-483">section0:key1</span><span class="sxs-lookup"><span data-stu-id="8fa60-483">section0:key1</span></span>
* <span data-ttu-id="8fa60-484">section1:key0</span><span class="sxs-lookup"><span data-stu-id="8fa60-484">section1:key0</span></span>
* <span data-ttu-id="8fa60-485">section1:key1</span><span class="sxs-lookup"><span data-stu-id="8fa60-485">section1:key1</span></span>

<span data-ttu-id="8fa60-486">若 `name` 屬性是用來區別元素，則可以重複那些使用相同元素名稱的元素：</span><span class="sxs-lookup"><span data-stu-id="8fa60-486">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="8fa60-487">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="8fa60-487">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="8fa60-488">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="8fa60-488">section:section0:key:key0</span></span>
* <span data-ttu-id="8fa60-489">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="8fa60-489">section:section0:key:key1</span></span>
* <span data-ttu-id="8fa60-490">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="8fa60-490">section:section1:key:key0</span></span>
* <span data-ttu-id="8fa60-491">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="8fa60-491">section:section1:key:key1</span></span>

<span data-ttu-id="8fa60-492">屬性可用來提供值：</span><span class="sxs-lookup"><span data-stu-id="8fa60-492">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="8fa60-493">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="8fa60-493">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="8fa60-494">key:attribute</span><span class="sxs-lookup"><span data-stu-id="8fa60-494">key:attribute</span></span>
* <span data-ttu-id="8fa60-495">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="8fa60-495">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="8fa60-496">每個檔案機碼設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-496">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="8fa60-497"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> 使用目錄的檔案做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="8fa60-497">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="8fa60-498">機碼是檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="8fa60-498">The key is the file name.</span></span> <span data-ttu-id="8fa60-499">值包含檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="8fa60-499">The value contains the file's contents.</span></span> <span data-ttu-id="8fa60-500">每個檔案機碼設定提供者是在 Docker 主機案例中使用。</span><span class="sxs-lookup"><span data-stu-id="8fa60-500">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="8fa60-501">若要啟用每個檔案機碼設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="8fa60-501">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="8fa60-502">檔案的 `directoryPath` 必須是絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="8fa60-502">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="8fa60-503">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="8fa60-503">Overloads permit specifying:</span></span>

* <span data-ttu-id="8fa60-504">設定來源的 `Action<KeyPerFileConfigurationSource>` 委派。</span><span class="sxs-lookup"><span data-stu-id="8fa60-504">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="8fa60-505">目錄是否為選擇性與目錄的路徑。</span><span class="sxs-lookup"><span data-stu-id="8fa60-505">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="8fa60-506">建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="8fa60-506">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="8fa60-507">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="8fa60-507">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="8fa60-508">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-508">Memory Configuration Provider</span></span>

<span data-ttu-id="8fa60-509"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> 使用記憶體內集合做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="8fa60-509">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="8fa60-510">若要啟用記憶體內集合設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="8fa60-510">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="8fa60-511">設定提供者可以使用 `IEnumerable<KeyValuePair<String,String>>` 來初始化。</span><span class="sxs-lookup"><span data-stu-id="8fa60-511">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8fa60-512">建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="8fa60-512">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="8fa60-513">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="8fa60-513">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8fa60-514">呼叫 `CreateDefaultBuilder` 時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="8fa60-514">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="8fa60-515">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="8fa60-515">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8fa60-516">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="8fa60-516">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="8fa60-517">GetValue</span><span class="sxs-lookup"><span data-stu-id="8fa60-517">GetValue</span></span>

<span data-ttu-id="8fa60-518">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) 會從具有所指定機碼的設定擷取值，並將它轉換為指定的型別。</span><span class="sxs-lookup"><span data-stu-id="8fa60-518">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="8fa60-519">若找不到機碼，多載允許您提供預設值。</span><span class="sxs-lookup"><span data-stu-id="8fa60-519">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="8fa60-520">下列範例會從具有機碼 `NumberKey` 的設定擷取字串值、將值的型別設定為 `int`，並將值存放在變數 `intValue` 中。</span><span class="sxs-lookup"><span data-stu-id="8fa60-520">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="8fa60-521">若在設定機碼中找不到 `NumberKey`，`intValue` 會接收 `99` 的預設值：</span><span class="sxs-lookup"><span data-stu-id="8fa60-521">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="8fa60-522">GetSection、GetChildren 與 Exists</span><span class="sxs-lookup"><span data-stu-id="8fa60-522">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="8fa60-523">針對下面的範例，請考慮下列 JSON 檔案。</span><span class="sxs-lookup"><span data-stu-id="8fa60-523">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="8fa60-524">在兩個區段中找到四個機碼，其中一個包括子區段組：</span><span class="sxs-lookup"><span data-stu-id="8fa60-524">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="8fa60-525">當檔案被讀入到設定之後，會建立下列唯一階層式機碼以存放設定值：</span><span class="sxs-lookup"><span data-stu-id="8fa60-525">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="8fa60-526">section0:key0</span><span class="sxs-lookup"><span data-stu-id="8fa60-526">section0:key0</span></span>
* <span data-ttu-id="8fa60-527">section0:key1</span><span class="sxs-lookup"><span data-stu-id="8fa60-527">section0:key1</span></span>
* <span data-ttu-id="8fa60-528">section1:key0</span><span class="sxs-lookup"><span data-stu-id="8fa60-528">section1:key0</span></span>
* <span data-ttu-id="8fa60-529">section1:key1</span><span class="sxs-lookup"><span data-stu-id="8fa60-529">section1:key1</span></span>
* <span data-ttu-id="8fa60-530">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="8fa60-530">section2:subsection0:key0</span></span>
* <span data-ttu-id="8fa60-531">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="8fa60-531">section2:subsection0:key1</span></span>
* <span data-ttu-id="8fa60-532">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="8fa60-532">section2:subsection1:key0</span></span>
* <span data-ttu-id="8fa60-533">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="8fa60-533">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="8fa60-534">GetSection</span><span class="sxs-lookup"><span data-stu-id="8fa60-534">GetSection</span></span>

<span data-ttu-id="8fa60-535">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) 會擷取具有所指定子區段機碼的設定子區段。</span><span class="sxs-lookup"><span data-stu-id="8fa60-535">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="8fa60-536">若要傳回 `section1` 中只包含一個機碼值組的 <xref:Microsoft.Extensions.Configuration.IConfigurationSection>，請呼叫 `GetSection` 並提供區段名稱：</span><span class="sxs-lookup"><span data-stu-id="8fa60-536">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="8fa60-537">同樣地，若要取得 `section2:subsection0` 中之機碼的值，請呼叫 `GetSection` 並提供區段路徑：</span><span class="sxs-lookup"><span data-stu-id="8fa60-537">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="8fa60-538">`GetSection` 絕不會傳回 `null`。</span><span class="sxs-lookup"><span data-stu-id="8fa60-538">`GetSection` never returns `null`.</span></span> <span data-ttu-id="8fa60-539">若找不到相符的區段，會傳回空的 `IConfigurationSection`。</span><span class="sxs-lookup"><span data-stu-id="8fa60-539">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

### <a name="getchildren"></a><span data-ttu-id="8fa60-540">GetChildren</span><span class="sxs-lookup"><span data-stu-id="8fa60-540">GetChildren</span></span>

<span data-ttu-id="8fa60-541">對 `section2` 上之 [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) 的呼叫會取得包括下列項目的 `IEnumerable<IConfigurationSection>`：</span><span class="sxs-lookup"><span data-stu-id="8fa60-541">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="8fa60-542">存在</span><span class="sxs-lookup"><span data-stu-id="8fa60-542">Exists</span></span>

<span data-ttu-id="8fa60-543">使用 [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) 來判斷設定區段是否存在：</span><span class="sxs-lookup"><span data-stu-id="8fa60-543">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="8fa60-544">以範例資料為例， `sectionExists` 是 `false`，因未設定資料中沒有 `section2:subsection2` 區段。</span><span class="sxs-lookup"><span data-stu-id="8fa60-544">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="8fa60-545">繫結到類別</span><span class="sxs-lookup"><span data-stu-id="8fa60-545">Bind to a class</span></span>

<span data-ttu-id="8fa60-546">設定可以繫結到類別，以使用*選項模式*代表相關設定的群組。</span><span class="sxs-lookup"><span data-stu-id="8fa60-546">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="8fa60-547">如需詳細資訊，請參閱<xref:fundamentals/configuration/options>。</span><span class="sxs-lookup"><span data-stu-id="8fa60-547">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="8fa60-548">設定值是以字串傳回，但是呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 會啟用 [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) 物件的建構。</span><span class="sxs-lookup"><span data-stu-id="8fa60-548">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="8fa60-549">範例應用程式包含 `Starship` 模型 (*Models/Starship.cs*)：</span><span class="sxs-lookup"><span data-stu-id="8fa60-549">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="8fa60-550">當範例應用程式使用 JSON 設定提供者來載入設定時，*starship.json* 檔案的 `starship` 區段會建立設定：</span><span class="sxs-lookup"><span data-stu-id="8fa60-550">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="8fa60-551">會建立下列設定機碼值組：</span><span class="sxs-lookup"><span data-stu-id="8fa60-551">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="8fa60-552">Key</span><span class="sxs-lookup"><span data-stu-id="8fa60-552">Key</span></span>                   | <span data-ttu-id="8fa60-553">值</span><span class="sxs-lookup"><span data-stu-id="8fa60-553">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="8fa60-554">starship:name</span><span class="sxs-lookup"><span data-stu-id="8fa60-554">starship:name</span></span>         | <span data-ttu-id="8fa60-555">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="8fa60-555">USS Enterprise</span></span>                                    |
| <span data-ttu-id="8fa60-556">starship:registry</span><span class="sxs-lookup"><span data-stu-id="8fa60-556">starship:registry</span></span>     | <span data-ttu-id="8fa60-557">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="8fa60-557">NCC-1701</span></span>                                          |
| <span data-ttu-id="8fa60-558">starship:class</span><span class="sxs-lookup"><span data-stu-id="8fa60-558">starship:class</span></span>        | <span data-ttu-id="8fa60-559">Constitution</span><span class="sxs-lookup"><span data-stu-id="8fa60-559">Constitution</span></span>                                      |
| <span data-ttu-id="8fa60-560">starship:length</span><span class="sxs-lookup"><span data-stu-id="8fa60-560">starship:length</span></span>       | <span data-ttu-id="8fa60-561">304.8</span><span class="sxs-lookup"><span data-stu-id="8fa60-561">304.8</span></span>                                             |
| <span data-ttu-id="8fa60-562">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="8fa60-562">starship:commissioned</span></span> | <span data-ttu-id="8fa60-563">False</span><span class="sxs-lookup"><span data-stu-id="8fa60-563">False</span></span>                                             |
| <span data-ttu-id="8fa60-564">trademark</span><span class="sxs-lookup"><span data-stu-id="8fa60-564">trademark</span></span>             | <span data-ttu-id="8fa60-565">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="8fa60-565">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="8fa60-566">範例應用程式使用 `starship` 機碼呼叫 `GetSection`。</span><span class="sxs-lookup"><span data-stu-id="8fa60-566">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="8fa60-567">`starship` 機碼值組會被隔離。</span><span class="sxs-lookup"><span data-stu-id="8fa60-567">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="8fa60-568">會在子區段上呼叫 `Bind` 方法，並傳入 `Starship` 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="8fa60-568">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="8fa60-569">繫結執行個體值之後，執行個體會被指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="8fa60-569">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="8fa60-570">繫結至物件圖形</span><span class="sxs-lookup"><span data-stu-id="8fa60-570">Bind to an object graph</span></span>

<span data-ttu-id="8fa60-571"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 可以繫結整個 POCO 物件圖形。</span><span class="sxs-lookup"><span data-stu-id="8fa60-571"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="8fa60-572">範例包含 `TvShow` 模型，其物件圖形包括 `Metadata` 與 `Actors` 類別 (*Models/TvShow.cs*)：</span><span class="sxs-lookup"><span data-stu-id="8fa60-572">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="8fa60-573">範例應用程式有 *tvshow.xml* 檔案，其中包含設定資料：</span><span class="sxs-lookup"><span data-stu-id="8fa60-573">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="8fa60-574">設定會被繫結到整個 `TvShow` 物件 (使用 `Bind` 方法)。</span><span class="sxs-lookup"><span data-stu-id="8fa60-574">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="8fa60-575">已繫結的執行個體會被指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="8fa60-575">The bound instance is assigned to a property for rendering:</span></span>

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

<span data-ttu-id="8fa60-576">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 會繫結並傳回所指定型別。</span><span class="sxs-lookup"><span data-stu-id="8fa60-576">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="8fa60-577">`Get<T>` 比使用 `Bind` 更方便。</span><span class="sxs-lookup"><span data-stu-id="8fa60-577">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="8fa60-578">下列程式碼顯示如何根據上面的範例使用 `Get<T>`，這可讓您直接將已繫結的執行個體指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="8fa60-578">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="8fa60-579">將陣列繫結到類別</span><span class="sxs-lookup"><span data-stu-id="8fa60-579">Bind an array to a class</span></span>

<span data-ttu-id="8fa60-580">範例應用程式示範此節中解釋的概念。</span><span class="sxs-lookup"><span data-stu-id="8fa60-580">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="8fa60-581"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="8fa60-581">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="8fa60-582">任何公開數值機碼區段 (`:0:`、`:1:`、&hellip; `:{n}:`) 的陣列格式都能繫結到POCO 類別陣列。</span><span class="sxs-lookup"><span data-stu-id="8fa60-582">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="8fa60-583">繫結是由慣例提供。</span><span class="sxs-lookup"><span data-stu-id="8fa60-583">Binding is provided by convention.</span></span> <span data-ttu-id="8fa60-584">自訂設定提供者不需要實作陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="8fa60-584">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="8fa60-585">**記憶體內陣列處理**</span><span class="sxs-lookup"><span data-stu-id="8fa60-585">**In-memory array processing**</span></span>

<span data-ttu-id="8fa60-586">考慮下表中顯示的設定機碼與值。</span><span class="sxs-lookup"><span data-stu-id="8fa60-586">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="8fa60-587">Key</span><span class="sxs-lookup"><span data-stu-id="8fa60-587">Key</span></span>             | <span data-ttu-id="8fa60-588">值</span><span class="sxs-lookup"><span data-stu-id="8fa60-588">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="8fa60-589">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="8fa60-589">array:entries:0</span></span> | <span data-ttu-id="8fa60-590">value0</span><span class="sxs-lookup"><span data-stu-id="8fa60-590">value0</span></span> |
| <span data-ttu-id="8fa60-591">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="8fa60-591">array:entries:1</span></span> | <span data-ttu-id="8fa60-592">value1</span><span class="sxs-lookup"><span data-stu-id="8fa60-592">value1</span></span> |
| <span data-ttu-id="8fa60-593">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="8fa60-593">array:entries:2</span></span> | <span data-ttu-id="8fa60-594">value2</span><span class="sxs-lookup"><span data-stu-id="8fa60-594">value2</span></span> |
| <span data-ttu-id="8fa60-595">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="8fa60-595">array:entries:4</span></span> | <span data-ttu-id="8fa60-596">value4</span><span class="sxs-lookup"><span data-stu-id="8fa60-596">value4</span></span> |
| <span data-ttu-id="8fa60-597">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="8fa60-597">array:entries:5</span></span> | <span data-ttu-id="8fa60-598">value5</span><span class="sxs-lookup"><span data-stu-id="8fa60-598">value5</span></span> |

<span data-ttu-id="8fa60-599">這些機碼與值是使用「記憶體設定提供者」在範例應用程式中載入：</span><span class="sxs-lookup"><span data-stu-id="8fa60-599">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="8fa60-600">陣列會跳過索引 &num;3 的值。</span><span class="sxs-lookup"><span data-stu-id="8fa60-600">The array skips a value for index &num;3.</span></span> <span data-ttu-id="8fa60-601">設定繫結程式無法繫結 Null 值或在已繫結的物件中建立 Null 項目，這在示範繫結此陣列的結果到某物件的時候已經很清楚。</span><span class="sxs-lookup"><span data-stu-id="8fa60-601">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="8fa60-602">在範例應用程式中，POCO 類別可用來存放繫結設定資料：</span><span class="sxs-lookup"><span data-stu-id="8fa60-602">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="8fa60-603">設定資料已繫結到物件：</span><span class="sxs-lookup"><span data-stu-id="8fa60-603">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="8fa60-604">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 語法也可以使用，這會產生更精簡的程式碼：</span><span class="sxs-lookup"><span data-stu-id="8fa60-604">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="8fa60-605">繫結物件 (`ArrayExample`的執行個體) 會從設定接收陣列資料。</span><span class="sxs-lookup"><span data-stu-id="8fa60-605">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="8fa60-606">`ArrayExample.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="8fa60-606">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="8fa60-607">`ArrayExample.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="8fa60-607">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="8fa60-608">0</span><span class="sxs-lookup"><span data-stu-id="8fa60-608">0</span></span>                            | <span data-ttu-id="8fa60-609">value0</span><span class="sxs-lookup"><span data-stu-id="8fa60-609">value0</span></span>                       |
| <span data-ttu-id="8fa60-610">1</span><span class="sxs-lookup"><span data-stu-id="8fa60-610">1</span></span>                            | <span data-ttu-id="8fa60-611">value1</span><span class="sxs-lookup"><span data-stu-id="8fa60-611">value1</span></span>                       |
| <span data-ttu-id="8fa60-612">2</span><span class="sxs-lookup"><span data-stu-id="8fa60-612">2</span></span>                            | <span data-ttu-id="8fa60-613">value2</span><span class="sxs-lookup"><span data-stu-id="8fa60-613">value2</span></span>                       |
| <span data-ttu-id="8fa60-614">3</span><span class="sxs-lookup"><span data-stu-id="8fa60-614">3</span></span>                            | <span data-ttu-id="8fa60-615">value4</span><span class="sxs-lookup"><span data-stu-id="8fa60-615">value4</span></span>                       |
| <span data-ttu-id="8fa60-616">4</span><span class="sxs-lookup"><span data-stu-id="8fa60-616">4</span></span>                            | <span data-ttu-id="8fa60-617">value5</span><span class="sxs-lookup"><span data-stu-id="8fa60-617">value5</span></span>                       |

<span data-ttu-id="8fa60-618">繫結物件中的索引 &num;3 存放 `array:4` 設定機碼與其 `value4` 的設定資料。</span><span class="sxs-lookup"><span data-stu-id="8fa60-618">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="8fa60-619">當繫結包含陣列的設定資料時，設定機碼中的陣列索引只會用來列舉設定資料 (當建立物件時)。</span><span class="sxs-lookup"><span data-stu-id="8fa60-619">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="8fa60-620">設定資料中不能保留 Null 值，當設定機碼中的陣列略過一或多個索引時，不會在繫結物件中建立 Null 值項目。</span><span class="sxs-lookup"><span data-stu-id="8fa60-620">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="8fa60-621">由會在設定中產生正確機碼值組的任何設定提供者繫結到 `ArrayExample` 執行個體之前，可以提供索引 &num;3 缺少的設定項目。</span><span class="sxs-lookup"><span data-stu-id="8fa60-621">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="8fa60-622">若範例包含具有缺少之機碼值組的額外 JSON 設定提供者，`ArrayExample.Entries` 會符合完整的設定陣列：</span><span class="sxs-lookup"><span data-stu-id="8fa60-622">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="8fa60-623">*missing_value.json*：</span><span class="sxs-lookup"><span data-stu-id="8fa60-623">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8fa60-624">在 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>中：</span><span class="sxs-lookup"><span data-stu-id="8fa60-624">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8fa60-625">在 `Startup` 建構函式中：</span><span class="sxs-lookup"><span data-stu-id="8fa60-625">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="8fa60-626">表格中顯示的機碼值組會載入到設定中。</span><span class="sxs-lookup"><span data-stu-id="8fa60-626">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="8fa60-627">Key</span><span class="sxs-lookup"><span data-stu-id="8fa60-627">Key</span></span>             | <span data-ttu-id="8fa60-628">值</span><span class="sxs-lookup"><span data-stu-id="8fa60-628">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="8fa60-629">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="8fa60-629">array:entries:3</span></span> | <span data-ttu-id="8fa60-630">value3</span><span class="sxs-lookup"><span data-stu-id="8fa60-630">value3</span></span> |

<span data-ttu-id="8fa60-631">在「JSON 設定提供者」包含索引 &num;3 的項目之後，若繫結 `ArrayExample` 類別執行個體，`ArrayExample.Entries` 陣列會包括這些值：</span><span class="sxs-lookup"><span data-stu-id="8fa60-631">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="8fa60-632">`ArrayExample.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="8fa60-632">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="8fa60-633">`ArrayExample.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="8fa60-633">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="8fa60-634">0</span><span class="sxs-lookup"><span data-stu-id="8fa60-634">0</span></span>                            | <span data-ttu-id="8fa60-635">value0</span><span class="sxs-lookup"><span data-stu-id="8fa60-635">value0</span></span>                       |
| <span data-ttu-id="8fa60-636">1</span><span class="sxs-lookup"><span data-stu-id="8fa60-636">1</span></span>                            | <span data-ttu-id="8fa60-637">value1</span><span class="sxs-lookup"><span data-stu-id="8fa60-637">value1</span></span>                       |
| <span data-ttu-id="8fa60-638">2</span><span class="sxs-lookup"><span data-stu-id="8fa60-638">2</span></span>                            | <span data-ttu-id="8fa60-639">value2</span><span class="sxs-lookup"><span data-stu-id="8fa60-639">value2</span></span>                       |
| <span data-ttu-id="8fa60-640">3</span><span class="sxs-lookup"><span data-stu-id="8fa60-640">3</span></span>                            | <span data-ttu-id="8fa60-641">value3</span><span class="sxs-lookup"><span data-stu-id="8fa60-641">value3</span></span>                       |
| <span data-ttu-id="8fa60-642">4</span><span class="sxs-lookup"><span data-stu-id="8fa60-642">4</span></span>                            | <span data-ttu-id="8fa60-643">value4</span><span class="sxs-lookup"><span data-stu-id="8fa60-643">value4</span></span>                       |
| <span data-ttu-id="8fa60-644">5</span><span class="sxs-lookup"><span data-stu-id="8fa60-644">5</span></span>                            | <span data-ttu-id="8fa60-645">value5</span><span class="sxs-lookup"><span data-stu-id="8fa60-645">value5</span></span>                       |

<span data-ttu-id="8fa60-646">**JSON 陣列處理**</span><span class="sxs-lookup"><span data-stu-id="8fa60-646">**JSON array processing**</span></span>

<span data-ttu-id="8fa60-647">若 JSON 檔案包含陣列，會為具有以零為基礎之區段索引的陣列元素建立設定機碼。</span><span class="sxs-lookup"><span data-stu-id="8fa60-647">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="8fa60-648">在下列設定檔中，`subsection` 是陣列：</span><span class="sxs-lookup"><span data-stu-id="8fa60-648">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="8fa60-649">「JSON 設定提供者」會將設定資料讀入到下列機碼值組：</span><span class="sxs-lookup"><span data-stu-id="8fa60-649">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="8fa60-650">Key</span><span class="sxs-lookup"><span data-stu-id="8fa60-650">Key</span></span>                     | <span data-ttu-id="8fa60-651">值</span><span class="sxs-lookup"><span data-stu-id="8fa60-651">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="8fa60-652">json_array:key</span><span class="sxs-lookup"><span data-stu-id="8fa60-652">json_array:key</span></span>          | <span data-ttu-id="8fa60-653">valueA</span><span class="sxs-lookup"><span data-stu-id="8fa60-653">valueA</span></span> |
| <span data-ttu-id="8fa60-654">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="8fa60-654">json_array:subsection:0</span></span> | <span data-ttu-id="8fa60-655">valueB</span><span class="sxs-lookup"><span data-stu-id="8fa60-655">valueB</span></span> |
| <span data-ttu-id="8fa60-656">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="8fa60-656">json_array:subsection:1</span></span> | <span data-ttu-id="8fa60-657">valueC</span><span class="sxs-lookup"><span data-stu-id="8fa60-657">valueC</span></span> |
| <span data-ttu-id="8fa60-658">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="8fa60-658">json_array:subsection:2</span></span> | <span data-ttu-id="8fa60-659">valueD</span><span class="sxs-lookup"><span data-stu-id="8fa60-659">valueD</span></span> |

<span data-ttu-id="8fa60-660">在範例應用程式中，下列 POCO 類別可用來繫結設定機碼值組：</span><span class="sxs-lookup"><span data-stu-id="8fa60-660">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="8fa60-661">在繫結之後，`JsonArrayExample.Key` 會存放值 `valueA`。</span><span class="sxs-lookup"><span data-stu-id="8fa60-661">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="8fa60-662">子區段值會存放在 POCO 陣列屬性 `Subsection` 中。</span><span class="sxs-lookup"><span data-stu-id="8fa60-662">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="8fa60-663">`JsonArrayExample.Subsection` 索引</span><span class="sxs-lookup"><span data-stu-id="8fa60-663">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="8fa60-664">`JsonArrayExample.Subsection` 值</span><span class="sxs-lookup"><span data-stu-id="8fa60-664">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="8fa60-665">0</span><span class="sxs-lookup"><span data-stu-id="8fa60-665">0</span></span>                                   | <span data-ttu-id="8fa60-666">valueB</span><span class="sxs-lookup"><span data-stu-id="8fa60-666">valueB</span></span>                              |
| <span data-ttu-id="8fa60-667">1</span><span class="sxs-lookup"><span data-stu-id="8fa60-667">1</span></span>                                   | <span data-ttu-id="8fa60-668">valueC</span><span class="sxs-lookup"><span data-stu-id="8fa60-668">valueC</span></span>                              |
| <span data-ttu-id="8fa60-669">2</span><span class="sxs-lookup"><span data-stu-id="8fa60-669">2</span></span>                                   | <span data-ttu-id="8fa60-670">valueD</span><span class="sxs-lookup"><span data-stu-id="8fa60-670">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="8fa60-671">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="8fa60-671">Custom configuration provider</span></span>

<span data-ttu-id="8fa60-672">範例應用程式示範如何建立會使用 [Entity Framework (EF)](/ef/core/) 從資料庫讀取設定機碼值組的基本設定提供者。</span><span class="sxs-lookup"><span data-stu-id="8fa60-672">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="8fa60-673">提供者具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="8fa60-673">The provider has the following characteristics:</span></span>

* <span data-ttu-id="8fa60-674">EF 記憶體內資料庫會用於示範用途。</span><span class="sxs-lookup"><span data-stu-id="8fa60-674">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="8fa60-675">若要使用需要連接字串的資料庫，請實作第二個 `ConfigurationBuilder` 以從另一個設定提供者提供連接字串。</span><span class="sxs-lookup"><span data-stu-id="8fa60-675">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="8fa60-676">啟動時，該提供者會將資料庫資料表讀入到設定中。</span><span class="sxs-lookup"><span data-stu-id="8fa60-676">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="8fa60-677">該提供者不會以個別機碼為基礎查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="8fa60-677">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="8fa60-678">未實作變更時重新載入，因此在應用程式啟動後更新資料庫對應用程式設定沒有影響。</span><span class="sxs-lookup"><span data-stu-id="8fa60-678">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="8fa60-679">定義 `EFConfigurationValue` 實體來在資料庫中存放設定值。</span><span class="sxs-lookup"><span data-stu-id="8fa60-679">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="8fa60-680">*Models/EFConfigurationValue.cs*：</span><span class="sxs-lookup"><span data-stu-id="8fa60-680">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="8fa60-681">新增 `EFConfigurationContext` 以存放及存取已設定的值。</span><span class="sxs-lookup"><span data-stu-id="8fa60-681">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="8fa60-682">*EFConfigurationProvider/EFConfigurationContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="8fa60-682">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="8fa60-683">建立會實作 <xref:Microsoft.Extensions.Configuration.IConfigurationSource> 的類別。</span><span class="sxs-lookup"><span data-stu-id="8fa60-683">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="8fa60-684">*EFConfigurationProvider/EFConfigurationSource.cs*：</span><span class="sxs-lookup"><span data-stu-id="8fa60-684">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="8fa60-685">透過繼承自 <xref:Microsoft.Extensions.Configuration.ConfigurationProvider> 來建立自訂設定提供者。</span><span class="sxs-lookup"><span data-stu-id="8fa60-685">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="8fa60-686">若資料庫是空的，設定提供者會初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="8fa60-686">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="8fa60-687">*EFConfigurationProvider/EFConfigurationProvider.cs*：</span><span class="sxs-lookup"><span data-stu-id="8fa60-687">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="8fa60-688">`AddEFConfiguration` 擴充方法允許新增設定來源到 `ConfigurationBuilder`。</span><span class="sxs-lookup"><span data-stu-id="8fa60-688">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="8fa60-689">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="8fa60-689">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="8fa60-690">下列程式碼顯示如何在 *Program.cs* 中使用自訂 `EFConfigurationProvider`：</span><span class="sxs-lookup"><span data-stu-id="8fa60-690">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="8fa60-691">在啟動期間存取組態</span><span class="sxs-lookup"><span data-stu-id="8fa60-691">Access configuration during startup</span></span>

<span data-ttu-id="8fa60-692">將 `IConfiguration` 插入到 `Startup` 建構函式，以存取 `Startup.ConfigureServices` 中的設定值。</span><span class="sxs-lookup"><span data-stu-id="8fa60-692">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8fa60-693">若要存取 `Startup.Configure` 中的設定，請直接將 `IConfiguration` 插入到方法或從建構函式使用執行個體：</span><span class="sxs-lookup"><span data-stu-id="8fa60-693">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="8fa60-694">如需使用啟動方便方法來存取設定的範例，請參閱[應用程式啟動：方便方法](xref:fundamentals/startup#convenience-methods)。</span><span class="sxs-lookup"><span data-stu-id="8fa60-694">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="8fa60-695">存取 Razor Pages 頁面或 MVC 檢視中的設定</span><span class="sxs-lookup"><span data-stu-id="8fa60-695">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="8fa60-696">若要在 [Razor Pages 頁面] 頁面或 MVC 檢視中存取組態設定，請針對 [Microsoft.Extensions.Configuration 命名空間](xref:Microsoft.Extensions.Configuration) [using 指示詞](xref:mvc/views/razor#using) ([C# 參考：using 指示詞](/dotnet/csharp/language-reference/keywords/using-directive))，並將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 插入到頁面或檢視中。</span><span class="sxs-lookup"><span data-stu-id="8fa60-696">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="8fa60-697">在 [Razor 頁面] 頁面中：</span><span class="sxs-lookup"><span data-stu-id="8fa60-697">In a Razor Pages page:</span></span>

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

<span data-ttu-id="8fa60-698">在 MVC 檢視中：</span><span class="sxs-lookup"><span data-stu-id="8fa60-698">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="8fa60-699">從外部組件新增設定</span><span class="sxs-lookup"><span data-stu-id="8fa60-699">Add configuration from an external assembly</span></span>

<span data-ttu-id="8fa60-700"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。</span><span class="sxs-lookup"><span data-stu-id="8fa60-700">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="8fa60-701">如需詳細資訊，請參閱<xref:fundamentals/configuration/platform-specific-configuration>。</span><span class="sxs-lookup"><span data-stu-id="8fa60-701">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8fa60-702">其他資源</span><span class="sxs-lookup"><span data-stu-id="8fa60-702">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="8fa60-703">深入了解 Microsoft 設定</span><span class="sxs-lookup"><span data-stu-id="8fa60-703">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
