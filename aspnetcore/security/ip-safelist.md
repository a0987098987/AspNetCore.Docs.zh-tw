---
title: 適用於 ASP.NET Core 的用戶端 IP 安全清單
author: damienbod
description: 了解如何撰寫中介軟體或動作篩選條件來驗證遠端 IP 位址，根據核准的 IP 位址的清單。
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: 40fe7b67359efd1692490099c3fb529ba4a6148f
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040105"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="e189e-103">適用於 ASP.NET Core 的用戶端 IP 安全清單</span><span class="sxs-lookup"><span data-stu-id="e189e-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="e189e-104">藉由[Damien Bowden](https://twitter.com/damien_bod)和[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="e189e-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="e189e-105">本文說明兩種方式來實作 IP 安全清單 （也稱為允許清單）：</span><span class="sxs-lookup"><span data-stu-id="e189e-105">This article shows two ways to implement an IP safelist (also known as a whitelist):</span></span>

* <span data-ttu-id="e189e-106">使用 ASP.NET Core 中介軟體來檢查每個要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="e189e-106">By using ASP.NET Core middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="e189e-107">使用 ASP.NET Core 動作篩選條件來檢查遠端 IP 位址的特定動作方法的要求。</span><span class="sxs-lookup"><span data-stu-id="e189e-107">By using ASP.NET Core action filters to check the remote IP address of requests for specific action methods.</span></span>

<span data-ttu-id="e189e-108">範例應用程式說明這兩種方法。</span><span class="sxs-lookup"><span data-stu-id="e189e-108">The sample app illustrates both approaches.</span></span> <span data-ttu-id="e189e-109">在每個案例中，包含已核准的用戶端 IP 位址的字串會儲存在應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="e189e-109">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="e189e-110">中介軟體或篩選器會將字串剖析成清單，並檢查 遠端 IP 是否在清單中。</span><span class="sxs-lookup"><span data-stu-id="e189e-110">The middleware or filter parses the string into a list and  checks if the remote IP is in the list.</span></span> <span data-ttu-id="e189e-111">如果沒有，則會傳回 HTTP 403 禁止狀態碼。</span><span class="sxs-lookup"><span data-stu-id="e189e-111">If not, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="e189e-112">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e189e-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-safelist"></a><span data-ttu-id="e189e-113">安全清單</span><span class="sxs-lookup"><span data-stu-id="e189e-113">The safelist</span></span>

<span data-ttu-id="e189e-114">在設定的清單*appsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="e189e-114">The list is configured in the *appsettings.json* file.</span></span> <span data-ttu-id="e189e-115">它是以分號分隔的清單，並可包含 IPv4 和 IPv6 位址。</span><span class="sxs-lookup"><span data-stu-id="e189e-115">It's a semicolon-delimited list and can contain IPv4 and IPv6 addresses.</span></span>

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a><span data-ttu-id="e189e-116">中介軟體</span><span class="sxs-lookup"><span data-stu-id="e189e-116">Middleware</span></span>

<span data-ttu-id="e189e-117">`Configure`方法新增中介軟體，並在建構函式參數傳遞給它的安全清單的字串。</span><span class="sxs-lookup"><span data-stu-id="e189e-117">The `Configure` method adds the middleware and passes the safelist string to it in a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=7)]

<span data-ttu-id="e189e-118">中介軟體會將字串剖析成陣列，並尋找陣列中的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="e189e-118">The middleware parses the string into an array and looks for the remote IP address in the array.</span></span> <span data-ttu-id="e189e-119">如果找不到的遠端 IP 位址中, 介軟體會傳回 HTTP 401 禁止。</span><span class="sxs-lookup"><span data-stu-id="e189e-119">If the remote IP address is not found, the middleware returns HTTP 401 Forbidden.</span></span> <span data-ttu-id="e189e-120">HTTP Get 要求，會略過此驗證程序。</span><span class="sxs-lookup"><span data-stu-id="e189e-120">This validation process is bypassed for HTTP Get requests.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="e189e-121">動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="e189e-121">Action filter</span></span>

<span data-ttu-id="e189e-122">如果您想安全清單僅適用於特定的控制器或動作方法時，請使用動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="e189e-122">If you want a safelist only for specific controllers or action methods, use an action filter.</span></span> <span data-ttu-id="e189e-123">以下為範例：</span><span class="sxs-lookup"><span data-stu-id="e189e-123">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

<span data-ttu-id="e189e-124">動作篩選條件加入至服務容器。</span><span class="sxs-lookup"><span data-stu-id="e189e-124">The action filter is added to the services container.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="e189e-125">篩選可用的控制器或動作方法上。</span><span class="sxs-lookup"><span data-stu-id="e189e-125">The filter can then be used on a controller or action method.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

<span data-ttu-id="e189e-126">在範例應用程式中，篩選會套用至`Get`方法。</span><span class="sxs-lookup"><span data-stu-id="e189e-126">In the sample app, the filter is applied to the `Get` method.</span></span> <span data-ttu-id="e189e-127">因此，當您測試應用程式傳送`Get`API 要求，該屬性，就會檢查用戶端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="e189e-127">So when you test the app by sending a `Get` API request, the attribute is validating the client IP address.</span></span> <span data-ttu-id="e189e-128">當您測試與任何其他 HTTP 方法呼叫 API 時中, 介軟體就驗證的用戶端 IP。</span><span class="sxs-lookup"><span data-stu-id="e189e-128">When you test by calling the API with any other HTTP method, the middleware is validating the client IP.</span></span>

## <a name="razor-pages-filter"></a><span data-ttu-id="e189e-129">Razor 頁面篩選</span><span class="sxs-lookup"><span data-stu-id="e189e-129">Razor Pages filter</span></span> 

<span data-ttu-id="e189e-130">如果您想要的 Razor 頁面應用程式安全清單，請使用 Razor 頁面篩選條件。</span><span class="sxs-lookup"><span data-stu-id="e189e-130">If you want a safelist for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="e189e-131">以下為範例：</span><span class="sxs-lookup"><span data-stu-id="e189e-131">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

<span data-ttu-id="e189e-132">將它新增至 MVC 篩選條件集合時，會啟用此篩選器。</span><span class="sxs-lookup"><span data-stu-id="e189e-132">This filter is enabled by adding it to the MVC Filters collection.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

<span data-ttu-id="e189e-133">當您執行應用程式，並要求 Razor 頁面時，Razor 頁面篩選條件就驗證的用戶端 IP。</span><span class="sxs-lookup"><span data-stu-id="e189e-133">When you run the app and request a Razor page, the Razor Pages filter is validating the client IP.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e189e-134">後續步驟</span><span class="sxs-lookup"><span data-stu-id="e189e-134">Next steps</span></span>

<span data-ttu-id="e189e-135">[深入了解 ASP.NET Core 中介軟體](xref:fundamentals/middleware/index)。</span><span class="sxs-lookup"><span data-stu-id="e189e-135">[Learn more about ASP.NET Core Middleware](xref:fundamentals/middleware/index).</span></span>
