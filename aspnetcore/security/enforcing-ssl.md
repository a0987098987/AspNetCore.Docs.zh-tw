---
title: 在 ASP.NET Core 中強制使用 HTTPS
author: rick-anderson
description: 瞭解如何在 ASP.NET Core web 應用程式中要求使用 HTTPS/TLS。
ms.author: riande
ms.custom: mvc
ms.date: 09/14/2019
uid: security/enforcing-ssl
ms.openlocfilehash: 82cd2e52f3bd929682b9eae24611ad04fd9f8682
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317362"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="34835-103">在 ASP.NET Core 中強制使用 HTTPS</span><span class="sxs-lookup"><span data-stu-id="34835-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="34835-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="34835-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="34835-105">本檔說明如何：</span><span class="sxs-lookup"><span data-stu-id="34835-105">This document shows how to:</span></span>

* <span data-ttu-id="34835-106">所有要求都需要 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="34835-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="34835-107">將所有 HTTP 要求都重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="34835-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="34835-108">沒有 API 可防止用戶端在第一次要求時傳送敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="34835-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> ## <a name="api-projects"></a><span data-ttu-id="34835-109">API 專案</span><span class="sxs-lookup"><span data-stu-id="34835-109">API projects</span></span>
>
> <span data-ttu-id="34835-110">請勿**在**接收機密資訊的 Web api 上使用[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) 。</span><span class="sxs-lookup"><span data-stu-id="34835-110">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="34835-111">`RequireHttpsAttribute` 會使用 HTTP 狀態碼，將瀏覽器從 HTTP 重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="34835-111">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="34835-112">API 用戶端可能無法瞭解或遵循從 HTTP 到 HTTPS 的重新導向。</span><span class="sxs-lookup"><span data-stu-id="34835-112">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="34835-113">這類用戶端可能會透過 HTTP 傳送資訊。</span><span class="sxs-lookup"><span data-stu-id="34835-113">Such clients may send information over HTTP.</span></span> <span data-ttu-id="34835-114">Web Api 應為：</span><span class="sxs-lookup"><span data-stu-id="34835-114">Web APIs should either:</span></span>
>
> * <span data-ttu-id="34835-115">不接聽 HTTP。</span><span class="sxs-lookup"><span data-stu-id="34835-115">Not listen on HTTP.</span></span>
> * <span data-ttu-id="34835-116">關閉具有狀態碼400（不正確的要求）的連接，而不提供要求。</span><span class="sxs-lookup"><span data-stu-id="34835-116">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>
>
> ## <a name="hsts-and-api-projects"></a><span data-ttu-id="34835-117">HSTS 和 API 專案</span><span class="sxs-lookup"><span data-stu-id="34835-117">HSTS and API projects</span></span>
>
> <span data-ttu-id="34835-118">預設 API 專案不包含[HSTS](#hsts) ，因為 HSTS 通常是瀏覽器唯一的指令。</span><span class="sxs-lookup"><span data-stu-id="34835-118">The default API projects don't include [HSTS](#hsts) because HSTS is generally a browser only instruction.</span></span> <span data-ttu-id="34835-119">其他呼叫端（例如電話或桌面應用程式）**不會**遵守指令。</span><span class="sxs-lookup"><span data-stu-id="34835-119">Other callers, such as phone or desktop apps, do **not** obey the instruction.</span></span> <span data-ttu-id="34835-120">即使在瀏覽器中，透過 HTTP 對 API 進行的單一驗證呼叫也會有不安全網路的風險。</span><span class="sxs-lookup"><span data-stu-id="34835-120">Even within browsers, a single authenticated call to an API over HTTP has risks on insecure networks.</span></span> <span data-ttu-id="34835-121">安全的方法是將 API 專案設定為僅接聽並透過 HTTPS 回應。</span><span class="sxs-lookup"><span data-stu-id="34835-121">The secure approach is to configure API projects to only listen to and respond over HTTPS.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

> [!WARNING]
> ## <a name="api-projects"></a><span data-ttu-id="34835-122">API 專案</span><span class="sxs-lookup"><span data-stu-id="34835-122">API projects</span></span>
>
> <span data-ttu-id="34835-123">請勿**在**接收機密資訊的 Web api 上使用[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) 。</span><span class="sxs-lookup"><span data-stu-id="34835-123">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="34835-124">`RequireHttpsAttribute` 會使用 HTTP 狀態碼，將瀏覽器從 HTTP 重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="34835-124">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="34835-125">API 用戶端可能無法瞭解或遵循從 HTTP 到 HTTPS 的重新導向。</span><span class="sxs-lookup"><span data-stu-id="34835-125">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="34835-126">這類用戶端可能會透過 HTTP 傳送資訊。</span><span class="sxs-lookup"><span data-stu-id="34835-126">Such clients may send information over HTTP.</span></span> <span data-ttu-id="34835-127">Web Api 應為：</span><span class="sxs-lookup"><span data-stu-id="34835-127">Web APIs should either:</span></span>
>
> * <span data-ttu-id="34835-128">不接聽 HTTP。</span><span class="sxs-lookup"><span data-stu-id="34835-128">Not listen on HTTP.</span></span>
> * <span data-ttu-id="34835-129">關閉具有狀態碼400（不正確的要求）的連接，而不提供要求。</span><span class="sxs-lookup"><span data-stu-id="34835-129">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

::: moniker-end

## <a name="require-https"></a><span data-ttu-id="34835-130">需要 HTTPS</span><span class="sxs-lookup"><span data-stu-id="34835-130">Require HTTPS</span></span>

<span data-ttu-id="34835-131">我們建議生產 ASP.NET Core web 應用程式使用：</span><span class="sxs-lookup"><span data-stu-id="34835-131">We recommend that production ASP.NET Core web apps use:</span></span>

* <span data-ttu-id="34835-132">用來將 HTTP 要求重新導向至 HTTPS 的 HTTPS 重新導向中介軟體（<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>）。</span><span class="sxs-lookup"><span data-stu-id="34835-132">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) to redirect HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="34835-133">HSTS 中介軟體（[UseHsts](#http-strict-transport-security-protocol-hsts)），以將 HTTP 嚴格傳輸安全性通訊協定（HSTS）標頭傳送給用戶端。</span><span class="sxs-lookup"><span data-stu-id="34835-133">HSTS Middleware ([UseHsts](#http-strict-transport-security-protocol-hsts)) to send HTTP Strict Transport Security Protocol (HSTS) headers to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="34835-134">部署在反向 proxy 設定中的應用程式可讓 proxy 處理連線安全性（HTTPS）。</span><span class="sxs-lookup"><span data-stu-id="34835-134">Apps deployed in a reverse proxy configuration allow the proxy to handle connection security (HTTPS).</span></span> <span data-ttu-id="34835-135">如果 proxy 也處理 HTTPS 重新導向，則不需要使用 HTTPS 重新導向中介軟體。</span><span class="sxs-lookup"><span data-stu-id="34835-135">If the proxy also handles HTTPS redirection, there's no need to use HTTPS Redirection Middleware.</span></span> <span data-ttu-id="34835-136">如果 proxy 伺服器也處理寫入 HSTS 標頭（例如， [IIS 10.0 （1709）或更新版本中的原生 HSTS 支援](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)），應用程式就不需要 HSTS 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="34835-136">If the proxy server also handles writing HSTS headers (for example, [native HSTS support in IIS 10.0 (1709) or later](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS Middleware isn't required by the app.</span></span> <span data-ttu-id="34835-137">如需詳細資訊，請參閱[在專案建立時退出宣告 HTTPS/HSTS](#opt-out-of-httpshsts-on-project-creation)。</span><span class="sxs-lookup"><span data-stu-id="34835-137">For more information, see [Opt-out of HTTPS/HSTS on project creation](#opt-out-of-httpshsts-on-project-creation).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="34835-138">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="34835-138">UseHttpsRedirection</span></span>

<span data-ttu-id="34835-139">下列程式碼會呼叫 `Startup` 類別中的 `UseHttpsRedirection`：</span><span class="sxs-lookup"><span data-stu-id="34835-139">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet1&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet1&highlight=13)]

::: moniker-end

<span data-ttu-id="34835-140">上述反白顯示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="34835-140">The preceding highlighted code:</span></span>

* <span data-ttu-id="34835-141">會使用預設的[HttpsRedirectionOptions RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) （[Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)）。</span><span class="sxs-lookup"><span data-stu-id="34835-141">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="34835-142">除非由 `ASPNETCORE_HTTPS_PORT` 環境變數或[IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)覆寫，否則會使用預設的[HttpsRedirectionOptions HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) （null）。</span><span class="sxs-lookup"><span data-stu-id="34835-142">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="34835-143">我們建議使用暫時重新導向，而不是永久重新導向。</span><span class="sxs-lookup"><span data-stu-id="34835-143">We recommend using temporary redirects rather than permanent redirects.</span></span> <span data-ttu-id="34835-144">連結快取在開發環境中可能會造成不穩定的行為。</span><span class="sxs-lookup"><span data-stu-id="34835-144">Link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="34835-145">如果您想要在應用程式處於非開發環境時傳送永久重新導向狀態碼，請參閱在[生產中設定永久重新導向](#configure-permanent-redirects-in-production)一節。</span><span class="sxs-lookup"><span data-stu-id="34835-145">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, see the [Configure permanent redirects in production](#configure-permanent-redirects-in-production) section.</span></span> <span data-ttu-id="34835-146">我們建議使用[HSTS](#http-strict-transport-security-protocol-hsts)來通知用戶端，只應將安全的資源要求傳送至應用程式（僅限生產環境）。</span><span class="sxs-lookup"><span data-stu-id="34835-146">We recommend using [HSTS](#http-strict-transport-security-protocol-hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

### <a name="port-configuration"></a><span data-ttu-id="34835-147">埠設定</span><span class="sxs-lookup"><span data-stu-id="34835-147">Port configuration</span></span>

<span data-ttu-id="34835-148">中介軟體必須要有埠，才可將不安全的要求重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="34835-148">A port must be available for the middleware to redirect an insecure request to HTTPS.</span></span> <span data-ttu-id="34835-149">如果沒有可用的埠：</span><span class="sxs-lookup"><span data-stu-id="34835-149">If no port is available:</span></span>

* <span data-ttu-id="34835-150">不會重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="34835-150">Redirection to HTTPS doesn't occur.</span></span>
* <span data-ttu-id="34835-151">中介軟體會記錄「無法判斷重新導向的 HTTPs 埠」的警告。</span><span class="sxs-lookup"><span data-stu-id="34835-151">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

<span data-ttu-id="34835-152">使用下列任何方法來指定 HTTPS 埠：</span><span class="sxs-lookup"><span data-stu-id="34835-152">Specify the HTTPS port using any of the following approaches:</span></span>

* <span data-ttu-id="34835-153">設定[HttpsRedirectionOptions. HttpsPort](#options)。</span><span class="sxs-lookup"><span data-stu-id="34835-153">Set [HttpsRedirectionOptions.HttpsPort](#options).</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="34835-154">設定 [`https_port`[主機] 設定](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#https_port)：</span><span class="sxs-lookup"><span data-stu-id="34835-154">Set the `https_port` [host setting](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#https_port):</span></span>

  * <span data-ttu-id="34835-155">在 [主機設定] 中。</span><span class="sxs-lookup"><span data-stu-id="34835-155">In host configuration.</span></span>
  * <span data-ttu-id="34835-156">藉由設定 `ASPNETCORE_HTTPS_PORT` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="34835-156">By setting the `ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
  * <span data-ttu-id="34835-157">藉由在*appsettings*中新增最上層專案：</span><span class="sxs-lookup"><span data-stu-id="34835-157">By adding a top-level entry in *appsettings.json*:</span></span>

    [!code-json[](enforcing-ssl/sample-snapshot/3.x/appsettings.json?highlight=2)]

* <span data-ttu-id="34835-158">使用[ASPNETCORE_URLS 環境變數](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#urls)，表示具有安全配置的埠。</span><span class="sxs-lookup"><span data-stu-id="34835-158">Indicate a port with the secure scheme using the [ASPNETCORE_URLS environment variable](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#urls).</span></span> <span data-ttu-id="34835-159">環境變數會設定伺服器。</span><span class="sxs-lookup"><span data-stu-id="34835-159">The environment variable configures the server.</span></span> <span data-ttu-id="34835-160">中介軟體會透過 <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>間接探索 HTTPS 埠。</span><span class="sxs-lookup"><span data-stu-id="34835-160">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="34835-161">這種方法無法在反向 proxy 部署中使用。</span><span class="sxs-lookup"><span data-stu-id="34835-161">This approach doesn't work in reverse proxy deployments.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

* <span data-ttu-id="34835-162">設定 [`https_port`[主機] 設定](xref:fundamentals/host/web-host#https-port)：</span><span class="sxs-lookup"><span data-stu-id="34835-162">Set the `https_port` [host setting](xref:fundamentals/host/web-host#https-port):</span></span>

  * <span data-ttu-id="34835-163">在 [主機設定] 中。</span><span class="sxs-lookup"><span data-stu-id="34835-163">In host configuration.</span></span>
  * <span data-ttu-id="34835-164">藉由設定 `ASPNETCORE_HTTPS_PORT` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="34835-164">By setting the `ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
  * <span data-ttu-id="34835-165">藉由在*appsettings*中新增最上層專案：</span><span class="sxs-lookup"><span data-stu-id="34835-165">By adding a top-level entry in *appsettings.json*:</span></span>

    [!code-json[](enforcing-ssl/sample-snapshot/2.x/appsettings.json?highlight=2)]

* <span data-ttu-id="34835-166">使用[ASPNETCORE_URLS 環境變數](xref:fundamentals/host/web-host#server-urls)，表示具有安全配置的埠。</span><span class="sxs-lookup"><span data-stu-id="34835-166">Indicate a port with the secure scheme using the [ASPNETCORE_URLS environment variable](xref:fundamentals/host/web-host#server-urls).</span></span> <span data-ttu-id="34835-167">環境變數會設定伺服器。</span><span class="sxs-lookup"><span data-stu-id="34835-167">The environment variable configures the server.</span></span> <span data-ttu-id="34835-168">中介軟體會透過 <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>間接探索 HTTPS 埠。</span><span class="sxs-lookup"><span data-stu-id="34835-168">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="34835-169">這種方法無法在反向 proxy 部署中使用。</span><span class="sxs-lookup"><span data-stu-id="34835-169">This approach doesn't work in reverse proxy deployments.</span></span>

::: moniker-end

* <span data-ttu-id="34835-170">在開發中，在*launchsettings.json*中設定 HTTPS URL。</span><span class="sxs-lookup"><span data-stu-id="34835-170">In development, set an HTTPS URL in *launchsettings.json*.</span></span> <span data-ttu-id="34835-171">使用 IIS Express 時啟用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="34835-171">Enable HTTPS when IIS Express is used.</span></span>

* <span data-ttu-id="34835-172">針對[Kestrel](xref:fundamentals/servers/kestrel)伺服器或[HTTP.sys](xref:fundamentals/servers/httpsys)伺服器的公眾面向邊緣部署，設定 HTTPS URL 端點。</span><span class="sxs-lookup"><span data-stu-id="34835-172">Configure an HTTPS URL endpoint for a public-facing edge deployment of [Kestrel](xref:fundamentals/servers/kestrel) server or [HTTP.sys](xref:fundamentals/servers/httpsys) server.</span></span> <span data-ttu-id="34835-173">應用程式只會使用**一個 HTTPS 埠**。</span><span class="sxs-lookup"><span data-stu-id="34835-173">Only **one HTTPS port** is used by the app.</span></span> <span data-ttu-id="34835-174">中介軟體會透過 <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>來探索埠。</span><span class="sxs-lookup"><span data-stu-id="34835-174">The middleware discovers the port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span>

> [!NOTE]
> <span data-ttu-id="34835-175">當應用程式在反向 proxy 設定中執行時，<xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> 無法使用。</span><span class="sxs-lookup"><span data-stu-id="34835-175">When an app is run in a reverse proxy configuration, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> isn't available.</span></span> <span data-ttu-id="34835-176">使用本節所述的其中一種其他方法來設定埠。</span><span class="sxs-lookup"><span data-stu-id="34835-176">Set the port using one of the other approaches described in this section.</span></span>

### <a name="edge-deployments"></a><span data-ttu-id="34835-177">邊緣部署</span><span class="sxs-lookup"><span data-stu-id="34835-177">Edge deployments</span></span> 

<span data-ttu-id="34835-178">當 Kestrel 或 HTTP.SYS 做為公眾面向的邊緣伺服器時，Kestrel 或 HTTP.SYS 必須設定為接聽兩者：</span><span class="sxs-lookup"><span data-stu-id="34835-178">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>

* <span data-ttu-id="34835-179">重新導向用戶端的安全埠（通常是生產環境中的443和開發中的5001）。</span><span class="sxs-lookup"><span data-stu-id="34835-179">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
* <span data-ttu-id="34835-180">不安全的埠（通常是在生產環境中為80，開發中則為5000）。</span><span class="sxs-lookup"><span data-stu-id="34835-180">The insecure port (typically, 80 in production and 5000 in development).</span></span>

<span data-ttu-id="34835-181">用戶端必須能夠存取不安全的埠，應用程式才能接收不安全的要求，並將用戶端重新導向至安全的埠。</span><span class="sxs-lookup"><span data-stu-id="34835-181">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect the client to the secure port.</span></span>

<span data-ttu-id="34835-182">如需詳細資訊，請參閱[Kestrel 端點 configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>。</span><span class="sxs-lookup"><span data-stu-id="34835-182">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

### <a name="deployment-scenarios"></a><span data-ttu-id="34835-183">部署案例</span><span class="sxs-lookup"><span data-stu-id="34835-183">Deployment scenarios</span></span>

<span data-ttu-id="34835-184">用戶端與伺服器之間的任何防火牆，也必須開啟流量的通訊埠。</span><span class="sxs-lookup"><span data-stu-id="34835-184">Any firewall between the client and server must also have communication ports open for traffic.</span></span>

<span data-ttu-id="34835-185">如果要求是在反向 proxy 設定中轉送，請在呼叫 HTTPS 重新導向中介軟體之前，使用[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="34835-185">If requests are forwarded in a reverse proxy configuration, use [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) before calling HTTPS Redirection Middleware.</span></span> <span data-ttu-id="34835-186">轉送的標頭中介軟體會使用 `X-Forwarded-Proto` 標頭來更新 `Request.Scheme`。</span><span class="sxs-lookup"><span data-stu-id="34835-186">Forwarded Headers Middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header.</span></span> <span data-ttu-id="34835-187">中介軟體允許重新導向 Uri 和其他安全性原則正確運作。</span><span class="sxs-lookup"><span data-stu-id="34835-187">The middleware permits redirect URIs and other security policies to work correctly.</span></span> <span data-ttu-id="34835-188">未使用轉送的標頭中介軟體時，後端應用程式可能不會收到正確的配置，且會在重新導向迴圈中結束。</span><span class="sxs-lookup"><span data-stu-id="34835-188">When Forwarded Headers Middleware isn't used, the backend app might not receive the correct scheme and end up in a redirect loop.</span></span> <span data-ttu-id="34835-189">常見的使用者錯誤訊息是發生太多次重新導向。</span><span class="sxs-lookup"><span data-stu-id="34835-189">A common end user error message is that too many redirects have occurred.</span></span>

<span data-ttu-id="34835-190">部署到 Azure App Service 時，請遵循教學課程[：將現有的自訂 SSL 憑證系結至 Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl)中的指引。</span><span class="sxs-lookup"><span data-stu-id="34835-190">When deploying to Azure App Service, follow the guidance in [Tutorial: Bind an existing custom SSL certificate to Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

### <a name="options"></a><span data-ttu-id="34835-191">選項</span><span class="sxs-lookup"><span data-stu-id="34835-191">Options</span></span>

<span data-ttu-id="34835-192">下列反白顯示的程式碼會呼叫[AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection)來設定中介軟體選項：</span><span class="sxs-lookup"><span data-stu-id="34835-192">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>


::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet2&highlight=14-18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet2&highlight=14-18)]

::: moniker-end


<span data-ttu-id="34835-193">只有在變更 `HttpsPort` 或 `RedirectStatusCode`的值時，才需要呼叫 `AddHttpsRedirection`。</span><span class="sxs-lookup"><span data-stu-id="34835-193">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="34835-194">上述反白顯示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="34835-194">The preceding highlighted code:</span></span>

* <span data-ttu-id="34835-195">將[HttpsRedirectionOptions RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*)設定為 <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>，這是預設值。</span><span class="sxs-lookup"><span data-stu-id="34835-195">Sets [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) to <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, which is the default value.</span></span> <span data-ttu-id="34835-196">使用 <xref:Microsoft.AspNetCore.Http.StatusCodes> 類別的欄位，以 `RedirectStatusCode`指派。</span><span class="sxs-lookup"><span data-stu-id="34835-196">Use the fields of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="34835-197">將 HTTPS 埠設定為5001。</span><span class="sxs-lookup"><span data-stu-id="34835-197">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="34835-198">預設值為443。</span><span class="sxs-lookup"><span data-stu-id="34835-198">The default value is 443.</span></span>

#### <a name="configure-permanent-redirects-in-production"></a><span data-ttu-id="34835-199">在生產環境中設定永久重新導向</span><span class="sxs-lookup"><span data-stu-id="34835-199">Configure permanent redirects in production</span></span>

<span data-ttu-id="34835-200">中介軟體會預設為傳送具有所有重新導向的[Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) 。</span><span class="sxs-lookup"><span data-stu-id="34835-200">The middleware defaults to sending a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) with all redirects.</span></span> <span data-ttu-id="34835-201">如果您想要在應用程式處於非開發環境時傳送永久重新導向狀態碼，請在非開發環境的條件式檢查中包裝中介軟體選項設定。</span><span class="sxs-lookup"><span data-stu-id="34835-201">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, wrap the middleware options configuration in a conditional check for a non-Development environment.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="34835-202">在*Startup.cs*中設定服務時：</span><span class="sxs-lookup"><span data-stu-id="34835-202">When configuring services in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IWebHostEnvironment (stored in _env) is injected into the Startup class.
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

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="34835-203">在*Startup.cs*中設定服務時：</span><span class="sxs-lookup"><span data-stu-id="34835-203">When configuring services in *Startup.cs*:</span></span>

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

::: moniker-end


## <a name="https-redirection-middleware-alternative-approach"></a><span data-ttu-id="34835-204">HTTPS 重新導向中介軟體替代方法</span><span class="sxs-lookup"><span data-stu-id="34835-204">HTTPS Redirection Middleware alternative approach</span></span>

<span data-ttu-id="34835-205">使用 HTTPS 重新導向中介軟體（`UseHttpsRedirection`）的替代方法是使用 URL 重寫中介軟體（`AddRedirectToHttps`）。</span><span class="sxs-lookup"><span data-stu-id="34835-205">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="34835-206">`AddRedirectToHttps` 也可以在重新導向執行時設定狀態碼和埠。</span><span class="sxs-lookup"><span data-stu-id="34835-206">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="34835-207">如需詳細資訊，請參閱[URL 重寫中介軟體](xref:fundamentals/url-rewriting)。</span><span class="sxs-lookup"><span data-stu-id="34835-207">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="34835-208">重新導向至 HTTPS 時，若不需要額外的重新導向規則，建議使用本主題中所述的 HTTPS 重新導向中介軟體（`UseHttpsRedirection`）。</span><span class="sxs-lookup"><span data-stu-id="34835-208">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

<a name="hsts"></a>

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="34835-209">HTTP 嚴格傳輸安全性通訊協定（HSTS）</span><span class="sxs-lookup"><span data-stu-id="34835-209">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="34835-210">根據[OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project)， [HTTP 嚴格傳輸安全性（HSTS）](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html)是由 web 應用程式透過使用回應標頭所指定的加入宣告安全性增強功能。</span><span class="sxs-lookup"><span data-stu-id="34835-210">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="34835-211">當[支援 HSTS 的瀏覽器](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html#browser-support)收到此標頭時：</span><span class="sxs-lookup"><span data-stu-id="34835-211">When a [browser that supports HSTS](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html#browser-support) receives this header:</span></span>

* <span data-ttu-id="34835-212">瀏覽器會儲存網域的設定，以防止透過 HTTP 傳送任何通訊。</span><span class="sxs-lookup"><span data-stu-id="34835-212">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="34835-213">瀏覽器會強制所有透過 HTTPS 進行的通訊。</span><span class="sxs-lookup"><span data-stu-id="34835-213">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="34835-214">瀏覽器會防止使用者使用不受信任或不正確憑證。</span><span class="sxs-lookup"><span data-stu-id="34835-214">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="34835-215">瀏覽器會停用允許使用者暫時信任這類憑證的提示。</span><span class="sxs-lookup"><span data-stu-id="34835-215">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="34835-216">因為 HSTS 是由用戶端強制執行，所以有一些限制：</span><span class="sxs-lookup"><span data-stu-id="34835-216">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="34835-217">用戶端必須支援 HSTS。</span><span class="sxs-lookup"><span data-stu-id="34835-217">The client must support HSTS.</span></span>
* <span data-ttu-id="34835-218">HSTS 至少需要一個成功的 HTTPS 要求，才能建立 HSTS 原則。</span><span class="sxs-lookup"><span data-stu-id="34835-218">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="34835-219">應用程式必須檢查每個 HTTP 要求，然後重新導向或拒絕 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="34835-219">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="34835-220">ASP.NET Core 2.1 和更新版本使用 `UseHsts` 擴充方法來執行 HSTS。</span><span class="sxs-lookup"><span data-stu-id="34835-220">ASP.NET Core 2.1 and later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="34835-221">當應用程式不在[開發模式](xref:fundamentals/environments)時，下列程式碼會呼叫 `UseHsts`：</span><span class="sxs-lookup"><span data-stu-id="34835-221">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet1&highlight=10)]

::: moniker-end

<span data-ttu-id="34835-222">`UseHsts` 不建議在開發中使用，因為瀏覽器會高度快取 HSTS 設定。</span><span class="sxs-lookup"><span data-stu-id="34835-222">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="34835-223">根據預設，`UseHsts` 會排除本機回送位址。</span><span class="sxs-lookup"><span data-stu-id="34835-223">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="34835-224">若為第一次執行 HTTPS 的生產環境，請使用其中一個 [方法，將初始 ](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*)HstsOptions 設定為較小的值<xref:System.TimeSpan>。</span><span class="sxs-lookup"><span data-stu-id="34835-224">For production environments that are implementing HTTPS for the first time, set the initial [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) to a small value using one of the <xref:System.TimeSpan> methods.</span></span> <span data-ttu-id="34835-225">如果您需要將 HTTPS 基礎結構還原為 HTTP，請將值從小時設定為不超過一天。</span><span class="sxs-lookup"><span data-stu-id="34835-225">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="34835-226">在您確信 HTTPS 設定的持續性之後，請增加 HSTS 的最大壽命值;常使用的值為一年。</span><span class="sxs-lookup"><span data-stu-id="34835-226">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span>

<span data-ttu-id="34835-227">下列程式碼範例：</span><span class="sxs-lookup"><span data-stu-id="34835-227">The following code:</span></span>


::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet2&highlight=5-12)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet2&highlight=5-12)]

::: moniker-end


* <span data-ttu-id="34835-228">設定嚴格傳輸安全性標頭的預先載入參數。</span><span class="sxs-lookup"><span data-stu-id="34835-228">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="34835-229">預先載入不是[RFC HSTS 規格](https://tools.ietf.org/html/rfc6797)的一部分，但 web 瀏覽器支援在全新安裝時預先載入 HSTS 網站。</span><span class="sxs-lookup"><span data-stu-id="34835-229">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="34835-230">請參閱 [https://hstspreload.org/](https://hstspreload.org/) 以取得詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="34835-230">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="34835-231">啟用[includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2)，這會將 HSTS 原則套用至裝載子域。</span><span class="sxs-lookup"><span data-stu-id="34835-231">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="34835-232">將嚴格傳輸安全性標頭的最大壽命參數明確設定為60天。</span><span class="sxs-lookup"><span data-stu-id="34835-232">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="34835-233">如果未設定，則預設為30天。</span><span class="sxs-lookup"><span data-stu-id="34835-233">If not set, defaults to 30 days.</span></span> <span data-ttu-id="34835-234">如需詳細資訊，請參閱[最大壽命](https://tools.ietf.org/html/rfc6797#section-6.1.1)指示詞。</span><span class="sxs-lookup"><span data-stu-id="34835-234">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="34835-235">將 `example.com` 新增至要排除的主機清單。</span><span class="sxs-lookup"><span data-stu-id="34835-235">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="34835-236">`UseHsts` 排除下列回送主機：</span><span class="sxs-lookup"><span data-stu-id="34835-236">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="34835-237">`localhost`： IPv4 回送位址。</span><span class="sxs-lookup"><span data-stu-id="34835-237">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="34835-238">`127.0.0.1`： IPv4 回送位址。</span><span class="sxs-lookup"><span data-stu-id="34835-238">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="34835-239">`[::1]`： IPv6 回送位址。</span><span class="sxs-lookup"><span data-stu-id="34835-239">`[::1]` : The IPv6 loopback address.</span></span>

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="34835-240">在建立專案時退出宣告 HTTPS/HSTS</span><span class="sxs-lookup"><span data-stu-id="34835-240">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="34835-241">在某些後端服務案例中，連線安全性會在網路的公開邊緣處理，而不需要在每個節點上設定連線安全性。</span><span class="sxs-lookup"><span data-stu-id="34835-241">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="34835-242">從 Visual Studio 中的範本或[dotnet new](/dotnet/core/tools/dotnet-new)命令產生的 Web 應用程式會啟用[HTTPS](#require-https)重新導向和[HSTS](#http-strict-transport-security-protocol-hsts)。</span><span class="sxs-lookup"><span data-stu-id="34835-242">Web apps that are generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require-https) and [HSTS](#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="34835-243">針對不需要這些案例的部署，您可以在從範本建立應用程式時退出宣告 HTTPS/HSTS。</span><span class="sxs-lookup"><span data-stu-id="34835-243">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="34835-244">若要退出 HTTPS/HSTS：</span><span class="sxs-lookup"><span data-stu-id="34835-244">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="34835-245">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34835-245">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="34835-246">取消核取 [**針對 HTTPS 設定**] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="34835-246">Uncheck the **Configure for HTTPS** check box.</span></span>

::: moniker range=">= aspnetcore-3.0"

![[新增 ASP.NET Core Web 應用程式] 對話方塊，其中顯示未選取的 [設定 HTTPS] 核取方塊。](enforcing-ssl/_static/out-vs2019.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

![[新增 ASP.NET Core Web 應用程式] 對話方塊，其中顯示未選取的 [設定 HTTPS] 核取方塊。](enforcing-ssl/_static/out.png)

::: moniker-end


# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="34835-249">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="34835-249">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="34835-250">使用 `--no-https` 選項。</span><span class="sxs-lookup"><span data-stu-id="34835-250">Use the `--no-https` option.</span></span> <span data-ttu-id="34835-251">例如</span><span class="sxs-lookup"><span data-stu-id="34835-251">For example</span></span>

```dotnetcli
dotnet new webapp --no-https
```

---

<a name="trust"></a>

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a><span data-ttu-id="34835-252">信任 Windows 和 macOS 上的 ASP.NET Core HTTPS 開發憑證</span><span class="sxs-lookup"><span data-stu-id="34835-252">Trust the ASP.NET Core HTTPS development certificate on Windows and macOS</span></span>

<span data-ttu-id="34835-253">.NET Core SDK 包含 HTTPS 開發憑證。</span><span class="sxs-lookup"><span data-stu-id="34835-253">The .NET Core SDK includes an HTTPS development certificate.</span></span> <span data-ttu-id="34835-254">憑證會在首次執行體驗中安裝。</span><span class="sxs-lookup"><span data-stu-id="34835-254">The certificate is installed as part of the first-run experience.</span></span> <span data-ttu-id="34835-255">例如，`dotnet --info` 會產生類似下列的輸出：</span><span class="sxs-lookup"><span data-stu-id="34835-255">For example, `dotnet --info` produces output similar to the following:</span></span>

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

<span data-ttu-id="34835-256">安裝 .NET Core SDK 會將 ASP.NET Core HTTPS 開發憑證安裝至本機使用者憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="34835-256">Installing the .NET Core SDK installs the ASP.NET Core HTTPS development certificate to the local user certificate store.</span></span> <span data-ttu-id="34835-257">憑證已安裝，但不受信任。</span><span class="sxs-lookup"><span data-stu-id="34835-257">The certificate has been installed, but it's not trusted.</span></span> <span data-ttu-id="34835-258">若要信任憑證，請執行 dotnet `dev-certs` 工具的一次性步驟：</span><span class="sxs-lookup"><span data-stu-id="34835-258">To trust the certificate perform the one-time step to run the dotnet `dev-certs` tool:</span></span>

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="34835-259">下列命令會提供 `dev-certs` 工具的說明：</span><span class="sxs-lookup"><span data-stu-id="34835-259">The following command provides help on the `dev-certs` tool:</span></span>

```dotnetcli
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="34835-260">如何設定 Docker 的開發人員憑證</span><span class="sxs-lookup"><span data-stu-id="34835-260">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="34835-261">請參閱[這個 GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/6199)。</span><span class="sxs-lookup"><span data-stu-id="34835-261">See [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6199).</span></span>

<a name="wsl"></a>

## <a name="trust-https-certificate-from-windows-subsystem-for-linux"></a><span data-ttu-id="34835-262">從適用于 Linux 的 Windows 子系統信任 HTTPS 憑證</span><span class="sxs-lookup"><span data-stu-id="34835-262">Trust HTTPS certificate from Windows Subsystem for Linux</span></span>

<span data-ttu-id="34835-263">適用于 Linux 的 Windows 子系統（WSL）會產生 HTTPS 自我簽署憑證。若要將 Windows 憑證存放區設定為信任 WSL 憑證：</span><span class="sxs-lookup"><span data-stu-id="34835-263">The Windows Subsystem for Linux (WSL) generates a HTTPS self-signed cert. To configure the Windows certificate store to trust the WSL certificate:</span></span>

* <span data-ttu-id="34835-264">執行下列命令以匯出 WSL 產生的憑證： `dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`</span><span class="sxs-lookup"><span data-stu-id="34835-264">Run the following command to export the WSL generated certificate: `dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`</span></span>
* <span data-ttu-id="34835-265">在 WSL 視窗中，執行下列命令： `ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`</span><span class="sxs-lookup"><span data-stu-id="34835-265">In a WSL window, run the following command: `ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`</span></span>

  <span data-ttu-id="34835-266">上述命令會設定環境變數，讓 Linux 使用 Windows 受信任的憑證。</span><span class="sxs-lookup"><span data-stu-id="34835-266">The preceding command sets the environment variables so Linux uses the Windows trusted certificate.</span></span>

## <a name="troubleshoot-certificate-problems"></a><span data-ttu-id="34835-267">針對憑證問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="34835-267">Troubleshoot certificate problems</span></span>

<span data-ttu-id="34835-268">當 ASP.NET Core 的 HTTPS 開發憑證已[安裝並受信任](#trust)，但您仍有瀏覽器警告，指出憑證不受信任時，本節會提供協助。</span><span class="sxs-lookup"><span data-stu-id="34835-268">This section provides help when the ASP.NET Core HTTPS development certificate has been [installed and trusted](#trust), but you still have browser warnings that the certificate is not trusted.</span></span> <span data-ttu-id="34835-269">[Kestrel](xref:fundamentals/servers/kestrel)會使用 ASP.NET Core HTTPS 開發憑證。</span><span class="sxs-lookup"><span data-stu-id="34835-269">The ASP.NET Core HTTPS development certificate is used by [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

### <a name="all-platforms---certificate-not-trusted"></a><span data-ttu-id="34835-270">所有平臺-憑證不受信任</span><span class="sxs-lookup"><span data-stu-id="34835-270">All platforms - certificate not trusted</span></span>

<span data-ttu-id="34835-271">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="34835-271">Run the following commands:</span></span>

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

<span data-ttu-id="34835-272">關閉任何開啟的瀏覽器實例。</span><span class="sxs-lookup"><span data-stu-id="34835-272">Close any browser instances open.</span></span> <span data-ttu-id="34835-273">在應用程式中開啟新的瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="34835-273">Open a new browser window to app.</span></span> <span data-ttu-id="34835-274">瀏覽器會快取憑證信任。</span><span class="sxs-lookup"><span data-stu-id="34835-274">Certificate trust is cached by browsers.</span></span>

<span data-ttu-id="34835-275">上述命令會解決大部分的瀏覽器信任問題。</span><span class="sxs-lookup"><span data-stu-id="34835-275">The preceding commands solve most browser trust issues.</span></span> <span data-ttu-id="34835-276">如果瀏覽器仍不信任憑證，請遵循遵循的平臺特定建議。</span><span class="sxs-lookup"><span data-stu-id="34835-276">If the browser is still not trusting the certificate, follow the platform specific suggestions that follow.</span></span>

### <a name="docker---certificate-not-trusted"></a><span data-ttu-id="34835-277">Docker-憑證不受信任</span><span class="sxs-lookup"><span data-stu-id="34835-277">Docker - certificate not trusted</span></span>

* <span data-ttu-id="34835-278">刪除*C:\Users\{USER} \AppData\Roaming\ASP.NET\Https*資料夾。</span><span class="sxs-lookup"><span data-stu-id="34835-278">Delete the *C:\Users\{USER}\AppData\Roaming\ASP.NET\Https* folder.</span></span>
* <span data-ttu-id="34835-279">清除方案。</span><span class="sxs-lookup"><span data-stu-id="34835-279">Clean the solution.</span></span> <span data-ttu-id="34835-280">刪除 [bin] 和 [obj] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="34835-280">Delete the *bin* and *obj* folders.</span></span>
* <span data-ttu-id="34835-281">重新開機開發工具。</span><span class="sxs-lookup"><span data-stu-id="34835-281">Restart the development tool.</span></span> <span data-ttu-id="34835-282">例如，Visual Studio、Visual Studio Code 或 Visual Studio for Mac。</span><span class="sxs-lookup"><span data-stu-id="34835-282">For example, Visual Studio, Visual Studio Code, or Visual Studio for Mac.</span></span>

### <a name="windows---certificate-not-trusted"></a><span data-ttu-id="34835-283">Windows-憑證不受信任</span><span class="sxs-lookup"><span data-stu-id="34835-283">Windows - certificate not trusted</span></span>

* <span data-ttu-id="34835-284">檢查證書存儲中的憑證。</span><span class="sxs-lookup"><span data-stu-id="34835-284">Check the certificates in the certificate store.</span></span> <span data-ttu-id="34835-285">在 [`Current User > Personal > Certificates`] 和 [`Current User > Trusted root certification authorities > Certificates`] 底下，應該會有一個 `localhost` 憑證具有 `ASP.NET Core HTTPS development certificate` 易記名稱</span><span class="sxs-lookup"><span data-stu-id="34835-285">There should be a `localhost` certificate with the `ASP.NET Core HTTPS development certificate` friendly name both under `Current User > Personal > Certificates` and `Current User > Trusted root certification authorities > Certificates`</span></span>
* <span data-ttu-id="34835-286">從個人和信任的根憑證授權單位移除所有找到的憑證。</span><span class="sxs-lookup"><span data-stu-id="34835-286">Remove all the found certificates from both Personal and Trusted root certification authorities.</span></span> <span data-ttu-id="34835-287">請勿**移除 IIS Express** localhost 憑證。</span><span class="sxs-lookup"><span data-stu-id="34835-287">Do **not** remove the IIS Express localhost certificate.</span></span>
* <span data-ttu-id="34835-288">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="34835-288">Run the following commands:</span></span>

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

<span data-ttu-id="34835-289">關閉任何開啟的瀏覽器實例。</span><span class="sxs-lookup"><span data-stu-id="34835-289">Close any browser instances open.</span></span> <span data-ttu-id="34835-290">在應用程式中開啟新的瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="34835-290">Open a new browser window to app.</span></span>

### <a name="os-x---certificate-not-trusted"></a><span data-ttu-id="34835-291">OS X-憑證不受信任</span><span class="sxs-lookup"><span data-stu-id="34835-291">OS X - certificate not trusted</span></span>

* <span data-ttu-id="34835-292">開啟 [KeyChain 存取]。</span><span class="sxs-lookup"><span data-stu-id="34835-292">Open KeyChain Access.</span></span>
* <span data-ttu-id="34835-293">選取 [系統 keychain]。</span><span class="sxs-lookup"><span data-stu-id="34835-293">Select the System keychain.</span></span>
* <span data-ttu-id="34835-294">檢查 localhost 憑證是否存在。</span><span class="sxs-lookup"><span data-stu-id="34835-294">Check for the presence of a localhost certificate.</span></span>
* <span data-ttu-id="34835-295">檢查其是否包含圖示上的 `+` 符號，以指出其是否受其信任，以供所有使用者使用。</span><span class="sxs-lookup"><span data-stu-id="34835-295">Check that it contains a `+` symbol on the icon to indicate its trusted for all users.</span></span>
* <span data-ttu-id="34835-296">從系統 keychain 中移除憑證。</span><span class="sxs-lookup"><span data-stu-id="34835-296">Remove the certificate from the system keychain.</span></span>
* <span data-ttu-id="34835-297">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="34835-297">Run the following commands:</span></span>

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

<span data-ttu-id="34835-298">關閉任何開啟的瀏覽器實例。</span><span class="sxs-lookup"><span data-stu-id="34835-298">Close any browser instances open.</span></span> <span data-ttu-id="34835-299">在應用程式中開啟新的瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="34835-299">Open a new browser window to app.</span></span>

<span data-ttu-id="34835-300">請參閱[使用 IIS Express （aspnet/AspNetCore #16892）的 HTTPS 錯誤](https://github.com/aspnet/AspNetCore/issues/16892)，以疑難排解 Visual Studio 的憑證問題。</span><span class="sxs-lookup"><span data-stu-id="34835-300">See [HTTPS Error using IIS Express (aspnet/AspNetCore #16892)](https://github.com/aspnet/AspNetCore/issues/16892) for troubleshooting certificate issues with Visual Studio.</span></span>

### <a name="iis-express-ssl-certificate-used-with-visual-studio"></a><span data-ttu-id="34835-301">搭配 Visual Studio 使用的 IIS Express SSL 憑證</span><span class="sxs-lookup"><span data-stu-id="34835-301">IIS Express SSL certificate used with Visual Studio</span></span>

<span data-ttu-id="34835-302">若要修正 IIS Express 憑證的問題，請從 Visual Studio 安裝程式中選取 [**修復**]。</span><span class="sxs-lookup"><span data-stu-id="34835-302">To fix problems with the IIS Express certificate, select **Repair** from the Visual Studio installer.</span></span>

## <a name="additional-information"></a><span data-ttu-id="34835-303">其他資訊</span><span class="sxs-lookup"><span data-stu-id="34835-303">Additional information</span></span>

* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="34835-304">在 Linux 上使用 Apache： HTTPS 設定的主機 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="34835-304">Host ASP.NET Core on Linux with Apache: HTTPS configuration</span></span>](xref:host-and-deploy/linux-apache#https-configuration)
* [<span data-ttu-id="34835-305">在 Linux 上使用 Nginx： HTTPS 設定的主機 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="34835-305">Host ASP.NET Core on Linux with Nginx: HTTPS configuration</span></span>](xref:host-and-deploy/linux-nginx#https-configuration)
* [<span data-ttu-id="34835-306">如何在 IIS 上設定 SSL</span><span class="sxs-lookup"><span data-stu-id="34835-306">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [<span data-ttu-id="34835-307">OWASP HSTS 瀏覽器支援</span><span class="sxs-lookup"><span data-stu-id="34835-307">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
