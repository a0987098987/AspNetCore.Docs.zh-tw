---
title: ASP.NET Core 中的 HTTP.sys 網頁伺服器實作
author: guardrex
description: 深入了解 HTTP.sys，這是 Windows 上的 ASP.NET Core 網頁伺服器。 HTTP.sys 建置在 HTTP.sys 核心模式驅動程式之上，是 Kestrel 的替代方式，可以用來直接連線到網際網路而不使用 IIS。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 8a315d859fa70d97501156ff3cf97d4c3a5c5bd4
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2020
ms.locfileid: "77171761"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="1e44d-104">ASP.NET Core 中的 HTTP.sys 網頁伺服器實作</span><span class="sxs-lookup"><span data-stu-id="1e44d-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="1e44d-105">作者：[Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1e44d-105">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.1"

<span data-ttu-id="1e44d-106">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) 是只在 Windows 上執行的 [ASP.NET Core 網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-106">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) is a [web server for ASP.NET Core](xref:fundamentals/servers/index) that only runs on Windows.</span></span> <span data-ttu-id="1e44d-107">HTTP.sys 是 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器的替代方式，提供了一些 Kestrel 未提供的功能。</span><span class="sxs-lookup"><span data-stu-id="1e44d-107">HTTP.sys is an alternative to [Kestrel](xref:fundamentals/servers/kestrel) server and offers some features that Kestrel doesn't provide.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1e44d-108">HTTP.sys 與 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)不相容，且不能與 IIS 或 IIS Express 搭配使用。</span><span class="sxs-lookup"><span data-stu-id="1e44d-108">HTTP.sys isn't compatible with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and can't be used with IIS or IIS Express.</span></span>

<span data-ttu-id="1e44d-109">HTTP.sys 支援下列功能：</span><span class="sxs-lookup"><span data-stu-id="1e44d-109">HTTP.sys supports the following features:</span></span>

* [<span data-ttu-id="1e44d-110">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="1e44d-110">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
* <span data-ttu-id="1e44d-111">連接埠共用</span><span class="sxs-lookup"><span data-stu-id="1e44d-111">Port sharing</span></span>
* <span data-ttu-id="1e44d-112">使用 SNI 的 HTTPS</span><span class="sxs-lookup"><span data-stu-id="1e44d-112">HTTPS with SNI</span></span>
* <span data-ttu-id="1e44d-113">透過 TLS 的 HTTP/2 (Windows 10 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="1e44d-113">HTTP/2 over TLS (Windows 10 or later)</span></span>
* <span data-ttu-id="1e44d-114">直接檔案傳輸</span><span class="sxs-lookup"><span data-stu-id="1e44d-114">Direct file transmission</span></span>
* <span data-ttu-id="1e44d-115">回應快取</span><span class="sxs-lookup"><span data-stu-id="1e44d-115">Response caching</span></span>
* <span data-ttu-id="1e44d-116">WebSocket (Windows 8 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="1e44d-116">WebSockets (Windows 8 or later)</span></span>

<span data-ttu-id="1e44d-117">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="1e44d-117">Supported Windows versions:</span></span>

* <span data-ttu-id="1e44d-118">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="1e44d-118">Windows 7 or later</span></span>
* <span data-ttu-id="1e44d-119">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="1e44d-119">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="1e44d-120">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1e44d-120">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="1e44d-121">使用 HTTP.sys 的時機</span><span class="sxs-lookup"><span data-stu-id="1e44d-121">When to use HTTP.sys</span></span>

<span data-ttu-id="1e44d-122">HTTP.sys 在下列部署環境中非常有用：</span><span class="sxs-lookup"><span data-stu-id="1e44d-122">HTTP.sys is useful for deployments where:</span></span>

* <span data-ttu-id="1e44d-123">需要直接向網際網路公開伺服器而不使用 IIS。</span><span class="sxs-lookup"><span data-stu-id="1e44d-123">There's a need to expose the server directly to the Internet without using IIS.</span></span>

  ![HTTP.sys 直接與網際網路通訊](httpsys/_static/httpsys-to-internet.png)

* <span data-ttu-id="1e44d-125">內部部署需要的功能無法在 Kestrel 中使用，例如 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-125">An internal deployment requires a feature not available in Kestrel, such as [Windows Authentication](xref:security/authentication/windowsauth).</span></span>

  ![HTTP.sys 直接與內部網路通訊](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="1e44d-127">HTTP.sys 是成熟的技術，可抵禦許多種類的攻擊，並提供功能完整之網頁伺服器的穩固性、安全性及延展性。</span><span class="sxs-lookup"><span data-stu-id="1e44d-127">HTTP.sys is mature technology that protects against many types of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="1e44d-128">IIS 本身在 HTTP.sys 之上以 HTTP 接聽程式的形式執行。</span><span class="sxs-lookup"><span data-stu-id="1e44d-128">IIS itself runs as an HTTP listener on top of HTTP.sys.</span></span>

## <a name="http2-support"></a><span data-ttu-id="1e44d-129">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="1e44d-129">HTTP/2 support</span></span>

<span data-ttu-id="1e44d-130">如果符合下列基本需求，則可以針對 ASP.NET Core 應用程式啟用 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="1e44d-130">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is enabled for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="1e44d-131">Windows Server 2016/Windows 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="1e44d-131">Windows Server 2016/Windows 10 or later</span></span>
* <span data-ttu-id="1e44d-132">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 連線</span><span class="sxs-lookup"><span data-stu-id="1e44d-132">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="1e44d-133">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="1e44d-133">TLS 1.2 or later connection</span></span>

<span data-ttu-id="1e44d-134">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="1e44d-134">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="1e44d-135">HTTP/2 預設為啟用。</span><span class="sxs-lookup"><span data-stu-id="1e44d-135">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="1e44d-136">如果 HTTP/2 連線尚未建立，連線會退為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="1e44d-136">If an HTTP/2 connection isn't established, the connection falls back to HTTP/1.1.</span></span> <span data-ttu-id="1e44d-137">Windows 的未來版本會推出 HTTP/2 設定旗標，包括使用 HTTP.sys 來停用 HTTP/2 的功能。</span><span class="sxs-lookup"><span data-stu-id="1e44d-137">In a future release of Windows, HTTP/2 configuration flags will be available, including the ability to disable HTTP/2 with HTTP.sys.</span></span>

## <a name="kernel-mode-authentication-with-kerberos"></a><span data-ttu-id="1e44d-138">使用 Kerberos 的核心模式驗證</span><span class="sxs-lookup"><span data-stu-id="1e44d-138">Kernel mode authentication with Kerberos</span></span>

<span data-ttu-id="1e44d-139">HTTP.sys 使用 Kerberos 驗證通訊協定委派給核心模式驗證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-139">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="1e44d-140">Kerberos 和 HTTP.sys 不支援使用者模式驗證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-140">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="1e44d-141">必須使用電腦帳戶來解密 Kerberos 權杖/票證，該權杖/票證取自 Active Directory，並由用戶端將其轉送至伺服器來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="1e44d-141">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="1e44d-142">請註冊主機的服務主體名稱 (SPN)，而非應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="1e44d-142">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

## <a name="how-to-use-httpsys"></a><span data-ttu-id="1e44d-143">如何使用 HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="1e44d-143">How to use HTTP.sys</span></span>

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a><span data-ttu-id="1e44d-144">設定 ASP.NET Core 應用程式使用 HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="1e44d-144">Configure the ASP.NET Core app to use HTTP.sys</span></span>

<span data-ttu-id="1e44d-145">建置主機時，呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> 擴充方法，並指定任何必要的 <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>。</span><span class="sxs-lookup"><span data-stu-id="1e44d-145">Call the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> extension method when building the host, specifying any required <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>.</span></span> <span data-ttu-id="1e44d-146">下列範例會將選項設定為它們的預設值：</span><span class="sxs-lookup"><span data-stu-id="1e44d-146">The following example sets options to their default values:</span></span>

[!code-csharp[](httpsys/samples/3.x/SampleApp/Program.cs?name=snippet1&highlight=5-13)]

<span data-ttu-id="1e44d-147">其他的 HTTP.sys 設定則透過[登錄設定](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows)處理。</span><span class="sxs-lookup"><span data-stu-id="1e44d-147">Additional HTTP.sys configuration is handled through [registry settings](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).</span></span>

<span data-ttu-id="1e44d-148">**HTTP.sys 選項**</span><span class="sxs-lookup"><span data-stu-id="1e44d-148">**HTTP.sys options**</span></span>

| <span data-ttu-id="1e44d-149">屬性</span><span class="sxs-lookup"><span data-stu-id="1e44d-149">Property</span></span> | <span data-ttu-id="1e44d-150">描述</span><span class="sxs-lookup"><span data-stu-id="1e44d-150">Description</span></span> | <span data-ttu-id="1e44d-151">預設</span><span class="sxs-lookup"><span data-stu-id="1e44d-151">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="1e44d-152">AllowSynchronousIO</span><span class="sxs-lookup"><span data-stu-id="1e44d-152">AllowSynchronousIO</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | <span data-ttu-id="1e44d-153">控制是否允許 `HttpContext.Request.Body` 和 `HttpContext.Response.Body` 同步輸出/輸入。</span><span class="sxs-lookup"><span data-stu-id="1e44d-153">Control whether synchronous input/output is allowed for the `HttpContext.Request.Body` and `HttpContext.Response.Body`.</span></span> | `false` |
| [<span data-ttu-id="1e44d-154">Authentication.AllowAnonymous</span><span class="sxs-lookup"><span data-stu-id="1e44d-154">Authentication.AllowAnonymous</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | <span data-ttu-id="1e44d-155">允許匿名要求。</span><span class="sxs-lookup"><span data-stu-id="1e44d-155">Allow anonymous requests.</span></span> | `true` |
| [<span data-ttu-id="1e44d-156">Authentication.Schemes</span><span class="sxs-lookup"><span data-stu-id="1e44d-156">Authentication.Schemes</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | <span data-ttu-id="1e44d-157">指定允許的驗證配置。</span><span class="sxs-lookup"><span data-stu-id="1e44d-157">Specify the allowed authentication schemes.</span></span> <span data-ttu-id="1e44d-158">處置接聽程式之前可隨時修改。</span><span class="sxs-lookup"><span data-stu-id="1e44d-158">May be modified at any time prior to disposing the listener.</span></span> <span data-ttu-id="1e44d-159">值是由 [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes) 提供：`Basic`、`Kerberos`、`Negotiate`、`None` 和 `NTLM`。</span><span class="sxs-lookup"><span data-stu-id="1e44d-159">Values are provided by the [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None`, and `NTLM`.</span></span> | `None` |
| [<span data-ttu-id="1e44d-160">EnableResponseCaching</span><span class="sxs-lookup"><span data-stu-id="1e44d-160">EnableResponseCaching</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | <span data-ttu-id="1e44d-161">針對含有合格標頭的回應嘗試[核心模式](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode)快取。</span><span class="sxs-lookup"><span data-stu-id="1e44d-161">Attempt [kernel-mode](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) caching for responses with eligible headers.</span></span> <span data-ttu-id="1e44d-162">回應可能不包含 `Set-Cookie`、`Vary` 或 `Pragma` 標頭。</span><span class="sxs-lookup"><span data-stu-id="1e44d-162">The response may not include `Set-Cookie`, `Vary`, or `Pragma` headers.</span></span> <span data-ttu-id="1e44d-163">它必須包含為 `Cache-Control` 的 `public` 標頭，且有 `shared-max-age` 或 `max-age` 值，或是 `Expires` 標頭。</span><span class="sxs-lookup"><span data-stu-id="1e44d-163">It must include a `Cache-Control` header that's `public` and either a `shared-max-age` or `max-age` value, or an `Expires` header.</span></span> | `true` |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | <span data-ttu-id="1e44d-164">可同時接受的數目上限。</span><span class="sxs-lookup"><span data-stu-id="1e44d-164">The maximum number of concurrent accepts.</span></span> | <span data-ttu-id="1e44d-165">5 &times;[環境。<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span><span class="sxs-lookup"><span data-stu-id="1e44d-165">5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | <span data-ttu-id="1e44d-166">可接受的同時連線數量上限。</span><span class="sxs-lookup"><span data-stu-id="1e44d-166">The maximum number of concurrent connections to accept.</span></span> <span data-ttu-id="1e44d-167">使用 `-1` 為無限多個。</span><span class="sxs-lookup"><span data-stu-id="1e44d-167">Use `-1` for infinite.</span></span> <span data-ttu-id="1e44d-168">使用 `null` 以使用登錄之整個電腦的設定。</span><span class="sxs-lookup"><span data-stu-id="1e44d-168">Use `null` to use the registry's machine-wide setting.</span></span> | `null`<br><span data-ttu-id="1e44d-169">（全電腦</span><span class="sxs-lookup"><span data-stu-id="1e44d-169">(machine-wide</span></span><br><span data-ttu-id="1e44d-170">設</span><span class="sxs-lookup"><span data-stu-id="1e44d-170">setting)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | <span data-ttu-id="1e44d-171">請參閱 <a href="#maxrequestbodysize">MaxRequestBodySize</a> 小節。</span><span class="sxs-lookup"><span data-stu-id="1e44d-171">See the <a href="#maxrequestbodysize">MaxRequestBodySize</a> section.</span></span> | <span data-ttu-id="1e44d-172">30000000 位元組</span><span class="sxs-lookup"><span data-stu-id="1e44d-172">30000000 bytes</span></span><br><span data-ttu-id="1e44d-173">(~28.6 MB)</span><span class="sxs-lookup"><span data-stu-id="1e44d-173">(~28.6 MB)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | <span data-ttu-id="1e44d-174">可以加入佇列的最大要求數目。</span><span class="sxs-lookup"><span data-stu-id="1e44d-174">The maximum number of requests that can be queued.</span></span> | <span data-ttu-id="1e44d-175">1000</span><span class="sxs-lookup"><span data-stu-id="1e44d-175">1000</span></span> |
| `RequestQueueMode` | <span data-ttu-id="1e44d-176">這會指出伺服器是否負責建立和設定要求佇列，或是否應附加至現有的佇列。</span><span class="sxs-lookup"><span data-stu-id="1e44d-176">This indicates whether the server is responsible for creating and configuring the request queue, or if it should attach to an existing queue.</span></span><br><span data-ttu-id="1e44d-177">附加至現有的佇列時，大部分的現有設定選項都不適用。</span><span class="sxs-lookup"><span data-stu-id="1e44d-177">Most existing configuration options do not apply when attaching to an existing queue.</span></span> | `RequestQueueMode.Create` |
| `RequestQueueName` | <span data-ttu-id="1e44d-178">Http.sys 要求佇列的名稱。</span><span class="sxs-lookup"><span data-stu-id="1e44d-178">The name of the HTTP.sys request queue.</span></span> | <span data-ttu-id="1e44d-179">`null` （匿名佇列）</span><span class="sxs-lookup"><span data-stu-id="1e44d-179">`null` (Anonymous queue)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | <span data-ttu-id="1e44d-180">指出若回應本文因為用戶端中斷連線而寫入失敗時，應擲回例外狀況或正常完成。</span><span class="sxs-lookup"><span data-stu-id="1e44d-180">Indicate if response body writes that fail due to client disconnects should throw exceptions or complete normally.</span></span> | `false`<br><span data-ttu-id="1e44d-181">(正常完成)</span><span class="sxs-lookup"><span data-stu-id="1e44d-181">(complete normally)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | <span data-ttu-id="1e44d-182">公開 HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> 設定，這也可在登錄中設定。</span><span class="sxs-lookup"><span data-stu-id="1e44d-182">Expose the HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> configuration, which may also be configured in the registry.</span></span> <span data-ttu-id="1e44d-183">API 連結可提供包括預設值在內每個設定的詳細資訊：</span><span class="sxs-lookup"><span data-stu-id="1e44d-183">Follow the API links to learn more about each setting, including default values:</span></span><ul><li><span data-ttu-id="1e44d-184">[TimeoutManager。](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) HTTP 伺服器 API 允許的 DrainEntityBody &ndash; 時間，以在保持連線的狀態中清空實體主體。</span><span class="sxs-lookup"><span data-stu-id="1e44d-184">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; Time allowed for the HTTP Server API to drain the entity body on a Keep-Alive connection.</span></span></li><li><span data-ttu-id="1e44d-185">[TimeoutManager. 允許](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody)&ndash; 允許要求實體主體抵達的時間。</span><span class="sxs-lookup"><span data-stu-id="1e44d-185">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; Time allowed for the request entity body to arrive.</span></span></li><li><span data-ttu-id="1e44d-186">[TimeoutManager：](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait)允許 HTTP 伺服器 API 剖析要求標頭所需的允許 &ndash; 時間。</span><span class="sxs-lookup"><span data-stu-id="1e44d-186">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; Time allowed for the HTTP Server API to parse the request header.</span></span></li><li><span data-ttu-id="1e44d-187">[TimeoutManager：](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection)允許閒置連接的 IdleConnection &ndash; 時間。</span><span class="sxs-lookup"><span data-stu-id="1e44d-187">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; Time allowed for an idle connection.</span></span></li><li><span data-ttu-id="1e44d-188">[TimeoutManager。回應](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond)&ndash; 回應的最小傳送速率。</span><span class="sxs-lookup"><span data-stu-id="1e44d-188">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; The minimum send rate for the response.</span></span></li><li><span data-ttu-id="1e44d-189">[TimeoutManager：](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue)允許要求在應用程式拾取之前保留在要求佇列中的 RequestQueue &ndash; 時間。</span><span class="sxs-lookup"><span data-stu-id="1e44d-189">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; Time allowed for the request to remain in the request queue before the app picks it up.</span></span></li></ul> |  |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | <span data-ttu-id="1e44d-190">指定 <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> 以向 HTTP.sys 註冊。</span><span class="sxs-lookup"><span data-stu-id="1e44d-190">Specify the <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> to register with HTTP.sys.</span></span> <span data-ttu-id="1e44d-191">最實用的是 [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*)，可用來將前置詞加入集合。</span><span class="sxs-lookup"><span data-stu-id="1e44d-191">The most useful is [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), which is used to add a prefix to the collection.</span></span> <span data-ttu-id="1e44d-192">處置接聽程式之前可隨時修改這些內容。</span><span class="sxs-lookup"><span data-stu-id="1e44d-192">These may be modified at any time prior to disposing the listener.</span></span> |  |

<a name="maxrequestbodysize"></a>

<span data-ttu-id="1e44d-193">**MaxRequestBodySize**</span><span class="sxs-lookup"><span data-stu-id="1e44d-193">**MaxRequestBodySize**</span></span>

<span data-ttu-id="1e44d-194">任何要求所允許的大小上限 (以位元組為單位)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-194">The maximum allowed size of any request body in bytes.</span></span> <span data-ttu-id="1e44d-195">當設定為 `null` 時，要求主體大小上限為無限制。</span><span class="sxs-lookup"><span data-stu-id="1e44d-195">When set to `null`, the maximum request body size is unlimited.</span></span> <span data-ttu-id="1e44d-196">此限制對升級連線不會有任何影響，因為其一律為無限制。</span><span class="sxs-lookup"><span data-stu-id="1e44d-196">This limit has no effect on upgraded connections, which are always unlimited.</span></span>

<span data-ttu-id="1e44d-197">在 ASP.NET Core MVC 應用程式中針對單一 `IActionResult` 覆寫限制的建議方式，是在動作方法上使用 <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> 屬性：</span><span class="sxs-lookup"><span data-stu-id="1e44d-197">The recommended method to override the limit in an ASP.NET Core MVC app for a single `IActionResult` is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="1e44d-198">如果應用程式已開始讀取要求之後，才嘗試設定要求的限制，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="1e44d-198">An exception is thrown if the app attempts to configure the limit on a request after the app has started reading the request.</span></span> <span data-ttu-id="1e44d-199">`IsReadOnly` 屬性可用來指出 `MaxRequestBodySize` 屬性是否處於唯讀狀態，代表要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="1e44d-199">An `IsReadOnly` property can be used to indicate if the `MaxRequestBodySize` property is in a read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="1e44d-200">如果應用程式應該覆寫每個要求的 <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize>，請使用 <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>：</span><span class="sxs-lookup"><span data-stu-id="1e44d-200">If the app should override <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> per-request, use the <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>:</span></span>

[!code-csharp[](httpsys/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6-7)]

<span data-ttu-id="1e44d-201">如果您使用 Visual Studio，請確定應用程式未設定為執行 IIS 或 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="1e44d-201">If using Visual Studio, make sure the app isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="1e44d-202">在 Visual Studio 中，預設啟動設定檔適用於 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="1e44d-202">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="1e44d-203">若要執行專案作為主控台應用程式，請手動變更選取的設定檔，如下列螢幕擷取畫面所示：</span><span class="sxs-lookup"><span data-stu-id="1e44d-203">To run the project as a console app, manually change the selected profile, as shown in the following screen shot:</span></span>

![選取主控台應用程式設定檔](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a><span data-ttu-id="1e44d-205">設定 Windows Server</span><span class="sxs-lookup"><span data-stu-id="1e44d-205">Configure Windows Server</span></span>

1. <span data-ttu-id="1e44d-206">判斷要為應用程式開啟的連接埠，然後使用 [Windows 防火牆](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule)或 [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) PowerShell Cmdlet 來開啟防火牆連接埠，以允許流量到達 HTTP.sys。</span><span class="sxs-lookup"><span data-stu-id="1e44d-206">Determine the ports to open for the app and use [Windows Firewall](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) or the [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) PowerShell cmdlet to open firewall ports to allow traffic to reach HTTP.sys.</span></span> <span data-ttu-id="1e44d-207">在下列命令和應用程式設定中，會使用連接埠 443。</span><span class="sxs-lookup"><span data-stu-id="1e44d-207">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="1e44d-208">部署至 Azure VM 時，請在[網路安全性群組](/azure/virtual-machines/windows/nsg-quickstart-portal)中開啟連接埠。</span><span class="sxs-lookup"><span data-stu-id="1e44d-208">When deploying to an Azure VM, open the ports in the [Network Security Group](/azure/virtual-machines/windows/nsg-quickstart-portal).</span></span> <span data-ttu-id="1e44d-209">在下列命令和應用程式設定中，會使用連接埠 443。</span><span class="sxs-lookup"><span data-stu-id="1e44d-209">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="1e44d-210">視需要取得並安裝 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-210">Obtain and install X.509 certificates, if required.</span></span>

   <span data-ttu-id="1e44d-211">在 Windows 上，請使用 [New-SelfSignedCertificate PowerShell Cmdlet](/powershell/module/pkiclient/new-selfsignedcertificate) 來建立自我簽署的憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-211">On Windows, create self-signed certificates using the [New-SelfSignedCertificate PowerShell cmdlet](/powershell/module/pkiclient/new-selfsignedcertificate).</span></span> <span data-ttu-id="1e44d-212">如需不支援的範例，請參閱 [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-212">For an unsupported example, see [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).</span></span>

   <span data-ttu-id="1e44d-213">在伺服器的**本機電腦**>**個人**存放區中，安裝自我簽署或 CA 簽署的憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-213">Install either self-signed or CA-signed certificates in the server's **Local Machine** > **Personal** store.</span></span>

1. <span data-ttu-id="1e44d-214">如果應用程式是[與架構相依的部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)，請安裝 .NET Core、.NET Framework 或兩者 (如果應用程式是以 .NET Framework 為目標的 .NET Core 應用程式)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-214">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd), install .NET Core, .NET Framework, or both (if the app is a .NET Core app targeting the .NET Framework).</span></span>

   * <span data-ttu-id="1e44d-215">**.Net core** &ndash; 如果應用程式需要 .net core，請從[.net core 下載](https://dotnet.microsoft.com/download)取得並執行 **.net core 運行**時間安裝程式。</span><span class="sxs-lookup"><span data-stu-id="1e44d-215">**.NET Core** &ndash; If the app requires .NET Core, obtain and run the **.NET Core Runtime** installer from [.NET Core Downloads](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="1e44d-216">請勿在伺服器上安裝完整的 SDK。</span><span class="sxs-lookup"><span data-stu-id="1e44d-216">Don't install the full SDK on the server.</span></span>
   * <span data-ttu-id="1e44d-217">**.NET Framework** &ndash; 如果應用程式需要 .NET Framework，請參閱[.NET Framework 安裝指南](/dotnet/framework/install/)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-217">**.NET Framework** &ndash; If the app requires .NET Framework, see the [.NET Framework installation guide](/dotnet/framework/install/).</span></span> <span data-ttu-id="1e44d-218">安裝必要的 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="1e44d-218">Install the required .NET Framework.</span></span> <span data-ttu-id="1e44d-219">您可以從 [.NET Core 下載](https://dotnet.microsoft.com/download)頁面取得最新 .NET Framework 的安裝程式。</span><span class="sxs-lookup"><span data-stu-id="1e44d-219">The installer for the latest .NET Framework is available from the [.NET Core Downloads](https://dotnet.microsoft.com/download) page.</span></span>

   <span data-ttu-id="1e44d-220">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)，則應用程式的部署中會包含執行階段。</span><span class="sxs-lookup"><span data-stu-id="1e44d-220">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd), the app includes the runtime in its deployment.</span></span> <span data-ttu-id="1e44d-221">不需要在伺服器上安裝任何架構。</span><span class="sxs-lookup"><span data-stu-id="1e44d-221">No framework installation is required on the server.</span></span>

1. <span data-ttu-id="1e44d-222">設定應用程式中的 URL 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="1e44d-222">Configure URLs and ports in the app.</span></span>

   <span data-ttu-id="1e44d-223">ASP.NET Core 預設會繫結至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="1e44d-223">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="1e44d-224">若要設定 URL 首碼和連接埠，選項包括：</span><span class="sxs-lookup"><span data-stu-id="1e44d-224">To configure URL prefixes and ports, options include:</span></span>

   * <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
   * <span data-ttu-id="1e44d-225">`urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="1e44d-225">`urls` command-line argument</span></span>
   * <span data-ttu-id="1e44d-226">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="1e44d-226">`ASPNETCORE_URLS` environment variable</span></span>
   * <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes>

   <span data-ttu-id="1e44d-227">下列程式碼範例示範如何使用 <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> 搭配位於連接埠 443 的伺服器本機 IP 位址 `10.0.0.4`：</span><span class="sxs-lookup"><span data-stu-id="1e44d-227">The following code example shows how to use <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> with the server's local IP address `10.0.0.4` on port 443:</span></span>

   [!code-csharp[](httpsys/samples_snapshot/3.x/Program.cs?highlight=7)]

   <span data-ttu-id="1e44d-228">`UrlPrefixes` 的優點是針對錯誤格式的前置詞會立即產生錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="1e44d-228">An advantage of `UrlPrefixes` is that an error message is generated immediately for improperly formatted prefixes.</span></span>

   <span data-ttu-id="1e44d-229">`UrlPrefixes` 中的設定會覆寫 `UseUrls`/`urls`/`ASPNETCORE_URLS` 設定。</span><span class="sxs-lookup"><span data-stu-id="1e44d-229">The settings in `UrlPrefixes` override `UseUrls`/`urls`/`ASPNETCORE_URLS` settings.</span></span> <span data-ttu-id="1e44d-230">因此，`UseUrls`、`urls` 和 `ASPNETCORE_URLS` 環境變數的優點，是能更輕鬆地在 Kestrel 和 HTTP.sys 之間切換。</span><span class="sxs-lookup"><span data-stu-id="1e44d-230">Therefore, an advantage of `UseUrls`, `urls`, and the `ASPNETCORE_URLS` environment variable is that it's easier to switch between Kestrel and HTTP.sys.</span></span>

   <span data-ttu-id="1e44d-231">HTTP.sys 使用 [HTTP Server API UrlPrefix 字串格式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-231">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

   > [!WARNING]
   > <span data-ttu-id="1e44d-232">請`http://*:80/`勿`http://+:80`使用最上層萬用字元繫結 (**與** )。</span><span class="sxs-lookup"><span data-stu-id="1e44d-232">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="1e44d-233">最上層萬用字元繫結會導致應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="1e44d-233">Top-level wildcard bindings create app security vulnerabilities.</span></span> <span data-ttu-id="1e44d-234">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="1e44d-234">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="1e44d-235">請使用明確的主機名稱或 IP 位址，而不要使用萬用字元。</span><span class="sxs-lookup"><span data-stu-id="1e44d-235">Use explicit host names or IP addresses rather than wildcards.</span></span> <span data-ttu-id="1e44d-236">若您擁有整個父網域 (相對於有弱點的 `*.mysub.com`) 的控制權，則子網域萬用字元繫結 (例如 `*.com`) 便不構成安全性風險。</span><span class="sxs-lookup"><span data-stu-id="1e44d-236">Subdomain wildcard binding (for example, `*.mysub.com`) isn't a security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="1e44d-237">如需詳細資訊，請參閱[RFC 7230：第5.4 節：主機](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-237">For more information, see [RFC 7230: Section 5.4: Host](https://tools.ietf.org/html/rfc7230#section-5.4).</span></span>

1. <span data-ttu-id="1e44d-238">在伺服器上預先註冊 URL 首碼。</span><span class="sxs-lookup"><span data-stu-id="1e44d-238">Preregister URL prefixes on the server.</span></span>

   <span data-ttu-id="1e44d-239">用來設定 HTTP.sys 的內建工具是 *netsh.exe*。</span><span class="sxs-lookup"><span data-stu-id="1e44d-239">The built-in tool for configuring HTTP.sys is *netsh.exe*.</span></span> <span data-ttu-id="1e44d-240">*netsh.exe* 是用來保留 URL 前置詞，並指派 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-240">*netsh.exe* is used to reserve URL prefixes and assign X.509 certificates.</span></span> <span data-ttu-id="1e44d-241">此工具需要系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="1e44d-241">The tool requires administrator privileges.</span></span>

   <span data-ttu-id="1e44d-242">使用 *netsh.exe* 工具來為應用程式註冊 URL：</span><span class="sxs-lookup"><span data-stu-id="1e44d-242">Use the *netsh.exe* tool to register URLs for the app:</span></span>

   ```console
   netsh http add urlacl url=<URL> user=<USER>
   ```

   * <span data-ttu-id="1e44d-243">`<URL>` &ndash; 完整的統一資源定位器（URL）。</span><span class="sxs-lookup"><span data-stu-id="1e44d-243">`<URL>` &ndash; The fully qualified Uniform Resource Locator (URL).</span></span> <span data-ttu-id="1e44d-244">請勿使用萬用字元繫結。</span><span class="sxs-lookup"><span data-stu-id="1e44d-244">Don't use a wildcard binding.</span></span> <span data-ttu-id="1e44d-245">請使用有效的主機名稱或本機 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="1e44d-245">Use a valid hostname or local IP address.</span></span> <span data-ttu-id="1e44d-246">URL 必須包含結尾斜線。</span><span class="sxs-lookup"><span data-stu-id="1e44d-246">*The URL must include a trailing slash.*</span></span>
   * <span data-ttu-id="1e44d-247">`<USER>` &ndash; 指定使用者或使用者組名。</span><span class="sxs-lookup"><span data-stu-id="1e44d-247">`<USER>` &ndash; Specifies the user or user-group name.</span></span>

   <span data-ttu-id="1e44d-248">在以下範例中，伺服器的本機 IP 位址是 `10.0.0.4`：</span><span class="sxs-lookup"><span data-stu-id="1e44d-248">In the following example, the local IP address of the server is `10.0.0.4`:</span></span>

   ```console
   netsh http add urlacl url=https://10.0.0.4:443/ user=Users
   ```

   <span data-ttu-id="1e44d-249">已註冊 URL 時，此工具會以 `URL reservation successfully added` 回應。</span><span class="sxs-lookup"><span data-stu-id="1e44d-249">When a URL is registered, the tool responds with `URL reservation successfully added`.</span></span>

   <span data-ttu-id="1e44d-250">若要刪除已註冊的 URL，請使用 `delete urlacl` 命令：</span><span class="sxs-lookup"><span data-stu-id="1e44d-250">To delete a registered URL, use the `delete urlacl` command:</span></span>

   ```console
   netsh http delete urlacl url=<URL>
   ```

1. <span data-ttu-id="1e44d-251">在伺服器上註冊 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-251">Register X.509 certificates on the server.</span></span>

   <span data-ttu-id="1e44d-252">使用 *netsh.exe* 工具來為應用程式註冊憑證：</span><span class="sxs-lookup"><span data-stu-id="1e44d-252">Use the *netsh.exe* tool to register certificates for the app:</span></span>

   ```console
   netsh http add sslcert ipport=<IP>:<PORT> certhash=<THUMBPRINT> appid="{<GUID>}"
   ```

   * <span data-ttu-id="1e44d-253">`<IP>` &ndash; 指定系結的本機 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="1e44d-253">`<IP>` &ndash; Specifies the local IP address for the binding.</span></span> <span data-ttu-id="1e44d-254">請勿使用萬用字元繫結。</span><span class="sxs-lookup"><span data-stu-id="1e44d-254">Don't use a wildcard binding.</span></span> <span data-ttu-id="1e44d-255">請使用有效的 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="1e44d-255">Use a valid IP address.</span></span>
   * <span data-ttu-id="1e44d-256">`<PORT>` &ndash; 指定系結的埠。</span><span class="sxs-lookup"><span data-stu-id="1e44d-256">`<PORT>` &ndash; Specifies the port for the binding.</span></span>
   * <span data-ttu-id="1e44d-257">`<THUMBPRINT>` &ndash; x.509 憑證指紋。</span><span class="sxs-lookup"><span data-stu-id="1e44d-257">`<THUMBPRINT>` &ndash; The X.509 certificate thumbprint.</span></span>
   * <span data-ttu-id="1e44d-258">`<GUID>` &ndash; 開發人員產生的 GUID，代表應用程式以供參考之用。</span><span class="sxs-lookup"><span data-stu-id="1e44d-258">`<GUID>` &ndash; A developer-generated GUID to represent the app for informational purposes.</span></span>

   <span data-ttu-id="1e44d-259">為了便於參考，請將 GUID 以套件標記的形式儲存在應用程式中：</span><span class="sxs-lookup"><span data-stu-id="1e44d-259">For reference purposes, store the GUID in the app as a package tag:</span></span>

   * <span data-ttu-id="1e44d-260">在 Visual Studio 中：</span><span class="sxs-lookup"><span data-stu-id="1e44d-260">In Visual Studio:</span></span>
     * <span data-ttu-id="1e44d-261">在 [方案總管] 中的專案上按一下滑鼠右鍵，然後選取 [屬性]，以開啟應用程式的專案屬性。</span><span class="sxs-lookup"><span data-stu-id="1e44d-261">Open the app's project properties by right-clicking on the app in **Solution Explorer** and selecting **Properties**.</span></span>
     * <span data-ttu-id="1e44d-262">選取 [套件] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="1e44d-262">Select the **Package** tab.</span></span>
     * <span data-ttu-id="1e44d-263">輸入您在 [標記] 欄位中建立的 GUID。</span><span class="sxs-lookup"><span data-stu-id="1e44d-263">Enter the GUID that you created in the **Tags** field.</span></span>
   * <span data-ttu-id="1e44d-264">不是使用 Visual Studio 時：</span><span class="sxs-lookup"><span data-stu-id="1e44d-264">When not using Visual Studio:</span></span>
     * <span data-ttu-id="1e44d-265">開啟應用程式的專案檔。</span><span class="sxs-lookup"><span data-stu-id="1e44d-265">Open the app's project file.</span></span>
     * <span data-ttu-id="1e44d-266">將 `<PackageTags>` 屬性搭配您所建立的 GUID 新增至新的或現有的 `<PropertyGroup>`：</span><span class="sxs-lookup"><span data-stu-id="1e44d-266">Add a `<PackageTags>` property to a new or existing `<PropertyGroup>` with the GUID that you created:</span></span>

       ```xml
       <PropertyGroup>
         <PackageTags>9412ee86-c21b-4eb8-bd89-f650fbf44931</PackageTags>
       </PropertyGroup>
       ```

   <span data-ttu-id="1e44d-267">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="1e44d-267">In the following example:</span></span>

   * <span data-ttu-id="1e44d-268">伺服器的本機 IP 位址是 `10.0.0.4`。</span><span class="sxs-lookup"><span data-stu-id="1e44d-268">The local IP address of the server is `10.0.0.4`.</span></span>
   * <span data-ttu-id="1e44d-269">線上隨機 GUID 產生器會提供 `appid` 值。</span><span class="sxs-lookup"><span data-stu-id="1e44d-269">An online random GUID generator provides the `appid` value.</span></span>

   ```console
   netsh http add sslcert 
       ipport=10.0.0.4:443 
       certhash=b66ee04419d4ee37464ab8785ff02449980eae10 
       appid="{9412ee86-c21b-4eb8-bd89-f650fbf44931}"
   ```

   <span data-ttu-id="1e44d-270">已註冊憑證時，此工具會以 `SSL Certificate successfully added` 回應。</span><span class="sxs-lookup"><span data-stu-id="1e44d-270">When a certificate is registered, the tool responds with `SSL Certificate successfully added`.</span></span>

   <span data-ttu-id="1e44d-271">若要刪除憑證註冊，請使用 `delete sslcert` 命令：</span><span class="sxs-lookup"><span data-stu-id="1e44d-271">To delete a certificate registration, use the `delete sslcert` command:</span></span>

   ```console
   netsh http delete sslcert ipport=<IP>:<PORT>
   ```

   <span data-ttu-id="1e44d-272">以下是 *netsh.exe* 的參考文件：</span><span class="sxs-lookup"><span data-stu-id="1e44d-272">Reference documentation for *netsh.exe*:</span></span>

   * [<span data-ttu-id="1e44d-273">超文字傳輸通訊協定 (HTTP) 的 Netsh 命令</span><span class="sxs-lookup"><span data-stu-id="1e44d-273">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
   * [<span data-ttu-id="1e44d-274">UrlPrefix 字串</span><span class="sxs-lookup"><span data-stu-id="1e44d-274">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

1. <span data-ttu-id="1e44d-275">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e44d-275">Run the app.</span></span>

   <span data-ttu-id="1e44d-276">使用 HTTP (不是 HTTPS) 搭配大於 1024 的連接埠號碼來繫結至 localhost 時，不需要系統管理員權限即可執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e44d-276">Administrator privileges aren't required to run the app when binding to localhost using HTTP (not HTTPS) with a port number greater than 1024.</span></span> <span data-ttu-id="1e44d-277">針對其他設定 (例如，使用本機 IP 位址或繫結至連接埠 443)，請使用系統管理員權限來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e44d-277">For other configurations (for example, using a local IP address or binding to port 443), run the app with administrator privileges.</span></span>

   <span data-ttu-id="1e44d-278">應用程式會在伺服器的公用 IP 位址回應。</span><span class="sxs-lookup"><span data-stu-id="1e44d-278">The app responds at the server's public IP address.</span></span> <span data-ttu-id="1e44d-279">在此範例中，是從網際網路透過伺服器的公用 IP 位址 `104.214.79.47` 連線至伺服器。</span><span class="sxs-lookup"><span data-stu-id="1e44d-279">In this example, the server is reached from the Internet at its public IP address of `104.214.79.47`.</span></span>

   <span data-ttu-id="1e44d-280">在此範例中使用的是開發憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-280">A development certificate is used in this example.</span></span> <span data-ttu-id="1e44d-281">在略過瀏覽器的未受信任憑證警告之後，頁面會安全地載入。</span><span class="sxs-lookup"><span data-stu-id="1e44d-281">The page loads securely after bypassing the browser's untrusted certificate warning.</span></span>

   ![顯示已載入應用程式索引頁面的瀏覽器視窗](httpsys/_static/browser.png)

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="1e44d-283">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="1e44d-283">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="1e44d-284">如果是 HTTP.sys 所裝載且與來自網際網路或公司網路的要求進行互動的應用程式，在裝載於 Proxy 伺服器和負載平衡器後方時，可能需要額外的組態。</span><span class="sxs-lookup"><span data-stu-id="1e44d-284">For apps hosted by HTTP.sys that interact with requests from the Internet or a corporate network, additional configuration might be required when hosting behind proxy servers and load balancers.</span></span> <span data-ttu-id="1e44d-285">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-285">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1e44d-286">其他資源</span><span class="sxs-lookup"><span data-stu-id="1e44d-286">Additional resources</span></span>

* <span data-ttu-id="1e44d-287">[使用 HTTP.sys 來啟用 Windows 驗證](xref:security/authentication/windowsauth#httpsys) \(機器翻譯\)</span><span class="sxs-lookup"><span data-stu-id="1e44d-287">[Enable Windows Authentication with HTTP.sys](xref:security/authentication/windowsauth#httpsys)</span></span>
* <span data-ttu-id="1e44d-288">[HTTP 伺服器 API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="1e44d-288">[HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)</span></span>
* <span data-ttu-id="1e44d-289">[aspnet/HttpSysServer GitHub 存放庫 (原始程式碼)](https://github.com/aspnet/HttpSysServer/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="1e44d-289">[aspnet/HttpSysServer GitHub repository (source code)](https://github.com/aspnet/HttpSysServer/)</span></span>
* [<span data-ttu-id="1e44d-290">主機</span><span class="sxs-lookup"><span data-stu-id="1e44d-290">The host</span></span>](xref:fundamentals/index#host)
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="1e44d-291">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) 是只在 Windows 上執行的 [ASP.NET Core 網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-291">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) is a [web server for ASP.NET Core](xref:fundamentals/servers/index) that only runs on Windows.</span></span> <span data-ttu-id="1e44d-292">HTTP.sys 是 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器的替代方式，提供了一些 Kestrel 未提供的功能。</span><span class="sxs-lookup"><span data-stu-id="1e44d-292">HTTP.sys is an alternative to [Kestrel](xref:fundamentals/servers/kestrel) server and offers some features that Kestrel doesn't provide.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1e44d-293">HTTP.sys 與 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)不相容，且不能與 IIS 或 IIS Express 搭配使用。</span><span class="sxs-lookup"><span data-stu-id="1e44d-293">HTTP.sys isn't compatible with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and can't be used with IIS or IIS Express.</span></span>

<span data-ttu-id="1e44d-294">HTTP.sys 支援下列功能：</span><span class="sxs-lookup"><span data-stu-id="1e44d-294">HTTP.sys supports the following features:</span></span>

* [<span data-ttu-id="1e44d-295">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="1e44d-295">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
* <span data-ttu-id="1e44d-296">連接埠共用</span><span class="sxs-lookup"><span data-stu-id="1e44d-296">Port sharing</span></span>
* <span data-ttu-id="1e44d-297">使用 SNI 的 HTTPS</span><span class="sxs-lookup"><span data-stu-id="1e44d-297">HTTPS with SNI</span></span>
* <span data-ttu-id="1e44d-298">透過 TLS 的 HTTP/2 (Windows 10 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="1e44d-298">HTTP/2 over TLS (Windows 10 or later)</span></span>
* <span data-ttu-id="1e44d-299">直接檔案傳輸</span><span class="sxs-lookup"><span data-stu-id="1e44d-299">Direct file transmission</span></span>
* <span data-ttu-id="1e44d-300">回應快取</span><span class="sxs-lookup"><span data-stu-id="1e44d-300">Response caching</span></span>
* <span data-ttu-id="1e44d-301">WebSocket (Windows 8 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="1e44d-301">WebSockets (Windows 8 or later)</span></span>

<span data-ttu-id="1e44d-302">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="1e44d-302">Supported Windows versions:</span></span>

* <span data-ttu-id="1e44d-303">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="1e44d-303">Windows 7 or later</span></span>
* <span data-ttu-id="1e44d-304">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="1e44d-304">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="1e44d-305">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1e44d-305">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="1e44d-306">使用 HTTP.sys 的時機</span><span class="sxs-lookup"><span data-stu-id="1e44d-306">When to use HTTP.sys</span></span>

<span data-ttu-id="1e44d-307">HTTP.sys 在下列部署環境中非常有用：</span><span class="sxs-lookup"><span data-stu-id="1e44d-307">HTTP.sys is useful for deployments where:</span></span>

* <span data-ttu-id="1e44d-308">需要直接向網際網路公開伺服器而不使用 IIS。</span><span class="sxs-lookup"><span data-stu-id="1e44d-308">There's a need to expose the server directly to the Internet without using IIS.</span></span>

  ![HTTP.sys 直接與網際網路通訊](httpsys/_static/httpsys-to-internet.png)

* <span data-ttu-id="1e44d-310">內部部署需要的功能無法在 Kestrel 中使用，例如 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-310">An internal deployment requires a feature not available in Kestrel, such as [Windows Authentication](xref:security/authentication/windowsauth).</span></span>

  ![HTTP.sys 直接與內部網路通訊](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="1e44d-312">HTTP.sys 是成熟的技術，可抵禦許多種類的攻擊，並提供功能完整之網頁伺服器的穩固性、安全性及延展性。</span><span class="sxs-lookup"><span data-stu-id="1e44d-312">HTTP.sys is mature technology that protects against many types of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="1e44d-313">IIS 本身在 HTTP.sys 之上以 HTTP 接聽程式的形式執行。</span><span class="sxs-lookup"><span data-stu-id="1e44d-313">IIS itself runs as an HTTP listener on top of HTTP.sys.</span></span>

## <a name="http2-support"></a><span data-ttu-id="1e44d-314">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="1e44d-314">HTTP/2 support</span></span>

<span data-ttu-id="1e44d-315">如果符合下列基本需求，則可以針對 ASP.NET Core 應用程式啟用 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="1e44d-315">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is enabled for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="1e44d-316">Windows Server 2016/Windows 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="1e44d-316">Windows Server 2016/Windows 10 or later</span></span>
* <span data-ttu-id="1e44d-317">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 連線</span><span class="sxs-lookup"><span data-stu-id="1e44d-317">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="1e44d-318">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="1e44d-318">TLS 1.2 or later connection</span></span>

<span data-ttu-id="1e44d-319">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="1e44d-319">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="1e44d-320">HTTP/2 預設為啟用。</span><span class="sxs-lookup"><span data-stu-id="1e44d-320">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="1e44d-321">如果 HTTP/2 連線尚未建立，連線會退為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="1e44d-321">If an HTTP/2 connection isn't established, the connection falls back to HTTP/1.1.</span></span> <span data-ttu-id="1e44d-322">Windows 的未來版本會推出 HTTP/2 設定旗標，包括使用 HTTP.sys 來停用 HTTP/2 的功能。</span><span class="sxs-lookup"><span data-stu-id="1e44d-322">In a future release of Windows, HTTP/2 configuration flags will be available, including the ability to disable HTTP/2 with HTTP.sys.</span></span>

## <a name="kernel-mode-authentication-with-kerberos"></a><span data-ttu-id="1e44d-323">使用 Kerberos 的核心模式驗證</span><span class="sxs-lookup"><span data-stu-id="1e44d-323">Kernel mode authentication with Kerberos</span></span>

<span data-ttu-id="1e44d-324">HTTP.sys 使用 Kerberos 驗證通訊協定委派給核心模式驗證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-324">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="1e44d-325">Kerberos 和 HTTP.sys 不支援使用者模式驗證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-325">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="1e44d-326">必須使用電腦帳戶來解密 Kerberos 權杖/票證，該權杖/票證取自 Active Directory，並由用戶端將其轉送至伺服器來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="1e44d-326">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="1e44d-327">請註冊主機的服務主體名稱 (SPN)，而非應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="1e44d-327">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

## <a name="how-to-use-httpsys"></a><span data-ttu-id="1e44d-328">如何使用 HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="1e44d-328">How to use HTTP.sys</span></span>

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a><span data-ttu-id="1e44d-329">設定 ASP.NET Core 應用程式使用 HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="1e44d-329">Configure the ASP.NET Core app to use HTTP.sys</span></span>

<span data-ttu-id="1e44d-330">建置主機時，呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> 擴充方法，並指定任何必要的 <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>。</span><span class="sxs-lookup"><span data-stu-id="1e44d-330">Call the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> extension method when building the host, specifying any required <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>.</span></span> <span data-ttu-id="1e44d-331">下列範例會將選項設定為它們的預設值：</span><span class="sxs-lookup"><span data-stu-id="1e44d-331">The following example sets options to their default values:</span></span>

[!code-csharp[](httpsys/samples/3.x/SampleApp/Program.cs?name=snippet1&highlight=5-13)]

<span data-ttu-id="1e44d-332">其他的 HTTP.sys 設定則透過[登錄設定](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows)處理。</span><span class="sxs-lookup"><span data-stu-id="1e44d-332">Additional HTTP.sys configuration is handled through [registry settings](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).</span></span>

<span data-ttu-id="1e44d-333">**HTTP.sys 選項**</span><span class="sxs-lookup"><span data-stu-id="1e44d-333">**HTTP.sys options**</span></span>

| <span data-ttu-id="1e44d-334">屬性</span><span class="sxs-lookup"><span data-stu-id="1e44d-334">Property</span></span> | <span data-ttu-id="1e44d-335">描述</span><span class="sxs-lookup"><span data-stu-id="1e44d-335">Description</span></span> | <span data-ttu-id="1e44d-336">預設</span><span class="sxs-lookup"><span data-stu-id="1e44d-336">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="1e44d-337">AllowSynchronousIO</span><span class="sxs-lookup"><span data-stu-id="1e44d-337">AllowSynchronousIO</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | <span data-ttu-id="1e44d-338">控制是否允許 `HttpContext.Request.Body` 和 `HttpContext.Response.Body` 同步輸出/輸入。</span><span class="sxs-lookup"><span data-stu-id="1e44d-338">Control whether synchronous input/output is allowed for the `HttpContext.Request.Body` and `HttpContext.Response.Body`.</span></span> | `false` |
| [<span data-ttu-id="1e44d-339">Authentication.AllowAnonymous</span><span class="sxs-lookup"><span data-stu-id="1e44d-339">Authentication.AllowAnonymous</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | <span data-ttu-id="1e44d-340">允許匿名要求。</span><span class="sxs-lookup"><span data-stu-id="1e44d-340">Allow anonymous requests.</span></span> | `true` |
| [<span data-ttu-id="1e44d-341">Authentication.Schemes</span><span class="sxs-lookup"><span data-stu-id="1e44d-341">Authentication.Schemes</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | <span data-ttu-id="1e44d-342">指定允許的驗證配置。</span><span class="sxs-lookup"><span data-stu-id="1e44d-342">Specify the allowed authentication schemes.</span></span> <span data-ttu-id="1e44d-343">處置接聽程式之前可隨時修改。</span><span class="sxs-lookup"><span data-stu-id="1e44d-343">May be modified at any time prior to disposing the listener.</span></span> <span data-ttu-id="1e44d-344">值是由 [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes) 提供：`Basic`、`Kerberos`、`Negotiate`、`None` 和 `NTLM`。</span><span class="sxs-lookup"><span data-stu-id="1e44d-344">Values are provided by the [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None`, and `NTLM`.</span></span> | `None` |
| [<span data-ttu-id="1e44d-345">EnableResponseCaching</span><span class="sxs-lookup"><span data-stu-id="1e44d-345">EnableResponseCaching</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | <span data-ttu-id="1e44d-346">針對含有合格標頭的回應嘗試[核心模式](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode)快取。</span><span class="sxs-lookup"><span data-stu-id="1e44d-346">Attempt [kernel-mode](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) caching for responses with eligible headers.</span></span> <span data-ttu-id="1e44d-347">回應可能不包含 `Set-Cookie`、`Vary` 或 `Pragma` 標頭。</span><span class="sxs-lookup"><span data-stu-id="1e44d-347">The response may not include `Set-Cookie`, `Vary`, or `Pragma` headers.</span></span> <span data-ttu-id="1e44d-348">它必須包含為 `Cache-Control` 的 `public` 標頭，且有 `shared-max-age` 或 `max-age` 值，或是 `Expires` 標頭。</span><span class="sxs-lookup"><span data-stu-id="1e44d-348">It must include a `Cache-Control` header that's `public` and either a `shared-max-age` or `max-age` value, or an `Expires` header.</span></span> | `true` |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | <span data-ttu-id="1e44d-349">可同時接受的數目上限。</span><span class="sxs-lookup"><span data-stu-id="1e44d-349">The maximum number of concurrent accepts.</span></span> | <span data-ttu-id="1e44d-350">5 &times;[環境。<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span><span class="sxs-lookup"><span data-stu-id="1e44d-350">5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | <span data-ttu-id="1e44d-351">可接受的同時連線數量上限。</span><span class="sxs-lookup"><span data-stu-id="1e44d-351">The maximum number of concurrent connections to accept.</span></span> <span data-ttu-id="1e44d-352">使用 `-1` 為無限多個。</span><span class="sxs-lookup"><span data-stu-id="1e44d-352">Use `-1` for infinite.</span></span> <span data-ttu-id="1e44d-353">使用 `null` 以使用登錄之整個電腦的設定。</span><span class="sxs-lookup"><span data-stu-id="1e44d-353">Use `null` to use the registry's machine-wide setting.</span></span> | `null`<br><span data-ttu-id="1e44d-354">（全電腦</span><span class="sxs-lookup"><span data-stu-id="1e44d-354">(machine-wide</span></span><br><span data-ttu-id="1e44d-355">設</span><span class="sxs-lookup"><span data-stu-id="1e44d-355">setting)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | <span data-ttu-id="1e44d-356">請參閱 <a href="#maxrequestbodysize">MaxRequestBodySize</a> 小節。</span><span class="sxs-lookup"><span data-stu-id="1e44d-356">See the <a href="#maxrequestbodysize">MaxRequestBodySize</a> section.</span></span> | <span data-ttu-id="1e44d-357">30000000 位元組</span><span class="sxs-lookup"><span data-stu-id="1e44d-357">30000000 bytes</span></span><br><span data-ttu-id="1e44d-358">(~28.6 MB)</span><span class="sxs-lookup"><span data-stu-id="1e44d-358">(~28.6 MB)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | <span data-ttu-id="1e44d-359">可以加入佇列的最大要求數目。</span><span class="sxs-lookup"><span data-stu-id="1e44d-359">The maximum number of requests that can be queued.</span></span> | <span data-ttu-id="1e44d-360">1000</span><span class="sxs-lookup"><span data-stu-id="1e44d-360">1000</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | <span data-ttu-id="1e44d-361">指出若回應本文因為用戶端中斷連線而寫入失敗時，應擲回例外狀況或正常完成。</span><span class="sxs-lookup"><span data-stu-id="1e44d-361">Indicate if response body writes that fail due to client disconnects should throw exceptions or complete normally.</span></span> | `false`<br><span data-ttu-id="1e44d-362">(正常完成)</span><span class="sxs-lookup"><span data-stu-id="1e44d-362">(complete normally)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | <span data-ttu-id="1e44d-363">公開 HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> 設定，這也可在登錄中設定。</span><span class="sxs-lookup"><span data-stu-id="1e44d-363">Expose the HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> configuration, which may also be configured in the registry.</span></span> <span data-ttu-id="1e44d-364">API 連結可提供包括預設值在內每個設定的詳細資訊：</span><span class="sxs-lookup"><span data-stu-id="1e44d-364">Follow the API links to learn more about each setting, including default values:</span></span><ul><li><span data-ttu-id="1e44d-365">[TimeoutManager。](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) HTTP 伺服器 API 允許的 DrainEntityBody &ndash; 時間，以在保持連線的狀態中清空實體主體。</span><span class="sxs-lookup"><span data-stu-id="1e44d-365">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; Time allowed for the HTTP Server API to drain the entity body on a Keep-Alive connection.</span></span></li><li><span data-ttu-id="1e44d-366">[TimeoutManager. 允許](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody)&ndash; 允許要求實體主體抵達的時間。</span><span class="sxs-lookup"><span data-stu-id="1e44d-366">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; Time allowed for the request entity body to arrive.</span></span></li><li><span data-ttu-id="1e44d-367">[TimeoutManager：](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait)允許 HTTP 伺服器 API 剖析要求標頭所需的允許 &ndash; 時間。</span><span class="sxs-lookup"><span data-stu-id="1e44d-367">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; Time allowed for the HTTP Server API to parse the request header.</span></span></li><li><span data-ttu-id="1e44d-368">[TimeoutManager：](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection)允許閒置連接的 IdleConnection &ndash; 時間。</span><span class="sxs-lookup"><span data-stu-id="1e44d-368">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; Time allowed for an idle connection.</span></span></li><li><span data-ttu-id="1e44d-369">[TimeoutManager。回應](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond)&ndash; 回應的最小傳送速率。</span><span class="sxs-lookup"><span data-stu-id="1e44d-369">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; The minimum send rate for the response.</span></span></li><li><span data-ttu-id="1e44d-370">[TimeoutManager：](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue)允許要求在應用程式拾取之前保留在要求佇列中的 RequestQueue &ndash; 時間。</span><span class="sxs-lookup"><span data-stu-id="1e44d-370">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; Time allowed for the request to remain in the request queue before the app picks it up.</span></span></li></ul> |  |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | <span data-ttu-id="1e44d-371">指定 <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> 以向 HTTP.sys 註冊。</span><span class="sxs-lookup"><span data-stu-id="1e44d-371">Specify the <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> to register with HTTP.sys.</span></span> <span data-ttu-id="1e44d-372">最實用的是 [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*)，可用來將前置詞加入集合。</span><span class="sxs-lookup"><span data-stu-id="1e44d-372">The most useful is [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), which is used to add a prefix to the collection.</span></span> <span data-ttu-id="1e44d-373">處置接聽程式之前可隨時修改這些內容。</span><span class="sxs-lookup"><span data-stu-id="1e44d-373">These may be modified at any time prior to disposing the listener.</span></span> |  |

<a name="maxrequestbodysize"></a>

<span data-ttu-id="1e44d-374">**MaxRequestBodySize**</span><span class="sxs-lookup"><span data-stu-id="1e44d-374">**MaxRequestBodySize**</span></span>

<span data-ttu-id="1e44d-375">任何要求所允許的大小上限 (以位元組為單位)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-375">The maximum allowed size of any request body in bytes.</span></span> <span data-ttu-id="1e44d-376">當設定為 `null` 時，要求主體大小上限為無限制。</span><span class="sxs-lookup"><span data-stu-id="1e44d-376">When set to `null`, the maximum request body size is unlimited.</span></span> <span data-ttu-id="1e44d-377">此限制對升級連線不會有任何影響，因為其一律為無限制。</span><span class="sxs-lookup"><span data-stu-id="1e44d-377">This limit has no effect on upgraded connections, which are always unlimited.</span></span>

<span data-ttu-id="1e44d-378">在 ASP.NET Core MVC 應用程式中針對單一 `IActionResult` 覆寫限制的建議方式，是在動作方法上使用 <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> 屬性：</span><span class="sxs-lookup"><span data-stu-id="1e44d-378">The recommended method to override the limit in an ASP.NET Core MVC app for a single `IActionResult` is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="1e44d-379">如果應用程式已開始讀取要求之後，才嘗試設定要求的限制，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="1e44d-379">An exception is thrown if the app attempts to configure the limit on a request after the app has started reading the request.</span></span> <span data-ttu-id="1e44d-380">`IsReadOnly` 屬性可用來指出 `MaxRequestBodySize` 屬性是否處於唯讀狀態，代表要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="1e44d-380">An `IsReadOnly` property can be used to indicate if the `MaxRequestBodySize` property is in a read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="1e44d-381">如果應用程式應該覆寫每個要求的 <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize>，請使用 <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>：</span><span class="sxs-lookup"><span data-stu-id="1e44d-381">If the app should override <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> per-request, use the <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>:</span></span>

[!code-csharp[](httpsys/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6-7)]

<span data-ttu-id="1e44d-382">如果您使用 Visual Studio，請確定應用程式未設定為執行 IIS 或 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="1e44d-382">If using Visual Studio, make sure the app isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="1e44d-383">在 Visual Studio 中，預設啟動設定檔適用於 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="1e44d-383">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="1e44d-384">若要執行專案作為主控台應用程式，請手動變更選取的設定檔，如下列螢幕擷取畫面所示：</span><span class="sxs-lookup"><span data-stu-id="1e44d-384">To run the project as a console app, manually change the selected profile, as shown in the following screen shot:</span></span>

![選取主控台應用程式設定檔](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a><span data-ttu-id="1e44d-386">設定 Windows Server</span><span class="sxs-lookup"><span data-stu-id="1e44d-386">Configure Windows Server</span></span>

1. <span data-ttu-id="1e44d-387">判斷要為應用程式開啟的連接埠，然後使用 [Windows 防火牆](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule)或 [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) PowerShell Cmdlet 來開啟防火牆連接埠，以允許流量到達 HTTP.sys。</span><span class="sxs-lookup"><span data-stu-id="1e44d-387">Determine the ports to open for the app and use [Windows Firewall](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) or the [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) PowerShell cmdlet to open firewall ports to allow traffic to reach HTTP.sys.</span></span> <span data-ttu-id="1e44d-388">在下列命令和應用程式設定中，會使用連接埠 443。</span><span class="sxs-lookup"><span data-stu-id="1e44d-388">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="1e44d-389">部署至 Azure VM 時，請在[網路安全性群組](/azure/virtual-machines/windows/nsg-quickstart-portal)中開啟連接埠。</span><span class="sxs-lookup"><span data-stu-id="1e44d-389">When deploying to an Azure VM, open the ports in the [Network Security Group](/azure/virtual-machines/windows/nsg-quickstart-portal).</span></span> <span data-ttu-id="1e44d-390">在下列命令和應用程式設定中，會使用連接埠 443。</span><span class="sxs-lookup"><span data-stu-id="1e44d-390">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="1e44d-391">視需要取得並安裝 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-391">Obtain and install X.509 certificates, if required.</span></span>

   <span data-ttu-id="1e44d-392">在 Windows 上，請使用 [New-SelfSignedCertificate PowerShell Cmdlet](/powershell/module/pkiclient/new-selfsignedcertificate) 來建立自我簽署的憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-392">On Windows, create self-signed certificates using the [New-SelfSignedCertificate PowerShell cmdlet](/powershell/module/pkiclient/new-selfsignedcertificate).</span></span> <span data-ttu-id="1e44d-393">如需不支援的範例，請參閱 [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-393">For an unsupported example, see [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).</span></span>

   <span data-ttu-id="1e44d-394">在伺服器的**本機電腦**>**個人**存放區中，安裝自我簽署或 CA 簽署的憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-394">Install either self-signed or CA-signed certificates in the server's **Local Machine** > **Personal** store.</span></span>

1. <span data-ttu-id="1e44d-395">如果應用程式是[與架構相依的部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)，請安裝 .NET Core、.NET Framework 或兩者 (如果應用程式是以 .NET Framework 為目標的 .NET Core 應用程式)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-395">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd), install .NET Core, .NET Framework, or both (if the app is a .NET Core app targeting the .NET Framework).</span></span>

   * <span data-ttu-id="1e44d-396">**.Net core** &ndash; 如果應用程式需要 .net core，請從[.net core 下載](https://dotnet.microsoft.com/download)取得並執行 **.net core 運行**時間安裝程式。</span><span class="sxs-lookup"><span data-stu-id="1e44d-396">**.NET Core** &ndash; If the app requires .NET Core, obtain and run the **.NET Core Runtime** installer from [.NET Core Downloads](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="1e44d-397">請勿在伺服器上安裝完整的 SDK。</span><span class="sxs-lookup"><span data-stu-id="1e44d-397">Don't install the full SDK on the server.</span></span>
   * <span data-ttu-id="1e44d-398">**.NET Framework** &ndash; 如果應用程式需要 .NET Framework，請參閱[.NET Framework 安裝指南](/dotnet/framework/install/)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-398">**.NET Framework** &ndash; If the app requires .NET Framework, see the [.NET Framework installation guide](/dotnet/framework/install/).</span></span> <span data-ttu-id="1e44d-399">安裝必要的 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="1e44d-399">Install the required .NET Framework.</span></span> <span data-ttu-id="1e44d-400">您可以從 [.NET Core 下載](https://dotnet.microsoft.com/download)頁面取得最新 .NET Framework 的安裝程式。</span><span class="sxs-lookup"><span data-stu-id="1e44d-400">The installer for the latest .NET Framework is available from the [.NET Core Downloads](https://dotnet.microsoft.com/download) page.</span></span>

   <span data-ttu-id="1e44d-401">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)，則應用程式的部署中會包含執行階段。</span><span class="sxs-lookup"><span data-stu-id="1e44d-401">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd), the app includes the runtime in its deployment.</span></span> <span data-ttu-id="1e44d-402">不需要在伺服器上安裝任何架構。</span><span class="sxs-lookup"><span data-stu-id="1e44d-402">No framework installation is required on the server.</span></span>

1. <span data-ttu-id="1e44d-403">設定應用程式中的 URL 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="1e44d-403">Configure URLs and ports in the app.</span></span>

   <span data-ttu-id="1e44d-404">ASP.NET Core 預設會繫結至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="1e44d-404">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="1e44d-405">若要設定 URL 首碼和連接埠，選項包括：</span><span class="sxs-lookup"><span data-stu-id="1e44d-405">To configure URL prefixes and ports, options include:</span></span>

   * <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
   * <span data-ttu-id="1e44d-406">`urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="1e44d-406">`urls` command-line argument</span></span>
   * <span data-ttu-id="1e44d-407">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="1e44d-407">`ASPNETCORE_URLS` environment variable</span></span>
   * <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes>

   <span data-ttu-id="1e44d-408">下列程式碼範例示範如何使用 <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> 搭配位於連接埠 443 的伺服器本機 IP 位址 `10.0.0.4`：</span><span class="sxs-lookup"><span data-stu-id="1e44d-408">The following code example shows how to use <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> with the server's local IP address `10.0.0.4` on port 443:</span></span>

   [!code-csharp[](httpsys/samples_snapshot/3.x/Program.cs?highlight=7)]

   <span data-ttu-id="1e44d-409">`UrlPrefixes` 的優點是針對錯誤格式的前置詞會立即產生錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="1e44d-409">An advantage of `UrlPrefixes` is that an error message is generated immediately for improperly formatted prefixes.</span></span>

   <span data-ttu-id="1e44d-410">`UrlPrefixes` 中的設定會覆寫 `UseUrls`/`urls`/`ASPNETCORE_URLS` 設定。</span><span class="sxs-lookup"><span data-stu-id="1e44d-410">The settings in `UrlPrefixes` override `UseUrls`/`urls`/`ASPNETCORE_URLS` settings.</span></span> <span data-ttu-id="1e44d-411">因此，`UseUrls`、`urls` 和 `ASPNETCORE_URLS` 環境變數的優點，是能更輕鬆地在 Kestrel 和 HTTP.sys 之間切換。</span><span class="sxs-lookup"><span data-stu-id="1e44d-411">Therefore, an advantage of `UseUrls`, `urls`, and the `ASPNETCORE_URLS` environment variable is that it's easier to switch between Kestrel and HTTP.sys.</span></span>

   <span data-ttu-id="1e44d-412">HTTP.sys 使用 [HTTP Server API UrlPrefix 字串格式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-412">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

   > [!WARNING]
   > <span data-ttu-id="1e44d-413">請`http://*:80/`勿`http://+:80`使用最上層萬用字元繫結 (**與** )。</span><span class="sxs-lookup"><span data-stu-id="1e44d-413">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="1e44d-414">最上層萬用字元繫結會導致應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="1e44d-414">Top-level wildcard bindings create app security vulnerabilities.</span></span> <span data-ttu-id="1e44d-415">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="1e44d-415">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="1e44d-416">請使用明確的主機名稱或 IP 位址，而不要使用萬用字元。</span><span class="sxs-lookup"><span data-stu-id="1e44d-416">Use explicit host names or IP addresses rather than wildcards.</span></span> <span data-ttu-id="1e44d-417">若您擁有整個父網域 (相對於有弱點的 `*.mysub.com`) 的控制權，則子網域萬用字元繫結 (例如 `*.com`) 便不構成安全性風險。</span><span class="sxs-lookup"><span data-stu-id="1e44d-417">Subdomain wildcard binding (for example, `*.mysub.com`) isn't a security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="1e44d-418">如需詳細資訊，請參閱[RFC 7230：第5.4 節：主機](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-418">For more information, see [RFC 7230: Section 5.4: Host](https://tools.ietf.org/html/rfc7230#section-5.4).</span></span>

1. <span data-ttu-id="1e44d-419">在伺服器上預先註冊 URL 首碼。</span><span class="sxs-lookup"><span data-stu-id="1e44d-419">Preregister URL prefixes on the server.</span></span>

   <span data-ttu-id="1e44d-420">用來設定 HTTP.sys 的內建工具是 *netsh.exe*。</span><span class="sxs-lookup"><span data-stu-id="1e44d-420">The built-in tool for configuring HTTP.sys is *netsh.exe*.</span></span> <span data-ttu-id="1e44d-421">*netsh.exe* 是用來保留 URL 前置詞，並指派 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-421">*netsh.exe* is used to reserve URL prefixes and assign X.509 certificates.</span></span> <span data-ttu-id="1e44d-422">此工具需要系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="1e44d-422">The tool requires administrator privileges.</span></span>

   <span data-ttu-id="1e44d-423">使用 *netsh.exe* 工具來為應用程式註冊 URL：</span><span class="sxs-lookup"><span data-stu-id="1e44d-423">Use the *netsh.exe* tool to register URLs for the app:</span></span>

   ```console
   netsh http add urlacl url=<URL> user=<USER>
   ```

   * <span data-ttu-id="1e44d-424">`<URL>` &ndash; 完整的統一資源定位器（URL）。</span><span class="sxs-lookup"><span data-stu-id="1e44d-424">`<URL>` &ndash; The fully qualified Uniform Resource Locator (URL).</span></span> <span data-ttu-id="1e44d-425">請勿使用萬用字元繫結。</span><span class="sxs-lookup"><span data-stu-id="1e44d-425">Don't use a wildcard binding.</span></span> <span data-ttu-id="1e44d-426">請使用有效的主機名稱或本機 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="1e44d-426">Use a valid hostname or local IP address.</span></span> <span data-ttu-id="1e44d-427">URL 必須包含結尾斜線。</span><span class="sxs-lookup"><span data-stu-id="1e44d-427">*The URL must include a trailing slash.*</span></span>
   * <span data-ttu-id="1e44d-428">`<USER>` &ndash; 指定使用者或使用者組名。</span><span class="sxs-lookup"><span data-stu-id="1e44d-428">`<USER>` &ndash; Specifies the user or user-group name.</span></span>

   <span data-ttu-id="1e44d-429">在以下範例中，伺服器的本機 IP 位址是 `10.0.0.4`：</span><span class="sxs-lookup"><span data-stu-id="1e44d-429">In the following example, the local IP address of the server is `10.0.0.4`:</span></span>

   ```console
   netsh http add urlacl url=https://10.0.0.4:443/ user=Users
   ```

   <span data-ttu-id="1e44d-430">已註冊 URL 時，此工具會以 `URL reservation successfully added` 回應。</span><span class="sxs-lookup"><span data-stu-id="1e44d-430">When a URL is registered, the tool responds with `URL reservation successfully added`.</span></span>

   <span data-ttu-id="1e44d-431">若要刪除已註冊的 URL，請使用 `delete urlacl` 命令：</span><span class="sxs-lookup"><span data-stu-id="1e44d-431">To delete a registered URL, use the `delete urlacl` command:</span></span>

   ```console
   netsh http delete urlacl url=<URL>
   ```

1. <span data-ttu-id="1e44d-432">在伺服器上註冊 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-432">Register X.509 certificates on the server.</span></span>

   <span data-ttu-id="1e44d-433">使用 *netsh.exe* 工具來為應用程式註冊憑證：</span><span class="sxs-lookup"><span data-stu-id="1e44d-433">Use the *netsh.exe* tool to register certificates for the app:</span></span>

   ```console
   netsh http add sslcert ipport=<IP>:<PORT> certhash=<THUMBPRINT> appid="{<GUID>}"
   ```

   * <span data-ttu-id="1e44d-434">`<IP>` &ndash; 指定系結的本機 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="1e44d-434">`<IP>` &ndash; Specifies the local IP address for the binding.</span></span> <span data-ttu-id="1e44d-435">請勿使用萬用字元繫結。</span><span class="sxs-lookup"><span data-stu-id="1e44d-435">Don't use a wildcard binding.</span></span> <span data-ttu-id="1e44d-436">請使用有效的 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="1e44d-436">Use a valid IP address.</span></span>
   * <span data-ttu-id="1e44d-437">`<PORT>` &ndash; 指定系結的埠。</span><span class="sxs-lookup"><span data-stu-id="1e44d-437">`<PORT>` &ndash; Specifies the port for the binding.</span></span>
   * <span data-ttu-id="1e44d-438">`<THUMBPRINT>` &ndash; x.509 憑證指紋。</span><span class="sxs-lookup"><span data-stu-id="1e44d-438">`<THUMBPRINT>` &ndash; The X.509 certificate thumbprint.</span></span>
   * <span data-ttu-id="1e44d-439">`<GUID>` &ndash; 開發人員產生的 GUID，代表應用程式以供參考之用。</span><span class="sxs-lookup"><span data-stu-id="1e44d-439">`<GUID>` &ndash; A developer-generated GUID to represent the app for informational purposes.</span></span>

   <span data-ttu-id="1e44d-440">為了便於參考，請將 GUID 以套件標記的形式儲存在應用程式中：</span><span class="sxs-lookup"><span data-stu-id="1e44d-440">For reference purposes, store the GUID in the app as a package tag:</span></span>

   * <span data-ttu-id="1e44d-441">在 Visual Studio 中：</span><span class="sxs-lookup"><span data-stu-id="1e44d-441">In Visual Studio:</span></span>
     * <span data-ttu-id="1e44d-442">在 [方案總管] 中的專案上按一下滑鼠右鍵，然後選取 [屬性]，以開啟應用程式的專案屬性。</span><span class="sxs-lookup"><span data-stu-id="1e44d-442">Open the app's project properties by right-clicking on the app in **Solution Explorer** and selecting **Properties**.</span></span>
     * <span data-ttu-id="1e44d-443">選取 [套件] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="1e44d-443">Select the **Package** tab.</span></span>
     * <span data-ttu-id="1e44d-444">輸入您在 [標記] 欄位中建立的 GUID。</span><span class="sxs-lookup"><span data-stu-id="1e44d-444">Enter the GUID that you created in the **Tags** field.</span></span>
   * <span data-ttu-id="1e44d-445">不是使用 Visual Studio 時：</span><span class="sxs-lookup"><span data-stu-id="1e44d-445">When not using Visual Studio:</span></span>
     * <span data-ttu-id="1e44d-446">開啟應用程式的專案檔。</span><span class="sxs-lookup"><span data-stu-id="1e44d-446">Open the app's project file.</span></span>
     * <span data-ttu-id="1e44d-447">將 `<PackageTags>` 屬性搭配您所建立的 GUID 新增至新的或現有的 `<PropertyGroup>`：</span><span class="sxs-lookup"><span data-stu-id="1e44d-447">Add a `<PackageTags>` property to a new or existing `<PropertyGroup>` with the GUID that you created:</span></span>

       ```xml
       <PropertyGroup>
         <PackageTags>9412ee86-c21b-4eb8-bd89-f650fbf44931</PackageTags>
       </PropertyGroup>
       ```

   <span data-ttu-id="1e44d-448">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="1e44d-448">In the following example:</span></span>

   * <span data-ttu-id="1e44d-449">伺服器的本機 IP 位址是 `10.0.0.4`。</span><span class="sxs-lookup"><span data-stu-id="1e44d-449">The local IP address of the server is `10.0.0.4`.</span></span>
   * <span data-ttu-id="1e44d-450">線上隨機 GUID 產生器會提供 `appid` 值。</span><span class="sxs-lookup"><span data-stu-id="1e44d-450">An online random GUID generator provides the `appid` value.</span></span>

   ```console
   netsh http add sslcert 
       ipport=10.0.0.4:443 
       certhash=b66ee04419d4ee37464ab8785ff02449980eae10 
       appid="{9412ee86-c21b-4eb8-bd89-f650fbf44931}"
   ```

   <span data-ttu-id="1e44d-451">已註冊憑證時，此工具會以 `SSL Certificate successfully added` 回應。</span><span class="sxs-lookup"><span data-stu-id="1e44d-451">When a certificate is registered, the tool responds with `SSL Certificate successfully added`.</span></span>

   <span data-ttu-id="1e44d-452">若要刪除憑證註冊，請使用 `delete sslcert` 命令：</span><span class="sxs-lookup"><span data-stu-id="1e44d-452">To delete a certificate registration, use the `delete sslcert` command:</span></span>

   ```console
   netsh http delete sslcert ipport=<IP>:<PORT>
   ```

   <span data-ttu-id="1e44d-453">以下是 *netsh.exe* 的參考文件：</span><span class="sxs-lookup"><span data-stu-id="1e44d-453">Reference documentation for *netsh.exe*:</span></span>

   * [<span data-ttu-id="1e44d-454">超文字傳輸通訊協定 (HTTP) 的 Netsh 命令</span><span class="sxs-lookup"><span data-stu-id="1e44d-454">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
   * [<span data-ttu-id="1e44d-455">UrlPrefix 字串</span><span class="sxs-lookup"><span data-stu-id="1e44d-455">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

1. <span data-ttu-id="1e44d-456">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e44d-456">Run the app.</span></span>

   <span data-ttu-id="1e44d-457">使用 HTTP (不是 HTTPS) 搭配大於 1024 的連接埠號碼來繫結至 localhost 時，不需要系統管理員權限即可執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e44d-457">Administrator privileges aren't required to run the app when binding to localhost using HTTP (not HTTPS) with a port number greater than 1024.</span></span> <span data-ttu-id="1e44d-458">針對其他設定 (例如，使用本機 IP 位址或繫結至連接埠 443)，請使用系統管理員權限來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e44d-458">For other configurations (for example, using a local IP address or binding to port 443), run the app with administrator privileges.</span></span>

   <span data-ttu-id="1e44d-459">應用程式會在伺服器的公用 IP 位址回應。</span><span class="sxs-lookup"><span data-stu-id="1e44d-459">The app responds at the server's public IP address.</span></span> <span data-ttu-id="1e44d-460">在此範例中，是從網際網路透過伺服器的公用 IP 位址 `104.214.79.47` 連線至伺服器。</span><span class="sxs-lookup"><span data-stu-id="1e44d-460">In this example, the server is reached from the Internet at its public IP address of `104.214.79.47`.</span></span>

   <span data-ttu-id="1e44d-461">在此範例中使用的是開發憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-461">A development certificate is used in this example.</span></span> <span data-ttu-id="1e44d-462">在略過瀏覽器的未受信任憑證警告之後，頁面會安全地載入。</span><span class="sxs-lookup"><span data-stu-id="1e44d-462">The page loads securely after bypassing the browser's untrusted certificate warning.</span></span>

   ![顯示已載入應用程式索引頁面的瀏覽器視窗](httpsys/_static/browser.png)

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="1e44d-464">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="1e44d-464">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="1e44d-465">如果是 HTTP.sys 所裝載且與來自網際網路或公司網路的要求進行互動的應用程式，在裝載於 Proxy 伺服器和負載平衡器後方時，可能需要額外的組態。</span><span class="sxs-lookup"><span data-stu-id="1e44d-465">For apps hosted by HTTP.sys that interact with requests from the Internet or a corporate network, additional configuration might be required when hosting behind proxy servers and load balancers.</span></span> <span data-ttu-id="1e44d-466">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-466">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1e44d-467">其他資源</span><span class="sxs-lookup"><span data-stu-id="1e44d-467">Additional resources</span></span>

* <span data-ttu-id="1e44d-468">[使用 HTTP.sys 來啟用 Windows 驗證](xref:security/authentication/windowsauth#httpsys) \(機器翻譯\)</span><span class="sxs-lookup"><span data-stu-id="1e44d-468">[Enable Windows Authentication with HTTP.sys](xref:security/authentication/windowsauth#httpsys)</span></span>
* <span data-ttu-id="1e44d-469">[HTTP 伺服器 API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="1e44d-469">[HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)</span></span>
* <span data-ttu-id="1e44d-470">[aspnet/HttpSysServer GitHub 存放庫 (原始程式碼)](https://github.com/aspnet/HttpSysServer/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="1e44d-470">[aspnet/HttpSysServer GitHub repository (source code)](https://github.com/aspnet/HttpSysServer/)</span></span>
* [<span data-ttu-id="1e44d-471">主機</span><span class="sxs-lookup"><span data-stu-id="1e44d-471">The host</span></span>](xref:fundamentals/index#host)
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="1e44d-472">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) 是只在 Windows 上執行的 [ASP.NET Core 網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-472">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) is a [web server for ASP.NET Core](xref:fundamentals/servers/index) that only runs on Windows.</span></span> <span data-ttu-id="1e44d-473">HTTP.sys 是 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器的替代方式，提供了一些 Kestrel 未提供的功能。</span><span class="sxs-lookup"><span data-stu-id="1e44d-473">HTTP.sys is an alternative to [Kestrel](xref:fundamentals/servers/kestrel) server and offers some features that Kestrel doesn't provide.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1e44d-474">HTTP.sys 與 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)不相容，且不能與 IIS 或 IIS Express 搭配使用。</span><span class="sxs-lookup"><span data-stu-id="1e44d-474">HTTP.sys isn't compatible with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and can't be used with IIS or IIS Express.</span></span>

<span data-ttu-id="1e44d-475">HTTP.sys 支援下列功能：</span><span class="sxs-lookup"><span data-stu-id="1e44d-475">HTTP.sys supports the following features:</span></span>

* [<span data-ttu-id="1e44d-476">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="1e44d-476">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
* <span data-ttu-id="1e44d-477">連接埠共用</span><span class="sxs-lookup"><span data-stu-id="1e44d-477">Port sharing</span></span>
* <span data-ttu-id="1e44d-478">使用 SNI 的 HTTPS</span><span class="sxs-lookup"><span data-stu-id="1e44d-478">HTTPS with SNI</span></span>
* <span data-ttu-id="1e44d-479">透過 TLS 的 HTTP/2 (Windows 10 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="1e44d-479">HTTP/2 over TLS (Windows 10 or later)</span></span>
* <span data-ttu-id="1e44d-480">直接檔案傳輸</span><span class="sxs-lookup"><span data-stu-id="1e44d-480">Direct file transmission</span></span>
* <span data-ttu-id="1e44d-481">回應快取</span><span class="sxs-lookup"><span data-stu-id="1e44d-481">Response caching</span></span>
* <span data-ttu-id="1e44d-482">WebSocket (Windows 8 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="1e44d-482">WebSockets (Windows 8 or later)</span></span>

<span data-ttu-id="1e44d-483">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="1e44d-483">Supported Windows versions:</span></span>

* <span data-ttu-id="1e44d-484">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="1e44d-484">Windows 7 or later</span></span>
* <span data-ttu-id="1e44d-485">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="1e44d-485">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="1e44d-486">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1e44d-486">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="1e44d-487">使用 HTTP.sys 的時機</span><span class="sxs-lookup"><span data-stu-id="1e44d-487">When to use HTTP.sys</span></span>

<span data-ttu-id="1e44d-488">HTTP.sys 在下列部署環境中非常有用：</span><span class="sxs-lookup"><span data-stu-id="1e44d-488">HTTP.sys is useful for deployments where:</span></span>

* <span data-ttu-id="1e44d-489">需要直接向網際網路公開伺服器而不使用 IIS。</span><span class="sxs-lookup"><span data-stu-id="1e44d-489">There's a need to expose the server directly to the Internet without using IIS.</span></span>

  ![HTTP.sys 直接與網際網路通訊](httpsys/_static/httpsys-to-internet.png)

* <span data-ttu-id="1e44d-491">內部部署需要的功能無法在 Kestrel 中使用，例如 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-491">An internal deployment requires a feature not available in Kestrel, such as [Windows Authentication](xref:security/authentication/windowsauth).</span></span>

  ![HTTP.sys 直接與內部網路通訊](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="1e44d-493">HTTP.sys 是成熟的技術，可抵禦許多種類的攻擊，並提供功能完整之網頁伺服器的穩固性、安全性及延展性。</span><span class="sxs-lookup"><span data-stu-id="1e44d-493">HTTP.sys is mature technology that protects against many types of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="1e44d-494">IIS 本身在 HTTP.sys 之上以 HTTP 接聽程式的形式執行。</span><span class="sxs-lookup"><span data-stu-id="1e44d-494">IIS itself runs as an HTTP listener on top of HTTP.sys.</span></span>

## <a name="http2-support"></a><span data-ttu-id="1e44d-495">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="1e44d-495">HTTP/2 support</span></span>

<span data-ttu-id="1e44d-496">如果符合下列基本需求，則可以針對 ASP.NET Core 應用程式啟用 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="1e44d-496">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is enabled for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="1e44d-497">Windows Server 2016/Windows 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="1e44d-497">Windows Server 2016/Windows 10 or later</span></span>
* <span data-ttu-id="1e44d-498">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 連線</span><span class="sxs-lookup"><span data-stu-id="1e44d-498">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="1e44d-499">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="1e44d-499">TLS 1.2 or later connection</span></span>

<span data-ttu-id="1e44d-500">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="1e44d-500">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="1e44d-501">HTTP/2 預設為啟用。</span><span class="sxs-lookup"><span data-stu-id="1e44d-501">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="1e44d-502">如果 HTTP/2 連線尚未建立，連線會退為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="1e44d-502">If an HTTP/2 connection isn't established, the connection falls back to HTTP/1.1.</span></span> <span data-ttu-id="1e44d-503">Windows 的未來版本會推出 HTTP/2 設定旗標，包括使用 HTTP.sys 來停用 HTTP/2 的功能。</span><span class="sxs-lookup"><span data-stu-id="1e44d-503">In a future release of Windows, HTTP/2 configuration flags will be available, including the ability to disable HTTP/2 with HTTP.sys.</span></span>

## <a name="kernel-mode-authentication-with-kerberos"></a><span data-ttu-id="1e44d-504">使用 Kerberos 的核心模式驗證</span><span class="sxs-lookup"><span data-stu-id="1e44d-504">Kernel mode authentication with Kerberos</span></span>

<span data-ttu-id="1e44d-505">HTTP.sys 使用 Kerberos 驗證通訊協定委派給核心模式驗證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-505">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="1e44d-506">Kerberos 和 HTTP.sys 不支援使用者模式驗證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-506">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="1e44d-507">必須使用電腦帳戶來解密 Kerberos 權杖/票證，該權杖/票證取自 Active Directory，並由用戶端將其轉送至伺服器來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="1e44d-507">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="1e44d-508">請註冊主機的服務主體名稱 (SPN)，而非應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="1e44d-508">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

## <a name="how-to-use-httpsys"></a><span data-ttu-id="1e44d-509">如何使用 HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="1e44d-509">How to use HTTP.sys</span></span>

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a><span data-ttu-id="1e44d-510">設定 ASP.NET Core 應用程式使用 HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="1e44d-510">Configure the ASP.NET Core app to use HTTP.sys</span></span>

<span data-ttu-id="1e44d-511">使用[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)（[nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)）時，不需要專案檔中的套件參考。</span><span class="sxs-lookup"><span data-stu-id="1e44d-511">A package reference in the project file isn't required when using the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)).</span></span> <span data-ttu-id="1e44d-512">若不是使用 `Microsoft.AspNetCore.App` 中繼套件，請將套件參考加入 [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-512">When not using the `Microsoft.AspNetCore.App` metapackage, add a package reference to [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).</span></span>

<span data-ttu-id="1e44d-513">建置主機時，呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> 擴充方法，並指定任何必要的 <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>。</span><span class="sxs-lookup"><span data-stu-id="1e44d-513">Call the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> extension method when building the host, specifying any required <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>.</span></span> <span data-ttu-id="1e44d-514">下列範例會將選項設定為它們的預設值：</span><span class="sxs-lookup"><span data-stu-id="1e44d-514">The following example sets options to their default values:</span></span>

[!code-csharp[](httpsys/samples/2.x/SampleApp/Program.cs?name=snippet1&highlight=4-12)]

<span data-ttu-id="1e44d-515">其他的 HTTP.sys 設定則透過[登錄設定](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows)處理。</span><span class="sxs-lookup"><span data-stu-id="1e44d-515">Additional HTTP.sys configuration is handled through [registry settings](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).</span></span>

<span data-ttu-id="1e44d-516">**HTTP.sys 選項**</span><span class="sxs-lookup"><span data-stu-id="1e44d-516">**HTTP.sys options**</span></span>

| <span data-ttu-id="1e44d-517">屬性</span><span class="sxs-lookup"><span data-stu-id="1e44d-517">Property</span></span> | <span data-ttu-id="1e44d-518">描述</span><span class="sxs-lookup"><span data-stu-id="1e44d-518">Description</span></span> | <span data-ttu-id="1e44d-519">預設</span><span class="sxs-lookup"><span data-stu-id="1e44d-519">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="1e44d-520">AllowSynchronousIO</span><span class="sxs-lookup"><span data-stu-id="1e44d-520">AllowSynchronousIO</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | <span data-ttu-id="1e44d-521">控制是否允許 `HttpContext.Request.Body` 和 `HttpContext.Response.Body` 同步輸出/輸入。</span><span class="sxs-lookup"><span data-stu-id="1e44d-521">Control whether synchronous input/output is allowed for the `HttpContext.Request.Body` and `HttpContext.Response.Body`.</span></span> | `true` |
| [<span data-ttu-id="1e44d-522">Authentication.AllowAnonymous</span><span class="sxs-lookup"><span data-stu-id="1e44d-522">Authentication.AllowAnonymous</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | <span data-ttu-id="1e44d-523">允許匿名要求。</span><span class="sxs-lookup"><span data-stu-id="1e44d-523">Allow anonymous requests.</span></span> | `true` |
| [<span data-ttu-id="1e44d-524">Authentication.Schemes</span><span class="sxs-lookup"><span data-stu-id="1e44d-524">Authentication.Schemes</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | <span data-ttu-id="1e44d-525">指定允許的驗證配置。</span><span class="sxs-lookup"><span data-stu-id="1e44d-525">Specify the allowed authentication schemes.</span></span> <span data-ttu-id="1e44d-526">處置接聽程式之前可隨時修改。</span><span class="sxs-lookup"><span data-stu-id="1e44d-526">May be modified at any time prior to disposing the listener.</span></span> <span data-ttu-id="1e44d-527">值是由 [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes) 提供：`Basic`、`Kerberos`、`Negotiate`、`None` 和 `NTLM`。</span><span class="sxs-lookup"><span data-stu-id="1e44d-527">Values are provided by the [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None`, and `NTLM`.</span></span> | `None` |
| [<span data-ttu-id="1e44d-528">EnableResponseCaching</span><span class="sxs-lookup"><span data-stu-id="1e44d-528">EnableResponseCaching</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | <span data-ttu-id="1e44d-529">針對含有合格標頭的回應嘗試[核心模式](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode)快取。</span><span class="sxs-lookup"><span data-stu-id="1e44d-529">Attempt [kernel-mode](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) caching for responses with eligible headers.</span></span> <span data-ttu-id="1e44d-530">回應可能不包含 `Set-Cookie`、`Vary` 或 `Pragma` 標頭。</span><span class="sxs-lookup"><span data-stu-id="1e44d-530">The response may not include `Set-Cookie`, `Vary`, or `Pragma` headers.</span></span> <span data-ttu-id="1e44d-531">它必須包含為 `Cache-Control` 的 `public` 標頭，且有 `shared-max-age` 或 `max-age` 值，或是 `Expires` 標頭。</span><span class="sxs-lookup"><span data-stu-id="1e44d-531">It must include a `Cache-Control` header that's `public` and either a `shared-max-age` or `max-age` value, or an `Expires` header.</span></span> | `true` |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | <span data-ttu-id="1e44d-532">可同時接受的數目上限。</span><span class="sxs-lookup"><span data-stu-id="1e44d-532">The maximum number of concurrent accepts.</span></span> | <span data-ttu-id="1e44d-533">5 &times;[環境。<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span><span class="sxs-lookup"><span data-stu-id="1e44d-533">5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | <span data-ttu-id="1e44d-534">可接受的同時連線數量上限。</span><span class="sxs-lookup"><span data-stu-id="1e44d-534">The maximum number of concurrent connections to accept.</span></span> <span data-ttu-id="1e44d-535">使用 `-1` 為無限多個。</span><span class="sxs-lookup"><span data-stu-id="1e44d-535">Use `-1` for infinite.</span></span> <span data-ttu-id="1e44d-536">使用 `null` 以使用登錄之整個電腦的設定。</span><span class="sxs-lookup"><span data-stu-id="1e44d-536">Use `null` to use the registry's machine-wide setting.</span></span> | `null`<br><span data-ttu-id="1e44d-537">（全電腦</span><span class="sxs-lookup"><span data-stu-id="1e44d-537">(machine-wide</span></span><br><span data-ttu-id="1e44d-538">設</span><span class="sxs-lookup"><span data-stu-id="1e44d-538">setting)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | <span data-ttu-id="1e44d-539">請參閱 <a href="#maxrequestbodysize">MaxRequestBodySize</a> 小節。</span><span class="sxs-lookup"><span data-stu-id="1e44d-539">See the <a href="#maxrequestbodysize">MaxRequestBodySize</a> section.</span></span> | <span data-ttu-id="1e44d-540">30000000 位元組</span><span class="sxs-lookup"><span data-stu-id="1e44d-540">30000000 bytes</span></span><br><span data-ttu-id="1e44d-541">(~28.6 MB)</span><span class="sxs-lookup"><span data-stu-id="1e44d-541">(~28.6 MB)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | <span data-ttu-id="1e44d-542">可以加入佇列的最大要求數目。</span><span class="sxs-lookup"><span data-stu-id="1e44d-542">The maximum number of requests that can be queued.</span></span> | <span data-ttu-id="1e44d-543">1000</span><span class="sxs-lookup"><span data-stu-id="1e44d-543">1000</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | <span data-ttu-id="1e44d-544">指出若回應本文因為用戶端中斷連線而寫入失敗時，應擲回例外狀況或正常完成。</span><span class="sxs-lookup"><span data-stu-id="1e44d-544">Indicate if response body writes that fail due to client disconnects should throw exceptions or complete normally.</span></span> | `false`<br><span data-ttu-id="1e44d-545">(正常完成)</span><span class="sxs-lookup"><span data-stu-id="1e44d-545">(complete normally)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | <span data-ttu-id="1e44d-546">公開 HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> 設定，這也可在登錄中設定。</span><span class="sxs-lookup"><span data-stu-id="1e44d-546">Expose the HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> configuration, which may also be configured in the registry.</span></span> <span data-ttu-id="1e44d-547">API 連結可提供包括預設值在內每個設定的詳細資訊：</span><span class="sxs-lookup"><span data-stu-id="1e44d-547">Follow the API links to learn more about each setting, including default values:</span></span><ul><li><span data-ttu-id="1e44d-548">[TimeoutManager。](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) HTTP 伺服器 API 允許的 DrainEntityBody &ndash; 時間，以在保持連線的狀態中清空實體主體。</span><span class="sxs-lookup"><span data-stu-id="1e44d-548">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; Time allowed for the HTTP Server API to drain the entity body on a Keep-Alive connection.</span></span></li><li><span data-ttu-id="1e44d-549">[TimeoutManager. 允許](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody)&ndash; 允許要求實體主體抵達的時間。</span><span class="sxs-lookup"><span data-stu-id="1e44d-549">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; Time allowed for the request entity body to arrive.</span></span></li><li><span data-ttu-id="1e44d-550">[TimeoutManager：](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait)允許 HTTP 伺服器 API 剖析要求標頭所需的允許 &ndash; 時間。</span><span class="sxs-lookup"><span data-stu-id="1e44d-550">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; Time allowed for the HTTP Server API to parse the request header.</span></span></li><li><span data-ttu-id="1e44d-551">[TimeoutManager：](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection)允許閒置連接的 IdleConnection &ndash; 時間。</span><span class="sxs-lookup"><span data-stu-id="1e44d-551">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; Time allowed for an idle connection.</span></span></li><li><span data-ttu-id="1e44d-552">[TimeoutManager。回應](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond)&ndash; 回應的最小傳送速率。</span><span class="sxs-lookup"><span data-stu-id="1e44d-552">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; The minimum send rate for the response.</span></span></li><li><span data-ttu-id="1e44d-553">[TimeoutManager：](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue)允許要求在應用程式拾取之前保留在要求佇列中的 RequestQueue &ndash; 時間。</span><span class="sxs-lookup"><span data-stu-id="1e44d-553">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; Time allowed for the request to remain in the request queue before the app picks it up.</span></span></li></ul> |  |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | <span data-ttu-id="1e44d-554">指定 <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> 以向 HTTP.sys 註冊。</span><span class="sxs-lookup"><span data-stu-id="1e44d-554">Specify the <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> to register with HTTP.sys.</span></span> <span data-ttu-id="1e44d-555">最實用的是 [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*)，可用來將前置詞加入集合。</span><span class="sxs-lookup"><span data-stu-id="1e44d-555">The most useful is [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), which is used to add a prefix to the collection.</span></span> <span data-ttu-id="1e44d-556">處置接聽程式之前可隨時修改這些內容。</span><span class="sxs-lookup"><span data-stu-id="1e44d-556">These may be modified at any time prior to disposing the listener.</span></span> |  |

<a name="maxrequestbodysize"></a>

<span data-ttu-id="1e44d-557">**MaxRequestBodySize**</span><span class="sxs-lookup"><span data-stu-id="1e44d-557">**MaxRequestBodySize**</span></span>

<span data-ttu-id="1e44d-558">任何要求所允許的大小上限 (以位元組為單位)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-558">The maximum allowed size of any request body in bytes.</span></span> <span data-ttu-id="1e44d-559">當設定為 `null` 時，要求主體大小上限為無限制。</span><span class="sxs-lookup"><span data-stu-id="1e44d-559">When set to `null`, the maximum request body size is unlimited.</span></span> <span data-ttu-id="1e44d-560">此限制對升級連線不會有任何影響，因為其一律為無限制。</span><span class="sxs-lookup"><span data-stu-id="1e44d-560">This limit has no effect on upgraded connections, which are always unlimited.</span></span>

<span data-ttu-id="1e44d-561">在 ASP.NET Core MVC 應用程式中針對單一 `IActionResult` 覆寫限制的建議方式，是在動作方法上使用 <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> 屬性：</span><span class="sxs-lookup"><span data-stu-id="1e44d-561">The recommended method to override the limit in an ASP.NET Core MVC app for a single `IActionResult` is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="1e44d-562">如果應用程式已開始讀取要求之後，才嘗試設定要求的限制，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="1e44d-562">An exception is thrown if the app attempts to configure the limit on a request after the app has started reading the request.</span></span> <span data-ttu-id="1e44d-563">`IsReadOnly` 屬性可用來指出 `MaxRequestBodySize` 屬性是否處於唯讀狀態，代表要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="1e44d-563">An `IsReadOnly` property can be used to indicate if the `MaxRequestBodySize` property is in a read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="1e44d-564">如果應用程式應該覆寫每個要求的 <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize>，請使用 <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>：</span><span class="sxs-lookup"><span data-stu-id="1e44d-564">If the app should override <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> per-request, use the <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>:</span></span>

[!code-csharp[](httpsys/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6-7)]

<span data-ttu-id="1e44d-565">如果您使用 Visual Studio，請確定應用程式未設定為執行 IIS 或 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="1e44d-565">If using Visual Studio, make sure the app isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="1e44d-566">在 Visual Studio 中，預設啟動設定檔適用於 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="1e44d-566">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="1e44d-567">若要執行專案作為主控台應用程式，請手動變更選取的設定檔，如下列螢幕擷取畫面所示：</span><span class="sxs-lookup"><span data-stu-id="1e44d-567">To run the project as a console app, manually change the selected profile, as shown in the following screen shot:</span></span>

![選取主控台應用程式設定檔](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a><span data-ttu-id="1e44d-569">設定 Windows Server</span><span class="sxs-lookup"><span data-stu-id="1e44d-569">Configure Windows Server</span></span>

1. <span data-ttu-id="1e44d-570">判斷要為應用程式開啟的連接埠，然後使用 [Windows 防火牆](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule)或 [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) PowerShell Cmdlet 來開啟防火牆連接埠，以允許流量到達 HTTP.sys。</span><span class="sxs-lookup"><span data-stu-id="1e44d-570">Determine the ports to open for the app and use [Windows Firewall](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) or the [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) PowerShell cmdlet to open firewall ports to allow traffic to reach HTTP.sys.</span></span> <span data-ttu-id="1e44d-571">在下列命令和應用程式設定中，會使用連接埠 443。</span><span class="sxs-lookup"><span data-stu-id="1e44d-571">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="1e44d-572">部署至 Azure VM 時，請在[網路安全性群組](/azure/virtual-machines/windows/nsg-quickstart-portal)中開啟連接埠。</span><span class="sxs-lookup"><span data-stu-id="1e44d-572">When deploying to an Azure VM, open the ports in the [Network Security Group](/azure/virtual-machines/windows/nsg-quickstart-portal).</span></span> <span data-ttu-id="1e44d-573">在下列命令和應用程式設定中，會使用連接埠 443。</span><span class="sxs-lookup"><span data-stu-id="1e44d-573">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="1e44d-574">視需要取得並安裝 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-574">Obtain and install X.509 certificates, if required.</span></span>

   <span data-ttu-id="1e44d-575">在 Windows 上，請使用 [New-SelfSignedCertificate PowerShell Cmdlet](/powershell/module/pkiclient/new-selfsignedcertificate) 來建立自我簽署的憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-575">On Windows, create self-signed certificates using the [New-SelfSignedCertificate PowerShell cmdlet](/powershell/module/pkiclient/new-selfsignedcertificate).</span></span> <span data-ttu-id="1e44d-576">如需不支援的範例，請參閱 [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-576">For an unsupported example, see [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).</span></span>

   <span data-ttu-id="1e44d-577">在伺服器的**本機電腦**>**個人**存放區中，安裝自我簽署或 CA 簽署的憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-577">Install either self-signed or CA-signed certificates in the server's **Local Machine** > **Personal** store.</span></span>

1. <span data-ttu-id="1e44d-578">如果應用程式是[與架構相依的部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)，請安裝 .NET Core、.NET Framework 或兩者 (如果應用程式是以 .NET Framework 為目標的 .NET Core 應用程式)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-578">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd), install .NET Core, .NET Framework, or both (if the app is a .NET Core app targeting the .NET Framework).</span></span>

   * <span data-ttu-id="1e44d-579">**.Net core** &ndash; 如果應用程式需要 .net core，請從[.net core 下載](https://dotnet.microsoft.com/download)取得並執行 **.net core 運行**時間安裝程式。</span><span class="sxs-lookup"><span data-stu-id="1e44d-579">**.NET Core** &ndash; If the app requires .NET Core, obtain and run the **.NET Core Runtime** installer from [.NET Core Downloads](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="1e44d-580">請勿在伺服器上安裝完整的 SDK。</span><span class="sxs-lookup"><span data-stu-id="1e44d-580">Don't install the full SDK on the server.</span></span>
   * <span data-ttu-id="1e44d-581">**.NET Framework** &ndash; 如果應用程式需要 .NET Framework，請參閱[.NET Framework 安裝指南](/dotnet/framework/install/)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-581">**.NET Framework** &ndash; If the app requires .NET Framework, see the [.NET Framework installation guide](/dotnet/framework/install/).</span></span> <span data-ttu-id="1e44d-582">安裝必要的 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="1e44d-582">Install the required .NET Framework.</span></span> <span data-ttu-id="1e44d-583">您可以從 [.NET Core 下載](https://dotnet.microsoft.com/download)頁面取得最新 .NET Framework 的安裝程式。</span><span class="sxs-lookup"><span data-stu-id="1e44d-583">The installer for the latest .NET Framework is available from the [.NET Core Downloads](https://dotnet.microsoft.com/download) page.</span></span>

   <span data-ttu-id="1e44d-584">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)，則應用程式的部署中會包含執行階段。</span><span class="sxs-lookup"><span data-stu-id="1e44d-584">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd), the app includes the runtime in its deployment.</span></span> <span data-ttu-id="1e44d-585">不需要在伺服器上安裝任何架構。</span><span class="sxs-lookup"><span data-stu-id="1e44d-585">No framework installation is required on the server.</span></span>

1. <span data-ttu-id="1e44d-586">設定應用程式中的 URL 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="1e44d-586">Configure URLs and ports in the app.</span></span>

   <span data-ttu-id="1e44d-587">ASP.NET Core 預設會繫結至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="1e44d-587">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="1e44d-588">若要設定 URL 首碼和連接埠，選項包括：</span><span class="sxs-lookup"><span data-stu-id="1e44d-588">To configure URL prefixes and ports, options include:</span></span>

   * <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
   * <span data-ttu-id="1e44d-589">`urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="1e44d-589">`urls` command-line argument</span></span>
   * <span data-ttu-id="1e44d-590">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="1e44d-590">`ASPNETCORE_URLS` environment variable</span></span>
   * <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes>

   <span data-ttu-id="1e44d-591">下列程式碼範例示範如何使用 <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> 搭配位於連接埠 443 的伺服器本機 IP 位址 `10.0.0.4`：</span><span class="sxs-lookup"><span data-stu-id="1e44d-591">The following code example shows how to use <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> with the server's local IP address `10.0.0.4` on port 443:</span></span>

   [!code-csharp[](httpsys/samples_snapshot/2.x/Program.cs?highlight=6)]

   <span data-ttu-id="1e44d-592">`UrlPrefixes` 的優點是針對錯誤格式的前置詞會立即產生錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="1e44d-592">An advantage of `UrlPrefixes` is that an error message is generated immediately for improperly formatted prefixes.</span></span>

   <span data-ttu-id="1e44d-593">`UrlPrefixes` 中的設定會覆寫 `UseUrls`/`urls`/`ASPNETCORE_URLS` 設定。</span><span class="sxs-lookup"><span data-stu-id="1e44d-593">The settings in `UrlPrefixes` override `UseUrls`/`urls`/`ASPNETCORE_URLS` settings.</span></span> <span data-ttu-id="1e44d-594">因此，`UseUrls`、`urls` 和 `ASPNETCORE_URLS` 環境變數的優點，是能更輕鬆地在 Kestrel 和 HTTP.sys 之間切換。</span><span class="sxs-lookup"><span data-stu-id="1e44d-594">Therefore, an advantage of `UseUrls`, `urls`, and the `ASPNETCORE_URLS` environment variable is that it's easier to switch between Kestrel and HTTP.sys.</span></span>

   <span data-ttu-id="1e44d-595">HTTP.sys 使用 [HTTP Server API UrlPrefix 字串格式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-595">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

   > [!WARNING]
   > <span data-ttu-id="1e44d-596">請`http://*:80/`勿`http://+:80`使用最上層萬用字元繫結 (**與** )。</span><span class="sxs-lookup"><span data-stu-id="1e44d-596">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="1e44d-597">最上層萬用字元繫結會導致應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="1e44d-597">Top-level wildcard bindings create app security vulnerabilities.</span></span> <span data-ttu-id="1e44d-598">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="1e44d-598">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="1e44d-599">請使用明確的主機名稱或 IP 位址，而不要使用萬用字元。</span><span class="sxs-lookup"><span data-stu-id="1e44d-599">Use explicit host names or IP addresses rather than wildcards.</span></span> <span data-ttu-id="1e44d-600">若您擁有整個父網域 (相對於有弱點的 `*.mysub.com`) 的控制權，則子網域萬用字元繫結 (例如 `*.com`) 便不構成安全性風險。</span><span class="sxs-lookup"><span data-stu-id="1e44d-600">Subdomain wildcard binding (for example, `*.mysub.com`) isn't a security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="1e44d-601">如需詳細資訊，請參閱[RFC 7230：第5.4 節：主機](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-601">For more information, see [RFC 7230: Section 5.4: Host](https://tools.ietf.org/html/rfc7230#section-5.4).</span></span>

1. <span data-ttu-id="1e44d-602">在伺服器上預先註冊 URL 首碼。</span><span class="sxs-lookup"><span data-stu-id="1e44d-602">Preregister URL prefixes on the server.</span></span>

   <span data-ttu-id="1e44d-603">用來設定 HTTP.sys 的內建工具是 *netsh.exe*。</span><span class="sxs-lookup"><span data-stu-id="1e44d-603">The built-in tool for configuring HTTP.sys is *netsh.exe*.</span></span> <span data-ttu-id="1e44d-604">*netsh.exe* 是用來保留 URL 前置詞，並指派 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-604">*netsh.exe* is used to reserve URL prefixes and assign X.509 certificates.</span></span> <span data-ttu-id="1e44d-605">此工具需要系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="1e44d-605">The tool requires administrator privileges.</span></span>

   <span data-ttu-id="1e44d-606">使用 *netsh.exe* 工具來為應用程式註冊 URL：</span><span class="sxs-lookup"><span data-stu-id="1e44d-606">Use the *netsh.exe* tool to register URLs for the app:</span></span>

   ```console
   netsh http add urlacl url=<URL> user=<USER>
   ```

   * <span data-ttu-id="1e44d-607">`<URL>` &ndash; 完整的統一資源定位器（URL）。</span><span class="sxs-lookup"><span data-stu-id="1e44d-607">`<URL>` &ndash; The fully qualified Uniform Resource Locator (URL).</span></span> <span data-ttu-id="1e44d-608">請勿使用萬用字元繫結。</span><span class="sxs-lookup"><span data-stu-id="1e44d-608">Don't use a wildcard binding.</span></span> <span data-ttu-id="1e44d-609">請使用有效的主機名稱或本機 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="1e44d-609">Use a valid hostname or local IP address.</span></span> <span data-ttu-id="1e44d-610">URL 必須包含結尾斜線。</span><span class="sxs-lookup"><span data-stu-id="1e44d-610">*The URL must include a trailing slash.*</span></span>
   * <span data-ttu-id="1e44d-611">`<USER>` &ndash; 指定使用者或使用者組名。</span><span class="sxs-lookup"><span data-stu-id="1e44d-611">`<USER>` &ndash; Specifies the user or user-group name.</span></span>

   <span data-ttu-id="1e44d-612">在以下範例中，伺服器的本機 IP 位址是 `10.0.0.4`：</span><span class="sxs-lookup"><span data-stu-id="1e44d-612">In the following example, the local IP address of the server is `10.0.0.4`:</span></span>

   ```console
   netsh http add urlacl url=https://10.0.0.4:443/ user=Users
   ```

   <span data-ttu-id="1e44d-613">已註冊 URL 時，此工具會以 `URL reservation successfully added` 回應。</span><span class="sxs-lookup"><span data-stu-id="1e44d-613">When a URL is registered, the tool responds with `URL reservation successfully added`.</span></span>

   <span data-ttu-id="1e44d-614">若要刪除已註冊的 URL，請使用 `delete urlacl` 命令：</span><span class="sxs-lookup"><span data-stu-id="1e44d-614">To delete a registered URL, use the `delete urlacl` command:</span></span>

   ```console
   netsh http delete urlacl url=<URL>
   ```

1. <span data-ttu-id="1e44d-615">在伺服器上註冊 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-615">Register X.509 certificates on the server.</span></span>

   <span data-ttu-id="1e44d-616">使用 *netsh.exe* 工具來為應用程式註冊憑證：</span><span class="sxs-lookup"><span data-stu-id="1e44d-616">Use the *netsh.exe* tool to register certificates for the app:</span></span>

   ```console
   netsh http add sslcert ipport=<IP>:<PORT> certhash=<THUMBPRINT> appid="{<GUID>}"
   ```

   * <span data-ttu-id="1e44d-617">`<IP>` &ndash; 指定系結的本機 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="1e44d-617">`<IP>` &ndash; Specifies the local IP address for the binding.</span></span> <span data-ttu-id="1e44d-618">請勿使用萬用字元繫結。</span><span class="sxs-lookup"><span data-stu-id="1e44d-618">Don't use a wildcard binding.</span></span> <span data-ttu-id="1e44d-619">請使用有效的 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="1e44d-619">Use a valid IP address.</span></span>
   * <span data-ttu-id="1e44d-620">`<PORT>` &ndash; 指定系結的埠。</span><span class="sxs-lookup"><span data-stu-id="1e44d-620">`<PORT>` &ndash; Specifies the port for the binding.</span></span>
   * <span data-ttu-id="1e44d-621">`<THUMBPRINT>` &ndash; x.509 憑證指紋。</span><span class="sxs-lookup"><span data-stu-id="1e44d-621">`<THUMBPRINT>` &ndash; The X.509 certificate thumbprint.</span></span>
   * <span data-ttu-id="1e44d-622">`<GUID>` &ndash; 開發人員產生的 GUID，代表應用程式以供參考之用。</span><span class="sxs-lookup"><span data-stu-id="1e44d-622">`<GUID>` &ndash; A developer-generated GUID to represent the app for informational purposes.</span></span>

   <span data-ttu-id="1e44d-623">為了便於參考，請將 GUID 以套件標記的形式儲存在應用程式中：</span><span class="sxs-lookup"><span data-stu-id="1e44d-623">For reference purposes, store the GUID in the app as a package tag:</span></span>

   * <span data-ttu-id="1e44d-624">在 Visual Studio 中：</span><span class="sxs-lookup"><span data-stu-id="1e44d-624">In Visual Studio:</span></span>
     * <span data-ttu-id="1e44d-625">在 [方案總管] 中的專案上按一下滑鼠右鍵，然後選取 [屬性]，以開啟應用程式的專案屬性。</span><span class="sxs-lookup"><span data-stu-id="1e44d-625">Open the app's project properties by right-clicking on the app in **Solution Explorer** and selecting **Properties**.</span></span>
     * <span data-ttu-id="1e44d-626">選取 [套件] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="1e44d-626">Select the **Package** tab.</span></span>
     * <span data-ttu-id="1e44d-627">輸入您在 [標記] 欄位中建立的 GUID。</span><span class="sxs-lookup"><span data-stu-id="1e44d-627">Enter the GUID that you created in the **Tags** field.</span></span>
   * <span data-ttu-id="1e44d-628">不是使用 Visual Studio 時：</span><span class="sxs-lookup"><span data-stu-id="1e44d-628">When not using Visual Studio:</span></span>
     * <span data-ttu-id="1e44d-629">開啟應用程式的專案檔。</span><span class="sxs-lookup"><span data-stu-id="1e44d-629">Open the app's project file.</span></span>
     * <span data-ttu-id="1e44d-630">將 `<PackageTags>` 屬性搭配您所建立的 GUID 新增至新的或現有的 `<PropertyGroup>`：</span><span class="sxs-lookup"><span data-stu-id="1e44d-630">Add a `<PackageTags>` property to a new or existing `<PropertyGroup>` with the GUID that you created:</span></span>

       ```xml
       <PropertyGroup>
         <PackageTags>9412ee86-c21b-4eb8-bd89-f650fbf44931</PackageTags>
       </PropertyGroup>
       ```

   <span data-ttu-id="1e44d-631">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="1e44d-631">In the following example:</span></span>

   * <span data-ttu-id="1e44d-632">伺服器的本機 IP 位址是 `10.0.0.4`。</span><span class="sxs-lookup"><span data-stu-id="1e44d-632">The local IP address of the server is `10.0.0.4`.</span></span>
   * <span data-ttu-id="1e44d-633">線上隨機 GUID 產生器會提供 `appid` 值。</span><span class="sxs-lookup"><span data-stu-id="1e44d-633">An online random GUID generator provides the `appid` value.</span></span>

   ```console
   netsh http add sslcert 
       ipport=10.0.0.4:443 
       certhash=b66ee04419d4ee37464ab8785ff02449980eae10 
       appid="{9412ee86-c21b-4eb8-bd89-f650fbf44931}"
   ```

   <span data-ttu-id="1e44d-634">已註冊憑證時，此工具會以 `SSL Certificate successfully added` 回應。</span><span class="sxs-lookup"><span data-stu-id="1e44d-634">When a certificate is registered, the tool responds with `SSL Certificate successfully added`.</span></span>

   <span data-ttu-id="1e44d-635">若要刪除憑證註冊，請使用 `delete sslcert` 命令：</span><span class="sxs-lookup"><span data-stu-id="1e44d-635">To delete a certificate registration, use the `delete sslcert` command:</span></span>

   ```console
   netsh http delete sslcert ipport=<IP>:<PORT>
   ```

   <span data-ttu-id="1e44d-636">以下是 *netsh.exe* 的參考文件：</span><span class="sxs-lookup"><span data-stu-id="1e44d-636">Reference documentation for *netsh.exe*:</span></span>

   * [<span data-ttu-id="1e44d-637">超文字傳輸通訊協定 (HTTP) 的 Netsh 命令</span><span class="sxs-lookup"><span data-stu-id="1e44d-637">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
   * [<span data-ttu-id="1e44d-638">UrlPrefix 字串</span><span class="sxs-lookup"><span data-stu-id="1e44d-638">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

1. <span data-ttu-id="1e44d-639">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e44d-639">Run the app.</span></span>

   <span data-ttu-id="1e44d-640">使用 HTTP (不是 HTTPS) 搭配大於 1024 的連接埠號碼來繫結至 localhost 時，不需要系統管理員權限即可執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e44d-640">Administrator privileges aren't required to run the app when binding to localhost using HTTP (not HTTPS) with a port number greater than 1024.</span></span> <span data-ttu-id="1e44d-641">針對其他設定 (例如，使用本機 IP 位址或繫結至連接埠 443)，請使用系統管理員權限來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e44d-641">For other configurations (for example, using a local IP address or binding to port 443), run the app with administrator privileges.</span></span>

   <span data-ttu-id="1e44d-642">應用程式會在伺服器的公用 IP 位址回應。</span><span class="sxs-lookup"><span data-stu-id="1e44d-642">The app responds at the server's public IP address.</span></span> <span data-ttu-id="1e44d-643">在此範例中，是從網際網路透過伺服器的公用 IP 位址 `104.214.79.47` 連線至伺服器。</span><span class="sxs-lookup"><span data-stu-id="1e44d-643">In this example, the server is reached from the Internet at its public IP address of `104.214.79.47`.</span></span>

   <span data-ttu-id="1e44d-644">在此範例中使用的是開發憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-644">A development certificate is used in this example.</span></span> <span data-ttu-id="1e44d-645">在略過瀏覽器的未受信任憑證警告之後，頁面會安全地載入。</span><span class="sxs-lookup"><span data-stu-id="1e44d-645">The page loads securely after bypassing the browser's untrusted certificate warning.</span></span>

   ![顯示已載入應用程式索引頁面的瀏覽器視窗](httpsys/_static/browser.png)

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="1e44d-647">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="1e44d-647">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="1e44d-648">如果是 HTTP.sys 所裝載且與來自網際網路或公司網路的要求進行互動的應用程式，在裝載於 Proxy 伺服器和負載平衡器後方時，可能需要額外的組態。</span><span class="sxs-lookup"><span data-stu-id="1e44d-648">For apps hosted by HTTP.sys that interact with requests from the Internet or a corporate network, additional configuration might be required when hosting behind proxy servers and load balancers.</span></span> <span data-ttu-id="1e44d-649">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-649">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1e44d-650">其他資源</span><span class="sxs-lookup"><span data-stu-id="1e44d-650">Additional resources</span></span>

* <span data-ttu-id="1e44d-651">[使用 HTTP.sys 來啟用 Windows 驗證](xref:security/authentication/windowsauth#httpsys) \(機器翻譯\)</span><span class="sxs-lookup"><span data-stu-id="1e44d-651">[Enable Windows Authentication with HTTP.sys](xref:security/authentication/windowsauth#httpsys)</span></span>
* <span data-ttu-id="1e44d-652">[HTTP 伺服器 API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="1e44d-652">[HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)</span></span>
* <span data-ttu-id="1e44d-653">[aspnet/HttpSysServer GitHub 存放庫 (原始程式碼)](https://github.com/aspnet/HttpSysServer/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="1e44d-653">[aspnet/HttpSysServer GitHub repository (source code)](https://github.com/aspnet/HttpSysServer/)</span></span>
* [<span data-ttu-id="1e44d-654">主機</span><span class="sxs-lookup"><span data-stu-id="1e44d-654">The host</span></span>](xref:fundamentals/index#host)
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="1e44d-655">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) 是只在 Windows 上執行的 [ASP.NET Core 網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-655">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) is a [web server for ASP.NET Core](xref:fundamentals/servers/index) that only runs on Windows.</span></span> <span data-ttu-id="1e44d-656">HTTP.sys 是 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器的替代方式，提供了一些 Kestrel 未提供的功能。</span><span class="sxs-lookup"><span data-stu-id="1e44d-656">HTTP.sys is an alternative to [Kestrel](xref:fundamentals/servers/kestrel) server and offers some features that Kestrel doesn't provide.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1e44d-657">HTTP.sys 與 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)不相容，且不能與 IIS 或 IIS Express 搭配使用。</span><span class="sxs-lookup"><span data-stu-id="1e44d-657">HTTP.sys isn't compatible with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and can't be used with IIS or IIS Express.</span></span>

<span data-ttu-id="1e44d-658">HTTP.sys 支援下列功能：</span><span class="sxs-lookup"><span data-stu-id="1e44d-658">HTTP.sys supports the following features:</span></span>

* [<span data-ttu-id="1e44d-659">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="1e44d-659">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
* <span data-ttu-id="1e44d-660">連接埠共用</span><span class="sxs-lookup"><span data-stu-id="1e44d-660">Port sharing</span></span>
* <span data-ttu-id="1e44d-661">使用 SNI 的 HTTPS</span><span class="sxs-lookup"><span data-stu-id="1e44d-661">HTTPS with SNI</span></span>
* <span data-ttu-id="1e44d-662">透過 TLS 的 HTTP/2 (Windows 10 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="1e44d-662">HTTP/2 over TLS (Windows 10 or later)</span></span>
* <span data-ttu-id="1e44d-663">直接檔案傳輸</span><span class="sxs-lookup"><span data-stu-id="1e44d-663">Direct file transmission</span></span>
* <span data-ttu-id="1e44d-664">回應快取</span><span class="sxs-lookup"><span data-stu-id="1e44d-664">Response caching</span></span>
* <span data-ttu-id="1e44d-665">WebSocket (Windows 8 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="1e44d-665">WebSockets (Windows 8 or later)</span></span>

<span data-ttu-id="1e44d-666">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="1e44d-666">Supported Windows versions:</span></span>

* <span data-ttu-id="1e44d-667">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="1e44d-667">Windows 7 or later</span></span>
* <span data-ttu-id="1e44d-668">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="1e44d-668">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="1e44d-669">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1e44d-669">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="1e44d-670">使用 HTTP.sys 的時機</span><span class="sxs-lookup"><span data-stu-id="1e44d-670">When to use HTTP.sys</span></span>

<span data-ttu-id="1e44d-671">HTTP.sys 在下列部署環境中非常有用：</span><span class="sxs-lookup"><span data-stu-id="1e44d-671">HTTP.sys is useful for deployments where:</span></span>

* <span data-ttu-id="1e44d-672">需要直接向網際網路公開伺服器而不使用 IIS。</span><span class="sxs-lookup"><span data-stu-id="1e44d-672">There's a need to expose the server directly to the Internet without using IIS.</span></span>

  ![HTTP.sys 直接與網際網路通訊](httpsys/_static/httpsys-to-internet.png)

* <span data-ttu-id="1e44d-674">內部部署需要的功能無法在 Kestrel 中使用，例如 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-674">An internal deployment requires a feature not available in Kestrel, such as [Windows Authentication](xref:security/authentication/windowsauth).</span></span>

  ![HTTP.sys 直接與內部網路通訊](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="1e44d-676">HTTP.sys 是成熟的技術，可抵禦許多種類的攻擊，並提供功能完整之網頁伺服器的穩固性、安全性及延展性。</span><span class="sxs-lookup"><span data-stu-id="1e44d-676">HTTP.sys is mature technology that protects against many types of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="1e44d-677">IIS 本身在 HTTP.sys 之上以 HTTP 接聽程式的形式執行。</span><span class="sxs-lookup"><span data-stu-id="1e44d-677">IIS itself runs as an HTTP listener on top of HTTP.sys.</span></span>

## <a name="http2-support"></a><span data-ttu-id="1e44d-678">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="1e44d-678">HTTP/2 support</span></span>

<span data-ttu-id="1e44d-679">如果符合下列基本需求，則可以針對 ASP.NET Core 應用程式啟用 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="1e44d-679">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is enabled for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="1e44d-680">Windows Server 2016/Windows 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="1e44d-680">Windows Server 2016/Windows 10 or later</span></span>
* <span data-ttu-id="1e44d-681">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 連線</span><span class="sxs-lookup"><span data-stu-id="1e44d-681">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="1e44d-682">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="1e44d-682">TLS 1.2 or later connection</span></span>

<span data-ttu-id="1e44d-683">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/1.1`。</span><span class="sxs-lookup"><span data-stu-id="1e44d-683">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="1e44d-684">HTTP/2 預設為啟用。</span><span class="sxs-lookup"><span data-stu-id="1e44d-684">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="1e44d-685">如果 HTTP/2 連線尚未建立，連線會退為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="1e44d-685">If an HTTP/2 connection isn't established, the connection falls back to HTTP/1.1.</span></span> <span data-ttu-id="1e44d-686">Windows 的未來版本會推出 HTTP/2 設定旗標，包括使用 HTTP.sys 來停用 HTTP/2 的功能。</span><span class="sxs-lookup"><span data-stu-id="1e44d-686">In a future release of Windows, HTTP/2 configuration flags will be available, including the ability to disable HTTP/2 with HTTP.sys.</span></span>

## <a name="kernel-mode-authentication-with-kerberos"></a><span data-ttu-id="1e44d-687">使用 Kerberos 的核心模式驗證</span><span class="sxs-lookup"><span data-stu-id="1e44d-687">Kernel mode authentication with Kerberos</span></span>

<span data-ttu-id="1e44d-688">HTTP.sys 使用 Kerberos 驗證通訊協定委派給核心模式驗證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-688">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="1e44d-689">Kerberos 和 HTTP.sys 不支援使用者模式驗證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-689">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="1e44d-690">必須使用電腦帳戶來解密 Kerberos 權杖/票證，該權杖/票證取自 Active Directory，並由用戶端將其轉送至伺服器來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="1e44d-690">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="1e44d-691">請註冊主機的服務主體名稱 (SPN)，而非應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="1e44d-691">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

## <a name="how-to-use-httpsys"></a><span data-ttu-id="1e44d-692">如何使用 HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="1e44d-692">How to use HTTP.sys</span></span>

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a><span data-ttu-id="1e44d-693">設定 ASP.NET Core 應用程式使用 HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="1e44d-693">Configure the ASP.NET Core app to use HTTP.sys</span></span>

<span data-ttu-id="1e44d-694">使用[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)（[nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)）時，不需要專案檔中的套件參考。</span><span class="sxs-lookup"><span data-stu-id="1e44d-694">A package reference in the project file isn't required when using the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)).</span></span> <span data-ttu-id="1e44d-695">若不是使用 `Microsoft.AspNetCore.App` 中繼套件，請將套件參考加入 [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-695">When not using the `Microsoft.AspNetCore.App` metapackage, add a package reference to [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).</span></span>

<span data-ttu-id="1e44d-696">建置主機時，呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> 擴充方法，並指定任何必要的 <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>。</span><span class="sxs-lookup"><span data-stu-id="1e44d-696">Call the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> extension method when building the host, specifying any required <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>.</span></span> <span data-ttu-id="1e44d-697">下列範例會將選項設定為它們的預設值：</span><span class="sxs-lookup"><span data-stu-id="1e44d-697">The following example sets options to their default values:</span></span>

[!code-csharp[](httpsys/samples/2.x/SampleApp/Program.cs?name=snippet1&highlight=4-12)]

<span data-ttu-id="1e44d-698">其他的 HTTP.sys 設定則透過[登錄設定](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows)處理。</span><span class="sxs-lookup"><span data-stu-id="1e44d-698">Additional HTTP.sys configuration is handled through [registry settings](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).</span></span>

<span data-ttu-id="1e44d-699">**HTTP.sys 選項**</span><span class="sxs-lookup"><span data-stu-id="1e44d-699">**HTTP.sys options**</span></span>

| <span data-ttu-id="1e44d-700">屬性</span><span class="sxs-lookup"><span data-stu-id="1e44d-700">Property</span></span> | <span data-ttu-id="1e44d-701">描述</span><span class="sxs-lookup"><span data-stu-id="1e44d-701">Description</span></span> | <span data-ttu-id="1e44d-702">預設</span><span class="sxs-lookup"><span data-stu-id="1e44d-702">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="1e44d-703">AllowSynchronousIO</span><span class="sxs-lookup"><span data-stu-id="1e44d-703">AllowSynchronousIO</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | <span data-ttu-id="1e44d-704">控制是否允許 `HttpContext.Request.Body` 和 `HttpContext.Response.Body` 同步輸出/輸入。</span><span class="sxs-lookup"><span data-stu-id="1e44d-704">Control whether synchronous input/output is allowed for the `HttpContext.Request.Body` and `HttpContext.Response.Body`.</span></span> | `true` |
| [<span data-ttu-id="1e44d-705">Authentication.AllowAnonymous</span><span class="sxs-lookup"><span data-stu-id="1e44d-705">Authentication.AllowAnonymous</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | <span data-ttu-id="1e44d-706">允許匿名要求。</span><span class="sxs-lookup"><span data-stu-id="1e44d-706">Allow anonymous requests.</span></span> | `true` |
| [<span data-ttu-id="1e44d-707">Authentication.Schemes</span><span class="sxs-lookup"><span data-stu-id="1e44d-707">Authentication.Schemes</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | <span data-ttu-id="1e44d-708">指定允許的驗證配置。</span><span class="sxs-lookup"><span data-stu-id="1e44d-708">Specify the allowed authentication schemes.</span></span> <span data-ttu-id="1e44d-709">處置接聽程式之前可隨時修改。</span><span class="sxs-lookup"><span data-stu-id="1e44d-709">May be modified at any time prior to disposing the listener.</span></span> <span data-ttu-id="1e44d-710">值是由 [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes) 提供：`Basic`、`Kerberos`、`Negotiate`、`None` 和 `NTLM`。</span><span class="sxs-lookup"><span data-stu-id="1e44d-710">Values are provided by the [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None`, and `NTLM`.</span></span> | `None` |
| [<span data-ttu-id="1e44d-711">EnableResponseCaching</span><span class="sxs-lookup"><span data-stu-id="1e44d-711">EnableResponseCaching</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | <span data-ttu-id="1e44d-712">針對含有合格標頭的回應嘗試[核心模式](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode)快取。</span><span class="sxs-lookup"><span data-stu-id="1e44d-712">Attempt [kernel-mode](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) caching for responses with eligible headers.</span></span> <span data-ttu-id="1e44d-713">回應可能不包含 `Set-Cookie`、`Vary` 或 `Pragma` 標頭。</span><span class="sxs-lookup"><span data-stu-id="1e44d-713">The response may not include `Set-Cookie`, `Vary`, or `Pragma` headers.</span></span> <span data-ttu-id="1e44d-714">它必須包含為 `Cache-Control` 的 `public` 標頭，且有 `shared-max-age` 或 `max-age` 值，或是 `Expires` 標頭。</span><span class="sxs-lookup"><span data-stu-id="1e44d-714">It must include a `Cache-Control` header that's `public` and either a `shared-max-age` or `max-age` value, or an `Expires` header.</span></span> | `true` |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | <span data-ttu-id="1e44d-715">可同時接受的數目上限。</span><span class="sxs-lookup"><span data-stu-id="1e44d-715">The maximum number of concurrent accepts.</span></span> | <span data-ttu-id="1e44d-716">5 &times;[環境。<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span><span class="sxs-lookup"><span data-stu-id="1e44d-716">5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | <span data-ttu-id="1e44d-717">可接受的同時連線數量上限。</span><span class="sxs-lookup"><span data-stu-id="1e44d-717">The maximum number of concurrent connections to accept.</span></span> <span data-ttu-id="1e44d-718">使用 `-1` 為無限多個。</span><span class="sxs-lookup"><span data-stu-id="1e44d-718">Use `-1` for infinite.</span></span> <span data-ttu-id="1e44d-719">使用 `null` 以使用登錄之整個電腦的設定。</span><span class="sxs-lookup"><span data-stu-id="1e44d-719">Use `null` to use the registry's machine-wide setting.</span></span> | `null`<br><span data-ttu-id="1e44d-720">（全電腦</span><span class="sxs-lookup"><span data-stu-id="1e44d-720">(machine-wide</span></span><br><span data-ttu-id="1e44d-721">設</span><span class="sxs-lookup"><span data-stu-id="1e44d-721">setting)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | <span data-ttu-id="1e44d-722">請參閱 <a href="#maxrequestbodysize">MaxRequestBodySize</a> 小節。</span><span class="sxs-lookup"><span data-stu-id="1e44d-722">See the <a href="#maxrequestbodysize">MaxRequestBodySize</a> section.</span></span> | <span data-ttu-id="1e44d-723">30000000 位元組</span><span class="sxs-lookup"><span data-stu-id="1e44d-723">30000000 bytes</span></span><br><span data-ttu-id="1e44d-724">(~28.6 MB)</span><span class="sxs-lookup"><span data-stu-id="1e44d-724">(~28.6 MB)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | <span data-ttu-id="1e44d-725">可以加入佇列的最大要求數目。</span><span class="sxs-lookup"><span data-stu-id="1e44d-725">The maximum number of requests that can be queued.</span></span> | <span data-ttu-id="1e44d-726">1000</span><span class="sxs-lookup"><span data-stu-id="1e44d-726">1000</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | <span data-ttu-id="1e44d-727">指出若回應本文因為用戶端中斷連線而寫入失敗時，應擲回例外狀況或正常完成。</span><span class="sxs-lookup"><span data-stu-id="1e44d-727">Indicate if response body writes that fail due to client disconnects should throw exceptions or complete normally.</span></span> | `false`<br><span data-ttu-id="1e44d-728">(正常完成)</span><span class="sxs-lookup"><span data-stu-id="1e44d-728">(complete normally)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | <span data-ttu-id="1e44d-729">公開 HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> 設定，這也可在登錄中設定。</span><span class="sxs-lookup"><span data-stu-id="1e44d-729">Expose the HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> configuration, which may also be configured in the registry.</span></span> <span data-ttu-id="1e44d-730">API 連結可提供包括預設值在內每個設定的詳細資訊：</span><span class="sxs-lookup"><span data-stu-id="1e44d-730">Follow the API links to learn more about each setting, including default values:</span></span><ul><li><span data-ttu-id="1e44d-731">[TimeoutManager。](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) HTTP 伺服器 API 允許的 DrainEntityBody &ndash; 時間，以在保持連線的狀態中清空實體主體。</span><span class="sxs-lookup"><span data-stu-id="1e44d-731">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; Time allowed for the HTTP Server API to drain the entity body on a Keep-Alive connection.</span></span></li><li><span data-ttu-id="1e44d-732">[TimeoutManager. 允許](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody)&ndash; 允許要求實體主體抵達的時間。</span><span class="sxs-lookup"><span data-stu-id="1e44d-732">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; Time allowed for the request entity body to arrive.</span></span></li><li><span data-ttu-id="1e44d-733">[TimeoutManager：](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait)允許 HTTP 伺服器 API 剖析要求標頭所需的允許 &ndash; 時間。</span><span class="sxs-lookup"><span data-stu-id="1e44d-733">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; Time allowed for the HTTP Server API to parse the request header.</span></span></li><li><span data-ttu-id="1e44d-734">[TimeoutManager：](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection)允許閒置連接的 IdleConnection &ndash; 時間。</span><span class="sxs-lookup"><span data-stu-id="1e44d-734">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; Time allowed for an idle connection.</span></span></li><li><span data-ttu-id="1e44d-735">[TimeoutManager。回應](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond)&ndash; 回應的最小傳送速率。</span><span class="sxs-lookup"><span data-stu-id="1e44d-735">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; The minimum send rate for the response.</span></span></li><li><span data-ttu-id="1e44d-736">[TimeoutManager：](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue)允許要求在應用程式拾取之前保留在要求佇列中的 RequestQueue &ndash; 時間。</span><span class="sxs-lookup"><span data-stu-id="1e44d-736">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; Time allowed for the request to remain in the request queue before the app picks it up.</span></span></li></ul> |  |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | <span data-ttu-id="1e44d-737">指定 <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> 以向 HTTP.sys 註冊。</span><span class="sxs-lookup"><span data-stu-id="1e44d-737">Specify the <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> to register with HTTP.sys.</span></span> <span data-ttu-id="1e44d-738">最實用的是 [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*)，可用來將前置詞加入集合。</span><span class="sxs-lookup"><span data-stu-id="1e44d-738">The most useful is [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), which is used to add a prefix to the collection.</span></span> <span data-ttu-id="1e44d-739">處置接聽程式之前可隨時修改這些內容。</span><span class="sxs-lookup"><span data-stu-id="1e44d-739">These may be modified at any time prior to disposing the listener.</span></span> |  |

<a name="maxrequestbodysize"></a>

<span data-ttu-id="1e44d-740">**MaxRequestBodySize**</span><span class="sxs-lookup"><span data-stu-id="1e44d-740">**MaxRequestBodySize**</span></span>

<span data-ttu-id="1e44d-741">任何要求所允許的大小上限 (以位元組為單位)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-741">The maximum allowed size of any request body in bytes.</span></span> <span data-ttu-id="1e44d-742">當設定為 `null` 時，要求主體大小上限為無限制。</span><span class="sxs-lookup"><span data-stu-id="1e44d-742">When set to `null`, the maximum request body size is unlimited.</span></span> <span data-ttu-id="1e44d-743">此限制對升級連線不會有任何影響，因為其一律為無限制。</span><span class="sxs-lookup"><span data-stu-id="1e44d-743">This limit has no effect on upgraded connections, which are always unlimited.</span></span>

<span data-ttu-id="1e44d-744">在 ASP.NET Core MVC 應用程式中針對單一 `IActionResult` 覆寫限制的建議方式，是在動作方法上使用 <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> 屬性：</span><span class="sxs-lookup"><span data-stu-id="1e44d-744">The recommended method to override the limit in an ASP.NET Core MVC app for a single `IActionResult` is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="1e44d-745">如果應用程式已開始讀取要求之後，才嘗試設定要求的限制，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="1e44d-745">An exception is thrown if the app attempts to configure the limit on a request after the app has started reading the request.</span></span> <span data-ttu-id="1e44d-746">`IsReadOnly` 屬性可用來指出 `MaxRequestBodySize` 屬性是否處於唯讀狀態，代表要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="1e44d-746">An `IsReadOnly` property can be used to indicate if the `MaxRequestBodySize` property is in a read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="1e44d-747">如果應用程式應該覆寫每個要求的 <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize>，請使用 <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>：</span><span class="sxs-lookup"><span data-stu-id="1e44d-747">If the app should override <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> per-request, use the <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>:</span></span>

[!code-csharp[](httpsys/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6-7)]

<span data-ttu-id="1e44d-748">如果您使用 Visual Studio，請確定應用程式未設定為執行 IIS 或 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="1e44d-748">If using Visual Studio, make sure the app isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="1e44d-749">在 Visual Studio 中，預設啟動設定檔適用於 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="1e44d-749">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="1e44d-750">若要執行專案作為主控台應用程式，請手動變更選取的設定檔，如下列螢幕擷取畫面所示：</span><span class="sxs-lookup"><span data-stu-id="1e44d-750">To run the project as a console app, manually change the selected profile, as shown in the following screen shot:</span></span>

![選取主控台應用程式設定檔](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a><span data-ttu-id="1e44d-752">設定 Windows Server</span><span class="sxs-lookup"><span data-stu-id="1e44d-752">Configure Windows Server</span></span>

1. <span data-ttu-id="1e44d-753">判斷要為應用程式開啟的連接埠，然後使用 [Windows 防火牆](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule)或 [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) PowerShell Cmdlet 來開啟防火牆連接埠，以允許流量到達 HTTP.sys。</span><span class="sxs-lookup"><span data-stu-id="1e44d-753">Determine the ports to open for the app and use [Windows Firewall](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) or the [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) PowerShell cmdlet to open firewall ports to allow traffic to reach HTTP.sys.</span></span> <span data-ttu-id="1e44d-754">在下列命令和應用程式設定中，會使用連接埠 443。</span><span class="sxs-lookup"><span data-stu-id="1e44d-754">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="1e44d-755">部署至 Azure VM 時，請在[網路安全性群組](/azure/virtual-machines/windows/nsg-quickstart-portal)中開啟連接埠。</span><span class="sxs-lookup"><span data-stu-id="1e44d-755">When deploying to an Azure VM, open the ports in the [Network Security Group](/azure/virtual-machines/windows/nsg-quickstart-portal).</span></span> <span data-ttu-id="1e44d-756">在下列命令和應用程式設定中，會使用連接埠 443。</span><span class="sxs-lookup"><span data-stu-id="1e44d-756">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="1e44d-757">視需要取得並安裝 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-757">Obtain and install X.509 certificates, if required.</span></span>

   <span data-ttu-id="1e44d-758">在 Windows 上，請使用 [New-SelfSignedCertificate PowerShell Cmdlet](/powershell/module/pkiclient/new-selfsignedcertificate) 來建立自我簽署的憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-758">On Windows, create self-signed certificates using the [New-SelfSignedCertificate PowerShell cmdlet](/powershell/module/pkiclient/new-selfsignedcertificate).</span></span> <span data-ttu-id="1e44d-759">如需不支援的範例，請參閱 [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-759">For an unsupported example, see [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).</span></span>

   <span data-ttu-id="1e44d-760">在伺服器的**本機電腦**>**個人**存放區中，安裝自我簽署或 CA 簽署的憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-760">Install either self-signed or CA-signed certificates in the server's **Local Machine** > **Personal** store.</span></span>

1. <span data-ttu-id="1e44d-761">如果應用程式是[與架構相依的部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)，請安裝 .NET Core、.NET Framework 或兩者 (如果應用程式是以 .NET Framework 為目標的 .NET Core 應用程式)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-761">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd), install .NET Core, .NET Framework, or both (if the app is a .NET Core app targeting the .NET Framework).</span></span>

   * <span data-ttu-id="1e44d-762">**.Net core** &ndash; 如果應用程式需要 .net core，請從[.net core 下載](https://dotnet.microsoft.com/download)取得並執行 **.net core 運行**時間安裝程式。</span><span class="sxs-lookup"><span data-stu-id="1e44d-762">**.NET Core** &ndash; If the app requires .NET Core, obtain and run the **.NET Core Runtime** installer from [.NET Core Downloads](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="1e44d-763">請勿在伺服器上安裝完整的 SDK。</span><span class="sxs-lookup"><span data-stu-id="1e44d-763">Don't install the full SDK on the server.</span></span>
   * <span data-ttu-id="1e44d-764">**.NET Framework** &ndash; 如果應用程式需要 .NET Framework，請參閱[.NET Framework 安裝指南](/dotnet/framework/install/)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-764">**.NET Framework** &ndash; If the app requires .NET Framework, see the [.NET Framework installation guide](/dotnet/framework/install/).</span></span> <span data-ttu-id="1e44d-765">安裝必要的 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="1e44d-765">Install the required .NET Framework.</span></span> <span data-ttu-id="1e44d-766">您可以從 [.NET Core 下載](https://dotnet.microsoft.com/download)頁面取得最新 .NET Framework 的安裝程式。</span><span class="sxs-lookup"><span data-stu-id="1e44d-766">The installer for the latest .NET Framework is available from the [.NET Core Downloads](https://dotnet.microsoft.com/download) page.</span></span>

   <span data-ttu-id="1e44d-767">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)，則應用程式的部署中會包含執行階段。</span><span class="sxs-lookup"><span data-stu-id="1e44d-767">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd), the app includes the runtime in its deployment.</span></span> <span data-ttu-id="1e44d-768">不需要在伺服器上安裝任何架構。</span><span class="sxs-lookup"><span data-stu-id="1e44d-768">No framework installation is required on the server.</span></span>

1. <span data-ttu-id="1e44d-769">設定應用程式中的 URL 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="1e44d-769">Configure URLs and ports in the app.</span></span>

   <span data-ttu-id="1e44d-770">ASP.NET Core 預設會繫結至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="1e44d-770">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="1e44d-771">若要設定 URL 首碼和連接埠，選項包括：</span><span class="sxs-lookup"><span data-stu-id="1e44d-771">To configure URL prefixes and ports, options include:</span></span>

   * <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
   * <span data-ttu-id="1e44d-772">`urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="1e44d-772">`urls` command-line argument</span></span>
   * <span data-ttu-id="1e44d-773">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="1e44d-773">`ASPNETCORE_URLS` environment variable</span></span>
   * <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes>

   <span data-ttu-id="1e44d-774">下列程式碼範例示範如何使用 <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> 搭配位於連接埠 443 的伺服器本機 IP 位址 `10.0.0.4`：</span><span class="sxs-lookup"><span data-stu-id="1e44d-774">The following code example shows how to use <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> with the server's local IP address `10.0.0.4` on port 443:</span></span>

   [!code-csharp[](httpsys/samples_snapshot/2.x/Program.cs?highlight=6)]

   <span data-ttu-id="1e44d-775">`UrlPrefixes` 的優點是針對錯誤格式的前置詞會立即產生錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="1e44d-775">An advantage of `UrlPrefixes` is that an error message is generated immediately for improperly formatted prefixes.</span></span>

   <span data-ttu-id="1e44d-776">`UrlPrefixes` 中的設定會覆寫 `UseUrls`/`urls`/`ASPNETCORE_URLS` 設定。</span><span class="sxs-lookup"><span data-stu-id="1e44d-776">The settings in `UrlPrefixes` override `UseUrls`/`urls`/`ASPNETCORE_URLS` settings.</span></span> <span data-ttu-id="1e44d-777">因此，`UseUrls`、`urls` 和 `ASPNETCORE_URLS` 環境變數的優點，是能更輕鬆地在 Kestrel 和 HTTP.sys 之間切換。</span><span class="sxs-lookup"><span data-stu-id="1e44d-777">Therefore, an advantage of `UseUrls`, `urls`, and the `ASPNETCORE_URLS` environment variable is that it's easier to switch between Kestrel and HTTP.sys.</span></span>

   <span data-ttu-id="1e44d-778">HTTP.sys 使用 [HTTP Server API UrlPrefix 字串格式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-778">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

   > [!WARNING]
   > <span data-ttu-id="1e44d-779">請`http://*:80/`勿`http://+:80`使用最上層萬用字元繫結 (**與** )。</span><span class="sxs-lookup"><span data-stu-id="1e44d-779">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="1e44d-780">最上層萬用字元繫結會導致應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="1e44d-780">Top-level wildcard bindings create app security vulnerabilities.</span></span> <span data-ttu-id="1e44d-781">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="1e44d-781">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="1e44d-782">請使用明確的主機名稱或 IP 位址，而不要使用萬用字元。</span><span class="sxs-lookup"><span data-stu-id="1e44d-782">Use explicit host names or IP addresses rather than wildcards.</span></span> <span data-ttu-id="1e44d-783">若您擁有整個父網域 (相對於有弱點的 `*.mysub.com`) 的控制權，則子網域萬用字元繫結 (例如 `*.com`) 便不構成安全性風險。</span><span class="sxs-lookup"><span data-stu-id="1e44d-783">Subdomain wildcard binding (for example, `*.mysub.com`) isn't a security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="1e44d-784">如需詳細資訊，請參閱[RFC 7230：第5.4 節：主機](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-784">For more information, see [RFC 7230: Section 5.4: Host](https://tools.ietf.org/html/rfc7230#section-5.4).</span></span>

1. <span data-ttu-id="1e44d-785">在伺服器上預先註冊 URL 首碼。</span><span class="sxs-lookup"><span data-stu-id="1e44d-785">Preregister URL prefixes on the server.</span></span>

   <span data-ttu-id="1e44d-786">用來設定 HTTP.sys 的內建工具是 *netsh.exe*。</span><span class="sxs-lookup"><span data-stu-id="1e44d-786">The built-in tool for configuring HTTP.sys is *netsh.exe*.</span></span> <span data-ttu-id="1e44d-787">*netsh.exe* 是用來保留 URL 前置詞，並指派 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-787">*netsh.exe* is used to reserve URL prefixes and assign X.509 certificates.</span></span> <span data-ttu-id="1e44d-788">此工具需要系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="1e44d-788">The tool requires administrator privileges.</span></span>

   <span data-ttu-id="1e44d-789">使用 *netsh.exe* 工具來為應用程式註冊 URL：</span><span class="sxs-lookup"><span data-stu-id="1e44d-789">Use the *netsh.exe* tool to register URLs for the app:</span></span>

   ```console
   netsh http add urlacl url=<URL> user=<USER>
   ```

   * <span data-ttu-id="1e44d-790">`<URL>` &ndash; 完整的統一資源定位器（URL）。</span><span class="sxs-lookup"><span data-stu-id="1e44d-790">`<URL>` &ndash; The fully qualified Uniform Resource Locator (URL).</span></span> <span data-ttu-id="1e44d-791">請勿使用萬用字元繫結。</span><span class="sxs-lookup"><span data-stu-id="1e44d-791">Don't use a wildcard binding.</span></span> <span data-ttu-id="1e44d-792">請使用有效的主機名稱或本機 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="1e44d-792">Use a valid hostname or local IP address.</span></span> <span data-ttu-id="1e44d-793">URL 必須包含結尾斜線。</span><span class="sxs-lookup"><span data-stu-id="1e44d-793">*The URL must include a trailing slash.*</span></span>
   * <span data-ttu-id="1e44d-794">`<USER>` &ndash; 指定使用者或使用者組名。</span><span class="sxs-lookup"><span data-stu-id="1e44d-794">`<USER>` &ndash; Specifies the user or user-group name.</span></span>

   <span data-ttu-id="1e44d-795">在以下範例中，伺服器的本機 IP 位址是 `10.0.0.4`：</span><span class="sxs-lookup"><span data-stu-id="1e44d-795">In the following example, the local IP address of the server is `10.0.0.4`:</span></span>

   ```console
   netsh http add urlacl url=https://10.0.0.4:443/ user=Users
   ```

   <span data-ttu-id="1e44d-796">已註冊 URL 時，此工具會以 `URL reservation successfully added` 回應。</span><span class="sxs-lookup"><span data-stu-id="1e44d-796">When a URL is registered, the tool responds with `URL reservation successfully added`.</span></span>

   <span data-ttu-id="1e44d-797">若要刪除已註冊的 URL，請使用 `delete urlacl` 命令：</span><span class="sxs-lookup"><span data-stu-id="1e44d-797">To delete a registered URL, use the `delete urlacl` command:</span></span>

   ```console
   netsh http delete urlacl url=<URL>
   ```

1. <span data-ttu-id="1e44d-798">在伺服器上註冊 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-798">Register X.509 certificates on the server.</span></span>

   <span data-ttu-id="1e44d-799">使用 *netsh.exe* 工具來為應用程式註冊憑證：</span><span class="sxs-lookup"><span data-stu-id="1e44d-799">Use the *netsh.exe* tool to register certificates for the app:</span></span>

   ```console
   netsh http add sslcert ipport=<IP>:<PORT> certhash=<THUMBPRINT> appid="{<GUID>}"
   ```

   * <span data-ttu-id="1e44d-800">`<IP>` &ndash; 指定系結的本機 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="1e44d-800">`<IP>` &ndash; Specifies the local IP address for the binding.</span></span> <span data-ttu-id="1e44d-801">請勿使用萬用字元繫結。</span><span class="sxs-lookup"><span data-stu-id="1e44d-801">Don't use a wildcard binding.</span></span> <span data-ttu-id="1e44d-802">請使用有效的 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="1e44d-802">Use a valid IP address.</span></span>
   * <span data-ttu-id="1e44d-803">`<PORT>` &ndash; 指定系結的埠。</span><span class="sxs-lookup"><span data-stu-id="1e44d-803">`<PORT>` &ndash; Specifies the port for the binding.</span></span>
   * <span data-ttu-id="1e44d-804">`<THUMBPRINT>` &ndash; x.509 憑證指紋。</span><span class="sxs-lookup"><span data-stu-id="1e44d-804">`<THUMBPRINT>` &ndash; The X.509 certificate thumbprint.</span></span>
   * <span data-ttu-id="1e44d-805">`<GUID>` &ndash; 開發人員產生的 GUID，代表應用程式以供參考之用。</span><span class="sxs-lookup"><span data-stu-id="1e44d-805">`<GUID>` &ndash; A developer-generated GUID to represent the app for informational purposes.</span></span>

   <span data-ttu-id="1e44d-806">為了便於參考，請將 GUID 以套件標記的形式儲存在應用程式中：</span><span class="sxs-lookup"><span data-stu-id="1e44d-806">For reference purposes, store the GUID in the app as a package tag:</span></span>

   * <span data-ttu-id="1e44d-807">在 Visual Studio 中：</span><span class="sxs-lookup"><span data-stu-id="1e44d-807">In Visual Studio:</span></span>
     * <span data-ttu-id="1e44d-808">在 [方案總管] 中的專案上按一下滑鼠右鍵，然後選取 [屬性]，以開啟應用程式的專案屬性。</span><span class="sxs-lookup"><span data-stu-id="1e44d-808">Open the app's project properties by right-clicking on the app in **Solution Explorer** and selecting **Properties**.</span></span>
     * <span data-ttu-id="1e44d-809">選取 [套件] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="1e44d-809">Select the **Package** tab.</span></span>
     * <span data-ttu-id="1e44d-810">輸入您在 [標記] 欄位中建立的 GUID。</span><span class="sxs-lookup"><span data-stu-id="1e44d-810">Enter the GUID that you created in the **Tags** field.</span></span>
   * <span data-ttu-id="1e44d-811">不是使用 Visual Studio 時：</span><span class="sxs-lookup"><span data-stu-id="1e44d-811">When not using Visual Studio:</span></span>
     * <span data-ttu-id="1e44d-812">開啟應用程式的專案檔。</span><span class="sxs-lookup"><span data-stu-id="1e44d-812">Open the app's project file.</span></span>
     * <span data-ttu-id="1e44d-813">將 `<PackageTags>` 屬性搭配您所建立的 GUID 新增至新的或現有的 `<PropertyGroup>`：</span><span class="sxs-lookup"><span data-stu-id="1e44d-813">Add a `<PackageTags>` property to a new or existing `<PropertyGroup>` with the GUID that you created:</span></span>

       ```xml
       <PropertyGroup>
         <PackageTags>9412ee86-c21b-4eb8-bd89-f650fbf44931</PackageTags>
       </PropertyGroup>
       ```

   <span data-ttu-id="1e44d-814">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="1e44d-814">In the following example:</span></span>

   * <span data-ttu-id="1e44d-815">伺服器的本機 IP 位址是 `10.0.0.4`。</span><span class="sxs-lookup"><span data-stu-id="1e44d-815">The local IP address of the server is `10.0.0.4`.</span></span>
   * <span data-ttu-id="1e44d-816">線上隨機 GUID 產生器會提供 `appid` 值。</span><span class="sxs-lookup"><span data-stu-id="1e44d-816">An online random GUID generator provides the `appid` value.</span></span>

   ```console
   netsh http add sslcert 
       ipport=10.0.0.4:443 
       certhash=b66ee04419d4ee37464ab8785ff02449980eae10 
       appid="{9412ee86-c21b-4eb8-bd89-f650fbf44931}"
   ```

   <span data-ttu-id="1e44d-817">已註冊憑證時，此工具會以 `SSL Certificate successfully added` 回應。</span><span class="sxs-lookup"><span data-stu-id="1e44d-817">When a certificate is registered, the tool responds with `SSL Certificate successfully added`.</span></span>

   <span data-ttu-id="1e44d-818">若要刪除憑證註冊，請使用 `delete sslcert` 命令：</span><span class="sxs-lookup"><span data-stu-id="1e44d-818">To delete a certificate registration, use the `delete sslcert` command:</span></span>

   ```console
   netsh http delete sslcert ipport=<IP>:<PORT>
   ```

   <span data-ttu-id="1e44d-819">以下是 *netsh.exe* 的參考文件：</span><span class="sxs-lookup"><span data-stu-id="1e44d-819">Reference documentation for *netsh.exe*:</span></span>

   * [<span data-ttu-id="1e44d-820">超文字傳輸通訊協定 (HTTP) 的 Netsh 命令</span><span class="sxs-lookup"><span data-stu-id="1e44d-820">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
   * [<span data-ttu-id="1e44d-821">UrlPrefix 字串</span><span class="sxs-lookup"><span data-stu-id="1e44d-821">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

1. <span data-ttu-id="1e44d-822">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e44d-822">Run the app.</span></span>

   <span data-ttu-id="1e44d-823">使用 HTTP (不是 HTTPS) 搭配大於 1024 的連接埠號碼來繫結至 localhost 時，不需要系統管理員權限即可執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e44d-823">Administrator privileges aren't required to run the app when binding to localhost using HTTP (not HTTPS) with a port number greater than 1024.</span></span> <span data-ttu-id="1e44d-824">針對其他設定 (例如，使用本機 IP 位址或繫結至連接埠 443)，請使用系統管理員權限來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e44d-824">For other configurations (for example, using a local IP address or binding to port 443), run the app with administrator privileges.</span></span>

   <span data-ttu-id="1e44d-825">應用程式會在伺服器的公用 IP 位址回應。</span><span class="sxs-lookup"><span data-stu-id="1e44d-825">The app responds at the server's public IP address.</span></span> <span data-ttu-id="1e44d-826">在此範例中，是從網際網路透過伺服器的公用 IP 位址 `104.214.79.47` 連線至伺服器。</span><span class="sxs-lookup"><span data-stu-id="1e44d-826">In this example, the server is reached from the Internet at its public IP address of `104.214.79.47`.</span></span>

   <span data-ttu-id="1e44d-827">在此範例中使用的是開發憑證。</span><span class="sxs-lookup"><span data-stu-id="1e44d-827">A development certificate is used in this example.</span></span> <span data-ttu-id="1e44d-828">在略過瀏覽器的未受信任憑證警告之後，頁面會安全地載入。</span><span class="sxs-lookup"><span data-stu-id="1e44d-828">The page loads securely after bypassing the browser's untrusted certificate warning.</span></span>

   ![顯示已載入應用程式索引頁面的瀏覽器視窗](httpsys/_static/browser.png)

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="1e44d-830">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="1e44d-830">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="1e44d-831">如果是 HTTP.sys 所裝載且與來自網際網路或公司網路的要求進行互動的應用程式，在裝載於 Proxy 伺服器和負載平衡器後方時，可能需要額外的組態。</span><span class="sxs-lookup"><span data-stu-id="1e44d-831">For apps hosted by HTTP.sys that interact with requests from the Internet or a corporate network, additional configuration might be required when hosting behind proxy servers and load balancers.</span></span> <span data-ttu-id="1e44d-832">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="1e44d-832">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1e44d-833">其他資源</span><span class="sxs-lookup"><span data-stu-id="1e44d-833">Additional resources</span></span>

* <span data-ttu-id="1e44d-834">[使用 HTTP.sys 來啟用 Windows 驗證](xref:security/authentication/windowsauth#httpsys) \(機器翻譯\)</span><span class="sxs-lookup"><span data-stu-id="1e44d-834">[Enable Windows Authentication with HTTP.sys](xref:security/authentication/windowsauth#httpsys)</span></span>
* <span data-ttu-id="1e44d-835">[HTTP 伺服器 API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="1e44d-835">[HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)</span></span>
* <span data-ttu-id="1e44d-836">[aspnet/HttpSysServer GitHub 存放庫 (原始程式碼)](https://github.com/aspnet/HttpSysServer/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="1e44d-836">[aspnet/HttpSysServer GitHub repository (source code)](https://github.com/aspnet/HttpSysServer/)</span></span>
* [<span data-ttu-id="1e44d-837">主機</span><span class="sxs-lookup"><span data-stu-id="1e44d-837">The host</span></span>](xref:fundamentals/index#host)
* <xref:test/troubleshoot>

::: moniker-end
