---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: ASP.NET Web API 中的路由 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a7bc998fc23c0453fc9cd6ac1e7b9af7bd516225
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207299"
---
<a name="routing-in-aspnet-web-api"></a><span data-ttu-id="5af73-102">ASP.NET Web API 中的路由</span><span class="sxs-lookup"><span data-stu-id="5af73-102">Routing in ASP.NET Web API</span></span>
====================
<span data-ttu-id="5af73-103">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5af73-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="5af73-104">這篇文章說明 ASP.NET Web API 將 HTTP 要求路由至控制器的方式。</span><span class="sxs-lookup"><span data-stu-id="5af73-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="5af73-105">如果您熟悉 ASP.NET MVC、 Web API 路由是非常類似於 MVC 路由。</span><span class="sxs-lookup"><span data-stu-id="5af73-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="5af73-106">主要差異在於，Web API 會使用 HTTP 指令動詞，而不是 URI 路徑，，在選取的動作。</span><span class="sxs-lookup"><span data-stu-id="5af73-106">The main difference is that Web API uses the HTTP verb, not the URI path, to select the action.</span></span> <span data-ttu-id="5af73-107">您也可以使用 Web API 中的 MVC 樣式路由。</span><span class="sxs-lookup"><span data-stu-id="5af73-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="5af73-108">這篇文章不採用任何 ASP.NET MVC 的知識。</span><span class="sxs-lookup"><span data-stu-id="5af73-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>

## <a name="routing-tables"></a><span data-ttu-id="5af73-109">路由表</span><span class="sxs-lookup"><span data-stu-id="5af73-109">Routing Tables</span></span>

<span data-ttu-id="5af73-110">在 ASP.NET Web API 中，*控制器*是處理 HTTP 要求的類別。</span><span class="sxs-lookup"><span data-stu-id="5af73-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="5af73-111">控制站的公用方法會呼叫*動作方法*簡稱*動作*。</span><span class="sxs-lookup"><span data-stu-id="5af73-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="5af73-112">當 Web API 架構會收到要求時，它會將要求路由至動作。</span><span class="sxs-lookup"><span data-stu-id="5af73-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="5af73-113">若要判斷要叫用動作，架構會使用*路由表*。</span><span class="sxs-lookup"><span data-stu-id="5af73-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="5af73-114">Web API 的 Visual Studio 專案範本會建立預設路由：</span><span class="sxs-lookup"><span data-stu-id="5af73-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="5af73-115">此路由定義於*WebApiConfig.cs*檔案，其會置於*應用程式\_啟動*目錄：</span><span class="sxs-lookup"><span data-stu-id="5af73-115">This route is defined in the *WebApiConfig.cs* file, which is placed in the *App\_Start* directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="5af73-116">如需詳細資訊`WebApiConfig`類別，請參閱[設定 ASP.NET Web API](../advanced/configuring-aspnet-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="5af73-116">For more information about the `WebApiConfig` class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="5af73-117">如果您自我裝載 Web API，您必須直接上設定的路由表`HttpSelfHostConfiguration`物件。</span><span class="sxs-lookup"><span data-stu-id="5af73-117">If you self-host Web API, you must set the routing table directly on the `HttpSelfHostConfiguration` object.</span></span> <span data-ttu-id="5af73-118">如需詳細資訊，請參閱 <<c0> [ 自我裝載 Web API](../older-versions/self-host-a-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="5af73-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="5af73-119">路由表中的每個項目包含*路由範本*。</span><span class="sxs-lookup"><span data-stu-id="5af73-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="5af73-120">Web API 的預設路由範本&quot;api / {controller} / {id}&quot;。</span><span class="sxs-lookup"><span data-stu-id="5af73-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="5af73-121">此範本中， &quot;api&quot;是常值路徑區段和 {controller} 和 {id} 是預留位置變數。</span><span class="sxs-lookup"><span data-stu-id="5af73-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="5af73-122">當 Web API 架構會收到 HTTP 要求時，它會嘗試比對針對其中一個路由表中的路由範本的 URI。</span><span class="sxs-lookup"><span data-stu-id="5af73-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="5af73-123">如果沒有路由相符，用戶端會收到 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="5af73-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="5af73-124">例如，下列 Uri 會符合預設路由：</span><span class="sxs-lookup"><span data-stu-id="5af73-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="5af73-125">/ api/連絡人</span><span class="sxs-lookup"><span data-stu-id="5af73-125">/api/contacts</span></span>
- <span data-ttu-id="5af73-126">/api/contacts/1</span><span class="sxs-lookup"><span data-stu-id="5af73-126">/api/contacts/1</span></span>
- <span data-ttu-id="5af73-127">/api/products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="5af73-127">/api/products/gizmo1</span></span>

<span data-ttu-id="5af73-128">不過，下列 URI 不符合，因為它缺少&quot;api&quot;區段：</span><span class="sxs-lookup"><span data-stu-id="5af73-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="5af73-129">連絡人/1</span><span class="sxs-lookup"><span data-stu-id="5af73-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="5af73-130">使用路由中的 「 api 」 的原因是避免與 ASP.NET MVC routing 發生衝突。</span><span class="sxs-lookup"><span data-stu-id="5af73-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="5af73-131">如此一來，您可以有&quot;/連絡&quot;移至 MVC 控制器，並&quot;/api/連絡人&quot;移至 Web API 控制器。</span><span class="sxs-lookup"><span data-stu-id="5af73-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="5af73-132">當然，如果您不喜歡這個慣例，您可以變更預設路由表。</span><span class="sxs-lookup"><span data-stu-id="5af73-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="5af73-133">一旦找到相符的路由，Web API 會選取控制器和動作：</span><span class="sxs-lookup"><span data-stu-id="5af73-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="5af73-134">若要尋找控制器，將 Web API&quot;控制器&quot;的值 *{controller}* 變數。</span><span class="sxs-lookup"><span data-stu-id="5af73-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="5af73-135">若要尋找此動作，Web API 會查看 HTTP 動詞命令，並接著會尋找名稱開頭為該 HTTP 指令動詞名稱的動作。</span><span class="sxs-lookup"><span data-stu-id="5af73-135">To find the action, Web API looks at the HTTP verb, and then looks for an action whose name begins with that HTTP verb name.</span></span> <span data-ttu-id="5af73-136">例如，透過 GET 要求，Web API 會尋找動作前面加上&quot;取得&quot;，例如&quot;GetContact&quot;或是&quot;GetAllContacts&quot;。</span><span class="sxs-lookup"><span data-stu-id="5af73-136">For example, with a GET request, Web API looks for an action prefixed with &quot;Get&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="5af73-137">這個慣例僅適用於 GET、 POST、 PUT、 DELETE、 HEAD、 選項和 PATCH 動詞命令。</span><span class="sxs-lookup"><span data-stu-id="5af73-137">This convention applies only to GET, POST, PUT, DELETE, HEAD, OPTIONS, and PATCH verbs.</span></span> <span data-ttu-id="5af73-138">您可以使用屬性控制站上，以啟用其他 HTTP 動詞命令。</span><span class="sxs-lookup"><span data-stu-id="5af73-138">You can enable other HTTP verbs by using attributes on your controller.</span></span> <span data-ttu-id="5af73-139">我們稍後所見的範例。</span><span class="sxs-lookup"><span data-stu-id="5af73-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="5af73-140">其他的預留位置變數在路由範本中，這類 *{id}，* 會對應至動作參數。</span><span class="sxs-lookup"><span data-stu-id="5af73-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="5af73-141">讓我們看看一個範例。</span><span class="sxs-lookup"><span data-stu-id="5af73-141">Let's look at an example.</span></span> <span data-ttu-id="5af73-142">假設您定義下列控制器：</span><span class="sxs-lookup"><span data-stu-id="5af73-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="5af73-143">以下是一些可能的 HTTP 要求，以及每個叫用的動作：</span><span class="sxs-lookup"><span data-stu-id="5af73-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="5af73-144">HTTP 指令動詞</span><span class="sxs-lookup"><span data-stu-id="5af73-144">HTTP Verb</span></span> | <span data-ttu-id="5af73-145">URI 的路徑</span><span class="sxs-lookup"><span data-stu-id="5af73-145">URI Path</span></span> | <span data-ttu-id="5af73-146">動作</span><span class="sxs-lookup"><span data-stu-id="5af73-146">Action</span></span> | <span data-ttu-id="5af73-147">參數</span><span class="sxs-lookup"><span data-stu-id="5af73-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5af73-148">GET</span><span class="sxs-lookup"><span data-stu-id="5af73-148">GET</span></span> | <span data-ttu-id="5af73-149">api/產品</span><span class="sxs-lookup"><span data-stu-id="5af73-149">api/products</span></span> | <span data-ttu-id="5af73-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="5af73-150">GetAllProducts</span></span> | <span data-ttu-id="5af73-151">*（無）*</span><span class="sxs-lookup"><span data-stu-id="5af73-151">*(none)*</span></span> |
| <span data-ttu-id="5af73-152">GET</span><span class="sxs-lookup"><span data-stu-id="5af73-152">GET</span></span> | <span data-ttu-id="5af73-153">api/產品/4</span><span class="sxs-lookup"><span data-stu-id="5af73-153">api/products/4</span></span> | <span data-ttu-id="5af73-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="5af73-154">GetProductById</span></span> | <span data-ttu-id="5af73-155">4</span><span class="sxs-lookup"><span data-stu-id="5af73-155">4</span></span> |
| <span data-ttu-id="5af73-156">DELETE</span><span class="sxs-lookup"><span data-stu-id="5af73-156">DELETE</span></span> | <span data-ttu-id="5af73-157">api/產品/4</span><span class="sxs-lookup"><span data-stu-id="5af73-157">api/products/4</span></span> | <span data-ttu-id="5af73-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="5af73-158">DeleteProduct</span></span> | <span data-ttu-id="5af73-159">4</span><span class="sxs-lookup"><span data-stu-id="5af73-159">4</span></span> |
| <span data-ttu-id="5af73-160">POST</span><span class="sxs-lookup"><span data-stu-id="5af73-160">POST</span></span> | <span data-ttu-id="5af73-161">api/產品</span><span class="sxs-lookup"><span data-stu-id="5af73-161">api/products</span></span> | <span data-ttu-id="5af73-162">*（沒有相符項目）*</span><span class="sxs-lookup"><span data-stu-id="5af73-162">*(no match)*</span></span> |  |

<span data-ttu-id="5af73-163">請注意， *{id}* 區段的 URI，如果有的話，會對應至*識別碼*動作參數。</span><span class="sxs-lookup"><span data-stu-id="5af73-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="5af73-164">在此範例中，控制器會定義兩個 GET 方法，一個具有*識別碼*參數和一個不含任何參數。</span><span class="sxs-lookup"><span data-stu-id="5af73-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="5af73-165">另外，請注意，POST 要求將會失敗，因為控制器不會定義&quot;文章...&quot;方法。</span><span class="sxs-lookup"><span data-stu-id="5af73-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="5af73-166">路由的變化</span><span class="sxs-lookup"><span data-stu-id="5af73-166">Routing Variations</span></span>

<span data-ttu-id="5af73-167">上一節所述的基本 ASP.NET Web API 路由機制。</span><span class="sxs-lookup"><span data-stu-id="5af73-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="5af73-168">本章節描述一些變化。</span><span class="sxs-lookup"><span data-stu-id="5af73-168">This section describes some variations.</span></span>

### <a name="http-verbs"></a><span data-ttu-id="5af73-169">HTTP 指令動詞</span><span class="sxs-lookup"><span data-stu-id="5af73-169">HTTP verbs</span></span>

<span data-ttu-id="5af73-170">而不是使用 HTTP 動詞命令的命名慣例，您可以裝飾具有下列屬性的其中一個動作方法，明確指定動作的 HTTP 動詞命令：</span><span class="sxs-lookup"><span data-stu-id="5af73-170">Instead of using the naming convention for HTTP verbs, you can explicitly specify the HTTP verb for an action by decorating the action method with one of the following attributes:</span></span>

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

<span data-ttu-id="5af73-171">在下列範例中，`FindProduct`方法會對應至 GET 要求：</span><span class="sxs-lookup"><span data-stu-id="5af73-171">In the following example, the `FindProduct` method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="5af73-172">若要允許多個 HTTP 動詞命令動作，或允許 GET、 PUT、 POST、 DELETE、 HEAD、 選項和修補程式以外的 HTTP 指令動詞，請使用`[AcceptVerbs]`屬性，根據 HTTP 指令動詞的清單。</span><span class="sxs-lookup"><span data-stu-id="5af73-172">To allow multiple HTTP verbs for an action, or to allow HTTP verbs other than GET, PUT, POST, DELETE, HEAD, OPTIONS, and PATCH, use the `[AcceptVerbs]` attribute, which takes a list of HTTP verbs.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="5af73-173">路由動作名稱</span><span class="sxs-lookup"><span data-stu-id="5af73-173">Routing by Action Name</span></span>

<span data-ttu-id="5af73-174">這個預設路由範本中，Web API 會使用 HTTP 動詞命令，來選取的動作。</span><span class="sxs-lookup"><span data-stu-id="5af73-174">With the default routing template, Web API uses the HTTP verb to select the action.</span></span> <span data-ttu-id="5af73-175">不過，您也可以建立的路由，其中包含 URI 中的動作名稱：</span><span class="sxs-lookup"><span data-stu-id="5af73-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="5af73-176">此路由範本中， *{action}* 參數名稱控制站上的動作方法。</span><span class="sxs-lookup"><span data-stu-id="5af73-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="5af73-177">使用這種路由方法，使用屬性來指定允許的 HTTP 動詞命令。</span><span class="sxs-lookup"><span data-stu-id="5af73-177">With this style of routing, use attributes to specify the allowed HTTP verbs.</span></span> <span data-ttu-id="5af73-178">例如，假設您的控制器具有下列方法：</span><span class="sxs-lookup"><span data-stu-id="5af73-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="5af73-179">在此情況下，"api/產品/詳細資料/1"的 GET 要求會對應至`Details`方法。</span><span class="sxs-lookup"><span data-stu-id="5af73-179">In this case, a GET request for "api/products/details/1" would map to the `Details` method.</span></span> <span data-ttu-id="5af73-180">這種路由樣式類似於 ASP.NET MVC，並可能是適當的 RPC 樣式 API。</span><span class="sxs-lookup"><span data-stu-id="5af73-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="5af73-181">您可以使用覆寫動作名稱`[ActionName]`屬性。</span><span class="sxs-lookup"><span data-stu-id="5af73-181">You can override the action name by using the `[ActionName]` attribute.</span></span> <span data-ttu-id="5af73-182">在下列範例中，有兩個動作對應至&quot;縮圖 api/產品 / /*識別碼*。其中一個支援 GET，另支援文章：</span><span class="sxs-lookup"><span data-stu-id="5af73-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="5af73-183">非動作</span><span class="sxs-lookup"><span data-stu-id="5af73-183">Non-Actions</span></span>

<span data-ttu-id="5af73-184">若要防止方法叫用動作，請使用`[NonAction]`屬性。</span><span class="sxs-lookup"><span data-stu-id="5af73-184">To prevent a method from getting invoked as an action, use the `[NonAction]` attribute.</span></span> <span data-ttu-id="5af73-185">這表示 framework 方法不是動作，即使否則找到相符的路由規則。</span><span class="sxs-lookup"><span data-stu-id="5af73-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="5af73-186">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="5af73-186">Further Reading</span></span>

<span data-ttu-id="5af73-187">本主題提供路由的高層級檢視。</span><span class="sxs-lookup"><span data-stu-id="5af73-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="5af73-188">如需詳細資訊，請參閱 <<c0> [ 路由和動作選取](routing-and-action-selection.md)，其中描述完全如何架構符合路由的 URI、 選取控制器，然後選取要叫用動作。</span><span class="sxs-lookup"><span data-stu-id="5af73-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
