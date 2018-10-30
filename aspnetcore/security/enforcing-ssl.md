---
title: 強制使用 ASP.NET Core 中的 HTTPS
author: rick-anderson
description: 了解如何在 ASP.NET Core web 應用程式需要 HTTPS/TLS。
ms.author: riande
ms.custom: mvc
ms.date: 10/18/2018
uid: security/enforcing-ssl
ms.openlocfilehash: afa40db4c84820db91878bb98dae082b3dd9a2e2
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244876"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="fb15b-103">強制使用 ASP.NET Core 中的 HTTPS</span><span class="sxs-lookup"><span data-stu-id="fb15b-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="fb15b-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fb15b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fb15b-105">本文件說明如何：</span><span class="sxs-lookup"><span data-stu-id="fb15b-105">This document shows how to:</span></span>

* <span data-ttu-id="fb15b-106">需要 HTTPS 進行的所有要求。</span><span class="sxs-lookup"><span data-stu-id="fb15b-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="fb15b-107">將所有 HTTP 要求重新都導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="fb15b-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="fb15b-108">沒有可用的 API 可以防止用戶端第一次要求中傳送機密資料。</span><span class="sxs-lookup"><span data-stu-id="fb15b-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="fb15b-109">請勿**未**使用[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute)接收機密資訊的 Web Api 上。</span><span class="sxs-lookup"><span data-stu-id="fb15b-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="fb15b-110">`RequireHttpsAttribute` 若要從 HTTP 至 HTTPS 的瀏覽器重新導向，會使用 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="fb15b-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="fb15b-111">API 用戶端可能不了解，或是遵循從 HTTP 重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="fb15b-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="fb15b-112">此類用戶端可能會透過 HTTP 傳送資訊。</span><span class="sxs-lookup"><span data-stu-id="fb15b-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="fb15b-113">Web Api 應執行下列之一：</span><span class="sxs-lookup"><span data-stu-id="fb15b-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="fb15b-114">不在 HTTP 上接聽。</span><span class="sxs-lookup"><span data-stu-id="fb15b-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="fb15b-115">關閉與狀態碼 400 （不正確的要求） 的連線，並不會提供要求。</span><span class="sxs-lookup"><span data-stu-id="fb15b-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

## <a name="require-https"></a><span data-ttu-id="fb15b-116">需要 HTTPS</span><span class="sxs-lookup"><span data-stu-id="fb15b-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="fb15b-117">我們建議您的生產環境 ASP.NET Core web 應用程式呼叫：</span><span class="sxs-lookup"><span data-stu-id="fb15b-117">We recommend that production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="fb15b-118">HTTPS 重新導向中介軟體 (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) 將 HTTP 要求重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="fb15b-118">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) to redirect HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="fb15b-119">HSTS 中介軟體 ([UseHsts](#http-strict-transport-security-protocol-hsts)) 傳送給用戶端的 HTTP Strict Transport Security 通訊協定 (HSTS) 標頭。</span><span class="sxs-lookup"><span data-stu-id="fb15b-119">HSTS Middleware ([UseHsts](#http-strict-transport-security-protocol-hsts)) to send HTTP Strict Transport Security Protocol (HSTS) headers to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="fb15b-120">在反向 proxy 組態中部署的應用程式允許 proxy 處理連線安全性 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="fb15b-120">Apps deployed in a reverse proxy configuration allow the proxy to handle connection security (HTTPS).</span></span> <span data-ttu-id="fb15b-121">如果 proxy 也會處理 HTTPS 重新導向，則不需要使用 HTTPS 重新導向中介軟體。</span><span class="sxs-lookup"><span data-stu-id="fb15b-121">If the proxy also handles HTTPS redirection, there's no need to use HTTPS Redirection Middleware.</span></span> <span data-ttu-id="fb15b-122">如果 proxy 伺服器也會負責編寫 HSTS 標頭 (例如[原生 HSTS 支援 IIS 10.0 (1709) 或更新版本](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support))，HSTS 中介軟體不需要應用程式。</span><span class="sxs-lookup"><span data-stu-id="fb15b-122">If the proxy server also handles writing HSTS headers (for example, [native HSTS support in IIS 10.0 (1709) or later](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS Middleware isn't required by the app.</span></span> <span data-ttu-id="fb15b-123">如需詳細資訊，請參閱 <<c0> [ 退出的 HTTPS/HSTS 專案建立](#opt-out-of-httpshsts-on-project-creation)。</span><span class="sxs-lookup"><span data-stu-id="fb15b-123">For more information, see [Opt-out of HTTPS/HSTS on project creation](#opt-out-of-httpshsts-on-project-creation).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="fb15b-124">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="fb15b-124">UseHttpsRedirection</span></span>

<span data-ttu-id="fb15b-125">下列程式碼會呼叫`UseHttpsRedirection`在`Startup`類別：</span><span class="sxs-lookup"><span data-stu-id="fb15b-125">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="fb15b-126">上述反白顯示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="fb15b-126">The preceding highlighted code:</span></span>

* <span data-ttu-id="fb15b-127">使用預設[HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect))。</span><span class="sxs-lookup"><span data-stu-id="fb15b-127">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="fb15b-128">使用預設[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) 但覆寫`ASPNETCORE_HTTPS_PORT`環境變數或[IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)。</span><span class="sxs-lookup"><span data-stu-id="fb15b-128">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="fb15b-129">我們建議使用暫時重新導向，而不是永久重新導向。</span><span class="sxs-lookup"><span data-stu-id="fb15b-129">We recommend using temporary redirects rather than permanent redirects.</span></span> <span data-ttu-id="fb15b-130">連結快取，會在開發環境中造成不穩定的行為。</span><span class="sxs-lookup"><span data-stu-id="fb15b-130">Link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="fb15b-131">如果您想要傳送的永久重新導向狀態碼，在非開發環境中應用程式時，請參閱[設定在生產環境中的永久重新導向](#configure-permanent-redirects-in-production)一節。</span><span class="sxs-lookup"><span data-stu-id="fb15b-131">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, see the [Configure permanent redirects in production](#configure-permanent-redirects-in-production) section.</span></span> <span data-ttu-id="fb15b-132">我們建議您使用[HSTS](#http-strict-transport-security-protocol-hsts)來只保護資源的用戶端通知要求應傳送至 （只在生產環境） 中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="fb15b-132">We recommend using [HSTS](#http-strict-transport-security-protocol-hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

### <a name="port-configuration"></a><span data-ttu-id="fb15b-133">連接埠組態</span><span class="sxs-lookup"><span data-stu-id="fb15b-133">Port configuration</span></span>

<span data-ttu-id="fb15b-134">將不安全的要求重新導向至 HTTPS 連接埠必須適用於中介軟體。</span><span class="sxs-lookup"><span data-stu-id="fb15b-134">A port must be available for the middleware to redirect an insecure request to HTTPS.</span></span> <span data-ttu-id="fb15b-135">如果沒有連接埠可用：</span><span class="sxs-lookup"><span data-stu-id="fb15b-135">If no port is available:</span></span>

* <span data-ttu-id="fb15b-136">不會發生重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="fb15b-136">Redirection to HTTPS doesn't occur.</span></span>
* <span data-ttu-id="fb15b-137">中介軟體會記錄警告 「 無法判定重新導向的 https 連接埠。 」</span><span class="sxs-lookup"><span data-stu-id="fb15b-137">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

<span data-ttu-id="fb15b-138">指定 HTTPS 連接埠，使用下列方法之一：</span><span class="sxs-lookup"><span data-stu-id="fb15b-138">Specify the HTTPS port using any of the following approaches:</span></span>

* <span data-ttu-id="fb15b-139">設定[HttpsRedirectionOptions.HttpsPort](#options)。</span><span class="sxs-lookup"><span data-stu-id="fb15b-139">Set [HttpsRedirectionOptions.HttpsPort](#options).</span></span>
* <span data-ttu-id="fb15b-140">設定`ASPNETCORE_HTTPS_PORT`環境變數或[https_port Web 主機組態設定](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="fb15b-140">Set the `ASPNETCORE_HTTPS_PORT` environment variable or [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

  <span data-ttu-id="fb15b-141">**索引鍵**: `https_port`</span><span class="sxs-lookup"><span data-stu-id="fb15b-141">**Key**: `https_port`</span></span>  
  <span data-ttu-id="fb15b-142">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="fb15b-142">**Type**: *string*</span></span>  
  <span data-ttu-id="fb15b-143">**預設**： 未設定預設值。</span><span class="sxs-lookup"><span data-stu-id="fb15b-143">**Default**: A default value isn't set.</span></span>  
  <span data-ttu-id="fb15b-144">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="fb15b-144">**Set using**: `UseSetting`</span></span>  
  <span data-ttu-id="fb15b-145">**環境變數**: `<PREFIX_>HTTPS_PORT` (前置詞`ASPNETCORE_`使用時[Web 主機](xref:fundamentals/host/web-host)。)</span><span class="sxs-lookup"><span data-stu-id="fb15b-145">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the [Web Host](xref:fundamentals/host/web-host).)</span></span>

  <span data-ttu-id="fb15b-146">設定時<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>在`Program`:</span><span class="sxs-lookup"><span data-stu-id="fb15b-146">When configuring an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> in `Program`:</span></span>

  [!code-csharp[](enforcing-ssl/sample-snapshot/Program.cs?name=snippet_Program&highlight=10)]
* <span data-ttu-id="fb15b-147">表示具有安全的配置使用的連接埠`ASPNETCORE_URLS`環境變數。</span><span class="sxs-lookup"><span data-stu-id="fb15b-147">Indicate a port with the secure scheme using the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="fb15b-148">環境變數設定伺服器。</span><span class="sxs-lookup"><span data-stu-id="fb15b-148">The environment variable configures the server.</span></span> <span data-ttu-id="fb15b-149">中介軟體間接探索透過 HTTPS 連接埠<xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>。</span><span class="sxs-lookup"><span data-stu-id="fb15b-149">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="fb15b-150">(沒有**不**反向 proxy 的部署中運作。)</span><span class="sxs-lookup"><span data-stu-id="fb15b-150">(Does **not** work in reverse proxy deployments.)</span></span>
* <span data-ttu-id="fb15b-151">在開發中，請在中設定的 HTTPS URL *launchsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="fb15b-151">In development, set an HTTPS URL in *launchsettings.json*.</span></span> <span data-ttu-id="fb15b-152">使用 IIS Express 時，請啟用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="fb15b-152">Enable HTTPS when IIS Express is used.</span></span>
* <span data-ttu-id="fb15b-153">設定向外公開 edge 部署的 HTTPS URL 端點[Kestrel](xref:fundamentals/servers/kestrel)或是[HTTP.sys](xref:fundamentals/servers/httpsys)。</span><span class="sxs-lookup"><span data-stu-id="fb15b-153">Configure an HTTPS URL endpoint for a public-facing edge deployment of [Kestrel](xref:fundamentals/servers/kestrel) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span> <span data-ttu-id="fb15b-154">只有**一個 HTTPS 連接埠**應用程式使用。</span><span class="sxs-lookup"><span data-stu-id="fb15b-154">Only **one HTTPS port** is used by the app.</span></span> <span data-ttu-id="fb15b-155">中介軟體會探索透過連接埠<xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>。</span><span class="sxs-lookup"><span data-stu-id="fb15b-155">The middleware discovers the port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span>

> [!NOTE]
> <span data-ttu-id="fb15b-156">應用程式執行 （例如 IIS、 IIS Express），在反向 proxy 後方時`IServerAddressesFeature`無法使用。</span><span class="sxs-lookup"><span data-stu-id="fb15b-156">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="fb15b-157">您必須手動設定連接埠。</span><span class="sxs-lookup"><span data-stu-id="fb15b-157">The port must be manually configured.</span></span> <span data-ttu-id="fb15b-158">當未設定連接埠時，不是重新導向要求。</span><span class="sxs-lookup"><span data-stu-id="fb15b-158">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="fb15b-159">使用 Kestrel 或 HTTP.sys 時做為向外公開邊緣伺服器，Kestrel 或 HTTP.sys 必須設定為接聽兩者：</span><span class="sxs-lookup"><span data-stu-id="fb15b-159">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>

* <span data-ttu-id="fb15b-160">會在重新導向用戶端的安全連接埠 (通常，在生產環境和開發 5001 443)。</span><span class="sxs-lookup"><span data-stu-id="fb15b-160">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
* <span data-ttu-id="fb15b-161">不安全的連接埠 (通常，在生產環境中為 80) 與開發中的 5000。</span><span class="sxs-lookup"><span data-stu-id="fb15b-161">The insecure port (typically, 80 in production and 5000 in development).</span></span>

<span data-ttu-id="fb15b-162">為了讓應用程式用戶端收到不安全的要求，並重新導向至安全的連接埠的用戶端必須能夠使用不安全的連接埠。</span><span class="sxs-lookup"><span data-stu-id="fb15b-162">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect the client to the secure port.</span></span>

<span data-ttu-id="fb15b-163">如需詳細資訊，請參閱 < [Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration)或<xref:fundamentals/servers/httpsys>。</span><span class="sxs-lookup"><span data-stu-id="fb15b-163">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

### <a name="deployment-scenarios"></a><span data-ttu-id="fb15b-164">部署案例</span><span class="sxs-lookup"><span data-stu-id="fb15b-164">Deployment scenarios</span></span>

<span data-ttu-id="fb15b-165">用戶端與伺服器之間的任何防火牆也必須開啟流量的通訊連接埠。</span><span class="sxs-lookup"><span data-stu-id="fb15b-165">Any firewall between the client and server must also have communication ports open for traffic.</span></span>

<span data-ttu-id="fb15b-166">如果要求轉送的反向 proxy 設定，使用[轉送標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)之前呼叫 HTTPS 重新導向中介軟體。</span><span class="sxs-lookup"><span data-stu-id="fb15b-166">If requests are forwarded in a reverse proxy configuration, use [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) before calling HTTPS Redirection Middleware.</span></span> <span data-ttu-id="fb15b-167">轉送標頭中介軟體更新`Request.Scheme`，並使用`X-Forwarded-Proto`標頭。</span><span class="sxs-lookup"><span data-stu-id="fb15b-167">Forwarded Headers Middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header.</span></span> <span data-ttu-id="fb15b-168">中介軟體允許重新導向 Uri 和其他安全性原則才能正常運作。</span><span class="sxs-lookup"><span data-stu-id="fb15b-168">The middleware permits redirect URIs and other security policies to work correctly.</span></span> <span data-ttu-id="fb15b-169">轉送標頭中介軟體不使用時後, 端應用程式可能不會收到正確的配置和得到的重新導向迴圈。</span><span class="sxs-lookup"><span data-stu-id="fb15b-169">When Forwarded Headers Middleware isn't used, the backend app might not receive the correct scheme and end up in a redirect loop.</span></span> <span data-ttu-id="fb15b-170">常見的使用者錯誤訊息是發生太多的重新導向。</span><span class="sxs-lookup"><span data-stu-id="fb15b-170">A common end user error message is that too many redirects have occurred.</span></span>

<span data-ttu-id="fb15b-171">部署至 Azure App Service 時，請依照下列中的指導方針[教學課程： 將現有的自訂 SSL 憑證繫結至 Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl)。</span><span class="sxs-lookup"><span data-stu-id="fb15b-171">When deploying to Azure App Service, follow the guidance in [Tutorial: Bind an existing custom SSL certificate to Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

### <a name="options"></a><span data-ttu-id="fb15b-172">選項</span><span class="sxs-lookup"><span data-stu-id="fb15b-172">Options</span></span>

<span data-ttu-id="fb15b-173">下列醒目提示程式碼會呼叫[AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection)設定中介軟體選項：</span><span class="sxs-lookup"><span data-stu-id="fb15b-173">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="fb15b-174">呼叫`AddHttpsRedirection`時，才需要變更的值`HttpsPort`或`RedirectStatusCode`。</span><span class="sxs-lookup"><span data-stu-id="fb15b-174">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="fb15b-175">上述反白顯示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="fb15b-175">The preceding highlighted code:</span></span>

* <span data-ttu-id="fb15b-176">設定組[HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*)到<xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>，這是預設值。</span><span class="sxs-lookup"><span data-stu-id="fb15b-176">Sets [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) to <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, which is the default value.</span></span> <span data-ttu-id="fb15b-177">使用的欄位<xref:Microsoft.AspNetCore.Http.StatusCodes>類別的工作分派`RedirectStatusCode`。</span><span class="sxs-lookup"><span data-stu-id="fb15b-177">Use the fields of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="fb15b-178">設定 HTTPS 連接埠為 5001。</span><span class="sxs-lookup"><span data-stu-id="fb15b-178">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="fb15b-179">預設值為 443。</span><span class="sxs-lookup"><span data-stu-id="fb15b-179">The default value is 443.</span></span>

#### <a name="configure-permanent-redirects-in-production"></a><span data-ttu-id="fb15b-180">在生產環境中設定永久重新導向</span><span class="sxs-lookup"><span data-stu-id="fb15b-180">Configure permanent redirects in production</span></span>

<span data-ttu-id="fb15b-181">中介軟體會預設為傳送[Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)與所有重新導向。</span><span class="sxs-lookup"><span data-stu-id="fb15b-181">The middleware defaults to sending a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) with all redirects.</span></span> <span data-ttu-id="fb15b-182">如果您想要傳送的永久重新導向狀態碼，在非開發環境中應用程式時，包裝中介軟體選項的組態中的非開發環境的條件式檢查。</span><span class="sxs-lookup"><span data-stu-id="fb15b-182">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, wrap the middleware options configuration in a conditional check for a non-Development environment.</span></span>

<span data-ttu-id="fb15b-183">設定時`IWebHostBuilder`中*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="fb15b-183">When configuring an `IWebHostBuilder` in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IHostingEnvironment (stored in _env) is injected into the Startup class.
    if (!_env.IsDevelopment())
    {
        services.AddHttpsRedirection(options =>
        {
            options.RedirectStatusCode = StatusCodes.Status308PermanentRedirect;
            options.HttpsPort = 443;
        });
    }
}
```

## <a name="https-redirection-middleware-alternative-approach"></a><span data-ttu-id="fb15b-184">HTTPS 重新導向中介軟體的替代方法</span><span class="sxs-lookup"><span data-stu-id="fb15b-184">HTTPS Redirection Middleware alternative approach</span></span>

<span data-ttu-id="fb15b-185">除了使用 HTTPS 重新導向中介軟體 (`UseHttpsRedirection`) 是使用 URL 重寫中介軟體 (`AddRedirectToHttps`)。</span><span class="sxs-lookup"><span data-stu-id="fb15b-185">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="fb15b-186">`AddRedirectToHttps` 也可以設定的狀態碼和連接埠重新導向為執行時。</span><span class="sxs-lookup"><span data-stu-id="fb15b-186">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="fb15b-187">如需詳細資訊，請參閱 < [URL 重寫中介軟體](xref:fundamentals/url-rewriting)。</span><span class="sxs-lookup"><span data-stu-id="fb15b-187">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="fb15b-188">當重新導向至 HTTPS，而不需要額外的重新導向規則，我們建議使用 HTTPS 重新導向中介軟體 (`UseHttpsRedirection`) 本主題中所述。</span><span class="sxs-lookup"><span data-stu-id="fb15b-188">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="fb15b-189">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute)用來要求 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="fb15b-189">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="fb15b-190">`[RequireHttpsAttribute]` 可以裝飾控制器或方法，或可以全域套用。</span><span class="sxs-lookup"><span data-stu-id="fb15b-190">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="fb15b-191">若要全域套用的屬性，新增下列程式碼`ConfigureServices`在`Startup`:</span><span class="sxs-lookup"><span data-stu-id="fb15b-191">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="fb15b-192">上述反白顯示的程式碼會要求所有要求都使用`HTTPS`; 因此，HTTP 要求會被忽略。</span><span class="sxs-lookup"><span data-stu-id="fb15b-192">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="fb15b-193">而下列反白顯示的程式碼會將所有 HTTP 要求都重新導向至 HTTPS:</span><span class="sxs-lookup"><span data-stu-id="fb15b-193">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="fb15b-194">如需詳細資訊，請參閱 < [URL 重寫中介軟體](xref:fundamentals/url-rewriting)。</span><span class="sxs-lookup"><span data-stu-id="fb15b-194">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="fb15b-195">中介軟體也允許應用程式執行時重新導向設定的狀態碼或狀態碼和連接埠。</span><span class="sxs-lookup"><span data-stu-id="fb15b-195">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="fb15b-196">全域使用 HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) 是安全性最佳作法。</span><span class="sxs-lookup"><span data-stu-id="fb15b-196">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="fb15b-197">將`[RequireHttps]`屬性套用至所有控制器，不會比全域使用 HTTPS 來的安全。</span><span class="sxs-lookup"><span data-stu-id="fb15b-197">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="fb15b-198">您無法保證`[RequireHttps]`新增新的控制器和 Razor 頁面時，屬性會套用。</span><span class="sxs-lookup"><span data-stu-id="fb15b-198">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="fb15b-199">HTTP Strict Transport 安全性通訊協定 (HSTS)</span><span class="sxs-lookup"><span data-stu-id="fb15b-199">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="fb15b-200">每個[OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project)， [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet)是透過回應標頭使用的 web 應用程式所指定的選擇加入的安全性增強功能。</span><span class="sxs-lookup"><span data-stu-id="fb15b-200">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="fb15b-201">當[瀏覽器支援 HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)收到此標頭：</span><span class="sxs-lookup"><span data-stu-id="fb15b-201">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="fb15b-202">瀏覽器會儲存可防止傳送的任何通訊透過 HTTP 的定義域的組態。</span><span class="sxs-lookup"><span data-stu-id="fb15b-202">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="fb15b-203">瀏覽器會強制透過 HTTPS 的所有通訊。</span><span class="sxs-lookup"><span data-stu-id="fb15b-203">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="fb15b-204">瀏覽器會防止使用者使用不受信任或不正確的憑證。</span><span class="sxs-lookup"><span data-stu-id="fb15b-204">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="fb15b-205">瀏覽器會停用允許使用者暫時信任此種憑證的提示。</span><span class="sxs-lookup"><span data-stu-id="fb15b-205">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="fb15b-206">因為 HSTS 會強制執行用戶端就會有一些限制：</span><span class="sxs-lookup"><span data-stu-id="fb15b-206">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="fb15b-207">用戶端必須支援 HSTS。</span><span class="sxs-lookup"><span data-stu-id="fb15b-207">The client must support HSTS.</span></span>
* <span data-ttu-id="fb15b-208">HSTS 需要至少一個成功的 HTTPS 要求建立 HSTS 原則。</span><span class="sxs-lookup"><span data-stu-id="fb15b-208">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="fb15b-209">應用程式必須檢查每個 HTTP 要求並重新導向或拒絕的 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="fb15b-209">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="fb15b-210">ASP.NET Core 2.1 或更新版本會實作 HSTS 與`UseHsts`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="fb15b-210">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="fb15b-211">下列程式碼會呼叫`UseHsts`應用程式不在[開發模式](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="fb15b-211">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="fb15b-212">`UseHsts` 不建議在開發過程中因為 HSTS 設定高度快取瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="fb15b-212">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="fb15b-213">根據預設，`UseHsts`排除本機回送位址。</span><span class="sxs-lookup"><span data-stu-id="fb15b-213">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="fb15b-214">生產環境中實作 HTTPS 第一次，設定初始[HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*)小的值，使用其中一種<xref:System.TimeSpan>方法。</span><span class="sxs-lookup"><span data-stu-id="fb15b-214">For production environments implementing HTTPS for the first time, set the initial [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) to a small value using one of the <xref:System.TimeSpan> methods.</span></span> <span data-ttu-id="fb15b-215">從設定值時數不超過一天的萬一您需要還原為 HTTP，HTTPS 基礎結構。</span><span class="sxs-lookup"><span data-stu-id="fb15b-215">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="fb15b-216">確定在 HTTPS 設定的持續性後，增加 HSTS 最大壽命值;常用的值為一年。</span><span class="sxs-lookup"><span data-stu-id="fb15b-216">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span>

<span data-ttu-id="fb15b-217">下列程式碼範例：</span><span class="sxs-lookup"><span data-stu-id="fb15b-217">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="fb15b-218">設定 Strict 傳輸安全性標頭的預先載入的參數。</span><span class="sxs-lookup"><span data-stu-id="fb15b-218">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="fb15b-219">預先載入不屬於[RFC HSTS 規格](https://tools.ietf.org/html/rfc6797)，但要預先載入 HSTS 上全新安裝的站台的網頁瀏覽器支援。</span><span class="sxs-lookup"><span data-stu-id="fb15b-219">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="fb15b-220">請參閱 [https://hstspreload.org/](https://hstspreload.org/) 以取得詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="fb15b-220">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="fb15b-221">可讓[includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2)，套用 HSTS 原則來裝載子網域。</span><span class="sxs-lookup"><span data-stu-id="fb15b-221">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="fb15b-222">明確設定為 60 天的 Strict 傳輸安全性標頭的最大壽命參數。</span><span class="sxs-lookup"><span data-stu-id="fb15b-222">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="fb15b-223">如果未設定，預設值為 30 天。</span><span class="sxs-lookup"><span data-stu-id="fb15b-223">If not set, defaults to 30 days.</span></span> <span data-ttu-id="fb15b-224">請參閱[最大壽命指示詞](https://tools.ietf.org/html/rfc6797#section-6.1.1)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="fb15b-224">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="fb15b-225">新增`example.com`的主機，以排除清單。</span><span class="sxs-lookup"><span data-stu-id="fb15b-225">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="fb15b-226">`UseHsts` 排除下列 「 回送 」 主控件：</span><span class="sxs-lookup"><span data-stu-id="fb15b-226">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="fb15b-227">`localhost` : IPv4 回送位址。</span><span class="sxs-lookup"><span data-stu-id="fb15b-227">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="fb15b-228">`127.0.0.1` : IPv4 回送位址。</span><span class="sxs-lookup"><span data-stu-id="fb15b-228">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="fb15b-229">`[::1]` : IPv6 回送位址。</span><span class="sxs-lookup"><span data-stu-id="fb15b-229">`[::1]` : The IPv6 loopback address.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="fb15b-230">選擇退出的 HTTPS/HSTS 專案建立</span><span class="sxs-lookup"><span data-stu-id="fb15b-230">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="fb15b-231">在某些後端服務案例中公開邊緣的網路處理連線安全性的位置，設定每個節點的連線安全性並非必要條件。</span><span class="sxs-lookup"><span data-stu-id="fb15b-231">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="fb15b-232">Web 應用程式從 Visual Studio 中，或從範本產生[dotnet 新](/dotnet/core/tools/dotnet-new)命令啟用[HTTPS 重新導向](#require-https)並[HSTS](#http-strict-transport-security-protocol-hsts)。</span><span class="sxs-lookup"><span data-stu-id="fb15b-232">Web apps generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require-https) and [HSTS](#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="fb15b-233">對於不需要這些案例的部署，您可以選擇退出的 HTTPS/HSTS 從範本建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="fb15b-233">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="fb15b-234">若要退出 HTTPS/HSTS:</span><span class="sxs-lookup"><span data-stu-id="fb15b-234">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fb15b-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fb15b-235">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="fb15b-236">取消核取**設定為使用 HTTPS**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="fb15b-236">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![顯示 HTTPS 核取方塊取消選取 [設定新的 ASP.NET Core Web 應用程式] 對話方塊。](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="fb15b-238">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="fb15b-238">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="fb15b-239">使用 `--no-https` 選項。</span><span class="sxs-lookup"><span data-stu-id="fb15b-239">Use the `--no-https` option.</span></span> <span data-ttu-id="fb15b-240">例如</span><span class="sxs-lookup"><span data-stu-id="fb15b-240">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a><span data-ttu-id="fb15b-241">信任 ASP.NET Core HTTPS 開發憑證，在 Windows 和 macOS 上</span><span class="sxs-lookup"><span data-stu-id="fb15b-241">Trust the ASP.NET Core HTTPS development certificate on Windows and macOS</span></span>

<span data-ttu-id="fb15b-242">.NET core SDK 包含 HTTPS 開發憑證。</span><span class="sxs-lookup"><span data-stu-id="fb15b-242">.NET Core SDK includes a HTTPS development certificate.</span></span> <span data-ttu-id="fb15b-243">憑證會安裝為初次執行體驗的一部分。</span><span class="sxs-lookup"><span data-stu-id="fb15b-243">The certificate is installed as part of the first-run experience.</span></span> <span data-ttu-id="fb15b-244">比方說，`dotnet --info`會產生類似下列輸出：</span><span class="sxs-lookup"><span data-stu-id="fb15b-244">For example, `dotnet --info` produces output similar to the following:</span></span>

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

<span data-ttu-id="fb15b-245">安裝.NET Core SDK 的本機使用者憑證存放區中安裝 ASP.NET Core HTTPS 開發憑證。</span><span class="sxs-lookup"><span data-stu-id="fb15b-245">Installing the .NET Core SDK installs the ASP.NET Core HTTPS development certificate to the local user certificate store.</span></span> <span data-ttu-id="fb15b-246">已安裝的憑證，但不是受信任。</span><span class="sxs-lookup"><span data-stu-id="fb15b-246">The certificate has been installed, but it's not trusted.</span></span> <span data-ttu-id="fb15b-247">若要信任的憑證執行一次性的步驟，以執行 dotnet`dev-certs`工具：</span><span class="sxs-lookup"><span data-stu-id="fb15b-247">To trust the certificate perform the one-time step to run the dotnet `dev-certs` tool:</span></span>

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="fb15b-248">下列命令會提供說明`dev-certs`工具：</span><span class="sxs-lookup"><span data-stu-id="fb15b-248">The following command provides help on the `dev-certs` tool:</span></span>

```console
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="fb15b-249">如何設定適用於 Docker 的開發人員憑證</span><span class="sxs-lookup"><span data-stu-id="fb15b-249">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="fb15b-250">請參閱[此 GitHub 問題](https://github.com/aspnet/Docs/issues/6199)。</span><span class="sxs-lookup"><span data-stu-id="fb15b-250">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="fb15b-251">其他資訊</span><span class="sxs-lookup"><span data-stu-id="fb15b-251">Additional information</span></span>

* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="fb15b-252">裝載 ASP.NET Core，Linux 上使用 Apache: SSL 設定</span><span class="sxs-lookup"><span data-stu-id="fb15b-252">Host ASP.NET Core on Linux with Apache: SSL configuration</span></span>](xref:host-and-deploy/linux-apache#ssl-configuration)
* [<span data-ttu-id="fb15b-253">裝載 ASP.NET Core，Linux 上使用 Nginx: SSL 設定</span><span class="sxs-lookup"><span data-stu-id="fb15b-253">Host ASP.NET Core on Linux with Nginx: SSL configuration</span></span>](xref:host-and-deploy/linux-nginx#configure-ssl)
* [<span data-ttu-id="fb15b-254">如何在 IIS 上設定 SSL</span><span class="sxs-lookup"><span data-stu-id="fb15b-254">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [<span data-ttu-id="fb15b-255">OWASP HSTS 瀏覽器支援</span><span class="sxs-lookup"><span data-stu-id="fb15b-255">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
