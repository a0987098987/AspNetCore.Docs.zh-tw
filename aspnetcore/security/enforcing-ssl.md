---
title: 強制使用 ASP.NET Core 中的 HTTPS
author: rick-anderson
description: 了解如何在 ASP.NET Core web 應用程式需要 HTTPS/TLS。
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 08ce50775d1b5348cb0528a1724cec2e5c72dae2
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "67152902"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="bff0c-103">強制使用 ASP.NET Core 中的 HTTPS</span><span class="sxs-lookup"><span data-stu-id="bff0c-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="bff0c-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bff0c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bff0c-105">本文件說明如何：</span><span class="sxs-lookup"><span data-stu-id="bff0c-105">This document shows how to:</span></span>

* <span data-ttu-id="bff0c-106">需要 HTTPS 進行的所有要求。</span><span class="sxs-lookup"><span data-stu-id="bff0c-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="bff0c-107">將所有 HTTP 要求重新都導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="bff0c-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="bff0c-108">沒有可用的 API 可以防止用戶端第一次要求中傳送機密資料。</span><span class="sxs-lookup"><span data-stu-id="bff0c-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> ## <a name="api-projects"></a><span data-ttu-id="bff0c-109">API 專案</span><span class="sxs-lookup"><span data-stu-id="bff0c-109">API projects</span></span>
>
> <span data-ttu-id="bff0c-110">請勿**未**使用[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute)接收機密資訊的 Web Api 上。</span><span class="sxs-lookup"><span data-stu-id="bff0c-110">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="bff0c-111">`RequireHttpsAttribute` 若要從 HTTP 至 HTTPS 的瀏覽器重新導向，會使用 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="bff0c-111">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="bff0c-112">API 用戶端可能不了解，或是遵循從 HTTP 重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="bff0c-112">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="bff0c-113">此類用戶端可能會透過 HTTP 傳送資訊。</span><span class="sxs-lookup"><span data-stu-id="bff0c-113">Such clients may send information over HTTP.</span></span> <span data-ttu-id="bff0c-114">Web Api 應執行下列之一：</span><span class="sxs-lookup"><span data-stu-id="bff0c-114">Web APIs should either:</span></span>
>
> * <span data-ttu-id="bff0c-115">不在 HTTP 上接聽。</span><span class="sxs-lookup"><span data-stu-id="bff0c-115">Not listen on HTTP.</span></span>
> * <span data-ttu-id="bff0c-116">關閉與狀態碼 400 （不正確的要求） 的連線，並不會提供要求。</span><span class="sxs-lookup"><span data-stu-id="bff0c-116">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> ## <a name="api-projects"></a><span data-ttu-id="bff0c-117">API 專案</span><span class="sxs-lookup"><span data-stu-id="bff0c-117">API projects</span></span>
>
> <span data-ttu-id="bff0c-118">請勿**未**使用[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute)接收機密資訊的 Web Api 上。</span><span class="sxs-lookup"><span data-stu-id="bff0c-118">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="bff0c-119">`RequireHttpsAttribute` 若要從 HTTP 至 HTTPS 的瀏覽器重新導向，會使用 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="bff0c-119">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="bff0c-120">API 用戶端可能不了解，或是遵循從 HTTP 重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="bff0c-120">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="bff0c-121">此類用戶端可能會透過 HTTP 傳送資訊。</span><span class="sxs-lookup"><span data-stu-id="bff0c-121">Such clients may send information over HTTP.</span></span> <span data-ttu-id="bff0c-122">Web Api 應執行下列之一：</span><span class="sxs-lookup"><span data-stu-id="bff0c-122">Web APIs should either:</span></span>
>
> * <span data-ttu-id="bff0c-123">不在 HTTP 上接聽。</span><span class="sxs-lookup"><span data-stu-id="bff0c-123">Not listen on HTTP.</span></span>
> * <span data-ttu-id="bff0c-124">關閉與狀態碼 400 （不正確的要求） 的連線，並不會提供要求。</span><span class="sxs-lookup"><span data-stu-id="bff0c-124">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>
>
> ## <a name="hsts-and-api-projects"></a><span data-ttu-id="bff0c-125">HSTS 和 API 專案</span><span class="sxs-lookup"><span data-stu-id="bff0c-125">HSTS and API projects</span></span>
>
> <span data-ttu-id="bff0c-126">預設 API 專案不包含[HSTS](#hsts)因為 HSTS 通常是瀏覽器的唯一指令。</span><span class="sxs-lookup"><span data-stu-id="bff0c-126">The default API projects don't include [HSTS](#hsts) because HSTS is generally a browser only instruction.</span></span> <span data-ttu-id="bff0c-127">其他呼叫端，例如電話或桌面應用程式，請勿**不**遵循指示。</span><span class="sxs-lookup"><span data-stu-id="bff0c-127">Other callers, such as phone or desktop apps, do **not** obey the instruction.</span></span> <span data-ttu-id="bff0c-128">甚至在瀏覽器中，呼叫一次驗證透過 HTTP API 會在不安全的網路上有風險。</span><span class="sxs-lookup"><span data-stu-id="bff0c-128">Even within browsers, a single authenticated call to an API over HTTP has risks on insecure networks.</span></span> <span data-ttu-id="bff0c-129">安全的方法是設定為只接聽及回應透過 HTTPS 的 API 專案。</span><span class="sxs-lookup"><span data-stu-id="bff0c-129">The secure approach is to configure API projects to only listen to and respond over HTTPS.</span></span>

::: moniker-end

## <a name="require-https"></a><span data-ttu-id="bff0c-130">需要 HTTPS</span><span class="sxs-lookup"><span data-stu-id="bff0c-130">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bff0c-131">我們建議您的生產環境 ASP.NET Core web 應用程式呼叫：</span><span class="sxs-lookup"><span data-stu-id="bff0c-131">We recommend that production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="bff0c-132">HTTPS 重新導向中介軟體 (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) 將 HTTP 要求重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="bff0c-132">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) to redirect HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="bff0c-133">HSTS 中介軟體 ([UseHsts](#http-strict-transport-security-protocol-hsts)) 傳送給用戶端的 HTTP Strict Transport Security 通訊協定 (HSTS) 標頭。</span><span class="sxs-lookup"><span data-stu-id="bff0c-133">HSTS Middleware ([UseHsts](#http-strict-transport-security-protocol-hsts)) to send HTTP Strict Transport Security Protocol (HSTS) headers to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="bff0c-134">在反向 proxy 組態中部署的應用程式允許 proxy 處理連線安全性 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="bff0c-134">Apps deployed in a reverse proxy configuration allow the proxy to handle connection security (HTTPS).</span></span> <span data-ttu-id="bff0c-135">如果 proxy 也會處理 HTTPS 重新導向，則不需要使用 HTTPS 重新導向中介軟體。</span><span class="sxs-lookup"><span data-stu-id="bff0c-135">If the proxy also handles HTTPS redirection, there's no need to use HTTPS Redirection Middleware.</span></span> <span data-ttu-id="bff0c-136">如果 proxy 伺服器也會負責編寫 HSTS 標頭 (例如[原生 HSTS 支援 IIS 10.0 (1709) 或更新版本](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support))，HSTS 中介軟體不需要應用程式。</span><span class="sxs-lookup"><span data-stu-id="bff0c-136">If the proxy server also handles writing HSTS headers (for example, [native HSTS support in IIS 10.0 (1709) or later](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS Middleware isn't required by the app.</span></span> <span data-ttu-id="bff0c-137">如需詳細資訊，請參閱 <<c0> [ 退出的 HTTPS/HSTS 專案建立](#opt-out-of-httpshsts-on-project-creation)。</span><span class="sxs-lookup"><span data-stu-id="bff0c-137">For more information, see [Opt-out of HTTPS/HSTS on project creation](#opt-out-of-httpshsts-on-project-creation).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="bff0c-138">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="bff0c-138">UseHttpsRedirection</span></span>

<span data-ttu-id="bff0c-139">下列程式碼會呼叫`UseHttpsRedirection`在`Startup`類別：</span><span class="sxs-lookup"><span data-stu-id="bff0c-139">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="bff0c-140">上述反白顯示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="bff0c-140">The preceding highlighted code:</span></span>

* <span data-ttu-id="bff0c-141">使用預設[HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect))。</span><span class="sxs-lookup"><span data-stu-id="bff0c-141">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="bff0c-142">使用預設[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) 但覆寫`ASPNETCORE_HTTPS_PORT`環境變數或[IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)。</span><span class="sxs-lookup"><span data-stu-id="bff0c-142">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="bff0c-143">我們建議使用暫時重新導向，而不是永久重新導向。</span><span class="sxs-lookup"><span data-stu-id="bff0c-143">We recommend using temporary redirects rather than permanent redirects.</span></span> <span data-ttu-id="bff0c-144">連結快取，會在開發環境中造成不穩定的行為。</span><span class="sxs-lookup"><span data-stu-id="bff0c-144">Link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="bff0c-145">如果您想要傳送的永久重新導向狀態碼，在非開發環境中應用程式時，請參閱[設定在生產環境中的永久重新導向](#configure-permanent-redirects-in-production)一節。</span><span class="sxs-lookup"><span data-stu-id="bff0c-145">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, see the [Configure permanent redirects in production](#configure-permanent-redirects-in-production) section.</span></span> <span data-ttu-id="bff0c-146">我們建議您使用[HSTS](#http-strict-transport-security-protocol-hsts)來只保護資源的用戶端通知要求應傳送至 （只在生產環境） 中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="bff0c-146">We recommend using [HSTS](#http-strict-transport-security-protocol-hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

### <a name="port-configuration"></a><span data-ttu-id="bff0c-147">連接埠組態</span><span class="sxs-lookup"><span data-stu-id="bff0c-147">Port configuration</span></span>

<span data-ttu-id="bff0c-148">將不安全的要求重新導向至 HTTPS 連接埠必須適用於中介軟體。</span><span class="sxs-lookup"><span data-stu-id="bff0c-148">A port must be available for the middleware to redirect an insecure request to HTTPS.</span></span> <span data-ttu-id="bff0c-149">如果沒有連接埠可用：</span><span class="sxs-lookup"><span data-stu-id="bff0c-149">If no port is available:</span></span>

* <span data-ttu-id="bff0c-150">不會發生重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="bff0c-150">Redirection to HTTPS doesn't occur.</span></span>
* <span data-ttu-id="bff0c-151">中介軟體會記錄警告 「 無法判定重新導向的 https 連接埠。 」</span><span class="sxs-lookup"><span data-stu-id="bff0c-151">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

<span data-ttu-id="bff0c-152">指定 HTTPS 連接埠，使用下列方法之一：</span><span class="sxs-lookup"><span data-stu-id="bff0c-152">Specify the HTTPS port using any of the following approaches:</span></span>

* <span data-ttu-id="bff0c-153">設定[HttpsRedirectionOptions.HttpsPort](#options)。</span><span class="sxs-lookup"><span data-stu-id="bff0c-153">Set [HttpsRedirectionOptions.HttpsPort](#options).</span></span>
* <span data-ttu-id="bff0c-154">設定`ASPNETCORE_HTTPS_PORT`環境變數或[https_port Web 主機組態設定](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="bff0c-154">Set the `ASPNETCORE_HTTPS_PORT` environment variable or [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

  <span data-ttu-id="bff0c-155">**索引鍵**: `https_port`</span><span class="sxs-lookup"><span data-stu-id="bff0c-155">**Key**: `https_port`</span></span>  
  <span data-ttu-id="bff0c-156">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="bff0c-156">**Type**: *string*</span></span>  
  <span data-ttu-id="bff0c-157">**預設**：未設定預設值。</span><span class="sxs-lookup"><span data-stu-id="bff0c-157">**Default**: A default value isn't set.</span></span>  
  <span data-ttu-id="bff0c-158">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="bff0c-158">**Set using**: `UseSetting`</span></span>  
  <span data-ttu-id="bff0c-159">**環境變數**:`<PREFIX_>HTTPS_PORT` (前置詞`ASPNETCORE_`使用時[Web 主機](xref:fundamentals/host/web-host)。)</span><span class="sxs-lookup"><span data-stu-id="bff0c-159">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the [Web Host](xref:fundamentals/host/web-host).)</span></span>

  <span data-ttu-id="bff0c-160">設定時<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>在`Program`:</span><span class="sxs-lookup"><span data-stu-id="bff0c-160">When configuring an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> in `Program`:</span></span>

  [!code-csharp[](enforcing-ssl/sample-snapshot/Program.cs?name=snippet_Program&highlight=10)]
* <span data-ttu-id="bff0c-161">表示具有安全的配置使用的連接埠`ASPNETCORE_URLS`環境變數。</span><span class="sxs-lookup"><span data-stu-id="bff0c-161">Indicate a port with the secure scheme using the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="bff0c-162">環境變數設定伺服器。</span><span class="sxs-lookup"><span data-stu-id="bff0c-162">The environment variable configures the server.</span></span> <span data-ttu-id="bff0c-163">中介軟體間接探索透過 HTTPS 連接埠<xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>。</span><span class="sxs-lookup"><span data-stu-id="bff0c-163">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="bff0c-164">這個方法不適用於反向 proxy 的部署。</span><span class="sxs-lookup"><span data-stu-id="bff0c-164">This approach doesn't work in reverse proxy deployments.</span></span>
* <span data-ttu-id="bff0c-165">在開發中，請在中設定的 HTTPS URL *launchsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="bff0c-165">In development, set an HTTPS URL in *launchsettings.json*.</span></span> <span data-ttu-id="bff0c-166">使用 IIS Express 時，請啟用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="bff0c-166">Enable HTTPS when IIS Express is used.</span></span>
* <span data-ttu-id="bff0c-167">設定向外公開 edge 部署的 HTTPS URL 端點[Kestrel](xref:fundamentals/servers/kestrel)伺服器或[HTTP.sys](xref:fundamentals/servers/httpsys)伺服器。</span><span class="sxs-lookup"><span data-stu-id="bff0c-167">Configure an HTTPS URL endpoint for a public-facing edge deployment of [Kestrel](xref:fundamentals/servers/kestrel) server or [HTTP.sys](xref:fundamentals/servers/httpsys) server.</span></span> <span data-ttu-id="bff0c-168">只有**一個 HTTPS 連接埠**應用程式使用。</span><span class="sxs-lookup"><span data-stu-id="bff0c-168">Only **one HTTPS port** is used by the app.</span></span> <span data-ttu-id="bff0c-169">中介軟體會探索透過連接埠<xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>。</span><span class="sxs-lookup"><span data-stu-id="bff0c-169">The middleware discovers the port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span>

> [!NOTE]
> <span data-ttu-id="bff0c-170">在反向 proxy 組態中，執行應用程式時<xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>無法使用。</span><span class="sxs-lookup"><span data-stu-id="bff0c-170">When an app is run in a reverse proxy configuration, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> isn't available.</span></span> <span data-ttu-id="bff0c-171">使用本節中所述的其他方法的其中一個連接埠設定。</span><span class="sxs-lookup"><span data-stu-id="bff0c-171">Set the port using one of the other approaches described in this section.</span></span>

<span data-ttu-id="bff0c-172">使用 Kestrel 或 HTTP.sys 時做為向外公開邊緣伺服器，Kestrel 或 HTTP.sys 必須設定為接聽兩者：</span><span class="sxs-lookup"><span data-stu-id="bff0c-172">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>

* <span data-ttu-id="bff0c-173">會在重新導向用戶端的安全連接埠 (通常，在生產環境和開發 5001 443)。</span><span class="sxs-lookup"><span data-stu-id="bff0c-173">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
* <span data-ttu-id="bff0c-174">不安全的連接埠 (通常，在生產環境中為 80) 與開發中的 5000。</span><span class="sxs-lookup"><span data-stu-id="bff0c-174">The insecure port (typically, 80 in production and 5000 in development).</span></span>

<span data-ttu-id="bff0c-175">為了讓應用程式用戶端收到不安全的要求，並重新導向至安全的連接埠的用戶端必須能夠使用不安全的連接埠。</span><span class="sxs-lookup"><span data-stu-id="bff0c-175">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect the client to the secure port.</span></span>

<span data-ttu-id="bff0c-176">如需詳細資訊，請參閱 < [Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration)或<xref:fundamentals/servers/httpsys>。</span><span class="sxs-lookup"><span data-stu-id="bff0c-176">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

### <a name="deployment-scenarios"></a><span data-ttu-id="bff0c-177">部署案例</span><span class="sxs-lookup"><span data-stu-id="bff0c-177">Deployment scenarios</span></span>

<span data-ttu-id="bff0c-178">用戶端與伺服器之間的任何防火牆也必須開啟流量的通訊連接埠。</span><span class="sxs-lookup"><span data-stu-id="bff0c-178">Any firewall between the client and server must also have communication ports open for traffic.</span></span>

<span data-ttu-id="bff0c-179">如果要求轉送的反向 proxy 設定，使用[轉送標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)之前呼叫 HTTPS 重新導向中介軟體。</span><span class="sxs-lookup"><span data-stu-id="bff0c-179">If requests are forwarded in a reverse proxy configuration, use [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) before calling HTTPS Redirection Middleware.</span></span> <span data-ttu-id="bff0c-180">轉送標頭中介軟體更新`Request.Scheme`，並使用`X-Forwarded-Proto`標頭。</span><span class="sxs-lookup"><span data-stu-id="bff0c-180">Forwarded Headers Middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header.</span></span> <span data-ttu-id="bff0c-181">中介軟體允許重新導向 Uri 和其他安全性原則才能正常運作。</span><span class="sxs-lookup"><span data-stu-id="bff0c-181">The middleware permits redirect URIs and other security policies to work correctly.</span></span> <span data-ttu-id="bff0c-182">轉送標頭中介軟體不使用時後, 端應用程式可能不會收到正確的配置和得到的重新導向迴圈。</span><span class="sxs-lookup"><span data-stu-id="bff0c-182">When Forwarded Headers Middleware isn't used, the backend app might not receive the correct scheme and end up in a redirect loop.</span></span> <span data-ttu-id="bff0c-183">常見的使用者錯誤訊息是發生太多的重新導向。</span><span class="sxs-lookup"><span data-stu-id="bff0c-183">A common end user error message is that too many redirects have occurred.</span></span>

<span data-ttu-id="bff0c-184">部署至 Azure App Service 時，請依照下列中的指導方針[教學課程：將現有的自訂 SSL 憑證繫結至 Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl)。</span><span class="sxs-lookup"><span data-stu-id="bff0c-184">When deploying to Azure App Service, follow the guidance in [Tutorial: Bind an existing custom SSL certificate to Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

### <a name="options"></a><span data-ttu-id="bff0c-185">選項</span><span class="sxs-lookup"><span data-stu-id="bff0c-185">Options</span></span>

<span data-ttu-id="bff0c-186">下列醒目提示程式碼會呼叫[AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection)設定中介軟體選項：</span><span class="sxs-lookup"><span data-stu-id="bff0c-186">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="bff0c-187">呼叫`AddHttpsRedirection`時，才需要變更的值`HttpsPort`或`RedirectStatusCode`。</span><span class="sxs-lookup"><span data-stu-id="bff0c-187">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="bff0c-188">上述反白顯示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="bff0c-188">The preceding highlighted code:</span></span>

* <span data-ttu-id="bff0c-189">設定組[HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*)到<xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>，這是預設值。</span><span class="sxs-lookup"><span data-stu-id="bff0c-189">Sets [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) to <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, which is the default value.</span></span> <span data-ttu-id="bff0c-190">使用的欄位<xref:Microsoft.AspNetCore.Http.StatusCodes>類別的工作分派`RedirectStatusCode`。</span><span class="sxs-lookup"><span data-stu-id="bff0c-190">Use the fields of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="bff0c-191">設定 HTTPS 連接埠為 5001。</span><span class="sxs-lookup"><span data-stu-id="bff0c-191">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="bff0c-192">預設值為 443。</span><span class="sxs-lookup"><span data-stu-id="bff0c-192">The default value is 443.</span></span>

#### <a name="configure-permanent-redirects-in-production"></a><span data-ttu-id="bff0c-193">在生產環境中設定永久重新導向</span><span class="sxs-lookup"><span data-stu-id="bff0c-193">Configure permanent redirects in production</span></span>

<span data-ttu-id="bff0c-194">中介軟體會預設為傳送[Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)與所有重新導向。</span><span class="sxs-lookup"><span data-stu-id="bff0c-194">The middleware defaults to sending a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) with all redirects.</span></span> <span data-ttu-id="bff0c-195">如果您想要傳送的永久重新導向狀態碼，在非開發環境中應用程式時，包裝中介軟體選項的組態中的非開發環境的條件式檢查。</span><span class="sxs-lookup"><span data-stu-id="bff0c-195">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, wrap the middleware options configuration in a conditional check for a non-Development environment.</span></span>

<span data-ttu-id="bff0c-196">設定時`IWebHostBuilder`中*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="bff0c-196">When configuring an `IWebHostBuilder` in *Startup.cs*:</span></span>

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

## <a name="https-redirection-middleware-alternative-approach"></a><span data-ttu-id="bff0c-197">HTTPS 重新導向中介軟體的替代方法</span><span class="sxs-lookup"><span data-stu-id="bff0c-197">HTTPS Redirection Middleware alternative approach</span></span>

<span data-ttu-id="bff0c-198">除了使用 HTTPS 重新導向中介軟體 (`UseHttpsRedirection`) 是使用 URL 重寫中介軟體 (`AddRedirectToHttps`)。</span><span class="sxs-lookup"><span data-stu-id="bff0c-198">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="bff0c-199">`AddRedirectToHttps` 也可以設定的狀態碼和連接埠重新導向為執行時。</span><span class="sxs-lookup"><span data-stu-id="bff0c-199">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="bff0c-200">如需詳細資訊，請參閱 < [URL 重寫中介軟體](xref:fundamentals/url-rewriting)。</span><span class="sxs-lookup"><span data-stu-id="bff0c-200">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="bff0c-201">當重新導向至 HTTPS，而不需要額外的重新導向規則，我們建議使用 HTTPS 重新導向中介軟體 (`UseHttpsRedirection`) 本主題中所述。</span><span class="sxs-lookup"><span data-stu-id="bff0c-201">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="bff0c-202">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute)用來要求 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="bff0c-202">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="bff0c-203">`[RequireHttpsAttribute]` 可以裝飾控制器或方法，或可以全域套用。</span><span class="sxs-lookup"><span data-stu-id="bff0c-203">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="bff0c-204">若要全域套用的屬性，新增下列程式碼`ConfigureServices`在`Startup`:</span><span class="sxs-lookup"><span data-stu-id="bff0c-204">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="bff0c-205">上述反白顯示的程式碼會要求所有要求都使用`HTTPS`; 因此，HTTP 要求會被忽略。</span><span class="sxs-lookup"><span data-stu-id="bff0c-205">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="bff0c-206">而下列反白顯示的程式碼會將所有 HTTP 要求都重新導向至 HTTPS:</span><span class="sxs-lookup"><span data-stu-id="bff0c-206">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="bff0c-207">如需詳細資訊，請參閱 < [URL 重寫中介軟體](xref:fundamentals/url-rewriting)。</span><span class="sxs-lookup"><span data-stu-id="bff0c-207">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="bff0c-208">中介軟體也允許應用程式執行時重新導向設定的狀態碼或狀態碼和連接埠。</span><span class="sxs-lookup"><span data-stu-id="bff0c-208">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="bff0c-209">全域使用 HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) 是安全性最佳作法。</span><span class="sxs-lookup"><span data-stu-id="bff0c-209">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="bff0c-210">將`[RequireHttps]`屬性套用至所有控制器，不會比全域使用 HTTPS 來的安全。</span><span class="sxs-lookup"><span data-stu-id="bff0c-210">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="bff0c-211">您無法保證`[RequireHttps]`新增新的控制器和 Razor 頁面時，屬性會套用。</span><span class="sxs-lookup"><span data-stu-id="bff0c-211">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="bff0c-212">HTTP Strict Transport 安全性通訊協定 (HSTS)</span><span class="sxs-lookup"><span data-stu-id="bff0c-212">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="bff0c-213">每個[OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project)， [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet)是透過回應標頭使用的 web 應用程式所指定的選擇加入的安全性增強功能。</span><span class="sxs-lookup"><span data-stu-id="bff0c-213">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="bff0c-214">當[瀏覽器支援 HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)收到此標頭：</span><span class="sxs-lookup"><span data-stu-id="bff0c-214">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="bff0c-215">瀏覽器會儲存可防止傳送的任何通訊透過 HTTP 的定義域的組態。</span><span class="sxs-lookup"><span data-stu-id="bff0c-215">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="bff0c-216">瀏覽器會強制透過 HTTPS 的所有通訊。</span><span class="sxs-lookup"><span data-stu-id="bff0c-216">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="bff0c-217">瀏覽器會防止使用者使用不受信任或不正確的憑證。</span><span class="sxs-lookup"><span data-stu-id="bff0c-217">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="bff0c-218">瀏覽器會停用允許使用者暫時信任此種憑證的提示。</span><span class="sxs-lookup"><span data-stu-id="bff0c-218">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="bff0c-219">因為 HSTS 會強制執行用戶端就會有一些限制：</span><span class="sxs-lookup"><span data-stu-id="bff0c-219">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="bff0c-220">用戶端必須支援 HSTS。</span><span class="sxs-lookup"><span data-stu-id="bff0c-220">The client must support HSTS.</span></span>
* <span data-ttu-id="bff0c-221">HSTS 需要至少一個成功的 HTTPS 要求建立 HSTS 原則。</span><span class="sxs-lookup"><span data-stu-id="bff0c-221">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="bff0c-222">應用程式必須檢查每個 HTTP 要求並重新導向或拒絕的 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="bff0c-222">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="bff0c-223">ASP.NET Core 2.1 或更新版本會實作 HSTS 與`UseHsts`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="bff0c-223">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="bff0c-224">下列程式碼會呼叫`UseHsts`應用程式不在[開發模式](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="bff0c-224">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="bff0c-225">`UseHsts` 不建議在開發過程中因為 HSTS 設定高度快取瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="bff0c-225">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="bff0c-226">根據預設，`UseHsts`排除本機回送位址。</span><span class="sxs-lookup"><span data-stu-id="bff0c-226">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="bff0c-227">生產環境中實作 HTTPS 第一次，設定初始[HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*)小的值，使用其中一種<xref:System.TimeSpan>方法。</span><span class="sxs-lookup"><span data-stu-id="bff0c-227">For production environments implementing HTTPS for the first time, set the initial [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) to a small value using one of the <xref:System.TimeSpan> methods.</span></span> <span data-ttu-id="bff0c-228">從設定值時數不超過一天的萬一您需要還原為 HTTP，HTTPS 基礎結構。</span><span class="sxs-lookup"><span data-stu-id="bff0c-228">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="bff0c-229">確定在 HTTPS 設定的持續性後，增加 HSTS 最大壽命值;常用的值為一年。</span><span class="sxs-lookup"><span data-stu-id="bff0c-229">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span>

<span data-ttu-id="bff0c-230">下列程式碼範例：</span><span class="sxs-lookup"><span data-stu-id="bff0c-230">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="bff0c-231">設定 Strict 傳輸安全性標頭的預先載入的參數。</span><span class="sxs-lookup"><span data-stu-id="bff0c-231">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="bff0c-232">預先載入不屬於[RFC HSTS 規格](https://tools.ietf.org/html/rfc6797)，但要預先載入 HSTS 上全新安裝的站台的網頁瀏覽器支援。</span><span class="sxs-lookup"><span data-stu-id="bff0c-232">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="bff0c-233">請參閱 [https://hstspreload.org/](https://hstspreload.org/) 以取得詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="bff0c-233">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="bff0c-234">可讓[includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2)，套用 HSTS 原則來裝載子網域。</span><span class="sxs-lookup"><span data-stu-id="bff0c-234">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="bff0c-235">明確設定為 60 天的 Strict 傳輸安全性標頭的最大壽命參數。</span><span class="sxs-lookup"><span data-stu-id="bff0c-235">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="bff0c-236">如果未設定，預設值為 30 天。</span><span class="sxs-lookup"><span data-stu-id="bff0c-236">If not set, defaults to 30 days.</span></span> <span data-ttu-id="bff0c-237">請參閱[最大壽命指示詞](https://tools.ietf.org/html/rfc6797#section-6.1.1)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="bff0c-237">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="bff0c-238">新增`example.com`的主機，以排除清單。</span><span class="sxs-lookup"><span data-stu-id="bff0c-238">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="bff0c-239">`UseHsts` 排除下列 「 回送 」 主控件：</span><span class="sxs-lookup"><span data-stu-id="bff0c-239">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="bff0c-240">`localhost`：IPv4 回送位址。</span><span class="sxs-lookup"><span data-stu-id="bff0c-240">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="bff0c-241">`127.0.0.1`：IPv4 回送位址。</span><span class="sxs-lookup"><span data-stu-id="bff0c-241">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="bff0c-242">`[::1]`：IPv6 回送位址。</span><span class="sxs-lookup"><span data-stu-id="bff0c-242">`[::1]` : The IPv6 loopback address.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="bff0c-243">選擇退出的 HTTPS/HSTS 專案建立</span><span class="sxs-lookup"><span data-stu-id="bff0c-243">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="bff0c-244">在某些後端服務案例中公開邊緣的網路處理連線安全性的位置，設定每個節點的連線安全性並非必要條件。</span><span class="sxs-lookup"><span data-stu-id="bff0c-244">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="bff0c-245">Web 應用程式從 Visual Studio 中，或從範本產生[dotnet 新](/dotnet/core/tools/dotnet-new)命令啟用[HTTPS 重新導向](#require-https)並[HSTS](#http-strict-transport-security-protocol-hsts)。</span><span class="sxs-lookup"><span data-stu-id="bff0c-245">Web apps generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require-https) and [HSTS](#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="bff0c-246">對於不需要這些案例的部署，您可以選擇退出的 HTTPS/HSTS 從範本建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="bff0c-246">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="bff0c-247">若要退出 HTTPS/HSTS:</span><span class="sxs-lookup"><span data-stu-id="bff0c-247">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bff0c-248">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bff0c-248">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="bff0c-249">取消核取**設定為使用 HTTPS**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="bff0c-249">Uncheck the **Configure for HTTPS** check box.</span></span>

![顯示 HTTPS 核取方塊取消選取 [設定新的 ASP.NET Core Web 應用程式] 對話方塊。](enforcing-ssl/_static/out.png)

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bff0c-251">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="bff0c-251">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="bff0c-252">使用 `--no-https` 選項。</span><span class="sxs-lookup"><span data-stu-id="bff0c-252">Use the `--no-https` option.</span></span> <span data-ttu-id="bff0c-253">例如</span><span class="sxs-lookup"><span data-stu-id="bff0c-253">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="trust"></a>

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a><span data-ttu-id="bff0c-254">信任 ASP.NET Core HTTPS 開發憑證，在 Windows 和 macOS 上</span><span class="sxs-lookup"><span data-stu-id="bff0c-254">Trust the ASP.NET Core HTTPS development certificate on Windows and macOS</span></span>

<span data-ttu-id="bff0c-255">.NET core SDK 包含 HTTPS 開發憑證。</span><span class="sxs-lookup"><span data-stu-id="bff0c-255">.NET Core SDK includes a HTTPS development certificate.</span></span> <span data-ttu-id="bff0c-256">憑證會安裝為初次執行體驗的一部分。</span><span class="sxs-lookup"><span data-stu-id="bff0c-256">The certificate is installed as part of the first-run experience.</span></span> <span data-ttu-id="bff0c-257">比方說，`dotnet --info`會產生類似下列輸出：</span><span class="sxs-lookup"><span data-stu-id="bff0c-257">For example, `dotnet --info` produces output similar to the following:</span></span>

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

<span data-ttu-id="bff0c-258">安裝 .NET Core SDK 會將 ASP.NET Core HTTPS 開發憑證安裝至本機使用者憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="bff0c-258">Installing the .NET Core SDK installs the ASP.NET Core HTTPS development certificate to the local user certificate store.</span></span> <span data-ttu-id="bff0c-259">已安裝的憑證，但不是受信任。</span><span class="sxs-lookup"><span data-stu-id="bff0c-259">The certificate has been installed, but it's not trusted.</span></span> <span data-ttu-id="bff0c-260">若要信任的憑證執行一次性的步驟，以執行 dotnet`dev-certs`工具：</span><span class="sxs-lookup"><span data-stu-id="bff0c-260">To trust the certificate perform the one-time step to run the dotnet `dev-certs` tool:</span></span>

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="bff0c-261">下列命令會提供 `dev-certs` 工具的說明：</span><span class="sxs-lookup"><span data-stu-id="bff0c-261">The following command provides help on the `dev-certs` tool:</span></span>

```console
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="bff0c-262">如何設定適用於 Docker 的開發人員憑證</span><span class="sxs-lookup"><span data-stu-id="bff0c-262">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="bff0c-263">請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/6199)。</span><span class="sxs-lookup"><span data-stu-id="bff0c-263">See [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6199).</span></span>

::: moniker-end

<a name="wsl"></a>

## <a name="trust-https-certificate-from-windows-subsystem-for-linux"></a><span data-ttu-id="bff0c-264">信任適用於 Linux 的 Windows 子系統的 HTTPS 憑證</span><span class="sxs-lookup"><span data-stu-id="bff0c-264">Trust HTTPS certificate from Windows Subsystem for Linux</span></span>

<span data-ttu-id="bff0c-265">Windows for Linux 子系統 (WSL) 會產生 HTTPS 的自我簽署的憑證。若要設定信任 WSL 憑證的 Windows 憑證存放區：</span><span class="sxs-lookup"><span data-stu-id="bff0c-265">The Windows Subsystem for Linux (WSL) generates a HTTPS self-signed cert. To configure the Windows certificate store to trust the WSL certificate:</span></span>

* <span data-ttu-id="bff0c-266">執行下列命令以匯出 WSL 產生憑證： `dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`</span><span class="sxs-lookup"><span data-stu-id="bff0c-266">Run the following command to export the WSL generated certificate: `dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`</span></span>
* <span data-ttu-id="bff0c-267">在 WSL 視窗中，執行下列命令： `ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`</span><span class="sxs-lookup"><span data-stu-id="bff0c-267">In a WSL window, run the following command: `ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`</span></span>

  <span data-ttu-id="bff0c-268">上述命令會設定環境變數，因此 Linux 會使用 Windows 信任的憑證。</span><span class="sxs-lookup"><span data-stu-id="bff0c-268">The preceding command sets the environment variables so Linux uses the Windows trusted certificate.</span></span>

## <a name="additional-information"></a><span data-ttu-id="bff0c-269">其他資訊</span><span class="sxs-lookup"><span data-stu-id="bff0c-269">Additional information</span></span>

* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="bff0c-270">Linux 上使用 Apache 裝載 ASP.NET Core:HTTPS 設定</span><span class="sxs-lookup"><span data-stu-id="bff0c-270">Host ASP.NET Core on Linux with Apache: HTTPS configuration</span></span>](xref:host-and-deploy/linux-apache#https-configuration)
* [<span data-ttu-id="bff0c-271">Linux 上使用 Nginx 裝載 ASP.NET Core:HTTPS 設定</span><span class="sxs-lookup"><span data-stu-id="bff0c-271">Host ASP.NET Core on Linux with Nginx: HTTPS configuration</span></span>](xref:host-and-deploy/linux-nginx#https-configuration)
* [<span data-ttu-id="bff0c-272">如何在 IIS 上設定 SSL</span><span class="sxs-lookup"><span data-stu-id="bff0c-272">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [<span data-ttu-id="bff0c-273">OWASP HSTS 瀏覽器支援</span><span class="sxs-lookup"><span data-stu-id="bff0c-273">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
