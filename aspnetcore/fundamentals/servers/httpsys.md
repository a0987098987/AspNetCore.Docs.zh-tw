---
title: ASP.NET Core 中的 HTTP.sys 網頁伺服器實作
author: guardrex
description: 深入了解 HTTP.sys，這是 Windows 上的 ASP.NET Core 網頁伺服器。 HTTP.sys 建置在 HTTP.sys 核心模式驅動程式之上，是 Kestrel 的替代方式，可以用來直接連線到網際網路而不使用 IIS。
monikerRange: '>= aspnetcore-2.0'
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/21/2019
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 7ba27b404cd10752ff9e304cd0a272eff7fa627a
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087049"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="94444-104">ASP.NET Core 中的 HTTP.sys 網頁伺服器實作</span><span class="sxs-lookup"><span data-stu-id="94444-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="94444-105">作者：[Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="94444-105">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="94444-106">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) 是只在 Windows 上執行的 [ASP.NET Core 網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="94444-106">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) is a [web server for ASP.NET Core](xref:fundamentals/servers/index) that only runs on Windows.</span></span> <span data-ttu-id="94444-107">HTTP.sys 是 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器的替代方式，提供了一些 Kestrel 未提供的功能。</span><span class="sxs-lookup"><span data-stu-id="94444-107">HTTP.sys is an alternative to [Kestrel](xref:fundamentals/servers/kestrel) server and offers some features that Kestrel doesn't provide.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="94444-108">HTTP.sys 與 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)不相容，且不能與 IIS 或 IIS Express 搭配使用。</span><span class="sxs-lookup"><span data-stu-id="94444-108">HTTP.sys isn't compatible with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and can't be used with IIS or IIS Express.</span></span>

<span data-ttu-id="94444-109">HTTP.sys 支援下列功能：</span><span class="sxs-lookup"><span data-stu-id="94444-109">HTTP.sys supports the following features:</span></span>

* [<span data-ttu-id="94444-110">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="94444-110">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
* <span data-ttu-id="94444-111">連接埠共用</span><span class="sxs-lookup"><span data-stu-id="94444-111">Port sharing</span></span>
* <span data-ttu-id="94444-112">使用 SNI 的 HTTPS</span><span class="sxs-lookup"><span data-stu-id="94444-112">HTTPS with SNI</span></span>
* <span data-ttu-id="94444-113">透過 TLS 的 HTTP/2 (Windows 10 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="94444-113">HTTP/2 over TLS (Windows 10 or later)</span></span>
* <span data-ttu-id="94444-114">直接檔案傳輸</span><span class="sxs-lookup"><span data-stu-id="94444-114">Direct file transmission</span></span>
* <span data-ttu-id="94444-115">回應快取</span><span class="sxs-lookup"><span data-stu-id="94444-115">Response caching</span></span>
* <span data-ttu-id="94444-116">WebSocket (Windows 8 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="94444-116">WebSockets (Windows 8 or later)</span></span>

<span data-ttu-id="94444-117">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="94444-117">Supported Windows versions:</span></span>

* <span data-ttu-id="94444-118">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="94444-118">Windows 7 or later</span></span>
* <span data-ttu-id="94444-119">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="94444-119">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="94444-120">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="94444-120">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="94444-121">使用 HTTP.sys 的時機</span><span class="sxs-lookup"><span data-stu-id="94444-121">When to use HTTP.sys</span></span>

<span data-ttu-id="94444-122">HTTP.sys 在下列部署環境中非常有用：</span><span class="sxs-lookup"><span data-stu-id="94444-122">HTTP.sys is useful for deployments where:</span></span>

* <span data-ttu-id="94444-123">需要直接向網際網路公開伺服器而不使用 IIS。</span><span class="sxs-lookup"><span data-stu-id="94444-123">There's a need to expose the server directly to the Internet without using IIS.</span></span>

  ![HTTP.sys 直接與網際網路通訊](httpsys/_static/httpsys-to-internet.png)

* <span data-ttu-id="94444-125">內部部署需要的功能無法在 Kestrel 中使用，例如 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="94444-125">An internal deployment requires a feature not available in Kestrel, such as [Windows Authentication](xref:security/authentication/windowsauth).</span></span>

  ![HTTP.sys 直接與內部網路通訊](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="94444-127">HTTP.sys 是成熟的技術，可抵禦許多種類的攻擊，並提供功能完整之網頁伺服器的穩固性、安全性及延展性。</span><span class="sxs-lookup"><span data-stu-id="94444-127">HTTP.sys is mature technology that protects against many types of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="94444-128">IIS 本身在 HTTP.sys 之上以 HTTP 接聽程式的形式執行。</span><span class="sxs-lookup"><span data-stu-id="94444-128">IIS itself runs as an HTTP listener on top of HTTP.sys.</span></span>

## <a name="http2-support"></a><span data-ttu-id="94444-129">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="94444-129">HTTP/2 support</span></span>

<span data-ttu-id="94444-130">如果符合下列基本需求，則可以針對 ASP.NET Core 應用程式啟用 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="94444-130">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is enabled for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="94444-131">Windows Server 2016/Windows 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="94444-131">Windows Server 2016/Windows 10 or later</span></span>
* <span data-ttu-id="94444-132">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 連線</span><span class="sxs-lookup"><span data-stu-id="94444-132">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="94444-133">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="94444-133">TLS 1.2 or later connection</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="94444-134">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="94444-134">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="94444-135">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/1.1`。</span><span class="sxs-lookup"><span data-stu-id="94444-135">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="94444-136">HTTP/2 預設為啟用。</span><span class="sxs-lookup"><span data-stu-id="94444-136">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="94444-137">如果 HTTP/2 連線尚未建立，連線會退為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="94444-137">If an HTTP/2 connection isn't established, the connection falls back to HTTP/1.1.</span></span> <span data-ttu-id="94444-138">Windows 的未來版本會推出 HTTP/2 設定旗標，包括使用 HTTP.sys 來停用 HTTP/2 的功能。</span><span class="sxs-lookup"><span data-stu-id="94444-138">In a future release of Windows, HTTP/2 configuration flags will be available, including the ability to disable HTTP/2 with HTTP.sys.</span></span>

## <a name="kernel-mode-authentication-with-kerberos"></a><span data-ttu-id="94444-139">使用 Kerberos 的核心模式驗證</span><span class="sxs-lookup"><span data-stu-id="94444-139">Kernel mode authentication with Kerberos</span></span>

<span data-ttu-id="94444-140">HTTP.sys 使用 Kerberos 驗證通訊協定委派給核心模式驗證。</span><span class="sxs-lookup"><span data-stu-id="94444-140">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="94444-141">Kerberos 和 HTTP.sys 不支援使用者模式驗證。</span><span class="sxs-lookup"><span data-stu-id="94444-141">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="94444-142">必須使用電腦帳戶來解密 Kerberos 權杖/票證，該權杖/票證取自 Active Directory，並由用戶端將其轉送至伺服器來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="94444-142">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="94444-143">請註冊主機的服務主體名稱 (SPN)，而非應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="94444-143">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

## <a name="how-to-use-httpsys"></a><span data-ttu-id="94444-144">如何使用 HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="94444-144">How to use HTTP.sys</span></span>

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a><span data-ttu-id="94444-145">設定 ASP.NET Core 應用程式使用 HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="94444-145">Configure the ASP.NET Core app to use HTTP.sys</span></span>

1. <span data-ttu-id="94444-146">使用 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (ASP.NET Core 2.1 或更新版本) 時，專案檔中不需要套件參考。</span><span class="sxs-lookup"><span data-stu-id="94444-146">A package reference in the project file isn't required when using the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="94444-147">若不是使用 `Microsoft.AspNetCore.App` 中繼套件，請將套件參考加入 [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/)。</span><span class="sxs-lookup"><span data-stu-id="94444-147">When not using the `Microsoft.AspNetCore.App` metapackage, add a package reference to [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).</span></span>

2. <span data-ttu-id="94444-148">在建置 Web 主機時，呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> 擴充方法，並指定任何必要的 <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>：</span><span class="sxs-lookup"><span data-stu-id="94444-148">Call the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> extension method when building Web Host, specifying any required <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>:</span></span>

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   <span data-ttu-id="94444-149">其他的 HTTP.sys 設定則透過[登錄設定](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows)處理。</span><span class="sxs-lookup"><span data-stu-id="94444-149">Additional HTTP.sys configuration is handled through [registry settings](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).</span></span>

   <span data-ttu-id="94444-150">**HTTP.sys 選項**</span><span class="sxs-lookup"><span data-stu-id="94444-150">**HTTP.sys options**</span></span>

   | <span data-ttu-id="94444-151">屬性</span><span class="sxs-lookup"><span data-stu-id="94444-151">Property</span></span> | <span data-ttu-id="94444-152">說明</span><span class="sxs-lookup"><span data-stu-id="94444-152">Description</span></span> | <span data-ttu-id="94444-153">預設</span><span class="sxs-lookup"><span data-stu-id="94444-153">Default</span></span> |
   | -------- | ----------- | :-----: |
   | [<span data-ttu-id="94444-154">AllowSynchronousIO</span><span class="sxs-lookup"><span data-stu-id="94444-154">AllowSynchronousIO</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | <span data-ttu-id="94444-155">控制是否允許 `HttpContext.Request.Body` 和 `HttpContext.Response.Body` 同步輸出/輸入。</span><span class="sxs-lookup"><span data-stu-id="94444-155">Control whether synchronous input/output is allowed for the `HttpContext.Request.Body` and `HttpContext.Response.Body`.</span></span> | `true` |
   | [<span data-ttu-id="94444-156">Authentication.AllowAnonymous</span><span class="sxs-lookup"><span data-stu-id="94444-156">Authentication.AllowAnonymous</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | <span data-ttu-id="94444-157">允許匿名要求。</span><span class="sxs-lookup"><span data-stu-id="94444-157">Allow anonymous requests.</span></span> | `true` |
   | [<span data-ttu-id="94444-158">Authentication.Schemes</span><span class="sxs-lookup"><span data-stu-id="94444-158">Authentication.Schemes</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | <span data-ttu-id="94444-159">指定允許的驗證配置。</span><span class="sxs-lookup"><span data-stu-id="94444-159">Specify the allowed authentication schemes.</span></span> <span data-ttu-id="94444-160">處置接聽程式之前可隨時修改。</span><span class="sxs-lookup"><span data-stu-id="94444-160">May be modified at any time prior to disposing the listener.</span></span> <span data-ttu-id="94444-161">值是由 [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes) 提供：`Basic`、`Kerberos`、`Negotiate`、`None` 和 `NTLM`。</span><span class="sxs-lookup"><span data-stu-id="94444-161">Values are provided by the [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None`, and `NTLM`.</span></span> | `None` |
   | [<span data-ttu-id="94444-162">EnableResponseCaching</span><span class="sxs-lookup"><span data-stu-id="94444-162">EnableResponseCaching</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | <span data-ttu-id="94444-163">針對含有合格標頭的回應嘗試[核心模式](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode)快取。</span><span class="sxs-lookup"><span data-stu-id="94444-163">Attempt [kernel-mode](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) caching for responses with eligible headers.</span></span> <span data-ttu-id="94444-164">回應可能不包含 `Set-Cookie`、`Vary` 或 `Pragma` 標頭。</span><span class="sxs-lookup"><span data-stu-id="94444-164">The response may not include `Set-Cookie`, `Vary`, or `Pragma` headers.</span></span> <span data-ttu-id="94444-165">它必須包含為 `public` 的 `Cache-Control` 標頭，且有 `shared-max-age` 或 `max-age` 值，或是 `Expires` 標頭。</span><span class="sxs-lookup"><span data-stu-id="94444-165">It must include a `Cache-Control` header that's `public` and either a `shared-max-age` or `max-age` value, or an `Expires` header.</span></span> | `true` |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | <span data-ttu-id="94444-166">可同時接受的數目上限。</span><span class="sxs-lookup"><span data-stu-id="94444-166">The maximum number of concurrent accepts.</span></span> | <span data-ttu-id="94444-167">5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span><span class="sxs-lookup"><span data-stu-id="94444-167">5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | <span data-ttu-id="94444-168">可接受的同時連線數量上限。</span><span class="sxs-lookup"><span data-stu-id="94444-168">The maximum number of concurrent connections to accept.</span></span> <span data-ttu-id="94444-169">使用 `-1` 為無限多個。</span><span class="sxs-lookup"><span data-stu-id="94444-169">Use `-1` for infinite.</span></span> <span data-ttu-id="94444-170">使用 `null` 以使用登錄之整個電腦的設定。</span><span class="sxs-lookup"><span data-stu-id="94444-170">Use `null` to use the registry's machine-wide setting.</span></span> | `null`<br><span data-ttu-id="94444-171">(無限制)</span><span class="sxs-lookup"><span data-stu-id="94444-171">(unlimited)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | <span data-ttu-id="94444-172">請參閱 <a href="#maxrequestbodysize">MaxRequestBodySize</a> 小節。</span><span class="sxs-lookup"><span data-stu-id="94444-172">See the <a href="#maxrequestbodysize">MaxRequestBodySize</a> section.</span></span> | <span data-ttu-id="94444-173">30000000 位元組</span><span class="sxs-lookup"><span data-stu-id="94444-173">30000000 bytes</span></span><br><span data-ttu-id="94444-174">(~28.6 MB)</span><span class="sxs-lookup"><span data-stu-id="94444-174">(~28.6 MB)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | <span data-ttu-id="94444-175">可以加入佇列的最大要求數目。</span><span class="sxs-lookup"><span data-stu-id="94444-175">The maximum number of requests that can be queued.</span></span> | <span data-ttu-id="94444-176">1000</span><span class="sxs-lookup"><span data-stu-id="94444-176">1000</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | <span data-ttu-id="94444-177">指出若回應本文因為用戶端中斷連線而寫入失敗時，應擲回例外狀況或正常完成。</span><span class="sxs-lookup"><span data-stu-id="94444-177">Indicate if response body writes that fail due to client disconnects should throw exceptions or complete normally.</span></span> | `false`<br><span data-ttu-id="94444-178">(正常完成)</span><span class="sxs-lookup"><span data-stu-id="94444-178">(complete normally)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | <span data-ttu-id="94444-179">公開 HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> 設定，這也可在登錄中設定。</span><span class="sxs-lookup"><span data-stu-id="94444-179">Expose the HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> configuration, which may also be configured in the registry.</span></span> <span data-ttu-id="94444-180">API 連結可提供包括預設值在內每個設定的詳細資訊：</span><span class="sxs-lookup"><span data-stu-id="94444-180">Follow the API links to learn more about each setting, including default values:</span></span><ul><li><span data-ttu-id="94444-181">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; HTTP 伺服器 API 清空持續連線上實體內容的允許時間。</span><span class="sxs-lookup"><span data-stu-id="94444-181">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; Time allowed for the HTTP Server API to drain the entity body on a Keep-Alive connection.</span></span></li><li><span data-ttu-id="94444-182">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; 要求實體內容到達的允許時間。</span><span class="sxs-lookup"><span data-stu-id="94444-182">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; Time allowed for the request entity body to arrive.</span></span></li><li><span data-ttu-id="94444-183">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; HTTP 伺服器 API 剖析要求標頭的允許時間。</span><span class="sxs-lookup"><span data-stu-id="94444-183">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; Time allowed for the HTTP Server API to parse the request header.</span></span></li><li><span data-ttu-id="94444-184">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; 允許連線閒置的時間。</span><span class="sxs-lookup"><span data-stu-id="94444-184">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; Time allowed for an idle connection.</span></span></li><li><span data-ttu-id="94444-185">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; 回應的最低傳送速率。</span><span class="sxs-lookup"><span data-stu-id="94444-185">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; The minimum send rate for the response.</span></span></li><li><span data-ttu-id="94444-186">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; 在應用程式 擷取要求之前，將要求保留於要求佇列中的允許時間。</span><span class="sxs-lookup"><span data-stu-id="94444-186">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; Time allowed for the request to remain in the request queue before the app picks it up.</span></span></li></ul> |  |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | <span data-ttu-id="94444-187">指定 <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> 以向 HTTP.sys 註冊。</span><span class="sxs-lookup"><span data-stu-id="94444-187">Specify the <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> to register with HTTP.sys.</span></span> <span data-ttu-id="94444-188">最實用的是 [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*)，可用來將前置詞加入集合。</span><span class="sxs-lookup"><span data-stu-id="94444-188">The most useful is [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), which is used to add a prefix to the collection.</span></span> <span data-ttu-id="94444-189">處置接聽程式之前可隨時修改這些內容。</span><span class="sxs-lookup"><span data-stu-id="94444-189">These may be modified at any time prior to disposing the listener.</span></span> |  |

   <a name="maxrequestbodysize"></a>

   <span data-ttu-id="94444-190">**MaxRequestBodySize**</span><span class="sxs-lookup"><span data-stu-id="94444-190">**MaxRequestBodySize**</span></span>

   <span data-ttu-id="94444-191">任何要求所允許的大小上限 (以位元組為單位)。</span><span class="sxs-lookup"><span data-stu-id="94444-191">The maximum allowed size of any request body in bytes.</span></span> <span data-ttu-id="94444-192">當設定為 `null` 時，要求主體大小上限為無限制。</span><span class="sxs-lookup"><span data-stu-id="94444-192">When set to `null`, the maximum request body size is unlimited.</span></span> <span data-ttu-id="94444-193">此限制對升級連線不會有任何影響，因為其一律為無限制。</span><span class="sxs-lookup"><span data-stu-id="94444-193">This limit has no effect on upgraded connections, which are always unlimited.</span></span>

   <span data-ttu-id="94444-194">在 ASP.NET Core MVC 應用程式中針對單一 `IActionResult` 覆寫限制的建議方式，是在動作方法上使用 <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> 屬性：</span><span class="sxs-lookup"><span data-stu-id="94444-194">The recommended method to override the limit in an ASP.NET Core MVC app for a single `IActionResult` is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   <span data-ttu-id="94444-195">如果應用程式已開始讀取要求之後，才嘗試設定要求的限制，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="94444-195">An exception is thrown if the app attempts to configure the limit on a request after the app has started reading the request.</span></span> <span data-ttu-id="94444-196">`IsReadOnly` 屬性可用來指出 `MaxRequestBodySize` 屬性是否處於唯讀狀態，代表要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="94444-196">An `IsReadOnly` property can be used to indicate if the `MaxRequestBodySize` property is in a read-only state, meaning it's too late to configure the limit.</span></span>

   <span data-ttu-id="94444-197">如果應用程式應該覆寫每個要求的 <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize>，請使用 <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>：</span><span class="sxs-lookup"><span data-stu-id="94444-197">If the app should override <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> per-request, use the <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>:</span></span>

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

3. <span data-ttu-id="94444-198">如果您使用 Visual Studio，請確定應用程式未設定為執行 IIS 或 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="94444-198">If using Visual Studio, make sure the app isn't configured to run IIS or IIS Express.</span></span>

   <span data-ttu-id="94444-199">在 Visual Studio 中，預設啟動設定檔適用於 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="94444-199">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="94444-200">若要執行專案作為主控台應用程式，請手動變更選取的設定檔，如下列螢幕擷取畫面所示：</span><span class="sxs-lookup"><span data-stu-id="94444-200">To run the project as a console app, manually change the selected profile, as shown in the following screen shot:</span></span>

   ![選取主控台應用程式設定檔](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a><span data-ttu-id="94444-202">設定 Windows Server</span><span class="sxs-lookup"><span data-stu-id="94444-202">Configure Windows Server</span></span>

1. <span data-ttu-id="94444-203">判斷要為應用程式開啟的連接埠，然後使用 [Windows 防火牆](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule)或 [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) PowerShell Cmdlet 來開啟防火牆連接埠，以允許流量到達 HTTP.sys。</span><span class="sxs-lookup"><span data-stu-id="94444-203">Determine the ports to open for the app and use [Windows Firewall](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) or the [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) PowerShell cmdlet to open firewall ports to allow traffic to reach HTTP.sys.</span></span> <span data-ttu-id="94444-204">在下列命令和應用程式設定中，會使用連接埠 443。</span><span class="sxs-lookup"><span data-stu-id="94444-204">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="94444-205">部署至 Azure VM 時，請在[網路安全性群組](/azure/virtual-machines/windows/nsg-quickstart-portal)中開啟連接埠。</span><span class="sxs-lookup"><span data-stu-id="94444-205">When deploying to an Azure VM, open the ports in the [Network Security Group](/azure/virtual-machines/windows/nsg-quickstart-portal).</span></span> <span data-ttu-id="94444-206">在下列命令和應用程式設定中，會使用連接埠 443。</span><span class="sxs-lookup"><span data-stu-id="94444-206">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="94444-207">視需要取得並安裝 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="94444-207">Obtain and install X.509 certificates, if required.</span></span>

   <span data-ttu-id="94444-208">在 Windows 上，請使用 [New-SelfSignedCertificate PowerShell Cmdlet](/powershell/module/pkiclient/new-selfsignedcertificate) 來建立自我簽署的憑證。</span><span class="sxs-lookup"><span data-stu-id="94444-208">On Windows, create self-signed certificates using the [New-SelfSignedCertificate PowerShell cmdlet](/powershell/module/pkiclient/new-selfsignedcertificate).</span></span> <span data-ttu-id="94444-209">如需不支援的範例，請參閱 [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="94444-209">For an unsupported example, see [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).</span></span>

   <span data-ttu-id="94444-210">將自我簽署的憑證或 CA 簽署的憑證安裝在伺服器的 [本機電腦] > 個人 存放區中。</span><span class="sxs-lookup"><span data-stu-id="94444-210">Install either self-signed or CA-signed certificates in the server's **Local Machine** > **Personal** store.</span></span>

1. <span data-ttu-id="94444-211">如果應用程式是[與架構相依的部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)，請安裝 .NET Core、.NET Framework 或兩者 (如果應用程式是以 .NET Framework 為目標的 .NET Core 應用程式)。</span><span class="sxs-lookup"><span data-stu-id="94444-211">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd), install .NET Core, .NET Framework, or both (if the app is a .NET Core app targeting the .NET Framework).</span></span>

   * <span data-ttu-id="94444-212">**.NET Core** &ndash; 如果應用程式需要 .NET Core，請從 [.NET Core 下載](https://dotnet.microsoft.com/download)取得並執行 **.NET Core 執行階段**安裝程式。</span><span class="sxs-lookup"><span data-stu-id="94444-212">**.NET Core** &ndash; If the app requires .NET Core, obtain and run the **.NET Core Runtime** installer from [.NET Core Downloads](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="94444-213">請勿在伺服器上安裝完整的 SDK。</span><span class="sxs-lookup"><span data-stu-id="94444-213">Don't install the full SDK on the server.</span></span>
   * <span data-ttu-id="94444-214">**.NET Framework** &ndash; 如果應用程式需要 .NET Framework，請參閱 [.NET Framework 安裝指南](/dotnet/framework/install/)。</span><span class="sxs-lookup"><span data-stu-id="94444-214">**.NET Framework** &ndash; If the app requires .NET Framework, see the [.NET Framework installation guide](/dotnet/framework/install/).</span></span> <span data-ttu-id="94444-215">安裝必要的 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="94444-215">Install the required .NET Framework.</span></span> <span data-ttu-id="94444-216">您可以從 [.NET Core 下載](https://dotnet.microsoft.com/download)頁面取得最新 .NET Framework 的安裝程式。</span><span class="sxs-lookup"><span data-stu-id="94444-216">The installer for the latest .NET Framework is available from the [.NET Core Downloads](https://dotnet.microsoft.com/download) page.</span></span>

   <span data-ttu-id="94444-217">如果應用程式是[自封式部署](/dotnet/core/deploying/#framework-dependent-deployments-scd)，則應用程式的部署中會包含執行階段。</span><span class="sxs-lookup"><span data-stu-id="94444-217">If the app is a [self-contained deployment](/dotnet/core/deploying/#framework-dependent-deployments-scd), the app includes the runtime in its deployment.</span></span> <span data-ttu-id="94444-218">不需要在伺服器上安裝任何架構。</span><span class="sxs-lookup"><span data-stu-id="94444-218">No framework installation is required on the server.</span></span>

1. <span data-ttu-id="94444-219">設定應用程式中的 URL 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="94444-219">Configure URLs and ports in the app.</span></span>

   <span data-ttu-id="94444-220">ASP.NET Core 預設會繫結至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="94444-220">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="94444-221">若要設定 URL 首碼和連接埠，選項包括：</span><span class="sxs-lookup"><span data-stu-id="94444-221">To configure URL prefixes and ports, options include:</span></span>

   * <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
   * <span data-ttu-id="94444-222">`urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="94444-222">`urls` command-line argument</span></span>
   * <span data-ttu-id="94444-223">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="94444-223">`ASPNETCORE_URLS` environment variable</span></span>
   * <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes>

   <span data-ttu-id="94444-224">下列程式碼範例示範如何使用 <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> 搭配位於連接埠 443 的伺服器本機 IP 位址 `10.0.0.4`：</span><span class="sxs-lookup"><span data-stu-id="94444-224">The following code example shows how to use <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> with the server's local IP address `10.0.0.4` on port 443:</span></span>

   [!code-csharp[](httpsys/sample_snapshot/Program.cs?name=snippet1&highlight=11)]

   <span data-ttu-id="94444-225">`UrlPrefixes` 的優點是針對錯誤格式的前置詞會立即產生錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="94444-225">An advantage of `UrlPrefixes` is that an error message is generated immediately for improperly formatted prefixes.</span></span>

   <span data-ttu-id="94444-226">`UrlPrefixes` 中的設定會覆寫 `UseUrls`/`urls`/`ASPNETCORE_URLS` 設定。</span><span class="sxs-lookup"><span data-stu-id="94444-226">The settings in `UrlPrefixes` override `UseUrls`/`urls`/`ASPNETCORE_URLS` settings.</span></span> <span data-ttu-id="94444-227">因此，`UseUrls`、`urls` 和 `ASPNETCORE_URLS` 環境變數的優點，是能更輕鬆地在 Kestrel 和 HTTP.sys 之間切換。</span><span class="sxs-lookup"><span data-stu-id="94444-227">Therefore, an advantage of `UseUrls`, `urls`, and the `ASPNETCORE_URLS` environment variable is that it's easier to switch between Kestrel and HTTP.sys.</span></span> <span data-ttu-id="94444-228">如需詳細資訊，請參閱<xref:fundamentals/host/web-host>。</span><span class="sxs-lookup"><span data-stu-id="94444-228">For more information, see <xref:fundamentals/host/web-host>.</span></span>

   <span data-ttu-id="94444-229">HTTP.sys 使用 [HTTP Server API UrlPrefix 字串格式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)。</span><span class="sxs-lookup"><span data-stu-id="94444-229">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

   > [!WARNING]
   > <span data-ttu-id="94444-230">請**勿**使用最上層萬用字元繫結 (`http://*:80/`與 `http://+:80`)。</span><span class="sxs-lookup"><span data-stu-id="94444-230">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="94444-231">最上層萬用字元繫結會導致應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="94444-231">Top-level wildcard bindings create app security vulnerabilities.</span></span> <span data-ttu-id="94444-232">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="94444-232">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="94444-233">請使用明確的主機名稱或 IP 位址，而不要使用萬用字元。</span><span class="sxs-lookup"><span data-stu-id="94444-233">Use explicit host names or IP addresses rather than wildcards.</span></span> <span data-ttu-id="94444-234">若您擁有整個父網域 (相對於有弱點的 `*.com`) 的控制權，則子網域萬用字元繫結 (例如 `*.mysub.com`) 便不構成安全性風險。</span><span class="sxs-lookup"><span data-stu-id="94444-234">Subdomain wildcard binding (for example, `*.mysub.com`) isn't a security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="94444-235">如需詳細資訊，請參閱[RFC 7230：5.4 節：主機](https://tools.ietf.org/html/rfc7230#section-5.4) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="94444-235">For more information, see [RFC 7230: Section 5.4: Host](https://tools.ietf.org/html/rfc7230#section-5.4).</span></span>

1. <span data-ttu-id="94444-236">在伺服器上預先註冊 URL 首碼。</span><span class="sxs-lookup"><span data-stu-id="94444-236">Preregister URL prefixes on the server.</span></span>

   <span data-ttu-id="94444-237">用來設定 HTTP.sys 的內建工具是 *netsh.exe*。</span><span class="sxs-lookup"><span data-stu-id="94444-237">The built-in tool for configuring HTTP.sys is *netsh.exe*.</span></span> <span data-ttu-id="94444-238">*netsh.exe* 是用來保留 URL 前置詞，並指派 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="94444-238">*netsh.exe* is used to reserve URL prefixes and assign X.509 certificates.</span></span> <span data-ttu-id="94444-239">此工具需要系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="94444-239">The tool requires administrator privileges.</span></span>

   <span data-ttu-id="94444-240">使用 *netsh.exe* 工具來為應用程式註冊 URL：</span><span class="sxs-lookup"><span data-stu-id="94444-240">Use the *netsh.exe* tool to register URLs for the app:</span></span>

   ```console
   netsh http add urlacl url=<URL> user=<USER>
   ```

   * <span data-ttu-id="94444-241">`<URL>` &ndash; 完整的「統一資源定位器」(URL)。</span><span class="sxs-lookup"><span data-stu-id="94444-241">`<URL>` &ndash; The fully qualified Uniform Resource Locator (URL).</span></span> <span data-ttu-id="94444-242">請勿使用萬用字元繫結。</span><span class="sxs-lookup"><span data-stu-id="94444-242">Don't use a wildcard binding.</span></span> <span data-ttu-id="94444-243">請使用有效的主機名稱或本機 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="94444-243">Use a valid hostname or local IP address.</span></span> <span data-ttu-id="94444-244">URL 必須包含結尾斜線。</span><span class="sxs-lookup"><span data-stu-id="94444-244">*The URL must include a trailing slash.*</span></span>
   * <span data-ttu-id="94444-245">`<USER>` &ndash; 會指定使用者或「使用者-群組」名稱。</span><span class="sxs-lookup"><span data-stu-id="94444-245">`<USER>` &ndash; Specifies the user or user-group name.</span></span>

   <span data-ttu-id="94444-246">在以下範例中，伺服器的本機 IP 位址是 `10.0.0.4`：</span><span class="sxs-lookup"><span data-stu-id="94444-246">In the following example, the local IP address of the server is `10.0.0.4`:</span></span>

   ```console
   netsh http add urlacl url=https://10.0.0.4:443/ user=Users
   ```

   <span data-ttu-id="94444-247">已註冊 URL 時，此工具會以 `URL reservation successfully added` 回應。</span><span class="sxs-lookup"><span data-stu-id="94444-247">When a URL is registered, the tool responds with `URL reservation successfully added`.</span></span>

   <span data-ttu-id="94444-248">若要刪除已註冊的 URL，請使用 `delete urlacl` 命令：</span><span class="sxs-lookup"><span data-stu-id="94444-248">To delete a registered URL, use the `delete urlacl` command:</span></span>

   ```console
   netsh http delete urlacl url=<URL>
   ```

1. <span data-ttu-id="94444-249">在伺服器上註冊 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="94444-249">Register X.509 certificates on the server.</span></span>

   <span data-ttu-id="94444-250">使用 *netsh.exe* 工具來為應用程式註冊憑證：</span><span class="sxs-lookup"><span data-stu-id="94444-250">Use the *netsh.exe* tool to register certificates for the app:</span></span>

   ```console
   netsh http add sslcert ipport=<IP>:<PORT> certhash=<THUMBPRINT> appid="{<GUID>}"
   ```

   * <span data-ttu-id="94444-251">`<IP>` &ndash; 會指定繫結的本機 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="94444-251">`<IP>` &ndash; Specifies the local IP address for the binding.</span></span> <span data-ttu-id="94444-252">請勿使用萬用字元繫結。</span><span class="sxs-lookup"><span data-stu-id="94444-252">Don't use a wildcard binding.</span></span> <span data-ttu-id="94444-253">請使用有效的 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="94444-253">Use a valid IP address.</span></span>
   * <span data-ttu-id="94444-254">`<PORT>` &ndash; 會指定繫結的連接埠。</span><span class="sxs-lookup"><span data-stu-id="94444-254">`<PORT>` &ndash; Specifies the port for the binding.</span></span>
   * <span data-ttu-id="94444-255">`<THUMBPRINT>` &ndash; X.509 憑證指紋。</span><span class="sxs-lookup"><span data-stu-id="94444-255">`<THUMBPRINT>` &ndash; The X.509 certificate thumbprint.</span></span>
   * <span data-ttu-id="94444-256">`<GUID>` &ndash; 開發人員所產生來代表應用程式的 GUID，供參考使用。</span><span class="sxs-lookup"><span data-stu-id="94444-256">`<GUID>` &ndash; A developer-generated GUID to represent the app for informational purposes.</span></span>

   <span data-ttu-id="94444-257">為了便於參考，請將 GUID 以套件標記的形式儲存在應用程式中：</span><span class="sxs-lookup"><span data-stu-id="94444-257">For reference purposes, store the GUID in the app as a package tag:</span></span>

   * <span data-ttu-id="94444-258">在 Visual Studio 中：</span><span class="sxs-lookup"><span data-stu-id="94444-258">In Visual Studio:</span></span>
     * <span data-ttu-id="94444-259">在 [方案總管] 中的專案上按一下滑鼠右鍵，然後選取 [屬性]，以開啟應用程式的專案屬性。</span><span class="sxs-lookup"><span data-stu-id="94444-259">Open the app's project properties by right-clicking on the app in **Solution Explorer** and selecting **Properties**.</span></span>
     * <span data-ttu-id="94444-260">選取 [套件] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="94444-260">Select the **Package** tab.</span></span>
     * <span data-ttu-id="94444-261">輸入您在 [標記] 欄位中建立的 GUID。</span><span class="sxs-lookup"><span data-stu-id="94444-261">Enter the GUID that you created in the **Tags** field.</span></span>
   * <span data-ttu-id="94444-262">不是使用 Visual Studio 時：</span><span class="sxs-lookup"><span data-stu-id="94444-262">When not using Visual Studio:</span></span>
     * <span data-ttu-id="94444-263">開啟應用程式的專案檔。</span><span class="sxs-lookup"><span data-stu-id="94444-263">Open the app's project file.</span></span>
     * <span data-ttu-id="94444-264">將 `<PackageTags>` 屬性搭配您所建立的 GUID 新增至新的或現有的 `<PropertyGroup>`：</span><span class="sxs-lookup"><span data-stu-id="94444-264">Add a `<PackageTags>` property to a new or existing `<PropertyGroup>` with the GUID that you created:</span></span>

       ```xml
       <PropertyGroup>
         <PackageTags>9412ee86-c21b-4eb8-bd89-f650fbf44931</PackageTags>
       </PropertyGroup>
       ```

   <span data-ttu-id="94444-265">在以下範例中：</span><span class="sxs-lookup"><span data-stu-id="94444-265">In the following example:</span></span>

   * <span data-ttu-id="94444-266">伺服器的本機 IP 位址是 `10.0.0.4`。</span><span class="sxs-lookup"><span data-stu-id="94444-266">The local IP address of the server is `10.0.0.4`.</span></span>
   * <span data-ttu-id="94444-267">線上隨機 GUID 產生器會提供 `appid` 值。</span><span class="sxs-lookup"><span data-stu-id="94444-267">An online random GUID generator provides the `appid` value.</span></span>

   ```console
   netsh http add sslcert 
       ipport=10.0.0.4:443 
       certhash=b66ee04419d4ee37464ab8785ff02449980eae10 
       appid="{9412ee86-c21b-4eb8-bd89-f650fbf44931}"
   ```

   <span data-ttu-id="94444-268">已註冊憑證時，此工具會以 `SSL Certificate successfully added` 回應。</span><span class="sxs-lookup"><span data-stu-id="94444-268">When a certificate is registered, the tool responds with `SSL Certificate successfully added`.</span></span>

   <span data-ttu-id="94444-269">若要刪除憑證註冊，請使用 `delete sslcert` 命令：</span><span class="sxs-lookup"><span data-stu-id="94444-269">To delete a certificate registration, use the `delete sslcert` command:</span></span>

   ```console
   netsh http delete sslcert ipport=<IP>:<PORT>
   ```

   <span data-ttu-id="94444-270">以下是 *netsh.exe* 的參考文件：</span><span class="sxs-lookup"><span data-stu-id="94444-270">Reference documentation for *netsh.exe*:</span></span>

   * [<span data-ttu-id="94444-271">超文字傳輸通訊協定 (HTTP) 的 Netsh 命令</span><span class="sxs-lookup"><span data-stu-id="94444-271">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
   * [<span data-ttu-id="94444-272">UrlPrefix 字串</span><span class="sxs-lookup"><span data-stu-id="94444-272">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

1. <span data-ttu-id="94444-273">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="94444-273">Run the app.</span></span>

   <span data-ttu-id="94444-274">使用 HTTP (不是 HTTPS) 搭配大於 1024 的連接埠號碼來繫結至 localhost 時，不需要系統管理員權限即可執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="94444-274">Administrator privileges aren't required to run the app when binding to localhost using HTTP (not HTTPS) with a port number greater than 1024.</span></span> <span data-ttu-id="94444-275">針對其他設定 (例如，使用本機 IP 位址或繫結至連接埠 443)，請使用系統管理員權限來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="94444-275">For other configurations (for example, using a local IP address or binding to port 443), run the app with administrator privileges.</span></span>

   <span data-ttu-id="94444-276">應用程式會在伺服器的公用 IP 位址回應。</span><span class="sxs-lookup"><span data-stu-id="94444-276">The app responds at the server's public IP address.</span></span> <span data-ttu-id="94444-277">在此範例中，是從網際網路透過伺服器的公用 IP 位址 `104.214.79.47` 連線至伺服器。</span><span class="sxs-lookup"><span data-stu-id="94444-277">In this example, the server is reached from the Internet at its public IP address of `104.214.79.47`.</span></span>

   <span data-ttu-id="94444-278">在此範例中使用的是開發憑證。</span><span class="sxs-lookup"><span data-stu-id="94444-278">A development certificate is used in this example.</span></span> <span data-ttu-id="94444-279">在略過瀏覽器的未受信任憑證警告之後，頁面會安全地載入。</span><span class="sxs-lookup"><span data-stu-id="94444-279">The page loads securely after bypassing the browser's untrusted certificate warning.</span></span>

   ![顯示已載入應用程式索引頁面的瀏覽器視窗](httpsys/_static/browser.png)

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="94444-281">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="94444-281">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="94444-282">如果是 HTTP.sys 所裝載且與來自網際網路或公司網路的要求進行互動的應用程式，在裝載於 Proxy 伺服器和負載平衡器後方時，可能需要額外的組態。</span><span class="sxs-lookup"><span data-stu-id="94444-282">For apps hosted by HTTP.sys that interact with requests from the Internet or a corporate network, additional configuration might be required when hosting behind proxy servers and load balancers.</span></span> <span data-ttu-id="94444-283">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="94444-283">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="94444-284">其他資源</span><span class="sxs-lookup"><span data-stu-id="94444-284">Additional resources</span></span>

* <span data-ttu-id="94444-285">[使用 HTTP.sys 來啟用 Windows 驗證](xref:security/authentication/windowsauth#enable-windows-authentication-with-httpsys) \(機器翻譯\)</span><span class="sxs-lookup"><span data-stu-id="94444-285">[Enable Windows Authentication with HTTP.sys](xref:security/authentication/windowsauth#enable-windows-authentication-with-httpsys)</span></span>
* <span data-ttu-id="94444-286">[HTTP 伺服器 API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="94444-286">[HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)</span></span>
* <span data-ttu-id="94444-287">[aspnet/HttpSysServer GitHub 存放庫 (原始程式碼)](https://github.com/aspnet/HttpSysServer/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="94444-287">[aspnet/HttpSysServer GitHub repository (source code)](https://github.com/aspnet/HttpSysServer/)</span></span>
* [<span data-ttu-id="94444-288">主機</span><span class="sxs-lookup"><span data-stu-id="94444-288">The host</span></span>](xref:fundamentals/index#host)
* <xref:test/troubleshoot>
