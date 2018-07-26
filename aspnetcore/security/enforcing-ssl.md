---
title: 強制使用 ASP.NET Core 中的 HTTPS
author: rick-anderson
description: 示範如何要求 HTTPS/TLS 中的 ASP.NET Core web 應用程式。
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: c3d92994c0331b1408e246953454910ca1f4dc43
ms.sourcegitcommit: c8e62aa766641aa55105f7db79cdf2b27a6e5977
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/25/2018
ms.locfileid: "39254827"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="701d5-103">強制使用 ASP.NET Core 中的 HTTPS</span><span class="sxs-lookup"><span data-stu-id="701d5-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="701d5-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="701d5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="701d5-105">本文件說明如何：</span><span class="sxs-lookup"><span data-stu-id="701d5-105">This document shows how to:</span></span>

* <span data-ttu-id="701d5-106">需要 HTTPS 進行的所有要求。</span><span class="sxs-lookup"><span data-stu-id="701d5-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="701d5-107">將所有 HTTP 要求重新都導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="701d5-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="701d5-108">請勿**未**使用[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute)接收機密資訊的 Web Api 上。</span><span class="sxs-lookup"><span data-stu-id="701d5-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="701d5-109">`RequireHttpsAttribute` 若要從 HTTP 至 HTTPS 的瀏覽器重新導向，會使用 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="701d5-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="701d5-110">API 用戶端可能不了解，或是遵循從 HTTP 重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="701d5-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="701d5-111">此類用戶端可能會透過 HTTP 傳送資訊。</span><span class="sxs-lookup"><span data-stu-id="701d5-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="701d5-112">Web Api 應執行下列之一：</span><span class="sxs-lookup"><span data-stu-id="701d5-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="701d5-113">不在 HTTP 上接聽。</span><span class="sxs-lookup"><span data-stu-id="701d5-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="701d5-114">關閉與狀態碼 400 （不正確的要求） 的連線，並不會提供要求。</span><span class="sxs-lookup"><span data-stu-id="701d5-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="701d5-115">需要 HTTPS</span><span class="sxs-lookup"><span data-stu-id="701d5-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="701d5-116">我們建議所有的 ASP.NET Core web 應用程式呼叫 HTTPS 重新導向中介軟體 ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) 到所有的 HTTP 要求重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="701d5-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="701d5-117">下列程式碼會呼叫`UseHttpsRedirection`在`Startup`類別：</span><span class="sxs-lookup"><span data-stu-id="701d5-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="701d5-118">上述反白顯示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="701d5-118">The preceding highlighted code:</span></span>

* <span data-ttu-id="701d5-119">使用預設[HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`)。</span><span class="sxs-lookup"><span data-stu-id="701d5-119">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span></span> <span data-ttu-id="701d5-120">生產環境應用程式應該呼叫[UseHsts](#hsts)。</span><span class="sxs-lookup"><span data-stu-id="701d5-120">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="701d5-121">使用預設[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443)。</span><span class="sxs-lookup"><span data-stu-id="701d5-121">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).</span></span>

<span data-ttu-id="701d5-122">下列程式碼會呼叫[AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection)設定中介軟體選項：</span><span class="sxs-lookup"><span data-stu-id="701d5-122">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="701d5-123">上述反白顯示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="701d5-123">The preceding highlighted code:</span></span>

* <span data-ttu-id="701d5-124">設定組[HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode)到`Status307TemporaryRedirect`，這是預設值。</span><span class="sxs-lookup"><span data-stu-id="701d5-124">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="701d5-125">生產環境應用程式應該呼叫[UseHsts](#hsts)。</span><span class="sxs-lookup"><span data-stu-id="701d5-125">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="701d5-126">設定 HTTPS 連接埠為 5001。</span><span class="sxs-lookup"><span data-stu-id="701d5-126">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="701d5-127">預設值為 443。</span><span class="sxs-lookup"><span data-stu-id="701d5-127">The default value is 443.</span></span>

<span data-ttu-id="701d5-128">下列機制會自動設定連接埠：</span><span class="sxs-lookup"><span data-stu-id="701d5-128">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="701d5-129">中介軟體可以探索透過連接埠[IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)當符合下列條件：</span><span class="sxs-lookup"><span data-stu-id="701d5-129">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="701d5-130">Kestrel 或 HTTP.sys 可直接使用 HTTPS 端點 （也適用於 Visual Studio Code 的偵錯工具執行應用程式）。</span><span class="sxs-lookup"><span data-stu-id="701d5-130">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="701d5-131">只有**一個 HTTPS 連接埠**應用程式使用。</span><span class="sxs-lookup"><span data-stu-id="701d5-131">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="701d5-132">使用 visual Studio:</span><span class="sxs-lookup"><span data-stu-id="701d5-132">Visual Studio is used:</span></span>
  - <span data-ttu-id="701d5-133">IIS Express 已啟用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="701d5-133">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="701d5-134">*launchSettings.json*設定`sslPort`適用於 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="701d5-134">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="701d5-135">應用程式執行 （例如 IIS、 IIS Express），在反向 proxy 後方時`IServerAddressesFeature`無法使用。</span><span class="sxs-lookup"><span data-stu-id="701d5-135">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="701d5-136">您必須手動設定連接埠。</span><span class="sxs-lookup"><span data-stu-id="701d5-136">The port must be manually configured.</span></span> <span data-ttu-id="701d5-137">當未設定連接埠時，不是重新導向要求。</span><span class="sxs-lookup"><span data-stu-id="701d5-137">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="701d5-138">可以藉由設定設定的連接埠:</span><span class="sxs-lookup"><span data-stu-id="701d5-138">The port can be configured by setting the:</span></span>

* <span data-ttu-id="701d5-139">`ASPNETCORE_HTTPS_PORT` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="701d5-139">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="701d5-140">`http_port` 主機組態機碼 (例如，透過*hostsettings.json*或命令列引數)。</span><span class="sxs-lookup"><span data-stu-id="701d5-140">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="701d5-141">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport)。</span><span class="sxs-lookup"><span data-stu-id="701d5-141">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="701d5-142">請參閱上述範例，示範如何設定連接埠為 5001。</span><span class="sxs-lookup"><span data-stu-id="701d5-142">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="701d5-143">可以間接設定的連接埠，藉由設定 URL`ASPNETCORE_URLS`環境變數。</span><span class="sxs-lookup"><span data-stu-id="701d5-143">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="701d5-144">環境變數設定的伺服器，，，然後在中介軟體間接探索透過 HTTPS 連接埠`IServerAddressesFeature`。</span><span class="sxs-lookup"><span data-stu-id="701d5-144">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="701d5-145">如果已設定任何連接埠：</span><span class="sxs-lookup"><span data-stu-id="701d5-145">If no port is set:</span></span>

* <span data-ttu-id="701d5-146">要求未重新導向。</span><span class="sxs-lookup"><span data-stu-id="701d5-146">Requests aren't redirected.</span></span>
* <span data-ttu-id="701d5-147">中介軟體會記錄警告。</span><span class="sxs-lookup"><span data-stu-id="701d5-147">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="701d5-148">除了使用 HTTPS 重新導向中介軟體 (`UseHttpsRedirection`) 是使用 URL 重寫中介軟體 (`AddRedirectToHttps`)。</span><span class="sxs-lookup"><span data-stu-id="701d5-148">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="701d5-149">`AddRedirectToHttps` 也可以設定的狀態碼和連接埠重新導向為執行時。</span><span class="sxs-lookup"><span data-stu-id="701d5-149">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="701d5-150">如需詳細資訊，請參閱 < [URL 重寫中介軟體](xref:fundamentals/url-rewriting)。</span><span class="sxs-lookup"><span data-stu-id="701d5-150">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="701d5-151">當重新導向至 HTTPS，而不需要額外的重新導向規則，我們建議使用 HTTPS 重新導向中介軟體 (`UseHttpsRedirection`) 本主題中所述。</span><span class="sxs-lookup"><span data-stu-id="701d5-151">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="701d5-152">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute)用來要求 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="701d5-152">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="701d5-153">`[RequireHttpsAttribute]` 可以裝飾控制器或方法，或可以全域套用。</span><span class="sxs-lookup"><span data-stu-id="701d5-153">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="701d5-154">若要全域套用的屬性，新增下列程式碼`ConfigureServices`在`Startup`:</span><span class="sxs-lookup"><span data-stu-id="701d5-154">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="701d5-155">上述反白顯示的程式碼會要求所有要求都使用`HTTPS`; 因此，HTTP 要求會被忽略。</span><span class="sxs-lookup"><span data-stu-id="701d5-155">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="701d5-156">而下列反白顯示的程式碼會將所有 HTTP 要求都重新導向至 HTTPS:</span><span class="sxs-lookup"><span data-stu-id="701d5-156">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="701d5-157">如需詳細資訊，請參閱 < [URL 重寫中介軟體](xref:fundamentals/url-rewriting)。</span><span class="sxs-lookup"><span data-stu-id="701d5-157">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="701d5-158">中介軟體也允許應用程式執行時重新導向設定的狀態碼或狀態碼和連接埠。</span><span class="sxs-lookup"><span data-stu-id="701d5-158">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="701d5-159">全域使用 HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) 是安全性最佳作法。</span><span class="sxs-lookup"><span data-stu-id="701d5-159">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="701d5-160">將`[RequireHttps]`屬性套用至所有控制器，不會比全域使用 HTTPS 來的安全。</span><span class="sxs-lookup"><span data-stu-id="701d5-160">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="701d5-161">您無法保證`[RequireHttps]`新增新的控制器和 Razor 頁面時，屬性會套用。</span><span class="sxs-lookup"><span data-stu-id="701d5-161">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="701d5-162">HTTP Strict Transport 安全性通訊協定 (HSTS)</span><span class="sxs-lookup"><span data-stu-id="701d5-162">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="701d5-163">每個[OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project)， [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet)是透過使用特殊的回應標頭的 web 應用程式所指定的選擇加入的安全性增強功能。</span><span class="sxs-lookup"><span data-stu-id="701d5-163">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="701d5-164">支援的瀏覽器收到此標頭之後該瀏覽器會防止任何通訊透過 HTTP 傳送至指定的網域，並改為將會透過 HTTPS 傳送的所有通訊。</span><span class="sxs-lookup"><span data-stu-id="701d5-164">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="701d5-165">它也會防止 HTTPS 點選提示上的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="701d5-165">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="701d5-166">ASP.NET Core 2.1 或更新版本會實作 HSTS 與`UseHsts`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="701d5-166">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="701d5-167">下列程式碼會呼叫`UseHsts`應用程式不在[開發模式](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="701d5-167">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="701d5-168">`UseHsts` 不建議在開發過程中因為 HSTS 標頭是高可快取瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="701d5-168">`UseHsts` isn't recommended in development because the HSTS header is highly cacheable by browsers.</span></span> <span data-ttu-id="701d5-169">根據預設，`UseHsts`排除本機回送位址。</span><span class="sxs-lookup"><span data-stu-id="701d5-169">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="701d5-170">下列程式碼範例：</span><span class="sxs-lookup"><span data-stu-id="701d5-170">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="701d5-171">設定 Strict 傳輸安全性標頭的預先載入的參數。</span><span class="sxs-lookup"><span data-stu-id="701d5-171">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="701d5-172">預先載入不屬於[RFC HSTS 規格](https://tools.ietf.org/html/rfc6797)，但要預先載入 HSTS 上全新安裝的站台的網頁瀏覽器支援。</span><span class="sxs-lookup"><span data-stu-id="701d5-172">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="701d5-173">請參閱 [https://hstspreload.org/](https://hstspreload.org/) 以取得詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="701d5-173">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="701d5-174">可讓[includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2)，套用 HSTS 原則來裝載子網域。</span><span class="sxs-lookup"><span data-stu-id="701d5-174">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="701d5-175">明確設定為 60 天的 Strict-傳輸層安全性標頭的最大壽命參數。</span><span class="sxs-lookup"><span data-stu-id="701d5-175">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="701d5-176">如果未設定，預設值為 30 天。</span><span class="sxs-lookup"><span data-stu-id="701d5-176">If not set, defaults to 30 days.</span></span> <span data-ttu-id="701d5-177">請參閱[最大壽命指示詞](https://tools.ietf.org/html/rfc6797#section-6.1.1)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="701d5-177">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="701d5-178">新增`example.com`的主機，以排除清單。</span><span class="sxs-lookup"><span data-stu-id="701d5-178">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="701d5-179">`UseHsts` 排除下列 「 回送 」 主控件：</span><span class="sxs-lookup"><span data-stu-id="701d5-179">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="701d5-180">`localhost` : IPv4 回送位址。</span><span class="sxs-lookup"><span data-stu-id="701d5-180">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="701d5-181">`127.0.0.1` : IPv4 回送位址。</span><span class="sxs-lookup"><span data-stu-id="701d5-181">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="701d5-182">`[::1]` : IPv6 回送位址。</span><span class="sxs-lookup"><span data-stu-id="701d5-182">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="701d5-183">上述範例顯示如何新增其他主機。</span><span class="sxs-lookup"><span data-stu-id="701d5-183">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="701d5-184">選擇退出的 HTTPS 上建立專案</span><span class="sxs-lookup"><span data-stu-id="701d5-184">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="701d5-185">（從 Visual Studio 或 dotnet 命令列） 的 ASP.NET Core 2.1 或更新版本的 web 應用程式範本可讓[HTTPS 重新導向](#require)並[HSTS](#hsts)。</span><span class="sxs-lookup"><span data-stu-id="701d5-185">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="701d5-186">對於不需要 HTTPS 的部署，您可以選擇退出的 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="701d5-186">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="701d5-187">比方說，就不需要其中 HTTPS 處理外部邊緣，每個節點上使用 HTTPS 的某些後端服務。</span><span class="sxs-lookup"><span data-stu-id="701d5-187">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="701d5-188">若要退出 HTTPS:</span><span class="sxs-lookup"><span data-stu-id="701d5-188">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="701d5-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="701d5-189">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="701d5-190">取消核取**設定為使用 HTTPS**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="701d5-190">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![實體圖表](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="701d5-192">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="701d5-192">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="701d5-193">使用 `--no-https` 選項。</span><span class="sxs-lookup"><span data-stu-id="701d5-193">Use the `--no-https` option.</span></span> <span data-ttu-id="701d5-194">例如</span><span class="sxs-lookup"><span data-stu-id="701d5-194">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="701d5-195">如何設定適用於 Docker 的開發人員憑證</span><span class="sxs-lookup"><span data-stu-id="701d5-195">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="701d5-196">請參閱[此 GitHub 問題](https://github.com/aspnet/Docs/issues/6199)。</span><span class="sxs-lookup"><span data-stu-id="701d5-196">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
