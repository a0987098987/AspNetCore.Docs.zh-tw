---
title: "ASP.NET Core HTTP.sys web 伺服器實作"
author: rick-anderson
description: "導入了 HTTP.sys，適用於在 Windows 上的 ASP.NET Core web 伺服器。 建置在 Http.Sys 核心模式驅動程式，HTTP.sys 會是 Kestrel 用於 IIS 沒有網際網路直接連線的替代方式。"
keywords: "ASP.NET Core,HttpSys,HTTP.sys,HttpListener,url 前置詞的 SSL"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: 0a7286e4-6428-424e-b5c4-5c98815cf61c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/httpsys
ms.openlocfilehash: d3f9eb4943ed62b674d6bb2ab1b275b0a3c02343
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="d3a7e-105">ASP.NET Core HTTP.sys web 伺服器實作</span><span class="sxs-lookup"><span data-stu-id="d3a7e-105">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="d3a7e-106">由[Tom Dykstra](https://github.com/tdykstra)和[Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="d3a7e-106">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="d3a7e-107">本主題只適用於 ASP.NET Core 2.0 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-107">This topic applies only to ASP.NET Core 2.0 and later.</span></span> <span data-ttu-id="d3a7e-108">在舊版的 ASP.NET Core，HTTP.sys 會命名為[WebListener](xref:fundamentals/servers/weblistener)。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-108">In earlier versions of ASP.NET Core, HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener).</span></span>

<span data-ttu-id="d3a7e-109">HTTP.sys 是[適用於 ASP.NET Core web 伺服器](index.md)只能在 Windows 上執行。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-109">HTTP.sys is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="d3a7e-110">此系統建置[Http.Sys 核心模式驅動程式](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-110">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="d3a7e-111">HTTP.sys 是替代[Kestrel](kestrel.md) ，提供了一些 Kestel 沒有的功能。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-111">HTTP.sys is an alternative to [Kestrel](kestrel.md) that offers some features that Kestel doesn't.</span></span> <span data-ttu-id="d3a7e-112">**HTTP.sys 不能與 IIS 或 IIS Express，並不相容於[ASP.NET 核心模組](aspnet-core-module.md)。**</span><span class="sxs-lookup"><span data-stu-id="d3a7e-112">**HTTP.sys can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="d3a7e-113">HTTP.sys 支援下列功能：</span><span class="sxs-lookup"><span data-stu-id="d3a7e-113">HTTP.sys supports the following features:</span></span>

- [<span data-ttu-id="d3a7e-114">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="d3a7e-114">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="d3a7e-115">連接埠共用</span><span class="sxs-lookup"><span data-stu-id="d3a7e-115">Port sharing</span></span>
- <span data-ttu-id="d3a7e-116">SNI 使用 HTTPS</span><span class="sxs-lookup"><span data-stu-id="d3a7e-116">HTTPS with SNI</span></span>
- <span data-ttu-id="d3a7e-117">HTTP/2 5060，TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="d3a7e-117">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="d3a7e-118">直接檔案傳輸</span><span class="sxs-lookup"><span data-stu-id="d3a7e-118">Direct file transmission</span></span>
- <span data-ttu-id="d3a7e-119">回應快取</span><span class="sxs-lookup"><span data-stu-id="d3a7e-119">Response caching</span></span>
- <span data-ttu-id="d3a7e-120">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="d3a7e-120">WebSockets (Windows 8)</span></span>

<span data-ttu-id="d3a7e-121">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="d3a7e-121">Supported Windows versions:</span></span>

- <span data-ttu-id="d3a7e-122">Windows 7 和 Windows Server 2008 R2 和更新版本</span><span class="sxs-lookup"><span data-stu-id="d3a7e-122">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="d3a7e-123">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d3a7e-123">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="d3a7e-124">使用 HTTP.sys 的時機</span><span class="sxs-lookup"><span data-stu-id="d3a7e-124">When to use HTTP.sys</span></span>

<span data-ttu-id="d3a7e-125">HTTP.sys 可用於部署您要公開，直接向網際網路伺服器而不使用 IIS。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-125">HTTP.sys is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![HTTP.sys 直接與網際網路通訊](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="d3a7e-127">由於它建置在 Http.Sys 中，HTTP.sys 不需要保護以對抗攻擊反向 proxy 伺服器。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-127">Because it's built on Http.Sys, HTTP.sys doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="d3a7e-128">Http.Sys 是成熟的技術，可保護免於許多種類的攻擊，並提供強固性、 安全性及功能完整的 web 伺服器的延展性。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-128">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="d3a7e-129">為 HTTP 接聽程式在 Http.Sys 之上，IIS 本身的執行。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-129">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="d3a7e-130">當您需要的功能不適用於 Kestrel，例如 Windows 驗證時，HTTP.sys 會是內部部署的理想選擇。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-130">HTTP.sys is a good choice for internal deployments when you need a feature not available in Kestrel, such as Windows authentication.</span></span>

![HTTP.sys 直接與內部網路通訊](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a><span data-ttu-id="d3a7e-132">如何使用 HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="d3a7e-132">How to use HTTP.sys</span></span>

<span data-ttu-id="d3a7e-133">以下是針對主機作業系統和您的 ASP.NET Core 應用程式安裝工作的概觀。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-133">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="d3a7e-134">設定 Windows Server</span><span class="sxs-lookup"><span data-stu-id="d3a7e-134">Configure Windows Server</span></span>

* <span data-ttu-id="d3a7e-135">安裝新版的.NET 應用程式所需，例如[.NET Core](https://www.microsoft.com/net/download/core)或[.NET Framework](https://www.microsoft.com/net/download/framework)。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-135">Install the version of .NET that your application requires, such as [.NET Core](https://www.microsoft.com/net/download/core) or [.NET Framework](https://www.microsoft.com/net/download/framework).</span></span>

* <span data-ttu-id="d3a7e-136">__'Asverify'__ 繫結至 HTTP.sys，以及設定 SSL 憑證的 URL 前置詞</span><span class="sxs-lookup"><span data-stu-id="d3a7e-136">Preregister URL prefixes to bind to HTTP.sys, and set up SSL certificates</span></span>

   <span data-ttu-id="d3a7e-137">如果您不要 __'asverify'__ URL 前置詞，在 Windows 中的，您必須以系統管理員權限執行您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-137">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="d3a7e-138">唯一的例外是如果您將繫結使用 HTTP (不是 HTTPS) 連接埠號碼大於 1024; localhost在此情況下，系統管理員權限不需要。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-138">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case, administrator privileges aren't required.</span></span>

   <span data-ttu-id="d3a7e-139">如需詳細資訊，請參閱[如何 __'asverify'__ 前置詞及設定 SSL](#preregister-url-prefixes-and-configure-ssl)本文稍後。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-139">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="d3a7e-140">開啟防火牆連接埠來允許流量到達 HTTP.sys。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-140">Open firewall ports to allow traffic to reach HTTP.sys.</span></span>

   <span data-ttu-id="d3a7e-141">您可以使用*netsh.exe*或[PowerShell cmdlet](https://technet.microsoft.com/library/jj554906)。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-141">You can use *netsh.exe* or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="d3a7e-142">另外還有[Http.Sys 登錄設定](https://support.microsoft.com/kb/820129)。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-142">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a><span data-ttu-id="d3a7e-143">設定您的 ASP.NET Core 應用程式使用 HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="d3a7e-143">Configure your ASP.NET Core application to use HTTP.sys</span></span>

* <span data-ttu-id="d3a7e-144">如果您使用，則需要任何封裝安裝[Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-144">No package install is needed if you use the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="d3a7e-145">[Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/)封裝包含在 metapackage。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-145">The [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) package is included in the metapackage.</span></span>

* <span data-ttu-id="d3a7e-146">呼叫`UseHttpSys`上的擴充方法`WebHostBuilder`中您`Main`方法，並指定任何[HTTP.sys 選項](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)，您需要如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="d3a7e-146">Call the `UseHttpSys` extension method on `WebHostBuilder` in your `Main` method, specifying any [HTTP.sys options](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a><span data-ttu-id="d3a7e-147">設定 HTTP.sys 選項</span><span class="sxs-lookup"><span data-stu-id="d3a7e-147">Configure HTTP.sys options</span></span>

<span data-ttu-id="d3a7e-148">以下是一些 HTTP.sys 設定，您可以設定的限制。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-148">Here are some of the HTTP.sys settings and limits that you can configure.</span></span>

<span data-ttu-id="d3a7e-149">**最大用戶端連線**</span><span class="sxs-lookup"><span data-stu-id="d3a7e-149">**Maximum client connections**</span></span>

<span data-ttu-id="d3a7e-150">同時開啟的 TCP 連線的數目上限可以設定整個應用程式中的下列程式碼*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="d3a7e-150">The maximum number of concurrent open TCP connections can be set for the entire application with the following code in *Program.cs*:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

<span data-ttu-id="d3a7e-151">最大連接數目是無限制 (null) 的預設值。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-151">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="d3a7e-152">**要求主體大小上限**</span><span class="sxs-lookup"><span data-stu-id="d3a7e-152">**Maximum request body size**</span></span>

<span data-ttu-id="d3a7e-153">預設的最大要求本文大小是 30,000,000 位元組，大約 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-153">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span>

<span data-ttu-id="d3a7e-154">若要覆寫中的 ASP.NET Core MVC 應用程式的限制的建議的方式是使用[RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs)動作方法上的屬性：</span><span class="sxs-lookup"><span data-stu-id="d3a7e-154">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="d3a7e-155">以下是範例，示範如何設定整個應用程式的每個要求的條件約束：</span><span class="sxs-lookup"><span data-stu-id="d3a7e-155">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

<span data-ttu-id="d3a7e-156">您可以覆寫中的特定要求上設定*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d3a7e-156">You can override the setting on a specific request in *Startup.cs*:</span></span>

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
<span data-ttu-id="d3a7e-157">如果您嘗試將應用程式已開始讀取要求之後，在要求上設定的限制，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-157">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="d3a7e-158">沒有`IsReadOnly`屬性會告訴您，如果`MaxRequestBodySize`屬性處於唯讀狀態，這表示它已經太遲設定的限制。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-158">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="d3a7e-159">HTTP.sys 中的其他選項的相關資訊，請參閱[HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-159">For information about other HTTP.sys options, see [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> 

### <a name="configure-urls-and-ports-to-listen-on"></a><span data-ttu-id="d3a7e-160">設定 Url 和連接埠上接聽</span><span class="sxs-lookup"><span data-stu-id="d3a7e-160">Configure URLs and ports to listen on</span></span> 

<span data-ttu-id="d3a7e-161">根據預設 ASP.NET Core 繫結至`http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-161">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="d3a7e-162">若要設定 URL 前置詞和連接埠，您可以使用`UseUrls`擴充方法，`urls`命令列引數、 ASPNETCORE_URLS 環境變數，或`UrlPrefixes`屬性[HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-162">To configure URL prefixes and ports, you can use the `UseUrls` extension method, the `urls` command-line argument, the ASPNETCORE_URLS environment variable, or the `UrlPrefixes` property on [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> <span data-ttu-id="d3a7e-163">下列程式碼範例使用`UrlPrefixes`。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-163">The following code example uses `UrlPrefixes`.</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

<span data-ttu-id="d3a7e-164">優點`UrlPrefixes`會收到錯誤訊息立即如果您嘗試新增的格式錯誤的前置詞。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-164">An advantage of `UrlPrefixes` is that you get an error message immediately if you try to add a prefix that is formatted wrong.</span></span> <span data-ttu-id="d3a7e-165">優點`UseUrls`(與共用`urls`和 ASPNETCORE_URLS) 是您可以更輕鬆地切換 Kestrel 和 HTTP.sys 之間。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-165">An advantage of `UseUrls` (shared with `urls` and ASPNETCORE_URLS) is that you can more easily switch between Kestrel and HTTP.sys.</span></span>

<span data-ttu-id="d3a7e-166">如果您同時使用`UseUrls`(或`urls`或 ASPNETCORE_URLS) 和`UrlPrefixes`中的設定`UrlPrefixes`覆寫中的`UseUrls`。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-166">If you use both `UseUrls` (or `urls` or ASPNETCORE_URLS) and `UrlPrefixes`, the settings in `UrlPrefixes` override the ones in `UseUrls`.</span></span> <span data-ttu-id="d3a7e-167">如需詳細資訊，請參閱[裝載](xref:fundamentals/hosting)。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-167">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="d3a7e-168">HTTP.sys 會使用[HTTP 伺服器 API UrlPrefix 字串格式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-168">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="d3a7e-169">請確定您指定在相同的前置詞字串`UseUrls`或`UrlPrefixes`，您預先註冊的伺服器上。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-169">Make sure that you specify the same prefix strings in `UseUrls` or `UrlPrefixes` that you preregister on the server.</span></span> 

### <a name="dont-use-iis"></a><span data-ttu-id="d3a7e-170">不使用 IIS</span><span class="sxs-lookup"><span data-stu-id="d3a7e-170">Don't use IIS</span></span>

<span data-ttu-id="d3a7e-171">請確定您的應用程式未設定為執行 IIS 或 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-171">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="d3a7e-172">在 Visual Studio 中的預設啟動設定檔為 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-172">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="d3a7e-173">若要執行專案為主控台應用程式，手動變更選取的設定檔，如下列螢幕擷取畫面所示。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-173">To run the project as a console application, manually change the selected profile, as shown in the following screen shot.</span></span>

![選取主控台應用程式設定檔](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="d3a7e-175">__'Asverify'__ URL 前置詞及設定 SSL</span><span class="sxs-lookup"><span data-stu-id="d3a7e-175">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="d3a7e-176">IIS 和 HTTP.sys 依賴接聽要求的基礎 Http.Sys 核心模式驅動程式，並執行初始處理。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-176">Both IIS and HTTP.sys rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="d3a7e-177">在 IIS 中，管理 UI 可讓您很輕鬆地設定所有項目。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-177">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="d3a7e-178">不過，您需要自行設定 Http.Sys。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-178">However, you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="d3a7e-179">內建的工具，也就是執行*netsh.exe*。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-179">The built-in tool for doing that is *netsh.exe*.</span></span> 

<span data-ttu-id="d3a7e-180">與*netsh.exe*就可以保留 URL 首碼，並指派 SSL 憑證。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-180">With *netsh.exe* you can reserve URL prefixes and assign SSL certificates.</span></span> <span data-ttu-id="d3a7e-181">此工具需要系統管理權限。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-181">The tool requires administrative privileges.</span></span>

<span data-ttu-id="d3a7e-182">下列範例會示範保留連接埠 80 和 443 的 URL 前置詞時所需的最小值：</span><span class="sxs-lookup"><span data-stu-id="d3a7e-182">The following example shows the minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="d3a7e-183">下列範例會示範如何將 SSL 憑證指派：</span><span class="sxs-lookup"><span data-stu-id="d3a7e-183">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

<span data-ttu-id="d3a7e-184">以下是參考文件*netsh.exe*:</span><span class="sxs-lookup"><span data-stu-id="d3a7e-184">Here is the reference documentation for *netsh.exe*:</span></span>

* [<span data-ttu-id="d3a7e-185">Netsh 命令都會使用超文字傳輸通訊協定 (HTTP)</span><span class="sxs-lookup"><span data-stu-id="d3a7e-185">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="d3a7e-186">UrlPrefix 字串</span><span class="sxs-lookup"><span data-stu-id="d3a7e-186">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="d3a7e-187">下列資源提供數種案例的詳細的指示。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-187">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="d3a7e-188">指 HttpListener 的發行項同樣適用於 HTTP.sys，同時根據 Http.Sys。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-188">Articles that refer to HttpListener apply equally to HTTP.sys, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="d3a7e-189">如何： 使用 SSL 憑證設定連接埠</span><span class="sxs-lookup"><span data-stu-id="d3a7e-189">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="d3a7e-190">[HTTPS 通訊-HttpListener 基礎代管與用戶端憑證](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html)這是協力廠商架構部落格和相當老舊，但仍有有用的資訊。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-190">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="d3a7e-191">[如何： 逐步解說使用 HttpListener 或 Http 伺服器 unmanaged 程式碼 （c + +） 做為 SSL 簡單伺服器](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/)這也是較舊的部落格包含有用資訊。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-191">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>

<span data-ttu-id="d3a7e-192">以下是一些協力廠商工具可以更輕鬆地使用比*netsh.exe*命令列。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-192">Here are some third-party tools that can be easier to use than the *netsh.exe* command line.</span></span> <span data-ttu-id="d3a7e-193">這些不是所提供，或經由 Microsoft 背書。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-193">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="d3a7e-194">這些工具執行系統管理員身分根據預設，由於*netsh.exe*本身需要系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-194">The tools run as administrator by default, since *netsh.exe* itself requires administrator privileges.</span></span>

* <span data-ttu-id="d3a7e-195">[http.sys 管理員](http://httpsysmanager.codeplex.com/)清單提供 UI，並設定 SSL 憑證和選項保留的前置詞及憑證信任清單。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-195">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="d3a7e-196">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx)可讓您列出或設定 SSL 憑證和 URL 前置詞。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-196">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="d3a7e-197">UI 會更精簡比 http.sys 管理員，並公開更多組態選項，但否則它會提供類似的功能。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-197">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="d3a7e-198">它無法建立新的憑證信任清單 (CTL)，但可以將現有的指派。</span><span class="sxs-lookup"><span data-stu-id="d3a7e-198">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a><span data-ttu-id="d3a7e-199">後續步驟</span><span class="sxs-lookup"><span data-stu-id="d3a7e-199">Next steps</span></span>

<span data-ttu-id="d3a7e-200">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="d3a7e-200">For more information, see the following resources:</span></span>

* [<span data-ttu-id="d3a7e-201">這篇文章的範例應用程式</span><span class="sxs-lookup"><span data-stu-id="d3a7e-201">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [<span data-ttu-id="d3a7e-202">HTTP.sys 原始程式碼</span><span class="sxs-lookup"><span data-stu-id="d3a7e-202">HTTP.sys source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="d3a7e-203">裝載</span><span class="sxs-lookup"><span data-stu-id="d3a7e-203">Hosting</span></span>](xref:fundamentals/hosting)
