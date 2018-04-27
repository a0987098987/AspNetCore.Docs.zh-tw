---
title: 強制執行中的 ASP.NET Core HTTPS
author: rick-anderson
description: 示範如何要求 HTTPS/TLS 中 ASP.NET Core web 應用程式。
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 0509bebe430c6ba213031a2cb7cb91bb7a39566d
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2018
---
# <a name="enforce-https-in-an-aspnet-core"></a><span data-ttu-id="83a6c-103">強制執行中的 ASP.NET Core HTTPS</span><span class="sxs-lookup"><span data-stu-id="83a6c-103">Enforce HTTPS in an ASP.NET Core</span></span>

<span data-ttu-id="83a6c-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="83a6c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="83a6c-105">本文件說明如何：</span><span class="sxs-lookup"><span data-stu-id="83a6c-105">This document shows how to:</span></span>

- <span data-ttu-id="83a6c-106">所有要求需要 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="83a6c-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="83a6c-107">將所有 HTTP 要求重新都導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="83a6c-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="83a6c-108">請勿**不**使用`RequireHttpsAttribute`上接收機密資訊的 Web Api。</span><span class="sxs-lookup"><span data-stu-id="83a6c-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="83a6c-109">`RequireHttpsAttribute` 從 HTTP 至 HTTPS 的瀏覽器重新導向會使用 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="83a6c-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="83a6c-110">API 用戶端可能不了解，或是遵循從 HTTP 重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="83a6c-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="83a6c-111">這類用戶端可能會透過 HTTP 傳送資訊。</span><span class="sxs-lookup"><span data-stu-id="83a6c-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="83a6c-112">Web 應用程式開發介面應執行下列之一：</span><span class="sxs-lookup"><span data-stu-id="83a6c-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="83a6c-113">不在 HTTP 上接聽。</span><span class="sxs-lookup"><span data-stu-id="83a6c-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="83a6c-114">關閉與狀態碼 400 （不正確的要求） 的連線，並不會處理要求。</span><span class="sxs-lookup"><span data-stu-id="83a6c-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="83a6c-115">需要 HTTPS</span><span class="sxs-lookup"><span data-stu-id="83a6c-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE[](~/includes/2.1.md)]

<span data-ttu-id="83a6c-117">我們建議所有 ASP.NET Core web 應用程式呼叫`UseHttpsRedirection`將所有 HTTP 要求重新都導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="83a6c-117">We recommend all ASP.NET Core web apps call `UseHttpsRedirection` to redirect all HTTP requests to HTTPS.</span></span> <span data-ttu-id="83a6c-118">如果`UseHsts`稱為應用程式，並在它之前必須先呼叫`UseHttpsRedirection`。</span><span class="sxs-lookup"><span data-stu-id="83a6c-118">If `UseHsts` is called in the app, it must be called before `UseHttpsRedirection`.</span></span>

<span data-ttu-id="83a6c-119">下列程式碼會呼叫`UseHttpsRedirection`中`Startup`類別：</span><span class="sxs-lookup"><span data-stu-id="83a6c-119">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="83a6c-120">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="83a6c-120">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>


<span data-ttu-id="83a6c-121">下列程式碼範例：</span><span class="sxs-lookup"><span data-stu-id="83a6c-121">The following code:</span></span>

<span data-ttu-id="83a6c-122">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="83a6c-122">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

* <span data-ttu-id="83a6c-123">設定`RedirectStatusCode`。</span><span class="sxs-lookup"><span data-stu-id="83a6c-123">Sets `RedirectStatusCode`.</span></span>
* <span data-ttu-id="83a6c-124">5001 設定 HTTPS 連接埠。</span><span class="sxs-lookup"><span data-stu-id="83a6c-124">Sets the HTTPS port to 5001.</span></span>

::: moniker-end


::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="83a6c-125">[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute)用來要求 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="83a6c-125">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="83a6c-126">`[RequireHttpsAttribute]` 可以裝飾控制器或方法，或可以全域套用。</span><span class="sxs-lookup"><span data-stu-id="83a6c-126">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="83a6c-127">若要全域套用的屬性，加入下列程式碼加入`ConfigureServices`中`Startup`:</span><span class="sxs-lookup"><span data-stu-id="83a6c-127">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="83a6c-128">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="83a6c-128">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="83a6c-129">上述的反白顯示程式碼需要的所有要求都使用`HTTPS`; 因此，HTTP 要求會被忽略。</span><span class="sxs-lookup"><span data-stu-id="83a6c-129">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="83a6c-130">而下列反白顯示的程式碼會將所有 HTTP 要求都重新導向至 HTTPS:</span><span class="sxs-lookup"><span data-stu-id="83a6c-130">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="83a6c-131">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="83a6c-131">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="83a6c-132">如需詳細資訊，請參閱[URL 重寫中介軟體](xref:fundamentals/url-rewriting)。</span><span class="sxs-lookup"><span data-stu-id="83a6c-132">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="83a6c-133">全域使用 HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) 是安全性最佳作法。</span><span class="sxs-lookup"><span data-stu-id="83a6c-133">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="83a6c-134">將`[RequireHttps]`屬性套用至所有控制器，不會比全域使用 HTTPS 來的安全。</span><span class="sxs-lookup"><span data-stu-id="83a6c-134">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="83a6c-135">您無法保證`[RequireHttps]`加入新的控制器和 Razor 頁面時，屬性會套用。</span><span class="sxs-lookup"><span data-stu-id="83a6c-135">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="83a6c-136">HTTP 嚴格的傳輸安全性通訊協定 (HSTS)</span><span class="sxs-lookup"><span data-stu-id="83a6c-136">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="83a6c-137">每個[OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project)， [HTTP 嚴格的傳輸安全性 (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet)是透過使用特殊的回應標頭的 web 應用程式所指定的選擇加入的安全性增強功能。</span><span class="sxs-lookup"><span data-stu-id="83a6c-137">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="83a6c-138">一旦支援的瀏覽器收到此標頭該瀏覽器將會避免任何透過 HTTP 傳送到指定的網域進行通訊，並改為將透過 HTTPS 傳送的所有通訊。</span><span class="sxs-lookup"><span data-stu-id="83a6c-138">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="83a6c-139">它也會防止 HTTPS 的點選連結的瀏覽器的提示。</span><span class="sxs-lookup"><span data-stu-id="83a6c-139">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="83a6c-140">ASP.NET 核心 2.1 preview 1 或更新版本會實作與 HSTS`UseHsts`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="83a6c-140">ASP.NET Core 2.1 preview1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="83a6c-141">下列程式碼會呼叫`UseHsts`應用程式不在[開發模式](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="83a6c-141">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="83a6c-142">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="83a6c-142">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="83a6c-143">`UseHsts` 不建議在開發因為 HSTS 標頭是高度可快取的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="83a6c-143">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="83a6c-144">根據預設，UseHsts 排除本機回送位址。</span><span class="sxs-lookup"><span data-stu-id="83a6c-144">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="83a6c-145">下列程式碼範例：</span><span class="sxs-lookup"><span data-stu-id="83a6c-145">The following code:</span></span>

<span data-ttu-id="83a6c-146">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="83a6c-146">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="83a6c-147">設定 Strict 傳輸安全性標頭的預先載入的參數。</span><span class="sxs-lookup"><span data-stu-id="83a6c-147">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="83a6c-148">預先載入不屬於[RFC HSTS 規格](https://tools.ietf.org/html/rfc6797)，但是要預先載入 HSTS 上全新安裝的站台的網頁瀏覽器支援。</span><span class="sxs-lookup"><span data-stu-id="83a6c-148">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="83a6c-149">請參閱 [https://hstspreload.org/](https://hstspreload.org/) 以取得詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="83a6c-149">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="83a6c-150">可讓[includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2)，這會套用到主機的子網域的 HSTS 原則。</span><span class="sxs-lookup"><span data-stu-id="83a6c-150">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="83a6c-151">明確設定為 60 天的 Strict 傳輸安全性標頭的保留時間上限參數。</span><span class="sxs-lookup"><span data-stu-id="83a6c-151">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="83a6c-152">如果沒有設定，預設值為 30 天。</span><span class="sxs-lookup"><span data-stu-id="83a6c-152">If not set, defaults to 30 days.</span></span> <span data-ttu-id="83a6c-153">請參閱[保留時間上限指示詞](https://tools.ietf.org/html/rfc6797#section-6.1.1)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="83a6c-153">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="83a6c-154">新增`example.com`的主機，以排除清單。</span><span class="sxs-lookup"><span data-stu-id="83a6c-154">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="83a6c-155">`UseHsts` 排除下列回送主機：</span><span class="sxs-lookup"><span data-stu-id="83a6c-155">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="83a6c-156">`localhost` : IPv4 回送位址。</span><span class="sxs-lookup"><span data-stu-id="83a6c-156">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="83a6c-157">`127.0.0.1` : IPv4 回送位址。</span><span class="sxs-lookup"><span data-stu-id="83a6c-157">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="83a6c-158">`[::1]` : IPv6 回送位址。</span><span class="sxs-lookup"><span data-stu-id="83a6c-158">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="83a6c-159">上述範例顯示如何新增其他主機。</span><span class="sxs-lookup"><span data-stu-id="83a6c-159">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end


::: moniker range=">= aspnetcore-2.1"
<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="83a6c-160">選擇不使用 HTTPS 的專案建立</span><span class="sxs-lookup"><span data-stu-id="83a6c-160">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="83a6c-161">ASP.NET 核心 2.1 和更新版本 （從 Visual Studio 或 dotnet 命令列） 的 web 應用程式範本可讓[HTTPS 的重新導向](#require)和[HSTS](#hsts)。</span><span class="sxs-lookup"><span data-stu-id="83a6c-161">The ASP.NET Core 2.1 and later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="83a6c-162">對於不需要 HTTPS 的部署，您可以選擇不使用的 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="83a6c-162">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="83a6c-163">例如，不需要其中 HTTPS 處理外部在邊緣，每個節點使用 HTTPS 的某些後端服務。</span><span class="sxs-lookup"><span data-stu-id="83a6c-163">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="83a6c-164">若要退出 HTTPS:</span><span class="sxs-lookup"><span data-stu-id="83a6c-164">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="83a6c-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83a6c-165">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="83a6c-166">取消核取**設定以進行 HTTPS**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="83a6c-166">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![實體圖表](enforcing-ssl/_static/out.png)


#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="83a6c-168">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="83a6c-168">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="83a6c-169">使用 `--no-https` 選項。</span><span class="sxs-lookup"><span data-stu-id="83a6c-169">Use the `--no-https` option.</span></span> <span data-ttu-id="83a6c-170">例如</span><span class="sxs-lookup"><span data-stu-id="83a6c-170">For example</span></span>

```cli
dotnet new razor --no-https
```

------

::: moniker-end