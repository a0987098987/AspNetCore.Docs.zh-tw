---
title: ASP.NET Core 中的 HTTP.sys 網頁伺服器實作
author: guardrex
description: 深入了解 HTTP.sys，這是 Windows 上的 ASP.NET Core 網頁伺服器。 HTTP.sys 建置在 HTTP.sys 核心模式驅動程式之上，是 Kestrel 的替代方式，可以用來直接連線到網際網路而不使用 IIS。
monikerRange: '>= aspnetcore-2.0'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/13/2018
uid: fundamentals/servers/httpsys
ms.openlocfilehash: e845cb4eb7fe805e3d2195124073f7ab646d66cb
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011663"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="ef768-104">ASP.NET Core 中的 HTTP.sys 網頁伺服器實作</span><span class="sxs-lookup"><span data-stu-id="ef768-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="ef768-105">作者：[Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ef768-105">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Luke Latham](https://github.com/guardrex)</span></span>

> [!NOTE]
> <span data-ttu-id="ef768-106">本主題適用於 ASP.NET Core 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="ef768-106">This topic applies to ASP.NET Core 2.0 or later.</span></span> <span data-ttu-id="ef768-107">在舊版的 ASP.NET Core 中，HTTP.sys 名為 [WebListener](xref:fundamentals/servers/weblistener)。</span><span class="sxs-lookup"><span data-stu-id="ef768-107">In earlier versions of ASP.NET Core, HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener).</span></span>

<span data-ttu-id="ef768-108">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) 是只在 Windows 上執行的 [ASP.NET Core 網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="ef768-108">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) is a [web server for ASP.NET Core](xref:fundamentals/servers/index) that only runs on Windows.</span></span> <span data-ttu-id="ef768-109">HTTP.sys 是 [Kestrel](xref:fundamentals/servers/kestrel) 的替代方式，提供了一些 Kestrel 未提供的功能。</span><span class="sxs-lookup"><span data-stu-id="ef768-109">HTTP.sys is an alternative to [Kestrel](xref:fundamentals/servers/kestrel) and offers some features that Kestrel doesn't provide.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ef768-110">HTTP.sys 與 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)不相容，且不能與 IIS 或 IIS Express 搭配使用。</span><span class="sxs-lookup"><span data-stu-id="ef768-110">HTTP.sys is incompatible with the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and can't be used with IIS or IIS Express.</span></span>

<span data-ttu-id="ef768-111">HTTP.sys 支援下列功能：</span><span class="sxs-lookup"><span data-stu-id="ef768-111">HTTP.sys supports the following features:</span></span>

* [<span data-ttu-id="ef768-112">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="ef768-112">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
* <span data-ttu-id="ef768-113">連接埠共用</span><span class="sxs-lookup"><span data-stu-id="ef768-113">Port sharing</span></span>
* <span data-ttu-id="ef768-114">使用 SNI 的 HTTPS</span><span class="sxs-lookup"><span data-stu-id="ef768-114">HTTPS with SNI</span></span>
* <span data-ttu-id="ef768-115">透過 TLS 的 HTTP/2 (Windows 10 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="ef768-115">HTTP/2 over TLS (Windows 10 or later)</span></span>
* <span data-ttu-id="ef768-116">直接檔案傳輸</span><span class="sxs-lookup"><span data-stu-id="ef768-116">Direct file transmission</span></span>
* <span data-ttu-id="ef768-117">回應快取</span><span class="sxs-lookup"><span data-stu-id="ef768-117">Response caching</span></span>
* <span data-ttu-id="ef768-118">WebSocket (Windows 8 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="ef768-118">WebSockets (Windows 8 or later)</span></span>

<span data-ttu-id="ef768-119">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="ef768-119">Supported Windows versions:</span></span>

* <span data-ttu-id="ef768-120">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="ef768-120">Windows 7 or later</span></span>
* <span data-ttu-id="ef768-121">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="ef768-121">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="ef768-122">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ef768-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="ef768-123">使用 HTTP.sys 的時機</span><span class="sxs-lookup"><span data-stu-id="ef768-123">When to use HTTP.sys</span></span>

<span data-ttu-id="ef768-124">HTTP.sys 在下列部署環境中非常有用：</span><span class="sxs-lookup"><span data-stu-id="ef768-124">HTTP.sys is useful for deployments where:</span></span>

* <span data-ttu-id="ef768-125">需要直接向網際網路公開伺服器而不使用 IIS。</span><span class="sxs-lookup"><span data-stu-id="ef768-125">There's a need to expose the server directly to the Internet without using IIS.</span></span>

  ![HTTP.sys 直接與網際網路通訊](httpsys/_static/httpsys-to-internet.png)

* <span data-ttu-id="ef768-127">內部部署需要的功能無法在 Kestrel 中使用，例如 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="ef768-127">An internal deployment requires a feature not available in Kestrel, such as [Windows Authentication](xref:security/authentication/windowsauth).</span></span>

  ![HTTP.sys 直接與內部網路通訊](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="ef768-129">HTTP.sys 是成熟的技術，可抵禦許多種類的攻擊，並提供功能完整之網頁伺服器的穩固性、安全性及延展性。</span><span class="sxs-lookup"><span data-stu-id="ef768-129">HTTP.sys is mature technology that protects against many types of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="ef768-130">IIS 本身在 HTTP.sys 之上以 HTTP 接聽程式的形式執行。</span><span class="sxs-lookup"><span data-stu-id="ef768-130">IIS itself runs as an HTTP listener on top of HTTP.sys.</span></span>

## <a name="http2-support"></a><span data-ttu-id="ef768-131">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="ef768-131">HTTP/2 support</span></span>

<span data-ttu-id="ef768-132">如果符合下列基本需求，則可以針對 ASP.NET Core 應用程式啟用 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="ef768-132">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is enabled for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="ef768-133">Windows Server 2016/Windows 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="ef768-133">Windows Server 2016/Windows 10 or later</span></span>
* <span data-ttu-id="ef768-134">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 連線</span><span class="sxs-lookup"><span data-stu-id="ef768-134">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="ef768-135">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="ef768-135">TLS 1.2 or later connection</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ef768-136">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="ef768-136">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ef768-137">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/1.1`。</span><span class="sxs-lookup"><span data-stu-id="ef768-137">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="ef768-138">HTTP/2 預設為啟用。</span><span class="sxs-lookup"><span data-stu-id="ef768-138">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="ef768-139">如果 HTTP/2 連線尚未建立，連線會退為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="ef768-139">If an HTTP/2 connection isn't established, the connection falls back to HTTP/1.1.</span></span> <span data-ttu-id="ef768-140">Windows 的未來版本會推出 HTTP/2 設定旗標，包括使用 HTTP.sys 來停用 HTTP/2 的功能。</span><span class="sxs-lookup"><span data-stu-id="ef768-140">In a future release of Windows, HTTP/2 configuration flags will be available, including the ability to disable HTTP/2 with HTTP.sys.</span></span>

## <a name="kernel-mode-authentication-with-kerberos"></a><span data-ttu-id="ef768-141">使用 Kerberos 的核心模式驗證</span><span class="sxs-lookup"><span data-stu-id="ef768-141">Kernel mode authentication with Kerberos</span></span>

<span data-ttu-id="ef768-142">HTTP.sys 使用 Kerberos 驗證通訊協定委派給核心模式驗證。</span><span class="sxs-lookup"><span data-stu-id="ef768-142">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="ef768-143">Kerberos 和 HTTP.sys 不支援使用者模式驗證。</span><span class="sxs-lookup"><span data-stu-id="ef768-143">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="ef768-144">必須使用電腦帳戶來解密 Kerberos 權杖/票證，該權杖/票證取自 Active Directory，並由用戶端將其轉送至伺服器來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="ef768-144">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="ef768-145">請註冊主機的服務主體名稱 (SPN)，而非應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="ef768-145">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

## <a name="how-to-use-httpsys"></a><span data-ttu-id="ef768-146">如何使用 HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ef768-146">How to use HTTP.sys</span></span>

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a><span data-ttu-id="ef768-147">設定 ASP.NET Core 應用程式使用 HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ef768-147">Configure the ASP.NET Core app to use HTTP.sys</span></span>

1. <span data-ttu-id="ef768-148">使用 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (ASP.NET Core 2.1 或更新版本) 時，專案檔中不需要套件參考。</span><span class="sxs-lookup"><span data-stu-id="ef768-148">A package reference in the project file isn't required when using the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="ef768-149">若不是使用 `Microsoft.AspNetCore.App` 中繼套件，請將套件參考加入 [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/)。</span><span class="sxs-lookup"><span data-stu-id="ef768-149">When not using the `Microsoft.AspNetCore.App` metapackage, add a package reference to [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).</span></span>

2. <span data-ttu-id="ef768-150">建置 Web 主機時，呼叫 [UseHttpSys](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderhttpsysextensions.usehttpsys) 擴充方法，並指定任何必要的 [HTTP.sys 選項](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions)：</span><span class="sxs-lookup"><span data-stu-id="ef768-150">Call the [UseHttpSys](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderhttpsysextensions.usehttpsys) extension method when building the web host, specifying any required [HTTP.sys options](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions):</span></span>

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   <span data-ttu-id="ef768-151">其他的 HTTP.sys 設定則透過[登錄設定](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows)處理。</span><span class="sxs-lookup"><span data-stu-id="ef768-151">Additional HTTP.sys configuration is handled through [registry settings](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).</span></span>

   <span data-ttu-id="ef768-152">**HTTP.sys 選項**</span><span class="sxs-lookup"><span data-stu-id="ef768-152">**HTTP.sys options**</span></span>

   | <span data-ttu-id="ef768-153">屬性</span><span class="sxs-lookup"><span data-stu-id="ef768-153">Property</span></span> | <span data-ttu-id="ef768-154">描述</span><span class="sxs-lookup"><span data-stu-id="ef768-154">Description</span></span> | <span data-ttu-id="ef768-155">預設</span><span class="sxs-lookup"><span data-stu-id="ef768-155">Default</span></span> |
   | -------- | ----------- | :-----: |
   | [<span data-ttu-id="ef768-156">AllowSynchronousIO</span><span class="sxs-lookup"><span data-stu-id="ef768-156">AllowSynchronousIO</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.allowsynchronousio) | <span data-ttu-id="ef768-157">控制是否允許 `HttpContext.Request.Body` 和 `HttpContext.Response.Body` 同步輸出/輸入。</span><span class="sxs-lookup"><span data-stu-id="ef768-157">Control whether synchronous input/output is allowed for the `HttpContext.Request.Body` and `HttpContext.Response.Body`.</span></span> | `true` |
   | [<span data-ttu-id="ef768-158">Authentication.AllowAnonymous</span><span class="sxs-lookup"><span data-stu-id="ef768-158">Authentication.AllowAnonymous</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.allowanonymous) | <span data-ttu-id="ef768-159">允許匿名要求。</span><span class="sxs-lookup"><span data-stu-id="ef768-159">Allow anonymous requests.</span></span> | `true` |
   | [<span data-ttu-id="ef768-160">Authentication.Schemes</span><span class="sxs-lookup"><span data-stu-id="ef768-160">Authentication.Schemes</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.schemes) | <span data-ttu-id="ef768-161">指定允許的驗證配置。</span><span class="sxs-lookup"><span data-stu-id="ef768-161">Specify the allowed authentication schemes.</span></span> <span data-ttu-id="ef768-162">處置接聽程式之前可隨時修改。</span><span class="sxs-lookup"><span data-stu-id="ef768-162">May be modified at any time prior to disposing the listener.</span></span> <span data-ttu-id="ef768-163">值是由 [AuthenticationSchemes enum](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationschemes) 提供：`Basic`、`Kerberos`、`Negotiate`、`None` 和 `NTLM`。</span><span class="sxs-lookup"><span data-stu-id="ef768-163">Values are provided by the [AuthenticationSchemes enum](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationschemes): `Basic`, `Kerberos`, `Negotiate`, `None`, and `NTLM`.</span></span> | `None` |
   | [<span data-ttu-id="ef768-164">EnableResponseCaching</span><span class="sxs-lookup"><span data-stu-id="ef768-164">EnableResponseCaching</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.enableresponsecaching) | <span data-ttu-id="ef768-165">針對含有合格標頭的回應嘗試[核心模式](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode)快取。</span><span class="sxs-lookup"><span data-stu-id="ef768-165">Attempt [kernel-mode](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) caching for responses with eligible headers.</span></span> <span data-ttu-id="ef768-166">回應可能不包含 `Set-Cookie`、`Vary` 或 `Pragma` 標頭。</span><span class="sxs-lookup"><span data-stu-id="ef768-166">The response may not include `Set-Cookie`, `Vary`, or `Pragma` headers.</span></span> <span data-ttu-id="ef768-167">它必須包含為 `public` 的 `Cache-Control` 標頭，且有 `shared-max-age` 或 `max-age` 值，或是 `Expires` 標頭。</span><span class="sxs-lookup"><span data-stu-id="ef768-167">It must include a `Cache-Control` header that's `public` and either a `shared-max-age` or `max-age` value, or an `Expires` header.</span></span> | `true` |
   | [<span data-ttu-id="ef768-168">MaxAccepts</span><span class="sxs-lookup"><span data-stu-id="ef768-168">MaxAccepts</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxaccepts) | <span data-ttu-id="ef768-169">可同時接受的數目上限。</span><span class="sxs-lookup"><span data-stu-id="ef768-169">The maximum number of concurrent accepts.</span></span> | <span data-ttu-id="ef768-170">5 &times; [Environment.<br>ProcessorCount](/dotnet/api/system.environment.processorcount)</span><span class="sxs-lookup"><span data-stu-id="ef768-170">5 &times; [Environment.<br>ProcessorCount](/dotnet/api/system.environment.processorcount)</span></span> |
   | [<span data-ttu-id="ef768-171">MaxConnections</span><span class="sxs-lookup"><span data-stu-id="ef768-171">MaxConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxconnections) | <span data-ttu-id="ef768-172">可接受的同時連線數量上限。</span><span class="sxs-lookup"><span data-stu-id="ef768-172">The maximum number of concurrent connections to accept.</span></span> <span data-ttu-id="ef768-173">使用 `-1` 為無限多個。</span><span class="sxs-lookup"><span data-stu-id="ef768-173">Use `-1` for infinite.</span></span> <span data-ttu-id="ef768-174">使用 `null` 以使用登錄之整個電腦的設定。</span><span class="sxs-lookup"><span data-stu-id="ef768-174">Use `null` to use the registry's machine-wide setting.</span></span> | `null`<br><span data-ttu-id="ef768-175">(無限制)</span><span class="sxs-lookup"><span data-stu-id="ef768-175">(unlimited)</span></span> |
   | [<span data-ttu-id="ef768-176">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="ef768-176">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) | <span data-ttu-id="ef768-177">請參閱 <a href="#maxrequestbodysize">MaxRequestBodySize</a> 小節。</span><span class="sxs-lookup"><span data-stu-id="ef768-177">See the <a href="#maxrequestbodysize">MaxRequestBodySize</a> section.</span></span> | <span data-ttu-id="ef768-178">30000000 位元組</span><span class="sxs-lookup"><span data-stu-id="ef768-178">30000000 bytes</span></span><br><span data-ttu-id="ef768-179">(~28.6 MB)</span><span class="sxs-lookup"><span data-stu-id="ef768-179">(~28.6 MB)</span></span> |
   | [<span data-ttu-id="ef768-180">RequestQueueLimit</span><span class="sxs-lookup"><span data-stu-id="ef768-180">RequestQueueLimit</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.requestqueuelimit) | <span data-ttu-id="ef768-181">可以加入佇列的最大要求數目。</span><span class="sxs-lookup"><span data-stu-id="ef768-181">The maximum number of requests that can be queued.</span></span> | <span data-ttu-id="ef768-182">1000</span><span class="sxs-lookup"><span data-stu-id="ef768-182">1000</span></span> |
   | [<span data-ttu-id="ef768-183">ThrowWriteExceptions</span><span class="sxs-lookup"><span data-stu-id="ef768-183">ThrowWriteExceptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.throwwriteexceptions) | <span data-ttu-id="ef768-184">指出若回應本文因為用戶端中斷連線而寫入失敗時，應擲回例外狀況或正常完成。</span><span class="sxs-lookup"><span data-stu-id="ef768-184">Indicate if response body writes that fail due to client disconnects should throw exceptions or complete normally.</span></span> | `false`<br><span data-ttu-id="ef768-185">(正常完成)</span><span class="sxs-lookup"><span data-stu-id="ef768-185">(complete normally)</span></span> |
   | [<span data-ttu-id="ef768-186">Timeouts</span><span class="sxs-lookup"><span data-stu-id="ef768-186">Timeouts</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts) | <span data-ttu-id="ef768-187">公開 HTTP.sys [TimeoutManager](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager) 設定，這也可在登錄中設定。</span><span class="sxs-lookup"><span data-stu-id="ef768-187">Expose the HTTP.sys [TimeoutManager](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager) configuration, which may also be configured in the registry.</span></span> <span data-ttu-id="ef768-188">API 連結可提供包括預設值在內每個設定的詳細資訊：</span><span class="sxs-lookup"><span data-stu-id="ef768-188">Follow the API links to learn more about each setting, including default values:</span></span><ul><li><span data-ttu-id="ef768-189">[TimeoutManager.DrainEntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.drainentitybody) &ndash; HTTP 伺服器 API 清空持續連線上實體內容的允許時間。</span><span class="sxs-lookup"><span data-stu-id="ef768-189">[TimeoutManager.DrainEntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.drainentitybody) &ndash; Time allowed for the HTTP Server API to drain the entity body on a Keep-Alive connection.</span></span></li><li><span data-ttu-id="ef768-190">[TimeoutManager.EntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.entitybody) &ndash; 要求實體內容到達的允許時間。</span><span class="sxs-lookup"><span data-stu-id="ef768-190">[TimeoutManager.EntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.entitybody) &ndash; Time allowed for the request entity body to arrive.</span></span></li><li><span data-ttu-id="ef768-191">[TimeoutManager.HeaderWait](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.headerwait) &ndash; HTTP 伺服器 API 剖析要求標頭的允許時間。</span><span class="sxs-lookup"><span data-stu-id="ef768-191">[TimeoutManager.HeaderWait](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.headerwait) &ndash; Time allowed for the HTTP Server API to parse the request header.</span></span></li><li><span data-ttu-id="ef768-192">[TimeoutManager.IdleConnection](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.idleconnection) &ndash; 允許連線閒置的時間。</span><span class="sxs-lookup"><span data-stu-id="ef768-192">[TimeoutManager.IdleConnection](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.idleconnection) &ndash; Time allowed for an idle connection.</span></span></li><li><span data-ttu-id="ef768-193">[TimeoutManager.MinSendBytesPerSecond](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.minsendbytespersecond) &ndash; 回應的最低傳送速率。</span><span class="sxs-lookup"><span data-stu-id="ef768-193">[TimeoutManager.MinSendBytesPerSecond](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.minsendbytespersecond) &ndash; The minimum send rate for the response.</span></span></li><li><span data-ttu-id="ef768-194">[TimeoutManager.RequestQueue](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.requestqueue) &ndash; 在應用程式 擷取要求之前，將要求保留於要求佇列中的允許時間。</span><span class="sxs-lookup"><span data-stu-id="ef768-194">[TimeoutManager.RequestQueue](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.requestqueue) &ndash; Time allowed for the request to remain in the request queue before the app picks it up.</span></span></li></ul> |  |
   | [<span data-ttu-id="ef768-195">UrlPrefixes</span><span class="sxs-lookup"><span data-stu-id="ef768-195">UrlPrefixes</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes) | <span data-ttu-id="ef768-196">指定 [UrlPrefixCollection](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection) 以向 HTTP.sys 註冊。</span><span class="sxs-lookup"><span data-stu-id="ef768-196">Specify the [UrlPrefixCollection](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection) to register with HTTP.sys.</span></span> <span data-ttu-id="ef768-197">最實用的是 [UrlPrefixCollection.Add](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection.add)，可用來將前置詞加入集合。</span><span class="sxs-lookup"><span data-stu-id="ef768-197">The most useful is [UrlPrefixCollection.Add](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection.add), which is used to add a prefix to the collection.</span></span> <span data-ttu-id="ef768-198">處置接聽程式之前可隨時修改這些內容。</span><span class="sxs-lookup"><span data-stu-id="ef768-198">These may be modified at any time prior to disposing the listener.</span></span> |  |

   <a name="maxrequestbodysize"></a>
   <span data-ttu-id="ef768-199">**MaxRequestBodySize**</span><span class="sxs-lookup"><span data-stu-id="ef768-199">**MaxRequestBodySize**</span></span>

   <span data-ttu-id="ef768-200">任何要求所允許的大小上限 (以位元組為單位)。</span><span class="sxs-lookup"><span data-stu-id="ef768-200">The maximum allowed size of any request body in bytes.</span></span> <span data-ttu-id="ef768-201">當設定為 `null` 時，要求主體大小上限為無限制。</span><span class="sxs-lookup"><span data-stu-id="ef768-201">When set to `null`, the maximum request body size is unlimited.</span></span> <span data-ttu-id="ef768-202">此限制對升級連線不會有任何影響，因為其一律為無限制。</span><span class="sxs-lookup"><span data-stu-id="ef768-202">This limit has no effect on upgraded connections, which are always unlimited.</span></span>

   <span data-ttu-id="ef768-203">在 ASP.NET Core MVC 應用程式中針對單一 `IActionResult` 覆寫限制的建議方法，是在動作方法上使用 [RequestSizeLimitAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) 屬性：</span><span class="sxs-lookup"><span data-stu-id="ef768-203">The recommended method to override the limit in an ASP.NET Core MVC app for a single `IActionResult` is to use the [RequestSizeLimitAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>

   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   <span data-ttu-id="ef768-204">如果應用程式已開始讀取要求之後，才嘗試設定要求的限制，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ef768-204">An exception is thrown if the app attempts to configure the limit on a request after the app has started reading the request.</span></span> <span data-ttu-id="ef768-205">`IsReadOnly` 屬性可用來指出 `MaxRequestBodySize` 屬性是否處於唯讀狀態，代表要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="ef768-205">An `IsReadOnly` property can be used to indicate if the `MaxRequestBodySize` property is in a read-only state, meaning it's too late to configure the limit.</span></span>

   <span data-ttu-id="ef768-206">如果應用程式應針對每個要求覆寫 [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize)，則使用 [IHttpMaxRequestBodySizeFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpmaxrequestbodysizefeature)：</span><span class="sxs-lookup"><span data-stu-id="ef768-206">If the app should override [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) per-request, use the [IHttpMaxRequestBodySizeFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpmaxrequestbodysizefeature):</span></span>

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

3. <span data-ttu-id="ef768-207">如果您使用 Visual Studio，請確定應用程式未設定為執行 IIS 或 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="ef768-207">If using Visual Studio, make sure the app isn't configured to run IIS or IIS Express.</span></span>

   <span data-ttu-id="ef768-208">在 Visual Studio 中，預設啟動設定檔適用於 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="ef768-208">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="ef768-209">若要執行專案作為主控台應用程式，請手動變更選取的設定檔，如下列螢幕擷取畫面所示：</span><span class="sxs-lookup"><span data-stu-id="ef768-209">To run the project as a console app, manually change the selected profile, as shown in the following screen shot:</span></span>

   ![選取主控台應用程式設定檔](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a><span data-ttu-id="ef768-211">設定 Windows Server</span><span class="sxs-lookup"><span data-stu-id="ef768-211">Configure Windows Server</span></span>

1. <span data-ttu-id="ef768-212">如果應用程式是[與架構相依的部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)，請安裝 .NET Core、.NET Framework 或兩者 (如果應用程式是以 .NET Framework 為目標的 .NET Core 應用程式)。</span><span class="sxs-lookup"><span data-stu-id="ef768-212">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd), install .NET Core, .NET Framework, or both (if the app is a .NET Core app targeting the .NET Framework).</span></span>

   * <span data-ttu-id="ef768-213">**.NET Core** &ndash; 如果應用程式需要 .NET Core，請從 [.NET All Downloads](https://www.microsoft.com/net/download/all) (.NET 所有下載) 取得並執行 .NET Core 安裝程式。</span><span class="sxs-lookup"><span data-stu-id="ef768-213">**.NET Core** &ndash; If the app requires .NET Core, obtain and run the .NET Core installer from [.NET All Downloads](https://www.microsoft.com/net/download/all).</span></span>
   * <span data-ttu-id="ef768-214">**.NET Framework** &ndash; 如果應用程式要求 .NET Framework，請參閱 [.NET Framework：安裝指南](/dotnet/framework/install/)以尋找安裝指示。</span><span class="sxs-lookup"><span data-stu-id="ef768-214">**.NET Framework** &ndash; If the app requires .NET Framework, see [.NET Framework: Installation guide](/dotnet/framework/install/) to find installation instructions.</span></span> <span data-ttu-id="ef768-215">安裝必要的 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="ef768-215">Install the required .NET Framework.</span></span> <span data-ttu-id="ef768-216">您可在 [.NET All Downloads](https://www.microsoft.com/net/download/all) (.NET 所有下載) 找到最新的 .NET Framework 安裝程式。</span><span class="sxs-lookup"><span data-stu-id="ef768-216">The installer for the latest .NET Framework can be found at [.NET All Downloads](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="ef768-217">設定要應用程式的 URL 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="ef768-217">Configure URLs and ports for the app.</span></span>

   <span data-ttu-id="ef768-218">ASP.NET Core 預設會繫結至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="ef768-218">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="ef768-219">若要設定 URL 前置詞和連接埠，選項包括使用：</span><span class="sxs-lookup"><span data-stu-id="ef768-219">To configure URL prefixes and ports, options include using:</span></span>

   * [<span data-ttu-id="ef768-220">UseUrls</span><span class="sxs-lookup"><span data-stu-id="ef768-220">UseUrls</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
   * <span data-ttu-id="ef768-221">`urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="ef768-221">`urls` command-line argument</span></span>
   * <span data-ttu-id="ef768-222">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="ef768-222">`ASPNETCORE_URLS` environment variable</span></span>
   * [<span data-ttu-id="ef768-223">UrlPrefixes</span><span class="sxs-lookup"><span data-stu-id="ef768-223">UrlPrefixes</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes)

   <span data-ttu-id="ef768-224">下列程式碼範例示範如何使用 [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes)：</span><span class="sxs-lookup"><span data-stu-id="ef768-224">The following code example shows how to use [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes):</span></span>

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=11)]

   <span data-ttu-id="ef768-225">`UrlPrefixes` 的優點是針對錯誤格式的前置詞會立即產生錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="ef768-225">An advantage of `UrlPrefixes` is that an error message is generated immediately for improperly formatted prefixes.</span></span>

   <span data-ttu-id="ef768-226">`UrlPrefixes` 中的設定會覆寫 `UseUrls`/`urls`/`ASPNETCORE_URLS` 設定。</span><span class="sxs-lookup"><span data-stu-id="ef768-226">The settings in `UrlPrefixes` override `UseUrls`/`urls`/`ASPNETCORE_URLS` settings.</span></span> <span data-ttu-id="ef768-227">因此，`UseUrls`、`urls` 和 `ASPNETCORE_URLS` 環境變數的優點，是能更輕鬆地在 Kestrel 和 HTTP.sys 之間切換。</span><span class="sxs-lookup"><span data-stu-id="ef768-227">Therefore, an advantage of `UseUrls`, `urls`, and the `ASPNETCORE_URLS` environment variable is that it's easier to switch between Kestrel and HTTP.sys.</span></span> <span data-ttu-id="ef768-228">如需 `UseUrls`、`urls` 和 `ASPNETCORE_URLS` 的詳細資訊，請參閱[在 ASP.NET Core 中代管](xref:fundamentals/host/index)主題。</span><span class="sxs-lookup"><span data-stu-id="ef768-228">For more information on `UseUrls`, `urls`, and `ASPNETCORE_URLS`, see the [Host in ASP.NET Core](xref:fundamentals/host/index) topic.</span></span>

   <span data-ttu-id="ef768-229">HTTP.sys 使用 [HTTP Server API UrlPrefix 字串格式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)。</span><span class="sxs-lookup"><span data-stu-id="ef768-229">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

   > [!WARNING]
   > <span data-ttu-id="ef768-230">請**勿**使用最上層萬用字元繫結 (`http://*:80/`與 `http://+:80`)。</span><span class="sxs-lookup"><span data-stu-id="ef768-230">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="ef768-231">最上層萬用字元繫結可能暴露您的應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="ef768-231">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="ef768-232">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="ef768-232">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="ef768-233">請使用明確主機名稱，而非萬用字元。</span><span class="sxs-lookup"><span data-stu-id="ef768-233">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="ef768-234">若您擁有整個父網域 (與具弱點的 `*.com` 相對) 的控制權，則子網域萬用字元繫結 (例如 `*.mysub.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="ef768-234">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="ef768-235">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="ef768-235">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

3. <span data-ttu-id="ef768-236">預先註冊 URL 前置詞以繫結至 HTTP.sys，然後設定 x.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="ef768-236">Preregister URL prefixes to bind to HTTP.sys and set up x.509 certificates.</span></span>

   <span data-ttu-id="ef768-237">如果 URL 前置詞並未在 Windows 中預先註冊，請以系統管理員權限執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="ef768-237">If URL prefixes aren't preregistered in Windows, run the app with administrator privileges.</span></span> <span data-ttu-id="ef768-238">唯一的例外狀況是使用 HTTP (不是 HTTPS) 透過大於 1024 的連接埠號碼繫結至 localhost。</span><span class="sxs-lookup"><span data-stu-id="ef768-238">The only exception is when binding to localhost using HTTP (not HTTPS) with a port number greater than 1024.</span></span> <span data-ttu-id="ef768-239">在此情況下，不需要系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="ef768-239">In that case, administrator privileges aren't required.</span></span>

   1. <span data-ttu-id="ef768-240">用來設定 HTTP.sys 的內建工具是 *netsh.exe*。</span><span class="sxs-lookup"><span data-stu-id="ef768-240">The built-in tool for configuring HTTP.sys is *netsh.exe*.</span></span> <span data-ttu-id="ef768-241">*netsh.exe* 是用來保留 URL 前置詞，並指派 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="ef768-241">*netsh.exe* is used to reserve URL prefixes and assign X.509 certificates.</span></span> <span data-ttu-id="ef768-242">此工具需要系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="ef768-242">The tool requires administrator privileges.</span></span>

      <span data-ttu-id="ef768-243">下列範例示範保留連接埠 80 和 443 的 URL 前置詞的命令：</span><span class="sxs-lookup"><span data-stu-id="ef768-243">The following example shows the commands to reserve URL prefixes for ports 80 and 443:</span></span>

      ```console
      netsh http add urlacl url=http://+:80/ user=Users
      netsh http add urlacl url=https://+:443/ user=Users
      ```

      <span data-ttu-id="ef768-244">下列範例會示範如何指派 X.509 憑證：</span><span class="sxs-lookup"><span data-stu-id="ef768-244">The following example shows how to assign an X.509 certificate:</span></span>

      ```console
      netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}"
      ```

      <span data-ttu-id="ef768-245">以下是 *netsh.exe* 的參考文件：</span><span class="sxs-lookup"><span data-stu-id="ef768-245">Reference documentation for *netsh.exe*:</span></span>

      * [<span data-ttu-id="ef768-246">超文字傳輸通訊協定 (HTTP) 的 Netsh 命令</span><span class="sxs-lookup"><span data-stu-id="ef768-246">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
      * [<span data-ttu-id="ef768-247">UrlPrefix 字串</span><span class="sxs-lookup"><span data-stu-id="ef768-247">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

   2. <span data-ttu-id="ef768-248">如有需要，可建立自我簽署的 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="ef768-248">Create self-signed X.509 certificates, if required.</span></span>

      [!INCLUDE [How to make an X.509 cert](../../includes/make-x509-cert.md)]

4. <span data-ttu-id="ef768-249">開啟防火牆連接埠來允許流量到達 HTTP.sys。</span><span class="sxs-lookup"><span data-stu-id="ef768-249">Open firewall ports to allow traffic to reach HTTP.sys.</span></span> <span data-ttu-id="ef768-250">使用 *netsh.exe* 或 [PowerShell Cmdlets](https://technet.microsoft.com/library/jj554906)。</span><span class="sxs-lookup"><span data-stu-id="ef768-250">Use *netsh.exe* or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="ef768-251">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="ef768-251">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="ef768-252">如果是 HTTP.sys 所裝載且與來自網際網路或公司網路的要求進行互動的應用程式，在裝載於 Proxy 伺服器和負載平衡器後方時，可能需要額外的組態。</span><span class="sxs-lookup"><span data-stu-id="ef768-252">For apps hosted by HTTP.sys that interact with requests from the Internet or a corporate network, additional configuration might be required when hosting behind proxy servers and load balancers.</span></span> <span data-ttu-id="ef768-253">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="ef768-253">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ef768-254">其他資源</span><span class="sxs-lookup"><span data-stu-id="ef768-254">Additional resources</span></span>

* <span data-ttu-id="ef768-255">[HTTP 伺服器 API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="ef768-255">[HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)</span></span>
* <span data-ttu-id="ef768-256">[aspnet/HttpSysServer GitHub 存放庫 (原始程式碼)](https://github.com/aspnet/HttpSysServer/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="ef768-256">[aspnet/HttpSysServer GitHub repository (source code)](https://github.com/aspnet/HttpSysServer/)</span></span>
* [<span data-ttu-id="ef768-257">在 ASP.NET Core 中代管</span><span class="sxs-lookup"><span data-stu-id="ef768-257">Host in ASP.NET Core</span></span>](xref:fundamentals/host/index)
