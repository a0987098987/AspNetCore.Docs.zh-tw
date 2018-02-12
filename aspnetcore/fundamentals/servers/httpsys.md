---
title: "ASP.NET Core 中的 HTTP.sys 網頁伺服器實作"
author: rick-anderson
description: "導入了 HTTP.sys，這是 Windows 上的 ASP.NET Core 網頁伺服器。 建置在 Http.Sys 核心模式驅動程式之上，HTTP.sys 是 Kestrel 的替代方式，可以用於直接連線到網際網路而不使用 IIS。"
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/httpsys
ms.openlocfilehash: f36a86fc67e715165afd0c38992f9767f90cf3b4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="9c729-104">ASP.NET Core 中的 HTTP.sys 網頁伺服器實作</span><span class="sxs-lookup"><span data-stu-id="9c729-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="9c729-105">作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="9c729-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="9c729-106">本主題只適用於 ASP.NET Core 2.0 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="9c729-106">This topic applies only to ASP.NET Core 2.0 and later.</span></span> <span data-ttu-id="9c729-107">在舊版的 ASP.NET Core 中，HTTP.sys 名為 [WebListener](xref:fundamentals/servers/weblistener)。</span><span class="sxs-lookup"><span data-stu-id="9c729-107">In earlier versions of ASP.NET Core, HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener).</span></span>

<span data-ttu-id="9c729-108">HTTP.sys 是只在 Windows 上執行的 [ASP.NET Core 網頁伺服器](index.md)。</span><span class="sxs-lookup"><span data-stu-id="9c729-108">HTTP.sys is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="9c729-109">它建置在 [Http.Sys 核心模式驅動程式](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)之上。</span><span class="sxs-lookup"><span data-stu-id="9c729-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="9c729-110">HTTP.sys 是 [Kestrel](kestrel.md) 的替代方式，提供了一些 Kestel 沒有的功能。</span><span class="sxs-lookup"><span data-stu-id="9c729-110">HTTP.sys is an alternative to [Kestrel](kestrel.md) that offers some features that Kestel doesn't.</span></span> <span data-ttu-id="9c729-111">**HTTP.sys 不能與 IIS 或 IIS Express 搭配使用，因為它與 [ASP.NET Core 模組](aspnet-core-module.md)不相容。**</span><span class="sxs-lookup"><span data-stu-id="9c729-111">**HTTP.sys can't be used with IIS or IIS Express, as it's incompatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="9c729-112">HTTP.sys 支援下列功能：</span><span class="sxs-lookup"><span data-stu-id="9c729-112">HTTP.sys supports the following features:</span></span>

- [<span data-ttu-id="9c729-113">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="9c729-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="9c729-114">連接埠共用</span><span class="sxs-lookup"><span data-stu-id="9c729-114">Port sharing</span></span>
- <span data-ttu-id="9c729-115">使用 SNI 的 HTTPS</span><span class="sxs-lookup"><span data-stu-id="9c729-115">HTTPS with SNI</span></span>
- <span data-ttu-id="9c729-116">HTTP/2 over TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="9c729-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="9c729-117">直接檔案傳輸</span><span class="sxs-lookup"><span data-stu-id="9c729-117">Direct file transmission</span></span>
- <span data-ttu-id="9c729-118">回應快取</span><span class="sxs-lookup"><span data-stu-id="9c729-118">Response caching</span></span>
- <span data-ttu-id="9c729-119">WebSocket (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="9c729-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="9c729-120">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="9c729-120">Supported Windows versions:</span></span>

- <span data-ttu-id="9c729-121">Windows 7 與 Windows Server 2008 R2 和更新版本</span><span class="sxs-lookup"><span data-stu-id="9c729-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="9c729-122">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9c729-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="9c729-123">使用 HTTP.sys 的時機</span><span class="sxs-lookup"><span data-stu-id="9c729-123">When to use HTTP.sys</span></span>

<span data-ttu-id="9c729-124">HTTP.sys 可用於您需要直接向網際網路公開伺服器而不使用 IIS 的部署。</span><span class="sxs-lookup"><span data-stu-id="9c729-124">HTTP.sys is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![HTTP.sys 直接與網際網路通訊](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="9c729-126">由於它建置在 Http.Sys 之上，HTTP.sys 不需要反向 Proxy 伺服器以抵禦攻擊。</span><span class="sxs-lookup"><span data-stu-id="9c729-126">Because it's built on Http.Sys, HTTP.sys doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="9c729-127">Http.Sys 是成熟的技術，可抵禦許多種類的攻擊，並提供功能完整之網頁伺服器的強固性、安全性及延展性。</span><span class="sxs-lookup"><span data-stu-id="9c729-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="9c729-128">IIS 本身在 Http.Sys 之上以 HTTP 接聽程式的形式執行。</span><span class="sxs-lookup"><span data-stu-id="9c729-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="9c729-129">當您需要的功能不適用於 Kestrel 時，例如 Windows 驗證，HTTP.sys 會是內部部署的理想選擇。</span><span class="sxs-lookup"><span data-stu-id="9c729-129">HTTP.sys is a good choice for internal deployments when you need a feature not available in Kestrel, such as Windows authentication.</span></span>

![HTTP.sys 直接與內部網路通訊](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a><span data-ttu-id="9c729-131">如何使用 HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="9c729-131">How to use HTTP.sys</span></span>

<span data-ttu-id="9c729-132">以下是針對主機作業系統和您的 ASP.NET Core 應用程式的安裝工作概觀。</span><span class="sxs-lookup"><span data-stu-id="9c729-132">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="9c729-133">設定 Windows Server</span><span class="sxs-lookup"><span data-stu-id="9c729-133">Configure Windows Server</span></span>

* <span data-ttu-id="9c729-134">安裝您應用程式所需的 .NET 版本，例如 [.NET Core](https://www.microsoft.com/net/download/core) 或 [.NET Framework](https://www.microsoft.com/net/download/framework)。</span><span class="sxs-lookup"><span data-stu-id="9c729-134">Install the version of .NET that your application requires, such as [.NET Core](https://www.microsoft.com/net/download/core) or [.NET Framework](https://www.microsoft.com/net/download/framework).</span></span>

* <span data-ttu-id="9c729-135">預先註冊 URL 前置詞以繫結至 HTTP.sys，然後設定 SSL 憑證</span><span class="sxs-lookup"><span data-stu-id="9c729-135">Preregister URL prefixes to bind to HTTP.sys, and set up SSL certificates</span></span>

   <span data-ttu-id="9c729-136">如果您不在 Windows 預先註冊 URL 前置詞，則必須以系統管理員權限執行您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9c729-136">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="9c729-137">唯一的例外是如果您使用 HTTP (不是 HTTPS) 繫結至 localhost，且使用大於 1024 的連接埠號碼；在此情況下不需要系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="9c729-137">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case, administrator privileges aren't required.</span></span>

   <span data-ttu-id="9c729-138">如需詳細資料，請參閱本文稍後的[如何預先註冊前置詞和設定 SSL](#preregister-url-prefixes-and-configure-ssl)。</span><span class="sxs-lookup"><span data-stu-id="9c729-138">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="9c729-139">開啟防火牆連接埠來允許流量到達 HTTP.sys。</span><span class="sxs-lookup"><span data-stu-id="9c729-139">Open firewall ports to allow traffic to reach HTTP.sys.</span></span>

   <span data-ttu-id="9c729-140">您可以使用 *netsh.exe* 或 [PowerShell Cmdlet](https://technet.microsoft.com/library/jj554906)。</span><span class="sxs-lookup"><span data-stu-id="9c729-140">You can use *netsh.exe* or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="9c729-141">另外還有 [Http.Sys 登錄設定](https://support.microsoft.com/kb/820129)。</span><span class="sxs-lookup"><span data-stu-id="9c729-141">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a><span data-ttu-id="9c729-142">設定 ASP.NET Core 應用程式以使用 HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="9c729-142">Configure your ASP.NET Core application to use HTTP.sys</span></span>

* <span data-ttu-id="9c729-143">如果您使用 [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) 中繼套件，則不需要安裝任何套件。</span><span class="sxs-lookup"><span data-stu-id="9c729-143">No package install is needed if you use the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="9c729-144">[Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) 套件包含在中繼套件裡。</span><span class="sxs-lookup"><span data-stu-id="9c729-144">The [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) package is included in the metapackage.</span></span>

* <span data-ttu-id="9c729-145">在您的 `Main` 方法中，對 `WebHostBuilder` 呼叫 `UseHttpSys` 擴充方法，並指定需要的任何 [HTTP.sys 選項](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="9c729-145">Call the `UseHttpSys` extension method on `WebHostBuilder` in your `Main` method, specifying any [HTTP.sys options](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a><span data-ttu-id="9c729-146">設定 HTTP.sys 選項</span><span class="sxs-lookup"><span data-stu-id="9c729-146">Configure HTTP.sys options</span></span>

<span data-ttu-id="9c729-147">以下是您可以設定的一些 HTTP.sys 設定和限制。</span><span class="sxs-lookup"><span data-stu-id="9c729-147">Here are some of the HTTP.sys settings and limits that you can configure.</span></span>

<span data-ttu-id="9c729-148">**用戶端連線數目上限**</span><span class="sxs-lookup"><span data-stu-id="9c729-148">**Maximum client connections**</span></span>

<span data-ttu-id="9c729-149">可以使用 *Program.cs* 中的下列程式碼，針對整個應用程式設定同時開啟的 TCP 連線數目上限：</span><span class="sxs-lookup"><span data-stu-id="9c729-149">The maximum number of concurrent open TCP connections can be set for the entire application with the following code in *Program.cs*:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

<span data-ttu-id="9c729-150">連線數目上限預設為無限制 (null)。</span><span class="sxs-lookup"><span data-stu-id="9c729-150">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="9c729-151">**要求主體大小上限**</span><span class="sxs-lookup"><span data-stu-id="9c729-151">**Maximum request body size**</span></span>

<span data-ttu-id="9c729-152">預設的要求主體大小上限是 30,000,000 個位元組，大約 28.6MB。</span><span class="sxs-lookup"><span data-stu-id="9c729-152">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span>

<span data-ttu-id="9c729-153">要覆寫 ASP.NET Core MVC 應用程式中的限制，建議的方式是使用 action 方法上的 [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) 屬性：</span><span class="sxs-lookup"><span data-stu-id="9c729-153">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="9c729-154">以下範例會示範如何設定整個應用程式、每個要求的條件約束：</span><span class="sxs-lookup"><span data-stu-id="9c729-154">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

<span data-ttu-id="9c729-155">您可以在 *Startup.cs* 中覆寫特定要求的設定：</span><span class="sxs-lookup"><span data-stu-id="9c729-155">You can override the setting on a specific request in *Startup.cs*:</span></span>

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
<span data-ttu-id="9c729-156">如果您嘗試在應用程式已開始讀取要求之後，設定要求的限制，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9c729-156">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="9c729-157">有一個 `IsReadOnly` 屬性會告訴您 `MaxRequestBodySize` 屬性處於唯讀狀態，這表示要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="9c729-157">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="9c729-158">如需其他 HTTP.sys 選項的資訊，請參閱 [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)。</span><span class="sxs-lookup"><span data-stu-id="9c729-158">For information about other HTTP.sys options, see [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> 

### <a name="configure-urls-and-ports-to-listen-on"></a><span data-ttu-id="9c729-159">設定要接聽的 URL 和連接埠</span><span class="sxs-lookup"><span data-stu-id="9c729-159">Configure URLs and ports to listen on</span></span> 

<span data-ttu-id="9c729-160">ASP.NET Core 預設會繫結至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="9c729-160">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="9c729-161">若要設定 URL 前置詞和連接埠，您可以使用 `UseUrls` 擴充方法、`urls` 命令列引數、ASPNETCORE_URLS 環境變數，或 [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) 的 `UrlPrefixes` 屬性。</span><span class="sxs-lookup"><span data-stu-id="9c729-161">To configure URL prefixes and ports, you can use the `UseUrls` extension method, the `urls` command-line argument, the ASPNETCORE_URLS environment variable, or the `UrlPrefixes` property on [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> <span data-ttu-id="9c729-162">下列程式碼範例使用 `UrlPrefixes`。</span><span class="sxs-lookup"><span data-stu-id="9c729-162">The following code example uses `UrlPrefixes`.</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

<span data-ttu-id="9c729-163">`UrlPrefixes` 的優點是如果您嘗試新增格式錯誤的前置詞，您會立即收到錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="9c729-163">An advantage of `UrlPrefixes` is that you get an error message immediately if you try to add a prefix that's formatted wrong.</span></span> <span data-ttu-id="9c729-164">`UseUrls` 的優點 (與 `urls` 和 ASPNETCORE_URLS 相同) 是您可以更輕鬆地切換 Kestrel 和 HTTP.sys。</span><span class="sxs-lookup"><span data-stu-id="9c729-164">An advantage of `UseUrls` (shared with `urls` and ASPNETCORE_URLS) is that you can more easily switch between Kestrel and HTTP.sys.</span></span>

<span data-ttu-id="9c729-165">如果您同時使用 `UseUrls` (或 `urls` 或 ASPNETCORE_URLS) 和 `UrlPrefixes`，`UrlPrefixes` 中的設定會覆寫 `UseUrls` 中的設定。</span><span class="sxs-lookup"><span data-stu-id="9c729-165">If you use both `UseUrls` (or `urls` or ASPNETCORE_URLS) and `UrlPrefixes`, the settings in `UrlPrefixes` override the ones in `UseUrls`.</span></span> <span data-ttu-id="9c729-166">如需詳細資訊，請參閱[裝載](xref:fundamentals/hosting)。</span><span class="sxs-lookup"><span data-stu-id="9c729-166">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="9c729-167">HTTP.sys 使用 [HTTP Server API UrlPrefix 字串格式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)。</span><span class="sxs-lookup"><span data-stu-id="9c729-167">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="9c729-168">請確定您在 `UseUrls` 或 `UrlPrefixes` 中指定您在伺服器上預先註冊的相同前置詞字串。</span><span class="sxs-lookup"><span data-stu-id="9c729-168">Make sure that you specify the same prefix strings in `UseUrls` or `UrlPrefixes` that you preregister on the server.</span></span> 

### <a name="dont-use-iis"></a><span data-ttu-id="9c729-169">不使用 IIS</span><span class="sxs-lookup"><span data-stu-id="9c729-169">Don't use IIS</span></span>

<span data-ttu-id="9c729-170">請確定您的應用程式未設定為執行 IIS 或 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="9c729-170">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="9c729-171">在 Visual Studio 中，預設啟動設定檔適用於 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="9c729-171">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="9c729-172">若要執行專案作為主控台應用程式，請手動變更選取的設定檔，如下列螢幕擷取畫面所示。</span><span class="sxs-lookup"><span data-stu-id="9c729-172">To run the project as a console application, manually change the selected profile, as shown in the following screen shot.</span></span>

![選取主控台應用程式設定檔](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="9c729-174">預先註冊 URL 前置詞及設定 SSL</span><span class="sxs-lookup"><span data-stu-id="9c729-174">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="9c729-175">IIS 和 HTTP.sys 都依賴基礎 Http.Sys 核心模式驅動程式來接聽要求，以及執行初始處理。</span><span class="sxs-lookup"><span data-stu-id="9c729-175">Both IIS and HTTP.sys rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="9c729-176">在 IIS 中，管理 UI 可讓您很輕鬆地設定所有項目。</span><span class="sxs-lookup"><span data-stu-id="9c729-176">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="9c729-177">不過，您需要自行設定 Http.Sys。</span><span class="sxs-lookup"><span data-stu-id="9c729-177">However, you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="9c729-178">自行設定 Http.Sys 的內建工具是 *netsh.exe*。</span><span class="sxs-lookup"><span data-stu-id="9c729-178">The built-in tool for doing that's *netsh.exe*.</span></span> 

<span data-ttu-id="9c729-179">使用 *netsh.exe*，您可以保留 URL 前置詞並指派 SSL 憑證。</span><span class="sxs-lookup"><span data-stu-id="9c729-179">With *netsh.exe* you can reserve URL prefixes and assign SSL certificates.</span></span> <span data-ttu-id="9c729-180">工具需要管理權限。</span><span class="sxs-lookup"><span data-stu-id="9c729-180">The tool requires administrative privileges.</span></span>

<span data-ttu-id="9c729-181">下列範例會示範保留連接埠 80 和 443 的 URL 前置詞時所需的最小值：</span><span class="sxs-lookup"><span data-stu-id="9c729-181">The following example shows the minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="9c729-182">下列範例會示範如何指派 SSL 憑證：</span><span class="sxs-lookup"><span data-stu-id="9c729-182">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

<span data-ttu-id="9c729-183">以下是 *netsh.exe* 的參考文件：</span><span class="sxs-lookup"><span data-stu-id="9c729-183">Here is the reference documentation for *netsh.exe*:</span></span>

* [<span data-ttu-id="9c729-184">超文字傳輸通訊協定 (HTTP) 的 Netsh 命令</span><span class="sxs-lookup"><span data-stu-id="9c729-184">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="9c729-185">UrlPrefix 字串</span><span class="sxs-lookup"><span data-stu-id="9c729-185">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="9c729-186">下列資源提供數種案例的詳細指示。</span><span class="sxs-lookup"><span data-stu-id="9c729-186">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="9c729-187">提到 HttpListener 的文章同樣適用於 HTTP.sys，因為兩者都是根據 Http.Sys。</span><span class="sxs-lookup"><span data-stu-id="9c729-187">Articles that refer to HttpListener apply equally to HTTP.sys, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="9c729-188">如何：使用 SSL 憑證設定連接埠</span><span class="sxs-lookup"><span data-stu-id="9c729-188">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="9c729-189">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) (HTTPS 通訊 - 以 HttpListener 為基礎的裝載和用戶端憑證) 這是協力廠商部落格，相當老舊但仍有有用的資訊。</span><span class="sxs-lookup"><span data-stu-id="9c729-189">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="9c729-190">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) (如何：逐步解說使用 HttpListener 或 Http Server unmanaged 程式碼 (C++) 作為 SSL 簡單伺服器) 這也是具有有用資訊的較舊部落格。</span><span class="sxs-lookup"><span data-stu-id="9c729-190">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>

<span data-ttu-id="9c729-191">以下是一些協力廠商工具，比起 *netsh.exe* 命令列可以更輕鬆地使用。</span><span class="sxs-lookup"><span data-stu-id="9c729-191">Here are some third-party tools that can be easier to use than the *netsh.exe* command line.</span></span> <span data-ttu-id="9c729-192">這些不是由 Microsoft 提供，或經由 Microsoft 背書。</span><span class="sxs-lookup"><span data-stu-id="9c729-192">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="9c729-193">工具預設會以系統管理員身分執行，因為 *netsh.exe* 本身需要系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="9c729-193">The tools run as administrator by default, since *netsh.exe* itself requires administrator privileges.</span></span>

* <span data-ttu-id="9c729-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) 提供 UI 以便列出和設定 SSL 憑證和選項、前置詞保留，以及憑證信任清單。</span><span class="sxs-lookup"><span data-stu-id="9c729-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="9c729-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) 可讓您列出或設定 SSL 憑證和 URL 前置詞。</span><span class="sxs-lookup"><span data-stu-id="9c729-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="9c729-196">UI 比 http.sys Manager 更精簡，並公開更多組態選項，但在其他方面提供類似的功能。</span><span class="sxs-lookup"><span data-stu-id="9c729-196">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="9c729-197">它無法建立新的憑證信任清單 (CTL)，但可以指派現有的憑證信任清單。</span><span class="sxs-lookup"><span data-stu-id="9c729-197">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a><span data-ttu-id="9c729-198">後續步驟</span><span class="sxs-lookup"><span data-stu-id="9c729-198">Next steps</span></span>

<span data-ttu-id="9c729-199">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="9c729-199">For more information, see the following resources:</span></span>

* [<span data-ttu-id="9c729-200">本文的範例應用程式</span><span class="sxs-lookup"><span data-stu-id="9c729-200">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [<span data-ttu-id="9c729-201">HTTP.sys 原始程式碼</span><span class="sxs-lookup"><span data-stu-id="9c729-201">HTTP.sys source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="9c729-202">裝載</span><span class="sxs-lookup"><span data-stu-id="9c729-202">Hosting</span></span>](xref:fundamentals/hosting)
