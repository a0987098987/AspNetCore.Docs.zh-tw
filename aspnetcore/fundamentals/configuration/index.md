---
title: ASP.NET Core 的設定
author: guardrex
description: 了解如何使用組態 API 設定 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: 09ef06f179e34cd7f4f04ac30c3b5dd95d058244
ms.sourcegitcommit: 2388c2a7334ce66b6be3ffbab06dd7923df18f60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75951846"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="2d7fe-103">ASP.NET Core 的設定</span><span class="sxs-lookup"><span data-stu-id="2d7fe-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="2d7fe-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2d7fe-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2d7fe-105">ASP.NET Core 中的應用程式設定是以由*設定提供者*所建立的機碼值組為基礎。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="2d7fe-106">設定提供者會從各種設定來源將設定資料讀取到機碼值組中：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="2d7fe-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="2d7fe-107">Azure Key Vault</span></span>
* <span data-ttu-id="2d7fe-108">Azure 應用程式組態</span><span class="sxs-lookup"><span data-stu-id="2d7fe-108">Azure App Configuration</span></span>
* <span data-ttu-id="2d7fe-109">命令列引數</span><span class="sxs-lookup"><span data-stu-id="2d7fe-109">Command-line arguments</span></span>
* <span data-ttu-id="2d7fe-110">自訂提供者 (已安裝或已建立)</span><span class="sxs-lookup"><span data-stu-id="2d7fe-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="2d7fe-111">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="2d7fe-111">Directory files</span></span>
* <span data-ttu-id="2d7fe-112">環境變數</span><span class="sxs-lookup"><span data-stu-id="2d7fe-112">Environment variables</span></span>
* <span data-ttu-id="2d7fe-113">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="2d7fe-113">In-memory .NET objects</span></span>
* <span data-ttu-id="2d7fe-114">設定檔</span><span class="sxs-lookup"><span data-stu-id="2d7fe-114">Settings files</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2d7fe-115">一般組態提供者案例 ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) 的組態套件會以隱含方式包含在架構中。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2d7fe-116">一般組態提供者案例 ([microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) 的組態套件包含在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-116">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="2d7fe-117">下列程式碼範例與範例應用程式中的程式碼範例使用 <xref:Microsoft.Extensions.Configuration> 命名空間：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-117">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="2d7fe-118">*選項模式*是此主題中所述之設定概念的延伸。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-118">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="2d7fe-119">選項使用類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-119">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="2d7fe-120">如需詳細資訊，請參閱<xref:fundamentals/configuration/options>。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-120">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="2d7fe-121">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2d7fe-121">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="2d7fe-122">主機與應用程式組態的比較</span><span class="sxs-lookup"><span data-stu-id="2d7fe-122">Host versus app configuration</span></span>

<span data-ttu-id="2d7fe-123">設定及啟動應用程式之前，會先設定及啟動「主機」。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-123">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="2d7fe-124">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-124">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="2d7fe-125">應用程式與主機都是使用此主題中所述的設定提供者來設定的。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-125">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="2d7fe-126">主機組態機碼/值組也會包含在應用程式的組態中。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-126">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="2d7fe-127">如需有關當建置主機時如何使用設定提供者的詳細資訊，以及設定來源如何影響主機設定的詳細資訊，請參閱 <xref:fundamentals/index#host>。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-127">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="2d7fe-128">預設的組態</span><span class="sxs-lookup"><span data-stu-id="2d7fe-128">Default configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2d7fe-129">以 ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) 範本為基礎的 Web 應用程式，會在建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-129">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="2d7fe-130">`CreateDefaultBuilder` 會以下列順序提供應用程式的預設組態：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-130">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="2d7fe-131">下列項目適用於使用[一般主機](xref:fundamentals/host/generic-host)的應用程式。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-131">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="2d7fe-132">如需使用 [Web 主機](xref:fundamentals/host/web-host)時預設組態的詳細資料，請參閱[本主題的 ASP.NET Core 2.2 版本](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2)。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-132">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="2d7fe-133">主機組態的提供來源：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-133">Host configuration is provided from:</span></span>
  * <span data-ttu-id="2d7fe-134">使用[環境變數組態提供者](#environment-variables-configuration-provider)且以 `DOTNET_` 為前置詞 (例如 `DOTNET_ENVIRONMENT`) 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-134">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="2d7fe-135">載入設定機碼值組時，會移除前置詞 (`DOTNET_`)。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-135">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="2d7fe-136">使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-136">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="2d7fe-137">已建立 Web 主機預設組態 (`ConfigureWebHostDefaults`)：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-137">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="2d7fe-138">Kestrel 會用作為網頁伺服器，並使用應用程式的組態提供者來設定。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-138">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="2d7fe-139">新增主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-139">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="2d7fe-140">如果 `ASPNETCORE_FORWARDEDHEADERS_ENABLED` 環境變數設定為 `true`，則會新增轉接的標頭中介軟體。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-140">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="2d7fe-141">啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-141">Enable IIS integration.</span></span>
* <span data-ttu-id="2d7fe-142">應用程式設定的提供來源：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-142">App configuration is provided from:</span></span>
  * <span data-ttu-id="2d7fe-143">使用[檔案組態提供者](#file-configuration-provider)的 *appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-143">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="2d7fe-144">使用[檔案組態提供者](#file-configuration-provider)的 *appsettings.{Environment}.json*。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-144">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="2d7fe-145">應用程式在使用項目組件之 `Development` 環境中執行時的[秘密管理員](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-145">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="2d7fe-146">使用[環境變數組態提供者](#environment-variables-configuration-provider)的環境變數。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-146">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="2d7fe-147">使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-147">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2d7fe-148">以 ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) 範本為基礎的 Web 應用程式，會在建置主機時呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-148">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="2d7fe-149">`CreateDefaultBuilder` 會以下列順序提供應用程式的預設組態：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-149">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="2d7fe-150">下列項目適用於使用 [Web 主機](xref:fundamentals/host/web-host)的應用程式。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-150">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="2d7fe-151">如需使用[一般主機](xref:fundamentals/host/generic-host)時預設組態的詳細資料，請參閱[本主題的最新版本](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-151">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="2d7fe-152">主機組態的提供來源：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-152">Host configuration is provided from:</span></span>
  * <span data-ttu-id="2d7fe-153">使用[環境變數組態提供者](#environment-variables-configuration-provider)且以 `ASPNETCORE_` 為前置詞 (例如 `ASPNETCORE_ENVIRONMENT`) 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-153">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="2d7fe-154">載入設定機碼值組時，會移除前置詞 (`ASPNETCORE_`)。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-154">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="2d7fe-155">使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-155">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="2d7fe-156">應用程式設定的提供來源：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-156">App configuration is provided from:</span></span>
  * <span data-ttu-id="2d7fe-157">使用[檔案組態提供者](#file-configuration-provider)的 *appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-157">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="2d7fe-158">使用[檔案組態提供者](#file-configuration-provider)的 *appsettings.{Environment}.json*。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-158">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="2d7fe-159">應用程式在使用項目組件之 `Development` 環境中執行時的[秘密管理員](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-159">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="2d7fe-160">使用[環境變數組態提供者](#environment-variables-configuration-provider)的環境變數。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-160">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="2d7fe-161">使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-161">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

## <a name="security"></a><span data-ttu-id="2d7fe-162">安全性</span><span class="sxs-lookup"><span data-stu-id="2d7fe-162">Security</span></span>

<span data-ttu-id="2d7fe-163">採用下列做法來保護敏感性組態資料：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-163">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="2d7fe-164">永遠不要將密碼或其他敏感性資料儲存在設定提供者程式碼或純文字設定檔中。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-164">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="2d7fe-165">不要在開發或測試環境中使用生產環境祕密。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-165">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="2d7fe-166">請在專案外部指定祕密，以防止其意外認可至開放原始碼存放庫。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-166">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="2d7fe-167">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-167">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="2d7fe-168"><xref:security/app-secrets> &ndash; 包含有關使用環境變數來儲存敏感性資料的建議。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-168"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="2d7fe-169">「祕密管理員」使用「檔案設定提供者」以 JSON 檔案在本機系統上存放使用者祕密。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-169">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="2d7fe-170">此主題稍後將說明「檔案設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-170">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="2d7fe-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 可安全地儲存 ASP.NET Core 應用程式的應用程式祕密。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="2d7fe-172">如需詳細資訊，請參閱<xref:security/key-vault-configuration>。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-172">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="2d7fe-173">階層式設定資料</span><span class="sxs-lookup"><span data-stu-id="2d7fe-173">Hierarchical configuration data</span></span>

<span data-ttu-id="2d7fe-174">設定 API 可在設定機碼中使用分隔符號來壓平合併階層式資料，以管理階層式設定資料。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-174">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="2d7fe-175">在下列 JSON 檔案中，兩個區段的結構式階層中有四個機碼存在：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-175">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="2d7fe-176">當該檔案被讀入設定之後，會建立唯一機碼來維護設定來源的原始階層式資料結構。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-176">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="2d7fe-177">系統會使用冒號 (`:`) 將區段與機碼壓平合併以維護原始結構：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-177">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="2d7fe-178">section0:key0</span><span class="sxs-lookup"><span data-stu-id="2d7fe-178">section0:key0</span></span>
* <span data-ttu-id="2d7fe-179">section0:key1</span><span class="sxs-lookup"><span data-stu-id="2d7fe-179">section0:key1</span></span>
* <span data-ttu-id="2d7fe-180">section1:key0</span><span class="sxs-lookup"><span data-stu-id="2d7fe-180">section1:key0</span></span>
* <span data-ttu-id="2d7fe-181">section1:key1</span><span class="sxs-lookup"><span data-stu-id="2d7fe-181">section1:key1</span></span>

<span data-ttu-id="2d7fe-182"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> 與 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> 方法可用來在設定資料中隔離區段與區段的子系。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-182"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="2d7fe-183">[GetSection,、 GetChildren 與 Exists](#getsection-getchildren-and-exists) 中說明這些方法。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-183">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="2d7fe-184">慣例</span><span class="sxs-lookup"><span data-stu-id="2d7fe-184">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="2d7fe-185">來源和提供者</span><span class="sxs-lookup"><span data-stu-id="2d7fe-185">Sources and providers</span></span>

<span data-ttu-id="2d7fe-186">在應用程式啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-186">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="2d7fe-187">實作變更偵測的組態提供者能夠在基礎設定變更時重新載入組態。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-187">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="2d7fe-188">例如，檔案組態提供者 (將於本主題稍後討論) 和 [Azure Key Vault 組態提供者](xref:security/key-vault-configuration)均會實作變更偵測。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-188">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="2d7fe-189">您可以在應用程式的[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器中找到 <xref:Microsoft.Extensions.Configuration.IConfiguration>。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-189"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="2d7fe-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> 可以插入至 Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> 或 MVC <xref:Microsoft.AspNetCore.Mvc.Controller>，以取得類別的設定。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="2d7fe-191">在下列範例中，`_config` 欄位是用來存取設定值：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-191">In the following examples, the `_config` field is used to access configuration values:</span></span>

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

<span data-ttu-id="2d7fe-192">設定提供者無法使用 DI，因為當它們由主機設定時，它無法使用。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-192">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="2d7fe-193">按鍵</span><span class="sxs-lookup"><span data-stu-id="2d7fe-193">Keys</span></span>

<span data-ttu-id="2d7fe-194">設定機碼會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-194">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="2d7fe-195">機碼區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-195">Keys are case-insensitive.</span></span> <span data-ttu-id="2d7fe-196">例如，`ConnectionString` 與 `connectionstring` 會被視為相等的機碼。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-196">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="2d7fe-197">若相同機碼的值是由相同或不同設定提供者設定，在機碼上設定的最後一個值是所使用的值。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-197">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="2d7fe-198">階層式機碼</span><span class="sxs-lookup"><span data-stu-id="2d7fe-198">Hierarchical keys</span></span>
  * <span data-ttu-id="2d7fe-199">在設定 API 內，冒號分隔字元 (`:`) 可在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-199">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="2d7fe-200">在環境變數中，冒號分隔字元可能無法在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-200">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="2d7fe-201">所有平台都支援雙底線 (`__`)，且會自動轉換為冒號。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-201">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="2d7fe-202">在 Azure Key Vault 中，階層式機碼使用 `--` (兩個破折號) 來做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-202">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="2d7fe-203">撰寫程式碼，以在將密碼載入應用程式的設定時，以冒號取代破折號。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-203">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="2d7fe-204"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-204">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="2d7fe-205">[將陣列繫結到類別](#bind-an-array-to-a-class)一節說明陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-205">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="2d7fe-206">值</span><span class="sxs-lookup"><span data-stu-id="2d7fe-206">Values</span></span>

<span data-ttu-id="2d7fe-207">設定值會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-207">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="2d7fe-208">值是字串。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-208">Values are strings.</span></span>
* <span data-ttu-id="2d7fe-209">Null 值無法存放在設定中或繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-209">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="2d7fe-210">提供者</span><span class="sxs-lookup"><span data-stu-id="2d7fe-210">Providers</span></span>

<span data-ttu-id="2d7fe-211">下表顯示可供 ASP.NET Core 應用程式使用的設定提供者。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-211">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="2d7fe-212">Provider</span><span class="sxs-lookup"><span data-stu-id="2d7fe-212">Provider</span></span> | <span data-ttu-id="2d7fe-213">從&hellip;提供設定</span><span class="sxs-lookup"><span data-stu-id="2d7fe-213">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="2d7fe-214">[Azure Key Vault 設定提供者](xref:security/key-vault-configuration) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="2d7fe-214">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="2d7fe-215">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="2d7fe-215">Azure Key Vault</span></span> |
| <span data-ttu-id="2d7fe-216">[Azure 應用程式組態提供者](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure 文件)</span><span class="sxs-lookup"><span data-stu-id="2d7fe-216">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="2d7fe-217">Azure 應用程式組態</span><span class="sxs-lookup"><span data-stu-id="2d7fe-217">Azure App Configuration</span></span> |
| [<span data-ttu-id="2d7fe-218">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="2d7fe-218">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="2d7fe-219">命令列參數</span><span class="sxs-lookup"><span data-stu-id="2d7fe-219">Command-line parameters</span></span> |
| [<span data-ttu-id="2d7fe-220">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="2d7fe-220">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="2d7fe-221">自訂來源</span><span class="sxs-lookup"><span data-stu-id="2d7fe-221">Custom source</span></span> |
| [<span data-ttu-id="2d7fe-222">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="2d7fe-222">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="2d7fe-223">環境變數</span><span class="sxs-lookup"><span data-stu-id="2d7fe-223">Environment variables</span></span> |
| [<span data-ttu-id="2d7fe-224">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="2d7fe-224">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="2d7fe-225">檔案 (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="2d7fe-225">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="2d7fe-226">每個檔案機碼的設定提供者</span><span class="sxs-lookup"><span data-stu-id="2d7fe-226">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="2d7fe-227">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="2d7fe-227">Directory files</span></span> |
| [<span data-ttu-id="2d7fe-228">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="2d7fe-228">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="2d7fe-229">記憶體內集合</span><span class="sxs-lookup"><span data-stu-id="2d7fe-229">In-memory collections</span></span> |
| <span data-ttu-id="2d7fe-230">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題)</span><span class="sxs-lookup"><span data-stu-id="2d7fe-230">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="2d7fe-231">使用者設定檔目錄中的檔案</span><span class="sxs-lookup"><span data-stu-id="2d7fe-231">File in the user profile directory</span></span> |

<span data-ttu-id="2d7fe-232">在啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-232">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="2d7fe-233">本主題所描述的設定提供者會依字母順序描述，而不是程式碼排列它們的順序。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-233">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="2d7fe-234">請在程式碼中訂購設定提供者，以符合應用程式所需之基礎設定來源的優先順序。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-234">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="2d7fe-235">典型的設定提供者順序是：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-235">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="2d7fe-236">檔案 (*appsettings.json*、*appsettings.{Environment}.json*，其中 `{Environment}` 是應用程式的目前裝載環境)</span><span class="sxs-lookup"><span data-stu-id="2d7fe-236">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="2d7fe-237">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="2d7fe-237">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="2d7fe-238">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (僅限開發環境)</span><span class="sxs-lookup"><span data-stu-id="2d7fe-238">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="2d7fe-239">環境變數</span><span class="sxs-lookup"><span data-stu-id="2d7fe-239">Environment variables</span></span>
1. <span data-ttu-id="2d7fe-240">命令列引數</span><span class="sxs-lookup"><span data-stu-id="2d7fe-240">Command-line arguments</span></span>

<span data-ttu-id="2d7fe-241">將命令列組態提供者放在提供者序列結尾是常見做法，因為這樣可以讓命令列引數覆寫由其他提供者所設定的組態。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-241">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="2d7fe-242">當使用 `CreateDefaultBuilder`初始化新的主機產生器時，會使用上述的提供者序列。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-242">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="2d7fe-243">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-243">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="2d7fe-244">使用 ConfigureHostConfiguration 設定主機建立器</span><span class="sxs-lookup"><span data-stu-id="2d7fe-244">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="2d7fe-245">若要設定主機建立器，請呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> 並提供組態。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-245">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="2d7fe-246">`ConfigureHostConfiguration` 用於初始化 <xref:Microsoft.Extensions.Hosting.IHostEnvironment>，以便稍後在建置程序中使用。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-246">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="2d7fe-247">`ConfigureHostConfiguration` 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-247">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

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

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="2d7fe-248">使用 UseConfiguration 設定主機建立器</span><span class="sxs-lookup"><span data-stu-id="2d7fe-248">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="2d7fe-249">若要設定主機建立器，請使用該組態在主機建立器上呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-249">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

## <a name="configureappconfiguration"></a><span data-ttu-id="2d7fe-250">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="2d7fe-250">ConfigureAppConfiguration</span></span>

<span data-ttu-id="2d7fe-251">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定提供者，以及由 `CreateDefaultBuilder` 自動新增的設定提供者：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-251">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="2d7fe-252">使用命令列引數覆寫先前的組態</span><span class="sxs-lookup"><span data-stu-id="2d7fe-252">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="2d7fe-253">若要提供可使用命令列引數覆寫的應用程式組態，請最後呼叫 `AddCommandLine`：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-253">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="2d7fe-254">移除 CreateDefaultBuilder 新增的提供者</span><span class="sxs-lookup"><span data-stu-id="2d7fe-254">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="2d7fe-255">若要移除 `CreateDefaultBuilder`新增的提供者，請先在[IConfigurationBuilder](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources)上呼叫[Clear](/dotnet/api/system.collections.generic.icollection-1.clear) ：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-255">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="2d7fe-256">在應用程式啟動期間使用組態</span><span class="sxs-lookup"><span data-stu-id="2d7fe-256">Consume configuration during app startup</span></span>

<span data-ttu-id="2d7fe-257">應用程式啟動期間，可以使用 `ConfigureAppConfiguration` 中為應用程式提供的組態，包括 `Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-257">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="2d7fe-258">如需詳細資訊，請參閱[在啟動期間存取組態](#access-configuration-during-startup)一節。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-258">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="2d7fe-259">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="2d7fe-259">Command-line Configuration Provider</span></span>

<span data-ttu-id="2d7fe-260"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> 會在執行階段從命令列引數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-260">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="2d7fe-261">為了啟用命令列設定，<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 延伸模組方法會在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-261">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="2d7fe-262">呼叫 `CreateDefaultBuilder(string [])` 時，會自動呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-262">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="2d7fe-263">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-263">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="2d7fe-264">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-264">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="2d7fe-265">從 *appsettings.json* 與 *appsettings.{Environment}.json* 檔案載入其他選擇性組態。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-265">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="2d7fe-266">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-266">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="2d7fe-267">環境變數。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-267">Environment variables.</span></span>

<span data-ttu-id="2d7fe-268">`CreateDefaultBuilder` 會最後才新增命令列設定提供者。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-268">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="2d7fe-269">在執行階段傳遞的命令列引數會覆寫由其它提供者所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-269">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="2d7fe-270">`CreateDefaultBuilder` 會在建構主機時執行作業。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-270">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="2d7fe-271">因此，由`CreateDefaultBuilder` 啟用的命令列設定可以影響主機的設定方式。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-271">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="2d7fe-272">針對以 ASP.NET Core 範本為基礎的應用程式，`AddCommandLine` 已由 `CreateDefaultBuilder` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-272">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="2d7fe-273">若要新增其他組態提供者並維持能夠使用命令列引數覆寫這些提供者組態的能力，請在 `ConfigureAppConfiguration` 中呼叫應用程式的其他提供者，且最後呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-273">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="2d7fe-274">**範例**</span><span class="sxs-lookup"><span data-stu-id="2d7fe-274">**Example**</span></span>

<span data-ttu-id="2d7fe-275">範例應用程式利用靜態方便方法 `CreateDefaultBuilder` 的優勢來建置主機，這包括對 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-275">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="2d7fe-276">從專案目錄中開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-276">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="2d7fe-277">提供命令列引數給 `dotnet run` 命令 `dotnet run CommandLineKey=CommandLineValue`。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-277">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="2d7fe-278">在應用程式執行之後，開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-278">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="2d7fe-279">觀察輸出是否包含提供給 `dotnet run` 之設定命令列引數的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-279">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="2d7fe-280">Arguments</span><span class="sxs-lookup"><span data-stu-id="2d7fe-280">Arguments</span></span>

<span data-ttu-id="2d7fe-281">當值後面接著空格時，值必須接著等號 (`=`)，或機碼必須有前置詞 (`--` 或 `/`)。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-281">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="2d7fe-282">如果使用等號 (例如 `CommandLineKey=`)，則不需要此值。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-282">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="2d7fe-283">索引鍵前置字元</span><span class="sxs-lookup"><span data-stu-id="2d7fe-283">Key prefix</span></span>               | <span data-ttu-id="2d7fe-284">範例</span><span class="sxs-lookup"><span data-stu-id="2d7fe-284">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="2d7fe-285">沒有前置字元</span><span class="sxs-lookup"><span data-stu-id="2d7fe-285">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="2d7fe-286">雙虛線 (`--`)</span><span class="sxs-lookup"><span data-stu-id="2d7fe-286">Two dashes (`--`)</span></span>        | <span data-ttu-id="2d7fe-287">`--CommandLineKey2=value2`、 `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="2d7fe-287">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="2d7fe-288">正斜線 (`/`)</span><span class="sxs-lookup"><span data-stu-id="2d7fe-288">Forward slash (`/`)</span></span>      | <span data-ttu-id="2d7fe-289">`/CommandLineKey3=value3`、 `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="2d7fe-289">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="2d7fe-290">在相同的命令中，請不要混合使用等號搭配使用空格之機碼值組的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-290">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="2d7fe-291">範例命令：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-291">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="2d7fe-292">切換對應</span><span class="sxs-lookup"><span data-stu-id="2d7fe-292">Switch mappings</span></span>

<span data-ttu-id="2d7fe-293">參數對應允許索引鍵名稱取代邏輯。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-293">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="2d7fe-294">以 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>手動建立設定時，請將參數取代的字典提供給 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 方法。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-294">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="2d7fe-295">使用切換對應字典時，會檢查字典中是否有任何索引鍵符合命令列引數所提供的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-295">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="2d7fe-296">如果在字典中找到命令列索引鍵，則會傳回字典值 (索引鍵取代) 以在應用程式的設定中設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-296">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="2d7fe-297">所有前面加上單虛線 (`-`) 的命令列索引鍵都需要切換對應。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-297">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="2d7fe-298">切換對應字典索引鍵規則：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-298">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="2d7fe-299">切換必須以單虛線 (`-`) 或雙虛線 (`--`) 開頭。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-299">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="2d7fe-300">切換對應字典不能包含重複索引鍵。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-300">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="2d7fe-301">建立切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-301">Create a switch mappings dictionary.</span></span> <span data-ttu-id="2d7fe-302">下列範例會建立兩個切換對應：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-302">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="2d7fe-303">建立主機時，請使用切換對應字典呼叫 `AddCommandLine`：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-303">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="2d7fe-304">針對使用切換對應的應用程式，呼叫 `CreateDefaultBuilder` 不應傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-304">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="2d7fe-305">`CreateDefaultBuilder` 方法的 `AddCommandLine` 呼叫不包括對應的切換，且沒有任何方式可以將切換對應字典傳遞給 `CreateDefaultBuilder`。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-305">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="2d7fe-306">解決方案並非將引數傳遞給 `CreateDefaultBuilder`，而是允許 `ConfigurationBuilder` 方法的 `AddCommandLine` 方法同時處理引數與切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-306">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="2d7fe-307">建立切換對應字典之後，它會包含下表中所示的資料。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-307">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="2d7fe-308">索引鍵</span><span class="sxs-lookup"><span data-stu-id="2d7fe-308">Key</span></span>       | <span data-ttu-id="2d7fe-309">{2&gt;值&lt;2}</span><span class="sxs-lookup"><span data-stu-id="2d7fe-309">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="2d7fe-310">若啟動應用程式時使用切換對應機碼，設定會接收由字典提供之機碼上的設定值：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-310">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="2d7fe-311">執行上述命令之後，設定包含下表中顯示的值。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-311">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="2d7fe-312">索引鍵</span><span class="sxs-lookup"><span data-stu-id="2d7fe-312">Key</span></span>               | <span data-ttu-id="2d7fe-313">{2&gt;值&lt;2}</span><span class="sxs-lookup"><span data-stu-id="2d7fe-313">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="2d7fe-314">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="2d7fe-314">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="2d7fe-315"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> 會在執行階段從環境變數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-315">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="2d7fe-316">若要啟用環境變數設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-316">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="2d7fe-317">[Azure App Service](https://azure.microsoft.com/services/app-service/)允許在 Azure 入口網站中設定環境變數，以使用環境變數設定提供者來覆寫應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-317">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="2d7fe-318">如需詳細資訊，請參閱 [Azure App：使用 Azure 入口網站覆寫應用程式設定](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-318">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2d7fe-319">使用[一般主機](xref:fundamentals/host/generic-host)初始化新的主機建立器並呼叫 `CreateDefaultBuilder` 時，可使用 `AddEnvironmentVariables` 為[主機組態](#host-versus-app-configuration)載入字首為 `DOTNET_` 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-319">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="2d7fe-320">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-320">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2d7fe-321">使用 [Web 主機](xref:fundamentals/host/web-host)初始化新的主機建立器並呼叫 `CreateDefaultBuilder` 時，可使用 `AddEnvironmentVariables` 為[主機組態](#host-versus-app-configuration)載入字首為 `ASPNETCORE_` 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-321">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="2d7fe-322">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-322">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

<span data-ttu-id="2d7fe-323">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-323">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="2d7fe-324">來自無首碼之環境變數的應用程式組態 (在未提供首碼的情況下呼叫 `AddEnvironmentVariables`)。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-324">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="2d7fe-325">從 *appsettings.json* 與 *appsettings.{Environment}.json* 檔案載入其他選擇性組態。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-325">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="2d7fe-326">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-326">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="2d7fe-327">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-327">Command-line arguments.</span></span>

<span data-ttu-id="2d7fe-328">從使用者祕密與 *appsettings* 檔案建立設定之後，會呼叫「環境變數設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-328">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="2d7fe-329">在此位置呼叫提供者可讓系統在執行階段讀取環境變數，以覆寫由使用者祕密與 *appsettings* 檔案所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-329">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="2d7fe-330">若要從其他環境變數提供應用程式設定，請在 `ConfigureAppConfiguration` 中呼叫應用程式的其他提供者，並使用前置詞呼叫 `AddEnvironmentVariables`：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-330">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="2d7fe-331">呼叫 last `AddEnvironmentVariables`，以允許具有指定前置詞的環境變數覆寫其他提供者的值。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-331">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="2d7fe-332">**範例**</span><span class="sxs-lookup"><span data-stu-id="2d7fe-332">**Example**</span></span>

<span data-ttu-id="2d7fe-333">範例應用程式利用靜態方便方法 `CreateDefaultBuilder` 的優勢來建置主機，這包括對 `AddEnvironmentVariables` 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-333">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="2d7fe-334">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-334">Run the sample app.</span></span> <span data-ttu-id="2d7fe-335">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-335">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="2d7fe-336">觀察輸出是否包含環境變數 `ENVIRONMENT` 的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-336">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="2d7fe-337">值反映應用程式執行所在環境，在本機執行時，通常是 `Development`。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-337">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="2d7fe-338">為縮短由應用程式轉譯的環境變數清單，應用程式會篩選環境變數。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-338">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="2d7fe-339">請參閱範例應用程式的 *Pages/Index.cshtml.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-339">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="2d7fe-340">若要公開應用程式可用的所有環境變數，請將*Pages/Index. cshtml*中的 `FilteredConfiguration` 變更為下列內容：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-340">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="2d7fe-341">首碼</span><span class="sxs-lookup"><span data-stu-id="2d7fe-341">Prefixes</span></span>

<span data-ttu-id="2d7fe-342">在 `AddEnvironmentVariables` 方法中提供前置詞時，會篩選載入應用程式設定中的環境變數。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-342">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="2d7fe-343">例如，若要篩選前置詞為 `CUSTOM_` 的環境變數，請提供前置詞給設定提供者：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-343">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="2d7fe-344">建立設定機碼值組時，會移除前置詞。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-344">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="2d7fe-345">建立主機建立器時，主機組態由環境變數提供。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-345">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="2d7fe-346">如需這些環境變數所用前置詞的詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-346">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="2d7fe-347">**連接字串前置詞**</span><span class="sxs-lookup"><span data-stu-id="2d7fe-347">**Connection string prefixes**</span></span>

<span data-ttu-id="2d7fe-348">設定 API 四個連接字串環境變數的特殊處理規則，這些這些環境變數牽涉到針對應用程式環境設定 Azure 連接字串。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-348">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="2d7fe-349">若將前置詞提供給 `AddEnvironmentVariables`具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-349">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="2d7fe-350">連接字串前置詞</span><span class="sxs-lookup"><span data-stu-id="2d7fe-350">Connection string prefix</span></span> | <span data-ttu-id="2d7fe-351">Provider</span><span class="sxs-lookup"><span data-stu-id="2d7fe-351">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="2d7fe-352">自訂提供者</span><span class="sxs-lookup"><span data-stu-id="2d7fe-352">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="2d7fe-353">MySQL</span><span class="sxs-lookup"><span data-stu-id="2d7fe-353">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="2d7fe-354">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="2d7fe-354">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="2d7fe-355">SQL Server</span><span class="sxs-lookup"><span data-stu-id="2d7fe-355">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="2d7fe-356">當探索到具有下表所顯示之任何四個前置詞的環境變數並將其載入到設定中時：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-356">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="2d7fe-357">會透過移除環境變數前置詞並新增設定機碼區段 (`ConnectionStrings`) 來移除具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-357">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="2d7fe-358">會建立新的設定機碼值組以代表資料庫連線提供者 (`CUSTOMCONNSTR_` 除外，這沒有所述提供者)。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-358">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="2d7fe-359">環境變數機碼</span><span class="sxs-lookup"><span data-stu-id="2d7fe-359">Environment variable key</span></span> | <span data-ttu-id="2d7fe-360">已轉換的設定機碼</span><span class="sxs-lookup"><span data-stu-id="2d7fe-360">Converted configuration key</span></span> | <span data-ttu-id="2d7fe-361">提供者設定項目</span><span class="sxs-lookup"><span data-stu-id="2d7fe-361">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="2d7fe-362">設定項目未建立。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-362">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="2d7fe-363">機碼：`ConnectionStrings:<KEY>_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-363">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="2d7fe-364">值：`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="2d7fe-364">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="2d7fe-365">機碼：`ConnectionStrings:<KEY>_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-365">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="2d7fe-366">值：`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="2d7fe-366">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="2d7fe-367">機碼：`ConnectionStrings:<KEY>_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-367">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="2d7fe-368">值：`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="2d7fe-368">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="2d7fe-369">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="2d7fe-369">File Configuration Provider</span></span>

<span data-ttu-id="2d7fe-370"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> 是用於從檔案系統載入設定的基底類別。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-370"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="2d7fe-371">下列設定提供者專用於特定檔案類型：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-371">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="2d7fe-372">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="2d7fe-372">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="2d7fe-373">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="2d7fe-373">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="2d7fe-374">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="2d7fe-374">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="2d7fe-375">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="2d7fe-375">INI Configuration Provider</span></span>

<span data-ttu-id="2d7fe-376"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> 會在執行階段從 INI 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-376">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="2d7fe-377">若要啟用 INI 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-377">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="2d7fe-378">冒號可用來做為 INI 檔案設定中的區段分隔符號。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-378">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="2d7fe-379">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-379">Overloads permit specifying:</span></span>

* <span data-ttu-id="2d7fe-380">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-380">Whether the file is optional.</span></span>
* <span data-ttu-id="2d7fe-381">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-381">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="2d7fe-382"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-382">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="2d7fe-383">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-383">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="2d7fe-384">INI 設定檔的一般範例：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-384">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="2d7fe-385">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-385">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="2d7fe-386">section0:key0</span><span class="sxs-lookup"><span data-stu-id="2d7fe-386">section0:key0</span></span>
* <span data-ttu-id="2d7fe-387">section0:key1</span><span class="sxs-lookup"><span data-stu-id="2d7fe-387">section0:key1</span></span>
* <span data-ttu-id="2d7fe-388">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="2d7fe-388">section1:subsection:key</span></span>
* <span data-ttu-id="2d7fe-389">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="2d7fe-389">section2:subsection0:key</span></span>
* <span data-ttu-id="2d7fe-390">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="2d7fe-390">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="2d7fe-391">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="2d7fe-391">JSON Configuration Provider</span></span>

<span data-ttu-id="2d7fe-392"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> 會在執行階段從 JSON 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-392">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="2d7fe-393">若要啟用 JSON 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-393">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="2d7fe-394">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-394">Overloads permit specifying:</span></span>

* <span data-ttu-id="2d7fe-395">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-395">Whether the file is optional.</span></span>
* <span data-ttu-id="2d7fe-396">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-396">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="2d7fe-397"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-397">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="2d7fe-398">當使用 `CreateDefaultBuilder`初始化新的主機產生器時，會自動呼叫 `AddJsonFile` 兩次。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-398">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="2d7fe-399">會呼叫此方法以從下列位置載入設定：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-399">The method is called to load configuration from:</span></span>

* <span data-ttu-id="2d7fe-400">*appsettings*會先讀取此檔案 &ndash;。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-400">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="2d7fe-401">檔案的環境版本可以覆寫由 *appsettings.json* 檔案提供的值。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-401">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="2d7fe-402">*appsettings。{環境}. json* &ndash; 檔案的環境版本是根據[IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)載入。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-402">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="2d7fe-403">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-403">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="2d7fe-404">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-404">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="2d7fe-405">環境變數。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-405">Environment variables.</span></span>
* <span data-ttu-id="2d7fe-406">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-406">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="2d7fe-407">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-407">Command-line arguments.</span></span>

<span data-ttu-id="2d7fe-408">會先建立 JSON 設定提供者。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-408">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="2d7fe-409">因此，使用者祕密、環境變數與命令列引數會覆寫由 *appsettings* 檔案設定的設定。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-409">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="2d7fe-410">在建置主機時，呼叫 `ConfigureAppConfiguration` 以指定檔案 (除了 *appsettings.json* 和  *appsettings.{Environment}.json* 以外) 的應用程式設定：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-410">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="2d7fe-411">**範例**</span><span class="sxs-lookup"><span data-stu-id="2d7fe-411">**Example**</span></span>

<span data-ttu-id="2d7fe-412">範例應用程式會利用靜態便利方法 `CreateDefaultBuilder` 來建立主機，其中包括兩個 `AddJsonFile`呼叫：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-412">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="2d7fe-413">第一次呼叫 `AddJsonFile` 會從*appsettings*載入設定：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-413">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="2d7fe-414">第二次呼叫 `AddJsonFile` 會從 appsettings 載入設定 *。 {環境}. json*。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-414">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="2d7fe-415">適用于*appsettings。* 範例應用程式中的開發 json 會載入下列檔案：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-415">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="2d7fe-416">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-416">Run the sample app.</span></span> <span data-ttu-id="2d7fe-417">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-417">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="2d7fe-418">輸出包含以應用程式環境為基礎之設定的索引鍵/值組。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-418">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="2d7fe-419">在開發環境中執行應用程式時，會 `Debug` 金鑰 `Logging:LogLevel:Default` 的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-419">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="2d7fe-420">在生產環境中再次執行範例應用程式：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-420">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="2d7fe-421">開啟*Properties/launchsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-421">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="2d7fe-422">在 `ConfigurationSample` 設定檔中，將 `ASPNETCORE_ENVIRONMENT` 環境變數的值變更為 [`Production`]。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-422">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="2d7fe-423">儲存檔案，並使用命令 shell 中的 `dotnet run` 來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-423">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="2d7fe-424">Appsettings 中的設定 *。* 在*appsettings*中，不會再覆寫 json 中的設定。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-424">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="2d7fe-425">`Information`金鑰 `Logging:LogLevel:Default` 的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-425">The log level for the key `Logging:LogLevel:Default` is `Information`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="2d7fe-426">第一次呼叫 `AddJsonFile` 會從*appsettings*載入設定：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-426">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="2d7fe-427">第二次呼叫 `AddJsonFile` 會從 appsettings 載入設定 *。 {環境}. json*。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-427">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="2d7fe-428">適用于*appsettings。* 範例應用程式中的開發 json 會載入下列檔案：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-428">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="2d7fe-429">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-429">Run the sample app.</span></span> <span data-ttu-id="2d7fe-430">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-430">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="2d7fe-431">輸出包含以應用程式環境為基礎之設定的索引鍵/值組。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-431">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="2d7fe-432">在開發環境中執行應用程式時，會 `Debug` 金鑰 `Logging:LogLevel:Default` 的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-432">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="2d7fe-433">在生產環境中再次執行範例應用程式：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-433">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="2d7fe-434">開啟*Properties/launchsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-434">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="2d7fe-435">在 `ConfigurationSample` 設定檔中，將 `ASPNETCORE_ENVIRONMENT` 環境變數的值變更為 [`Production`]。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-435">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="2d7fe-436">儲存檔案，並使用命令 shell 中的 `dotnet run` 來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-436">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="2d7fe-437">Appsettings 中的設定 *。* 在*appsettings*中，不會再覆寫 json 中的設定。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-437">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="2d7fe-438">`Warning`金鑰 `Logging:LogLevel:Default` 的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-438">The log level for the key `Logging:LogLevel:Default` is `Warning`.</span></span>

::: moniker-end

### <a name="xml-configuration-provider"></a><span data-ttu-id="2d7fe-439">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="2d7fe-439">XML Configuration Provider</span></span>

<span data-ttu-id="2d7fe-440"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> 會在執行階段從 XML 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-440">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="2d7fe-441">若要啟用 XML 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-441">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="2d7fe-442">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-442">Overloads permit specifying:</span></span>

* <span data-ttu-id="2d7fe-443">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-443">Whether the file is optional.</span></span>
* <span data-ttu-id="2d7fe-444">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-444">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="2d7fe-445"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-445">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="2d7fe-446">建立設定機碼值組時，會忽略設定檔案的根節點。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-446">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="2d7fe-447">請勿在檔案中指定文件類型定義 (DTD) 或命名空間。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-447">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="2d7fe-448">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-448">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="2d7fe-449">XML 設定檔可以針對重複的區段使用相異元素名稱：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-449">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="2d7fe-450">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-450">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="2d7fe-451">section0:key0</span><span class="sxs-lookup"><span data-stu-id="2d7fe-451">section0:key0</span></span>
* <span data-ttu-id="2d7fe-452">section0:key1</span><span class="sxs-lookup"><span data-stu-id="2d7fe-452">section0:key1</span></span>
* <span data-ttu-id="2d7fe-453">section1:key0</span><span class="sxs-lookup"><span data-stu-id="2d7fe-453">section1:key0</span></span>
* <span data-ttu-id="2d7fe-454">section1:key1</span><span class="sxs-lookup"><span data-stu-id="2d7fe-454">section1:key1</span></span>

<span data-ttu-id="2d7fe-455">若 `name` 屬性是用來區別元素，則可以重複那些使用相同元素名稱的元素：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-455">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="2d7fe-456">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-456">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="2d7fe-457">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="2d7fe-457">section:section0:key:key0</span></span>
* <span data-ttu-id="2d7fe-458">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="2d7fe-458">section:section0:key:key1</span></span>
* <span data-ttu-id="2d7fe-459">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="2d7fe-459">section:section1:key:key0</span></span>
* <span data-ttu-id="2d7fe-460">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="2d7fe-460">section:section1:key:key1</span></span>

<span data-ttu-id="2d7fe-461">屬性可用來提供值：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-461">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="2d7fe-462">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-462">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="2d7fe-463">key:attribute</span><span class="sxs-lookup"><span data-stu-id="2d7fe-463">key:attribute</span></span>
* <span data-ttu-id="2d7fe-464">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="2d7fe-464">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="2d7fe-465">每個檔案機碼設定提供者</span><span class="sxs-lookup"><span data-stu-id="2d7fe-465">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="2d7fe-466"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> 使用目錄的檔案做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-466">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="2d7fe-467">機碼是檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-467">The key is the file name.</span></span> <span data-ttu-id="2d7fe-468">值包含檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-468">The value contains the file's contents.</span></span> <span data-ttu-id="2d7fe-469">每個檔案機碼設定提供者是在 Docker 主機案例中使用。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-469">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="2d7fe-470">若要啟用每個檔案機碼設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-470">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="2d7fe-471">檔案的 `directoryPath` 必須是絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-471">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="2d7fe-472">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-472">Overloads permit specifying:</span></span>

* <span data-ttu-id="2d7fe-473">設定來源的 `Action<KeyPerFileConfigurationSource>` 委派。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-473">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="2d7fe-474">目錄是否為選擇性與目錄的路徑。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-474">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="2d7fe-475">雙底線 (`__`) 是做為檔案名稱中的設定金鑰分隔符號使用。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-475">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="2d7fe-476">例如，檔案名稱 `Logging__LogLevel__System` 會產生設定金鑰 `Logging:LogLevel:System`。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-476">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="2d7fe-477">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-477">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="2d7fe-478">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="2d7fe-478">Memory Configuration Provider</span></span>

<span data-ttu-id="2d7fe-479"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> 使用記憶體內集合做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-479">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="2d7fe-480">若要啟用記憶體內集合設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-480">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="2d7fe-481">設定提供者可以使用 `IEnumerable<KeyValuePair<String,String>>` 來初始化。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-481">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="2d7fe-482">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-482">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="2d7fe-483">下列範例會建立組態字典：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-483">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="2d7fe-484">字典會與 `AddInMemoryCollection` 的呼叫搭配使用以提供組態：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-484">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="2d7fe-485">GetValue</span><span class="sxs-lookup"><span data-stu-id="2d7fe-485">GetValue</span></span>

<span data-ttu-id="2d7fe-486">[ConfigurationBinder\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*)會使用指定的索引鍵從設定中解壓縮單一值，並將它轉換成指定的 noncollection 類型。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-486">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="2d7fe-487">多載會接受預設值。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-487">An overload accepts a default value.</span></span>

<span data-ttu-id="2d7fe-488">下列範例︰</span><span class="sxs-lookup"><span data-stu-id="2d7fe-488">The following example:</span></span>

* <span data-ttu-id="2d7fe-489">從具有機碼 `NumberKey` 的組態擷取字串值。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-489">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="2d7fe-490">若在組態機碼中找不到 `NumberKey`，則會使用預設值 `99`。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-490">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="2d7fe-491">鍵入值為 `int`。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-491">Types the value as an `int`.</span></span>
* <span data-ttu-id="2d7fe-492">在 `NumberConfig` 屬性中儲存值供頁面使用。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-492">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="2d7fe-493">GetSection、GetChildren 與 Exists</span><span class="sxs-lookup"><span data-stu-id="2d7fe-493">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="2d7fe-494">針對下面的範例，請考慮下列 JSON 檔案。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-494">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="2d7fe-495">在兩個區段中找到四個機碼，其中一個包括子區段組：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-495">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="2d7fe-496">當檔案被讀入到設定之後，會建立下列唯一階層式機碼以存放設定值：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-496">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="2d7fe-497">section0:key0</span><span class="sxs-lookup"><span data-stu-id="2d7fe-497">section0:key0</span></span>
* <span data-ttu-id="2d7fe-498">section0:key1</span><span class="sxs-lookup"><span data-stu-id="2d7fe-498">section0:key1</span></span>
* <span data-ttu-id="2d7fe-499">section1:key0</span><span class="sxs-lookup"><span data-stu-id="2d7fe-499">section1:key0</span></span>
* <span data-ttu-id="2d7fe-500">section1:key1</span><span class="sxs-lookup"><span data-stu-id="2d7fe-500">section1:key1</span></span>
* <span data-ttu-id="2d7fe-501">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="2d7fe-501">section2:subsection0:key0</span></span>
* <span data-ttu-id="2d7fe-502">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="2d7fe-502">section2:subsection0:key1</span></span>
* <span data-ttu-id="2d7fe-503">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="2d7fe-503">section2:subsection1:key0</span></span>
* <span data-ttu-id="2d7fe-504">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="2d7fe-504">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="2d7fe-505">GetSection</span><span class="sxs-lookup"><span data-stu-id="2d7fe-505">GetSection</span></span>

<span data-ttu-id="2d7fe-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) 會擷取具有所指定子區段機碼的設定子區段。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="2d7fe-507">若要傳回 `section1` 中只包含一個機碼值組的 <xref:Microsoft.Extensions.Configuration.IConfigurationSection>，請呼叫 `GetSection` 並提供區段名稱：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-507">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="2d7fe-508">`configSection` 沒有值，只有索引鍵和路徑。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-508">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="2d7fe-509">同樣地，若要取得 `section2:subsection0` 中之機碼的值，請呼叫 `GetSection` 並提供區段路徑：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-509">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="2d7fe-510">`GetSection` 絕不會傳回 `null`。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-510">`GetSection` never returns `null`.</span></span> <span data-ttu-id="2d7fe-511">若找不到相符的區段，會傳回空的 `IConfigurationSection`。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-511">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="2d7fe-512">當 `GetSection` 傳回相符區段時，未填入 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value>。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-512">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="2d7fe-513">當區段存在時，會傳回 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> 與 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path>。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-513">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="2d7fe-514">GetChildren</span><span class="sxs-lookup"><span data-stu-id="2d7fe-514">GetChildren</span></span>

<span data-ttu-id="2d7fe-515">對 `section2` 上之 [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) 的呼叫會取得包括下列項目的 `IEnumerable<IConfigurationSection>`：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-515">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="2d7fe-516">存在</span><span class="sxs-lookup"><span data-stu-id="2d7fe-516">Exists</span></span>

<span data-ttu-id="2d7fe-517">使用 [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) 來判斷設定區段是否存在：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-517">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="2d7fe-518">以範例資料為例， `sectionExists` 是 `false`，因未設定資料中沒有 `section2:subsection2` 區段。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-518">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="2d7fe-519">繫結到類別</span><span class="sxs-lookup"><span data-stu-id="2d7fe-519">Bind to a class</span></span>

<span data-ttu-id="2d7fe-520">設定可以繫結到類別，以使用*選項模式*代表相關設定的群組。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-520">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="2d7fe-521">如需詳細資訊，請參閱<xref:fundamentals/configuration/options>。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-521">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="2d7fe-522">設定值是以字串傳回，但是呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 會啟用 [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) 物件的建構。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-522">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="2d7fe-523">範例應用程式包含 `Starship` 模型 (*Models/Starship.cs*)：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-523">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="2d7fe-524">當範例應用程式使用 JSON 設定提供者來載入設定時，*starship.json* 檔案的 `starship` 區段會建立設定：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-524">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="2d7fe-525">會建立下列設定機碼值組：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-525">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="2d7fe-526">索引鍵</span><span class="sxs-lookup"><span data-stu-id="2d7fe-526">Key</span></span>                   | <span data-ttu-id="2d7fe-527">{2&gt;值&lt;2}</span><span class="sxs-lookup"><span data-stu-id="2d7fe-527">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="2d7fe-528">starship:name</span><span class="sxs-lookup"><span data-stu-id="2d7fe-528">starship:name</span></span>         | <span data-ttu-id="2d7fe-529">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="2d7fe-529">USS Enterprise</span></span>                                    |
| <span data-ttu-id="2d7fe-530">starship:registry</span><span class="sxs-lookup"><span data-stu-id="2d7fe-530">starship:registry</span></span>     | <span data-ttu-id="2d7fe-531">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="2d7fe-531">NCC-1701</span></span>                                          |
| <span data-ttu-id="2d7fe-532">starship:class</span><span class="sxs-lookup"><span data-stu-id="2d7fe-532">starship:class</span></span>        | <span data-ttu-id="2d7fe-533">Constitution</span><span class="sxs-lookup"><span data-stu-id="2d7fe-533">Constitution</span></span>                                      |
| <span data-ttu-id="2d7fe-534">starship:length</span><span class="sxs-lookup"><span data-stu-id="2d7fe-534">starship:length</span></span>       | <span data-ttu-id="2d7fe-535">304.8</span><span class="sxs-lookup"><span data-stu-id="2d7fe-535">304.8</span></span>                                             |
| <span data-ttu-id="2d7fe-536">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="2d7fe-536">starship:commissioned</span></span> | <span data-ttu-id="2d7fe-537">False</span><span class="sxs-lookup"><span data-stu-id="2d7fe-537">False</span></span>                                             |
| <span data-ttu-id="2d7fe-538">trademark</span><span class="sxs-lookup"><span data-stu-id="2d7fe-538">trademark</span></span>             | <span data-ttu-id="2d7fe-539">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="2d7fe-539">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="2d7fe-540">範例應用程式使用 `starship` 機碼呼叫 `GetSection`。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-540">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="2d7fe-541">`starship` 機碼值組會被隔離。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-541">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="2d7fe-542">會在子區段上呼叫 `Bind` 方法，並傳入 `Starship` 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-542">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="2d7fe-543">繫結執行個體值之後，執行個體會被指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-543">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="2d7fe-544">繫結至物件圖形</span><span class="sxs-lookup"><span data-stu-id="2d7fe-544">Bind to an object graph</span></span>

<span data-ttu-id="2d7fe-545"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 可以繫結整個 POCO 物件圖形。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-545"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="2d7fe-546">範例包含 `TvShow` 模型，其物件圖形包括 `Metadata` 與 `Actors` 類別 (*Models/TvShow.cs*)：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-546">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="2d7fe-547">範例應用程式有 *tvshow.xml* 檔案，其中包含設定資料：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-547">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="2d7fe-548">設定會被繫結到整個 `TvShow` 物件 (使用 `Bind` 方法)。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-548">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="2d7fe-549">已繫結的執行個體會被指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-549">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="2d7fe-550">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 會繫結並傳回所指定型別。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-550">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="2d7fe-551">`Get<T>` 比使用 `Bind` 更方便。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-551">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="2d7fe-552">下列程式碼顯示如何根據上面的範例使用 `Get<T>`，這可讓您直接將已繫結的執行個體指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-552">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="2d7fe-553">將陣列繫結到類別</span><span class="sxs-lookup"><span data-stu-id="2d7fe-553">Bind an array to a class</span></span>

<span data-ttu-id="2d7fe-554">範例應用程式示範此節中解釋的概念。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-554">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="2d7fe-555"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-555">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="2d7fe-556">公開數值索引鍵區段（`:0:`、`:1:`&hellip; `:{n}:`）的任何陣列格式，都能夠系結至 POCO 類別陣列。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-556">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="2d7fe-557">繫結是由慣例提供。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-557">Binding is provided by convention.</span></span> <span data-ttu-id="2d7fe-558">自訂設定提供者不需要實作陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-558">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="2d7fe-559">**記憶體內陣列處理**</span><span class="sxs-lookup"><span data-stu-id="2d7fe-559">**In-memory array processing**</span></span>

<span data-ttu-id="2d7fe-560">考慮下表中顯示的設定機碼與值。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-560">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="2d7fe-561">索引鍵</span><span class="sxs-lookup"><span data-stu-id="2d7fe-561">Key</span></span>             | <span data-ttu-id="2d7fe-562">{2&gt;值&lt;2}</span><span class="sxs-lookup"><span data-stu-id="2d7fe-562">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="2d7fe-563">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="2d7fe-563">array:entries:0</span></span> | <span data-ttu-id="2d7fe-564">value0</span><span class="sxs-lookup"><span data-stu-id="2d7fe-564">value0</span></span> |
| <span data-ttu-id="2d7fe-565">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="2d7fe-565">array:entries:1</span></span> | <span data-ttu-id="2d7fe-566">value1</span><span class="sxs-lookup"><span data-stu-id="2d7fe-566">value1</span></span> |
| <span data-ttu-id="2d7fe-567">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="2d7fe-567">array:entries:2</span></span> | <span data-ttu-id="2d7fe-568">value2</span><span class="sxs-lookup"><span data-stu-id="2d7fe-568">value2</span></span> |
| <span data-ttu-id="2d7fe-569">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="2d7fe-569">array:entries:4</span></span> | <span data-ttu-id="2d7fe-570">value4</span><span class="sxs-lookup"><span data-stu-id="2d7fe-570">value4</span></span> |
| <span data-ttu-id="2d7fe-571">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="2d7fe-571">array:entries:5</span></span> | <span data-ttu-id="2d7fe-572">value5</span><span class="sxs-lookup"><span data-stu-id="2d7fe-572">value5</span></span> |

<span data-ttu-id="2d7fe-573">這些機碼與值是使用「記憶體設定提供者」在範例應用程式中載入：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-573">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

<span data-ttu-id="2d7fe-574">陣列會跳過索引 &num;3 的值。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-574">The array skips a value for index &num;3.</span></span> <span data-ttu-id="2d7fe-575">設定繫結程式無法繫結 Null 值或在已繫結的物件中建立 Null 項目，這在示範繫結此陣列的結果到某物件的時候已經很清楚。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-575">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="2d7fe-576">在範例應用程式中，POCO 類別可用來存放繫結設定資料：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-576">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="2d7fe-577">設定資料已繫結到物件：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-577">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="2d7fe-578">也可以使用 [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 語法，其會產生更精簡的程式碼：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-578">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="2d7fe-579">繫結物件 (`ArrayExample`的執行個體) 會從設定接收陣列資料。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-579">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="2d7fe-580">`ArrayExample.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="2d7fe-580">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="2d7fe-581">`ArrayExample.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="2d7fe-581">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="2d7fe-582">0</span><span class="sxs-lookup"><span data-stu-id="2d7fe-582">0</span></span>                            | <span data-ttu-id="2d7fe-583">value0</span><span class="sxs-lookup"><span data-stu-id="2d7fe-583">value0</span></span>                       |
| <span data-ttu-id="2d7fe-584">1</span><span class="sxs-lookup"><span data-stu-id="2d7fe-584">1</span></span>                            | <span data-ttu-id="2d7fe-585">value1</span><span class="sxs-lookup"><span data-stu-id="2d7fe-585">value1</span></span>                       |
| <span data-ttu-id="2d7fe-586">2</span><span class="sxs-lookup"><span data-stu-id="2d7fe-586">2</span></span>                            | <span data-ttu-id="2d7fe-587">value2</span><span class="sxs-lookup"><span data-stu-id="2d7fe-587">value2</span></span>                       |
| <span data-ttu-id="2d7fe-588">3</span><span class="sxs-lookup"><span data-stu-id="2d7fe-588">3</span></span>                            | <span data-ttu-id="2d7fe-589">value4</span><span class="sxs-lookup"><span data-stu-id="2d7fe-589">value4</span></span>                       |
| <span data-ttu-id="2d7fe-590">4</span><span class="sxs-lookup"><span data-stu-id="2d7fe-590">4</span></span>                            | <span data-ttu-id="2d7fe-591">value5</span><span class="sxs-lookup"><span data-stu-id="2d7fe-591">value5</span></span>                       |

<span data-ttu-id="2d7fe-592">繫結物件中的索引 &num;3 存放 `array:4` 設定機碼與其 `value4` 的設定資料。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-592">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="2d7fe-593">當繫結包含陣列的設定資料時，設定機碼中的陣列索引只會用來列舉設定資料 (當建立物件時)。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-593">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="2d7fe-594">設定資料中不能保留 Null 值，當設定機碼中的陣列略過一或多個索引時，不會在繫結物件中建立 Null 值項目。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-594">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="2d7fe-595">由會在設定中產生正確機碼值組的任何設定提供者繫結到 `ArrayExample` 執行個體之前，可以提供索引 &num;3 缺少的設定項目。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-595">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="2d7fe-596">若範例包含具有缺少之機碼值組的額外 JSON 設定提供者，`ArrayExample.Entries` 會符合完整的設定陣列：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-596">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="2d7fe-597">*missing_value.json*：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-597">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="2d7fe-598">在 `ConfigureAppConfiguration`中：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-598">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="2d7fe-599">表格中顯示的機碼值組會載入到設定中。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-599">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="2d7fe-600">索引鍵</span><span class="sxs-lookup"><span data-stu-id="2d7fe-600">Key</span></span>             | <span data-ttu-id="2d7fe-601">{2&gt;值&lt;2}</span><span class="sxs-lookup"><span data-stu-id="2d7fe-601">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="2d7fe-602">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="2d7fe-602">array:entries:3</span></span> | <span data-ttu-id="2d7fe-603">value3</span><span class="sxs-lookup"><span data-stu-id="2d7fe-603">value3</span></span> |

<span data-ttu-id="2d7fe-604">在「JSON 設定提供者」包含索引 &num;3 的項目之後，若繫結 `ArrayExample` 類別執行個體，`ArrayExample.Entries` 陣列會包括這些值：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-604">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="2d7fe-605">`ArrayExample.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="2d7fe-605">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="2d7fe-606">`ArrayExample.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="2d7fe-606">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="2d7fe-607">0</span><span class="sxs-lookup"><span data-stu-id="2d7fe-607">0</span></span>                            | <span data-ttu-id="2d7fe-608">value0</span><span class="sxs-lookup"><span data-stu-id="2d7fe-608">value0</span></span>                       |
| <span data-ttu-id="2d7fe-609">1</span><span class="sxs-lookup"><span data-stu-id="2d7fe-609">1</span></span>                            | <span data-ttu-id="2d7fe-610">value1</span><span class="sxs-lookup"><span data-stu-id="2d7fe-610">value1</span></span>                       |
| <span data-ttu-id="2d7fe-611">2</span><span class="sxs-lookup"><span data-stu-id="2d7fe-611">2</span></span>                            | <span data-ttu-id="2d7fe-612">value2</span><span class="sxs-lookup"><span data-stu-id="2d7fe-612">value2</span></span>                       |
| <span data-ttu-id="2d7fe-613">3</span><span class="sxs-lookup"><span data-stu-id="2d7fe-613">3</span></span>                            | <span data-ttu-id="2d7fe-614">value3</span><span class="sxs-lookup"><span data-stu-id="2d7fe-614">value3</span></span>                       |
| <span data-ttu-id="2d7fe-615">4</span><span class="sxs-lookup"><span data-stu-id="2d7fe-615">4</span></span>                            | <span data-ttu-id="2d7fe-616">value4</span><span class="sxs-lookup"><span data-stu-id="2d7fe-616">value4</span></span>                       |
| <span data-ttu-id="2d7fe-617">5</span><span class="sxs-lookup"><span data-stu-id="2d7fe-617">5</span></span>                            | <span data-ttu-id="2d7fe-618">value5</span><span class="sxs-lookup"><span data-stu-id="2d7fe-618">value5</span></span>                       |

<span data-ttu-id="2d7fe-619">**JSON 陣列處理**</span><span class="sxs-lookup"><span data-stu-id="2d7fe-619">**JSON array processing**</span></span>

<span data-ttu-id="2d7fe-620">若 JSON 檔案包含陣列，會為具有以零為基礎之區段索引的陣列元素建立設定機碼。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-620">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="2d7fe-621">在下列設定檔中，`subsection` 是陣列：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-621">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="2d7fe-622">「JSON 設定提供者」會將設定資料讀入到下列機碼值組：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-622">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="2d7fe-623">索引鍵</span><span class="sxs-lookup"><span data-stu-id="2d7fe-623">Key</span></span>                     | <span data-ttu-id="2d7fe-624">{2&gt;值&lt;2}</span><span class="sxs-lookup"><span data-stu-id="2d7fe-624">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="2d7fe-625">json_array:key</span><span class="sxs-lookup"><span data-stu-id="2d7fe-625">json_array:key</span></span>          | <span data-ttu-id="2d7fe-626">valueA</span><span class="sxs-lookup"><span data-stu-id="2d7fe-626">valueA</span></span> |
| <span data-ttu-id="2d7fe-627">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="2d7fe-627">json_array:subsection:0</span></span> | <span data-ttu-id="2d7fe-628">valueB</span><span class="sxs-lookup"><span data-stu-id="2d7fe-628">valueB</span></span> |
| <span data-ttu-id="2d7fe-629">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="2d7fe-629">json_array:subsection:1</span></span> | <span data-ttu-id="2d7fe-630">valueC</span><span class="sxs-lookup"><span data-stu-id="2d7fe-630">valueC</span></span> |
| <span data-ttu-id="2d7fe-631">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="2d7fe-631">json_array:subsection:2</span></span> | <span data-ttu-id="2d7fe-632">valueD</span><span class="sxs-lookup"><span data-stu-id="2d7fe-632">valueD</span></span> |

<span data-ttu-id="2d7fe-633">在範例應用程式中，下列 POCO 類別可用來繫結設定機碼值組：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-633">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="2d7fe-634">在繫結之後，`JsonArrayExample.Key` 會存放值 `valueA`。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-634">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="2d7fe-635">子區段值會存放在 POCO 陣列屬性 `Subsection` 中。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-635">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="2d7fe-636">`JsonArrayExample.Subsection` 索引</span><span class="sxs-lookup"><span data-stu-id="2d7fe-636">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="2d7fe-637">`JsonArrayExample.Subsection` 值</span><span class="sxs-lookup"><span data-stu-id="2d7fe-637">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="2d7fe-638">0</span><span class="sxs-lookup"><span data-stu-id="2d7fe-638">0</span></span>                                   | <span data-ttu-id="2d7fe-639">valueB</span><span class="sxs-lookup"><span data-stu-id="2d7fe-639">valueB</span></span>                              |
| <span data-ttu-id="2d7fe-640">1</span><span class="sxs-lookup"><span data-stu-id="2d7fe-640">1</span></span>                                   | <span data-ttu-id="2d7fe-641">valueC</span><span class="sxs-lookup"><span data-stu-id="2d7fe-641">valueC</span></span>                              |
| <span data-ttu-id="2d7fe-642">2</span><span class="sxs-lookup"><span data-stu-id="2d7fe-642">2</span></span>                                   | <span data-ttu-id="2d7fe-643">valueD</span><span class="sxs-lookup"><span data-stu-id="2d7fe-643">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="2d7fe-644">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="2d7fe-644">Custom configuration provider</span></span>

<span data-ttu-id="2d7fe-645">範例應用程式示範如何建立會使用 [Entity Framework (EF)](/ef/core/) 從資料庫讀取設定機碼值組的基本設定提供者。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-645">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="2d7fe-646">提供者具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-646">The provider has the following characteristics:</span></span>

* <span data-ttu-id="2d7fe-647">EF 記憶體內資料庫會用於示範用途。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-647">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="2d7fe-648">若要使用需要連接字串的資料庫，請實作第二個 `ConfigurationBuilder` 以從另一個設定提供者提供連接字串。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-648">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="2d7fe-649">啟動時，該提供者會將資料庫資料表讀入到設定中。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-649">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="2d7fe-650">該提供者不會以個別機碼為基礎查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-650">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="2d7fe-651">未實作變更時重新載入，因此在應用程式啟動後更新資料庫對應用程式設定沒有影響。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-651">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="2d7fe-652">定義 `EFConfigurationValue` 實體來在資料庫中存放設定值。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-652">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2d7fe-653">*Models/EFConfigurationValue.cs*：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-653">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="2d7fe-654">新增 `EFConfigurationContext` 以存放及存取已設定的值。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-654">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="2d7fe-655">*EFConfigurationProvider/EFConfigurationContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-655">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="2d7fe-656">建立會實作 <xref:Microsoft.Extensions.Configuration.IConfigurationSource> 的類別。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-656">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="2d7fe-657">*EFConfigurationProvider/EFConfigurationSource.cs*：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-657">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="2d7fe-658">透過繼承自 <xref:Microsoft.Extensions.Configuration.ConfigurationProvider> 來建立自訂設定提供者。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-658">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="2d7fe-659">若資料庫是空的，設定提供者會初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-659">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="2d7fe-660">由於設定索引[鍵不區分大小寫](#keys)，用來初始化資料庫的字典會以不區分大小寫的比較子（[StringComparer. OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)）來建立。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-660">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="2d7fe-661">*EFConfigurationProvider/EFConfigurationProvider.cs*：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-661">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="2d7fe-662">`AddEFConfiguration` 擴充方法允許新增設定來源到 `ConfigurationBuilder`。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-662">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="2d7fe-663">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="2d7fe-663">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="2d7fe-664">下列程式碼顯示如何在 *Program.cs* 中使用自訂 `EFConfigurationProvider`：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-664">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2d7fe-665">*Models/EFConfigurationValue.cs*：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-665">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="2d7fe-666">新增 `EFConfigurationContext` 以存放及存取已設定的值。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-666">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="2d7fe-667">*EFConfigurationProvider/EFConfigurationContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-667">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="2d7fe-668">建立會實作 <xref:Microsoft.Extensions.Configuration.IConfigurationSource> 的類別。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-668">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="2d7fe-669">*EFConfigurationProvider/EFConfigurationSource.cs*：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-669">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="2d7fe-670">透過繼承自 <xref:Microsoft.Extensions.Configuration.ConfigurationProvider> 來建立自訂設定提供者。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-670">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="2d7fe-671">若資料庫是空的，設定提供者會初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-671">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="2d7fe-672">*EFConfigurationProvider/EFConfigurationProvider.cs*：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-672">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="2d7fe-673">`AddEFConfiguration` 擴充方法允許新增設定來源到 `ConfigurationBuilder`。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-673">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="2d7fe-674">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="2d7fe-674">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="2d7fe-675">下列程式碼顯示如何在 *Program.cs* 中使用自訂 `EFConfigurationProvider`：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-675">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="2d7fe-676">在啟動期間存取組態</span><span class="sxs-lookup"><span data-stu-id="2d7fe-676">Access configuration during startup</span></span>

<span data-ttu-id="2d7fe-677">將 `IConfiguration` 插入到 `Startup` 建構函式，以存取 `Startup.ConfigureServices` 中的設定值。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-677">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="2d7fe-678">若要存取 `Startup.Configure` 中的設定，請直接將 `IConfiguration` 插入到方法或從建構函式使用執行個體：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-678">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="2d7fe-679">如需使用啟動方便方法來存取設定的範例，請參閱[應用程式啟動：方便方法](xref:fundamentals/startup#convenience-methods)。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-679">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="2d7fe-680">存取 Razor Pages 頁面或 MVC 檢視中的設定</span><span class="sxs-lookup"><span data-stu-id="2d7fe-680">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="2d7fe-681">若要在 [Razor Pages 頁面] 頁面或 MVC 檢視中存取組態設定，請針對 [Microsoft.Extensions.Configuration 命名空間](xref:Microsoft.Extensions.Configuration)[using 指示詞](xref:mvc/views/razor#using) ([C# 參考：using 指示詞](/dotnet/csharp/language-reference/keywords/using-directive))，並將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 插入到頁面或檢視中。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-681">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="2d7fe-682">在 [Razor 頁面] 頁面中：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-682">In a Razor Pages page:</span></span>

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

<span data-ttu-id="2d7fe-683">在 MVC 檢視中：</span><span class="sxs-lookup"><span data-stu-id="2d7fe-683">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="2d7fe-684">從外部組件新增設定</span><span class="sxs-lookup"><span data-stu-id="2d7fe-684">Add configuration from an external assembly</span></span>

<span data-ttu-id="2d7fe-685"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-685">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="2d7fe-686">如需詳細資訊，請參閱<xref:fundamentals/configuration/platform-specific-configuration>。</span><span class="sxs-lookup"><span data-stu-id="2d7fe-686">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2d7fe-687">其他資源</span><span class="sxs-lookup"><span data-stu-id="2d7fe-687">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
