---
title: ASP.NET核心用戶端 IP 安全性清單
author: damienbod
description: 瞭解如何編寫中間件或操作篩選器,以便根據已批准的 IP 位址清單驗證遠端 IP 位址。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/12/2020
uid: security/ip-safelist
ms.openlocfilehash: 2db879a6918245cbacff8b1a5dc15786ffab6a34
ms.sourcegitcommit: 196e4a36df5be5b04fedcff484a4261f8046ec57
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/31/2020
ms.locfileid: "80471786"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="9e901-103">ASP.NET核心用戶端 IP 安全性清單</span><span class="sxs-lookup"><span data-stu-id="9e901-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="9e901-104">由[達米安·鮑登](https://twitter.com/damien_bod)和[湯姆·戴克斯特拉](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="9e901-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="9e901-105">本文介紹了在ASP.NET核心應用中實現IP位址安全清單(也稱為允許清單)的三種方法。</span><span class="sxs-lookup"><span data-stu-id="9e901-105">This article shows three ways to implement an IP address safelist (also known as an allow list) in an ASP.NET Core app.</span></span> <span data-ttu-id="9e901-106">隨附的示例應用演示了所有三種方法。</span><span class="sxs-lookup"><span data-stu-id="9e901-106">An accompanying sample app demonstrates all three approaches.</span></span> <span data-ttu-id="9e901-107">您可以使用：</span><span class="sxs-lookup"><span data-stu-id="9e901-107">You can use:</span></span>

* <span data-ttu-id="9e901-108">檢查每個要求的遠端 IP 位址的中間件。</span><span class="sxs-lookup"><span data-stu-id="9e901-108">Middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="9e901-109">MVC 操作篩選器,用於檢查特定控制器或操作方法的請求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="9e901-109">MVC action filters to check the remote IP address of requests for specific controllers or action methods.</span></span>
* <span data-ttu-id="9e901-110">Razor 頁面篩選器以檢查 Razor 頁面請求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="9e901-110">Razor Pages filters to check the remote IP address of requests for Razor pages.</span></span>

<span data-ttu-id="9e901-111">在每種情況下,包含已批准的用戶端 IP 位址的字串都存儲在應用設置中。</span><span class="sxs-lookup"><span data-stu-id="9e901-111">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="9e901-112">中間件或過濾器:</span><span class="sxs-lookup"><span data-stu-id="9e901-112">The middleware or filter:</span></span>

* <span data-ttu-id="9e901-113">將字串解析為陣列。</span><span class="sxs-lookup"><span data-stu-id="9e901-113">Parses the string into an array.</span></span> 
* <span data-ttu-id="9e901-114">檢查陣列中是否存在遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="9e901-114">Checks if the remote IP address exists in the array.</span></span>

<span data-ttu-id="9e901-115">如果陣列包含 IP 位址,則允許訪問。</span><span class="sxs-lookup"><span data-stu-id="9e901-115">Access is allowed if the array contains the IP address.</span></span> <span data-ttu-id="9e901-116">否則,將返回 HTTP 403 禁止狀態代碼。</span><span class="sxs-lookup"><span data-stu-id="9e901-116">Otherwise, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="9e901-117">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9e901-117">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ip-address-safelist"></a><span data-ttu-id="9e901-118">IP 位址安全清單</span><span class="sxs-lookup"><span data-stu-id="9e901-118">IP address safelist</span></span>

<span data-ttu-id="9e901-119">在範例應用中,IP 位址安全清單為:</span><span class="sxs-lookup"><span data-stu-id="9e901-119">In the sample app, the IP address safelist is:</span></span>

* <span data-ttu-id="9e901-120">由`AdminSafeList`*appsettings.json*檔中的屬性定義。</span><span class="sxs-lookup"><span data-stu-id="9e901-120">Defined by the `AdminSafeList` property in the *appsettings.json* file.</span></span>
* <span data-ttu-id="9e901-121">一個分號分隔字串,可能包含 Internet[協定版本 4 (IPv4)](https://wikipedia.org/wiki/IPv4)和[Internet 協定版本 6 (IPv6)](https://wikipedia.org/wiki/IPv6)位址。</span><span class="sxs-lookup"><span data-stu-id="9e901-121">A semicolon-delimited string that may contain both [Internet Protocol version 4 (IPv4)](https://wikipedia.org/wiki/IPv4) and [Internet Protocol version 6 (IPv6)](https://wikipedia.org/wiki/IPv6) addresses.</span></span>

[!code-json[](ip-safelist/samples/3.x/ClientIpAspNetCore/appsettings.json?range=1-3&highlight=2)]

<span data-ttu-id="9e901-122">在前面的範例中,`127.0.0.1`允許`192.168.1.5`和的 IPv4`::1`位址和 IPv6 回`0:0:0:0:0:0:0:1`回位址(壓縮格式 )。</span><span class="sxs-lookup"><span data-stu-id="9e901-122">In the preceding example, the IPv4 addresses of `127.0.0.1` and `192.168.1.5` and the IPv6 loopback address of `::1` (compressed format for `0:0:0:0:0:0:0:1`) are allowed.</span></span>

## <a name="middleware"></a><span data-ttu-id="9e901-123">中介軟體</span><span class="sxs-lookup"><span data-stu-id="9e901-123">Middleware</span></span>

<span data-ttu-id="9e901-124">該方法`Startup.Configure`將自`AdminSafeListMiddleware`定義 中間件類型添加到應用的請求管道中。</span><span class="sxs-lookup"><span data-stu-id="9e901-124">The `Startup.Configure` method adds the custom `AdminSafeListMiddleware` middleware type to the app's request pipeline.</span></span> <span data-ttu-id="9e901-125">使用 .NET Core 配置提供程式檢索安全清單,並作為建構函數參數傳遞。</span><span class="sxs-lookup"><span data-stu-id="9e901-125">The safelist is retrieved with the .NET Core configuration provider and is passed as a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureAddMiddleware)]

<span data-ttu-id="9e901-126">中間件將字串解析為陣列並搜尋陣列中的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="9e901-126">The middleware parses the string into an array and searches for the remote IP address in the array.</span></span> <span data-ttu-id="9e901-127">如果未找到遠端 IP 位址,中間件將返回 HTTP403 禁止。</span><span class="sxs-lookup"><span data-stu-id="9e901-127">If the remote IP address isn't found, the middleware returns HTTP 403 Forbidden.</span></span> <span data-ttu-id="9e901-128">此驗證過程是繞過 HTTP GET 請求的。</span><span class="sxs-lookup"><span data-stu-id="9e901-128">This validation process is bypassed for HTTP GET requests.</span></span>

[!code-csharp[](ip-safelist/samples/Shared/ClientIpSafelistComponents/Middlewares/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="9e901-129">操作篩選器</span><span class="sxs-lookup"><span data-stu-id="9e901-129">Action filter</span></span>

<span data-ttu-id="9e901-130">如果需要針對特定 MVC 控制器或操作方法的安全列表驅動存取控制,請使用操作篩選器。</span><span class="sxs-lookup"><span data-stu-id="9e901-130">If you want safelist-driven access control for specific MVC controllers or action methods, use an action filter.</span></span> <span data-ttu-id="9e901-131">例如：</span><span class="sxs-lookup"><span data-stu-id="9e901-131">For example:</span></span>

[!code-csharp[](ip-safelist/samples/Shared/ClientIpSafelistComponents/Filters/ClientIpCheckActionFilter.cs?name=snippet_ClassOnly)]

<span data-ttu-id="9e901-132">在`Startup.ConfigureServices`中,將操作篩選器添加到 MVC 篩選器集合。</span><span class="sxs-lookup"><span data-stu-id="9e901-132">In `Startup.ConfigureServices`, add the action filter to the MVC filters collection.</span></span> <span data-ttu-id="9e901-133">在下面的示例中,將添加`ClientIpCheckActionFilter`操作篩選器。</span><span class="sxs-lookup"><span data-stu-id="9e901-133">In the following example, a `ClientIpCheckActionFilter` action filter is added.</span></span> <span data-ttu-id="9e901-134">安全清單和控制台記錄器實例作為構造函數參數傳遞。</span><span class="sxs-lookup"><span data-stu-id="9e901-134">A safelist and a console logger instance are passed as constructor parameters.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesActionFilter)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesActionFilter)]

::: moniker-end

<span data-ttu-id="9e901-135">然後,操作篩選器可以應用於具有[[服務篩選器]](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute)屬性的控制器或操作方法:</span><span class="sxs-lookup"><span data-stu-id="9e901-135">The action filter can then be applied to a controller or action method with the [[ServiceFilter]](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute) attribute:</span></span>

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_ActionFilter&highlight=1)]

<span data-ttu-id="9e901-136">在示例應用中,操作篩選器應用於控制器的操作`Get`方法。</span><span class="sxs-lookup"><span data-stu-id="9e901-136">In the sample app, the action filter is applied to the controller's `Get` action method.</span></span> <span data-ttu-id="9e901-137">透過送出測試應用程式時:</span><span class="sxs-lookup"><span data-stu-id="9e901-137">When you test the app by sending:</span></span>

* <span data-ttu-id="9e901-138">HTTP GET`[ServiceFilter]`請求, 屬性驗證用戶端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="9e901-138">An HTTP GET request, the `[ServiceFilter]` attribute validates the client IP address.</span></span> <span data-ttu-id="9e901-139">如果允許存`Get`取 操作方法,操作篩選器和操作方法將產生以下主控台輸出的變體:</span><span class="sxs-lookup"><span data-stu-id="9e901-139">If access is allowed to the `Get` action method, a variation of the following console output is produced by the action filter and action method:</span></span>

    ```
    dbug: ClientIpSafelistComponents.Filters.ClientIpCheckActionFilter[0]
          Remote IpAddress: ::1
    dbug: ClientIpAspNetCore.Controllers.ValuesController[0]
          successful HTTP GET    
    ```

* <span data-ttu-id="9e901-140">除了 GET`AdminSafeListMiddleware`之外, 中間件的 HTTP 請求謂詞將驗證用戶端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="9e901-140">An HTTP request verb other than GET, the `AdminSafeListMiddleware` middleware validates the client IP address.</span></span>

## <a name="razor-pages-filter"></a><span data-ttu-id="9e901-141">剃刀頁面過濾器</span><span class="sxs-lookup"><span data-stu-id="9e901-141">Razor Pages filter</span></span>

<span data-ttu-id="9e901-142">如果需要 Razor Pages 應用的安全清單驅動存取控制,請使用 Razor 頁面篩選器。</span><span class="sxs-lookup"><span data-stu-id="9e901-142">If you want safelist-driven access control for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="9e901-143">例如：</span><span class="sxs-lookup"><span data-stu-id="9e901-143">For example:</span></span>

[!code-csharp[](ip-safelist/samples/Shared/ClientIpSafelistComponents/Filters/ClientIpCheckPageFilter.cs?name=snippet_ClassOnly)]

<span data-ttu-id="9e901-144">在`Startup.ConfigureServices`中,通過將 Razor 頁面添加到 MVC 篩選器集合,使其啟用該篩選器。</span><span class="sxs-lookup"><span data-stu-id="9e901-144">In `Startup.ConfigureServices`, enable the Razor Pages filter by adding it to the MVC filters collection.</span></span> <span data-ttu-id="9e901-145">在下面的範例中,將添加`ClientIpCheckPageFilter`Razor 頁面篩選器。</span><span class="sxs-lookup"><span data-stu-id="9e901-145">In the following example, a `ClientIpCheckPageFilter` Razor Pages filter is added.</span></span> <span data-ttu-id="9e901-146">安全清單和控制台記錄器實例作為構造函數參數傳遞。</span><span class="sxs-lookup"><span data-stu-id="9e901-146">A safelist and a console logger instance are passed as constructor parameters.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesPageFilter)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesPageFilter)]

::: moniker-end

<span data-ttu-id="9e901-147">當請求範例應用的*索引*Razor 頁面時,"Razor 頁面"篩選器將驗證用戶端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="9e901-147">When the sample app's *Index* Razor page is requested, the Razor Pages filter validates the client IP address.</span></span> <span data-ttu-id="9e901-148">篩選器產生以下主控台輸出的變體:</span><span class="sxs-lookup"><span data-stu-id="9e901-148">The filter produces a variation of the following console output:</span></span>

```
dbug: ClientIpSafelistComponents.Filters.ClientIpCheckPageFilter[0]
      Remote IpAddress: ::1
```

## <a name="additional-resources"></a><span data-ttu-id="9e901-149">其他資源</span><span class="sxs-lookup"><span data-stu-id="9e901-149">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="9e901-150">動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="9e901-150">Action filters</span></span>](xref:mvc/controllers/filters#action-filters)
* <xref:razor-pages/filter>
