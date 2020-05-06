---
title: ASP.NET Core 的用戶端 IP 安全
author: damienbod
description: 瞭解如何撰寫中介軟體或動作篩選器，以根據核准的 IP 位址清單來驗證遠端 IP 位址。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/12/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/ip-safelist
ms.openlocfilehash: 7923a81e72124cfb0e11e3c1ac327c1e32194b21
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82776496"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="7eb80-103">ASP.NET Core 的用戶端 IP 安全</span><span class="sxs-lookup"><span data-stu-id="7eb80-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="7eb80-104">By [Damien Bowden](https://twitter.com/damien_bod)和[Tom 作者: dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="7eb80-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="7eb80-105">本文說明三種在 ASP.NET Core 應用程式中執行 IP 位址安全清單（也稱為允許清單）的方式。</span><span class="sxs-lookup"><span data-stu-id="7eb80-105">This article shows three ways to implement an IP address safelist (also known as an allow list) in an ASP.NET Core app.</span></span> <span data-ttu-id="7eb80-106">隨附的範例應用程式會示範這三種方法。</span><span class="sxs-lookup"><span data-stu-id="7eb80-106">An accompanying sample app demonstrates all three approaches.</span></span> <span data-ttu-id="7eb80-107">您可以使用：</span><span class="sxs-lookup"><span data-stu-id="7eb80-107">You can use:</span></span>

* <span data-ttu-id="7eb80-108">中介軟體，以檢查每個要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="7eb80-108">Middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="7eb80-109">MVC 動作篩選準則，以檢查特定控制器或動作方法的遠端 IP 位址要求。</span><span class="sxs-lookup"><span data-stu-id="7eb80-109">MVC action filters to check the remote IP address of requests for specific controllers or action methods.</span></span>
* Razor<span data-ttu-id="7eb80-110">頁面篩選以檢查Razor頁面要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="7eb80-110"> Pages filters to check the remote IP address of requests for Razor pages.</span></span>

<span data-ttu-id="7eb80-111">在每個案例中，包含已核准用戶端 IP 位址的字串會儲存在應用程式設定中。</span><span class="sxs-lookup"><span data-stu-id="7eb80-111">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="7eb80-112">中介軟體或篩選：</span><span class="sxs-lookup"><span data-stu-id="7eb80-112">The middleware or filter:</span></span>

* <span data-ttu-id="7eb80-113">將字串剖析為數組。</span><span class="sxs-lookup"><span data-stu-id="7eb80-113">Parses the string into an array.</span></span> 
* <span data-ttu-id="7eb80-114">檢查陣列中是否有遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="7eb80-114">Checks if the remote IP address exists in the array.</span></span>

<span data-ttu-id="7eb80-115">如果陣列包含 IP 位址，則允許存取。</span><span class="sxs-lookup"><span data-stu-id="7eb80-115">Access is allowed if the array contains the IP address.</span></span> <span data-ttu-id="7eb80-116">否則，會傳回 HTTP 403 禁止狀態碼。</span><span class="sxs-lookup"><span data-stu-id="7eb80-116">Otherwise, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="7eb80-117">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="7eb80-117">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ip-address-safelist"></a><span data-ttu-id="7eb80-118">IP 位址的安全</span><span class="sxs-lookup"><span data-stu-id="7eb80-118">IP address safelist</span></span>

<span data-ttu-id="7eb80-119">在範例應用程式中，IP 位址的「安全」是：</span><span class="sxs-lookup"><span data-stu-id="7eb80-119">In the sample app, the IP address safelist is:</span></span>

* <span data-ttu-id="7eb80-120">由 appsettings 中`AdminSafeList`的屬性所*appsettings.json*定義。</span><span class="sxs-lookup"><span data-stu-id="7eb80-120">Defined by the `AdminSafeList` property in the *appsettings.json* file.</span></span>
* <span data-ttu-id="7eb80-121">以分號分隔的字串，其中可能包含[網際網路通訊協定第4版（IPv4）](https://wikipedia.org/wiki/IPv4)和[網際網路通訊協定第6版（IPv6）](https://wikipedia.org/wiki/IPv6)位址。</span><span class="sxs-lookup"><span data-stu-id="7eb80-121">A semicolon-delimited string that may contain both [Internet Protocol version 4 (IPv4)](https://wikipedia.org/wiki/IPv4) and [Internet Protocol version 6 (IPv6)](https://wikipedia.org/wiki/IPv6) addresses.</span></span>

[!code-json[](ip-safelist/samples/3.x/ClientIpAspNetCore/appsettings.json?range=1-3&highlight=2)]

<span data-ttu-id="7eb80-122">在上述範例中， `127.0.0.1`允許和`192.168.1.5`的 IPv4 位址和的 IPv6 回送位址`::1` （的壓縮格式`0:0:0:0:0:0:0:1`）。</span><span class="sxs-lookup"><span data-stu-id="7eb80-122">In the preceding example, the IPv4 addresses of `127.0.0.1` and `192.168.1.5` and the IPv6 loopback address of `::1` (compressed format for `0:0:0:0:0:0:0:1`) are allowed.</span></span>

## <a name="middleware"></a><span data-ttu-id="7eb80-123">中介軟體</span><span class="sxs-lookup"><span data-stu-id="7eb80-123">Middleware</span></span>

<span data-ttu-id="7eb80-124">`Startup.Configure`方法會將自訂`AdminSafeListMiddleware`中介軟體類型新增至應用程式的要求管線。</span><span class="sxs-lookup"><span data-stu-id="7eb80-124">The `Startup.Configure` method adds the custom `AdminSafeListMiddleware` middleware type to the app's request pipeline.</span></span> <span data-ttu-id="7eb80-125">您可使用 .NET Core 設定提供者來抓取安全，並將其當做「函式參數」傳遞。</span><span class="sxs-lookup"><span data-stu-id="7eb80-125">The safelist is retrieved with the .NET Core configuration provider and is passed as a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureAddMiddleware)]

<span data-ttu-id="7eb80-126">中介軟體會將字串剖析為數組，並在陣列中搜尋遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="7eb80-126">The middleware parses the string into an array and searches for the remote IP address in the array.</span></span> <span data-ttu-id="7eb80-127">如果找不到遠端 IP 位址，中介軟體會傳回 HTTP 403 禁止。</span><span class="sxs-lookup"><span data-stu-id="7eb80-127">If the remote IP address isn't found, the middleware returns HTTP 403 Forbidden.</span></span> <span data-ttu-id="7eb80-128">針對 HTTP GET 要求，會略過此驗證程式。</span><span class="sxs-lookup"><span data-stu-id="7eb80-128">This validation process is bypassed for HTTP GET requests.</span></span>

[!code-csharp[](ip-safelist/samples/Shared/ClientIpSafelistComponents/Middlewares/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="7eb80-129">動作篩選準則</span><span class="sxs-lookup"><span data-stu-id="7eb80-129">Action filter</span></span>

<span data-ttu-id="7eb80-130">如果您想要針對特定的 MVC 控制器或動作方法，使用以安全的方式導向的存取控制，請使用動作篩選準則。</span><span class="sxs-lookup"><span data-stu-id="7eb80-130">If you want safelist-driven access control for specific MVC controllers or action methods, use an action filter.</span></span> <span data-ttu-id="7eb80-131">例如：</span><span class="sxs-lookup"><span data-stu-id="7eb80-131">For example:</span></span>

[!code-csharp[](ip-safelist/samples/Shared/ClientIpSafelistComponents/Filters/ClientIpCheckActionFilter.cs?name=snippet_ClassOnly)]

<span data-ttu-id="7eb80-132">在`Startup.ConfigureServices`中，將動作篩選準則新增至 MVC 篩選集合。</span><span class="sxs-lookup"><span data-stu-id="7eb80-132">In `Startup.ConfigureServices`, add the action filter to the MVC filters collection.</span></span> <span data-ttu-id="7eb80-133">在下列範例中，會`ClientIpCheckActionFilter`加入動作篩選準則。</span><span class="sxs-lookup"><span data-stu-id="7eb80-133">In the following example, a `ClientIpCheckActionFilter` action filter is added.</span></span> <span data-ttu-id="7eb80-134">「安全性記錄檔」和「主控台記錄器」實例會當做「函式參數」傳遞。</span><span class="sxs-lookup"><span data-stu-id="7eb80-134">A safelist and a console logger instance are passed as constructor parameters.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesActionFilter)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesActionFilter)]

::: moniker-end

<span data-ttu-id="7eb80-135">接著，您可以使用[[ServiceFilter]](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute)屬性，將動作篩選套用至控制器或動作方法：</span><span class="sxs-lookup"><span data-stu-id="7eb80-135">The action filter can then be applied to a controller or action method with the [[ServiceFilter]](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute) attribute:</span></span>

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_ActionFilter&highlight=1)]

<span data-ttu-id="7eb80-136">在範例應用程式中，動作篩選準則會套用至控制器的`Get`動作方法。</span><span class="sxs-lookup"><span data-stu-id="7eb80-136">In the sample app, the action filter is applied to the controller's `Get` action method.</span></span> <span data-ttu-id="7eb80-137">當您藉由傳送來測試應用程式時：</span><span class="sxs-lookup"><span data-stu-id="7eb80-137">When you test the app by sending:</span></span>

* <span data-ttu-id="7eb80-138">HTTP GET 要求， `[ServiceFilter]`屬性會驗證用戶端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="7eb80-138">An HTTP GET request, the `[ServiceFilter]` attribute validates the client IP address.</span></span> <span data-ttu-id="7eb80-139">如果允許存取`Get`動作方法，則動作篩選和動作方法會產生下列主控台輸出的變化：</span><span class="sxs-lookup"><span data-stu-id="7eb80-139">If access is allowed to the `Get` action method, a variation of the following console output is produced by the action filter and action method:</span></span>

    ```
    dbug: ClientIpSafelistComponents.Filters.ClientIpCheckActionFilter[0]
          Remote IpAddress: ::1
    dbug: ClientIpAspNetCore.Controllers.ValuesController[0]
          successful HTTP GET    
    ```

* <span data-ttu-id="7eb80-140">GET 以外的`AdminSafeListMiddleware` HTTP 要求動詞，中介軟體會驗證用戶端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="7eb80-140">An HTTP request verb other than GET, the `AdminSafeListMiddleware` middleware validates the client IP address.</span></span>

## <a name="razor-pages-filter"></a>Razor<span data-ttu-id="7eb80-141">頁面篩選</span><span class="sxs-lookup"><span data-stu-id="7eb80-141"> Pages filter</span></span>

<span data-ttu-id="7eb80-142">如果您想要使用Razor網頁應用程式的 [以安全] 為導向Razor的存取控制，請使用 [頁面] 篩選準則。</span><span class="sxs-lookup"><span data-stu-id="7eb80-142">If you want safelist-driven access control for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="7eb80-143">例如：</span><span class="sxs-lookup"><span data-stu-id="7eb80-143">For example:</span></span>

[!code-csharp[](ip-safelist/samples/Shared/ClientIpSafelistComponents/Filters/ClientIpCheckPageFilter.cs?name=snippet_ClassOnly)]

<span data-ttu-id="7eb80-144">在`Startup.ConfigureServices`中，藉Razor由將頁面篩選新增至 MVC 篩選器集合來啟用它們。</span><span class="sxs-lookup"><span data-stu-id="7eb80-144">In `Startup.ConfigureServices`, enable the Razor Pages filter by adding it to the MVC filters collection.</span></span> <span data-ttu-id="7eb80-145">在下列範例中， `ClientIpCheckPageFilter` Razor會加入頁面篩選。</span><span class="sxs-lookup"><span data-stu-id="7eb80-145">In the following example, a `ClientIpCheckPageFilter` Razor Pages filter is added.</span></span> <span data-ttu-id="7eb80-146">「安全性記錄檔」和「主控台記錄器」實例會當做「函式參數」傳遞。</span><span class="sxs-lookup"><span data-stu-id="7eb80-146">A safelist and a console logger instance are passed as constructor parameters.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesPageFilter)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesPageFilter)]

::: moniker-end

<span data-ttu-id="7eb80-147">要求範例應用程式的 [*索引* Razor ] 頁面時， Razor [頁面] 篩選準則會驗證用戶端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="7eb80-147">When the sample app's *Index* Razor page is requested, the Razor Pages filter validates the client IP address.</span></span> <span data-ttu-id="7eb80-148">此篩選會產生下列主控台輸出的變化：</span><span class="sxs-lookup"><span data-stu-id="7eb80-148">The filter produces a variation of the following console output:</span></span>

```
dbug: ClientIpSafelistComponents.Filters.ClientIpCheckPageFilter[0]
      Remote IpAddress: ::1
```

## <a name="additional-resources"></a><span data-ttu-id="7eb80-149">其他資源</span><span class="sxs-lookup"><span data-stu-id="7eb80-149">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="7eb80-150">動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="7eb80-150">Action filters</span></span>](xref:mvc/controllers/filters#action-filters)
* <xref:razor-pages/filter>
