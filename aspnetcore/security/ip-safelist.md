---
title: ASP.NET Core 的用戶端 IP 安全
author: damienbod
description: 瞭解如何撰寫中介軟體或動作篩選器, 以根據核准的 IP 位址清單來驗證遠端 IP 位址。
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: ca05989efabea3a71c6912e98055a6746e0f5966
ms.sourcegitcommit: 1bf80f4acd62151ff8cce517f03f6fa891136409
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/15/2019
ms.locfileid: "68223925"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="6317f-103">ASP.NET Core 的用戶端 IP 安全</span><span class="sxs-lookup"><span data-stu-id="6317f-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="6317f-104">By [Damien Bowden](https://twitter.com/damien_bod)和[Tom 作者: dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="6317f-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="6317f-105">本文說明三種在 ASP.NET Core 應用程式中執行 IP 安全清單 (也稱為「白名單」) 的方式。</span><span class="sxs-lookup"><span data-stu-id="6317f-105">This article shows three ways to implement an IP safelist (also known as a whitelist) in an ASP.NET Core app.</span></span> <span data-ttu-id="6317f-106">您可以使用:</span><span class="sxs-lookup"><span data-stu-id="6317f-106">You can use:</span></span>

* <span data-ttu-id="6317f-107">中介軟體, 以檢查每個要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="6317f-107">Middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="6317f-108">動作篩選準則, 以檢查特定控制器或動作方法的遠端 IP 位址要求。</span><span class="sxs-lookup"><span data-stu-id="6317f-108">Action filters to check the remote IP address of requests for specific controllers or action methods.</span></span>
* <span data-ttu-id="6317f-109">Razor Pages 篩選準則, 以檢查 Razor 頁面要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="6317f-109">Razor Pages filters to check the remote IP address of requests for Razor pages.</span></span>

<span data-ttu-id="6317f-110">在每個案例中, 包含已核准用戶端 IP 位址的字串會儲存在應用程式設定中。</span><span class="sxs-lookup"><span data-stu-id="6317f-110">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="6317f-111">中介軟體或篩選器會將字串剖析為清單, 並檢查遠端 IP 是否在清單中。</span><span class="sxs-lookup"><span data-stu-id="6317f-111">The middleware or filter parses the string into a list and checks if the remote IP is in the list.</span></span> <span data-ttu-id="6317f-112">如果不是, 則會傳回 HTTP 403 禁止狀態碼。</span><span class="sxs-lookup"><span data-stu-id="6317f-112">If not, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="6317f-113">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6317f-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="the-safelist"></a><span data-ttu-id="6317f-114">安全的</span><span class="sxs-lookup"><span data-stu-id="6317f-114">The safelist</span></span>

<span data-ttu-id="6317f-115">此清單會在*appsettings*中設定。</span><span class="sxs-lookup"><span data-stu-id="6317f-115">The list is configured in the *appsettings.json* file.</span></span> <span data-ttu-id="6317f-116">它是以分號分隔的清單, 而且可以包含 IPv4 和 IPv6 位址。</span><span class="sxs-lookup"><span data-stu-id="6317f-116">It's a semicolon-delimited list and can contain IPv4 and IPv6 addresses.</span></span>

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a><span data-ttu-id="6317f-117">中介軟體</span><span class="sxs-lookup"><span data-stu-id="6317f-117">Middleware</span></span>

<span data-ttu-id="6317f-118">`Configure`方法會新增中介軟體, 並在函式參數中將安全檔案字串傳遞給它。</span><span class="sxs-lookup"><span data-stu-id="6317f-118">The `Configure` method adds the middleware and passes the safelist string to it in a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=10)]

<span data-ttu-id="6317f-119">中介軟體會將字串剖析為數組, 並在陣列中尋找遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="6317f-119">The middleware parses the string into an array and looks for the remote IP address in the array.</span></span> <span data-ttu-id="6317f-120">如果找不到遠端 IP 位址, 中介軟體會傳回 HTTP 401 禁止。</span><span class="sxs-lookup"><span data-stu-id="6317f-120">If the remote IP address is not found, the middleware returns HTTP 401 Forbidden.</span></span> <span data-ttu-id="6317f-121">針對 HTTP Get 要求, 會略過此驗證程式。</span><span class="sxs-lookup"><span data-stu-id="6317f-121">This validation process is bypassed for HTTP Get requests.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="6317f-122">動作篩選準則</span><span class="sxs-lookup"><span data-stu-id="6317f-122">Action filter</span></span>

<span data-ttu-id="6317f-123">如果您只想要特定的控制器或動作方法的安全, 請使用動作篩選準則。</span><span class="sxs-lookup"><span data-stu-id="6317f-123">If you want a safelist only for specific controllers or action methods, use an action filter.</span></span> <span data-ttu-id="6317f-124">以下為範例：</span><span class="sxs-lookup"><span data-stu-id="6317f-124">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

<span data-ttu-id="6317f-125">動作篩選準則會新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="6317f-125">The action filter is added to the services container.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="6317f-126">然後, 您可以在控制器或動作方法上使用此篩選器。</span><span class="sxs-lookup"><span data-stu-id="6317f-126">The filter can then be used on a controller or action method.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

<span data-ttu-id="6317f-127">在範例應用程式中, 篩選準則會套用至`Get`方法。</span><span class="sxs-lookup"><span data-stu-id="6317f-127">In the sample app, the filter is applied to the `Get` method.</span></span> <span data-ttu-id="6317f-128">因此, 當您藉由傳送`Get` API 要求來測試應用程式時, 屬性會驗證用戶端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="6317f-128">So when you test the app by sending a `Get` API request, the attribute is validating the client IP address.</span></span> <span data-ttu-id="6317f-129">當您使用任何其他 HTTP 方法呼叫 API 來進行測試時, 中介軟體會驗證用戶端 IP。</span><span class="sxs-lookup"><span data-stu-id="6317f-129">When you test by calling the API with any other HTTP method, the middleware is validating the client IP.</span></span>

## <a name="razor-pages-filter"></a><span data-ttu-id="6317f-130">Razor Pages 篩選</span><span class="sxs-lookup"><span data-stu-id="6317f-130">Razor Pages filter</span></span> 

<span data-ttu-id="6317f-131">如果您想要 Razor Pages 應用程式的安全, 請使用 Razor Pages 篩選器。</span><span class="sxs-lookup"><span data-stu-id="6317f-131">If you want a safelist for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="6317f-132">以下為範例：</span><span class="sxs-lookup"><span data-stu-id="6317f-132">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

<span data-ttu-id="6317f-133">藉由將此篩選準則新增至 MVC 篩選器集合, 即可加以啟用。</span><span class="sxs-lookup"><span data-stu-id="6317f-133">This filter is enabled by adding it to the MVC Filters collection.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

<span data-ttu-id="6317f-134">當您執行應用程式並要求 Razor 頁面時, Razor Pages 篩選器會驗證用戶端 IP。</span><span class="sxs-lookup"><span data-stu-id="6317f-134">When you run the app and request a Razor page, the Razor Pages filter is validating the client IP.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6317f-135">後續步驟</span><span class="sxs-lookup"><span data-stu-id="6317f-135">Next steps</span></span>

<span data-ttu-id="6317f-136">[深入瞭解 ASP.NET Core 中介軟體](xref:fundamentals/middleware/index)。</span><span class="sxs-lookup"><span data-stu-id="6317f-136">[Learn more about ASP.NET Core Middleware](xref:fundamentals/middleware/index).</span></span>
