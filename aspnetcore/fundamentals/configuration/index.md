---
title: ASP.NET Core 的設定
author: guardrex
description: 了解如何使用組態 API 設定 ASP.NET Core 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 11/15/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 766ac77a2af01509f8e4bc646a18f7dfbc923511
ms.sourcegitcommit: d3392f688cfebc1f25616da7489664d69c6ee330
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/16/2018
ms.locfileid: "51818391"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="f949f-103">ASP.NET Core 的設定</span><span class="sxs-lookup"><span data-stu-id="f949f-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="f949f-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f949f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f949f-105">ASP.NET Core 中的應用程式設定是以由*設定提供者*所建立的機碼值組為基礎。</span><span class="sxs-lookup"><span data-stu-id="f949f-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="f949f-106">設定提供者會從各種設定來源將設定資料讀取到機碼值組中：</span><span class="sxs-lookup"><span data-stu-id="f949f-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="f949f-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="f949f-107">Azure Key Vault</span></span>
* <span data-ttu-id="f949f-108">命令列引數</span><span class="sxs-lookup"><span data-stu-id="f949f-108">Command-line arguments</span></span>
* <span data-ttu-id="f949f-109">自訂提供者 (已安裝或已建立)</span><span class="sxs-lookup"><span data-stu-id="f949f-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="f949f-110">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="f949f-110">Directory files</span></span>
* <span data-ttu-id="f949f-111">環境變數</span><span class="sxs-lookup"><span data-stu-id="f949f-111">Environment variables</span></span>
* <span data-ttu-id="f949f-112">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="f949f-112">In-memory .NET objects</span></span>
* <span data-ttu-id="f949f-113">設定檔</span><span class="sxs-lookup"><span data-stu-id="f949f-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="f949f-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="f949f-114">Azure Key Vault</span></span>
* <span data-ttu-id="f949f-115">命令列引數</span><span class="sxs-lookup"><span data-stu-id="f949f-115">Command-line arguments</span></span>
* <span data-ttu-id="f949f-116">自訂提供者 (已安裝或已建立)</span><span class="sxs-lookup"><span data-stu-id="f949f-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="f949f-117">環境變數</span><span class="sxs-lookup"><span data-stu-id="f949f-117">Environment variables</span></span>
* <span data-ttu-id="f949f-118">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="f949f-118">In-memory .NET objects</span></span>
* <span data-ttu-id="f949f-119">設定檔</span><span class="sxs-lookup"><span data-stu-id="f949f-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="f949f-120">命令列引數</span><span class="sxs-lookup"><span data-stu-id="f949f-120">Command-line arguments</span></span>
* <span data-ttu-id="f949f-121">自訂提供者 (已安裝或已建立)</span><span class="sxs-lookup"><span data-stu-id="f949f-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="f949f-122">環境變數</span><span class="sxs-lookup"><span data-stu-id="f949f-122">Environment variables</span></span>
* <span data-ttu-id="f949f-123">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="f949f-123">In-memory .NET objects</span></span>
* <span data-ttu-id="f949f-124">設定檔</span><span class="sxs-lookup"><span data-stu-id="f949f-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="f949f-125">*選項模式*是此主題中所述之設定概念的延伸。</span><span class="sxs-lookup"><span data-stu-id="f949f-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="f949f-126">選項使用類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="f949f-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="f949f-127">如需使用選項模式的詳細資訊，請參閱 <xref:fundamentals/configuration/options>。</span><span class="sxs-lookup"><span data-stu-id="f949f-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="f949f-128">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f949f-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f949f-129">此主題中提供的範例需要：</span><span class="sxs-lookup"><span data-stu-id="f949f-129">The examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="f949f-130">使用 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 設定應用程式的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="f949f-130">Setting the base path of the app with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="f949f-131">參考 [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) 套件即可讓應用程式使用 `SetBasePath`。</span><span class="sxs-lookup"><span data-stu-id="f949f-131">`SetBasePath` is made available to an app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="f949f-132">使用 <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> 解析設定檔的區段。</span><span class="sxs-lookup"><span data-stu-id="f949f-132">Resolving sections of configuration files with <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span></span> <span data-ttu-id="f949f-133">參考 [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) 套件即可讓應用程式使用 `GetSection`。</span><span class="sxs-lookup"><span data-stu-id="f949f-133">`GetSection` is made available to an app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="f949f-134">使用 <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 與 [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 將設定繫結到 .NET 類別。</span><span class="sxs-lookup"><span data-stu-id="f949f-134">Binding configuration to .NET classes with <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> and [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span></span> <span data-ttu-id="f949f-135">參考 [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) 套件即可讓應用程式使用 `Bind` 與 `Get<T>`。</span><span class="sxs-lookup"><span data-stu-id="f949f-135">`Bind` and `Get<T>` are made available to an app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span> <span data-ttu-id="f949f-136">ASP.NET Core 1.1 或更新版本中提供了 `Get<T>`。</span><span class="sxs-lookup"><span data-stu-id="f949f-136">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f949f-137">這些套件均包含在 [Microsoft.AspNetCore.App j5/ 中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="f949f-137">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f949f-138">這三個套件都包含在 [Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)中。</span><span class="sxs-lookup"><span data-stu-id="f949f-138">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="f949f-139">主機與應用程式設定的比較</span><span class="sxs-lookup"><span data-stu-id="f949f-139">Host vs. app configuration</span></span>

<span data-ttu-id="f949f-140">設定及啟動應用程式之前，會先設定及啟動「主機」。</span><span class="sxs-lookup"><span data-stu-id="f949f-140">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="f949f-141">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="f949f-141">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="f949f-142">應用程式與主機都是使用此主題中所述的設定提供者來設定的。</span><span class="sxs-lookup"><span data-stu-id="f949f-142">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="f949f-143">主機設定機碼值組會成為應用程式全域設定的一部分。</span><span class="sxs-lookup"><span data-stu-id="f949f-143">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="f949f-144">如需有關當建置主機時如何使用設定提供者的詳細資訊，以及設定來源如何影響主機設定的詳細資訊，請參閱 <xref:fundamentals/host/index>。</span><span class="sxs-lookup"><span data-stu-id="f949f-144">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="security"></a><span data-ttu-id="f949f-145">安全性</span><span class="sxs-lookup"><span data-stu-id="f949f-145">Security</span></span>

<span data-ttu-id="f949f-146">採用下列最佳做法：</span><span class="sxs-lookup"><span data-stu-id="f949f-146">Adopt the following best practices:</span></span>

* <span data-ttu-id="f949f-147">永遠不要將密碼或其他敏感性資料儲存在設定提供者程式碼或純文字設定檔中。</span><span class="sxs-lookup"><span data-stu-id="f949f-147">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="f949f-148">不要在開發或測試環境中使用生產環境祕密。</span><span class="sxs-lookup"><span data-stu-id="f949f-148">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="f949f-149">請在專案外部指定祕密，以防止其意外認可至開放原始碼存放庫。</span><span class="sxs-lookup"><span data-stu-id="f949f-149">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="f949f-150">深入了解[如何使用多個環境](xref:fundamentals/environments)及[使用祕密管理員管理開發中的應用程式祕密安全儲存體](xref:security/app-secrets) (包括有關使用環境變數來存放敏感性資料的建議)。</span><span class="sxs-lookup"><span data-stu-id="f949f-150">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="f949f-151">「祕密管理員」使用「檔案設定提供者」以 JSON 檔案在本機系統上存放使用者祕密。</span><span class="sxs-lookup"><span data-stu-id="f949f-151">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="f949f-152">此主題稍後將說明「檔案設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="f949f-152">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="f949f-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 是安全儲存應用程式祕密的一個選項。</span><span class="sxs-lookup"><span data-stu-id="f949f-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="f949f-154">如需詳細資訊，請參閱<xref:security/key-vault-configuration>。</span><span class="sxs-lookup"><span data-stu-id="f949f-154">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="f949f-155">階層式設定資料</span><span class="sxs-lookup"><span data-stu-id="f949f-155">Hierarchical configuration data</span></span>

<span data-ttu-id="f949f-156">設定 API 可在設定機碼中使用分隔符號來壓平合併階層式資料，以管理階層式設定資料。</span><span class="sxs-lookup"><span data-stu-id="f949f-156">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="f949f-157">在下列 JSON 檔案中，兩個區段的結構式階層中有四個機碼存在：</span><span class="sxs-lookup"><span data-stu-id="f949f-157">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="f949f-158">當該檔案被讀入設定之後，會建立唯一機碼來維護設定來源的原始階層式資料結構。</span><span class="sxs-lookup"><span data-stu-id="f949f-158">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="f949f-159">系統會使用冒號 (`:`) 將區段與機碼壓平合併以維護原始結構：</span><span class="sxs-lookup"><span data-stu-id="f949f-159">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="f949f-160">section0:key0</span><span class="sxs-lookup"><span data-stu-id="f949f-160">section0:key0</span></span>
* <span data-ttu-id="f949f-161">section0:key1</span><span class="sxs-lookup"><span data-stu-id="f949f-161">section0:key1</span></span>
* <span data-ttu-id="f949f-162">section1:key0</span><span class="sxs-lookup"><span data-stu-id="f949f-162">section1:key0</span></span>
* <span data-ttu-id="f949f-163">section1:key1</span><span class="sxs-lookup"><span data-stu-id="f949f-163">section1:key1</span></span>

<span data-ttu-id="f949f-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> 與 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> 方法可用來在設定資料中隔離區段與區段的子系。</span><span class="sxs-lookup"><span data-stu-id="f949f-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="f949f-165">[GetSection,、 GetChildren 與 Exists](#getsection-getchildren-and-exists) 中說明這些方法。</span><span class="sxs-lookup"><span data-stu-id="f949f-165">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="f949f-166">慣例</span><span class="sxs-lookup"><span data-stu-id="f949f-166">Conventions</span></span>

<span data-ttu-id="f949f-167">在應用程式啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="f949f-167">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="f949f-168">「檔案設定提供者」可以在應用程式啟動之後且底層設定檔案變更時重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="f949f-168">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="f949f-169">此主題稍後將說明「檔案設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="f949f-169">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="f949f-170">您可以在應用程式的[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器中找到 <xref:Microsoft.Extensions.Configuration.IConfiguration>。</span><span class="sxs-lookup"><span data-stu-id="f949f-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="f949f-171">設定提供者無法使用 DI，因為當它們由主機設定時，它無法使用。</span><span class="sxs-lookup"><span data-stu-id="f949f-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="f949f-172">設定機碼會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="f949f-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="f949f-173">機碼區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="f949f-173">Keys are case-insensitive.</span></span> <span data-ttu-id="f949f-174">例如，`ConnectionString` 與 `connectionstring` 會被視為相等的機碼。</span><span class="sxs-lookup"><span data-stu-id="f949f-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="f949f-175">若相同機碼的值是由相同或不同設定提供者設定，在機碼上設定的最後一個值是所使用的值。</span><span class="sxs-lookup"><span data-stu-id="f949f-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="f949f-176">階層式機碼</span><span class="sxs-lookup"><span data-stu-id="f949f-176">Hierarchical keys</span></span>
  * <span data-ttu-id="f949f-177">在設定 API 內，冒號分隔字元 (`:`) 可在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="f949f-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="f949f-178">在環境變數中，冒號分隔字元可能無法在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="f949f-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="f949f-179">所有平台都支援雙底線 (`__`)，而且它會被轉換為冒號。</span><span class="sxs-lookup"><span data-stu-id="f949f-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="f949f-180">在 Azure Key Vault 中，階層式機碼使用 `--` (兩個破折號) 來做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="f949f-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="f949f-181">您必須提供程式碼，在祕密載入到應用程式的設定時將破折號取代為冒號。</span><span class="sxs-lookup"><span data-stu-id="f949f-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="f949f-182"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="f949f-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="f949f-183">[將陣列繫結到類別](#bind-an-array-to-a-class)一節說明陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="f949f-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="f949f-184">設定值會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="f949f-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="f949f-185">值是字串。</span><span class="sxs-lookup"><span data-stu-id="f949f-185">Values are strings.</span></span>
* <span data-ttu-id="f949f-186">Null 值無法存放在設定中或繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="f949f-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="f949f-187">提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-187">Providers</span></span>

<span data-ttu-id="f949f-188">下表顯示可供 ASP.NET Core 應用程式使用的設定提供者。</span><span class="sxs-lookup"><span data-stu-id="f949f-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="f949f-189">提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-189">Provider</span></span> | <span data-ttu-id="f949f-190">從&hellip;提供設定</span><span class="sxs-lookup"><span data-stu-id="f949f-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="f949f-191">[Azure Key Vault 設定提供者](xref:security/key-vault-configuration) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="f949f-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="f949f-192">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="f949f-192">Azure Key Vault</span></span> |
| [<span data-ttu-id="f949f-193">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-193">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="f949f-194">命令列參數</span><span class="sxs-lookup"><span data-stu-id="f949f-194">Command-line parameters</span></span> |
| [<span data-ttu-id="f949f-195">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-195">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="f949f-196">自訂來源</span><span class="sxs-lookup"><span data-stu-id="f949f-196">Custom source</span></span> |
| [<span data-ttu-id="f949f-197">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-197">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="f949f-198">環境變數</span><span class="sxs-lookup"><span data-stu-id="f949f-198">Environment variables</span></span> |
| [<span data-ttu-id="f949f-199">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-199">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="f949f-200">檔案 (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="f949f-200">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="f949f-201">每個檔案機碼的設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-201">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="f949f-202">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="f949f-202">Directory files</span></span> |
| [<span data-ttu-id="f949f-203">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-203">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="f949f-204">記憶體內集合</span><span class="sxs-lookup"><span data-stu-id="f949f-204">In-memory collections</span></span> |
| <span data-ttu-id="f949f-205">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="f949f-205">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="f949f-206">使用者設定檔目錄中的檔案</span><span class="sxs-lookup"><span data-stu-id="f949f-206">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="f949f-207">提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-207">Provider</span></span> | <span data-ttu-id="f949f-208">從&hellip;提供設定</span><span class="sxs-lookup"><span data-stu-id="f949f-208">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="f949f-209">[Azure Key Vault 設定提供者](xref:security/key-vault-configuration) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="f949f-209">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="f949f-210">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="f949f-210">Azure Key Vault</span></span> |
| [<span data-ttu-id="f949f-211">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="f949f-212">命令列參數</span><span class="sxs-lookup"><span data-stu-id="f949f-212">Command-line parameters</span></span> |
| [<span data-ttu-id="f949f-213">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="f949f-214">自訂來源</span><span class="sxs-lookup"><span data-stu-id="f949f-214">Custom source</span></span> |
| [<span data-ttu-id="f949f-215">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-215">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="f949f-216">環境變數</span><span class="sxs-lookup"><span data-stu-id="f949f-216">Environment variables</span></span> |
| [<span data-ttu-id="f949f-217">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="f949f-218">檔案 (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="f949f-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="f949f-219">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-219">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="f949f-220">記憶體內集合</span><span class="sxs-lookup"><span data-stu-id="f949f-220">In-memory collections</span></span> |
| <span data-ttu-id="f949f-221">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="f949f-221">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="f949f-222">使用者設定檔目錄中的檔案</span><span class="sxs-lookup"><span data-stu-id="f949f-222">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="f949f-223">提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-223">Provider</span></span> | <span data-ttu-id="f949f-224">從&hellip;提供設定</span><span class="sxs-lookup"><span data-stu-id="f949f-224">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="f949f-225">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-225">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="f949f-226">命令列參數</span><span class="sxs-lookup"><span data-stu-id="f949f-226">Command-line parameters</span></span> |
| [<span data-ttu-id="f949f-227">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-227">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="f949f-228">自訂來源</span><span class="sxs-lookup"><span data-stu-id="f949f-228">Custom source</span></span> |
| [<span data-ttu-id="f949f-229">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-229">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="f949f-230">環境變數</span><span class="sxs-lookup"><span data-stu-id="f949f-230">Environment variables</span></span> |
| [<span data-ttu-id="f949f-231">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-231">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="f949f-232">檔案 (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="f949f-232">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="f949f-233">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-233">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="f949f-234">記憶體內集合</span><span class="sxs-lookup"><span data-stu-id="f949f-234">In-memory collections</span></span> |
| <span data-ttu-id="f949f-235">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="f949f-235">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="f949f-236">使用者設定檔目錄中的檔案</span><span class="sxs-lookup"><span data-stu-id="f949f-236">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="f949f-237">在啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="f949f-237">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="f949f-238">此主題中所述的設定提供者是以字母順序描述，而非以您的程式碼安排它們的順序。</span><span class="sxs-lookup"><span data-stu-id="f949f-238">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="f949f-239">在您的程式碼中針對底層設定來源的優先順序，為設定提供者排序。</span><span class="sxs-lookup"><span data-stu-id="f949f-239">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="f949f-240">典型的設定提供者順序是：</span><span class="sxs-lookup"><span data-stu-id="f949f-240">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="f949f-241">檔案 (*appsettings.json*、*appsettings.{Environment}.json*，其中 `{Environment}` 是應用程式的目前裝載環境)</span><span class="sxs-lookup"><span data-stu-id="f949f-241">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="f949f-242">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="f949f-242">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="f949f-243">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (僅限開發環境)</span><span class="sxs-lookup"><span data-stu-id="f949f-243">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="f949f-244">環境變數</span><span class="sxs-lookup"><span data-stu-id="f949f-244">Environment variables</span></span>
1. <span data-ttu-id="f949f-245">命令列引數</span><span class="sxs-lookup"><span data-stu-id="f949f-245">Command-line arguments</span></span>

<span data-ttu-id="f949f-246">將命令列設定提供者放在提供者序列結尾是常見作法，因為這樣可以讓命令列引數覆寫由其它提供者所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="f949f-246">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f949f-247">當您使用 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>初始化新的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 時，就會有這個提供者順序。</span><span class="sxs-lookup"><span data-stu-id="f949f-247">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="f949f-248">如需詳細資訊，請參閱 [Web主機：設定主機](xref:fundamentals/host/web-host#set-up-a-host)。</span><span class="sxs-lookup"><span data-stu-id="f949f-248">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f949f-249">您可以使用 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 並在 `Startup` <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> 方法，以為應用程式 (而非主機) 建立提供者順序：</span><span class="sxs-lookup"><span data-stu-id="f949f-249">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

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

<span data-ttu-id="f949f-250">在上面的範例中，環境名稱 (`env.EnvironmentName`) 與應用程式組件名稱 (`env.ApplicationName`) 是由 <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> 提供。</span><span class="sxs-lookup"><span data-stu-id="f949f-250">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="f949f-251">如需詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="f949f-251">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="configureappconfiguration"></a><span data-ttu-id="f949f-252">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="f949f-252">ConfigureAppConfiguration</span></span>

<span data-ttu-id="f949f-253">建置 Web 主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定提供者，以及由 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 自動新增的設定提供者：</span><span class="sxs-lookup"><span data-stu-id="f949f-253">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the Web Host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="f949f-254">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-254">Command-line Configuration Provider</span></span>

<span data-ttu-id="f949f-255"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> 會在執行階段從命令列引數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="f949f-255">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="f949f-256">為了啟用命令列設定，<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 延伸模組方法會在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫。</span><span class="sxs-lookup"><span data-stu-id="f949f-256">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f949f-257">當您使用 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 初始化新的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 時，會自動呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="f949f-257">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="f949f-258">如需詳細資訊，請參閱 [Web主機：設定主機](xref:fundamentals/host/web-host#set-up-a-host)。</span><span class="sxs-lookup"><span data-stu-id="f949f-258">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="f949f-259">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="f949f-259">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="f949f-260">從 *appsettings.json* 與 *appsettings.{Environment}.json* 載入其他選擇性設定。</span><span class="sxs-lookup"><span data-stu-id="f949f-260">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="f949f-261">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (在開發環境中)。</span><span class="sxs-lookup"><span data-stu-id="f949f-261">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="f949f-262">環境變數。</span><span class="sxs-lookup"><span data-stu-id="f949f-262">Environment variables.</span></span>

<span data-ttu-id="f949f-263">`CreateDefaultBuilder` 會最後才新增命令列設定提供者。</span><span class="sxs-lookup"><span data-stu-id="f949f-263">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="f949f-264">在執行階段傳遞的命令列引數會覆寫由其它提供者所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="f949f-264">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="f949f-265">`CreateDefaultBuilder` 會在建構主機時執行作業。</span><span class="sxs-lookup"><span data-stu-id="f949f-265">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="f949f-266">因此，由`CreateDefaultBuilder` 啟用的命令列設定可以影響主機的設定方式。</span><span class="sxs-lookup"><span data-stu-id="f949f-266">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f949f-267">建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="f949f-267">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="f949f-268">`CreateDefaultBuilder` 已經呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="f949f-268">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="f949f-269">如果您需要提供應用程式設定並仍要能夠使用命令列引數覆寫該設定，請在 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 中呼叫應用程式的其他提供者，最後呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="f949f-269">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="f949f-270">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="f949f-270">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f949f-271">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>。</span><span class="sxs-lookup"><span data-stu-id="f949f-271">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="f949f-272">呼叫 `UseConfiguration` 時，`CreateDefaultBuilder` 已經呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="f949f-272">`AddCommandLine` has already been called by `CreateDefaultBuilder` when `UseConfiguration` is called.</span></span> <span data-ttu-id="f949f-273">如果您需要提供應用程式設定並仍要能夠使用命令列引數覆寫該設定，請在 `ConfigurationBuilder` 上呼叫應用程式的其他提供者，最後呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="f949f-273">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers on a `ConfigurationBuilder` and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="f949f-274">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="f949f-274">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f949f-275">若要啟用命令列設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="f949f-275">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f949f-276">最後才呼叫提供者，以允許在執行階段傳遞的命令列引數覆寫由其他設定提供者所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="f949f-276">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="f949f-277">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="f949f-277">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="f949f-278">**範例**</span><span class="sxs-lookup"><span data-stu-id="f949f-278">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f949f-279">2.x 範例應用程式可發揮靜態方便方法 `CreateDefaultBuilder` 的優勢以建置主機，這包括對 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="f949f-279">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f949f-280">1.x 範例應用程式會呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 上的 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>。</span><span class="sxs-lookup"><span data-stu-id="f949f-280">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="f949f-281">從專案目錄中開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="f949f-281">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="f949f-282">提供命令列引數給 `dotnet run` 命令 `dotnet run CommandLineKey=CommandLineValue`。</span><span class="sxs-lookup"><span data-stu-id="f949f-282">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="f949f-283">在應用程式執行之後，開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f949f-283">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="f949f-284">觀察輸出是否包含提供給 `dotnet run` 之設定命令列引數的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="f949f-284">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="f949f-285">引數</span><span class="sxs-lookup"><span data-stu-id="f949f-285">Arguments</span></span>

<span data-ttu-id="f949f-286">當值後面接著空格時，值必須接著等號 (`=`)，或機碼必須有前置詞 (`--` 或 `/`)。</span><span class="sxs-lookup"><span data-stu-id="f949f-286">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="f949f-287">若使用等號 (例如 `CommandLineKey=`)，值可以是 Null。</span><span class="sxs-lookup"><span data-stu-id="f949f-287">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="f949f-288">索引鍵前置字元</span><span class="sxs-lookup"><span data-stu-id="f949f-288">Key prefix</span></span>               | <span data-ttu-id="f949f-289">範例</span><span class="sxs-lookup"><span data-stu-id="f949f-289">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="f949f-290">沒有前置字元</span><span class="sxs-lookup"><span data-stu-id="f949f-290">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="f949f-291">雙虛線 (`--`)</span><span class="sxs-lookup"><span data-stu-id="f949f-291">Two dashes (`--`)</span></span>        | <span data-ttu-id="f949f-292">`--CommandLineKey2=value2`、 `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="f949f-292">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="f949f-293">正斜線 (`/`)</span><span class="sxs-lookup"><span data-stu-id="f949f-293">Forward slash (`/`)</span></span>      | <span data-ttu-id="f949f-294">`/CommandLineKey3=value3`、 `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="f949f-294">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="f949f-295">在相同的命令中，請不要混合使用等號搭配使用空格之機碼值組的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="f949f-295">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="f949f-296">範例命令：</span><span class="sxs-lookup"><span data-stu-id="f949f-296">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="f949f-297">切換對應</span><span class="sxs-lookup"><span data-stu-id="f949f-297">Switch mappings</span></span>

<span data-ttu-id="f949f-298">參數對應允許索引鍵名稱取代邏輯。</span><span class="sxs-lookup"><span data-stu-id="f949f-298">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="f949f-299">當您使用 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 手動建置設定時，可以提供切換取代的字典給 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 方法。</span><span class="sxs-lookup"><span data-stu-id="f949f-299">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="f949f-300">使用切換對應字典時，會檢查字典中是否有任何索引鍵符合命令列引數所提供的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="f949f-300">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="f949f-301">如果在字典中找到命令列索引鍵，則會傳回字典值 (索引鍵取代) 以在應用程式的設定中設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="f949f-301">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="f949f-302">所有前面加上單虛線 (`-`) 的命令列索引鍵都需要切換對應。</span><span class="sxs-lookup"><span data-stu-id="f949f-302">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="f949f-303">切換對應字典索引鍵規則：</span><span class="sxs-lookup"><span data-stu-id="f949f-303">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="f949f-304">切換必須以單虛線 (`-`) 或雙虛線 (`--`) 開頭。</span><span class="sxs-lookup"><span data-stu-id="f949f-304">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="f949f-305">切換對應字典不能包含重複索引鍵。</span><span class="sxs-lookup"><span data-stu-id="f949f-305">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f949f-306">建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="f949f-306">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="f949f-307">如上面的範例所示，當使用切換對應時，對 `CreateDefaultBuilder` 的呼叫不應該傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="f949f-307">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="f949f-308">`CreateDefaultBuilder` 方法的 `AddCommandLine` 呼叫不包括對應的切換，而且沒有任何方式可以將切換對應字典傳遞給 `CreateDefaultBuilder`。</span><span class="sxs-lookup"><span data-stu-id="f949f-308">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="f949f-309">若對應切換中包含引數且已傳遞給 `CreateDefaultBuilder`，其 `AddCommandLine` 提供者會無法使用 <xref:System.FormatException> 來初始化。</span><span class="sxs-lookup"><span data-stu-id="f949f-309">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="f949f-310">解決方案並非將引數傳遞給 `CreateDefaultBuilder`，而是允許 `ConfigurationBuilder` 方法的 `AddCommandLine` 方法同時處理引數與切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="f949f-310">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="f949f-311">如上面的範例所示，當使用切換對應時，對 `CreateDefaultBuilder` 的呼叫不應該傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="f949f-311">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="f949f-312">`CreateDefaultBuilder` 方法的 `AddCommandLine` 呼叫不包括對應的切換，而且沒有任何方式可以將切換對應字典傳遞給 `CreateDefaultBuilder`。</span><span class="sxs-lookup"><span data-stu-id="f949f-312">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="f949f-313">若對應切換中包含引數且已傳遞給 `CreateDefaultBuilder`，其 `AddCommandLine` 提供者會無法使用 <xref:System.FormatException> 來初始化。</span><span class="sxs-lookup"><span data-stu-id="f949f-313">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="f949f-314">解決方案並非將引數傳遞給 `CreateDefaultBuilder`，而是允許 `ConfigurationBuilder` 方法的 `AddCommandLine` 方法同時處理引數與切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="f949f-314">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="f949f-315">建立切換對應字典之後，它會包含下表中所示的資料。</span><span class="sxs-lookup"><span data-stu-id="f949f-315">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="f949f-316">Key</span><span class="sxs-lookup"><span data-stu-id="f949f-316">Key</span></span>       | <span data-ttu-id="f949f-317">值</span><span class="sxs-lookup"><span data-stu-id="f949f-317">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="f949f-318">若啟動應用程式時使用切換對應機碼，設定會接收由字典提供之機碼上的設定值：</span><span class="sxs-lookup"><span data-stu-id="f949f-318">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="f949f-319">執行上述命令之後，設定包含下表中顯示的值。</span><span class="sxs-lookup"><span data-stu-id="f949f-319">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="f949f-320">Key</span><span class="sxs-lookup"><span data-stu-id="f949f-320">Key</span></span>               | <span data-ttu-id="f949f-321">值</span><span class="sxs-lookup"><span data-stu-id="f949f-321">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="f949f-322">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-322">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="f949f-323"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> 會在執行階段從環境變數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="f949f-323">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="f949f-324">若要啟用環境變數設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="f949f-324">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f949f-325">在環境變數中搭配階層式機碼使用時，冒號分隔字元 (`:`) 可能無法在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="f949f-325">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="f949f-326">所有平台都支援雙底線 (`__`)，而且它會被冒號取代。</span><span class="sxs-lookup"><span data-stu-id="f949f-326">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="f949f-327">[Azure App Service](https://azure.microsoft.com/services/app-service/) 允許您在 Azure 入口網站中設定環境變數，以使用「環境變數設定提供者」覆寫應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="f949f-327">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="f949f-328">如需詳細資訊，請參閱 [Azure App：使用 Azure 入口網站覆寫應用程式設定](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)。</span><span class="sxs-lookup"><span data-stu-id="f949f-328">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f949f-329">當您初始新的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 時，會為首碼為 `ASPNETCORE_` 的環境變數自動呼叫 `AddEnvironmentVariables`。</span><span class="sxs-lookup"><span data-stu-id="f949f-329">`AddEnvironmentVariables` is automatically called for environment variables prefixed with `ASPNETCORE_` when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="f949f-330">如需詳細資訊，請參閱 [Web主機：設定主機](xref:fundamentals/host/web-host#set-up-a-host)。</span><span class="sxs-lookup"><span data-stu-id="f949f-330">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="f949f-331">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="f949f-331">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="f949f-332">來自無首碼之環境變數的應用程式組態 (在未提供首碼的情況下呼叫 `AddEnvironmentVariables`)。</span><span class="sxs-lookup"><span data-stu-id="f949f-332">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="f949f-333">從 *appsettings.json* 與 *appsettings.{Environment}.json* 載入其他選擇性設定。</span><span class="sxs-lookup"><span data-stu-id="f949f-333">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="f949f-334">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (在開發環境中)。</span><span class="sxs-lookup"><span data-stu-id="f949f-334">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="f949f-335">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="f949f-335">Command-line arguments.</span></span>

<span data-ttu-id="f949f-336">從使用者祕密與 *appsettings* 檔案建立設定之後，會呼叫「環境變數設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="f949f-336">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="f949f-337">在此位置呼叫提供者可讓系統在執行階段讀取環境變數，以覆寫由使用者祕密與 *appsettings* 檔案所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="f949f-337">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f949f-338">建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="f949f-338">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="f949f-339">如果您需要從其他環境變數提供應用程式設定，請呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 中的應用程式其他提供者，並呼叫具有該前置詞的 `AddEnvironmentVariables`。</span><span class="sxs-lookup"><span data-stu-id="f949f-339">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="f949f-340">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="f949f-340">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f949f-341">呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 執行個體上的 `AddEnvironmentVariables` 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="f949f-341">Call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="f949f-342">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>。</span><span class="sxs-lookup"><span data-stu-id="f949f-342">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="f949f-343">如果您需要從其他環境變數提供應用程式設定，請呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 中的應用程式其他提供者，並呼叫具有該前置詞的 `AddEnvironmentVariables`。</span><span class="sxs-lookup"><span data-stu-id="f949f-343">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="f949f-344">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="f949f-344">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f949f-345">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="f949f-345">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="f949f-346">**範例**</span><span class="sxs-lookup"><span data-stu-id="f949f-346">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f949f-347">2.x 範例應用程式可發揮靜態方便方法 `CreateDefaultBuilder` 的優勢以建置主機，這包括對 `AddEnvironmentVariables` 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="f949f-347">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f949f-348">1.x 範例應用程式會呼叫 `ConfigurationBuilder` 上的 `AddEnvironmentVariables`。</span><span class="sxs-lookup"><span data-stu-id="f949f-348">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="f949f-349">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="f949f-349">Run the sample app.</span></span> <span data-ttu-id="f949f-350">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f949f-350">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="f949f-351">觀察輸出是否包含環境變數 `ENVIRONMENT` 的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="f949f-351">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="f949f-352">值反映應用程式執行所在環境，在本機執行時，通常是 `Development`。</span><span class="sxs-lookup"><span data-stu-id="f949f-352">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="f949f-353">為縮短由應用程式轉譯的環境變數清單，應用程式會篩選開頭如下的環境變數：</span><span class="sxs-lookup"><span data-stu-id="f949f-353">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="f949f-354">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="f949f-354">ASPNETCORE_</span></span>
* <span data-ttu-id="f949f-355">urls</span><span class="sxs-lookup"><span data-stu-id="f949f-355">urls</span></span>
* <span data-ttu-id="f949f-356">記錄</span><span class="sxs-lookup"><span data-stu-id="f949f-356">Logging</span></span>
* <span data-ttu-id="f949f-357">環境</span><span class="sxs-lookup"><span data-stu-id="f949f-357">ENVIRONMENT</span></span>
* <span data-ttu-id="f949f-358">contentRoot</span><span class="sxs-lookup"><span data-stu-id="f949f-358">contentRoot</span></span>
* <span data-ttu-id="f949f-359">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="f949f-359">AllowedHosts</span></span>
* <span data-ttu-id="f949f-360">applicationName</span><span class="sxs-lookup"><span data-stu-id="f949f-360">applicationName</span></span>
* <span data-ttu-id="f949f-361">CommandLine</span><span class="sxs-lookup"><span data-stu-id="f949f-361">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f949f-362">若想要將所有環境變數公開給應用程式使用，請將 *Pages/Index.cshtml.cs* 中的 `FilteredConfiguration` 變更為下面這樣：</span><span class="sxs-lookup"><span data-stu-id="f949f-362">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f949f-363">若想要將所有環境變數公開給應用程式使用，請將 *Controllers/HomeController.cs* 中的 `FilteredConfiguration` 變更為下面這樣：</span><span class="sxs-lookup"><span data-stu-id="f949f-363">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="f949f-364">首碼</span><span class="sxs-lookup"><span data-stu-id="f949f-364">Prefixes</span></span>

<span data-ttu-id="f949f-365">當您將前置詞套用到 `AddEnvironmentVariables` 方法時，會篩選載入到應用程式設定中的環境變數。</span><span class="sxs-lookup"><span data-stu-id="f949f-365">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="f949f-366">例如，若要篩選前置詞為 `CUSTOM_` 的環境變數，請提供前置詞給設定提供者：</span><span class="sxs-lookup"><span data-stu-id="f949f-366">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="f949f-367">建立設定機碼值組時，會移除前置詞。</span><span class="sxs-lookup"><span data-stu-id="f949f-367">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f949f-368">靜態方便方法 `CreateDefaultBuilder` 會建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 以建立應用程式的主機。</span><span class="sxs-lookup"><span data-stu-id="f949f-368">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="f949f-369">當 `WebHostBuilder` 建立時，它會在前置詞為 `ASPNETCORE_` 的環境變數中尋找其主機設定。</span><span class="sxs-lookup"><span data-stu-id="f949f-369">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="f949f-370">**連接字串前置詞**</span><span class="sxs-lookup"><span data-stu-id="f949f-370">**Connection string prefixes**</span></span>

<span data-ttu-id="f949f-371">設定 API 四個連接字串環境變數的特殊處理規則，這些這些環境變數牽涉到針對應用程式環境設定 Azure 連接字串。</span><span class="sxs-lookup"><span data-stu-id="f949f-371">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="f949f-372">若將前置詞提供給 `AddEnvironmentVariables`具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="f949f-372">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="f949f-373">連接字串前置詞</span><span class="sxs-lookup"><span data-stu-id="f949f-373">Connection string prefix</span></span> | <span data-ttu-id="f949f-374">提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-374">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="f949f-375">自訂提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-375">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="f949f-376">MySQL</span><span class="sxs-lookup"><span data-stu-id="f949f-376">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="f949f-377">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="f949f-377">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="f949f-378">SQL Server</span><span class="sxs-lookup"><span data-stu-id="f949f-378">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="f949f-379">當探索到具有下表所顯示之任何四個前置詞的環境變數並將其載入到設定中時：</span><span class="sxs-lookup"><span data-stu-id="f949f-379">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="f949f-380">會透過移除環境變數前置詞並新增設定機碼區段 (`ConnectionStrings`) 來移除具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="f949f-380">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="f949f-381">會建立新的設定機碼值組以代表資料庫連線提供者 (`CUSTOMCONNSTR_` 除外，這沒有所述提供者)。</span><span class="sxs-lookup"><span data-stu-id="f949f-381">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="f949f-382">環境變數機碼</span><span class="sxs-lookup"><span data-stu-id="f949f-382">Environment variable key</span></span> | <span data-ttu-id="f949f-383">已轉換的設定機碼</span><span class="sxs-lookup"><span data-stu-id="f949f-383">Converted configuration key</span></span> | <span data-ttu-id="f949f-384">提供者設定項目</span><span class="sxs-lookup"><span data-stu-id="f949f-384">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="f949f-385">設定項目未建立。</span><span class="sxs-lookup"><span data-stu-id="f949f-385">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="f949f-386">機碼：`ConnectionStrings:<KEY>_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="f949f-386">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="f949f-387">值：`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="f949f-387">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="f949f-388">機碼：`ConnectionStrings:<KEY>_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="f949f-388">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="f949f-389">值：`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="f949f-389">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="f949f-390">機碼：`ConnectionStrings:<KEY>_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="f949f-390">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="f949f-391">值：`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="f949f-391">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="f949f-392">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-392">File Configuration Provider</span></span>

<span data-ttu-id="f949f-393"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> 是用於從檔案系統載入設定的基底類別。</span><span class="sxs-lookup"><span data-stu-id="f949f-393"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="f949f-394">下列設定提供者專用於特定檔案類型：</span><span class="sxs-lookup"><span data-stu-id="f949f-394">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="f949f-395">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-395">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="f949f-396">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-396">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="f949f-397">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-397">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="f949f-398">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-398">INI Configuration Provider</span></span>

<span data-ttu-id="f949f-399"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> 會在執行階段從 INI 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="f949f-399">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="f949f-400">若要啟用 INI 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="f949f-400">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f949f-401">冒號可用來做為 INI 檔案設定中的區段分隔符號。</span><span class="sxs-lookup"><span data-stu-id="f949f-401">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="f949f-402">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="f949f-402">Overloads permit specifying:</span></span>

* <span data-ttu-id="f949f-403">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="f949f-403">Whether the file is optional.</span></span>
* <span data-ttu-id="f949f-404">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="f949f-404">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="f949f-405"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="f949f-405">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f949f-406">建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="f949f-406">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="f949f-407">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="f949f-407">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f949f-408">呼叫 `CreateDefaultBuilder` 時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="f949f-408">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="f949f-409">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="f949f-409">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f949f-410">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="f949f-410">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="f949f-411">INI 設定檔的一般範例：</span><span class="sxs-lookup"><span data-stu-id="f949f-411">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="f949f-412">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="f949f-412">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="f949f-413">section0:key0</span><span class="sxs-lookup"><span data-stu-id="f949f-413">section0:key0</span></span>
* <span data-ttu-id="f949f-414">section0:key1</span><span class="sxs-lookup"><span data-stu-id="f949f-414">section0:key1</span></span>
* <span data-ttu-id="f949f-415">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="f949f-415">section1:subsection:key</span></span>
* <span data-ttu-id="f949f-416">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="f949f-416">section2:subsection0:key</span></span>
* <span data-ttu-id="f949f-417">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="f949f-417">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="f949f-418">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-418">JSON Configuration Provider</span></span>

<span data-ttu-id="f949f-419"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> 會在執行階段從 JSON 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="f949f-419">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="f949f-420">若要啟用 JSON 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="f949f-420">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f949f-421">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="f949f-421">Overloads permit specifying:</span></span>

* <span data-ttu-id="f949f-422">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="f949f-422">Whether the file is optional.</span></span>
* <span data-ttu-id="f949f-423">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="f949f-423">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="f949f-424"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="f949f-424">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f949f-425">當您使用 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 初始化新的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 時，會自動呼叫 `AddJsonFile` 兩次。</span><span class="sxs-lookup"><span data-stu-id="f949f-425">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="f949f-426">會呼叫此方法以從下列位置載入設定：</span><span class="sxs-lookup"><span data-stu-id="f949f-426">The method is called to load configuration from:</span></span>

* <span data-ttu-id="f949f-427">*appsettings.json* &ndash; 會先讀取此檔案。</span><span class="sxs-lookup"><span data-stu-id="f949f-427">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="f949f-428">檔案的環境版本可以覆寫由 *appsettings.json* 檔案提供的值。</span><span class="sxs-lookup"><span data-stu-id="f949f-428">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="f949f-429">*appsettings.{Environment}.json* &ndash; 檔案的環境版本是根據 [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*) 來載入的。</span><span class="sxs-lookup"><span data-stu-id="f949f-429">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="f949f-430">如需詳細資訊，請參閱 [Web主機：設定主機](xref:fundamentals/host/web-host#set-up-a-host)。</span><span class="sxs-lookup"><span data-stu-id="f949f-430">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="f949f-431">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="f949f-431">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="f949f-432">環境變數。</span><span class="sxs-lookup"><span data-stu-id="f949f-432">Environment variables.</span></span>
* <span data-ttu-id="f949f-433">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (在開發環境中)。</span><span class="sxs-lookup"><span data-stu-id="f949f-433">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="f949f-434">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="f949f-434">Command-line arguments.</span></span>

<span data-ttu-id="f949f-435">會先建立 JSON 設定提供者。</span><span class="sxs-lookup"><span data-stu-id="f949f-435">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="f949f-436">因此，使用者祕密、環境變數與命令列引數會覆寫由 *appsettings* 檔案設定的設定。</span><span class="sxs-lookup"><span data-stu-id="f949f-436">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f949f-437">在建置主機時，呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定檔案 (除了 *appsettings.json* 和  *appsettings.{Environment}.json* 以外) 的應用程式設定：</span><span class="sxs-lookup"><span data-stu-id="f949f-437">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

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

<span data-ttu-id="f949f-438">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="f949f-438">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f949f-439">您也可以直接呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 執行個體上的 `AddJsonFile` 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="f949f-439">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f949f-440">呼叫 `CreateDefaultBuilder` 時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="f949f-440">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="f949f-441">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="f949f-441">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f949f-442">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="f949f-442">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="f949f-443">**範例**</span><span class="sxs-lookup"><span data-stu-id="f949f-443">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f949f-444">2.x 範例應用程式可發揮靜態方便方法 `CreateDefaultBuilder` 的優勢以建置主機，這包括對 `AddJsonFile` 的兩次呼叫。</span><span class="sxs-lookup"><span data-stu-id="f949f-444">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="f949f-445">從 *appsettings.json* 與 *appsettings.{Environment}.json* 載入設定。</span><span class="sxs-lookup"><span data-stu-id="f949f-445">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f949f-446">1.x 範例應用程式會呼叫 `ConfigurationBuilder` 上的 `AddJsonFile` 兩次。</span><span class="sxs-lookup"><span data-stu-id="f949f-446">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="f949f-447">從 *appsettings.json* 與 *appsettings.{Environment}.json* 載入設定。</span><span class="sxs-lookup"><span data-stu-id="f949f-447">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="f949f-448">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="f949f-448">Run the sample app.</span></span> <span data-ttu-id="f949f-449">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f949f-449">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="f949f-450">觀察輸出是否包含表格中所顯示之設定的機碼值組 (視環境而定)。</span><span class="sxs-lookup"><span data-stu-id="f949f-450">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="f949f-451">記錄設定機碼會使用冒號 (`:`) 做為階層式分隔符號。</span><span class="sxs-lookup"><span data-stu-id="f949f-451">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="f949f-452">Key</span><span class="sxs-lookup"><span data-stu-id="f949f-452">Key</span></span>                        | <span data-ttu-id="f949f-453">開發值</span><span class="sxs-lookup"><span data-stu-id="f949f-453">Development Value</span></span> | <span data-ttu-id="f949f-454">生產值</span><span class="sxs-lookup"><span data-stu-id="f949f-454">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="f949f-455">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="f949f-455">Logging:LogLevel:System</span></span>    | <span data-ttu-id="f949f-456">資訊</span><span class="sxs-lookup"><span data-stu-id="f949f-456">Information</span></span>       | <span data-ttu-id="f949f-457">資訊</span><span class="sxs-lookup"><span data-stu-id="f949f-457">Information</span></span>      |
| <span data-ttu-id="f949f-458">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="f949f-458">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="f949f-459">資訊</span><span class="sxs-lookup"><span data-stu-id="f949f-459">Information</span></span>       | <span data-ttu-id="f949f-460">資訊</span><span class="sxs-lookup"><span data-stu-id="f949f-460">Information</span></span>      |
| <span data-ttu-id="f949f-461">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="f949f-461">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="f949f-462">偵錯</span><span class="sxs-lookup"><span data-stu-id="f949f-462">Debug</span></span>             | <span data-ttu-id="f949f-463">錯誤</span><span class="sxs-lookup"><span data-stu-id="f949f-463">Error</span></span>            |
| <span data-ttu-id="f949f-464">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="f949f-464">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="f949f-465">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-465">XML Configuration Provider</span></span>

<span data-ttu-id="f949f-466"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> 會在執行階段從 XML 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="f949f-466">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="f949f-467">若要啟用 XML 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="f949f-467">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f949f-468">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="f949f-468">Overloads permit specifying:</span></span>

* <span data-ttu-id="f949f-469">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="f949f-469">Whether the file is optional.</span></span>
* <span data-ttu-id="f949f-470">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="f949f-470">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="f949f-471"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="f949f-471">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="f949f-472">建立設定機碼值組時，會忽略設定檔案的根節點。</span><span class="sxs-lookup"><span data-stu-id="f949f-472">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="f949f-473">請勿在檔案中指定文件類型定義 (DTD) 或命名空間。</span><span class="sxs-lookup"><span data-stu-id="f949f-473">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f949f-474">建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="f949f-474">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="f949f-475">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="f949f-475">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f949f-476">呼叫 `CreateDefaultBuilder` 時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="f949f-476">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="f949f-477">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="f949f-477">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f949f-478">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="f949f-478">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="f949f-479">XML 設定檔可以針對重複的區段使用相異元素名稱：</span><span class="sxs-lookup"><span data-stu-id="f949f-479">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="f949f-480">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="f949f-480">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="f949f-481">section0:key0</span><span class="sxs-lookup"><span data-stu-id="f949f-481">section0:key0</span></span>
* <span data-ttu-id="f949f-482">section0:key1</span><span class="sxs-lookup"><span data-stu-id="f949f-482">section0:key1</span></span>
* <span data-ttu-id="f949f-483">section1:key0</span><span class="sxs-lookup"><span data-stu-id="f949f-483">section1:key0</span></span>
* <span data-ttu-id="f949f-484">section1:key1</span><span class="sxs-lookup"><span data-stu-id="f949f-484">section1:key1</span></span>

<span data-ttu-id="f949f-485">若 `name` 屬性是用來區別元素，則可以重複那些使用相同元素名稱的元素：</span><span class="sxs-lookup"><span data-stu-id="f949f-485">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="f949f-486">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="f949f-486">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="f949f-487">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="f949f-487">section:section0:key:key0</span></span>
* <span data-ttu-id="f949f-488">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="f949f-488">section:section0:key:key1</span></span>
* <span data-ttu-id="f949f-489">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="f949f-489">section:section1:key:key0</span></span>
* <span data-ttu-id="f949f-490">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="f949f-490">section:section1:key:key1</span></span>

<span data-ttu-id="f949f-491">屬性可用來提供值：</span><span class="sxs-lookup"><span data-stu-id="f949f-491">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="f949f-492">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="f949f-492">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="f949f-493">key:attribute</span><span class="sxs-lookup"><span data-stu-id="f949f-493">key:attribute</span></span>
* <span data-ttu-id="f949f-494">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="f949f-494">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="f949f-495">每個檔案機碼設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-495">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="f949f-496"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> 使用目錄的檔案做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="f949f-496">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="f949f-497">機碼是檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="f949f-497">The key is the file name.</span></span> <span data-ttu-id="f949f-498">值包含檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="f949f-498">The value contains the file's contents.</span></span> <span data-ttu-id="f949f-499">每個檔案機碼設定提供者是在 Docker 主機案例中使用。</span><span class="sxs-lookup"><span data-stu-id="f949f-499">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="f949f-500">若要啟用每個檔案機碼設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="f949f-500">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="f949f-501">檔案的 `directoryPath` 必須是絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="f949f-501">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="f949f-502">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="f949f-502">Overloads permit specifying:</span></span>

* <span data-ttu-id="f949f-503">設定來源的 `Action<KeyPerFileConfigurationSource>` 委派。</span><span class="sxs-lookup"><span data-stu-id="f949f-503">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="f949f-504">目錄是否為選擇性與目錄的路徑。</span><span class="sxs-lookup"><span data-stu-id="f949f-504">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="f949f-505">建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="f949f-505">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="f949f-506">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="f949f-506">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="f949f-507">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-507">Memory Configuration Provider</span></span>

<span data-ttu-id="f949f-508"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> 使用記憶體內集合做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="f949f-508">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="f949f-509">若要啟用記憶體內集合設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="f949f-509">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f949f-510">設定提供者可以使用 `IEnumerable<KeyValuePair<String,String>>` 來初始化。</span><span class="sxs-lookup"><span data-stu-id="f949f-510">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f949f-511">建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="f949f-511">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="f949f-512">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="f949f-512">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f949f-513">呼叫 `CreateDefaultBuilder` 時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="f949f-513">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="f949f-514">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：</span><span class="sxs-lookup"><span data-stu-id="f949f-514">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f949f-515">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="f949f-515">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="f949f-516">GetValue</span><span class="sxs-lookup"><span data-stu-id="f949f-516">GetValue</span></span>

<span data-ttu-id="f949f-517">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) 會從具有所指定機碼的設定擷取值，並將它轉換為指定的型別。</span><span class="sxs-lookup"><span data-stu-id="f949f-517">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="f949f-518">若找不到機碼，多載允許您提供預設值。</span><span class="sxs-lookup"><span data-stu-id="f949f-518">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="f949f-519">下列範例會從具有機碼 `NumberKey` 的設定擷取字串值、將值的型別設定為 `int`，並將值存放在變數 `intValue` 中。</span><span class="sxs-lookup"><span data-stu-id="f949f-519">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="f949f-520">若在設定機碼中找不到 `NumberKey`，`intValue` 會接收 `99` 的預設值：</span><span class="sxs-lookup"><span data-stu-id="f949f-520">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="f949f-521">GetSection、GetChildren 與 Exists</span><span class="sxs-lookup"><span data-stu-id="f949f-521">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="f949f-522">針對下面的範例，請考慮下列 JSON 檔案。</span><span class="sxs-lookup"><span data-stu-id="f949f-522">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="f949f-523">在兩個區段中找到四個機碼，其中一個包括子區段組：</span><span class="sxs-lookup"><span data-stu-id="f949f-523">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="f949f-524">當檔案被讀入到設定之後，會建立下列唯一階層式機碼以存放設定值：</span><span class="sxs-lookup"><span data-stu-id="f949f-524">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="f949f-525">section0:key0</span><span class="sxs-lookup"><span data-stu-id="f949f-525">section0:key0</span></span>
* <span data-ttu-id="f949f-526">section0:key1</span><span class="sxs-lookup"><span data-stu-id="f949f-526">section0:key1</span></span>
* <span data-ttu-id="f949f-527">section1:key0</span><span class="sxs-lookup"><span data-stu-id="f949f-527">section1:key0</span></span>
* <span data-ttu-id="f949f-528">section1:key1</span><span class="sxs-lookup"><span data-stu-id="f949f-528">section1:key1</span></span>
* <span data-ttu-id="f949f-529">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="f949f-529">section2:subsection0:key0</span></span>
* <span data-ttu-id="f949f-530">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="f949f-530">section2:subsection0:key1</span></span>
* <span data-ttu-id="f949f-531">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="f949f-531">section2:subsection1:key0</span></span>
* <span data-ttu-id="f949f-532">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="f949f-532">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="f949f-533">GetSection</span><span class="sxs-lookup"><span data-stu-id="f949f-533">GetSection</span></span>

<span data-ttu-id="f949f-534">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) 會擷取具有所指定子區段機碼的設定子區段。</span><span class="sxs-lookup"><span data-stu-id="f949f-534">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="f949f-535">若要傳回 `section1` 中只包含一個機碼值組的 <xref:Microsoft.Extensions.Configuration.IConfigurationSection>，請呼叫 `GetSection` 並提供區段名稱：</span><span class="sxs-lookup"><span data-stu-id="f949f-535">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="f949f-536">同樣地，若要取得 `section2:subsection0` 中之機碼的值，請呼叫 `GetSection` 並提供區段路徑：</span><span class="sxs-lookup"><span data-stu-id="f949f-536">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="f949f-537">`GetSection` 絕不會傳回 `null`。</span><span class="sxs-lookup"><span data-stu-id="f949f-537">`GetSection` never returns `null`.</span></span> <span data-ttu-id="f949f-538">若找不到相符的區段，會傳回空的 `IConfigurationSection`。</span><span class="sxs-lookup"><span data-stu-id="f949f-538">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

### <a name="getchildren"></a><span data-ttu-id="f949f-539">GetChildren</span><span class="sxs-lookup"><span data-stu-id="f949f-539">GetChildren</span></span>

<span data-ttu-id="f949f-540">對 `section2` 上之 [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) 的呼叫會取得包括下列項目的 `IEnumerable<IConfigurationSection>`：</span><span class="sxs-lookup"><span data-stu-id="f949f-540">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="f949f-541">存在</span><span class="sxs-lookup"><span data-stu-id="f949f-541">Exists</span></span>

<span data-ttu-id="f949f-542">使用 [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) 來判斷設定區段是否存在：</span><span class="sxs-lookup"><span data-stu-id="f949f-542">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="f949f-543">以範例資料為例， `sectionExists` 是 `false`，因未設定資料中沒有 `section2:subsection2` 區段。</span><span class="sxs-lookup"><span data-stu-id="f949f-543">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="f949f-544">繫結到類別</span><span class="sxs-lookup"><span data-stu-id="f949f-544">Bind to a class</span></span>

<span data-ttu-id="f949f-545">設定可以繫結到類別，以使用*選項模式*代表相關設定的群組。</span><span class="sxs-lookup"><span data-stu-id="f949f-545">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="f949f-546">如需詳細資訊，請參閱<xref:fundamentals/configuration/options>。</span><span class="sxs-lookup"><span data-stu-id="f949f-546">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="f949f-547">設定值是以字串傳回，但是呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 會啟用 [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) 物件的建構。</span><span class="sxs-lookup"><span data-stu-id="f949f-547">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="f949f-548">範例應用程式包含 `Starship` 模型 (*Models/Starship.cs*)：</span><span class="sxs-lookup"><span data-stu-id="f949f-548">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f949f-549">當範例應用程式使用 JSON 設定提供者來載入設定時，*starship.json* 檔案的 `starship` 區段會建立設定：</span><span class="sxs-lookup"><span data-stu-id="f949f-549">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="f949f-550">會建立下列設定機碼值組：</span><span class="sxs-lookup"><span data-stu-id="f949f-550">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="f949f-551">Key</span><span class="sxs-lookup"><span data-stu-id="f949f-551">Key</span></span>                   | <span data-ttu-id="f949f-552">值</span><span class="sxs-lookup"><span data-stu-id="f949f-552">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="f949f-553">starship:name</span><span class="sxs-lookup"><span data-stu-id="f949f-553">starship:name</span></span>         | <span data-ttu-id="f949f-554">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="f949f-554">USS Enterprise</span></span>                                    |
| <span data-ttu-id="f949f-555">starship:registry</span><span class="sxs-lookup"><span data-stu-id="f949f-555">starship:registry</span></span>     | <span data-ttu-id="f949f-556">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="f949f-556">NCC-1701</span></span>                                          |
| <span data-ttu-id="f949f-557">starship:class</span><span class="sxs-lookup"><span data-stu-id="f949f-557">starship:class</span></span>        | <span data-ttu-id="f949f-558">Constitution</span><span class="sxs-lookup"><span data-stu-id="f949f-558">Constitution</span></span>                                      |
| <span data-ttu-id="f949f-559">starship:length</span><span class="sxs-lookup"><span data-stu-id="f949f-559">starship:length</span></span>       | <span data-ttu-id="f949f-560">304.8</span><span class="sxs-lookup"><span data-stu-id="f949f-560">304.8</span></span>                                             |
| <span data-ttu-id="f949f-561">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="f949f-561">starship:commissioned</span></span> | <span data-ttu-id="f949f-562">False</span><span class="sxs-lookup"><span data-stu-id="f949f-562">False</span></span>                                             |
| <span data-ttu-id="f949f-563">trademark</span><span class="sxs-lookup"><span data-stu-id="f949f-563">trademark</span></span>             | <span data-ttu-id="f949f-564">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="f949f-564">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="f949f-565">範例應用程式使用 `starship` 機碼呼叫 `GetSection`。</span><span class="sxs-lookup"><span data-stu-id="f949f-565">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="f949f-566">`starship` 機碼值組會被隔離。</span><span class="sxs-lookup"><span data-stu-id="f949f-566">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="f949f-567">會在子區段上呼叫 `Bind` 方法，並傳入 `Starship` 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="f949f-567">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="f949f-568">繫結執行個體值之後，執行個體會被指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="f949f-568">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="f949f-569">繫結至物件圖形</span><span class="sxs-lookup"><span data-stu-id="f949f-569">Bind to an object graph</span></span>

<span data-ttu-id="f949f-570"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 可以繫結整個 POCO 物件圖形。</span><span class="sxs-lookup"><span data-stu-id="f949f-570"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="f949f-571">範例包含 `TvShow` 模型，其物件圖形包括 `Metadata` 與 `Actors` 類別 (*Models/TvShow.cs*)：</span><span class="sxs-lookup"><span data-stu-id="f949f-571">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f949f-572">範例應用程式有 *tvshow.xml* 檔案，其中包含設定資料：</span><span class="sxs-lookup"><span data-stu-id="f949f-572">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="f949f-573">設定會被繫結到整個 `TvShow` 物件 (使用 `Bind` 方法)。</span><span class="sxs-lookup"><span data-stu-id="f949f-573">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="f949f-574">已繫結的執行個體會被指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="f949f-574">The bound instance is assigned to a property for rendering:</span></span>

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

<span data-ttu-id="f949f-575">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 會繫結並傳回所指定型別。</span><span class="sxs-lookup"><span data-stu-id="f949f-575">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="f949f-576">`Get<T>` 比使用 `Bind` 更方便。</span><span class="sxs-lookup"><span data-stu-id="f949f-576">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="f949f-577">下列程式碼顯示如何根據上面的範例使用 `Get<T>`，這可讓您直接將已繫結的執行個體指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="f949f-577">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="f949f-578">將陣列繫結到類別</span><span class="sxs-lookup"><span data-stu-id="f949f-578">Bind an array to a class</span></span>

<span data-ttu-id="f949f-579">範例應用程式示範此節中解釋的概念。</span><span class="sxs-lookup"><span data-stu-id="f949f-579">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="f949f-580"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="f949f-580">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="f949f-581">任何公開數值機碼區段 (`:0:`、`:1:`、&hellip; `:{n}:`) 的陣列格式都能繫結到POCO 類別陣列。</span><span class="sxs-lookup"><span data-stu-id="f949f-581">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="f949f-582">繫結是由慣例提供。</span><span class="sxs-lookup"><span data-stu-id="f949f-582">Binding is provided by convention.</span></span> <span data-ttu-id="f949f-583">自訂設定提供者不需要實作陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="f949f-583">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="f949f-584">**記憶體內陣列處理**</span><span class="sxs-lookup"><span data-stu-id="f949f-584">**In-memory array processing**</span></span>

<span data-ttu-id="f949f-585">考慮下表中顯示的設定機碼與值。</span><span class="sxs-lookup"><span data-stu-id="f949f-585">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="f949f-586">Key</span><span class="sxs-lookup"><span data-stu-id="f949f-586">Key</span></span>             | <span data-ttu-id="f949f-587">值</span><span class="sxs-lookup"><span data-stu-id="f949f-587">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="f949f-588">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="f949f-588">array:entries:0</span></span> | <span data-ttu-id="f949f-589">value0</span><span class="sxs-lookup"><span data-stu-id="f949f-589">value0</span></span> |
| <span data-ttu-id="f949f-590">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="f949f-590">array:entries:1</span></span> | <span data-ttu-id="f949f-591">value1</span><span class="sxs-lookup"><span data-stu-id="f949f-591">value1</span></span> |
| <span data-ttu-id="f949f-592">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="f949f-592">array:entries:2</span></span> | <span data-ttu-id="f949f-593">value2</span><span class="sxs-lookup"><span data-stu-id="f949f-593">value2</span></span> |
| <span data-ttu-id="f949f-594">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="f949f-594">array:entries:4</span></span> | <span data-ttu-id="f949f-595">value4</span><span class="sxs-lookup"><span data-stu-id="f949f-595">value4</span></span> |
| <span data-ttu-id="f949f-596">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="f949f-596">array:entries:5</span></span> | <span data-ttu-id="f949f-597">value5</span><span class="sxs-lookup"><span data-stu-id="f949f-597">value5</span></span> |

<span data-ttu-id="f949f-598">這些機碼與值是使用「記憶體設定提供者」在範例應用程式中載入：</span><span class="sxs-lookup"><span data-stu-id="f949f-598">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="f949f-599">陣列會跳過索引 &num;3 的值。</span><span class="sxs-lookup"><span data-stu-id="f949f-599">The array skips a value for index &num;3.</span></span> <span data-ttu-id="f949f-600">設定繫結程式無法繫結 Null 值或在已繫結的物件中建立 Null 項目，這在示範繫結此陣列的結果到某物件的時候已經很清楚。</span><span class="sxs-lookup"><span data-stu-id="f949f-600">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="f949f-601">在範例應用程式中，POCO 類別可用來存放繫結設定資料：</span><span class="sxs-lookup"><span data-stu-id="f949f-601">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f949f-602">設定資料已繫結到物件：</span><span class="sxs-lookup"><span data-stu-id="f949f-602">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="f949f-603">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 語法也可以使用，這會產生更精簡的程式碼：</span><span class="sxs-lookup"><span data-stu-id="f949f-603">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="f949f-604">繫結物件 (`ArrayExample`的執行個體) 會從設定接收陣列資料。</span><span class="sxs-lookup"><span data-stu-id="f949f-604">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="f949f-605">`ArrayExample.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="f949f-605">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="f949f-606">`ArrayExample.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="f949f-606">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="f949f-607">0</span><span class="sxs-lookup"><span data-stu-id="f949f-607">0</span></span>                            | <span data-ttu-id="f949f-608">value0</span><span class="sxs-lookup"><span data-stu-id="f949f-608">value0</span></span>                       |
| <span data-ttu-id="f949f-609">1</span><span class="sxs-lookup"><span data-stu-id="f949f-609">1</span></span>                            | <span data-ttu-id="f949f-610">value1</span><span class="sxs-lookup"><span data-stu-id="f949f-610">value1</span></span>                       |
| <span data-ttu-id="f949f-611">2</span><span class="sxs-lookup"><span data-stu-id="f949f-611">2</span></span>                            | <span data-ttu-id="f949f-612">value2</span><span class="sxs-lookup"><span data-stu-id="f949f-612">value2</span></span>                       |
| <span data-ttu-id="f949f-613">3</span><span class="sxs-lookup"><span data-stu-id="f949f-613">3</span></span>                            | <span data-ttu-id="f949f-614">value4</span><span class="sxs-lookup"><span data-stu-id="f949f-614">value4</span></span>                       |
| <span data-ttu-id="f949f-615">4</span><span class="sxs-lookup"><span data-stu-id="f949f-615">4</span></span>                            | <span data-ttu-id="f949f-616">value5</span><span class="sxs-lookup"><span data-stu-id="f949f-616">value5</span></span>                       |

<span data-ttu-id="f949f-617">繫結物件中的索引 &num;3 存放 `array:4` 設定機碼與其 `value4` 的設定資料。</span><span class="sxs-lookup"><span data-stu-id="f949f-617">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="f949f-618">當繫結包含陣列的設定資料時，設定機碼中的陣列索引只會用來列舉設定資料 (當建立物件時)。</span><span class="sxs-lookup"><span data-stu-id="f949f-618">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="f949f-619">設定資料中不能保留 Null 值，當設定機碼中的陣列略過一或多個索引時，不會在繫結物件中建立 Null 值項目。</span><span class="sxs-lookup"><span data-stu-id="f949f-619">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="f949f-620">由會在設定中產生正確機碼值組的任何設定提供者繫結到 `ArrayExample` 執行個體之前，可以提供索引 &num;3 缺少的設定項目。</span><span class="sxs-lookup"><span data-stu-id="f949f-620">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="f949f-621">若範例包含具有缺少之機碼值組的額外 JSON 設定提供者，`ArrayExample.Entries` 會符合完整的設定陣列：</span><span class="sxs-lookup"><span data-stu-id="f949f-621">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="f949f-622">*missing_value.json*：</span><span class="sxs-lookup"><span data-stu-id="f949f-622">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f949f-623">在 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>中：</span><span class="sxs-lookup"><span data-stu-id="f949f-623">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f949f-624">在 `Startup` 建構函式中：</span><span class="sxs-lookup"><span data-stu-id="f949f-624">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="f949f-625">表格中顯示的機碼值組會載入到設定中。</span><span class="sxs-lookup"><span data-stu-id="f949f-625">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="f949f-626">Key</span><span class="sxs-lookup"><span data-stu-id="f949f-626">Key</span></span>             | <span data-ttu-id="f949f-627">值</span><span class="sxs-lookup"><span data-stu-id="f949f-627">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="f949f-628">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="f949f-628">array:entries:3</span></span> | <span data-ttu-id="f949f-629">value3</span><span class="sxs-lookup"><span data-stu-id="f949f-629">value3</span></span> |

<span data-ttu-id="f949f-630">在「JSON 設定提供者」包含索引 &num;3 的項目之後，若繫結 `ArrayExample` 類別執行個體，`ArrayExample.Entries` 陣列會包括這些值：</span><span class="sxs-lookup"><span data-stu-id="f949f-630">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="f949f-631">`ArrayExample.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="f949f-631">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="f949f-632">`ArrayExample.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="f949f-632">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="f949f-633">0</span><span class="sxs-lookup"><span data-stu-id="f949f-633">0</span></span>                            | <span data-ttu-id="f949f-634">value0</span><span class="sxs-lookup"><span data-stu-id="f949f-634">value0</span></span>                       |
| <span data-ttu-id="f949f-635">1</span><span class="sxs-lookup"><span data-stu-id="f949f-635">1</span></span>                            | <span data-ttu-id="f949f-636">value1</span><span class="sxs-lookup"><span data-stu-id="f949f-636">value1</span></span>                       |
| <span data-ttu-id="f949f-637">2</span><span class="sxs-lookup"><span data-stu-id="f949f-637">2</span></span>                            | <span data-ttu-id="f949f-638">value2</span><span class="sxs-lookup"><span data-stu-id="f949f-638">value2</span></span>                       |
| <span data-ttu-id="f949f-639">3</span><span class="sxs-lookup"><span data-stu-id="f949f-639">3</span></span>                            | <span data-ttu-id="f949f-640">value3</span><span class="sxs-lookup"><span data-stu-id="f949f-640">value3</span></span>                       |
| <span data-ttu-id="f949f-641">4</span><span class="sxs-lookup"><span data-stu-id="f949f-641">4</span></span>                            | <span data-ttu-id="f949f-642">value4</span><span class="sxs-lookup"><span data-stu-id="f949f-642">value4</span></span>                       |
| <span data-ttu-id="f949f-643">5</span><span class="sxs-lookup"><span data-stu-id="f949f-643">5</span></span>                            | <span data-ttu-id="f949f-644">value5</span><span class="sxs-lookup"><span data-stu-id="f949f-644">value5</span></span>                       |

<span data-ttu-id="f949f-645">**JSON 陣列處理**</span><span class="sxs-lookup"><span data-stu-id="f949f-645">**JSON array processing**</span></span>

<span data-ttu-id="f949f-646">若 JSON 檔案包含陣列，會為具有以零為基礎之區段索引的陣列元素建立設定機碼。</span><span class="sxs-lookup"><span data-stu-id="f949f-646">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="f949f-647">在下列設定檔中，`subsection` 是陣列：</span><span class="sxs-lookup"><span data-stu-id="f949f-647">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="f949f-648">「JSON 設定提供者」會將設定資料讀入到下列機碼值組：</span><span class="sxs-lookup"><span data-stu-id="f949f-648">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="f949f-649">Key</span><span class="sxs-lookup"><span data-stu-id="f949f-649">Key</span></span>                     | <span data-ttu-id="f949f-650">值</span><span class="sxs-lookup"><span data-stu-id="f949f-650">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="f949f-651">json_array:key</span><span class="sxs-lookup"><span data-stu-id="f949f-651">json_array:key</span></span>          | <span data-ttu-id="f949f-652">valueA</span><span class="sxs-lookup"><span data-stu-id="f949f-652">valueA</span></span> |
| <span data-ttu-id="f949f-653">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="f949f-653">json_array:subsection:0</span></span> | <span data-ttu-id="f949f-654">valueB</span><span class="sxs-lookup"><span data-stu-id="f949f-654">valueB</span></span> |
| <span data-ttu-id="f949f-655">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="f949f-655">json_array:subsection:1</span></span> | <span data-ttu-id="f949f-656">valueC</span><span class="sxs-lookup"><span data-stu-id="f949f-656">valueC</span></span> |
| <span data-ttu-id="f949f-657">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="f949f-657">json_array:subsection:2</span></span> | <span data-ttu-id="f949f-658">valueD</span><span class="sxs-lookup"><span data-stu-id="f949f-658">valueD</span></span> |

<span data-ttu-id="f949f-659">在範例應用程式中，下列 POCO 類別可用來繫結設定機碼值組：</span><span class="sxs-lookup"><span data-stu-id="f949f-659">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f949f-660">在繫結之後，`JsonArrayExample.Key` 會存放值 `valueA`。</span><span class="sxs-lookup"><span data-stu-id="f949f-660">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="f949f-661">子區段值會存放在 POCO 陣列屬性 `Subsection` 中。</span><span class="sxs-lookup"><span data-stu-id="f949f-661">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="f949f-662">`JsonArrayExample.Subsection` 索引</span><span class="sxs-lookup"><span data-stu-id="f949f-662">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="f949f-663">`JsonArrayExample.Subsection` 值</span><span class="sxs-lookup"><span data-stu-id="f949f-663">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="f949f-664">0</span><span class="sxs-lookup"><span data-stu-id="f949f-664">0</span></span>                                   | <span data-ttu-id="f949f-665">valueB</span><span class="sxs-lookup"><span data-stu-id="f949f-665">valueB</span></span>                              |
| <span data-ttu-id="f949f-666">1</span><span class="sxs-lookup"><span data-stu-id="f949f-666">1</span></span>                                   | <span data-ttu-id="f949f-667">valueC</span><span class="sxs-lookup"><span data-stu-id="f949f-667">valueC</span></span>                              |
| <span data-ttu-id="f949f-668">2</span><span class="sxs-lookup"><span data-stu-id="f949f-668">2</span></span>                                   | <span data-ttu-id="f949f-669">valueD</span><span class="sxs-lookup"><span data-stu-id="f949f-669">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="f949f-670">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="f949f-670">Custom configuration provider</span></span>

<span data-ttu-id="f949f-671">範例應用程式示範如何建立會使用 [Entity Framework (EF)](/ef/core/) 從資料庫讀取設定機碼值組的基本設定提供者。</span><span class="sxs-lookup"><span data-stu-id="f949f-671">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="f949f-672">提供者具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="f949f-672">The provider has the following characteristics:</span></span>

* <span data-ttu-id="f949f-673">EF 記憶體內資料庫會用於示範用途。</span><span class="sxs-lookup"><span data-stu-id="f949f-673">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="f949f-674">若要使用需要連接字串的資料庫，請實作第二個 `ConfigurationBuilder` 以從另一個設定提供者提供連接字串。</span><span class="sxs-lookup"><span data-stu-id="f949f-674">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="f949f-675">啟動時，該提供者會將資料庫資料表讀入到設定中。</span><span class="sxs-lookup"><span data-stu-id="f949f-675">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="f949f-676">該提供者不會以個別機碼為基礎查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="f949f-676">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="f949f-677">未實作變更時重新載入，因此在應用程式啟動後更新資料庫對應用程式設定沒有影響。</span><span class="sxs-lookup"><span data-stu-id="f949f-677">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="f949f-678">定義 `EFConfigurationValue` 實體來在資料庫中存放設定值。</span><span class="sxs-lookup"><span data-stu-id="f949f-678">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="f949f-679">*Models/EFConfigurationValue.cs*：</span><span class="sxs-lookup"><span data-stu-id="f949f-679">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f949f-680">新增 `EFConfigurationContext` 以存放及存取已設定的值。</span><span class="sxs-lookup"><span data-stu-id="f949f-680">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="f949f-681">*EFConfigurationProvider/EFConfigurationContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="f949f-681">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f949f-682">建立會實作 <xref:Microsoft.Extensions.Configuration.IConfigurationSource> 的類別。</span><span class="sxs-lookup"><span data-stu-id="f949f-682">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="f949f-683">*EFConfigurationProvider/EFConfigurationSource.cs*：</span><span class="sxs-lookup"><span data-stu-id="f949f-683">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f949f-684">透過繼承自 <xref:Microsoft.Extensions.Configuration.ConfigurationProvider> 來建立自訂設定提供者。</span><span class="sxs-lookup"><span data-stu-id="f949f-684">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="f949f-685">若資料庫是空的，設定提供者會初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="f949f-685">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="f949f-686">*EFConfigurationProvider/EFConfigurationProvider.cs*：</span><span class="sxs-lookup"><span data-stu-id="f949f-686">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f949f-687">`AddEFConfiguration` 擴充方法允許新增設定來源到 `ConfigurationBuilder`。</span><span class="sxs-lookup"><span data-stu-id="f949f-687">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="f949f-688">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="f949f-688">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f949f-689">下列程式碼顯示如何在 *Program.cs* 中使用自訂 `EFConfigurationProvider`：</span><span class="sxs-lookup"><span data-stu-id="f949f-689">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="f949f-690">在啟動期間存取組態</span><span class="sxs-lookup"><span data-stu-id="f949f-690">Access configuration during startup</span></span>

<span data-ttu-id="f949f-691">將 `IConfiguration` 插入到 `Startup` 建構函式，以存取 `Startup.ConfigureServices` 中的設定值。</span><span class="sxs-lookup"><span data-stu-id="f949f-691">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="f949f-692">若要存取 `Startup.Configure` 中的設定，請直接將 `IConfiguration` 插入到方法或從建構函式使用執行個體：</span><span class="sxs-lookup"><span data-stu-id="f949f-692">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="f949f-693">如需使用啟動方便方法來存取設定的範例，請參閱[應用程式啟動：方便方法](xref:fundamentals/startup#convenience-methods)。</span><span class="sxs-lookup"><span data-stu-id="f949f-693">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="f949f-694">存取 Razor Pages 頁面或 MVC 檢視中的設定</span><span class="sxs-lookup"><span data-stu-id="f949f-694">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="f949f-695">若要在 [Razor Pages 頁面] 頁面或 MVC 檢視中存取組態設定，請針對 [Microsoft.Extensions.Configuration 命名空間](xref:Microsoft.Extensions.Configuration) [using 指示詞](xref:mvc/views/razor#using) ([C# 參考：using 指示詞](/dotnet/csharp/language-reference/keywords/using-directive))，並將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 插入到頁面或檢視中。</span><span class="sxs-lookup"><span data-stu-id="f949f-695">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="f949f-696">在 [Razor 頁面] 頁面中：</span><span class="sxs-lookup"><span data-stu-id="f949f-696">In a Razor Pages page:</span></span>

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

<span data-ttu-id="f949f-697">在 MVC 檢視中：</span><span class="sxs-lookup"><span data-stu-id="f949f-697">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="f949f-698">從外部組件新增設定</span><span class="sxs-lookup"><span data-stu-id="f949f-698">Add configuration from an external assembly</span></span>

<span data-ttu-id="f949f-699"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。</span><span class="sxs-lookup"><span data-stu-id="f949f-699">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="f949f-700">如需詳細資訊，請參閱<xref:fundamentals/configuration/platform-specific-configuration>。</span><span class="sxs-lookup"><span data-stu-id="f949f-700">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f949f-701">其他資源</span><span class="sxs-lookup"><span data-stu-id="f949f-701">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="f949f-702">深入了解 Microsoft 設定</span><span class="sxs-lookup"><span data-stu-id="f949f-702">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
