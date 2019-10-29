---
title: ASP.NET Core 的設定
author: guardrex
description: 了解如何使用組態 API 設定 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 263f9f7c4c800a74b745fd636196e1e135afc91b
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2019
ms.locfileid: "73033908"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="9e097-103">ASP.NET Core 的設定</span><span class="sxs-lookup"><span data-stu-id="9e097-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="9e097-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9e097-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9e097-105">ASP.NET Core 中的應用程式設定是以由*設定提供者*所建立的機碼值組為基礎。</span><span class="sxs-lookup"><span data-stu-id="9e097-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="9e097-106">設定提供者會從各種設定來源將設定資料讀取到機碼值組中：</span><span class="sxs-lookup"><span data-stu-id="9e097-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="9e097-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="9e097-107">Azure Key Vault</span></span>
* <span data-ttu-id="9e097-108">Azure 應用程式組態</span><span class="sxs-lookup"><span data-stu-id="9e097-108">Azure App Configuration</span></span>
* <span data-ttu-id="9e097-109">命令列引數</span><span class="sxs-lookup"><span data-stu-id="9e097-109">Command-line arguments</span></span>
* <span data-ttu-id="9e097-110">自訂提供者 (已安裝或已建立)</span><span class="sxs-lookup"><span data-stu-id="9e097-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="9e097-111">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="9e097-111">Directory files</span></span>
* <span data-ttu-id="9e097-112">環境變數</span><span class="sxs-lookup"><span data-stu-id="9e097-112">Environment variables</span></span>
* <span data-ttu-id="9e097-113">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="9e097-113">In-memory .NET objects</span></span>
* <span data-ttu-id="9e097-114">設定檔</span><span class="sxs-lookup"><span data-stu-id="9e097-114">Settings files</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9e097-115">一般組態提供者案例 ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) 的組態套件會以隱含方式包含在架構中。</span><span class="sxs-lookup"><span data-stu-id="9e097-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9e097-116">一般組態提供者案例 ([microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) 的組態套件包含在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="9e097-116">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="9e097-117">下列程式碼範例與範例應用程式中的程式碼範例使用 <xref:Microsoft.Extensions.Configuration> 命名空間：</span><span class="sxs-lookup"><span data-stu-id="9e097-117">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="9e097-118">*選項模式*是此主題中所述之設定概念的延伸。</span><span class="sxs-lookup"><span data-stu-id="9e097-118">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="9e097-119">選項使用類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="9e097-119">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="9e097-120">如需詳細資訊，請參閱<xref:fundamentals/configuration/options>。</span><span class="sxs-lookup"><span data-stu-id="9e097-120">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="9e097-121">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9e097-121">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="9e097-122">主機與應用程式組態的比較</span><span class="sxs-lookup"><span data-stu-id="9e097-122">Host versus app configuration</span></span>

<span data-ttu-id="9e097-123">設定及啟動應用程式之前，會先設定及啟動「主機」。</span><span class="sxs-lookup"><span data-stu-id="9e097-123">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="9e097-124">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="9e097-124">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="9e097-125">應用程式與主機都是使用此主題中所述的設定提供者來設定的。</span><span class="sxs-lookup"><span data-stu-id="9e097-125">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="9e097-126">主機組態機碼/值組也會包含在應用程式的組態中。</span><span class="sxs-lookup"><span data-stu-id="9e097-126">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="9e097-127">如需有關當建置主機時如何使用設定提供者的詳細資訊，以及設定來源如何影響主機設定的詳細資訊，請參閱 <xref:fundamentals/index#host>。</span><span class="sxs-lookup"><span data-stu-id="9e097-127">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="9e097-128">預設的組態</span><span class="sxs-lookup"><span data-stu-id="9e097-128">Default configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9e097-129">以 ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) 範本為基礎的 Web 應用程式，會在建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>。</span><span class="sxs-lookup"><span data-stu-id="9e097-129">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="9e097-130">`CreateDefaultBuilder` 會以下列順序提供應用程式的預設組態：</span><span class="sxs-lookup"><span data-stu-id="9e097-130">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="9e097-131">下列項目適用於使用[一般主機](xref:fundamentals/host/generic-host)的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9e097-131">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="9e097-132">如需使用 [Web 主機](xref:fundamentals/host/web-host)時預設組態的詳細資料，請參閱[本主題的 ASP.NET Core 2.2 版本](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2)。</span><span class="sxs-lookup"><span data-stu-id="9e097-132">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="9e097-133">主機組態的提供來源：</span><span class="sxs-lookup"><span data-stu-id="9e097-133">Host configuration is provided from:</span></span>
  * <span data-ttu-id="9e097-134">使用[環境變數組態提供者](#environment-variables-configuration-provider)且以 `DOTNET_` 為前置詞 (例如 `DOTNET_ENVIRONMENT`) 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="9e097-134">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="9e097-135">載入設定機碼值組時，會移除前置詞 (`DOTNET_`)。</span><span class="sxs-lookup"><span data-stu-id="9e097-135">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="9e097-136">使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="9e097-136">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="9e097-137">已建立 Web 主機預設組態 (`ConfigureWebHostDefaults`)：</span><span class="sxs-lookup"><span data-stu-id="9e097-137">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="9e097-138">Kestrel 會用作為網頁伺服器，並使用應用程式的組態提供者來設定。</span><span class="sxs-lookup"><span data-stu-id="9e097-138">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="9e097-139">新增主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="9e097-139">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="9e097-140">如果 `ASPNETCORE_FORWARDEDHEADERS_ENABLED` 環境變數設定為 `true`，則會新增轉接的標頭中介軟體。</span><span class="sxs-lookup"><span data-stu-id="9e097-140">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="9e097-141">啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="9e097-141">Enable IIS integration.</span></span>
* <span data-ttu-id="9e097-142">應用程式設定的提供來源：</span><span class="sxs-lookup"><span data-stu-id="9e097-142">App configuration is provided from:</span></span>
  * <span data-ttu-id="9e097-143">使用[檔案組態提供者](#file-configuration-provider)的 *appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="9e097-143">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="9e097-144">使用[檔案組態提供者](#file-configuration-provider)的 *appsettings.{Environment}.json*。</span><span class="sxs-lookup"><span data-stu-id="9e097-144">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="9e097-145">應用程式在使用項目組件之 `Development` 環境中執行時的[秘密管理員](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="9e097-145">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="9e097-146">使用[環境變數組態提供者](#environment-variables-configuration-provider)的環境變數。</span><span class="sxs-lookup"><span data-stu-id="9e097-146">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="9e097-147">使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="9e097-147">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9e097-148">以 ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) 範本為基礎的 Web 應用程式，會在建置主機時呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>。</span><span class="sxs-lookup"><span data-stu-id="9e097-148">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="9e097-149">`CreateDefaultBuilder` 會以下列順序提供應用程式的預設組態：</span><span class="sxs-lookup"><span data-stu-id="9e097-149">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="9e097-150">下列項目適用於使用 [Web 主機](xref:fundamentals/host/web-host)的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9e097-150">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="9e097-151">如需使用[一般主機](xref:fundamentals/host/generic-host)時預設組態的詳細資料，請參閱[本主題的最新版本](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="9e097-151">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="9e097-152">主機組態的提供來源：</span><span class="sxs-lookup"><span data-stu-id="9e097-152">Host configuration is provided from:</span></span>
  * <span data-ttu-id="9e097-153">使用[環境變數組態提供者](#environment-variables-configuration-provider)且以 `ASPNETCORE_` 為前置詞 (例如 `ASPNETCORE_ENVIRONMENT`) 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="9e097-153">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="9e097-154">載入設定機碼值組時，會移除前置詞 (`ASPNETCORE_`)。</span><span class="sxs-lookup"><span data-stu-id="9e097-154">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="9e097-155">使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="9e097-155">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="9e097-156">應用程式設定的提供來源：</span><span class="sxs-lookup"><span data-stu-id="9e097-156">App configuration is provided from:</span></span>
  * <span data-ttu-id="9e097-157">使用[檔案組態提供者](#file-configuration-provider)的 *appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="9e097-157">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="9e097-158">使用[檔案組態提供者](#file-configuration-provider)的 *appsettings.{Environment}.json*。</span><span class="sxs-lookup"><span data-stu-id="9e097-158">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="9e097-159">應用程式在使用項目組件之 `Development` 環境中執行時的[秘密管理員](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="9e097-159">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="9e097-160">使用[環境變數組態提供者](#environment-variables-configuration-provider)的環境變數。</span><span class="sxs-lookup"><span data-stu-id="9e097-160">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="9e097-161">使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="9e097-161">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

## <a name="security"></a><span data-ttu-id="9e097-162">安全性</span><span class="sxs-lookup"><span data-stu-id="9e097-162">Security</span></span>

<span data-ttu-id="9e097-163">採用下列做法來保護敏感性組態資料：</span><span class="sxs-lookup"><span data-stu-id="9e097-163">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="9e097-164">永遠不要將密碼或其他敏感性資料儲存在設定提供者程式碼或純文字設定檔中。</span><span class="sxs-lookup"><span data-stu-id="9e097-164">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="9e097-165">不要在開發或測試環境中使用生產環境祕密。</span><span class="sxs-lookup"><span data-stu-id="9e097-165">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="9e097-166">請在專案外部指定祕密，以防止其意外認可至開放原始碼存放庫。</span><span class="sxs-lookup"><span data-stu-id="9e097-166">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="9e097-167">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="9e097-167">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="9e097-168"><xref:security/app-secrets> &ndash; 包含使用環境變數來儲存敏感性資料的建議。</span><span class="sxs-lookup"><span data-stu-id="9e097-168"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="9e097-169">「祕密管理員」使用「檔案設定提供者」以 JSON 檔案在本機系統上存放使用者祕密。</span><span class="sxs-lookup"><span data-stu-id="9e097-169">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="9e097-170">此主題稍後將說明「檔案設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="9e097-170">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="9e097-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 可安全地儲存 ASP.NET Core 應用程式的應用程式祕密。</span><span class="sxs-lookup"><span data-stu-id="9e097-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="9e097-172">如需詳細資訊，請參閱<xref:security/key-vault-configuration>。</span><span class="sxs-lookup"><span data-stu-id="9e097-172">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="9e097-173">階層式設定資料</span><span class="sxs-lookup"><span data-stu-id="9e097-173">Hierarchical configuration data</span></span>

<span data-ttu-id="9e097-174">設定 API 可在設定機碼中使用分隔符號來壓平合併階層式資料，以管理階層式設定資料。</span><span class="sxs-lookup"><span data-stu-id="9e097-174">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="9e097-175">在下列 JSON 檔案中，兩個區段的結構式階層中有四個機碼存在：</span><span class="sxs-lookup"><span data-stu-id="9e097-175">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="9e097-176">當該檔案被讀入設定之後，會建立唯一機碼來維護設定來源的原始階層式資料結構。</span><span class="sxs-lookup"><span data-stu-id="9e097-176">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="9e097-177">系統會使用冒號 (`:`) 將區段與機碼壓平合併以維護原始結構：</span><span class="sxs-lookup"><span data-stu-id="9e097-177">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="9e097-178">section0:key0</span><span class="sxs-lookup"><span data-stu-id="9e097-178">section0:key0</span></span>
* <span data-ttu-id="9e097-179">section0:key1</span><span class="sxs-lookup"><span data-stu-id="9e097-179">section0:key1</span></span>
* <span data-ttu-id="9e097-180">section1:key0</span><span class="sxs-lookup"><span data-stu-id="9e097-180">section1:key0</span></span>
* <span data-ttu-id="9e097-181">section1:key1</span><span class="sxs-lookup"><span data-stu-id="9e097-181">section1:key1</span></span>

<span data-ttu-id="9e097-182"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> 與 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> 方法可用來在設定資料中隔離區段與區段的子系。</span><span class="sxs-lookup"><span data-stu-id="9e097-182"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="9e097-183">[GetSection,、 GetChildren 與 Exists](#getsection-getchildren-and-exists) 中說明這些方法。</span><span class="sxs-lookup"><span data-stu-id="9e097-183">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="9e097-184">慣例</span><span class="sxs-lookup"><span data-stu-id="9e097-184">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="9e097-185">來源和提供者</span><span class="sxs-lookup"><span data-stu-id="9e097-185">Sources and providers</span></span>

<span data-ttu-id="9e097-186">在應用程式啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="9e097-186">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="9e097-187">實作變更偵測的組態提供者能夠在基礎設定變更時重新載入組態。</span><span class="sxs-lookup"><span data-stu-id="9e097-187">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="9e097-188">例如，檔案組態提供者 (將於本主題稍後討論) 和 [Azure Key Vault 組態提供者](xref:security/key-vault-configuration)均會實作變更偵測。</span><span class="sxs-lookup"><span data-stu-id="9e097-188">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="9e097-189">您可以在應用程式的[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器中找到 <xref:Microsoft.Extensions.Configuration.IConfiguration>。</span><span class="sxs-lookup"><span data-stu-id="9e097-189"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="9e097-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> 可插入 Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> 來取得類別的組態：</span><span class="sxs-lookup"><span data-stu-id="9e097-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

```csharp
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

<span data-ttu-id="9e097-191">設定提供者無法使用 DI，因為當它們由主機設定時，它無法使用。</span><span class="sxs-lookup"><span data-stu-id="9e097-191">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="9e097-192">按鍵</span><span class="sxs-lookup"><span data-stu-id="9e097-192">Keys</span></span>

<span data-ttu-id="9e097-193">設定機碼會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="9e097-193">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="9e097-194">機碼區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="9e097-194">Keys are case-insensitive.</span></span> <span data-ttu-id="9e097-195">例如，`ConnectionString` 與 `connectionstring` 會被視為相等的機碼。</span><span class="sxs-lookup"><span data-stu-id="9e097-195">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="9e097-196">若相同機碼的值是由相同或不同設定提供者設定，在機碼上設定的最後一個值是所使用的值。</span><span class="sxs-lookup"><span data-stu-id="9e097-196">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="9e097-197">階層式機碼</span><span class="sxs-lookup"><span data-stu-id="9e097-197">Hierarchical keys</span></span>
  * <span data-ttu-id="9e097-198">在設定 API 內，冒號分隔字元 (`:`) 可在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="9e097-198">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="9e097-199">在環境變數中，冒號分隔字元可能無法在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="9e097-199">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="9e097-200">所有平台都支援雙底線 (`__`)，且會自動轉換為冒號。</span><span class="sxs-lookup"><span data-stu-id="9e097-200">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="9e097-201">在 Azure Key Vault 中，階層式機碼使用 `--` (兩個破折號) 來做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="9e097-201">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="9e097-202">您必須提供程式碼，在祕密載入到應用程式的設定時將破折號取代為冒號。</span><span class="sxs-lookup"><span data-stu-id="9e097-202">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="9e097-203"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="9e097-203">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="9e097-204">[將陣列繫結到類別](#bind-an-array-to-a-class)一節說明陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="9e097-204">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="9e097-205">值</span><span class="sxs-lookup"><span data-stu-id="9e097-205">Values</span></span>

<span data-ttu-id="9e097-206">設定值會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="9e097-206">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="9e097-207">值是字串。</span><span class="sxs-lookup"><span data-stu-id="9e097-207">Values are strings.</span></span>
* <span data-ttu-id="9e097-208">Null 值無法存放在設定中或繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="9e097-208">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="9e097-209">提供者</span><span class="sxs-lookup"><span data-stu-id="9e097-209">Providers</span></span>

<span data-ttu-id="9e097-210">下表顯示可供 ASP.NET Core 應用程式使用的設定提供者。</span><span class="sxs-lookup"><span data-stu-id="9e097-210">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="9e097-211">Provider</span><span class="sxs-lookup"><span data-stu-id="9e097-211">Provider</span></span> | <span data-ttu-id="9e097-212">從&hellip;提供設定</span><span class="sxs-lookup"><span data-stu-id="9e097-212">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="9e097-213">[Azure Key Vault 設定提供者](xref:security/key-vault-configuration) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="9e097-213">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="9e097-214">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="9e097-214">Azure Key Vault</span></span> |
| <span data-ttu-id="9e097-215">[Azure 應用程式組態提供者](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure 文件)</span><span class="sxs-lookup"><span data-stu-id="9e097-215">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="9e097-216">Azure 應用程式組態</span><span class="sxs-lookup"><span data-stu-id="9e097-216">Azure App Configuration</span></span> |
| [<span data-ttu-id="9e097-217">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="9e097-217">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="9e097-218">命令列參數</span><span class="sxs-lookup"><span data-stu-id="9e097-218">Command-line parameters</span></span> |
| [<span data-ttu-id="9e097-219">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="9e097-219">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="9e097-220">自訂來源</span><span class="sxs-lookup"><span data-stu-id="9e097-220">Custom source</span></span> |
| [<span data-ttu-id="9e097-221">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="9e097-221">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="9e097-222">環境變數</span><span class="sxs-lookup"><span data-stu-id="9e097-222">Environment variables</span></span> |
| [<span data-ttu-id="9e097-223">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="9e097-223">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="9e097-224">檔案 (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="9e097-224">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="9e097-225">每個檔案機碼的設定提供者</span><span class="sxs-lookup"><span data-stu-id="9e097-225">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="9e097-226">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="9e097-226">Directory files</span></span> |
| [<span data-ttu-id="9e097-227">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="9e097-227">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="9e097-228">記憶體內集合</span><span class="sxs-lookup"><span data-stu-id="9e097-228">In-memory collections</span></span> |
| <span data-ttu-id="9e097-229">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="9e097-229">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="9e097-230">使用者設定檔目錄中的檔案</span><span class="sxs-lookup"><span data-stu-id="9e097-230">File in the user profile directory</span></span> |

<span data-ttu-id="9e097-231">在啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="9e097-231">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="9e097-232">此主題中所述的設定提供者是以字母順序描述，而非以您的程式碼安排它們的順序。</span><span class="sxs-lookup"><span data-stu-id="9e097-232">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="9e097-233">在您的程式碼中針對底層設定來源的優先順序，為設定提供者排序。</span><span class="sxs-lookup"><span data-stu-id="9e097-233">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="9e097-234">典型的設定提供者順序是：</span><span class="sxs-lookup"><span data-stu-id="9e097-234">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="9e097-235">檔案 (*appsettings.json*、*appsettings.{Environment}.json*，其中 `{Environment}` 是應用程式的目前裝載環境)</span><span class="sxs-lookup"><span data-stu-id="9e097-235">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="9e097-236">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="9e097-236">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="9e097-237">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (僅限開發環境)</span><span class="sxs-lookup"><span data-stu-id="9e097-237">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="9e097-238">環境變數</span><span class="sxs-lookup"><span data-stu-id="9e097-238">Environment variables</span></span>
1. <span data-ttu-id="9e097-239">命令列引數</span><span class="sxs-lookup"><span data-stu-id="9e097-239">Command-line arguments</span></span>

<span data-ttu-id="9e097-240">將命令列組態提供者放在提供者序列結尾是常見做法，因為這樣可以讓命令列引數覆寫由其他提供者所設定的組態。</span><span class="sxs-lookup"><span data-stu-id="9e097-240">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="9e097-241">當您使用 `CreateDefaultBuilder` 初始化新的主機建立器時，會使用上述的提供者序列。</span><span class="sxs-lookup"><span data-stu-id="9e097-241">The preceding sequence of providers is used when you initialize a new host builder with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="9e097-242">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="9e097-242">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="9e097-243">使用 ConfigureHostConfiguration 設定主機建立器</span><span class="sxs-lookup"><span data-stu-id="9e097-243">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="9e097-244">若要設定主機建立器，請呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> 並提供組態。</span><span class="sxs-lookup"><span data-stu-id="9e097-244">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="9e097-245">`ConfigureHostConfiguration` 用於初始化 <xref:Microsoft.Extensions.Hosting.IHostEnvironment>，以便稍後在建置程序中使用。</span><span class="sxs-lookup"><span data-stu-id="9e097-245">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="9e097-246">`ConfigureHostConfiguration` 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="9e097-246">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="9e097-247">使用 UseConfiguration 設定主機建立器</span><span class="sxs-lookup"><span data-stu-id="9e097-247">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="9e097-248">若要設定主機建立器，請使用該組態在主機建立器上呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>。</span><span class="sxs-lookup"><span data-stu-id="9e097-248">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

::: moniker-end

## <a name="configureappconfiguration"></a><span data-ttu-id="9e097-249">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="9e097-249">ConfigureAppConfiguration</span></span>

<span data-ttu-id="9e097-250">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定提供者，以及由 `CreateDefaultBuilder` 自動新增的設定提供者：</span><span class="sxs-lookup"><span data-stu-id="9e097-250">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="9e097-251">使用命令列引數覆寫先前的組態</span><span class="sxs-lookup"><span data-stu-id="9e097-251">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="9e097-252">若要提供可使用命令列引數覆寫的應用程式組態，請最後呼叫 `AddCommandLine`：</span><span class="sxs-lookup"><span data-stu-id="9e097-252">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="9e097-253">在應用程式啟動期間使用組態</span><span class="sxs-lookup"><span data-stu-id="9e097-253">Consume configuration during app startup</span></span>

<span data-ttu-id="9e097-254">應用程式啟動期間，可以使用 `ConfigureAppConfiguration` 中為應用程式提供的組態，包括 `Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="9e097-254">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9e097-255">如需詳細資訊，請參閱[在啟動期間存取組態](#access-configuration-during-startup)一節。</span><span class="sxs-lookup"><span data-stu-id="9e097-255">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="9e097-256">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="9e097-256">Command-line Configuration Provider</span></span>

<span data-ttu-id="9e097-257"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> 會在執行階段從命令列引數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="9e097-257">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="9e097-258">為了啟用命令列設定，<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 延伸模組方法會在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫。</span><span class="sxs-lookup"><span data-stu-id="9e097-258">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="9e097-259">呼叫 `CreateDefaultBuilder(string [])` 時，會自動呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="9e097-259">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="9e097-260">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="9e097-260">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="9e097-261">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="9e097-261">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="9e097-262">從 *appsettings.json* 與 *appsettings.{Environment}.json* 檔案載入其他選擇性組態。</span><span class="sxs-lookup"><span data-stu-id="9e097-262">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="9e097-263">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="9e097-263">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="9e097-264">環境變數。</span><span class="sxs-lookup"><span data-stu-id="9e097-264">Environment variables.</span></span>

<span data-ttu-id="9e097-265">`CreateDefaultBuilder` 會最後才新增命令列設定提供者。</span><span class="sxs-lookup"><span data-stu-id="9e097-265">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="9e097-266">在執行階段傳遞的命令列引數會覆寫由其它提供者所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="9e097-266">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="9e097-267">`CreateDefaultBuilder` 會在建構主機時執行作業。</span><span class="sxs-lookup"><span data-stu-id="9e097-267">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="9e097-268">因此，由`CreateDefaultBuilder` 啟用的命令列設定可以影響主機的設定方式。</span><span class="sxs-lookup"><span data-stu-id="9e097-268">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="9e097-269">針對以 ASP.NET Core 範本為基礎的應用程式，`AddCommandLine` 已由 `CreateDefaultBuilder` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="9e097-269">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="9e097-270">若要新增其他組態提供者並維持能夠使用命令列引數覆寫這些提供者組態的能力，請在 `ConfigureAppConfiguration` 中呼叫應用程式的其他提供者，且最後呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="9e097-270">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="9e097-271">**範例**</span><span class="sxs-lookup"><span data-stu-id="9e097-271">**Example**</span></span>

<span data-ttu-id="9e097-272">範例應用程式利用靜態方便方法 `CreateDefaultBuilder` 的優勢來建置主機，這包括對 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="9e097-272">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="9e097-273">從專案目錄中開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="9e097-273">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="9e097-274">提供命令列引數給 `dotnet run` 命令 `dotnet run CommandLineKey=CommandLineValue`。</span><span class="sxs-lookup"><span data-stu-id="9e097-274">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="9e097-275">在應用程式執行之後，開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9e097-275">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="9e097-276">觀察輸出是否包含提供給 `dotnet run` 之設定命令列引數的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="9e097-276">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="9e097-277">引數</span><span class="sxs-lookup"><span data-stu-id="9e097-277">Arguments</span></span>

<span data-ttu-id="9e097-278">當值後面接著空格時，值必須接著等號 (`=`)，或機碼必須有前置詞 (`--` 或 `/`)。</span><span class="sxs-lookup"><span data-stu-id="9e097-278">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="9e097-279">如果使用等號 (例如 `CommandLineKey=`)，則不需要此值。</span><span class="sxs-lookup"><span data-stu-id="9e097-279">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="9e097-280">索引鍵前置字元</span><span class="sxs-lookup"><span data-stu-id="9e097-280">Key prefix</span></span>               | <span data-ttu-id="9e097-281">範例</span><span class="sxs-lookup"><span data-stu-id="9e097-281">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="9e097-282">沒有前置字元</span><span class="sxs-lookup"><span data-stu-id="9e097-282">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="9e097-283">雙虛線 (`--`)</span><span class="sxs-lookup"><span data-stu-id="9e097-283">Two dashes (`--`)</span></span>        | <span data-ttu-id="9e097-284">`--CommandLineKey2=value2`、 `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="9e097-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="9e097-285">正斜線 (`/`)</span><span class="sxs-lookup"><span data-stu-id="9e097-285">Forward slash (`/`)</span></span>      | <span data-ttu-id="9e097-286">`/CommandLineKey3=value3`、 `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="9e097-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="9e097-287">在相同的命令中，請不要混合使用等號搭配使用空格之機碼值組的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="9e097-287">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="9e097-288">範例命令：</span><span class="sxs-lookup"><span data-stu-id="9e097-288">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="9e097-289">切換對應</span><span class="sxs-lookup"><span data-stu-id="9e097-289">Switch mappings</span></span>

<span data-ttu-id="9e097-290">參數對應允許索引鍵名稱取代邏輯。</span><span class="sxs-lookup"><span data-stu-id="9e097-290">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="9e097-291">當您使用 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 手動建置設定時，可以提供切換取代的字典給 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 方法。</span><span class="sxs-lookup"><span data-stu-id="9e097-291">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="9e097-292">使用切換對應字典時，會檢查字典中是否有任何索引鍵符合命令列引數所提供的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="9e097-292">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="9e097-293">如果在字典中找到命令列索引鍵，則會傳回字典值 (索引鍵取代) 以在應用程式的設定中設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="9e097-293">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="9e097-294">所有前面加上單虛線 (`-`) 的命令列索引鍵都需要切換對應。</span><span class="sxs-lookup"><span data-stu-id="9e097-294">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="9e097-295">切換對應字典索引鍵規則：</span><span class="sxs-lookup"><span data-stu-id="9e097-295">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="9e097-296">切換必須以單虛線 (`-`) 或雙虛線 (`--`) 開頭。</span><span class="sxs-lookup"><span data-stu-id="9e097-296">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="9e097-297">切換對應字典不能包含重複索引鍵。</span><span class="sxs-lookup"><span data-stu-id="9e097-297">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="9e097-298">建立切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="9e097-298">Create a switch mappings dictionary.</span></span> <span data-ttu-id="9e097-299">下列範例會建立兩個切換對應：</span><span class="sxs-lookup"><span data-stu-id="9e097-299">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="9e097-300">建立主機時，請使用切換對應字典呼叫 `AddCommandLine`：</span><span class="sxs-lookup"><span data-stu-id="9e097-300">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="9e097-301">針對使用切換對應的應用程式，呼叫 `CreateDefaultBuilder` 不應傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="9e097-301">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="9e097-302">`CreateDefaultBuilder` 方法的 `AddCommandLine` 呼叫不包括對應的切換，且沒有任何方式可以將切換對應字典傳遞給 `CreateDefaultBuilder`。</span><span class="sxs-lookup"><span data-stu-id="9e097-302">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="9e097-303">解決方案並非將引數傳遞給 `CreateDefaultBuilder`，而是允許 `ConfigurationBuilder` 方法的 `AddCommandLine` 方法同時處理引數與切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="9e097-303">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="9e097-304">建立切換對應字典之後，它會包含下表中所示的資料。</span><span class="sxs-lookup"><span data-stu-id="9e097-304">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="9e097-305">機碼</span><span class="sxs-lookup"><span data-stu-id="9e097-305">Key</span></span>       | <span data-ttu-id="9e097-306">值</span><span class="sxs-lookup"><span data-stu-id="9e097-306">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="9e097-307">若啟動應用程式時使用切換對應機碼，設定會接收由字典提供之機碼上的設定值：</span><span class="sxs-lookup"><span data-stu-id="9e097-307">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="9e097-308">執行上述命令之後，設定包含下表中顯示的值。</span><span class="sxs-lookup"><span data-stu-id="9e097-308">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="9e097-309">機碼</span><span class="sxs-lookup"><span data-stu-id="9e097-309">Key</span></span>               | <span data-ttu-id="9e097-310">值</span><span class="sxs-lookup"><span data-stu-id="9e097-310">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="9e097-311">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="9e097-311">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="9e097-312"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> 會在執行階段從環境變數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="9e097-312">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="9e097-313">若要啟用環境變數設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="9e097-313">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="9e097-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) 允許您在 Azure 入口網站中設定環境變數，以使用「環境變數設定提供者」覆寫應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="9e097-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="9e097-315">如需詳細資訊，請參閱 [Azure App：使用 Azure 入口網站覆寫應用程式設定](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)。</span><span class="sxs-lookup"><span data-stu-id="9e097-315">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9e097-316">使用[一般主機](xref:fundamentals/host/generic-host)初始化新的主機建立器並呼叫 `CreateDefaultBuilder` 時，可使用 `AddEnvironmentVariables` 為[主機組態](#host-versus-app-configuration)載入字首為 `DOTNET_` 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="9e097-316">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="9e097-317">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="9e097-317">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9e097-318">使用 [Web 主機](xref:fundamentals/host/web-host)初始化新的主機建立器並呼叫 `CreateDefaultBuilder` 時，可使用 `AddEnvironmentVariables` 為[主機組態](#host-versus-app-configuration)載入字首為 `ASPNETCORE_` 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="9e097-318">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="9e097-319">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="9e097-319">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

<span data-ttu-id="9e097-320">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="9e097-320">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="9e097-321">來自無首碼之環境變數的應用程式組態 (在未提供首碼的情況下呼叫 `AddEnvironmentVariables`)。</span><span class="sxs-lookup"><span data-stu-id="9e097-321">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="9e097-322">從 *appsettings.json* 與 *appsettings.{Environment}.json* 檔案載入其他選擇性組態。</span><span class="sxs-lookup"><span data-stu-id="9e097-322">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="9e097-323">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="9e097-323">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="9e097-324">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="9e097-324">Command-line arguments.</span></span>

<span data-ttu-id="9e097-325">從使用者祕密與 *appsettings* 檔案建立設定之後，會呼叫「環境變數設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="9e097-325">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="9e097-326">在此位置呼叫提供者可讓系統在執行階段讀取環境變數，以覆寫由使用者祕密與 *appsettings* 檔案所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="9e097-326">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="9e097-327">如果您需要從其他環境變數提供應用程式設定，請呼叫 `ConfigureAppConfiguration` 中的應用程式其他提供者，並呼叫具有該前置詞的 `AddEnvironmentVariables`。</span><span class="sxs-lookup"><span data-stu-id="9e097-327">If you need to provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call additional providers here as needed.
    // Call AddEnvironmentVariables last if you need to allow
    // environment variables to override values from other 
    // providers.
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
}
```

<span data-ttu-id="9e097-328">**範例**</span><span class="sxs-lookup"><span data-stu-id="9e097-328">**Example**</span></span>

<span data-ttu-id="9e097-329">範例應用程式利用靜態方便方法 `CreateDefaultBuilder` 的優勢來建置主機，這包括對 `AddEnvironmentVariables` 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="9e097-329">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="9e097-330">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="9e097-330">Run the sample app.</span></span> <span data-ttu-id="9e097-331">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9e097-331">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="9e097-332">觀察輸出是否包含環境變數 `ENVIRONMENT` 的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="9e097-332">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="9e097-333">值反映應用程式執行所在環境，在本機執行時，通常是 `Development`。</span><span class="sxs-lookup"><span data-stu-id="9e097-333">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="9e097-334">為縮短由應用程式轉譯的環境變數清單，應用程式會篩選環境變數。</span><span class="sxs-lookup"><span data-stu-id="9e097-334">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="9e097-335">請參閱範例應用程式的 *Pages/Index.cshtml.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="9e097-335">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="9e097-336">若想要將所有環境變數公開給應用程式使用，請將 *Pages/Index.cshtml.cs* 中的 `FilteredConfiguration` 變更為下面這樣：</span><span class="sxs-lookup"><span data-stu-id="9e097-336">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="9e097-337">首碼</span><span class="sxs-lookup"><span data-stu-id="9e097-337">Prefixes</span></span>

<span data-ttu-id="9e097-338">當您將前置詞套用到 `AddEnvironmentVariables` 方法時，會篩選載入到應用程式設定中的環境變數。</span><span class="sxs-lookup"><span data-stu-id="9e097-338">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="9e097-339">例如，若要篩選前置詞為 `CUSTOM_` 的環境變數，請提供前置詞給設定提供者：</span><span class="sxs-lookup"><span data-stu-id="9e097-339">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="9e097-340">建立設定機碼值組時，會移除前置詞。</span><span class="sxs-lookup"><span data-stu-id="9e097-340">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="9e097-341">建立主機建立器時，主機組態由環境變數提供。</span><span class="sxs-lookup"><span data-stu-id="9e097-341">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="9e097-342">如需這些環境變數所用前置詞的詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="9e097-342">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="9e097-343">**連接字串前置詞**</span><span class="sxs-lookup"><span data-stu-id="9e097-343">**Connection string prefixes**</span></span>

<span data-ttu-id="9e097-344">設定 API 四個連接字串環境變數的特殊處理規則，這些這些環境變數牽涉到針對應用程式環境設定 Azure 連接字串。</span><span class="sxs-lookup"><span data-stu-id="9e097-344">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="9e097-345">若將前置詞提供給 `AddEnvironmentVariables`具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="9e097-345">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="9e097-346">連接字串前置詞</span><span class="sxs-lookup"><span data-stu-id="9e097-346">Connection string prefix</span></span> | <span data-ttu-id="9e097-347">Provider</span><span class="sxs-lookup"><span data-stu-id="9e097-347">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="9e097-348">自訂提供者</span><span class="sxs-lookup"><span data-stu-id="9e097-348">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="9e097-349">MySQL</span><span class="sxs-lookup"><span data-stu-id="9e097-349">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="9e097-350">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="9e097-350">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="9e097-351">SQL Server</span><span class="sxs-lookup"><span data-stu-id="9e097-351">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="9e097-352">當探索到具有下表所顯示之任何四個前置詞的環境變數並將其載入到設定中時：</span><span class="sxs-lookup"><span data-stu-id="9e097-352">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="9e097-353">會透過移除環境變數前置詞並新增設定機碼區段 (`ConnectionStrings`) 來移除具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="9e097-353">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="9e097-354">會建立新的設定機碼值組以代表資料庫連線提供者 (`CUSTOMCONNSTR_` 除外，這沒有所述提供者)。</span><span class="sxs-lookup"><span data-stu-id="9e097-354">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="9e097-355">環境變數機碼</span><span class="sxs-lookup"><span data-stu-id="9e097-355">Environment variable key</span></span> | <span data-ttu-id="9e097-356">已轉換的設定機碼</span><span class="sxs-lookup"><span data-stu-id="9e097-356">Converted configuration key</span></span> | <span data-ttu-id="9e097-357">提供者設定項目</span><span class="sxs-lookup"><span data-stu-id="9e097-357">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="9e097-358">設定項目未建立。</span><span class="sxs-lookup"><span data-stu-id="9e097-358">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="9e097-359">機碼：`ConnectionStrings:<KEY>_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="9e097-359">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="9e097-360">值：`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="9e097-360">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="9e097-361">機碼：`ConnectionStrings:<KEY>_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="9e097-361">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="9e097-362">值：`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="9e097-362">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="9e097-363">機碼：`ConnectionStrings:<KEY>_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="9e097-363">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="9e097-364">值：`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="9e097-364">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="9e097-365">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="9e097-365">File Configuration Provider</span></span>

<span data-ttu-id="9e097-366"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> 是用於從檔案系統載入設定的基底類別。</span><span class="sxs-lookup"><span data-stu-id="9e097-366"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="9e097-367">下列設定提供者專用於特定檔案類型：</span><span class="sxs-lookup"><span data-stu-id="9e097-367">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="9e097-368">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="9e097-368">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="9e097-369">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="9e097-369">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="9e097-370">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="9e097-370">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="9e097-371">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="9e097-371">INI Configuration Provider</span></span>

<span data-ttu-id="9e097-372"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> 會在執行階段從 INI 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="9e097-372">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="9e097-373">若要啟用 INI 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="9e097-373">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="9e097-374">冒號可用來做為 INI 檔案設定中的區段分隔符號。</span><span class="sxs-lookup"><span data-stu-id="9e097-374">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="9e097-375">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="9e097-375">Overloads permit specifying:</span></span>

* <span data-ttu-id="9e097-376">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="9e097-376">Whether the file is optional.</span></span>
* <span data-ttu-id="9e097-377">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="9e097-377">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="9e097-378"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="9e097-378">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="9e097-379">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="9e097-379">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="9e097-380">INI 設定檔的一般範例：</span><span class="sxs-lookup"><span data-stu-id="9e097-380">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="9e097-381">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="9e097-381">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="9e097-382">section0:key0</span><span class="sxs-lookup"><span data-stu-id="9e097-382">section0:key0</span></span>
* <span data-ttu-id="9e097-383">section0:key1</span><span class="sxs-lookup"><span data-stu-id="9e097-383">section0:key1</span></span>
* <span data-ttu-id="9e097-384">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="9e097-384">section1:subsection:key</span></span>
* <span data-ttu-id="9e097-385">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="9e097-385">section2:subsection0:key</span></span>
* <span data-ttu-id="9e097-386">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="9e097-386">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="9e097-387">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="9e097-387">JSON Configuration Provider</span></span>

<span data-ttu-id="9e097-388"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> 會在執行階段從 JSON 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="9e097-388">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="9e097-389">若要啟用 JSON 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="9e097-389">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="9e097-390">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="9e097-390">Overloads permit specifying:</span></span>

* <span data-ttu-id="9e097-391">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="9e097-391">Whether the file is optional.</span></span>
* <span data-ttu-id="9e097-392">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="9e097-392">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="9e097-393"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="9e097-393">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="9e097-394">當您使用 `CreateDefaultBuilder` 初始化新的主機建立器時，會自動呼叫 `AddJsonFile` 兩次。</span><span class="sxs-lookup"><span data-stu-id="9e097-394">`AddJsonFile` is automatically called twice when you initialize a new host builder with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="9e097-395">會呼叫此方法以從下列位置載入設定：</span><span class="sxs-lookup"><span data-stu-id="9e097-395">The method is called to load configuration from:</span></span>

* <span data-ttu-id="9e097-396">*appsettings.json* &ndash; 會先讀取此檔案。</span><span class="sxs-lookup"><span data-stu-id="9e097-396">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="9e097-397">檔案的環境版本可以覆寫由 *appsettings.json* 檔案提供的值。</span><span class="sxs-lookup"><span data-stu-id="9e097-397">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="9e097-398">*appsettings.{Environment}.json* &ndash; 檔案的環境版本是根據 [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*) 來載入的。</span><span class="sxs-lookup"><span data-stu-id="9e097-398">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="9e097-399">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="9e097-399">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="9e097-400">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="9e097-400">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="9e097-401">環境變數。</span><span class="sxs-lookup"><span data-stu-id="9e097-401">Environment variables.</span></span>
* <span data-ttu-id="9e097-402">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="9e097-402">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="9e097-403">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="9e097-403">Command-line arguments.</span></span>

<span data-ttu-id="9e097-404">會先建立 JSON 設定提供者。</span><span class="sxs-lookup"><span data-stu-id="9e097-404">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="9e097-405">因此，使用者祕密、環境變數與命令列引數會覆寫由 *appsettings* 檔案設定的設定。</span><span class="sxs-lookup"><span data-stu-id="9e097-405">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="9e097-406">在建置主機時，呼叫 `ConfigureAppConfiguration` 以指定檔案 (除了 *appsettings.json* 和  *appsettings.{Environment}.json* 以外) 的應用程式設定：</span><span class="sxs-lookup"><span data-stu-id="9e097-406">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="9e097-407">**範例**</span><span class="sxs-lookup"><span data-stu-id="9e097-407">**Example**</span></span>

<span data-ttu-id="9e097-408">範例應用程式利用靜態方便方法 `CreateDefaultBuilder` 的優勢來建置主機，這包括對 `AddJsonFile` 的兩次呼叫。</span><span class="sxs-lookup"><span data-stu-id="9e097-408">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="9e097-409">從 *appsettings.json* 與 *appsettings.{Environment}.json* 載入設定。</span><span class="sxs-lookup"><span data-stu-id="9e097-409">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

1. <span data-ttu-id="9e097-410">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="9e097-410">Run the sample app.</span></span> <span data-ttu-id="9e097-411">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9e097-411">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="9e097-412">觀察輸出是否包含表格中所顯示之設定的機碼值組 (視環境而定)。</span><span class="sxs-lookup"><span data-stu-id="9e097-412">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="9e097-413">記錄設定機碼會使用冒號 (`:`) 做為階層式分隔符號。</span><span class="sxs-lookup"><span data-stu-id="9e097-413">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="9e097-414">機碼</span><span class="sxs-lookup"><span data-stu-id="9e097-414">Key</span></span>                        | <span data-ttu-id="9e097-415">開發值</span><span class="sxs-lookup"><span data-stu-id="9e097-415">Development Value</span></span> | <span data-ttu-id="9e097-416">生產值</span><span class="sxs-lookup"><span data-stu-id="9e097-416">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="9e097-417">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="9e097-417">Logging:LogLevel:System</span></span>    | <span data-ttu-id="9e097-418">內容</span><span class="sxs-lookup"><span data-stu-id="9e097-418">Information</span></span>       | <span data-ttu-id="9e097-419">內容</span><span class="sxs-lookup"><span data-stu-id="9e097-419">Information</span></span>      |
| <span data-ttu-id="9e097-420">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="9e097-420">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="9e097-421">內容</span><span class="sxs-lookup"><span data-stu-id="9e097-421">Information</span></span>       | <span data-ttu-id="9e097-422">內容</span><span class="sxs-lookup"><span data-stu-id="9e097-422">Information</span></span>      |
| <span data-ttu-id="9e097-423">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="9e097-423">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="9e097-424">偵錯</span><span class="sxs-lookup"><span data-stu-id="9e097-424">Debug</span></span>             | <span data-ttu-id="9e097-425">錯誤</span><span class="sxs-lookup"><span data-stu-id="9e097-425">Error</span></span>            |
| <span data-ttu-id="9e097-426">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="9e097-426">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="9e097-427">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="9e097-427">XML Configuration Provider</span></span>

<span data-ttu-id="9e097-428"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> 會在執行階段從 XML 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="9e097-428">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="9e097-429">若要啟用 XML 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="9e097-429">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="9e097-430">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="9e097-430">Overloads permit specifying:</span></span>

* <span data-ttu-id="9e097-431">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="9e097-431">Whether the file is optional.</span></span>
* <span data-ttu-id="9e097-432">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="9e097-432">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="9e097-433"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="9e097-433">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="9e097-434">建立設定機碼值組時，會忽略設定檔案的根節點。</span><span class="sxs-lookup"><span data-stu-id="9e097-434">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="9e097-435">請勿在檔案中指定文件類型定義 (DTD) 或命名空間。</span><span class="sxs-lookup"><span data-stu-id="9e097-435">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="9e097-436">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="9e097-436">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="9e097-437">XML 設定檔可以針對重複的區段使用相異元素名稱：</span><span class="sxs-lookup"><span data-stu-id="9e097-437">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="9e097-438">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="9e097-438">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="9e097-439">section0:key0</span><span class="sxs-lookup"><span data-stu-id="9e097-439">section0:key0</span></span>
* <span data-ttu-id="9e097-440">section0:key1</span><span class="sxs-lookup"><span data-stu-id="9e097-440">section0:key1</span></span>
* <span data-ttu-id="9e097-441">section1:key0</span><span class="sxs-lookup"><span data-stu-id="9e097-441">section1:key0</span></span>
* <span data-ttu-id="9e097-442">section1:key1</span><span class="sxs-lookup"><span data-stu-id="9e097-442">section1:key1</span></span>

<span data-ttu-id="9e097-443">若 `name` 屬性是用來區別元素，則可以重複那些使用相同元素名稱的元素：</span><span class="sxs-lookup"><span data-stu-id="9e097-443">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="9e097-444">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="9e097-444">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="9e097-445">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="9e097-445">section:section0:key:key0</span></span>
* <span data-ttu-id="9e097-446">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="9e097-446">section:section0:key:key1</span></span>
* <span data-ttu-id="9e097-447">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="9e097-447">section:section1:key:key0</span></span>
* <span data-ttu-id="9e097-448">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="9e097-448">section:section1:key:key1</span></span>

<span data-ttu-id="9e097-449">屬性可用來提供值：</span><span class="sxs-lookup"><span data-stu-id="9e097-449">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="9e097-450">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="9e097-450">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="9e097-451">key:attribute</span><span class="sxs-lookup"><span data-stu-id="9e097-451">key:attribute</span></span>
* <span data-ttu-id="9e097-452">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="9e097-452">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="9e097-453">每個檔案機碼設定提供者</span><span class="sxs-lookup"><span data-stu-id="9e097-453">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="9e097-454"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> 使用目錄的檔案做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="9e097-454">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="9e097-455">機碼是檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="9e097-455">The key is the file name.</span></span> <span data-ttu-id="9e097-456">值包含檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="9e097-456">The value contains the file's contents.</span></span> <span data-ttu-id="9e097-457">每個檔案機碼設定提供者是在 Docker 主機案例中使用。</span><span class="sxs-lookup"><span data-stu-id="9e097-457">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="9e097-458">若要啟用每個檔案機碼設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="9e097-458">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="9e097-459">檔案的 `directoryPath` 必須是絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="9e097-459">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="9e097-460">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="9e097-460">Overloads permit specifying:</span></span>

* <span data-ttu-id="9e097-461">設定來源的 `Action<KeyPerFileConfigurationSource>` 委派。</span><span class="sxs-lookup"><span data-stu-id="9e097-461">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="9e097-462">目錄是否為選擇性與目錄的路徑。</span><span class="sxs-lookup"><span data-stu-id="9e097-462">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="9e097-463">雙底線 (`__`) 是做為檔案名稱中的設定金鑰分隔符號使用。</span><span class="sxs-lookup"><span data-stu-id="9e097-463">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="9e097-464">例如，檔案名稱 `Logging__LogLevel__System` 會產生設定金鑰 `Logging:LogLevel:System`。</span><span class="sxs-lookup"><span data-stu-id="9e097-464">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="9e097-465">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="9e097-465">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="9e097-466">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="9e097-466">Memory Configuration Provider</span></span>

<span data-ttu-id="9e097-467"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> 使用記憶體內集合做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="9e097-467">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="9e097-468">若要啟用記憶體內集合設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="9e097-468">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="9e097-469">設定提供者可以使用 `IEnumerable<KeyValuePair<String,String>>` 來初始化。</span><span class="sxs-lookup"><span data-stu-id="9e097-469">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="9e097-470">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="9e097-470">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="9e097-471">下列範例會建立組態字典：</span><span class="sxs-lookup"><span data-stu-id="9e097-471">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="9e097-472">字典會與 `AddInMemoryCollection` 的呼叫搭配使用以提供組態：</span><span class="sxs-lookup"><span data-stu-id="9e097-472">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="9e097-473">GetValue</span><span class="sxs-lookup"><span data-stu-id="9e097-473">GetValue</span></span>

<span data-ttu-id="9e097-474">[ConfigurationBinder \<T >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*)會使用指定的索引鍵從設定中解壓縮單一值，並將它轉換成指定的 noncollection 類型。</span><span class="sxs-lookup"><span data-stu-id="9e097-474">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="9e097-475">多載會接受預設值。</span><span class="sxs-lookup"><span data-stu-id="9e097-475">An overload accepts a default value.</span></span>

<span data-ttu-id="9e097-476">下列範例：</span><span class="sxs-lookup"><span data-stu-id="9e097-476">The following example:</span></span>

* <span data-ttu-id="9e097-477">從具有機碼 `NumberKey` 的組態擷取字串值。</span><span class="sxs-lookup"><span data-stu-id="9e097-477">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="9e097-478">若在組態機碼中找不到 `NumberKey`，則會使用預設值 `99`。</span><span class="sxs-lookup"><span data-stu-id="9e097-478">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="9e097-479">鍵入值為 `int`。</span><span class="sxs-lookup"><span data-stu-id="9e097-479">Types the value as an `int`.</span></span>
* <span data-ttu-id="9e097-480">在 `NumberConfig` 屬性中儲存值供頁面使用。</span><span class="sxs-lookup"><span data-stu-id="9e097-480">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="9e097-481">GetSection、GetChildren 與 Exists</span><span class="sxs-lookup"><span data-stu-id="9e097-481">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="9e097-482">針對下面的範例，請考慮下列 JSON 檔案。</span><span class="sxs-lookup"><span data-stu-id="9e097-482">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="9e097-483">在兩個區段中找到四個機碼，其中一個包括子區段組：</span><span class="sxs-lookup"><span data-stu-id="9e097-483">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="9e097-484">當檔案被讀入到設定之後，會建立下列唯一階層式機碼以存放設定值：</span><span class="sxs-lookup"><span data-stu-id="9e097-484">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="9e097-485">section0:key0</span><span class="sxs-lookup"><span data-stu-id="9e097-485">section0:key0</span></span>
* <span data-ttu-id="9e097-486">section0:key1</span><span class="sxs-lookup"><span data-stu-id="9e097-486">section0:key1</span></span>
* <span data-ttu-id="9e097-487">section1:key0</span><span class="sxs-lookup"><span data-stu-id="9e097-487">section1:key0</span></span>
* <span data-ttu-id="9e097-488">section1:key1</span><span class="sxs-lookup"><span data-stu-id="9e097-488">section1:key1</span></span>
* <span data-ttu-id="9e097-489">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="9e097-489">section2:subsection0:key0</span></span>
* <span data-ttu-id="9e097-490">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="9e097-490">section2:subsection0:key1</span></span>
* <span data-ttu-id="9e097-491">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="9e097-491">section2:subsection1:key0</span></span>
* <span data-ttu-id="9e097-492">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="9e097-492">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="9e097-493">GetSection</span><span class="sxs-lookup"><span data-stu-id="9e097-493">GetSection</span></span>

<span data-ttu-id="9e097-494">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) 會擷取具有所指定子區段機碼的設定子區段。</span><span class="sxs-lookup"><span data-stu-id="9e097-494">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="9e097-495">若要傳回 `section1` 中只包含一個機碼值組的 <xref:Microsoft.Extensions.Configuration.IConfigurationSection>，請呼叫 `GetSection` 並提供區段名稱：</span><span class="sxs-lookup"><span data-stu-id="9e097-495">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="9e097-496">`configSection` 沒有值，只有索引鍵和路徑。</span><span class="sxs-lookup"><span data-stu-id="9e097-496">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="9e097-497">同樣地，若要取得 `section2:subsection0` 中之機碼的值，請呼叫 `GetSection` 並提供區段路徑：</span><span class="sxs-lookup"><span data-stu-id="9e097-497">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="9e097-498">`GetSection` 絕不會傳回 `null`。</span><span class="sxs-lookup"><span data-stu-id="9e097-498">`GetSection` never returns `null`.</span></span> <span data-ttu-id="9e097-499">若找不到相符的區段，會傳回空的 `IConfigurationSection`。</span><span class="sxs-lookup"><span data-stu-id="9e097-499">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="9e097-500">當 `GetSection` 傳回相符區段時，未填入 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value>。</span><span class="sxs-lookup"><span data-stu-id="9e097-500">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="9e097-501">當區段存在時，會傳回 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> 與 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path>。</span><span class="sxs-lookup"><span data-stu-id="9e097-501">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="9e097-502">GetChildren</span><span class="sxs-lookup"><span data-stu-id="9e097-502">GetChildren</span></span>

<span data-ttu-id="9e097-503">對 `section2` 上之 [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) 的呼叫會取得包括下列項目的 `IEnumerable<IConfigurationSection>`：</span><span class="sxs-lookup"><span data-stu-id="9e097-503">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="9e097-504">存在</span><span class="sxs-lookup"><span data-stu-id="9e097-504">Exists</span></span>

<span data-ttu-id="9e097-505">使用 [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) 來判斷設定區段是否存在：</span><span class="sxs-lookup"><span data-stu-id="9e097-505">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="9e097-506">以範例資料為例， `sectionExists` 是 `false`，因未設定資料中沒有 `section2:subsection2` 區段。</span><span class="sxs-lookup"><span data-stu-id="9e097-506">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="9e097-507">繫結到類別</span><span class="sxs-lookup"><span data-stu-id="9e097-507">Bind to a class</span></span>

<span data-ttu-id="9e097-508">設定可以繫結到類別，以使用*選項模式*代表相關設定的群組。</span><span class="sxs-lookup"><span data-stu-id="9e097-508">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="9e097-509">如需詳細資訊，請參閱<xref:fundamentals/configuration/options>。</span><span class="sxs-lookup"><span data-stu-id="9e097-509">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="9e097-510">設定值是以字串傳回，但是呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 會啟用 [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) 物件的建構。</span><span class="sxs-lookup"><span data-stu-id="9e097-510">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="9e097-511">範例應用程式包含 `Starship` 模型 (*Models/Starship.cs*)：</span><span class="sxs-lookup"><span data-stu-id="9e097-511">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="9e097-512">當範例應用程式使用 JSON 設定提供者來載入設定時，*starship.json* 檔案的 `starship` 區段會建立設定：</span><span class="sxs-lookup"><span data-stu-id="9e097-512">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="9e097-513">會建立下列設定機碼值組：</span><span class="sxs-lookup"><span data-stu-id="9e097-513">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="9e097-514">機碼</span><span class="sxs-lookup"><span data-stu-id="9e097-514">Key</span></span>                   | <span data-ttu-id="9e097-515">值</span><span class="sxs-lookup"><span data-stu-id="9e097-515">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="9e097-516">starship:name</span><span class="sxs-lookup"><span data-stu-id="9e097-516">starship:name</span></span>         | <span data-ttu-id="9e097-517">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="9e097-517">USS Enterprise</span></span>                                    |
| <span data-ttu-id="9e097-518">starship:registry</span><span class="sxs-lookup"><span data-stu-id="9e097-518">starship:registry</span></span>     | <span data-ttu-id="9e097-519">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="9e097-519">NCC-1701</span></span>                                          |
| <span data-ttu-id="9e097-520">starship:class</span><span class="sxs-lookup"><span data-stu-id="9e097-520">starship:class</span></span>        | <span data-ttu-id="9e097-521">Constitution</span><span class="sxs-lookup"><span data-stu-id="9e097-521">Constitution</span></span>                                      |
| <span data-ttu-id="9e097-522">starship:length</span><span class="sxs-lookup"><span data-stu-id="9e097-522">starship:length</span></span>       | <span data-ttu-id="9e097-523">304.8</span><span class="sxs-lookup"><span data-stu-id="9e097-523">304.8</span></span>                                             |
| <span data-ttu-id="9e097-524">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="9e097-524">starship:commissioned</span></span> | <span data-ttu-id="9e097-525">False</span><span class="sxs-lookup"><span data-stu-id="9e097-525">False</span></span>                                             |
| <span data-ttu-id="9e097-526">trademark</span><span class="sxs-lookup"><span data-stu-id="9e097-526">trademark</span></span>             | <span data-ttu-id="9e097-527">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="9e097-527">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="9e097-528">範例應用程式使用 `starship` 機碼呼叫 `GetSection`。</span><span class="sxs-lookup"><span data-stu-id="9e097-528">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="9e097-529">`starship` 機碼值組會被隔離。</span><span class="sxs-lookup"><span data-stu-id="9e097-529">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="9e097-530">會在子區段上呼叫 `Bind` 方法，並傳入 `Starship` 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="9e097-530">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="9e097-531">繫結執行個體值之後，執行個體會被指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="9e097-531">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="9e097-532">繫結至物件圖形</span><span class="sxs-lookup"><span data-stu-id="9e097-532">Bind to an object graph</span></span>

<span data-ttu-id="9e097-533"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 可以繫結整個 POCO 物件圖形。</span><span class="sxs-lookup"><span data-stu-id="9e097-533"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="9e097-534">範例包含 `TvShow` 模型，其物件圖形包括 `Metadata` 與 `Actors` 類別 (*Models/TvShow.cs*)：</span><span class="sxs-lookup"><span data-stu-id="9e097-534">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="9e097-535">範例應用程式有 *tvshow.xml* 檔案，其中包含設定資料：</span><span class="sxs-lookup"><span data-stu-id="9e097-535">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="9e097-536">設定會被繫結到整個 `TvShow` 物件 (使用 `Bind` 方法)。</span><span class="sxs-lookup"><span data-stu-id="9e097-536">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="9e097-537">已繫結的執行個體會被指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="9e097-537">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="9e097-538">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 會繫結並傳回所指定型別。</span><span class="sxs-lookup"><span data-stu-id="9e097-538">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="9e097-539">`Get<T>` 比使用 `Bind` 更方便。</span><span class="sxs-lookup"><span data-stu-id="9e097-539">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="9e097-540">下列程式碼顯示如何根據上面的範例使用 `Get<T>`，這可讓您直接將已繫結的執行個體指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="9e097-540">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="9e097-541">將陣列繫結到類別</span><span class="sxs-lookup"><span data-stu-id="9e097-541">Bind an array to a class</span></span>

<span data-ttu-id="9e097-542">範例應用程式示範此節中解釋的概念。</span><span class="sxs-lookup"><span data-stu-id="9e097-542">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="9e097-543"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="9e097-543">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="9e097-544">任何公開數值機碼區段 (`:0:`、`:1:`、&hellip; `:{n}:`) 的陣列格式都能繫結到POCO 類別陣列。</span><span class="sxs-lookup"><span data-stu-id="9e097-544">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="9e097-545">繫結是由慣例提供。</span><span class="sxs-lookup"><span data-stu-id="9e097-545">Binding is provided by convention.</span></span> <span data-ttu-id="9e097-546">自訂設定提供者不需要實作陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="9e097-546">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="9e097-547">**記憶體內陣列處理**</span><span class="sxs-lookup"><span data-stu-id="9e097-547">**In-memory array processing**</span></span>

<span data-ttu-id="9e097-548">考慮下表中顯示的設定機碼與值。</span><span class="sxs-lookup"><span data-stu-id="9e097-548">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="9e097-549">機碼</span><span class="sxs-lookup"><span data-stu-id="9e097-549">Key</span></span>             | <span data-ttu-id="9e097-550">值</span><span class="sxs-lookup"><span data-stu-id="9e097-550">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="9e097-551">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="9e097-551">array:entries:0</span></span> | <span data-ttu-id="9e097-552">value0</span><span class="sxs-lookup"><span data-stu-id="9e097-552">value0</span></span> |
| <span data-ttu-id="9e097-553">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="9e097-553">array:entries:1</span></span> | <span data-ttu-id="9e097-554">value1</span><span class="sxs-lookup"><span data-stu-id="9e097-554">value1</span></span> |
| <span data-ttu-id="9e097-555">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="9e097-555">array:entries:2</span></span> | <span data-ttu-id="9e097-556">value2</span><span class="sxs-lookup"><span data-stu-id="9e097-556">value2</span></span> |
| <span data-ttu-id="9e097-557">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="9e097-557">array:entries:4</span></span> | <span data-ttu-id="9e097-558">value4</span><span class="sxs-lookup"><span data-stu-id="9e097-558">value4</span></span> |
| <span data-ttu-id="9e097-559">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="9e097-559">array:entries:5</span></span> | <span data-ttu-id="9e097-560">value5</span><span class="sxs-lookup"><span data-stu-id="9e097-560">value5</span></span> |

<span data-ttu-id="9e097-561">這些機碼與值是使用「記憶體設定提供者」在範例應用程式中載入：</span><span class="sxs-lookup"><span data-stu-id="9e097-561">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

<span data-ttu-id="9e097-562">陣列會跳過索引 &num;3 的值。</span><span class="sxs-lookup"><span data-stu-id="9e097-562">The array skips a value for index &num;3.</span></span> <span data-ttu-id="9e097-563">設定繫結程式無法繫結 Null 值或在已繫結的物件中建立 Null 項目，這在示範繫結此陣列的結果到某物件的時候已經很清楚。</span><span class="sxs-lookup"><span data-stu-id="9e097-563">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="9e097-564">在範例應用程式中，POCO 類別可用來存放繫結設定資料：</span><span class="sxs-lookup"><span data-stu-id="9e097-564">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="9e097-565">設定資料已繫結到物件：</span><span class="sxs-lookup"><span data-stu-id="9e097-565">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="9e097-566">也可以使用 [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 語法，其會產生更精簡的程式碼：</span><span class="sxs-lookup"><span data-stu-id="9e097-566">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="9e097-567">繫結物件 (`ArrayExample`的執行個體) 會從設定接收陣列資料。</span><span class="sxs-lookup"><span data-stu-id="9e097-567">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="9e097-568">`ArrayExample.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="9e097-568">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="9e097-569">`ArrayExample.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="9e097-569">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="9e097-570">0</span><span class="sxs-lookup"><span data-stu-id="9e097-570">0</span></span>                            | <span data-ttu-id="9e097-571">value0</span><span class="sxs-lookup"><span data-stu-id="9e097-571">value0</span></span>                       |
| <span data-ttu-id="9e097-572">1</span><span class="sxs-lookup"><span data-stu-id="9e097-572">1</span></span>                            | <span data-ttu-id="9e097-573">value1</span><span class="sxs-lookup"><span data-stu-id="9e097-573">value1</span></span>                       |
| <span data-ttu-id="9e097-574">2</span><span class="sxs-lookup"><span data-stu-id="9e097-574">2</span></span>                            | <span data-ttu-id="9e097-575">value2</span><span class="sxs-lookup"><span data-stu-id="9e097-575">value2</span></span>                       |
| <span data-ttu-id="9e097-576">3</span><span class="sxs-lookup"><span data-stu-id="9e097-576">3</span></span>                            | <span data-ttu-id="9e097-577">value4</span><span class="sxs-lookup"><span data-stu-id="9e097-577">value4</span></span>                       |
| <span data-ttu-id="9e097-578">4</span><span class="sxs-lookup"><span data-stu-id="9e097-578">4</span></span>                            | <span data-ttu-id="9e097-579">value5</span><span class="sxs-lookup"><span data-stu-id="9e097-579">value5</span></span>                       |

<span data-ttu-id="9e097-580">繫結物件中的索引 &num;3 存放 `array:4` 設定機碼與其 `value4` 的設定資料。</span><span class="sxs-lookup"><span data-stu-id="9e097-580">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="9e097-581">當繫結包含陣列的設定資料時，設定機碼中的陣列索引只會用來列舉設定資料 (當建立物件時)。</span><span class="sxs-lookup"><span data-stu-id="9e097-581">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="9e097-582">設定資料中不能保留 Null 值，當設定機碼中的陣列略過一或多個索引時，不會在繫結物件中建立 Null 值項目。</span><span class="sxs-lookup"><span data-stu-id="9e097-582">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="9e097-583">由會在設定中產生正確機碼值組的任何設定提供者繫結到 `ArrayExample` 執行個體之前，可以提供索引 &num;3 缺少的設定項目。</span><span class="sxs-lookup"><span data-stu-id="9e097-583">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="9e097-584">若範例包含具有缺少之機碼值組的額外 JSON 設定提供者，`ArrayExample.Entries` 會符合完整的設定陣列：</span><span class="sxs-lookup"><span data-stu-id="9e097-584">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="9e097-585">*missing_value.json*：</span><span class="sxs-lookup"><span data-stu-id="9e097-585">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="9e097-586">在 `ConfigureAppConfiguration`中：</span><span class="sxs-lookup"><span data-stu-id="9e097-586">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="9e097-587">表格中顯示的機碼值組會載入到設定中。</span><span class="sxs-lookup"><span data-stu-id="9e097-587">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="9e097-588">機碼</span><span class="sxs-lookup"><span data-stu-id="9e097-588">Key</span></span>             | <span data-ttu-id="9e097-589">值</span><span class="sxs-lookup"><span data-stu-id="9e097-589">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="9e097-590">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="9e097-590">array:entries:3</span></span> | <span data-ttu-id="9e097-591">value3</span><span class="sxs-lookup"><span data-stu-id="9e097-591">value3</span></span> |

<span data-ttu-id="9e097-592">在「JSON 設定提供者」包含索引 &num;3 的項目之後，若繫結 `ArrayExample` 類別執行個體，`ArrayExample.Entries` 陣列會包括這些值：</span><span class="sxs-lookup"><span data-stu-id="9e097-592">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="9e097-593">`ArrayExample.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="9e097-593">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="9e097-594">`ArrayExample.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="9e097-594">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="9e097-595">0</span><span class="sxs-lookup"><span data-stu-id="9e097-595">0</span></span>                            | <span data-ttu-id="9e097-596">value0</span><span class="sxs-lookup"><span data-stu-id="9e097-596">value0</span></span>                       |
| <span data-ttu-id="9e097-597">1</span><span class="sxs-lookup"><span data-stu-id="9e097-597">1</span></span>                            | <span data-ttu-id="9e097-598">value1</span><span class="sxs-lookup"><span data-stu-id="9e097-598">value1</span></span>                       |
| <span data-ttu-id="9e097-599">2</span><span class="sxs-lookup"><span data-stu-id="9e097-599">2</span></span>                            | <span data-ttu-id="9e097-600">value2</span><span class="sxs-lookup"><span data-stu-id="9e097-600">value2</span></span>                       |
| <span data-ttu-id="9e097-601">3</span><span class="sxs-lookup"><span data-stu-id="9e097-601">3</span></span>                            | <span data-ttu-id="9e097-602">value3</span><span class="sxs-lookup"><span data-stu-id="9e097-602">value3</span></span>                       |
| <span data-ttu-id="9e097-603">4</span><span class="sxs-lookup"><span data-stu-id="9e097-603">4</span></span>                            | <span data-ttu-id="9e097-604">value4</span><span class="sxs-lookup"><span data-stu-id="9e097-604">value4</span></span>                       |
| <span data-ttu-id="9e097-605">5</span><span class="sxs-lookup"><span data-stu-id="9e097-605">5</span></span>                            | <span data-ttu-id="9e097-606">value5</span><span class="sxs-lookup"><span data-stu-id="9e097-606">value5</span></span>                       |

<span data-ttu-id="9e097-607">**JSON 陣列處理**</span><span class="sxs-lookup"><span data-stu-id="9e097-607">**JSON array processing**</span></span>

<span data-ttu-id="9e097-608">若 JSON 檔案包含陣列，會為具有以零為基礎之區段索引的陣列元素建立設定機碼。</span><span class="sxs-lookup"><span data-stu-id="9e097-608">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="9e097-609">在下列設定檔中，`subsection` 是陣列：</span><span class="sxs-lookup"><span data-stu-id="9e097-609">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="9e097-610">「JSON 設定提供者」會將設定資料讀入到下列機碼值組：</span><span class="sxs-lookup"><span data-stu-id="9e097-610">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="9e097-611">機碼</span><span class="sxs-lookup"><span data-stu-id="9e097-611">Key</span></span>                     | <span data-ttu-id="9e097-612">值</span><span class="sxs-lookup"><span data-stu-id="9e097-612">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="9e097-613">json_array:key</span><span class="sxs-lookup"><span data-stu-id="9e097-613">json_array:key</span></span>          | <span data-ttu-id="9e097-614">valueA</span><span class="sxs-lookup"><span data-stu-id="9e097-614">valueA</span></span> |
| <span data-ttu-id="9e097-615">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="9e097-615">json_array:subsection:0</span></span> | <span data-ttu-id="9e097-616">valueB</span><span class="sxs-lookup"><span data-stu-id="9e097-616">valueB</span></span> |
| <span data-ttu-id="9e097-617">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="9e097-617">json_array:subsection:1</span></span> | <span data-ttu-id="9e097-618">valueC</span><span class="sxs-lookup"><span data-stu-id="9e097-618">valueC</span></span> |
| <span data-ttu-id="9e097-619">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="9e097-619">json_array:subsection:2</span></span> | <span data-ttu-id="9e097-620">valueD</span><span class="sxs-lookup"><span data-stu-id="9e097-620">valueD</span></span> |

<span data-ttu-id="9e097-621">在範例應用程式中，下列 POCO 類別可用來繫結設定機碼值組：</span><span class="sxs-lookup"><span data-stu-id="9e097-621">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="9e097-622">在繫結之後，`JsonArrayExample.Key` 會存放值 `valueA`。</span><span class="sxs-lookup"><span data-stu-id="9e097-622">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="9e097-623">子區段值會存放在 POCO 陣列屬性 `Subsection` 中。</span><span class="sxs-lookup"><span data-stu-id="9e097-623">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="9e097-624">`JsonArrayExample.Subsection` 索引</span><span class="sxs-lookup"><span data-stu-id="9e097-624">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="9e097-625">`JsonArrayExample.Subsection` 值</span><span class="sxs-lookup"><span data-stu-id="9e097-625">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="9e097-626">0</span><span class="sxs-lookup"><span data-stu-id="9e097-626">0</span></span>                                   | <span data-ttu-id="9e097-627">valueB</span><span class="sxs-lookup"><span data-stu-id="9e097-627">valueB</span></span>                              |
| <span data-ttu-id="9e097-628">1</span><span class="sxs-lookup"><span data-stu-id="9e097-628">1</span></span>                                   | <span data-ttu-id="9e097-629">valueC</span><span class="sxs-lookup"><span data-stu-id="9e097-629">valueC</span></span>                              |
| <span data-ttu-id="9e097-630">2</span><span class="sxs-lookup"><span data-stu-id="9e097-630">2</span></span>                                   | <span data-ttu-id="9e097-631">valueD</span><span class="sxs-lookup"><span data-stu-id="9e097-631">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="9e097-632">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="9e097-632">Custom configuration provider</span></span>

<span data-ttu-id="9e097-633">範例應用程式示範如何建立會使用 [Entity Framework (EF)](/ef/core/) 從資料庫讀取設定機碼值組的基本設定提供者。</span><span class="sxs-lookup"><span data-stu-id="9e097-633">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="9e097-634">提供者具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="9e097-634">The provider has the following characteristics:</span></span>

* <span data-ttu-id="9e097-635">EF 記憶體內資料庫會用於示範用途。</span><span class="sxs-lookup"><span data-stu-id="9e097-635">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="9e097-636">若要使用需要連接字串的資料庫，請實作第二個 `ConfigurationBuilder` 以從另一個設定提供者提供連接字串。</span><span class="sxs-lookup"><span data-stu-id="9e097-636">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="9e097-637">啟動時，該提供者會將資料庫資料表讀入到設定中。</span><span class="sxs-lookup"><span data-stu-id="9e097-637">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="9e097-638">該提供者不會以個別機碼為基礎查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="9e097-638">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="9e097-639">未實作變更時重新載入，因此在應用程式啟動後更新資料庫對應用程式設定沒有影響。</span><span class="sxs-lookup"><span data-stu-id="9e097-639">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="9e097-640">定義 `EFConfigurationValue` 實體來在資料庫中存放設定值。</span><span class="sxs-lookup"><span data-stu-id="9e097-640">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9e097-641">*Models/EFConfigurationValue.cs*：</span><span class="sxs-lookup"><span data-stu-id="9e097-641">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="9e097-642">新增 `EFConfigurationContext` 以存放及存取已設定的值。</span><span class="sxs-lookup"><span data-stu-id="9e097-642">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="9e097-643">*EFConfigurationProvider/EFConfigurationContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="9e097-643">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="9e097-644">建立會實作 <xref:Microsoft.Extensions.Configuration.IConfigurationSource> 的類別。</span><span class="sxs-lookup"><span data-stu-id="9e097-644">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="9e097-645">*EFConfigurationProvider/EFConfigurationSource.cs*：</span><span class="sxs-lookup"><span data-stu-id="9e097-645">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="9e097-646">透過繼承自 <xref:Microsoft.Extensions.Configuration.ConfigurationProvider> 來建立自訂設定提供者。</span><span class="sxs-lookup"><span data-stu-id="9e097-646">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="9e097-647">若資料庫是空的，設定提供者會初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="9e097-647">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="9e097-648">*EFConfigurationProvider/EFConfigurationProvider.cs*：</span><span class="sxs-lookup"><span data-stu-id="9e097-648">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="9e097-649">`AddEFConfiguration` 擴充方法允許新增設定來源到 `ConfigurationBuilder`。</span><span class="sxs-lookup"><span data-stu-id="9e097-649">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="9e097-650">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="9e097-650">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="9e097-651">下列程式碼顯示如何在 *Program.cs* 中使用自訂 `EFConfigurationProvider`：</span><span class="sxs-lookup"><span data-stu-id="9e097-651">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9e097-652">*Models/EFConfigurationValue.cs*：</span><span class="sxs-lookup"><span data-stu-id="9e097-652">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="9e097-653">新增 `EFConfigurationContext` 以存放及存取已設定的值。</span><span class="sxs-lookup"><span data-stu-id="9e097-653">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="9e097-654">*EFConfigurationProvider/EFConfigurationContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="9e097-654">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="9e097-655">建立會實作 <xref:Microsoft.Extensions.Configuration.IConfigurationSource> 的類別。</span><span class="sxs-lookup"><span data-stu-id="9e097-655">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="9e097-656">*EFConfigurationProvider/EFConfigurationSource.cs*：</span><span class="sxs-lookup"><span data-stu-id="9e097-656">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="9e097-657">透過繼承自 <xref:Microsoft.Extensions.Configuration.ConfigurationProvider> 來建立自訂設定提供者。</span><span class="sxs-lookup"><span data-stu-id="9e097-657">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="9e097-658">若資料庫是空的，設定提供者會初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="9e097-658">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="9e097-659">*EFConfigurationProvider/EFConfigurationProvider.cs*：</span><span class="sxs-lookup"><span data-stu-id="9e097-659">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="9e097-660">`AddEFConfiguration` 擴充方法允許新增設定來源到 `ConfigurationBuilder`。</span><span class="sxs-lookup"><span data-stu-id="9e097-660">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="9e097-661">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="9e097-661">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="9e097-662">下列程式碼顯示如何在 *Program.cs* 中使用自訂 `EFConfigurationProvider`：</span><span class="sxs-lookup"><span data-stu-id="9e097-662">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="9e097-663">在啟動期間存取組態</span><span class="sxs-lookup"><span data-stu-id="9e097-663">Access configuration during startup</span></span>

<span data-ttu-id="9e097-664">將 `IConfiguration` 插入到 `Startup` 建構函式，以存取 `Startup.ConfigureServices` 中的設定值。</span><span class="sxs-lookup"><span data-stu-id="9e097-664">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9e097-665">若要存取 `Startup.Configure` 中的設定，請直接將 `IConfiguration` 插入到方法或從建構函式使用執行個體：</span><span class="sxs-lookup"><span data-stu-id="9e097-665">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="9e097-666">如需使用啟動方便方法來存取設定的範例，請參閱[應用程式啟動：方便方法](xref:fundamentals/startup#convenience-methods)。</span><span class="sxs-lookup"><span data-stu-id="9e097-666">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="9e097-667">存取 Razor Pages 頁面或 MVC 檢視中的設定</span><span class="sxs-lookup"><span data-stu-id="9e097-667">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="9e097-668">若要在 [Razor Pages 頁面] 頁面或 MVC 檢視中存取組態設定，請針對 [Microsoft.Extensions.Configuration 命名空間](xref:Microsoft.Extensions.Configuration) [using 指示詞](xref:mvc/views/razor#using) ([C# 參考：using 指示詞](/dotnet/csharp/language-reference/keywords/using-directive))，並將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 插入到頁面或檢視中。</span><span class="sxs-lookup"><span data-stu-id="9e097-668">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="9e097-669">在 [Razor 頁面] 頁面中：</span><span class="sxs-lookup"><span data-stu-id="9e097-669">In a Razor Pages page:</span></span>

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

<span data-ttu-id="9e097-670">在 MVC 檢視中：</span><span class="sxs-lookup"><span data-stu-id="9e097-670">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="9e097-671">從外部組件新增設定</span><span class="sxs-lookup"><span data-stu-id="9e097-671">Add configuration from an external assembly</span></span>

<span data-ttu-id="9e097-672"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。</span><span class="sxs-lookup"><span data-stu-id="9e097-672">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="9e097-673">如需詳細資訊，請參閱<xref:fundamentals/configuration/platform-specific-configuration>。</span><span class="sxs-lookup"><span data-stu-id="9e097-673">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9e097-674">其他資源</span><span class="sxs-lookup"><span data-stu-id="9e097-674">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
