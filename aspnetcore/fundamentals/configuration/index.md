---
title: ASP.NET Core 的設定
author: rick-anderson
description: 了解如何使用組態 API 設定 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 3/29/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: 506f01ace72d6e915c0f3ebdaae5b4a3328a79b9
ms.sourcegitcommit: e72a58d6ebde8604badd254daae8077628f9d63e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2020
ms.locfileid: "81007154"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="75ad6-103">ASP.NET Core 的設定</span><span class="sxs-lookup"><span data-stu-id="75ad6-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="75ad6-104">由[里克·安德森](https://twitter.com/RickAndMSFT)和[柯克·拉金](https://twitter.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="75ad6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Kirk Larkin](https://twitter.com/serpent5)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="75ad6-105">ASP.NET核心中的配置使用一個或多個[配置提供程式](#cp)執行。</span><span class="sxs-lookup"><span data-stu-id="75ad6-105">Configuration in ASP.NET Core is performed using one or more [configuration providers](#cp).</span></span> <span data-ttu-id="75ad6-106">設定提供者使用各種設定來源從鍵值對讀取設定資料:</span><span class="sxs-lookup"><span data-stu-id="75ad6-106">Configuration providers read configuration data from key-value pairs using a variety of configuration sources:</span></span>

* <span data-ttu-id="75ad6-107">設定檔,如*應用程式設定.json*</span><span class="sxs-lookup"><span data-stu-id="75ad6-107">Settings files, such as *appsettings.json*</span></span>
* <span data-ttu-id="75ad6-108">環境變數</span><span class="sxs-lookup"><span data-stu-id="75ad6-108">Environment variables</span></span>
* <span data-ttu-id="75ad6-109">Azure 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="75ad6-109">Azure Key Vault</span></span>
* <span data-ttu-id="75ad6-110">Azure 應用程式組態</span><span class="sxs-lookup"><span data-stu-id="75ad6-110">Azure App Configuration</span></span>
* <span data-ttu-id="75ad6-111">命令列引數</span><span class="sxs-lookup"><span data-stu-id="75ad6-111">Command-line arguments</span></span>
* <span data-ttu-id="75ad6-112">已安裝或建立的自訂提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-112">Custom providers, installed or created</span></span>
* <span data-ttu-id="75ad6-113">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="75ad6-113">Directory files</span></span>
* <span data-ttu-id="75ad6-114">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="75ad6-114">In-memory .NET objects</span></span>

<span data-ttu-id="75ad6-115">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="75ad6-115">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<a name="default"></a>

## <a name="default-configuration"></a><span data-ttu-id="75ad6-116">預設組態</span><span class="sxs-lookup"><span data-stu-id="75ad6-116">Default configuration</span></span>

<span data-ttu-id="75ad6-117">ASP.NET使用[dotnet 新](/dotnet/core/tools/dotnet-new)工作室或 Visual Studio 建立的核心 Web 應用程式產生以下代碼:</span><span class="sxs-lookup"><span data-stu-id="75ad6-117">ASP.NET Core web apps created with [dotnet new](/dotnet/core/tools/dotnet-new) or Visual Studio generate the following code:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Program.cs?name=snippet&highlight=9)]

 <span data-ttu-id="75ad6-118"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> 會以下列順序提供應用程式的預設組態：</span><span class="sxs-lookup"><span data-stu-id="75ad6-118"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> provides default configuration for the app in the following order:</span></span>

1. <span data-ttu-id="75ad6-119">[鏈式配置提供程式](xref:Microsoft.Extensions.Configuration.ChainedConfigurationSource):添加`IConfiguration`現有來源。</span><span class="sxs-lookup"><span data-stu-id="75ad6-119">[ChainedConfigurationProvider](xref:Microsoft.Extensions.Configuration.ChainedConfigurationSource) :  Adds an existing `IConfiguration` as a source.</span></span> <span data-ttu-id="75ad6-120">在預設配置情況下,添加[主機](#hvac)配置並將其設置為_應用_配置的第一個源。</span><span class="sxs-lookup"><span data-stu-id="75ad6-120">In the default configuration case, adds the [host](#hvac) configuration and setting it as the first source for the _app_ configuration.</span></span>
1. <span data-ttu-id="75ad6-121">[應用程式設定.json](#appsettingsjson)使用[JSON 設定提供者](#file-configuration-provider)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-121">[appsettings.json](#appsettingsjson) using the [JSON configuration provider](#file-configuration-provider).</span></span>
1. <span data-ttu-id="75ad6-122">*應用設置。*`Environment` *.json*使用[JSON 設定提供者](#file-configuration-provider)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-122">*appsettings.*`Environment`*.json* using the [JSON configuration provider](#file-configuration-provider).</span></span> <span data-ttu-id="75ad6-123">例如,*應用程式設定*。***生產***。*json*與*應用程式設定*。***發展***.*json*.</span><span class="sxs-lookup"><span data-stu-id="75ad6-123">For example, *appsettings*.***Production***.*json* and *appsettings*.***Development***.*json*.</span></span>
1. <span data-ttu-id="75ad6-124">套用`Development`在環境中執行時[的應用程式 。](xref:security/app-secrets)</span><span class="sxs-lookup"><span data-stu-id="75ad6-124">[App secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
1. <span data-ttu-id="75ad6-125">使用[環境變數配置提供程式](#evcp)的環境變數。</span><span class="sxs-lookup"><span data-stu-id="75ad6-125">Environment variables using the [Environment Variables configuration provider](#evcp).</span></span>
1. <span data-ttu-id="75ad6-126">使用[指令列設定提供者](#command-line)的指令列參數 。</span><span class="sxs-lookup"><span data-stu-id="75ad6-126">Command-line arguments using the [Command-line configuration provider](#command-line).</span></span>

<span data-ttu-id="75ad6-127">稍後添加的配置提供程式將覆蓋以前的鍵設置。</span><span class="sxs-lookup"><span data-stu-id="75ad6-127">Configuration providers that are added later override previous key settings.</span></span> <span data-ttu-id="75ad6-128">例如,如果在`MyKey` *appset.json*和環境中設置,則使用環境值。</span><span class="sxs-lookup"><span data-stu-id="75ad6-128">For example, if `MyKey` is set in both *appsettings.json* and the environment, the environment value is used.</span></span> <span data-ttu-id="75ad6-129">使用預設設定提供者,[命令列設定提供程式](#command-line-configuration-provider)將覆蓋所有其他提供程式。</span><span class="sxs-lookup"><span data-stu-id="75ad6-129">Using the default configuration providers, the  [Command-line configuration provider](#command-line-configuration-provider) overrides all other providers.</span></span>

<span data-ttu-id="75ad6-130">有關 的詳細`CreateDefaultBuilder`資訊 ,請參閱[預設產生器設定](xref:fundamentals/host/generic-host#default-builder-settings)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-130">For more information on `CreateDefaultBuilder`, see [Default builder settings](xref:fundamentals/host/generic-host#default-builder-settings).</span></span>

<span data-ttu-id="75ad6-131">以下代碼按已新增的順序顯示已啟用的設定提供者:</span><span class="sxs-lookup"><span data-stu-id="75ad6-131">The following code displays the enabled configuration providers in the order they were added:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Index2.cshtml.cs?name=snippet)]

### <a name="appsettingsjson"></a><span data-ttu-id="75ad6-132">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="75ad6-132">appsettings.json</span></span>

<span data-ttu-id="75ad6-133">請考慮以下*應用程式設定.json*檔:</span><span class="sxs-lookup"><span data-stu-id="75ad6-133">Consider the following *appsettings.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/appsettings.json)]

<span data-ttu-id="75ad6-134">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)的以下代碼顯示以下幾個設定設定:</span><span class="sxs-lookup"><span data-stu-id="75ad6-134">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="75ad6-135">預設<xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider>依以下順序載入設定:</span><span class="sxs-lookup"><span data-stu-id="75ad6-135">The default <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration in the following order:</span></span>

1. <span data-ttu-id="75ad6-136">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="75ad6-136">*appsettings.json*</span></span>
1. <span data-ttu-id="75ad6-137">*應用設置。*`Environment` *.json* : 例如,*應用程式設定*。***生產***。*json*與*應用程式設定*。***發展***.*json*檔。</span><span class="sxs-lookup"><span data-stu-id="75ad6-137">*appsettings.*`Environment`*.json* : For example, the *appsettings*.***Production***.*json* and *appsettings*.***Development***.*json* files.</span></span> <span data-ttu-id="75ad6-138">檔的環境版本基於[IHosting 環境.環境名稱](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)載入。</span><span class="sxs-lookup"><span data-stu-id="75ad6-138">The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span> <span data-ttu-id="75ad6-139">如需詳細資訊，請參閱 <xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="75ad6-139">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="75ad6-140">*應用程式設定*。`Environment`.*json*值覆蓋*應用設定中的*鍵.</span><span class="sxs-lookup"><span data-stu-id="75ad6-140">*appsettings*.`Environment`.*json* values override keys in *appsettings.json*.</span></span> <span data-ttu-id="75ad6-141">例如，根據預設：</span><span class="sxs-lookup"><span data-stu-id="75ad6-141">For example, by default:</span></span>

* <span data-ttu-id="75ad6-142">在開發中,*應用程式設定 。\*\*\*\*發展\*\*\*.*json*配置覆蓋*應用設置.json\*中找到的值。</span><span class="sxs-lookup"><span data-stu-id="75ad6-142">In development, *appsettings*.***Development***.*json* configuration overwrites values found in *appsettings.json*.</span></span>
* <span data-ttu-id="75ad6-143">在生產中,*應用程式設定 。\*\*\*\*生產\*\*\*。*json*配置覆蓋*應用設置.json\*中找到的值。</span><span class="sxs-lookup"><span data-stu-id="75ad6-143">In production, *appsettings*.***Production***.*json* configuration overwrites values found in *appsettings.json*.</span></span> <span data-ttu-id="75ad6-144">例如,將應用部署到 Azure 時。</span><span class="sxs-lookup"><span data-stu-id="75ad6-144">For example, when deploying the app to Azure.</span></span>

<a name="optpat"></a>

#### <a name="bind-hierarchical-configuration-data-using-the-options-pattern"></a><span data-ttu-id="75ad6-145">使用選項模式繫結分層設定資料</span><span class="sxs-lookup"><span data-stu-id="75ad6-145">Bind hierarchical configuration data using the options pattern</span></span>

<span data-ttu-id="75ad6-146">讀取相關設定值的偏好方法是使用[選項模式](xref:fundamentals/configuration/options)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-146">The preferred way to read related configuration values is using the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="75ad6-147">例如,要讀取以下設定值:</span><span class="sxs-lookup"><span data-stu-id="75ad6-147">For example, to read the following configuration values:</span></span>

```json
  "Position": {
    "Title": "Editor",
    "Name": "Joe Smith"
  }
```

<span data-ttu-id="75ad6-148">建立以下`PositionOptions`類別:</span><span class="sxs-lookup"><span data-stu-id="75ad6-148">Create the following `PositionOptions` class:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Options/PositionOptions.cs?name=snippet)]

<span data-ttu-id="75ad6-149">該類型的所有公共讀寫屬性都綁定。</span><span class="sxs-lookup"><span data-stu-id="75ad6-149">All the public read-write properties of the type are bound.</span></span> <span data-ttu-id="75ad6-150">字段***未***綁定。</span><span class="sxs-lookup"><span data-stu-id="75ad6-150">Fields are ***not*** bound.</span></span>

<span data-ttu-id="75ad6-151">下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="75ad6-151">The following code:</span></span>

* <span data-ttu-id="75ad6-152">調用[配置綁定](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*)`PositionOptions`以將 類綁`Position`定到該 部分。</span><span class="sxs-lookup"><span data-stu-id="75ad6-152">Calls [ConfigurationBinder.Bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*) to bind the `PositionOptions` class to the `Position` section.</span></span>
* <span data-ttu-id="75ad6-153">顯示`Position`配置數據。</span><span class="sxs-lookup"><span data-stu-id="75ad6-153">Displays the `Position` configuration data.</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test22.cshtml.cs?name=snippet)]

<span data-ttu-id="75ad6-154">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*)綁定並返回指定的類型。</span><span class="sxs-lookup"><span data-stu-id="75ad6-154">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="75ad6-155">`ConfigurationBinder.Get<T>`可能比使用`ConfigurationBinder.Bind`更方便。</span><span class="sxs-lookup"><span data-stu-id="75ad6-155">`ConfigurationBinder.Get<T>` may be more convenient than using `ConfigurationBinder.Bind`.</span></span> <span data-ttu-id="75ad6-156">以下程式碼展示如何`ConfigurationBinder.Get<T>``PositionOptions`與 類別使用:</span><span class="sxs-lookup"><span data-stu-id="75ad6-156">The following code shows how to use `ConfigurationBinder.Get<T>` with the `PositionOptions` class:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test21.cshtml.cs?name=snippet)]

<span data-ttu-id="75ad6-157">使用***選項模式***時的另一種方法是繫`Position`結節並將其新增到[相依項的服務容器](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-157">An alternative approach when using the ***options pattern*** is to bind the `Position` section and add it to the [dependency injection service container](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="75ad6-158">在以下代碼中,`PositionOptions`將新增到<xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*>具有與繫結到設定的服務容器中:</span><span class="sxs-lookup"><span data-stu-id="75ad6-158">In the following code, `PositionOptions` is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Startup.cs?name=snippet)]

<span data-ttu-id="75ad6-159">使用此代碼,以下代碼讀取位置選項:</span><span class="sxs-lookup"><span data-stu-id="75ad6-159">Using the preceding code, the following code reads the position options:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test2.cshtml.cs?name=snippet)]

<span data-ttu-id="75ad6-160">使用[預設](#default)設定,*應用設定.json*和*應用程式設定。*`Environment` *.json*檔案啟用與[重新載入OnChange: true](https://github.com/dotnet/extensions/blob/release/3.1/src/Hosting/Hosting/src/Host.cs#L74-L75)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-160">Using the [default](#default) configuration, the *appsettings.json* and *appsettings.*`Environment`*.json* files are enabled with [reloadOnChange: true](https://github.com/dotnet/extensions/blob/release/3.1/src/Hosting/Hosting/src/Host.cs#L74-L75).</span></span> <span data-ttu-id="75ad6-161">對*應用設置.json*和*應用程式設置所做的更改。*`Environment` *.json*檔***後***,應用程式啟動由[JSON 配置提供程式](#jcp)讀取。</span><span class="sxs-lookup"><span data-stu-id="75ad6-161">Changes made to the *appsettings.json* and *appsettings.*`Environment`*.json* file ***after*** the app starts are read by the [JSON configuration provider](#jcp).</span></span>

<span data-ttu-id="75ad6-162">有關新增其他 JSON 設定檔的資訊,請參考此文件中的[JSON 設定提供者](#jcp)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-162">See [JSON configuration provider](#jcp) in this document for information on adding additional JSON configuration files.</span></span>

<a name="security"></a>

## <a name="security-and-secret-manager"></a><span data-ttu-id="75ad6-163">安全和秘密經理</span><span class="sxs-lookup"><span data-stu-id="75ad6-163">Security and secret manager</span></span>

<span data-ttu-id="75ad6-164">設定資料指南:</span><span class="sxs-lookup"><span data-stu-id="75ad6-164">Configuration data guidelines:</span></span>

* <span data-ttu-id="75ad6-165">永遠不要將密碼或其他敏感性資料儲存在設定提供者程式碼或純文字設定檔中。</span><span class="sxs-lookup"><span data-stu-id="75ad6-165">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="75ad6-166">[機密管理器](xref:security/app-secrets)可用於在開發中儲存機密。</span><span class="sxs-lookup"><span data-stu-id="75ad6-166">The [Secret manager](xref:security/app-secrets) can be used to store secrets in development.</span></span>
* <span data-ttu-id="75ad6-167">不要在開發或測試環境中使用生產環境祕密。</span><span class="sxs-lookup"><span data-stu-id="75ad6-167">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="75ad6-168">請在專案外部指定祕密，以防止其意外認可至開放原始碼存放庫。</span><span class="sxs-lookup"><span data-stu-id="75ad6-168">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="75ad6-169">[默認情況下](#default),[秘密管理員](xref:security/app-secrets)在*應用設置.json*和應用程式設置後讀取配置*設置。*`Environment` *.json*.</span><span class="sxs-lookup"><span data-stu-id="75ad6-169">By [default](#default), [Secret manager](xref:security/app-secrets) reads configuration settings after *appsettings.json* and *appsettings.*`Environment`*.json*.</span></span>

<span data-ttu-id="75ad6-170">關於儲存密碼或其他敏感資料的詳細資訊:</span><span class="sxs-lookup"><span data-stu-id="75ad6-170">For more information on storing passwords or other sensitive data:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="75ad6-171"><xref:security/app-secrets>:包括有關使用環境變數存儲敏感數據的建議。</span><span class="sxs-lookup"><span data-stu-id="75ad6-171"><xref:security/app-secrets>:  Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="75ad6-172">機密管理員使用[檔案配置提供程式](#fcp)將使用者機密存儲在本地系統上的 JSON 檔中。</span><span class="sxs-lookup"><span data-stu-id="75ad6-172">The Secret Manager uses the [File configuration provider](#fcp) to store user secrets in a JSON file on the local system.</span></span>

<span data-ttu-id="75ad6-173">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 可安全地儲存 ASP.NET Core 應用程式的應用程式祕密。</span><span class="sxs-lookup"><span data-stu-id="75ad6-173">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="75ad6-174">如需詳細資訊，請參閱 <xref:security/key-vault-configuration>。</span><span class="sxs-lookup"><span data-stu-id="75ad6-174">For more information, see <xref:security/key-vault-configuration>.</span></span>

<a name="evcp"></a>

## <a name="environment-variables"></a><span data-ttu-id="75ad6-175">環境變數</span><span class="sxs-lookup"><span data-stu-id="75ad6-175">Environment variables</span></span>

<span data-ttu-id="75ad6-176">使用[預設](#default)設定,<xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider>在讀取*appsettings.json*應用設定後,從環境變數鍵值對載入設定 *。*`Environment` *.json*, 和[秘密經理](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="75ad6-176">Using the [default](#default) configuration, the <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs after reading *appsettings.json*, *appsettings.*`Environment`*.json*, and [Secret manager](xref:security/app-secrets).</span></span> <span data-ttu-id="75ad6-177">因此,從環境中讀取的鍵值將覆蓋從*appsettings.json*、*應用設置*讀取的值。`Environment` *.json*和秘密經理。</span><span class="sxs-lookup"><span data-stu-id="75ad6-177">Therefore, key values read from the environment override values read from *appsettings.json*, *appsettings.*`Environment`*.json*, and Secret manager.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="75ad6-178">以下`set`指令:</span><span class="sxs-lookup"><span data-stu-id="75ad6-178">The following `set` commands:</span></span>

* <span data-ttu-id="75ad6-179">在 Windows 上設置[上述範例](#appsettingsjson)的環境鍵和值。</span><span class="sxs-lookup"><span data-stu-id="75ad6-179">Set the environment keys and values of the [preceding example](#appsettingsjson) on Windows.</span></span>
* <span data-ttu-id="75ad6-180">使用[示例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)時測試設置。</span><span class="sxs-lookup"><span data-stu-id="75ad6-180">Test the settings when using the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample).</span></span> <span data-ttu-id="75ad6-181">該`dotnet run`命令必須在專案目錄中運行。</span><span class="sxs-lookup"><span data-stu-id="75ad6-181">The `dotnet run` command must be run in the project directory.</span></span>

```dotnetcli
set MyKey="My key from Environment"
set Position__Title=Environment_Editor
set Position__Name=Environment_Rick
dotnet run
```

<span data-ttu-id="75ad6-182">前面的環境設定:</span><span class="sxs-lookup"><span data-stu-id="75ad6-182">The preceding environment settings:</span></span>

* <span data-ttu-id="75ad6-183">僅在從它們設置的命令視窗中啟動的進程中設置。</span><span class="sxs-lookup"><span data-stu-id="75ad6-183">Are only set in processes launched from the command window they were set in.</span></span>
* <span data-ttu-id="75ad6-184">使用 Visual Studio 啟動的瀏覽器不會讀取。</span><span class="sxs-lookup"><span data-stu-id="75ad6-184">Won't be read by browsers launched with Visual Studio.</span></span>

<span data-ttu-id="75ad6-185">以下[setx](/windows-server/administration/windows-commands/setx)命令可用於在 Windows 上設定環境鍵和值。</span><span class="sxs-lookup"><span data-stu-id="75ad6-185">The following [setx](/windows-server/administration/windows-commands/setx) commands can be used to set the environment keys and values on Windows.</span></span> <span data-ttu-id="75ad6-186">與`set``setx`不同,設置是保留的。</span><span class="sxs-lookup"><span data-stu-id="75ad6-186">Unlike `set`, `setx` settings are persisted.</span></span> <span data-ttu-id="75ad6-187">`/M`在系統環境中設置變數。</span><span class="sxs-lookup"><span data-stu-id="75ad6-187">`/M` sets the variable in the system environment.</span></span> <span data-ttu-id="75ad6-188">如果未使用`/M`交換機,則設置使用者環境變數。</span><span class="sxs-lookup"><span data-stu-id="75ad6-188">If the `/M` switch isn't used, a user environment variable is set.</span></span>

```cmd
setx MyKey "My key from setx Environment" /M
setx Position__Title Setx_Environment_Editor /M
setx Position__Name Environment_Rick /M
```

<span data-ttu-id="75ad6-189">要測試前面的命令覆蓋*應用設置.json*和應用*設置。*`Environment` *.json*:</span><span class="sxs-lookup"><span data-stu-id="75ad6-189">To test that the preceding commands override *appsettings.json* and *appsettings.*`Environment`*.json*:</span></span>

* <span data-ttu-id="75ad6-190">使用可視化工作室:退出並重新啟動視覺工作室。</span><span class="sxs-lookup"><span data-stu-id="75ad6-190">With Visual Studio: Exit and restart Visual Studio.</span></span>
* <span data-ttu-id="75ad6-191">使用 CLI:啟動新的指令視窗並`dotnet run`輸入 。</span><span class="sxs-lookup"><span data-stu-id="75ad6-191">With the CLI: Start a new command window and enter `dotnet run`.</span></span>

<span data-ttu-id="75ad6-192">使用<xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*>字串呼叫以指定環境變數的前置字串:</span><span class="sxs-lookup"><span data-stu-id="75ad6-192">Call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> with a string to specify a prefix for environment variables:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Program.cs?name=snippet4&highlight=12)]

<span data-ttu-id="75ad6-193">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="75ad6-193">In the preceding code:</span></span>

* <span data-ttu-id="75ad6-194">`config.AddEnvironmentVariables(prefix: "MyCustomPrefix_")`預設[設定提供者](#default)之後新增 。</span><span class="sxs-lookup"><span data-stu-id="75ad6-194">`config.AddEnvironmentVariables(prefix: "MyCustomPrefix_")` is added after the [default configuration providers](#default).</span></span> <span data-ttu-id="75ad6-195">有關排序設定提供者的範例,請參考[JSON 設定提供者](#jcp)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-195">For an example of ordering the configuration providers, see [JSON configuration provider](#jcp).</span></span>
* <span data-ttu-id="75ad6-196">使用`MyCustomPrefix_`前置文字設定的環境變數會覆蓋[預設設定提供者](#default)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-196">Environment variables set with the `MyCustomPrefix_` prefix override the [default configuration providers](#default).</span></span> <span data-ttu-id="75ad6-197">這包括沒有首碼的環境變數。</span><span class="sxs-lookup"><span data-stu-id="75ad6-197">This includes environment variables without the prefix.</span></span>

<span data-ttu-id="75ad6-198">讀取配置鍵值對時,前置碼將被刪除。</span><span class="sxs-lookup"><span data-stu-id="75ad6-198">The prefix is stripped off when the configuration key-value pairs are read.</span></span>

<span data-ttu-id="75ad6-199">以下命令測試自訂前置字串:</span><span class="sxs-lookup"><span data-stu-id="75ad6-199">The following commands test the custom prefix:</span></span>

```dotnetcli
set MyCustomPrefix_MyKey="My key with MyCustomPrefix_ Environment"
set MyCustomPrefix_Position__Title=Editor_with_customPrefix
set MyCustomPrefix_Position__Name=Environment_Rick_cp
dotnet run
```

<span data-ttu-id="75ad6-200">[預設設定](#default)增入環境變數和指令列參數,預先設定`DOTNET_`。`ASPNETCORE_`</span><span class="sxs-lookup"><span data-stu-id="75ad6-200">The [default configuration](#default) loads environment variables and command line arguments prefixed with `DOTNET_` and `ASPNETCORE_`.</span></span> <span data-ttu-id="75ad6-201">和`DOTNET_``ASPNETCORE_`首碼由ASP.NET核心用於[主機和應用配置](xref:fundamentals/host/generic-host#host-configuration),但不用於使用者配置。</span><span class="sxs-lookup"><span data-stu-id="75ad6-201">The `DOTNET_` and `ASPNETCORE_` prefixes are used by ASP.NET Core for [host and app configuration](xref:fundamentals/host/generic-host#host-configuration), but not for user configuration.</span></span> <span data-ttu-id="75ad6-202">有關主機和應用設定的詳細資訊,請參閱[.NET 通用主機](xref:fundamentals/host/generic-host)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-202">For more information on host and app configuration, see [.NET Generic Host](xref:fundamentals/host/generic-host).</span></span>

<span data-ttu-id="75ad6-203">在[Azure 應用服務](https://azure.microsoft.com/services/app-service/)上,在 **「設定>設定」** 頁上選擇 **「新應用程式」設定**。</span><span class="sxs-lookup"><span data-stu-id="75ad6-203">On [Azure App Service](https://azure.microsoft.com/services/app-service/), select **New application setting** on the **Settings > Configuration** page.</span></span> <span data-ttu-id="75ad6-204">Azure 應用程式服務應用程式設定包括:</span><span class="sxs-lookup"><span data-stu-id="75ad6-204">Azure App Service application settings are:</span></span>

* <span data-ttu-id="75ad6-205">靜態加密,通過加密通道傳輸。</span><span class="sxs-lookup"><span data-stu-id="75ad6-205">Encrypted at rest and transmitted over an encrypted channel.</span></span>
* <span data-ttu-id="75ad6-206">作為環境變數公開。</span><span class="sxs-lookup"><span data-stu-id="75ad6-206">Exposed as environment variables.</span></span>

<span data-ttu-id="75ad6-207">如需詳細資訊，請參閱 [Azure App：使用 Azure 入口網站覆寫應用程式設定](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-207">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="75ad6-208">關於 Azure 資料庫連接字串的資訊[,請參考連接字串前置字串 。](#constr)</span><span class="sxs-lookup"><span data-stu-id="75ad6-208">See [Connection string prefixes](#constr) for information on Azure database connection strings.</span></span>

<a name="clcp"></a>

## <a name="command-line"></a><span data-ttu-id="75ad6-209">命令列</span><span class="sxs-lookup"><span data-stu-id="75ad6-209">Command-line</span></span>

<span data-ttu-id="75ad6-210">使用[預設](#default)設定<xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider>, 在以下設定源之後,從命令列參數鍵值對載入設定:</span><span class="sxs-lookup"><span data-stu-id="75ad6-210">Using the [default](#default) configuration, the <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs after the following configuration sources:</span></span>

* <span data-ttu-id="75ad6-211">*應用程式設定.json*與*應用程式設定*。`Environment`.*json*檔。</span><span class="sxs-lookup"><span data-stu-id="75ad6-211">*appsettings.json* and *appsettings*.`Environment`.*json* files.</span></span>
* <span data-ttu-id="75ad6-212">開發環境中[的應用機密(秘密管理器)。](xref:security/app-secrets)</span><span class="sxs-lookup"><span data-stu-id="75ad6-212">[App secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="75ad6-213">環境變數。</span><span class="sxs-lookup"><span data-stu-id="75ad6-213">Environment variables.</span></span>

<span data-ttu-id="75ad6-214">[默認情況下](#default),在命令列重寫配置值上設置的配置值與所有其他配置提供程式一起設置。</span><span class="sxs-lookup"><span data-stu-id="75ad6-214">By [default](#default), configuration values set on the command-line override configuration values set with all the other configuration providers.</span></span>

### <a name="command-line-arguments"></a><span data-ttu-id="75ad6-215">命令列引數</span><span class="sxs-lookup"><span data-stu-id="75ad6-215">Command-line arguments</span></span>

<span data-ttu-id="75ad6-216">以下指令使用`=`設定鍵與值:</span><span class="sxs-lookup"><span data-stu-id="75ad6-216">The following command sets keys and values using `=`:</span></span>

```dotnetcli
dotnet run MyKey="My key from command line" Position:Title=Cmd Position:Name=Cmd_Rick
```

<span data-ttu-id="75ad6-217">以下指令使用`/`設定鍵與值:</span><span class="sxs-lookup"><span data-stu-id="75ad6-217">The following command sets keys and values using `/`:</span></span>

```dotnetcli
dotnet run /MyKey "Using /" /Position:Title=Cmd_ /Position:Name=Cmd_Rick
```

<span data-ttu-id="75ad6-218">以下指令使用`--`設定鍵與值:</span><span class="sxs-lookup"><span data-stu-id="75ad6-218">The following command sets keys and values using `--`:</span></span>

```dotnetcli
dotnet run --MyKey "Using --" --Position:Title=Cmd-- --Position:Name=Cmd--Rick
```

<span data-ttu-id="75ad6-219">關鍵值:</span><span class="sxs-lookup"><span data-stu-id="75ad6-219">The key value:</span></span>

* <span data-ttu-id="75ad6-220">必須遵循`=`,或者鍵必須具有`--``/`首碼或當值跟隨空格時。</span><span class="sxs-lookup"><span data-stu-id="75ad6-220">Must follow `=`, or the key must have a prefix of `--` or `/` when the value follows a space.</span></span>
* <span data-ttu-id="75ad6-221">如果使用`=`,則不是必需的。</span><span class="sxs-lookup"><span data-stu-id="75ad6-221">Isn't required if `=` is used.</span></span> <span data-ttu-id="75ad6-222">例如： `MySetting=` 。</span><span class="sxs-lookup"><span data-stu-id="75ad6-222">For example, `MySetting=`.</span></span>

<span data-ttu-id="75ad6-223">在同一命令中,不要將使用`=`鍵值對的命令列參數鍵值對與使用空格的鍵值對混合。</span><span class="sxs-lookup"><span data-stu-id="75ad6-223">Within the same command, don't mix command-line argument key-value pairs that use `=` with key-value pairs that use a space.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="75ad6-224">切換對應</span><span class="sxs-lookup"><span data-stu-id="75ad6-224">Switch mappings</span></span>

<span data-ttu-id="75ad6-225">交換機對應**金鑰**名稱替換邏輯。</span><span class="sxs-lookup"><span data-stu-id="75ad6-225">Switch mappings allow **key** name replacement logic.</span></span> <span data-ttu-id="75ad6-226">提供<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>方法的交換機替換字典。</span><span class="sxs-lookup"><span data-stu-id="75ad6-226">Provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="75ad6-227">使用切換對應字典時，會檢查字典中是否有任何索引鍵符合命令列引數所提供的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="75ad6-227">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="75ad6-228">如果在字典中找到命令行鍵,則將回傳遞字典值,將鍵值對設置為應用的配置中。</span><span class="sxs-lookup"><span data-stu-id="75ad6-228">If the command-line key is found in the dictionary, the dictionary value is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="75ad6-229">所有前面加上單虛線 (`-`) 的命令列索引鍵都需要切換對應。</span><span class="sxs-lookup"><span data-stu-id="75ad6-229">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="75ad6-230">切換對應字典索引鍵規則：</span><span class="sxs-lookup"><span data-stu-id="75ad6-230">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="75ad6-231">開關必須從`-``--`或 開始。</span><span class="sxs-lookup"><span data-stu-id="75ad6-231">Switches must start with `-` or `--`.</span></span>
* <span data-ttu-id="75ad6-232">切換對應字典不能包含重複索引鍵。</span><span class="sxs-lookup"><span data-stu-id="75ad6-232">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="75ad6-233">要使用交換機映射字典,請將其傳遞到呼叫`AddCommandLine`:</span><span class="sxs-lookup"><span data-stu-id="75ad6-233">To use a switch mappings dictionary, pass it into the call to `AddCommandLine`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramSwitch.cs?name=snippet&highlight=10-18,23)]

<span data-ttu-id="75ad6-234">以下代碼顯示取代金鑰的鍵值:</span><span class="sxs-lookup"><span data-stu-id="75ad6-234">The following code shows the key values for the replaced keys:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test3.cshtml.cs?name=snippet)]

<span data-ttu-id="75ad6-235">執行以下指令以測試金鑰取代:</span><span class="sxs-lookup"><span data-stu-id="75ad6-235">Run the following command to test the key replacement:</span></span>

```dotnetcli
dotnet run -k1=value1 -k2 value2 --alt3=value2 /alt4=value3 --alt5 value5 /alt6 value6
```

<span data-ttu-id="75ad6-236">注意:目前,`=`不能使用單個破折`-`號 設置密鑰替換值。</span><span class="sxs-lookup"><span data-stu-id="75ad6-236">Note: Currently, `=` cannot be used to set key-replacement values with a single dash `-`.</span></span> <span data-ttu-id="75ad6-237">請參閱[這個 GitHub 問題](https://github.com/dotnet/extensions/issues/3059)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-237">See [this GitHub issue](https://github.com/dotnet/extensions/issues/3059).</span></span>

<span data-ttu-id="75ad6-238">以下命令用於測試金鑰取代:</span><span class="sxs-lookup"><span data-stu-id="75ad6-238">The following command works to test key replacement:</span></span>

```dotnetcli
dotnet run -k1 value1 -k2 value2 --alt3=value2 /alt4=value3 --alt5 value5 /alt6 value6
```

<span data-ttu-id="75ad6-239">針對使用切換對應的應用程式，呼叫 `CreateDefaultBuilder` 不應傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="75ad6-239">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="75ad6-240">該方法`CreateDefaultBuilder``AddCommandLine`的 呼叫不包括對應的交換機,並且無法將交換機映射字典傳遞`CreateDefaultBuilder`給 。</span><span class="sxs-lookup"><span data-stu-id="75ad6-240">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch-mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="75ad6-241">解不是將參數傳遞給`CreateDefaultBuilder`,而是`ConfigurationBuilder`允許`AddCommandLine`該方法 的方法同時處理參數和開關映射字典。</span><span class="sxs-lookup"><span data-stu-id="75ad6-241">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch-mapping dictionary.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="75ad6-242">階層式設定資料</span><span class="sxs-lookup"><span data-stu-id="75ad6-242">Hierarchical configuration data</span></span>

<span data-ttu-id="75ad6-243">設定 API 透過在設定鍵中使用分隔符來拼平分層資料來讀取分層設定資料。</span><span class="sxs-lookup"><span data-stu-id="75ad6-243">The Configuration API reads hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="75ad6-244">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)包含以下*應用程式設定.json*檔:</span><span class="sxs-lookup"><span data-stu-id="75ad6-244">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following  *appsettings.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/appsettings.json)]

<span data-ttu-id="75ad6-245">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)的以下代碼顯示幾個設定設定:</span><span class="sxs-lookup"><span data-stu-id="75ad6-245">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="75ad6-246">讀取分層配置數據的首選方法是使用選項模式。</span><span class="sxs-lookup"><span data-stu-id="75ad6-246">The preferred way to read hierarchical configuration data is using the options pattern.</span></span> <span data-ttu-id="75ad6-247">關於詳細資訊,請參考文件中[連結的分層設定資料](#optpat)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-247">For more information, see [Bind hierarchical configuration data](#optpat) in this document.</span></span>

<span data-ttu-id="75ad6-248"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> 與 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> 方法可用來在設定資料中隔離區段與區段的子系。</span><span class="sxs-lookup"><span data-stu-id="75ad6-248"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="75ad6-249">[GetSection,、 GetChildren 與 Exists](#getsection) 中說明這些方法。</span><span class="sxs-lookup"><span data-stu-id="75ad6-249">These methods are described later in [GetSection, GetChildren, and Exists](#getsection).</span></span>

<!--
[Azure Key Vault configuration provider](xref:security/key-vault-configuration) implement change detection.
-->

## <a name="configuration-keys-and-values"></a><span data-ttu-id="75ad6-250">設定鍵與值</span><span class="sxs-lookup"><span data-stu-id="75ad6-250">Configuration keys and values</span></span>

<span data-ttu-id="75ad6-251">設定金鑰:</span><span class="sxs-lookup"><span data-stu-id="75ad6-251">Configuration keys:</span></span>

* <span data-ttu-id="75ad6-252">不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="75ad6-252">Are case-insensitive.</span></span> <span data-ttu-id="75ad6-253">例如，`ConnectionString` 與 `connectionstring` 會被視為相等的機碼。</span><span class="sxs-lookup"><span data-stu-id="75ad6-253">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="75ad6-254">如果在多個配置提供程式中設置了密鑰和值,則使用上次添加提供程式的值。</span><span class="sxs-lookup"><span data-stu-id="75ad6-254">If a key and value is set in more than one configuration providers, the value from the last provider added is used.</span></span> <span data-ttu-id="75ad6-255">有關詳細資訊,請參閱[預設設定](#default)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-255">For more information, see [Default configuration](#default).</span></span>
* <span data-ttu-id="75ad6-256">階層式機碼</span><span class="sxs-lookup"><span data-stu-id="75ad6-256">Hierarchical keys</span></span>
  * <span data-ttu-id="75ad6-257">在設定 API 內，冒號分隔字元 (`:`) 可在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="75ad6-257">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="75ad6-258">在環境變數中，冒號分隔字元可能無法在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="75ad6-258">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="75ad6-259">雙下劃線`__`由所有平台支援,並自動轉換為冒`:`號 。</span><span class="sxs-lookup"><span data-stu-id="75ad6-259">A double underscore, `__`, is supported by all platforms and is automatically converted into a colon `:`.</span></span>
  * <span data-ttu-id="75ad6-260">在 Azure 金鑰保管庫`--`中, 分層鍵用作分隔符。</span><span class="sxs-lookup"><span data-stu-id="75ad6-260">In Azure Key Vault, hierarchical keys use `--` as a separator.</span></span> <span data-ttu-id="75ad6-261">當機密載入到套用的設定中`--`[時 ,Azure 金鑰保管庫設定提供程式](xref:security/key-vault-configuration)會`:`自動取代為 。</span><span class="sxs-lookup"><span data-stu-id="75ad6-261">The [Azure Key Vault configuration provider](xref:security/key-vault-configuration) automatically replaces `--` with a `:` when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="75ad6-262"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="75ad6-262">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="75ad6-263">[將陣列繫結到類別](#boa)一節說明陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="75ad6-263">Array binding is described in the [Bind an array to a class](#boa) section.</span></span>

<span data-ttu-id="75ad6-264">設定值:</span><span class="sxs-lookup"><span data-stu-id="75ad6-264">Configuration values:</span></span>

* <span data-ttu-id="75ad6-265">是字串。</span><span class="sxs-lookup"><span data-stu-id="75ad6-265">Are strings.</span></span>
* <span data-ttu-id="75ad6-266">Null 值無法存放在設定中或繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="75ad6-266">Null values can't be stored in configuration or bound to objects.</span></span>

<a name="cp"></a>

## <a name="configuration-providers"></a><span data-ttu-id="75ad6-267">設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-267">Configuration providers</span></span>

<span data-ttu-id="75ad6-268">下表顯示可供 ASP.NET Core 應用程式使用的設定提供者。</span><span class="sxs-lookup"><span data-stu-id="75ad6-268">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="75ad6-269">提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-269">Provider</span></span> | <span data-ttu-id="75ad6-270">從提供設定</span><span class="sxs-lookup"><span data-stu-id="75ad6-270">Provides configuration from</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="75ad6-271">Azure Key Vault 組態提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-271">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration) | <span data-ttu-id="75ad6-272">Azure 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="75ad6-272">Azure Key Vault</span></span> |
| [<span data-ttu-id="75ad6-273">Azure 應用程式設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-273">Azure App configuration provider</span></span>](/azure/azure-app-configuration/quickstart-aspnet-core-app) | <span data-ttu-id="75ad6-274">Azure 應用程式組態</span><span class="sxs-lookup"><span data-stu-id="75ad6-274">Azure App Configuration</span></span> |
| [<span data-ttu-id="75ad6-275">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-275">Command-line configuration provider</span></span>](#clcp) | <span data-ttu-id="75ad6-276">命令列參數</span><span class="sxs-lookup"><span data-stu-id="75ad6-276">Command-line parameters</span></span> |
| [<span data-ttu-id="75ad6-277">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-277">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="75ad6-278">自訂來源</span><span class="sxs-lookup"><span data-stu-id="75ad6-278">Custom source</span></span> |
| [<span data-ttu-id="75ad6-279">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-279">Environment Variables configuration provider</span></span>](#evcp) | <span data-ttu-id="75ad6-280">環境變數</span><span class="sxs-lookup"><span data-stu-id="75ad6-280">Environment variables</span></span> |
| [<span data-ttu-id="75ad6-281">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-281">File configuration provider</span></span>](#file-configuration-provider) | <span data-ttu-id="75ad6-282">INI、JSON 和 XML 檔</span><span class="sxs-lookup"><span data-stu-id="75ad6-282">INI, JSON, and XML files</span></span> |
| [<span data-ttu-id="75ad6-283">按檔案鍵設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-283">Key-per-file configuration provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="75ad6-284">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="75ad6-284">Directory files</span></span> |
| [<span data-ttu-id="75ad6-285">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-285">Memory configuration provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="75ad6-286">記憶體內集合</span><span class="sxs-lookup"><span data-stu-id="75ad6-286">In-memory collections</span></span> |
| [<span data-ttu-id="75ad6-287">秘密經理</span><span class="sxs-lookup"><span data-stu-id="75ad6-287">Secret Manager</span></span>](xref:security/app-secrets)  | <span data-ttu-id="75ad6-288">使用者設定檔目錄中的檔案</span><span class="sxs-lookup"><span data-stu-id="75ad6-288">File in the user profile directory</span></span> |

<span data-ttu-id="75ad6-289">按指定其配置提供程式的順序讀取配置源。</span><span class="sxs-lookup"><span data-stu-id="75ad6-289">Configuration sources are read in the order that their configuration providers are specified.</span></span> <span data-ttu-id="75ad6-290">在代碼中訂購配置提供程式,以滿足應用所需的基礎配置源的優先順序。</span><span class="sxs-lookup"><span data-stu-id="75ad6-290">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="75ad6-291">典型的設定提供者順序是：</span><span class="sxs-lookup"><span data-stu-id="75ad6-291">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="75ad6-292">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="75ad6-292">*appsettings.json*</span></span>
1. <span data-ttu-id="75ad6-293">*應用程式設定*。`Environment`.*傑森*</span><span class="sxs-lookup"><span data-stu-id="75ad6-293">*appsettings*.`Environment`.*json*</span></span>
1. [<span data-ttu-id="75ad6-294">秘密經理</span><span class="sxs-lookup"><span data-stu-id="75ad6-294">Secret Manager</span></span>](xref:security/app-secrets)
1. <span data-ttu-id="75ad6-295">使用[環境變數配置提供程式](#evcp)的環境變數。</span><span class="sxs-lookup"><span data-stu-id="75ad6-295">Environment variables using the [Environment Variables configuration provider](#evcp).</span></span>
1. <span data-ttu-id="75ad6-296">使用[指令列設定提供者](#command-line-configuration-provider)的指令列參數 。</span><span class="sxs-lookup"><span data-stu-id="75ad6-296">Command-line arguments using the [Command-line configuration provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="75ad6-297">常見做法是添加命令列配置提供程式在一系列提供程式中的最後一個提供程式,以允許命令列參數覆蓋其他提供程式設置的配置。</span><span class="sxs-lookup"><span data-stu-id="75ad6-297">A common practice is to add the Command-line configuration provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="75ad6-298">前面的提供程式序列用於[預設設定](#default)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-298">The preceding sequence of providers is used in the [default configuration](#default).</span></span>

<a name="constr"></a>

### <a name="connection-string-prefixes"></a><span data-ttu-id="75ad6-299">連接字串前置詞</span><span class="sxs-lookup"><span data-stu-id="75ad6-299">Connection string prefixes</span></span>

<span data-ttu-id="75ad6-300">設定 API 具有針對四個連接字串環境變數的特殊處理規則。</span><span class="sxs-lookup"><span data-stu-id="75ad6-300">The Configuration API has special processing rules for four connection string environment variables.</span></span> <span data-ttu-id="75ad6-301">這些連接字串涉及為應用環境配置 Azure 連接字串。</span><span class="sxs-lookup"><span data-stu-id="75ad6-301">These connection strings are involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="75ad6-302">具有表中顯示的前置碼的環境變數將載入到具有[預設配置](#default)的應用中,或者沒有向`AddEnvironmentVariables`提供前置碼時。</span><span class="sxs-lookup"><span data-stu-id="75ad6-302">Environment variables with the prefixes shown in the table are loaded into the app with the [default configuration](#default) or when no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="75ad6-303">連接字串前置詞</span><span class="sxs-lookup"><span data-stu-id="75ad6-303">Connection string prefix</span></span> | <span data-ttu-id="75ad6-304">提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-304">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="75ad6-305">自訂提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-305">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="75ad6-306">MySQL</span><span class="sxs-lookup"><span data-stu-id="75ad6-306">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="75ad6-307">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="75ad6-307">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="75ad6-308">SQL Server</span><span class="sxs-lookup"><span data-stu-id="75ad6-308">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="75ad6-309">當探索到具有下表所顯示之任何四個前置詞的環境變數並將其載入到設定中時：</span><span class="sxs-lookup"><span data-stu-id="75ad6-309">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="75ad6-310">會透過移除環境變數前置詞並新增設定機碼區段 (`ConnectionStrings`) 來移除具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="75ad6-310">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="75ad6-311">會建立新的設定機碼值組以代表資料庫連線提供者 (`CUSTOMCONNSTR_` 除外，這沒有所述提供者)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-311">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="75ad6-312">環境變數機碼</span><span class="sxs-lookup"><span data-stu-id="75ad6-312">Environment variable key</span></span> | <span data-ttu-id="75ad6-313">已轉換的設定機碼</span><span class="sxs-lookup"><span data-stu-id="75ad6-313">Converted configuration key</span></span> | <span data-ttu-id="75ad6-314">提供者設定項目</span><span class="sxs-lookup"><span data-stu-id="75ad6-314">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="75ad6-315">設定項目未建立。</span><span class="sxs-lookup"><span data-stu-id="75ad6-315">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="75ad6-316">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="75ad6-316">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="75ad6-317">值: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="75ad6-317">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="75ad6-318">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="75ad6-318">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="75ad6-319">值: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="75ad6-319">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="75ad6-320">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="75ad6-320">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="75ad6-321">值: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="75ad6-321">Value: `System.Data.SqlClient`</span></span>  |

<a name="jcp"></a>

### <a name="json-configuration-provider"></a><span data-ttu-id="75ad6-322">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-322">JSON configuration provider</span></span>

<span data-ttu-id="75ad6-323">從<xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider>JSON 檔鍵值對載入配置。</span><span class="sxs-lookup"><span data-stu-id="75ad6-323">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs.</span></span>

<span data-ttu-id="75ad6-324">重新載入指定:</span><span class="sxs-lookup"><span data-stu-id="75ad6-324">Overloads can specify:</span></span>

* <span data-ttu-id="75ad6-325">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="75ad6-325">Whether the file is optional.</span></span>
* <span data-ttu-id="75ad6-326">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="75ad6-326">Whether the configuration is reloaded if the file changes.</span></span>

<span data-ttu-id="75ad6-327">請考慮下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="75ad6-327">Consider the following code:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSON.cs?name=snippet&highlight=12-14)]

<span data-ttu-id="75ad6-328">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="75ad6-328">The preceding code:</span></span>

* <span data-ttu-id="75ad6-329">設定 JSON 設定提供者以載入*MyConfig.json*檔,並包含以下選項:</span><span class="sxs-lookup"><span data-stu-id="75ad6-329">Configures the JSON configuration provider to load the *MyConfig.json* file with the following options:</span></span>
  * <span data-ttu-id="75ad6-330">`optional: true`:該檔是可選的。</span><span class="sxs-lookup"><span data-stu-id="75ad6-330">`optional: true`: The file is optional.</span></span>
  * <span data-ttu-id="75ad6-331">`reloadOnChange: true`:保存更改時將重新載入該檔。</span><span class="sxs-lookup"><span data-stu-id="75ad6-331">`reloadOnChange: true` : The file is reloaded when changes are saved.</span></span>
* <span data-ttu-id="75ad6-332">在*MyConfig.json*檔之前讀取[預設設定提供者](#default)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-332">Reads the [default configuration providers](#default) before the *MyConfig.json* file.</span></span> <span data-ttu-id="75ad6-333">*MyConfig.json*檔中的設定預設設定提供程式中的設定,包括[環境變數設定提供者](#evcp)與[命令列設定提供者](#clcp)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-333">Settings in the *MyConfig.json* file override setting in the default configuration providers, including the [Environment variables configuration provider](#evcp) and the [Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="75ad6-334">通常***不希望***[在環境變數配置提供程式](#evcp)和[命令列配置提供程式](#clcp)中設置自定義 JSON 檔重寫值。</span><span class="sxs-lookup"><span data-stu-id="75ad6-334">You typically ***don't*** want a custom JSON file overriding values set in the [Environment variables configuration provider](#evcp) and the [Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="75ad6-335">以下代碼清除所有設定提供者並新增多個設定提供者:</span><span class="sxs-lookup"><span data-stu-id="75ad6-335">The following code clears all the configuration providers and adds several configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSON2.cs?name=snippet)]

<span data-ttu-id="75ad6-336">在前面的代碼中 *,MyConfig.json*和*MyConfig*中的設置。`Environment`.*json*檔案:</span><span class="sxs-lookup"><span data-stu-id="75ad6-336">In the preceding code, settings in the *MyConfig.json* and  *MyConfig*.`Environment`.*json* files:</span></span>

* <span data-ttu-id="75ad6-337">覆蓋*應用設置.json*和*應用程式設置*中的設置。`Environment`.*json*檔。</span><span class="sxs-lookup"><span data-stu-id="75ad6-337">Override settings in the *appsettings.json* and *appsettings*.`Environment`.*json* files.</span></span>
* <span data-ttu-id="75ad6-338">被[環境變數配置提供程式](#evcp)和[命令列配置提供程式](#clcp)中的設置覆蓋。</span><span class="sxs-lookup"><span data-stu-id="75ad6-338">Are overridden by settings in the [Environment variables configuration provider](#evcp) and the [Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="75ad6-339">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)包含以下*MyConfig.json*檔:</span><span class="sxs-lookup"><span data-stu-id="75ad6-339">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following  *MyConfig.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/MyConfig.json)]

<span data-ttu-id="75ad6-340">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)的以下代碼顯示以下幾個設定設定:</span><span class="sxs-lookup"><span data-stu-id="75ad6-340">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<a name="fcp"></a>

## <a name="file-configuration-provider"></a><span data-ttu-id="75ad6-341">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-341">File configuration provider</span></span>

<span data-ttu-id="75ad6-342"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> 是用於從檔案系統載入設定的基底類別。</span><span class="sxs-lookup"><span data-stu-id="75ad6-342"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="75ad6-343">以下設定提供者集集`FileConfigurationProvider`:</span><span class="sxs-lookup"><span data-stu-id="75ad6-343">The following configuration providers derive from `FileConfigurationProvider`:</span></span>

* [<span data-ttu-id="75ad6-344">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-344">INI configuration provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="75ad6-345">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-345">JSON configuration provider</span></span>](#jcp)
* [<span data-ttu-id="75ad6-346">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-346">XML configuration provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="75ad6-347">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-347">INI configuration provider</span></span>

<span data-ttu-id="75ad6-348"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> 會在執行階段從 INI 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="75ad6-348">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="75ad6-349">以下代碼清除所有設定提供者並新增多個設定提供者:</span><span class="sxs-lookup"><span data-stu-id="75ad6-349">The following code clears all the configuration providers and adds several configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramINI.cs?name=snippet&highlight=10-30)]

<span data-ttu-id="75ad6-350">在前面的代碼中 *,MyIniConfig.ini*和*MyIniConfig*中的設置。`Environment`.*ini*檔案被 以下中的設定覆寫:</span><span class="sxs-lookup"><span data-stu-id="75ad6-350">In the preceding code, settings in the *MyIniConfig.ini* and  *MyIniConfig*.`Environment`.*ini* files are overridden by settings in the:</span></span>

* [<span data-ttu-id="75ad6-351">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-351">Environment variables configuration provider</span></span>](#evcp)
* <span data-ttu-id="75ad6-352">[命令列設定提供者](#clcp)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-352">[Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="75ad6-353">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)包含以下*MyIniConfig.ini*檔案:</span><span class="sxs-lookup"><span data-stu-id="75ad6-353">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following *MyIniConfig.ini* file:</span></span>

[!code-ini[](index/samples/3.x/ConfigSample/MyIniConfig.ini)]

<span data-ttu-id="75ad6-354">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)的以下代碼顯示以下幾個設定設定:</span><span class="sxs-lookup"><span data-stu-id="75ad6-354">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

### <a name="xml-configuration-provider"></a><span data-ttu-id="75ad6-355">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-355">XML configuration provider</span></span>

<span data-ttu-id="75ad6-356"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> 會在執行階段從 XML 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="75ad6-356">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="75ad6-357">以下代碼清除所有設定提供者並新增多個設定提供者:</span><span class="sxs-lookup"><span data-stu-id="75ad6-357">The following code clears all the configuration providers and adds several configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramXML.cs?name=snippet)]

<span data-ttu-id="75ad6-358">在前面的代碼中 *,MyXMLFile.xml*和*MyXMLFile*中的設置。`Environment`.*xml*檔案被 以下設定覆寫:</span><span class="sxs-lookup"><span data-stu-id="75ad6-358">In the preceding code, settings in the *MyXMLFile.xml* and  *MyXMLFile*.`Environment`.*xml* files are overridden by settings in the:</span></span>

* [<span data-ttu-id="75ad6-359">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-359">Environment variables configuration provider</span></span>](#evcp)
* <span data-ttu-id="75ad6-360">[命令列設定提供者](#clcp)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-360">[Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="75ad6-361">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)包含以下*MyXMLFile.xml*檔:</span><span class="sxs-lookup"><span data-stu-id="75ad6-361">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following *MyXMLFile.xml* file:</span></span>

[!code-xml[](index/samples/3.x/ConfigSample/MyXMLFile.xml)]

<span data-ttu-id="75ad6-362">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)的以下代碼顯示以下幾個設定設定:</span><span class="sxs-lookup"><span data-stu-id="75ad6-362">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="75ad6-363">若 `name` 屬性是用來區別元素，則可以重複那些使用相同元素名稱的元素：</span><span class="sxs-lookup"><span data-stu-id="75ad6-363">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

[!code-xml[](index/samples/3.x/ConfigSample/MyXMLFile3.xml)]

<span data-ttu-id="75ad6-364">以下代碼讀取以前的設定檔並顯示鍵與值:</span><span class="sxs-lookup"><span data-stu-id="75ad6-364">The following code reads the previous configuration file and displays the keys and values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/XML/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="75ad6-365">屬性可用來提供值：</span><span class="sxs-lookup"><span data-stu-id="75ad6-365">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="75ad6-366">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="75ad6-366">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="75ad6-367">key:attribute</span><span class="sxs-lookup"><span data-stu-id="75ad6-367">key:attribute</span></span>
* <span data-ttu-id="75ad6-368">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="75ad6-368">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="75ad6-369">按檔案鍵設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-369">Key-per-file configuration provider</span></span>

<span data-ttu-id="75ad6-370"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> 使用目錄的檔案做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="75ad6-370">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="75ad6-371">機碼是檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="75ad6-371">The key is the file name.</span></span> <span data-ttu-id="75ad6-372">值包含檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="75ad6-372">The value contains the file's contents.</span></span> <span data-ttu-id="75ad6-373">每個檔金鑰設定提供者用於 Docker 託管方案。</span><span class="sxs-lookup"><span data-stu-id="75ad6-373">The Key-per-file configuration provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="75ad6-374">若要啟用每個檔案機碼設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="75ad6-374">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="75ad6-375">檔案的 `directoryPath` 必須是絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="75ad6-375">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="75ad6-376">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="75ad6-376">Overloads permit specifying:</span></span>

* <span data-ttu-id="75ad6-377">設定來源的 `Action<KeyPerFileConfigurationSource>` 委派。</span><span class="sxs-lookup"><span data-stu-id="75ad6-377">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="75ad6-378">目錄是否為選擇性與目錄的路徑。</span><span class="sxs-lookup"><span data-stu-id="75ad6-378">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="75ad6-379">雙底線 (`__`) 是做為檔案名稱中的設定金鑰分隔符號使用。</span><span class="sxs-lookup"><span data-stu-id="75ad6-379">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="75ad6-380">例如，檔案名稱 `Logging__LogLevel__System` 會產生設定金鑰 `Logging:LogLevel:System`。</span><span class="sxs-lookup"><span data-stu-id="75ad6-380">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="75ad6-381">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="75ad6-381">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

<a name="mcp"></a>

## <a name="memory-configuration-provider"></a><span data-ttu-id="75ad6-382">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-382">Memory configuration provider</span></span>

<span data-ttu-id="75ad6-383"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> 使用記憶體內集合做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="75ad6-383">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="75ad6-384">以下代碼向設定系統新增記憶體集合:</span><span class="sxs-lookup"><span data-stu-id="75ad6-384">The following code adds a memory collection to the configuration system:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet6)]

<span data-ttu-id="75ad6-385">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)的以下代碼顯示前面的設定設定:</span><span class="sxs-lookup"><span data-stu-id="75ad6-385">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="75ad6-386">在前面的代碼中,`config.AddInMemoryCollection(Dict)`預設[設定提供程式](#default)之後新增 。</span><span class="sxs-lookup"><span data-stu-id="75ad6-386">In the preceding code, `config.AddInMemoryCollection(Dict)` is added after the [default configuration providers](#default).</span></span> <span data-ttu-id="75ad6-387">有關排序設定提供者的範例,請參考[JSON 設定提供者](#jcp)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-387">For an example of ordering the configuration providers, see [JSON configuration provider](#jcp).</span></span>

<span data-ttu-id="75ad6-388">有關排序設定提供者的範例,請參考[JSON 設定提供者](#jcp)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-388">For an example of ordering the configuration providers, see [JSON configuration provider](#jcp).</span></span>

<span data-ttu-id="75ad6-389">有關`MemoryConfigurationProvider`使用另一個範例,請參閱[連結的陣列](#boa)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-389">See [Bind an array](#boa) for another example using `MemoryConfigurationProvider`.</span></span>

## <a name="getvalue"></a><span data-ttu-id="75ad6-390">GetValue</span><span class="sxs-lookup"><span data-stu-id="75ad6-390">GetValue</span></span>

<span data-ttu-id="75ad6-391">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*)使用指定金鑰從設定中提取單個值並將其轉換為指定類型:</span><span class="sxs-lookup"><span data-stu-id="75ad6-391">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified type:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestNum.cshtml.cs?name=snippet)]

<span data-ttu-id="75ad6-392">在前面的代碼中,如果在`NumberKey`配置中找不到,則使用`99`的 預設值。</span><span class="sxs-lookup"><span data-stu-id="75ad6-392">In the preceding code,  if `NumberKey` isn't found in the configuration, the default value of `99` is used.</span></span>

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="75ad6-393">GetSection、GetChildren 與 Exists</span><span class="sxs-lookup"><span data-stu-id="75ad6-393">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="75ad6-394">有關以下範例,請考慮以下*MySub 節.json*檔:</span><span class="sxs-lookup"><span data-stu-id="75ad6-394">For the examples that follow, consider the following *MySubsection.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/MySubsection.json)]

<span data-ttu-id="75ad6-395">以下代碼將*MySub 節.json*新增到設定提供者:</span><span class="sxs-lookup"><span data-stu-id="75ad6-395">The following code adds *MySubsection.json* to the configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSONsection.cs?name=snippet)]

### <a name="getsection"></a><span data-ttu-id="75ad6-396">GetSection</span><span class="sxs-lookup"><span data-stu-id="75ad6-396">GetSection</span></span>

<span data-ttu-id="75ad6-397">[I配置.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*)返回具有指定子節鍵的配置子節。</span><span class="sxs-lookup"><span data-stu-id="75ad6-397">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) returns a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="75ad6-398">以下代碼傳回的`section1`值 :</span><span class="sxs-lookup"><span data-stu-id="75ad6-398">The following code returns values for `section1`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection.cshtml.cs?name=snippet)]

<span data-ttu-id="75ad6-399">以下代碼傳回的`section2:subsection0`值 :</span><span class="sxs-lookup"><span data-stu-id="75ad6-399">The following code returns values for `section2:subsection0`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection2.cshtml.cs?name=snippet)]

<span data-ttu-id="75ad6-400">`GetSection` 絕不會傳回 `null`。</span><span class="sxs-lookup"><span data-stu-id="75ad6-400">`GetSection` never returns `null`.</span></span> <span data-ttu-id="75ad6-401">若找不到相符的區段，會傳回空的 `IConfigurationSection`。</span><span class="sxs-lookup"><span data-stu-id="75ad6-401">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="75ad6-402">當 `GetSection` 傳回相符區段時，未填入 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value>。</span><span class="sxs-lookup"><span data-stu-id="75ad6-402">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="75ad6-403">當區段存在時，會傳回 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> 與 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path>。</span><span class="sxs-lookup"><span data-stu-id="75ad6-403">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren-and-exists"></a><span data-ttu-id="75ad6-404">取得子級與存在</span><span class="sxs-lookup"><span data-stu-id="75ad6-404">GetChildren and Exists</span></span>

<span data-ttu-id="75ad6-405">以下代碼呼叫[I 設定.Get 子級](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*)並傳回`section2:subsection0`的值:</span><span class="sxs-lookup"><span data-stu-id="75ad6-405">The following code calls [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) and returns values for `section2:subsection0`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection4.cshtml.cs?name=snippet)]

<span data-ttu-id="75ad6-406">前面的代碼呼叫[配置延伸.存在](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*)以驗證該部分是否存在:</span><span class="sxs-lookup"><span data-stu-id="75ad6-406">The preceding code calls [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to verify the  section exists:</span></span>

 <a name="boa"></a>

## <a name="bind-an-array"></a><span data-ttu-id="75ad6-407">繫結陣列</span><span class="sxs-lookup"><span data-stu-id="75ad6-407">Bind an array</span></span>

<span data-ttu-id="75ad6-408">[設定 Binder.Bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*)支援使用配置鍵中的陣列索引將數位列綁定到物件。</span><span class="sxs-lookup"><span data-stu-id="75ad6-408">The [ConfigurationBinder.Bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*) supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="75ad6-409">公開數位鍵段的任何陣列格式都能夠綁定到[POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object)類陣組。</span><span class="sxs-lookup"><span data-stu-id="75ad6-409">Any array format that exposes a numeric key segment is capable of array binding to a [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) class array.</span></span>

<span data-ttu-id="75ad6-410">從[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)中考慮*MyArray.json:*</span><span class="sxs-lookup"><span data-stu-id="75ad6-410">Consider *MyArray.json* from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample):</span></span>

[!code-json[](index/samples/3.x/ConfigSample/MyArray.json)]

<span data-ttu-id="75ad6-411">以下代碼將*MyArray.json*新增到設定提供者:</span><span class="sxs-lookup"><span data-stu-id="75ad6-411">The following code adds *MyArray.json* to the configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSONarray.cs?name=snippet)]

<span data-ttu-id="75ad6-412">以下代碼讀取設定並顯示值:</span><span class="sxs-lookup"><span data-stu-id="75ad6-412">The following code reads the configuration and displays the values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

<span data-ttu-id="75ad6-413">前面的代碼傳回以下輸出:</span><span class="sxs-lookup"><span data-stu-id="75ad6-413">The preceding code returns the following output:</span></span>

```text
Index: 0  Value: value00
Index: 1  Value: value10
Index: 2  Value: value20
Index: 3  Value: value40
Index: 4  Value: value50
```

<span data-ttu-id="75ad6-414">在前面的輸出中,索引`value40`3 具有`"4": "value40",`值, 對應於*MyArray.json*。</span><span class="sxs-lookup"><span data-stu-id="75ad6-414">In the preceding output, Index 3 has value `value40`, corresponding to `"4": "value40",` in *MyArray.json*.</span></span> <span data-ttu-id="75ad6-415">綁定陣列索引是連續的,不綁定到配置密鑰索引。</span><span class="sxs-lookup"><span data-stu-id="75ad6-415">The bound array indices are continuous and not bound to the configuration key index.</span></span> <span data-ttu-id="75ad6-416">設定活頁夾無法結合空值或在繫結物件建立空條目</span><span class="sxs-lookup"><span data-stu-id="75ad6-416">The configuration binder isn't capable of binding null values or creating null entries in bound objects</span></span>

<span data-ttu-id="75ad6-417">以下程式碼<xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*>使用`array:entries`擴充方法載入設定:</span><span class="sxs-lookup"><span data-stu-id="75ad6-417">The  following code loads the `array:entries` configuration with the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet)]

<span data-ttu-id="75ad6-418">以下代碼讀取`arrayDict``Dictionary`的設定,並顯示值:</span><span class="sxs-lookup"><span data-stu-id="75ad6-418">The following code reads the configuration in the `arrayDict` `Dictionary` and displays the values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

<span data-ttu-id="75ad6-419">前面的代碼傳回以下輸出:</span><span class="sxs-lookup"><span data-stu-id="75ad6-419">The preceding code returns the following output:</span></span>

```text
Index: 0  Value: value0
Index: 1  Value: value1
Index: 2  Value: value2
Index: 3  Value: value4
Index: 4  Value: value5
```

<span data-ttu-id="75ad6-420">繫結物件中的索引 &num;3 存放 `array:4` 設定機碼與其 `value4` 的設定資料。</span><span class="sxs-lookup"><span data-stu-id="75ad6-420">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="75ad6-421">綁定包含數位的配置資料時,配置鍵中的陣列索引用於在創建物件時反覆運算配置資料。</span><span class="sxs-lookup"><span data-stu-id="75ad6-421">When configuration data containing an array is bound, the array indices in the configuration keys are used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="75ad6-422">設定資料中不能保留 Null 值，當設定機碼中的陣列略過一或多個索引時，不會在繫結物件中建立 Null 值項目。</span><span class="sxs-lookup"><span data-stu-id="75ad6-422">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="75ad6-423">索引 3&num;的缺失 配置項可以在讀&num;取`ArrayExample`索引 3 鍵/值對的任何配置提供程式綁定到實例之前提供。</span><span class="sxs-lookup"><span data-stu-id="75ad6-423">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that reads the index &num;3 key/value pair.</span></span> <span data-ttu-id="75ad6-424">考慮以下從範例下載*的 Value3.json*檔:</span><span class="sxs-lookup"><span data-stu-id="75ad6-424">Consider the following *Value3.json* file from the sample download:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/Value3.json)]

<span data-ttu-id="75ad6-425">以下代碼包括*Value3.json*`arrayDict``Dictionary`和 的 設定:</span><span class="sxs-lookup"><span data-stu-id="75ad6-425">The following code includes configuration for *Value3.json* and the `arrayDict` `Dictionary`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet2)]

<span data-ttu-id="75ad6-426">以下代碼讀取前面的設定並顯示值:</span><span class="sxs-lookup"><span data-stu-id="75ad6-426">The following code reads the preceding configuration and displays the values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

<span data-ttu-id="75ad6-427">前面的代碼傳回以下輸出:</span><span class="sxs-lookup"><span data-stu-id="75ad6-427">The preceding code returns the following output:</span></span>

```text
Index: 0  Value: value0
Index: 1  Value: value1
Index: 2  Value: value2
Index: 3  Value: value3
Index: 4  Value: value4
Index: 5  Value: value5
```

<span data-ttu-id="75ad6-428">自訂設定提供者不需要實作陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="75ad6-428">Custom configuration providers aren't required to implement array binding.</span></span>

## <a name="custom-configuration-provider"></a><span data-ttu-id="75ad6-429">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-429">Custom configuration provider</span></span>

<span data-ttu-id="75ad6-430">範例應用程式示範如何建立會使用 [Entity Framework (EF)](/ef/core/) 從資料庫讀取設定機碼值組的基本設定提供者。</span><span class="sxs-lookup"><span data-stu-id="75ad6-430">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="75ad6-431">提供者具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="75ad6-431">The provider has the following characteristics:</span></span>

* <span data-ttu-id="75ad6-432">EF 記憶體內資料庫會用於示範用途。</span><span class="sxs-lookup"><span data-stu-id="75ad6-432">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="75ad6-433">若要使用需要連接字串的資料庫，請實作第二個 `ConfigurationBuilder` 以從另一個設定提供者提供連接字串。</span><span class="sxs-lookup"><span data-stu-id="75ad6-433">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="75ad6-434">啟動時，該提供者會將資料庫資料表讀入到設定中。</span><span class="sxs-lookup"><span data-stu-id="75ad6-434">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="75ad6-435">該提供者不會以個別機碼為基礎查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="75ad6-435">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="75ad6-436">未實作變更時重新載入，因此在應用程式啟動後更新資料庫對應用程式設定沒有影響。</span><span class="sxs-lookup"><span data-stu-id="75ad6-436">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="75ad6-437">定義 `EFConfigurationValue` 實體來在資料庫中存放設定值。</span><span class="sxs-lookup"><span data-stu-id="75ad6-437">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="75ad6-438">*Models/EFConfigurationValue.cs*：</span><span class="sxs-lookup"><span data-stu-id="75ad6-438">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="75ad6-439">新增 `EFConfigurationContext` 以存放及存取已設定的值。</span><span class="sxs-lookup"><span data-stu-id="75ad6-439">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="75ad6-440">*EFConfigurationProvider/EFConfigurationContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="75ad6-440">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="75ad6-441">建立會實作 <xref:Microsoft.Extensions.Configuration.IConfigurationSource> 的類別。</span><span class="sxs-lookup"><span data-stu-id="75ad6-441">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="75ad6-442">*EFConfigurationProvider/EFConfigurationSource.cs*：</span><span class="sxs-lookup"><span data-stu-id="75ad6-442">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="75ad6-443">透過繼承自 <xref:Microsoft.Extensions.Configuration.ConfigurationProvider> 來建立自訂設定提供者。</span><span class="sxs-lookup"><span data-stu-id="75ad6-443">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="75ad6-444">若資料庫是空的，設定提供者會初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="75ad6-444">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="75ad6-445">由於[配置鍵不區分大小寫](#keys),因此使用不區分大小寫的比較器[(StringComparer.OrdinalIgnoreCase)](xref:System.StringComparer.OrdinalIgnoreCase)創建用於初始化資料庫的字典。</span><span class="sxs-lookup"><span data-stu-id="75ad6-445">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="75ad6-446">*EFConfigurationProvider/EFConfigurationProvider.cs*：</span><span class="sxs-lookup"><span data-stu-id="75ad6-446">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="75ad6-447">`AddEFConfiguration` 擴充方法允許新增設定來源到 `ConfigurationBuilder`。</span><span class="sxs-lookup"><span data-stu-id="75ad6-447">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="75ad6-448">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="75ad6-448">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="75ad6-449">下列程式碼顯示如何在 *Program.cs* 中使用自訂 `EFConfigurationProvider`：</span><span class="sxs-lookup"><span data-stu-id="75ad6-449">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

<a name="acs"></a>

## <a name="access-configuration-in-startup"></a><span data-ttu-id="75ad6-450">啟動中的存取設定</span><span class="sxs-lookup"><span data-stu-id="75ad6-450">Access configuration in Startup</span></span>

<span data-ttu-id="75ad6-451">以下代碼以`Startup`方法顯示設定資料:</span><span class="sxs-lookup"><span data-stu-id="75ad6-451">The following code displays configuration data in `Startup` methods:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/StartupKey.cs?name=snippet&highlight=13,18)]

<span data-ttu-id="75ad6-452">如需使用啟動方便方法來存取設定的範例，請參閱[應用程式啟動：方便方法](xref:fundamentals/startup#convenience-methods)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-452">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-razor-pages"></a><span data-ttu-id="75ad6-453">剃刀頁中的存取設定</span><span class="sxs-lookup"><span data-stu-id="75ad6-453">Access configuration in Razor Pages</span></span>

<span data-ttu-id="75ad6-454">以下代碼在 Razor 頁面中顯示設定資料:</span><span class="sxs-lookup"><span data-stu-id="75ad6-454">The following code displays configuration data in a Razor Page:</span></span>

[!code-cshtml[](index/samples/3.x/ConfigSample/Pages/Test5.cshtml)]

## <a name="access-configuration-in-a-mvc-view-file"></a><span data-ttu-id="75ad6-455">造訪 MVC 檢視檔中的設定</span><span class="sxs-lookup"><span data-stu-id="75ad6-455">Access configuration in a MVC view file</span></span>

<span data-ttu-id="75ad6-456">以下代碼在 MVC 檢視中顯示設定資料:</span><span class="sxs-lookup"><span data-stu-id="75ad6-456">The following code displays configuration data in a MVC view:</span></span>

[!code-cshtml[](index/samples/3.x/ConfigSample/Views/Home2/Index.cshtml)]

<a name="hvac"></a>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="75ad6-457">主機與應用程式組態的比較</span><span class="sxs-lookup"><span data-stu-id="75ad6-457">Host versus app configuration</span></span>

<span data-ttu-id="75ad6-458">設定及啟動應用程式之前，會先設定及啟動「主機」\*\*。</span><span class="sxs-lookup"><span data-stu-id="75ad6-458">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="75ad6-459">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="75ad6-459">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="75ad6-460">應用程式與主機都是使用此主題中所述的設定提供者來設定的。</span><span class="sxs-lookup"><span data-stu-id="75ad6-460">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="75ad6-461">主機組態機碼/值組也會包含在應用程式的組態中。</span><span class="sxs-lookup"><span data-stu-id="75ad6-461">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="75ad6-462">如需有關當建置主機時如何使用設定提供者的詳細資訊，以及設定來源如何影響主機設定的詳細資訊，請參閱 <xref:fundamentals/index#host>。</span><span class="sxs-lookup"><span data-stu-id="75ad6-462">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

<a name="dhc"></a>

## <a name="default-host-configuration"></a><span data-ttu-id="75ad6-463">預設主機設定</span><span class="sxs-lookup"><span data-stu-id="75ad6-463">Default host configuration</span></span>

<span data-ttu-id="75ad6-464">如需使用 [Web 主機](xref:fundamentals/host/web-host)時預設組態的詳細資料，請參閱[本主題的 ASP.NET Core 2.2 版本](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-464">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="75ad6-465">主機組態的提供來源：</span><span class="sxs-lookup"><span data-stu-id="75ad6-465">Host configuration is provided from:</span></span>
  * <span data-ttu-id="75ad6-466">使用[環境變數配置提供者](#environment-variables-configuration-provider)(`DOTNET_`例如),`DOTNET_ENVIRONMENT`環境變數已預先固定。</span><span class="sxs-lookup"><span data-stu-id="75ad6-466">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables configuration provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="75ad6-467">載入設定機碼值組時，會移除前置詞 (`DOTNET_`)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-467">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="75ad6-468">使用[指令列設定提供者](#command-line-configuration-provider)的指令列參數 。</span><span class="sxs-lookup"><span data-stu-id="75ad6-468">Command-line arguments using the [Command-line configuration provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="75ad6-469">已建立 Web 主機預設組態 (`ConfigureWebHostDefaults`)：</span><span class="sxs-lookup"><span data-stu-id="75ad6-469">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="75ad6-470">Kestrel 會用作為網頁伺服器，並使用應用程式的組態提供者來設定。</span><span class="sxs-lookup"><span data-stu-id="75ad6-470">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="75ad6-471">新增主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="75ad6-471">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="75ad6-472">如果 `ASPNETCORE_FORWARDEDHEADERS_ENABLED` 環境變數設定為 `true`，則會新增轉接的標頭中介軟體。</span><span class="sxs-lookup"><span data-stu-id="75ad6-472">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="75ad6-473">啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="75ad6-473">Enable IIS integration.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="75ad6-474">其他設定</span><span class="sxs-lookup"><span data-stu-id="75ad6-474">Other configuration</span></span>

<span data-ttu-id="75ad6-475">這個主題僅涉及*應用設定*。</span><span class="sxs-lookup"><span data-stu-id="75ad6-475">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="75ad6-476">使用本主題未涵蓋的設定檔設定 ASP.NET 核心應用執行和託管的其他方面:</span><span class="sxs-lookup"><span data-stu-id="75ad6-476">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="75ad6-477">*啟動.json*/*啟動設定.json*是開發環境的工具設定檔,描述:</span><span class="sxs-lookup"><span data-stu-id="75ad6-477">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="75ad6-478">在<xref:fundamentals/environments#development>中。</span><span class="sxs-lookup"><span data-stu-id="75ad6-478">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="75ad6-479">跨文檔集,其中使用檔配置ASP.NET開發方案的核心應用。</span><span class="sxs-lookup"><span data-stu-id="75ad6-479">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="75ad6-480">*web.config*是伺服器設定檔,在以下主題中描述:</span><span class="sxs-lookup"><span data-stu-id="75ad6-480">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="75ad6-481">有關從早期版本的ASP.NET遷移應用配置的詳細資訊,請參閱<xref:migration/proper-to-2x/index#store-configurations>。</span><span class="sxs-lookup"><span data-stu-id="75ad6-481">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="75ad6-482">從外部組件新增設定</span><span class="sxs-lookup"><span data-stu-id="75ad6-482">Add configuration from an external assembly</span></span>

<span data-ttu-id="75ad6-483"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。</span><span class="sxs-lookup"><span data-stu-id="75ad6-483">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="75ad6-484">如需詳細資訊，請參閱 <xref:fundamentals/configuration/platform-specific-configuration>。</span><span class="sxs-lookup"><span data-stu-id="75ad6-484">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="75ad6-485">其他資源</span><span class="sxs-lookup"><span data-stu-id="75ad6-485">Additional resources</span></span>

* [<span data-ttu-id="75ad6-486">設定原始碼</span><span class="sxs-lookup"><span data-stu-id="75ad6-486">Configuration source code</span></span>](https://github.com/dotnet/extensions/tree/master/src/Configuration)
* <xref:fundamentals/configuration/options>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="75ad6-487">ASP.NET Core 中的應用程式設定是以由*設定提供者*所建立的機碼值組為基礎。</span><span class="sxs-lookup"><span data-stu-id="75ad6-487">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="75ad6-488">設定提供者會從各種設定來源將設定資料讀取到機碼值組中：</span><span class="sxs-lookup"><span data-stu-id="75ad6-488">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="75ad6-489">Azure 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="75ad6-489">Azure Key Vault</span></span>
* <span data-ttu-id="75ad6-490">Azure 應用程式組態</span><span class="sxs-lookup"><span data-stu-id="75ad6-490">Azure App Configuration</span></span>
* <span data-ttu-id="75ad6-491">命令列引數</span><span class="sxs-lookup"><span data-stu-id="75ad6-491">Command-line arguments</span></span>
* <span data-ttu-id="75ad6-492">自訂提供者 (已安裝或已建立)</span><span class="sxs-lookup"><span data-stu-id="75ad6-492">Custom providers (installed or created)</span></span>
* <span data-ttu-id="75ad6-493">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="75ad6-493">Directory files</span></span>
* <span data-ttu-id="75ad6-494">環境變數</span><span class="sxs-lookup"><span data-stu-id="75ad6-494">Environment variables</span></span>
* <span data-ttu-id="75ad6-495">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="75ad6-495">In-memory .NET objects</span></span>
* <span data-ttu-id="75ad6-496">設定檔</span><span class="sxs-lookup"><span data-stu-id="75ad6-496">Settings files</span></span>

<span data-ttu-id="75ad6-497">一般組態提供者案例 ([microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) 的組態套件包含在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="75ad6-497">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="75ad6-498">下列程式碼範例與範例應用程式中的程式碼範例使用 <xref:Microsoft.Extensions.Configuration> 命名空間：</span><span class="sxs-lookup"><span data-stu-id="75ad6-498">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="75ad6-499">*選項模式*是此主題中所述之設定概念的延伸。</span><span class="sxs-lookup"><span data-stu-id="75ad6-499">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="75ad6-500">選項使用類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="75ad6-500">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="75ad6-501">如需詳細資訊，請參閱 <xref:fundamentals/configuration/options>。</span><span class="sxs-lookup"><span data-stu-id="75ad6-501">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="75ad6-502">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="75ad6-502">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="75ad6-503">主機與應用程式組態的比較</span><span class="sxs-lookup"><span data-stu-id="75ad6-503">Host versus app configuration</span></span>

<span data-ttu-id="75ad6-504">設定及啟動應用程式之前，會先設定及啟動「主機」\*\*。</span><span class="sxs-lookup"><span data-stu-id="75ad6-504">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="75ad6-505">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="75ad6-505">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="75ad6-506">應用程式與主機都是使用此主題中所述的設定提供者來設定的。</span><span class="sxs-lookup"><span data-stu-id="75ad6-506">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="75ad6-507">主機組態機碼/值組也會包含在應用程式的組態中。</span><span class="sxs-lookup"><span data-stu-id="75ad6-507">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="75ad6-508">如需有關當建置主機時如何使用設定提供者的詳細資訊，以及設定來源如何影響主機設定的詳細資訊，請參閱 <xref:fundamentals/index#host>。</span><span class="sxs-lookup"><span data-stu-id="75ad6-508">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="75ad6-509">其他設定</span><span class="sxs-lookup"><span data-stu-id="75ad6-509">Other configuration</span></span>

<span data-ttu-id="75ad6-510">這個主題僅涉及*應用設定*。</span><span class="sxs-lookup"><span data-stu-id="75ad6-510">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="75ad6-511">使用本主題未涵蓋的設定檔設定 ASP.NET 核心應用執行和託管的其他方面:</span><span class="sxs-lookup"><span data-stu-id="75ad6-511">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="75ad6-512">*啟動.json*/*啟動設定.json*是開發環境的工具設定檔,描述:</span><span class="sxs-lookup"><span data-stu-id="75ad6-512">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="75ad6-513">在<xref:fundamentals/environments#development>中。</span><span class="sxs-lookup"><span data-stu-id="75ad6-513">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="75ad6-514">跨文檔集,其中使用檔配置ASP.NET開發方案的核心應用。</span><span class="sxs-lookup"><span data-stu-id="75ad6-514">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="75ad6-515">*web.config*是伺服器設定檔,在以下主題中描述:</span><span class="sxs-lookup"><span data-stu-id="75ad6-515">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="75ad6-516">有關從早期版本的ASP.NET遷移應用配置的詳細資訊,請參閱<xref:migration/proper-to-2x/index#store-configurations>。</span><span class="sxs-lookup"><span data-stu-id="75ad6-516">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="75ad6-517">預設組態</span><span class="sxs-lookup"><span data-stu-id="75ad6-517">Default configuration</span></span>

<span data-ttu-id="75ad6-518">以 ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) 範本為基礎的 Web 應用程式，會在建置主機時呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>。</span><span class="sxs-lookup"><span data-stu-id="75ad6-518">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="75ad6-519">`CreateDefaultBuilder` 會以下列順序提供應用程式的預設組態：</span><span class="sxs-lookup"><span data-stu-id="75ad6-519">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="75ad6-520">下列項目適用於使用 [Web 主機](xref:fundamentals/host/web-host)的應用程式。</span><span class="sxs-lookup"><span data-stu-id="75ad6-520">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="75ad6-521">如需使用[一般主機](xref:fundamentals/host/generic-host)時預設組態的詳細資料，請參閱[本主題的最新版本](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-521">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="75ad6-522">主機組態的提供來源：</span><span class="sxs-lookup"><span data-stu-id="75ad6-522">Host configuration is provided from:</span></span>
  * <span data-ttu-id="75ad6-523">使用[環境變數組態提供者](#environment-variables-configuration-provider)且以 `ASPNETCORE_` 為前置詞 (例如 `ASPNETCORE_ENVIRONMENT`) 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="75ad6-523">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="75ad6-524">載入設定機碼值組時，會移除前置詞 (`ASPNETCORE_`)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-524">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="75ad6-525">使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="75ad6-525">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="75ad6-526">應用程式設定的提供來源：</span><span class="sxs-lookup"><span data-stu-id="75ad6-526">App configuration is provided from:</span></span>
  * <span data-ttu-id="75ad6-527">使用[檔案組態提供者](#file-configuration-provider)的 *appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="75ad6-527">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="75ad6-528">使用[檔案組態提供者](#file-configuration-provider)的 *appsettings.{Environment}.json*。</span><span class="sxs-lookup"><span data-stu-id="75ad6-528">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="75ad6-529">應用程式在使用項目組件之 `Development` 環境中執行時的[秘密管理員](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-529">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="75ad6-530">使用[環境變數組態提供者](#environment-variables-configuration-provider)的環境變數。</span><span class="sxs-lookup"><span data-stu-id="75ad6-530">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="75ad6-531">使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="75ad6-531">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

## <a name="security"></a><span data-ttu-id="75ad6-532">安全性</span><span class="sxs-lookup"><span data-stu-id="75ad6-532">Security</span></span>

<span data-ttu-id="75ad6-533">採用下列做法來保護敏感性組態資料：</span><span class="sxs-lookup"><span data-stu-id="75ad6-533">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="75ad6-534">永遠不要將密碼或其他敏感性資料儲存在設定提供者程式碼或純文字設定檔中。</span><span class="sxs-lookup"><span data-stu-id="75ad6-534">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="75ad6-535">不要在開發或測試環境中使用生產環境祕密。</span><span class="sxs-lookup"><span data-stu-id="75ad6-535">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="75ad6-536">請在專案外部指定祕密，以防止其意外認可至開放原始碼存放庫。</span><span class="sxs-lookup"><span data-stu-id="75ad6-536">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="75ad6-537">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="75ad6-537">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="75ad6-538"><xref:security/app-secrets>&ndash;包括有關使用環境變數存儲敏感數據的建議。</span><span class="sxs-lookup"><span data-stu-id="75ad6-538"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="75ad6-539">「祕密管理員」使用「檔案設定提供者」以 JSON 檔案在本機系統上存放使用者祕密。</span><span class="sxs-lookup"><span data-stu-id="75ad6-539">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="75ad6-540">此主題稍後將說明「檔案設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="75ad6-540">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="75ad6-541">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 可安全地儲存 ASP.NET Core 應用程式的應用程式祕密。</span><span class="sxs-lookup"><span data-stu-id="75ad6-541">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="75ad6-542">如需詳細資訊，請參閱 <xref:security/key-vault-configuration>。</span><span class="sxs-lookup"><span data-stu-id="75ad6-542">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="75ad6-543">階層式設定資料</span><span class="sxs-lookup"><span data-stu-id="75ad6-543">Hierarchical configuration data</span></span>

<span data-ttu-id="75ad6-544">設定 API 可在設定機碼中使用分隔符號來壓平合併階層式資料，以管理階層式設定資料。</span><span class="sxs-lookup"><span data-stu-id="75ad6-544">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="75ad6-545">在下列 JSON 檔案中，兩個區段的結構式階層中有四個機碼存在：</span><span class="sxs-lookup"><span data-stu-id="75ad6-545">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="75ad6-546">當該檔案被讀入設定之後，會建立唯一機碼來維護設定來源的原始階層式資料結構。</span><span class="sxs-lookup"><span data-stu-id="75ad6-546">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="75ad6-547">系統會使用冒號 (`:`) 將區段與機碼壓平合併以維護原始結構：</span><span class="sxs-lookup"><span data-stu-id="75ad6-547">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="75ad6-548">section0:key0</span><span class="sxs-lookup"><span data-stu-id="75ad6-548">section0:key0</span></span>
* <span data-ttu-id="75ad6-549">section0:key1</span><span class="sxs-lookup"><span data-stu-id="75ad6-549">section0:key1</span></span>
* <span data-ttu-id="75ad6-550">section1:key0</span><span class="sxs-lookup"><span data-stu-id="75ad6-550">section1:key0</span></span>
* <span data-ttu-id="75ad6-551">section1:key1</span><span class="sxs-lookup"><span data-stu-id="75ad6-551">section1:key1</span></span>

<span data-ttu-id="75ad6-552"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> 與 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> 方法可用來在設定資料中隔離區段與區段的子系。</span><span class="sxs-lookup"><span data-stu-id="75ad6-552"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="75ad6-553">[GetSection,、 GetChildren 與 Exists](#getsection-getchildren-and-exists) 中說明這些方法。</span><span class="sxs-lookup"><span data-stu-id="75ad6-553">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="75ad6-554">慣例</span><span class="sxs-lookup"><span data-stu-id="75ad6-554">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="75ad6-555">來源和提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-555">Sources and providers</span></span>

<span data-ttu-id="75ad6-556">在應用程式啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="75ad6-556">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="75ad6-557">實作變更偵測的組態提供者能夠在基礎設定變更時重新載入組態。</span><span class="sxs-lookup"><span data-stu-id="75ad6-557">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="75ad6-558">例如，檔案組態提供者 (將於本主題稍後討論) 和 [Azure Key Vault 組態提供者](xref:security/key-vault-configuration)均會實作變更偵測。</span><span class="sxs-lookup"><span data-stu-id="75ad6-558">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="75ad6-559">您可以在應用程式的[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器中找到 <xref:Microsoft.Extensions.Configuration.IConfiguration>。</span><span class="sxs-lookup"><span data-stu-id="75ad6-559"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="75ad6-560"><xref:Microsoft.Extensions.Configuration.IConfiguration>可以注入 Razor<xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel>頁面<xref:Microsoft.AspNetCore.Mvc.Controller>或 MVC 以獲取類的配置。</span><span class="sxs-lookup"><span data-stu-id="75ad6-560"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="75ad6-561">在以下範例中,該`_config`欄位用於存取設定值:</span><span class="sxs-lookup"><span data-stu-id="75ad6-561">In the following examples, the `_config` field is used to access configuration values:</span></span>

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

<span data-ttu-id="75ad6-562">設定提供者無法使用 DI，因為當它們由主機設定時，它無法使用。</span><span class="sxs-lookup"><span data-stu-id="75ad6-562">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="75ad6-563">索引鍵</span><span class="sxs-lookup"><span data-stu-id="75ad6-563">Keys</span></span>

<span data-ttu-id="75ad6-564">設定機碼會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="75ad6-564">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="75ad6-565">機碼區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="75ad6-565">Keys are case-insensitive.</span></span> <span data-ttu-id="75ad6-566">例如，`ConnectionString` 與 `connectionstring` 會被視為相等的機碼。</span><span class="sxs-lookup"><span data-stu-id="75ad6-566">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="75ad6-567">若相同機碼的值是由相同或不同設定提供者設定，在機碼上設定的最後一個值是所使用的值。</span><span class="sxs-lookup"><span data-stu-id="75ad6-567">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="75ad6-568">階層式機碼</span><span class="sxs-lookup"><span data-stu-id="75ad6-568">Hierarchical keys</span></span>
  * <span data-ttu-id="75ad6-569">在設定 API 內，冒號分隔字元 (`:`) 可在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="75ad6-569">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="75ad6-570">在環境變數中，冒號分隔字元可能無法在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="75ad6-570">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="75ad6-571">所有平台都支援雙底線 (`__`)，且會自動轉換為冒號。</span><span class="sxs-lookup"><span data-stu-id="75ad6-571">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="75ad6-572">在 Azure Key Vault 中，階層式機碼使用 `--` (兩個破折號) 來做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="75ad6-572">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="75ad6-573">編寫代碼,在機密載入到應用的配置中時,用冒號替換破折號。</span><span class="sxs-lookup"><span data-stu-id="75ad6-573">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="75ad6-574"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="75ad6-574">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="75ad6-575">[將陣列繫結到類別](#bind-an-array-to-a-class)一節說明陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="75ad6-575">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="75ad6-576">值</span><span class="sxs-lookup"><span data-stu-id="75ad6-576">Values</span></span>

<span data-ttu-id="75ad6-577">設定值會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="75ad6-577">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="75ad6-578">值是字串。</span><span class="sxs-lookup"><span data-stu-id="75ad6-578">Values are strings.</span></span>
* <span data-ttu-id="75ad6-579">Null 值無法存放在設定中或繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="75ad6-579">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="75ad6-580">提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-580">Providers</span></span>

<span data-ttu-id="75ad6-581">下表顯示可供 ASP.NET Core 應用程式使用的設定提供者。</span><span class="sxs-lookup"><span data-stu-id="75ad6-581">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="75ad6-582">提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-582">Provider</span></span> | <span data-ttu-id="75ad6-583">從&hellip;提供設定</span><span class="sxs-lookup"><span data-stu-id="75ad6-583">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="75ad6-584">[Azure Key Vault 設定提供者](xref:security/key-vault-configuration) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="75ad6-584">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="75ad6-585">Azure 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="75ad6-585">Azure Key Vault</span></span> |
| <span data-ttu-id="75ad6-586">[Azure 應用程式組態提供者](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure 文件)</span><span class="sxs-lookup"><span data-stu-id="75ad6-586">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="75ad6-587">Azure 應用程式組態</span><span class="sxs-lookup"><span data-stu-id="75ad6-587">Azure App Configuration</span></span> |
| [<span data-ttu-id="75ad6-588">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-588">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="75ad6-589">命令列參數</span><span class="sxs-lookup"><span data-stu-id="75ad6-589">Command-line parameters</span></span> |
| [<span data-ttu-id="75ad6-590">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-590">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="75ad6-591">自訂來源</span><span class="sxs-lookup"><span data-stu-id="75ad6-591">Custom source</span></span> |
| [<span data-ttu-id="75ad6-592">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-592">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="75ad6-593">環境變數</span><span class="sxs-lookup"><span data-stu-id="75ad6-593">Environment variables</span></span> |
| [<span data-ttu-id="75ad6-594">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-594">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="75ad6-595">檔案 (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="75ad6-595">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="75ad6-596">每個檔案機碼的設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-596">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="75ad6-597">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="75ad6-597">Directory files</span></span> |
| [<span data-ttu-id="75ad6-598">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-598">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="75ad6-599">記憶體內集合</span><span class="sxs-lookup"><span data-stu-id="75ad6-599">In-memory collections</span></span> |
| <span data-ttu-id="75ad6-600">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="75ad6-600">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="75ad6-601">使用者設定檔目錄中的檔案</span><span class="sxs-lookup"><span data-stu-id="75ad6-601">File in the user profile directory</span></span> |

<span data-ttu-id="75ad6-602">在啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="75ad6-602">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="75ad6-603">本主題中描述的配置提供程式按字母順序描述,而不是按代碼排列順序描述。</span><span class="sxs-lookup"><span data-stu-id="75ad6-603">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="75ad6-604">在代碼中訂購配置提供程式,以滿足應用所需的基礎配置源的優先順序。</span><span class="sxs-lookup"><span data-stu-id="75ad6-604">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="75ad6-605">典型的設定提供者順序是：</span><span class="sxs-lookup"><span data-stu-id="75ad6-605">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="75ad6-606">檔(*應用程式設定.json,\*\*應用程式設置。環境\.json*`{Environment}`, 應用程式目前的託管環境在哪裡)</span><span class="sxs-lookup"><span data-stu-id="75ad6-606">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="75ad6-607">Azure 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="75ad6-607">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="75ad6-608">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (僅限開發環境)</span><span class="sxs-lookup"><span data-stu-id="75ad6-608">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="75ad6-609">環境變數</span><span class="sxs-lookup"><span data-stu-id="75ad6-609">Environment variables</span></span>
1. <span data-ttu-id="75ad6-610">命令列引數</span><span class="sxs-lookup"><span data-stu-id="75ad6-610">Command-line arguments</span></span>

<span data-ttu-id="75ad6-611">將命令列組態提供者放在提供者序列結尾是常見做法，因為這樣可以讓命令列引數覆寫由其他提供者所設定的組態。</span><span class="sxs-lookup"><span data-stu-id="75ad6-611">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="75ad6-612">使用 初始化新的主機產生器時,將使用前面的提供程式`CreateDefaultBuilder`序列 。</span><span class="sxs-lookup"><span data-stu-id="75ad6-612">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="75ad6-613">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="75ad6-613">For more information, see the [Default configuration](#default-configuration) section.</span></span>

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="75ad6-614">使用 UseConfiguration 設定主機建立器</span><span class="sxs-lookup"><span data-stu-id="75ad6-614">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="75ad6-615">若要設定主機建立器，請使用該組態在主機建立器上呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>。</span><span class="sxs-lookup"><span data-stu-id="75ad6-615">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

## <a name="configureappconfiguration"></a><span data-ttu-id="75ad6-616">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="75ad6-616">ConfigureAppConfiguration</span></span>

<span data-ttu-id="75ad6-617">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定提供者，以及由 `CreateDefaultBuilder` 自動新增的設定提供者：</span><span class="sxs-lookup"><span data-stu-id="75ad6-617">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="75ad6-618">使用命令列引數覆寫先前的組態</span><span class="sxs-lookup"><span data-stu-id="75ad6-618">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="75ad6-619">若要提供可使用命令列引數覆寫的應用程式組態，請最後呼叫 `AddCommandLine`：</span><span class="sxs-lookup"><span data-stu-id="75ad6-619">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="75ad6-620">移除由建立預設產生器新增的提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-620">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="75ad6-621">要刪除`CreateDefaultBuilder`新增的提供者,請先在[I 設定產生器](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources)上調用[Clear:](/dotnet/api/system.collections.generic.icollection-1.clear)</span><span class="sxs-lookup"><span data-stu-id="75ad6-621">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="75ad6-622">在應用程式啟動期間使用組態</span><span class="sxs-lookup"><span data-stu-id="75ad6-622">Consume configuration during app startup</span></span>

<span data-ttu-id="75ad6-623">應用程式啟動期間，可以使用 `ConfigureAppConfiguration` 中為應用程式提供的組態，包括 `Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="75ad6-623">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="75ad6-624">如需詳細資訊，請參閱[在啟動期間存取組態](#access-configuration-during-startup)一節。</span><span class="sxs-lookup"><span data-stu-id="75ad6-624">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="75ad6-625">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-625">Command-line Configuration Provider</span></span>

<span data-ttu-id="75ad6-626"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> 會在執行階段從命令列引數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="75ad6-626">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="75ad6-627">為了啟用命令列設定，<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 延伸模組方法會在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫。</span><span class="sxs-lookup"><span data-stu-id="75ad6-627">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="75ad6-628">呼叫 `CreateDefaultBuilder(string [])` 時，會自動呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="75ad6-628">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="75ad6-629">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="75ad6-629">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="75ad6-630">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="75ad6-630">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="75ad6-631">從 *appsettings.json* 與 *appsettings.{Environment}.json* 檔案載入其他選擇性組態。</span><span class="sxs-lookup"><span data-stu-id="75ad6-631">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="75ad6-632">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-632">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="75ad6-633">環境變數。</span><span class="sxs-lookup"><span data-stu-id="75ad6-633">Environment variables.</span></span>

<span data-ttu-id="75ad6-634">`CreateDefaultBuilder` 會最後才新增命令列設定提供者。</span><span class="sxs-lookup"><span data-stu-id="75ad6-634">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="75ad6-635">在執行階段傳遞的命令列引數會覆寫由其它提供者所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="75ad6-635">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="75ad6-636">`CreateDefaultBuilder` 會在建構主機時執行作業。</span><span class="sxs-lookup"><span data-stu-id="75ad6-636">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="75ad6-637">因此，由`CreateDefaultBuilder` 啟用的命令列設定可以影響主機的設定方式。</span><span class="sxs-lookup"><span data-stu-id="75ad6-637">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="75ad6-638">針對以 ASP.NET Core 範本為基礎的應用程式，`AddCommandLine` 已由 `CreateDefaultBuilder` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="75ad6-638">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="75ad6-639">若要新增其他組態提供者並維持能夠使用命令列引數覆寫這些提供者組態的能力，請在 `ConfigureAppConfiguration` 中呼叫應用程式的其他提供者，且最後呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="75ad6-639">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="75ad6-640">**範例**</span><span class="sxs-lookup"><span data-stu-id="75ad6-640">**Example**</span></span>

<span data-ttu-id="75ad6-641">範例應用程式利用靜態方便方法 `CreateDefaultBuilder` 的優勢來建置主機，這包括對 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="75ad6-641">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="75ad6-642">從專案目錄中開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="75ad6-642">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="75ad6-643">提供命令列引數給 `dotnet run` 命令 `dotnet run CommandLineKey=CommandLineValue`。</span><span class="sxs-lookup"><span data-stu-id="75ad6-643">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="75ad6-644">在應用程式執行之後，開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="75ad6-644">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="75ad6-645">觀察輸出是否包含提供給 `dotnet run` 之設定命令列引數的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="75ad6-645">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="75ad6-646">引數</span><span class="sxs-lookup"><span data-stu-id="75ad6-646">Arguments</span></span>

<span data-ttu-id="75ad6-647">當值後面接著空格時，值必須接著等號 (`=`)，或機碼必須有前置詞 (`--` 或 `/`)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-647">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="75ad6-648">如果使用等號 (例如 `CommandLineKey=`)，則不需要此值。</span><span class="sxs-lookup"><span data-stu-id="75ad6-648">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="75ad6-649">索引鍵前置字元</span><span class="sxs-lookup"><span data-stu-id="75ad6-649">Key prefix</span></span>               | <span data-ttu-id="75ad6-650">範例</span><span class="sxs-lookup"><span data-stu-id="75ad6-650">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="75ad6-651">沒有前置字元</span><span class="sxs-lookup"><span data-stu-id="75ad6-651">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="75ad6-652">雙虛線 (`--`)</span><span class="sxs-lookup"><span data-stu-id="75ad6-652">Two dashes (`--`)</span></span>        | <span data-ttu-id="75ad6-653">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="75ad6-653">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="75ad6-654">正斜線 (`/`)</span><span class="sxs-lookup"><span data-stu-id="75ad6-654">Forward slash (`/`)</span></span>      | <span data-ttu-id="75ad6-655">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="75ad6-655">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="75ad6-656">在相同的命令中，請不要混合使用等號搭配使用空格之機碼值組的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="75ad6-656">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="75ad6-657">範例命令：</span><span class="sxs-lookup"><span data-stu-id="75ad6-657">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="75ad6-658">切換對應</span><span class="sxs-lookup"><span data-stu-id="75ad6-658">Switch mappings</span></span>

<span data-ttu-id="75ad6-659">參數對應允許索引鍵名稱取代邏輯。</span><span class="sxs-lookup"><span data-stu-id="75ad6-659">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="75ad6-660">使用 手動構建配置<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>時<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>,提供方法的交換機替換字典。</span><span class="sxs-lookup"><span data-stu-id="75ad6-660">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="75ad6-661">使用切換對應字典時，會檢查字典中是否有任何索引鍵符合命令列引數所提供的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="75ad6-661">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="75ad6-662">如果在字典中找到命令列索引鍵，則會傳回字典值 (索引鍵取代) 以在應用程式的設定中設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="75ad6-662">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="75ad6-663">所有前面加上單虛線 (`-`) 的命令列索引鍵都需要切換對應。</span><span class="sxs-lookup"><span data-stu-id="75ad6-663">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="75ad6-664">切換對應字典索引鍵規則：</span><span class="sxs-lookup"><span data-stu-id="75ad6-664">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="75ad6-665">切換必須以單虛線 (`-`) 或雙虛線 (`--`) 開頭。</span><span class="sxs-lookup"><span data-stu-id="75ad6-665">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="75ad6-666">切換對應字典不能包含重複索引鍵。</span><span class="sxs-lookup"><span data-stu-id="75ad6-666">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="75ad6-667">建立切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="75ad6-667">Create a switch mappings dictionary.</span></span> <span data-ttu-id="75ad6-668">下列範例會建立兩個切換對應：</span><span class="sxs-lookup"><span data-stu-id="75ad6-668">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="75ad6-669">建立主機時，請使用切換對應字典呼叫 `AddCommandLine`：</span><span class="sxs-lookup"><span data-stu-id="75ad6-669">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="75ad6-670">針對使用切換對應的應用程式，呼叫 `CreateDefaultBuilder` 不應傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="75ad6-670">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="75ad6-671">`CreateDefaultBuilder` 方法的 `AddCommandLine` 呼叫不包括對應的切換，且沒有任何方式可以將切換對應字典傳遞給 `CreateDefaultBuilder`。</span><span class="sxs-lookup"><span data-stu-id="75ad6-671">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="75ad6-672">解決方案並非將引數傳遞給 `CreateDefaultBuilder`，而是允許 `ConfigurationBuilder` 方法的 `AddCommandLine` 方法同時處理引數與切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="75ad6-672">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="75ad6-673">建立切換對應字典之後，它會包含下表中所示的資料。</span><span class="sxs-lookup"><span data-stu-id="75ad6-673">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="75ad6-674">Key</span><span class="sxs-lookup"><span data-stu-id="75ad6-674">Key</span></span>       | <span data-ttu-id="75ad6-675">值</span><span class="sxs-lookup"><span data-stu-id="75ad6-675">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="75ad6-676">若啟動應用程式時使用切換對應機碼，設定會接收由字典提供之機碼上的設定值：</span><span class="sxs-lookup"><span data-stu-id="75ad6-676">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="75ad6-677">執行上述命令之後，設定包含下表中顯示的值。</span><span class="sxs-lookup"><span data-stu-id="75ad6-677">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="75ad6-678">Key</span><span class="sxs-lookup"><span data-stu-id="75ad6-678">Key</span></span>               | <span data-ttu-id="75ad6-679">值</span><span class="sxs-lookup"><span data-stu-id="75ad6-679">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="75ad6-680">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-680">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="75ad6-681"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> 會在執行階段從環境變數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="75ad6-681">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="75ad6-682">若要啟用環境變數設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="75ad6-682">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="75ad6-683">[Azure 應用服務](https://azure.microsoft.com/services/app-service/)允許在 Azure 門戶中設置環境變數,這些變數可以使用環境變數配置提供程式覆蓋應用配置。</span><span class="sxs-lookup"><span data-stu-id="75ad6-683">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="75ad6-684">如需詳細資訊，請參閱 [Azure App：使用 Azure 入口網站覆寫應用程式設定](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-684">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="75ad6-685">使用 [Web 主機](xref:fundamentals/host/web-host)初始化新的主機建立器並呼叫 `CreateDefaultBuilder` 時，可使用 `AddEnvironmentVariables` 為[主機組態](#host-versus-app-configuration)載入字首為 `ASPNETCORE_` 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="75ad6-685">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="75ad6-686">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="75ad6-686">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="75ad6-687">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="75ad6-687">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="75ad6-688">來自無首碼之環境變數的應用程式組態 (在未提供首碼的情況下呼叫 `AddEnvironmentVariables`)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-688">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="75ad6-689">從 *appsettings.json* 與 *appsettings.{Environment}.json* 檔案載入其他選擇性組態。</span><span class="sxs-lookup"><span data-stu-id="75ad6-689">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="75ad6-690">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-690">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="75ad6-691">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="75ad6-691">Command-line arguments.</span></span>

<span data-ttu-id="75ad6-692">從使用者祕密與 *appsettings* 檔案建立設定之後，會呼叫「環境變數設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="75ad6-692">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="75ad6-693">在此位置呼叫提供者可讓系統在執行階段讀取環境變數，以覆寫由使用者祕密與 *appsettings* 檔案所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="75ad6-693">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="75ad6-694">要從其他環境變數提供應用配置,請調用應用的其他提供程式`ConfigureAppConfiguration`,然後調用`AddEnvironmentVariables`首碼:</span><span class="sxs-lookup"><span data-stu-id="75ad6-694">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="75ad6-695">最後`AddEnvironmentVariables`調用以允許具有給定首碼的環境變數覆蓋來自其他提供程式的值。</span><span class="sxs-lookup"><span data-stu-id="75ad6-695">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="75ad6-696">**範例**</span><span class="sxs-lookup"><span data-stu-id="75ad6-696">**Example**</span></span>

<span data-ttu-id="75ad6-697">範例應用程式利用靜態方便方法 `CreateDefaultBuilder` 的優勢來建置主機，這包括對 `AddEnvironmentVariables` 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="75ad6-697">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="75ad6-698">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="75ad6-698">Run the sample app.</span></span> <span data-ttu-id="75ad6-699">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="75ad6-699">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="75ad6-700">觀察輸出是否包含環境變數 `ENVIRONMENT` 的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="75ad6-700">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="75ad6-701">值反映應用程式執行所在環境，在本機執行時，通常是 `Development`。</span><span class="sxs-lookup"><span data-stu-id="75ad6-701">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="75ad6-702">為縮短由應用程式轉譯的環境變數清單，應用程式會篩選環境變數。</span><span class="sxs-lookup"><span data-stu-id="75ad6-702">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="75ad6-703">請參閱範例應用程式的 *Pages/Index.cshtml.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="75ad6-703">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="75ad6-704">要公開應用程式可用的所有環境變數,將`FilteredConfiguration`*頁/Index.cshtml.cs*中的內容更改為以下內容:</span><span class="sxs-lookup"><span data-stu-id="75ad6-704">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="75ad6-705">首碼</span><span class="sxs-lookup"><span data-stu-id="75ad6-705">Prefixes</span></span>

<span data-ttu-id="75ad6-706">向應用配置提供首碼`AddEnvironmentVariables`時,將篩選載入到應用配置中的環境變數。</span><span class="sxs-lookup"><span data-stu-id="75ad6-706">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="75ad6-707">例如，若要篩選前置詞為 `CUSTOM_` 的環境變數，請提供前置詞給設定提供者：</span><span class="sxs-lookup"><span data-stu-id="75ad6-707">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="75ad6-708">建立設定機碼值組時，會移除前置詞。</span><span class="sxs-lookup"><span data-stu-id="75ad6-708">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="75ad6-709">建立主機建立器時，主機組態由環境變數提供。</span><span class="sxs-lookup"><span data-stu-id="75ad6-709">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="75ad6-710">如需這些環境變數所用前置詞的詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="75ad6-710">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="75ad6-711">**連接字串前置詞**</span><span class="sxs-lookup"><span data-stu-id="75ad6-711">**Connection string prefixes**</span></span>

<span data-ttu-id="75ad6-712">設定 API 四個連接字串環境變數的特殊處理規則，這些這些環境變數牽涉到針對應用程式環境設定 Azure 連接字串。</span><span class="sxs-lookup"><span data-stu-id="75ad6-712">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="75ad6-713">若將前置詞提供給 `AddEnvironmentVariables`具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="75ad6-713">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="75ad6-714">連接字串前置詞</span><span class="sxs-lookup"><span data-stu-id="75ad6-714">Connection string prefix</span></span> | <span data-ttu-id="75ad6-715">提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-715">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="75ad6-716">自訂提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-716">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="75ad6-717">MySQL</span><span class="sxs-lookup"><span data-stu-id="75ad6-717">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="75ad6-718">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="75ad6-718">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="75ad6-719">SQL Server</span><span class="sxs-lookup"><span data-stu-id="75ad6-719">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="75ad6-720">當探索到具有下表所顯示之任何四個前置詞的環境變數並將其載入到設定中時：</span><span class="sxs-lookup"><span data-stu-id="75ad6-720">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="75ad6-721">會透過移除環境變數前置詞並新增設定機碼區段 (`ConnectionStrings`) 來移除具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="75ad6-721">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="75ad6-722">會建立新的設定機碼值組以代表資料庫連線提供者 (`CUSTOMCONNSTR_` 除外，這沒有所述提供者)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-722">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="75ad6-723">環境變數機碼</span><span class="sxs-lookup"><span data-stu-id="75ad6-723">Environment variable key</span></span> | <span data-ttu-id="75ad6-724">已轉換的設定機碼</span><span class="sxs-lookup"><span data-stu-id="75ad6-724">Converted configuration key</span></span> | <span data-ttu-id="75ad6-725">提供者設定項目</span><span class="sxs-lookup"><span data-stu-id="75ad6-725">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="75ad6-726">設定項目未建立。</span><span class="sxs-lookup"><span data-stu-id="75ad6-726">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="75ad6-727">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="75ad6-727">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="75ad6-728">值: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="75ad6-728">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="75ad6-729">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="75ad6-729">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="75ad6-730">值: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="75ad6-730">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="75ad6-731">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="75ad6-731">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="75ad6-732">值: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="75ad6-732">Value: `System.Data.SqlClient`</span></span>  |

<span data-ttu-id="75ad6-733">**範例**</span><span class="sxs-lookup"><span data-stu-id="75ad6-733">**Example**</span></span>

<span data-ttu-id="75ad6-734">在伺服器上建立自訂連接字串環境變數:</span><span class="sxs-lookup"><span data-stu-id="75ad6-734">A custom connection string environment variable is created on the server:</span></span>

* <span data-ttu-id="75ad6-735">名稱&ndash;`CUSTOMCONNSTR_ReleaseDB`</span><span class="sxs-lookup"><span data-stu-id="75ad6-735">Name &ndash; `CUSTOMCONNSTR_ReleaseDB`</span></span>
* <span data-ttu-id="75ad6-736">值&ndash;`Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span><span class="sxs-lookup"><span data-stu-id="75ad6-736">Value &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span></span>

<span data-ttu-id="75ad6-737">如果`IConfiguration`注入並分配給名為的`_config`欄位,請閱讀該值:</span><span class="sxs-lookup"><span data-stu-id="75ad6-737">If `IConfiguration` is injected and assigned to a field named `_config`, read the value:</span></span>

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a><span data-ttu-id="75ad6-738">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-738">File Configuration Provider</span></span>

<span data-ttu-id="75ad6-739"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> 是用於從檔案系統載入設定的基底類別。</span><span class="sxs-lookup"><span data-stu-id="75ad6-739"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="75ad6-740">下列設定提供者專用於特定檔案類型：</span><span class="sxs-lookup"><span data-stu-id="75ad6-740">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="75ad6-741">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-741">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="75ad6-742">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-742">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="75ad6-743">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-743">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="75ad6-744">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-744">INI Configuration Provider</span></span>

<span data-ttu-id="75ad6-745"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> 會在執行階段從 INI 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="75ad6-745">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="75ad6-746">若要啟用 INI 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="75ad6-746">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="75ad6-747">冒號可用來做為 INI 檔案設定中的區段分隔符號。</span><span class="sxs-lookup"><span data-stu-id="75ad6-747">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="75ad6-748">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="75ad6-748">Overloads permit specifying:</span></span>

* <span data-ttu-id="75ad6-749">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="75ad6-749">Whether the file is optional.</span></span>
* <span data-ttu-id="75ad6-750">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="75ad6-750">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="75ad6-751"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="75ad6-751">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="75ad6-752">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="75ad6-752">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="75ad6-753">INI 設定檔的一般範例：</span><span class="sxs-lookup"><span data-stu-id="75ad6-753">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="75ad6-754">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="75ad6-754">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="75ad6-755">section0:key0</span><span class="sxs-lookup"><span data-stu-id="75ad6-755">section0:key0</span></span>
* <span data-ttu-id="75ad6-756">section0:key1</span><span class="sxs-lookup"><span data-stu-id="75ad6-756">section0:key1</span></span>
* <span data-ttu-id="75ad6-757">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="75ad6-757">section1:subsection:key</span></span>
* <span data-ttu-id="75ad6-758">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="75ad6-758">section2:subsection0:key</span></span>
* <span data-ttu-id="75ad6-759">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="75ad6-759">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="75ad6-760">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-760">JSON Configuration Provider</span></span>

<span data-ttu-id="75ad6-761"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> 會在執行階段從 JSON 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="75ad6-761">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="75ad6-762">若要啟用 JSON 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="75ad6-762">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="75ad6-763">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="75ad6-763">Overloads permit specifying:</span></span>

* <span data-ttu-id="75ad6-764">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="75ad6-764">Whether the file is optional.</span></span>
* <span data-ttu-id="75ad6-765">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="75ad6-765">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="75ad6-766"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="75ad6-766">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="75ad6-767">`AddJsonFile`使用 初始化新的主機產生器時,將自動呼叫兩`CreateDefaultBuilder`次 。</span><span class="sxs-lookup"><span data-stu-id="75ad6-767">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="75ad6-768">會呼叫此方法以從下列位置載入設定：</span><span class="sxs-lookup"><span data-stu-id="75ad6-768">The method is called to load configuration from:</span></span>

* <span data-ttu-id="75ad6-769">*appsettings.json* &ndash; 會先讀取此檔案。</span><span class="sxs-lookup"><span data-stu-id="75ad6-769">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="75ad6-770">檔案的環境版本可以覆寫由 *appsettings.json* 檔案提供的值。</span><span class="sxs-lookup"><span data-stu-id="75ad6-770">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="75ad6-771">*應用設置。{環境}.json*&ndash;檔的環境版本根據[IHosting 環境.環境名稱](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)載入。</span><span class="sxs-lookup"><span data-stu-id="75ad6-771">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="75ad6-772">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="75ad6-772">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="75ad6-773">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="75ad6-773">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="75ad6-774">環境變數。</span><span class="sxs-lookup"><span data-stu-id="75ad6-774">Environment variables.</span></span>
* <span data-ttu-id="75ad6-775">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-775">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="75ad6-776">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="75ad6-776">Command-line arguments.</span></span>

<span data-ttu-id="75ad6-777">會先建立 JSON 設定提供者。</span><span class="sxs-lookup"><span data-stu-id="75ad6-777">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="75ad6-778">因此，使用者祕密、環境變數與命令列引數會覆寫由 *appsettings* 檔案設定的設定。</span><span class="sxs-lookup"><span data-stu-id="75ad6-778">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="75ad6-779">在建置主機時，呼叫 `ConfigureAppConfiguration` 以指定檔案 (除了 *appsettings.json* 和 \* appsettings.{Environment}.json\* 以外) 的應用程式設定：</span><span class="sxs-lookup"><span data-stu-id="75ad6-779">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="75ad6-780">**範例**</span><span class="sxs-lookup"><span data-stu-id="75ad6-780">**Example**</span></span>

<span data-ttu-id="75ad6-781">範例應用利用靜態便利方法`CreateDefaultBuilder`構建主機,其中包括對`AddJsonFile`的 兩個調用:</span><span class="sxs-lookup"><span data-stu-id="75ad6-781">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

* <span data-ttu-id="75ad6-782">第一個呼叫`AddJsonFile`從*應用程式設定載入設定. json*:</span><span class="sxs-lookup"><span data-stu-id="75ad6-782">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="75ad6-783">從`AddJsonFile`應用程式設定載入設定的第二個呼叫 *。環境\.json*.</span><span class="sxs-lookup"><span data-stu-id="75ad6-783">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="75ad6-784">對於*應用設置。在範例應用程式中的開發.json*中,將載入以下檔:</span><span class="sxs-lookup"><span data-stu-id="75ad6-784">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="75ad6-785">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="75ad6-785">Run the sample app.</span></span> <span data-ttu-id="75ad6-786">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="75ad6-786">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="75ad6-787">輸出包含基於應用環境的配置的鍵值對。</span><span class="sxs-lookup"><span data-stu-id="75ad6-787">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="75ad6-788">金鑰`Logging:LogLevel:Default`的日誌級別是在`Debug`開發環境中運行應用時。</span><span class="sxs-lookup"><span data-stu-id="75ad6-788">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="75ad6-789">在「生產」環境中再次運行範例應用:</span><span class="sxs-lookup"><span data-stu-id="75ad6-789">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="75ad6-790">打開*屬性/啟動設置.json*檔。</span><span class="sxs-lookup"><span data-stu-id="75ad6-790">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="75ad6-791">在設定檔`ConfigurationSample`中,將`ASPNETCORE_ENVIRONMENT`環境變數的值變更為`Production`。</span><span class="sxs-lookup"><span data-stu-id="75ad6-791">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="75ad6-792">保存檔案並在命令 shell`dotnet run`中運行應用。</span><span class="sxs-lookup"><span data-stu-id="75ad6-792">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="75ad6-793">*應用設置中的設置。開發.json*不再覆*寫 應用程式設定中的設定*。</span><span class="sxs-lookup"><span data-stu-id="75ad6-793">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="75ad6-794">鍵`Logging:LogLevel:Default`的紀錄等級為`Warning`。</span><span class="sxs-lookup"><span data-stu-id="75ad6-794">The log level for the key `Logging:LogLevel:Default` is `Warning`.</span></span>

### <a name="xml-configuration-provider"></a><span data-ttu-id="75ad6-795">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-795">XML Configuration Provider</span></span>

<span data-ttu-id="75ad6-796"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> 會在執行階段從 XML 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="75ad6-796">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="75ad6-797">若要啟用 XML 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="75ad6-797">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="75ad6-798">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="75ad6-798">Overloads permit specifying:</span></span>

* <span data-ttu-id="75ad6-799">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="75ad6-799">Whether the file is optional.</span></span>
* <span data-ttu-id="75ad6-800">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="75ad6-800">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="75ad6-801"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="75ad6-801">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="75ad6-802">建立設定機碼值組時，會忽略設定檔案的根節點。</span><span class="sxs-lookup"><span data-stu-id="75ad6-802">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="75ad6-803">請勿在檔案中指定文件類型定義 (DTD) 或命名空間。</span><span class="sxs-lookup"><span data-stu-id="75ad6-803">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="75ad6-804">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="75ad6-804">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="75ad6-805">XML 設定檔可以針對重複的區段使用相異元素名稱：</span><span class="sxs-lookup"><span data-stu-id="75ad6-805">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="75ad6-806">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="75ad6-806">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="75ad6-807">section0:key0</span><span class="sxs-lookup"><span data-stu-id="75ad6-807">section0:key0</span></span>
* <span data-ttu-id="75ad6-808">section0:key1</span><span class="sxs-lookup"><span data-stu-id="75ad6-808">section0:key1</span></span>
* <span data-ttu-id="75ad6-809">section1:key0</span><span class="sxs-lookup"><span data-stu-id="75ad6-809">section1:key0</span></span>
* <span data-ttu-id="75ad6-810">section1:key1</span><span class="sxs-lookup"><span data-stu-id="75ad6-810">section1:key1</span></span>

<span data-ttu-id="75ad6-811">若 `name` 屬性是用來區別元素，則可以重複那些使用相同元素名稱的元素：</span><span class="sxs-lookup"><span data-stu-id="75ad6-811">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="75ad6-812">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="75ad6-812">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="75ad6-813">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="75ad6-813">section:section0:key:key0</span></span>
* <span data-ttu-id="75ad6-814">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="75ad6-814">section:section0:key:key1</span></span>
* <span data-ttu-id="75ad6-815">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="75ad6-815">section:section1:key:key0</span></span>
* <span data-ttu-id="75ad6-816">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="75ad6-816">section:section1:key:key1</span></span>

<span data-ttu-id="75ad6-817">屬性可用來提供值：</span><span class="sxs-lookup"><span data-stu-id="75ad6-817">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="75ad6-818">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="75ad6-818">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="75ad6-819">key:attribute</span><span class="sxs-lookup"><span data-stu-id="75ad6-819">key:attribute</span></span>
* <span data-ttu-id="75ad6-820">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="75ad6-820">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="75ad6-821">每個檔案機碼設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-821">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="75ad6-822"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> 使用目錄的檔案做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="75ad6-822">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="75ad6-823">機碼是檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="75ad6-823">The key is the file name.</span></span> <span data-ttu-id="75ad6-824">值包含檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="75ad6-824">The value contains the file's contents.</span></span> <span data-ttu-id="75ad6-825">每個檔案機碼設定提供者是在 Docker 主機案例中使用。</span><span class="sxs-lookup"><span data-stu-id="75ad6-825">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="75ad6-826">若要啟用每個檔案機碼設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="75ad6-826">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="75ad6-827">檔案的 `directoryPath` 必須是絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="75ad6-827">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="75ad6-828">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="75ad6-828">Overloads permit specifying:</span></span>

* <span data-ttu-id="75ad6-829">設定來源的 `Action<KeyPerFileConfigurationSource>` 委派。</span><span class="sxs-lookup"><span data-stu-id="75ad6-829">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="75ad6-830">目錄是否為選擇性與目錄的路徑。</span><span class="sxs-lookup"><span data-stu-id="75ad6-830">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="75ad6-831">雙底線 (`__`) 是做為檔案名稱中的設定金鑰分隔符號使用。</span><span class="sxs-lookup"><span data-stu-id="75ad6-831">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="75ad6-832">例如，檔案名稱 `Logging__LogLevel__System` 會產生設定金鑰 `Logging:LogLevel:System`。</span><span class="sxs-lookup"><span data-stu-id="75ad6-832">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="75ad6-833">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="75ad6-833">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="75ad6-834">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-834">Memory Configuration Provider</span></span>

<span data-ttu-id="75ad6-835"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> 使用記憶體內集合做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="75ad6-835">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="75ad6-836">若要啟用記憶體內集合設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="75ad6-836">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="75ad6-837">設定提供者可以使用 `IEnumerable<KeyValuePair<String,String>>` 來初始化。</span><span class="sxs-lookup"><span data-stu-id="75ad6-837">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="75ad6-838">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="75ad6-838">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="75ad6-839">下列範例會建立組態字典：</span><span class="sxs-lookup"><span data-stu-id="75ad6-839">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="75ad6-840">字典會與 `AddInMemoryCollection` 的呼叫搭配使用以提供組態：</span><span class="sxs-lookup"><span data-stu-id="75ad6-840">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="75ad6-841">GetValue</span><span class="sxs-lookup"><span data-stu-id="75ad6-841">GetValue</span></span>

<span data-ttu-id="75ad6-842">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*)使用指定金鑰從配置中提取單個值,並將其轉換為指定的非集合類型。</span><span class="sxs-lookup"><span data-stu-id="75ad6-842">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="75ad6-843">重載接受預設值。</span><span class="sxs-lookup"><span data-stu-id="75ad6-843">An overload accepts a default value.</span></span>

<span data-ttu-id="75ad6-844">下列範例將：</span><span class="sxs-lookup"><span data-stu-id="75ad6-844">The following example:</span></span>

* <span data-ttu-id="75ad6-845">從具有機碼 `NumberKey` 的組態擷取字串值。</span><span class="sxs-lookup"><span data-stu-id="75ad6-845">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="75ad6-846">若在組態機碼中找不到 `NumberKey`，則會使用預設值 `99`。</span><span class="sxs-lookup"><span data-stu-id="75ad6-846">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="75ad6-847">鍵入值為 `int`。</span><span class="sxs-lookup"><span data-stu-id="75ad6-847">Types the value as an `int`.</span></span>
* <span data-ttu-id="75ad6-848">在 `NumberConfig` 屬性中儲存值供頁面使用。</span><span class="sxs-lookup"><span data-stu-id="75ad6-848">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="75ad6-849">GetSection、GetChildren 與 Exists</span><span class="sxs-lookup"><span data-stu-id="75ad6-849">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="75ad6-850">針對下面的範例，請考慮下列 JSON 檔案。</span><span class="sxs-lookup"><span data-stu-id="75ad6-850">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="75ad6-851">在兩個區段中找到四個機碼，其中一個包括子區段組：</span><span class="sxs-lookup"><span data-stu-id="75ad6-851">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="75ad6-852">當檔案被讀入到設定之後，會建立下列唯一階層式機碼以存放設定值：</span><span class="sxs-lookup"><span data-stu-id="75ad6-852">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="75ad6-853">section0:key0</span><span class="sxs-lookup"><span data-stu-id="75ad6-853">section0:key0</span></span>
* <span data-ttu-id="75ad6-854">section0:key1</span><span class="sxs-lookup"><span data-stu-id="75ad6-854">section0:key1</span></span>
* <span data-ttu-id="75ad6-855">section1:key0</span><span class="sxs-lookup"><span data-stu-id="75ad6-855">section1:key0</span></span>
* <span data-ttu-id="75ad6-856">section1:key1</span><span class="sxs-lookup"><span data-stu-id="75ad6-856">section1:key1</span></span>
* <span data-ttu-id="75ad6-857">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="75ad6-857">section2:subsection0:key0</span></span>
* <span data-ttu-id="75ad6-858">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="75ad6-858">section2:subsection0:key1</span></span>
* <span data-ttu-id="75ad6-859">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="75ad6-859">section2:subsection1:key0</span></span>
* <span data-ttu-id="75ad6-860">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="75ad6-860">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="75ad6-861">GetSection</span><span class="sxs-lookup"><span data-stu-id="75ad6-861">GetSection</span></span>

<span data-ttu-id="75ad6-862">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) 會擷取具有所指定子區段機碼的設定子區段。</span><span class="sxs-lookup"><span data-stu-id="75ad6-862">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="75ad6-863">若要傳回 `section1` 中只包含一個機碼值組的 <xref:Microsoft.Extensions.Configuration.IConfigurationSection>，請呼叫 `GetSection` 並提供區段名稱：</span><span class="sxs-lookup"><span data-stu-id="75ad6-863">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="75ad6-864">`configSection` 沒有值，只有索引鍵和路徑。</span><span class="sxs-lookup"><span data-stu-id="75ad6-864">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="75ad6-865">同樣地，若要取得 `section2:subsection0` 中之機碼的值，請呼叫 `GetSection` 並提供區段路徑：</span><span class="sxs-lookup"><span data-stu-id="75ad6-865">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="75ad6-866">`GetSection` 絕不會傳回 `null`。</span><span class="sxs-lookup"><span data-stu-id="75ad6-866">`GetSection` never returns `null`.</span></span> <span data-ttu-id="75ad6-867">若找不到相符的區段，會傳回空的 `IConfigurationSection`。</span><span class="sxs-lookup"><span data-stu-id="75ad6-867">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="75ad6-868">當 `GetSection` 傳回相符區段時，未填入 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value>。</span><span class="sxs-lookup"><span data-stu-id="75ad6-868">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="75ad6-869">當區段存在時，會傳回 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> 與 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path>。</span><span class="sxs-lookup"><span data-stu-id="75ad6-869">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="75ad6-870">GetChildren</span><span class="sxs-lookup"><span data-stu-id="75ad6-870">GetChildren</span></span>

<span data-ttu-id="75ad6-871">對 `section2` 上之 [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) 的呼叫會取得包括下列項目的 `IEnumerable<IConfigurationSection>`：</span><span class="sxs-lookup"><span data-stu-id="75ad6-871">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="75ad6-872">Exists</span><span class="sxs-lookup"><span data-stu-id="75ad6-872">Exists</span></span>

<span data-ttu-id="75ad6-873">使用 [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) 來判斷設定區段是否存在：</span><span class="sxs-lookup"><span data-stu-id="75ad6-873">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="75ad6-874">以範例資料為例， `sectionExists` 是 `false`，因未設定資料中沒有 `section2:subsection2` 區段。</span><span class="sxs-lookup"><span data-stu-id="75ad6-874">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="75ad6-875">繫結至物件圖形</span><span class="sxs-lookup"><span data-stu-id="75ad6-875">Bind to an object graph</span></span>

<span data-ttu-id="75ad6-876"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 可以繫結整個 POCO 物件圖形。</span><span class="sxs-lookup"><span data-stu-id="75ad6-876"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="75ad6-877">與綁定簡單物件一樣,僅綁定公共讀/寫屬性。</span><span class="sxs-lookup"><span data-stu-id="75ad6-877">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="75ad6-878">範例包含 `TvShow` 模型，其物件圖形包括 `Metadata` 與 `Actors` 類別 (*Models/TvShow.cs*)：</span><span class="sxs-lookup"><span data-stu-id="75ad6-878">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="75ad6-879">範例應用程式有 *tvshow.xml* 檔案，其中包含設定資料：</span><span class="sxs-lookup"><span data-stu-id="75ad6-879">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="75ad6-880">設定會被繫結到整個 `TvShow` 物件 (使用 `Bind` 方法)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-880">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="75ad6-881">已繫結的執行個體會被指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="75ad6-881">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="75ad6-882">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*)綁定並返回指定的類型。</span><span class="sxs-lookup"><span data-stu-id="75ad6-882">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="75ad6-883">`Get<T>` 比使用 `Bind` 更方便。</span><span class="sxs-lookup"><span data-stu-id="75ad6-883">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="75ad6-884">以下代碼展示如何與前面的範例一`Get<T>`起使用:</span><span class="sxs-lookup"><span data-stu-id="75ad6-884">The following code shows how to use `Get<T>` with the preceding example:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="75ad6-885">將陣列繫結到類別</span><span class="sxs-lookup"><span data-stu-id="75ad6-885">Bind an array to a class</span></span>

<span data-ttu-id="75ad6-886">範例應用程式示範此節中解釋的概念。\*\*</span><span class="sxs-lookup"><span data-stu-id="75ad6-886">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="75ad6-887"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="75ad6-887">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="75ad6-888">公開數位`:0:`鍵段`:1:`&hellip;`:{n}:`(、 、 ) 的任何陣列格式都能夠綁定到 POCO 類陣列。</span><span class="sxs-lookup"><span data-stu-id="75ad6-888">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="75ad6-889">繫結是由慣例提供。</span><span class="sxs-lookup"><span data-stu-id="75ad6-889">Binding is provided by convention.</span></span> <span data-ttu-id="75ad6-890">自訂設定提供者不需要實作陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="75ad6-890">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="75ad6-891">**記憶體內陣列處理**</span><span class="sxs-lookup"><span data-stu-id="75ad6-891">**In-memory array processing**</span></span>

<span data-ttu-id="75ad6-892">考慮下表中顯示的設定機碼與值。</span><span class="sxs-lookup"><span data-stu-id="75ad6-892">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="75ad6-893">Key</span><span class="sxs-lookup"><span data-stu-id="75ad6-893">Key</span></span>             | <span data-ttu-id="75ad6-894">值</span><span class="sxs-lookup"><span data-stu-id="75ad6-894">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="75ad6-895">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="75ad6-895">array:entries:0</span></span> | <span data-ttu-id="75ad6-896">value0</span><span class="sxs-lookup"><span data-stu-id="75ad6-896">value0</span></span> |
| <span data-ttu-id="75ad6-897">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="75ad6-897">array:entries:1</span></span> | <span data-ttu-id="75ad6-898">value1</span><span class="sxs-lookup"><span data-stu-id="75ad6-898">value1</span></span> |
| <span data-ttu-id="75ad6-899">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="75ad6-899">array:entries:2</span></span> | <span data-ttu-id="75ad6-900">value2</span><span class="sxs-lookup"><span data-stu-id="75ad6-900">value2</span></span> |
| <span data-ttu-id="75ad6-901">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="75ad6-901">array:entries:4</span></span> | <span data-ttu-id="75ad6-902">value4</span><span class="sxs-lookup"><span data-stu-id="75ad6-902">value4</span></span> |
| <span data-ttu-id="75ad6-903">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="75ad6-903">array:entries:5</span></span> | <span data-ttu-id="75ad6-904">value5</span><span class="sxs-lookup"><span data-stu-id="75ad6-904">value5</span></span> |

<span data-ttu-id="75ad6-905">這些機碼與值是使用「記憶體設定提供者」在範例應用程式中載入：</span><span class="sxs-lookup"><span data-stu-id="75ad6-905">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

<span data-ttu-id="75ad6-906">陣列會跳過索引 &num;3 的值。</span><span class="sxs-lookup"><span data-stu-id="75ad6-906">The array skips a value for index &num;3.</span></span> <span data-ttu-id="75ad6-907">設定繫結程式無法繫結 Null 值或在已繫結的物件中建立 Null 項目，這在示範繫結此陣列的結果到某物件的時候已經很清楚。</span><span class="sxs-lookup"><span data-stu-id="75ad6-907">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="75ad6-908">在範例應用程式中，POCO 類別可用來存放繫結設定資料：</span><span class="sxs-lookup"><span data-stu-id="75ad6-908">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="75ad6-909">設定資料已繫結到物件：</span><span class="sxs-lookup"><span data-stu-id="75ad6-909">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="75ad6-910">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*)也可以使用語法,這會產生更緊湊的代碼:</span><span class="sxs-lookup"><span data-stu-id="75ad6-910">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="75ad6-911">繫結物件 (`ArrayExample`的執行個體) 會從設定接收陣列資料。</span><span class="sxs-lookup"><span data-stu-id="75ad6-911">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="75ad6-912">`ArrayExample.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="75ad6-912">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="75ad6-913">`ArrayExample.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="75ad6-913">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="75ad6-914">0</span><span class="sxs-lookup"><span data-stu-id="75ad6-914">0</span></span>                            | <span data-ttu-id="75ad6-915">value0</span><span class="sxs-lookup"><span data-stu-id="75ad6-915">value0</span></span>                       |
| <span data-ttu-id="75ad6-916">1</span><span class="sxs-lookup"><span data-stu-id="75ad6-916">1</span></span>                            | <span data-ttu-id="75ad6-917">value1</span><span class="sxs-lookup"><span data-stu-id="75ad6-917">value1</span></span>                       |
| <span data-ttu-id="75ad6-918">2</span><span class="sxs-lookup"><span data-stu-id="75ad6-918">2</span></span>                            | <span data-ttu-id="75ad6-919">value2</span><span class="sxs-lookup"><span data-stu-id="75ad6-919">value2</span></span>                       |
| <span data-ttu-id="75ad6-920">3</span><span class="sxs-lookup"><span data-stu-id="75ad6-920">3</span></span>                            | <span data-ttu-id="75ad6-921">value4</span><span class="sxs-lookup"><span data-stu-id="75ad6-921">value4</span></span>                       |
| <span data-ttu-id="75ad6-922">4</span><span class="sxs-lookup"><span data-stu-id="75ad6-922">4</span></span>                            | <span data-ttu-id="75ad6-923">value5</span><span class="sxs-lookup"><span data-stu-id="75ad6-923">value5</span></span>                       |

<span data-ttu-id="75ad6-924">繫結物件中的索引 &num;3 存放 `array:4` 設定機碼與其 `value4` 的設定資料。</span><span class="sxs-lookup"><span data-stu-id="75ad6-924">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="75ad6-925">當繫結包含陣列的設定資料時，設定機碼中的陣列索引只會用來列舉設定資料 (當建立物件時)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-925">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="75ad6-926">設定資料中不能保留 Null 值，當設定機碼中的陣列略過一或多個索引時，不會在繫結物件中建立 Null 值項目。</span><span class="sxs-lookup"><span data-stu-id="75ad6-926">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="75ad6-927">由會在設定中產生正確機碼值組的任何設定提供者繫結到 `ArrayExample` 執行個體之前，可以提供索引 &num;3 缺少的設定項目。</span><span class="sxs-lookup"><span data-stu-id="75ad6-927">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="75ad6-928">若範例包含具有缺少之機碼值組的額外 JSON 設定提供者，`ArrayExample.Entries` 會符合完整的設定陣列：</span><span class="sxs-lookup"><span data-stu-id="75ad6-928">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="75ad6-929">*missing_value.json*：</span><span class="sxs-lookup"><span data-stu-id="75ad6-929">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="75ad6-930">在 `ConfigureAppConfiguration` 中：</span><span class="sxs-lookup"><span data-stu-id="75ad6-930">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="75ad6-931">表格中顯示的機碼值組會載入到設定中。</span><span class="sxs-lookup"><span data-stu-id="75ad6-931">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="75ad6-932">Key</span><span class="sxs-lookup"><span data-stu-id="75ad6-932">Key</span></span>             | <span data-ttu-id="75ad6-933">值</span><span class="sxs-lookup"><span data-stu-id="75ad6-933">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="75ad6-934">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="75ad6-934">array:entries:3</span></span> | <span data-ttu-id="75ad6-935">value3</span><span class="sxs-lookup"><span data-stu-id="75ad6-935">value3</span></span> |

<span data-ttu-id="75ad6-936">在「JSON 設定提供者」包含索引 &num;3 的項目之後，若繫結 `ArrayExample` 類別執行個體，`ArrayExample.Entries` 陣列會包括這些值：</span><span class="sxs-lookup"><span data-stu-id="75ad6-936">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="75ad6-937">`ArrayExample.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="75ad6-937">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="75ad6-938">`ArrayExample.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="75ad6-938">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="75ad6-939">0</span><span class="sxs-lookup"><span data-stu-id="75ad6-939">0</span></span>                            | <span data-ttu-id="75ad6-940">value0</span><span class="sxs-lookup"><span data-stu-id="75ad6-940">value0</span></span>                       |
| <span data-ttu-id="75ad6-941">1</span><span class="sxs-lookup"><span data-stu-id="75ad6-941">1</span></span>                            | <span data-ttu-id="75ad6-942">value1</span><span class="sxs-lookup"><span data-stu-id="75ad6-942">value1</span></span>                       |
| <span data-ttu-id="75ad6-943">2</span><span class="sxs-lookup"><span data-stu-id="75ad6-943">2</span></span>                            | <span data-ttu-id="75ad6-944">value2</span><span class="sxs-lookup"><span data-stu-id="75ad6-944">value2</span></span>                       |
| <span data-ttu-id="75ad6-945">3</span><span class="sxs-lookup"><span data-stu-id="75ad6-945">3</span></span>                            | <span data-ttu-id="75ad6-946">value3</span><span class="sxs-lookup"><span data-stu-id="75ad6-946">value3</span></span>                       |
| <span data-ttu-id="75ad6-947">4</span><span class="sxs-lookup"><span data-stu-id="75ad6-947">4</span></span>                            | <span data-ttu-id="75ad6-948">value4</span><span class="sxs-lookup"><span data-stu-id="75ad6-948">value4</span></span>                       |
| <span data-ttu-id="75ad6-949">5</span><span class="sxs-lookup"><span data-stu-id="75ad6-949">5</span></span>                            | <span data-ttu-id="75ad6-950">value5</span><span class="sxs-lookup"><span data-stu-id="75ad6-950">value5</span></span>                       |

<span data-ttu-id="75ad6-951">**JSON 陣列處理**</span><span class="sxs-lookup"><span data-stu-id="75ad6-951">**JSON array processing**</span></span>

<span data-ttu-id="75ad6-952">若 JSON 檔案包含陣列，會為具有以零為基礎之區段索引的陣列元素建立設定機碼。</span><span class="sxs-lookup"><span data-stu-id="75ad6-952">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="75ad6-953">在下列設定檔中，`subsection` 是陣列：</span><span class="sxs-lookup"><span data-stu-id="75ad6-953">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="75ad6-954">「JSON 設定提供者」會將設定資料讀入到下列機碼值組：</span><span class="sxs-lookup"><span data-stu-id="75ad6-954">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="75ad6-955">Key</span><span class="sxs-lookup"><span data-stu-id="75ad6-955">Key</span></span>                     | <span data-ttu-id="75ad6-956">值</span><span class="sxs-lookup"><span data-stu-id="75ad6-956">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="75ad6-957">json_array:key</span><span class="sxs-lookup"><span data-stu-id="75ad6-957">json_array:key</span></span>          | <span data-ttu-id="75ad6-958">valueA</span><span class="sxs-lookup"><span data-stu-id="75ad6-958">valueA</span></span> |
| <span data-ttu-id="75ad6-959">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="75ad6-959">json_array:subsection:0</span></span> | <span data-ttu-id="75ad6-960">valueB</span><span class="sxs-lookup"><span data-stu-id="75ad6-960">valueB</span></span> |
| <span data-ttu-id="75ad6-961">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="75ad6-961">json_array:subsection:1</span></span> | <span data-ttu-id="75ad6-962">valueC</span><span class="sxs-lookup"><span data-stu-id="75ad6-962">valueC</span></span> |
| <span data-ttu-id="75ad6-963">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="75ad6-963">json_array:subsection:2</span></span> | <span data-ttu-id="75ad6-964">valueD</span><span class="sxs-lookup"><span data-stu-id="75ad6-964">valueD</span></span> |

<span data-ttu-id="75ad6-965">在範例應用程式中，下列 POCO 類別可用來繫結設定機碼值組：</span><span class="sxs-lookup"><span data-stu-id="75ad6-965">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="75ad6-966">在繫結之後，`JsonArrayExample.Key` 會存放值 `valueA`。</span><span class="sxs-lookup"><span data-stu-id="75ad6-966">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="75ad6-967">子區段值會存放在 POCO 陣列屬性 `Subsection` 中。</span><span class="sxs-lookup"><span data-stu-id="75ad6-967">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="75ad6-968">`JsonArrayExample.Subsection` 索引</span><span class="sxs-lookup"><span data-stu-id="75ad6-968">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="75ad6-969">`JsonArrayExample.Subsection` 值</span><span class="sxs-lookup"><span data-stu-id="75ad6-969">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="75ad6-970">0</span><span class="sxs-lookup"><span data-stu-id="75ad6-970">0</span></span>                                   | <span data-ttu-id="75ad6-971">valueB</span><span class="sxs-lookup"><span data-stu-id="75ad6-971">valueB</span></span>                              |
| <span data-ttu-id="75ad6-972">1</span><span class="sxs-lookup"><span data-stu-id="75ad6-972">1</span></span>                                   | <span data-ttu-id="75ad6-973">valueC</span><span class="sxs-lookup"><span data-stu-id="75ad6-973">valueC</span></span>                              |
| <span data-ttu-id="75ad6-974">2</span><span class="sxs-lookup"><span data-stu-id="75ad6-974">2</span></span>                                   | <span data-ttu-id="75ad6-975">valueD</span><span class="sxs-lookup"><span data-stu-id="75ad6-975">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="75ad6-976">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="75ad6-976">Custom configuration provider</span></span>

<span data-ttu-id="75ad6-977">範例應用程式示範如何建立會使用 [Entity Framework (EF)](/ef/core/) 從資料庫讀取設定機碼值組的基本設定提供者。</span><span class="sxs-lookup"><span data-stu-id="75ad6-977">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="75ad6-978">提供者具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="75ad6-978">The provider has the following characteristics:</span></span>

* <span data-ttu-id="75ad6-979">EF 記憶體內資料庫會用於示範用途。</span><span class="sxs-lookup"><span data-stu-id="75ad6-979">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="75ad6-980">若要使用需要連接字串的資料庫，請實作第二個 `ConfigurationBuilder` 以從另一個設定提供者提供連接字串。</span><span class="sxs-lookup"><span data-stu-id="75ad6-980">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="75ad6-981">啟動時，該提供者會將資料庫資料表讀入到設定中。</span><span class="sxs-lookup"><span data-stu-id="75ad6-981">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="75ad6-982">該提供者不會以個別機碼為基礎查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="75ad6-982">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="75ad6-983">未實作變更時重新載入，因此在應用程式啟動後更新資料庫對應用程式設定沒有影響。</span><span class="sxs-lookup"><span data-stu-id="75ad6-983">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="75ad6-984">定義 `EFConfigurationValue` 實體來在資料庫中存放設定值。</span><span class="sxs-lookup"><span data-stu-id="75ad6-984">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="75ad6-985">*Models/EFConfigurationValue.cs*：</span><span class="sxs-lookup"><span data-stu-id="75ad6-985">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="75ad6-986">新增 `EFConfigurationContext` 以存放及存取已設定的值。</span><span class="sxs-lookup"><span data-stu-id="75ad6-986">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="75ad6-987">*EFConfigurationProvider/EFConfigurationContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="75ad6-987">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="75ad6-988">建立會實作 <xref:Microsoft.Extensions.Configuration.IConfigurationSource> 的類別。</span><span class="sxs-lookup"><span data-stu-id="75ad6-988">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="75ad6-989">*EFConfigurationProvider/EFConfigurationSource.cs*：</span><span class="sxs-lookup"><span data-stu-id="75ad6-989">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="75ad6-990">透過繼承自 <xref:Microsoft.Extensions.Configuration.ConfigurationProvider> 來建立自訂設定提供者。</span><span class="sxs-lookup"><span data-stu-id="75ad6-990">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="75ad6-991">若資料庫是空的，設定提供者會初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="75ad6-991">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="75ad6-992">*EFConfigurationProvider/EFConfigurationProvider.cs*：</span><span class="sxs-lookup"><span data-stu-id="75ad6-992">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="75ad6-993">`AddEFConfiguration` 擴充方法允許新增設定來源到 `ConfigurationBuilder`。</span><span class="sxs-lookup"><span data-stu-id="75ad6-993">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="75ad6-994">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="75ad6-994">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="75ad6-995">下列程式碼顯示如何在 *Program.cs* 中使用自訂 `EFConfigurationProvider`：</span><span class="sxs-lookup"><span data-stu-id="75ad6-995">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="75ad6-996">在啟動期間存取組態</span><span class="sxs-lookup"><span data-stu-id="75ad6-996">Access configuration during startup</span></span>

<span data-ttu-id="75ad6-997">將 `IConfiguration` 插入到 `Startup` 建構函式，以存取 `Startup.ConfigureServices` 中的設定值。</span><span class="sxs-lookup"><span data-stu-id="75ad6-997">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="75ad6-998">若要存取 `Startup.Configure` 中的設定，請直接將 `IConfiguration` 插入到方法或從建構函式使用執行個體：</span><span class="sxs-lookup"><span data-stu-id="75ad6-998">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="75ad6-999">如需使用啟動方便方法來存取設定的範例，請參閱[應用程式啟動：方便方法](xref:fundamentals/startup#convenience-methods)。</span><span class="sxs-lookup"><span data-stu-id="75ad6-999">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="75ad6-1000">存取 Razor Pages 頁面或 MVC 檢視中的設定</span><span class="sxs-lookup"><span data-stu-id="75ad6-1000">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="75ad6-1001">若要在 [Razor Pages 頁面] 頁面或 MVC 檢視中存取組態設定，請針對 [Microsoft.Extensions.Configuration 命名空間](xref:Microsoft.Extensions.Configuration)[using 指示詞](xref:mvc/views/razor#using) ([C# 參考：using 指示詞](/dotnet/csharp/language-reference/keywords/using-directive))，並將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 插入到頁面或檢視中。</span><span class="sxs-lookup"><span data-stu-id="75ad6-1001">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="75ad6-1002">在 [Razor 頁面] 頁面中：</span><span class="sxs-lookup"><span data-stu-id="75ad6-1002">In a Razor Pages page:</span></span>

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

<span data-ttu-id="75ad6-1003">在 MVC 檢視中：</span><span class="sxs-lookup"><span data-stu-id="75ad6-1003">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="75ad6-1004">從外部組件新增設定</span><span class="sxs-lookup"><span data-stu-id="75ad6-1004">Add configuration from an external assembly</span></span>

<span data-ttu-id="75ad6-1005"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。</span><span class="sxs-lookup"><span data-stu-id="75ad6-1005">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="75ad6-1006">如需詳細資訊，請參閱 <xref:fundamentals/configuration/platform-specific-configuration>。</span><span class="sxs-lookup"><span data-stu-id="75ad6-1006">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="75ad6-1007">其他資源</span><span class="sxs-lookup"><span data-stu-id="75ad6-1007">Additional resources</span></span>

* <xref:fundamentals/configuration/options>

::: moniker-end
