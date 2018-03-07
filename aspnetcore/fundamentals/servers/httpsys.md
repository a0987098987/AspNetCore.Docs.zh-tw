---
title: "ASP.NET Core 中的 HTTP.sys 網頁伺服器實作"
author: tdykstra
description: "深入了解 HTTP.sys，這是 Windows 上的 ASP.NET Core 網頁伺服器。 HTTP.sys 建置在 HTTP.sys 核心模式驅動程式之上，是 Kestrel 的替代方式，可以用來直接連線到網際網路而不使用 IIS。"
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/28/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 730ecf12f718f6bbbdefb7cdc561481b126c995b
ms.sourcegitcommit: c5ecda3c5b1674b62294cfddcb104e7f0b9ce465
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="56235-104">ASP.NET Core 中的 HTTP.sys 網頁伺服器實作</span><span class="sxs-lookup"><span data-stu-id="56235-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="56235-105">作者：[Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="56235-105">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Luke Latham](https://github.com/guardrex)</span></span>

> [!NOTE]
> <span data-ttu-id="56235-106">本主題適用於 ASP.NET Core 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="56235-106">This topic applies to ASP.NET Core 2.0 or later.</span></span> <span data-ttu-id="56235-107">在舊版的 ASP.NET Core 中，HTTP.sys 名為 [WebListener](xref:fundamentals/servers/weblistener)。</span><span class="sxs-lookup"><span data-stu-id="56235-107">In earlier versions of ASP.NET Core, HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener).</span></span>

<span data-ttu-id="56235-108">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) 是只在 Windows 上執行的 [ASP.NET Core 網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="56235-108">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) is a [web server for ASP.NET Core](xref:fundamentals/servers/index) that only runs on Windows.</span></span> <span data-ttu-id="56235-109">HTTP.sys 是 [Kestrel](xref:fundamentals/servers/kestrel) 的替代方式，提供了一些 Kestrel 未提供的功能。</span><span class="sxs-lookup"><span data-stu-id="56235-109">HTTP.sys is an alternative to [Kestrel](xref:fundamentals/servers/kestrel) and offers some features that Kestrel doesn't provide.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="56235-110">HTTP.sys 與 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)不相容，且不能與 IIS 或 IIS Express 搭配使用。</span><span class="sxs-lookup"><span data-stu-id="56235-110">HTTP.sys is incompatible with the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and can't be used with IIS or IIS Express.</span></span>

<span data-ttu-id="56235-111">HTTP.sys 支援下列功能：</span><span class="sxs-lookup"><span data-stu-id="56235-111">HTTP.sys supports the following features:</span></span>

* [<span data-ttu-id="56235-112">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="56235-112">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
* <span data-ttu-id="56235-113">連接埠共用</span><span class="sxs-lookup"><span data-stu-id="56235-113">Port sharing</span></span>
* <span data-ttu-id="56235-114">使用 SNI 的 HTTPS</span><span class="sxs-lookup"><span data-stu-id="56235-114">HTTPS with SNI</span></span>
* <span data-ttu-id="56235-115">透過 TLS 的 HTTP/2 (Windows 10 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="56235-115">HTTP/2 over TLS (Windows 10 or later)</span></span>
* <span data-ttu-id="56235-116">直接檔案傳輸</span><span class="sxs-lookup"><span data-stu-id="56235-116">Direct file transmission</span></span>
* <span data-ttu-id="56235-117">回應快取</span><span class="sxs-lookup"><span data-stu-id="56235-117">Response caching</span></span>
* <span data-ttu-id="56235-118">WebSocket (Windows 8 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="56235-118">WebSockets (Windows 8 or later)</span></span>

<span data-ttu-id="56235-119">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="56235-119">Supported Windows versions:</span></span>

* <span data-ttu-id="56235-120">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="56235-120">Windows 7 or later</span></span>
* <span data-ttu-id="56235-121">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="56235-121">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="56235-122">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="56235-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="56235-123">使用 HTTP.sys 的時機</span><span class="sxs-lookup"><span data-stu-id="56235-123">When to use HTTP.sys</span></span>

<span data-ttu-id="56235-124">HTTP.sys 在下列部署環境中非常有用：</span><span class="sxs-lookup"><span data-stu-id="56235-124">HTTP.sys is useful for deployments where:</span></span>

* <span data-ttu-id="56235-125">需要直接向網際網路公開伺服器而不使用 IIS。</span><span class="sxs-lookup"><span data-stu-id="56235-125">There's a need to expose the server directly to the Internet without using IIS.</span></span>

  ![HTTP.sys 直接與網際網路通訊](httpsys/_static/httpsys-to-internet.png)

* <span data-ttu-id="56235-127">內部部署需要的功能無法在 Kestrel 中使用，例如 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="56235-127">An internal deployment requires a feature not available in Kestrel, such as [Windows Authentication](xref:security/authentication/windowsauth).</span></span>

  ![HTTP.sys 直接與內部網路通訊](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="56235-129">HTTP.sys 是成熟的技術，可抵禦許多種類的攻擊，並提供功能完整之網頁伺服器的穩固性、安全性及延展性。</span><span class="sxs-lookup"><span data-stu-id="56235-129">HTTP.sys is mature technology that protects against many types of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="56235-130">IIS 本身在 HTTP.sys 之上以 HTTP 接聽程式的形式執行。</span><span class="sxs-lookup"><span data-stu-id="56235-130">IIS itself runs as an HTTP listener on top of HTTP.sys.</span></span> 

## <a name="how-to-use-httpsys"></a><span data-ttu-id="56235-131">如何使用 HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="56235-131">How to use HTTP.sys</span></span>

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a><span data-ttu-id="56235-132">設定 ASP.NET Core 應用程式使用 HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="56235-132">Configure the ASP.NET Core app to use HTTP.sys</span></span>

1. <span data-ttu-id="56235-133">使用 [Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) \(英文\)) (ASP.NET Core 2.0 或更新版本) 時，專案檔中不需要套件參考。</span><span class="sxs-lookup"><span data-stu-id="56235-133">A package reference in the project file isn't required when using the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.All/)) (ASP.NET Core 2.0 or later).</span></span> <span data-ttu-id="56235-134">若不是使用 `Microsoft.AspNetCore.All` 中繼套件，請將套件參考加入 [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/)。</span><span class="sxs-lookup"><span data-stu-id="56235-134">When not using the `Microsoft.AspNetCore.All` metapackage, add a package reference to [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).</span></span>

1. <span data-ttu-id="56235-135">建置 Web 主機時，呼叫 [UseHttpSys](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderhttpsysextensions.usehttpsys) 擴充方法，並指定任何必要的 [HTTP.sys 選項](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions)：</span><span class="sxs-lookup"><span data-stu-id="56235-135">Call the [UseHttpSys](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderhttpsysextensions.usehttpsys) extension method when building the web host, specifying any required [HTTP.sys options](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions):</span></span>

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   <span data-ttu-id="56235-136">其他的 HTTP.sys 設定則透過[登錄設定](https://support.microsoft.com/kb/820129)處理。</span><span class="sxs-lookup"><span data-stu-id="56235-136">Additional HTTP.sys configuration is handled through [registry settings](https://support.microsoft.com/kb/820129).</span></span>

   <span data-ttu-id="56235-137">**HTTP.sys 選項**</span><span class="sxs-lookup"><span data-stu-id="56235-137">**HTTP.sys options**</span></span>

   | <span data-ttu-id="56235-138">屬性</span><span class="sxs-lookup"><span data-stu-id="56235-138">Property</span></span> | <span data-ttu-id="56235-139">描述</span><span class="sxs-lookup"><span data-stu-id="56235-139">Description</span></span> | <span data-ttu-id="56235-140">預設</span><span class="sxs-lookup"><span data-stu-id="56235-140">Default</span></span> |
   | -------- | ----------- | :-----: |
   | [<span data-ttu-id="56235-141">AllowSynchronousIO</span><span class="sxs-lookup"><span data-stu-id="56235-141">AllowSynchronousIO</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.allowsynchronousio) | <span data-ttu-id="56235-142">控制是否允許 `HttpContext.Request.Body` 和 `HttpContext.Response.Body` 同步輸出/輸入。</span><span class="sxs-lookup"><span data-stu-id="56235-142">Control whether synchronous input/output is allowed for the `HttpContext.Request.Body` and `HttpContext.Response.Body`.</span></span> | `true` |
   | [<span data-ttu-id="56235-143">Authentication.AllowAnonymous</span><span class="sxs-lookup"><span data-stu-id="56235-143">Authentication.AllowAnonymous</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.allowanonymous) | <span data-ttu-id="56235-144">允許匿名要求。</span><span class="sxs-lookup"><span data-stu-id="56235-144">Allow anonymous requests.</span></span> | `true` |
   | [<span data-ttu-id="56235-145">Authentication.Schemes</span><span class="sxs-lookup"><span data-stu-id="56235-145">Authentication.Schemes</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.schemes) | <span data-ttu-id="56235-146">指定允許的驗證配置。</span><span class="sxs-lookup"><span data-stu-id="56235-146">Specify the allowed authentication schemes.</span></span> <span data-ttu-id="56235-147">處置接聽程式之前可隨時修改。</span><span class="sxs-lookup"><span data-stu-id="56235-147">May be modified at any time prior to disposing the listener.</span></span> <span data-ttu-id="56235-148">值是由 [AuthenticationSchemes enum](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationschemes) 提供：`Basic`、`Kerberos`、`Negotiate`、`None` 和 `NTLM`。</span><span class="sxs-lookup"><span data-stu-id="56235-148">Values are provided by the [AuthenticationSchemes enum](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationschemes): `Basic`, `Kerberos`, `Negotiate`, `None`, and `NTLM`.</span></span> | `None` |
   | [<span data-ttu-id="56235-149">EnableResponseCaching</span><span class="sxs-lookup"><span data-stu-id="56235-149">EnableResponseCaching</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.enableresponsecaching) | <span data-ttu-id="56235-150">針對含有合格標頭的回應嘗試[核心模式](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode)快取。</span><span class="sxs-lookup"><span data-stu-id="56235-150">Attempt [kernel-mode](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) caching for responses with eligible headers.</span></span> <span data-ttu-id="56235-151">回應可能不包含 `Set-Cookie`、`Vary` 或 `Pragma` 標頭。</span><span class="sxs-lookup"><span data-stu-id="56235-151">The response may not include `Set-Cookie`, `Vary`, or `Pragma` headers.</span></span> <span data-ttu-id="56235-152">它必須包含為 `public` 的 `Cache-Control` 標頭，且有 `shared-max-age` 或 `max-age` 值，或是 `Expires` 標頭。</span><span class="sxs-lookup"><span data-stu-id="56235-152">It must include a `Cache-Control` header that's `public` and either a `shared-max-age` or `max-age` value, or an `Expires` header.</span></span> | `true` |
   | [<span data-ttu-id="56235-153">MaxAccepts</span><span class="sxs-lookup"><span data-stu-id="56235-153">MaxAccepts</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxaccepts) | <span data-ttu-id="56235-154">可同時接受的數目上限。</span><span class="sxs-lookup"><span data-stu-id="56235-154">The maximum number of concurrent accepts.</span></span> | <span data-ttu-id="56235-155">5 &times; [Environment.<br>ProcessorCount](/dotnet/api/system.environment.processorcount)</span><span class="sxs-lookup"><span data-stu-id="56235-155">5 &times; [Environment.<br>ProcessorCount](/dotnet/api/system.environment.processorcount)</span></span> |
   | [<span data-ttu-id="56235-156">MaxConnections</span><span class="sxs-lookup"><span data-stu-id="56235-156">MaxConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxconnections) | <span data-ttu-id="56235-157">可接受的同時連線數量上限。</span><span class="sxs-lookup"><span data-stu-id="56235-157">The maximum number of concurrent connections to accept.</span></span> <span data-ttu-id="56235-158">使用 `-1` 為無限多個。</span><span class="sxs-lookup"><span data-stu-id="56235-158">Use `-1` for infinite.</span></span> <span data-ttu-id="56235-159">使用 `null` 以使用登錄之整個電腦的設定。</span><span class="sxs-lookup"><span data-stu-id="56235-159">Use `null` to use the registry's machine-wide setting.</span></span> | `null`<br><span data-ttu-id="56235-160">(無限制)</span><span class="sxs-lookup"><span data-stu-id="56235-160">(unlimited)</span></span> |
   | [<span data-ttu-id="56235-161">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="56235-161">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) | <span data-ttu-id="56235-162">請參閱 <a href="#maxrequestbodysize">MaxRequestBodySize</a> 小節。</span><span class="sxs-lookup"><span data-stu-id="56235-162">See the <a href="#maxrequestbodysize">MaxRequestBodySize</a> section.</span></span> | <span data-ttu-id="56235-163">30000000 位元組</span><span class="sxs-lookup"><span data-stu-id="56235-163">30000000 bytes</span></span><br><span data-ttu-id="56235-164">(~28.6 MB)</span><span class="sxs-lookup"><span data-stu-id="56235-164">(~28.6 MB)</span></span> |
   | [<span data-ttu-id="56235-165">RequestQueueLimit</span><span class="sxs-lookup"><span data-stu-id="56235-165">RequestQueueLimit</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.requestqueuelimit) | <span data-ttu-id="56235-166">可以加入佇列的最大要求數目。</span><span class="sxs-lookup"><span data-stu-id="56235-166">The maximum number of requests that can be queued.</span></span> | <span data-ttu-id="56235-167">1000</span><span class="sxs-lookup"><span data-stu-id="56235-167">1000</span></span> |
   | [<span data-ttu-id="56235-168">ThrowWriteExceptions</span><span class="sxs-lookup"><span data-stu-id="56235-168">ThrowWriteExceptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.throwwriteexceptions) | <span data-ttu-id="56235-169">指出若回應本文因為用戶端中斷連線而寫入失敗時，應擲回例外狀況或正常完成。</span><span class="sxs-lookup"><span data-stu-id="56235-169">Indicate if response body writes that fail due to client disconnects should throw exceptions or complete normally.</span></span> | `false`<br><span data-ttu-id="56235-170">(正常完成)</span><span class="sxs-lookup"><span data-stu-id="56235-170">(complete normally)</span></span> |
   | [<span data-ttu-id="56235-171">Timeouts</span><span class="sxs-lookup"><span data-stu-id="56235-171">Timeouts</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts) | <span data-ttu-id="56235-172">公開 HTTP.sys [TimeoutManager](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager) 設定，這也可在登錄中設定。</span><span class="sxs-lookup"><span data-stu-id="56235-172">Expose the HTTP.sys [TimeoutManager](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager) configuration, which may also be configured in the registry.</span></span> <span data-ttu-id="56235-173">API 連結可提供包括預設值在內每個設定的詳細資訊：</span><span class="sxs-lookup"><span data-stu-id="56235-173">Follow the API links to learn more about each setting, including default values:</span></span><ul><li><span data-ttu-id="56235-174">[Timeouts.DrainEntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.drainentitybody) &ndash; HTTP 伺服器 API 清空持續連線上實體內容的允許時間。</span><span class="sxs-lookup"><span data-stu-id="56235-174">[Timeouts.DrainEntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.drainentitybody) &ndash; Time allowed for the HTTP Server API to drain the entity body on a Keep-Alive connection.</span></span></li><li><span data-ttu-id="56235-175">[Timeouts.EntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.entitybody) &ndash; 要求實體內容到達的允許時間。</span><span class="sxs-lookup"><span data-stu-id="56235-175">[Timeouts.EntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.entitybody) &ndash; Time allowed for the request entity body to arrive.</span></span></li><li><span data-ttu-id="56235-176">[Timeouts.HeaderWait](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.headerwait) &ndash; HTTP 伺服器 API 剖析要求標頭的允許時間。</span><span class="sxs-lookup"><span data-stu-id="56235-176">[Timeouts.HeaderWait](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.headerwait) &ndash; Time allowed for the HTTP Server API to parse the request header.</span></span></li><li><span data-ttu-id="56235-177">[Timeouts.IdleConnection](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.idleconnection) &ndash; 允許連線閒置的時間。</span><span class="sxs-lookup"><span data-stu-id="56235-177">[Timeouts.IdleConnection](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.idleconnection) &ndash; Time allowed for an idle connection.</span></span></li><li><span data-ttu-id="56235-178">[Timeouts.MinSendBytesPerSecond](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.minsendbytespersecond) &ndash; 回應的最低傳送速率。</span><span class="sxs-lookup"><span data-stu-id="56235-178">[Timeouts.MinSendBytesPerSecond](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.minsendbytespersecond) &ndash; The minimum send rate for the response.</span></span></li><li><span data-ttu-id="56235-179">[Timeouts.RequestQueue](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.requestqueue) &ndash; 在應用程式 擷取要求之前，將要求保留於要求佇列中的允許時間。</span><span class="sxs-lookup"><span data-stu-id="56235-179">[Timeouts.RequestQueue](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.requestqueue) &ndash; Time allowed for the request to remain in the request queue before the app picks it up.</span></span></li></ul> |  |
   | [<span data-ttu-id="56235-180">UrlPrefixes</span><span class="sxs-lookup"><span data-stu-id="56235-180">UrlPrefixes</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes) | <span data-ttu-id="56235-181">指定 [UrlPrefixCollection](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection) 以向 HTTP.sys 註冊。</span><span class="sxs-lookup"><span data-stu-id="56235-181">Specify the [UrlPrefixCollection](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection) to register with HTTP.sys.</span></span> <span data-ttu-id="56235-182">最實用的是 [UrlPrefixCollection.Add](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection.add)，可用來將前置詞加入集合。</span><span class="sxs-lookup"><span data-stu-id="56235-182">The most useful is [UrlPrefixCollection.Add](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection.add), which is used to add a prefix to the collection.</span></span> <span data-ttu-id="56235-183">處置接聽程式之前可隨時修改這些內容。</span><span class="sxs-lookup"><span data-stu-id="56235-183">These may be modified at any time prior to disposing the listener.</span></span> |  |

   <a name="maxrequestbodysize"></a>
   <span data-ttu-id="56235-184">**MaxRequestBodySize**</span><span class="sxs-lookup"><span data-stu-id="56235-184">**MaxRequestBodySize**</span></span>

   <span data-ttu-id="56235-185">任何要求所允許的大小上限 (以位元組為單位)。</span><span class="sxs-lookup"><span data-stu-id="56235-185">The maximum allowed size of any request body in bytes.</span></span> <span data-ttu-id="56235-186">當設定為 `null` 時，要求主體大小上限為無限制。</span><span class="sxs-lookup"><span data-stu-id="56235-186">When set to `null`, the maximum request body size is unlimited.</span></span> <span data-ttu-id="56235-187">此限制對升級連線不會有任何影響，因為其一律為無限制。</span><span class="sxs-lookup"><span data-stu-id="56235-187">This limit has no effect on upgraded connections, which are always unlimited.</span></span>

   <span data-ttu-id="56235-188">在 ASP.NET Core MVC 應用程式中針對單一 `IActionResult` 覆寫限制的建議方法，是在動作方法上使用 [RequestSizeLimitAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) 屬性：</span><span class="sxs-lookup"><span data-stu-id="56235-188">The recommended method to override the limit in an ASP.NET Core MVC app for a single `IActionResult` is to use the [RequestSizeLimitAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>
   
   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   <span data-ttu-id="56235-189">如果應用程式已開始讀取要求之後，才嘗試設定要求的限制，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="56235-189">An exception is thrown if the app attempts to configure the limit on a request after the app has started reading the request.</span></span> <span data-ttu-id="56235-190">`IsReadOnly` 屬性可用來指出 `MaxRequestBodySize` 屬性是否處於唯讀狀態，代表要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="56235-190">An `IsReadOnly` property can be used to indicate if the `MaxRequestBodySize` property is in a read-only state, meaning it's too late to configure the limit.</span></span>

   <span data-ttu-id="56235-191">如果應用程式應針對每個要求覆寫 [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize)，則使用 [IHttpMaxRequestBodySizeFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpmaxrequestbodysizefeature)：</span><span class="sxs-lookup"><span data-stu-id="56235-191">If the app should override [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) per-request, use the [IHttpMaxRequestBodySizeFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpmaxrequestbodysizefeature):</span></span>

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

1. <span data-ttu-id="56235-192">如果您使用 Visual Studio，請確定應用程式未設定為執行 IIS 或 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="56235-192">If using Visual Studio, make sure the app isn't configured to run IIS or IIS Express.</span></span>

   <span data-ttu-id="56235-193">在 Visual Studio 中，預設啟動設定檔適用於 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="56235-193">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="56235-194">若要執行專案作為主控台應用程式，請手動變更選取的設定檔，如下列螢幕擷取畫面所示：</span><span class="sxs-lookup"><span data-stu-id="56235-194">To run the project as a console app, manually change the selected profile, as shown in the following screen shot:</span></span>

   ![選取主控台應用程式設定檔](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a><span data-ttu-id="56235-196">設定 Windows Server</span><span class="sxs-lookup"><span data-stu-id="56235-196">Configure Windows Server</span></span>

1. <span data-ttu-id="56235-197">如果應用程式是[與架構相依的部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)，請安裝 .NET Core、.NET Framework 或兩者 (如果應用程式是以 .NET Framework 為目標的 .NET Core 應用程式)。</span><span class="sxs-lookup"><span data-stu-id="56235-197">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd), install .NET Core, .NET Framework, or both (if the app is a .NET Core app targeting the .NET Framework).</span></span>

   * <span data-ttu-id="56235-198">**.NET Core** &ndash; 如果應用程式需要 .NET Core，請從 [.NET 下載](https://www.microsoft.com/net/download/windows) \(英文\) 取得並執行 .NET Core 安裝程式。</span><span class="sxs-lookup"><span data-stu-id="56235-198">**.NET Core** &ndash; If the app requires .NET Core, obtain and run the .NET Core installer from [.NET Downloads](https://www.microsoft.com/net/download/windows).</span></span>
   * <span data-ttu-id="56235-199">**.NET Framework** &ndash; 如果應用程式要求 .NET Framework，請參閱 [.NET Framework：安裝指南](/dotnet/framework/install/)以尋找安裝指示。</span><span class="sxs-lookup"><span data-stu-id="56235-199">**.NET Framework** &ndash; If the app requires .NET Framework, see [.NET Framework: Installation guide](/dotnet/framework/install/) to find installation instructions.</span></span> <span data-ttu-id="56235-200">安裝必要的 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="56235-200">Install the required .NET Framework.</span></span> <span data-ttu-id="56235-201">您可在 [.NET 下載](https://www.microsoft.com/net/download/windows) \(英文\) 找到最新的 .NET Framework 安裝程式。</span><span class="sxs-lookup"><span data-stu-id="56235-201">The installer for the latest .NET Framework can be found at [.NET Downloads](https://www.microsoft.com/net/download/windows).</span></span>

1. <span data-ttu-id="56235-202">設定要應用程式的 URL 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="56235-202">Configure URLs and ports for the app.</span></span>

   <span data-ttu-id="56235-203">ASP.NET Core 預設會繫結至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="56235-203">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="56235-204">若要設定 URL 前置詞和連接埠，選項包括使用：</span><span class="sxs-lookup"><span data-stu-id="56235-204">To configure URL prefixes and ports, options include using:</span></span>

   * [<span data-ttu-id="56235-205">UseUrls</span><span class="sxs-lookup"><span data-stu-id="56235-205">UseUrls</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
   * <span data-ttu-id="56235-206">`urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="56235-206">`urls` command-line argument</span></span>
   * <span data-ttu-id="56235-207">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="56235-207">`ASPNETCORE_URLS` environment variable</span></span>
   * [<span data-ttu-id="56235-208">UrlPrefixes</span><span class="sxs-lookup"><span data-stu-id="56235-208">UrlPrefixes</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes)

   <span data-ttu-id="56235-209">下列程式碼範例示範如何使用 [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes)：</span><span class="sxs-lookup"><span data-stu-id="56235-209">The following code example shows how to use [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes):</span></span>

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=11)]

   <span data-ttu-id="56235-210">`UrlPrefixes` 的優點是針對錯誤格式的前置詞會立即產生錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="56235-210">An advantage of `UrlPrefixes` is that an error message is generated immediately for improperly formatted prefixes.</span></span>

   <span data-ttu-id="56235-211">`UrlPrefixes` 中的設定會覆寫 `UseUrls`/`urls`/`ASPNETCORE_URLS` 設定。</span><span class="sxs-lookup"><span data-stu-id="56235-211">The settings in `UrlPrefixes` override `UseUrls`/`urls`/`ASPNETCORE_URLS` settings.</span></span> <span data-ttu-id="56235-212">因此，`UseUrls`、`urls` 和 `ASPNETCORE_URLS` 環境變數的優點，是能更輕鬆地在 Kestrel 和 HTTP.sys 之間切換。</span><span class="sxs-lookup"><span data-stu-id="56235-212">Therefore, an advantage of `UseUrls`, `urls`, and the `ASPNETCORE_URLS` environment variable is that it's easier to switch between Kestrel and HTTP.sys.</span></span> <span data-ttu-id="56235-213">如需 `UseUrls`、`urls` 和 `ASPNETCORE_URLS` 的詳細資訊，請參閱[裝載](xref:fundamentals/hosting)。</span><span class="sxs-lookup"><span data-stu-id="56235-213">For more information on `UseUrls`, `urls`, and `ASPNETCORE_URLS`, see [Hosting](xref:fundamentals/hosting).</span></span>

   <span data-ttu-id="56235-214">HTTP.sys 使用 [HTTP Server API UrlPrefix 字串格式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)。</span><span class="sxs-lookup"><span data-stu-id="56235-214">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

1. <span data-ttu-id="56235-215">預先註冊 URL 前置詞以繫結至 HTTP.sys，然後設定 x.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="56235-215">Preregister URL prefixes to bind to HTTP.sys and set up x.509 certificates.</span></span>

   <span data-ttu-id="56235-216">如果 URL 前置詞並未在 Windows 中預先註冊，請以系統管理員權限執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="56235-216">If URL prefixes aren't preregistered in Windows, run the app with administrator privileges.</span></span> <span data-ttu-id="56235-217">唯一的例外狀況是使用 HTTP (不是 HTTPS) 透過大於 1024 的連接埠號碼繫結至 localhost。</span><span class="sxs-lookup"><span data-stu-id="56235-217">The only exception is when binding to localhost using HTTP (not HTTPS) with a port number greater than 1024.</span></span> <span data-ttu-id="56235-218">在此情況下，不需要系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="56235-218">In that case, administrator privileges aren't required.</span></span>

   1. <span data-ttu-id="56235-219">用來設定 HTTP.sys 的內建工具是 *netsh.exe*。</span><span class="sxs-lookup"><span data-stu-id="56235-219">The built-in tool for configuring HTTP.sys is *netsh.exe*.</span></span> <span data-ttu-id="56235-220">*netsh.exe* 是用來保留 URL 前置詞，並指派 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="56235-220">*netsh.exe* is used to reserve URL prefixes and assign X.509 certificates.</span></span> <span data-ttu-id="56235-221">此工具需要系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="56235-221">The tool requires administrator privileges.</span></span>

      <span data-ttu-id="56235-222">下列範例示範保留連接埠 80 和 443 的 URL 前置詞的命令：</span><span class="sxs-lookup"><span data-stu-id="56235-222">The following example shows the commands to reserve URL prefixes for ports 80 and 443:</span></span>

      ```console
      netsh http add urlacl url=http://+:80/ user=Users
      netsh http add urlacl url=https://+:443/ user=Users
      ```

      <span data-ttu-id="56235-223">下列範例會示範如何指派 X.509 憑證：</span><span class="sxs-lookup"><span data-stu-id="56235-223">The following example shows how to assign an X.509 certificate:</span></span>

      ```console
      netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
      ```

      <span data-ttu-id="56235-224">以下是 *netsh.exe* 的參考文件：</span><span class="sxs-lookup"><span data-stu-id="56235-224">Reference documentation for *netsh.exe*:</span></span>

      * [<span data-ttu-id="56235-225">超文字傳輸通訊協定 (HTTP) 的 Netsh 命令</span><span class="sxs-lookup"><span data-stu-id="56235-225">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
      * [<span data-ttu-id="56235-226">UrlPrefix 字串</span><span class="sxs-lookup"><span data-stu-id="56235-226">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

   1. <span data-ttu-id="56235-227">如有需要，可建立自我簽署的 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="56235-227">Create self-signed X.509 certificates, if required.</span></span>

     [!INCLUDE[How to make an X.509 cert](../../includes/make-x509-cert.md)]

1. <span data-ttu-id="56235-228">開啟防火牆連接埠來允許流量到達 HTTP.sys。</span><span class="sxs-lookup"><span data-stu-id="56235-228">Open firewall ports to allow traffic to reach HTTP.sys.</span></span> <span data-ttu-id="56235-229">使用 *netsh.exe* 或 [PowerShell Cmdlets](https://technet.microsoft.com/library/jj554906)。</span><span class="sxs-lookup"><span data-stu-id="56235-229">Use *netsh.exe* or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="56235-230">其他資源</span><span class="sxs-lookup"><span data-stu-id="56235-230">Additional resources</span></span>

* <span data-ttu-id="56235-231">[HTTP 伺服器 API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="56235-231">[HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)</span></span>
* <span data-ttu-id="56235-232">[aspnet/HttpSysServer GitHub 存放庫 (原始程式碼)](https://github.com/aspnet/HttpSysServer/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="56235-232">[aspnet/HttpSysServer GitHub repository (source code)](https://github.com/aspnet/HttpSysServer/)</span></span>
* [<span data-ttu-id="56235-233">裝載</span><span class="sxs-lookup"><span data-stu-id="56235-233">Hosting</span></span>](xref:fundamentals/hosting)
