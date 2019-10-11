---
title: ASP.NET Core 中的 HTTP.sys 網頁伺服器實作
author: guardrex
description: 深入了解 HTTP.sys，這是 Windows 上的 ASP.NET Core 網頁伺服器。 HTTP.sys 建置在 HTTP.sys 核心模式驅動程式之上，是 Kestrel 的替代方式，可以用來直接連線到網際網路而不使用 IIS。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/08/2019
uid: fundamentals/servers/httpsys
ms.openlocfilehash: acdcdca3250f2aa3445458cc2c4e5f50360338a1
ms.sourcegitcommit: 73a451e9a58ac7102f90b608d661d8c23dd9bbaf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/08/2019
ms.locfileid: "72037609"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="08c89-104">ASP.NET Core 中的 HTTP.sys 網頁伺服器實作</span><span class="sxs-lookup"><span data-stu-id="08c89-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="08c89-105">作者：[Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="08c89-105">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="08c89-106">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) 是只在 Windows 上執行的 [ASP.NET Core 網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="08c89-106">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) is a [web server for ASP.NET Core](xref:fundamentals/servers/index) that only runs on Windows.</span></span> <span data-ttu-id="08c89-107">HTTP.sys 是 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器的替代方式，提供了一些 Kestrel 未提供的功能。</span><span class="sxs-lookup"><span data-stu-id="08c89-107">HTTP.sys is an alternative to [Kestrel](xref:fundamentals/servers/kestrel) server and offers some features that Kestrel doesn't provide.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="08c89-108">HTTP.sys 與 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)不相容，且不能與 IIS 或 IIS Express 搭配使用。</span><span class="sxs-lookup"><span data-stu-id="08c89-108">HTTP.sys isn't compatible with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and can't be used with IIS or IIS Express.</span></span>

<span data-ttu-id="08c89-109">HTTP.sys 支援下列功能：</span><span class="sxs-lookup"><span data-stu-id="08c89-109">HTTP.sys supports the following features:</span></span>

* [<span data-ttu-id="08c89-110">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="08c89-110">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
* <span data-ttu-id="08c89-111">連接埠共用</span><span class="sxs-lookup"><span data-stu-id="08c89-111">Port sharing</span></span>
* <span data-ttu-id="08c89-112">使用 SNI 的 HTTPS</span><span class="sxs-lookup"><span data-stu-id="08c89-112">HTTPS with SNI</span></span>
* <span data-ttu-id="08c89-113">透過 TLS 的 HTTP/2 (Windows 10 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="08c89-113">HTTP/2 over TLS (Windows 10 or later)</span></span>
* <span data-ttu-id="08c89-114">直接檔案傳輸</span><span class="sxs-lookup"><span data-stu-id="08c89-114">Direct file transmission</span></span>
* <span data-ttu-id="08c89-115">回應快取</span><span class="sxs-lookup"><span data-stu-id="08c89-115">Response caching</span></span>
* <span data-ttu-id="08c89-116">WebSocket (Windows 8 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="08c89-116">WebSockets (Windows 8 or later)</span></span>

<span data-ttu-id="08c89-117">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="08c89-117">Supported Windows versions:</span></span>

* <span data-ttu-id="08c89-118">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="08c89-118">Windows 7 or later</span></span>
* <span data-ttu-id="08c89-119">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="08c89-119">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="08c89-120">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="08c89-120">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="08c89-121">使用 HTTP.sys 的時機</span><span class="sxs-lookup"><span data-stu-id="08c89-121">When to use HTTP.sys</span></span>

<span data-ttu-id="08c89-122">HTTP.sys 在下列部署環境中非常有用：</span><span class="sxs-lookup"><span data-stu-id="08c89-122">HTTP.sys is useful for deployments where:</span></span>

* <span data-ttu-id="08c89-123">需要直接向網際網路公開伺服器而不使用 IIS。</span><span class="sxs-lookup"><span data-stu-id="08c89-123">There's a need to expose the server directly to the Internet without using IIS.</span></span>

  ![HTTP.sys 直接與網際網路通訊](httpsys/_static/httpsys-to-internet.png)

* <span data-ttu-id="08c89-125">內部部署需要的功能無法在 Kestrel 中使用，例如 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="08c89-125">An internal deployment requires a feature not available in Kestrel, such as [Windows Authentication](xref:security/authentication/windowsauth).</span></span>

  ![HTTP.sys 直接與內部網路通訊](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="08c89-127">HTTP.sys 是成熟的技術，可抵禦許多種類的攻擊，並提供功能完整之網頁伺服器的穩固性、安全性及延展性。</span><span class="sxs-lookup"><span data-stu-id="08c89-127">HTTP.sys is mature technology that protects against many types of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="08c89-128">IIS 本身在 HTTP.sys 之上以 HTTP 接聽程式的形式執行。</span><span class="sxs-lookup"><span data-stu-id="08c89-128">IIS itself runs as an HTTP listener on top of HTTP.sys.</span></span>

## <a name="http2-support"></a><span data-ttu-id="08c89-129">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="08c89-129">HTTP/2 support</span></span>

<span data-ttu-id="08c89-130">如果符合下列基本需求，則可以針對 ASP.NET Core 應用程式啟用 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="08c89-130">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is enabled for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="08c89-131">Windows Server 2016/Windows 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="08c89-131">Windows Server 2016/Windows 10 or later</span></span>
* <span data-ttu-id="08c89-132">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 連線</span><span class="sxs-lookup"><span data-stu-id="08c89-132">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="08c89-133">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="08c89-133">TLS 1.2 or later connection</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="08c89-134">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="08c89-134">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="08c89-135">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/1.1`。</span><span class="sxs-lookup"><span data-stu-id="08c89-135">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="08c89-136">HTTP/2 預設為啟用。</span><span class="sxs-lookup"><span data-stu-id="08c89-136">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="08c89-137">如果 HTTP/2 連線尚未建立，連線會退為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="08c89-137">If an HTTP/2 connection isn't established, the connection falls back to HTTP/1.1.</span></span> <span data-ttu-id="08c89-138">Windows 的未來版本會推出 HTTP/2 設定旗標，包括使用 HTTP.sys 來停用 HTTP/2 的功能。</span><span class="sxs-lookup"><span data-stu-id="08c89-138">In a future release of Windows, HTTP/2 configuration flags will be available, including the ability to disable HTTP/2 with HTTP.sys.</span></span>

## <a name="kernel-mode-authentication-with-kerberos"></a><span data-ttu-id="08c89-139">使用 Kerberos 的核心模式驗證</span><span class="sxs-lookup"><span data-stu-id="08c89-139">Kernel mode authentication with Kerberos</span></span>

<span data-ttu-id="08c89-140">HTTP.sys 使用 Kerberos 驗證通訊協定委派給核心模式驗證。</span><span class="sxs-lookup"><span data-stu-id="08c89-140">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="08c89-141">Kerberos 和 HTTP.sys 不支援使用者模式驗證。</span><span class="sxs-lookup"><span data-stu-id="08c89-141">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="08c89-142">必須使用電腦帳戶來解密 Kerberos 權杖/票證，該權杖/票證取自 Active Directory，並由用戶端將其轉送至伺服器來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="08c89-142">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="08c89-143">請註冊主機的服務主體名稱 (SPN)，而非應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="08c89-143">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

## <a name="how-to-use-httpsys"></a><span data-ttu-id="08c89-144">如何使用 HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="08c89-144">How to use HTTP.sys</span></span>

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a><span data-ttu-id="08c89-145">設定 ASP.NET Core 應用程式使用 HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="08c89-145">Configure the ASP.NET Core app to use HTTP.sys</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="08c89-146">使用[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)（[nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)）時，不需要專案檔中的套件參考。</span><span class="sxs-lookup"><span data-stu-id="08c89-146">A package reference in the project file isn't required when using the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)).</span></span> <span data-ttu-id="08c89-147">若不是使用 `Microsoft.AspNetCore.App` 中繼套件，請將套件參考加入 [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/)。</span><span class="sxs-lookup"><span data-stu-id="08c89-147">When not using the `Microsoft.AspNetCore.App` metapackage, add a package reference to [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).</span></span>

::: moniker-end

<span data-ttu-id="08c89-148">建置主機時，呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> 擴充方法，並指定任何必要的 <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>。</span><span class="sxs-lookup"><span data-stu-id="08c89-148">Call the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> extension method when building the host, specifying any required <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>.</span></span> <span data-ttu-id="08c89-149">下列範例會將選項設定為它們的預設值：</span><span class="sxs-lookup"><span data-stu-id="08c89-149">The following example sets options to their default values:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](httpsys/samples/3.x/SampleApp/Program.cs?name=snippet1&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](httpsys/samples/2.x/SampleApp/Program.cs?name=snippet1&highlight=4-12)]

::: moniker-end

<span data-ttu-id="08c89-150">其他的 HTTP.sys 設定則透過[登錄設定](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows)處理。</span><span class="sxs-lookup"><span data-stu-id="08c89-150">Additional HTTP.sys configuration is handled through [registry settings](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).</span></span>

   <span data-ttu-id="08c89-151">**HTTP.sys 選項**</span><span class="sxs-lookup"><span data-stu-id="08c89-151">**HTTP.sys options**</span></span>

::: moniker range=">= aspnetcore-3.1"

| <span data-ttu-id="08c89-152">屬性</span><span class="sxs-lookup"><span data-stu-id="08c89-152">Property</span></span> | <span data-ttu-id="08c89-153">描述</span><span class="sxs-lookup"><span data-stu-id="08c89-153">Description</span></span> | <span data-ttu-id="08c89-154">預設</span><span class="sxs-lookup"><span data-stu-id="08c89-154">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="08c89-155">AllowSynchronousIO</span><span class="sxs-lookup"><span data-stu-id="08c89-155">AllowSynchronousIO</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | <span data-ttu-id="08c89-156">控制是否允許 `HttpContext.Request.Body` 和 `HttpContext.Response.Body` 同步輸出/輸入。</span><span class="sxs-lookup"><span data-stu-id="08c89-156">Control whether synchronous input/output is allowed for the `HttpContext.Request.Body` and `HttpContext.Response.Body`.</span></span> | `false` |
| [<span data-ttu-id="08c89-157">Authentication.AllowAnonymous</span><span class="sxs-lookup"><span data-stu-id="08c89-157">Authentication.AllowAnonymous</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | <span data-ttu-id="08c89-158">允許匿名要求。</span><span class="sxs-lookup"><span data-stu-id="08c89-158">Allow anonymous requests.</span></span> | `true` |
| [<span data-ttu-id="08c89-159">Authentication.Schemes</span><span class="sxs-lookup"><span data-stu-id="08c89-159">Authentication.Schemes</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | <span data-ttu-id="08c89-160">指定允許的驗證配置。</span><span class="sxs-lookup"><span data-stu-id="08c89-160">Specify the allowed authentication schemes.</span></span> <span data-ttu-id="08c89-161">處置接聽程式之前可隨時修改。</span><span class="sxs-lookup"><span data-stu-id="08c89-161">May be modified at any time prior to disposing the listener.</span></span> <span data-ttu-id="08c89-162">值是由 [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes) 提供：`Basic`、`Kerberos`、`Negotiate`、`None` 和 `NTLM`。</span><span class="sxs-lookup"><span data-stu-id="08c89-162">Values are provided by the [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None`, and `NTLM`.</span></span> | `None` |
| [<span data-ttu-id="08c89-163">EnableResponseCaching</span><span class="sxs-lookup"><span data-stu-id="08c89-163">EnableResponseCaching</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | <span data-ttu-id="08c89-164">針對含有合格標頭的回應嘗試[核心模式](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode)快取。</span><span class="sxs-lookup"><span data-stu-id="08c89-164">Attempt [kernel-mode](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) caching for responses with eligible headers.</span></span> <span data-ttu-id="08c89-165">回應可能不包含 `Set-Cookie`、`Vary` 或 `Pragma` 標頭。</span><span class="sxs-lookup"><span data-stu-id="08c89-165">The response may not include `Set-Cookie`, `Vary`, or `Pragma` headers.</span></span> <span data-ttu-id="08c89-166">它必須包含為 `public` 的 `Cache-Control` 標頭，且有 `shared-max-age` 或 `max-age` 值，或是 `Expires` 標頭。</span><span class="sxs-lookup"><span data-stu-id="08c89-166">It must include a `Cache-Control` header that's `public` and either a `shared-max-age` or `max-age` value, or an `Expires` header.</span></span> | `true` |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | <span data-ttu-id="08c89-167">可同時接受的數目上限。</span><span class="sxs-lookup"><span data-stu-id="08c89-167">The maximum number of concurrent accepts.</span></span> | <span data-ttu-id="08c89-168">5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span><span class="sxs-lookup"><span data-stu-id="08c89-168">5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | <span data-ttu-id="08c89-169">可接受的同時連線數量上限。</span><span class="sxs-lookup"><span data-stu-id="08c89-169">The maximum number of concurrent connections to accept.</span></span> <span data-ttu-id="08c89-170">使用 `-1` 為無限多個。</span><span class="sxs-lookup"><span data-stu-id="08c89-170">Use `-1` for infinite.</span></span> <span data-ttu-id="08c89-171">使用 `null` 以使用登錄之整個電腦的設定。</span><span class="sxs-lookup"><span data-stu-id="08c89-171">Use `null` to use the registry's machine-wide setting.</span></span> | `null`<br><span data-ttu-id="08c89-172">(無限制)</span><span class="sxs-lookup"><span data-stu-id="08c89-172">(unlimited)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | <span data-ttu-id="08c89-173">請參閱 <a href="#maxrequestbodysize">MaxRequestBodySize</a> 小節。</span><span class="sxs-lookup"><span data-stu-id="08c89-173">See the <a href="#maxrequestbodysize">MaxRequestBodySize</a> section.</span></span> | <span data-ttu-id="08c89-174">30000000 位元組</span><span class="sxs-lookup"><span data-stu-id="08c89-174">30000000 bytes</span></span><br><span data-ttu-id="08c89-175">(~28.6 MB)</span><span class="sxs-lookup"><span data-stu-id="08c89-175">(~28.6 MB)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | <span data-ttu-id="08c89-176">可以加入佇列的最大要求數目。</span><span class="sxs-lookup"><span data-stu-id="08c89-176">The maximum number of requests that can be queued.</span></span> | <span data-ttu-id="08c89-177">1000</span><span class="sxs-lookup"><span data-stu-id="08c89-177">1000</span></span> |
| `RequestQueueMode` | <span data-ttu-id="08c89-178">這會指出伺服器是否負責建立和設定要求佇列，或是否應附加至現有的佇列。</span><span class="sxs-lookup"><span data-stu-id="08c89-178">This indicates whether the server is responsible for creating and configuring the request queue, or if it should attach to an existing queue.</span></span><br><span data-ttu-id="08c89-179">附加至現有的佇列時，大部分的現有設定選項都不適用。</span><span class="sxs-lookup"><span data-stu-id="08c89-179">Most existing configuration options do not apply when attaching to an existing queue.</span></span> | `RequestQueueMode.Create` |
| `RequestQueueName` | <span data-ttu-id="08c89-180">Http.sys 要求佇列的名稱。</span><span class="sxs-lookup"><span data-stu-id="08c89-180">The name of the HTTP.sys request queue.</span></span> | <span data-ttu-id="08c89-181">`null` （匿名佇列）</span><span class="sxs-lookup"><span data-stu-id="08c89-181">`null` (Anonymous queue)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | <span data-ttu-id="08c89-182">指出若回應本文因為用戶端中斷連線而寫入失敗時，應擲回例外狀況或正常完成。</span><span class="sxs-lookup"><span data-stu-id="08c89-182">Indicate if response body writes that fail due to client disconnects should throw exceptions or complete normally.</span></span> | `false`<br><span data-ttu-id="08c89-183">(正常完成)</span><span class="sxs-lookup"><span data-stu-id="08c89-183">(complete normally)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | <span data-ttu-id="08c89-184">公開 HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> 設定，這也可在登錄中設定。</span><span class="sxs-lookup"><span data-stu-id="08c89-184">Expose the HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> configuration, which may also be configured in the registry.</span></span> <span data-ttu-id="08c89-185">API 連結可提供包括預設值在內每個設定的詳細資訊：</span><span class="sxs-lookup"><span data-stu-id="08c89-185">Follow the API links to learn more about each setting, including default values:</span></span><ul><li><span data-ttu-id="08c89-186">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; HTTP 伺服器 API 清空持續連線上實體內容的允許時間。</span><span class="sxs-lookup"><span data-stu-id="08c89-186">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; Time allowed for the HTTP Server API to drain the entity body on a Keep-Alive connection.</span></span></li><li><span data-ttu-id="08c89-187">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; 要求實體內容到達的允許時間。</span><span class="sxs-lookup"><span data-stu-id="08c89-187">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; Time allowed for the request entity body to arrive.</span></span></li><li><span data-ttu-id="08c89-188">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; HTTP 伺服器 API 剖析要求標頭的允許時間。</span><span class="sxs-lookup"><span data-stu-id="08c89-188">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; Time allowed for the HTTP Server API to parse the request header.</span></span></li><li><span data-ttu-id="08c89-189">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; 允許連線閒置的時間。</span><span class="sxs-lookup"><span data-stu-id="08c89-189">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; Time allowed for an idle connection.</span></span></li><li><span data-ttu-id="08c89-190">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; 回應的最低傳送速率。</span><span class="sxs-lookup"><span data-stu-id="08c89-190">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; The minimum send rate for the response.</span></span></li><li><span data-ttu-id="08c89-191">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; 在應用程式 擷取要求之前，將要求保留於要求佇列中的允許時間。</span><span class="sxs-lookup"><span data-stu-id="08c89-191">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; Time allowed for the request to remain in the request queue before the app picks it up.</span></span></li></ul> |  |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | <span data-ttu-id="08c89-192">指定 <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> 以向 HTTP.sys 註冊。</span><span class="sxs-lookup"><span data-stu-id="08c89-192">Specify the <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> to register with HTTP.sys.</span></span> <span data-ttu-id="08c89-193">最實用的是 [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*)，可用來將前置詞加入集合。</span><span class="sxs-lookup"><span data-stu-id="08c89-193">The most useful is [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), which is used to add a prefix to the collection.</span></span> <span data-ttu-id="08c89-194">處置接聽程式之前可隨時修改這些內容。</span><span class="sxs-lookup"><span data-stu-id="08c89-194">These may be modified at any time prior to disposing the listener.</span></span> |  |

::: moniker-end

::: moniker range="= aspnetcore-3.0"

| <span data-ttu-id="08c89-195">屬性</span><span class="sxs-lookup"><span data-stu-id="08c89-195">Property</span></span> | <span data-ttu-id="08c89-196">描述</span><span class="sxs-lookup"><span data-stu-id="08c89-196">Description</span></span> | <span data-ttu-id="08c89-197">預設</span><span class="sxs-lookup"><span data-stu-id="08c89-197">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="08c89-198">AllowSynchronousIO</span><span class="sxs-lookup"><span data-stu-id="08c89-198">AllowSynchronousIO</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | <span data-ttu-id="08c89-199">控制是否允許 `HttpContext.Request.Body` 和 `HttpContext.Response.Body` 同步輸出/輸入。</span><span class="sxs-lookup"><span data-stu-id="08c89-199">Control whether synchronous input/output is allowed for the `HttpContext.Request.Body` and `HttpContext.Response.Body`.</span></span> | `false` |
| [<span data-ttu-id="08c89-200">Authentication.AllowAnonymous</span><span class="sxs-lookup"><span data-stu-id="08c89-200">Authentication.AllowAnonymous</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | <span data-ttu-id="08c89-201">允許匿名要求。</span><span class="sxs-lookup"><span data-stu-id="08c89-201">Allow anonymous requests.</span></span> | `true` |
| [<span data-ttu-id="08c89-202">Authentication.Schemes</span><span class="sxs-lookup"><span data-stu-id="08c89-202">Authentication.Schemes</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | <span data-ttu-id="08c89-203">指定允許的驗證配置。</span><span class="sxs-lookup"><span data-stu-id="08c89-203">Specify the allowed authentication schemes.</span></span> <span data-ttu-id="08c89-204">處置接聽程式之前可隨時修改。</span><span class="sxs-lookup"><span data-stu-id="08c89-204">May be modified at any time prior to disposing the listener.</span></span> <span data-ttu-id="08c89-205">值是由 [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes) 提供：`Basic`、`Kerberos`、`Negotiate`、`None` 和 `NTLM`。</span><span class="sxs-lookup"><span data-stu-id="08c89-205">Values are provided by the [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None`, and `NTLM`.</span></span> | `None` |
| [<span data-ttu-id="08c89-206">EnableResponseCaching</span><span class="sxs-lookup"><span data-stu-id="08c89-206">EnableResponseCaching</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | <span data-ttu-id="08c89-207">針對含有合格標頭的回應嘗試[核心模式](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode)快取。</span><span class="sxs-lookup"><span data-stu-id="08c89-207">Attempt [kernel-mode](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) caching for responses with eligible headers.</span></span> <span data-ttu-id="08c89-208">回應可能不包含 `Set-Cookie`、`Vary` 或 `Pragma` 標頭。</span><span class="sxs-lookup"><span data-stu-id="08c89-208">The response may not include `Set-Cookie`, `Vary`, or `Pragma` headers.</span></span> <span data-ttu-id="08c89-209">它必須包含為 `public` 的 `Cache-Control` 標頭，且有 `shared-max-age` 或 `max-age` 值，或是 `Expires` 標頭。</span><span class="sxs-lookup"><span data-stu-id="08c89-209">It must include a `Cache-Control` header that's `public` and either a `shared-max-age` or `max-age` value, or an `Expires` header.</span></span> | `true` |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | <span data-ttu-id="08c89-210">可同時接受的數目上限。</span><span class="sxs-lookup"><span data-stu-id="08c89-210">The maximum number of concurrent accepts.</span></span> | <span data-ttu-id="08c89-211">5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span><span class="sxs-lookup"><span data-stu-id="08c89-211">5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | <span data-ttu-id="08c89-212">可接受的同時連線數量上限。</span><span class="sxs-lookup"><span data-stu-id="08c89-212">The maximum number of concurrent connections to accept.</span></span> <span data-ttu-id="08c89-213">使用 `-1` 為無限多個。</span><span class="sxs-lookup"><span data-stu-id="08c89-213">Use `-1` for infinite.</span></span> <span data-ttu-id="08c89-214">使用 `null` 以使用登錄之整個電腦的設定。</span><span class="sxs-lookup"><span data-stu-id="08c89-214">Use `null` to use the registry's machine-wide setting.</span></span> | `null`<br><span data-ttu-id="08c89-215">(無限制)</span><span class="sxs-lookup"><span data-stu-id="08c89-215">(unlimited)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | <span data-ttu-id="08c89-216">請參閱 <a href="#maxrequestbodysize">MaxRequestBodySize</a> 小節。</span><span class="sxs-lookup"><span data-stu-id="08c89-216">See the <a href="#maxrequestbodysize">MaxRequestBodySize</a> section.</span></span> | <span data-ttu-id="08c89-217">30000000 位元組</span><span class="sxs-lookup"><span data-stu-id="08c89-217">30000000 bytes</span></span><br><span data-ttu-id="08c89-218">(~28.6 MB)</span><span class="sxs-lookup"><span data-stu-id="08c89-218">(~28.6 MB)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | <span data-ttu-id="08c89-219">可以加入佇列的最大要求數目。</span><span class="sxs-lookup"><span data-stu-id="08c89-219">The maximum number of requests that can be queued.</span></span> | <span data-ttu-id="08c89-220">1000</span><span class="sxs-lookup"><span data-stu-id="08c89-220">1000</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | <span data-ttu-id="08c89-221">指出若回應本文因為用戶端中斷連線而寫入失敗時，應擲回例外狀況或正常完成。</span><span class="sxs-lookup"><span data-stu-id="08c89-221">Indicate if response body writes that fail due to client disconnects should throw exceptions or complete normally.</span></span> | `false`<br><span data-ttu-id="08c89-222">(正常完成)</span><span class="sxs-lookup"><span data-stu-id="08c89-222">(complete normally)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | <span data-ttu-id="08c89-223">公開 HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> 設定，這也可在登錄中設定。</span><span class="sxs-lookup"><span data-stu-id="08c89-223">Expose the HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> configuration, which may also be configured in the registry.</span></span> <span data-ttu-id="08c89-224">API 連結可提供包括預設值在內每個設定的詳細資訊：</span><span class="sxs-lookup"><span data-stu-id="08c89-224">Follow the API links to learn more about each setting, including default values:</span></span><ul><li><span data-ttu-id="08c89-225">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; HTTP 伺服器 API 清空持續連線上實體內容的允許時間。</span><span class="sxs-lookup"><span data-stu-id="08c89-225">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; Time allowed for the HTTP Server API to drain the entity body on a Keep-Alive connection.</span></span></li><li><span data-ttu-id="08c89-226">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; 要求實體內容到達的允許時間。</span><span class="sxs-lookup"><span data-stu-id="08c89-226">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; Time allowed for the request entity body to arrive.</span></span></li><li><span data-ttu-id="08c89-227">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; HTTP 伺服器 API 剖析要求標頭的允許時間。</span><span class="sxs-lookup"><span data-stu-id="08c89-227">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; Time allowed for the HTTP Server API to parse the request header.</span></span></li><li><span data-ttu-id="08c89-228">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; 允許連線閒置的時間。</span><span class="sxs-lookup"><span data-stu-id="08c89-228">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; Time allowed for an idle connection.</span></span></li><li><span data-ttu-id="08c89-229">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; 回應的最低傳送速率。</span><span class="sxs-lookup"><span data-stu-id="08c89-229">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; The minimum send rate for the response.</span></span></li><li><span data-ttu-id="08c89-230">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; 在應用程式 擷取要求之前，將要求保留於要求佇列中的允許時間。</span><span class="sxs-lookup"><span data-stu-id="08c89-230">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; Time allowed for the request to remain in the request queue before the app picks it up.</span></span></li></ul> |  |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | <span data-ttu-id="08c89-231">指定 <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> 以向 HTTP.sys 註冊。</span><span class="sxs-lookup"><span data-stu-id="08c89-231">Specify the <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> to register with HTTP.sys.</span></span> <span data-ttu-id="08c89-232">最實用的是 [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*)，可用來將前置詞加入集合。</span><span class="sxs-lookup"><span data-stu-id="08c89-232">The most useful is [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), which is used to add a prefix to the collection.</span></span> <span data-ttu-id="08c89-233">處置接聽程式之前可隨時修改這些內容。</span><span class="sxs-lookup"><span data-stu-id="08c89-233">These may be modified at any time prior to disposing the listener.</span></span> |  |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="08c89-234">屬性</span><span class="sxs-lookup"><span data-stu-id="08c89-234">Property</span></span> | <span data-ttu-id="08c89-235">描述</span><span class="sxs-lookup"><span data-stu-id="08c89-235">Description</span></span> | <span data-ttu-id="08c89-236">預設</span><span class="sxs-lookup"><span data-stu-id="08c89-236">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="08c89-237">AllowSynchronousIO</span><span class="sxs-lookup"><span data-stu-id="08c89-237">AllowSynchronousIO</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | <span data-ttu-id="08c89-238">控制是否允許 `HttpContext.Request.Body` 和 `HttpContext.Response.Body` 同步輸出/輸入。</span><span class="sxs-lookup"><span data-stu-id="08c89-238">Control whether synchronous input/output is allowed for the `HttpContext.Request.Body` and `HttpContext.Response.Body`.</span></span> | `true` |
| [<span data-ttu-id="08c89-239">Authentication.AllowAnonymous</span><span class="sxs-lookup"><span data-stu-id="08c89-239">Authentication.AllowAnonymous</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | <span data-ttu-id="08c89-240">允許匿名要求。</span><span class="sxs-lookup"><span data-stu-id="08c89-240">Allow anonymous requests.</span></span> | `true` |
| [<span data-ttu-id="08c89-241">Authentication.Schemes</span><span class="sxs-lookup"><span data-stu-id="08c89-241">Authentication.Schemes</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | <span data-ttu-id="08c89-242">指定允許的驗證配置。</span><span class="sxs-lookup"><span data-stu-id="08c89-242">Specify the allowed authentication schemes.</span></span> <span data-ttu-id="08c89-243">處置接聽程式之前可隨時修改。</span><span class="sxs-lookup"><span data-stu-id="08c89-243">May be modified at any time prior to disposing the listener.</span></span> <span data-ttu-id="08c89-244">值是由 [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes) 提供：`Basic`、`Kerberos`、`Negotiate`、`None` 和 `NTLM`。</span><span class="sxs-lookup"><span data-stu-id="08c89-244">Values are provided by the [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None`, and `NTLM`.</span></span> | `None` |
| [<span data-ttu-id="08c89-245">EnableResponseCaching</span><span class="sxs-lookup"><span data-stu-id="08c89-245">EnableResponseCaching</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | <span data-ttu-id="08c89-246">針對含有合格標頭的回應嘗試[核心模式](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode)快取。</span><span class="sxs-lookup"><span data-stu-id="08c89-246">Attempt [kernel-mode](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) caching for responses with eligible headers.</span></span> <span data-ttu-id="08c89-247">回應可能不包含 `Set-Cookie`、`Vary` 或 `Pragma` 標頭。</span><span class="sxs-lookup"><span data-stu-id="08c89-247">The response may not include `Set-Cookie`, `Vary`, or `Pragma` headers.</span></span> <span data-ttu-id="08c89-248">它必須包含為 `public` 的 `Cache-Control` 標頭，且有 `shared-max-age` 或 `max-age` 值，或是 `Expires` 標頭。</span><span class="sxs-lookup"><span data-stu-id="08c89-248">It must include a `Cache-Control` header that's `public` and either a `shared-max-age` or `max-age` value, or an `Expires` header.</span></span> | `true` |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | <span data-ttu-id="08c89-249">可同時接受的數目上限。</span><span class="sxs-lookup"><span data-stu-id="08c89-249">The maximum number of concurrent accepts.</span></span> | <span data-ttu-id="08c89-250">5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span><span class="sxs-lookup"><span data-stu-id="08c89-250">5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | <span data-ttu-id="08c89-251">可接受的同時連線數量上限。</span><span class="sxs-lookup"><span data-stu-id="08c89-251">The maximum number of concurrent connections to accept.</span></span> <span data-ttu-id="08c89-252">使用 `-1` 為無限多個。</span><span class="sxs-lookup"><span data-stu-id="08c89-252">Use `-1` for infinite.</span></span> <span data-ttu-id="08c89-253">使用 `null` 以使用登錄之整個電腦的設定。</span><span class="sxs-lookup"><span data-stu-id="08c89-253">Use `null` to use the registry's machine-wide setting.</span></span> | `null`<br><span data-ttu-id="08c89-254">(無限制)</span><span class="sxs-lookup"><span data-stu-id="08c89-254">(unlimited)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | <span data-ttu-id="08c89-255">請參閱 <a href="#maxrequestbodysize">MaxRequestBodySize</a> 小節。</span><span class="sxs-lookup"><span data-stu-id="08c89-255">See the <a href="#maxrequestbodysize">MaxRequestBodySize</a> section.</span></span> | <span data-ttu-id="08c89-256">30000000 位元組</span><span class="sxs-lookup"><span data-stu-id="08c89-256">30000000 bytes</span></span><br><span data-ttu-id="08c89-257">(~28.6 MB)</span><span class="sxs-lookup"><span data-stu-id="08c89-257">(~28.6 MB)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | <span data-ttu-id="08c89-258">可以加入佇列的最大要求數目。</span><span class="sxs-lookup"><span data-stu-id="08c89-258">The maximum number of requests that can be queued.</span></span> | <span data-ttu-id="08c89-259">1000</span><span class="sxs-lookup"><span data-stu-id="08c89-259">1000</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | <span data-ttu-id="08c89-260">指出若回應本文因為用戶端中斷連線而寫入失敗時，應擲回例外狀況或正常完成。</span><span class="sxs-lookup"><span data-stu-id="08c89-260">Indicate if response body writes that fail due to client disconnects should throw exceptions or complete normally.</span></span> | `false`<br><span data-ttu-id="08c89-261">(正常完成)</span><span class="sxs-lookup"><span data-stu-id="08c89-261">(complete normally)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | <span data-ttu-id="08c89-262">公開 HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> 設定，這也可在登錄中設定。</span><span class="sxs-lookup"><span data-stu-id="08c89-262">Expose the HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> configuration, which may also be configured in the registry.</span></span> <span data-ttu-id="08c89-263">API 連結可提供包括預設值在內每個設定的詳細資訊：</span><span class="sxs-lookup"><span data-stu-id="08c89-263">Follow the API links to learn more about each setting, including default values:</span></span><ul><li><span data-ttu-id="08c89-264">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; HTTP 伺服器 API 清空持續連線上實體內容的允許時間。</span><span class="sxs-lookup"><span data-stu-id="08c89-264">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; Time allowed for the HTTP Server API to drain the entity body on a Keep-Alive connection.</span></span></li><li><span data-ttu-id="08c89-265">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; 要求實體內容到達的允許時間。</span><span class="sxs-lookup"><span data-stu-id="08c89-265">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; Time allowed for the request entity body to arrive.</span></span></li><li><span data-ttu-id="08c89-266">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; HTTP 伺服器 API 剖析要求標頭的允許時間。</span><span class="sxs-lookup"><span data-stu-id="08c89-266">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; Time allowed for the HTTP Server API to parse the request header.</span></span></li><li><span data-ttu-id="08c89-267">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; 允許連線閒置的時間。</span><span class="sxs-lookup"><span data-stu-id="08c89-267">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; Time allowed for an idle connection.</span></span></li><li><span data-ttu-id="08c89-268">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; 回應的最低傳送速率。</span><span class="sxs-lookup"><span data-stu-id="08c89-268">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; The minimum send rate for the response.</span></span></li><li><span data-ttu-id="08c89-269">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; 在應用程式 擷取要求之前，將要求保留於要求佇列中的允許時間。</span><span class="sxs-lookup"><span data-stu-id="08c89-269">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; Time allowed for the request to remain in the request queue before the app picks it up.</span></span></li></ul> |  |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | <span data-ttu-id="08c89-270">指定 <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> 以向 HTTP.sys 註冊。</span><span class="sxs-lookup"><span data-stu-id="08c89-270">Specify the <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> to register with HTTP.sys.</span></span> <span data-ttu-id="08c89-271">最實用的是 [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*)，可用來將前置詞加入集合。</span><span class="sxs-lookup"><span data-stu-id="08c89-271">The most useful is [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), which is used to add a prefix to the collection.</span></span> <span data-ttu-id="08c89-272">處置接聽程式之前可隨時修改這些內容。</span><span class="sxs-lookup"><span data-stu-id="08c89-272">These may be modified at any time prior to disposing the listener.</span></span> |  |

::: moniker-end

<a name="maxrequestbodysize"></a>

<span data-ttu-id="08c89-273">**MaxRequestBodySize**</span><span class="sxs-lookup"><span data-stu-id="08c89-273">**MaxRequestBodySize**</span></span>

<span data-ttu-id="08c89-274">任何要求所允許的大小上限 (以位元組為單位)。</span><span class="sxs-lookup"><span data-stu-id="08c89-274">The maximum allowed size of any request body in bytes.</span></span> <span data-ttu-id="08c89-275">當設定為 `null` 時，要求主體大小上限為無限制。</span><span class="sxs-lookup"><span data-stu-id="08c89-275">When set to `null`, the maximum request body size is unlimited.</span></span> <span data-ttu-id="08c89-276">此限制對升級連線不會有任何影響，因為其一律為無限制。</span><span class="sxs-lookup"><span data-stu-id="08c89-276">This limit has no effect on upgraded connections, which are always unlimited.</span></span>

<span data-ttu-id="08c89-277">在 ASP.NET Core MVC 應用程式中針對單一 `IActionResult` 覆寫限制的建議方式，是在動作方法上使用 <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> 屬性：</span><span class="sxs-lookup"><span data-stu-id="08c89-277">The recommended method to override the limit in an ASP.NET Core MVC app for a single `IActionResult` is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="08c89-278">如果應用程式已開始讀取要求之後，才嘗試設定要求的限制，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="08c89-278">An exception is thrown if the app attempts to configure the limit on a request after the app has started reading the request.</span></span> <span data-ttu-id="08c89-279">`IsReadOnly` 屬性可用來指出 `MaxRequestBodySize` 屬性是否處於唯讀狀態，代表要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="08c89-279">An `IsReadOnly` property can be used to indicate if the `MaxRequestBodySize` property is in a read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="08c89-280">如果應用程式應該覆寫每個要求的 <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize>，請使用 <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>：</span><span class="sxs-lookup"><span data-stu-id="08c89-280">If the app should override <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> per-request, use the <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](httpsys/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6-7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](httpsys/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6-7)]

::: moniker-end

<span data-ttu-id="08c89-281">如果您使用 Visual Studio，請確定應用程式未設定為執行 IIS 或 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="08c89-281">If using Visual Studio, make sure the app isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="08c89-282">在 Visual Studio 中，預設啟動設定檔適用於 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="08c89-282">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="08c89-283">若要執行專案作為主控台應用程式，請手動變更選取的設定檔，如下列螢幕擷取畫面所示：</span><span class="sxs-lookup"><span data-stu-id="08c89-283">To run the project as a console app, manually change the selected profile, as shown in the following screen shot:</span></span>

![選取主控台應用程式設定檔](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a><span data-ttu-id="08c89-285">設定 Windows Server</span><span class="sxs-lookup"><span data-stu-id="08c89-285">Configure Windows Server</span></span>

1. <span data-ttu-id="08c89-286">判斷要為應用程式開啟的連接埠，然後使用 [Windows 防火牆](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule)或 [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) PowerShell Cmdlet 來開啟防火牆連接埠，以允許流量到達 HTTP.sys。</span><span class="sxs-lookup"><span data-stu-id="08c89-286">Determine the ports to open for the app and use [Windows Firewall](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) or the [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) PowerShell cmdlet to open firewall ports to allow traffic to reach HTTP.sys.</span></span> <span data-ttu-id="08c89-287">在下列命令和應用程式設定中，會使用連接埠 443。</span><span class="sxs-lookup"><span data-stu-id="08c89-287">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="08c89-288">部署至 Azure VM 時，請在[網路安全性群組](/azure/virtual-machines/windows/nsg-quickstart-portal)中開啟連接埠。</span><span class="sxs-lookup"><span data-stu-id="08c89-288">When deploying to an Azure VM, open the ports in the [Network Security Group](/azure/virtual-machines/windows/nsg-quickstart-portal).</span></span> <span data-ttu-id="08c89-289">在下列命令和應用程式設定中，會使用連接埠 443。</span><span class="sxs-lookup"><span data-stu-id="08c89-289">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="08c89-290">視需要取得並安裝 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="08c89-290">Obtain and install X.509 certificates, if required.</span></span>

   <span data-ttu-id="08c89-291">在 Windows 上，請使用 [New-SelfSignedCertificate PowerShell Cmdlet](/powershell/module/pkiclient/new-selfsignedcertificate) 來建立自我簽署的憑證。</span><span class="sxs-lookup"><span data-stu-id="08c89-291">On Windows, create self-signed certificates using the [New-SelfSignedCertificate PowerShell cmdlet](/powershell/module/pkiclient/new-selfsignedcertificate).</span></span> <span data-ttu-id="08c89-292">如需不支援的範例，請參閱 [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="08c89-292">For an unsupported example, see [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).</span></span>

   <span data-ttu-id="08c89-293">將自我簽署的憑證或 CA 簽署的憑證安裝在伺服器的 [本機電腦] > 個人 存放區中。</span><span class="sxs-lookup"><span data-stu-id="08c89-293">Install either self-signed or CA-signed certificates in the server's **Local Machine** > **Personal** store.</span></span>

1. <span data-ttu-id="08c89-294">如果應用程式是[與架構相依的部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)，請安裝 .NET Core、.NET Framework 或兩者 (如果應用程式是以 .NET Framework 為目標的 .NET Core 應用程式)。</span><span class="sxs-lookup"><span data-stu-id="08c89-294">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd), install .NET Core, .NET Framework, or both (if the app is a .NET Core app targeting the .NET Framework).</span></span>

   * <span data-ttu-id="08c89-295">**.NET Core** &ndash; 如果應用程式需要 .NET Core，請從 [.NET Core 下載](https://dotnet.microsoft.com/download)取得並執行 **.NET Core 執行階段**安裝程式。</span><span class="sxs-lookup"><span data-stu-id="08c89-295">**.NET Core** &ndash; If the app requires .NET Core, obtain and run the **.NET Core Runtime** installer from [.NET Core Downloads](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="08c89-296">請勿在伺服器上安裝完整的 SDK。</span><span class="sxs-lookup"><span data-stu-id="08c89-296">Don't install the full SDK on the server.</span></span>
   * <span data-ttu-id="08c89-297">**.NET Framework** &ndash; 如果應用程式需要 .NET Framework，請參閱 [.NET Framework 安裝指南](/dotnet/framework/install/)。</span><span class="sxs-lookup"><span data-stu-id="08c89-297">**.NET Framework** &ndash; If the app requires .NET Framework, see the [.NET Framework installation guide](/dotnet/framework/install/).</span></span> <span data-ttu-id="08c89-298">安裝必要的 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="08c89-298">Install the required .NET Framework.</span></span> <span data-ttu-id="08c89-299">您可以從 [.NET Core 下載](https://dotnet.microsoft.com/download)頁面取得最新 .NET Framework 的安裝程式。</span><span class="sxs-lookup"><span data-stu-id="08c89-299">The installer for the latest .NET Framework is available from the [.NET Core Downloads](https://dotnet.microsoft.com/download) page.</span></span>

   <span data-ttu-id="08c89-300">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)，則應用程式的部署中會包含執行階段。</span><span class="sxs-lookup"><span data-stu-id="08c89-300">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd), the app includes the runtime in its deployment.</span></span> <span data-ttu-id="08c89-301">不需要在伺服器上安裝任何架構。</span><span class="sxs-lookup"><span data-stu-id="08c89-301">No framework installation is required on the server.</span></span>

1. <span data-ttu-id="08c89-302">設定應用程式中的 URL 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="08c89-302">Configure URLs and ports in the app.</span></span>

   <span data-ttu-id="08c89-303">ASP.NET Core 預設會繫結至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="08c89-303">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="08c89-304">若要設定 URL 首碼和連接埠，選項包括：</span><span class="sxs-lookup"><span data-stu-id="08c89-304">To configure URL prefixes and ports, options include:</span></span>

   * <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
   * <span data-ttu-id="08c89-305">`urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="08c89-305">`urls` command-line argument</span></span>
   * <span data-ttu-id="08c89-306">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="08c89-306">`ASPNETCORE_URLS` environment variable</span></span>
   * <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes>

   <span data-ttu-id="08c89-307">下列程式碼範例示範如何使用 <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> 搭配位於連接埠 443 的伺服器本機 IP 位址 `10.0.0.4`：</span><span class="sxs-lookup"><span data-stu-id="08c89-307">The following code example shows how to use <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> with the server's local IP address `10.0.0.4` on port 443:</span></span>

::: moniker range=">= aspnetcore-3.0"

   [!code-csharp[](httpsys/samples_snapshot/3.x/Program.cs?highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

   [!code-csharp[](httpsys/samples_snapshot/2.x/Program.cs?highlight=6)]

::: moniker-end

   <span data-ttu-id="08c89-308">`UrlPrefixes` 的優點是針對錯誤格式的前置詞會立即產生錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="08c89-308">An advantage of `UrlPrefixes` is that an error message is generated immediately for improperly formatted prefixes.</span></span>

   <span data-ttu-id="08c89-309">`UrlPrefixes` 中的設定會覆寫 `UseUrls`/`urls`/`ASPNETCORE_URLS` 設定。</span><span class="sxs-lookup"><span data-stu-id="08c89-309">The settings in `UrlPrefixes` override `UseUrls`/`urls`/`ASPNETCORE_URLS` settings.</span></span> <span data-ttu-id="08c89-310">因此，`UseUrls`、`urls` 和 `ASPNETCORE_URLS` 環境變數的優點，是能更輕鬆地在 Kestrel 和 HTTP.sys 之間切換。</span><span class="sxs-lookup"><span data-stu-id="08c89-310">Therefore, an advantage of `UseUrls`, `urls`, and the `ASPNETCORE_URLS` environment variable is that it's easier to switch between Kestrel and HTTP.sys.</span></span>

   <span data-ttu-id="08c89-311">HTTP.sys 使用 [HTTP Server API UrlPrefix 字串格式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)。</span><span class="sxs-lookup"><span data-stu-id="08c89-311">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

   > [!WARNING]
   > <span data-ttu-id="08c89-312">請**勿**使用最上層萬用字元繫結 (`http://*:80/`與 `http://+:80`)。</span><span class="sxs-lookup"><span data-stu-id="08c89-312">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="08c89-313">最上層萬用字元繫結會導致應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="08c89-313">Top-level wildcard bindings create app security vulnerabilities.</span></span> <span data-ttu-id="08c89-314">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="08c89-314">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="08c89-315">請使用明確的主機名稱或 IP 位址，而不要使用萬用字元。</span><span class="sxs-lookup"><span data-stu-id="08c89-315">Use explicit host names or IP addresses rather than wildcards.</span></span> <span data-ttu-id="08c89-316">若您擁有整個父網域 (相對於有弱點的 `*.com`) 的控制權，則子網域萬用字元繫結 (例如 `*.mysub.com`) 便不構成安全性風險。</span><span class="sxs-lookup"><span data-stu-id="08c89-316">Subdomain wildcard binding (for example, `*.mysub.com`) isn't a security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="08c89-317">如需詳細資訊，請參閱[RFC 7230：5.4 節：主機](https://tools.ietf.org/html/rfc7230#section-5.4) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="08c89-317">For more information, see [RFC 7230: Section 5.4: Host](https://tools.ietf.org/html/rfc7230#section-5.4).</span></span>

1. <span data-ttu-id="08c89-318">在伺服器上預先註冊 URL 首碼。</span><span class="sxs-lookup"><span data-stu-id="08c89-318">Preregister URL prefixes on the server.</span></span>

   <span data-ttu-id="08c89-319">用來設定 HTTP.sys 的內建工具是 *netsh.exe*。</span><span class="sxs-lookup"><span data-stu-id="08c89-319">The built-in tool for configuring HTTP.sys is *netsh.exe*.</span></span> <span data-ttu-id="08c89-320">*netsh.exe* 是用來保留 URL 前置詞，並指派 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="08c89-320">*netsh.exe* is used to reserve URL prefixes and assign X.509 certificates.</span></span> <span data-ttu-id="08c89-321">此工具需要系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="08c89-321">The tool requires administrator privileges.</span></span>

   <span data-ttu-id="08c89-322">使用 *netsh.exe* 工具來為應用程式註冊 URL：</span><span class="sxs-lookup"><span data-stu-id="08c89-322">Use the *netsh.exe* tool to register URLs for the app:</span></span>

   ```console
   netsh http add urlacl url=<URL> user=<USER>
   ```

   * <span data-ttu-id="08c89-323">`<URL>` &ndash; 完整的「統一資源定位器」(URL)。</span><span class="sxs-lookup"><span data-stu-id="08c89-323">`<URL>` &ndash; The fully qualified Uniform Resource Locator (URL).</span></span> <span data-ttu-id="08c89-324">請勿使用萬用字元繫結。</span><span class="sxs-lookup"><span data-stu-id="08c89-324">Don't use a wildcard binding.</span></span> <span data-ttu-id="08c89-325">請使用有效的主機名稱或本機 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="08c89-325">Use a valid hostname or local IP address.</span></span> <span data-ttu-id="08c89-326">URL 必須包含結尾斜線。</span><span class="sxs-lookup"><span data-stu-id="08c89-326">*The URL must include a trailing slash.*</span></span>
   * <span data-ttu-id="08c89-327">`<USER>` &ndash; 會指定使用者或「使用者-群組」名稱。</span><span class="sxs-lookup"><span data-stu-id="08c89-327">`<USER>` &ndash; Specifies the user or user-group name.</span></span>

   <span data-ttu-id="08c89-328">在以下範例中，伺服器的本機 IP 位址是 `10.0.0.4`：</span><span class="sxs-lookup"><span data-stu-id="08c89-328">In the following example, the local IP address of the server is `10.0.0.4`:</span></span>

   ```console
   netsh http add urlacl url=https://10.0.0.4:443/ user=Users
   ```

   <span data-ttu-id="08c89-329">已註冊 URL 時，此工具會以 `URL reservation successfully added` 回應。</span><span class="sxs-lookup"><span data-stu-id="08c89-329">When a URL is registered, the tool responds with `URL reservation successfully added`.</span></span>

   <span data-ttu-id="08c89-330">若要刪除已註冊的 URL，請使用 `delete urlacl` 命令：</span><span class="sxs-lookup"><span data-stu-id="08c89-330">To delete a registered URL, use the `delete urlacl` command:</span></span>

   ```console
   netsh http delete urlacl url=<URL>
   ```

1. <span data-ttu-id="08c89-331">在伺服器上註冊 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="08c89-331">Register X.509 certificates on the server.</span></span>

   <span data-ttu-id="08c89-332">使用 *netsh.exe* 工具來為應用程式註冊憑證：</span><span class="sxs-lookup"><span data-stu-id="08c89-332">Use the *netsh.exe* tool to register certificates for the app:</span></span>

   ```console
   netsh http add sslcert ipport=<IP>:<PORT> certhash=<THUMBPRINT> appid="{<GUID>}"
   ```

   * <span data-ttu-id="08c89-333">`<IP>` &ndash; 會指定繫結的本機 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="08c89-333">`<IP>` &ndash; Specifies the local IP address for the binding.</span></span> <span data-ttu-id="08c89-334">請勿使用萬用字元繫結。</span><span class="sxs-lookup"><span data-stu-id="08c89-334">Don't use a wildcard binding.</span></span> <span data-ttu-id="08c89-335">請使用有效的 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="08c89-335">Use a valid IP address.</span></span>
   * <span data-ttu-id="08c89-336">`<PORT>` &ndash; 會指定繫結的連接埠。</span><span class="sxs-lookup"><span data-stu-id="08c89-336">`<PORT>` &ndash; Specifies the port for the binding.</span></span>
   * <span data-ttu-id="08c89-337">`<THUMBPRINT>` &ndash; X.509 憑證指紋。</span><span class="sxs-lookup"><span data-stu-id="08c89-337">`<THUMBPRINT>` &ndash; The X.509 certificate thumbprint.</span></span>
   * <span data-ttu-id="08c89-338">`<GUID>` &ndash; 開發人員所產生來代表應用程式的 GUID，供參考使用。</span><span class="sxs-lookup"><span data-stu-id="08c89-338">`<GUID>` &ndash; A developer-generated GUID to represent the app for informational purposes.</span></span>

   <span data-ttu-id="08c89-339">為了便於參考，請將 GUID 以套件標記的形式儲存在應用程式中：</span><span class="sxs-lookup"><span data-stu-id="08c89-339">For reference purposes, store the GUID in the app as a package tag:</span></span>

   * <span data-ttu-id="08c89-340">在 Visual Studio 中：</span><span class="sxs-lookup"><span data-stu-id="08c89-340">In Visual Studio:</span></span>
     * <span data-ttu-id="08c89-341">在 [方案總管] 中的專案上按一下滑鼠右鍵，然後選取 [屬性]，以開啟應用程式的專案屬性。</span><span class="sxs-lookup"><span data-stu-id="08c89-341">Open the app's project properties by right-clicking on the app in **Solution Explorer** and selecting **Properties**.</span></span>
     * <span data-ttu-id="08c89-342">選取 [套件] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="08c89-342">Select the **Package** tab.</span></span>
     * <span data-ttu-id="08c89-343">輸入您在 [標記] 欄位中建立的 GUID。</span><span class="sxs-lookup"><span data-stu-id="08c89-343">Enter the GUID that you created in the **Tags** field.</span></span>
   * <span data-ttu-id="08c89-344">不是使用 Visual Studio 時：</span><span class="sxs-lookup"><span data-stu-id="08c89-344">When not using Visual Studio:</span></span>
     * <span data-ttu-id="08c89-345">開啟應用程式的專案檔。</span><span class="sxs-lookup"><span data-stu-id="08c89-345">Open the app's project file.</span></span>
     * <span data-ttu-id="08c89-346">將 `<PackageTags>` 屬性搭配您所建立的 GUID 新增至新的或現有的 `<PropertyGroup>`：</span><span class="sxs-lookup"><span data-stu-id="08c89-346">Add a `<PackageTags>` property to a new or existing `<PropertyGroup>` with the GUID that you created:</span></span>

       ```xml
       <PropertyGroup>
         <PackageTags>9412ee86-c21b-4eb8-bd89-f650fbf44931</PackageTags>
       </PropertyGroup>
       ```

   <span data-ttu-id="08c89-347">在以下範例中：</span><span class="sxs-lookup"><span data-stu-id="08c89-347">In the following example:</span></span>

   * <span data-ttu-id="08c89-348">伺服器的本機 IP 位址是 `10.0.0.4`。</span><span class="sxs-lookup"><span data-stu-id="08c89-348">The local IP address of the server is `10.0.0.4`.</span></span>
   * <span data-ttu-id="08c89-349">線上隨機 GUID 產生器會提供 `appid` 值。</span><span class="sxs-lookup"><span data-stu-id="08c89-349">An online random GUID generator provides the `appid` value.</span></span>

   ```console
   netsh http add sslcert 
       ipport=10.0.0.4:443 
       certhash=b66ee04419d4ee37464ab8785ff02449980eae10 
       appid="{9412ee86-c21b-4eb8-bd89-f650fbf44931}"
   ```

   <span data-ttu-id="08c89-350">已註冊憑證時，此工具會以 `SSL Certificate successfully added` 回應。</span><span class="sxs-lookup"><span data-stu-id="08c89-350">When a certificate is registered, the tool responds with `SSL Certificate successfully added`.</span></span>

   <span data-ttu-id="08c89-351">若要刪除憑證註冊，請使用 `delete sslcert` 命令：</span><span class="sxs-lookup"><span data-stu-id="08c89-351">To delete a certificate registration, use the `delete sslcert` command:</span></span>

   ```console
   netsh http delete sslcert ipport=<IP>:<PORT>
   ```

   <span data-ttu-id="08c89-352">以下是 *netsh.exe* 的參考文件：</span><span class="sxs-lookup"><span data-stu-id="08c89-352">Reference documentation for *netsh.exe*:</span></span>

   * [<span data-ttu-id="08c89-353">超文字傳輸通訊協定 (HTTP) 的 Netsh 命令</span><span class="sxs-lookup"><span data-stu-id="08c89-353">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
   * [<span data-ttu-id="08c89-354">UrlPrefix 字串</span><span class="sxs-lookup"><span data-stu-id="08c89-354">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

1. <span data-ttu-id="08c89-355">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="08c89-355">Run the app.</span></span>

   <span data-ttu-id="08c89-356">使用 HTTP (不是 HTTPS) 搭配大於 1024 的連接埠號碼來繫結至 localhost 時，不需要系統管理員權限即可執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="08c89-356">Administrator privileges aren't required to run the app when binding to localhost using HTTP (not HTTPS) with a port number greater than 1024.</span></span> <span data-ttu-id="08c89-357">針對其他設定 (例如，使用本機 IP 位址或繫結至連接埠 443)，請使用系統管理員權限來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="08c89-357">For other configurations (for example, using a local IP address or binding to port 443), run the app with administrator privileges.</span></span>

   <span data-ttu-id="08c89-358">應用程式會在伺服器的公用 IP 位址回應。</span><span class="sxs-lookup"><span data-stu-id="08c89-358">The app responds at the server's public IP address.</span></span> <span data-ttu-id="08c89-359">在此範例中，是從網際網路透過伺服器的公用 IP 位址 `104.214.79.47` 連線至伺服器。</span><span class="sxs-lookup"><span data-stu-id="08c89-359">In this example, the server is reached from the Internet at its public IP address of `104.214.79.47`.</span></span>

   <span data-ttu-id="08c89-360">在此範例中使用的是開發憑證。</span><span class="sxs-lookup"><span data-stu-id="08c89-360">A development certificate is used in this example.</span></span> <span data-ttu-id="08c89-361">在略過瀏覽器的未受信任憑證警告之後，頁面會安全地載入。</span><span class="sxs-lookup"><span data-stu-id="08c89-361">The page loads securely after bypassing the browser's untrusted certificate warning.</span></span>

   ![顯示已載入應用程式索引頁面的瀏覽器視窗](httpsys/_static/browser.png)

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="08c89-363">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="08c89-363">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="08c89-364">如果是 HTTP.sys 所裝載且與來自網際網路或公司網路的要求進行互動的應用程式，在裝載於 Proxy 伺服器和負載平衡器後方時，可能需要額外的組態。</span><span class="sxs-lookup"><span data-stu-id="08c89-364">For apps hosted by HTTP.sys that interact with requests from the Internet or a corporate network, additional configuration might be required when hosting behind proxy servers and load balancers.</span></span> <span data-ttu-id="08c89-365">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="08c89-365">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="08c89-366">其他資源</span><span class="sxs-lookup"><span data-stu-id="08c89-366">Additional resources</span></span>

* <span data-ttu-id="08c89-367">[使用 HTTP.sys 來啟用 Windows 驗證](xref:security/authentication/windowsauth#httpsys) \(機器翻譯\)</span><span class="sxs-lookup"><span data-stu-id="08c89-367">[Enable Windows Authentication with HTTP.sys](xref:security/authentication/windowsauth#httpsys)</span></span>
* <span data-ttu-id="08c89-368">[HTTP 伺服器 API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="08c89-368">[HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)</span></span>
* <span data-ttu-id="08c89-369">[aspnet/HttpSysServer GitHub 存放庫 (原始程式碼)](https://github.com/aspnet/HttpSysServer/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="08c89-369">[aspnet/HttpSysServer GitHub repository (source code)](https://github.com/aspnet/HttpSysServer/)</span></span>
* [<span data-ttu-id="08c89-370">主機</span><span class="sxs-lookup"><span data-stu-id="08c89-370">The host</span></span>](xref:fundamentals/index#host)
* <xref:test/troubleshoot>
