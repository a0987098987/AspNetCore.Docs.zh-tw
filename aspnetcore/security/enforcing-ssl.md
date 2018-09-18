---
title: 強制使用 ASP.NET Core 中的 HTTPS
author: rick-anderson
description: 示範如何要求 HTTPS/TLS 中的 ASP.NET Core web 應用程式。
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 06ff89d30fb3586c69274cc7cb3e6c75065b098a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011322"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="7a872-103">強制使用 ASP.NET Core 中的 HTTPS</span><span class="sxs-lookup"><span data-stu-id="7a872-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="7a872-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7a872-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7a872-105">本文件說明如何：</span><span class="sxs-lookup"><span data-stu-id="7a872-105">This document shows how to:</span></span>

* <span data-ttu-id="7a872-106">需要 HTTPS 進行的所有要求。</span><span class="sxs-lookup"><span data-stu-id="7a872-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="7a872-107">將所有 HTTP 要求重新都導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="7a872-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="7a872-108">沒有可用的 API 可以防止用戶端第一次要求中傳送機密資料。</span><span class="sxs-lookup"><span data-stu-id="7a872-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="7a872-109">請勿**未**使用[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute)接收機密資訊的 Web Api 上。</span><span class="sxs-lookup"><span data-stu-id="7a872-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="7a872-110">`RequireHttpsAttribute` 若要從 HTTP 至 HTTPS 的瀏覽器重新導向，會使用 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="7a872-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="7a872-111">API 用戶端可能不了解，或是遵循從 HTTP 重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="7a872-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="7a872-112">此類用戶端可能會透過 HTTP 傳送資訊。</span><span class="sxs-lookup"><span data-stu-id="7a872-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="7a872-113">Web Api 應執行下列之一：</span><span class="sxs-lookup"><span data-stu-id="7a872-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="7a872-114">不在 HTTP 上接聽。</span><span class="sxs-lookup"><span data-stu-id="7a872-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="7a872-115">關閉與狀態碼 400 （不正確的要求） 的連線，並不會提供要求。</span><span class="sxs-lookup"><span data-stu-id="7a872-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="7a872-116">需要 HTTPS</span><span class="sxs-lookup"><span data-stu-id="7a872-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="7a872-117">我們建議您所有的生產環境 ASP.NET Core web 應用程式呼叫：</span><span class="sxs-lookup"><span data-stu-id="7a872-117">We recommend all production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="7a872-118">HTTPS 重新導向中介軟體 ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) 到所有的 HTTP 要求重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="7a872-118">The HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="7a872-119">[UseHsts](#hsts)，HTTP Strict Transport 安全性通訊協定 (HSTS)。</span><span class="sxs-lookup"><span data-stu-id="7a872-119">[UseHsts](#hsts), HTTP Strict Transport Security Protocol (HSTS).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="7a872-120">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="7a872-120">UseHttpsRedirection</span></span>

<span data-ttu-id="7a872-121">下列程式碼會呼叫`UseHttpsRedirection`在`Startup`類別：</span><span class="sxs-lookup"><span data-stu-id="7a872-121">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="7a872-122">上述反白顯示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="7a872-122">The preceding highlighted code:</span></span>

* <span data-ttu-id="7a872-123">使用預設[HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`)。</span><span class="sxs-lookup"><span data-stu-id="7a872-123">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span></span>
* <span data-ttu-id="7a872-124">使用預設[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) 但覆寫`ASPNETCORE_HTTPS_PORT`環境變數或[IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)。</span><span class="sxs-lookup"><span data-stu-id="7a872-124">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

> [!WARNING] 
><span data-ttu-id="7a872-125">連接埠必須是適用於中介軟體重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="7a872-125">A port must be available for the middleware to redirect to HTTPS.</span></span> <span data-ttu-id="7a872-126">如果沒有連接埠可用，則不會重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="7a872-126">If no port is available, redirection to HTTPS does not occur.</span></span> <span data-ttu-id="7a872-127">HTTPS 連接埠可以指定任何下列設定：</span><span class="sxs-lookup"><span data-stu-id="7a872-127">The HTTPS port can be specified by any of the following setting:</span></span>
> 
>* `HttpsRedirectionOptions.HttpsPort` 
>* <span data-ttu-id="7a872-128">`ASPNETCORE_HTTPS_PORT`環境變數。</span><span class="sxs-lookup"><span data-stu-id="7a872-128">The `ASPNETCORE_HTTPS_PORT` environment variable.</span></span> 
>* <span data-ttu-id="7a872-129">在開發中，在 HTTPS url *launchsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="7a872-129">In development, an HTTPS url in *launchsettings.json*.</span></span> 
>* <span data-ttu-id="7a872-130">直接在 Kestrel 或 HttpSys 上設定 HTTPS url。</span><span class="sxs-lookup"><span data-stu-id="7a872-130">An HTTPS url configured directly on Kestrel or HttpSys.</span></span> 

<span data-ttu-id="7a872-131">下列醒目提示程式碼會呼叫[AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection)設定中介軟體選項：</span><span class="sxs-lookup"><span data-stu-id="7a872-131">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="7a872-132">呼叫`AddHttpsRedirection`時，才需要變更的值` HttpsPort`或` RedirectStatusCode`;</span><span class="sxs-lookup"><span data-stu-id="7a872-132">Calling `AddHttpsRedirection` is only necessary to change the values of ` HttpsPort` or ` RedirectStatusCode`;</span></span>

<span data-ttu-id="7a872-133">上述反白顯示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="7a872-133">The preceding highlighted code:</span></span>

* <span data-ttu-id="7a872-134">設定組[HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode)到`Status307TemporaryRedirect`，這是預設值。</span><span class="sxs-lookup"><span data-stu-id="7a872-134">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span>
* <span data-ttu-id="7a872-135">設定 HTTPS 連接埠為 5001。</span><span class="sxs-lookup"><span data-stu-id="7a872-135">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="7a872-136">預設值為 443。</span><span class="sxs-lookup"><span data-stu-id="7a872-136">The default value is 443.</span></span>

<span data-ttu-id="7a872-137">下列機制會自動設定連接埠：</span><span class="sxs-lookup"><span data-stu-id="7a872-137">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="7a872-138">中介軟體可以探索透過連接埠[IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)當符合下列條件：</span><span class="sxs-lookup"><span data-stu-id="7a872-138">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>

   * <span data-ttu-id="7a872-139">Kestrel 或 HTTP.sys 可直接使用 HTTPS 端點 （也適用於 Visual Studio Code 的偵錯工具執行應用程式）。</span><span class="sxs-lookup"><span data-stu-id="7a872-139">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
   * <span data-ttu-id="7a872-140">只有**一個 HTTPS 連接埠**應用程式使用。</span><span class="sxs-lookup"><span data-stu-id="7a872-140">Only **one HTTPS port** is used by the app.</span></span>

* <span data-ttu-id="7a872-141">使用 visual Studio:</span><span class="sxs-lookup"><span data-stu-id="7a872-141">Visual Studio is used:</span></span>
   * <span data-ttu-id="7a872-142">IIS Express 已啟用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="7a872-142">IIS Express has HTTPS enabled.</span></span>
   * <span data-ttu-id="7a872-143">*launchSettings.json*設定`sslPort`適用於 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="7a872-143">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="7a872-144">應用程式執行 （例如 IIS、 IIS Express），在反向 proxy 後方時`IServerAddressesFeature`無法使用。</span><span class="sxs-lookup"><span data-stu-id="7a872-144">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="7a872-145">您必須手動設定連接埠。</span><span class="sxs-lookup"><span data-stu-id="7a872-145">The port must be manually configured.</span></span> <span data-ttu-id="7a872-146">當未設定連接埠時，不是重新導向要求。</span><span class="sxs-lookup"><span data-stu-id="7a872-146">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="7a872-147">可以藉由設定設定的連接埠[https_port Web 主機組態設定](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="7a872-147">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="7a872-148">**索引鍵**: https_port</span><span class="sxs-lookup"><span data-stu-id="7a872-148">**Key**: https_port</span></span>  
<span data-ttu-id="7a872-149">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="7a872-149">**Type**: *string*</span></span>  
<span data-ttu-id="7a872-150">**預設**： 未設定預設值。</span><span class="sxs-lookup"><span data-stu-id="7a872-150">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="7a872-151">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="7a872-151">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="7a872-152">**環境變數**: `<PREFIX_>HTTPS_PORT` (前置詞是`ASPNETCORE_`使用 Web 主機時。)</span><span class="sxs-lookup"><span data-stu-id="7a872-152">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="7a872-153">可以間接設定的連接埠，藉由設定 URL`ASPNETCORE_URLS`環境變數。</span><span class="sxs-lookup"><span data-stu-id="7a872-153">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="7a872-154">環境變數設定的伺服器，，，然後在中介軟體間接探索透過 HTTPS 連接埠`IServerAddressesFeature`。</span><span class="sxs-lookup"><span data-stu-id="7a872-154">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="7a872-155">如果已設定任何連接埠：</span><span class="sxs-lookup"><span data-stu-id="7a872-155">If no port is set:</span></span>

* <span data-ttu-id="7a872-156">要求未重新導向。</span><span class="sxs-lookup"><span data-stu-id="7a872-156">Requests aren't redirected.</span></span>
* <span data-ttu-id="7a872-157">中介軟體會記錄警告 「 無法判定重新導向的 https 連接埠。 」</span><span class="sxs-lookup"><span data-stu-id="7a872-157">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

> [!NOTE]
> <span data-ttu-id="7a872-158">除了使用 HTTPS 重新導向中介軟體 (`UseHttpsRedirection`) 是使用 URL 重寫中介軟體 (`AddRedirectToHttps`)。</span><span class="sxs-lookup"><span data-stu-id="7a872-158">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="7a872-159">`AddRedirectToHttps` 也可以設定的狀態碼和連接埠重新導向為執行時。</span><span class="sxs-lookup"><span data-stu-id="7a872-159">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="7a872-160">如需詳細資訊，請參閱 < [URL 重寫中介軟體](xref:fundamentals/url-rewriting)。</span><span class="sxs-lookup"><span data-stu-id="7a872-160">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="7a872-161">當重新導向至 HTTPS，而不需要額外的重新導向規則，我們建議使用 HTTPS 重新導向中介軟體 (`UseHttpsRedirection`) 本主題中所述。</span><span class="sxs-lookup"><span data-stu-id="7a872-161">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="7a872-162">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute)用來要求 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="7a872-162">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="7a872-163">`[RequireHttpsAttribute]` 可以裝飾控制器或方法，或可以全域套用。</span><span class="sxs-lookup"><span data-stu-id="7a872-163">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="7a872-164">若要全域套用的屬性，新增下列程式碼`ConfigureServices`在`Startup`:</span><span class="sxs-lookup"><span data-stu-id="7a872-164">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="7a872-165">上述反白顯示的程式碼會要求所有要求都使用`HTTPS`; 因此，HTTP 要求會被忽略。</span><span class="sxs-lookup"><span data-stu-id="7a872-165">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="7a872-166">而下列反白顯示的程式碼會將所有 HTTP 要求都重新導向至 HTTPS:</span><span class="sxs-lookup"><span data-stu-id="7a872-166">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="7a872-167">如需詳細資訊，請參閱 < [URL 重寫中介軟體](xref:fundamentals/url-rewriting)。</span><span class="sxs-lookup"><span data-stu-id="7a872-167">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="7a872-168">中介軟體也允許應用程式執行時重新導向設定的狀態碼或狀態碼和連接埠。</span><span class="sxs-lookup"><span data-stu-id="7a872-168">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="7a872-169">全域使用 HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) 是安全性最佳作法。</span><span class="sxs-lookup"><span data-stu-id="7a872-169">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="7a872-170">將`[RequireHttps]`屬性套用至所有控制器，不會比全域使用 HTTPS 來的安全。</span><span class="sxs-lookup"><span data-stu-id="7a872-170">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="7a872-171">您無法保證`[RequireHttps]`新增新的控制器和 Razor 頁面時，屬性會套用。</span><span class="sxs-lookup"><span data-stu-id="7a872-171">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="7a872-172">HTTP Strict Transport 安全性通訊協定 (HSTS)</span><span class="sxs-lookup"><span data-stu-id="7a872-172">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="7a872-173">每個[OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project)， [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet)是透過回應標頭使用的 web 應用程式所指定的選擇加入的安全性增強功能。</span><span class="sxs-lookup"><span data-stu-id="7a872-173">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="7a872-174">當[瀏覽器支援 HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)收到此標頭：</span><span class="sxs-lookup"><span data-stu-id="7a872-174">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="7a872-175">瀏覽器會儲存可防止傳送的任何通訊透過 HTTP 的定義域的組態。</span><span class="sxs-lookup"><span data-stu-id="7a872-175">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="7a872-176">瀏覽器會強制透過 HTTPS 的所有通訊。</span><span class="sxs-lookup"><span data-stu-id="7a872-176">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="7a872-177">瀏覽器會防止使用者使用不受信任或不正確的憑證。</span><span class="sxs-lookup"><span data-stu-id="7a872-177">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="7a872-178">瀏覽器會停用允許使用者暫時信任此種憑證的提示。</span><span class="sxs-lookup"><span data-stu-id="7a872-178">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="7a872-179">因為 HSTS 會強制執行用戶端就會有一些限制：</span><span class="sxs-lookup"><span data-stu-id="7a872-179">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="7a872-180">用戶端必須支援 HSTS。</span><span class="sxs-lookup"><span data-stu-id="7a872-180">The client must support HSTS.</span></span>
* <span data-ttu-id="7a872-181">HSTS 需要至少一個成功的 HTTPS 要求建立 HSTS 原則。</span><span class="sxs-lookup"><span data-stu-id="7a872-181">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="7a872-182">應用程式必須檢查每個 HTTP 要求並重新導向或拒絕的 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="7a872-182">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="7a872-183">ASP.NET Core 2.1 或更新版本會實作 HSTS 與`UseHsts`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="7a872-183">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="7a872-184">下列程式碼會呼叫`UseHsts`應用程式不在[開發模式](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="7a872-184">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="7a872-185">`UseHsts` 不建議在開發過程中因為 HSTS 設定高度快取瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="7a872-185">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="7a872-186">根據預設，`UseHsts`排除本機回送位址。</span><span class="sxs-lookup"><span data-stu-id="7a872-186">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="7a872-187">針對生產環境中實作 HTTPS 第一次，初始 HSTS 將值設定為較小的值。</span><span class="sxs-lookup"><span data-stu-id="7a872-187">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="7a872-188">從設定值時數不超過一天的萬一您需要還原為 HTTP，HTTPS 基礎結構。</span><span class="sxs-lookup"><span data-stu-id="7a872-188">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="7a872-189">確定在 HTTPS 設定的持續性後，增加 HSTS 最大壽命值;常用的值為一年。</span><span class="sxs-lookup"><span data-stu-id="7a872-189">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="7a872-190">下列程式碼範例：</span><span class="sxs-lookup"><span data-stu-id="7a872-190">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="7a872-191">設定 Strict 傳輸安全性標頭的預先載入的參數。</span><span class="sxs-lookup"><span data-stu-id="7a872-191">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="7a872-192">預先載入不屬於[RFC HSTS 規格](https://tools.ietf.org/html/rfc6797)，但要預先載入 HSTS 上全新安裝的站台的網頁瀏覽器支援。</span><span class="sxs-lookup"><span data-stu-id="7a872-192">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="7a872-193">請參閱 [https://hstspreload.org/](https://hstspreload.org/) 以取得詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="7a872-193">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="7a872-194">可讓[includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2)，套用 HSTS 原則來裝載子網域。</span><span class="sxs-lookup"><span data-stu-id="7a872-194">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="7a872-195">明確設定為 60 天的 Strict 傳輸安全性標頭的最大壽命參數。</span><span class="sxs-lookup"><span data-stu-id="7a872-195">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="7a872-196">如果未設定，預設值為 30 天。</span><span class="sxs-lookup"><span data-stu-id="7a872-196">If not set, defaults to 30 days.</span></span> <span data-ttu-id="7a872-197">請參閱[最大壽命指示詞](https://tools.ietf.org/html/rfc6797#section-6.1.1)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="7a872-197">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="7a872-198">新增`example.com`的主機，以排除清單。</span><span class="sxs-lookup"><span data-stu-id="7a872-198">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="7a872-199">`UseHsts` 排除下列 「 回送 」 主控件：</span><span class="sxs-lookup"><span data-stu-id="7a872-199">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="7a872-200">`localhost` : IPv4 回送位址。</span><span class="sxs-lookup"><span data-stu-id="7a872-200">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="7a872-201">`127.0.0.1` : IPv4 回送位址。</span><span class="sxs-lookup"><span data-stu-id="7a872-201">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="7a872-202">`[::1]` : IPv6 回送位址。</span><span class="sxs-lookup"><span data-stu-id="7a872-202">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="7a872-203">上述範例顯示如何新增其他主機。</span><span class="sxs-lookup"><span data-stu-id="7a872-203">The preceding example shows how to add additional hosts.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="7a872-204">選擇退出的 HTTPS 上建立專案</span><span class="sxs-lookup"><span data-stu-id="7a872-204">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="7a872-205">（從 Visual Studio 或 dotnet 命令列） 的 ASP.NET Core 2.1 或更新版本的 web 應用程式範本可讓[HTTPS 重新導向](#require)並[HSTS](#hsts)。</span><span class="sxs-lookup"><span data-stu-id="7a872-205">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="7a872-206">對於不需要 HTTPS 的部署，您可以選擇退出的 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="7a872-206">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="7a872-207">比方說，不需要其中 HTTPS 處理外部邊緣，每個節點上使用 HTTPS 的某些後端服務。</span><span class="sxs-lookup"><span data-stu-id="7a872-207">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node isn't needed.</span></span>

<span data-ttu-id="7a872-208">若要退出 HTTPS:</span><span class="sxs-lookup"><span data-stu-id="7a872-208">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7a872-209">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7a872-209">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="7a872-210">取消核取**設定為使用 HTTPS**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="7a872-210">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![實體圖表](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7a872-212">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7a872-212">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="7a872-213">使用 `--no-https` 選項。</span><span class="sxs-lookup"><span data-stu-id="7a872-213">Use the `--no-https` option.</span></span> <span data-ttu-id="7a872-214">例如</span><span class="sxs-lookup"><span data-stu-id="7a872-214">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="7a872-215">如何設定適用於 Docker 的開發人員憑證</span><span class="sxs-lookup"><span data-stu-id="7a872-215">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="7a872-216">請參閱[此 GitHub 問題](https://github.com/aspnet/Docs/issues/6199)。</span><span class="sxs-lookup"><span data-stu-id="7a872-216">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="7a872-217">其他資訊</span><span class="sxs-lookup"><span data-stu-id="7a872-217">Additional information</span></span>

* [<span data-ttu-id="7a872-218">OWASP HSTS 瀏覽器支援</span><span class="sxs-lookup"><span data-stu-id="7a872-218">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
