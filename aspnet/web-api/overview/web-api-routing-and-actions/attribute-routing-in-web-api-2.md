---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: 屬性 ASP.NET Web API 2 中的路由 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: d16dcc618bf6c60714179601db14f4dd2a9e41ce
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912148"
---
<a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="8b0ec-102">ASP.NET Web API 2 中的屬性路由</span><span class="sxs-lookup"><span data-stu-id="8b0ec-102">Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="8b0ec-103">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8b0ec-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="8b0ec-104">*路由*是 Web API 如何比對動作的 URI。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="8b0ec-105">Web API 2 支援新類型的路由，稱為*屬性路由*。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="8b0ec-106">如同名稱所暗示，屬性路由會使用屬性來定義的路由。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="8b0ec-107">屬性路由可讓您更充分掌控 Uri 在 web API 中。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="8b0ec-108">例如，您可以輕鬆地建立描述階層的資源的 Uri。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="8b0ec-109">呼叫以慣例為基礎的舊版樣式的路由，路由仍然完整支援。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="8b0ec-110">事實上，您可以結合這兩種技巧，在相同的專案。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="8b0ec-111">本主題說明如何啟用屬性路由，並且描述屬性路由的各種選項。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="8b0ec-112">使用屬性路由端對端教學課程中，請參閱[使用 Web API 2 中的屬性路由建立 REST API](create-a-rest-api-with-attribute-routing.md)。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8b0ec-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="8b0ec-113">Prerequisites</span></span>

<span data-ttu-id="8b0ec-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community、 Professional 或 Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="8b0ec-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional, or Enterprise edition</span></span>

<span data-ttu-id="8b0ec-115">或者，使用 NuGet 套件管理員來安裝必要的套件。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="8b0ec-116">從**工具**功能表，在 Visual Studio 中，選取**NuGet 套件管理員**，然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-116">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="8b0ec-117">在 [套件管理員主控台] 視窗中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="8b0ec-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="8b0ec-118">為什麼屬性路由？</span><span class="sxs-lookup"><span data-stu-id="8b0ec-118">Why Attribute Routing?</span></span>

<span data-ttu-id="8b0ec-119">使用 Web API 的第一版*以慣例為基礎*路由。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="8b0ec-120">在這類的路由中，您定義一個或多個路由範本，基本上便是參數化字串。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="8b0ec-121">當架構收到要求時，它會比對路由範本的 URI。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="8b0ec-122">(如需以慣例為基礎的路由的詳細資訊，請參閱[ASP.NET Web API 中的路由](routing-in-aspnet-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="8b0ec-123">以慣例為基礎的路由的優點之一是，範本會定義在單一位置，和路由規則會一致地套用到所有控制站。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="8b0ec-124">不幸的是，將慣例為基礎的路由，讓更難以支援特定常見於 RESTful Api 的 URI 模式。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="8b0ec-125">例如，資源通常會包含子資源： 客戶具有訂單，電影的動作項目，而活頁簿有作者，等等。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="8b0ec-126">很自然地建立反映這些關聯性的 Uri:</span><span class="sxs-lookup"><span data-stu-id="8b0ec-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="8b0ec-127">此類型的 URI 很難建立使用以慣例為基礎的路由。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="8b0ec-128">雖然也可以完成，但如果您有許多的控制站或資源類型，就不會調整結果。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="8b0ec-129">使用屬性路由中，就一般定義的路由，這個 uri。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="8b0ec-130">您只會將屬性新增至控制器動作：</span><span class="sxs-lookup"><span data-stu-id="8b0ec-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="8b0ec-131">以下是一些其他屬性路由可讓您輕鬆的模式。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="8b0ec-132">**API 版本設定**</span><span class="sxs-lookup"><span data-stu-id="8b0ec-132">**API versioning**</span></span>

<span data-ttu-id="8b0ec-133">在此範例中，"/ api/v1/產品"會路由傳送到不同的控制器，比"/ api/v2/產品 」。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`
`/api/v2/products`

<span data-ttu-id="8b0ec-134">**多載的 URI 區段**</span><span class="sxs-lookup"><span data-stu-id="8b0ec-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="8b0ec-135">在此範例中，"1"是順序的數字，但是 「 擱置 」 對應至集合。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`
`/orders/pending`

<span data-ttu-id="8b0ec-136">**多個參數類型**</span><span class="sxs-lookup"><span data-stu-id="8b0ec-136">**Multiple parameter types**</span></span>

<span data-ttu-id="8b0ec-137">在此範例中，"1"是順序的數字，但是"2013年/06/16"指定的日期。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="8b0ec-138">啟用屬性路由</span><span class="sxs-lookup"><span data-stu-id="8b0ec-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="8b0ec-139">若要啟用屬性路由，請呼叫**MapHttpAttributeRoutes**設定期間。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="8b0ec-140">這個擴充方法中定義**System.Web.Http.HttpConfigurationExtensions**類別。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="8b0ec-141">屬性路由可以結合[以慣例為基礎](routing-in-aspnet-web-api.md)路由。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="8b0ec-142">若要定義以慣例為基礎的路由，請呼叫**MapHttpRoute**方法。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="8b0ec-143">如需有關如何設定 Web API 的詳細資訊，請參閱 <<c0> [ 設定 ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="8b0ec-144">注意： 從 Web API 1 進行移轉</span><span class="sxs-lookup"><span data-stu-id="8b0ec-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="8b0ec-145">Web API 2 之前, 的 Web API 專案範本會產生如下的程式碼：</span><span class="sxs-lookup"><span data-stu-id="8b0ec-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="8b0ec-146">如果已啟用屬性路由，此程式碼會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="8b0ec-147">如果您升級現有的 Web API 專案以使用屬性路由，請務必更新這個組態程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="8b0ec-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="8b0ec-148">如需詳細資訊，請參閱 <<c0> [ 設定與 ASP.NET 裝載的 Web API](../advanced/configuring-aspnet-web-api.md#webhost)。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="8b0ec-149">新增路由屬性</span><span class="sxs-lookup"><span data-stu-id="8b0ec-149">Adding Route Attributes</span></span>

<span data-ttu-id="8b0ec-150">使用屬性來定義路由的範例如下：</span><span class="sxs-lookup"><span data-stu-id="8b0ec-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="8b0ec-151">字串&quot;客戶 / {customerId} /&quot;是路由的 URI 範本。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="8b0ec-152">Web API 會嘗試比對要求的 URI 範本。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="8b0ec-153">在此範例中，「 客戶 」 和 「 訂單 」 是常值的區段，且"{customerId}"變數的參數。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="8b0ec-154">下列 Uri 會符合此範本：</span><span class="sxs-lookup"><span data-stu-id="8b0ec-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="8b0ec-155">您可以藉由比對來限制[條件約束](#constraints)，本主題稍後所述。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="8b0ec-156">請注意， &quot;{customerId}&quot;路由範本中的參數名稱相符*customerId*方法中的參數。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="8b0ec-157">當 Web API 會叫用控制器動作時，它會嘗試將路由參數繫結。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="8b0ec-158">比方說，如果 URI 為`http://example.com/customers/1/orders`，Web API 會嘗試將值"1"的繫結*customerId*動作中的參數。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="8b0ec-159">URI 樣板可以有數個參數：</span><span class="sxs-lookup"><span data-stu-id="8b0ec-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="8b0ec-160">路由屬性沒有任何控制器方法會使用以慣例為基礎的路由。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="8b0ec-161">如此一來，您可以結合這兩種類型的同一個專案中的路由。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="8b0ec-162">HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="8b0ec-162">HTTP Methods</span></span>

<span data-ttu-id="8b0ec-163">Web API，也會根據要求的 HTTP 方法 （GET、 POST 等） 的動作。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="8b0ec-164">根據預設，Web API 會尋找不區分大小寫的比對控制器方法名稱的開頭。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="8b0ec-165">例如，名為控制器方法`PutCustomers`符合 HTTP PUT 要求。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="8b0ec-166">您可以覆寫此慣例裝飾的方法與任何下列屬性：</span><span class="sxs-lookup"><span data-stu-id="8b0ec-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="8b0ec-167">**[HttpDelete]**</span><span class="sxs-lookup"><span data-stu-id="8b0ec-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="8b0ec-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="8b0ec-168">**[HttpGet]**</span></span>
- <span data-ttu-id="8b0ec-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="8b0ec-169">**[HttpHead]**</span></span>
- <span data-ttu-id="8b0ec-170">**[HttpOptions]**</span><span class="sxs-lookup"><span data-stu-id="8b0ec-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="8b0ec-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="8b0ec-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="8b0ec-172">**[HttpPost]**</span><span class="sxs-lookup"><span data-stu-id="8b0ec-172">**[HttpPost]**</span></span>
- <span data-ttu-id="8b0ec-173">**[HttpPut]**</span><span class="sxs-lookup"><span data-stu-id="8b0ec-173">**[HttpPut]**</span></span>

<span data-ttu-id="8b0ec-174">下列範例會將 CreateBook 方法對應至 HTTP POST 要求。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="8b0ec-175">對於所有其他 HTTP 方法，包括非標準的方法，請使用**AcceptVerbs**屬性，它會使用 HTTP 方法清單。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="8b0ec-176">路由前置詞</span><span class="sxs-lookup"><span data-stu-id="8b0ec-176">Route Prefixes</span></span>

<span data-ttu-id="8b0ec-177">通常中的路由所有以相同的前置詞開頭的控制站。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="8b0ec-178">例如: </span><span class="sxs-lookup"><span data-stu-id="8b0ec-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="8b0ec-179">您也可以使用整個控制器設定一般的前置詞 **[RoutePrefix]** 屬性：</span><span class="sxs-lookup"><span data-stu-id="8b0ec-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="8b0ec-180">方法的屬性上使用波狀符號 （~），來覆寫的路由前置詞：</span><span class="sxs-lookup"><span data-stu-id="8b0ec-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="8b0ec-181">路由前置詞可以包含參數：</span><span class="sxs-lookup"><span data-stu-id="8b0ec-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="8b0ec-182">路由條件約束</span><span class="sxs-lookup"><span data-stu-id="8b0ec-182">Route Constraints</span></span>

<span data-ttu-id="8b0ec-183">路由條件約束可讓您限制如何比對路由範本中的參數。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="8b0ec-184">一般語法&quot;{參數： 條件約束}&quot;。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="8b0ec-185">例如: </span><span class="sxs-lookup"><span data-stu-id="8b0ec-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="8b0ec-186">在這裡，第一個路由將只會選取如果&quot;識別碼&quot;URI 區段是一個整數。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="8b0ec-187">否則，將會選擇第二個路由。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="8b0ec-188">下表列出支援的條件約束。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="8b0ec-189">條件約束</span><span class="sxs-lookup"><span data-stu-id="8b0ec-189">Constraint</span></span> | <span data-ttu-id="8b0ec-190">描述</span><span class="sxs-lookup"><span data-stu-id="8b0ec-190">Description</span></span> | <span data-ttu-id="8b0ec-191">範例</span><span class="sxs-lookup"><span data-stu-id="8b0ec-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8b0ec-192">alpha</span><span class="sxs-lookup"><span data-stu-id="8b0ec-192">alpha</span></span> | <span data-ttu-id="8b0ec-193">比對大寫或小寫英文字母字元 (a-z、 A-Z)</span><span class="sxs-lookup"><span data-stu-id="8b0ec-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="8b0ec-194">{x: alpha}</span><span class="sxs-lookup"><span data-stu-id="8b0ec-194">{x:alpha}</span></span> |
| <span data-ttu-id="8b0ec-195">bool</span><span class="sxs-lookup"><span data-stu-id="8b0ec-195">bool</span></span> | <span data-ttu-id="8b0ec-196">布林值，會比對。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-196">Matches a Boolean value.</span></span> | <span data-ttu-id="8b0ec-197">{x: bool}</span><span class="sxs-lookup"><span data-stu-id="8b0ec-197">{x:bool}</span></span> |
| <span data-ttu-id="8b0ec-198">datetime</span><span class="sxs-lookup"><span data-stu-id="8b0ec-198">datetime</span></span> | <span data-ttu-id="8b0ec-199">相符項目**DateTime**值。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="8b0ec-200">{x: datetime}</span><span class="sxs-lookup"><span data-stu-id="8b0ec-200">{x:datetime}</span></span> |
| <span data-ttu-id="8b0ec-201">decimal</span><span class="sxs-lookup"><span data-stu-id="8b0ec-201">decimal</span></span> | <span data-ttu-id="8b0ec-202">符合十進位值。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-202">Matches a decimal value.</span></span> | <span data-ttu-id="8b0ec-203">{x： 十進位}</span><span class="sxs-lookup"><span data-stu-id="8b0ec-203">{x:decimal}</span></span> |
| <span data-ttu-id="8b0ec-204">double</span><span class="sxs-lookup"><span data-stu-id="8b0ec-204">double</span></span> | <span data-ttu-id="8b0ec-205">比對 64 位元浮點值。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="8b0ec-206">{x： 雙}</span><span class="sxs-lookup"><span data-stu-id="8b0ec-206">{x:double}</span></span> |
| <span data-ttu-id="8b0ec-207">float</span><span class="sxs-lookup"><span data-stu-id="8b0ec-207">float</span></span> | <span data-ttu-id="8b0ec-208">比對 32 位元浮點值。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="8b0ec-209">{x: float}</span><span class="sxs-lookup"><span data-stu-id="8b0ec-209">{x:float}</span></span> |
| <span data-ttu-id="8b0ec-210">guid</span><span class="sxs-lookup"><span data-stu-id="8b0ec-210">guid</span></span> | <span data-ttu-id="8b0ec-211">比對的 GUID 值。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-211">Matches a GUID value.</span></span> | <span data-ttu-id="8b0ec-212">{x: guid}</span><span class="sxs-lookup"><span data-stu-id="8b0ec-212">{x:guid}</span></span> |
| <span data-ttu-id="8b0ec-213">int</span><span class="sxs-lookup"><span data-stu-id="8b0ec-213">int</span></span> | <span data-ttu-id="8b0ec-214">比對 32 位元整數值。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="8b0ec-215">{x: int}</span><span class="sxs-lookup"><span data-stu-id="8b0ec-215">{x:int}</span></span> |
| <span data-ttu-id="8b0ec-216">長度</span><span class="sxs-lookup"><span data-stu-id="8b0ec-216">length</span></span> | <span data-ttu-id="8b0ec-217">符合使用指定的長度，或在指定的長度範圍內的字串。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="8b0ec-218">{x: length(6)}{x: length(1,20)}</span><span class="sxs-lookup"><span data-stu-id="8b0ec-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="8b0ec-219">long</span><span class="sxs-lookup"><span data-stu-id="8b0ec-219">long</span></span> | <span data-ttu-id="8b0ec-220">比對 64 位元整數值。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="8b0ec-221">{x： 長時間}</span><span class="sxs-lookup"><span data-stu-id="8b0ec-221">{x:long}</span></span> |
| <span data-ttu-id="8b0ec-222">max</span><span class="sxs-lookup"><span data-stu-id="8b0ec-222">max</span></span> | <span data-ttu-id="8b0ec-223">比對的最大值的整數。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="8b0ec-224">{x:max(10)}</span><span class="sxs-lookup"><span data-stu-id="8b0ec-224">{x:max(10)}</span></span> |
| <span data-ttu-id="8b0ec-225">maxlength</span><span class="sxs-lookup"><span data-stu-id="8b0ec-225">maxlength</span></span> | <span data-ttu-id="8b0ec-226">符合最大長度的字串。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="8b0ec-227">{x: maxlength(10)}</span><span class="sxs-lookup"><span data-stu-id="8b0ec-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="8b0ec-228">min</span><span class="sxs-lookup"><span data-stu-id="8b0ec-228">min</span></span> | <span data-ttu-id="8b0ec-229">符合最小值的整數。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="8b0ec-230">{x: min(10)}</span><span class="sxs-lookup"><span data-stu-id="8b0ec-230">{x:min(10)}</span></span> |
| <span data-ttu-id="8b0ec-231">minlength</span><span class="sxs-lookup"><span data-stu-id="8b0ec-231">minlength</span></span> | <span data-ttu-id="8b0ec-232">符合最小長度的字串。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="8b0ec-233">{x: minlength(10)}</span><span class="sxs-lookup"><span data-stu-id="8b0ec-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="8b0ec-234">range</span><span class="sxs-lookup"><span data-stu-id="8b0ec-234">range</span></span> | <span data-ttu-id="8b0ec-235">比對的值範圍內的整數。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="8b0ec-236">{x: range(10,50)}</span><span class="sxs-lookup"><span data-stu-id="8b0ec-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="8b0ec-237">regex</span><span class="sxs-lookup"><span data-stu-id="8b0ec-237">regex</span></span> | <span data-ttu-id="8b0ec-238">符合規則運算式。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-238">Matches a regular expression.</span></span> | <span data-ttu-id="8b0ec-239">{x: regex(^\d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="8b0ec-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="8b0ec-240">請注意一些條件約束，例如&quot;min&quot;，接受括號括住的引數。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="8b0ec-241">您可以將多個條件約束套用至參數，並以冒號分隔。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="8b0ec-242">自訂路由條件約束</span><span class="sxs-lookup"><span data-stu-id="8b0ec-242">Custom Route Constraints</span></span>

<span data-ttu-id="8b0ec-243">您可以建立自訂路由條件約束，藉由實作**IHttpRouteConstraint**介面。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="8b0ec-244">例如，下列條件約束限制參數為非零的整數值。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="8b0ec-245">下列程式碼示範如何註冊條件約束：</span><span class="sxs-lookup"><span data-stu-id="8b0ec-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="8b0ec-246">現在您可以套用條件約束，在您的路由：</span><span class="sxs-lookup"><span data-stu-id="8b0ec-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="8b0ec-247">您也可以取代整個**DefaultInlineConstraintResolver**藉由實作類別**IInlineConstraintResolver**介面。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="8b0ec-248">這樣將會取代所有的內建的條件約束，除非您實作**IInlineConstraintResolver**特別將其加入。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="8b0ec-249">選擇性 URI 參數和預設值</span><span class="sxs-lookup"><span data-stu-id="8b0ec-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="8b0ec-250">您可以讓 URI 參數選擇性加入路由參數的問號。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="8b0ec-251">如果路由參數為選擇性的您必須定義方法參數的預設值。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="8b0ec-252">在此範例中，`/api/books/locale/1033`和`/api/books/locale`傳回相同的資源。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="8b0ec-253">或者，您可以依下列方式指定在路由範本中，預設值：</span><span class="sxs-lookup"><span data-stu-id="8b0ec-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="8b0ec-254">這是上述範例中，幾乎相同，但有所些微差異的行為時所套用的預設值。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="8b0ec-255">在第一個範例 ("{lcid？}") 中，1033年的預設值是直接指派給方法參數，讓參數會將此的確切值。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-255">In the first example ("{lcid?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="8b0ec-256">在第二個範例中 ("{lcid = 1033年}")，"1033"的預設值會透過模型繫結程序。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-256">In the second example ("{lcid=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="8b0ec-257">預設模型繫結器將轉換成數值 1033年"1033"。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="8b0ec-258">不過，您可以插入可能會執行一些不同的自訂模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="8b0ec-259">（在大部分情況下，除非您有自訂模型繫結器在管線中，兩種形式會是對等）。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="8b0ec-260">路由名稱</span><span class="sxs-lookup"><span data-stu-id="8b0ec-260">Route Names</span></span>

<span data-ttu-id="8b0ec-261">在 Web API 中，每個路由會有一個名稱。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-261">In Web API, every route has a name.</span></span> <span data-ttu-id="8b0ec-262">路由名稱適合用於產生連結，以便您可以在 HTTP 回應中包含的連結。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="8b0ec-263">指定的路由名稱，請設定**名稱**屬性上的屬性。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="8b0ec-264">下列範例會示範如何設定路由名稱，以及如何產生連結時所使用的路由名稱。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="8b0ec-265">路由順序</span><span class="sxs-lookup"><span data-stu-id="8b0ec-265">Route Order</span></span>

<span data-ttu-id="8b0ec-266">當此架構會嘗試比對路由的 URI 時，它會評估以特定順序的路由。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="8b0ec-267">若要指定的順序，將**RouteOrder**屬性路由。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-267">To specify the order, set the **RouteOrder** property on the route attribute.</span></span> <span data-ttu-id="8b0ec-268">會先評估較低的值。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-268">Lower values are evaluated first.</span></span> <span data-ttu-id="8b0ec-269">預設順序值為零。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-269">The default order value is zero.</span></span>

<span data-ttu-id="8b0ec-270">以下是如何決定總排序：</span><span class="sxs-lookup"><span data-stu-id="8b0ec-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="8b0ec-271">比較**RouteOrder**路由屬性的屬性。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-271">Compare the **RouteOrder** property of the route attribute.</span></span>
2. <span data-ttu-id="8b0ec-272">查看路由範本中的每個 URI 區段。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="8b0ec-273">對於每個區段中，排序，如下所示：</span><span class="sxs-lookup"><span data-stu-id="8b0ec-273">For each segment, order as follows:</span></span>

    1. <span data-ttu-id="8b0ec-274">常值的區段。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-274">Literal segments.</span></span>
    2. <span data-ttu-id="8b0ec-275">路由條件約束使用的參數。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="8b0ec-276">路由參數，而不需要條件約束。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="8b0ec-277">使用條件約束的萬用字元參數區段。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="8b0ec-278">萬用字元參數區段，而不需要條件約束。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="8b0ec-279">在繫結的情況下，路由會依不區分大小寫的序數字串比較 ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="8b0ec-280">以下是一個範例。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-280">Here is an example.</span></span> <span data-ttu-id="8b0ec-281">假設您會定義下列控制器：</span><span class="sxs-lookup"><span data-stu-id="8b0ec-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="8b0ec-282">這些路由會排序，如下所示。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="8b0ec-283">訂單/詳細資料</span><span class="sxs-lookup"><span data-stu-id="8b0ec-283">orders/details</span></span>
2. <span data-ttu-id="8b0ec-284">訂單 / {id}</span><span class="sxs-lookup"><span data-stu-id="8b0ec-284">orders/{id}</span></span>
3. <span data-ttu-id="8b0ec-285">orders/{customerName}</span><span class="sxs-lookup"><span data-stu-id="8b0ec-285">orders/{customerName}</span></span>
4. <span data-ttu-id="8b0ec-286">orders/{\*date}</span><span class="sxs-lookup"><span data-stu-id="8b0ec-286">orders/{\*date}</span></span>
5. <span data-ttu-id="8b0ec-287">訂單 / 擱置中</span><span class="sxs-lookup"><span data-stu-id="8b0ec-287">orders/pending</span></span>

<span data-ttu-id="8b0ec-288">請注意，[詳細資料] 是常值的區段以及"{id}"之前出現，但"pending"會顯示上次因為**RouteOrder**屬性為 1。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **RouteOrder** property is 1.</span></span> <span data-ttu-id="8b0ec-289">(這個範例那里假設客戶名為 「 詳細資料 」 或 「 暫止 」。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="8b0ec-290">一般情況下，請盡量避免模稜兩可的路由。</span><span class="sxs-lookup"><span data-stu-id="8b0ec-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="8b0ec-291">在此範例中，較佳的路由範本，如`GetByCustomer`是 「 客戶 / {customerName}")</span><span class="sxs-lookup"><span data-stu-id="8b0ec-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
