---
title: ASP.NET Core 的設定
author: guardrex
description: 了解如何使用組態 API 設定 ASP.NET Core 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 288f8ba5b45cdecd8c9eae060fee2c2c25dec7f9
ms.sourcegitcommit: 4cd8dce371d63a66d780e4af1baab2bcf9d61b24
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2018
ms.locfileid: "43893242"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="d73a3-103">ASP.NET Core 的設定</span><span class="sxs-lookup"><span data-stu-id="d73a3-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="d73a3-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d73a3-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d73a3-105">ASP.NET Core 中的應用程式設定是以由*設定提供者*所建立的機碼值組為基礎。</span><span class="sxs-lookup"><span data-stu-id="d73a3-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="d73a3-106">設定提供者會從各種設定來源將設定資料讀取到機碼值組中：</span><span class="sxs-lookup"><span data-stu-id="d73a3-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="d73a3-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="d73a3-107">Azure Key Vault</span></span>
* <span data-ttu-id="d73a3-108">命令列引數</span><span class="sxs-lookup"><span data-stu-id="d73a3-108">Command-line arguments</span></span>
* <span data-ttu-id="d73a3-109">自訂提供者 (已安裝或已建立)</span><span class="sxs-lookup"><span data-stu-id="d73a3-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="d73a3-110">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="d73a3-110">Directory files</span></span>
* <span data-ttu-id="d73a3-111">環境變數</span><span class="sxs-lookup"><span data-stu-id="d73a3-111">Environment variables</span></span>
* <span data-ttu-id="d73a3-112">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="d73a3-112">In-memory .NET objects</span></span>
* <span data-ttu-id="d73a3-113">設定檔</span><span class="sxs-lookup"><span data-stu-id="d73a3-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="d73a3-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="d73a3-114">Azure Key Vault</span></span>
* <span data-ttu-id="d73a3-115">命令列引數</span><span class="sxs-lookup"><span data-stu-id="d73a3-115">Command-line arguments</span></span>
* <span data-ttu-id="d73a3-116">自訂提供者 (已安裝或已建立)</span><span class="sxs-lookup"><span data-stu-id="d73a3-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="d73a3-117">環境變數</span><span class="sxs-lookup"><span data-stu-id="d73a3-117">Environment variables</span></span>
* <span data-ttu-id="d73a3-118">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="d73a3-118">In-memory .NET objects</span></span>
* <span data-ttu-id="d73a3-119">設定檔</span><span class="sxs-lookup"><span data-stu-id="d73a3-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="d73a3-120">命令列引數</span><span class="sxs-lookup"><span data-stu-id="d73a3-120">Command-line arguments</span></span>
* <span data-ttu-id="d73a3-121">自訂提供者 (已安裝或已建立)</span><span class="sxs-lookup"><span data-stu-id="d73a3-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="d73a3-122">環境變數</span><span class="sxs-lookup"><span data-stu-id="d73a3-122">Environment variables</span></span>
* <span data-ttu-id="d73a3-123">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="d73a3-123">In-memory .NET objects</span></span>
* <span data-ttu-id="d73a3-124">設定檔</span><span class="sxs-lookup"><span data-stu-id="d73a3-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="d73a3-125">*選項模式*是此主題中所述之設定概念的延伸。</span><span class="sxs-lookup"><span data-stu-id="d73a3-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="d73a3-126">選項使用類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="d73a3-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="d73a3-127">如需使用選項模式的詳細資訊，請參閱 <xref:fundamentals/configuration/options>。</span><span class="sxs-lookup"><span data-stu-id="d73a3-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="d73a3-128">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d73a3-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="d73a3-129">此主題中提供的範例需要：</span><span class="sxs-lookup"><span data-stu-id="d73a3-129">The examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="d73a3-130">使用 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 設定應用程式的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="d73a3-130">Setting the base path of the app with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="d73a3-131">參考 [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) 套件即可讓應用程式使用 `SetBasePath`。</span><span class="sxs-lookup"><span data-stu-id="d73a3-131">`SetBasePath` is made available to an app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="d73a3-132">使用 <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> 解析設定檔的區段。</span><span class="sxs-lookup"><span data-stu-id="d73a3-132">Resolving sections of configuration files with <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span></span> <span data-ttu-id="d73a3-133">參考 [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) 套件即可讓應用程式使用 `GetSection`。</span><span class="sxs-lookup"><span data-stu-id="d73a3-133">`GetSection` is made available to an app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="d73a3-134">使用 <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 與 [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 將設定繫結到 .NET 類別。</span><span class="sxs-lookup"><span data-stu-id="d73a3-134">Binding configuration to .NET classes with <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> and [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span></span> <span data-ttu-id="d73a3-135">參考 [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) 套件即可讓應用程式使用 `Bind` 與 `Get<T>`。</span><span class="sxs-lookup"><span data-stu-id="d73a3-135">`Bind` and `Get<T>` are made available to an app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span> <span data-ttu-id="d73a3-136">ASP.NET Core 1.1 或更新版本中提供了 `Get<T>`。</span><span class="sxs-lookup"><span data-stu-id="d73a3-136">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d73a3-137">這些套件均包含在 [Microsoft.AspNetCore.App j5/ 中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="d73a3-137">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="d73a3-138">這三個套件都包含在 [Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)中。</span><span class="sxs-lookup"><span data-stu-id="d73a3-138">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="d73a3-139">主機與應用程式設定的比較</span><span class="sxs-lookup"><span data-stu-id="d73a3-139">Host vs. app configuration</span></span>

<span data-ttu-id="d73a3-140">設定及啟動應用程式之前，會先設定及啟動「主機」。</span><span class="sxs-lookup"><span data-stu-id="d73a3-140">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="d73a3-141">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="d73a3-141">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="d73a3-142">應用程式與主機都是使用此主題中所述的設定提供者來設定的。</span><span class="sxs-lookup"><span data-stu-id="d73a3-142">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="d73a3-143">主機設定機碼值組會成為應用程式全域設定的一部分。</span><span class="sxs-lookup"><span data-stu-id="d73a3-143">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="d73a3-144">如需有關當建置主機時如何使用設定提供者的詳細資訊，以及設定來源如何影響主機設定的詳細資訊，請參閱 <xref:fundamentals/host/index>。</span><span class="sxs-lookup"><span data-stu-id="d73a3-144">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="security"></a><span data-ttu-id="d73a3-145">安全性</span><span class="sxs-lookup"><span data-stu-id="d73a3-145">Security</span></span>

<span data-ttu-id="d73a3-146">採用下列最佳做法：</span><span class="sxs-lookup"><span data-stu-id="d73a3-146">Adopt the following best practices:</span></span>

* <span data-ttu-id="d73a3-147">永遠不要將密碼或其他敏感性資料儲存在設定提供者程式碼或純文字設定檔中。</span><span class="sxs-lookup"><span data-stu-id="d73a3-147">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="d73a3-148">不要在開發或測試環境中使用生產環境祕密。</span><span class="sxs-lookup"><span data-stu-id="d73a3-148">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="d73a3-149">請在專案外部指定祕密，以防止其意外認可至開放原始碼存放庫。</span><span class="sxs-lookup"><span data-stu-id="d73a3-149">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="d73a3-150">深入了解[如何使用多個環境](xref:fundamentals/environments)及[使用祕密管理員管理開發中的應用程式祕密安全儲存體](xref:security/app-secrets) (包括有關使用環境變數來存放敏感性資料的建議)。</span><span class="sxs-lookup"><span data-stu-id="d73a3-150">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="d73a3-151">「祕密管理員」使用「檔案設定提供者」以 JSON 檔案在本機系統上存放使用者祕密。</span><span class="sxs-lookup"><span data-stu-id="d73a3-151">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="d73a3-152">此主題稍後將說明「檔案設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="d73a3-152">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="d73a3-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 是安全儲存應用程式祕密的一個選項。</span><span class="sxs-lookup"><span data-stu-id="d73a3-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="d73a3-154">如需詳細資訊，請參閱<xref:security/key-vault-configuration>。</span><span class="sxs-lookup"><span data-stu-id="d73a3-154">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="d73a3-155">階層式設定資料</span><span class="sxs-lookup"><span data-stu-id="d73a3-155">Hierarchical configuration data</span></span>

<span data-ttu-id="d73a3-156">設定 API 可在設定機碼中使用分隔符號來壓平合併階層式資料，以管理階層式設定資料。</span><span class="sxs-lookup"><span data-stu-id="d73a3-156">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="d73a3-157">在下列 JSON 檔案中，兩個區段的結構式階層中有四個機碼存在：</span><span class="sxs-lookup"><span data-stu-id="d73a3-157">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="d73a3-158">當該檔案被讀入設定之後，會建立唯一機碼來維護設定來源的原始階層式資料結構。</span><span class="sxs-lookup"><span data-stu-id="d73a3-158">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="d73a3-159">系統會使用冒號 (`:`) 將區段與機碼壓平合併以維護原始結構：</span><span class="sxs-lookup"><span data-stu-id="d73a3-159">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="d73a3-160">section0:key0</span><span class="sxs-lookup"><span data-stu-id="d73a3-160">section0:key0</span></span>
* <span data-ttu-id="d73a3-161">section0:key1</span><span class="sxs-lookup"><span data-stu-id="d73a3-161">section0:key1</span></span>
* <span data-ttu-id="d73a3-162">section1:key0</span><span class="sxs-lookup"><span data-stu-id="d73a3-162">section1:key0</span></span>
* <span data-ttu-id="d73a3-163">section1:key1</span><span class="sxs-lookup"><span data-stu-id="d73a3-163">section1:key1</span></span>

<span data-ttu-id="d73a3-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> 與 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> 方法可用來在設定資料中隔離區段與區段的子系。</span><span class="sxs-lookup"><span data-stu-id="d73a3-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="d73a3-165">[GetSection,、 GetChildren 與 Exists](#getsection-getchildren-and-exists) 中說明這些方法。</span><span class="sxs-lookup"><span data-stu-id="d73a3-165">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="d73a3-166">慣例</span><span class="sxs-lookup"><span data-stu-id="d73a3-166">Conventions</span></span>

<span data-ttu-id="d73a3-167">在應用程式啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="d73a3-167">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="d73a3-168">「檔案設定提供者」可以在應用程式啟動之後且底層設定檔案變更時重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="d73a3-168">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="d73a3-169">此主題稍後將說明「檔案設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="d73a3-169">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="d73a3-170">您可以在應用程式的[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器中找到 <xref:Microsoft.Extensions.Configuration.IConfiguration>。</span><span class="sxs-lookup"><span data-stu-id="d73a3-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="d73a3-171">設定提供者無法使用 DI，因為當它們由主機設定時，它無法使用。</span><span class="sxs-lookup"><span data-stu-id="d73a3-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="d73a3-172">設定機碼會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="d73a3-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="d73a3-173">機碼區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="d73a3-173">Keys are case-insensitive.</span></span> <span data-ttu-id="d73a3-174">例如，`ConnectionString` 與 `connectionstring` 會被視為相等的機碼。</span><span class="sxs-lookup"><span data-stu-id="d73a3-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="d73a3-175">若相同機碼的值是由相同或不同設定提供者設定，在機碼上設定的最後一個值是所使用的值。</span><span class="sxs-lookup"><span data-stu-id="d73a3-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="d73a3-176">階層式機碼</span><span class="sxs-lookup"><span data-stu-id="d73a3-176">Hierarchical keys</span></span>
  * <span data-ttu-id="d73a3-177">在設定 API 內，冒號分隔字元 (`:`) 可在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="d73a3-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="d73a3-178">在環境變數中，冒號分隔字元可能無法在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="d73a3-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="d73a3-179">所有平台都支援雙底線 (`__`)，而且它會被轉換為冒號。</span><span class="sxs-lookup"><span data-stu-id="d73a3-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="d73a3-180">在 Azure Key Vault 中，階層式機碼使用 `--` (兩個破折號) 來做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="d73a3-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="d73a3-181">您必須提供程式碼，在祕密載入到應用程式的設定時將破折號取代為冒號。</span><span class="sxs-lookup"><span data-stu-id="d73a3-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="d73a3-182"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="d73a3-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="d73a3-183">[將陣列繫結到類別](#bind-an-array-to-a-class)一節說明陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="d73a3-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="d73a3-184">設定值會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="d73a3-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="d73a3-185">值是字串。</span><span class="sxs-lookup"><span data-stu-id="d73a3-185">Values are strings.</span></span>
* <span data-ttu-id="d73a3-186">Null 值無法存放在設定中或繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="d73a3-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="d73a3-187">提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-187">Providers</span></span>

<span data-ttu-id="d73a3-188">下表顯示可供 ASP.NET Core 應用程式使用的設定提供者。</span><span class="sxs-lookup"><span data-stu-id="d73a3-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="d73a3-189">提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-189">Provider</span></span> | <span data-ttu-id="d73a3-190">從&hellip;提供設定</span><span class="sxs-lookup"><span data-stu-id="d73a3-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="d73a3-191">[Azure Key Vault 設定提供者](xref:security/key-vault-configuration) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="d73a3-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="d73a3-192">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="d73a3-192">Azure Key Vault</span></span> |
| [<span data-ttu-id="d73a3-193">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-193">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="d73a3-194">命令列參數</span><span class="sxs-lookup"><span data-stu-id="d73a3-194">Command-line parameters</span></span> |
| [<span data-ttu-id="d73a3-195">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-195">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="d73a3-196">自訂來源</span><span class="sxs-lookup"><span data-stu-id="d73a3-196">Custom source</span></span> |
| [<span data-ttu-id="d73a3-197">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-197">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="d73a3-198">環境變數</span><span class="sxs-lookup"><span data-stu-id="d73a3-198">Environment variables</span></span> |
| [<span data-ttu-id="d73a3-199">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-199">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="d73a3-200">檔案 (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="d73a3-200">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="d73a3-201">每個檔案機碼的設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-201">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="d73a3-202">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="d73a3-202">Directory files</span></span> |
| [<span data-ttu-id="d73a3-203">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-203">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="d73a3-204">記憶體內集合</span><span class="sxs-lookup"><span data-stu-id="d73a3-204">In-memory collections</span></span> |
| <span data-ttu-id="d73a3-205">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="d73a3-205">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="d73a3-206">使用者設定檔目錄中的檔案</span><span class="sxs-lookup"><span data-stu-id="d73a3-206">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="d73a3-207">提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-207">Provider</span></span> | <span data-ttu-id="d73a3-208">從&hellip;提供設定</span><span class="sxs-lookup"><span data-stu-id="d73a3-208">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="d73a3-209">[Azure Key Vault 設定提供者](xref:security/key-vault-configuration) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="d73a3-209">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="d73a3-210">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="d73a3-210">Azure Key Vault</span></span> |
| [<span data-ttu-id="d73a3-211">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="d73a3-212">命令列參數</span><span class="sxs-lookup"><span data-stu-id="d73a3-212">Command-line parameters</span></span> |
| [<span data-ttu-id="d73a3-213">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="d73a3-214">自訂來源</span><span class="sxs-lookup"><span data-stu-id="d73a3-214">Custom source</span></span> |
| [<span data-ttu-id="d73a3-215">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-215">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="d73a3-216">環境變數</span><span class="sxs-lookup"><span data-stu-id="d73a3-216">Environment variables</span></span> |
| [<span data-ttu-id="d73a3-217">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="d73a3-218">檔案 (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="d73a3-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="d73a3-219">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-219">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="d73a3-220">記憶體內集合</span><span class="sxs-lookup"><span data-stu-id="d73a3-220">In-memory collections</span></span> |
| <span data-ttu-id="d73a3-221">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="d73a3-221">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="d73a3-222">使用者設定檔目錄中的檔案</span><span class="sxs-lookup"><span data-stu-id="d73a3-222">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="d73a3-223">提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-223">Provider</span></span> | <span data-ttu-id="d73a3-224">從&hellip;提供設定</span><span class="sxs-lookup"><span data-stu-id="d73a3-224">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="d73a3-225">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-225">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="d73a3-226">命令列參數</span><span class="sxs-lookup"><span data-stu-id="d73a3-226">Command-line parameters</span></span> |
| [<span data-ttu-id="d73a3-227">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-227">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="d73a3-228">自訂來源</span><span class="sxs-lookup"><span data-stu-id="d73a3-228">Custom source</span></span> |
| [<span data-ttu-id="d73a3-229">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-229">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="d73a3-230">環境變數</span><span class="sxs-lookup"><span data-stu-id="d73a3-230">Environment variables</span></span> |
| [<span data-ttu-id="d73a3-231">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-231">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="d73a3-232">檔案 (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="d73a3-232">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="d73a3-233">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-233">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="d73a3-234">記憶體內集合</span><span class="sxs-lookup"><span data-stu-id="d73a3-234">In-memory collections</span></span> |
| <span data-ttu-id="d73a3-235">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="d73a3-235">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="d73a3-236">使用者設定檔目錄中的檔案</span><span class="sxs-lookup"><span data-stu-id="d73a3-236">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="d73a3-237">在啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="d73a3-237">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="d73a3-238">此主題中所述的設定提供者是以字母順序描述，而非以您的程式碼安排它們的順序。</span><span class="sxs-lookup"><span data-stu-id="d73a3-238">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="d73a3-239">在您的程式碼中針對底層設定來源的優先順序，為設定提供者排序。</span><span class="sxs-lookup"><span data-stu-id="d73a3-239">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="d73a3-240">典型的設定提供者順序是：</span><span class="sxs-lookup"><span data-stu-id="d73a3-240">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="d73a3-241">檔案 (*appsettings.json*、*appsettings.&lt;Environment&gt;.json*，其中 `<Environment>` 是應用程式的目前裝載環境)</span><span class="sxs-lookup"><span data-stu-id="d73a3-241">Files (*appsettings.json*, *appsettings.&lt;Environment&gt;.json*, where `<Environment>` is the app's current hosting environment)</span></span>
1. <span data-ttu-id="d73a3-242">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (僅限開發環境)</span><span class="sxs-lookup"><span data-stu-id="d73a3-242">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="d73a3-243">環境變數</span><span class="sxs-lookup"><span data-stu-id="d73a3-243">Environment variables</span></span>
1. <span data-ttu-id="d73a3-244">命令列引數</span><span class="sxs-lookup"><span data-stu-id="d73a3-244">Command-line arguments</span></span>

<span data-ttu-id="d73a3-245">將命令列設定提供者放在提供者序列結尾是常見作法，因為這樣可以讓命令列引數覆寫由其它提供者所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="d73a3-245">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d73a3-246">當您使用 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>初始化新的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 時，就會有這個提供者順序。</span><span class="sxs-lookup"><span data-stu-id="d73a3-246">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="d73a3-247">如需詳細資訊，請參閱 [Web主機：設定主機](xref:fundamentals/host/web-host#set-up-a-host)。</span><span class="sxs-lookup"><span data-stu-id="d73a3-247">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="d73a3-248">建置 Web 主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定提供者：</span><span class="sxs-lookup"><span data-stu-id="d73a3-248">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the Web Host to specify the app's configuration providers:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

<span data-ttu-id="d73a3-249">ASP.NET Core 2.1 或更新版本中提供了 `ConfigureAppConfiguration` *。*</span><span class="sxs-lookup"><span data-stu-id="d73a3-249">`ConfigureAppConfiguration` *is available in ASP.NET Core 2.1 or later.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d73a3-250">您可以使用 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 並在 `Startup` <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> 方法，以為應用程式 (而非主機) 建立提供者順序：</span><span class="sxs-lookup"><span data-stu-id="d73a3-250">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

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

<span data-ttu-id="d73a3-251">在上面的範例中，環境名稱 (`env.EnvironmentName`) 與應用程式組件名稱 (`env.ApplicationName`) 是由 <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> 提供。</span><span class="sxs-lookup"><span data-stu-id="d73a3-251">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="d73a3-252">如需詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="d73a3-252">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="d73a3-253">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-253">Command-line Configuration Provider</span></span>

<span data-ttu-id="d73a3-254"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> 會在執行階段從命令列引數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="d73a3-254">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d73a3-255">若要啟用命令列設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="d73a3-255">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d73a3-256">當您使用 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 初始化新的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 時，會自動呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="d73a3-256">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="d73a3-257">如需詳細資訊，請參閱 [Web主機：設定主機](xref:fundamentals/host/web-host#set-up-a-host)。</span><span class="sxs-lookup"><span data-stu-id="d73a3-257">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="d73a3-258">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="d73a3-258">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="d73a3-259">從 *appsettings.json* 與 *appsettings.&lt;Environment&gt;.json* 載入其他選擇性設定。</span><span class="sxs-lookup"><span data-stu-id="d73a3-259">Optional configuration from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>
* <span data-ttu-id="d73a3-260">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (在開發環境中)。</span><span class="sxs-lookup"><span data-stu-id="d73a3-260">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="d73a3-261">環境變數。</span><span class="sxs-lookup"><span data-stu-id="d73a3-261">Environment variables.</span></span>

<span data-ttu-id="d73a3-262">`CreateDefaultBuilder` 會最後才新增命令列設定提供者。</span><span class="sxs-lookup"><span data-stu-id="d73a3-262">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="d73a3-263">在執行階段傳遞的命令列引數會覆寫由其它提供者所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="d73a3-263">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="d73a3-264">`CreateDefaultBuilder` 會在建構主機時執行作業。</span><span class="sxs-lookup"><span data-stu-id="d73a3-264">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="d73a3-265">因此，由`CreateDefaultBuilder` 啟用的命令列設定可以影響主機的設定方式。</span><span class="sxs-lookup"><span data-stu-id="d73a3-265">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="d73a3-266">手動建置主機而且未呼叫 `CreateDefaultBuilder` 時，請呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 執行個體的 `AddCommandLine` 延伸模組方法：</span><span class="sxs-lookup"><span data-stu-id="d73a3-266">When building the host manually and not calling `CreateDefaultBuilder`, call the `AddCommandLine` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d73a3-267">若要啟用命令列設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="d73a3-267">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d73a3-268">最後才呼叫提供者，以允許在執行階段傳遞的命令列引數覆寫由其他設定提供者所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="d73a3-268">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="d73a3-269">使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="d73a3-269">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="d73a3-270">**範例**</span><span class="sxs-lookup"><span data-stu-id="d73a3-270">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d73a3-271">2.x 範例應用程式可發揮靜態方便方法 `CreateDefaultBuilder` 的優勢以建置主機，這包括對 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="d73a3-271">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d73a3-272">1.x 範例應用程式會呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 上的 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>。</span><span class="sxs-lookup"><span data-stu-id="d73a3-272">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="d73a3-273">從專案目錄中開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="d73a3-273">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="d73a3-274">提供命令列引數給 `dotnet run` 命令 `dotnet run CommandLineKey=CommandLineValue`。</span><span class="sxs-lookup"><span data-stu-id="d73a3-274">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="d73a3-275">在應用程式執行之後，開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d73a3-275">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="d73a3-276">觀察輸出是否包含提供給 `dotnet run` 之設定命令列引數的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="d73a3-276">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="d73a3-277">引數</span><span class="sxs-lookup"><span data-stu-id="d73a3-277">Arguments</span></span>

<span data-ttu-id="d73a3-278">當值後面接著空格時，值必須接著等號 (`=`)，或機碼必須有前置詞 (`--` 或 `/`)。</span><span class="sxs-lookup"><span data-stu-id="d73a3-278">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="d73a3-279">若使用等號 (例如 `CommandLineKey=`)，值可以是 Null。</span><span class="sxs-lookup"><span data-stu-id="d73a3-279">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="d73a3-280">索引鍵前置字元</span><span class="sxs-lookup"><span data-stu-id="d73a3-280">Key prefix</span></span>               | <span data-ttu-id="d73a3-281">範例</span><span class="sxs-lookup"><span data-stu-id="d73a3-281">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="d73a3-282">沒有前置字元</span><span class="sxs-lookup"><span data-stu-id="d73a3-282">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="d73a3-283">雙虛線 (`--`)</span><span class="sxs-lookup"><span data-stu-id="d73a3-283">Two dashes (`--`)</span></span>        | <span data-ttu-id="d73a3-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="d73a3-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="d73a3-285">正斜線 (`/`)</span><span class="sxs-lookup"><span data-stu-id="d73a3-285">Forward slash (`/`)</span></span>      | <span data-ttu-id="d73a3-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="d73a3-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="d73a3-287">在相同的命令中，請不要混合使用等號搭配使用空格之機碼值組的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="d73a3-287">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="d73a3-288">範例命令：</span><span class="sxs-lookup"><span data-stu-id="d73a3-288">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value --CommandLineKey2=value /CommandLineKey2=value
dotnet run --CommandLineKey1 value /CommandLineKey2 value
dotnet run CommandLineKey1= CommandLineKey2=value
```

### <a name="switch-mappings"></a><span data-ttu-id="d73a3-289">切換對應</span><span class="sxs-lookup"><span data-stu-id="d73a3-289">Switch mappings</span></span>

<span data-ttu-id="d73a3-290">參數對應允許索引鍵名稱取代邏輯。</span><span class="sxs-lookup"><span data-stu-id="d73a3-290">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="d73a3-291">當您使用 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 手動建置設定時，可以提供切換取代的字典給 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 方法。</span><span class="sxs-lookup"><span data-stu-id="d73a3-291">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="d73a3-292">使用切換對應字典時，會檢查字典中是否有任何索引鍵符合命令列引數所提供的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="d73a3-292">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="d73a3-293">如果在字典中找到命令列索引鍵，則會傳回字典值 (索引鍵取代) 以在應用程式的設定中設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="d73a3-293">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="d73a3-294">所有前面加上單虛線 (`-`) 的命令列索引鍵都需要切換對應。</span><span class="sxs-lookup"><span data-stu-id="d73a3-294">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="d73a3-295">切換對應字典索引鍵規則：</span><span class="sxs-lookup"><span data-stu-id="d73a3-295">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="d73a3-296">切換必須以單虛線 (`-`) 或雙虛線 (`--`) 開頭。</span><span class="sxs-lookup"><span data-stu-id="d73a3-296">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="d73a3-297">切換對應字典不能包含重複索引鍵。</span><span class="sxs-lookup"><span data-stu-id="d73a3-297">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.0"

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

        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="d73a3-298">如上面的範例所示，當使用切換對應時，對 `CreateDefaultBuilder` 的呼叫不應該傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="d73a3-298">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="d73a3-299">`CreateDefaultBuilder` 方法的 `AddCommandLine` 呼叫不包括對應的切換，而且沒有任何方式可以將切換對應字典傳遞給 `CreateDefaultBuilder`。</span><span class="sxs-lookup"><span data-stu-id="d73a3-299">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="d73a3-300">若對應切換中包含引數且已傳遞給 `CreateDefaultBuilder`，其 `AddCommandLine` 提供者會無法使用 <xref:System.FormatException> 來初始化。</span><span class="sxs-lookup"><span data-stu-id="d73a3-300">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="d73a3-301">解決方式是不要傳遞引數給 `CreateDefaultBuilder`，而是允許 `ConfigurationBuilder` 方法的 `AddCommandLine` 方法同時處理引數與切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="d73a3-301">The solution is not to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="d73a3-302">建立切換對應字典之後，它會包含下表中所示的資料。</span><span class="sxs-lookup"><span data-stu-id="d73a3-302">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="d73a3-303">Key</span><span class="sxs-lookup"><span data-stu-id="d73a3-303">Key</span></span>       | <span data-ttu-id="d73a3-304">值</span><span class="sxs-lookup"><span data-stu-id="d73a3-304">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="d73a3-305">若啟動應用程式時使用切換對應機碼，設定會接收由字典提供之機碼上的設定值：</span><span class="sxs-lookup"><span data-stu-id="d73a3-305">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="d73a3-306">執行上述命令之後，設定包含下表中顯示的值。</span><span class="sxs-lookup"><span data-stu-id="d73a3-306">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="d73a3-307">Key</span><span class="sxs-lookup"><span data-stu-id="d73a3-307">Key</span></span>               | <span data-ttu-id="d73a3-308">值</span><span class="sxs-lookup"><span data-stu-id="d73a3-308">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="d73a3-309">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-309">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="d73a3-310"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> 會在執行階段從環境變數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="d73a3-310">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="d73a3-311">若要啟用環境變數設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="d73a3-311">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d73a3-312">在環境變數中搭配階層式機碼使用時，冒號分隔字元 (`:`) 可能無法在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="d73a3-312">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="d73a3-313">所有平台都支援雙底線 (`__`)，而且它會被冒號取代。</span><span class="sxs-lookup"><span data-stu-id="d73a3-313">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="d73a3-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) 允許您在 Azure 入口網站中設定環境變數，以使用「環境變數設定提供者」覆寫應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="d73a3-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="d73a3-315">如需詳細資訊，請參閱 [Azure App：使用 Azure 入口網站覆寫應用程式設定](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)。</span><span class="sxs-lookup"><span data-stu-id="d73a3-315">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d73a3-316">當您使用 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 初始化新的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 時，會自動呼叫 `AddEnvironmentVariables`。</span><span class="sxs-lookup"><span data-stu-id="d73a3-316">`AddEnvironmentVariables` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="d73a3-317">如需詳細資訊，請參閱 [Web主機：設定主機](xref:fundamentals/host/web-host#set-up-a-host)。</span><span class="sxs-lookup"><span data-stu-id="d73a3-317">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="d73a3-318">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="d73a3-318">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="d73a3-319">從 *appsettings.json* 與 *appsettings.&lt;Environment&gt;.json* 載入其他選擇性設定。</span><span class="sxs-lookup"><span data-stu-id="d73a3-319">Optional configuration from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>
* <span data-ttu-id="d73a3-320">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (在開發環境中)。</span><span class="sxs-lookup"><span data-stu-id="d73a3-320">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="d73a3-321">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="d73a3-321">Command-line arguments.</span></span>

<span data-ttu-id="d73a3-322">從使用者祕密與 *appsettings* 檔案建立設定之後，會呼叫「環境變數設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="d73a3-322">The Environment Variable Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="d73a3-323">在此位置呼叫提供者可讓系統在執行階段讀取環境變數，以覆寫由使用者祕密與 *appsettings* 檔案所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="d73a3-323">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="d73a3-324">您也可以直接呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 執行個體上的 `AddEnvironmentVariables` 延伸模組方法：</span><span class="sxs-lookup"><span data-stu-id="d73a3-324">You can also directly call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d73a3-325">使用 `UseConfiguration` 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="d73a3-325">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="d73a3-326">**範例**</span><span class="sxs-lookup"><span data-stu-id="d73a3-326">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d73a3-327">2.x 範例應用程式可發揮靜態方便方法 `CreateDefaultBuilder` 的優勢以建置主機，這包括對 `AddEnvironmentVariables` 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="d73a3-327">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d73a3-328">1.x 範例應用程式會呼叫 `ConfigurationBuilder` 上的 `AddEnvironmentVariables`。</span><span class="sxs-lookup"><span data-stu-id="d73a3-328">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="d73a3-329">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="d73a3-329">Run the sample app.</span></span> <span data-ttu-id="d73a3-330">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d73a3-330">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="d73a3-331">觀察輸出是否包含環境變數 `ENVIRONMENT` 的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="d73a3-331">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="d73a3-332">值反映應用程式執行所在環境，在本機執行時，通常是 `Development`。</span><span class="sxs-lookup"><span data-stu-id="d73a3-332">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="d73a3-333">為縮短由應用程式轉譯的環境變數清單，應用程式會篩選開頭如下的環境變數：</span><span class="sxs-lookup"><span data-stu-id="d73a3-333">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="d73a3-334">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="d73a3-334">ASPNETCORE_</span></span>
* <span data-ttu-id="d73a3-335">urls</span><span class="sxs-lookup"><span data-stu-id="d73a3-335">urls</span></span>
* <span data-ttu-id="d73a3-336">記錄</span><span class="sxs-lookup"><span data-stu-id="d73a3-336">Logging</span></span>
* <span data-ttu-id="d73a3-337">環境</span><span class="sxs-lookup"><span data-stu-id="d73a3-337">ENVIRONMENT</span></span>
* <span data-ttu-id="d73a3-338">contentRoot</span><span class="sxs-lookup"><span data-stu-id="d73a3-338">contentRoot</span></span>
* <span data-ttu-id="d73a3-339">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="d73a3-339">AllowedHosts</span></span>
* <span data-ttu-id="d73a3-340">applicationName</span><span class="sxs-lookup"><span data-stu-id="d73a3-340">applicationName</span></span>
* <span data-ttu-id="d73a3-341">CommandLine</span><span class="sxs-lookup"><span data-stu-id="d73a3-341">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d73a3-342">若想要將所有環境變數公開給應用程式使用，請將 *Pages/Index.cshtml.cs* 中的 `FilteredConfiguration` 變更為下面這樣：</span><span class="sxs-lookup"><span data-stu-id="d73a3-342">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d73a3-343">若想要將所有環境變數公開給應用程式使用，請將 *Controllers/HomeController.cs* 中的 `FilteredConfiguration` 變更為下面這樣：</span><span class="sxs-lookup"><span data-stu-id="d73a3-343">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="d73a3-344">首碼</span><span class="sxs-lookup"><span data-stu-id="d73a3-344">Prefixes</span></span>

<span data-ttu-id="d73a3-345">當您將前置詞套用到 `AddEnvironmentVariables` 方法時，會篩選載入到應用程式設定中的環境變數。</span><span class="sxs-lookup"><span data-stu-id="d73a3-345">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="d73a3-346">例如，若要篩選前置詞為 `CUSTOM_` 的環境變數，請提供前置詞給設定提供者：</span><span class="sxs-lookup"><span data-stu-id="d73a3-346">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="d73a3-347">建立設定機碼值組時，會移除前置詞。</span><span class="sxs-lookup"><span data-stu-id="d73a3-347">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d73a3-348">靜態方便方法 `CreateDefaultBuilder` 會建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 以建立應用程式的主機。</span><span class="sxs-lookup"><span data-stu-id="d73a3-348">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="d73a3-349">當 `WebHostBuilder` 建立時，它會在前置詞為 `ASPNETCORE_` 的環境變數中尋找其主機設定。</span><span class="sxs-lookup"><span data-stu-id="d73a3-349">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="d73a3-350">**連接字串前置詞**</span><span class="sxs-lookup"><span data-stu-id="d73a3-350">**Connection string prefixes**</span></span>

<span data-ttu-id="d73a3-351">設定 API 四個連接字串環境變數的特殊處理規則，這些這些環境變數牽涉到針對應用程式環境設定 Azure 連接字串。</span><span class="sxs-lookup"><span data-stu-id="d73a3-351">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="d73a3-352">若將前置詞提供給 `AddEnvironmentVariables`具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="d73a3-352">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="d73a3-353">連接字串前置詞</span><span class="sxs-lookup"><span data-stu-id="d73a3-353">Connection string prefix</span></span> | <span data-ttu-id="d73a3-354">提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-354">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="d73a3-355">自訂提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-355">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="d73a3-356">MySQL</span><span class="sxs-lookup"><span data-stu-id="d73a3-356">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="d73a3-357">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="d73a3-357">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="d73a3-358">SQL Server</span><span class="sxs-lookup"><span data-stu-id="d73a3-358">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="d73a3-359">當探索到具有下表所顯示之任何四個前置詞的環境變數並將其載入到設定中時：</span><span class="sxs-lookup"><span data-stu-id="d73a3-359">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="d73a3-360">會透過移除環境變數前置詞並新增設定機碼區段 (`ConnectionStrings`) 來移除具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="d73a3-360">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="d73a3-361">會建立新的設定機碼值組以代表資料庫連線提供者 (`CUSTOMCONNSTR_` 除外，這沒有所述提供者)。</span><span class="sxs-lookup"><span data-stu-id="d73a3-361">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="d73a3-362">環境變數機碼</span><span class="sxs-lookup"><span data-stu-id="d73a3-362">Environment variable key</span></span> | <span data-ttu-id="d73a3-363">已轉換的設定機碼</span><span class="sxs-lookup"><span data-stu-id="d73a3-363">Converted configuration key</span></span> | <span data-ttu-id="d73a3-364">提供者設定項目</span><span class="sxs-lookup"><span data-stu-id="d73a3-364">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="d73a3-365">設定項目未建立。</span><span class="sxs-lookup"><span data-stu-id="d73a3-365">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="d73a3-366">機碼：`ConnectionStrings:<KEY>_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="d73a3-366">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="d73a3-367">值：`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="d73a3-367">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="d73a3-368">機碼：`ConnectionStrings:<KEY>_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="d73a3-368">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="d73a3-369">值：`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="d73a3-369">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="d73a3-370">機碼：`ConnectionStrings:<KEY>_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="d73a3-370">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="d73a3-371">值：`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="d73a3-371">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="d73a3-372">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-372">File Configuration Provider</span></span>

<span data-ttu-id="d73a3-373"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> 是用於從檔案系統載入設定的基底類別。</span><span class="sxs-lookup"><span data-stu-id="d73a3-373"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="d73a3-374">下列設定提供者專用於特定檔案類型：</span><span class="sxs-lookup"><span data-stu-id="d73a3-374">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="d73a3-375">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-375">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="d73a3-376">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-376">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="d73a3-377">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-377">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="d73a3-378">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-378">INI Configuration Provider</span></span>

<span data-ttu-id="d73a3-379"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> 會在執行階段從 INI 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="d73a3-379">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="d73a3-380">若要啟用 INI 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="d73a3-380">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d73a3-381">冒號可用來做為 INI 檔案設定中的區段分隔符號。</span><span class="sxs-lookup"><span data-stu-id="d73a3-381">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="d73a3-382">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="d73a3-382">Overloads permit specifying:</span></span>

* <span data-ttu-id="d73a3-383">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="d73a3-383">Whether the file is optional.</span></span>
* <span data-ttu-id="d73a3-384">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="d73a3-384">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="d73a3-385"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="d73a3-385">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d73a3-386">呼叫 `CreateDefaultBuilder` 時，請使用下列設定呼叫 `UseConfiguration`：</span><span class="sxs-lookup"><span data-stu-id="d73a3-386">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="d73a3-387">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 `UseConfiguration`：</span><span class="sxs-lookup"><span data-stu-id="d73a3-387">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d73a3-388">使用 `UseConfiguration` 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="d73a3-388">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="d73a3-389">INI 設定檔的一般範例：</span><span class="sxs-lookup"><span data-stu-id="d73a3-389">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="d73a3-390">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="d73a3-390">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="d73a3-391">section0:key0</span><span class="sxs-lookup"><span data-stu-id="d73a3-391">section0:key0</span></span>
* <span data-ttu-id="d73a3-392">section0:key1</span><span class="sxs-lookup"><span data-stu-id="d73a3-392">section0:key1</span></span>
* <span data-ttu-id="d73a3-393">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="d73a3-393">section1:subsection:key</span></span>
* <span data-ttu-id="d73a3-394">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="d73a3-394">section2:subsection0:key</span></span>
* <span data-ttu-id="d73a3-395">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="d73a3-395">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="d73a3-396">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-396">JSON Configuration Provider</span></span>

<span data-ttu-id="d73a3-397"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> 會在執行階段從 JSON 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="d73a3-397">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="d73a3-398">若要啟用 JSON 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="d73a3-398">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d73a3-399">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="d73a3-399">Overloads permit specifying:</span></span>

* <span data-ttu-id="d73a3-400">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="d73a3-400">Whether the file is optional.</span></span>
* <span data-ttu-id="d73a3-401">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="d73a3-401">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="d73a3-402"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="d73a3-402">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d73a3-403">當您使用 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 初始化新的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 時，會自動呼叫 `AddJsonFile` 兩次。</span><span class="sxs-lookup"><span data-stu-id="d73a3-403">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="d73a3-404">會呼叫此方法以從下列位置載入設定：</span><span class="sxs-lookup"><span data-stu-id="d73a3-404">The method is called to load configuration from:</span></span>

* <span data-ttu-id="d73a3-405">*appsettings.json* &ndash; 會先讀取此檔案。</span><span class="sxs-lookup"><span data-stu-id="d73a3-405">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="d73a3-406">檔案的環境版本可以覆寫由 *appsettings.json* 檔案提供的值。</span><span class="sxs-lookup"><span data-stu-id="d73a3-406">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="d73a3-407">*appsettings.&lt;Environment&gt;.json* &ndash; 檔案的環境版本是根據 [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*) 來載入的。</span><span class="sxs-lookup"><span data-stu-id="d73a3-407">*appsettings.&lt;Environment&gt;.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="d73a3-408">如需詳細資訊，請參閱 [Web主機：設定主機](xref:fundamentals/host/web-host#set-up-a-host)。</span><span class="sxs-lookup"><span data-stu-id="d73a3-408">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="d73a3-409">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="d73a3-409">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="d73a3-410">環境變數。</span><span class="sxs-lookup"><span data-stu-id="d73a3-410">Environment variables.</span></span>
* <span data-ttu-id="d73a3-411">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (在開發環境中)。</span><span class="sxs-lookup"><span data-stu-id="d73a3-411">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="d73a3-412">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="d73a3-412">Command-line arguments.</span></span>

<span data-ttu-id="d73a3-413">會先建立 JSON 設定提供者。</span><span class="sxs-lookup"><span data-stu-id="d73a3-413">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="d73a3-414">因此，使用者祕密、環境變數與命令列引數會覆寫由 *appsettings* 檔案設定的設定。</span><span class="sxs-lookup"><span data-stu-id="d73a3-414">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="d73a3-415">您也可以直接呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 執行個體上的 `AddJsonFile` 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="d73a3-415">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d73a3-416">呼叫 `CreateDefaultBuilder` 時，請使用下列設定呼叫 `UseConfiguration`：</span><span class="sxs-lookup"><span data-stu-id="d73a3-416">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="d73a3-417">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 `UseConfiguration`：</span><span class="sxs-lookup"><span data-stu-id="d73a3-417">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d73a3-418">使用 `UseConfiguration` 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="d73a3-418">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="d73a3-419">**範例**</span><span class="sxs-lookup"><span data-stu-id="d73a3-419">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d73a3-420">2.x 範例應用程式可發揮靜態方便方法 `CreateDefaultBuilder` 的優勢以建置主機，這包括對 `AddJsonFile` 的兩次呼叫。</span><span class="sxs-lookup"><span data-stu-id="d73a3-420">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="d73a3-421">從 *appsettings.json* 與 *appsettings.&lt;Environment&gt;.json* 載入設定。</span><span class="sxs-lookup"><span data-stu-id="d73a3-421">Configuration is loaded from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d73a3-422">1.x 範例應用程式會呼叫 `ConfigurationBuilder` 上的 `AddJsonFile` 兩次。</span><span class="sxs-lookup"><span data-stu-id="d73a3-422">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="d73a3-423">從 *appsettings.json* 與 *appsettings.&lt;Environment&gt;.json* 載入設定。</span><span class="sxs-lookup"><span data-stu-id="d73a3-423">Configuration is loaded from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="d73a3-424">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="d73a3-424">Run the sample app.</span></span> <span data-ttu-id="d73a3-425">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d73a3-425">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="d73a3-426">觀察輸出是否包含表格中所顯示之設定的機碼值組 (視環境而定)。</span><span class="sxs-lookup"><span data-stu-id="d73a3-426">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="d73a3-427">記錄設定機碼會使用冒號 (`:`) 做為階層式分隔符號。</span><span class="sxs-lookup"><span data-stu-id="d73a3-427">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="d73a3-428">Key</span><span class="sxs-lookup"><span data-stu-id="d73a3-428">Key</span></span>                        | <span data-ttu-id="d73a3-429">開發值</span><span class="sxs-lookup"><span data-stu-id="d73a3-429">Development Value</span></span> | <span data-ttu-id="d73a3-430">生產值</span><span class="sxs-lookup"><span data-stu-id="d73a3-430">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="d73a3-431">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="d73a3-431">Logging:LogLevel:System</span></span>    | <span data-ttu-id="d73a3-432">資訊</span><span class="sxs-lookup"><span data-stu-id="d73a3-432">Information</span></span>       | <span data-ttu-id="d73a3-433">資訊</span><span class="sxs-lookup"><span data-stu-id="d73a3-433">Information</span></span>      |
| <span data-ttu-id="d73a3-434">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="d73a3-434">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="d73a3-435">資訊</span><span class="sxs-lookup"><span data-stu-id="d73a3-435">Information</span></span>       | <span data-ttu-id="d73a3-436">資訊</span><span class="sxs-lookup"><span data-stu-id="d73a3-436">Information</span></span>      |
| <span data-ttu-id="d73a3-437">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="d73a3-437">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="d73a3-438">偵錯</span><span class="sxs-lookup"><span data-stu-id="d73a3-438">Debug</span></span>             | <span data-ttu-id="d73a3-439">錯誤</span><span class="sxs-lookup"><span data-stu-id="d73a3-439">Error</span></span>            |
| <span data-ttu-id="d73a3-440">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="d73a3-440">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="d73a3-441">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-441">XML Configuration Provider</span></span>

<span data-ttu-id="d73a3-442"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> 會在執行階段從 XML 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="d73a3-442">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="d73a3-443">若要啟用 XML 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="d73a3-443">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d73a3-444">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="d73a3-444">Overloads permit specifying:</span></span>

* <span data-ttu-id="d73a3-445">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="d73a3-445">Whether the file is optional.</span></span>
* <span data-ttu-id="d73a3-446">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="d73a3-446">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="d73a3-447"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="d73a3-447">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="d73a3-448">建立設定機碼值組時，會忽略設定檔案的根節點。</span><span class="sxs-lookup"><span data-stu-id="d73a3-448">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="d73a3-449">請勿在檔案中指定文件類型定義 (DTD) 或命名空間。</span><span class="sxs-lookup"><span data-stu-id="d73a3-449">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d73a3-450">呼叫 `CreateDefaultBuilder` 時，請使用下列設定呼叫 `UseConfiguration`：</span><span class="sxs-lookup"><span data-stu-id="d73a3-450">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="d73a3-451">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 `UseConfiguration`：</span><span class="sxs-lookup"><span data-stu-id="d73a3-451">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d73a3-452">使用 `UseConfiguration` 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="d73a3-452">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="d73a3-453">XML 設定檔可以針對重複的區段使用相異元素名稱：</span><span class="sxs-lookup"><span data-stu-id="d73a3-453">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="d73a3-454">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="d73a3-454">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="d73a3-455">section0:key0</span><span class="sxs-lookup"><span data-stu-id="d73a3-455">section0:key0</span></span>
* <span data-ttu-id="d73a3-456">section0:key1</span><span class="sxs-lookup"><span data-stu-id="d73a3-456">section0:key1</span></span>
* <span data-ttu-id="d73a3-457">section1:key0</span><span class="sxs-lookup"><span data-stu-id="d73a3-457">section1:key0</span></span>
* <span data-ttu-id="d73a3-458">section1:key1</span><span class="sxs-lookup"><span data-stu-id="d73a3-458">section1:key1</span></span>

<span data-ttu-id="d73a3-459">若 `name` 屬性是用來區別元素，則可以重複那些使用相同元素名稱的元素：</span><span class="sxs-lookup"><span data-stu-id="d73a3-459">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="d73a3-460">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="d73a3-460">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="d73a3-461">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="d73a3-461">section:section0:key:key0</span></span>
* <span data-ttu-id="d73a3-462">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="d73a3-462">section:section0:key:key1</span></span>
* <span data-ttu-id="d73a3-463">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="d73a3-463">section:section1:key:key0</span></span>
* <span data-ttu-id="d73a3-464">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="d73a3-464">section:section1:key:key1</span></span>

<span data-ttu-id="d73a3-465">屬性可用來提供值：</span><span class="sxs-lookup"><span data-stu-id="d73a3-465">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="d73a3-466">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="d73a3-466">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="d73a3-467">key:attribute</span><span class="sxs-lookup"><span data-stu-id="d73a3-467">key:attribute</span></span>
* <span data-ttu-id="d73a3-468">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="d73a3-468">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="d73a3-469">每個檔案機碼設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-469">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="d73a3-470"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> 使用目錄的檔案做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="d73a3-470">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="d73a3-471">機碼是檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="d73a3-471">The key is the file name.</span></span> <span data-ttu-id="d73a3-472">值包含檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="d73a3-472">The value contains the file's contents.</span></span> <span data-ttu-id="d73a3-473">每個檔案機碼設定提供者是在 Docker 主機案例中使用。</span><span class="sxs-lookup"><span data-stu-id="d73a3-473">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="d73a3-474">若要啟用每個檔案機碼設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="d73a3-474">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="d73a3-475">檔案的 `directoryPath` 必須是絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="d73a3-475">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="d73a3-476">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="d73a3-476">Overloads permit specifying:</span></span>

* <span data-ttu-id="d73a3-477">設定來源的 `Action<KeyPerFileConfigurationSource>` 委派。</span><span class="sxs-lookup"><span data-stu-id="d73a3-477">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="d73a3-478">目錄是否為選擇性與目錄的路徑。</span><span class="sxs-lookup"><span data-stu-id="d73a3-478">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="d73a3-479">呼叫 `CreateDefaultBuilder` 時，請使用下列設定呼叫 `UseConfiguration`：</span><span class="sxs-lookup"><span data-stu-id="d73a3-479">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
        var config = new ConfigurationBuilder()
            .AddKeyPerFile(directoryPath: path, optional: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="d73a3-480">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 `UseConfiguration`：</span><span class="sxs-lookup"><span data-stu-id="d73a3-480">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="d73a3-481">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-481">Memory Configuration Provider</span></span>

<span data-ttu-id="d73a3-482"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> 使用記憶體內集合做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="d73a3-482">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="d73a3-483">若要啟用記憶體內集合設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="d73a3-483">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d73a3-484">設定提供者可以使用 `IEnumerable<KeyValuePair<String,String>>` 來初始化。</span><span class="sxs-lookup"><span data-stu-id="d73a3-484">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d73a3-485">呼叫 `CreateDefaultBuilder` 時，請使用下列設定呼叫 `UseConfiguration`：</span><span class="sxs-lookup"><span data-stu-id="d73a3-485">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="d73a3-486">建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 `UseConfiguration`：</span><span class="sxs-lookup"><span data-stu-id="d73a3-486">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d73a3-487">使用 `UseConfiguration` 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：</span><span class="sxs-lookup"><span data-stu-id="d73a3-487">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="d73a3-488">GetValue</span><span class="sxs-lookup"><span data-stu-id="d73a3-488">GetValue</span></span>

<span data-ttu-id="d73a3-489">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) 會從具有所指定機碼的設定擷取值，並將它轉換為指定的型別。</span><span class="sxs-lookup"><span data-stu-id="d73a3-489">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="d73a3-490">若找不到機碼，多載允許您提供預設值。</span><span class="sxs-lookup"><span data-stu-id="d73a3-490">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="d73a3-491">下列範例會從具有機碼 `NumberKey` 的設定擷取字串值、將值的型別設定為 `int`，並將值存放在變數 `intValue` 中。</span><span class="sxs-lookup"><span data-stu-id="d73a3-491">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="d73a3-492">若在設定機碼中找不到 `NumberKey`，`intValue` 會接收 `99` 的預設值：</span><span class="sxs-lookup"><span data-stu-id="d73a3-492">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="d73a3-493">GetSection、GetChildren 與 Exists</span><span class="sxs-lookup"><span data-stu-id="d73a3-493">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="d73a3-494">針對下面的範例，請考慮下列 JSON 檔案。</span><span class="sxs-lookup"><span data-stu-id="d73a3-494">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="d73a3-495">在兩個區段中找到四個機碼，其中一個包括子區段組：</span><span class="sxs-lookup"><span data-stu-id="d73a3-495">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="d73a3-496">當檔案被讀入到設定之後，會建立下列唯一階層式機碼以存放設定值：</span><span class="sxs-lookup"><span data-stu-id="d73a3-496">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="d73a3-497">section0:key0</span><span class="sxs-lookup"><span data-stu-id="d73a3-497">section0:key0</span></span>
* <span data-ttu-id="d73a3-498">section0:key1</span><span class="sxs-lookup"><span data-stu-id="d73a3-498">section0:key1</span></span>
* <span data-ttu-id="d73a3-499">section1:key0</span><span class="sxs-lookup"><span data-stu-id="d73a3-499">section1:key0</span></span>
* <span data-ttu-id="d73a3-500">section1:key1</span><span class="sxs-lookup"><span data-stu-id="d73a3-500">section1:key1</span></span>
* <span data-ttu-id="d73a3-501">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="d73a3-501">section2:subsection0:key0</span></span>
* <span data-ttu-id="d73a3-502">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="d73a3-502">section2:subsection0:key1</span></span>
* <span data-ttu-id="d73a3-503">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="d73a3-503">section2:subsection1:key0</span></span>
* <span data-ttu-id="d73a3-504">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="d73a3-504">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="d73a3-505">GetSection</span><span class="sxs-lookup"><span data-stu-id="d73a3-505">GetSection</span></span>

<span data-ttu-id="d73a3-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) 會擷取具有所指定子區段機碼的設定子區段。</span><span class="sxs-lookup"><span data-stu-id="d73a3-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="d73a3-507">若要傳回 `section1` 中只包含一個機碼值組的 <xref:Microsoft.Extensions.Configuration.IConfigurationSection>，請呼叫 `GetSection` 並提供區段名稱：</span><span class="sxs-lookup"><span data-stu-id="d73a3-507">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="d73a3-508">同樣地，若要取得 `section2:subsection0` 中之機碼的值，請呼叫 `GetSection` 並提供區段路徑：</span><span class="sxs-lookup"><span data-stu-id="d73a3-508">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="d73a3-509">`GetSection` 絕不會傳回 `null`。</span><span class="sxs-lookup"><span data-stu-id="d73a3-509">`GetSection` never returns `null`.</span></span> <span data-ttu-id="d73a3-510">若找不到相符的區段，會傳回空的 `IConfigurationSection`。</span><span class="sxs-lookup"><span data-stu-id="d73a3-510">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

### <a name="getchildren"></a><span data-ttu-id="d73a3-511">GetChildren</span><span class="sxs-lookup"><span data-stu-id="d73a3-511">GetChildren</span></span>

<span data-ttu-id="d73a3-512">對 `section2` 上之 [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) 的呼叫會取得包括下列項目的 `IEnumerable<IConfigurationSection>`：</span><span class="sxs-lookup"><span data-stu-id="d73a3-512">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="d73a3-513">存在</span><span class="sxs-lookup"><span data-stu-id="d73a3-513">Exists</span></span>

<span data-ttu-id="d73a3-514">使用 [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) 來判斷設定區段是否存在：</span><span class="sxs-lookup"><span data-stu-id="d73a3-514">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="d73a3-515">以範例資料為例， `sectionExists` 是 `false`，因未設定資料中沒有 `section2:subsection2` 區段。</span><span class="sxs-lookup"><span data-stu-id="d73a3-515">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="d73a3-516">繫結到類別</span><span class="sxs-lookup"><span data-stu-id="d73a3-516">Bind to a class</span></span>

<span data-ttu-id="d73a3-517">設定可以繫結到類別，以使用*選項模式*代表相關設定的群組。</span><span class="sxs-lookup"><span data-stu-id="d73a3-517">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="d73a3-518">如需詳細資訊，請參閱<xref:fundamentals/configuration/options>。</span><span class="sxs-lookup"><span data-stu-id="d73a3-518">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="d73a3-519">設定值是以字串傳回，但是呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 會啟用 [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) 物件的建構。</span><span class="sxs-lookup"><span data-stu-id="d73a3-519">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="d73a3-520">範例應用程式包含 `Starship` 模型 (*Models/Starship.cs*)：</span><span class="sxs-lookup"><span data-stu-id="d73a3-520">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="d73a3-521">當範例應用程式使用 JSON 設定提供者來載入設定時，*starship.json* 檔案的 `starship` 區段會建立設定：</span><span class="sxs-lookup"><span data-stu-id="d73a3-521">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="d73a3-522">會建立下列設定機碼值組：</span><span class="sxs-lookup"><span data-stu-id="d73a3-522">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="d73a3-523">Key</span><span class="sxs-lookup"><span data-stu-id="d73a3-523">Key</span></span>                   | <span data-ttu-id="d73a3-524">值</span><span class="sxs-lookup"><span data-stu-id="d73a3-524">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="d73a3-525">starship:name</span><span class="sxs-lookup"><span data-stu-id="d73a3-525">starship:name</span></span>         | <span data-ttu-id="d73a3-526">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="d73a3-526">USS Enterprise</span></span>                                    |
| <span data-ttu-id="d73a3-527">starship:registry</span><span class="sxs-lookup"><span data-stu-id="d73a3-527">starship:registry</span></span>     | <span data-ttu-id="d73a3-528">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="d73a3-528">NCC-1701</span></span>                                          |
| <span data-ttu-id="d73a3-529">starship:class</span><span class="sxs-lookup"><span data-stu-id="d73a3-529">starship:class</span></span>        | <span data-ttu-id="d73a3-530">Constitution</span><span class="sxs-lookup"><span data-stu-id="d73a3-530">Constitution</span></span>                                      |
| <span data-ttu-id="d73a3-531">starship:length</span><span class="sxs-lookup"><span data-stu-id="d73a3-531">starship:length</span></span>       | <span data-ttu-id="d73a3-532">304.8</span><span class="sxs-lookup"><span data-stu-id="d73a3-532">304.8</span></span>                                             |
| <span data-ttu-id="d73a3-533">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="d73a3-533">starship:commissioned</span></span> | <span data-ttu-id="d73a3-534">False</span><span class="sxs-lookup"><span data-stu-id="d73a3-534">False</span></span>                                             |
| <span data-ttu-id="d73a3-535">trademark</span><span class="sxs-lookup"><span data-stu-id="d73a3-535">trademark</span></span>             | <span data-ttu-id="d73a3-536">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="d73a3-536">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="d73a3-537">範例應用程式使用 `starship` 機碼呼叫 `GetSection`。</span><span class="sxs-lookup"><span data-stu-id="d73a3-537">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="d73a3-538">`starship` 機碼值組會被隔離。</span><span class="sxs-lookup"><span data-stu-id="d73a3-538">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="d73a3-539">會在子區段上呼叫 `Bind` 方法，並傳入 `Starship` 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="d73a3-539">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="d73a3-540">繫結執行個體值之後，執行個體會被指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="d73a3-540">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="d73a3-541">繫結至物件圖形</span><span class="sxs-lookup"><span data-stu-id="d73a3-541">Bind to an object graph</span></span>

<span data-ttu-id="d73a3-542"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 可以繫結整個 POCO 物件圖形。</span><span class="sxs-lookup"><span data-stu-id="d73a3-542"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="d73a3-543">範例包含 `TvShow` 模型，其物件圖形包括 `Metadata` 與 `Actors` 類別 (*Models/TvShow.cs*)：</span><span class="sxs-lookup"><span data-stu-id="d73a3-543">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="d73a3-544">範例應用程式有 *tvshow.xml* 檔案，其中包含設定資料：</span><span class="sxs-lookup"><span data-stu-id="d73a3-544">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="d73a3-545">設定會被繫結到整個 `TvShow` 物件 (使用 `Bind` 方法)。</span><span class="sxs-lookup"><span data-stu-id="d73a3-545">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="d73a3-546">已繫結的執行個體會被指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="d73a3-546">The bound instance is assigned to a property for rendering:</span></span>

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

<span data-ttu-id="d73a3-547">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 會繫結並傳回所指定型別。</span><span class="sxs-lookup"><span data-stu-id="d73a3-547">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="d73a3-548">`Get<T>` 比使用 `Bind` 更方便。</span><span class="sxs-lookup"><span data-stu-id="d73a3-548">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="d73a3-549">下列程式碼顯示如何根據上面的範例使用 `Get<T>`，這可讓您直接將已繫結的執行個體指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="d73a3-549">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="d73a3-550">將陣列繫結到類別</span><span class="sxs-lookup"><span data-stu-id="d73a3-550">Bind an array to a class</span></span>

<span data-ttu-id="d73a3-551">範例應用程式示範此節中解釋的概念。</span><span class="sxs-lookup"><span data-stu-id="d73a3-551">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="d73a3-552"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="d73a3-552">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="d73a3-553">任何公開數值機碼區段 (`:0:`、`:1:`、&hellip; `:{n}:`) 的陣列格式都能繫結到POCO 類別陣列。</span><span class="sxs-lookup"><span data-stu-id="d73a3-553">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="d73a3-554">繫結是由慣例提供。</span><span class="sxs-lookup"><span data-stu-id="d73a3-554">Binding is provided by convention.</span></span> <span data-ttu-id="d73a3-555">自訂設定提供者不需要實作陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="d73a3-555">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="d73a3-556">**記憶體內陣列處理**</span><span class="sxs-lookup"><span data-stu-id="d73a3-556">**In-memory array processing**</span></span>

<span data-ttu-id="d73a3-557">考慮下表中顯示的設定機碼與值。</span><span class="sxs-lookup"><span data-stu-id="d73a3-557">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="d73a3-558">Key</span><span class="sxs-lookup"><span data-stu-id="d73a3-558">Key</span></span>     | <span data-ttu-id="d73a3-559">值</span><span class="sxs-lookup"><span data-stu-id="d73a3-559">Value</span></span>  |
| :-----: | :----: |
| <span data-ttu-id="d73a3-560">array:0</span><span class="sxs-lookup"><span data-stu-id="d73a3-560">array:0</span></span> | <span data-ttu-id="d73a3-561">value0</span><span class="sxs-lookup"><span data-stu-id="d73a3-561">value0</span></span> |
| <span data-ttu-id="d73a3-562">array:1</span><span class="sxs-lookup"><span data-stu-id="d73a3-562">array:1</span></span> | <span data-ttu-id="d73a3-563">value1</span><span class="sxs-lookup"><span data-stu-id="d73a3-563">value1</span></span> |
| <span data-ttu-id="d73a3-564">array:2</span><span class="sxs-lookup"><span data-stu-id="d73a3-564">array:2</span></span> | <span data-ttu-id="d73a3-565">value2</span><span class="sxs-lookup"><span data-stu-id="d73a3-565">value2</span></span> |
| <span data-ttu-id="d73a3-566">array:4</span><span class="sxs-lookup"><span data-stu-id="d73a3-566">array:4</span></span> | <span data-ttu-id="d73a3-567">value4</span><span class="sxs-lookup"><span data-stu-id="d73a3-567">value4</span></span> |
| <span data-ttu-id="d73a3-568">array:5</span><span class="sxs-lookup"><span data-stu-id="d73a3-568">array:5</span></span> | <span data-ttu-id="d73a3-569">value5</span><span class="sxs-lookup"><span data-stu-id="d73a3-569">value5</span></span> |

<span data-ttu-id="d73a3-570">這些機碼與值是使用「記憶體設定提供者」在範例應用程式中載入：</span><span class="sxs-lookup"><span data-stu-id="d73a3-570">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="d73a3-571">陣列會跳過索引 &num;3 的值。</span><span class="sxs-lookup"><span data-stu-id="d73a3-571">The array skips a value for index &num;3.</span></span> <span data-ttu-id="d73a3-572">設定繫結程式無法繫結 Null 值或在已繫結的物件中建立 Null 項目，這在示範繫結此陣列的結果到某物件的時候已經很清楚。</span><span class="sxs-lookup"><span data-stu-id="d73a3-572">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="d73a3-573">在範例應用程式中，POCO 類別可用來存放繫結設定資料：</span><span class="sxs-lookup"><span data-stu-id="d73a3-573">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="d73a3-574">設定資料已繫結到物件：</span><span class="sxs-lookup"><span data-stu-id="d73a3-574">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="d73a3-575">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 語法也可以使用，這會產生更精簡的程式碼：</span><span class="sxs-lookup"><span data-stu-id="d73a3-575">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="d73a3-576">繫結物件 (`ArrayExample`的執行個體) 會從設定接收陣列資料。</span><span class="sxs-lookup"><span data-stu-id="d73a3-576">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="d73a3-577">`ArrayExamples.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="d73a3-577">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="d73a3-578">`ArrayExamples.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="d73a3-578">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="d73a3-579">0</span><span class="sxs-lookup"><span data-stu-id="d73a3-579">0</span></span>                             | <span data-ttu-id="d73a3-580">value0</span><span class="sxs-lookup"><span data-stu-id="d73a3-580">value0</span></span>                        |
| <span data-ttu-id="d73a3-581">1</span><span class="sxs-lookup"><span data-stu-id="d73a3-581">1</span></span>                             | <span data-ttu-id="d73a3-582">value1</span><span class="sxs-lookup"><span data-stu-id="d73a3-582">value1</span></span>                        |
| <span data-ttu-id="d73a3-583">2</span><span class="sxs-lookup"><span data-stu-id="d73a3-583">2</span></span>                             | <span data-ttu-id="d73a3-584">value2</span><span class="sxs-lookup"><span data-stu-id="d73a3-584">value2</span></span>                        |
| <span data-ttu-id="d73a3-585">3</span><span class="sxs-lookup"><span data-stu-id="d73a3-585">3</span></span>                             | <span data-ttu-id="d73a3-586">value4</span><span class="sxs-lookup"><span data-stu-id="d73a3-586">value4</span></span>                        |
| <span data-ttu-id="d73a3-587">4</span><span class="sxs-lookup"><span data-stu-id="d73a3-587">4</span></span>                             | <span data-ttu-id="d73a3-588">value5</span><span class="sxs-lookup"><span data-stu-id="d73a3-588">value5</span></span>                        |

<span data-ttu-id="d73a3-589">繫結物件中的索引 &num;3 存放 `array:4` 設定機碼與其 `value4` 的設定資料。</span><span class="sxs-lookup"><span data-stu-id="d73a3-589">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="d73a3-590">當繫結包含陣列的設定資料時，設定機碼中的陣列索引只會用來列舉設定資料 (當建立物件時)。</span><span class="sxs-lookup"><span data-stu-id="d73a3-590">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="d73a3-591">設定資料中不能保留 Null 值，當設定機碼中的陣列略過一或多個索引時，不會在繫結物件中建立 Null 值項目。</span><span class="sxs-lookup"><span data-stu-id="d73a3-591">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="d73a3-592">由會在設定中產生正確機碼值組的任何設定提供者繫結到 `ArrayExamples` 執行個體之前，可以提供索引 &num;3 缺少的設定項目。</span><span class="sxs-lookup"><span data-stu-id="d73a3-592">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExamples` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="d73a3-593">若範例包含具有缺少之機碼值組的額外 JSON 設定提供者，`ArrayExamples.Entries` 會符合完整的設定陣列：</span><span class="sxs-lookup"><span data-stu-id="d73a3-593">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExamples.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="d73a3-594">*missing_value.json*：</span><span class="sxs-lookup"><span data-stu-id="d73a3-594">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d73a3-595">在 `ConfigureAppConfiguration` 中：</span><span class="sxs-lookup"><span data-stu-id="d73a3-595">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d73a3-596">在 `Startup` 建構函式中：</span><span class="sxs-lookup"><span data-stu-id="d73a3-596">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="d73a3-597">表格中顯示的機碼值組會載入到設定中。</span><span class="sxs-lookup"><span data-stu-id="d73a3-597">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="d73a3-598">Key</span><span class="sxs-lookup"><span data-stu-id="d73a3-598">Key</span></span>             | <span data-ttu-id="d73a3-599">值</span><span class="sxs-lookup"><span data-stu-id="d73a3-599">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="d73a3-600">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="d73a3-600">array:entries:3</span></span> | <span data-ttu-id="d73a3-601">value3</span><span class="sxs-lookup"><span data-stu-id="d73a3-601">value3</span></span> |

<span data-ttu-id="d73a3-602">在「JSON 設定提供者」包含索引 &num;3 的項目之後，若繫結 `ArrayExamples` 類別執行個體，`ArrayExamples.Entries` 陣列會包括這些值：</span><span class="sxs-lookup"><span data-stu-id="d73a3-602">If the `ArrayExamples` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExamples.Entries` array includes the value.</span></span>

| <span data-ttu-id="d73a3-603">`ArrayExamples.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="d73a3-603">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="d73a3-604">`ArrayExamples.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="d73a3-604">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="d73a3-605">0</span><span class="sxs-lookup"><span data-stu-id="d73a3-605">0</span></span>                             | <span data-ttu-id="d73a3-606">value0</span><span class="sxs-lookup"><span data-stu-id="d73a3-606">value0</span></span>                        |
| <span data-ttu-id="d73a3-607">1</span><span class="sxs-lookup"><span data-stu-id="d73a3-607">1</span></span>                             | <span data-ttu-id="d73a3-608">value1</span><span class="sxs-lookup"><span data-stu-id="d73a3-608">value1</span></span>                        |
| <span data-ttu-id="d73a3-609">2</span><span class="sxs-lookup"><span data-stu-id="d73a3-609">2</span></span>                             | <span data-ttu-id="d73a3-610">value2</span><span class="sxs-lookup"><span data-stu-id="d73a3-610">value2</span></span>                        |
| <span data-ttu-id="d73a3-611">3</span><span class="sxs-lookup"><span data-stu-id="d73a3-611">3</span></span>                             | <span data-ttu-id="d73a3-612">value3</span><span class="sxs-lookup"><span data-stu-id="d73a3-612">value3</span></span>                        |
| <span data-ttu-id="d73a3-613">4</span><span class="sxs-lookup"><span data-stu-id="d73a3-613">4</span></span>                             | <span data-ttu-id="d73a3-614">value4</span><span class="sxs-lookup"><span data-stu-id="d73a3-614">value4</span></span>                        |
| <span data-ttu-id="d73a3-615">5</span><span class="sxs-lookup"><span data-stu-id="d73a3-615">5</span></span>                             | <span data-ttu-id="d73a3-616">value5</span><span class="sxs-lookup"><span data-stu-id="d73a3-616">value5</span></span>                        |

<span data-ttu-id="d73a3-617">**JSON 陣列處理**</span><span class="sxs-lookup"><span data-stu-id="d73a3-617">**JSON array processing**</span></span>

<span data-ttu-id="d73a3-618">若 JSON 檔案包含陣列，會為具有以零為基礎之區段索引的陣列元素建立設定機碼。</span><span class="sxs-lookup"><span data-stu-id="d73a3-618">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="d73a3-619">在下列設定檔中，`subsection` 是陣列：</span><span class="sxs-lookup"><span data-stu-id="d73a3-619">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="d73a3-620">「JSON 設定提供者」會將設定資料讀入到下列機碼值組：</span><span class="sxs-lookup"><span data-stu-id="d73a3-620">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="d73a3-621">Key</span><span class="sxs-lookup"><span data-stu-id="d73a3-621">Key</span></span>                     | <span data-ttu-id="d73a3-622">值</span><span class="sxs-lookup"><span data-stu-id="d73a3-622">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="d73a3-623">json_array:key</span><span class="sxs-lookup"><span data-stu-id="d73a3-623">json_array:key</span></span>          | <span data-ttu-id="d73a3-624">valueA</span><span class="sxs-lookup"><span data-stu-id="d73a3-624">valueA</span></span> |
| <span data-ttu-id="d73a3-625">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="d73a3-625">json_array:subsection:0</span></span> | <span data-ttu-id="d73a3-626">valueB</span><span class="sxs-lookup"><span data-stu-id="d73a3-626">valueB</span></span> |
| <span data-ttu-id="d73a3-627">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="d73a3-627">json_array:subsection:1</span></span> | <span data-ttu-id="d73a3-628">valueC</span><span class="sxs-lookup"><span data-stu-id="d73a3-628">valueC</span></span> |
| <span data-ttu-id="d73a3-629">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="d73a3-629">json_array:subsection:2</span></span> | <span data-ttu-id="d73a3-630">valueD</span><span class="sxs-lookup"><span data-stu-id="d73a3-630">valueD</span></span> |

<span data-ttu-id="d73a3-631">在範例應用程式中，下列 POCO 類別可用來繫結設定機碼值組：</span><span class="sxs-lookup"><span data-stu-id="d73a3-631">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="d73a3-632">在繫結之後，`JsonArrayExample.Key` 會存放值 `valueA`。</span><span class="sxs-lookup"><span data-stu-id="d73a3-632">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="d73a3-633">子區段值會存放在 POCO 陣列屬性 `Subsection` 中。</span><span class="sxs-lookup"><span data-stu-id="d73a3-633">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="d73a3-634">`JsonArrayExample.Subsection` 索引</span><span class="sxs-lookup"><span data-stu-id="d73a3-634">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="d73a3-635">`JsonArrayExample.Subsection` 值</span><span class="sxs-lookup"><span data-stu-id="d73a3-635">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="d73a3-636">0</span><span class="sxs-lookup"><span data-stu-id="d73a3-636">0</span></span>                                   | <span data-ttu-id="d73a3-637">valueB</span><span class="sxs-lookup"><span data-stu-id="d73a3-637">valueB</span></span>                              |
| <span data-ttu-id="d73a3-638">1</span><span class="sxs-lookup"><span data-stu-id="d73a3-638">1</span></span>                                   | <span data-ttu-id="d73a3-639">valueC</span><span class="sxs-lookup"><span data-stu-id="d73a3-639">valueC</span></span>                              |
| <span data-ttu-id="d73a3-640">2</span><span class="sxs-lookup"><span data-stu-id="d73a3-640">2</span></span>                                   | <span data-ttu-id="d73a3-641">valueD</span><span class="sxs-lookup"><span data-stu-id="d73a3-641">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="d73a3-642">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="d73a3-642">Custom configuration provider</span></span>

<span data-ttu-id="d73a3-643">範例應用程式示範如何建立會使用 [Entity Framework (EF)](/ef/core/) 從資料庫讀取設定機碼值組的基本設定提供者。</span><span class="sxs-lookup"><span data-stu-id="d73a3-643">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="d73a3-644">提供者具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="d73a3-644">The provider has the following characteristics:</span></span>

* <span data-ttu-id="d73a3-645">EF 記憶體內資料庫會用於示範用途。</span><span class="sxs-lookup"><span data-stu-id="d73a3-645">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="d73a3-646">若要使用需要連接字串的資料庫，請實作第二個 `ConfigurationBuilder` 以從另一個設定提供者提供連接字串。</span><span class="sxs-lookup"><span data-stu-id="d73a3-646">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="d73a3-647">啟動時，該提供者會將資料庫資料表讀入到設定中。</span><span class="sxs-lookup"><span data-stu-id="d73a3-647">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="d73a3-648">該提供者不會以個別機碼為基礎查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="d73a3-648">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="d73a3-649">未實作變更時重新載入，因此在應用程式啟動後更新資料庫對應用程式設定沒有影響。</span><span class="sxs-lookup"><span data-stu-id="d73a3-649">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="d73a3-650">定義 `EFConfigurationValue` 實體來在資料庫中存放設定值。</span><span class="sxs-lookup"><span data-stu-id="d73a3-650">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="d73a3-651">*Models/EFConfigurationValue.cs*：</span><span class="sxs-lookup"><span data-stu-id="d73a3-651">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="d73a3-652">新增 `EFConfigurationContext` 以存放及存取已設定的值。</span><span class="sxs-lookup"><span data-stu-id="d73a3-652">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="d73a3-653">*EFConfigurationProvider/EFConfigurationContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="d73a3-653">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="d73a3-654">建立會實作 <xref:Microsoft.Extensions.Configuration.IConfigurationSource> 的類別。</span><span class="sxs-lookup"><span data-stu-id="d73a3-654">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="d73a3-655">*EFConfigurationProvider/EFConfigurationSource.cs*：</span><span class="sxs-lookup"><span data-stu-id="d73a3-655">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="d73a3-656">透過繼承自 <xref:Microsoft.Extensions.Configuration.ConfigurationProvider> 來建立自訂設定提供者。</span><span class="sxs-lookup"><span data-stu-id="d73a3-656">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="d73a3-657">若資料庫是空的，設定提供者會初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="d73a3-657">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="d73a3-658">*EFConfigurationProvider/EFConfigurationProvider.cs*：</span><span class="sxs-lookup"><span data-stu-id="d73a3-658">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="d73a3-659">`AddEFConfiguration` 擴充方法允許新增設定來源到 `ConfigurationBuilder`。</span><span class="sxs-lookup"><span data-stu-id="d73a3-659">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="d73a3-660">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="d73a3-660">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="d73a3-661">下列程式碼顯示如何在 *Program.cs* 中使用自訂 `EFConfigurationProvider`：</span><span class="sxs-lookup"><span data-stu-id="d73a3-661">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="d73a3-662">在啟動期間存取組態</span><span class="sxs-lookup"><span data-stu-id="d73a3-662">Access configuration during startup</span></span>

<span data-ttu-id="d73a3-663">將 `IConfiguration` 插入到 `Startup` 建構函式，以存取 `Startup.ConfigureServices` 中的設定值。</span><span class="sxs-lookup"><span data-stu-id="d73a3-663">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d73a3-664">若要存取 `Startup.Configure` 中的設定，請直接將 `IConfiguration` 插入到方法或從建構函式使用執行個體：</span><span class="sxs-lookup"><span data-stu-id="d73a3-664">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="d73a3-665">如需使用啟動方便方法來存取設定的範例，請參閱[應用程式啟動：方便方法](xref:fundamentals/startup#convenience-methods)。</span><span class="sxs-lookup"><span data-stu-id="d73a3-665">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="d73a3-666">存取 Razor Pages 頁面或 MVC 檢視中的設定</span><span class="sxs-lookup"><span data-stu-id="d73a3-666">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="d73a3-667">若要在 [Razor Pages 頁面] 頁面或 MVC 檢視中存取組態設定，請針對 [Microsoft.Extensions.Configuration 命名空間](xref:Microsoft.Extensions.Configuration) [using 指示詞](xref:mvc/views/razor#using) ([C# 參考：using 指示詞](/dotnet/csharp/language-reference/keywords/using-directive))，並將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 插入到頁面或檢視中。</span><span class="sxs-lookup"><span data-stu-id="d73a3-667">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="d73a3-668">在 [Razor 頁面] 頁面中：</span><span class="sxs-lookup"><span data-stu-id="d73a3-668">In a Razor Pages page:</span></span>

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

<span data-ttu-id="d73a3-669">在 MVC 檢視中：</span><span class="sxs-lookup"><span data-stu-id="d73a3-669">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="d73a3-670">從外部組件新增設定</span><span class="sxs-lookup"><span data-stu-id="d73a3-670">Add configuration from an external assembly</span></span>

<span data-ttu-id="d73a3-671"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。</span><span class="sxs-lookup"><span data-stu-id="d73a3-671">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="d73a3-672">如需詳細資訊，請參閱<xref:fundamentals/configuration/platform-specific-configuration>。</span><span class="sxs-lookup"><span data-stu-id="d73a3-672">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d73a3-673">其他資源</span><span class="sxs-lookup"><span data-stu-id="d73a3-673">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="d73a3-674">深入了解 Microsoft 設定</span><span class="sxs-lookup"><span data-stu-id="d73a3-674">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
