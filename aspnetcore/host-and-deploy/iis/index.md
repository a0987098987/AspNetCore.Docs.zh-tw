---
title: "在使用 IIS 的 Windows 上裝載 ASP.NET Core"
author: guardrex
description: "了解如何在 Windows Server Internet Information Services (IIS) 上裝載 ASP.NET Core 應用程式。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/index
ms.openlocfilehash: fa9e60c52f143b20dbf179679fc4932e838a9137
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="dedea-103">在使用 IIS 的 Windows 上裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dedea-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="dedea-104">作者：[Luke Latham](https://github.com/guardrex) 及 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dedea-104">By [Luke Latham](https://github.com/guardrex) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="supported-operating-systems"></a><span data-ttu-id="dedea-105">支援的作業系統</span><span class="sxs-lookup"><span data-stu-id="dedea-105">Supported operating systems</span></span>

<span data-ttu-id="dedea-106">支援下列作業系統：</span><span class="sxs-lookup"><span data-stu-id="dedea-106">The following operating systems are supported:</span></span>

* <span data-ttu-id="dedea-107">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="dedea-107">Windows 7 or later</span></span>
* <span data-ttu-id="dedea-108">Windows Server 2008 R2 或更新版本&#8224;</span><span class="sxs-lookup"><span data-stu-id="dedea-108">Windows Server 2008 R2 or later&#8224;</span></span>

<span data-ttu-id="dedea-109">&#8224;就概念而言，本文件中描述的 IIS 設定也適用於在 Nano 伺服器 IIS 上裝載 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="dedea-109">&#8224;Conceptually, the IIS configuration described in this document also applies to hosting ASP.NET Core apps on Nano Server IIS.</span></span> <span data-ttu-id="dedea-110">如需 Nano 伺服器的特定指示，請參閱 [Nano 伺服器上的 ASP.NET Core 與 IIS](xref:tutorials/nano-server) 教學課程。</span><span class="sxs-lookup"><span data-stu-id="dedea-110">For instructions specific to Nano Server, see the [ASP.NET Core with IIS on Nano Server](xref:tutorials/nano-server) tutorial.</span></span>

<span data-ttu-id="dedea-111">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (先前稱為 [WebListener](xref:fundamentals/servers/weblistener)) 不適用搭配 IIS 的反向 Proxy 設定。</span><span class="sxs-lookup"><span data-stu-id="dedea-111">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="dedea-112">請使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="dedea-112">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

## <a name="application-configuration"></a><span data-ttu-id="dedea-113">應用程式組態</span><span class="sxs-lookup"><span data-stu-id="dedea-113">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="dedea-114">啟用 IISIntegration 元件</span><span class="sxs-lookup"><span data-stu-id="dedea-114">Enable the IISIntegration components</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dedea-115">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dedea-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="dedea-116">一般 *Program.cs* 會呼叫 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 以開始設定主機。</span><span class="sxs-lookup"><span data-stu-id="dedea-116">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to begin setting up a host.</span></span> <span data-ttu-id="dedea-117">`CreateDefaultBuilder` 會將 [Kestrel](xref:fundamentals/servers/kestrel) 設為網頁伺服器，並設定 [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) 的基底路徑與連接埠來啟用 IIS 整合：</span><span class="sxs-lookup"><span data-stu-id="dedea-117">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

<span data-ttu-id="dedea-118">ASP.NET Core 模組會產生要指派給後端處理序的動態連接埠。</span><span class="sxs-lookup"><span data-stu-id="dedea-118">The ASP.NET Core Module generates a dynamic port to assign to the back-end process.</span></span> <span data-ttu-id="dedea-119">`UseIISIntegration` 方法採用此動態連接埠，並設定 Kestrel 來接聽 `http://locahost:{dynamicPort}/`。</span><span class="sxs-lookup"><span data-stu-id="dedea-119">The `UseIISIntegration` method picks up the dynamic port and configures Kestrel to listen on `http://locahost:{dynamicPort}/`.</span></span> <span data-ttu-id="dedea-120">這會覆寫其他 URL 設定，例如呼叫 `UseUrls` 或 [Kestrel 的 Listen API](xref:fundamentals/servers/kestrel#endpoint-configuration)。</span><span class="sxs-lookup"><span data-stu-id="dedea-120">This overrides other URL configurations, such as calls to `UseUrls` or [Kestrel's Listen API](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span> <span data-ttu-id="dedea-121">因此在使用模組時無須呼叫 `UseUrls` 或 Kestrel 的 `Listen` API。</span><span class="sxs-lookup"><span data-stu-id="dedea-121">Therefore, calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="dedea-122">若呼叫 `UseUrls` 或 `Listen`，Kestrel 就會接聽未使用 IIS 執行應用程式時指定的連接埠。</span><span class="sxs-lookup"><span data-stu-id="dedea-122">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified when running the app without IIS.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dedea-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dedea-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="dedea-124">在應用程式相依性的 [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) 套件中包含相依性。</span><span class="sxs-lookup"><span data-stu-id="dedea-124">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="dedea-125">將 [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) 擴充方法新增至 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)，以使用 IIS 整合中介軟體：</span><span class="sxs-lookup"><span data-stu-id="dedea-125">Use IIS Integration middleware by adding the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) extension method to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder):</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="dedea-126">同時需要 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) 和 [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration)。</span><span class="sxs-lookup"><span data-stu-id="dedea-126">Both [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) and [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) are required.</span></span> <span data-ttu-id="dedea-127">呼叫 `UseIISIntegration` 的程式碼並不會影響程式碼的可攜性。</span><span class="sxs-lookup"><span data-stu-id="dedea-127">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="dedea-128">若應用程式不在 IIS 背後執行 (例如，應用程式直接在 Kestrel 上執行)，`UseIISIntegration` 就不會運作。</span><span class="sxs-lookup"><span data-stu-id="dedea-128">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` doesn't operate.</span></span>

<span data-ttu-id="dedea-129">ASP.NET Core 模組會產生要指派給後端處理序的動態連接埠。</span><span class="sxs-lookup"><span data-stu-id="dedea-129">The ASP.NET Core Module generates a dynamic port to assign to the back-end process.</span></span> <span data-ttu-id="dedea-130">`UseIISIntegration` 方法採用此動態連接埠，並設定 Kestrel 來接聽 `http://locahost:{dynamicPort}/`。</span><span class="sxs-lookup"><span data-stu-id="dedea-130">The `UseIISIntegration` method picks up the dynamic port and configures Kestrel to listen on `http://locahost:{dynamicPort}/`.</span></span> <span data-ttu-id="dedea-131">這會覆寫其他 URL 設定，例如呼叫 `UseUrls`。</span><span class="sxs-lookup"><span data-stu-id="dedea-131">This overrides other URL configurations, such as calls to `UseUrls`.</span></span> <span data-ttu-id="dedea-132">因此，使用模組時不需要 `UseUrls`。</span><span class="sxs-lookup"><span data-stu-id="dedea-132">Therefore, a call to `UseUrls` isn't required when using the module.</span></span> <span data-ttu-id="dedea-133">若呼叫 `UseUrls`，Kestrel 就會接聽未使用 IIS 執行應用程式時指定的連接埠。</span><span class="sxs-lookup"><span data-stu-id="dedea-133">If `UseUrls` is called, Kestrel listens on the port specified when running the app without IIS.</span></span>

<span data-ttu-id="dedea-134">若要在 ASP.NET Core 1.0 應用程式上呼叫 `UseUrls`，請在呼叫 `UseIISIntegration`**前**予以呼叫，以避免模組設定的連接埠受到覆寫。</span><span class="sxs-lookup"><span data-stu-id="dedea-134">If `UseUrls` is called in an ASP.NET Core 1.0 app, call it **before** calling `UseIISIntegration` so that the module-configured port isn't overwritten.</span></span> <span data-ttu-id="dedea-135">在 ASP.NET Core 1.1 中，因為模組設定會覆寫 `UseUrls`，所以不需要呼叫順序。</span><span class="sxs-lookup"><span data-stu-id="dedea-135">This calling order isn't required with ASP.NET Core 1.1 because the module setting overrides `UseUrls`.</span></span>

---

<span data-ttu-id="dedea-136">如需裝載的詳細資訊，請參閱[在 ASP.NET Core 中裝載](xref:fundamentals/hosting)。</span><span class="sxs-lookup"><span data-stu-id="dedea-136">For more information on hosting, see [Hosting in ASP.NET Core](xref:fundamentals/hosting).</span></span>

### <a name="iis-options"></a><span data-ttu-id="dedea-137">IIS 選項</span><span class="sxs-lookup"><span data-stu-id="dedea-137">IIS options</span></span>

<span data-ttu-id="dedea-138">若要設定 IIS 選項，請在 [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices) 中包括 [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="dedea-138">To configure IIS options, include a service configuration for [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span></span> <span data-ttu-id="dedea-139">在下列範例中，已停用將用戶端憑證轉送至應用程式以填入 `HttpContext.Connection.ClientCertificate` 的功能：</span><span class="sxs-lookup"><span data-stu-id="dedea-139">In the following example, forwarding client certificates to the app to populate `HttpContext.Connection.ClientCertificate` is disabled:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="dedea-140">選項</span><span class="sxs-lookup"><span data-stu-id="dedea-140">Option</span></span>                         | <span data-ttu-id="dedea-141">預設</span><span class="sxs-lookup"><span data-stu-id="dedea-141">Default</span></span> | <span data-ttu-id="dedea-142">設定</span><span class="sxs-lookup"><span data-stu-id="dedea-142">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="dedea-143">若為 `true`，IIS 整合中介軟體會設定由 [Windows Authentication](xref:security/authentication/windowsauth) 所驗證的 `HttpContext.User`。</span><span class="sxs-lookup"><span data-stu-id="dedea-143">If `true`, IIS Integration Middleware sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="dedea-144">如果為 `false`，則驗證中介軟體僅針對 `HttpContext.User` 提供身分識別，並在游 `AuthenticationScheme` 提出明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="dedea-144">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="dedea-145">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="dedea-145">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="dedea-146">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="dedea-146">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="dedea-147">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="dedea-147">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="dedea-148">如果為 `true` 且 `MS-ASPNETCORE-CLIENTCERT` 要求標頭已存在，則會填入 `HttpContext.Connection.ClientCertificate`。</span><span class="sxs-lookup"><span data-stu-id="dedea-148">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="webconfig-file"></a><span data-ttu-id="dedea-149">web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="dedea-149">web.config file</span></span>

<span data-ttu-id="dedea-150">*web.config* 檔案是用來設定 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="dedea-150">The *web.config* file configures the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="dedea-151">建立、轉換及發行 *web.config* 是由 .NET Core Web SDK (`Microsoft.NET.Sdk.Web`) 處理。</span><span class="sxs-lookup"><span data-stu-id="dedea-151">Creating, transforming, and publishing *web.config* is handled by the .NET Core Web SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="dedea-152">SDK 設定在專案檔的頂端：</span><span class="sxs-lookup"><span data-stu-id="dedea-152">The SDK is set  at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="dedea-153">如果 *web.config* 檔案不存在於專案中，則系統會使用正確的 *processPath* 和 *arguments* 來建立該檔案以設定 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)，然後將它移至[已發行的輸出](xref:host-and-deploy/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="dedea-153">If a *web.config* file isn't present the project, the file is created with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="dedea-154">如果 *web.config* 檔案存在於專案中，則系統會使用正確的 *processPath* 和 *arguments* 來轉換該檔案以設定 ASP.NET Core 模組，然後將它移至已發行的輸出。</span><span class="sxs-lookup"><span data-stu-id="dedea-154">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="dedea-155">轉換不會修改檔案中的 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="dedea-155">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="dedea-156">*web.config* 檔案可提供能控制作用中 IIS 模組的額外 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="dedea-156">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="dedea-157">如需能處理 ASP.NET Core 應用程式要求之 IIS 模組的相關資訊，請參閱[使用 IIS 模組](xref:host-and-deploy/iis/modules)主題。</span><span class="sxs-lookup"><span data-stu-id="dedea-157">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [Using IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="dedea-158">為防止 Web SDK 轉換 *web.config* 檔案，請使用專案檔中的 **\<IsTransformWebConfigDisabled>** 屬性：</span><span class="sxs-lookup"><span data-stu-id="dedea-158">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="dedea-159">使 Web SDK 無法轉換檔案時，應該由開發人員手動設定 *processPath* 和 *arguments*。</span><span class="sxs-lookup"><span data-stu-id="dedea-159">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="dedea-160">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="dedea-160">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="dedea-161">web.config 檔案位置</span><span class="sxs-lookup"><span data-stu-id="dedea-161">web.config file location</span></span>

<span data-ttu-id="dedea-162">ASP.NET Core 應用程式是裝載於 IIS 和 Kestrel 伺服器之間的反向 Proxy 中。</span><span class="sxs-lookup"><span data-stu-id="dedea-162">ASP.NET Core apps are hosted in a reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="dedea-163">為了建立反向 Proxy，*web.config* 檔案必須存在於已部署應用程式的內容根路徑 (通常是應用程式基底路徑)。</span><span class="sxs-lookup"><span data-stu-id="dedea-163">In order to create the reverse proxy, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="dedea-164">這是與提供給 IIS 的網站實體路徑相同的位置。</span><span class="sxs-lookup"><span data-stu-id="dedea-164">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="dedea-165">應用程式的根目錄需有 *web.config* 檔案，才能使用 Web Deploy 發行多個應用程式。</span><span class="sxs-lookup"><span data-stu-id="dedea-165">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="dedea-166">機密檔案存在於應用程式的實體路徑上，例如 *\<組件>.runtimeconfig.json*、*\<組件>.xml* (XML 文件註解)，以及 *\<組件>.deps.json*。</span><span class="sxs-lookup"><span data-stu-id="dedea-166">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="dedea-167">當 *web.config* 檔案存在且網站正常啟動時，在有人要求機密檔案的情況下，IIS 並不會提供它們。</span><span class="sxs-lookup"><span data-stu-id="dedea-167">When the *web.config* file is present and and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="dedea-168">若 *web.config* 檔案遺失或沒有正確命名，或是無法設定網站以正常啟動，IIS 可能會公開提供機密檔案。</span><span class="sxs-lookup"><span data-stu-id="dedea-168">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="dedea-169">***web.config* 檔案必須持續存在於部署之中、已正確命名，並能夠設定網站以正常啟動。無論在任何情況下，請都不要從生產環境部署移除 *web.config* 檔案。**</span><span class="sxs-lookup"><span data-stu-id="dedea-169">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="dedea-170">IIS 組態</span><span class="sxs-lookup"><span data-stu-id="dedea-170">IIS configuration</span></span>

<span data-ttu-id="dedea-171">**Windows Server 作業系統**</span><span class="sxs-lookup"><span data-stu-id="dedea-171">**Windows Server operating systems**</span></span>

<span data-ttu-id="dedea-172">啟用**網頁伺服器 (IIS)** 伺服器角色，並建立角色服務。</span><span class="sxs-lookup"><span data-stu-id="dedea-172">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="dedea-173">使用來自 [管理] 功能表的 [新增角色及功能] 精靈，或是 [伺服器管理員] 中的連結。</span><span class="sxs-lookup"><span data-stu-id="dedea-173">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="dedea-174">在**伺服器角色**步驟中，核取 [網頁伺服器 (IIS)] 方塊。</span><span class="sxs-lookup"><span data-stu-id="dedea-174">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![在選取伺服器角色步驟中選取網頁伺服器 IIS 角色。](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="dedea-176">在 [功能] 步驟之後，[角色服務] 步驟會針對網頁伺服器 (IIS) 進行載入。</span><span class="sxs-lookup"><span data-stu-id="dedea-176">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="dedea-177">選取所需的 IIS 角色服務或接受所提供的預設角色服務。</span><span class="sxs-lookup"><span data-stu-id="dedea-177">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![在選取角色服務步驟中，選取預設的角色服務。](index/_static/role-services-ws2016.png)

   <span data-ttu-id="dedea-179">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="dedea-179">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="dedea-180">若要啟用 Windows 驗證，請展開下列節點：[網頁伺服器] > [安全性]。</span><span class="sxs-lookup"><span data-stu-id="dedea-180">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="dedea-181">選取 [Windows 驗證] 功能。</span><span class="sxs-lookup"><span data-stu-id="dedea-181">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="dedea-182">如需詳細資訊，請參閱 [Windows 驗證 \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) 和[設定 Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="dedea-182">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="dedea-183">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="dedea-183">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="dedea-184">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="dedea-184">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="dedea-185">若要啟用 WebSocket，請展開下列節點：[網頁伺服器] > [應用程式開發]。</span><span class="sxs-lookup"><span data-stu-id="dedea-185">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="dedea-186">選取 [WebSocket 通訊協定] 功能。</span><span class="sxs-lookup"><span data-stu-id="dedea-186">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="dedea-187">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="dedea-187">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="dedea-188">透過**確認**步驟繼續作業，安裝網頁伺服器角色和服務。</span><span class="sxs-lookup"><span data-stu-id="dedea-188">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="dedea-189">安裝**網頁伺服器 (IIS)** 角色之後，不需要重新啟動伺服器/IIS。</span><span class="sxs-lookup"><span data-stu-id="dedea-189">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="dedea-190">**Windows 桌面作業系統**</span><span class="sxs-lookup"><span data-stu-id="dedea-190">**Windows desktop operating systems**</span></span>

<span data-ttu-id="dedea-191">啟用 [IIS 管理主控台] 和 [World Wide Web 服務]。</span><span class="sxs-lookup"><span data-stu-id="dedea-191">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="dedea-192">瀏覽至控制台 > [程式] > [程式和功能] > [開啟或關閉 Windows 功能] (畫面左側)。</span><span class="sxs-lookup"><span data-stu-id="dedea-192">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="dedea-193">開啟 [Internet Information Services] 節點。</span><span class="sxs-lookup"><span data-stu-id="dedea-193">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="dedea-194">開啟 [Web 管理工具] 節點。</span><span class="sxs-lookup"><span data-stu-id="dedea-194">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="dedea-195">核取 [IIS 管理主控台] 方塊。</span><span class="sxs-lookup"><span data-stu-id="dedea-195">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="dedea-196">[World Wide Web Services] (全球資訊網服務) 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="dedea-196">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="dedea-197">接受**全球資訊網服務**的預設功能，或自訂 IIS 功能。</span><span class="sxs-lookup"><span data-stu-id="dedea-197">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="dedea-198">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="dedea-198">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="dedea-199">若要啟用 Windows 驗證，請展開下列節點：[World Wide Web 服務] > [安全性]。</span><span class="sxs-lookup"><span data-stu-id="dedea-199">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="dedea-200">選取 [Windows 驗證] 功能。</span><span class="sxs-lookup"><span data-stu-id="dedea-200">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="dedea-201">如需詳細資訊，請參閱 [Windows 驗證 \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) 和[設定 Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="dedea-201">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="dedea-202">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="dedea-202">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="dedea-203">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="dedea-203">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="dedea-204">若要啟用 WebSocket，請展開下列節點：[World Wide Web 服務] > [應用程式開發功能]。</span><span class="sxs-lookup"><span data-stu-id="dedea-204">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="dedea-205">選取 [WebSocket 通訊協定] 功能。</span><span class="sxs-lookup"><span data-stu-id="dedea-205">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="dedea-206">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="dedea-206">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="dedea-207">若 IIS 安裝需要重新啟動，請重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="dedea-207">If the IIS installation requires a restart, restart the system.</span></span>

![選取 [Windows 功能] 中的 [IIS 管理主控台] 和 [World Wide Web Services] (全球資訊網服務)。](index/_static/windows-features-win10.png)

---

## <a name="install-the-net-core-windows-server-hosting-bundle"></a><span data-ttu-id="dedea-209">安裝 .NET Core Windows Server 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="dedea-209">Install the .NET Core Windows Server Hosting bundle</span></span>

1. <span data-ttu-id="dedea-210">在主控系統上安裝 [.NET Core Windows Server 裝載套件組合](https://aka.ms/dotnetcore-2-windowshosting)。</span><span class="sxs-lookup"><span data-stu-id="dedea-210">Install the [.NET Core Windows Server Hosting bundle](https://aka.ms/dotnetcore-2-windowshosting) on the hosting system.</span></span> <span data-ttu-id="dedea-211">套件組合會安裝 .NET Core 執行階段、.NET Core 程式庫和 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="dedea-211">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="dedea-212">此模組會在 IIS 和 Kestrel 伺服器之間建立反向 Proxy。</span><span class="sxs-lookup"><span data-stu-id="dedea-212">The module creates the reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="dedea-213">如果系統沒有網際網路連線，請先取得並安裝 [Microsoft Visual C++ 2015 可轉散發套件](https://www.microsoft.com/download/details.aspx?id=53840)，再安裝 .NET Core Windows Server 裝載套件組合。</span><span class="sxs-lookup"><span data-stu-id="dedea-213">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Windows Server Hosting bundle.</span></span>

   <span data-ttu-id="dedea-214">**重要！**</span><span class="sxs-lookup"><span data-stu-id="dedea-214">**Important!**</span></span> <span data-ttu-id="dedea-215">若裝載套件組合是在 IIS 之前安裝，則必須對該套件組合安裝進行修復。</span><span class="sxs-lookup"><span data-stu-id="dedea-215">If the hosting bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="dedea-216">請在安裝 IIS 之後再次執行裝載套件組合安裝程式。</span><span class="sxs-lookup"><span data-stu-id="dedea-216">Run the hosting bundle installer again after installing IIS.</span></span>
   
   <span data-ttu-id="dedea-217">若要避免安裝程式在 x64 OS 上安裝 x86 套件，請以 `OPT_NO_X86=1` 參數從系統管理員命令提示字元執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="dedea-217">To prevent the installer from installing x86 packages on an x64 OS, run the installer from an administrator command prompt with the switch `OPT_NO_X86=1`.</span></span>

1. <span data-ttu-id="dedea-218">請重新啟動系統，或是從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**。</span><span class="sxs-lookup"><span data-stu-id="dedea-218">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span> <span data-ttu-id="dedea-219">重新啟動 IIS 將能偵測到由安裝程式對系統路徑所做出的變更。</span><span class="sxs-lookup"><span data-stu-id="dedea-219">Restarting IIS picks up a change to the system PATH made by the installer.</span></span>

> [!NOTE]
> <span data-ttu-id="dedea-220">如需 IIS 共用組態的資訊，請參閱[使用 IIS 共用組態的 ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)。</span><span class="sxs-lookup"><span data-stu-id="dedea-220">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="dedea-221">使用 Visual Studio 發佈時安裝 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="dedea-221">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="dedea-222">將應用程式部署到具有 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 的伺服器時，請在伺服器上安裝最新版的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="dedea-222">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="dedea-223">若要安裝 Web Deploy，請使用 [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) 或從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=43717)直接取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="dedea-223">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="dedea-224">慣用的方法是使用 WebPI。</span><span class="sxs-lookup"><span data-stu-id="dedea-224">The preferred method is to use WebPI.</span></span> <span data-ttu-id="dedea-225">WebPI 提供獨立的安裝程式和組態以裝載提供者。</span><span class="sxs-lookup"><span data-stu-id="dedea-225">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="dedea-226">建立 IIS 網站</span><span class="sxs-lookup"><span data-stu-id="dedea-226">Create the IIS site</span></span>

1. <span data-ttu-id="dedea-227">在主控系統上，請建立資料夾以容納應用程式的已發行資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="dedea-227">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="dedea-228">應用程式的部署配置已詳述於[目錄結構](xref:host-and-deploy/directory-structure)主題。</span><span class="sxs-lookup"><span data-stu-id="dedea-228">An app's deployment layout is described in the [Directory Structure](xref:host-and-deploy/directory-structure) topic.</span></span>

1. <span data-ttu-id="dedea-229">在新的資料夾內，建立 *logs* 資料夾，以在啟用 stdout 記錄時保留 ASP.NET Core 模組 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="dedea-229">Within the new folder, create a *logs* folder to hold ASP.NET Core Module stdout logs when stdout logging is enabled.</span></span> <span data-ttu-id="dedea-230">如果應用程式已在承載中部署 *logs* 資料夾，請略過此步驟。</span><span class="sxs-lookup"><span data-stu-id="dedea-230">If the app is deployed with a *logs* folder in the payload, skip this step.</span></span> <span data-ttu-id="dedea-231">如需在於本機建置專案的情況下使 MSBuild 自動建立 *logs* 資料夾的相關指示，請參閱[目錄結構](xref:host-and-deploy/directory-structure)主題。</span><span class="sxs-lookup"><span data-stu-id="dedea-231">For instructions on how to enable MSBuild to create the *logs* folder automatically when the project is built locally, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>

   <span data-ttu-id="dedea-232">**重要！**</span><span class="sxs-lookup"><span data-stu-id="dedea-232">**Important!**</span></span> <span data-ttu-id="dedea-233">請只使用 stdout 記錄來對應用程式啟動失敗進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="dedea-233">Only use the stdout log to troubleshoot app startup failures.</span></span> <span data-ttu-id="dedea-234">請永遠不要將 stdout 記錄用於例行的應用程式記錄。</span><span class="sxs-lookup"><span data-stu-id="dedea-234">Never use stdout logging for routine app logging.</span></span> <span data-ttu-id="dedea-235">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="dedea-235">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="dedea-236">如需 stdout 記錄的詳細資訊，請參閱[記錄建立和重新導向](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)。</span><span class="sxs-lookup"><span data-stu-id="dedea-236">For more information on the stdout log, see [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span> <span data-ttu-id="dedea-237">如需在 ASP.NET Core 應用程式中進行記錄的相關資訊，請參閱[記錄](xref:fundamentals/logging/index)主題。</span><span class="sxs-lookup"><span data-stu-id="dedea-237">For information on logging in an ASP.NET Core app, see the [Logging](xref:fundamentals/logging/index) topic.</span></span>

1. <span data-ttu-id="dedea-238">在 [IIS 管理員] 中，於 [連線] 面板中開啟伺服器的節點。</span><span class="sxs-lookup"><span data-stu-id="dedea-238">In **IIS Manager**, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="dedea-239">以滑鼠右鍵按一下 [網站] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="dedea-239">Right-click the **Sites** folder.</span></span> <span data-ttu-id="dedea-240">從操作功能表選取 [新增網站]。</span><span class="sxs-lookup"><span data-stu-id="dedea-240">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="dedea-241">提供**網站名稱**，並將**實體路徑**設定為應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="dedea-241">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="dedea-242">透過選取 [確定] 來提供**繫結**設定並建立網站：</span><span class="sxs-lookup"><span data-stu-id="dedea-242">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![在新增網站步驟中提供站台名稱、實體路徑和主機名稱。](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="dedea-244">請**勿**使用最上層萬用字元繫結 (`http://*:80/`與 `http://+:80`)。</span><span class="sxs-lookup"><span data-stu-id="dedea-244">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="dedea-245">最上層萬用字元繫結可能暴露您的應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="dedea-245">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="dedea-246">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="dedea-246">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="dedea-247">請使用明確主機名稱，而非萬用字元。</span><span class="sxs-lookup"><span data-stu-id="dedea-247">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="dedea-248">若您擁有整個父網域 (與具弱點的 `*.com` 相對) 的控制權，則子網域萬用字元繫結 (例如 `*.mysub.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="dedea-248">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="dedea-249">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="dedea-249">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="dedea-250">在伺服器的節點之下，選取 [應用程式集區]。</span><span class="sxs-lookup"><span data-stu-id="dedea-250">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="dedea-251">以滑鼠右鍵按一下網站的應用程式集區，然後從操作功能表選取 [基本設定]。</span><span class="sxs-lookup"><span data-stu-id="dedea-251">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="dedea-252">在 [編輯應用程式集區] 視窗中，將 [.NET CLR 版本] 設定為 [沒有受控碼]：</span><span class="sxs-lookup"><span data-stu-id="dedea-252">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![將 .NET CLR 版本設為 [沒有受控碼]。](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="dedea-254">ASP.NET Core 會在不同的處理序中執行，並管理執行階段。</span><span class="sxs-lookup"><span data-stu-id="dedea-254">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="dedea-255">ASP.NET Core 不依賴載入桌面 CLR。</span><span class="sxs-lookup"><span data-stu-id="dedea-255">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span> <span data-ttu-id="dedea-256">將 [.NET CLR 版本] 設為 [沒有受控碼] 是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="dedea-256">Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span>

1. <span data-ttu-id="dedea-257">確認處理序模型身分識別具有適當的權限。</span><span class="sxs-lookup"><span data-stu-id="dedea-257">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="dedea-258">如果您將應用程式集區的預設身分識別 ([處理序模型] > [身分識別]) 從 **ApplicationPoolIdentity** 變更為其他身分識別，請確認新的身分識別具有必要權限，可存取應用程式的資料夾、資料庫和其他必要的資源。</span><span class="sxs-lookup"><span data-stu-id="dedea-258">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="dedea-259">例如，應用程式集區需要針對應用程式讀取和寫入檔案的資料夾取得讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="dedea-259">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="dedea-260">**Windows 驗證設定 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="dedea-260">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="dedea-261">如需詳細資訊，請參閱[設定 Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="dedea-261">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="dedea-262">部署應用程式</span><span class="sxs-lookup"><span data-stu-id="dedea-262">Deploy the app</span></span>

<span data-ttu-id="dedea-263">將應用程式部署至在主控系統上建立的資料夾。</span><span class="sxs-lookup"><span data-stu-id="dedea-263">Deploy the app to the folder created on the hosting system.</span></span> <span data-ttu-id="dedea-264">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 是建議的部署機制。</span><span class="sxs-lookup"><span data-stu-id="dedea-264">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="dedea-265">使用 Visual Studio 的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="dedea-265">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="dedea-266">請參閱[適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)主題，了解如何建立搭配 Web Deploy 使用的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="dedea-266">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="dedea-267">如果裝載提供者提供或支援建立發行設定檔，請下載其設定檔，並使用 Visual Studio 的 [發行] 對話方塊匯入其設定檔。</span><span class="sxs-lookup"><span data-stu-id="dedea-267">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![[發佈] 對話方塊頁](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="dedea-269">Visual Studio 外部的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="dedea-269">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="dedea-270">透過命令列也可以在 Visual Studio 外部使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy)。</span><span class="sxs-lookup"><span data-stu-id="dedea-270">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="dedea-271">如需詳細資訊，請參閱 [Web 部署工具](/iis/publish/using-web-deploy/use-the-web-deployment-tool)。</span><span class="sxs-lookup"><span data-stu-id="dedea-271">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="dedea-272">Web Deploy 的替代項目</span><span class="sxs-lookup"><span data-stu-id="dedea-272">Alternatives to Web Deploy</span></span>

<span data-ttu-id="dedea-273">使用數種方法的其中一種將應用程式移到主控系統，例如手動複製、Xcopy、Robocopy 或 PowerShell。</span><span class="sxs-lookup"><span data-stu-id="dedea-273">Use any of several methods to move the app to the hosting system, such as manual copy, Xcopy, Robocopy, or PowerShell.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="dedea-274">瀏覽網站</span><span class="sxs-lookup"><span data-stu-id="dedea-274">Browse the website</span></span>

![Microsoft Edge 瀏覽器已載入 IIS 啟動頁面。](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="dedea-276">已鎖定的部署檔案</span><span class="sxs-lookup"><span data-stu-id="dedea-276">Locked deployment files</span></span>

<span data-ttu-id="dedea-277">當應用程式執行時，會鎖定部署資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="dedea-277">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="dedea-278">無法於部署期間覆寫已鎖定的檔案。</span><span class="sxs-lookup"><span data-stu-id="dedea-278">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="dedea-279">若要釋放部署中的已鎖定檔案，請使用下列其中**一種**方法停止應用程式集區：</span><span class="sxs-lookup"><span data-stu-id="dedea-279">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="dedea-280">使用 Web Deploy 並參考專案檔中的 `Microsoft.NET.Sdk.Web`。</span><span class="sxs-lookup"><span data-stu-id="dedea-280">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="dedea-281">*app_offline.htm* 檔案是放在 Web 應用程式目錄的根目錄中。</span><span class="sxs-lookup"><span data-stu-id="dedea-281">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="dedea-282">當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。</span><span class="sxs-lookup"><span data-stu-id="dedea-282">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="dedea-283">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#appofflinehtm)。</span><span class="sxs-lookup"><span data-stu-id="dedea-283">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>
* <span data-ttu-id="dedea-284">在伺服器上的 IIS 管理員中手動停止應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="dedea-284">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="dedea-285">使用 PowerShell 停止和重新啟動應用程式集區 (需要 PowerShell 5 或更新版本)：</span><span class="sxs-lookup"><span data-stu-id="dedea-285">Use PowerShell to stop and restart the app pool (requires PowerShell 5 or later):</span></span>

  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

## <a name="data-protection"></a><span data-ttu-id="dedea-286">資料保護</span><span class="sxs-lookup"><span data-stu-id="dedea-286">Data protection</span></span>

<span data-ttu-id="dedea-287">[ASP.NET Core 資料保護堆疊](xref:security/data-protection/index)是由數個 ASP.NET Core [中介軟體](xref:fundamentals/middleware/index)所使用，包括用於驗證的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="dedea-287">The [ASP.NET Core Data Protection stack](xref:security/data-protection/index) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="dedea-288">即使資料保護 API 不是由使用者程式碼呼叫，仍應使用部署指令碼或是在使用者程式碼中設定資料保護，以建立持續性的密碼編譯[金鑰存放區](xref:security/data-protection/implementation/key-management)。</span><span class="sxs-lookup"><span data-stu-id="dedea-288">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="dedea-289">如不設定資料保護，金鑰會保留在記憶體中，並於應用程式重新啟動時捨棄。</span><span class="sxs-lookup"><span data-stu-id="dedea-289">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="dedea-290">如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：</span><span class="sxs-lookup"><span data-stu-id="dedea-290">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="dedea-291">所有以 Cookie 為基礎的驗證權杖都會失效。</span><span class="sxs-lookup"><span data-stu-id="dedea-291">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="dedea-292">當使用者提出下一個要求時，需要再次登入。</span><span class="sxs-lookup"><span data-stu-id="dedea-292">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="dedea-293">所有以 Keyring 保護的資料都無法再解密。</span><span class="sxs-lookup"><span data-stu-id="dedea-293">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="dedea-294">這可能會包含 [CSRF 權杖](xref:security/anti-request-forgery#how-does-aspnet-core-mvc-address-csrf)和 [ASP.NET Core MVC tempdata cookie](xref:fundamentals/app-state#tempdata)。</span><span class="sxs-lookup"><span data-stu-id="dedea-294">This may include [CSRF tokens](xref:security/anti-request-forgery#how-does-aspnet-core-mvc-address-csrf) and [ASP.NET Core MVC tempdata cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="dedea-295">若要在 IIS 下設定資料保護以保存 Keyring，請使用下列其中**一種**方法：</span><span class="sxs-lookup"><span data-stu-id="dedea-295">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="dedea-296">**建立資料保護登錄機碼**</span><span class="sxs-lookup"><span data-stu-id="dedea-296">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="dedea-297">ASP.NET Core 應用程式所使用的資料保護金鑰會儲存在應用程式外部的登錄中。</span><span class="sxs-lookup"><span data-stu-id="dedea-297">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="dedea-298">若要保存指定應用程式的金鑰，請為應用程式集區建立登錄機碼。</span><span class="sxs-lookup"><span data-stu-id="dedea-298">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="dedea-299">若為獨立的非Web 伺服陣列 IIS 安裝，請針對搭配使用 ASP.NET Core 應用程式的每個應用程式集區，使用[資料保護 Provision-AutoGenKeys.ps1 PowerShell 指令碼](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1)。</span><span class="sxs-lookup"><span data-stu-id="dedea-299">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="dedea-300">此指令碼會在 HKLM 登錄中建立登錄機碼，只有應用程式之應用程式集區的背景工作處理序帳戶可以存取它。</span><span class="sxs-lookup"><span data-stu-id="dedea-300">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="dedea-301">在待用期間使用 DPAPI 和全電腦金鑰加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="dedea-301">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="dedea-302">在 Web 伺服陣列案例中，應用程式可以設定成使用 UNC 路徑來儲存其資料保護 Keyring。</span><span class="sxs-lookup"><span data-stu-id="dedea-302">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="dedea-303">根據預設，資料保護金鑰不予加密。</span><span class="sxs-lookup"><span data-stu-id="dedea-303">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="dedea-304">請確保網路共用的檔案權限僅限於執行應用程式的 Windows 帳戶。</span><span class="sxs-lookup"><span data-stu-id="dedea-304">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="dedea-305">可以使用 X509 憑證來保護待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="dedea-305">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="dedea-306">請考慮下列讓使用者上傳憑證的機制：將憑證放入使用者的受信任憑證存放區，並確保在執行使用者應用程式的所有電腦上都能使用這些憑證。</span><span class="sxs-lookup"><span data-stu-id="dedea-306">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="dedea-307">詳細資訊請參閱[設定資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="dedea-307">See [Configuring Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="dedea-308">**設定 IIS 應用程式集區載入使用者設定檔**</span><span class="sxs-lookup"><span data-stu-id="dedea-308">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="dedea-309">此設定位在應用程式集區 [進階設定] 下的 [處理序模型] 區段中。</span><span class="sxs-lookup"><span data-stu-id="dedea-309">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="dedea-310">將 [載入使用者設定檔] 設為 `True`。</span><span class="sxs-lookup"><span data-stu-id="dedea-310">Set Load User Profile to `True`.</span></span> <span data-ttu-id="dedea-311">這會將金鑰儲存在使用者設定檔目錄下，並使用 DPAPI 與應用程式集區所用的特定使用者帳戶金鑰加以保護。</span><span class="sxs-lookup"><span data-stu-id="dedea-311">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used by the app pool.</span></span>

* <span data-ttu-id="dedea-312">**將檔案系統當作 Keyring 存放區使用**</span><span class="sxs-lookup"><span data-stu-id="dedea-312">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="dedea-313">調整應用程式程式碼，以[將檔案系統當作 Keyring 存放區使用](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="dedea-313">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="dedea-314">使用 X509 憑證來保護 Keyring，並確保憑證是受信任的憑證。</span><span class="sxs-lookup"><span data-stu-id="dedea-314">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="dedea-315">如果憑證為自我簽署，請將憑證放在受信任的根存放區。</span><span class="sxs-lookup"><span data-stu-id="dedea-315">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="dedea-316">在 Web 伺服陣列中使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="dedea-316">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="dedea-317">請使用所有電腦都可以存取的檔案共用。</span><span class="sxs-lookup"><span data-stu-id="dedea-317">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="dedea-318">將 X509 憑證部署到每一部電腦。</span><span class="sxs-lookup"><span data-stu-id="dedea-318">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="dedea-319">設定[程式碼中的資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="dedea-319">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="dedea-320">**設定資料保護的全電腦原則**</span><span class="sxs-lookup"><span data-stu-id="dedea-320">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="dedea-321">針對取用資料保護 API 的所有應用程式，資料保護系統僅支援有限的預設[全電腦原則](xref:security/data-protection/configuration/machine-wide-policy)設定。</span><span class="sxs-lookup"><span data-stu-id="dedea-321">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="dedea-322">如需詳細資料，請參閱[資料保護文件](xref:security/data-protection/index)。</span><span class="sxs-lookup"><span data-stu-id="dedea-322">See the [data protection documentation](xref:security/data-protection/index) for details.</span></span>

## <a name="sub-application-configuration"></a><span data-ttu-id="dedea-323">子應用程式設定</span><span class="sxs-lookup"><span data-stu-id="dedea-323">Sub-application configuration</span></span>

<span data-ttu-id="dedea-324">根應用程式下新增的子應用程式不應包含當作處理常式的 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="dedea-324">Sub-apps added under the root app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="dedea-325">如果您在子應用程式的 *web.config* 檔案中將模組新增為處理常式，則嘗試瀏覽子應用程式時，會收到參考錯誤設定檔的 *500.19 內部伺服器錯誤*。</span><span class="sxs-lookup"><span data-stu-id="dedea-325">If the module is added as a handler in a sub-app's *web.config* file, a *500.19 Internal Server Error* referencing the faulty config file is received when attempting to browse the sub-app.</span></span>

<span data-ttu-id="dedea-326">下列範例顯示 ASP.NET Core 子應用程式的已發佈 *web.config* 檔案：</span><span class="sxs-lookup"><span data-stu-id="dedea-326">The following example shows a published *web.config* file for an ASP.NET Core sub-app:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="dedea-327">在 ASP.NET Core 應用程式下裝載非 ASP.NET Core 子應用程式時，請明確移除子應用程式 *web.config* 檔案中已繼承的處理常式：</span><span class="sxs-lookup"><span data-stu-id="dedea-327">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="dedea-328">如需設定 ASP.NET Core 模組的詳細資訊，請參閱 [ASP.NET Core 模組簡介](xref:fundamentals/servers/aspnet-core-module)主題和 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="dedea-328">For more information on configuring the ASP.NET Core Module, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="dedea-329">使用 web.config 的 IIS 組態</span><span class="sxs-lookup"><span data-stu-id="dedea-329">Configuration of IIS with web.config</span></span>

<span data-ttu-id="dedea-330">IIS 組態受到這些適用於反向 Proxy 組態之 IIS 功能 *web.config* 的 **\<system.webServer>** 區段所影響。</span><span class="sxs-lookup"><span data-stu-id="dedea-330">IIS configuration is influenced by the **\<system.webServer>** section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="dedea-331">如果在伺服器層級設定 IIS 使用動態壓縮，則在應用程式 *web.config* 檔案中的 **\<urlCompression>** 元素將可停用它。</span><span class="sxs-lookup"><span data-stu-id="dedea-331">If IIS is configured at the server level to use dynamic compression, the **\<urlCompression>** element in the app's *web.config* file can disable it.</span></span>

<span data-ttu-id="dedea-332">如需詳細資訊，請參閱 [\<system.webServer> 的設定參考](/iis/configuration/system.webServer/)、[ASP.NET Core 模組設定參考](xref:host-and-deploy/aspnet-core-module)以及[使用 IIS 模組與 ASP.NET Core](xref:host-and-deploy/iis/modules)。</span><span class="sxs-lookup"><span data-stu-id="dedea-332">For more information, see the [configuration reference for \<system.webServer>](/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module), and [Using IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="dedea-333">若要設定在隔離的應用程式集區中執行之個別應用程式的環境變數 (支援 IIS 10.0 或更新版本)，請參閱 IIS 參考文件之[環境變數 \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的 *AppCmd.exe 命令*一節。</span><span class="sxs-lookup"><span data-stu-id="dedea-333">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="dedea-334">web.config 的組態區段</span><span class="sxs-lookup"><span data-stu-id="dedea-334">Configuration sections of web.config</span></span>

<span data-ttu-id="dedea-335">ASP.NET Core 應用程式的設定不使用 *web.config* 中 ASP.NET 4.x 應用程式的設定區段：</span><span class="sxs-lookup"><span data-stu-id="dedea-335">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* <span data-ttu-id="dedea-336">**\<system.web>**</span><span class="sxs-lookup"><span data-stu-id="dedea-336">**\<system.web>**</span></span>
* <span data-ttu-id="dedea-337">**\<appSettings>**</span><span class="sxs-lookup"><span data-stu-id="dedea-337">**\<appSettings>**</span></span>
* <span data-ttu-id="dedea-338">**\<connectionStrings>**</span><span class="sxs-lookup"><span data-stu-id="dedea-338">**\<connectionStrings>**</span></span>
* <span data-ttu-id="dedea-339">**\<位置>**</span><span class="sxs-lookup"><span data-stu-id="dedea-339">**\<location>**</span></span>

<span data-ttu-id="dedea-340">使用其他組態提供者設定的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="dedea-340">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="dedea-341">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="dedea-341">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="dedea-342">應用程式集區</span><span class="sxs-lookup"><span data-stu-id="dedea-342">Application Pools</span></span>

<span data-ttu-id="dedea-343">在伺服器上裝載多個網站時，請在其各自的應用程式集區中執行每個應用程式，以將應用程式互相隔離。</span><span class="sxs-lookup"><span data-stu-id="dedea-343">When hosting multiple websites on a server, isolate the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="dedea-344">IIS [新增網站] 對話方塊預設成此組態。</span><span class="sxs-lookup"><span data-stu-id="dedea-344">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="dedea-345">當您提供**網站名稱**時，文字會自動轉移至 [應用程式集區] 文字方塊。</span><span class="sxs-lookup"><span data-stu-id="dedea-345">When **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="dedea-346">新增網站時，會使用該網站名稱建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="dedea-346">A new app pool is created using the site name when the site is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="dedea-347">應用程式集區身分識別</span><span class="sxs-lookup"><span data-stu-id="dedea-347">Application Pool Identity</span></span>

<span data-ttu-id="dedea-348">應用程式集區身分識別帳戶可讓應用程式在唯一的帳戶下執行，不必建立及管理網域或本機帳戶。</span><span class="sxs-lookup"><span data-stu-id="dedea-348">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="dedea-349">在 IIS 8.0 或更新版本中，IIS 管理背景工作處理序 (WAS) 會使用新的應用程式集區名稱建立虛擬帳戶，並預設在此帳戶下執行應用程式集區的背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="dedea-349">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="dedea-350">在 IIS 管理主控台中，於應用程式集區的 [進階設定] 下，確定 [身分識別] 設定為使用 **ApplicationPoolIdentity**：</span><span class="sxs-lookup"><span data-stu-id="dedea-350">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![應用程式集區進階設定對話方塊](index/_static/apppool-identity.png)

<span data-ttu-id="dedea-352">IIS 管理程序會在 Windows 安全系統中，以應用程式集區的名稱建立安全識別碼。</span><span class="sxs-lookup"><span data-stu-id="dedea-352">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="dedea-353">可使用此身分識別來保護資源。</span><span class="sxs-lookup"><span data-stu-id="dedea-353">Resources can be secured using this identity.</span></span> <span data-ttu-id="dedea-354">不過，此身分識別不是真正的使用者帳戶，也不會顯示在 Windows 使用者管理主控台中。</span><span class="sxs-lookup"><span data-stu-id="dedea-354">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="dedea-355">如果 IIS 背景工作處理序需要提升應用程式的存取權限，請修改包含應用程式的目錄存取控制清單 (ACL)：</span><span class="sxs-lookup"><span data-stu-id="dedea-355">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="dedea-356">開啟 Windows 檔案總管，巡覽至目錄。</span><span class="sxs-lookup"><span data-stu-id="dedea-356">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="dedea-357">以滑鼠右鍵按一下目錄並選取 [屬性]。</span><span class="sxs-lookup"><span data-stu-id="dedea-357">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="dedea-358">依序選取 [安全性] 索引標籤下的 [編輯] 按鈕和 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="dedea-358">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="dedea-359">選取 [位置] 按鈕，並確定選取系統。</span><span class="sxs-lookup"><span data-stu-id="dedea-359">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="dedea-360">在 [輸入要選取的物件名稱] 區域中，輸入 **IIS AppPool\\<app_pool_name>**。</span><span class="sxs-lookup"><span data-stu-id="dedea-360">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="dedea-361">選取 [檢查名稱] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="dedea-361">Select the **Check Names** button.</span></span> <span data-ttu-id="dedea-362">針對 [DefaultAppPool]，請使用 **IIS AppPool\DefaultAppPool** 檢查名稱。</span><span class="sxs-lookup"><span data-stu-id="dedea-362">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="dedea-363">選取 [檢查名稱] 按鈕時，[DefaultAppPool] 的值便會顯示於物件名稱區域中。</span><span class="sxs-lookup"><span data-stu-id="dedea-363">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="dedea-364">您無法直接將應用程式集區名稱輸入至物件名稱區域。</span><span class="sxs-lookup"><span data-stu-id="dedea-364">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="dedea-365">檢查物件名稱時，請使用 **IIS AppPool\\<app_pool_name>**的格式。</span><span class="sxs-lookup"><span data-stu-id="dedea-365">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：在選取 [檢查名稱] 之前，"DefaultAppPool" 這個應用程式集區名稱在物件名稱區域中會附加至 "IIS AppPool\"。](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="dedea-367">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="dedea-367">Select **OK**.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：選取 [檢查名稱] 之後，物件名稱 "DefaultAppPool" 會顯示在物件名稱區域中。](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="dedea-369">預設應該會授與讀取 &amp; 執行權限。</span><span class="sxs-lookup"><span data-stu-id="dedea-369">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="dedea-370">請視需要提供其他權限。</span><span class="sxs-lookup"><span data-stu-id="dedea-370">Provide additional permissions as needed.</span></span>

<span data-ttu-id="dedea-371">也可使用 **ICACLS** 工具透過命令提示字元授與存取權限。</span><span class="sxs-lookup"><span data-stu-id="dedea-371">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="dedea-372">使用 *DefaultAppPool*作為範例，將會使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="dedea-372">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="dedea-373">如需詳細資訊，請參閱 [icacls](/windows-server/administration/windows-commands/icacls) 主題。</span><span class="sxs-lookup"><span data-stu-id="dedea-373">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dedea-374">其他資源</span><span class="sxs-lookup"><span data-stu-id="dedea-374">Additional resources</span></span>

* [<span data-ttu-id="dedea-375">針對 IIS 上的 ASP.NET Core 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="dedea-375">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="dedea-376">Azure 應用程式服務和 IIS 常見的 ASP.NET Core 錯誤參考</span><span class="sxs-lookup"><span data-stu-id="dedea-376">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="dedea-377">ASP.NET Core 模組簡介</span><span class="sxs-lookup"><span data-stu-id="dedea-377">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="dedea-378">ASP.NET Core 模組組態參考</span><span class="sxs-lookup"><span data-stu-id="dedea-378">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="dedea-379">使用 IIS 模組與 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dedea-379">Using IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="dedea-380">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="dedea-380">Introduction to ASP.NET Core</span></span>](../index.md)
* [<span data-ttu-id="dedea-381">Microsoft IIS 官方網站</span><span class="sxs-lookup"><span data-stu-id="dedea-381">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="dedea-382">Microsoft TechNet Library：Windows Server</span><span class="sxs-lookup"><span data-stu-id="dedea-382">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
