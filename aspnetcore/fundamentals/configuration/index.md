---
title: ASP.NET Core 的設定
author: rick-anderson
description: 了解如何使用組態 API 設定 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 3/29/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/configuration/index
ms.openlocfilehash: 6e47e627915bd8988d161f7d5af4a89f3671c0a7
ms.sourcegitcommit: 384833762c614851db653b841cc09fbc944da463
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86445446"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="1c2b0-103">ASP.NET Core 的設定</span><span class="sxs-lookup"><span data-stu-id="1c2b0-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="1c2b0-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Kirk Larkin](https://twitter.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="1c2b0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Kirk Larkin](https://twitter.com/serpent5)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1c2b0-105">ASP.NET Core 中的設定是使用一或多個設定[提供者](#cp)來執行。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-105">Configuration in ASP.NET Core is performed using one or more [configuration providers](#cp).</span></span> <span data-ttu-id="1c2b0-106">設定提供者會使用各種不同的設定來源，從機碼值組讀取設定資料：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-106">Configuration providers read configuration data from key-value pairs using a variety of configuration sources:</span></span>

* <span data-ttu-id="1c2b0-107">設定檔案，例如*appsettings.js*</span><span class="sxs-lookup"><span data-stu-id="1c2b0-107">Settings files, such as *appsettings.json*</span></span>
* <span data-ttu-id="1c2b0-108">環境變數</span><span class="sxs-lookup"><span data-stu-id="1c2b0-108">Environment variables</span></span>
* <span data-ttu-id="1c2b0-109">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="1c2b0-109">Azure Key Vault</span></span>
* <span data-ttu-id="1c2b0-110">Azure 應用程式組態</span><span class="sxs-lookup"><span data-stu-id="1c2b0-110">Azure App Configuration</span></span>
* <span data-ttu-id="1c2b0-111">命令列引數</span><span class="sxs-lookup"><span data-stu-id="1c2b0-111">Command-line arguments</span></span>
* <span data-ttu-id="1c2b0-112">已安裝或建立的自訂提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-112">Custom providers, installed or created</span></span>
* <span data-ttu-id="1c2b0-113">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="1c2b0-113">Directory files</span></span>
* <span data-ttu-id="1c2b0-114">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="1c2b0-114">In-memory .NET objects</span></span>

<span data-ttu-id="1c2b0-115">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="1c2b0-115">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<a name="default"></a>

## <a name="default-configuration"></a><span data-ttu-id="1c2b0-116">預設組態</span><span class="sxs-lookup"><span data-stu-id="1c2b0-116">Default configuration</span></span>

<span data-ttu-id="1c2b0-117">ASP.NET Core 以[dotnet new](/dotnet/core/tools/dotnet-new)或 Visual Studio 建立的 web 應用程式會產生下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-117">ASP.NET Core web apps created with [dotnet new](/dotnet/core/tools/dotnet-new) or Visual Studio generate the following code:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Program.cs?name=snippet&highlight=9)]

 <span data-ttu-id="1c2b0-118"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> 會以下列順序提供應用程式的預設組態：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-118"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> provides default configuration for the app in the following order:</span></span>

1. <span data-ttu-id="1c2b0-119">[ChainedConfigurationProvider](xref:Microsoft.Extensions.Configuration.ChainedConfigurationSource) ：加入現有的 `IConfiguration` 做為來源。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-119">[ChainedConfigurationProvider](xref:Microsoft.Extensions.Configuration.ChainedConfigurationSource) :  Adds an existing `IConfiguration` as a source.</span></span> <span data-ttu-id="1c2b0-120">在預設設定案例中，會新增[主機](#hvac)配置，並將其設為_應用程式_設定的第一個來源。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-120">In the default configuration case, adds the [host](#hvac) configuration and setting it as the first source for the _app_ configuration.</span></span>
1. <span data-ttu-id="1c2b0-121">[appsettings.js](#appsettingsjson)使用 JSON 設定[提供者](#file-configuration-provider)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-121">[appsettings.json](#appsettingsjson) using the [JSON configuration provider](#file-configuration-provider).</span></span>
1. <span data-ttu-id="1c2b0-122">*appsettings。* `Environment`使用[json 設定提供者](#file-configuration-provider)的*json* 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-122">*appsettings.*`Environment`*.json* using the [JSON configuration provider](#file-configuration-provider).</span></span> <span data-ttu-id="1c2b0-123">例如， *appsettings*。***生產***。*json*和*appsettings*。***開發***。*json*。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-123">For example, *appsettings*.***Production***.*json* and *appsettings*.***Development***.*json*.</span></span>
1. <span data-ttu-id="1c2b0-124">應用程式在環境中執行時的[密碼](xref:security/app-secrets) `Development` 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-124">[App secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
1. <span data-ttu-id="1c2b0-125">使用[環境變數設定提供者](#evcp)的環境變數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-125">Environment variables using the [Environment Variables configuration provider](#evcp).</span></span>
1. <span data-ttu-id="1c2b0-126">使用[命令列設定提供者](#command-line)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-126">Command-line arguments using the [Command-line configuration provider](#command-line).</span></span>

<span data-ttu-id="1c2b0-127">稍後新增的設定提供者會覆寫先前的金鑰設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-127">Configuration providers that are added later override previous key settings.</span></span> <span data-ttu-id="1c2b0-128">例如，如果 `MyKey` 同時在和環境的*appsettings.js*中設定，則會使用環境值。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-128">For example, if `MyKey` is set in both *appsettings.json* and the environment, the environment value is used.</span></span> <span data-ttu-id="1c2b0-129">使用預設的設定提供者時，[命令列設定提供者](#clcp)會覆寫所有其他提供者。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-129">Using the default configuration providers, the  [Command-line configuration provider](#clcp) overrides all other providers.</span></span>

<span data-ttu-id="1c2b0-130">如需的詳細資訊 `CreateDefaultBuilder` ，請參閱預設產生器[設定](xref:fundamentals/host/generic-host#default-builder-settings)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-130">For more information on `CreateDefaultBuilder`, see [Default builder settings](xref:fundamentals/host/generic-host#default-builder-settings).</span></span>

<span data-ttu-id="1c2b0-131">下列程式碼會依新增的順序顯示已啟用的設定提供者：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-131">The following code displays the enabled configuration providers in the order they were added:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Index2.cshtml.cs?name=snippet)]

### <a name="appsettingsjson"></a><span data-ttu-id="1c2b0-132">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="1c2b0-132">appsettings.json</span></span>

<span data-ttu-id="1c2b0-133">請考慮下列*appsettings.js*檔案：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-133">Consider the following *appsettings.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/appsettings.json)]

<span data-ttu-id="1c2b0-134">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)中的下列程式碼會顯示上述幾個設定值：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-134">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="1c2b0-135">預設會 <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> 以下列順序載入設定：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-135">The default <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration in the following order:</span></span>

1. <span data-ttu-id="1c2b0-136">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="1c2b0-136">*appsettings.json*</span></span>
1. <span data-ttu-id="1c2b0-137">*appsettings。* `Environment`*. json* ：例如， *appsettings*。***生產***。*json*和*appsettings*。***開發***。*json*檔案。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-137">*appsettings.*`Environment`*.json* : For example, the *appsettings*.***Production***.*json* and *appsettings*.***Development***.*json* files.</span></span> <span data-ttu-id="1c2b0-138">檔案的環境版本是根據[IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)載入。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-138">The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span> <span data-ttu-id="1c2b0-139">如需詳細資訊，請參閱 <xref:fundamentals/environments> 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-139">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="1c2b0-140">*appsettings*。 `Environment`*json*值會覆寫中*appsettings.js的*索引鍵。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-140">*appsettings*.`Environment`.*json* values override keys in *appsettings.json*.</span></span> <span data-ttu-id="1c2b0-141">例如，根據預設：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-141">For example, by default:</span></span>

* <span data-ttu-id="1c2b0-142">在開發中， *appsettings*。***開發***。*json*設定會覆寫在*appsettings.js*中找到的值。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-142">In development, *appsettings*.***Development***.*json* configuration overwrites values found in *appsettings.json*.</span></span>
* <span data-ttu-id="1c2b0-143">在生產環境中， *appsettings*。***生產***。*json*設定會覆寫在*appsettings.js*中找到的值。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-143">In production, *appsettings*.***Production***.*json* configuration overwrites values found in *appsettings.json*.</span></span> <span data-ttu-id="1c2b0-144">例如，將應用程式部署至 Azure 時。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-144">For example, when deploying the app to Azure.</span></span>

<a name="optpat"></a>

### <a name="bind-hierarchical-configuration-data-using-the-options-pattern"></a><span data-ttu-id="1c2b0-145">使用選項模式系結階層式設定資料</span><span class="sxs-lookup"><span data-stu-id="1c2b0-145">Bind hierarchical configuration data using the options pattern</span></span>

[!INCLUDE[](~/includes/bind.md)]

<span data-ttu-id="1c2b0-146">使用[預設](#default)設定， *appsettings.json*和*appsettings。* `Environment`已啟用[reloadOnChange： true](https://github.com/dotnet/extensions/blob/release/3.1/src/Hosting/Hosting/src/Host.cs#L74-L75)的*json*檔案。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-146">Using the [default](#default) configuration, the *appsettings.json* and *appsettings.*`Environment`*.json* files are enabled with [reloadOnChange: true](https://github.com/dotnet/extensions/blob/release/3.1/src/Hosting/Hosting/src/Host.cs#L74-L75).</span></span> <span data-ttu-id="1c2b0-147">和 appsettings 上對*appsettings.js*所做的變更 *。* `Environment`應用程式啟動***後***的*json*檔案會由 json 設定[提供者](#jcp)讀取。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-147">Changes made to the *appsettings.json* and *appsettings.*`Environment`*.json* file ***after*** the app starts are read by the [JSON configuration provider](#jcp).</span></span>

<span data-ttu-id="1c2b0-148">如需新增其他 JSON 設定檔的相關資訊，請參閱本檔中的[JSON 設定提供者](#jcp)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-148">See [JSON configuration provider](#jcp) in this document for information on adding additional JSON configuration files.</span></span>

<a name="security"></a>

## <a name="security-and-secret-manager"></a><span data-ttu-id="1c2b0-149">安全性和秘密管理員</span><span class="sxs-lookup"><span data-stu-id="1c2b0-149">Security and secret manager</span></span>

<span data-ttu-id="1c2b0-150">設定資料方針：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-150">Configuration data guidelines:</span></span>

* <span data-ttu-id="1c2b0-151">永遠不要將密碼或其他敏感性資料儲存在設定提供者程式碼或純文字設定檔中。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-151">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="1c2b0-152">[秘密管理員](xref:security/app-secrets)可以用來將秘密儲存在開發中。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-152">The [Secret manager](xref:security/app-secrets) can be used to store secrets in development.</span></span>
* <span data-ttu-id="1c2b0-153">不要在開發或測試環境中使用生產環境祕密。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-153">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="1c2b0-154">請在專案外部指定祕密，以防止其意外認可至開放原始碼存放庫。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-154">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="1c2b0-155">根據[預設](#default)，[秘密管理員](xref:security/app-secrets)會*在appsettings.js*和 appsettings 之後讀取設定 *。* `Environment`*. json*。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-155">By [default](#default), [Secret manager](xref:security/app-secrets) reads configuration settings after *appsettings.json* and *appsettings.*`Environment`*.json*.</span></span>

<span data-ttu-id="1c2b0-156">如需儲存密碼或其他機密資料的詳細資訊：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-156">For more information on storing passwords or other sensitive data:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="1c2b0-157"><xref:security/app-secrets>：包含有關使用環境變數來儲存敏感性資料的建議。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-157"><xref:security/app-secrets>:  Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="1c2b0-158">秘密管理員會使用檔案設定[提供者](#fcp)，將使用者秘密儲存在本機系統上的 JSON 檔案中。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-158">The Secret Manager uses the [File configuration provider](#fcp) to store user secrets in a JSON file on the local system.</span></span>

<span data-ttu-id="1c2b0-159">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 可安全地儲存 ASP.NET Core 應用程式的應用程式祕密。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-159">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="1c2b0-160">如需詳細資訊，請參閱 <xref:security/key-vault-configuration> 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-160">For more information, see <xref:security/key-vault-configuration>.</span></span>

<a name="evcp"></a>

## <a name="environment-variables"></a><span data-ttu-id="1c2b0-161">環境變數</span><span class="sxs-lookup"><span data-stu-id="1c2b0-161">Environment variables</span></span>

<span data-ttu-id="1c2b0-162">使用[預設](#default)設定時，會在 <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> *appsettings.js*讀取 appsettings 之後，從環境變數的機碼值組載入設定 *。* `Environment`*. json*和[密碼管理員](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-162">Using the [default](#default) configuration, the <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs after reading *appsettings.json*, *appsettings.*`Environment`*.json*, and [Secret manager](xref:security/app-secrets).</span></span> <span data-ttu-id="1c2b0-163">因此，從環境中讀取的索引鍵值會覆寫從*appsettings.js*讀取的值，也就是*appsettings。* `Environment`*. json*和密碼管理員。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-163">Therefore, key values read from the environment override values read from *appsettings.json*, *appsettings.*`Environment`*.json*, and Secret manager.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="1c2b0-164">下列 `set` 命令：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-164">The following `set` commands:</span></span>

* <span data-ttu-id="1c2b0-165">在 Windows 上設定[上述範例](#appsettingsjson)的環境索引鍵和值。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-165">Set the environment keys and values of the [preceding example](#appsettingsjson) on Windows.</span></span>
* <span data-ttu-id="1c2b0-166">使用[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)時測試設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-166">Test the settings when using the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample).</span></span> <span data-ttu-id="1c2b0-167">`dotnet run`命令必須在專案目錄中執行。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-167">The `dotnet run` command must be run in the project directory.</span></span>

```dotnetcli
set MyKey="My key from Environment"
set Position__Title=Environment_Editor
set Position__Name=Environment_Rick
dotnet run
```

<span data-ttu-id="1c2b0-168">先前的環境設定：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-168">The preceding environment settings:</span></span>

* <span data-ttu-id="1c2b0-169">只會在從其設定所在的命令視窗中啟動的進程中設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-169">Are only set in processes launched from the command window they were set in.</span></span>
* <span data-ttu-id="1c2b0-170">以 Visual Studio 啟動的瀏覽器將不會讀取。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-170">Won't be read by browsers launched with Visual Studio.</span></span>

<span data-ttu-id="1c2b0-171">下列[setx](/windows-server/administration/windows-commands/setx)命令可以用來設定 Windows 上的環境索引鍵和值。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-171">The following [setx](/windows-server/administration/windows-commands/setx) commands can be used to set the environment keys and values on Windows.</span></span> <span data-ttu-id="1c2b0-172">不同于 `set` ， `setx` 設定會保存下來。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-172">Unlike `set`, `setx` settings are persisted.</span></span> <span data-ttu-id="1c2b0-173">`/M`設定系統內容中的變數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-173">`/M` sets the variable in the system environment.</span></span> <span data-ttu-id="1c2b0-174">如果 `/M` 未使用參數，則會設定使用者環境變數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-174">If the `/M` switch isn't used, a user environment variable is set.</span></span>

```cmd
setx MyKey "My key from setx Environment" /M
setx Position__Title Setx_Environment_Editor /M
setx Position__Name Environment_Rick /M
```

<span data-ttu-id="1c2b0-175">若要測試先前的命令是否會覆寫和 appsettings*上的appsettings.js* *。* `Environment`*. json*：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-175">To test that the preceding commands override *appsettings.json* and *appsettings.*`Environment`*.json*:</span></span>

* <span data-ttu-id="1c2b0-176">使用 Visual Studio： [結束] 和 [重新開機] Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-176">With Visual Studio: Exit and restart Visual Studio.</span></span>
* <span data-ttu-id="1c2b0-177">使用 CLI：啟動新的命令視窗，然後輸入 `dotnet run` 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-177">With the CLI: Start a new command window and enter `dotnet run`.</span></span>

<span data-ttu-id="1c2b0-178"><xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*>使用字串呼叫以指定環境變數的前置詞：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-178">Call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> with a string to specify a prefix for environment variables:</span></span>

[!code-csharp[](~/fundamentals/configuration/index/samples/3.x/ConfigSample/Program.cs?name=snippet4&highlight=12)]

<span data-ttu-id="1c2b0-179">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-179">In the preceding code:</span></span>

* <span data-ttu-id="1c2b0-180">`config.AddEnvironmentVariables(prefix: "MyCustomPrefix_")`會在預設的設定[提供者](#default)之後加入。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-180">`config.AddEnvironmentVariables(prefix: "MyCustomPrefix_")` is added after the [default configuration providers](#default).</span></span> <span data-ttu-id="1c2b0-181">如需排序設定提供者的範例，請參閱[JSON 設定提供者](#jcp)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-181">For an example of ordering the configuration providers, see [JSON configuration provider](#jcp).</span></span>
* <span data-ttu-id="1c2b0-182">以前置詞設定的環境變數會覆 `MyCustomPrefix_` 寫預設的設定[提供者](#default)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-182">Environment variables set with the `MyCustomPrefix_` prefix override the [default configuration providers](#default).</span></span> <span data-ttu-id="1c2b0-183">這包括不含前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-183">This includes environment variables without the prefix.</span></span>

<span data-ttu-id="1c2b0-184">讀取設定機碼值組時，會去除前置詞。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-184">The prefix is stripped off when the configuration key-value pairs are read.</span></span>

<span data-ttu-id="1c2b0-185">下列命令會測試自訂前置詞：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-185">The following commands test the custom prefix:</span></span>

```dotnetcli
set MyCustomPrefix_MyKey="My key with MyCustomPrefix_ Environment"
set MyCustomPrefix_Position__Title=Editor_with_customPrefix
set MyCustomPrefix_Position__Name=Environment_Rick_cp
dotnet run
```

<span data-ttu-id="1c2b0-186">[預設](#default)設定會載入前面加上和的環境變數和命令列引數 `DOTNET_` `ASPNETCORE_` 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-186">The [default configuration](#default) loads environment variables and command line arguments prefixed with `DOTNET_` and `ASPNETCORE_`.</span></span> <span data-ttu-id="1c2b0-187">和前置詞 `DOTNET_` `ASPNETCORE_` 是由[主機和應用程式](xref:fundamentals/host/generic-host#host-configuration)設定的 ASP.NET Core 所使用，但不適用於使用者設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-187">The `DOTNET_` and `ASPNETCORE_` prefixes are used by ASP.NET Core for [host and app configuration](xref:fundamentals/host/generic-host#host-configuration), but not for user configuration.</span></span> <span data-ttu-id="1c2b0-188">如需主機和應用程式設定的詳細資訊，請參閱[.Net 泛型主機](xref:fundamentals/host/generic-host)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-188">For more information on host and app configuration, see [.NET Generic Host](xref:fundamentals/host/generic-host).</span></span>

<span data-ttu-id="1c2b0-189">在[Azure App Service](https://azure.microsoft.com/services/app-service/)上，選取 [**設定] > 設定**] 頁面上的 [**新增應用程式設定**]。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-189">On [Azure App Service](https://azure.microsoft.com/services/app-service/), select **New application setting** on the **Settings > Configuration** page.</span></span> <span data-ttu-id="1c2b0-190">Azure App Service 的應用程式設定如下：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-190">Azure App Service application settings are:</span></span>

* <span data-ttu-id="1c2b0-191">待用加密，並透過加密通道傳輸。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-191">Encrypted at rest and transmitted over an encrypted channel.</span></span>
* <span data-ttu-id="1c2b0-192">公開為環境變數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-192">Exposed as environment variables.</span></span>

<span data-ttu-id="1c2b0-193">如需詳細資訊，請參閱 [Azure App：使用 Azure 入口網站覆寫應用程式設定](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-193">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="1c2b0-194">如需 Azure 資料庫連接字串的相關資訊，請參閱[連接字串](#constr)前置詞。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-194">See [Connection string prefixes](#constr) for information on Azure database connection strings.</span></span>

### <a name="environment-variables-set-in-launchsettingsjson"></a><span data-ttu-id="1c2b0-195">在 launchSettings.js中設定的環境變數</span><span class="sxs-lookup"><span data-stu-id="1c2b0-195">Environment variables set in launchSettings.json</span></span>

<span data-ttu-id="1c2b0-196">在*launchSettings.js*中設定的環境變數會覆寫系統內容中的設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-196">Environment variables set in *launchSettings.json* override those set in the system environment.</span></span>

<a name="clcp"></a>

## <a name="command-line"></a><span data-ttu-id="1c2b0-197">命令列</span><span class="sxs-lookup"><span data-stu-id="1c2b0-197">Command-line</span></span>

<span data-ttu-id="1c2b0-198">使用[預設](#default)設定時，會在 <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> 下列設定來源之後，從命令列引數的機碼值組載入設定：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-198">Using the [default](#default) configuration, the <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs after the following configuration sources:</span></span>

* <span data-ttu-id="1c2b0-199">*appsettings.json*和*appsettings*. `Environment` 。*json*檔案。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-199">*appsettings.json* and *appsettings*.`Environment`.*json* files.</span></span>
* <span data-ttu-id="1c2b0-200">開發環境中的[應用程式秘密（秘密管理員）](xref:security/app-secrets) 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-200">[App secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="1c2b0-201">環境變數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-201">Environment variables.</span></span>

<span data-ttu-id="1c2b0-202">根據[預設](#default)，在命令列上設定的設定值會覆寫所有其他設定提供者所設定的設定值。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-202">By [default](#default), configuration values set on the command-line override configuration values set with all the other configuration providers.</span></span>

### <a name="command-line-arguments"></a><span data-ttu-id="1c2b0-203">命令列引數</span><span class="sxs-lookup"><span data-stu-id="1c2b0-203">Command-line arguments</span></span>

<span data-ttu-id="1c2b0-204">下列命令會使用來設定索引鍵和值 `=` ：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-204">The following command sets keys and values using `=`:</span></span>

```dotnetcli
dotnet run MyKey="My key from command line" Position:Title=Cmd Position:Name=Cmd_Rick
```

<span data-ttu-id="1c2b0-205">下列命令會使用來設定索引鍵和值 `/` ：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-205">The following command sets keys and values using `/`:</span></span>

```dotnetcli
dotnet run /MyKey "Using /" /Position:Title=Cmd_ /Position:Name=Cmd_Rick
```

<span data-ttu-id="1c2b0-206">下列命令會使用來設定索引鍵和值 `--` ：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-206">The following command sets keys and values using `--`:</span></span>

```dotnetcli
dotnet run --MyKey "Using --" --Position:Title=Cmd-- --Position:Name=Cmd--Rick
```

<span data-ttu-id="1c2b0-207">索引鍵值：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-207">The key value:</span></span>

* <span data-ttu-id="1c2b0-208">必須遵循 `=` ，或者 `--` `/` 當值在空格後面時，索引鍵必須有或的前置詞。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-208">Must follow `=`, or the key must have a prefix of `--` or `/` when the value follows a space.</span></span>
* <span data-ttu-id="1c2b0-209">如果 `=` 使用，則不需要。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-209">Isn't required if `=` is used.</span></span> <span data-ttu-id="1c2b0-210">例如： `MySetting=` 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-210">For example, `MySetting=`.</span></span>

<span data-ttu-id="1c2b0-211">在相同的命令中，請不要混合使用搭配使用空格的機碼值組的命令列引數索引鍵/值配對 `=` 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-211">Within the same command, don't mix command-line argument key-value pairs that use `=` with key-value pairs that use a space.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="1c2b0-212">切換對應</span><span class="sxs-lookup"><span data-stu-id="1c2b0-212">Switch mappings</span></span>

<span data-ttu-id="1c2b0-213">交換器對應允許索引**鍵**名稱取代邏輯。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-213">Switch mappings allow **key** name replacement logic.</span></span> <span data-ttu-id="1c2b0-214">將參數取代的字典提供給 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 方法。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-214">Provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="1c2b0-215">使用切換對應字典時，會檢查字典中是否有任何索引鍵符合命令列引數所提供的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-215">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="1c2b0-216">如果在字典中找到命令列索引鍵，則會傳回字典值，將索引鍵/值組設為應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-216">If the command-line key is found in the dictionary, the dictionary value is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="1c2b0-217">所有前面加上單虛線 (`-`) 的命令列索引鍵都需要切換對應。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-217">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="1c2b0-218">切換對應字典索引鍵規則：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-218">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="1c2b0-219">參數的開頭必須是 `-` 或 `--` 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-219">Switches must start with `-` or `--`.</span></span>
* <span data-ttu-id="1c2b0-220">切換對應字典不能包含重複索引鍵。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-220">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="1c2b0-221">若要使用交換器對應字典，請將它傳遞至對的呼叫 `AddCommandLine` ：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-221">To use a switch mappings dictionary, pass it into the call to `AddCommandLine`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramSwitch.cs?name=snippet&highlight=10-18,23)]

<span data-ttu-id="1c2b0-222">下列程式碼顯示已取代金鑰的索引鍵值：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-222">The following code shows the key values for the replaced keys:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test3.cshtml.cs?name=snippet)]

<span data-ttu-id="1c2b0-223">下列命令適用于測試金鑰取代：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-223">The following command works to test key replacement:</span></span>

```dotnetcli
dotnet run -k1 value1 -k2 value2 --alt3=value2 /alt4=value3 --alt5 value5 /alt6 value6
```

<!-- Run the following command to test the key replacement: -->

<span data-ttu-id="1c2b0-224">注意：目前 `=` 無法使用單一破折號來設定索引鍵取代值 `-` 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-224">Note: Currently, `=` cannot be used to set key-replacement values with a single dash `-`.</span></span> <span data-ttu-id="1c2b0-225">請參閱[這個 GitHub 問題](https://github.com/dotnet/extensions/issues/3059)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-225">See [this GitHub issue](https://github.com/dotnet/extensions/issues/3059).</span></span>

```dotnetcli
dotnet run -k1=value1 -k2 value2 --alt3=value2 /alt4=value3 --alt5 value5 /alt6 value6
```

<span data-ttu-id="1c2b0-226">針對使用切換對應的應用程式，呼叫 `CreateDefaultBuilder` 不應傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-226">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="1c2b0-227">`CreateDefaultBuilder`方法的 `AddCommandLine` 呼叫不包含對應的參數，而且沒有任何方法可將參數對應字典傳遞至 `CreateDefaultBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-227">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch-mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="1c2b0-228">解決方案並不會將引數傳遞給， `CreateDefaultBuilder` 而是允許 `ConfigurationBuilder` 方法的 `AddCommandLine` 方法同時處理引數和切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-228">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch-mapping dictionary.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="1c2b0-229">階層式設定資料</span><span class="sxs-lookup"><span data-stu-id="1c2b0-229">Hierarchical configuration data</span></span>

<span data-ttu-id="1c2b0-230">設定 API 會藉由使用設定機碼中的分隔符號來簡維階層式資料，以讀取階層式設定資料。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-230">The Configuration API reads hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="1c2b0-231">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)包含檔案上的下列*appsettings.js* ：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-231">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following  *appsettings.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/appsettings.json)]

<span data-ttu-id="1c2b0-232">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)中的下列程式碼會顯示數個設定值：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-232">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="1c2b0-233">讀取階層式設定資料的慣用方法是使用選項模式。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-233">The preferred way to read hierarchical configuration data is using the options pattern.</span></span> <span data-ttu-id="1c2b0-234">如需詳細資訊，請參閱本檔中的系結階層式設定[資料](#optpat)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-234">For more information, see [Bind hierarchical configuration data](#optpat) in this document.</span></span>

<span data-ttu-id="1c2b0-235"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> 與 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> 方法可用來在設定資料中隔離區段與區段的子系。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-235"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="1c2b0-236">[GetSection,、 GetChildren 與 Exists](#getsection) 中說明這些方法。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-236">These methods are described later in [GetSection, GetChildren, and Exists](#getsection).</span></span>

<!--
[Azure Key Vault configuration provider](xref:security/key-vault-configuration) implement change detection.
-->

## <a name="configuration-keys-and-values"></a><span data-ttu-id="1c2b0-237">設定機碼和值</span><span class="sxs-lookup"><span data-stu-id="1c2b0-237">Configuration keys and values</span></span>

<span data-ttu-id="1c2b0-238">設定機碼：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-238">Configuration keys:</span></span>

* <span data-ttu-id="1c2b0-239">不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-239">Are case-insensitive.</span></span> <span data-ttu-id="1c2b0-240">例如，`ConnectionString` 與 `connectionstring` 會被視為相等的機碼。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-240">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="1c2b0-241">如果在多個設定提供者中設定索引鍵和值，則會使用最後新增的提供者中的值。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-241">If a key and value is set in more than one configuration providers, the value from the last provider added is used.</span></span> <span data-ttu-id="1c2b0-242">如需詳細資訊，請參閱[預設](#default)設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-242">For more information, see [Default configuration](#default).</span></span>
* <span data-ttu-id="1c2b0-243">階層式機碼</span><span class="sxs-lookup"><span data-stu-id="1c2b0-243">Hierarchical keys</span></span>
  * <span data-ttu-id="1c2b0-244">在設定 API 內，冒號分隔字元 (`:`) 可在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-244">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="1c2b0-245">在環境變數中，冒號分隔字元可能無法在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-245">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="1c2b0-246">`__`所有平臺都支援雙底線（），而且會自動轉換成冒號 `:` 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-246">A double underscore, `__`, is supported by all platforms and is automatically converted into a colon `:`.</span></span>
  * <span data-ttu-id="1c2b0-247">在 Azure Key Vault 中，階層式索引鍵會使用 `--` 做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-247">In Azure Key Vault, hierarchical keys use `--` as a separator.</span></span> <span data-ttu-id="1c2b0-248">[Azure Key Vault configuration provider](xref:security/key-vault-configuration) `--` `:` 當密碼載入應用程式的設定時，Azure Key Vault 設定提供者會自動將取代為。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-248">The [Azure Key Vault configuration provider](xref:security/key-vault-configuration) automatically replaces `--` with a `:` when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="1c2b0-249"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-249">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="1c2b0-250">[將陣列繫結到類別](#boa)一節說明陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-250">Array binding is described in the [Bind an array to a class](#boa) section.</span></span>

<span data-ttu-id="1c2b0-251">設定值：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-251">Configuration values:</span></span>

* <span data-ttu-id="1c2b0-252">為字串。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-252">Are strings.</span></span>
* <span data-ttu-id="1c2b0-253">Null 值無法存放在設定中或繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-253">Null values can't be stored in configuration or bound to objects.</span></span>

<a name="cp"></a>

## <a name="configuration-providers"></a><span data-ttu-id="1c2b0-254">設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-254">Configuration providers</span></span>

<span data-ttu-id="1c2b0-255">下表顯示可供 ASP.NET Core 應用程式使用的設定提供者。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-255">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="1c2b0-256">提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-256">Provider</span></span> | <span data-ttu-id="1c2b0-257">從提供設定</span><span class="sxs-lookup"><span data-stu-id="1c2b0-257">Provides configuration from</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="1c2b0-258">Azure Key Vault 設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-258">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration) | <span data-ttu-id="1c2b0-259">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="1c2b0-259">Azure Key Vault</span></span> |
| [<span data-ttu-id="1c2b0-260">Azure App 設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-260">Azure App configuration provider</span></span>](/azure/azure-app-configuration/quickstart-aspnet-core-app) | <span data-ttu-id="1c2b0-261">Azure 應用程式組態</span><span class="sxs-lookup"><span data-stu-id="1c2b0-261">Azure App Configuration</span></span> |
| [<span data-ttu-id="1c2b0-262">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-262">Command-line configuration provider</span></span>](#clcp) | <span data-ttu-id="1c2b0-263">命令列參數</span><span class="sxs-lookup"><span data-stu-id="1c2b0-263">Command-line parameters</span></span> |
| [<span data-ttu-id="1c2b0-264">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-264">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="1c2b0-265">自訂來源</span><span class="sxs-lookup"><span data-stu-id="1c2b0-265">Custom source</span></span> |
| [<span data-ttu-id="1c2b0-266">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-266">Environment Variables configuration provider</span></span>](#evcp) | <span data-ttu-id="1c2b0-267">環境變數</span><span class="sxs-lookup"><span data-stu-id="1c2b0-267">Environment variables</span></span> |
| [<span data-ttu-id="1c2b0-268">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-268">File configuration provider</span></span>](#file-configuration-provider) | <span data-ttu-id="1c2b0-269">INI、JSON 和 XML 檔案</span><span class="sxs-lookup"><span data-stu-id="1c2b0-269">INI, JSON, and XML files</span></span> |
| [<span data-ttu-id="1c2b0-270">每個檔案的索引鍵設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-270">Key-per-file configuration provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="1c2b0-271">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="1c2b0-271">Directory files</span></span> |
| [<span data-ttu-id="1c2b0-272">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-272">Memory configuration provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="1c2b0-273">記憶體內集合</span><span class="sxs-lookup"><span data-stu-id="1c2b0-273">In-memory collections</span></span> |
| [<span data-ttu-id="1c2b0-274">秘密管理員</span><span class="sxs-lookup"><span data-stu-id="1c2b0-274">Secret Manager</span></span>](xref:security/app-secrets)  | <span data-ttu-id="1c2b0-275">使用者設定檔目錄中的檔案</span><span class="sxs-lookup"><span data-stu-id="1c2b0-275">File in the user profile directory</span></span> |

<span data-ttu-id="1c2b0-276">設定來源會依照其設定提供者的指定順序讀取。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-276">Configuration sources are read in the order that their configuration providers are specified.</span></span> <span data-ttu-id="1c2b0-277">請在程式碼中訂購設定提供者，以符合應用程式所需之基礎設定來源的優先順序。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-277">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="1c2b0-278">典型的設定提供者順序是：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-278">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="1c2b0-279">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="1c2b0-279">*appsettings.json*</span></span>
1. <span data-ttu-id="1c2b0-280">*appsettings*。 `Environment`*json*</span><span class="sxs-lookup"><span data-stu-id="1c2b0-280">*appsettings*.`Environment`.*json*</span></span>
1. [<span data-ttu-id="1c2b0-281">秘密管理員</span><span class="sxs-lookup"><span data-stu-id="1c2b0-281">Secret Manager</span></span>](xref:security/app-secrets)
1. <span data-ttu-id="1c2b0-282">使用[環境變數設定提供者](#evcp)的環境變數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-282">Environment variables using the [Environment Variables configuration provider](#evcp).</span></span>
1. <span data-ttu-id="1c2b0-283">使用[命令列設定提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-283">Command-line arguments using the [Command-line configuration provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="1c2b0-284">常見的做法是在一系列提供者中新增命令列設定提供者，以允許命令列引數覆寫其他提供者所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-284">A common practice is to add the Command-line configuration provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="1c2b0-285">先前的提供者序列會用於[預設](#default)設定中。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-285">The preceding sequence of providers is used in the [default configuration](#default).</span></span>

<a name="constr"></a>

### <a name="connection-string-prefixes"></a><span data-ttu-id="1c2b0-286">連接字串前置詞</span><span class="sxs-lookup"><span data-stu-id="1c2b0-286">Connection string prefixes</span></span>

<span data-ttu-id="1c2b0-287">設定 API 具有四個連接字串環境變數的特殊處理規則。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-287">The Configuration API has special processing rules for four connection string environment variables.</span></span> <span data-ttu-id="1c2b0-288">這些連接字串牽涉到設定應用程式環境的 Azure 連接字串。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-288">These connection strings are involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="1c2b0-289">具有資料表中所顯示前置詞的環境變數，會載入至具有[預設](#default)設定的應用程式，或未提供任何首碼時 `AddEnvironmentVariables` 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-289">Environment variables with the prefixes shown in the table are loaded into the app with the [default configuration](#default) or when no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="1c2b0-290">連接字串前置詞</span><span class="sxs-lookup"><span data-stu-id="1c2b0-290">Connection string prefix</span></span> | <span data-ttu-id="1c2b0-291">提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-291">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="1c2b0-292">自訂提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-292">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="1c2b0-293">MySQL</span><span class="sxs-lookup"><span data-stu-id="1c2b0-293">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="1c2b0-294">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="1c2b0-294">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="1c2b0-295">SQL Server</span><span class="sxs-lookup"><span data-stu-id="1c2b0-295">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="1c2b0-296">當探索到具有下表所顯示之任何四個前置詞的環境變數並將其載入到設定中時：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-296">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="1c2b0-297">會透過移除環境變數前置詞並新增設定機碼區段 (`ConnectionStrings`) 來移除具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-297">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="1c2b0-298">會建立新的設定機碼值組以代表資料庫連線提供者 (`CUSTOMCONNSTR_` 除外，這沒有所述提供者)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-298">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="1c2b0-299">環境變數機碼</span><span class="sxs-lookup"><span data-stu-id="1c2b0-299">Environment variable key</span></span> | <span data-ttu-id="1c2b0-300">已轉換的設定機碼</span><span class="sxs-lookup"><span data-stu-id="1c2b0-300">Converted configuration key</span></span> | <span data-ttu-id="1c2b0-301">提供者設定項目</span><span class="sxs-lookup"><span data-stu-id="1c2b0-301">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="1c2b0-302">設定項目未建立。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-302">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="1c2b0-303">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-303">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="1c2b0-304">值：`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="1c2b0-304">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="1c2b0-305">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-305">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="1c2b0-306">值：`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="1c2b0-306">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="1c2b0-307">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-307">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="1c2b0-308">值：`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="1c2b0-308">Value: `System.Data.SqlClient`</span></span>  |

<a name="jcp"></a>

### <a name="json-configuration-provider"></a><span data-ttu-id="1c2b0-309">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-309">JSON configuration provider</span></span>

<span data-ttu-id="1c2b0-310">會 <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> 從 JSON 檔案索引鍵/值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-310">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs.</span></span>

<span data-ttu-id="1c2b0-311">多載可以指定：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-311">Overloads can specify:</span></span>

* <span data-ttu-id="1c2b0-312">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-312">Whether the file is optional.</span></span>
* <span data-ttu-id="1c2b0-313">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-313">Whether the configuration is reloaded if the file changes.</span></span>

<span data-ttu-id="1c2b0-314">請考慮下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-314">Consider the following code:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSON.cs?name=snippet&highlight=12-14)]

<span data-ttu-id="1c2b0-315">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-315">The preceding code:</span></span>

* <span data-ttu-id="1c2b0-316">設定 JSON 設定提供者，以使用下列選項載入檔案*上的MyConfig.js* ：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-316">Configures the JSON configuration provider to load the *MyConfig.json* file with the following options:</span></span>
  * <span data-ttu-id="1c2b0-317">`optional: true`：檔案是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-317">`optional: true`: The file is optional.</span></span>
  * <span data-ttu-id="1c2b0-318">`reloadOnChange: true`：儲存變更時，會重載檔案。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-318">`reloadOnChange: true` : The file is reloaded when changes are saved.</span></span>
* <span data-ttu-id="1c2b0-319">在檔案*MyConfig.js*之前，讀取預設的設定[提供者](#default)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-319">Reads the [default configuration providers](#default) before the *MyConfig.json* file.</span></span> <span data-ttu-id="1c2b0-320">預設設定提供者（包括[環境變數設定提供者](#evcp)和[命令列設定提供者](#clcp)）中，[檔案覆寫] 設定中的*MyConfig.js*設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-320">Settings in the *MyConfig.json* file override setting in the default configuration providers, including the [Environment variables configuration provider](#evcp) and the [Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="1c2b0-321">您通常***不***會想要覆寫[環境變數設定提供者](#evcp)和[命令列設定提供者](#clcp)中所設定之值的自訂 JSON 檔案。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-321">You typically ***don't*** want a custom JSON file overriding values set in the [Environment variables configuration provider](#evcp) and the [Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="1c2b0-322">下列程式碼會清除所有設定提供者，並新增數個設定提供者：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-322">The following code clears all the configuration providers and adds several configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSON2.cs?name=snippet)]

<span data-ttu-id="1c2b0-323">在上述程式碼中， *MyConfig.js上*的設定和*myconfig.xml*。 `Environment`*json*檔案：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-323">In the preceding code, settings in the *MyConfig.json* and  *MyConfig*.`Environment`.*json* files:</span></span>

* <span data-ttu-id="1c2b0-324">覆寫和 appsettings*上appsettings.js*中*appsettings*的設定 `Environment` 。*json*檔案。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-324">Override settings in the *appsettings.json* and *appsettings*.`Environment`.*json* files.</span></span>
* <span data-ttu-id="1c2b0-325">會由[環境變數設定提供者](#evcp)和[命令列設定提供者](#clcp)中的設定覆寫。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-325">Are overridden by settings in the [Environment variables configuration provider](#evcp) and the [Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="1c2b0-326">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)包含檔案上的下列*MyConfig.js* ：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-326">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following  *MyConfig.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/MyConfig.json)]

<span data-ttu-id="1c2b0-327">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)中的下列程式碼會顯示上述幾個設定值：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-327">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<a name="fcp"></a>

## <a name="file-configuration-provider"></a><span data-ttu-id="1c2b0-328">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-328">File configuration provider</span></span>

<span data-ttu-id="1c2b0-329"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> 是用於從檔案系統載入設定的基底類別。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-329"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="1c2b0-330">下列是衍生自的設定提供者 `FileConfigurationProvider` ：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-330">The following configuration providers derive from `FileConfigurationProvider`:</span></span>

* [<span data-ttu-id="1c2b0-331">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-331">INI configuration provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="1c2b0-332">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-332">JSON configuration provider</span></span>](#jcp)
* [<span data-ttu-id="1c2b0-333">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-333">XML configuration provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="1c2b0-334">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-334">INI configuration provider</span></span>

<span data-ttu-id="1c2b0-335"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> 會在執行階段從 INI 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-335">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="1c2b0-336">下列程式碼會清除所有設定提供者，並新增數個設定提供者：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-336">The following code clears all the configuration providers and adds several configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramINI.cs?name=snippet&highlight=10-30)]

<span data-ttu-id="1c2b0-337">在上述程式碼中， *MyIniConfig.ini*和*MyIniConfig*中的設定 `Environment` 。*ini*檔案會由中的設定覆寫：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-337">In the preceding code, settings in the *MyIniConfig.ini* and  *MyIniConfig*.`Environment`.*ini* files are overridden by settings in the:</span></span>

* [<span data-ttu-id="1c2b0-338">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-338">Environment variables configuration provider</span></span>](#evcp)
* <span data-ttu-id="1c2b0-339">[命令列設定提供者](#clcp)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-339">[Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="1c2b0-340">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)包含下列*MyIniConfig.ini*檔案：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-340">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following *MyIniConfig.ini* file:</span></span>

[!code-ini[](index/samples/3.x/ConfigSample/MyIniConfig.ini)]

<span data-ttu-id="1c2b0-341">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)中的下列程式碼會顯示上述幾個設定值：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-341">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

### <a name="xml-configuration-provider"></a><span data-ttu-id="1c2b0-342">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-342">XML configuration provider</span></span>

<span data-ttu-id="1c2b0-343"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> 會在執行階段從 XML 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-343">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="1c2b0-344">下列程式碼會清除所有設定提供者，並新增數個設定提供者：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-344">The following code clears all the configuration providers and adds several configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramXML.cs?name=snippet)]

<span data-ttu-id="1c2b0-345">在上述程式碼中， *MyXMLFile.xml*和*MyXMLFile*中的設定 `Environment` 。中的設定會覆寫*xml*檔案：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-345">In the preceding code, settings in the *MyXMLFile.xml* and  *MyXMLFile*.`Environment`.*xml* files are overridden by settings in the:</span></span>

* [<span data-ttu-id="1c2b0-346">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-346">Environment variables configuration provider</span></span>](#evcp)
* <span data-ttu-id="1c2b0-347">[命令列設定提供者](#clcp)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-347">[Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="1c2b0-348">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)包含下列*MyXMLFile.xml*檔案：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-348">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following *MyXMLFile.xml* file:</span></span>

[!code-xml[](index/samples/3.x/ConfigSample/MyXMLFile.xml)]

<span data-ttu-id="1c2b0-349">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)中的下列程式碼會顯示上述幾個設定值：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-349">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="1c2b0-350">若 `name` 屬性是用來區別元素，則可以重複那些使用相同元素名稱的元素：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-350">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

[!code-xml[](index/samples/3.x/ConfigSample/MyXMLFile3.xml)]

<span data-ttu-id="1c2b0-351">下列程式碼會讀取先前的設定檔，並顯示金鑰和值：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-351">The following code reads the previous configuration file and displays the keys and values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/XML/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="1c2b0-352">屬性可用來提供值：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-352">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="1c2b0-353">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-353">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="1c2b0-354">key:attribute</span><span class="sxs-lookup"><span data-stu-id="1c2b0-354">key:attribute</span></span>
* <span data-ttu-id="1c2b0-355">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="1c2b0-355">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="1c2b0-356">每個檔案的索引鍵設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-356">Key-per-file configuration provider</span></span>

<span data-ttu-id="1c2b0-357"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> 使用目錄的檔案做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-357">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="1c2b0-358">機碼是檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-358">The key is the file name.</span></span> <span data-ttu-id="1c2b0-359">值包含檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-359">The value contains the file's contents.</span></span> <span data-ttu-id="1c2b0-360">Docker 裝載案例中會使用每個檔案的索引鍵設定提供者。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-360">The Key-per-file configuration provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="1c2b0-361">若要啟用每個檔案機碼設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-361">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="1c2b0-362">檔案的 `directoryPath` 必須是絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-362">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="1c2b0-363">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-363">Overloads permit specifying:</span></span>

* <span data-ttu-id="1c2b0-364">設定來源的 `Action<KeyPerFileConfigurationSource>` 委派。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-364">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="1c2b0-365">目錄是否為選擇性與目錄的路徑。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-365">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="1c2b0-366">雙底線 (`__`) 是做為檔案名稱中的設定金鑰分隔符號使用。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-366">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="1c2b0-367">例如，檔案名稱 `Logging__LogLevel__System` 會產生設定金鑰 `Logging:LogLevel:System`。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-367">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="1c2b0-368">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-368">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

<a name="mcp"></a>

## <a name="memory-configuration-provider"></a><span data-ttu-id="1c2b0-369">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-369">Memory configuration provider</span></span>

<span data-ttu-id="1c2b0-370"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> 使用記憶體內集合做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-370">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="1c2b0-371">下列程式碼會將記憶體集合新增至設定系統：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-371">The following code adds a memory collection to the configuration system:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet6)]

<span data-ttu-id="1c2b0-372">下列來自[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)的程式碼會顯示先前的設定值：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-372">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="1c2b0-373">在上述程式碼中， `config.AddInMemoryCollection(Dict)` 會在預設的設定[提供者](#default)之後加入。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-373">In the preceding code, `config.AddInMemoryCollection(Dict)` is added after the [default configuration providers](#default).</span></span> <span data-ttu-id="1c2b0-374">如需排序設定提供者的範例，請參閱[JSON 設定提供者](#jcp)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-374">For an example of ordering the configuration providers, see [JSON configuration provider](#jcp).</span></span>

<span data-ttu-id="1c2b0-375">如需其他範例，請參閱使用來系結[陣列](#boa) `MemoryConfigurationProvider` 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-375">See [Bind an array](#boa) for another example using `MemoryConfigurationProvider`.</span></span>

## <a name="getvalue"></a><span data-ttu-id="1c2b0-376">GetValue</span><span class="sxs-lookup"><span data-stu-id="1c2b0-376">GetValue</span></span>

<span data-ttu-id="1c2b0-377">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*)使用指定的索引鍵從設定中解壓縮單一值，並將其轉換為指定的類型：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-377">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified type:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestNum.cshtml.cs?name=snippet)]

<span data-ttu-id="1c2b0-378">在上述程式碼中，如果在設定 `NumberKey` 中找不到，則會使用的預設值 `99` 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-378">In the preceding code,  if `NumberKey` isn't found in the configuration, the default value of `99` is used.</span></span>

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="1c2b0-379">GetSection、GetChildren 與 Exists</span><span class="sxs-lookup"><span data-stu-id="1c2b0-379">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="1c2b0-380">針對接下來的範例，請考慮下列*MySubsection.js*檔案：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-380">For the examples that follow, consider the following *MySubsection.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/MySubsection.json)]

<span data-ttu-id="1c2b0-381">下列程式碼會將*MySubsection.js*新增至設定提供者：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-381">The following code adds *MySubsection.json* to the configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSONsection.cs?name=snippet)]

### <a name="getsection"></a><span data-ttu-id="1c2b0-382">GetSection</span><span class="sxs-lookup"><span data-stu-id="1c2b0-382">GetSection</span></span>

<span data-ttu-id="1c2b0-383">[IConfiguration。 GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*)會傳回具有指定子區段索引鍵的設定子區段。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-383">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) returns a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="1c2b0-384">下列程式碼會傳回的值 `section1` ：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-384">The following code returns values for `section1`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection.cshtml.cs?name=snippet)]

<span data-ttu-id="1c2b0-385">下列程式碼會傳回的值 `section2:subsection0` ：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-385">The following code returns values for `section2:subsection0`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection2.cshtml.cs?name=snippet)]

<span data-ttu-id="1c2b0-386">`GetSection` 絕不會傳回 `null`。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-386">`GetSection` never returns `null`.</span></span> <span data-ttu-id="1c2b0-387">若找不到相符的區段，會傳回空的 `IConfigurationSection`。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-387">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="1c2b0-388">當 `GetSection` 傳回相符區段時，未填入 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value>。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-388">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="1c2b0-389">當區段存在時，會傳回 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> 與 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path>。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-389">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren-and-exists"></a><span data-ttu-id="1c2b0-390">GetChildren 和 Exists</span><span class="sxs-lookup"><span data-stu-id="1c2b0-390">GetChildren and Exists</span></span>

<span data-ttu-id="1c2b0-391">下列程式碼會呼叫[IConfiguration. GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) ，並傳回的值 `section2:subsection0` ：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-391">The following code calls [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) and returns values for `section2:subsection0`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection4.cshtml.cs?name=snippet)]

<span data-ttu-id="1c2b0-392">上述程式碼會呼叫[microsoft.extensions.options.configurationextensions](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) ，以確認區段是否存在：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-392">The preceding code calls [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to verify the  section exists:</span></span>

 <a name="boa"></a>

## <a name="bind-an-array"></a><span data-ttu-id="1c2b0-393">系結陣列</span><span class="sxs-lookup"><span data-stu-id="1c2b0-393">Bind an array</span></span>

<span data-ttu-id="1c2b0-394">[ConfigurationBinder](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*)支援在設定機碼中使用陣列索引將陣列系結至物件。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-394">The [ConfigurationBinder.Bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*) supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="1c2b0-395">任何會公開數值索引鍵的陣列格式，都可以系結至[POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object)類別陣列。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-395">Any array format that exposes a numeric key segment is capable of array binding to a [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) class array.</span></span>

<span data-ttu-id="1c2b0-396">請考慮從[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)中的*MyArray.js* ：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-396">Consider *MyArray.json* from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample):</span></span>

[!code-json[](index/samples/3.x/ConfigSample/MyArray.json)]

<span data-ttu-id="1c2b0-397">下列程式碼會將*MyArray.js*新增至設定提供者：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-397">The following code adds *MyArray.json* to the configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSONarray.cs?name=snippet)]

<span data-ttu-id="1c2b0-398">下列程式碼會讀取設定並顯示值：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-398">The following code reads the configuration and displays the values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

<span data-ttu-id="1c2b0-399">上述程式碼會傳回下列輸出：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-399">The preceding code returns the following output:</span></span>

```text
Index: 0  Value: value00
Index: 1  Value: value10
Index: 2  Value: value20
Index: 3  Value: value40
Index: 4  Value: value50
```

<span data-ttu-id="1c2b0-400">在上述輸出中，索引3具有值 `value40` ，對應于 `"4": "value40",` 中的*MyArray.js*。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-400">In the preceding output, Index 3 has value `value40`, corresponding to `"4": "value40",` in *MyArray.json*.</span></span> <span data-ttu-id="1c2b0-401">系結的陣列索引是連續的，而且不會系結至設定金鑰索引。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-401">The bound array indices are continuous and not bound to the configuration key index.</span></span> <span data-ttu-id="1c2b0-402">設定系結器無法系結 null 值，或在系結物件中建立 null 專案</span><span class="sxs-lookup"><span data-stu-id="1c2b0-402">The configuration binder isn't capable of binding null values or creating null entries in bound objects</span></span>

<span data-ttu-id="1c2b0-403">下列程式碼會 `array:entries` 使用擴充方法來載入設定 <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> ：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-403">The  following code loads the `array:entries` configuration with the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet)]

<span data-ttu-id="1c2b0-404">下列程式碼會讀取中的設定 `arrayDict` `Dictionary` ，並顯示值：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-404">The following code reads the configuration in the `arrayDict` `Dictionary` and displays the values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

<span data-ttu-id="1c2b0-405">上述程式碼會傳回下列輸出：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-405">The preceding code returns the following output:</span></span>

```text
Index: 0  Value: value0
Index: 1  Value: value1
Index: 2  Value: value2
Index: 3  Value: value4
Index: 4  Value: value5
```

<span data-ttu-id="1c2b0-406">繫結物件中的索引 &num;3 存放 `array:4` 設定機碼與其 `value4` 的設定資料。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-406">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="1c2b0-407">當系結包含陣列的設定資料時，在建立物件時，會使用設定機碼中的陣列索引來反復查看設定資料。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-407">When configuration data containing an array is bound, the array indices in the configuration keys are used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="1c2b0-408">設定資料中不能保留 Null 值，當設定機碼中的陣列略過一或多個索引時，不會在繫結物件中建立 Null 值項目。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-408">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="1c2b0-409">在系 &num; 結至實例之前，您可以先提供索引3的遺漏設定專案 `ArrayExample` ，以讀取索引 &num; 3 鍵/值組的任何設定提供者。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-409">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that reads the index &num;3 key/value pair.</span></span> <span data-ttu-id="1c2b0-410">請考慮下列來自範例下載的檔案*Value3.js* ：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-410">Consider the following *Value3.json* file from the sample download:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/Value3.json)]

<span data-ttu-id="1c2b0-411">下列程式碼包含在和*上Value3.js*的設定 `arrayDict` `Dictionary` ：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-411">The following code includes configuration for *Value3.json* and the `arrayDict` `Dictionary`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet2)]

<span data-ttu-id="1c2b0-412">下列程式碼會讀取先前的設定，並顯示值：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-412">The following code reads the preceding configuration and displays the values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

<span data-ttu-id="1c2b0-413">上述程式碼會傳回下列輸出：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-413">The preceding code returns the following output:</span></span>

```text
Index: 0  Value: value0
Index: 1  Value: value1
Index: 2  Value: value2
Index: 3  Value: value3
Index: 4  Value: value4
Index: 5  Value: value5
```

<span data-ttu-id="1c2b0-414">自訂設定提供者不需要實作陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-414">Custom configuration providers aren't required to implement array binding.</span></span>

## <a name="custom-configuration-provider"></a><span data-ttu-id="1c2b0-415">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-415">Custom configuration provider</span></span>

<span data-ttu-id="1c2b0-416">範例應用程式示範如何建立會使用 [Entity Framework (EF)](/ef/core/) 從資料庫讀取設定機碼值組的基本設定提供者。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-416">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="1c2b0-417">提供者具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-417">The provider has the following characteristics:</span></span>

* <span data-ttu-id="1c2b0-418">EF 記憶體內資料庫會用於示範用途。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-418">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="1c2b0-419">若要使用需要連接字串的資料庫，請實作第二個 `ConfigurationBuilder` 以從另一個設定提供者提供連接字串。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-419">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="1c2b0-420">啟動時，該提供者會將資料庫資料表讀入到設定中。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-420">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="1c2b0-421">該提供者不會以個別機碼為基礎查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-421">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="1c2b0-422">未實作變更時重新載入，因此在應用程式啟動後更新資料庫對應用程式設定沒有影響。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-422">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="1c2b0-423">定義 `EFConfigurationValue` 實體來在資料庫中存放設定值。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-423">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="1c2b0-424">*Models/EFConfigurationValue.cs*：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-424">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="1c2b0-425">新增 `EFConfigurationContext` 以存放及存取已設定的值。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-425">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="1c2b0-426">*EFConfigurationProvider/EFConfigurationContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-426">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="1c2b0-427">建立會實作 <xref:Microsoft.Extensions.Configuration.IConfigurationSource> 的類別。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-427">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="1c2b0-428">*EFConfigurationProvider/EFConfigurationSource.cs*：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-428">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="1c2b0-429">透過繼承自 <xref:Microsoft.Extensions.Configuration.ConfigurationProvider> 來建立自訂設定提供者。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-429">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="1c2b0-430">若資料庫是空的，設定提供者會初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-430">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="1c2b0-431">由於設定索引[鍵不區分大小寫](#keys)，用來初始化資料庫的字典會以不區分大小寫的比較子（[StringComparer. OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)）來建立。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-431">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="1c2b0-432">*EFConfigurationProvider/EFConfigurationProvider.cs*：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-432">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="1c2b0-433">`AddEFConfiguration` 擴充方法允許新增設定來源到 `ConfigurationBuilder`。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-433">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="1c2b0-434">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c2b0-434">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="1c2b0-435">下列程式碼顯示如何在 *Program.cs* 中使用自訂 `EFConfigurationProvider`：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-435">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

<a name="acs"></a>

## <a name="access-configuration-in-startup"></a><span data-ttu-id="1c2b0-436">啟動時的存取設定</span><span class="sxs-lookup"><span data-stu-id="1c2b0-436">Access configuration in Startup</span></span>

<span data-ttu-id="1c2b0-437">下列程式碼會顯示方法中的設定資料 `Startup` ：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-437">The following code displays configuration data in `Startup` methods:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/StartupKey.cs?name=snippet&highlight=13,18)]

<span data-ttu-id="1c2b0-438">如需使用啟動方便方法來存取設定的範例，請參閱[應用程式啟動：方便方法](xref:fundamentals/startup#convenience-methods)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-438">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-razor-pages"></a><span data-ttu-id="1c2b0-439">頁面中的存取設定 Razor</span><span class="sxs-lookup"><span data-stu-id="1c2b0-439">Access configuration in Razor Pages</span></span>

<span data-ttu-id="1c2b0-440">下列程式碼會在頁面中顯示設定資料 Razor ：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-440">The following code displays configuration data in a Razor Page:</span></span>

[!code-cshtml[](index/samples/3.x/ConfigSample/Pages/Test5.cshtml)]

<span data-ttu-id="1c2b0-441">在下列程式碼中， `MyOptions` 會新增至服務容器， <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> 並系結至設定：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-441">In the following code, `MyOptions` is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](~/fundamentals/configuration/options/samples/3.x/OptionsSample/Startup3.cs?name=snippet_Example2)]

<span data-ttu-id="1c2b0-442">下列標記會使用指示詞 [`@inject`](xref:mvc/views/razor#inject) Razor 來解析並顯示選項值：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-442">The following markup uses the [`@inject`](xref:mvc/views/razor#inject) Razor directive to resolve and display the options values:</span></span>

[!code-cshtml[](~/fundamentals/configuration/options/samples/3.x/OptionsSample/Pages/Test3.cshtml)]

## <a name="access-configuration-in-a-mvc-view-file"></a><span data-ttu-id="1c2b0-443">MVC 視圖檔案中的存取設定</span><span class="sxs-lookup"><span data-stu-id="1c2b0-443">Access configuration in a MVC view file</span></span>

<span data-ttu-id="1c2b0-444">下列程式碼會在 MVC 視圖中顯示設定資料：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-444">The following code displays configuration data in a MVC view:</span></span>

[!code-cshtml[](index/samples/3.x/ConfigSample/Views/Home2/Index.cshtml)]

## <a name="configure-options-with-a-delegate"></a><span data-ttu-id="1c2b0-445">使用委派設定選項</span><span class="sxs-lookup"><span data-stu-id="1c2b0-445">Configure options with a delegate</span></span>

<span data-ttu-id="1c2b0-446">委派中設定的選項會覆寫設定提供者中所設定的值。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-446">Options configured in a delegate override values set in the configuration providers.</span></span>

<span data-ttu-id="1c2b0-447">範例應用程式中的範例2會示範如何使用委派設定選項。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-447">Configuring options with a delegate is demonstrated as Example 2 in the sample app.</span></span>

<span data-ttu-id="1c2b0-448">在下列程式碼中， <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 服務會加入至服務容器。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-448">In the following code, an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="1c2b0-449">它會使用委派來設定的值 `MyOptions` ：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-449">It uses a delegate to configure values for `MyOptions`:</span></span>

[!code-csharp[](~/fundamentals/configuration/options/samples/3.x/OptionsSample/Startup2.cs?name=snippet_Example2)]

<span data-ttu-id="1c2b0-450">下列程式碼會顯示選項值：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-450">The following code displays the options values:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Test2.cshtml.cs?name=snippet)]

<span data-ttu-id="1c2b0-451">在上述範例中，和的值 `Option1` `Option2` 是在*appsettings.js*中指定，然後由設定的委派覆寫。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-451">In the preceding example, the values of `Option1` and `Option2` are specified in *appsettings.json* and then overridden by the configured delegate.</span></span>

<a name="hvac"></a>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="1c2b0-452">主機與應用程式組態的比較</span><span class="sxs-lookup"><span data-stu-id="1c2b0-452">Host versus app configuration</span></span>

<span data-ttu-id="1c2b0-453">設定及啟動應用程式之前，會先設定及啟動「主機」\*\*。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-453">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="1c2b0-454">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-454">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="1c2b0-455">應用程式與主機都是使用此主題中所述的設定提供者來設定的。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-455">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="1c2b0-456">主機組態機碼/值組也會包含在應用程式的組態中。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-456">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="1c2b0-457">如需有關當建置主機時如何使用設定提供者的詳細資訊，以及設定來源如何影響主機設定的詳細資訊，請參閱 <xref:fundamentals/index#host>。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-457">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

<a name="dhc"></a>

## <a name="default-host-configuration"></a><span data-ttu-id="1c2b0-458">預設主機設定</span><span class="sxs-lookup"><span data-stu-id="1c2b0-458">Default host configuration</span></span>

<span data-ttu-id="1c2b0-459">如需使用 [Web 主機](xref:fundamentals/host/web-host)時預設組態的詳細資料，請參閱[本主題的 ASP.NET Core 2.2 版本](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-459">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="1c2b0-460">主機組態的提供來源：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-460">Host configuration is provided from:</span></span>
  * <span data-ttu-id="1c2b0-461">前面加上的環境變數 `DOTNET_` （例如 `DOTNET_ENVIRONMENT` ），並使用[環境變數設定提供者](#environment-variables)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-461">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables configuration provider](#environment-variables).</span></span> <span data-ttu-id="1c2b0-462">載入設定機碼值組時，會移除前置詞 (`DOTNET_`)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-462">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="1c2b0-463">使用[命令列設定提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-463">Command-line arguments using the [Command-line configuration provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="1c2b0-464">已建立 Web 主機預設組態 (`ConfigureWebHostDefaults`)：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-464">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="1c2b0-465">Kestrel 會用作為網頁伺服器，並使用應用程式的組態提供者來設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-465">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="1c2b0-466">新增主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-466">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="1c2b0-467">如果 `ASPNETCORE_FORWARDEDHEADERS_ENABLED` 環境變數設定為 `true`，則會新增轉接的標頭中介軟體。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-467">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="1c2b0-468">啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-468">Enable IIS integration.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="1c2b0-469">其他設定</span><span class="sxs-lookup"><span data-stu-id="1c2b0-469">Other configuration</span></span>

<span data-ttu-id="1c2b0-470">本主題僅適用于*應用程式*設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-470">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="1c2b0-471">執行和裝載 ASP.NET Core 應用程式的其他層面，是使用本主題未涵蓋的設定檔來設定：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-471">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="1c2b0-472">*launch.js于* /*上的launchSettings.js*是開發環境的工具設定檔，如下所述：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-472">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="1c2b0-473">在中 <xref:fundamentals/environments#development> 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-473">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="1c2b0-474">在檔集內，用來為開發案例設定 ASP.NET Core 應用程式的檔案。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-474">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="1c2b0-475">*web.config*是伺服器設定檔，如下列主題所述：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-475">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="1c2b0-476">在*launchSettings.js*中設定的環境變數會覆寫系統內容中的設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-476">Environment variables set in *launchSettings.json* override those set in the system environment.</span></span>

<span data-ttu-id="1c2b0-477">如需從舊版 ASP.NET 遷移應用程式設定的詳細資訊，請參閱 <xref:migration/proper-to-2x/index#store-configurations> 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-477">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="1c2b0-478">從外部組件新增設定</span><span class="sxs-lookup"><span data-stu-id="1c2b0-478">Add configuration from an external assembly</span></span>

<span data-ttu-id="1c2b0-479"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-479">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="1c2b0-480">如需詳細資訊，請參閱 <xref:fundamentals/configuration/platform-specific-configuration> 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-480">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1c2b0-481">其他資源</span><span class="sxs-lookup"><span data-stu-id="1c2b0-481">Additional resources</span></span>

* [<span data-ttu-id="1c2b0-482">設定原始程式碼</span><span class="sxs-lookup"><span data-stu-id="1c2b0-482">Configuration source code</span></span>](https://github.com/dotnet/extensions/tree/master/src/Configuration)
* <xref:fundamentals/configuration/options>
* <xref:blazor/fundamentals/configuration>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1c2b0-483">ASP.NET Core 中的應用程式設定是以由*設定提供者*所建立的機碼值組為基礎。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-483">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="1c2b0-484">設定提供者會從各種設定來源將設定資料讀取到機碼值組中：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-484">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="1c2b0-485">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="1c2b0-485">Azure Key Vault</span></span>
* <span data-ttu-id="1c2b0-486">Azure 應用程式組態</span><span class="sxs-lookup"><span data-stu-id="1c2b0-486">Azure App Configuration</span></span>
* <span data-ttu-id="1c2b0-487">命令列引數</span><span class="sxs-lookup"><span data-stu-id="1c2b0-487">Command-line arguments</span></span>
* <span data-ttu-id="1c2b0-488">自訂提供者 (已安裝或已建立)</span><span class="sxs-lookup"><span data-stu-id="1c2b0-488">Custom providers (installed or created)</span></span>
* <span data-ttu-id="1c2b0-489">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="1c2b0-489">Directory files</span></span>
* <span data-ttu-id="1c2b0-490">環境變數</span><span class="sxs-lookup"><span data-stu-id="1c2b0-490">Environment variables</span></span>
* <span data-ttu-id="1c2b0-491">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="1c2b0-491">In-memory .NET objects</span></span>
* <span data-ttu-id="1c2b0-492">設定檔</span><span class="sxs-lookup"><span data-stu-id="1c2b0-492">Settings files</span></span>

<span data-ttu-id="1c2b0-493">一般組態提供者案例 ([microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) 的組態套件包含在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-493">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="1c2b0-494">下列程式碼範例與範例應用程式中的程式碼範例使用 <xref:Microsoft.Extensions.Configuration> 命名空間：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-494">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="1c2b0-495">*選項模式*是此主題中所述之設定概念的延伸。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-495">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="1c2b0-496">選項使用類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-496">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="1c2b0-497">如需詳細資訊，請參閱 <xref:fundamentals/configuration/options> 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-497">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="1c2b0-498">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="1c2b0-498">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="1c2b0-499">主機與應用程式組態的比較</span><span class="sxs-lookup"><span data-stu-id="1c2b0-499">Host versus app configuration</span></span>

<span data-ttu-id="1c2b0-500">設定及啟動應用程式之前，會先設定及啟動「主機」\*\*。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-500">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="1c2b0-501">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-501">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="1c2b0-502">應用程式與主機都是使用此主題中所述的設定提供者來設定的。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-502">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="1c2b0-503">主機組態機碼/值組也會包含在應用程式的組態中。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-503">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="1c2b0-504">如需有關當建置主機時如何使用設定提供者的詳細資訊，以及設定來源如何影響主機設定的詳細資訊，請參閱 <xref:fundamentals/index#host>。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-504">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="1c2b0-505">其他設定</span><span class="sxs-lookup"><span data-stu-id="1c2b0-505">Other configuration</span></span>

<span data-ttu-id="1c2b0-506">本主題僅適用于*應用程式*設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-506">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="1c2b0-507">執行和裝載 ASP.NET Core 應用程式的其他層面，是使用本主題未涵蓋的設定檔來設定：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-507">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="1c2b0-508">*launch.js于* /*上的launchSettings.js*是開發環境的工具設定檔，如下所述：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-508">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="1c2b0-509">在中 <xref:fundamentals/environments#development> 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-509">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="1c2b0-510">在檔集內，用來為開發案例設定 ASP.NET Core 應用程式的檔案。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-510">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="1c2b0-511">*web.config*是伺服器設定檔，如下列主題所述：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-511">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="1c2b0-512">如需從舊版 ASP.NET 遷移應用程式設定的詳細資訊，請參閱 <xref:migration/proper-to-2x/index#store-configurations> 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-512">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="1c2b0-513">預設組態</span><span class="sxs-lookup"><span data-stu-id="1c2b0-513">Default configuration</span></span>

<span data-ttu-id="1c2b0-514">以 ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) 範本為基礎的 Web 應用程式，會在建置主機時呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-514">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="1c2b0-515">`CreateDefaultBuilder` 會以下列順序提供應用程式的預設組態：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-515">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="1c2b0-516">下列項目適用於使用 [Web 主機](xref:fundamentals/host/web-host)的應用程式。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-516">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="1c2b0-517">如需使用[一般主機](xref:fundamentals/host/generic-host)時預設組態的詳細資料，請參閱[本主題的最新版本](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-517">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="1c2b0-518">主機組態的提供來源：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-518">Host configuration is provided from:</span></span>
  * <span data-ttu-id="1c2b0-519">使用[環境變數組態提供者](#environment-variables-configuration-provider)且以 `ASPNETCORE_` 為前置詞 (例如 `ASPNETCORE_ENVIRONMENT`) 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-519">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="1c2b0-520">載入設定機碼值組時，會移除前置詞 (`ASPNETCORE_`)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-520">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="1c2b0-521">使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-521">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="1c2b0-522">應用程式設定的提供來源：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-522">App configuration is provided from:</span></span>
  * <span data-ttu-id="1c2b0-523">使用[檔案組態提供者](#file-configuration-provider)的 *appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-523">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="1c2b0-524">使用[檔案組態提供者](#file-configuration-provider)的 *appsettings.{Environment}.json*。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-524">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="1c2b0-525">應用程式在使用項目組件之 `Development` 環境中執行時的[秘密管理員](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-525">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="1c2b0-526">使用[環境變數組態提供者](#environment-variables-configuration-provider)的環境變數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-526">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="1c2b0-527">使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-527">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

## <a name="security"></a><span data-ttu-id="1c2b0-528">安全性</span><span class="sxs-lookup"><span data-stu-id="1c2b0-528">Security</span></span>

<span data-ttu-id="1c2b0-529">採用下列做法來保護敏感性組態資料：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-529">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="1c2b0-530">永遠不要將密碼或其他敏感性資料儲存在設定提供者程式碼或純文字設定檔中。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-530">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="1c2b0-531">不要在開發或測試環境中使用生產環境祕密。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-531">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="1c2b0-532">請在專案外部指定祕密，以防止其意外認可至開放原始碼存放庫。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-532">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="1c2b0-533">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-533">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="1c2b0-534"><xref:security/app-secrets>：包含有關使用環境變數來儲存敏感性資料的建議。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-534"><xref:security/app-secrets>: Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="1c2b0-535">「祕密管理員」使用「檔案設定提供者」以 JSON 檔案在本機系統上存放使用者祕密。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-535">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="1c2b0-536">此主題稍後將說明「檔案設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-536">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="1c2b0-537">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 可安全地儲存 ASP.NET Core 應用程式的應用程式祕密。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-537">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="1c2b0-538">如需詳細資訊，請參閱 <xref:security/key-vault-configuration> 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-538">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="1c2b0-539">階層式設定資料</span><span class="sxs-lookup"><span data-stu-id="1c2b0-539">Hierarchical configuration data</span></span>

<span data-ttu-id="1c2b0-540">設定 API 可在設定機碼中使用分隔符號來壓平合併階層式資料，以管理階層式設定資料。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-540">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="1c2b0-541">在下列 JSON 檔案中，兩個區段的結構式階層中有四個機碼存在：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-541">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="1c2b0-542">當該檔案被讀入設定之後，會建立唯一機碼來維護設定來源的原始階層式資料結構。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-542">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="1c2b0-543">系統會使用冒號 (`:`) 將區段與機碼壓平合併以維護原始結構：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-543">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="1c2b0-544">section0:key0</span><span class="sxs-lookup"><span data-stu-id="1c2b0-544">section0:key0</span></span>
* <span data-ttu-id="1c2b0-545">section0:key1</span><span class="sxs-lookup"><span data-stu-id="1c2b0-545">section0:key1</span></span>
* <span data-ttu-id="1c2b0-546">section1:key0</span><span class="sxs-lookup"><span data-stu-id="1c2b0-546">section1:key0</span></span>
* <span data-ttu-id="1c2b0-547">section1:key1</span><span class="sxs-lookup"><span data-stu-id="1c2b0-547">section1:key1</span></span>

<span data-ttu-id="1c2b0-548"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> 與 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> 方法可用來在設定資料中隔離區段與區段的子系。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-548"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="1c2b0-549">[GetSection,、 GetChildren 與 Exists](#getsection-getchildren-and-exists) 中說明這些方法。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-549">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="1c2b0-550">慣例</span><span class="sxs-lookup"><span data-stu-id="1c2b0-550">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="1c2b0-551">來源和提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-551">Sources and providers</span></span>

<span data-ttu-id="1c2b0-552">在應用程式啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-552">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="1c2b0-553">實作變更偵測的組態提供者能夠在基礎設定變更時重新載入組態。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-553">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="1c2b0-554">例如，檔案組態提供者 (將於本主題稍後討論) 和 [Azure Key Vault 組態提供者](xref:security/key-vault-configuration)均會實作變更偵測。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-554">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="1c2b0-555">您可以在應用程式的[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器中找到 <xref:Microsoft.Extensions.Configuration.IConfiguration>。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-555"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="1c2b0-556"><xref:Microsoft.Extensions.Configuration.IConfiguration>可以插入 Razor 頁面 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> 或 MVC 中 <xref:Microsoft.AspNetCore.Mvc.Controller> ，以取得類別的設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-556"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="1c2b0-557">在下列範例中， `_config` 欄位是用來存取設定值：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-557">In the following examples, the `_config` field is used to access configuration values:</span></span>

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

<span data-ttu-id="1c2b0-558">設定提供者無法使用 DI，因為當它們由主機設定時，它無法使用。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-558">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="1c2b0-559">按鍵</span><span class="sxs-lookup"><span data-stu-id="1c2b0-559">Keys</span></span>

<span data-ttu-id="1c2b0-560">設定機碼會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-560">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="1c2b0-561">機碼區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-561">Keys are case-insensitive.</span></span> <span data-ttu-id="1c2b0-562">例如，`ConnectionString` 與 `connectionstring` 會被視為相等的機碼。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-562">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="1c2b0-563">若相同機碼的值是由相同或不同設定提供者設定，在機碼上設定的最後一個值是所使用的值。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-563">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span> <span data-ttu-id="1c2b0-564">如需重複 JSON 金鑰的詳細資訊，請參閱[此 GitHub 問題](https://github.com/dotnet/extensions/issues/2381)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-564">For more information on duplicate JSON keys, see [this GitHub issue](https://github.com/dotnet/extensions/issues/2381).</span></span>
* <span data-ttu-id="1c2b0-565">階層式機碼</span><span class="sxs-lookup"><span data-stu-id="1c2b0-565">Hierarchical keys</span></span>
  * <span data-ttu-id="1c2b0-566">在設定 API 內，冒號分隔字元 (`:`) 可在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-566">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="1c2b0-567">在環境變數中，冒號分隔字元可能無法在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-567">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="1c2b0-568">所有平台都支援雙底線 (`__`)，且會自動轉換為冒號。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-568">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="1c2b0-569">在 Azure Key Vault 中，階層式機碼使用 `--` (兩個破折號) 來做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-569">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="1c2b0-570">撰寫程式碼，以在將密碼載入應用程式的設定時，以冒號取代破折號。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-570">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="1c2b0-571"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-571">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="1c2b0-572">[將陣列繫結到類別](#bind-an-array-to-a-class)一節說明陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-572">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="1c2b0-573">值</span><span class="sxs-lookup"><span data-stu-id="1c2b0-573">Values</span></span>

<span data-ttu-id="1c2b0-574">設定值會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-574">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="1c2b0-575">值是字串。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-575">Values are strings.</span></span>
* <span data-ttu-id="1c2b0-576">Null 值無法存放在設定中或繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-576">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="1c2b0-577">提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-577">Providers</span></span>

<span data-ttu-id="1c2b0-578">下表顯示可供 ASP.NET Core 應用程式使用的設定提供者。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-578">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="1c2b0-579">提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-579">Provider</span></span> | <span data-ttu-id="1c2b0-580">從&hellip;提供設定</span><span class="sxs-lookup"><span data-stu-id="1c2b0-580">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="1c2b0-581">[Azure Key Vault 設定提供者](xref:security/key-vault-configuration) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="1c2b0-581">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="1c2b0-582">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="1c2b0-582">Azure Key Vault</span></span> |
| <span data-ttu-id="1c2b0-583">[Azure 應用程式組態提供者](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure 文件)</span><span class="sxs-lookup"><span data-stu-id="1c2b0-583">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="1c2b0-584">Azure 應用程式組態</span><span class="sxs-lookup"><span data-stu-id="1c2b0-584">Azure App Configuration</span></span> |
| [<span data-ttu-id="1c2b0-585">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-585">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="1c2b0-586">命令列參數</span><span class="sxs-lookup"><span data-stu-id="1c2b0-586">Command-line parameters</span></span> |
| [<span data-ttu-id="1c2b0-587">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-587">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="1c2b0-588">自訂來源</span><span class="sxs-lookup"><span data-stu-id="1c2b0-588">Custom source</span></span> |
| [<span data-ttu-id="1c2b0-589">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-589">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="1c2b0-590">環境變數</span><span class="sxs-lookup"><span data-stu-id="1c2b0-590">Environment variables</span></span> |
| [<span data-ttu-id="1c2b0-591">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-591">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="1c2b0-592">檔案 (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="1c2b0-592">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="1c2b0-593">每個檔案機碼的設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-593">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="1c2b0-594">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="1c2b0-594">Directory files</span></span> |
| [<span data-ttu-id="1c2b0-595">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-595">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="1c2b0-596">記憶體內集合</span><span class="sxs-lookup"><span data-stu-id="1c2b0-596">In-memory collections</span></span> |
| <span data-ttu-id="1c2b0-597">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="1c2b0-597">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="1c2b0-598">使用者設定檔目錄中的檔案</span><span class="sxs-lookup"><span data-stu-id="1c2b0-598">File in the user profile directory</span></span> |

<span data-ttu-id="1c2b0-599">在啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-599">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="1c2b0-600">本主題所描述的設定提供者會依字母順序描述，而不是程式碼排列它們的順序。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-600">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="1c2b0-601">請在程式碼中訂購設定提供者，以符合應用程式所需之基礎設定來源的優先順序。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-601">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="1c2b0-602">典型的設定提供者順序是：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-602">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="1c2b0-603">Files （*appsettings.json*、 *appsettings. {環境}. json*，其中 `{Environment}` 是應用程式的目前裝載環境）</span><span class="sxs-lookup"><span data-stu-id="1c2b0-603">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="1c2b0-604">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="1c2b0-604">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="1c2b0-605">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (僅限開發環境)</span><span class="sxs-lookup"><span data-stu-id="1c2b0-605">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="1c2b0-606">環境變數</span><span class="sxs-lookup"><span data-stu-id="1c2b0-606">Environment variables</span></span>
1. <span data-ttu-id="1c2b0-607">命令列引數</span><span class="sxs-lookup"><span data-stu-id="1c2b0-607">Command-line arguments</span></span>

<span data-ttu-id="1c2b0-608">將命令列組態提供者放在提供者序列結尾是常見做法，因為這樣可以讓命令列引數覆寫由其他提供者所設定的組態。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-608">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="1c2b0-609">當使用初始化新的主機產生器時，會使用上述的提供者序列 `CreateDefaultBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-609">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="1c2b0-610">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-610">For more information, see the [Default configuration](#default-configuration) section.</span></span>

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="1c2b0-611">使用 UseConfiguration 設定主機建立器</span><span class="sxs-lookup"><span data-stu-id="1c2b0-611">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="1c2b0-612">若要設定主機建立器，請使用該組態在主機建立器上呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-612">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

## <a name="configureappconfiguration"></a><span data-ttu-id="1c2b0-613">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="1c2b0-613">ConfigureAppConfiguration</span></span>

<span data-ttu-id="1c2b0-614">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定提供者，以及由 `CreateDefaultBuilder` 自動新增的設定提供者：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-614">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="1c2b0-615">使用命令列引數覆寫先前的組態</span><span class="sxs-lookup"><span data-stu-id="1c2b0-615">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="1c2b0-616">若要提供可使用命令列引數覆寫的應用程式組態，請最後呼叫 `AddCommandLine`：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-616">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="1c2b0-617">移除 CreateDefaultBuilder 新增的提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-617">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="1c2b0-618">若要移除新增的提供者 `CreateDefaultBuilder` ，請先在[IConfigurationBuilder](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources)上呼叫[Clear](/dotnet/api/system.collections.generic.icollection-1.clear) ：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-618">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="1c2b0-619">在應用程式啟動期間使用組態</span><span class="sxs-lookup"><span data-stu-id="1c2b0-619">Consume configuration during app startup</span></span>

<span data-ttu-id="1c2b0-620">應用程式啟動期間，可以使用 `ConfigureAppConfiguration` 中為應用程式提供的組態，包括 `Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-620">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1c2b0-621">如需詳細資訊，請參閱[在啟動期間存取組態](#access-configuration-during-startup)一節。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-621">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="1c2b0-622">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-622">Command-line Configuration Provider</span></span>

<span data-ttu-id="1c2b0-623"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> 會在執行階段從命令列引數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-623">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="1c2b0-624">為了啟用命令列設定，<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 延伸模組方法會在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-624">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1c2b0-625">呼叫 `CreateDefaultBuilder(string [])` 時，會自動呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-625">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="1c2b0-626">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-626">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="1c2b0-627">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-627">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="1c2b0-628">從 *appsettings.json* 與 *appsettings.{Environment}.json* 檔案載入其他選擇性組態。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-628">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="1c2b0-629">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-629">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="1c2b0-630">環境變數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-630">Environment variables.</span></span>

<span data-ttu-id="1c2b0-631">`CreateDefaultBuilder` 會最後才新增命令列設定提供者。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-631">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="1c2b0-632">在執行階段傳遞的命令列引數會覆寫由其它提供者所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-632">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="1c2b0-633">`CreateDefaultBuilder` 會在建構主機時執行作業。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-633">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="1c2b0-634">因此，由`CreateDefaultBuilder` 啟用的命令列設定可以影響主機的設定方式。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-634">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="1c2b0-635">針對以 ASP.NET Core 範本為基礎的應用程式，`AddCommandLine` 已由 `CreateDefaultBuilder` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-635">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="1c2b0-636">若要新增其他組態提供者並維持能夠使用命令列引數覆寫這些提供者組態的能力，請在 `ConfigureAppConfiguration` 中呼叫應用程式的其他提供者，且最後呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-636">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="1c2b0-637">**範例**</span><span class="sxs-lookup"><span data-stu-id="1c2b0-637">**Example**</span></span>

<span data-ttu-id="1c2b0-638">範例應用程式利用靜態方便方法 `CreateDefaultBuilder` 的優勢來建置主機，這包括對 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-638">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="1c2b0-639">從專案目錄中開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-639">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="1c2b0-640">提供命令列引數給 `dotnet run` 命令 `dotnet run CommandLineKey=CommandLineValue`。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-640">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="1c2b0-641">在應用程式執行之後，開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-641">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="1c2b0-642">觀察輸出是否包含提供給 `dotnet run` 之設定命令列引數的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-642">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="1c2b0-643">引數</span><span class="sxs-lookup"><span data-stu-id="1c2b0-643">Arguments</span></span>

<span data-ttu-id="1c2b0-644">當值後面接著空格時，值必須接著等號 (`=`)，或機碼必須有前置詞 (`--` 或 `/`)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-644">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="1c2b0-645">如果使用等號 (例如 `CommandLineKey=`)，則不需要此值。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-645">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="1c2b0-646">索引鍵前置字元</span><span class="sxs-lookup"><span data-stu-id="1c2b0-646">Key prefix</span></span>               | <span data-ttu-id="1c2b0-647">範例</span><span class="sxs-lookup"><span data-stu-id="1c2b0-647">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="1c2b0-648">沒有前置字元</span><span class="sxs-lookup"><span data-stu-id="1c2b0-648">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="1c2b0-649">雙虛線 (`--`)</span><span class="sxs-lookup"><span data-stu-id="1c2b0-649">Two dashes (`--`)</span></span>        | <span data-ttu-id="1c2b0-650">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="1c2b0-650">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="1c2b0-651">正斜線 (`/`)</span><span class="sxs-lookup"><span data-stu-id="1c2b0-651">Forward slash (`/`)</span></span>      | <span data-ttu-id="1c2b0-652">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="1c2b0-652">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="1c2b0-653">在相同的命令中，請不要混合使用等號搭配使用空格之機碼值組的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-653">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="1c2b0-654">範例命令：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-654">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="1c2b0-655">切換對應</span><span class="sxs-lookup"><span data-stu-id="1c2b0-655">Switch mappings</span></span>

<span data-ttu-id="1c2b0-656">參數對應允許索引鍵名稱取代邏輯。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-656">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="1c2b0-657">當以手動方式建立設定時 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> ，請提供方法的切換取代的字典 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-657">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="1c2b0-658">使用切換對應字典時，會檢查字典中是否有任何索引鍵符合命令列引數所提供的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-658">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="1c2b0-659">如果在字典中找到命令列索引鍵，則會傳回字典值 (索引鍵取代) 以在應用程式的設定中設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-659">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="1c2b0-660">所有前面加上單虛線 (`-`) 的命令列索引鍵都需要切換對應。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-660">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="1c2b0-661">切換對應字典索引鍵規則：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-661">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="1c2b0-662">切換必須以單虛線 (`-`) 或雙虛線 (`--`) 開頭。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-662">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="1c2b0-663">切換對應字典不能包含重複索引鍵。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-663">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="1c2b0-664">建立切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-664">Create a switch mappings dictionary.</span></span> <span data-ttu-id="1c2b0-665">下列範例會建立兩個切換對應：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-665">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="1c2b0-666">建立主機時，請使用切換對應字典呼叫 `AddCommandLine`：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-666">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="1c2b0-667">針對使用切換對應的應用程式，呼叫 `CreateDefaultBuilder` 不應傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-667">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="1c2b0-668">`CreateDefaultBuilder` 方法的 `AddCommandLine` 呼叫不包括對應的切換，且沒有任何方式可以將切換對應字典傳遞給 `CreateDefaultBuilder`。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-668">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="1c2b0-669">解決方案並非將引數傳遞給 `CreateDefaultBuilder`，而是允許 `ConfigurationBuilder` 方法的 `AddCommandLine` 方法同時處理引數與切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-669">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="1c2b0-670">建立切換對應字典之後，它會包含下表中所示的資料。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-670">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="1c2b0-671">Key</span><span class="sxs-lookup"><span data-stu-id="1c2b0-671">Key</span></span>       | <span data-ttu-id="1c2b0-672">值</span><span class="sxs-lookup"><span data-stu-id="1c2b0-672">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="1c2b0-673">若啟動應用程式時使用切換對應機碼，設定會接收由字典提供之機碼上的設定值：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-673">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="1c2b0-674">執行上述命令之後，設定包含下表中顯示的值。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-674">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="1c2b0-675">Key</span><span class="sxs-lookup"><span data-stu-id="1c2b0-675">Key</span></span>               | <span data-ttu-id="1c2b0-676">值</span><span class="sxs-lookup"><span data-stu-id="1c2b0-676">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="1c2b0-677">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-677">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="1c2b0-678"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> 會在執行階段從環境變數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-678">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="1c2b0-679">若要啟用環境變數設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-679">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="1c2b0-680">[Azure App Service](https://azure.microsoft.com/services/app-service/)允許在 Azure 入口網站中設定環境變數，以使用環境變數設定提供者來覆寫應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-680">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="1c2b0-681">如需詳細資訊，請參閱 [Azure App：使用 Azure 入口網站覆寫應用程式設定](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-681">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="1c2b0-682">使用 [Web 主機](xref:fundamentals/host/web-host)初始化新的主機建立器並呼叫 `CreateDefaultBuilder` 時，可使用 `AddEnvironmentVariables` 為[主機組態](#host-versus-app-configuration)載入字首為 `ASPNETCORE_` 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-682">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="1c2b0-683">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-683">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="1c2b0-684">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-684">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="1c2b0-685">來自無首碼之環境變數的應用程式組態 (在未提供首碼的情況下呼叫 `AddEnvironmentVariables`)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-685">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="1c2b0-686">從 *appsettings.json* 與 *appsettings.{Environment}.json* 檔案載入其他選擇性組態。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-686">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="1c2b0-687">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-687">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="1c2b0-688">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-688">Command-line arguments.</span></span>

<span data-ttu-id="1c2b0-689">從使用者祕密與 *appsettings* 檔案建立設定之後，會呼叫「環境變數設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-689">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="1c2b0-690">在此位置呼叫提供者可讓系統在執行階段讀取環境變數，以覆寫由使用者祕密與 *appsettings* 檔案所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-690">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="1c2b0-691">若要從其他環境變數提供應用程式設定，請在中呼叫應用程式的其他提供者， `ConfigureAppConfiguration` 並 `AddEnvironmentVariables` 使用前置詞呼叫：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-691">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="1c2b0-692">`AddEnvironmentVariables`最後呼叫以允許具有指定前置詞的環境變數，覆寫其他提供者的值。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-692">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="1c2b0-693">**範例**</span><span class="sxs-lookup"><span data-stu-id="1c2b0-693">**Example**</span></span>

<span data-ttu-id="1c2b0-694">範例應用程式利用靜態方便方法 `CreateDefaultBuilder` 的優勢來建置主機，這包括對 `AddEnvironmentVariables` 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-694">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="1c2b0-695">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-695">Run the sample app.</span></span> <span data-ttu-id="1c2b0-696">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-696">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="1c2b0-697">觀察輸出是否包含環境變數 `ENVIRONMENT` 的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-697">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="1c2b0-698">值反映應用程式執行所在環境，在本機執行時，通常是 `Development`。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-698">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="1c2b0-699">為縮短由應用程式轉譯的環境變數清單，應用程式會篩選環境變數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-699">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="1c2b0-700">請參閱範例應用程式的 *Pages/Index.cshtml.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-700">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="1c2b0-701">若要公開應用程式可用的所有環境變數，請將 `FilteredConfiguration` *Pages/Index. cshtml*中的變更為下列內容：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-701">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="1c2b0-702">首碼</span><span class="sxs-lookup"><span data-stu-id="1c2b0-702">Prefixes</span></span>

<span data-ttu-id="1c2b0-703">在方法中提供前置詞時，會篩選載入應用程式設定中的環境變數 `AddEnvironmentVariables` 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-703">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="1c2b0-704">例如，若要篩選前置詞為 `CUSTOM_` 的環境變數，請提供前置詞給設定提供者：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-704">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="1c2b0-705">建立設定機碼值組時，會移除前置詞。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-705">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="1c2b0-706">建立主機建立器時，主機組態由環境變數提供。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-706">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="1c2b0-707">如需這些環境變數所用前置詞的詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-707">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="1c2b0-708">**連接字串前置詞**</span><span class="sxs-lookup"><span data-stu-id="1c2b0-708">**Connection string prefixes**</span></span>

<span data-ttu-id="1c2b0-709">設定 API 四個連接字串環境變數的特殊處理規則，這些這些環境變數牽涉到針對應用程式環境設定 Azure 連接字串。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-709">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="1c2b0-710">若將前置詞提供給 `AddEnvironmentVariables`具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-710">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="1c2b0-711">連接字串前置詞</span><span class="sxs-lookup"><span data-stu-id="1c2b0-711">Connection string prefix</span></span> | <span data-ttu-id="1c2b0-712">提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-712">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="1c2b0-713">自訂提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-713">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="1c2b0-714">MySQL</span><span class="sxs-lookup"><span data-stu-id="1c2b0-714">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="1c2b0-715">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="1c2b0-715">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="1c2b0-716">SQL Server</span><span class="sxs-lookup"><span data-stu-id="1c2b0-716">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="1c2b0-717">當探索到具有下表所顯示之任何四個前置詞的環境變數並將其載入到設定中時：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-717">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="1c2b0-718">會透過移除環境變數前置詞並新增設定機碼區段 (`ConnectionStrings`) 來移除具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-718">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="1c2b0-719">會建立新的設定機碼值組以代表資料庫連線提供者 (`CUSTOMCONNSTR_` 除外，這沒有所述提供者)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-719">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="1c2b0-720">環境變數機碼</span><span class="sxs-lookup"><span data-stu-id="1c2b0-720">Environment variable key</span></span> | <span data-ttu-id="1c2b0-721">已轉換的設定機碼</span><span class="sxs-lookup"><span data-stu-id="1c2b0-721">Converted configuration key</span></span> | <span data-ttu-id="1c2b0-722">提供者設定項目</span><span class="sxs-lookup"><span data-stu-id="1c2b0-722">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="1c2b0-723">設定項目未建立。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-723">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="1c2b0-724">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-724">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="1c2b0-725">值：`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="1c2b0-725">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="1c2b0-726">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-726">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="1c2b0-727">值：`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="1c2b0-727">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="1c2b0-728">機碼：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-728">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="1c2b0-729">值：`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="1c2b0-729">Value: `System.Data.SqlClient`</span></span>  |

<span data-ttu-id="1c2b0-730">**範例**</span><span class="sxs-lookup"><span data-stu-id="1c2b0-730">**Example**</span></span>

<span data-ttu-id="1c2b0-731">在伺服器上建立自訂連接字串環境變數：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-731">A custom connection string environment variable is created on the server:</span></span>

* <span data-ttu-id="1c2b0-732">名稱：`CUSTOMCONNSTR_ReleaseDB`</span><span class="sxs-lookup"><span data-stu-id="1c2b0-732">Name: `CUSTOMCONNSTR_ReleaseDB`</span></span>
* <span data-ttu-id="1c2b0-733">值：`Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span><span class="sxs-lookup"><span data-stu-id="1c2b0-733">Value: `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span></span>

<span data-ttu-id="1c2b0-734">如果 `IConfiguration` 插入，並將其指派給名為的欄位 `_config` ，請閱讀值：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-734">If `IConfiguration` is injected and assigned to a field named `_config`, read the value:</span></span>

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a><span data-ttu-id="1c2b0-735">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-735">File Configuration Provider</span></span>

<span data-ttu-id="1c2b0-736"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> 是用於從檔案系統載入設定的基底類別。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-736"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="1c2b0-737">下列設定提供者專用於特定檔案類型：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-737">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="1c2b0-738">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-738">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="1c2b0-739">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-739">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="1c2b0-740">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-740">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="1c2b0-741">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-741">INI Configuration Provider</span></span>

<span data-ttu-id="1c2b0-742"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> 會在執行階段從 INI 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-742">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="1c2b0-743">若要啟用 INI 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-743">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1c2b0-744">冒號可用來做為 INI 檔案設定中的區段分隔符號。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-744">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="1c2b0-745">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-745">Overloads permit specifying:</span></span>

* <span data-ttu-id="1c2b0-746">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-746">Whether the file is optional.</span></span>
* <span data-ttu-id="1c2b0-747">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-747">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="1c2b0-748"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-748">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="1c2b0-749">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-749">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="1c2b0-750">INI 設定檔的一般範例：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-750">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="1c2b0-751">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-751">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="1c2b0-752">section0:key0</span><span class="sxs-lookup"><span data-stu-id="1c2b0-752">section0:key0</span></span>
* <span data-ttu-id="1c2b0-753">section0:key1</span><span class="sxs-lookup"><span data-stu-id="1c2b0-753">section0:key1</span></span>
* <span data-ttu-id="1c2b0-754">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="1c2b0-754">section1:subsection:key</span></span>
* <span data-ttu-id="1c2b0-755">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="1c2b0-755">section2:subsection0:key</span></span>
* <span data-ttu-id="1c2b0-756">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="1c2b0-756">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="1c2b0-757">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-757">JSON Configuration Provider</span></span>

<span data-ttu-id="1c2b0-758"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> 會在執行階段從 JSON 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-758">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="1c2b0-759">若要啟用 JSON 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-759">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1c2b0-760">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-760">Overloads permit specifying:</span></span>

* <span data-ttu-id="1c2b0-761">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-761">Whether the file is optional.</span></span>
* <span data-ttu-id="1c2b0-762">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-762">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="1c2b0-763"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-763">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="1c2b0-764">`AddJsonFile`當使用初始化新的主機產生器時，會自動呼叫兩次 `CreateDefaultBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-764">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="1c2b0-765">會呼叫此方法以從下列位置載入設定：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-765">The method is called to load configuration from:</span></span>

* <span data-ttu-id="1c2b0-766">*appsettings.js開啟*：先讀取此檔案。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-766">*appsettings.json*: This file is read first.</span></span> <span data-ttu-id="1c2b0-767">檔案的環境版本可以覆寫由 *appsettings.json* 檔案提供的值。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-767">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="1c2b0-768">*appsettings。{環境}. json*：檔案的環境版本是根據[IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)載入。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-768">*appsettings.{Environment}.json*: The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="1c2b0-769">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-769">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="1c2b0-770">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-770">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="1c2b0-771">環境變數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-771">Environment variables.</span></span>
* <span data-ttu-id="1c2b0-772">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-772">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="1c2b0-773">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-773">Command-line arguments.</span></span>

<span data-ttu-id="1c2b0-774">會先建立 JSON 設定提供者。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-774">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="1c2b0-775">因此，使用者祕密、環境變數與命令列引數會覆寫由 *appsettings* 檔案設定的設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-775">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="1c2b0-776">在建置主機時，呼叫 `ConfigureAppConfiguration` 以指定檔案 (除了 *appsettings.json* 和 \* appsettings.{Environment}.json\* 以外) 的應用程式設定：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-776">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="1c2b0-777">**範例**</span><span class="sxs-lookup"><span data-stu-id="1c2b0-777">**Example**</span></span>

<span data-ttu-id="1c2b0-778">範例應用程式會利用靜態便利方法 `CreateDefaultBuilder` 來建立主機，其中包含兩個對的呼叫 `AddJsonFile` ：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-778">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

* <span data-ttu-id="1c2b0-779">第一次呼叫會 `AddJsonFile` 從*appsettings.js*載入設定：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-779">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="1c2b0-780">第二個呼叫會 `AddJsonFile` 從 appsettings 載入設定 *。 {環境}. json*。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-780">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="1c2b0-781">針對範例應用程式中的*appsettings.Development.js* ，會載入下列檔案：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-781">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="1c2b0-782">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-782">Run the sample app.</span></span> <span data-ttu-id="1c2b0-783">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-783">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="1c2b0-784">輸出包含以應用程式環境為基礎之設定的索引鍵/值組。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-784">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="1c2b0-785">金鑰的記錄層級 `Logging:LogLevel:Default` 是在 `Debug` 開發環境中執行應用程式時。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-785">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="1c2b0-786">在生產環境中再次執行範例應用程式：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-786">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="1c2b0-787">開啟 [*屬性]/[launchSettings.js*檔案]。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-787">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="1c2b0-788">在 `ConfigurationSample` 設定檔中，將環境變數的值變更 `ASPNETCORE_ENVIRONMENT` 為 `Production` 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-788">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="1c2b0-789">儲存檔案，並 `dotnet run` 在命令 shell 中使用來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-789">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="1c2b0-790">*appsettings.Development.js*中的設定不會再覆寫中*appsettings.js*的設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-790">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="1c2b0-791">索引鍵的記錄層級 `Logging:LogLevel:Default` 是 `Warning` 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-791">The log level for the key `Logging:LogLevel:Default` is `Warning`.</span></span>

### <a name="xml-configuration-provider"></a><span data-ttu-id="1c2b0-792">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-792">XML Configuration Provider</span></span>

<span data-ttu-id="1c2b0-793"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> 會在執行階段從 XML 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-793">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="1c2b0-794">若要啟用 XML 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-794">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1c2b0-795">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-795">Overloads permit specifying:</span></span>

* <span data-ttu-id="1c2b0-796">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-796">Whether the file is optional.</span></span>
* <span data-ttu-id="1c2b0-797">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-797">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="1c2b0-798"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-798">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="1c2b0-799">建立設定機碼值組時，會忽略設定檔案的根節點。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-799">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="1c2b0-800">請勿在檔案中指定文件類型定義 (DTD) 或命名空間。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-800">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="1c2b0-801">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-801">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="1c2b0-802">XML 設定檔可以針對重複的區段使用相異元素名稱：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-802">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="1c2b0-803">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-803">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="1c2b0-804">section0:key0</span><span class="sxs-lookup"><span data-stu-id="1c2b0-804">section0:key0</span></span>
* <span data-ttu-id="1c2b0-805">section0:key1</span><span class="sxs-lookup"><span data-stu-id="1c2b0-805">section0:key1</span></span>
* <span data-ttu-id="1c2b0-806">section1:key0</span><span class="sxs-lookup"><span data-stu-id="1c2b0-806">section1:key0</span></span>
* <span data-ttu-id="1c2b0-807">section1:key1</span><span class="sxs-lookup"><span data-stu-id="1c2b0-807">section1:key1</span></span>

<span data-ttu-id="1c2b0-808">若 `name` 屬性是用來區別元素，則可以重複那些使用相同元素名稱的元素：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-808">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="1c2b0-809">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-809">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="1c2b0-810">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="1c2b0-810">section:section0:key:key0</span></span>
* <span data-ttu-id="1c2b0-811">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="1c2b0-811">section:section0:key:key1</span></span>
* <span data-ttu-id="1c2b0-812">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="1c2b0-812">section:section1:key:key0</span></span>
* <span data-ttu-id="1c2b0-813">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="1c2b0-813">section:section1:key:key1</span></span>

<span data-ttu-id="1c2b0-814">屬性可用來提供值：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-814">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="1c2b0-815">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-815">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="1c2b0-816">key:attribute</span><span class="sxs-lookup"><span data-stu-id="1c2b0-816">key:attribute</span></span>
* <span data-ttu-id="1c2b0-817">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="1c2b0-817">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="1c2b0-818">每個檔案機碼設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-818">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="1c2b0-819"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> 使用目錄的檔案做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-819">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="1c2b0-820">機碼是檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-820">The key is the file name.</span></span> <span data-ttu-id="1c2b0-821">值包含檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-821">The value contains the file's contents.</span></span> <span data-ttu-id="1c2b0-822">每個檔案機碼設定提供者是在 Docker 主機案例中使用。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-822">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="1c2b0-823">若要啟用每個檔案機碼設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-823">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="1c2b0-824">檔案的 `directoryPath` 必須是絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-824">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="1c2b0-825">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-825">Overloads permit specifying:</span></span>

* <span data-ttu-id="1c2b0-826">設定來源的 `Action<KeyPerFileConfigurationSource>` 委派。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-826">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="1c2b0-827">目錄是否為選擇性與目錄的路徑。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-827">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="1c2b0-828">雙底線 (`__`) 是做為檔案名稱中的設定金鑰分隔符號使用。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-828">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="1c2b0-829">例如，檔案名稱 `Logging__LogLevel__System` 會產生設定金鑰 `Logging:LogLevel:System`。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-829">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="1c2b0-830">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-830">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="1c2b0-831">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-831">Memory Configuration Provider</span></span>

<span data-ttu-id="1c2b0-832"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> 使用記憶體內集合做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-832">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="1c2b0-833">若要啟用記憶體內集合設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-833">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1c2b0-834">設定提供者可以使用 `IEnumerable<KeyValuePair<String,String>>` 來初始化。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-834">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="1c2b0-835">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-835">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="1c2b0-836">下列範例會建立組態字典：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-836">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="1c2b0-837">字典會與 `AddInMemoryCollection` 的呼叫搭配使用以提供組態：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-837">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="1c2b0-838">GetValue</span><span class="sxs-lookup"><span data-stu-id="1c2b0-838">GetValue</span></span>

<span data-ttu-id="1c2b0-839">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*)使用指定的索引鍵從設定中解壓縮單一值，並將其轉換為指定的 noncollection 類型。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-839">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="1c2b0-840">多載會接受預設值。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-840">An overload accepts a default value.</span></span>

<span data-ttu-id="1c2b0-841">下列範例將：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-841">The following example:</span></span>

* <span data-ttu-id="1c2b0-842">從具有機碼 `NumberKey` 的組態擷取字串值。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-842">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="1c2b0-843">若在組態機碼中找不到 `NumberKey`，則會使用預設值 `99`。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-843">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="1c2b0-844">鍵入值為 `int`。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-844">Types the value as an `int`.</span></span>
* <span data-ttu-id="1c2b0-845">在 `NumberConfig` 屬性中儲存值供頁面使用。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-845">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="1c2b0-846">GetSection、GetChildren 與 Exists</span><span class="sxs-lookup"><span data-stu-id="1c2b0-846">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="1c2b0-847">針對下面的範例，請考慮下列 JSON 檔案。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-847">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="1c2b0-848">在兩個區段中找到四個機碼，其中一個包括子區段組：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-848">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="1c2b0-849">當檔案被讀入到設定之後，會建立下列唯一階層式機碼以存放設定值：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-849">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="1c2b0-850">section0:key0</span><span class="sxs-lookup"><span data-stu-id="1c2b0-850">section0:key0</span></span>
* <span data-ttu-id="1c2b0-851">section0:key1</span><span class="sxs-lookup"><span data-stu-id="1c2b0-851">section0:key1</span></span>
* <span data-ttu-id="1c2b0-852">section1:key0</span><span class="sxs-lookup"><span data-stu-id="1c2b0-852">section1:key0</span></span>
* <span data-ttu-id="1c2b0-853">section1:key1</span><span class="sxs-lookup"><span data-stu-id="1c2b0-853">section1:key1</span></span>
* <span data-ttu-id="1c2b0-854">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="1c2b0-854">section2:subsection0:key0</span></span>
* <span data-ttu-id="1c2b0-855">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="1c2b0-855">section2:subsection0:key1</span></span>
* <span data-ttu-id="1c2b0-856">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="1c2b0-856">section2:subsection1:key0</span></span>
* <span data-ttu-id="1c2b0-857">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="1c2b0-857">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="1c2b0-858">GetSection</span><span class="sxs-lookup"><span data-stu-id="1c2b0-858">GetSection</span></span>

<span data-ttu-id="1c2b0-859">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) 會擷取具有所指定子區段機碼的設定子區段。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-859">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="1c2b0-860">若要傳回 `section1` 中只包含一個機碼值組的 <xref:Microsoft.Extensions.Configuration.IConfigurationSection>，請呼叫 `GetSection` 並提供區段名稱：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-860">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="1c2b0-861">`configSection` 沒有值，只有索引鍵和路徑。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-861">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="1c2b0-862">同樣地，若要取得 `section2:subsection0` 中之機碼的值，請呼叫 `GetSection` 並提供區段路徑：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-862">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="1c2b0-863">`GetSection` 絕不會傳回 `null`。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-863">`GetSection` never returns `null`.</span></span> <span data-ttu-id="1c2b0-864">若找不到相符的區段，會傳回空的 `IConfigurationSection`。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-864">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="1c2b0-865">當 `GetSection` 傳回相符區段時，未填入 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value>。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-865">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="1c2b0-866">當區段存在時，會傳回 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> 與 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path>。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-866">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="1c2b0-867">GetChildren</span><span class="sxs-lookup"><span data-stu-id="1c2b0-867">GetChildren</span></span>

<span data-ttu-id="1c2b0-868">對 `section2` 上之 [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) 的呼叫會取得包括下列項目的 `IEnumerable<IConfigurationSection>`：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-868">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="1c2b0-869">Exists</span><span class="sxs-lookup"><span data-stu-id="1c2b0-869">Exists</span></span>

<span data-ttu-id="1c2b0-870">使用 [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) 來判斷設定區段是否存在：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-870">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="1c2b0-871">以範例資料為例， `sectionExists` 是 `false`，因未設定資料中沒有 `section2:subsection2` 區段。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-871">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="1c2b0-872">繫結至物件圖形</span><span class="sxs-lookup"><span data-stu-id="1c2b0-872">Bind to an object graph</span></span>

<span data-ttu-id="1c2b0-873"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 可以繫結整個 POCO 物件圖形。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-873"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="1c2b0-874">如同系結簡單的物件，只會系結公用讀取/寫入屬性。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-874">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="1c2b0-875">範例包含 `TvShow` 模型，其物件圖形包括 `Metadata` 與 `Actors` 類別 (*Models/TvShow.cs*)：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-875">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="1c2b0-876">範例應用程式有 *tvshow.xml* 檔案，其中包含設定資料：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-876">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="1c2b0-877">設定會被繫結到整個 `TvShow` 物件 (使用 `Bind` 方法)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-877">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="1c2b0-878">已繫結的執行個體會被指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-878">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="1c2b0-879">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*)系結並傳回指定的型別。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-879">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="1c2b0-880">`Get<T>` 比使用 `Bind` 更方便。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-880">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="1c2b0-881">下列程式碼示範如何 `Get<T>` 在上述範例中使用：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-881">The following code shows how to use `Get<T>` with the preceding example:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="1c2b0-882">將陣列繫結到類別</span><span class="sxs-lookup"><span data-stu-id="1c2b0-882">Bind an array to a class</span></span>

<span data-ttu-id="1c2b0-883">範例應用程式示範此節中解釋的概念。\*\*</span><span class="sxs-lookup"><span data-stu-id="1c2b0-883">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="1c2b0-884"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-884">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="1c2b0-885">公開數值索引鍵區段（、、）的任何陣列格式， `:0:` `:1:` 都能夠系結 &hellip; `:{n}:` 至 POCO 類別陣列的陣列。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-885">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="1c2b0-886">繫結是由慣例提供。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-886">Binding is provided by convention.</span></span> <span data-ttu-id="1c2b0-887">自訂設定提供者不需要實作陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-887">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="1c2b0-888">**記憶體內陣列處理**</span><span class="sxs-lookup"><span data-stu-id="1c2b0-888">**In-memory array processing**</span></span>

<span data-ttu-id="1c2b0-889">考慮下表中顯示的設定機碼與值。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-889">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="1c2b0-890">Key</span><span class="sxs-lookup"><span data-stu-id="1c2b0-890">Key</span></span>             | <span data-ttu-id="1c2b0-891">值</span><span class="sxs-lookup"><span data-stu-id="1c2b0-891">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="1c2b0-892">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="1c2b0-892">array:entries:0</span></span> | <span data-ttu-id="1c2b0-893">value0</span><span class="sxs-lookup"><span data-stu-id="1c2b0-893">value0</span></span> |
| <span data-ttu-id="1c2b0-894">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="1c2b0-894">array:entries:1</span></span> | <span data-ttu-id="1c2b0-895">value1</span><span class="sxs-lookup"><span data-stu-id="1c2b0-895">value1</span></span> |
| <span data-ttu-id="1c2b0-896">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="1c2b0-896">array:entries:2</span></span> | <span data-ttu-id="1c2b0-897">value2</span><span class="sxs-lookup"><span data-stu-id="1c2b0-897">value2</span></span> |
| <span data-ttu-id="1c2b0-898">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="1c2b0-898">array:entries:4</span></span> | <span data-ttu-id="1c2b0-899">value4</span><span class="sxs-lookup"><span data-stu-id="1c2b0-899">value4</span></span> |
| <span data-ttu-id="1c2b0-900">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="1c2b0-900">array:entries:5</span></span> | <span data-ttu-id="1c2b0-901">value5</span><span class="sxs-lookup"><span data-stu-id="1c2b0-901">value5</span></span> |

<span data-ttu-id="1c2b0-902">這些機碼與值是使用「記憶體設定提供者」在範例應用程式中載入：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-902">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

<span data-ttu-id="1c2b0-903">陣列會跳過索引 &num;3 的值。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-903">The array skips a value for index &num;3.</span></span> <span data-ttu-id="1c2b0-904">設定繫結程式無法繫結 Null 值或在已繫結的物件中建立 Null 項目，這在示範繫結此陣列的結果到某物件的時候已經很清楚。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-904">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="1c2b0-905">在範例應用程式中，POCO 類別可用來存放繫結設定資料：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-905">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="1c2b0-906">設定資料已繫結到物件：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-906">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="1c2b0-907">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*)語法也可以使用，以產生更精簡的程式碼：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-907">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="1c2b0-908">繫結物件 (`ArrayExample`的執行個體) 會從設定接收陣列資料。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-908">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="1c2b0-909">`ArrayExample.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="1c2b0-909">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="1c2b0-910">`ArrayExample.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="1c2b0-910">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="1c2b0-911">0</span><span class="sxs-lookup"><span data-stu-id="1c2b0-911">0</span></span>                            | <span data-ttu-id="1c2b0-912">value0</span><span class="sxs-lookup"><span data-stu-id="1c2b0-912">value0</span></span>                       |
| <span data-ttu-id="1c2b0-913">1</span><span class="sxs-lookup"><span data-stu-id="1c2b0-913">1</span></span>                            | <span data-ttu-id="1c2b0-914">value1</span><span class="sxs-lookup"><span data-stu-id="1c2b0-914">value1</span></span>                       |
| <span data-ttu-id="1c2b0-915">2</span><span class="sxs-lookup"><span data-stu-id="1c2b0-915">2</span></span>                            | <span data-ttu-id="1c2b0-916">value2</span><span class="sxs-lookup"><span data-stu-id="1c2b0-916">value2</span></span>                       |
| <span data-ttu-id="1c2b0-917">3</span><span class="sxs-lookup"><span data-stu-id="1c2b0-917">3</span></span>                            | <span data-ttu-id="1c2b0-918">value4</span><span class="sxs-lookup"><span data-stu-id="1c2b0-918">value4</span></span>                       |
| <span data-ttu-id="1c2b0-919">4</span><span class="sxs-lookup"><span data-stu-id="1c2b0-919">4</span></span>                            | <span data-ttu-id="1c2b0-920">value5</span><span class="sxs-lookup"><span data-stu-id="1c2b0-920">value5</span></span>                       |

<span data-ttu-id="1c2b0-921">繫結物件中的索引 &num;3 存放 `array:4` 設定機碼與其 `value4` 的設定資料。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-921">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="1c2b0-922">當繫結包含陣列的設定資料時，設定機碼中的陣列索引只會用來列舉設定資料 (當建立物件時)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-922">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="1c2b0-923">設定資料中不能保留 Null 值，當設定機碼中的陣列略過一或多個索引時，不會在繫結物件中建立 Null 值項目。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-923">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="1c2b0-924">由會在設定中產生正確機碼值組的任何設定提供者繫結到 `ArrayExample` 執行個體之前，可以提供索引 &num;3 缺少的設定項目。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-924">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="1c2b0-925">若範例包含具有缺少之機碼值組的額外 JSON 設定提供者，`ArrayExample.Entries` 會符合完整的設定陣列：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-925">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="1c2b0-926">*missing_value.json*：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-926">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="1c2b0-927">在 `ConfigureAppConfiguration` 中：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-927">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="1c2b0-928">表格中顯示的機碼值組會載入到設定中。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-928">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="1c2b0-929">Key</span><span class="sxs-lookup"><span data-stu-id="1c2b0-929">Key</span></span>             | <span data-ttu-id="1c2b0-930">值</span><span class="sxs-lookup"><span data-stu-id="1c2b0-930">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="1c2b0-931">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="1c2b0-931">array:entries:3</span></span> | <span data-ttu-id="1c2b0-932">value3</span><span class="sxs-lookup"><span data-stu-id="1c2b0-932">value3</span></span> |

<span data-ttu-id="1c2b0-933">在「JSON 設定提供者」包含索引 &num;3 的項目之後，若繫結 `ArrayExample` 類別執行個體，`ArrayExample.Entries` 陣列會包括這些值：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-933">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="1c2b0-934">`ArrayExample.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="1c2b0-934">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="1c2b0-935">`ArrayExample.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="1c2b0-935">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="1c2b0-936">0</span><span class="sxs-lookup"><span data-stu-id="1c2b0-936">0</span></span>                            | <span data-ttu-id="1c2b0-937">value0</span><span class="sxs-lookup"><span data-stu-id="1c2b0-937">value0</span></span>                       |
| <span data-ttu-id="1c2b0-938">1</span><span class="sxs-lookup"><span data-stu-id="1c2b0-938">1</span></span>                            | <span data-ttu-id="1c2b0-939">value1</span><span class="sxs-lookup"><span data-stu-id="1c2b0-939">value1</span></span>                       |
| <span data-ttu-id="1c2b0-940">2</span><span class="sxs-lookup"><span data-stu-id="1c2b0-940">2</span></span>                            | <span data-ttu-id="1c2b0-941">value2</span><span class="sxs-lookup"><span data-stu-id="1c2b0-941">value2</span></span>                       |
| <span data-ttu-id="1c2b0-942">3</span><span class="sxs-lookup"><span data-stu-id="1c2b0-942">3</span></span>                            | <span data-ttu-id="1c2b0-943">value3</span><span class="sxs-lookup"><span data-stu-id="1c2b0-943">value3</span></span>                       |
| <span data-ttu-id="1c2b0-944">4</span><span class="sxs-lookup"><span data-stu-id="1c2b0-944">4</span></span>                            | <span data-ttu-id="1c2b0-945">value4</span><span class="sxs-lookup"><span data-stu-id="1c2b0-945">value4</span></span>                       |
| <span data-ttu-id="1c2b0-946">5</span><span class="sxs-lookup"><span data-stu-id="1c2b0-946">5</span></span>                            | <span data-ttu-id="1c2b0-947">value5</span><span class="sxs-lookup"><span data-stu-id="1c2b0-947">value5</span></span>                       |

<span data-ttu-id="1c2b0-948">**JSON 陣列處理**</span><span class="sxs-lookup"><span data-stu-id="1c2b0-948">**JSON array processing**</span></span>

<span data-ttu-id="1c2b0-949">若 JSON 檔案包含陣列，會為具有以零為基礎之區段索引的陣列元素建立設定機碼。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-949">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="1c2b0-950">在下列設定檔中，`subsection` 是陣列：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-950">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="1c2b0-951">「JSON 設定提供者」會將設定資料讀入到下列機碼值組：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-951">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="1c2b0-952">Key</span><span class="sxs-lookup"><span data-stu-id="1c2b0-952">Key</span></span>                     | <span data-ttu-id="1c2b0-953">值</span><span class="sxs-lookup"><span data-stu-id="1c2b0-953">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="1c2b0-954">json_array:key</span><span class="sxs-lookup"><span data-stu-id="1c2b0-954">json_array:key</span></span>          | <span data-ttu-id="1c2b0-955">valueA</span><span class="sxs-lookup"><span data-stu-id="1c2b0-955">valueA</span></span> |
| <span data-ttu-id="1c2b0-956">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="1c2b0-956">json_array:subsection:0</span></span> | <span data-ttu-id="1c2b0-957">valueB</span><span class="sxs-lookup"><span data-stu-id="1c2b0-957">valueB</span></span> |
| <span data-ttu-id="1c2b0-958">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="1c2b0-958">json_array:subsection:1</span></span> | <span data-ttu-id="1c2b0-959">valueC</span><span class="sxs-lookup"><span data-stu-id="1c2b0-959">valueC</span></span> |
| <span data-ttu-id="1c2b0-960">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="1c2b0-960">json_array:subsection:2</span></span> | <span data-ttu-id="1c2b0-961">valueD</span><span class="sxs-lookup"><span data-stu-id="1c2b0-961">valueD</span></span> |

<span data-ttu-id="1c2b0-962">在範例應用程式中，下列 POCO 類別可用來繫結設定機碼值組：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-962">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="1c2b0-963">在繫結之後，`JsonArrayExample.Key` 會存放值 `valueA`。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-963">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="1c2b0-964">子區段值會存放在 POCO 陣列屬性 `Subsection` 中。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-964">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="1c2b0-965">`JsonArrayExample.Subsection` 索引</span><span class="sxs-lookup"><span data-stu-id="1c2b0-965">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="1c2b0-966">`JsonArrayExample.Subsection` 值</span><span class="sxs-lookup"><span data-stu-id="1c2b0-966">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="1c2b0-967">0</span><span class="sxs-lookup"><span data-stu-id="1c2b0-967">0</span></span>                                   | <span data-ttu-id="1c2b0-968">valueB</span><span class="sxs-lookup"><span data-stu-id="1c2b0-968">valueB</span></span>                              |
| <span data-ttu-id="1c2b0-969">1</span><span class="sxs-lookup"><span data-stu-id="1c2b0-969">1</span></span>                                   | <span data-ttu-id="1c2b0-970">valueC</span><span class="sxs-lookup"><span data-stu-id="1c2b0-970">valueC</span></span>                              |
| <span data-ttu-id="1c2b0-971">2</span><span class="sxs-lookup"><span data-stu-id="1c2b0-971">2</span></span>                                   | <span data-ttu-id="1c2b0-972">valueD</span><span class="sxs-lookup"><span data-stu-id="1c2b0-972">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="1c2b0-973">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="1c2b0-973">Custom configuration provider</span></span>

<span data-ttu-id="1c2b0-974">範例應用程式示範如何建立會使用 [Entity Framework (EF)](/ef/core/) 從資料庫讀取設定機碼值組的基本設定提供者。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-974">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="1c2b0-975">提供者具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-975">The provider has the following characteristics:</span></span>

* <span data-ttu-id="1c2b0-976">EF 記憶體內資料庫會用於示範用途。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-976">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="1c2b0-977">若要使用需要連接字串的資料庫，請實作第二個 `ConfigurationBuilder` 以從另一個設定提供者提供連接字串。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-977">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="1c2b0-978">啟動時，該提供者會將資料庫資料表讀入到設定中。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-978">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="1c2b0-979">該提供者不會以個別機碼為基礎查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-979">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="1c2b0-980">未實作變更時重新載入，因此在應用程式啟動後更新資料庫對應用程式設定沒有影響。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-980">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="1c2b0-981">定義 `EFConfigurationValue` 實體來在資料庫中存放設定值。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-981">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="1c2b0-982">*Models/EFConfigurationValue.cs*：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-982">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="1c2b0-983">新增 `EFConfigurationContext` 以存放及存取已設定的值。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-983">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="1c2b0-984">*EFConfigurationProvider/EFConfigurationContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-984">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="1c2b0-985">建立會實作 <xref:Microsoft.Extensions.Configuration.IConfigurationSource> 的類別。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-985">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="1c2b0-986">*EFConfigurationProvider/EFConfigurationSource.cs*：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-986">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="1c2b0-987">透過繼承自 <xref:Microsoft.Extensions.Configuration.ConfigurationProvider> 來建立自訂設定提供者。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-987">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="1c2b0-988">若資料庫是空的，設定提供者會初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-988">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="1c2b0-989">*EFConfigurationProvider/EFConfigurationProvider.cs*：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-989">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="1c2b0-990">`AddEFConfiguration` 擴充方法允許新增設定來源到 `ConfigurationBuilder`。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-990">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="1c2b0-991">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c2b0-991">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="1c2b0-992">下列程式碼顯示如何在 *Program.cs* 中使用自訂 `EFConfigurationProvider`：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-992">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="1c2b0-993">在啟動期間存取組態</span><span class="sxs-lookup"><span data-stu-id="1c2b0-993">Access configuration during startup</span></span>

<span data-ttu-id="1c2b0-994">將 `IConfiguration` 插入到 `Startup` 建構函式，以存取 `Startup.ConfigureServices` 中的設定值。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-994">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1c2b0-995">若要存取 `Startup.Configure` 中的設定，請直接將 `IConfiguration` 插入到方法或從建構函式使用執行個體：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-995">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="1c2b0-996">如需使用啟動方便方法來存取設定的範例，請參閱[應用程式啟動：方便方法](xref:fundamentals/startup#convenience-methods)。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-996">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="1c2b0-997">Razor頁面頁面或 MVC 視圖中的存取設定</span><span class="sxs-lookup"><span data-stu-id="1c2b0-997">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="1c2b0-998">若要存取 Razor [頁面] 頁面或 MVC 視圖中的設定，請為[Microsoft.Extensions.Configuration 命名空間](xref:Microsoft.Extensions.Configuration)新增[using](xref:mvc/views/razor#using)指示詞（[c # 參考： using](/dotnet/csharp/language-reference/keywords/using-directive)指示詞），並將其插入 <xref:Microsoft.Extensions.Configuration.IConfiguration> 頁面或視圖中。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-998">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="1c2b0-999">在 [ Razor 頁面] 頁面中：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-999">In a Razor Pages page:</span></span>

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

<span data-ttu-id="1c2b0-1000">在 MVC 檢視中：</span><span class="sxs-lookup"><span data-stu-id="1c2b0-1000">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="1c2b0-1001">從外部組件新增設定</span><span class="sxs-lookup"><span data-stu-id="1c2b0-1001">Add configuration from an external assembly</span></span>

<span data-ttu-id="1c2b0-1002"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-1002">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="1c2b0-1003">如需詳細資訊，請參閱 <xref:fundamentals/configuration/platform-specific-configuration> 。</span><span class="sxs-lookup"><span data-stu-id="1c2b0-1003">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1c2b0-1004">其他資源</span><span class="sxs-lookup"><span data-stu-id="1c2b0-1004">Additional resources</span></span>

* <xref:fundamentals/configuration/options>

::: moniker-end
