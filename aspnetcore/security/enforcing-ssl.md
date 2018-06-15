---
title: 強制在 ASP.NET Core HTTPS
author: rick-anderson
description: 示範如何要求 HTTPS/TLS 中 ASP.NET Core web 應用程式。
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 48a25b7ba7affe84cfa6fe16096409239c510221
ms.sourcegitcommit: 40b102ecf88e53d9d872603ce6f3f7044bca95ce
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/15/2018
ms.locfileid: "35652184"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="fa4f5-103">強制在 ASP.NET Core HTTPS</span><span class="sxs-lookup"><span data-stu-id="fa4f5-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="fa4f5-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fa4f5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fa4f5-105">本文件說明如何：</span><span class="sxs-lookup"><span data-stu-id="fa4f5-105">This document shows how to:</span></span>

* <span data-ttu-id="fa4f5-106">所有要求需要 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="fa4f5-107">將所有 HTTP 要求重新都導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="fa4f5-108">請勿**不**使用[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute)上接收機密資訊的 Web Api。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="fa4f5-109">`RequireHttpsAttribute` 從 HTTP 至 HTTPS 的瀏覽器重新導向會使用 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="fa4f5-110">API 用戶端可能不了解，或是遵循從 HTTP 重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="fa4f5-111">這類用戶端可能會透過 HTTP 傳送資訊。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="fa4f5-112">Web 應用程式開發介面應執行下列之一：</span><span class="sxs-lookup"><span data-stu-id="fa4f5-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="fa4f5-113">不在 HTTP 上接聽。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="fa4f5-114">關閉與狀態碼 400 （不正確的要求） 的連線，並不會處理要求。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="fa4f5-115">需要 HTTPS</span><span class="sxs-lookup"><span data-stu-id="fa4f5-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="fa4f5-116">我們建議所有 ASP.NET Core web 應用程式都呼叫 HTTPS 的重新導向中介軟體 ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) 將所有 HTTP 要求重新都導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="fa4f5-117">下列程式碼會呼叫`UseHttpsRedirection`中`Startup`類別：</span><span class="sxs-lookup"><span data-stu-id="fa4f5-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="fa4f5-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="fa4f5-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>

<span data-ttu-id="fa4f5-119">下列程式碼會呼叫[AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection)設定中介軟體選項：</span><span class="sxs-lookup"><span data-stu-id="fa4f5-119">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

<span data-ttu-id="fa4f5-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="fa4f5-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

<span data-ttu-id="fa4f5-121">上述的反白顯示程式碼：</span><span class="sxs-lookup"><span data-stu-id="fa4f5-121">The preceding highlighted code:</span></span>

* <span data-ttu-id="fa4f5-122">設定[HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode)至`Status307TemporaryRedirect`，這是預設值。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-122">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="fa4f5-123">實際執行應用程式應該呼叫[UseHsts](#hsts)。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-123">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="fa4f5-124">5001 設定 HTTPS 連接埠。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-124">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="fa4f5-125">預設值是 443。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-125">The default value is 443.</span></span>

<span data-ttu-id="fa4f5-126">下列的機制會自動設定連接埠：</span><span class="sxs-lookup"><span data-stu-id="fa4f5-126">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="fa4f5-127">中介軟體可探索的連接埠透過[IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)套用以下條件：</span><span class="sxs-lookup"><span data-stu-id="fa4f5-127">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="fa4f5-128">Kestrel 或 HTTP.sys 可直接與 HTTPS 端點 （也適用於 Visual Studio 程式碼的偵錯工具執行應用程式）。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-128">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="fa4f5-129">只有**一個 HTTPS 連接埠**應用程式使用。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-129">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="fa4f5-130">使用 visual Studio:</span><span class="sxs-lookup"><span data-stu-id="fa4f5-130">Visual Studio is used:</span></span>
  - <span data-ttu-id="fa4f5-131">IIS Express 已啟用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-131">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="fa4f5-132">*launchSettings.json*設定`sslPort`IIS express。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-132">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="fa4f5-133">應用程式執行 （例如，IIS、 IIS Express） 的反向 proxy 後方時`IServerAddressesFeature`無法使用。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-133">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="fa4f5-134">您必須手動設定連接埠。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-134">The port must be manually configured.</span></span> <span data-ttu-id="fa4f5-135">當未設定連接埠時，不是重新導向要求。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-135">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="fa4f5-136">您可以設定連接埠設定:</span><span class="sxs-lookup"><span data-stu-id="fa4f5-136">The port can be configured by setting the:</span></span>

* <span data-ttu-id="fa4f5-137">`ASPNETCORE_HTTPS_PORT` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-137">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="fa4f5-138">`http_port` 主機組態機碼 (例如，透過*hostsettings.json*或命令列引數)。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-138">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="fa4f5-139">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport)。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-139">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="fa4f5-140">示範如何設定連接埠 5001 至上述範例，請參閱。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-140">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="fa4f5-141">可以間接設定連接埠，藉由設定 URL`ASPNETCORE_URLS`環境變數。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-141">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="fa4f5-142">環境變數設定的伺服器，和中介軟體然後間接探索透過 HTTPS 連接埠`IServerAddressesFeature`。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-142">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="fa4f5-143">如果沒有連接埠設定：</span><span class="sxs-lookup"><span data-stu-id="fa4f5-143">If no port is set:</span></span>

* <span data-ttu-id="fa4f5-144">未重新導向要求。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-144">Requests aren't redirected.</span></span>
* <span data-ttu-id="fa4f5-145">中介軟體，記錄警告。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-145">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="fa4f5-146">使用 HTTPS 的重新導向中介軟體的替代方案 (`UseHttpsRedirection`) 是使用 URL 重寫中介軟體 (`AddRedirectToHttps`)。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-146">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="fa4f5-147">`AddRedirectToHttps` 也可以設定的狀態碼和連接埠重新導向執行時。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-147">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="fa4f5-148">如需詳細資訊，請參閱[URL 重寫中介軟體](xref:fundamentals/url-rewriting)。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-148">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="fa4f5-149">當重新導向至 HTTPS，而不需要額外的重新導向規則，我們建議使用 HTTPS 的重新導向中介軟體 (`UseHttpsRedirection`) 本主題中所述。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-149">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="fa4f5-150">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute)用來要求 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-150">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="fa4f5-151">`[RequireHttpsAttribute]` 可以裝飾控制器或方法，或可以全域套用。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-151">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="fa4f5-152">若要全域套用的屬性，加入下列程式碼加入`ConfigureServices`中`Startup`:</span><span class="sxs-lookup"><span data-stu-id="fa4f5-152">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="fa4f5-153">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="fa4f5-153">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="fa4f5-154">上述的反白顯示程式碼需要的所有要求都使用`HTTPS`; 因此，HTTP 要求會被忽略。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-154">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="fa4f5-155">而下列反白顯示的程式碼會將所有 HTTP 要求都重新導向至 HTTPS:</span><span class="sxs-lookup"><span data-stu-id="fa4f5-155">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="fa4f5-156">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="fa4f5-156">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="fa4f5-157">如需詳細資訊，請參閱[URL 重寫中介軟體](xref:fundamentals/url-rewriting)。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-157">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="fa4f5-158">中介軟體也可讓應用程式，以執行重新導向時設定的狀態碼或狀態碼和連接埠。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-158">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="fa4f5-159">全域使用 HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) 是安全性最佳作法。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-159">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="fa4f5-160">將`[RequireHttps]`屬性套用至所有控制器，不會比全域使用 HTTPS 來的安全。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-160">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="fa4f5-161">您無法保證`[RequireHttps]`加入新的控制器和 Razor 頁面時，屬性會套用。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-161">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="fa4f5-162">HTTP 嚴格的傳輸安全性通訊協定 (HSTS)</span><span class="sxs-lookup"><span data-stu-id="fa4f5-162">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="fa4f5-163">每個[OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project)， [HTTP 嚴格的傳輸安全性 (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet)是透過使用特殊的回應標頭的 web 應用程式所指定的選擇加入的安全性增強功能。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-163">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="fa4f5-164">一旦支援的瀏覽器收到此標頭該瀏覽器將會避免任何透過 HTTP 傳送到指定的網域進行通訊，並改為將透過 HTTPS 傳送的所有通訊。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-164">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="fa4f5-165">它也會防止 HTTPS 的點選連結的瀏覽器的提示。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-165">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="fa4f5-166">2.1 或更新版本的 ASP.NET Core 實作與 HSTS`UseHsts`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-166">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="fa4f5-167">下列程式碼會呼叫`UseHsts`應用程式不在[開發模式](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="fa4f5-167">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="fa4f5-168">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="fa4f5-168">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="fa4f5-169">`UseHsts` 不建議在開發因為 HSTS 標頭是高度可快取的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-169">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="fa4f5-170">根據預設，UseHsts 排除本機回送位址。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-170">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="fa4f5-171">下列程式碼範例：</span><span class="sxs-lookup"><span data-stu-id="fa4f5-171">The following code:</span></span>

<span data-ttu-id="fa4f5-172">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="fa4f5-172">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="fa4f5-173">設定 Strict 傳輸安全性標頭的預先載入的參數。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-173">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="fa4f5-174">預先載入不屬於[RFC HSTS 規格](https://tools.ietf.org/html/rfc6797)，但是要預先載入 HSTS 上全新安裝的站台的網頁瀏覽器支援。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-174">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="fa4f5-175">請參閱 [https://hstspreload.org/](https://hstspreload.org/) 以取得詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-175">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="fa4f5-176">可讓[includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2)，這會套用到主機的子網域的 HSTS 原則。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-176">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="fa4f5-177">明確設定為 60 天的 Strict 傳輸安全性標頭的保留時間上限參數。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-177">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="fa4f5-178">如果沒有設定，預設值為 30 天。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-178">If not set, defaults to 30 days.</span></span> <span data-ttu-id="fa4f5-179">請參閱[保留時間上限指示詞](https://tools.ietf.org/html/rfc6797#section-6.1.1)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-179">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="fa4f5-180">新增`example.com`的主機，以排除清單。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-180">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="fa4f5-181">`UseHsts` 排除下列回送主機：</span><span class="sxs-lookup"><span data-stu-id="fa4f5-181">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="fa4f5-182">`localhost` : IPv4 回送位址。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-182">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="fa4f5-183">`127.0.0.1` : IPv4 回送位址。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-183">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="fa4f5-184">`[::1]` : IPv6 回送位址。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-184">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="fa4f5-185">上述範例顯示如何新增其他主機。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-185">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="fa4f5-186">選擇不使用 HTTPS 的專案建立</span><span class="sxs-lookup"><span data-stu-id="fa4f5-186">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="fa4f5-187">啟用 ASP.NET Core 2.1 或更新版本的 web 應用程式範本 （從 Visual Studio 或 dotnet 命令列） [HTTPS 的重新導向](#require)和[HSTS](#hsts)。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-187">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="fa4f5-188">對於不需要 HTTPS 的部署，您可以選擇不使用的 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-188">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="fa4f5-189">例如，不需要其中 HTTPS 處理外部在邊緣，每個節點使用 HTTPS 的某些後端服務。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-189">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="fa4f5-190">若要退出 HTTPS:</span><span class="sxs-lookup"><span data-stu-id="fa4f5-190">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fa4f5-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fa4f5-191">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="fa4f5-192">取消核取**設定以進行 HTTPS**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-192">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![實體圖表](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="fa4f5-194">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="fa4f5-194">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="fa4f5-195">使用 `--no-https` 選項。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-195">Use the `--no-https` option.</span></span> <span data-ttu-id="fa4f5-196">例如</span><span class="sxs-lookup"><span data-stu-id="fa4f5-196">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="fa4f5-198">安裝 Docker 的開發人員憑證</span><span class="sxs-lookup"><span data-stu-id="fa4f5-198">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="fa4f5-199">請參閱[此 GitHub 問題](https://github.com/aspnet/Docs/issues/6199)。</span><span class="sxs-lookup"><span data-stu-id="fa4f5-199">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
