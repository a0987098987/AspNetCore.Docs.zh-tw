---
title: "URL 重寫中 ASP.NET Core 中的介軟體"
author: guardrex
description: "簡介 URL 重寫，並將重新導向，其中包含有關如何在 ASP.NET Core 應用程式中使用 URL 重寫中介軟體的指示。"
keywords: "ASP.NET Core URL 重寫，URL 重寫 URL 重新導向 URL 重新導向中, 介軟體，apache_mod"
ms.author: riande
manager: wpickett
ms.date: 08/17/2017
ms.topic: article
ms.assetid: e6130638-c410-4161-9921-b658ce988bd1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/url-rewriting
ms.openlocfilehash: 735c53ab8a01d995c4945a95705ee9adb2fcdf02
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/25/2017
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="c6d97-104">URL 重寫中 ASP.NET Core 中的介軟體</span><span class="sxs-lookup"><span data-stu-id="c6d97-104">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="c6d97-105">由[Luke Latham](https://github.com/GuardRex)和[黃冠馬力](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="c6d97-105">By [Luke Latham](https://github.com/GuardRex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

[<span data-ttu-id="c6d97-106">檢視或下載範例程式碼</span><span class="sxs-lookup"><span data-stu-id="c6d97-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)

<span data-ttu-id="c6d97-107">URL 重寫是指修改的要求 Url 會根據一或多個預先定義的規則。</span><span class="sxs-lookup"><span data-stu-id="c6d97-107">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="c6d97-108">URL 重寫會建立資源位置和地址之間的抽象概念，讓不緊密連結的位置和地址。</span><span class="sxs-lookup"><span data-stu-id="c6d97-108">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="c6d97-109">有幾種的情況很重要 URL 重寫：</span><span class="sxs-lookup"><span data-stu-id="c6d97-109">There are several scenarios where URL rewriting is valuable:</span></span>
* <span data-ttu-id="c6d97-110">移動或取代伺服器資源暫時或永久地維持穩定的定位器，針對這些資源</span><span class="sxs-lookup"><span data-stu-id="c6d97-110">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources</span></span>
* <span data-ttu-id="c6d97-111">分割的要求處理跨不同的應用程式或跨區域的一個應用程式</span><span class="sxs-lookup"><span data-stu-id="c6d97-111">Splitting request processing across different apps or across areas of one app</span></span>
* <span data-ttu-id="c6d97-112">移除、 加入或重新組織的連入要求的 URL 區段</span><span class="sxs-lookup"><span data-stu-id="c6d97-112">Removing, adding, or reorganizing URL segments on incoming requests</span></span>
* <span data-ttu-id="c6d97-113">最佳化公用 Url 搜尋引擎最佳化 (SEO)</span><span class="sxs-lookup"><span data-stu-id="c6d97-113">Optimizing public URLs for Search Engine Optimization (SEO)</span></span>
* <span data-ttu-id="c6d97-114">允許使用易記的公用 Url，以協助使用者在預測他們會依照下列連結找到的內容</span><span class="sxs-lookup"><span data-stu-id="c6d97-114">Permitting the use of friendly public URLs to help people predict the content they will find by following a link</span></span>
* <span data-ttu-id="c6d97-115">將保護端點不安全的要求重新導向</span><span class="sxs-lookup"><span data-stu-id="c6d97-115">Redirecting insecure requests to secure endpoints</span></span>
* <span data-ttu-id="c6d97-116">防止 hotlinking 映像</span><span class="sxs-lookup"><span data-stu-id="c6d97-116">Preventing image hotlinking</span></span>

<span data-ttu-id="c6d97-117">您可以定義規則來變更數種方式的 URL，包括 regex、 Apache mod_rewrite 模組規則、 IIS 重寫模組規則和使用自訂規則的邏輯。</span><span class="sxs-lookup"><span data-stu-id="c6d97-117">You can define rules for changing the URL in several ways, including regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="c6d97-118">本文件介紹 URL 重寫有關如何在 ASP.NET Core 應用程式中使用 URL 重寫中介軟體的指示。</span><span class="sxs-lookup"><span data-stu-id="c6d97-118">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="c6d97-119">URL 重寫可能會降低應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="c6d97-119">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="c6d97-120">在可行的時候，您應該限制的數目與複雜度的規則。</span><span class="sxs-lookup"><span data-stu-id="c6d97-120">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="c6d97-121">重新導向 URL 和 URL 重寫</span><span class="sxs-lookup"><span data-stu-id="c6d97-121">URL redirect and URL rewrite</span></span>
<span data-ttu-id="c6d97-122">用語之間的差異*URL 重新導向*和*URL 重寫*可能看起來很難以察覺在第一次，但是擁有資源提供給用戶端重要的影響。</span><span class="sxs-lookup"><span data-stu-id="c6d97-122">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="c6d97-123">ASP.NET Core URL 重寫中介軟體都可以達成兩個需要。</span><span class="sxs-lookup"><span data-stu-id="c6d97-123">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="c6d97-124">A *URL 重新導向*是用戶端的作業，其中會指示用戶端在另一個位址存取資源。</span><span class="sxs-lookup"><span data-stu-id="c6d97-124">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="c6d97-125">這需要往返伺服器，並傳回至用戶端重新導向 URL 出現在瀏覽器的網址列時用戶端提出新要求的資源。</span><span class="sxs-lookup"><span data-stu-id="c6d97-125">This requires a round-trip to the server, and the redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> <span data-ttu-id="c6d97-126">如果`/resource`是*重新導向*至`/different-resource`，用戶端要求`/resource`，伺服器會回應，用戶端應該取得的資源和`/different-resource`狀態程式碼，表示重新導向為與暫時或永久的。</span><span class="sxs-lookup"><span data-stu-id="c6d97-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`, and the server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="c6d97-127">用戶端執行新的資源重新導向 URL 的要求。</span><span class="sxs-lookup"><span data-stu-id="c6d97-127">The client executes a new request for the resource at the redirect URL.</span></span>

![WebAPI 服務端點已暫時變更從版本 1 (v1) 為伺服器上的版本 2 (v2)。](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="c6d97-133">將要求重新導向至不同的 URL，當您指出是否永久或暫時重新導向。</span><span class="sxs-lookup"><span data-stu-id="c6d97-133">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="c6d97-134">301 （已永久移動） 狀態碼使用資源具有新的永久的 URL，而且您想以指示用戶端的所有未來的要求資源應該使用新的 URL。</span><span class="sxs-lookup"><span data-stu-id="c6d97-134">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="c6d97-135">*用戶端收到 301 狀態程式碼時，可能會快取的回應。*</span><span class="sxs-lookup"><span data-stu-id="c6d97-135">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="c6d97-136">302 （找到） 狀態碼會重新導向其中是暫存或通常主旨變更，使得用戶端不應儲存，並在未來重複使用重新導向 URL。</span><span class="sxs-lookup"><span data-stu-id="c6d97-136">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="c6d97-137">如需詳細資訊，請參閱[RFC 2616： 狀態碼定義](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)。</span><span class="sxs-lookup"><span data-stu-id="c6d97-137">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="c6d97-138">A *URL 重寫*是伺服器端作業，以提供不同的資源位址中的資源。</span><span class="sxs-lookup"><span data-stu-id="c6d97-138">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="c6d97-139">重寫 URL，並不需要往返伺服器。</span><span class="sxs-lookup"><span data-stu-id="c6d97-139">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="c6d97-140">重寫的 URL 不傳回至用戶端，並不會出現在瀏覽器的網址列。</span><span class="sxs-lookup"><span data-stu-id="c6d97-140">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="c6d97-141">當`/resource`是*重寫*至`/different-resource`，用戶端要求`/resource`，與伺服器*內部*擷取資源時`/different-resource`。</span><span class="sxs-lookup"><span data-stu-id="c6d97-141">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="c6d97-142">雖然用戶端可能會無法擷取在重寫 URL 資源，用戶端將不會收到通知時，它會建立其要求並接收回應的資源存在於重寫 URL。</span><span class="sxs-lookup"><span data-stu-id="c6d97-142">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![WebAPI 服務端點已從版本 1 (v1) 變更為伺服器上的版本 2 (v2)。](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="c6d97-147">URL 重寫的範例應用程式</span><span class="sxs-lookup"><span data-stu-id="c6d97-147">URL rewriting sample app</span></span>
<span data-ttu-id="c6d97-148">您可以瀏覽的 URL 重寫中介軟體功能[URL 重寫的範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)。</span><span class="sxs-lookup"><span data-stu-id="c6d97-148">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span></span> <span data-ttu-id="c6d97-149">應用程式適用重新寫入和重新導向規則，並顯示重寫或重新導向 URL。</span><span class="sxs-lookup"><span data-stu-id="c6d97-149">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="c6d97-150">使用 URL 重寫中介軟體的時機</span><span class="sxs-lookup"><span data-stu-id="c6d97-150">When to use URL Rewriting Middleware</span></span>
<span data-ttu-id="c6d97-151">當您無法使用，請使用 URL 重寫中介軟體[URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite)與 Windows Server 上的 IIS [Apache mod_rewrite 模組](https://httpd.apache.org/docs/2.4/rewrite/)Apache 在伺服器上， [URL 重寫 Nginx上](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)，或者您的應用程式裝載於[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys)(先前稱為[WebListener](xref:fundamentals/servers/weblistener))。</span><span class="sxs-lookup"><span data-stu-id="c6d97-151">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="c6d97-152">若要使用的伺服器為基礎的 URL 重寫技術 IIS、 Apache 或 Nginx 中的主要原因是中, 介軟體不支援這些模組的完整功能，和中介軟體的效能可能不符合模組。</span><span class="sxs-lookup"><span data-stu-id="c6d97-152">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="c6d97-153">不過，有一些功能，不使用 ASP.NET Core 專案，例如伺服器模組`IsFile`和`IsDirectory`IIS Rewrite module 的條件約束。</span><span class="sxs-lookup"><span data-stu-id="c6d97-153">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="c6d97-154">在這些情況下，請改為使用中介軟體。</span><span class="sxs-lookup"><span data-stu-id="c6d97-154">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="c6d97-155">Package</span><span class="sxs-lookup"><span data-stu-id="c6d97-155">Package</span></span>
<span data-ttu-id="c6d97-156">若要在專案中包含中介軟體，將參考加入[ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/)封裝。</span><span class="sxs-lookup"><span data-stu-id="c6d97-156">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="c6d97-157">這項功能是適用於 ASP.NET Core 1.1 版為目標的應用程式或更新版本。</span><span class="sxs-lookup"><span data-stu-id="c6d97-157">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="c6d97-158">擴充功能和選項</span><span class="sxs-lookup"><span data-stu-id="c6d97-158">Extension and options</span></span>
<span data-ttu-id="c6d97-159">建立您的 URL 重寫，並重新導向規則建立的執行個體`RewriteOptions`與每個規則的擴充方法的類別。</span><span class="sxs-lookup"><span data-stu-id="c6d97-159">Establish your URL rewrite and redirect rules by creating an instance of the `RewriteOptions` class with extension methods for each of your rules.</span></span> <span data-ttu-id="c6d97-160">鏈結多個規則，您想要處理它們的順序。</span><span class="sxs-lookup"><span data-stu-id="c6d97-160">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="c6d97-161">`RewriteOptions`將加入至要求管線傳入 URL 重寫中介軟體`app.UseRewriter(options);`。</span><span class="sxs-lookup"><span data-stu-id="c6d97-161">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c6d97-162">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c6d97-162">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c6d97-163">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="c6d97-163">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c6d97-164">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c6d97-164">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c6d97-165">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="c6d97-165">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1)]</span></span>

---

### <a name="url-redirect"></a><span data-ttu-id="c6d97-166">重新導向 URL</span><span class="sxs-lookup"><span data-stu-id="c6d97-166">URL redirect</span></span>
<span data-ttu-id="c6d97-167">使用`AddRedirect`將要求重新導向。</span><span class="sxs-lookup"><span data-stu-id="c6d97-167">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="c6d97-168">第一個參數會包含您的 regex 進行比對傳入 URL 的路徑上。</span><span class="sxs-lookup"><span data-stu-id="c6d97-168">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="c6d97-169">第二個參數是取代字串。</span><span class="sxs-lookup"><span data-stu-id="c6d97-169">The second parameter is the replacement string.</span></span> <span data-ttu-id="c6d97-170">第三個參數，如果有的話，指定的狀態碼。</span><span class="sxs-lookup"><span data-stu-id="c6d97-170">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="c6d97-171">如果您未指定的狀態碼，則預設為 302 （找到），表示已暫時移動或取代資源。</span><span class="sxs-lookup"><span data-stu-id="c6d97-171">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c6d97-172">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c6d97-172">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c6d97-173">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="c6d97-173">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=5)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c6d97-174">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c6d97-174">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c6d97-175">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="c6d97-175">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=2)]</span></span>

---

<span data-ttu-id="c6d97-176">使用開發人員工具啟用瀏覽器，提出要求的路徑範例應用程式`/redirect-rule/1234/5678`。</span><span class="sxs-lookup"><span data-stu-id="c6d97-176">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="c6d97-177">Regex 上相符的要求路徑`redirect-rule/(.*)`，而且路徑取代`/redirected/1234/5678`。</span><span class="sxs-lookup"><span data-stu-id="c6d97-177">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="c6d97-178">重新導向 URL 會傳送回用戶端以 302 （找到） 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="c6d97-178">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="c6d97-179">瀏覽器重新導向 URL，它會出現在瀏覽器的網址列在提出新要求。</span><span class="sxs-lookup"><span data-stu-id="c6d97-179">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="c6d97-180">範例應用程式中的任何規則不比對重新導向 URL，因為第二個要求收到回應 200 （確定），從應用程式，並將回應主體會顯示重新導向 URL。</span><span class="sxs-lookup"><span data-stu-id="c6d97-180">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="c6d97-181">URL 時對伺服器提出往返*重新導向*。</span><span class="sxs-lookup"><span data-stu-id="c6d97-181">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="c6d97-182">建立重新導向規則時，請務必謹慎。</span><span class="sxs-lookup"><span data-stu-id="c6d97-182">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="c6d97-183">重新導向規則會評估每個應用程式，其中包括後重新導向的要求。</span><span class="sxs-lookup"><span data-stu-id="c6d97-183">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="c6d97-184">很容易意外建立無限重新導向迴圈。</span><span class="sxs-lookup"><span data-stu-id="c6d97-184">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="c6d97-185">原始要求：`/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="c6d97-185">Original Request: `/redirect-rule/1234/5678`</span></span>

![瀏覽器視窗中使用追蹤的要求和回應的開發人員工具](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="c6d97-187">包含在括號內的運算式部份稱為*擷取群組*。</span><span class="sxs-lookup"><span data-stu-id="c6d97-187">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="c6d97-188">點 (`.`) 的運算式表示*比對任何字元*。</span><span class="sxs-lookup"><span data-stu-id="c6d97-188">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="c6d97-189">星號 (`*`) 表示*零或多次比對前一個字元*。</span><span class="sxs-lookup"><span data-stu-id="c6d97-189">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="c6d97-190">因此，URL 的最後兩個路徑區段`1234/5678`，擷取群組所擷取`(.*)`。</span><span class="sxs-lookup"><span data-stu-id="c6d97-190">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="c6d97-191">您在之後的要求 URL 中提供的任何值`redirect-rule/`此單一擷取群組所擷取。</span><span class="sxs-lookup"><span data-stu-id="c6d97-191">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="c6d97-192">取代字串中的擷取的群組會插入貨幣符號的字串 (`$`) 後面接著擷取的序號。</span><span class="sxs-lookup"><span data-stu-id="c6d97-192">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="c6d97-193">取得第一個擷取群組值`$1`、 與第二個`$2`，而且會持續在您的 regex 的擷取群組的順序。</span><span class="sxs-lookup"><span data-stu-id="c6d97-193">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="c6d97-194">只有一個擷取的群組中沒有重新導向規則 regex 範例應用程式，因此在取代字串中，這是只有一個插入的群組`$1`。</span><span class="sxs-lookup"><span data-stu-id="c6d97-194">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="c6d97-195">套用規則時，URL 會成為`/redirected/1234/5678`。</span><span class="sxs-lookup"><span data-stu-id="c6d97-195">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

<a name=url-redirect-to-secure-endpoint></a>
### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="c6d97-196">安全端點的 URL 重新導向</span><span class="sxs-lookup"><span data-stu-id="c6d97-196">URL redirect to a secure endpoint</span></span>
<span data-ttu-id="c6d97-197">使用`AddRedirectToHttps`將 HTTP 要求重新導向至相同的主機，並使用 HTTPS 的路徑 (`https://`)。</span><span class="sxs-lookup"><span data-stu-id="c6d97-197">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="c6d97-198">如果未提供的狀態碼中, 介軟體會預設為 302 （找到）。</span><span class="sxs-lookup"><span data-stu-id="c6d97-198">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="c6d97-199">如果未提供連接埠中, 介軟體會預設為`null`，這表示通訊協定變更為`https://`和用戶端會存取連接埠 443 上的資源。</span><span class="sxs-lookup"><span data-stu-id="c6d97-199">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="c6d97-200">此範例示範如何設定狀態碼 301 （已永久移動） 到，並將連接埠變更為 5001。</span><span class="sxs-lookup"><span data-stu-id="c6d97-200">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>
```csharp
var options = new RewriteOptions()
    .AddRedirectToHttps(301, 5001);

app.UseRewriter(options);
```
<span data-ttu-id="c6d97-201">使用`AddRedirectToHttpsPermanent`將不安全的要求重新導向至相同的主機，並使用安全的 HTTPS 通訊協定的路徑 (`https://`連接埠 443)。</span><span class="sxs-lookup"><span data-stu-id="c6d97-201">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="c6d97-202">中介軟體設定的狀態碼 301 （已永久移動）。</span><span class="sxs-lookup"><span data-stu-id="c6d97-202">The middleware sets the status code to 301 (Moved Permanently).</span></span>

<span data-ttu-id="c6d97-203">範例應用程式是否能夠示範如何使用`AddRedirectToHttps`或`AddRedirectToHttpsPermanent`。</span><span class="sxs-lookup"><span data-stu-id="c6d97-203">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="c6d97-204">Add 擴充方法，以`RewriteOptions`。</span><span class="sxs-lookup"><span data-stu-id="c6d97-204">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="c6d97-205">執行任何 URL 的應用程式會對不安全的要求。</span><span class="sxs-lookup"><span data-stu-id="c6d97-205">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="c6d97-206">關閉瀏覽器安全性警告的自我簽署的憑證不受信任。</span><span class="sxs-lookup"><span data-stu-id="c6d97-206">Dismiss the browser security warning that the self-signed certificate is untrusted.</span></span>

<span data-ttu-id="c6d97-207">原始要求使用`AddRedirectToHttps(301, 5001)`:`/secure`</span><span class="sxs-lookup"><span data-stu-id="c6d97-207">Original Request using `AddRedirectToHttps(301, 5001)`: `/secure`</span></span>

![瀏覽器視窗中使用追蹤的要求和回應的開發人員工具](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="c6d97-209">原始要求使用`AddRedirectToHttpsPermanent`:`/secure`</span><span class="sxs-lookup"><span data-stu-id="c6d97-209">Original Request using `AddRedirectToHttpsPermanent`: `/secure`</span></span>

![瀏覽器視窗中使用追蹤的要求和回應的開發人員工具](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="c6d97-211">URL 重寫</span><span class="sxs-lookup"><span data-stu-id="c6d97-211">URL rewrite</span></span>
<span data-ttu-id="c6d97-212">使用`AddRewrite`建立 Url 重寫規則。</span><span class="sxs-lookup"><span data-stu-id="c6d97-212">Use `AddRewrite` to create a rules for rewriting URLs.</span></span> <span data-ttu-id="c6d97-213">第一個參數會包含您的 regex 的比對連入的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="c6d97-213">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="c6d97-214">第二個參數是取代字串。</span><span class="sxs-lookup"><span data-stu-id="c6d97-214">The second parameter is the replacement string.</span></span> <span data-ttu-id="c6d97-215">第三個參數， `skipRemainingRules: {true|false}`，指示中介軟體，就不會略過其他重寫規則，如果套用目前的規則。</span><span class="sxs-lookup"><span data-stu-id="c6d97-215">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c6d97-216">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c6d97-216">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c6d97-217">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=6)]</span><span class="sxs-lookup"><span data-stu-id="c6d97-217">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=6)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c6d97-218">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c6d97-218">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c6d97-219">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="c6d97-219">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=3)]</span></span>

---

<span data-ttu-id="c6d97-220">原始要求：`/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="c6d97-220">Original Request: `/rewrite-rule/1234/5678`</span></span>

![瀏覽器視窗中使用追蹤要求和回應的開發人員工具](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="c6d97-222">您會注意到 regex 中第一件事是克拉記號 (`^`) 運算式的開頭。</span><span class="sxs-lookup"><span data-stu-id="c6d97-222">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="c6d97-223">這表示，比對會從 URL 路徑的開頭。</span><span class="sxs-lookup"><span data-stu-id="c6d97-223">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="c6d97-224">重新導向規則，在前面範例中`redirect-rule/(.*)`，在 regex 開始處沒有任何克拉記號; 因此，任何字元可能會在`redirect-rule/`之路徑的成功比對。</span><span class="sxs-lookup"><span data-stu-id="c6d97-224">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="c6d97-225">路徑</span><span class="sxs-lookup"><span data-stu-id="c6d97-225">Path</span></span>                               | <span data-ttu-id="c6d97-226">比對</span><span class="sxs-lookup"><span data-stu-id="c6d97-226">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="c6d97-227">是</span><span class="sxs-lookup"><span data-stu-id="c6d97-227">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="c6d97-228">是</span><span class="sxs-lookup"><span data-stu-id="c6d97-228">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="c6d97-229">是</span><span class="sxs-lookup"><span data-stu-id="c6d97-229">Yes</span></span>   |

<span data-ttu-id="c6d97-230">請重寫規則， `^rewrite-rule/(\d+)/(\d+)`，只比對路徑，開頭為`rewrite-rule/`。</span><span class="sxs-lookup"><span data-stu-id="c6d97-230">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="c6d97-231">請注意的差異比對下列重寫規則與上述的重新導向規則。</span><span class="sxs-lookup"><span data-stu-id="c6d97-231">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="c6d97-232">路徑</span><span class="sxs-lookup"><span data-stu-id="c6d97-232">Path</span></span>                              | <span data-ttu-id="c6d97-233">比對</span><span class="sxs-lookup"><span data-stu-id="c6d97-233">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="c6d97-234">是</span><span class="sxs-lookup"><span data-stu-id="c6d97-234">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="c6d97-235">否</span><span class="sxs-lookup"><span data-stu-id="c6d97-235">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="c6d97-236">否</span><span class="sxs-lookup"><span data-stu-id="c6d97-236">No</span></span>    |

<span data-ttu-id="c6d97-237">遵循`^rewrite-rule/`部份的運算式，包含兩個擷取群組， `(\d+)/(\d+)`。</span><span class="sxs-lookup"><span data-stu-id="c6d97-237">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="c6d97-238">`\d`表示*比對的數字 （數字）*。</span><span class="sxs-lookup"><span data-stu-id="c6d97-238">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="c6d97-239">加號 (`+`) 表示*符合一或多個前置字元*。</span><span class="sxs-lookup"><span data-stu-id="c6d97-239">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="c6d97-240">因此，此 URL 必須包含後面接著斜線後面接著另一個數字的數字。</span><span class="sxs-lookup"><span data-stu-id="c6d97-240">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="c6d97-241">這些群組會插入做為 URL 重寫的擷取`$1`和`$2`。</span><span class="sxs-lookup"><span data-stu-id="c6d97-241">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="c6d97-242">重寫規則的取代字串會將擷取的群組放入查詢字串。</span><span class="sxs-lookup"><span data-stu-id="c6d97-242">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="c6d97-243">要求的路徑的`/rewrite-rule/1234/5678`取得的資源重寫`/rewritten?var1=1234&var2=5678`。</span><span class="sxs-lookup"><span data-stu-id="c6d97-243">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="c6d97-244">如果查詢字串是原始要求上呈現，則會保留在重寫 URL 時。</span><span class="sxs-lookup"><span data-stu-id="c6d97-244">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="c6d97-245">沒有任何往返到伺服器，以取得資源。</span><span class="sxs-lookup"><span data-stu-id="c6d97-245">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="c6d97-246">如果資源存在，它具有讀取，並傳回 200 (OK) 狀態碼的用戶端。</span><span class="sxs-lookup"><span data-stu-id="c6d97-246">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="c6d97-247">因為用戶端未重新導向，則不會變更瀏覽器網址列中的 URL。</span><span class="sxs-lookup"><span data-stu-id="c6d97-247">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="c6d97-248">當用戶端是而言，URL 重寫永遠不會發生作業。</span><span class="sxs-lookup"><span data-stu-id="c6d97-248">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="c6d97-249">使用`skipRemainingRules: true`可能的話，因為比對規則是高度耗費資源的程序，並減少應用程式回應時間。</span><span class="sxs-lookup"><span data-stu-id="c6d97-249">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="c6d97-250">最快速的應用程式回應：</span><span class="sxs-lookup"><span data-stu-id="c6d97-250">For the fastest app response:</span></span>
> * <span data-ttu-id="c6d97-251">訂購至少經常比對規則通常比對規則重寫規則。</span><span class="sxs-lookup"><span data-stu-id="c6d97-251">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="c6d97-252">當出現相符，而且需要任何額外的規則處理時，略過剩餘的規則的處理。</span><span class="sxs-lookup"><span data-stu-id="c6d97-252">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="c6d97-253">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="c6d97-253">Apache mod_rewrite</span></span>
<span data-ttu-id="c6d97-254">適用於使用 Apache mod_rewrite 規則`AddApacheModRewrite`。</span><span class="sxs-lookup"><span data-stu-id="c6d97-254">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="c6d97-255">請確定規則檔與應用程式部署。</span><span class="sxs-lookup"><span data-stu-id="c6d97-255">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="c6d97-256">如需詳細資訊和 mod_rewrite 規則的範例，請參閱[Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)。</span><span class="sxs-lookup"><span data-stu-id="c6d97-256">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c6d97-257">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c6d97-257">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c6d97-258">A`StreamReader`用於讀取從規則*ApacheModRewrite.txt*規則檔案。</span><span class="sxs-lookup"><span data-stu-id="c6d97-258">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

<span data-ttu-id="c6d97-259">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=1,7)]</span><span class="sxs-lookup"><span data-stu-id="c6d97-259">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=1,7)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c6d97-260">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c6d97-260">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c6d97-261">第一個參數會採用`IFileProvider`，這會透過提供[相依性插入](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="c6d97-261">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="c6d97-262">`IHostingEnvironment`插入提供`ContentRootFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="c6d97-262">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="c6d97-263">第二個參數是您規則的檔案，也就是路徑*ApacheModRewrite.txt*範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="c6d97-263">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

<span data-ttu-id="c6d97-264">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=4)]</span><span class="sxs-lookup"><span data-stu-id="c6d97-264">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=4)]</span></span>

---

<span data-ttu-id="c6d97-265">範例應用程式將要求重新導向`/apache-mod-rules-redirect/(.\*)`至`/redirected?id=$1`。</span><span class="sxs-lookup"><span data-stu-id="c6d97-265">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="c6d97-266">回應狀態碼為 302 （找到）。</span><span class="sxs-lookup"><span data-stu-id="c6d97-266">The response status code is 302 (Found).</span></span>

[!code[Main](url-rewriting/samples/2.x/ApacheModRewrite.txt)]

<span data-ttu-id="c6d97-267">原始要求：`/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="c6d97-267">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![瀏覽器視窗中使用追蹤的要求和回應的開發人員工具](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a><span data-ttu-id="c6d97-269">支援的伺服器變數</span><span class="sxs-lookup"><span data-stu-id="c6d97-269">Supported server variables</span></span>
<span data-ttu-id="c6d97-270">中介軟體可支援下列 Apache mod_rewrite 伺服器變數：</span><span class="sxs-lookup"><span data-stu-id="c6d97-270">The middleware supports the following Apache mod_rewrite server variables:</span></span>
* <span data-ttu-id="c6d97-271">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="c6d97-271">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="c6d97-272">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="c6d97-272">HTTP_ACCEPT</span></span>
* <span data-ttu-id="c6d97-273">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="c6d97-273">HTTP_CONNECTION</span></span>
* <span data-ttu-id="c6d97-274">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="c6d97-274">HTTP_COOKIE</span></span>
* <span data-ttu-id="c6d97-275">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="c6d97-275">HTTP_FORWARDED</span></span>
* <span data-ttu-id="c6d97-276">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="c6d97-276">HTTP_HOST</span></span>
* <span data-ttu-id="c6d97-277">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="c6d97-277">HTTP_REFERER</span></span>
* <span data-ttu-id="c6d97-278">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="c6d97-278">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="c6d97-279">HTTPS</span><span class="sxs-lookup"><span data-stu-id="c6d97-279">HTTPS</span></span>
* <span data-ttu-id="c6d97-280">IPV6</span><span class="sxs-lookup"><span data-stu-id="c6d97-280">IPV6</span></span>
* <span data-ttu-id="c6d97-281">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="c6d97-281">QUERY_STRING</span></span>
* <span data-ttu-id="c6d97-282">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="c6d97-282">REMOTE_ADDR</span></span>
* <span data-ttu-id="c6d97-283">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="c6d97-283">REMOTE_PORT</span></span>
* <span data-ttu-id="c6d97-284">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="c6d97-284">REQUEST_FILENAME</span></span>
* <span data-ttu-id="c6d97-285">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="c6d97-285">REQUEST_METHOD</span></span>
* <span data-ttu-id="c6d97-286">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="c6d97-286">REQUEST_SCHEME</span></span>
* <span data-ttu-id="c6d97-287">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="c6d97-287">REQUEST_URI</span></span>
* <span data-ttu-id="c6d97-288">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="c6d97-288">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="c6d97-289">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="c6d97-289">SERVER_ADDR</span></span>
* <span data-ttu-id="c6d97-290">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="c6d97-290">SERVER_PORT</span></span>
* <span data-ttu-id="c6d97-291">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="c6d97-291">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="c6d97-292">時間</span><span class="sxs-lookup"><span data-stu-id="c6d97-292">TIME</span></span>
* <span data-ttu-id="c6d97-293">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="c6d97-293">TIME_DAY</span></span>
* <span data-ttu-id="c6d97-294">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="c6d97-294">TIME_HOUR</span></span>
* <span data-ttu-id="c6d97-295">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="c6d97-295">TIME_MIN</span></span>
* <span data-ttu-id="c6d97-296">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="c6d97-296">TIME_MON</span></span>
* <span data-ttu-id="c6d97-297">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="c6d97-297">TIME_SEC</span></span>
* <span data-ttu-id="c6d97-298">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="c6d97-298">TIME_WDAY</span></span>
* <span data-ttu-id="c6d97-299">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="c6d97-299">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="c6d97-300">IIS URL Rewrite 模組規則</span><span class="sxs-lookup"><span data-stu-id="c6d97-300">IIS URL Rewrite Module rules</span></span>
<span data-ttu-id="c6d97-301">若要使用適用於 IIS URL Rewrite 模組的規則， `AddIISUrlRewrite`。</span><span class="sxs-lookup"><span data-stu-id="c6d97-301">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="c6d97-302">請確定規則檔與應用程式部署。</span><span class="sxs-lookup"><span data-stu-id="c6d97-302">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="c6d97-303">不直接的中介軟體来使用您*web.config*檔案時在 Windows Server IIS 上執行。</span><span class="sxs-lookup"><span data-stu-id="c6d97-303">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="c6d97-304">使用 IIS 時，應該儲存這些規則的外部程式*web.config*以避免與 IIS Rewrite module 發生衝突。</span><span class="sxs-lookup"><span data-stu-id="c6d97-304">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="c6d97-305">如需詳細資訊和 IIS URL Rewrite 模組規則的範例，請參閱[使用 Url 重寫模組 2.0](https://www.iis.net/learn/extensions/url-rewrite-module/using-url-rewrite-module-20)和[URL Rewrite 模組組態參考](https://www.iis.net/learn/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)。</span><span class="sxs-lookup"><span data-stu-id="c6d97-305">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](https://www.iis.net/learn/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](https://www.iis.net/learn/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c6d97-306">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c6d97-306">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c6d97-307">A`StreamReader`用於讀取從規則*IISUrlRewrite.xml*規則檔案。</span><span class="sxs-lookup"><span data-stu-id="c6d97-307">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

<span data-ttu-id="c6d97-308">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=2,8)]</span><span class="sxs-lookup"><span data-stu-id="c6d97-308">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=2,8)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c6d97-309">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c6d97-309">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c6d97-310">第一個參數會採用`IFileProvider`，而第二個參數則是 XML 規則檔案，也就是路徑*IISUrlRewrite.xml*範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="c6d97-310">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

<span data-ttu-id="c6d97-311">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="c6d97-311">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=5)]</span></span>

---

<span data-ttu-id="c6d97-312">範例應用程式重寫從要求`/iis-rules-rewrite/(.*)`至`/rewritten?id=$1`。</span><span class="sxs-lookup"><span data-stu-id="c6d97-312">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="c6d97-313">回應會傳送至用戶端以 200 （確定） 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="c6d97-313">The response is sent to the client with a 200 (OK) status code.</span></span>

<span data-ttu-id="c6d97-314">[!code-xml[Main](url-rewriting/samples/2.x/IISUrlRewrite.xml)]</span><span class="sxs-lookup"><span data-stu-id="c6d97-314">[!code-xml[Main](url-rewriting/samples/2.x/IISUrlRewrite.xml)]</span></span>

<span data-ttu-id="c6d97-315">原始要求：`/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="c6d97-315">Original Request: `/iis-rules-rewrite/1234`</span></span>

![瀏覽器視窗中使用追蹤要求和回應的開發人員工具](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="c6d97-317">如果您有使用中的 IIS 重寫模組的伺服器層級規則設定，會影響您的應用程式非預期的方式，您可以停用應用程式的 IIS Rewrite Module。</span><span class="sxs-lookup"><span data-stu-id="c6d97-317">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="c6d97-318">如需詳細資訊，請參閱[停用 IIS 模組](xref:hosting/iis-modules#disabling-iis-modules)。</span><span class="sxs-lookup"><span data-stu-id="c6d97-318">For more information, see [Disabling IIS modules](xref:hosting/iis-modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="c6d97-319">不支援的功能</span><span class="sxs-lookup"><span data-stu-id="c6d97-319">Unsupported features</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c6d97-320">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c6d97-320">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c6d97-321">發行的中介軟體與 ASP.NET Core 2.x 不支援下列 IIS URL Rewrite Module 功能：</span><span class="sxs-lookup"><span data-stu-id="c6d97-321">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="c6d97-322">輸出規則</span><span class="sxs-lookup"><span data-stu-id="c6d97-322">Outbound Rules</span></span>
* <span data-ttu-id="c6d97-323">自訂伺服器變數</span><span class="sxs-lookup"><span data-stu-id="c6d97-323">Custom Server Variables</span></span>
* <span data-ttu-id="c6d97-324">萬用字元</span><span class="sxs-lookup"><span data-stu-id="c6d97-324">Wildcards</span></span>
* <span data-ttu-id="c6d97-325">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="c6d97-325">LogRewrittenUrl</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c6d97-326">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c6d97-326">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c6d97-327">發行的中介軟體與 ASP.NET Core 1.x 不支援下列 IIS URL Rewrite Module 功能：</span><span class="sxs-lookup"><span data-stu-id="c6d97-327">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="c6d97-328">全域規則</span><span class="sxs-lookup"><span data-stu-id="c6d97-328">Global Rules</span></span>
* <span data-ttu-id="c6d97-329">輸出規則</span><span class="sxs-lookup"><span data-stu-id="c6d97-329">Outbound Rules</span></span>
* <span data-ttu-id="c6d97-330">請重寫對應</span><span class="sxs-lookup"><span data-stu-id="c6d97-330">Rewrite Maps</span></span>
* <span data-ttu-id="c6d97-331">CustomResponse 動作</span><span class="sxs-lookup"><span data-stu-id="c6d97-331">CustomResponse Action</span></span>
* <span data-ttu-id="c6d97-332">自訂伺服器變數</span><span class="sxs-lookup"><span data-stu-id="c6d97-332">Custom Server Variables</span></span>
* <span data-ttu-id="c6d97-333">萬用字元</span><span class="sxs-lookup"><span data-stu-id="c6d97-333">Wildcards</span></span>
* <span data-ttu-id="c6d97-334">CustomResponse 動作：</span><span class="sxs-lookup"><span data-stu-id="c6d97-334">Action:CustomResponse</span></span>
* <span data-ttu-id="c6d97-335">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="c6d97-335">LogRewrittenUrl</span></span>

---

#### <a name="supported-server-variables"></a><span data-ttu-id="c6d97-336">支援的伺服器變數</span><span class="sxs-lookup"><span data-stu-id="c6d97-336">Supported server variables</span></span>
<span data-ttu-id="c6d97-337">中介軟體可支援下列 IIS URL Rewrite Module 伺服器變數：</span><span class="sxs-lookup"><span data-stu-id="c6d97-337">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>
* <span data-ttu-id="c6d97-338">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="c6d97-338">CONTENT_LENGTH</span></span>
* <span data-ttu-id="c6d97-339">有效</span><span class="sxs-lookup"><span data-stu-id="c6d97-339">CONTENT_TYPE</span></span>
* <span data-ttu-id="c6d97-340">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="c6d97-340">HTTP_ACCEPT</span></span>
* <span data-ttu-id="c6d97-341">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="c6d97-341">HTTP_CONNECTION</span></span>
* <span data-ttu-id="c6d97-342">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="c6d97-342">HTTP_COOKIE</span></span>
* <span data-ttu-id="c6d97-343">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="c6d97-343">HTTP_HOST</span></span>
* <span data-ttu-id="c6d97-344">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="c6d97-344">HTTP_REFERER</span></span>
* <span data-ttu-id="c6d97-345">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="c6d97-345">HTTP_URL</span></span>
* <span data-ttu-id="c6d97-346">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="c6d97-346">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="c6d97-347">HTTPS</span><span class="sxs-lookup"><span data-stu-id="c6d97-347">HTTPS</span></span>
* <span data-ttu-id="c6d97-348">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="c6d97-348">LOCAL_ADDR</span></span>
* <span data-ttu-id="c6d97-349">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="c6d97-349">QUERY_STRING</span></span>
* <span data-ttu-id="c6d97-350">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="c6d97-350">REMOTE_ADDR</span></span>
* <span data-ttu-id="c6d97-351">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="c6d97-351">REMOTE_PORT</span></span>
* <span data-ttu-id="c6d97-352">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="c6d97-352">REQUEST_FILENAME</span></span>
* <span data-ttu-id="c6d97-353">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="c6d97-353">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="c6d97-354">您也可以取得`IFileProvider`透過`PhysicalFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="c6d97-354">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="c6d97-355">規則檔案時，這種方法可能會提供更靈活彈性，您的重寫的位置。</span><span class="sxs-lookup"><span data-stu-id="c6d97-355">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="c6d97-356">請確定您重寫規則的檔案會部署到您提供的路徑的伺服器。</span><span class="sxs-lookup"><span data-stu-id="c6d97-356">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="c6d97-357">以方法為基礎的規則</span><span class="sxs-lookup"><span data-stu-id="c6d97-357">Method-based rule</span></span>
<span data-ttu-id="c6d97-358">使用`Add(Action<RewriteContext> applyRule)`來實作您自己的規則邏輯的方法中。</span><span class="sxs-lookup"><span data-stu-id="c6d97-358">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="c6d97-359">`RewriteContext`公開`HttpContext`以便用於您的方法。</span><span class="sxs-lookup"><span data-stu-id="c6d97-359">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="c6d97-360">`context.Result`判斷其他管線處理。</span><span class="sxs-lookup"><span data-stu-id="c6d97-360">The `context.Result` determines how additional pipeline processing is handled.</span></span>

| <span data-ttu-id="c6d97-361">內容。結果</span><span class="sxs-lookup"><span data-stu-id="c6d97-361">context.Result</span></span>                       | <span data-ttu-id="c6d97-362">動作</span><span class="sxs-lookup"><span data-stu-id="c6d97-362">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="c6d97-363">`RuleResult.ContinueRules` (預設值)</span><span class="sxs-lookup"><span data-stu-id="c6d97-363">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="c6d97-364">繼續套用規則</span><span class="sxs-lookup"><span data-stu-id="c6d97-364">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="c6d97-365">停止套用規則，並傳送回應</span><span class="sxs-lookup"><span data-stu-id="c6d97-365">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="c6d97-366">停止套用規則，並將內容傳送至下一個中介軟體</span><span class="sxs-lookup"><span data-stu-id="c6d97-366">Stop applying rules and send the context to the next middleware</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c6d97-367">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c6d97-367">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c6d97-368">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="c6d97-368">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=9)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c6d97-369">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c6d97-369">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c6d97-370">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=6)]</span><span class="sxs-lookup"><span data-stu-id="c6d97-370">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=6)]</span></span>

---

<span data-ttu-id="c6d97-371">範例應用程式會示範一個方法重新導向要求的路徑結尾*.xml*。</span><span class="sxs-lookup"><span data-stu-id="c6d97-371">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="c6d97-372">如果您為要求`/file.xml`，它會重新導向至`/xmlfiles/file.xml`。</span><span class="sxs-lookup"><span data-stu-id="c6d97-372">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="c6d97-373">狀態碼會設定為 301 （已永久移動）。</span><span class="sxs-lookup"><span data-stu-id="c6d97-373">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="c6d97-374">重新導向，您必須明確設定的狀態碼的回應。否則，會傳回 200 （確定） 」 狀態碼，並重新導向不會發生在用戶端。</span><span class="sxs-lookup"><span data-stu-id="c6d97-374">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c6d97-375">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c6d97-375">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c6d97-376">[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="c6d97-376">[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet1)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c6d97-377">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c6d97-377">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c6d97-378">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="c6d97-378">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet2)]</span></span>

---

<span data-ttu-id="c6d97-379">原始要求：`/file.xml`</span><span class="sxs-lookup"><span data-stu-id="c6d97-379">Original Request: `/file.xml`</span></span>

![瀏覽器視窗中使用追蹤的要求和回應的 file.xml 的開發人員工具](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="c6d97-381">IRule 型規則</span><span class="sxs-lookup"><span data-stu-id="c6d97-381">IRule-based rule</span></span>
<span data-ttu-id="c6d97-382">使用`Add(IRule)`衍生自的類別中實作您自己的規則邏輯`IRule`。</span><span class="sxs-lookup"><span data-stu-id="c6d97-382">Use `Add(IRule)` to implement your own rule logic in a class that derives from `IRule`.</span></span> <span data-ttu-id="c6d97-383">使用`IRule`使用以方法為基礎的規則方法相比，提供更大的彈性。</span><span class="sxs-lookup"><span data-stu-id="c6d97-383">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="c6d97-384">您的衍生的類別可能包括建構函式，您可以傳遞參數中`ApplyRule`方法。</span><span class="sxs-lookup"><span data-stu-id="c6d97-384">Your derived class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c6d97-385">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c6d97-385">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c6d97-386">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=10-11)]</span><span class="sxs-lookup"><span data-stu-id="c6d97-386">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=10-11)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c6d97-387">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c6d97-387">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c6d97-388">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=7-8)]</span><span class="sxs-lookup"><span data-stu-id="c6d97-388">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=7-8)]</span></span>

---

<span data-ttu-id="c6d97-389">範例應用程式中的參數值`extension`和`newPath`會檢查，以符合數個條件。</span><span class="sxs-lookup"><span data-stu-id="c6d97-389">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="c6d97-390">`extension`必須包含值，而且此值必須是*.png*， *.jpg*，或*.gif*。</span><span class="sxs-lookup"><span data-stu-id="c6d97-390">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="c6d97-391">如果`newPath`不是有效的`ArgumentException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="c6d97-391">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="c6d97-392">如果您為要求*image.png*，它會重新導向至`/png-images/image.png`。</span><span class="sxs-lookup"><span data-stu-id="c6d97-392">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="c6d97-393">如果您為要求*image.jpg*，它會重新導向至`/jpg-images/image.jpg`。</span><span class="sxs-lookup"><span data-stu-id="c6d97-393">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="c6d97-394">狀態碼 301 （已永久移動），來設定和`context.Result`設停止處理規則，並傳送回應。</span><span class="sxs-lookup"><span data-stu-id="c6d97-394">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c6d97-395">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c6d97-395">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c6d97-396">[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="c6d97-396">[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet2)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c6d97-397">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c6d97-397">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c6d97-398">[!code-csharp[Main](url-rewriting/samples/1.x/RewriteRule.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="c6d97-398">[!code-csharp[Main](url-rewriting/samples/1.x/RewriteRule.cs?name=snippet1)]</span></span>

---

<span data-ttu-id="c6d97-399">原始要求：`/image.png`</span><span class="sxs-lookup"><span data-stu-id="c6d97-399">Original Request: `/image.png`</span></span>

![瀏覽器視窗中使用追蹤的要求和回應的 image.png 的開發人員工具](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="c6d97-401">原始要求：`/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="c6d97-401">Original Request: `/image.jpg`</span></span>

![瀏覽器視窗中使用追蹤的要求和回應的 image.jpg 的開發人員工具](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="c6d97-403">Regex 範例</span><span class="sxs-lookup"><span data-stu-id="c6d97-403">Regex examples</span></span>

| <span data-ttu-id="c6d97-404">Goal</span><span class="sxs-lookup"><span data-stu-id="c6d97-404">Goal</span></span> | <span data-ttu-id="c6d97-405">Regex 字串 （& s)</span><span class="sxs-lookup"><span data-stu-id="c6d97-405">Regex String &</span></span><br><span data-ttu-id="c6d97-406">比對範例</span><span class="sxs-lookup"><span data-stu-id="c6d97-406">Match Example</span></span> | <span data-ttu-id="c6d97-407">取代字串 （& s)</span><span class="sxs-lookup"><span data-stu-id="c6d97-407">Replacement String &</span></span><br><span data-ttu-id="c6d97-408">輸出範例</span><span class="sxs-lookup"><span data-stu-id="c6d97-408">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="c6d97-409">重新撰寫成查詢字串的路徑</span><span class="sxs-lookup"><span data-stu-id="c6d97-409">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="c6d97-410">寬帶尾端斜線</span><span class="sxs-lookup"><span data-stu-id="c6d97-410">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="c6d97-411">強制執行結尾的斜線</span><span class="sxs-lookup"><span data-stu-id="c6d97-411">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="c6d97-412">避免重新撰寫特定的要求</span><span class="sxs-lookup"><span data-stu-id="c6d97-412">Avoid rewriting specific requests</span></span> | `(.*[^(\.axd)])$`<br><span data-ttu-id="c6d97-413">[是]:`/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="c6d97-413">Yes: `/resource.htm`</span></span><br><span data-ttu-id="c6d97-414">否：`/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="c6d97-414">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="c6d97-415">重新排列 URL 區段</span><span class="sxs-lookup"><span data-stu-id="c6d97-415">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="c6d97-416">取代 URL 區段</span><span class="sxs-lookup"><span data-stu-id="c6d97-416">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="c6d97-417">其他資源</span><span class="sxs-lookup"><span data-stu-id="c6d97-417">Additional resources</span></span>
* [<span data-ttu-id="c6d97-418">應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="c6d97-418">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="c6d97-419">中介軟體</span><span class="sxs-lookup"><span data-stu-id="c6d97-419">Middleware</span></span>](middleware.md)
* [<span data-ttu-id="c6d97-420">.NET 中的規則運算式</span><span class="sxs-lookup"><span data-stu-id="c6d97-420">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="c6d97-421">規則運算式語言 - 快速參考</span><span class="sxs-lookup"><span data-stu-id="c6d97-421">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="c6d97-422">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="c6d97-422">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="c6d97-423">使用 Url Rewrite 模組 2.0 （適用於 IIS)</span><span class="sxs-lookup"><span data-stu-id="c6d97-423">Using Url Rewrite Module 2.0 (for IIS)</span></span>](https://www.iis.net/learn/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [<span data-ttu-id="c6d97-424">URL 重寫模組的組態參考</span><span class="sxs-lookup"><span data-stu-id="c6d97-424">URL Rewrite Module Configuration Reference</span></span>](https://www.iis.net/learn/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [<span data-ttu-id="c6d97-425">IIS URL 重寫模組論壇</span><span class="sxs-lookup"><span data-stu-id="c6d97-425">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* [<span data-ttu-id="c6d97-426">保持簡單的 URL 結構</span><span class="sxs-lookup"><span data-stu-id="c6d97-426">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* [<span data-ttu-id="c6d97-427">10 的 URL 重寫的秘訣和訣竅</span><span class="sxs-lookup"><span data-stu-id="c6d97-427">10 URL Rewriting Tips and Tricks</span></span>](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [<span data-ttu-id="c6d97-428">斜線或不進行斜線</span><span class="sxs-lookup"><span data-stu-id="c6d97-428">To slash or not to slash</span></span>](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
