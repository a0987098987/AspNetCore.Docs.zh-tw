---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: "屬性 ASP.NET Web API 2 中的路由 |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 67ab1536b4a72abf8c0d3ed5aa0c48bc79a8fb5f
ms.sourcegitcommit: 3d512ea991ac36dfd4c800b7d1f8a27bfc50635e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2018
---
<a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="12e33-102">ASP.NET Web API 2 中的路由屬性</span><span class="sxs-lookup"><span data-stu-id="12e33-102">Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="12e33-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="12e33-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="12e33-104">*路由*是 Web 應用程式開發介面如何比對的動作 URI。</span><span class="sxs-lookup"><span data-stu-id="12e33-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="12e33-105">Web API 2 支援新類型的路由，稱為*屬性路由*。</span><span class="sxs-lookup"><span data-stu-id="12e33-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="12e33-106">正如其名，路由屬性使用屬性來定義路由。</span><span class="sxs-lookup"><span data-stu-id="12e33-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="12e33-107">路由屬性讓您更充分掌控 Uri 在您的 web API。</span><span class="sxs-lookup"><span data-stu-id="12e33-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="12e33-108">例如，您可以輕鬆地建立描述階層架構的資源的 Uri。</span><span class="sxs-lookup"><span data-stu-id="12e33-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="12e33-109">呼叫慣例為基礎的舊樣式的路由，路由，仍完整支援。</span><span class="sxs-lookup"><span data-stu-id="12e33-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="12e33-110">事實上，您可以結合相同專案中的這兩種技術。</span><span class="sxs-lookup"><span data-stu-id="12e33-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="12e33-111">本主題顯示如何啟用路由的屬性，並描述屬性路由的各種選項。</span><span class="sxs-lookup"><span data-stu-id="12e33-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="12e33-112">端對端教學課程會使用屬性的路由，請參閱[屬由路由的 Web API 2 中建立 REST API](create-a-rest-api-with-attribute-routing.md)。</span><span class="sxs-lookup"><span data-stu-id="12e33-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="12e33-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="12e33-113">Prerequisites</span></span>

<span data-ttu-id="12e33-114">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community、 Professional 或 Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="12e33-114">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional, or Enterprise Edition</span></span>

<span data-ttu-id="12e33-115">或者，使用 NuGet 套件管理員來安裝必要的封裝。</span><span class="sxs-lookup"><span data-stu-id="12e33-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="12e33-116">從**工具**在 Visual Studio 中，選取功能表**程式庫套件管理員**，然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="12e33-116">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="12e33-117">在 [封裝管理員主控台] 視窗中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="12e33-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="12e33-118">為什麼屬性路由？</span><span class="sxs-lookup"><span data-stu-id="12e33-118">Why Attribute Routing?</span></span>

<span data-ttu-id="12e33-119">Web API 所使用的第一版*慣例為基礎*路由。</span><span class="sxs-lookup"><span data-stu-id="12e33-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="12e33-120">在該類型的路由中，定義一個或多個路由範本，這基本上是參數化字串。</span><span class="sxs-lookup"><span data-stu-id="12e33-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="12e33-121">架構收到要求時，它會比對針對路由範本的 URI。</span><span class="sxs-lookup"><span data-stu-id="12e33-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="12e33-122">(如需慣例為基礎的路由的詳細資訊，請參閱[中 ASP.NET Web API 的路由](routing-in-aspnet-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="12e33-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="12e33-123">慣例為基礎的路由的其中一個優點是，樣板會定義在單一位置，和路由規則會一致地套用所有控制站。</span><span class="sxs-lookup"><span data-stu-id="12e33-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="12e33-124">不幸的是，慣例為基礎的路由讓人難以支援 rest 式應用程式開發介面中常見特定 URI 模式。</span><span class="sxs-lookup"><span data-stu-id="12e33-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="12e33-125">例如，資源通常會包含子資源： 客戶具有訂單，電影演員，書籍的作者可能，等等。</span><span class="sxs-lookup"><span data-stu-id="12e33-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="12e33-126">很自然建立 Uri，以反映這些關聯性：</span><span class="sxs-lookup"><span data-stu-id="12e33-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="12e33-127">這種類型的 URI 很難建立使用以慣例為基礎的路由。</span><span class="sxs-lookup"><span data-stu-id="12e33-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="12e33-128">雖然可以完成，結果不縮放，如果您有多個控制站或資源類型。</span><span class="sxs-lookup"><span data-stu-id="12e33-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="12e33-129">路由屬性，它是 trivial 這個 uri 定義路由。</span><span class="sxs-lookup"><span data-stu-id="12e33-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="12e33-130">您只會將屬性加入控制器動作：</span><span class="sxs-lookup"><span data-stu-id="12e33-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="12e33-131">以下是一些其他屬性路由可讓您輕鬆的模式。</span><span class="sxs-lookup"><span data-stu-id="12e33-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="12e33-132">**API 版本設定**</span><span class="sxs-lookup"><span data-stu-id="12e33-132">**API versioning**</span></span>

<span data-ttu-id="12e33-133">在此範例中，"/ 產品/api/v1"會路由至不同的控制站，比"/ api/v2/產品"。</span><span class="sxs-lookup"><span data-stu-id="12e33-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`  
`/api/v2/products`

<span data-ttu-id="12e33-134">**多載的 URI 區段**</span><span class="sxs-lookup"><span data-stu-id="12e33-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="12e33-135">在此範例中，"1"是訂單號碼，但是 「 擱置 」 對應至集合。</span><span class="sxs-lookup"><span data-stu-id="12e33-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`  
`/orders/pending`

<span data-ttu-id="12e33-136">**多個參數類型**</span><span class="sxs-lookup"><span data-stu-id="12e33-136">**Multiple parameter types**</span></span>

<span data-ttu-id="12e33-137">在此範例中，"1"是訂單號碼，但是"2013年/06/16"指定的日期。</span><span class="sxs-lookup"><span data-stu-id="12e33-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="12e33-138">啟用路由的屬性</span><span class="sxs-lookup"><span data-stu-id="12e33-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="12e33-139">若要啟用路由的屬性，請呼叫**MapHttpAttributeRoutes**期間設定。</span><span class="sxs-lookup"><span data-stu-id="12e33-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="12e33-140">這個擴充方法定義在**System.Web.Http.HttpConfigurationExtensions**類別。</span><span class="sxs-lookup"><span data-stu-id="12e33-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="12e33-141">路由屬性可以結合[慣例為基礎](routing-in-aspnet-web-api.md)路由。</span><span class="sxs-lookup"><span data-stu-id="12e33-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="12e33-142">若要定義以慣例為基礎的路由，請呼叫**MapHttpRoute**方法。</span><span class="sxs-lookup"><span data-stu-id="12e33-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="12e33-143">如需有關設定 Web API 的詳細資訊，請參閱[設定 ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="12e33-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="12e33-144">從 Web API 1 移轉附註：</span><span class="sxs-lookup"><span data-stu-id="12e33-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="12e33-145">Web API 2 之前, 的 Web API 專案範本會產生如下的程式碼：</span><span class="sxs-lookup"><span data-stu-id="12e33-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="12e33-146">如果屬性已啟用路由，這段程式碼將會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="12e33-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="12e33-147">如果您升級現有的 Web API 專案以使用屬性路由，請務必更新這個組態程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="12e33-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="12e33-148">如需詳細資訊，請參閱[設定與 ASP.NET 裝載的 Web API](../advanced/configuring-aspnet-web-api.md#webhost)。</span><span class="sxs-lookup"><span data-stu-id="12e33-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="12e33-149">新增路由屬性</span><span class="sxs-lookup"><span data-stu-id="12e33-149">Adding Route Attributes</span></span>

<span data-ttu-id="12e33-150">路由屬性來定義的範例如下：</span><span class="sxs-lookup"><span data-stu-id="12e33-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="12e33-151">字串&quot;客戶 / {customerId} / 排序&quot;是路由的 URI 範本。</span><span class="sxs-lookup"><span data-stu-id="12e33-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="12e33-152">Web API 會嘗試比對要求 URI 範本。</span><span class="sxs-lookup"><span data-stu-id="12e33-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="12e33-153">在此範例中，「 客戶 」 和 「 訂單 」 是常值的區段，而且 {"customerId}"是變數參數。</span><span class="sxs-lookup"><span data-stu-id="12e33-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="12e33-154">下列 Uri 會符合此範本：</span><span class="sxs-lookup"><span data-stu-id="12e33-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="12e33-155">您可以限制透過比對[條件約束](#constraints)，在本主題稍後所述。</span><span class="sxs-lookup"><span data-stu-id="12e33-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="12e33-156">請注意， &quot;{customerId}&quot;路由範本中的參數符合名稱的*customerId*方法中的參數。</span><span class="sxs-lookup"><span data-stu-id="12e33-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="12e33-157">當 Web 應用程式開發介面會叫用控制器動作時，它會嘗試將路由參數繫結。</span><span class="sxs-lookup"><span data-stu-id="12e33-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="12e33-158">例如，如果 URI 是`http://example.com/customers/1/orders`，Web 應用程式開發介面來繫結值"1"嘗試將*customerId*動作中的參數。</span><span class="sxs-lookup"><span data-stu-id="12e33-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="12e33-159">URI 樣板可以有數個參數：</span><span class="sxs-lookup"><span data-stu-id="12e33-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="12e33-160">路由屬性沒有任何控制器方法使用慣例為基礎的路由。</span><span class="sxs-lookup"><span data-stu-id="12e33-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="12e33-161">這樣一來，您可以結合兩種類型的相同專案中的路由。</span><span class="sxs-lookup"><span data-stu-id="12e33-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="12e33-162">HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="12e33-162">HTTP Methods</span></span>

<span data-ttu-id="12e33-163">Web 應用程式開發介面也會選取動作的要求 （GET、 POST 等） 的 HTTP 方法為基礎。</span><span class="sxs-lookup"><span data-stu-id="12e33-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="12e33-164">根據預設，Web API 會尋找不區分大小寫的比對控制器方法名稱的開頭。</span><span class="sxs-lookup"><span data-stu-id="12e33-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="12e33-165">例如，名為控制器方法`PutCustomers`符合 HTTP PUT 要求。</span><span class="sxs-lookup"><span data-stu-id="12e33-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="12e33-166">您可以覆寫這個慣例裝飾的方法與任何下列屬性：</span><span class="sxs-lookup"><span data-stu-id="12e33-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="12e33-167">**[HttpDelete]**</span><span class="sxs-lookup"><span data-stu-id="12e33-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="12e33-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="12e33-168">**[HttpGet]**</span></span>
- <span data-ttu-id="12e33-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="12e33-169">**[HttpHead]**</span></span>
- <span data-ttu-id="12e33-170">**[HttpOptions]**</span><span class="sxs-lookup"><span data-stu-id="12e33-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="12e33-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="12e33-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="12e33-172">**[HttpPost]**</span><span class="sxs-lookup"><span data-stu-id="12e33-172">**[HttpPost]**</span></span>
- <span data-ttu-id="12e33-173">**[HttpPut]**</span><span class="sxs-lookup"><span data-stu-id="12e33-173">**[HttpPut]**</span></span>

<span data-ttu-id="12e33-174">下列範例會將 CreateBook 方法對應至 HTTP POST 要求。</span><span class="sxs-lookup"><span data-stu-id="12e33-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="12e33-175">對於所有其他 HTTP 方法，包括非標準的方法，請使用**AcceptVerbs**屬性，根據 HTTP 方法的清單。</span><span class="sxs-lookup"><span data-stu-id="12e33-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="12e33-176">路由前置詞</span><span class="sxs-lookup"><span data-stu-id="12e33-176">Route Prefixes</span></span>

<span data-ttu-id="12e33-177">通常中的路由所有開頭為相同的前置詞的控制站。</span><span class="sxs-lookup"><span data-stu-id="12e33-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="12e33-178">例如: </span><span class="sxs-lookup"><span data-stu-id="12e33-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="12e33-179">您也可以使用整個控制器設定通用的前置詞**[RoutePrefix]**屬性：</span><span class="sxs-lookup"><span data-stu-id="12e33-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="12e33-180">路由前置詞會覆寫方法的屬性上使用波狀符號 （~）：</span><span class="sxs-lookup"><span data-stu-id="12e33-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="12e33-181">路由前置詞可以包含參數：</span><span class="sxs-lookup"><span data-stu-id="12e33-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="12e33-182">路由條件約束</span><span class="sxs-lookup"><span data-stu-id="12e33-182">Route Constraints</span></span>

<span data-ttu-id="12e33-183">路由條件約束，可讓您限制如何比對路由範本中的參數。</span><span class="sxs-lookup"><span data-stu-id="12e33-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="12e33-184">一般語法&quot;{參數： 條件約束}&quot;。</span><span class="sxs-lookup"><span data-stu-id="12e33-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="12e33-185">例如: </span><span class="sxs-lookup"><span data-stu-id="12e33-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="12e33-186">在這裡，第一個路由將只會選取如果&quot;識別碼&quot;區段的 URI 是整數。</span><span class="sxs-lookup"><span data-stu-id="12e33-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="12e33-187">否則，會選擇第二個路由。</span><span class="sxs-lookup"><span data-stu-id="12e33-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="12e33-188">下表列出支援的條件約束。</span><span class="sxs-lookup"><span data-stu-id="12e33-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="12e33-189">條件約束</span><span class="sxs-lookup"><span data-stu-id="12e33-189">Constraint</span></span> | <span data-ttu-id="12e33-190">描述</span><span class="sxs-lookup"><span data-stu-id="12e33-190">Description</span></span> | <span data-ttu-id="12e33-191">範例</span><span class="sxs-lookup"><span data-stu-id="12e33-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="12e33-192">Alpha</span><span class="sxs-lookup"><span data-stu-id="12e33-192">alpha</span></span> | <span data-ttu-id="12e33-193">比對大寫或小寫英文字母字元 (a-z、 A 到 Z)</span><span class="sxs-lookup"><span data-stu-id="12e33-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="12e33-194">{x: alpha}</span><span class="sxs-lookup"><span data-stu-id="12e33-194">{x:alpha}</span></span> |
| <span data-ttu-id="12e33-195">bool</span><span class="sxs-lookup"><span data-stu-id="12e33-195">bool</span></span> | <span data-ttu-id="12e33-196">比對的布林值。</span><span class="sxs-lookup"><span data-stu-id="12e33-196">Matches a Boolean value.</span></span> | <span data-ttu-id="12e33-197">{x: bool}</span><span class="sxs-lookup"><span data-stu-id="12e33-197">{x:bool}</span></span> |
| <span data-ttu-id="12e33-198">datetime</span><span class="sxs-lookup"><span data-stu-id="12e33-198">datetime</span></span> | <span data-ttu-id="12e33-199">相符項目**DateTime**值。</span><span class="sxs-lookup"><span data-stu-id="12e33-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="12e33-200">{x: datetime}</span><span class="sxs-lookup"><span data-stu-id="12e33-200">{x:datetime}</span></span> |
| <span data-ttu-id="12e33-201">decimal</span><span class="sxs-lookup"><span data-stu-id="12e33-201">decimal</span></span> | <span data-ttu-id="12e33-202">符合十進位值。</span><span class="sxs-lookup"><span data-stu-id="12e33-202">Matches a decimal value.</span></span> | <span data-ttu-id="12e33-203">{x:decimal}</span><span class="sxs-lookup"><span data-stu-id="12e33-203">{x:decimal}</span></span> |
| <span data-ttu-id="12e33-204">double</span><span class="sxs-lookup"><span data-stu-id="12e33-204">double</span></span> | <span data-ttu-id="12e33-205">比對 64 位元浮點值。</span><span class="sxs-lookup"><span data-stu-id="12e33-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="12e33-206">{x:double}</span><span class="sxs-lookup"><span data-stu-id="12e33-206">{x:double}</span></span> |
| <span data-ttu-id="12e33-207">float</span><span class="sxs-lookup"><span data-stu-id="12e33-207">float</span></span> | <span data-ttu-id="12e33-208">比對 32 位元浮點值。</span><span class="sxs-lookup"><span data-stu-id="12e33-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="12e33-209">{x: f}</span><span class="sxs-lookup"><span data-stu-id="12e33-209">{x:float}</span></span> |
| <span data-ttu-id="12e33-210">Guid</span><span class="sxs-lookup"><span data-stu-id="12e33-210">guid</span></span> | <span data-ttu-id="12e33-211">比對的 GUID 值。</span><span class="sxs-lookup"><span data-stu-id="12e33-211">Matches a GUID value.</span></span> | <span data-ttu-id="12e33-212">{x: guid}</span><span class="sxs-lookup"><span data-stu-id="12e33-212">{x:guid}</span></span> |
| <span data-ttu-id="12e33-213">int</span><span class="sxs-lookup"><span data-stu-id="12e33-213">int</span></span> | <span data-ttu-id="12e33-214">比對 32 位元整數值。</span><span class="sxs-lookup"><span data-stu-id="12e33-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="12e33-215">{x:int}</span><span class="sxs-lookup"><span data-stu-id="12e33-215">{x:int}</span></span> |
| <span data-ttu-id="12e33-216">長度</span><span class="sxs-lookup"><span data-stu-id="12e33-216">length</span></span> | <span data-ttu-id="12e33-217">比對字串指定長度或長度的指定範圍內。</span><span class="sxs-lookup"><span data-stu-id="12e33-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="12e33-218">{x:length(6)} {x:length(1,20)}</span><span class="sxs-lookup"><span data-stu-id="12e33-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="12e33-219">long</span><span class="sxs-lookup"><span data-stu-id="12e33-219">long</span></span> | <span data-ttu-id="12e33-220">比對 64 位元整數值。</span><span class="sxs-lookup"><span data-stu-id="12e33-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="12e33-221">{x:long}</span><span class="sxs-lookup"><span data-stu-id="12e33-221">{x:long}</span></span> |
| <span data-ttu-id="12e33-222">max</span><span class="sxs-lookup"><span data-stu-id="12e33-222">max</span></span> | <span data-ttu-id="12e33-223">符合最大值的整數。</span><span class="sxs-lookup"><span data-stu-id="12e33-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="12e33-224">{x:max(10)}</span><span class="sxs-lookup"><span data-stu-id="12e33-224">{x:max(10)}</span></span> |
| <span data-ttu-id="12e33-225">maxlength</span><span class="sxs-lookup"><span data-stu-id="12e33-225">maxlength</span></span> | <span data-ttu-id="12e33-226">符合最大長度的字串。</span><span class="sxs-lookup"><span data-stu-id="12e33-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="12e33-227">{x:maxlength(10)}</span><span class="sxs-lookup"><span data-stu-id="12e33-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="12e33-228">min</span><span class="sxs-lookup"><span data-stu-id="12e33-228">min</span></span> | <span data-ttu-id="12e33-229">符合最小值的整數。</span><span class="sxs-lookup"><span data-stu-id="12e33-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="12e33-230">{x:min(10)}</span><span class="sxs-lookup"><span data-stu-id="12e33-230">{x:min(10)}</span></span> |
| <span data-ttu-id="12e33-231">minlength</span><span class="sxs-lookup"><span data-stu-id="12e33-231">minlength</span></span> | <span data-ttu-id="12e33-232">符合最小長度的字串。</span><span class="sxs-lookup"><span data-stu-id="12e33-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="12e33-233">{x:minlength(10)}</span><span class="sxs-lookup"><span data-stu-id="12e33-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="12e33-234">range</span><span class="sxs-lookup"><span data-stu-id="12e33-234">range</span></span> | <span data-ttu-id="12e33-235">比對的值範圍內的整數。</span><span class="sxs-lookup"><span data-stu-id="12e33-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="12e33-236">{x:range(10,50)}</span><span class="sxs-lookup"><span data-stu-id="12e33-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="12e33-237">regex</span><span class="sxs-lookup"><span data-stu-id="12e33-237">regex</span></span> | <span data-ttu-id="12e33-238">符合規則運算式。</span><span class="sxs-lookup"><span data-stu-id="12e33-238">Matches a regular expression.</span></span> | <span data-ttu-id="12e33-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="12e33-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="12e33-240">請注意一些條件約束，例如&quot;min&quot;，接受引數括號括住。</span><span class="sxs-lookup"><span data-stu-id="12e33-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="12e33-241">您可以將多個條件約束套用至參數，以冒號分隔。</span><span class="sxs-lookup"><span data-stu-id="12e33-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="12e33-242">自訂路由條件約束</span><span class="sxs-lookup"><span data-stu-id="12e33-242">Custom Route Constraints</span></span>

<span data-ttu-id="12e33-243">您可以建立自訂的路由條件約束，藉由實作**IHttpRouteConstraint**介面。</span><span class="sxs-lookup"><span data-stu-id="12e33-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="12e33-244">例如，下列條件約束限制參數為非零整數值。</span><span class="sxs-lookup"><span data-stu-id="12e33-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="12e33-245">下列程式碼顯示如何註冊條件約束：</span><span class="sxs-lookup"><span data-stu-id="12e33-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="12e33-246">現在您可以在您的路由中套用條件約束：</span><span class="sxs-lookup"><span data-stu-id="12e33-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="12e33-247">您也可以取代整個**DefaultInlineConstraintResolver**藉由實作類別**IInlineConstraintResolver**介面。</span><span class="sxs-lookup"><span data-stu-id="12e33-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="12e33-248">這樣將會取代所有的內建的條件約束，除非您實作**IInlineConstraintResolver**特別將其加入。</span><span class="sxs-lookup"><span data-stu-id="12e33-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="12e33-249">選擇性 URI 參數和預設值</span><span class="sxs-lookup"><span data-stu-id="12e33-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="12e33-250">您可以讓 URI 參數選擇性加入路由參數的問號。</span><span class="sxs-lookup"><span data-stu-id="12e33-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="12e33-251">路由參數為選擇性，如果您必須定義的方法參數的預設值。</span><span class="sxs-lookup"><span data-stu-id="12e33-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="12e33-252">在此範例中，`/api/books/locale/1033`和`/api/books/locale`傳回相同的資源。</span><span class="sxs-lookup"><span data-stu-id="12e33-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="12e33-253">或者，您可以指定在路由範本中，預設值如下：</span><span class="sxs-lookup"><span data-stu-id="12e33-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="12e33-254">這是與上述範例中，幾乎相同，但是時，會有些微的差異的行為會套用預設值。</span><span class="sxs-lookup"><span data-stu-id="12e33-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="12e33-255">在第一個範例 ("{lcid？}") 中，1033年的預設值是直接指派給方法參數，讓參數會有這個精確的值。</span><span class="sxs-lookup"><span data-stu-id="12e33-255">In the first example ("{lcid?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="12e33-256">在第二個範例中 ("{lcid = 1033年}")，"1033"的預設值會進行模型繫結程序。</span><span class="sxs-lookup"><span data-stu-id="12e33-256">In the second example ("{lcid=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="12e33-257">預設模型繫結器將會轉換成數值 1033年"1033"。</span><span class="sxs-lookup"><span data-stu-id="12e33-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="12e33-258">不過，您可以插入可能會執行一些不同的自訂模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="12e33-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="12e33-259">（在大部分情況下，除非您有自訂的模型繫結器在管線中，兩種形式會是對等）。</span><span class="sxs-lookup"><span data-stu-id="12e33-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="12e33-260">路由名稱</span><span class="sxs-lookup"><span data-stu-id="12e33-260">Route Names</span></span>

<span data-ttu-id="12e33-261">在 Web API 中，每個路由會有一個名稱。</span><span class="sxs-lookup"><span data-stu-id="12e33-261">In Web API, every route has a name.</span></span> <span data-ttu-id="12e33-262">路由名稱可用來產生連結，以便您可以在 HTTP 回應中包含的連結。</span><span class="sxs-lookup"><span data-stu-id="12e33-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="12e33-263">指定的路由名稱，請設定**名稱**屬性上的屬性。</span><span class="sxs-lookup"><span data-stu-id="12e33-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="12e33-264">下列範例會示範如何設定路由名稱，以及如何產生連結時所使用的路由名稱。</span><span class="sxs-lookup"><span data-stu-id="12e33-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="12e33-265">路由順序</span><span class="sxs-lookup"><span data-stu-id="12e33-265">Route Order</span></span>

<span data-ttu-id="12e33-266">架構會嘗試比對具有路由的 URI，它會評估以特定順序中的路由。</span><span class="sxs-lookup"><span data-stu-id="12e33-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="12e33-267">若要指定的順序，將**RouteOrder**路由的屬性上的屬性。</span><span class="sxs-lookup"><span data-stu-id="12e33-267">To specify the order, set the **RouteOrder** property on the route attribute.</span></span> <span data-ttu-id="12e33-268">會先評估較低的值。</span><span class="sxs-lookup"><span data-stu-id="12e33-268">Lower values are evaluated first.</span></span> <span data-ttu-id="12e33-269">預設順序值是零。</span><span class="sxs-lookup"><span data-stu-id="12e33-269">The default order value is zero.</span></span>

<span data-ttu-id="12e33-270">以下是如何決定總排序：</span><span class="sxs-lookup"><span data-stu-id="12e33-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="12e33-271">比較**RouteOrder**路由屬性的屬性。</span><span class="sxs-lookup"><span data-stu-id="12e33-271">Compare the **RouteOrder** property of the route attribute.</span></span>
2. <span data-ttu-id="12e33-272">查看路由範本中每個 URI 區段。</span><span class="sxs-lookup"><span data-stu-id="12e33-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="12e33-273">對於每個區段，排列資料行，如下所示：</span><span class="sxs-lookup"><span data-stu-id="12e33-273">For each segment, order as follows:</span></span> 

    1. <span data-ttu-id="12e33-274">常值的區段。</span><span class="sxs-lookup"><span data-stu-id="12e33-274">Literal segments.</span></span>
    2. <span data-ttu-id="12e33-275">路由參數的條件約束。</span><span class="sxs-lookup"><span data-stu-id="12e33-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="12e33-276">沒有限制路由參數。</span><span class="sxs-lookup"><span data-stu-id="12e33-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="12e33-277">條件約束與萬用字元參數區段。</span><span class="sxs-lookup"><span data-stu-id="12e33-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="12e33-278">萬用字元參數區段沒有限制。</span><span class="sxs-lookup"><span data-stu-id="12e33-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="12e33-279">在繫結的情況下，路由會依不區分大小寫的序數字串比較 ([OrdinalIgnoreCase](https://msdn.microsoft.com/en-us/library/system.stringcomparer.ordinalignorecase.aspx)) 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="12e33-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/en-us/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="12e33-280">以下是一個範例。</span><span class="sxs-lookup"><span data-stu-id="12e33-280">Here is an example.</span></span> <span data-ttu-id="12e33-281">假設您定義下列控制站：</span><span class="sxs-lookup"><span data-stu-id="12e33-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="12e33-282">這些路由會排序，如下所示。</span><span class="sxs-lookup"><span data-stu-id="12e33-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="12e33-283">orders/details</span><span class="sxs-lookup"><span data-stu-id="12e33-283">orders/details</span></span>
2. <span data-ttu-id="12e33-284">orders/{id}</span><span class="sxs-lookup"><span data-stu-id="12e33-284">orders/{id}</span></span>
3. <span data-ttu-id="12e33-285">orders/{customerName}</span><span class="sxs-lookup"><span data-stu-id="12e33-285">orders/{customerName}</span></span>
4. <span data-ttu-id="12e33-286">orders/{\*date}</span><span class="sxs-lookup"><span data-stu-id="12e33-286">orders/{\*date}</span></span>
5. <span data-ttu-id="12e33-287">訂單 / 擱置中</span><span class="sxs-lookup"><span data-stu-id="12e33-287">orders/pending</span></span>

<span data-ttu-id="12e33-288">請注意，[詳細資料] 是常值的區段，"{id}"之前出現，但是 「 擱置 」 會顯示上次因為**RouteOrder**屬性為 1。</span><span class="sxs-lookup"><span data-stu-id="12e33-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **RouteOrder** property is 1.</span></span> <span data-ttu-id="12e33-289">(這個範例那里假設沒有客戶名為 「 詳細資料 」 或 「 暫止 」。</span><span class="sxs-lookup"><span data-stu-id="12e33-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="12e33-290">一般情況下，嘗試避免模稜兩可的路由。</span><span class="sxs-lookup"><span data-stu-id="12e33-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="12e33-291">在此範例中，較佳路由範本`GetByCustomer`是 「 客戶 / {customerName}")</span><span class="sxs-lookup"><span data-stu-id="12e33-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
