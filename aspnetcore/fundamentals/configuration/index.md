---
title: ASP.NET Core 的設定
author: rick-anderson
description: 了解如何使用組態 API 設定 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/29/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: e1237db2625a127bfa5c31ac29b4394be6941b2f
ms.sourcegitcommit: 9e2b3aaccc9a41291eb23bf4561159e79cf6bc9d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2020
ms.locfileid: "79546337"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="c8758-103">ASP.NET Core 的設定</span><span class="sxs-lookup"><span data-stu-id="c8758-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="c8758-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Kirk Larkin](https://twitter.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="c8758-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Kirk Larkin](https://twitter.com/serpent5)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c8758-105">ASP.NET Core 中的設定是使用一或多個設定[提供者](#cp)來執行。</span><span class="sxs-lookup"><span data-stu-id="c8758-105">Configuration in ASP.NET Core is performed using one or more [configuration providers](#cp).</span></span> <span data-ttu-id="c8758-106">設定提供者會使用各種不同的設定來源，從機碼值組讀取設定資料：</span><span class="sxs-lookup"><span data-stu-id="c8758-106">Configuration providers read configuration data from key-value pairs using a variety of configuration sources:</span></span>

* <span data-ttu-id="c8758-107">設定檔案，例如*appsettings. json*</span><span class="sxs-lookup"><span data-stu-id="c8758-107">Settings files, such as *appsettings.json*</span></span>
* <span data-ttu-id="c8758-108">環境變數</span><span class="sxs-lookup"><span data-stu-id="c8758-108">Environment variables</span></span>
* <span data-ttu-id="c8758-109">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="c8758-109">Azure Key Vault</span></span>
* <span data-ttu-id="c8758-110">Azure 應用程式組態</span><span class="sxs-lookup"><span data-stu-id="c8758-110">Azure App Configuration</span></span>
* <span data-ttu-id="c8758-111">命令列引數</span><span class="sxs-lookup"><span data-stu-id="c8758-111">Command-line arguments</span></span>
* <span data-ttu-id="c8758-112">已安裝或建立的自訂提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-112">Custom providers, installed or created</span></span>
* <span data-ttu-id="c8758-113">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="c8758-113">Directory files</span></span>
* <span data-ttu-id="c8758-114">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="c8758-114">In-memory .NET objects</span></span>

<span data-ttu-id="c8758-115">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c8758-115">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<a name="default"></a>

## <a name="default-configuration"></a><span data-ttu-id="c8758-116">預設組態</span><span class="sxs-lookup"><span data-stu-id="c8758-116">Default configuration</span></span>

<span data-ttu-id="c8758-117">ASP.NET Core 以[dotnet new](/dotnet/core/tools/dotnet-new)或 Visual Studio 建立的 web 應用程式會產生下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="c8758-117">ASP.NET Core web apps created with [dotnet new](/dotnet/core/tools/dotnet-new) or Visual Studio generate the following code:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Program.cs?name=snippet&highlight=9)]

 <span data-ttu-id="c8758-118"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> 會以下列順序提供應用程式的預設組態：</span><span class="sxs-lookup"><span data-stu-id="c8758-118"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> provides default configuration for the app in the following order:</span></span>

1. <span data-ttu-id="c8758-119">[ChainedConfigurationProvider](xref:Microsoft.Extensions.Configuration.ChainedConfigurationSource) ：加入現有的 `IConfiguration` 做為來源。</span><span class="sxs-lookup"><span data-stu-id="c8758-119">[ChainedConfigurationProvider](xref:Microsoft.Extensions.Configuration.ChainedConfigurationSource) :  Adds an existing `IConfiguration` as a source.</span></span> <span data-ttu-id="c8758-120">在預設設定案例中，會新增[主機](#hvac)配置，並將其設為_應用程式_設定的第一個來源。</span><span class="sxs-lookup"><span data-stu-id="c8758-120">In the default configuration case, adds the [host](#hvac) configuration and setting it as the first source for the _app_ configuration.</span></span>
1. <span data-ttu-id="c8758-121">使用[json 設定提供者](#file-configuration-provider)的[appsettings。](#appsettingsjson)</span><span class="sxs-lookup"><span data-stu-id="c8758-121">[appsettings.json](#appsettingsjson) using the [JSON configuration provider](#file-configuration-provider).</span></span>
1. <span data-ttu-id="c8758-122">*appsettings。* 使用[json 設定提供者](#file-configuration-provider)`Environment`*json* 。</span><span class="sxs-lookup"><span data-stu-id="c8758-122">*appsettings.*`Environment`*.json* using the [JSON configuration provider](#file-configuration-provider).</span></span> <span data-ttu-id="c8758-123">例如， *appsettings*。***生產***。*json*和*appsettings*。***開發***。*json*。</span><span class="sxs-lookup"><span data-stu-id="c8758-123">For example, *appsettings*.***Production***.*json* and *appsettings*.***Development***.*json*.</span></span>
1. <span data-ttu-id="c8758-124">應用程式在 `Development` 環境中執行時的[密碼](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="c8758-124">[App secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
1. <span data-ttu-id="c8758-125">使用[環境變數設定提供者](#evcp)的環境變數。</span><span class="sxs-lookup"><span data-stu-id="c8758-125">Environment variables using the [Environment Variables configuration provider](#evcp).</span></span>
1. <span data-ttu-id="c8758-126">使用[命令列設定提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="c8758-126">Command-line arguments using the [Command-line configuration provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="c8758-127">稍後新增的設定提供者會覆寫先前的金鑰設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-127">Configuration providers that are added later override previous key settings.</span></span> <span data-ttu-id="c8758-128">例如，如果 `MyKey` 同時在*appsettings*和環境中設定，則會使用環境值。</span><span class="sxs-lookup"><span data-stu-id="c8758-128">For example, if `MyKey` is set in both *appsettings.json* and the environment, the environment value is used.</span></span> <span data-ttu-id="c8758-129">使用預設的設定提供者時，[命令列設定提供者](#command-line-configuration-provider)會覆寫所有其他提供者。</span><span class="sxs-lookup"><span data-stu-id="c8758-129">Using the default configuration providers, the  [Command-line configuration provider](#command-line-configuration-provider) overrides all other providers.</span></span>

<span data-ttu-id="c8758-130">如需 `CreateDefaultBuilder`的詳細資訊，請參閱預設產生器[設定](xref:fundamentals/host/generic-host#default-builder-settings)。</span><span class="sxs-lookup"><span data-stu-id="c8758-130">For more information on `CreateDefaultBuilder`, see [Default builder settings](xref:fundamentals/host/generic-host#default-builder-settings).</span></span>

<span data-ttu-id="c8758-131">下列程式碼會依新增的順序顯示已啟用的設定提供者：</span><span class="sxs-lookup"><span data-stu-id="c8758-131">The following code displays the enabled configuration providers in the order they were added:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Index2.cshtml.cs?name=snippet)]

### <a name="appsettingsjson"></a><span data-ttu-id="c8758-132">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="c8758-132">appsettings.json</span></span>

<span data-ttu-id="c8758-133">請考慮下列*appsettings json*檔案：</span><span class="sxs-lookup"><span data-stu-id="c8758-133">Consider the following *appsettings.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/appsettings.json)]

<span data-ttu-id="c8758-134">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)中的下列程式碼會顯示上述幾個設定值：</span><span class="sxs-lookup"><span data-stu-id="c8758-134">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="c8758-135">預設 <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> 會依照下列順序載入設定：</span><span class="sxs-lookup"><span data-stu-id="c8758-135">The default <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration in the following order:</span></span>

1. <span data-ttu-id="c8758-136">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="c8758-136">*appsettings.json*</span></span>
1. <span data-ttu-id="c8758-137">*appsettings。* `Environment` *. json* ：例如， *appsettings*。***生產***。*json*和*appsettings*。***開發***。*json*檔案。</span><span class="sxs-lookup"><span data-stu-id="c8758-137">*appsettings.*`Environment`*.json* : For example, the *appsettings*.***Production***.*json* and *appsettings*.***Development***.*json* files.</span></span> <span data-ttu-id="c8758-138">檔案的環境版本是根據[IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)載入。</span><span class="sxs-lookup"><span data-stu-id="c8758-138">The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span> <span data-ttu-id="c8758-139">如需詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="c8758-139">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="c8758-140">*appsettings*。`Environment`。*json*值會覆寫*appsettings*中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="c8758-140">*appsettings*.`Environment`.*json* values override keys in *appsettings.json*.</span></span> <span data-ttu-id="c8758-141">例如，預設為：</span><span class="sxs-lookup"><span data-stu-id="c8758-141">For example, by default:</span></span>

* <span data-ttu-id="c8758-142">在開發中， *appsettings*。***開發***。*json*設定會覆寫在*appsettings*中找到的值。</span><span class="sxs-lookup"><span data-stu-id="c8758-142">In development, *appsettings*.***Development***.*json* configuration overwrites values found in *appsettings.json*.</span></span>
* <span data-ttu-id="c8758-143">在生產環境中， *appsettings*。***生產***。*json*設定會覆寫在*appsettings*中找到的值。</span><span class="sxs-lookup"><span data-stu-id="c8758-143">In production, *appsettings*.***Production***.*json* configuration overwrites values found in *appsettings.json*.</span></span> <span data-ttu-id="c8758-144">例如，將應用程式部署至 Azure 時。</span><span class="sxs-lookup"><span data-stu-id="c8758-144">For example, when deploying the app to Azure.</span></span>

<a name="optpat"></a>

#### <a name="bind-hierarchical-configuration-data-using-the-options-pattern"></a><span data-ttu-id="c8758-145">使用選項模式系結階層式設定資料</span><span class="sxs-lookup"><span data-stu-id="c8758-145">Bind hierarchical configuration data using the options pattern</span></span>

<span data-ttu-id="c8758-146">讀取相關設定值的慣用方法是使用[選項模式](xref:fundamentals/configuration/options)。</span><span class="sxs-lookup"><span data-stu-id="c8758-146">The preferred way to read related configuration values is using the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="c8758-147">例如，若要讀取下列設定值：</span><span class="sxs-lookup"><span data-stu-id="c8758-147">For example, to read the following configuration values:</span></span>

```json
  "Position": {
    "Title": "Editor",
    "Name": "Joe Smith"
  }
```

<span data-ttu-id="c8758-148">建立下列 `PositionOptions` 類別：</span><span class="sxs-lookup"><span data-stu-id="c8758-148">Create the following `PositionOptions` class:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Options/PositionOptions.cs?name=snippet)]

<span data-ttu-id="c8758-149">類型的所有公用讀寫屬性都會系結。</span><span class="sxs-lookup"><span data-stu-id="c8758-149">All the public read-write properties of the type are bound.</span></span> <span data-ttu-id="c8758-150">欄位***未***系結。</span><span class="sxs-lookup"><span data-stu-id="c8758-150">Fields are ***not*** bound.</span></span>

<span data-ttu-id="c8758-151">下列程式碼範例：</span><span class="sxs-lookup"><span data-stu-id="c8758-151">The following code:</span></span>

* <span data-ttu-id="c8758-152">呼叫[ConfigurationBinder](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*) ，將 `PositionOptions` 類別系結至 `Position` 區段。</span><span class="sxs-lookup"><span data-stu-id="c8758-152">Calls [ConfigurationBinder.Bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*) to bind the `PositionOptions` class to the `Position` section.</span></span>
* <span data-ttu-id="c8758-153">顯示 `Position` 設定資料。</span><span class="sxs-lookup"><span data-stu-id="c8758-153">Displays the `Position` configuration data.</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test22.cshtml.cs?name=snippet)]

<span data-ttu-id="c8758-154">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*)系結並傳回指定的型別。</span><span class="sxs-lookup"><span data-stu-id="c8758-154">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="c8758-155">`ConfigurationBinder.Get<T>` 可能比使用 `ConfigurationBinder.Bind`更方便。</span><span class="sxs-lookup"><span data-stu-id="c8758-155">`ConfigurationBinder.Get<T>` may be more convenient than using `ConfigurationBinder.Bind`.</span></span> <span data-ttu-id="c8758-156">下列程式碼示範如何使用 `ConfigurationBinder.Get<T>` 搭配 `PositionOptions` 類別：</span><span class="sxs-lookup"><span data-stu-id="c8758-156">The following code shows how to use `ConfigurationBinder.Get<T>` with the `PositionOptions` class:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test21.cshtml.cs?name=snippet)]

<span data-ttu-id="c8758-157">使用 [***選項] 模式***時，另一種方法是系結 `Position` 區段，並將它加入至相依性[插入服務容器](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="c8758-157">An alternative approach when using the ***options pattern*** is to bind the `Position` section and add it to the [dependency injection service container](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c8758-158">在下列程式碼中，`PositionOptions` 會使用 <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> 新增至服務容器，並系結至設定：</span><span class="sxs-lookup"><span data-stu-id="c8758-158">In the following code, `PositionOptions` is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Startup.cs?name=snippet)]

<span data-ttu-id="c8758-159">使用上述程式碼，下列程式碼會讀取位置選項：</span><span class="sxs-lookup"><span data-stu-id="c8758-159">Using the preceding code, the following code reads the position options:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test2.cshtml.cs?name=snippet)]

<span data-ttu-id="c8758-160">使用[預設](#default)設定時，會啟用*appsettings*與[reloadOnChange： true](https://github.com/dotnet/extensions/blob/release/3.1/src/Hosting/Hosting/src/Host.cs#L74-L75)的*json 和`Environment`* *appsettings*檔案。</span><span class="sxs-lookup"><span data-stu-id="c8758-160">Using the [default](#default) configuration, the *appsettings.json* and *appsettings.*`Environment`*.json* files are enabled with [reloadOnChange: true](https://github.com/dotnet/extensions/blob/release/3.1/src/Hosting/Hosting/src/Host.cs#L74-L75).</span></span> <span data-ttu-id="c8758-161">[Json 設定提供者](#jcp)會讀取應用程式啟動***後***，對*appsettings*和`Environment`*appsettings* *的變更。*</span><span class="sxs-lookup"><span data-stu-id="c8758-161">Changes made to the *appsettings.json* and *appsettings.*`Environment`*.json* file ***after*** the app starts are read by the [JSON configuration provider](#jcp).</span></span>

<span data-ttu-id="c8758-162">如需新增其他 JSON 設定檔的相關資訊，請參閱本檔中的[JSON 設定提供者](#jcp)。</span><span class="sxs-lookup"><span data-stu-id="c8758-162">See [JSON configuration provider](#jcp) in this document for information on adding additional JSON configuration files.</span></span>

<a name="security"></a>

## <a name="security-and-secret-manager"></a><span data-ttu-id="c8758-163">安全性和秘密管理員</span><span class="sxs-lookup"><span data-stu-id="c8758-163">Security and secret manager</span></span>

<span data-ttu-id="c8758-164">設定資料方針：</span><span class="sxs-lookup"><span data-stu-id="c8758-164">Configuration data guidelines:</span></span>

* <span data-ttu-id="c8758-165">永遠不要將密碼或其他敏感性資料儲存在設定提供者程式碼或純文字設定檔中。</span><span class="sxs-lookup"><span data-stu-id="c8758-165">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="c8758-166">[秘密管理員](xref:security/app-secrets)可以用來將秘密儲存在開發中。</span><span class="sxs-lookup"><span data-stu-id="c8758-166">The [Secret manager](xref:security/app-secrets) can be used to store secrets in development.</span></span>
* <span data-ttu-id="c8758-167">不要在開發或測試環境中使用生產環境祕密。</span><span class="sxs-lookup"><span data-stu-id="c8758-167">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="c8758-168">請在專案外部指定祕密，以防止其意外認可至開放原始碼存放庫。</span><span class="sxs-lookup"><span data-stu-id="c8758-168">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="c8758-169">根據[預設](#default)，[秘密管理員](xref:security/app-secrets)會在*appsettings*之後讀取設定和 appsettings *`Environment`* *。*</span><span class="sxs-lookup"><span data-stu-id="c8758-169">By [default](#default), [Secret manager](xref:security/app-secrets) reads configuration settings after *appsettings.json* and *appsettings.*`Environment`*.json*.</span></span>

<span data-ttu-id="c8758-170">如需儲存密碼或其他機密資料的詳細資訊：</span><span class="sxs-lookup"><span data-stu-id="c8758-170">For more information on storing passwords or other sensitive data:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="c8758-171"><xref:security/app-secrets>：包含使用環境變數來儲存敏感性資料的建議。</span><span class="sxs-lookup"><span data-stu-id="c8758-171"><xref:security/app-secrets>:  Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="c8758-172">秘密管理員會使用檔案設定[提供者](#fcp)，將使用者秘密儲存在本機系統上的 JSON 檔案中。</span><span class="sxs-lookup"><span data-stu-id="c8758-172">The Secret Manager uses the [File configuration provider](#fcp) to store user secrets in a JSON file on the local system.</span></span>

<span data-ttu-id="c8758-173">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 可安全地儲存 ASP.NET Core 應用程式的應用程式祕密。</span><span class="sxs-lookup"><span data-stu-id="c8758-173">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="c8758-174">如需詳細資訊，請參閱<xref:security/key-vault-configuration>。</span><span class="sxs-lookup"><span data-stu-id="c8758-174">For more information, see <xref:security/key-vault-configuration>.</span></span>

<a name="evcp"></a>

## <a name="environment-variables"></a><span data-ttu-id="c8758-175">環境變數</span><span class="sxs-lookup"><span data-stu-id="c8758-175">Environment variables</span></span>

<span data-ttu-id="c8758-176">使用[預設](#default)設定時，<xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> 在讀取*appsettings*、`Environment`*appsettings*和[秘密管理員](xref:security/app-secrets)*之後，* 從環境變數的機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-176">Using the [default](#default) configuration, the <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs after reading *appsettings.json*, *appsettings.*`Environment`*.json*, and [Secret manager](xref:security/app-secrets).</span></span> <span data-ttu-id="c8758-177">因此，從環境中讀取的索引鍵值會覆寫從*appsettings*讀取的值、 *appsettings* *、`Environment`和*密碼管理員。</span><span class="sxs-lookup"><span data-stu-id="c8758-177">Therefore, key values read from the environment override values read from *appsettings.json*, *appsettings.*`Environment`*.json*, and Secret manager.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="c8758-178">下列 `set` 命令：</span><span class="sxs-lookup"><span data-stu-id="c8758-178">The following `set` commands:</span></span>

* <span data-ttu-id="c8758-179">在 Windows 上設定[上述範例](#appsettingsjson)的環境索引鍵和值。</span><span class="sxs-lookup"><span data-stu-id="c8758-179">Set the environment keys and values of the [preceding example](#appsettingsjson) on Windows.</span></span>
* <span data-ttu-id="c8758-180">使用[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)時測試設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-180">Test the settings when using the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample).</span></span> <span data-ttu-id="c8758-181">`dotnet run` 命令必須在專案目錄中執行。</span><span class="sxs-lookup"><span data-stu-id="c8758-181">The `dotnet run` command must be run in the project directory.</span></span>

```cmd
set MyKey="My key from Environment"
set Position__Title=Environment_Editor
set Position__Name=Environment_Rick
dotnet run
```

<span data-ttu-id="c8758-182">先前的環境設定：</span><span class="sxs-lookup"><span data-stu-id="c8758-182">The preceding environment settings:</span></span>

* <span data-ttu-id="c8758-183">只會在從其設定所在的命令視窗中啟動的進程中設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-183">Are only set in processes launched from the command window they were set in.</span></span>
* <span data-ttu-id="c8758-184">以 Visual Studio 啟動的瀏覽器將不會讀取。</span><span class="sxs-lookup"><span data-stu-id="c8758-184">Won't be read by browsers launched with Visual Studio.</span></span>

<span data-ttu-id="c8758-185">下列[setx](/windows-server/administration/windows-commands/setx)命令可以用來設定 Windows 上的環境索引鍵和值。</span><span class="sxs-lookup"><span data-stu-id="c8758-185">The following [setx](/windows-server/administration/windows-commands/setx) commands can be used to set the environment keys and values on Windows.</span></span> <span data-ttu-id="c8758-186">不同于 `set`，`setx` 設定會保存下來。</span><span class="sxs-lookup"><span data-stu-id="c8758-186">Unlike `set`, `setx` settings are persisted.</span></span> <span data-ttu-id="c8758-187">`/M` 在系統內容中設定變數。</span><span class="sxs-lookup"><span data-stu-id="c8758-187">`/M` sets the variable in the system environment.</span></span> <span data-ttu-id="c8758-188">如果未使用 `/M` 參數，則會設定使用者環境變數。</span><span class="sxs-lookup"><span data-stu-id="c8758-188">If the `/M` switch isn't used, a user environment variable is set.</span></span>

```cmd
setx MyKey "My key from setx Environment" /M
setx Position__Title Setx_Environment_Editor /M
setx Position__Name Environment_Rick /M
```

<span data-ttu-id="c8758-189">若要測試先前的命令是否會覆寫*apsettings*和*appsettings。* `Environment`*json*：</span><span class="sxs-lookup"><span data-stu-id="c8758-189">To test that the preceding commands override *apsettings.json* and *appsettings.*`Environment`*.json*:</span></span>

* <span data-ttu-id="c8758-190">使用 Visual Studio： [結束] 和 [重新開機] Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="c8758-190">With Visual Studio: Exit and restart Visual Studio.</span></span>
* <span data-ttu-id="c8758-191">使用 CLI：啟動新的命令視窗，並輸入 `dotnet run`。</span><span class="sxs-lookup"><span data-stu-id="c8758-191">With the CLI: Start a new command window and enter `dotnet run`.</span></span>

<span data-ttu-id="c8758-192">使用字串呼叫 <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*>，以指定環境變數的前置詞：</span><span class="sxs-lookup"><span data-stu-id="c8758-192">Call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> with a string to specify a prefix for environment variables:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Program.cs?name=snippet4&highlight=12)]

<span data-ttu-id="c8758-193">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="c8758-193">In the preceding code:</span></span>

* <span data-ttu-id="c8758-194">`config.AddEnvironmentVariables(prefix: "MyCustomPrefix_")` 會在預設的設定[提供者](#default)之後加入。</span><span class="sxs-lookup"><span data-stu-id="c8758-194">`config.AddEnvironmentVariables(prefix: "MyCustomPrefix_")` is added after the [default configuration providers](#default).</span></span> <span data-ttu-id="c8758-195">如需排序設定提供者的範例，請參閱[JSON 設定提供者](#jcp)。</span><span class="sxs-lookup"><span data-stu-id="c8758-195">For an example of ordering the configuration providers, see [JSON configuration provider](#jcp).</span></span>
* <span data-ttu-id="c8758-196">以 `MyCustomPrefix_` 前置詞設定的環境變數會覆寫預設的設定[提供者](#default)。</span><span class="sxs-lookup"><span data-stu-id="c8758-196">Environment variables set with the `MyCustomPrefix_` prefix override the [default configuration providers](#default).</span></span> <span data-ttu-id="c8758-197">這包括不含前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="c8758-197">This includes environment variables without the prefix.</span></span>

<span data-ttu-id="c8758-198">讀取設定機碼值組時，會去除前置詞。</span><span class="sxs-lookup"><span data-stu-id="c8758-198">The prefix is stripped off when the configuration key-value pairs are read.</span></span>

<span data-ttu-id="c8758-199">下列命令會測試自訂前置詞：</span><span class="sxs-lookup"><span data-stu-id="c8758-199">The following commands test the custom prefix:</span></span>

```cmd
set MyCustomPrefix_MyKey="My key with MyCustomPrefix_ Environment"
set MyCustomPrefix_Position__Title=Editor_with_customPrefix
set MyCustomPrefix_Position__Name=Environment_Rick_cp
dotnet run
```

<span data-ttu-id="c8758-200">[預設](#default)設定會載入前面加上 `DOTNET_` 和 `ASPNETCORE_`的環境變數和命令列引數。</span><span class="sxs-lookup"><span data-stu-id="c8758-200">The [default configuration](#default) loads environment variables and command line arguments prefixed with `DOTNET_` and `ASPNETCORE_`.</span></span> <span data-ttu-id="c8758-201">ASP.NET Core 會使用 `DOTNET_` 和 `ASPNETCORE_` 首碼來進行[主機和應用程式](xref:fundamentals/host/generic-host#host-configuration)設定，但不適用於使用者設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-201">The `DOTNET_` and `ASPNETCORE_` prefixes are used by ASP.NET Core for [host and app configuration](xref:fundamentals/host/generic-host#host-configuration), but not for user configuration.</span></span> <span data-ttu-id="c8758-202">如需主機和應用程式設定的詳細資訊，請參閱[.Net 泛型主機](xref:fundamentals/host/generic-host)。</span><span class="sxs-lookup"><span data-stu-id="c8758-202">For more information on host and app configuration, see [.NET Generic Host](xref:fundamentals/host/generic-host).</span></span>

<span data-ttu-id="c8758-203">在[Azure App Service](https://azure.microsoft.com/services/app-service/)上，選取 [**設定 > 設定**] 頁面上的 [**新增應用程式設定**]。</span><span class="sxs-lookup"><span data-stu-id="c8758-203">On [Azure App Service](https://azure.microsoft.com/services/app-service/), select **New application setting** on the **Settings > Configuration** page.</span></span> <span data-ttu-id="c8758-204">Azure App Service 的應用程式設定如下：</span><span class="sxs-lookup"><span data-stu-id="c8758-204">Azure App Service application settings are:</span></span>

* <span data-ttu-id="c8758-205">待用加密，並透過加密通道傳輸。</span><span class="sxs-lookup"><span data-stu-id="c8758-205">Encrypted at rest and transmitted over an encrypted channel.</span></span>
* <span data-ttu-id="c8758-206">公開為環境變數。</span><span class="sxs-lookup"><span data-stu-id="c8758-206">Exposed as environment variables.</span></span>

<span data-ttu-id="c8758-207">如需詳細資訊，請參閱 [Azure App：使用 Azure 入口網站覆寫應用程式設定](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)。</span><span class="sxs-lookup"><span data-stu-id="c8758-207">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="c8758-208">如需 Azure 資料庫連接字串的相關資訊，請參閱[連接字串](#constr)前置詞。</span><span class="sxs-lookup"><span data-stu-id="c8758-208">See [Connection string prefixes](#constr) for information on Azure database connection strings.</span></span>

<a name="clcp"></a>

## <a name="command-line"></a><span data-ttu-id="c8758-209">命令列</span><span class="sxs-lookup"><span data-stu-id="c8758-209">Command-line</span></span>

<span data-ttu-id="c8758-210">使用[預設](#default)設定時，<xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> 會在下列設定來源之後，從命令列引數的機碼值組載入設定：</span><span class="sxs-lookup"><span data-stu-id="c8758-210">Using the [default](#default) configuration, the <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs after the following configuration sources:</span></span>

* <span data-ttu-id="c8758-211">*appsettings. json*和*appsettings*。`Environment`。*json*檔案。</span><span class="sxs-lookup"><span data-stu-id="c8758-211">*appsettings.json* and *appsettings*.`Environment`.*json* files.</span></span>
* <span data-ttu-id="c8758-212">開發環境中的[應用程式秘密（秘密管理員）](xref:security/app-secrets) 。</span><span class="sxs-lookup"><span data-stu-id="c8758-212">[App secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="c8758-213">環境變數。</span><span class="sxs-lookup"><span data-stu-id="c8758-213">Environment variables.</span></span>

<span data-ttu-id="c8758-214">根據[預設](#default)，在命令列上設定的設定值會覆寫所有其他設定提供者所設定的設定值。</span><span class="sxs-lookup"><span data-stu-id="c8758-214">By [default](#default), configuration values set on the command-line override configuration values set with all the other configuration providers.</span></span>

### <a name="command-line-arguments"></a><span data-ttu-id="c8758-215">命令列引數</span><span class="sxs-lookup"><span data-stu-id="c8758-215">Command-line arguments</span></span>

<span data-ttu-id="c8758-216">下列命令會使用 `=`來設定索引鍵和值：</span><span class="sxs-lookup"><span data-stu-id="c8758-216">The following command sets keys and values using `=`:</span></span>

```dotnetcli
dotnet run MyKey="My key from command line" Position:Title=Cmd Position:Name=Cmd_Rick
```

<span data-ttu-id="c8758-217">下列命令會使用 `/`來設定索引鍵和值：</span><span class="sxs-lookup"><span data-stu-id="c8758-217">The following command sets keys and values using `/`:</span></span>

```dotnetcli
dotnet run /MyKey "Using /" /Position:Title=Cmd_ /Position:Name=Cmd_Rick
```

<span data-ttu-id="c8758-218">下列命令會使用 `--`來設定索引鍵和值：</span><span class="sxs-lookup"><span data-stu-id="c8758-218">The following command sets keys and values using `--`:</span></span>

```dotnetcli
dotnet run --MyKey "Using --" --Position:Title=Cmd-- --Position:Name=Cmd--Rick
```

<span data-ttu-id="c8758-219">索引鍵值：</span><span class="sxs-lookup"><span data-stu-id="c8758-219">The key value:</span></span>

* <span data-ttu-id="c8758-220">必須遵循 `=`，否則當值在空格後面時，索引鍵必須具有 `--` 或 `/` 的前置詞。</span><span class="sxs-lookup"><span data-stu-id="c8758-220">Must follow `=`, or the key must have a prefix of `--` or `/` when the value follows a space.</span></span>
* <span data-ttu-id="c8758-221">如果使用 `=`，則不需要。</span><span class="sxs-lookup"><span data-stu-id="c8758-221">Isn't required if `=` is used.</span></span> <span data-ttu-id="c8758-222">例如， `MySetting=`。</span><span class="sxs-lookup"><span data-stu-id="c8758-222">For example, `MySetting=`.</span></span>

<span data-ttu-id="c8758-223">在相同的命令中，請勿混合使用 `=` 搭配使用空格的機碼值組的命令列引數索引鍵/值組。</span><span class="sxs-lookup"><span data-stu-id="c8758-223">Within the same command, don't mix command-line argument key-value pairs that use `=` with key-value pairs that use a space.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="c8758-224">切換對應</span><span class="sxs-lookup"><span data-stu-id="c8758-224">Switch mappings</span></span>

<span data-ttu-id="c8758-225">交換器對應允許索引**鍵**名稱取代邏輯。</span><span class="sxs-lookup"><span data-stu-id="c8758-225">Switch mappings allow **key** name replacement logic.</span></span> <span data-ttu-id="c8758-226">提供 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 方法之切換取代的字典。</span><span class="sxs-lookup"><span data-stu-id="c8758-226">Provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="c8758-227">使用切換對應字典時，會檢查字典中是否有任何索引鍵符合命令列引數所提供的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="c8758-227">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="c8758-228">如果在字典中找到命令列索引鍵，則會傳回字典值，將索引鍵/值組設為應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-228">If the command-line key is found in the dictionary, the dictionary value is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="c8758-229">所有前面加上單虛線 (`-`) 的命令列索引鍵都需要切換對應。</span><span class="sxs-lookup"><span data-stu-id="c8758-229">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="c8758-230">切換對應字典索引鍵規則：</span><span class="sxs-lookup"><span data-stu-id="c8758-230">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="c8758-231">參數必須以 `-` 或 `--`開頭。</span><span class="sxs-lookup"><span data-stu-id="c8758-231">Switches must start with `-` or `--`.</span></span>
* <span data-ttu-id="c8758-232">切換對應字典不能包含重複索引鍵。</span><span class="sxs-lookup"><span data-stu-id="c8758-232">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="c8758-233">若要使用交換器對應字典，請將它傳遞至 `AddCommandLine`的呼叫：</span><span class="sxs-lookup"><span data-stu-id="c8758-233">To use a switch mappings dictionary, pass it into the call to `AddCommandLine`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramSwitch.cs?name=snippet&highlight=10-18,23)]

<span data-ttu-id="c8758-234">下列程式碼顯示已取代金鑰的索引鍵值：</span><span class="sxs-lookup"><span data-stu-id="c8758-234">The following code shows the key values for the replaced keys:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test3.cshtml.cs?name=snippet)]

<span data-ttu-id="c8758-235">執行下列命令來測試金鑰取代：</span><span class="sxs-lookup"><span data-stu-id="c8758-235">Run the following command to test the key replacement:</span></span>

```dotnetcli
dotnet run -k1=value1 -k2 value2 --alt3=value2 /alt4=value3 --alt5 value5 /alt6 value6
```

<span data-ttu-id="c8758-236">注意：目前，`=` 不能用單一破折號 `-`來設定索引鍵取代值。</span><span class="sxs-lookup"><span data-stu-id="c8758-236">Note: Currently, `=` cannot be used to set key-replacement values with a single dash `-`.</span></span> <span data-ttu-id="c8758-237">請參閱[這個 GitHub 問題](https://github.com/dotnet/extensions/issues/3059)。</span><span class="sxs-lookup"><span data-stu-id="c8758-237">See [this GitHub issue](https://github.com/dotnet/extensions/issues/3059).</span></span>

<span data-ttu-id="c8758-238">下列命令適用于測試金鑰取代：</span><span class="sxs-lookup"><span data-stu-id="c8758-238">The following command works to test key replacement:</span></span>

```dotnetcli
dotnet run -k1 value1 -k2 value2 --alt3=value2 /alt4=value3 --alt5 value5 /alt6 value6
```

<span data-ttu-id="c8758-239">針對使用切換對應的應用程式，呼叫 `CreateDefaultBuilder` 不應傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="c8758-239">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="c8758-240">`CreateDefaultBuilder` 方法的 `AddCommandLine` 呼叫不包含對應的參數，而且沒有任何方法可將參數對應字典傳遞給 `CreateDefaultBuilder`。</span><span class="sxs-lookup"><span data-stu-id="c8758-240">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch-mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="c8758-241">解決方案並不會將引數傳遞給 `CreateDefaultBuilder`，而是允許 `ConfigurationBuilder` 方法的 `AddCommandLine` 方法處理引數和切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="c8758-241">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch-mapping dictionary.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="c8758-242">階層式設定資料</span><span class="sxs-lookup"><span data-stu-id="c8758-242">Hierarchical configuration data</span></span>

<span data-ttu-id="c8758-243">設定 API 會藉由使用設定機碼中的分隔符號來簡維階層式資料，以讀取階層式設定資料。</span><span class="sxs-lookup"><span data-stu-id="c8758-243">The Configuration API is reads hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="c8758-244">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)包含下列*appsettings*檔案：</span><span class="sxs-lookup"><span data-stu-id="c8758-244">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following  *appsettings.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/appsettings.json)]

<span data-ttu-id="c8758-245">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)中的下列程式碼會顯示數個設定值：</span><span class="sxs-lookup"><span data-stu-id="c8758-245">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="c8758-246">讀取階層式設定資料的慣用方法是使用選項模式。</span><span class="sxs-lookup"><span data-stu-id="c8758-246">The preferred way to read hierarchical configuration data is using the options pattern.</span></span> <span data-ttu-id="c8758-247">如需詳細資訊，請參閱本檔中的系結階層式設定[資料](#optpat)。</span><span class="sxs-lookup"><span data-stu-id="c8758-247">For more information, see [Bind hierarchical configuration data](#optpat) in this document.</span></span>

<span data-ttu-id="c8758-248"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> 與 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> 方法可用來在設定資料中隔離區段與區段的子系。</span><span class="sxs-lookup"><span data-stu-id="c8758-248"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="c8758-249">[GetSection,、 GetChildren 與 Exists](#getsection) 中說明這些方法。</span><span class="sxs-lookup"><span data-stu-id="c8758-249">These methods are described later in [GetSection, GetChildren, and Exists](#getsection).</span></span>

<!--
[Azure Key Vault configuration provider](xref:security/key-vault-configuration) implement change detection.
-->

## <a name="configuration-keys-and-values"></a><span data-ttu-id="c8758-250">設定機碼和值</span><span class="sxs-lookup"><span data-stu-id="c8758-250">Configuration keys and values</span></span>

<span data-ttu-id="c8758-251">設定機碼：</span><span class="sxs-lookup"><span data-stu-id="c8758-251">Configuration keys:</span></span>

* <span data-ttu-id="c8758-252">不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="c8758-252">Are case-insensitive.</span></span> <span data-ttu-id="c8758-253">例如，`ConnectionString` 與 `connectionstring` 會被視為相等的機碼。</span><span class="sxs-lookup"><span data-stu-id="c8758-253">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="c8758-254">如果在多個設定提供者中設定索引鍵和值，則會使用最後新增的提供者中的值。</span><span class="sxs-lookup"><span data-stu-id="c8758-254">If a key and value is set in more than one configuration providers, the value from the last provider added is used.</span></span> <span data-ttu-id="c8758-255">如需詳細資訊，請參閱[預設](#default)設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-255">For more information, see [Default configuration](#default).</span></span>
* <span data-ttu-id="c8758-256">階層式機碼</span><span class="sxs-lookup"><span data-stu-id="c8758-256">Hierarchical keys</span></span>
  * <span data-ttu-id="c8758-257">在設定 API 內，冒號分隔字元 (`:`) 可在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="c8758-257">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="c8758-258">在環境變數中，冒號分隔字元可能無法在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="c8758-258">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="c8758-259">所有平臺都支援雙底線（`__`），而且會自動轉換成冒號 `:`。</span><span class="sxs-lookup"><span data-stu-id="c8758-259">A double underscore, `__`, is supported by all platforms and is automatically converted into a colon `:`.</span></span>
  * <span data-ttu-id="c8758-260">在 Azure Key Vault 中，階層式索引鍵會使用 `--` 作為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="c8758-260">In Azure Key Vault, hierarchical keys use `--` as a separator.</span></span> <span data-ttu-id="c8758-261">撰寫程式碼，以在將密碼載入應用程式的設定時，以 `:` 取代 `--`。</span><span class="sxs-lookup"><span data-stu-id="c8758-261">Write code to replace the `--` with a `:` when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="c8758-262"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="c8758-262">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="c8758-263">[將陣列繫結到類別](#boa)一節說明陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="c8758-263">Array binding is described in the [Bind an array to a class](#boa) section.</span></span>

<span data-ttu-id="c8758-264">設定值：</span><span class="sxs-lookup"><span data-stu-id="c8758-264">Configuration values:</span></span>

* <span data-ttu-id="c8758-265">為字串。</span><span class="sxs-lookup"><span data-stu-id="c8758-265">Are strings.</span></span>
* <span data-ttu-id="c8758-266">Null 值無法存放在設定中或繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="c8758-266">Null values can't be stored in configuration or bound to objects.</span></span>

<a name="cp"></a>

## <a name="configuration-providers"></a><span data-ttu-id="c8758-267">設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-267">Configuration providers</span></span>

<span data-ttu-id="c8758-268">下表顯示可供 ASP.NET Core 應用程式使用的設定提供者。</span><span class="sxs-lookup"><span data-stu-id="c8758-268">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="c8758-269">Provider</span><span class="sxs-lookup"><span data-stu-id="c8758-269">Provider</span></span> | <span data-ttu-id="c8758-270">從提供設定</span><span class="sxs-lookup"><span data-stu-id="c8758-270">Provides configuration from</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="c8758-271">Azure Key Vault 組態提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-271">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration) | <span data-ttu-id="c8758-272">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="c8758-272">Azure Key Vault</span></span> |
| [<span data-ttu-id="c8758-273">Azure App 設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-273">Azure App configuration provider</span></span>](/azure/azure-app-configuration/quickstart-aspnet-core-app) | <span data-ttu-id="c8758-274">Azure 應用程式組態</span><span class="sxs-lookup"><span data-stu-id="c8758-274">Azure App Configuration</span></span> |
| [<span data-ttu-id="c8758-275">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-275">Command-line configuration provider</span></span>](#clcp) | <span data-ttu-id="c8758-276">命令列參數</span><span class="sxs-lookup"><span data-stu-id="c8758-276">Command-line parameters</span></span> |
| [<span data-ttu-id="c8758-277">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-277">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="c8758-278">自訂來源</span><span class="sxs-lookup"><span data-stu-id="c8758-278">Custom source</span></span> |
| [<span data-ttu-id="c8758-279">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-279">Environment Variables configuration provider</span></span>](#evcp) | <span data-ttu-id="c8758-280">環境變數</span><span class="sxs-lookup"><span data-stu-id="c8758-280">Environment variables</span></span> |
| [<span data-ttu-id="c8758-281">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-281">File configuration provider</span></span>](#file-configuration-provider) | <span data-ttu-id="c8758-282">INI、JSON 和 XML 檔案</span><span class="sxs-lookup"><span data-stu-id="c8758-282">INI, JSON, and XML files</span></span> |
| [<span data-ttu-id="c8758-283">每個檔案的索引鍵設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-283">Key-per-file configuration provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="c8758-284">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="c8758-284">Directory files</span></span> |
| [<span data-ttu-id="c8758-285">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-285">Memory configuration provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="c8758-286">記憶體內集合</span><span class="sxs-lookup"><span data-stu-id="c8758-286">In-memory collections</span></span> |
| [<span data-ttu-id="c8758-287">秘密管理員</span><span class="sxs-lookup"><span data-stu-id="c8758-287">Secret Manager</span></span>](xref:security/app-secrets)  | <span data-ttu-id="c8758-288">使用者設定檔目錄中的檔案</span><span class="sxs-lookup"><span data-stu-id="c8758-288">File in the user profile directory</span></span> |

<span data-ttu-id="c8758-289">設定來源會依照其設定提供者的指定順序讀取。</span><span class="sxs-lookup"><span data-stu-id="c8758-289">Configuration sources are read in the order that their configuration providers are specified.</span></span> <span data-ttu-id="c8758-290">請在程式碼中訂購設定提供者，以符合應用程式所需之基礎設定來源的優先順序。</span><span class="sxs-lookup"><span data-stu-id="c8758-290">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="c8758-291">典型的設定提供者順序是：</span><span class="sxs-lookup"><span data-stu-id="c8758-291">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="c8758-292">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="c8758-292">*appsettings.json*</span></span>
1. <span data-ttu-id="c8758-293">*appsettings*。`Environment`。*json*</span><span class="sxs-lookup"><span data-stu-id="c8758-293">*appsettings*.`Environment`.*json*</span></span>
1. [<span data-ttu-id="c8758-294">秘密管理員</span><span class="sxs-lookup"><span data-stu-id="c8758-294">Secret Manager</span></span>](xref:security/app-secrets)
1. <span data-ttu-id="c8758-295">使用[環境變數設定提供者](#evcp)的環境變數。</span><span class="sxs-lookup"><span data-stu-id="c8758-295">Environment variables using the [Environment Variables configuration provider](#evcp).</span></span>
1. <span data-ttu-id="c8758-296">使用[命令列設定提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="c8758-296">Command-line arguments using the [Command-line configuration provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="c8758-297">常見的做法是在一系列提供者中新增命令列設定提供者，以允許命令列引數覆寫其他提供者所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-297">A common practice is to add the Command-line configuration provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="c8758-298">先前的提供者序列會用於[預設](#default)設定中。</span><span class="sxs-lookup"><span data-stu-id="c8758-298">The preceding sequence of providers is used in the [default configuration](#default).</span></span>

<a name="constr"></a>

### <a name="connection-string-prefixes"></a><span data-ttu-id="c8758-299">連接字串前置詞</span><span class="sxs-lookup"><span data-stu-id="c8758-299">Connection string prefixes</span></span>

<span data-ttu-id="c8758-300">設定 API 具有四個連接字串環境變數的特殊處理規則。</span><span class="sxs-lookup"><span data-stu-id="c8758-300">The Configuration API has special processing rules for four connection string environment variables.</span></span> <span data-ttu-id="c8758-301">這些連接字串牽涉到設定應用程式環境的 Azure 連接字串。</span><span class="sxs-lookup"><span data-stu-id="c8758-301">These connection strings are involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="c8758-302">具有資料表中所顯示前置詞的環境變數，會載入至具有[預設](#default)設定的應用程式，或在沒有提供首碼給 `AddEnvironmentVariables`時使用。</span><span class="sxs-lookup"><span data-stu-id="c8758-302">Environment variables with the prefixes shown in the table are loaded into the app with the [default configuration](#default) or when no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="c8758-303">連接字串前置詞</span><span class="sxs-lookup"><span data-stu-id="c8758-303">Connection string prefix</span></span> | <span data-ttu-id="c8758-304">Provider</span><span class="sxs-lookup"><span data-stu-id="c8758-304">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="c8758-305">自訂提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-305">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="c8758-306">MySQL</span><span class="sxs-lookup"><span data-stu-id="c8758-306">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="c8758-307">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="c8758-307">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="c8758-308">SQL Server</span><span class="sxs-lookup"><span data-stu-id="c8758-308">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="c8758-309">當探索到具有下表所顯示之任何四個前置詞的環境變數並將其載入到設定中時：</span><span class="sxs-lookup"><span data-stu-id="c8758-309">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="c8758-310">會透過移除環境變數前置詞並新增設定機碼區段 (`ConnectionStrings`) 來移除具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="c8758-310">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="c8758-311">會建立新的設定機碼值組以代表資料庫連線提供者 (`CUSTOMCONNSTR_` 除外，這沒有所述提供者)。</span><span class="sxs-lookup"><span data-stu-id="c8758-311">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="c8758-312">環境變數機碼</span><span class="sxs-lookup"><span data-stu-id="c8758-312">Environment variable key</span></span> | <span data-ttu-id="c8758-313">已轉換的設定機碼</span><span class="sxs-lookup"><span data-stu-id="c8758-313">Converted configuration key</span></span> | <span data-ttu-id="c8758-314">提供者設定項目</span><span class="sxs-lookup"><span data-stu-id="c8758-314">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="c8758-315">設定項目未建立。</span><span class="sxs-lookup"><span data-stu-id="c8758-315">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="c8758-316">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="c8758-316">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="c8758-317">值：`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="c8758-317">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="c8758-318">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="c8758-318">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="c8758-319">值：`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="c8758-319">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="c8758-320">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="c8758-320">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="c8758-321">值：`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="c8758-321">Value: `System.Data.SqlClient`</span></span>  |

<a name="jcp"></a>

### <a name="json-configuration-provider"></a><span data-ttu-id="c8758-322">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-322">JSON configuration provider</span></span>

<span data-ttu-id="c8758-323"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> 會從 JSON 檔案索引鍵/值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-323">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs.</span></span>

<span data-ttu-id="c8758-324">多載可以指定：</span><span class="sxs-lookup"><span data-stu-id="c8758-324">Overloads can specify:</span></span>

* <span data-ttu-id="c8758-325">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="c8758-325">Whether the file is optional.</span></span>
* <span data-ttu-id="c8758-326">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-326">Whether the configuration is reloaded if the file changes.</span></span>

<span data-ttu-id="c8758-327">請考慮下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="c8758-327">Consider the following code:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSON.cs?name=snippet&highlight=12-14)]

<span data-ttu-id="c8758-328">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="c8758-328">The preceding code:</span></span>

* <span data-ttu-id="c8758-329">設定 JSON 設定提供者以使用下列選項載入*myconfig.xml* ：</span><span class="sxs-lookup"><span data-stu-id="c8758-329">Configures the JSON configuration provider to load the *MyConfig.json* file with the following options:</span></span>
  * <span data-ttu-id="c8758-330">`optional: true`：檔案是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="c8758-330">`optional: true`: The file is optional.</span></span>
  * <span data-ttu-id="c8758-331">`reloadOnChange: true`：儲存變更時，會重載檔案。</span><span class="sxs-lookup"><span data-stu-id="c8758-331">`reloadOnChange: true` : The file is reloaded when changes are saved.</span></span>
* <span data-ttu-id="c8758-332">在*myconfig.xml json*檔案之前讀取預設的設定[提供者](#default)。</span><span class="sxs-lookup"><span data-stu-id="c8758-332">Reads the [default configuration providers](#default) before the *MyConfig.json* file.</span></span> <span data-ttu-id="c8758-333">預設設定提供者（包括[環境變數設定提供者](#evcp)和[命令列設定提供者](#clcp)）中的*myconfig.xml*檔案覆寫設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-333">Settings in the *MyConfig.json* file override setting in the default configuration providers, including the [Environment variables configuration provider](#evcp) and the [Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="c8758-334">您通常***不***會想要覆寫[環境變數設定提供者](#evcp)和[命令列設定提供者](#clcp)中所設定之值的自訂 JSON 檔案。</span><span class="sxs-lookup"><span data-stu-id="c8758-334">You typically ***don't*** want a custom JSON file overriding values set in the [Environment variables configuration provider](#evcp) and the [Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="c8758-335">下列程式碼會清除所有設定提供者，並新增數個設定提供者：</span><span class="sxs-lookup"><span data-stu-id="c8758-335">The following code clears all the configuration providers and adds several configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSON2.cs?name=snippet)]

<span data-ttu-id="c8758-336">在上述程式碼中， *myconfig.xml*和*myconfig.xml*中的設定。`Environment`。*json*檔案：</span><span class="sxs-lookup"><span data-stu-id="c8758-336">In the preceding code, settings in the *MyConfig.json* and  *MyConfig*.`Environment`.*json* files:</span></span>

* <span data-ttu-id="c8758-337">覆寫*appsettings*和*appsettings*中的設定。`Environment`。*json*檔案。</span><span class="sxs-lookup"><span data-stu-id="c8758-337">Override settings in the *appsettings.json* and *appsettings*.`Environment`.*json* files.</span></span>
* <span data-ttu-id="c8758-338">會由[環境變數設定提供者](#evcp)和[命令列設定提供者](#clcp)中的設定覆寫。</span><span class="sxs-lookup"><span data-stu-id="c8758-338">Are overridden by settings in the [Environment variables configuration provider](#evcp) and the [Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="c8758-339">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)包含下列*myconfig.xml*檔案：</span><span class="sxs-lookup"><span data-stu-id="c8758-339">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following  *MyConfig.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/MyConfig.json)]

<span data-ttu-id="c8758-340">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)中的下列程式碼會顯示上述幾個設定值：</span><span class="sxs-lookup"><span data-stu-id="c8758-340">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<a name="fcp"></a>

## <a name="file-configuration-provider"></a><span data-ttu-id="c8758-341">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-341">File configuration provider</span></span>

<span data-ttu-id="c8758-342"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> 是用於從檔案系統載入設定的基底類別。</span><span class="sxs-lookup"><span data-stu-id="c8758-342"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="c8758-343">下列設定提供者衍生自 `FileConfigurationProvider`：</span><span class="sxs-lookup"><span data-stu-id="c8758-343">The following configuration providers derive from `FileConfigurationProvider`:</span></span>

* [<span data-ttu-id="c8758-344">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-344">INI configuration provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="c8758-345">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-345">JSON configuration provider</span></span>](#jcp)
* [<span data-ttu-id="c8758-346">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-346">XML configuration provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="c8758-347">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-347">INI configuration provider</span></span>

<span data-ttu-id="c8758-348"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> 會在執行階段從 INI 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-348">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="c8758-349">下列程式碼會清除所有設定提供者，並新增數個設定提供者：</span><span class="sxs-lookup"><span data-stu-id="c8758-349">The following code clears all the configuration providers and adds several configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramINI.cs?name=snippet&highlight=10-30)]

<span data-ttu-id="c8758-350">在上述程式碼中， *MyIniConfig*和*MyIniConfig*中的設定。`Environment`。*ini*檔案會由中的設定覆寫：</span><span class="sxs-lookup"><span data-stu-id="c8758-350">In the preceding code, settings in the *MyIniConfig.ini* and  *MyIniConfig*.`Environment`.*ini* files are overridden by settings in the:</span></span>

* [<span data-ttu-id="c8758-351">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-351">Environment variables configuration provider</span></span>](#evcp)
* <span data-ttu-id="c8758-352">[命令列設定提供者](#clcp)。</span><span class="sxs-lookup"><span data-stu-id="c8758-352">[Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="c8758-353">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)包含下列*MyIniConfig .ini*檔案：</span><span class="sxs-lookup"><span data-stu-id="c8758-353">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following *MyIniConfig.ini* file:</span></span>

[!code-ini[](index/samples/3.x/ConfigSample/MyIniConfig.ini)]

<span data-ttu-id="c8758-354">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)中的下列程式碼會顯示上述幾個設定值：</span><span class="sxs-lookup"><span data-stu-id="c8758-354">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

### <a name="xml-configuration-provider"></a><span data-ttu-id="c8758-355">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-355">XML configuration provider</span></span>

<span data-ttu-id="c8758-356"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> 會在執行階段從 XML 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-356">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="c8758-357">下列程式碼會清除所有設定提供者，並新增數個設定提供者：</span><span class="sxs-lookup"><span data-stu-id="c8758-357">The following code clears all the configuration providers and adds several configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramXML.cs?name=snippet)]

<span data-ttu-id="c8758-358">在上述程式碼中， *MyXMLFile*和*MyXMLFile*中的設定。`Environment`。中的設定會覆寫*xml*檔案：</span><span class="sxs-lookup"><span data-stu-id="c8758-358">In the preceding code, settings in the *MyXMLFile.xml* and  *MyXMLFile*.`Environment`.*xml* files are overridden by settings in the:</span></span>

* [<span data-ttu-id="c8758-359">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-359">Environment variables configuration provider</span></span>](#evcp)
* <span data-ttu-id="c8758-360">[命令列設定提供者](#clcp)。</span><span class="sxs-lookup"><span data-stu-id="c8758-360">[Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="c8758-361">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)包含下列*MyXMLFile*檔案：</span><span class="sxs-lookup"><span data-stu-id="c8758-361">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following *MyXMLFile.xml* file:</span></span>

[!code-xml[](index/samples/3.x/ConfigSample/MyXMLFile.xml)]

<span data-ttu-id="c8758-362">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)中的下列程式碼會顯示上述幾個設定值：</span><span class="sxs-lookup"><span data-stu-id="c8758-362">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="c8758-363">若 `name` 屬性是用來區別元素，則可以重複那些使用相同元素名稱的元素：</span><span class="sxs-lookup"><span data-stu-id="c8758-363">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

[!code-xml[](index/samples/3.x/ConfigSample/MyXMLFile3.xml)]

<span data-ttu-id="c8758-364">下列程式碼會讀取先前的設定檔，並顯示金鑰和值：</span><span class="sxs-lookup"><span data-stu-id="c8758-364">The following code reads the previous configuration file and displays the keys and values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/XML/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="c8758-365">屬性可用來提供值：</span><span class="sxs-lookup"><span data-stu-id="c8758-365">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="c8758-366">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="c8758-366">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="c8758-367">key:attribute</span><span class="sxs-lookup"><span data-stu-id="c8758-367">key:attribute</span></span>
* <span data-ttu-id="c8758-368">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="c8758-368">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="c8758-369">每個檔案的索引鍵設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-369">Key-per-file configuration provider</span></span>

<span data-ttu-id="c8758-370"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> 使用目錄的檔案做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="c8758-370">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="c8758-371">機碼是檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="c8758-371">The key is the file name.</span></span> <span data-ttu-id="c8758-372">值包含檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="c8758-372">The value contains the file's contents.</span></span> <span data-ttu-id="c8758-373">Docker 裝載案例中會使用每個檔案的索引鍵設定提供者。</span><span class="sxs-lookup"><span data-stu-id="c8758-373">The Key-per-file configuration provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="c8758-374">若要啟用每個檔案機碼設定，請在 <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="c8758-374">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="c8758-375">檔案的 `directoryPath` 必須是絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="c8758-375">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="c8758-376">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="c8758-376">Overloads permit specifying:</span></span>

* <span data-ttu-id="c8758-377">設定來源的 `Action<KeyPerFileConfigurationSource>` 委派。</span><span class="sxs-lookup"><span data-stu-id="c8758-377">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="c8758-378">目錄是否為選擇性與目錄的路徑。</span><span class="sxs-lookup"><span data-stu-id="c8758-378">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="c8758-379">雙底線 (`__`) 是做為檔案名稱中的設定金鑰分隔符號使用。</span><span class="sxs-lookup"><span data-stu-id="c8758-379">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="c8758-380">例如，檔案名稱 `Logging__LogLevel__System` 會產生設定金鑰 `Logging:LogLevel:System`。</span><span class="sxs-lookup"><span data-stu-id="c8758-380">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="c8758-381">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="c8758-381">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

<a name="mcp"></a>

## <a name="memory-configuration-provider"></a><span data-ttu-id="c8758-382">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-382">Memory configuration provider</span></span>

<span data-ttu-id="c8758-383"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> 使用記憶體內集合做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="c8758-383">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="c8758-384">下列程式碼會將記憶體集合新增至設定系統：</span><span class="sxs-lookup"><span data-stu-id="c8758-384">The following code adds a memory collection to the configuration system:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet6)]

<span data-ttu-id="c8758-385">下列來自[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)的程式碼會顯示先前的設定值：</span><span class="sxs-lookup"><span data-stu-id="c8758-385">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="c8758-386">在上述程式碼中，`config.AddInMemoryCollection(Dict)` 會在預設的設定[提供者](#default)之後加入。</span><span class="sxs-lookup"><span data-stu-id="c8758-386">In the preceding code, `config.AddInMemoryCollection(Dict)` is added after the [default configuration providers](#default).</span></span> <span data-ttu-id="c8758-387">如需排序設定提供者的範例，請參閱[JSON 設定提供者](#jcp)。</span><span class="sxs-lookup"><span data-stu-id="c8758-387">For an example of ordering the configuration providers, see [JSON configuration provider](#jcp).</span></span>

<span data-ttu-id="c8758-388">如需排序設定提供者的範例，請參閱[JSON 設定提供者](#jcp)。</span><span class="sxs-lookup"><span data-stu-id="c8758-388">For an example of ordering the configuration providers, see [JSON configuration provider](#jcp).</span></span>

<span data-ttu-id="c8758-389">如需使用 `MemoryConfigurationProvider`的另一個範例，請參閱系結[陣列](#boa)。</span><span class="sxs-lookup"><span data-stu-id="c8758-389">See [Bind an array](#boa) for another example using `MemoryConfigurationProvider`.</span></span>

## <a name="getvalue"></a><span data-ttu-id="c8758-390">GetValue</span><span class="sxs-lookup"><span data-stu-id="c8758-390">GetValue</span></span>

<span data-ttu-id="c8758-391">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*)會使用指定的索引鍵來解壓縮設定中的單一值，並將其轉換為指定的類型：</span><span class="sxs-lookup"><span data-stu-id="c8758-391">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified type:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestNum.cshtml.cs?name=snippet)]

<span data-ttu-id="c8758-392">在上述程式碼中，如果在設定中找不到 `NumberKey`，則會使用 `99` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="c8758-392">In the preceding code,  if `NumberKey` isn't found in the configuration, the default value of `99` is used.</span></span>

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="c8758-393">GetSection、GetChildren 與 Exists</span><span class="sxs-lookup"><span data-stu-id="c8758-393">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="c8758-394">針對接下來的範例，請考慮下列*MySubsection*檔：</span><span class="sxs-lookup"><span data-stu-id="c8758-394">For the examples that follow, consider the following *MySubsection.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/MySubsection.json)]

<span data-ttu-id="c8758-395">下列程式碼會將*MySubsection*新增至設定提供者：</span><span class="sxs-lookup"><span data-stu-id="c8758-395">The following code adds *MySubsection.json* to the configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSONsection.cs?name=snippet)]

### <a name="getsection"></a><span data-ttu-id="c8758-396">GetSection</span><span class="sxs-lookup"><span data-stu-id="c8758-396">GetSection</span></span>

<span data-ttu-id="c8758-397">[IConfiguration。 GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*)會傳回具有指定子區段索引鍵的設定子區段。</span><span class="sxs-lookup"><span data-stu-id="c8758-397">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) returns a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="c8758-398">下列程式碼會傳回 `section1`的值：</span><span class="sxs-lookup"><span data-stu-id="c8758-398">The following code returns values for `section1`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection.cshtml.cs?name=snippet)]

<span data-ttu-id="c8758-399">下列程式碼會傳回 `section2:subsection0`的值：</span><span class="sxs-lookup"><span data-stu-id="c8758-399">The following code returns values for `section2:subsection0`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection2.cshtml.cs?name=snippet)]

<span data-ttu-id="c8758-400">`GetSection` 絕不會傳回 `null`。</span><span class="sxs-lookup"><span data-stu-id="c8758-400">`GetSection` never returns `null`.</span></span> <span data-ttu-id="c8758-401">若找不到相符的區段，會傳回空的 `IConfigurationSection`。</span><span class="sxs-lookup"><span data-stu-id="c8758-401">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="c8758-402">當 `GetSection` 傳回相符區段時，未填入 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value>。</span><span class="sxs-lookup"><span data-stu-id="c8758-402">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="c8758-403">當區段存在時，會傳回 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> 與 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path>。</span><span class="sxs-lookup"><span data-stu-id="c8758-403">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren-and-exists"></a><span data-ttu-id="c8758-404">GetChildren 和 Exists</span><span class="sxs-lookup"><span data-stu-id="c8758-404">GetChildren and Exists</span></span>

<span data-ttu-id="c8758-405">下列程式碼會呼叫[IConfiguration. GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*)並傳回 `section2:subsection0`的值：</span><span class="sxs-lookup"><span data-stu-id="c8758-405">The following code calls [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) and returns values for `section2:subsection0`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection4.cshtml.cs?name=snippet)]

<span data-ttu-id="c8758-406">上述程式碼會呼叫[microsoft.extensions.options.configurationextensions](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) ，以確認區段是否存在：</span><span class="sxs-lookup"><span data-stu-id="c8758-406">The preceding code calls [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to verify the  section exists:</span></span>

 <a name="boa"></a>

## <a name="bind-an-array"></a><span data-ttu-id="c8758-407">系結陣列</span><span class="sxs-lookup"><span data-stu-id="c8758-407">Bind an array</span></span>

<span data-ttu-id="c8758-408">[ConfigurationBinder](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*)支援在設定機碼中使用陣列索引將陣列系結至物件。</span><span class="sxs-lookup"><span data-stu-id="c8758-408">The [ConfigurationBinder.Bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*) supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="c8758-409">任何會公開數值索引鍵的陣列格式，都可以系結至[POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object)類別陣列。</span><span class="sxs-lookup"><span data-stu-id="c8758-409">Any array format that exposes a numeric key segment is capable of array binding to a [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) class array.</span></span>

<span data-ttu-id="c8758-410">請考慮[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)中的*MyArray* ：</span><span class="sxs-lookup"><span data-stu-id="c8758-410">Consider *MyArray.json* from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample):</span></span>

[!code-json[](index/samples/3.x/ConfigSample/MyArray.json)]

<span data-ttu-id="c8758-411">下列程式碼會將*MyArray*新增至設定提供者：</span><span class="sxs-lookup"><span data-stu-id="c8758-411">The following code adds *MyArray.json* to the configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSONarray.cs?name=snippet)]

<span data-ttu-id="c8758-412">下列程式碼會讀取設定並顯示值：</span><span class="sxs-lookup"><span data-stu-id="c8758-412">The following code reads the configuration and displays the values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

<span data-ttu-id="c8758-413">上述程式碼會傳回下列輸出：</span><span class="sxs-lookup"><span data-stu-id="c8758-413">The preceding code returns the following output:</span></span>

```text
Index: 0  Value: value00
Index: 1  Value: value10
Index: 2  Value: value20
Index: 3  Value: value40
Index: 4  Value: value50
```

<span data-ttu-id="c8758-414">在上述輸出中，索引3具有與*MyArray*中 `"4": "value40",` 對應的值 `value40`。</span><span class="sxs-lookup"><span data-stu-id="c8758-414">In the preceding output, Index 3 has value `value40`, corresponding to `"4": "value40",` in *MyArray.json*.</span></span> <span data-ttu-id="c8758-415">系結的陣列索引是連續的，而且不會系結至設定金鑰索引。</span><span class="sxs-lookup"><span data-stu-id="c8758-415">The bound array indices are continuous and not bound to the configuration key index.</span></span> <span data-ttu-id="c8758-416">設定系結器無法系結 null 值，或在系結物件中建立 null 專案</span><span class="sxs-lookup"><span data-stu-id="c8758-416">The configuration binder isn't capable of binding null values or creating null entries in bound objects</span></span>

<span data-ttu-id="c8758-417">下列程式碼會使用 <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> 擴充方法來載入 `array:entries` 設定：</span><span class="sxs-lookup"><span data-stu-id="c8758-417">The  following code loads the `array:entries` configuration with the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet)]

<span data-ttu-id="c8758-418">下列程式碼會讀取 `arrayDict` `Dictionary` 中的設定，並顯示這些值：</span><span class="sxs-lookup"><span data-stu-id="c8758-418">The following code reads the configuration in the `arrayDict` `Dictionary` and displays the values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

<span data-ttu-id="c8758-419">上述程式碼會傳回下列輸出：</span><span class="sxs-lookup"><span data-stu-id="c8758-419">The preceding code returns the following output:</span></span>

```text
Index: 0  Value: value0
Index: 1  Value: value1
Index: 2  Value: value2
Index: 3  Value: value4
Index: 4  Value: value5
```

<span data-ttu-id="c8758-420">繫結物件中的索引 &num;3 存放 `array:4` 設定機碼與其 `value4` 的設定資料。</span><span class="sxs-lookup"><span data-stu-id="c8758-420">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="c8758-421">當系結包含陣列的設定資料時，在建立物件時，會使用設定機碼中的陣列索引來反復查看設定資料。</span><span class="sxs-lookup"><span data-stu-id="c8758-421">When configuration data containing an array is bound, the array indices in the configuration keys are used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="c8758-422">設定資料中不能保留 Null 值，當設定機碼中的陣列略過一或多個索引時，不會在繫結物件中建立 Null 值項目。</span><span class="sxs-lookup"><span data-stu-id="c8758-422">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="c8758-423">在系結至 `ArrayExample` 實例之前，可以先提供索引 &num;3 的遺漏設定專案，該設定提供者會讀取索引 &num;3 機碼/值組。</span><span class="sxs-lookup"><span data-stu-id="c8758-423">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that reads the index &num;3 key/value pair.</span></span> <span data-ttu-id="c8758-424">請考慮下列來自範例下載的*Value3 json*檔案：</span><span class="sxs-lookup"><span data-stu-id="c8758-424">Consider the following *Value3.json* file from the sample download:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/Value3.json)]

<span data-ttu-id="c8758-425">下列程式碼包含*Value3*的設定，以及 `Dictionary`的 `arrayDict`：</span><span class="sxs-lookup"><span data-stu-id="c8758-425">The following code includes configuration for *Value3.json* and the `arrayDict` `Dictionary`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet2)]

<span data-ttu-id="c8758-426">下列程式碼會讀取先前的設定，並顯示值：</span><span class="sxs-lookup"><span data-stu-id="c8758-426">The following code reads the preceding configuration and displays the values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

<span data-ttu-id="c8758-427">上述程式碼會傳回下列輸出：</span><span class="sxs-lookup"><span data-stu-id="c8758-427">The preceding code returns the following output:</span></span>

```text
Index: 0  Value: value0
Index: 1  Value: value1
Index: 2  Value: value2
Index: 3  Value: value3
Index: 4  Value: value4
Index: 5  Value: value5
```

<span data-ttu-id="c8758-428">自訂設定提供者不需要實作陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="c8758-428">Custom configuration providers aren't required to implement array binding.</span></span>

## <a name="custom-configuration-provider"></a><span data-ttu-id="c8758-429">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-429">Custom configuration provider</span></span>

<span data-ttu-id="c8758-430">範例應用程式示範如何建立會使用 [Entity Framework (EF)](/ef/core/) 從資料庫讀取設定機碼值組的基本設定提供者。</span><span class="sxs-lookup"><span data-stu-id="c8758-430">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="c8758-431">提供者具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="c8758-431">The provider has the following characteristics:</span></span>

* <span data-ttu-id="c8758-432">EF 記憶體內資料庫會用於示範用途。</span><span class="sxs-lookup"><span data-stu-id="c8758-432">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="c8758-433">若要使用需要連接字串的資料庫，請實作第二個 `ConfigurationBuilder` 以從另一個設定提供者提供連接字串。</span><span class="sxs-lookup"><span data-stu-id="c8758-433">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="c8758-434">啟動時，該提供者會將資料庫資料表讀入到設定中。</span><span class="sxs-lookup"><span data-stu-id="c8758-434">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="c8758-435">該提供者不會以個別機碼為基礎查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="c8758-435">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="c8758-436">未實作變更時重新載入，因此在應用程式啟動後更新資料庫對應用程式設定沒有影響。</span><span class="sxs-lookup"><span data-stu-id="c8758-436">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="c8758-437">定義 `EFConfigurationValue` 實體來在資料庫中存放設定值。</span><span class="sxs-lookup"><span data-stu-id="c8758-437">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="c8758-438">*Models/EFConfigurationValue.cs*：</span><span class="sxs-lookup"><span data-stu-id="c8758-438">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="c8758-439">新增 `EFConfigurationContext` 以存放及存取已設定的值。</span><span class="sxs-lookup"><span data-stu-id="c8758-439">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="c8758-440">*EFConfigurationProvider/EFConfigurationContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="c8758-440">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="c8758-441">建立會實作 <xref:Microsoft.Extensions.Configuration.IConfigurationSource> 的類別。</span><span class="sxs-lookup"><span data-stu-id="c8758-441">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="c8758-442">*EFConfigurationProvider/EFConfigurationSource.cs*：</span><span class="sxs-lookup"><span data-stu-id="c8758-442">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="c8758-443">透過繼承自 <xref:Microsoft.Extensions.Configuration.ConfigurationProvider> 來建立自訂設定提供者。</span><span class="sxs-lookup"><span data-stu-id="c8758-443">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="c8758-444">若資料庫是空的，設定提供者會初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="c8758-444">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="c8758-445">由於設定索引[鍵不區分大小寫](#keys)，用來初始化資料庫的字典會以不區分大小寫的比較子（[StringComparer. OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)）來建立。</span><span class="sxs-lookup"><span data-stu-id="c8758-445">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="c8758-446">*EFConfigurationProvider/EFConfigurationProvider.cs*：</span><span class="sxs-lookup"><span data-stu-id="c8758-446">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="c8758-447">`AddEFConfiguration` 擴充方法允許新增設定來源到 `ConfigurationBuilder`。</span><span class="sxs-lookup"><span data-stu-id="c8758-447">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="c8758-448">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="c8758-448">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="c8758-449">下列程式碼顯示如何在 `EFConfigurationProvider`Program.cs*中使用自訂*：</span><span class="sxs-lookup"><span data-stu-id="c8758-449">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

<a name="acs"></a>

## <a name="access-configuration-in-startup"></a><span data-ttu-id="c8758-450">啟動時的存取設定</span><span class="sxs-lookup"><span data-stu-id="c8758-450">Access configuration in Startup</span></span>

<span data-ttu-id="c8758-451">下列程式碼會顯示 `Startup` 方法中的設定資料：</span><span class="sxs-lookup"><span data-stu-id="c8758-451">The following code displays configuration data in `Startup` methods:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/StartupKey.cs?name=snippet&highlight=13,18)]

<span data-ttu-id="c8758-452">如需使用啟動方便方法來存取設定的範例，請參閱[應用程式啟動：方便方法](xref:fundamentals/startup#convenience-methods)。</span><span class="sxs-lookup"><span data-stu-id="c8758-452">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-razor-pages"></a><span data-ttu-id="c8758-453">Razor Pages 中的存取設定</span><span class="sxs-lookup"><span data-stu-id="c8758-453">Access configuration in Razor Pages</span></span>

<span data-ttu-id="c8758-454">下列程式碼會顯示 Razor 頁面中的設定資料：</span><span class="sxs-lookup"><span data-stu-id="c8758-454">The following code displays configuration data in a Razor Page:</span></span>

[!code-cshtml[](index/samples/3.x/ConfigSample/Pages/Test5.cshtml)]

## <a name="access-configuration-in-a-mvc-view-file"></a><span data-ttu-id="c8758-455">MVC 視圖檔案中的存取設定</span><span class="sxs-lookup"><span data-stu-id="c8758-455">Access configuration in a MVC view file</span></span>

<span data-ttu-id="c8758-456">下列程式碼會在 MVC 視圖中顯示設定資料：</span><span class="sxs-lookup"><span data-stu-id="c8758-456">The following code displays configuration data in a MVC view:</span></span>

[!code-cshtml[](index/samples/3.x/ConfigSample/Views/Home2/Index.cshtml)]

<a name="hvac"></a>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="c8758-457">主機與應用程式組態的比較</span><span class="sxs-lookup"><span data-stu-id="c8758-457">Host versus app configuration</span></span>

<span data-ttu-id="c8758-458">設定及啟動應用程式之前，會先設定及啟動「主機」。</span><span class="sxs-lookup"><span data-stu-id="c8758-458">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="c8758-459">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="c8758-459">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="c8758-460">應用程式與主機都是使用此主題中所述的設定提供者來設定的。</span><span class="sxs-lookup"><span data-stu-id="c8758-460">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="c8758-461">主機組態機碼/值組也會包含在應用程式的組態中。</span><span class="sxs-lookup"><span data-stu-id="c8758-461">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="c8758-462">如需有關當建置主機時如何使用設定提供者的詳細資訊，以及設定來源如何影響主機設定的詳細資訊，請參閱 <xref:fundamentals/index#host>。</span><span class="sxs-lookup"><span data-stu-id="c8758-462">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

<a name="dhc"></a>

## <a name="default-host-configuration"></a><span data-ttu-id="c8758-463">預設主機設定</span><span class="sxs-lookup"><span data-stu-id="c8758-463">Default host configuration</span></span>

<span data-ttu-id="c8758-464">如需使用 [Web 主機](xref:fundamentals/host/web-host)時預設組態的詳細資料，請參閱[本主題的 ASP.NET Core 2.2 版本](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2)。</span><span class="sxs-lookup"><span data-stu-id="c8758-464">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="c8758-465">主機組態的提供來源：</span><span class="sxs-lookup"><span data-stu-id="c8758-465">Host configuration is provided from:</span></span>
  * <span data-ttu-id="c8758-466">前面加上 `DOTNET_` 的環境變數（例如，`DOTNET_ENVIRONMENT`）使用[環境變數設定提供者](#environment-variables-configuration-provider)。</span><span class="sxs-lookup"><span data-stu-id="c8758-466">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables configuration provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="c8758-467">載入設定機碼值組時，會移除前置詞 (`DOTNET_`)。</span><span class="sxs-lookup"><span data-stu-id="c8758-467">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="c8758-468">使用[命令列設定提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="c8758-468">Command-line arguments using the [Command-line configuration provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="c8758-469">已建立 Web 主機預設組態 (`ConfigureWebHostDefaults`)：</span><span class="sxs-lookup"><span data-stu-id="c8758-469">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="c8758-470">Kestrel 會用作為網頁伺服器，並使用應用程式的組態提供者來設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-470">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="c8758-471">新增主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="c8758-471">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="c8758-472">如果 `ASPNETCORE_FORWARDEDHEADERS_ENABLED` 環境變數設定為 `true`，則會新增轉接的標頭中介軟體。</span><span class="sxs-lookup"><span data-stu-id="c8758-472">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="c8758-473">啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="c8758-473">Enable IIS integration.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="c8758-474">其他設定</span><span class="sxs-lookup"><span data-stu-id="c8758-474">Other configuration</span></span>

<span data-ttu-id="c8758-475">本主題僅適用于*應用程式*設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-475">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="c8758-476">執行和裝載 ASP.NET Core 應用程式的其他層面，是使用本主題未涵蓋的設定檔來設定：</span><span class="sxs-lookup"><span data-stu-id="c8758-476">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="c8758-477">/*launchsettings.json* *，json 是*開發環境的工具設定檔，如下所述：</span><span class="sxs-lookup"><span data-stu-id="c8758-477">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="c8758-478">在 <xref:fundamentals/environments#development>。</span><span class="sxs-lookup"><span data-stu-id="c8758-478">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="c8758-479">在檔集內，用來為開發案例設定 ASP.NET Core 應用程式的檔案。</span><span class="sxs-lookup"><span data-stu-id="c8758-479">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="c8758-480">*web.config*是伺服器設定檔，如下列主題所述：</span><span class="sxs-lookup"><span data-stu-id="c8758-480">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="c8758-481">如需從舊版 ASP.NET 遷移應用程式設定的詳細資訊，請參閱 <xref:migration/proper-to-2x/index#store-configurations>。</span><span class="sxs-lookup"><span data-stu-id="c8758-481">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="c8758-482">從外部組件新增設定</span><span class="sxs-lookup"><span data-stu-id="c8758-482">Add configuration from an external assembly</span></span>

<span data-ttu-id="c8758-483"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。</span><span class="sxs-lookup"><span data-stu-id="c8758-483">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="c8758-484">如需詳細資訊，請參閱<xref:fundamentals/configuration/platform-specific-configuration>。</span><span class="sxs-lookup"><span data-stu-id="c8758-484">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c8758-485">其他資源</span><span class="sxs-lookup"><span data-stu-id="c8758-485">Additional resources</span></span>

* [<span data-ttu-id="c8758-486">設定原始程式碼</span><span class="sxs-lookup"><span data-stu-id="c8758-486">Configuration source code</span></span>](https://github.com/dotnet/extensions/tree/master/src/Configuration)
* <xref:fundamentals/configuration/options>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c8758-487">ASP.NET Core 中的應用程式設定是以由*設定提供者*所建立的機碼值組為基礎。</span><span class="sxs-lookup"><span data-stu-id="c8758-487">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="c8758-488">設定提供者會從各種設定來源將設定資料讀取到機碼值組中：</span><span class="sxs-lookup"><span data-stu-id="c8758-488">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="c8758-489">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="c8758-489">Azure Key Vault</span></span>
* <span data-ttu-id="c8758-490">Azure 應用程式組態</span><span class="sxs-lookup"><span data-stu-id="c8758-490">Azure App Configuration</span></span>
* <span data-ttu-id="c8758-491">命令列引數</span><span class="sxs-lookup"><span data-stu-id="c8758-491">Command-line arguments</span></span>
* <span data-ttu-id="c8758-492">自訂提供者 (已安裝或已建立)</span><span class="sxs-lookup"><span data-stu-id="c8758-492">Custom providers (installed or created)</span></span>
* <span data-ttu-id="c8758-493">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="c8758-493">Directory files</span></span>
* <span data-ttu-id="c8758-494">環境變數</span><span class="sxs-lookup"><span data-stu-id="c8758-494">Environment variables</span></span>
* <span data-ttu-id="c8758-495">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="c8758-495">In-memory .NET objects</span></span>
* <span data-ttu-id="c8758-496">設定檔</span><span class="sxs-lookup"><span data-stu-id="c8758-496">Settings files</span></span>

<span data-ttu-id="c8758-497">一般組態提供者案例 ([microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) 的組態套件包含在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="c8758-497">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c8758-498">下列程式碼範例與範例應用程式中的程式碼範例使用 <xref:Microsoft.Extensions.Configuration> 命名空間：</span><span class="sxs-lookup"><span data-stu-id="c8758-498">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="c8758-499">*選項模式*是此主題中所述之設定概念的延伸。</span><span class="sxs-lookup"><span data-stu-id="c8758-499">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="c8758-500">選項使用類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-500">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="c8758-501">如需詳細資訊，請參閱<xref:fundamentals/configuration/options>。</span><span class="sxs-lookup"><span data-stu-id="c8758-501">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="c8758-502">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c8758-502">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="c8758-503">主機與應用程式組態的比較</span><span class="sxs-lookup"><span data-stu-id="c8758-503">Host versus app configuration</span></span>

<span data-ttu-id="c8758-504">設定及啟動應用程式之前，會先設定及啟動「主機」。</span><span class="sxs-lookup"><span data-stu-id="c8758-504">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="c8758-505">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="c8758-505">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="c8758-506">應用程式與主機都是使用此主題中所述的設定提供者來設定的。</span><span class="sxs-lookup"><span data-stu-id="c8758-506">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="c8758-507">主機組態機碼/值組也會包含在應用程式的組態中。</span><span class="sxs-lookup"><span data-stu-id="c8758-507">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="c8758-508">如需有關當建置主機時如何使用設定提供者的詳細資訊，以及設定來源如何影響主機設定的詳細資訊，請參閱 <xref:fundamentals/index#host>。</span><span class="sxs-lookup"><span data-stu-id="c8758-508">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="c8758-509">其他設定</span><span class="sxs-lookup"><span data-stu-id="c8758-509">Other configuration</span></span>

<span data-ttu-id="c8758-510">本主題僅適用于*應用程式*設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-510">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="c8758-511">執行和裝載 ASP.NET Core 應用程式的其他層面，是使用本主題未涵蓋的設定檔來設定：</span><span class="sxs-lookup"><span data-stu-id="c8758-511">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="c8758-512">/*launchsettings.json* *，json 是*開發環境的工具設定檔，如下所述：</span><span class="sxs-lookup"><span data-stu-id="c8758-512">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="c8758-513">在 <xref:fundamentals/environments#development>。</span><span class="sxs-lookup"><span data-stu-id="c8758-513">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="c8758-514">在檔集內，用來為開發案例設定 ASP.NET Core 應用程式的檔案。</span><span class="sxs-lookup"><span data-stu-id="c8758-514">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="c8758-515">*web.config*是伺服器設定檔，如下列主題所述：</span><span class="sxs-lookup"><span data-stu-id="c8758-515">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="c8758-516">如需從舊版 ASP.NET 遷移應用程式設定的詳細資訊，請參閱 <xref:migration/proper-to-2x/index#store-configurations>。</span><span class="sxs-lookup"><span data-stu-id="c8758-516">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="c8758-517">預設組態</span><span class="sxs-lookup"><span data-stu-id="c8758-517">Default configuration</span></span>

<span data-ttu-id="c8758-518">以 ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) 範本為基礎的 Web 應用程式，會在建置主機時呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>。</span><span class="sxs-lookup"><span data-stu-id="c8758-518">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="c8758-519">`CreateDefaultBuilder` 會以下列順序提供應用程式的預設組態：</span><span class="sxs-lookup"><span data-stu-id="c8758-519">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="c8758-520">下列項目適用於使用 [Web 主機](xref:fundamentals/host/web-host)的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8758-520">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="c8758-521">如需使用[一般主機](xref:fundamentals/host/generic-host)時預設組態的詳細資料，請參閱[本主題的最新版本](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="c8758-521">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="c8758-522">主機組態的提供來源：</span><span class="sxs-lookup"><span data-stu-id="c8758-522">Host configuration is provided from:</span></span>
  * <span data-ttu-id="c8758-523">使用`ASPNETCORE_`環境變數組態提供者`ASPNETCORE_ENVIRONMENT`且以 [ 為前置詞 (例如 ](#environment-variables-configuration-provider)) 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="c8758-523">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="c8758-524">載入設定機碼值組時，會移除前置詞 (`ASPNETCORE_`)。</span><span class="sxs-lookup"><span data-stu-id="c8758-524">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="c8758-525">使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="c8758-525">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="c8758-526">應用程式設定的提供來源：</span><span class="sxs-lookup"><span data-stu-id="c8758-526">App configuration is provided from:</span></span>
  * <span data-ttu-id="c8758-527">使用*檔案組態提供者*的 [appsettings.json](#file-configuration-provider)。</span><span class="sxs-lookup"><span data-stu-id="c8758-527">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="c8758-528">使用*檔案組態提供者*的 [appsettings.{Environment}.json](#file-configuration-provider)。</span><span class="sxs-lookup"><span data-stu-id="c8758-528">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="c8758-529">應用程式在使用項目組件之 [ 環境中執行時的](xref:security/app-secrets)秘密管理員`Development`。</span><span class="sxs-lookup"><span data-stu-id="c8758-529">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="c8758-530">使用[環境變數組態提供者](#environment-variables-configuration-provider)的環境變數。</span><span class="sxs-lookup"><span data-stu-id="c8758-530">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="c8758-531">使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="c8758-531">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

## <a name="security"></a><span data-ttu-id="c8758-532">安全性</span><span class="sxs-lookup"><span data-stu-id="c8758-532">Security</span></span>

<span data-ttu-id="c8758-533">採用下列做法來保護敏感性組態資料：</span><span class="sxs-lookup"><span data-stu-id="c8758-533">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="c8758-534">永遠不要將密碼或其他敏感性資料儲存在設定提供者程式碼或純文字設定檔中。</span><span class="sxs-lookup"><span data-stu-id="c8758-534">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="c8758-535">不要在開發或測試環境中使用生產環境祕密。</span><span class="sxs-lookup"><span data-stu-id="c8758-535">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="c8758-536">請在專案外部指定祕密，以防止其意外認可至開放原始碼存放庫。</span><span class="sxs-lookup"><span data-stu-id="c8758-536">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="c8758-537">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="c8758-537">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="c8758-538"><xref:security/app-secrets> &ndash; 包含有關使用環境變數來儲存敏感性資料的建議。</span><span class="sxs-lookup"><span data-stu-id="c8758-538"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="c8758-539">「祕密管理員」使用「檔案設定提供者」以 JSON 檔案在本機系統上存放使用者祕密。</span><span class="sxs-lookup"><span data-stu-id="c8758-539">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="c8758-540">此主題稍後將說明「檔案設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="c8758-540">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="c8758-541">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 可安全地儲存 ASP.NET Core 應用程式的應用程式祕密。</span><span class="sxs-lookup"><span data-stu-id="c8758-541">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="c8758-542">如需詳細資訊，請參閱<xref:security/key-vault-configuration>。</span><span class="sxs-lookup"><span data-stu-id="c8758-542">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="c8758-543">階層式設定資料</span><span class="sxs-lookup"><span data-stu-id="c8758-543">Hierarchical configuration data</span></span>

<span data-ttu-id="c8758-544">設定 API 可在設定機碼中使用分隔符號來壓平合併階層式資料，以管理階層式設定資料。</span><span class="sxs-lookup"><span data-stu-id="c8758-544">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="c8758-545">在下列 JSON 檔案中，兩個區段的結構式階層中有四個機碼存在：</span><span class="sxs-lookup"><span data-stu-id="c8758-545">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="c8758-546">當該檔案被讀入設定之後，會建立唯一機碼來維護設定來源的原始階層式資料結構。</span><span class="sxs-lookup"><span data-stu-id="c8758-546">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="c8758-547">系統會使用冒號 (`:`) 將區段與機碼壓平合併以維護原始結構：</span><span class="sxs-lookup"><span data-stu-id="c8758-547">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="c8758-548">section0:key0</span><span class="sxs-lookup"><span data-stu-id="c8758-548">section0:key0</span></span>
* <span data-ttu-id="c8758-549">section0:key1</span><span class="sxs-lookup"><span data-stu-id="c8758-549">section0:key1</span></span>
* <span data-ttu-id="c8758-550">section1:key0</span><span class="sxs-lookup"><span data-stu-id="c8758-550">section1:key0</span></span>
* <span data-ttu-id="c8758-551">section1:key1</span><span class="sxs-lookup"><span data-stu-id="c8758-551">section1:key1</span></span>

<span data-ttu-id="c8758-552"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> 與 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> 方法可用來在設定資料中隔離區段與區段的子系。</span><span class="sxs-lookup"><span data-stu-id="c8758-552"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="c8758-553">[GetSection,、 GetChildren 與 Exists](#getsection-getchildren-and-exists) 中說明這些方法。</span><span class="sxs-lookup"><span data-stu-id="c8758-553">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="c8758-554">慣例</span><span class="sxs-lookup"><span data-stu-id="c8758-554">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="c8758-555">來源和提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-555">Sources and providers</span></span>

<span data-ttu-id="c8758-556">在應用程式啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="c8758-556">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="c8758-557">實作變更偵測的組態提供者能夠在基礎設定變更時重新載入組態。</span><span class="sxs-lookup"><span data-stu-id="c8758-557">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="c8758-558">例如，檔案組態提供者 (將於本主題稍後討論) 和 [Azure Key Vault 組態提供者](xref:security/key-vault-configuration)均會實作變更偵測。</span><span class="sxs-lookup"><span data-stu-id="c8758-558">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="c8758-559">您可以在應用程式的<xref:Microsoft.Extensions.Configuration.IConfiguration>相依性插入 (DI)[ 容器中找到 ](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="c8758-559"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="c8758-560"><xref:Microsoft.Extensions.Configuration.IConfiguration> 可以插入至 Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> 或 MVC <xref:Microsoft.AspNetCore.Mvc.Controller>，以取得類別的設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-560"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="c8758-561">在下列範例中，`_config` 欄位是用來存取設定值：</span><span class="sxs-lookup"><span data-stu-id="c8758-561">In the following examples, the `_config` field is used to access configuration values:</span></span>

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

<span data-ttu-id="c8758-562">設定提供者無法使用 DI，因為當它們由主機設定時，它無法使用。</span><span class="sxs-lookup"><span data-stu-id="c8758-562">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="c8758-563">按鍵</span><span class="sxs-lookup"><span data-stu-id="c8758-563">Keys</span></span>

<span data-ttu-id="c8758-564">設定機碼會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="c8758-564">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="c8758-565">機碼區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="c8758-565">Keys are case-insensitive.</span></span> <span data-ttu-id="c8758-566">例如，`ConnectionString` 與 `connectionstring` 會被視為相等的機碼。</span><span class="sxs-lookup"><span data-stu-id="c8758-566">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="c8758-567">若相同機碼的值是由相同或不同設定提供者設定，在機碼上設定的最後一個值是所使用的值。</span><span class="sxs-lookup"><span data-stu-id="c8758-567">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="c8758-568">階層式機碼</span><span class="sxs-lookup"><span data-stu-id="c8758-568">Hierarchical keys</span></span>
  * <span data-ttu-id="c8758-569">在設定 API 內，冒號分隔字元 (`:`) 可在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="c8758-569">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="c8758-570">在環境變數中，冒號分隔字元可能無法在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="c8758-570">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="c8758-571">所有平台都支援雙底線 (`__`)，且會自動轉換為冒號。</span><span class="sxs-lookup"><span data-stu-id="c8758-571">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="c8758-572">在 Azure Key Vault 中，階層式機碼使用 `--` (兩個破折號) 來做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="c8758-572">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="c8758-573">撰寫程式碼，以在將密碼載入應用程式的設定時，以冒號取代破折號。</span><span class="sxs-lookup"><span data-stu-id="c8758-573">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="c8758-574"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="c8758-574">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="c8758-575">[將陣列繫結到類別](#bind-an-array-to-a-class)一節說明陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="c8758-575">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="c8758-576">值</span><span class="sxs-lookup"><span data-stu-id="c8758-576">Values</span></span>

<span data-ttu-id="c8758-577">設定值會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="c8758-577">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="c8758-578">值是字串。</span><span class="sxs-lookup"><span data-stu-id="c8758-578">Values are strings.</span></span>
* <span data-ttu-id="c8758-579">Null 值無法存放在設定中或繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="c8758-579">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="c8758-580">提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-580">Providers</span></span>

<span data-ttu-id="c8758-581">下表顯示可供 ASP.NET Core 應用程式使用的設定提供者。</span><span class="sxs-lookup"><span data-stu-id="c8758-581">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="c8758-582">Provider</span><span class="sxs-lookup"><span data-stu-id="c8758-582">Provider</span></span> | <span data-ttu-id="c8758-583">從&hellip;提供設定</span><span class="sxs-lookup"><span data-stu-id="c8758-583">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="c8758-584">[Azure Key Vault 設定提供者](xref:security/key-vault-configuration) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="c8758-584">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="c8758-585">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="c8758-585">Azure Key Vault</span></span> |
| <span data-ttu-id="c8758-586">[Azure 應用程式組態提供者](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure 文件)</span><span class="sxs-lookup"><span data-stu-id="c8758-586">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="c8758-587">Azure 應用程式組態</span><span class="sxs-lookup"><span data-stu-id="c8758-587">Azure App Configuration</span></span> |
| [<span data-ttu-id="c8758-588">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-588">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="c8758-589">命令列參數</span><span class="sxs-lookup"><span data-stu-id="c8758-589">Command-line parameters</span></span> |
| [<span data-ttu-id="c8758-590">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-590">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="c8758-591">自訂來源</span><span class="sxs-lookup"><span data-stu-id="c8758-591">Custom source</span></span> |
| [<span data-ttu-id="c8758-592">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-592">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="c8758-593">環境變數</span><span class="sxs-lookup"><span data-stu-id="c8758-593">Environment variables</span></span> |
| [<span data-ttu-id="c8758-594">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-594">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="c8758-595">檔案 (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="c8758-595">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="c8758-596">每個檔案機碼的設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-596">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="c8758-597">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="c8758-597">Directory files</span></span> |
| [<span data-ttu-id="c8758-598">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-598">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="c8758-599">記憶體內集合</span><span class="sxs-lookup"><span data-stu-id="c8758-599">In-memory collections</span></span> |
| <span data-ttu-id="c8758-600">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="c8758-600">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="c8758-601">使用者設定檔目錄中的檔案</span><span class="sxs-lookup"><span data-stu-id="c8758-601">File in the user profile directory</span></span> |

<span data-ttu-id="c8758-602">在啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="c8758-602">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="c8758-603">本主題所描述的設定提供者會依字母順序描述，而不是程式碼排列它們的順序。</span><span class="sxs-lookup"><span data-stu-id="c8758-603">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="c8758-604">請在程式碼中訂購設定提供者，以符合應用程式所需之基礎設定來源的優先順序。</span><span class="sxs-lookup"><span data-stu-id="c8758-604">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="c8758-605">典型的設定提供者順序是：</span><span class="sxs-lookup"><span data-stu-id="c8758-605">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="c8758-606">檔案 (*appsettings.json*、*appsettings.{Environment}.json*，其中 `{Environment}` 是應用程式的目前裝載環境)</span><span class="sxs-lookup"><span data-stu-id="c8758-606">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="c8758-607">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="c8758-607">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="c8758-608">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (僅限開發環境)</span><span class="sxs-lookup"><span data-stu-id="c8758-608">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="c8758-609">環境變數</span><span class="sxs-lookup"><span data-stu-id="c8758-609">Environment variables</span></span>
1. <span data-ttu-id="c8758-610">命令列引數</span><span class="sxs-lookup"><span data-stu-id="c8758-610">Command-line arguments</span></span>

<span data-ttu-id="c8758-611">將命令列組態提供者放在提供者序列結尾是常見做法，因為這樣可以讓命令列引數覆寫由其他提供者所設定的組態。</span><span class="sxs-lookup"><span data-stu-id="c8758-611">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="c8758-612">當使用 `CreateDefaultBuilder`初始化新的主機產生器時，會使用上述的提供者序列。</span><span class="sxs-lookup"><span data-stu-id="c8758-612">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="c8758-613">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="c8758-613">For more information, see the [Default configuration](#default-configuration) section.</span></span>

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="c8758-614">使用 UseConfiguration 設定主機建立器</span><span class="sxs-lookup"><span data-stu-id="c8758-614">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="c8758-615">若要設定主機建立器，請使用該組態在主機建立器上呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>。</span><span class="sxs-lookup"><span data-stu-id="c8758-615">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

## <a name="configureappconfiguration"></a><span data-ttu-id="c8758-616">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="c8758-616">ConfigureAppConfiguration</span></span>

<span data-ttu-id="c8758-617">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定提供者，以及由 `CreateDefaultBuilder` 自動新增的設定提供者：</span><span class="sxs-lookup"><span data-stu-id="c8758-617">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="c8758-618">使用命令列引數覆寫先前的組態</span><span class="sxs-lookup"><span data-stu-id="c8758-618">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="c8758-619">若要提供可使用命令列引數覆寫的應用程式組態，請最後呼叫 `AddCommandLine`：</span><span class="sxs-lookup"><span data-stu-id="c8758-619">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="c8758-620">移除 CreateDefaultBuilder 新增的提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-620">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="c8758-621">若要移除 `CreateDefaultBuilder`新增的提供者，請先在[IConfigurationBuilder](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources)上呼叫[Clear](/dotnet/api/system.collections.generic.icollection-1.clear) ：</span><span class="sxs-lookup"><span data-stu-id="c8758-621">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="c8758-622">在應用程式啟動期間使用組態</span><span class="sxs-lookup"><span data-stu-id="c8758-622">Consume configuration during app startup</span></span>

<span data-ttu-id="c8758-623">應用程式啟動期間，可以使用 `ConfigureAppConfiguration` 中為應用程式提供的組態，包括 `Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="c8758-623">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c8758-624">如需詳細資訊，請參閱[在啟動期間存取組態](#access-configuration-during-startup)一節。</span><span class="sxs-lookup"><span data-stu-id="c8758-624">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="c8758-625">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-625">Command-line Configuration Provider</span></span>

<span data-ttu-id="c8758-626"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> 會在執行階段從命令列引數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-626">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="c8758-627">為了啟用命令列設定，<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 延伸模組方法會在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫。</span><span class="sxs-lookup"><span data-stu-id="c8758-627">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="c8758-628">呼叫 `AddCommandLine` 時，會自動呼叫 `CreateDefaultBuilder(string [])`。</span><span class="sxs-lookup"><span data-stu-id="c8758-628">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="c8758-629">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="c8758-629">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="c8758-630">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="c8758-630">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="c8758-631">從 *appsettings.json* 與 *appsettings.{Environment}.json* 檔案載入其他選擇性組態。</span><span class="sxs-lookup"><span data-stu-id="c8758-631">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="c8758-632">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="c8758-632">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="c8758-633">環境變數。</span><span class="sxs-lookup"><span data-stu-id="c8758-633">Environment variables.</span></span>

<span data-ttu-id="c8758-634">`CreateDefaultBuilder` 會最後才新增命令列設定提供者。</span><span class="sxs-lookup"><span data-stu-id="c8758-634">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="c8758-635">在執行階段傳遞的命令列引數會覆寫由其它提供者所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-635">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="c8758-636">`CreateDefaultBuilder` 會在建構主機時執行作業。</span><span class="sxs-lookup"><span data-stu-id="c8758-636">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="c8758-637">因此，由`CreateDefaultBuilder` 啟用的命令列設定可以影響主機的設定方式。</span><span class="sxs-lookup"><span data-stu-id="c8758-637">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="c8758-638">針對以 ASP.NET Core 範本為基礎的應用程式，`AddCommandLine` 已由 `CreateDefaultBuilder` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="c8758-638">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="c8758-639">若要新增其他組態提供者並維持能夠使用命令列引數覆寫這些提供者組態的能力，請在 `ConfigureAppConfiguration` 中呼叫應用程式的其他提供者，且最後呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="c8758-639">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="c8758-640">**範例**</span><span class="sxs-lookup"><span data-stu-id="c8758-640">**Example**</span></span>

<span data-ttu-id="c8758-641">範例應用程式利用靜態方便方法 `CreateDefaultBuilder` 的優勢來建置主機，這包括對 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="c8758-641">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="c8758-642">從專案目錄中開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="c8758-642">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="c8758-643">提供命令列引數給 `dotnet run` 命令 `dotnet run CommandLineKey=CommandLineValue`。</span><span class="sxs-lookup"><span data-stu-id="c8758-643">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="c8758-644">在應用程式執行之後，開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8758-644">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="c8758-645">觀察輸出是否包含提供給 `dotnet run` 之設定命令列引數的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="c8758-645">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="c8758-646">引數</span><span class="sxs-lookup"><span data-stu-id="c8758-646">Arguments</span></span>

<span data-ttu-id="c8758-647">當值後面接著空格時，值必須接著等號 (`=`)，或機碼必須有前置詞 (`--` 或 `/`)。</span><span class="sxs-lookup"><span data-stu-id="c8758-647">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="c8758-648">如果使用等號 (例如 `CommandLineKey=`)，則不需要此值。</span><span class="sxs-lookup"><span data-stu-id="c8758-648">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="c8758-649">索引鍵前置字元</span><span class="sxs-lookup"><span data-stu-id="c8758-649">Key prefix</span></span>               | <span data-ttu-id="c8758-650">範例</span><span class="sxs-lookup"><span data-stu-id="c8758-650">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="c8758-651">沒有前置字元</span><span class="sxs-lookup"><span data-stu-id="c8758-651">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="c8758-652">雙虛線 (`--`)</span><span class="sxs-lookup"><span data-stu-id="c8758-652">Two dashes (`--`)</span></span>        | <span data-ttu-id="c8758-653">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="c8758-653">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="c8758-654">正斜線 (`/`)</span><span class="sxs-lookup"><span data-stu-id="c8758-654">Forward slash (`/`)</span></span>      | <span data-ttu-id="c8758-655">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="c8758-655">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="c8758-656">在相同的命令中，請不要混合使用等號搭配使用空格之機碼值組的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="c8758-656">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="c8758-657">範例命令：</span><span class="sxs-lookup"><span data-stu-id="c8758-657">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="c8758-658">切換對應</span><span class="sxs-lookup"><span data-stu-id="c8758-658">Switch mappings</span></span>

<span data-ttu-id="c8758-659">參數對應允許索引鍵名稱取代邏輯。</span><span class="sxs-lookup"><span data-stu-id="c8758-659">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="c8758-660">以 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>手動建立設定時，請將參數取代的字典提供給 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 方法。</span><span class="sxs-lookup"><span data-stu-id="c8758-660">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="c8758-661">使用切換對應字典時，會檢查字典中是否有任何索引鍵符合命令列引數所提供的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="c8758-661">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="c8758-662">如果在字典中找到命令列索引鍵，則會傳回字典值 (索引鍵取代) 以在應用程式的設定中設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="c8758-662">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="c8758-663">所有前面加上單虛線 (`-`) 的命令列索引鍵都需要切換對應。</span><span class="sxs-lookup"><span data-stu-id="c8758-663">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="c8758-664">切換對應字典索引鍵規則：</span><span class="sxs-lookup"><span data-stu-id="c8758-664">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="c8758-665">切換必須以單虛線 (`-`) 或雙虛線 (`--`) 開頭。</span><span class="sxs-lookup"><span data-stu-id="c8758-665">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="c8758-666">切換對應字典不能包含重複索引鍵。</span><span class="sxs-lookup"><span data-stu-id="c8758-666">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="c8758-667">建立切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="c8758-667">Create a switch mappings dictionary.</span></span> <span data-ttu-id="c8758-668">下列範例會建立兩個切換對應：</span><span class="sxs-lookup"><span data-stu-id="c8758-668">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="c8758-669">建立主機時，請使用切換對應字典呼叫 `AddCommandLine`：</span><span class="sxs-lookup"><span data-stu-id="c8758-669">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="c8758-670">針對使用切換對應的應用程式，呼叫 `CreateDefaultBuilder` 不應傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="c8758-670">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="c8758-671">`CreateDefaultBuilder` 方法的 `AddCommandLine` 呼叫不包括對應的切換，且沒有任何方式可以將切換對應字典傳遞給 `CreateDefaultBuilder`。</span><span class="sxs-lookup"><span data-stu-id="c8758-671">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="c8758-672">解決方案並非將引數傳遞給 `CreateDefaultBuilder`，而是允許 `ConfigurationBuilder` 方法的 `AddCommandLine` 方法同時處理引數與切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="c8758-672">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="c8758-673">建立切換對應字典之後，它會包含下表中所示的資料。</span><span class="sxs-lookup"><span data-stu-id="c8758-673">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="c8758-674">Key</span><span class="sxs-lookup"><span data-stu-id="c8758-674">Key</span></span>       | <span data-ttu-id="c8758-675">值</span><span class="sxs-lookup"><span data-stu-id="c8758-675">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="c8758-676">若啟動應用程式時使用切換對應機碼，設定會接收由字典提供之機碼上的設定值：</span><span class="sxs-lookup"><span data-stu-id="c8758-676">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="c8758-677">執行上述命令之後，設定包含下表中顯示的值。</span><span class="sxs-lookup"><span data-stu-id="c8758-677">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="c8758-678">Key</span><span class="sxs-lookup"><span data-stu-id="c8758-678">Key</span></span>               | <span data-ttu-id="c8758-679">值</span><span class="sxs-lookup"><span data-stu-id="c8758-679">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="c8758-680">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-680">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="c8758-681"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> 會在執行階段從環境變數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-681">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="c8758-682">若要啟用環境變數設定，請在 <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="c8758-682">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="c8758-683">[Azure App Service](https://azure.microsoft.com/services/app-service/)允許在 Azure 入口網站中設定環境變數，以使用環境變數設定提供者來覆寫應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-683">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="c8758-684">如需詳細資訊，請參閱 [Azure App：使用 Azure 入口網站覆寫應用程式設定](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)。</span><span class="sxs-lookup"><span data-stu-id="c8758-684">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="c8758-685">使用 `AddEnvironmentVariables`Web 主機`ASPNETCORE_`初始化新的主機建立器並呼叫 [ 時，可使用 ](#host-versus-app-configuration) 為[主機組態](xref:fundamentals/host/web-host)載入字首為 `CreateDefaultBuilder` 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="c8758-685">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="c8758-686">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="c8758-686">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="c8758-687">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="c8758-687">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="c8758-688">來自無首碼之環境變數的應用程式組態 (在未提供首碼的情況下呼叫 `AddEnvironmentVariables`)。</span><span class="sxs-lookup"><span data-stu-id="c8758-688">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="c8758-689">從 *appsettings.json* 與 *appsettings.{Environment}.json* 檔案載入其他選擇性組態。</span><span class="sxs-lookup"><span data-stu-id="c8758-689">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="c8758-690">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="c8758-690">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="c8758-691">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="c8758-691">Command-line arguments.</span></span>

<span data-ttu-id="c8758-692">從使用者祕密與 *appsettings* 檔案建立設定之後，會呼叫「環境變數設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="c8758-692">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="c8758-693">在此位置呼叫提供者可讓系統在執行階段讀取環境變數，以覆寫由使用者祕密與 *appsettings* 檔案所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-693">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="c8758-694">若要從其他環境變數提供應用程式設定，請在 `ConfigureAppConfiguration` 中呼叫應用程式的其他提供者，並使用前置詞呼叫 `AddEnvironmentVariables`：</span><span class="sxs-lookup"><span data-stu-id="c8758-694">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="c8758-695">呼叫 last `AddEnvironmentVariables`，以允許具有指定前置詞的環境變數覆寫其他提供者的值。</span><span class="sxs-lookup"><span data-stu-id="c8758-695">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="c8758-696">**範例**</span><span class="sxs-lookup"><span data-stu-id="c8758-696">**Example**</span></span>

<span data-ttu-id="c8758-697">範例應用程式利用靜態方便方法 `CreateDefaultBuilder` 的優勢來建置主機，這包括對 `AddEnvironmentVariables` 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="c8758-697">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="c8758-698">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8758-698">Run the sample app.</span></span> <span data-ttu-id="c8758-699">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8758-699">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="c8758-700">觀察輸出是否包含環境變數 `ENVIRONMENT` 的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="c8758-700">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="c8758-701">值反映應用程式執行所在環境，在本機執行時，通常是 `Development`。</span><span class="sxs-lookup"><span data-stu-id="c8758-701">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="c8758-702">為縮短由應用程式轉譯的環境變數清單，應用程式會篩選環境變數。</span><span class="sxs-lookup"><span data-stu-id="c8758-702">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="c8758-703">請參閱範例應用程式的 *Pages/Index.cshtml.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="c8758-703">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="c8758-704">若要公開應用程式可用的所有環境變數，請將*Pages/Index. cshtml*中的 `FilteredConfiguration` 變更為下列內容：</span><span class="sxs-lookup"><span data-stu-id="c8758-704">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="c8758-705">首碼</span><span class="sxs-lookup"><span data-stu-id="c8758-705">Prefixes</span></span>

<span data-ttu-id="c8758-706">在 `AddEnvironmentVariables` 方法中提供前置詞時，會篩選載入應用程式設定中的環境變數。</span><span class="sxs-lookup"><span data-stu-id="c8758-706">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="c8758-707">例如，若要篩選前置詞為 `CUSTOM_` 的環境變數，請提供前置詞給設定提供者：</span><span class="sxs-lookup"><span data-stu-id="c8758-707">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="c8758-708">建立設定機碼值組時，會移除前置詞。</span><span class="sxs-lookup"><span data-stu-id="c8758-708">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="c8758-709">建立主機建立器時，主機組態由環境變數提供。</span><span class="sxs-lookup"><span data-stu-id="c8758-709">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="c8758-710">如需這些環境變數所用前置詞的詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="c8758-710">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="c8758-711">**連接字串前置詞**</span><span class="sxs-lookup"><span data-stu-id="c8758-711">**Connection string prefixes**</span></span>

<span data-ttu-id="c8758-712">設定 API 四個連接字串環境變數的特殊處理規則，這些這些環境變數牽涉到針對應用程式環境設定 Azure 連接字串。</span><span class="sxs-lookup"><span data-stu-id="c8758-712">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="c8758-713">若將前置詞提供給 `AddEnvironmentVariables`具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="c8758-713">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="c8758-714">連接字串前置詞</span><span class="sxs-lookup"><span data-stu-id="c8758-714">Connection string prefix</span></span> | <span data-ttu-id="c8758-715">Provider</span><span class="sxs-lookup"><span data-stu-id="c8758-715">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="c8758-716">自訂提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-716">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="c8758-717">MySQL</span><span class="sxs-lookup"><span data-stu-id="c8758-717">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="c8758-718">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="c8758-718">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="c8758-719">SQL Server</span><span class="sxs-lookup"><span data-stu-id="c8758-719">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="c8758-720">當探索到具有下表所顯示之任何四個前置詞的環境變數並將其載入到設定中時：</span><span class="sxs-lookup"><span data-stu-id="c8758-720">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="c8758-721">會透過移除環境變數前置詞並新增設定機碼區段 (`ConnectionStrings`) 來移除具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="c8758-721">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="c8758-722">會建立新的設定機碼值組以代表資料庫連線提供者 (`CUSTOMCONNSTR_` 除外，這沒有所述提供者)。</span><span class="sxs-lookup"><span data-stu-id="c8758-722">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="c8758-723">環境變數機碼</span><span class="sxs-lookup"><span data-stu-id="c8758-723">Environment variable key</span></span> | <span data-ttu-id="c8758-724">已轉換的設定機碼</span><span class="sxs-lookup"><span data-stu-id="c8758-724">Converted configuration key</span></span> | <span data-ttu-id="c8758-725">提供者設定項目</span><span class="sxs-lookup"><span data-stu-id="c8758-725">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="c8758-726">設定項目未建立。</span><span class="sxs-lookup"><span data-stu-id="c8758-726">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="c8758-727">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="c8758-727">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="c8758-728">值：`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="c8758-728">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="c8758-729">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="c8758-729">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="c8758-730">值：`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="c8758-730">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="c8758-731">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="c8758-731">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="c8758-732">值：`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="c8758-732">Value: `System.Data.SqlClient`</span></span>  |

<span data-ttu-id="c8758-733">**範例**</span><span class="sxs-lookup"><span data-stu-id="c8758-733">**Example**</span></span>

<span data-ttu-id="c8758-734">在伺服器上建立自訂連接字串環境變數：</span><span class="sxs-lookup"><span data-stu-id="c8758-734">A custom connection string environment variable is created on the server:</span></span>

* <span data-ttu-id="c8758-735">名稱 &ndash; `CUSTOMCONNSTR_ReleaseDB`</span><span class="sxs-lookup"><span data-stu-id="c8758-735">Name &ndash; `CUSTOMCONNSTR_ReleaseDB`</span></span>
* <span data-ttu-id="c8758-736">值 &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span><span class="sxs-lookup"><span data-stu-id="c8758-736">Value &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span></span>

<span data-ttu-id="c8758-737">如果 `IConfiguration` 插入，並將其指派給名為 `_config`的欄位，請閱讀值：</span><span class="sxs-lookup"><span data-stu-id="c8758-737">If `IConfiguration` is injected and assigned to a field named `_config`, read the value:</span></span>

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a><span data-ttu-id="c8758-738">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-738">File Configuration Provider</span></span>

<span data-ttu-id="c8758-739"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> 是用於從檔案系統載入設定的基底類別。</span><span class="sxs-lookup"><span data-stu-id="c8758-739"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="c8758-740">下列設定提供者專用於特定檔案類型：</span><span class="sxs-lookup"><span data-stu-id="c8758-740">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="c8758-741">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-741">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="c8758-742">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-742">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="c8758-743">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-743">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="c8758-744">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-744">INI Configuration Provider</span></span>

<span data-ttu-id="c8758-745"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> 會在執行階段從 INI 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-745">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="c8758-746">若要啟用 INI 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="c8758-746">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="c8758-747">冒號可用來做為 INI 檔案設定中的區段分隔符號。</span><span class="sxs-lookup"><span data-stu-id="c8758-747">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="c8758-748">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="c8758-748">Overloads permit specifying:</span></span>

* <span data-ttu-id="c8758-749">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="c8758-749">Whether the file is optional.</span></span>
* <span data-ttu-id="c8758-750">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-750">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="c8758-751"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="c8758-751">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="c8758-752">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="c8758-752">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="c8758-753">INI 設定檔的一般範例：</span><span class="sxs-lookup"><span data-stu-id="c8758-753">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="c8758-754">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="c8758-754">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="c8758-755">section0:key0</span><span class="sxs-lookup"><span data-stu-id="c8758-755">section0:key0</span></span>
* <span data-ttu-id="c8758-756">section0:key1</span><span class="sxs-lookup"><span data-stu-id="c8758-756">section0:key1</span></span>
* <span data-ttu-id="c8758-757">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="c8758-757">section1:subsection:key</span></span>
* <span data-ttu-id="c8758-758">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="c8758-758">section2:subsection0:key</span></span>
* <span data-ttu-id="c8758-759">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="c8758-759">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="c8758-760">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-760">JSON Configuration Provider</span></span>

<span data-ttu-id="c8758-761"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> 會在執行階段從 JSON 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-761">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="c8758-762">若要啟用 JSON 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="c8758-762">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="c8758-763">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="c8758-763">Overloads permit specifying:</span></span>

* <span data-ttu-id="c8758-764">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="c8758-764">Whether the file is optional.</span></span>
* <span data-ttu-id="c8758-765">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-765">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="c8758-766"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="c8758-766">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="c8758-767">當使用 `CreateDefaultBuilder`初始化新的主機產生器時，會自動呼叫 `AddJsonFile` 兩次。</span><span class="sxs-lookup"><span data-stu-id="c8758-767">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="c8758-768">會呼叫此方法以從下列位置載入設定：</span><span class="sxs-lookup"><span data-stu-id="c8758-768">The method is called to load configuration from:</span></span>

* <span data-ttu-id="c8758-769">*appsettings*會先讀取此檔案 &ndash;。</span><span class="sxs-lookup"><span data-stu-id="c8758-769">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="c8758-770">檔案的環境版本可以覆寫由 *appsettings.json* 檔案提供的值。</span><span class="sxs-lookup"><span data-stu-id="c8758-770">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="c8758-771">*appsettings。{環境}. json* &ndash; 檔案的環境版本是根據[IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)載入。</span><span class="sxs-lookup"><span data-stu-id="c8758-771">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="c8758-772">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="c8758-772">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="c8758-773">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="c8758-773">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="c8758-774">環境變數。</span><span class="sxs-lookup"><span data-stu-id="c8758-774">Environment variables.</span></span>
* <span data-ttu-id="c8758-775">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="c8758-775">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="c8758-776">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="c8758-776">Command-line arguments.</span></span>

<span data-ttu-id="c8758-777">會先建立 JSON 設定提供者。</span><span class="sxs-lookup"><span data-stu-id="c8758-777">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="c8758-778">因此，使用者祕密、環境變數與命令列引數會覆寫由 *appsettings* 檔案設定的設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-778">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="c8758-779">在建置主機時，呼叫 `ConfigureAppConfiguration` 以指定檔案 (除了 *appsettings.json* 和  *appsettings.{Environment}.json* 以外) 的應用程式設定：</span><span class="sxs-lookup"><span data-stu-id="c8758-779">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="c8758-780">**範例**</span><span class="sxs-lookup"><span data-stu-id="c8758-780">**Example**</span></span>

<span data-ttu-id="c8758-781">範例應用程式會利用靜態便利方法 `CreateDefaultBuilder` 來建立主機，其中包括兩個 `AddJsonFile`呼叫：</span><span class="sxs-lookup"><span data-stu-id="c8758-781">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

* <span data-ttu-id="c8758-782">第一次呼叫 `AddJsonFile` 會從*appsettings*載入設定：</span><span class="sxs-lookup"><span data-stu-id="c8758-782">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="c8758-783">第二次呼叫 `AddJsonFile` 會從 appsettings 載入設定 *。 {環境}. json*。</span><span class="sxs-lookup"><span data-stu-id="c8758-783">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="c8758-784">適用于*appsettings。* 範例應用程式中的開發 json 會載入下列檔案：</span><span class="sxs-lookup"><span data-stu-id="c8758-784">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="c8758-785">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8758-785">Run the sample app.</span></span> <span data-ttu-id="c8758-786">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8758-786">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="c8758-787">輸出包含以應用程式環境為基礎之設定的索引鍵/值組。</span><span class="sxs-lookup"><span data-stu-id="c8758-787">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="c8758-788">在開發環境中執行應用程式時，會 `Debug` 金鑰 `Logging:LogLevel:Default` 的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="c8758-788">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="c8758-789">在生產環境中再次執行範例應用程式：</span><span class="sxs-lookup"><span data-stu-id="c8758-789">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="c8758-790">開啟*Properties/launchsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="c8758-790">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="c8758-791">在 `ConfigurationSample` 設定檔中，將 `ASPNETCORE_ENVIRONMENT` 環境變數的值變更為 [`Production`]。</span><span class="sxs-lookup"><span data-stu-id="c8758-791">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="c8758-792">儲存檔案，並使用命令 shell 中的 `dotnet run` 來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8758-792">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="c8758-793">Appsettings 中的設定 *。* 在*appsettings*中，不會再覆寫 json 中的設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-793">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="c8758-794">`Warning`金鑰 `Logging:LogLevel:Default` 的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="c8758-794">The log level for the key `Logging:LogLevel:Default` is `Warning`.</span></span>

### <a name="xml-configuration-provider"></a><span data-ttu-id="c8758-795">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-795">XML Configuration Provider</span></span>

<span data-ttu-id="c8758-796"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> 會在執行階段從 XML 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-796">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="c8758-797">若要啟用 XML 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="c8758-797">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="c8758-798">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="c8758-798">Overloads permit specifying:</span></span>

* <span data-ttu-id="c8758-799">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="c8758-799">Whether the file is optional.</span></span>
* <span data-ttu-id="c8758-800">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-800">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="c8758-801"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="c8758-801">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="c8758-802">建立設定機碼值組時，會忽略設定檔案的根節點。</span><span class="sxs-lookup"><span data-stu-id="c8758-802">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="c8758-803">請勿在檔案中指定文件類型定義 (DTD) 或命名空間。</span><span class="sxs-lookup"><span data-stu-id="c8758-803">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="c8758-804">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="c8758-804">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="c8758-805">XML 設定檔可以針對重複的區段使用相異元素名稱：</span><span class="sxs-lookup"><span data-stu-id="c8758-805">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="c8758-806">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="c8758-806">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="c8758-807">section0:key0</span><span class="sxs-lookup"><span data-stu-id="c8758-807">section0:key0</span></span>
* <span data-ttu-id="c8758-808">section0:key1</span><span class="sxs-lookup"><span data-stu-id="c8758-808">section0:key1</span></span>
* <span data-ttu-id="c8758-809">section1:key0</span><span class="sxs-lookup"><span data-stu-id="c8758-809">section1:key0</span></span>
* <span data-ttu-id="c8758-810">section1:key1</span><span class="sxs-lookup"><span data-stu-id="c8758-810">section1:key1</span></span>

<span data-ttu-id="c8758-811">若 `name` 屬性是用來區別元素，則可以重複那些使用相同元素名稱的元素：</span><span class="sxs-lookup"><span data-stu-id="c8758-811">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="c8758-812">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="c8758-812">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="c8758-813">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="c8758-813">section:section0:key:key0</span></span>
* <span data-ttu-id="c8758-814">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="c8758-814">section:section0:key:key1</span></span>
* <span data-ttu-id="c8758-815">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="c8758-815">section:section1:key:key0</span></span>
* <span data-ttu-id="c8758-816">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="c8758-816">section:section1:key:key1</span></span>

<span data-ttu-id="c8758-817">屬性可用來提供值：</span><span class="sxs-lookup"><span data-stu-id="c8758-817">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="c8758-818">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="c8758-818">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="c8758-819">key:attribute</span><span class="sxs-lookup"><span data-stu-id="c8758-819">key:attribute</span></span>
* <span data-ttu-id="c8758-820">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="c8758-820">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="c8758-821">每個檔案機碼設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-821">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="c8758-822"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> 使用目錄的檔案做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="c8758-822">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="c8758-823">機碼是檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="c8758-823">The key is the file name.</span></span> <span data-ttu-id="c8758-824">值包含檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="c8758-824">The value contains the file's contents.</span></span> <span data-ttu-id="c8758-825">每個檔案機碼設定提供者是在 Docker 主機案例中使用。</span><span class="sxs-lookup"><span data-stu-id="c8758-825">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="c8758-826">若要啟用每個檔案機碼設定，請在 <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="c8758-826">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="c8758-827">檔案的 `directoryPath` 必須是絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="c8758-827">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="c8758-828">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="c8758-828">Overloads permit specifying:</span></span>

* <span data-ttu-id="c8758-829">設定來源的 `Action<KeyPerFileConfigurationSource>` 委派。</span><span class="sxs-lookup"><span data-stu-id="c8758-829">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="c8758-830">目錄是否為選擇性與目錄的路徑。</span><span class="sxs-lookup"><span data-stu-id="c8758-830">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="c8758-831">雙底線 (`__`) 是做為檔案名稱中的設定金鑰分隔符號使用。</span><span class="sxs-lookup"><span data-stu-id="c8758-831">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="c8758-832">例如，檔案名稱 `Logging__LogLevel__System` 會產生設定金鑰 `Logging:LogLevel:System`。</span><span class="sxs-lookup"><span data-stu-id="c8758-832">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="c8758-833">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="c8758-833">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="c8758-834">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-834">Memory Configuration Provider</span></span>

<span data-ttu-id="c8758-835"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> 使用記憶體內集合做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="c8758-835">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="c8758-836">若要啟用記憶體內集合設定，請在 <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="c8758-836">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="c8758-837">設定提供者可以使用 `IEnumerable<KeyValuePair<String,String>>` 來初始化。</span><span class="sxs-lookup"><span data-stu-id="c8758-837">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="c8758-838">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="c8758-838">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="c8758-839">下列範例會建立組態字典：</span><span class="sxs-lookup"><span data-stu-id="c8758-839">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="c8758-840">字典會與 `AddInMemoryCollection` 的呼叫搭配使用以提供組態：</span><span class="sxs-lookup"><span data-stu-id="c8758-840">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="c8758-841">GetValue</span><span class="sxs-lookup"><span data-stu-id="c8758-841">GetValue</span></span>

<span data-ttu-id="c8758-842">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*)會使用指定的索引鍵來解壓縮設定中的單一值，並將其轉換為指定的 noncollection 類型。</span><span class="sxs-lookup"><span data-stu-id="c8758-842">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="c8758-843">多載會接受預設值。</span><span class="sxs-lookup"><span data-stu-id="c8758-843">An overload accepts a default value.</span></span>

<span data-ttu-id="c8758-844">下列範例︰</span><span class="sxs-lookup"><span data-stu-id="c8758-844">The following example:</span></span>

* <span data-ttu-id="c8758-845">從具有機碼 `NumberKey` 的組態擷取字串值。</span><span class="sxs-lookup"><span data-stu-id="c8758-845">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="c8758-846">若在組態機碼中找不到 `NumberKey`，則會使用預設值 `99`。</span><span class="sxs-lookup"><span data-stu-id="c8758-846">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="c8758-847">鍵入值為 `int`。</span><span class="sxs-lookup"><span data-stu-id="c8758-847">Types the value as an `int`.</span></span>
* <span data-ttu-id="c8758-848">在 `NumberConfig` 屬性中儲存值供頁面使用。</span><span class="sxs-lookup"><span data-stu-id="c8758-848">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="c8758-849">GetSection、GetChildren 與 Exists</span><span class="sxs-lookup"><span data-stu-id="c8758-849">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="c8758-850">針對下面的範例，請考慮下列 JSON 檔案。</span><span class="sxs-lookup"><span data-stu-id="c8758-850">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="c8758-851">在兩個區段中找到四個機碼，其中一個包括子區段組：</span><span class="sxs-lookup"><span data-stu-id="c8758-851">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="c8758-852">當檔案被讀入到設定之後，會建立下列唯一階層式機碼以存放設定值：</span><span class="sxs-lookup"><span data-stu-id="c8758-852">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="c8758-853">section0:key0</span><span class="sxs-lookup"><span data-stu-id="c8758-853">section0:key0</span></span>
* <span data-ttu-id="c8758-854">section0:key1</span><span class="sxs-lookup"><span data-stu-id="c8758-854">section0:key1</span></span>
* <span data-ttu-id="c8758-855">section1:key0</span><span class="sxs-lookup"><span data-stu-id="c8758-855">section1:key0</span></span>
* <span data-ttu-id="c8758-856">section1:key1</span><span class="sxs-lookup"><span data-stu-id="c8758-856">section1:key1</span></span>
* <span data-ttu-id="c8758-857">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="c8758-857">section2:subsection0:key0</span></span>
* <span data-ttu-id="c8758-858">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="c8758-858">section2:subsection0:key1</span></span>
* <span data-ttu-id="c8758-859">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="c8758-859">section2:subsection1:key0</span></span>
* <span data-ttu-id="c8758-860">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="c8758-860">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="c8758-861">GetSection</span><span class="sxs-lookup"><span data-stu-id="c8758-861">GetSection</span></span>

<span data-ttu-id="c8758-862">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) 會擷取具有所指定子區段機碼的設定子區段。</span><span class="sxs-lookup"><span data-stu-id="c8758-862">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="c8758-863">若要傳回 <xref:Microsoft.Extensions.Configuration.IConfigurationSection> 中只包含一個機碼值組的 `section1`，請呼叫 `GetSection` 並提供區段名稱：</span><span class="sxs-lookup"><span data-stu-id="c8758-863">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="c8758-864">`configSection` 沒有值，只有索引鍵和路徑。</span><span class="sxs-lookup"><span data-stu-id="c8758-864">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="c8758-865">同樣地，若要取得 `section2:subsection0` 中之機碼的值，請呼叫 `GetSection` 並提供區段路徑：</span><span class="sxs-lookup"><span data-stu-id="c8758-865">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="c8758-866">`GetSection` 絕不會傳回 `null`。</span><span class="sxs-lookup"><span data-stu-id="c8758-866">`GetSection` never returns `null`.</span></span> <span data-ttu-id="c8758-867">若找不到相符的區段，會傳回空的 `IConfigurationSection`。</span><span class="sxs-lookup"><span data-stu-id="c8758-867">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="c8758-868">當 `GetSection` 傳回相符區段時，未填入 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value>。</span><span class="sxs-lookup"><span data-stu-id="c8758-868">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="c8758-869">當區段存在時，會傳回 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> 與 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path>。</span><span class="sxs-lookup"><span data-stu-id="c8758-869">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="c8758-870">GetChildren</span><span class="sxs-lookup"><span data-stu-id="c8758-870">GetChildren</span></span>

<span data-ttu-id="c8758-871">對 [ 上之 ](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*)IConfiguration.GetChildren`section2` 的呼叫會取得包括下列項目的 `IEnumerable<IConfigurationSection>`：</span><span class="sxs-lookup"><span data-stu-id="c8758-871">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="c8758-872">存在</span><span class="sxs-lookup"><span data-stu-id="c8758-872">Exists</span></span>

<span data-ttu-id="c8758-873">使用 [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) 來判斷設定區段是否存在：</span><span class="sxs-lookup"><span data-stu-id="c8758-873">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="c8758-874">以範例資料為例， `sectionExists` 是 `false`，因未設定資料中沒有 `section2:subsection2` 區段。</span><span class="sxs-lookup"><span data-stu-id="c8758-874">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="c8758-875">繫結至物件圖形</span><span class="sxs-lookup"><span data-stu-id="c8758-875">Bind to an object graph</span></span>

<span data-ttu-id="c8758-876"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 可以繫結整個 POCO 物件圖形。</span><span class="sxs-lookup"><span data-stu-id="c8758-876"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="c8758-877">如同系結簡單的物件，只會系結公用讀取/寫入屬性。</span><span class="sxs-lookup"><span data-stu-id="c8758-877">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="c8758-878">範例包含 `TvShow` 模型，其物件圖形包括 `Metadata` 與 `Actors` 類別 (*Models/TvShow.cs*)：</span><span class="sxs-lookup"><span data-stu-id="c8758-878">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="c8758-879">範例應用程式有 *tvshow.xml* 檔案，其中包含設定資料：</span><span class="sxs-lookup"><span data-stu-id="c8758-879">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="c8758-880">設定會被繫結到整個 `TvShow` 物件 (使用 `Bind` 方法)。</span><span class="sxs-lookup"><span data-stu-id="c8758-880">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="c8758-881">已繫結的執行個體會被指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="c8758-881">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="c8758-882">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*)系結並傳回指定的型別。</span><span class="sxs-lookup"><span data-stu-id="c8758-882">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="c8758-883">`Get<T>` 比使用 `Bind` 更方便。</span><span class="sxs-lookup"><span data-stu-id="c8758-883">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="c8758-884">下列程式碼示範如何使用上述範例中的 `Get<T>`：</span><span class="sxs-lookup"><span data-stu-id="c8758-884">The following code shows how to use `Get<T>` with the preceding example:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="c8758-885">將陣列繫結到類別</span><span class="sxs-lookup"><span data-stu-id="c8758-885">Bind an array to a class</span></span>

<span data-ttu-id="c8758-886">範例應用程式示範此節中解釋的概念。</span><span class="sxs-lookup"><span data-stu-id="c8758-886">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="c8758-887"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="c8758-887">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="c8758-888">公開數值索引鍵區段（`:0:`、`:1:`&hellip; `:{n}:`）的任何陣列格式，都能夠系結至 POCO 類別陣列。</span><span class="sxs-lookup"><span data-stu-id="c8758-888">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="c8758-889">繫結是由慣例提供。</span><span class="sxs-lookup"><span data-stu-id="c8758-889">Binding is provided by convention.</span></span> <span data-ttu-id="c8758-890">自訂設定提供者不需要實作陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="c8758-890">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="c8758-891">**記憶體內陣列處理**</span><span class="sxs-lookup"><span data-stu-id="c8758-891">**In-memory array processing**</span></span>

<span data-ttu-id="c8758-892">考慮下表中顯示的設定機碼與值。</span><span class="sxs-lookup"><span data-stu-id="c8758-892">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="c8758-893">Key</span><span class="sxs-lookup"><span data-stu-id="c8758-893">Key</span></span>             | <span data-ttu-id="c8758-894">值</span><span class="sxs-lookup"><span data-stu-id="c8758-894">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="c8758-895">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="c8758-895">array:entries:0</span></span> | <span data-ttu-id="c8758-896">value0</span><span class="sxs-lookup"><span data-stu-id="c8758-896">value0</span></span> |
| <span data-ttu-id="c8758-897">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="c8758-897">array:entries:1</span></span> | <span data-ttu-id="c8758-898">value1</span><span class="sxs-lookup"><span data-stu-id="c8758-898">value1</span></span> |
| <span data-ttu-id="c8758-899">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="c8758-899">array:entries:2</span></span> | <span data-ttu-id="c8758-900">value2</span><span class="sxs-lookup"><span data-stu-id="c8758-900">value2</span></span> |
| <span data-ttu-id="c8758-901">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="c8758-901">array:entries:4</span></span> | <span data-ttu-id="c8758-902">value4</span><span class="sxs-lookup"><span data-stu-id="c8758-902">value4</span></span> |
| <span data-ttu-id="c8758-903">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="c8758-903">array:entries:5</span></span> | <span data-ttu-id="c8758-904">value5</span><span class="sxs-lookup"><span data-stu-id="c8758-904">value5</span></span> |

<span data-ttu-id="c8758-905">這些機碼與值是使用「記憶體設定提供者」在範例應用程式中載入：</span><span class="sxs-lookup"><span data-stu-id="c8758-905">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

<span data-ttu-id="c8758-906">陣列會跳過索引 &num;3 的值。</span><span class="sxs-lookup"><span data-stu-id="c8758-906">The array skips a value for index &num;3.</span></span> <span data-ttu-id="c8758-907">設定繫結程式無法繫結 Null 值或在已繫結的物件中建立 Null 項目，這在示範繫結此陣列的結果到某物件的時候已經很清楚。</span><span class="sxs-lookup"><span data-stu-id="c8758-907">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="c8758-908">在範例應用程式中，POCO 類別可用來存放繫結設定資料：</span><span class="sxs-lookup"><span data-stu-id="c8758-908">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="c8758-909">設定資料已繫結到物件：</span><span class="sxs-lookup"><span data-stu-id="c8758-909">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="c8758-910">您也可以使用[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*)語法，這會產生更精簡的程式碼：</span><span class="sxs-lookup"><span data-stu-id="c8758-910">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="c8758-911">繫結物件 (`ArrayExample`的執行個體) 會從設定接收陣列資料。</span><span class="sxs-lookup"><span data-stu-id="c8758-911">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="c8758-912">`ArrayExample.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="c8758-912">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="c8758-913">`ArrayExample.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="c8758-913">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="c8758-914">0</span><span class="sxs-lookup"><span data-stu-id="c8758-914">0</span></span>                            | <span data-ttu-id="c8758-915">value0</span><span class="sxs-lookup"><span data-stu-id="c8758-915">value0</span></span>                       |
| <span data-ttu-id="c8758-916">1</span><span class="sxs-lookup"><span data-stu-id="c8758-916">1</span></span>                            | <span data-ttu-id="c8758-917">value1</span><span class="sxs-lookup"><span data-stu-id="c8758-917">value1</span></span>                       |
| <span data-ttu-id="c8758-918">2</span><span class="sxs-lookup"><span data-stu-id="c8758-918">2</span></span>                            | <span data-ttu-id="c8758-919">value2</span><span class="sxs-lookup"><span data-stu-id="c8758-919">value2</span></span>                       |
| <span data-ttu-id="c8758-920">3</span><span class="sxs-lookup"><span data-stu-id="c8758-920">3</span></span>                            | <span data-ttu-id="c8758-921">value4</span><span class="sxs-lookup"><span data-stu-id="c8758-921">value4</span></span>                       |
| <span data-ttu-id="c8758-922">4</span><span class="sxs-lookup"><span data-stu-id="c8758-922">4</span></span>                            | <span data-ttu-id="c8758-923">value5</span><span class="sxs-lookup"><span data-stu-id="c8758-923">value5</span></span>                       |

<span data-ttu-id="c8758-924">繫結物件中的索引 &num;3 存放 `array:4` 設定機碼與其 `value4` 的設定資料。</span><span class="sxs-lookup"><span data-stu-id="c8758-924">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="c8758-925">當繫結包含陣列的設定資料時，設定機碼中的陣列索引只會用來列舉設定資料 (當建立物件時)。</span><span class="sxs-lookup"><span data-stu-id="c8758-925">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="c8758-926">設定資料中不能保留 Null 值，當設定機碼中的陣列略過一或多個索引時，不會在繫結物件中建立 Null 值項目。</span><span class="sxs-lookup"><span data-stu-id="c8758-926">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="c8758-927">由會在設定中產生正確機碼值組的任何設定提供者繫結到 &num; 執行個體之前，可以提供索引 `ArrayExample`3 缺少的設定項目。</span><span class="sxs-lookup"><span data-stu-id="c8758-927">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="c8758-928">若範例包含具有缺少之機碼值組的額外 JSON 設定提供者，`ArrayExample.Entries` 會符合完整的設定陣列：</span><span class="sxs-lookup"><span data-stu-id="c8758-928">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="c8758-929">*missing_value.json*：</span><span class="sxs-lookup"><span data-stu-id="c8758-929">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="c8758-930">在 `ConfigureAppConfiguration`中：</span><span class="sxs-lookup"><span data-stu-id="c8758-930">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="c8758-931">表格中顯示的機碼值組會載入到設定中。</span><span class="sxs-lookup"><span data-stu-id="c8758-931">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="c8758-932">Key</span><span class="sxs-lookup"><span data-stu-id="c8758-932">Key</span></span>             | <span data-ttu-id="c8758-933">值</span><span class="sxs-lookup"><span data-stu-id="c8758-933">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="c8758-934">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="c8758-934">array:entries:3</span></span> | <span data-ttu-id="c8758-935">value3</span><span class="sxs-lookup"><span data-stu-id="c8758-935">value3</span></span> |

<span data-ttu-id="c8758-936">在「JSON 設定提供者」包含索引 `ArrayExample`3 的項目之後，若繫結 &num; 類別執行個體，`ArrayExample.Entries` 陣列會包括這些值：</span><span class="sxs-lookup"><span data-stu-id="c8758-936">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="c8758-937">`ArrayExample.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="c8758-937">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="c8758-938">`ArrayExample.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="c8758-938">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="c8758-939">0</span><span class="sxs-lookup"><span data-stu-id="c8758-939">0</span></span>                            | <span data-ttu-id="c8758-940">value0</span><span class="sxs-lookup"><span data-stu-id="c8758-940">value0</span></span>                       |
| <span data-ttu-id="c8758-941">1</span><span class="sxs-lookup"><span data-stu-id="c8758-941">1</span></span>                            | <span data-ttu-id="c8758-942">value1</span><span class="sxs-lookup"><span data-stu-id="c8758-942">value1</span></span>                       |
| <span data-ttu-id="c8758-943">2</span><span class="sxs-lookup"><span data-stu-id="c8758-943">2</span></span>                            | <span data-ttu-id="c8758-944">value2</span><span class="sxs-lookup"><span data-stu-id="c8758-944">value2</span></span>                       |
| <span data-ttu-id="c8758-945">3</span><span class="sxs-lookup"><span data-stu-id="c8758-945">3</span></span>                            | <span data-ttu-id="c8758-946">value3</span><span class="sxs-lookup"><span data-stu-id="c8758-946">value3</span></span>                       |
| <span data-ttu-id="c8758-947">4</span><span class="sxs-lookup"><span data-stu-id="c8758-947">4</span></span>                            | <span data-ttu-id="c8758-948">value4</span><span class="sxs-lookup"><span data-stu-id="c8758-948">value4</span></span>                       |
| <span data-ttu-id="c8758-949">5</span><span class="sxs-lookup"><span data-stu-id="c8758-949">5</span></span>                            | <span data-ttu-id="c8758-950">value5</span><span class="sxs-lookup"><span data-stu-id="c8758-950">value5</span></span>                       |

<span data-ttu-id="c8758-951">**JSON 陣列處理**</span><span class="sxs-lookup"><span data-stu-id="c8758-951">**JSON array processing**</span></span>

<span data-ttu-id="c8758-952">若 JSON 檔案包含陣列，會為具有以零為基礎之區段索引的陣列元素建立設定機碼。</span><span class="sxs-lookup"><span data-stu-id="c8758-952">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="c8758-953">在下列設定檔中，`subsection` 是陣列：</span><span class="sxs-lookup"><span data-stu-id="c8758-953">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="c8758-954">「JSON 設定提供者」會將設定資料讀入到下列機碼值組：</span><span class="sxs-lookup"><span data-stu-id="c8758-954">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="c8758-955">Key</span><span class="sxs-lookup"><span data-stu-id="c8758-955">Key</span></span>                     | <span data-ttu-id="c8758-956">值</span><span class="sxs-lookup"><span data-stu-id="c8758-956">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="c8758-957">json_array:key</span><span class="sxs-lookup"><span data-stu-id="c8758-957">json_array:key</span></span>          | <span data-ttu-id="c8758-958">valueA</span><span class="sxs-lookup"><span data-stu-id="c8758-958">valueA</span></span> |
| <span data-ttu-id="c8758-959">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="c8758-959">json_array:subsection:0</span></span> | <span data-ttu-id="c8758-960">valueB</span><span class="sxs-lookup"><span data-stu-id="c8758-960">valueB</span></span> |
| <span data-ttu-id="c8758-961">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="c8758-961">json_array:subsection:1</span></span> | <span data-ttu-id="c8758-962">valueC</span><span class="sxs-lookup"><span data-stu-id="c8758-962">valueC</span></span> |
| <span data-ttu-id="c8758-963">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="c8758-963">json_array:subsection:2</span></span> | <span data-ttu-id="c8758-964">valueD</span><span class="sxs-lookup"><span data-stu-id="c8758-964">valueD</span></span> |

<span data-ttu-id="c8758-965">在範例應用程式中，下列 POCO 類別可用來繫結設定機碼值組：</span><span class="sxs-lookup"><span data-stu-id="c8758-965">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="c8758-966">在繫結之後，`JsonArrayExample.Key` 會存放值 `valueA`。</span><span class="sxs-lookup"><span data-stu-id="c8758-966">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="c8758-967">子區段值會存放在 POCO 陣列屬性 `Subsection` 中。</span><span class="sxs-lookup"><span data-stu-id="c8758-967">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="c8758-968">`JsonArrayExample.Subsection` 索引</span><span class="sxs-lookup"><span data-stu-id="c8758-968">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="c8758-969">`JsonArrayExample.Subsection` 值</span><span class="sxs-lookup"><span data-stu-id="c8758-969">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="c8758-970">0</span><span class="sxs-lookup"><span data-stu-id="c8758-970">0</span></span>                                   | <span data-ttu-id="c8758-971">valueB</span><span class="sxs-lookup"><span data-stu-id="c8758-971">valueB</span></span>                              |
| <span data-ttu-id="c8758-972">1</span><span class="sxs-lookup"><span data-stu-id="c8758-972">1</span></span>                                   | <span data-ttu-id="c8758-973">valueC</span><span class="sxs-lookup"><span data-stu-id="c8758-973">valueC</span></span>                              |
| <span data-ttu-id="c8758-974">2</span><span class="sxs-lookup"><span data-stu-id="c8758-974">2</span></span>                                   | <span data-ttu-id="c8758-975">valueD</span><span class="sxs-lookup"><span data-stu-id="c8758-975">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="c8758-976">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="c8758-976">Custom configuration provider</span></span>

<span data-ttu-id="c8758-977">範例應用程式示範如何建立會使用 [Entity Framework (EF)](/ef/core/) 從資料庫讀取設定機碼值組的基本設定提供者。</span><span class="sxs-lookup"><span data-stu-id="c8758-977">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="c8758-978">提供者具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="c8758-978">The provider has the following characteristics:</span></span>

* <span data-ttu-id="c8758-979">EF 記憶體內資料庫會用於示範用途。</span><span class="sxs-lookup"><span data-stu-id="c8758-979">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="c8758-980">若要使用需要連接字串的資料庫，請實作第二個 `ConfigurationBuilder` 以從另一個設定提供者提供連接字串。</span><span class="sxs-lookup"><span data-stu-id="c8758-980">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="c8758-981">啟動時，該提供者會將資料庫資料表讀入到設定中。</span><span class="sxs-lookup"><span data-stu-id="c8758-981">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="c8758-982">該提供者不會以個別機碼為基礎查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="c8758-982">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="c8758-983">未實作變更時重新載入，因此在應用程式啟動後更新資料庫對應用程式設定沒有影響。</span><span class="sxs-lookup"><span data-stu-id="c8758-983">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="c8758-984">定義 `EFConfigurationValue` 實體來在資料庫中存放設定值。</span><span class="sxs-lookup"><span data-stu-id="c8758-984">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="c8758-985">*Models/EFConfigurationValue.cs*：</span><span class="sxs-lookup"><span data-stu-id="c8758-985">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="c8758-986">新增 `EFConfigurationContext` 以存放及存取已設定的值。</span><span class="sxs-lookup"><span data-stu-id="c8758-986">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="c8758-987">*EFConfigurationProvider/EFConfigurationContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="c8758-987">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="c8758-988">建立會實作 <xref:Microsoft.Extensions.Configuration.IConfigurationSource> 的類別。</span><span class="sxs-lookup"><span data-stu-id="c8758-988">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="c8758-989">*EFConfigurationProvider/EFConfigurationSource.cs*：</span><span class="sxs-lookup"><span data-stu-id="c8758-989">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="c8758-990">透過繼承自 <xref:Microsoft.Extensions.Configuration.ConfigurationProvider> 來建立自訂設定提供者。</span><span class="sxs-lookup"><span data-stu-id="c8758-990">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="c8758-991">若資料庫是空的，設定提供者會初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="c8758-991">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="c8758-992">*EFConfigurationProvider/EFConfigurationProvider.cs*：</span><span class="sxs-lookup"><span data-stu-id="c8758-992">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="c8758-993">`AddEFConfiguration` 擴充方法允許新增設定來源到 `ConfigurationBuilder`。</span><span class="sxs-lookup"><span data-stu-id="c8758-993">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="c8758-994">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="c8758-994">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="c8758-995">下列程式碼顯示如何在 `EFConfigurationProvider`Program.cs*中使用自訂*：</span><span class="sxs-lookup"><span data-stu-id="c8758-995">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="c8758-996">在啟動期間存取組態</span><span class="sxs-lookup"><span data-stu-id="c8758-996">Access configuration during startup</span></span>

<span data-ttu-id="c8758-997">將 `IConfiguration` 插入到 `Startup` 建構函式，以存取 `Startup.ConfigureServices` 中的設定值。</span><span class="sxs-lookup"><span data-stu-id="c8758-997">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c8758-998">若要存取 `Startup.Configure` 中的設定，請直接將 `IConfiguration` 插入到方法或從建構函式使用執行個體：</span><span class="sxs-lookup"><span data-stu-id="c8758-998">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="c8758-999">如需使用啟動方便方法來存取設定的範例，請參閱[應用程式啟動：方便方法](xref:fundamentals/startup#convenience-methods)。</span><span class="sxs-lookup"><span data-stu-id="c8758-999">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="c8758-1000">存取 Razor Pages 頁面或 MVC 檢視中的設定</span><span class="sxs-lookup"><span data-stu-id="c8758-1000">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="c8758-1001">若要在 [Razor Pages 頁面] 頁面或 MVC 檢視中存取組態設定，請針對 [Microsoft.Extensions.Configuration 命名空間](xref:mvc/views/razor#using)[using 指示詞](/dotnet/csharp/language-reference/keywords/using-directive) ([C# 參考：using 指示詞](xref:Microsoft.Extensions.Configuration))，並將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 插入到頁面或檢視中。</span><span class="sxs-lookup"><span data-stu-id="c8758-1001">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="c8758-1002">在 [Razor 頁面] 頁面中：</span><span class="sxs-lookup"><span data-stu-id="c8758-1002">In a Razor Pages page:</span></span>

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

<span data-ttu-id="c8758-1003">在 MVC 檢視中：</span><span class="sxs-lookup"><span data-stu-id="c8758-1003">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="c8758-1004">從外部組件新增設定</span><span class="sxs-lookup"><span data-stu-id="c8758-1004">Add configuration from an external assembly</span></span>

<span data-ttu-id="c8758-1005"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。</span><span class="sxs-lookup"><span data-stu-id="c8758-1005">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="c8758-1006">如需詳細資訊，請參閱<xref:fundamentals/configuration/platform-specific-configuration>。</span><span class="sxs-lookup"><span data-stu-id="c8758-1006">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c8758-1007">其他資源</span><span class="sxs-lookup"><span data-stu-id="c8758-1007">Additional resources</span></span>

* <xref:fundamentals/configuration/options>

::: moniker-end
