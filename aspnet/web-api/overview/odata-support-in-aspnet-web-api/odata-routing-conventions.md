---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: "路由慣例，在 ASP.NET Web API 2 Odata |Microsoft 文件"
author: MikeWasson
description: "本文說明 Web API OData 端點所使用的路由慣例。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/31/2013
ms.topic: article
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 0ab99dd443040b90ffefd2f5b9261a63b91e9463
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="routing-conventions-in-aspnet-web-api-2-odata"></a><span data-ttu-id="85955-103">路由慣例，在 ASP.NET Web API 2 Odata</span><span class="sxs-lookup"><span data-stu-id="85955-103">Routing Conventions in ASP.NET Web API 2 Odata</span></span>
====================
<span data-ttu-id="85955-104">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="85955-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="85955-105">本文說明 Web API OData 端點所使用的路由慣例。</span><span class="sxs-lookup"><span data-stu-id="85955-105">This article describes the routing conventions that Web API uses for OData endpoints.</span></span>


<span data-ttu-id="85955-106">當 Web API 取得 OData 要求時，它會將要求對應至控制器名稱和動作名稱中。</span><span class="sxs-lookup"><span data-stu-id="85955-106">When Web API gets an OData request, it maps the request to a controller name and an action name.</span></span> <span data-ttu-id="85955-107">對應為基礎的 HTTP 方法和 URI。</span><span class="sxs-lookup"><span data-stu-id="85955-107">The mapping is based on the HTTP method and the URI.</span></span> <span data-ttu-id="85955-108">例如，`GET /odata/Products(1)`對應至`ProductsController.GetProduct`。</span><span class="sxs-lookup"><span data-stu-id="85955-108">For example, `GET /odata/Products(1)` maps to `ProductsController.GetProduct`.</span></span>

<span data-ttu-id="85955-109">在本文的第 1 部分，我會描述的內建的 OData 路由慣例。</span><span class="sxs-lookup"><span data-stu-id="85955-109">In part 1 of this article, I describe the built-in OData routing conventions.</span></span> <span data-ttu-id="85955-110">這些慣例專為 OData 端點，這些屬性取代預設的 Web API 路由系統。</span><span class="sxs-lookup"><span data-stu-id="85955-110">These conventions are designed specifically for OData endpoints, and they replace the default Web API routing system.</span></span> <span data-ttu-id="85955-111">(當您呼叫時，會發生取代**MapODataRoute**。)</span><span class="sxs-lookup"><span data-stu-id="85955-111">(The replacement happens when you call **MapODataRoute**.)</span></span>

<span data-ttu-id="85955-112">在第 2 部分，我會示範如何加入自訂路由慣例。</span><span class="sxs-lookup"><span data-stu-id="85955-112">In part 2, I show how to add custom routing conventions.</span></span> <span data-ttu-id="85955-113">目前的內建的慣例不涵蓋整個範圍的 OData Uri，但您可以將其處理其他情況下擴充。</span><span class="sxs-lookup"><span data-stu-id="85955-113">Currently the built-in conventions do not cover the entire range of OData URIs, but you can extend them to handle additional cases.</span></span>

- [<span data-ttu-id="85955-114">內建路由慣例</span><span class="sxs-lookup"><span data-stu-id="85955-114">Built-in Routing Conventions</span></span>](#conventions)
- [<span data-ttu-id="85955-115">自訂路由慣例</span><span class="sxs-lookup"><span data-stu-id="85955-115">Custom Routing Conventions</span></span>](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a><span data-ttu-id="85955-116">內建路由慣例</span><span class="sxs-lookup"><span data-stu-id="85955-116">Built-in Routing Conventions</span></span>

<span data-ttu-id="85955-117">我描述 Web API 中的 OData 路由慣例之前，最好先了解 OData Uri。</span><span class="sxs-lookup"><span data-stu-id="85955-117">Before I describe the OData routing conventions in Web API, it's helpful to understand OData URIs.</span></span> <span data-ttu-id="85955-118">[OData URI](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/)所組成：</span><span class="sxs-lookup"><span data-stu-id="85955-118">An [OData URI](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/) consists of:</span></span>

- <span data-ttu-id="85955-119">服務根目錄</span><span class="sxs-lookup"><span data-stu-id="85955-119">The service root</span></span>
- <span data-ttu-id="85955-120">資源路徑</span><span class="sxs-lookup"><span data-stu-id="85955-120">The resource path</span></span>
- <span data-ttu-id="85955-121">查詢選項</span><span class="sxs-lookup"><span data-stu-id="85955-121">Query options</span></span>

![](odata-routing-conventions/_static/image1.png)

<span data-ttu-id="85955-122">路由，重要的部分是資源路徑。</span><span class="sxs-lookup"><span data-stu-id="85955-122">For routing, the important part is the resource path.</span></span> <span data-ttu-id="85955-123">資源路徑會分成區段。</span><span class="sxs-lookup"><span data-stu-id="85955-123">The resource path is divided into segments.</span></span> <span data-ttu-id="85955-124">例如，`/Products(1)/Supplier`有三個區段：</span><span class="sxs-lookup"><span data-stu-id="85955-124">For example, `/Products(1)/Supplier` has three segments:</span></span>

- <span data-ttu-id="85955-125">`Products`指的是名為 「 產品 」 實體集。</span><span class="sxs-lookup"><span data-stu-id="85955-125">`Products` refers to an entity set named "Products".</span></span>
- <span data-ttu-id="85955-126">`1`從集合中選取單一實體為實體索引鍵。</span><span class="sxs-lookup"><span data-stu-id="85955-126">`1` is an entity key, selecting a single entity from the set.</span></span>
- <span data-ttu-id="85955-127">`Supplier`為選取相關的實體的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="85955-127">`Supplier` is a navigation property that selects a related entity.</span></span>

<span data-ttu-id="85955-128">因此這個路徑會挑選出 1 產品的供應商。</span><span class="sxs-lookup"><span data-stu-id="85955-128">So this path picks out the supplier of product 1.</span></span>

> [!NOTE]
> <span data-ttu-id="85955-129">OData 路徑區段未一律對應至 URI 區段。</span><span class="sxs-lookup"><span data-stu-id="85955-129">OData path segments do not always correspond to URI segments.</span></span> <span data-ttu-id="85955-130">例如，"1"會被視為路徑區段。</span><span class="sxs-lookup"><span data-stu-id="85955-130">For example, "1" is considered a path segment.</span></span>


<span data-ttu-id="85955-131">**控制器名稱。**</span><span class="sxs-lookup"><span data-stu-id="85955-131">**Controller Names.**</span></span> <span data-ttu-id="85955-132">控制器名稱一定被衍生自的實體集資源路徑的根目錄。</span><span class="sxs-lookup"><span data-stu-id="85955-132">The controller name is always derived from the entity set at the root of the resource path.</span></span> <span data-ttu-id="85955-133">例如，如果資源路徑是`/Products(1)/Supplier`，Web API 會尋找名為控制器`ProductsController`。</span><span class="sxs-lookup"><span data-stu-id="85955-133">For example, if the resource path is `/Products(1)/Supplier`, Web API looks for a controller named `ProductsController`.</span></span>

<span data-ttu-id="85955-134">**動作名稱。**</span><span class="sxs-lookup"><span data-stu-id="85955-134">**Action Names.**</span></span> <span data-ttu-id="85955-135">下列表格所列的動作名稱衍生自的路徑區段，再加上的實體資料模型 (EDM)。</span><span class="sxs-lookup"><span data-stu-id="85955-135">Action names are derived from the path segments plus the entity data model (EDM), as listed in the following tables.</span></span> <span data-ttu-id="85955-136">在某些情況下，您會有兩種選項的動作名稱。</span><span class="sxs-lookup"><span data-stu-id="85955-136">In some cases, you have two choices for the action name.</span></span> <span data-ttu-id="85955-137">例如，「 取得 」 或&quot;GetProducts&quot;。</span><span class="sxs-lookup"><span data-stu-id="85955-137">For example, "Get" or &quot;GetProducts&quot;.</span></span>

<span data-ttu-id="85955-138">**查詢實體**</span><span class="sxs-lookup"><span data-stu-id="85955-138">**Querying Entities**</span></span>

| <span data-ttu-id="85955-139">要求</span><span class="sxs-lookup"><span data-stu-id="85955-139">Request</span></span> | <span data-ttu-id="85955-140">範例 URI</span><span class="sxs-lookup"><span data-stu-id="85955-140">Example URI</span></span> | <span data-ttu-id="85955-141">動作名稱</span><span class="sxs-lookup"><span data-stu-id="85955-141">Action Name</span></span> | <span data-ttu-id="85955-142">範例動作</span><span class="sxs-lookup"><span data-stu-id="85955-142">Example Action</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="85955-143">取得 /entityset</span><span class="sxs-lookup"><span data-stu-id="85955-143">GET /entityset</span></span> | <span data-ttu-id="85955-144">/ 產品</span><span class="sxs-lookup"><span data-stu-id="85955-144">/Products</span></span> | <span data-ttu-id="85955-145">GetEntitySet 或 Get</span><span class="sxs-lookup"><span data-stu-id="85955-145">GetEntitySet or Get</span></span> | <span data-ttu-id="85955-146">GetProducts</span><span class="sxs-lookup"><span data-stu-id="85955-146">GetProducts</span></span> |
| <span data-ttu-id="85955-147">取得 /entityset(key)</span><span class="sxs-lookup"><span data-stu-id="85955-147">GET /entityset(key)</span></span> | <span data-ttu-id="85955-148">/Products(1)</span><span class="sxs-lookup"><span data-stu-id="85955-148">/Products(1)</span></span> | <span data-ttu-id="85955-149">GetEntityType 或 Get</span><span class="sxs-lookup"><span data-stu-id="85955-149">GetEntityType or Get</span></span> | <span data-ttu-id="85955-150">GetProduct</span><span class="sxs-lookup"><span data-stu-id="85955-150">GetProduct</span></span> |
| <span data-ttu-id="85955-151">取得 /entityset （金鑰）/轉換</span><span class="sxs-lookup"><span data-stu-id="85955-151">GET /entityset(key)/cast</span></span> | <span data-ttu-id="85955-152">/Products(1)/Models.Book</span><span class="sxs-lookup"><span data-stu-id="85955-152">/Products(1)/Models.Book</span></span> | <span data-ttu-id="85955-153">GetEntityType 或 Get</span><span class="sxs-lookup"><span data-stu-id="85955-153">GetEntityType or Get</span></span> | <span data-ttu-id="85955-154">GetBook</span><span class="sxs-lookup"><span data-stu-id="85955-154">GetBook</span></span> |

<span data-ttu-id="85955-155">如需詳細資訊，請參閱[建立唯讀的 OData 端點](odata-v3/creating-an-odata-endpoint.md)。</span><span class="sxs-lookup"><span data-stu-id="85955-155">For more information, see [Create a Read-Only OData Endpoint](odata-v3/creating-an-odata-endpoint.md).</span></span>

<span data-ttu-id="85955-156">**建立、 更新及刪除實體**</span><span class="sxs-lookup"><span data-stu-id="85955-156">**Creating, Updating, and Deleting Entities**</span></span>

| <span data-ttu-id="85955-157">要求</span><span class="sxs-lookup"><span data-stu-id="85955-157">Request</span></span> | <span data-ttu-id="85955-158">範例 URI</span><span class="sxs-lookup"><span data-stu-id="85955-158">Example URI</span></span> | <span data-ttu-id="85955-159">動作名稱</span><span class="sxs-lookup"><span data-stu-id="85955-159">Action Name</span></span> | <span data-ttu-id="85955-160">範例動作</span><span class="sxs-lookup"><span data-stu-id="85955-160">Example Action</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="85955-161">張貼 /entityset</span><span class="sxs-lookup"><span data-stu-id="85955-161">POST /entityset</span></span> | <span data-ttu-id="85955-162">/ 產品</span><span class="sxs-lookup"><span data-stu-id="85955-162">/Products</span></span> | <span data-ttu-id="85955-163">PostEntityType 或 Post</span><span class="sxs-lookup"><span data-stu-id="85955-163">PostEntityType or Post</span></span> | <span data-ttu-id="85955-164">PostProduct</span><span class="sxs-lookup"><span data-stu-id="85955-164">PostProduct</span></span> |
| <span data-ttu-id="85955-165">PUT /entityset(key)</span><span class="sxs-lookup"><span data-stu-id="85955-165">PUT /entityset(key)</span></span> | <span data-ttu-id="85955-166">/Products(1)</span><span class="sxs-lookup"><span data-stu-id="85955-166">/Products(1)</span></span> | <span data-ttu-id="85955-167">PutEntityType 或 Put</span><span class="sxs-lookup"><span data-stu-id="85955-167">PutEntityType or Put</span></span> | <span data-ttu-id="85955-168">PutProduct</span><span class="sxs-lookup"><span data-stu-id="85955-168">PutProduct</span></span> |
| <span data-ttu-id="85955-169">PUT /entityset （金鑰）/轉換</span><span class="sxs-lookup"><span data-stu-id="85955-169">PUT /entityset(key)/cast</span></span> | <span data-ttu-id="85955-170">/Products(1)/Models.Book</span><span class="sxs-lookup"><span data-stu-id="85955-170">/Products(1)/Models.Book</span></span> | <span data-ttu-id="85955-171">PutEntityType 或 Put</span><span class="sxs-lookup"><span data-stu-id="85955-171">PutEntityType or Put</span></span> | <span data-ttu-id="85955-172">PutBook</span><span class="sxs-lookup"><span data-stu-id="85955-172">PutBook</span></span> |
| <span data-ttu-id="85955-173">修補程式 /entityset(key)</span><span class="sxs-lookup"><span data-stu-id="85955-173">PATCH /entityset(key)</span></span> | <span data-ttu-id="85955-174">/Products(1)</span><span class="sxs-lookup"><span data-stu-id="85955-174">/Products(1)</span></span> | <span data-ttu-id="85955-175">PatchEntityType 或修補程式</span><span class="sxs-lookup"><span data-stu-id="85955-175">PatchEntityType or Patch</span></span> | <span data-ttu-id="85955-176">PatchProduct</span><span class="sxs-lookup"><span data-stu-id="85955-176">PatchProduct</span></span> |
| <span data-ttu-id="85955-177">修補程式 /entityset （金鑰）/轉換</span><span class="sxs-lookup"><span data-stu-id="85955-177">PATCH /entityset(key)/cast</span></span> | <span data-ttu-id="85955-178">/Products(1)/Models.Book</span><span class="sxs-lookup"><span data-stu-id="85955-178">/Products(1)/Models.Book</span></span> | <span data-ttu-id="85955-179">PatchEntityType 或修補程式</span><span class="sxs-lookup"><span data-stu-id="85955-179">PatchEntityType or Patch</span></span> | <span data-ttu-id="85955-180">PatchBook</span><span class="sxs-lookup"><span data-stu-id="85955-180">PatchBook</span></span> |
| <span data-ttu-id="85955-181">DELETE /entityset(key)</span><span class="sxs-lookup"><span data-stu-id="85955-181">DELETE /entityset(key)</span></span> | <span data-ttu-id="85955-182">/Products(1)</span><span class="sxs-lookup"><span data-stu-id="85955-182">/Products(1)</span></span> | <span data-ttu-id="85955-183">DeleteEntityType 或刪除</span><span class="sxs-lookup"><span data-stu-id="85955-183">DeleteEntityType or Delete</span></span> | <span data-ttu-id="85955-184">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="85955-184">DeleteProduct</span></span> |
| <span data-ttu-id="85955-185">DELETE /entityset(key)/cast</span><span class="sxs-lookup"><span data-stu-id="85955-185">DELETE /entityset(key)/cast</span></span> | <span data-ttu-id="85955-186">/Products(1)/Models.Book</span><span class="sxs-lookup"><span data-stu-id="85955-186">/Products(1)/Models.Book</span></span> | <span data-ttu-id="85955-187">DeleteEntityType 或刪除</span><span class="sxs-lookup"><span data-stu-id="85955-187">DeleteEntityType or Delete</span></span> | <span data-ttu-id="85955-188">DeleteBook</span><span class="sxs-lookup"><span data-stu-id="85955-188">DeleteBook</span></span> |

<span data-ttu-id="85955-189">**查詢的導覽屬性**</span><span class="sxs-lookup"><span data-stu-id="85955-189">**Querying a Navigation Property**</span></span>

| <span data-ttu-id="85955-190">要求</span><span class="sxs-lookup"><span data-stu-id="85955-190">Request</span></span> | <span data-ttu-id="85955-191">範例 URI</span><span class="sxs-lookup"><span data-stu-id="85955-191">Example URI</span></span> | <span data-ttu-id="85955-192">動作名稱</span><span class="sxs-lookup"><span data-stu-id="85955-192">Action Name</span></span> | <span data-ttu-id="85955-193">範例動作</span><span class="sxs-lookup"><span data-stu-id="85955-193">Example Action</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="85955-194">GET /entityset （金鑰） / 瀏覽</span><span class="sxs-lookup"><span data-stu-id="85955-194">GET /entityset(key)/navigation</span></span> | <span data-ttu-id="85955-195">/Products(1)/Supplier</span><span class="sxs-lookup"><span data-stu-id="85955-195">/Products(1)/Supplier</span></span> | <span data-ttu-id="85955-196">GetNavigationFromEntityType or GetNavigation</span><span class="sxs-lookup"><span data-stu-id="85955-196">GetNavigationFromEntityType or GetNavigation</span></span> | <span data-ttu-id="85955-197">GetSupplierFromProduct</span><span class="sxs-lookup"><span data-stu-id="85955-197">GetSupplierFromProduct</span></span> |
| <span data-ttu-id="85955-198">取得 /entityset （金鑰）/轉換/瀏覽</span><span class="sxs-lookup"><span data-stu-id="85955-198">GET /entityset(key)/cast/navigation</span></span> | <span data-ttu-id="85955-199">/Products(1)/Models.Book/Author</span><span class="sxs-lookup"><span data-stu-id="85955-199">/Products(1)/Models.Book/Author</span></span> | <span data-ttu-id="85955-200">GetNavigationFromEntityType or GetNavigation</span><span class="sxs-lookup"><span data-stu-id="85955-200">GetNavigationFromEntityType or GetNavigation</span></span> | <span data-ttu-id="85955-201">GetAuthorFromBook</span><span class="sxs-lookup"><span data-stu-id="85955-201">GetAuthorFromBook</span></span> |

<span data-ttu-id="85955-202">如需詳細資訊，請參閱[使用實體關係](odata-v3/working-with-entity-relations.md)。</span><span class="sxs-lookup"><span data-stu-id="85955-202">For more information, see [Working with Entity Relations](odata-v3/working-with-entity-relations.md).</span></span>

<span data-ttu-id="85955-203">**建立和刪除連結**</span><span class="sxs-lookup"><span data-stu-id="85955-203">**Creating and Deleting Links**</span></span>

| <span data-ttu-id="85955-204">要求</span><span class="sxs-lookup"><span data-stu-id="85955-204">Request</span></span> | <span data-ttu-id="85955-205">範例 URI</span><span class="sxs-lookup"><span data-stu-id="85955-205">Example URI</span></span> | <span data-ttu-id="85955-206">動作名稱</span><span class="sxs-lookup"><span data-stu-id="85955-206">Action Name</span></span> |
| --- | --- | --- |
| <span data-ttu-id="85955-207">POST /entityset(key)/$links/navigation</span><span class="sxs-lookup"><span data-stu-id="85955-207">POST /entityset(key)/$links/navigation</span></span> | <span data-ttu-id="85955-208">/Products(1)/$links/Supplier</span><span class="sxs-lookup"><span data-stu-id="85955-208">/Products(1)/$links/Supplier</span></span> | <span data-ttu-id="85955-209">CreateLink</span><span class="sxs-lookup"><span data-stu-id="85955-209">CreateLink</span></span> |
| <span data-ttu-id="85955-210">PUT 的 /entityset （金鑰） / $links/瀏覽</span><span class="sxs-lookup"><span data-stu-id="85955-210">PUT /entityset(key)/$links/navigation</span></span> | <span data-ttu-id="85955-211">/Products(1)/$links/Supplier</span><span class="sxs-lookup"><span data-stu-id="85955-211">/Products(1)/$links/Supplier</span></span> | <span data-ttu-id="85955-212">CreateLink</span><span class="sxs-lookup"><span data-stu-id="85955-212">CreateLink</span></span> |
| <span data-ttu-id="85955-213">DELETE /entityset(key)/$links/navigation</span><span class="sxs-lookup"><span data-stu-id="85955-213">DELETE /entityset(key)/$links/navigation</span></span> | <span data-ttu-id="85955-214">/Products(1)/$links/Supplier</span><span class="sxs-lookup"><span data-stu-id="85955-214">/Products(1)/$links/Supplier</span></span> | <span data-ttu-id="85955-215">DeleteLink</span><span class="sxs-lookup"><span data-stu-id="85955-215">DeleteLink</span></span> |
| <span data-ttu-id="85955-216">DELETE /entityset(key)/$links/navigation(relatedKey)</span><span class="sxs-lookup"><span data-stu-id="85955-216">DELETE /entityset(key)/$links/navigation(relatedKey)</span></span> | <span data-ttu-id="85955-217">/Products/(1)/$links/Suppliers(1)</span><span class="sxs-lookup"><span data-stu-id="85955-217">/Products/(1)/$links/Suppliers(1)</span></span> | <span data-ttu-id="85955-218">DeleteLink</span><span class="sxs-lookup"><span data-stu-id="85955-218">DeleteLink</span></span> |

<span data-ttu-id="85955-219">如需詳細資訊，請參閱[使用實體關係](odata-v3/working-with-entity-relations.md)。</span><span class="sxs-lookup"><span data-stu-id="85955-219">For more information, see [Working with Entity Relations](odata-v3/working-with-entity-relations.md).</span></span>

<span data-ttu-id="85955-220">**屬性**</span><span class="sxs-lookup"><span data-stu-id="85955-220">**Properties**</span></span>

<span data-ttu-id="85955-221">*需要 Web API 2*</span><span class="sxs-lookup"><span data-stu-id="85955-221">*Requires Web API 2*</span></span>

| <span data-ttu-id="85955-222">要求</span><span class="sxs-lookup"><span data-stu-id="85955-222">Request</span></span> | <span data-ttu-id="85955-223">範例 URI</span><span class="sxs-lookup"><span data-stu-id="85955-223">Example URI</span></span> | <span data-ttu-id="85955-224">動作名稱</span><span class="sxs-lookup"><span data-stu-id="85955-224">Action Name</span></span> | <span data-ttu-id="85955-225">範例動作</span><span class="sxs-lookup"><span data-stu-id="85955-225">Example Action</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="85955-226">GET /entityset （金鑰） / 屬性</span><span class="sxs-lookup"><span data-stu-id="85955-226">GET /entityset(key)/property</span></span> | <span data-ttu-id="85955-227">/Products(1)/Name</span><span class="sxs-lookup"><span data-stu-id="85955-227">/Products(1)/Name</span></span> | <span data-ttu-id="85955-228">GetPropertyFromEntityType 或 GetProperty</span><span class="sxs-lookup"><span data-stu-id="85955-228">GetPropertyFromEntityType or GetProperty</span></span> | <span data-ttu-id="85955-229">GetNameFromProduct</span><span class="sxs-lookup"><span data-stu-id="85955-229">GetNameFromProduct</span></span> |
| <span data-ttu-id="85955-230">取得 /entityset （金鑰）/轉換/屬性</span><span class="sxs-lookup"><span data-stu-id="85955-230">GET /entityset(key)/cast/property</span></span> | <span data-ttu-id="85955-231">/Products(1)/Models.Book/Author</span><span class="sxs-lookup"><span data-stu-id="85955-231">/Products(1)/Models.Book/Author</span></span> | <span data-ttu-id="85955-232">GetPropertyFromEntityType 或 GetProperty</span><span class="sxs-lookup"><span data-stu-id="85955-232">GetPropertyFromEntityType or GetProperty</span></span> | <span data-ttu-id="85955-233">GetTitleFromBook</span><span class="sxs-lookup"><span data-stu-id="85955-233">GetTitleFromBook</span></span> |

<span data-ttu-id="85955-234">**動作**</span><span class="sxs-lookup"><span data-stu-id="85955-234">**Actions**</span></span>

| <span data-ttu-id="85955-235">要求</span><span class="sxs-lookup"><span data-stu-id="85955-235">Request</span></span> | <span data-ttu-id="85955-236">範例 URI</span><span class="sxs-lookup"><span data-stu-id="85955-236">Example URI</span></span> | <span data-ttu-id="85955-237">動作名稱</span><span class="sxs-lookup"><span data-stu-id="85955-237">Action Name</span></span> | <span data-ttu-id="85955-238">範例動作</span><span class="sxs-lookup"><span data-stu-id="85955-238">Example Action</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="85955-239">POST /entityset(key)/action</span><span class="sxs-lookup"><span data-stu-id="85955-239">POST /entityset(key)/action</span></span> | <span data-ttu-id="85955-240">/Products(1)/Rate</span><span class="sxs-lookup"><span data-stu-id="85955-240">/Products(1)/Rate</span></span> | <span data-ttu-id="85955-241">ActionNameOnEntityType 或 ActionName</span><span class="sxs-lookup"><span data-stu-id="85955-241">ActionNameOnEntityType or ActionName</span></span> | <span data-ttu-id="85955-242">RateOnProduct</span><span class="sxs-lookup"><span data-stu-id="85955-242">RateOnProduct</span></span> |
| <span data-ttu-id="85955-243">POST /entityset(key)/cast/action</span><span class="sxs-lookup"><span data-stu-id="85955-243">POST /entityset(key)/cast/action</span></span> | <span data-ttu-id="85955-244">/Products(1)/Models.Book/CheckOut</span><span class="sxs-lookup"><span data-stu-id="85955-244">/Products(1)/Models.Book/CheckOut</span></span> | <span data-ttu-id="85955-245">ActionNameOnEntityType 或 ActionName</span><span class="sxs-lookup"><span data-stu-id="85955-245">ActionNameOnEntityType or ActionName</span></span> | <span data-ttu-id="85955-246">CheckOutOnBook</span><span class="sxs-lookup"><span data-stu-id="85955-246">CheckOutOnBook</span></span> |

<span data-ttu-id="85955-247">如需詳細資訊，請參閱[OData 動作](odata-v3/odata-actions.md)。</span><span class="sxs-lookup"><span data-stu-id="85955-247">For more information, see [OData Actions](odata-v3/odata-actions.md).</span></span>

<span data-ttu-id="85955-248">**方法簽章**</span><span class="sxs-lookup"><span data-stu-id="85955-248">**Method Signatures**</span></span>

<span data-ttu-id="85955-249">以下是一些方法簽章的規則：</span><span class="sxs-lookup"><span data-stu-id="85955-249">Here are some rules for the method signatures:</span></span>

- <span data-ttu-id="85955-250">如果路徑包含索引鍵，動作應該有一個名為參數*金鑰*。</span><span class="sxs-lookup"><span data-stu-id="85955-250">If the path contains a key, the action should have a parameter named *key*.</span></span>
- <span data-ttu-id="85955-251">如果路徑包含索引鍵到導覽屬性，此動作應該具有名為參數*relatedKey*。</span><span class="sxs-lookup"><span data-stu-id="85955-251">If the path contains a key into a navigation property, the action should have a parameter named *relatedKey*.</span></span>
- <span data-ttu-id="85955-252">裝飾*金鑰*和*relatedKey*參數**[FromODataUri]**參數。</span><span class="sxs-lookup"><span data-stu-id="85955-252">Decorate *key* and *relatedKey* parameters with the **[FromODataUri]** parameter.</span></span>
- <span data-ttu-id="85955-253">POST 和 PUT 要求需要實體類型的參數。</span><span class="sxs-lookup"><span data-stu-id="85955-253">POST and PUT requests take a parameter of the entity type.</span></span>
- <span data-ttu-id="85955-254">PATCH 要求接受參數的型別**差異&lt;T&gt;**，其中*T*是實體類型。</span><span class="sxs-lookup"><span data-stu-id="85955-254">PATCH requests take a parameter of type **Delta&lt;T&gt;**, where *T* is the entity type.</span></span>

<span data-ttu-id="85955-255">如需參考，以下是範例，顯示每個內建的 OData 路由慣例的方法簽章。</span><span class="sxs-lookup"><span data-stu-id="85955-255">For reference, here is an example that shows method signatures for every built-in OData routing convention.</span></span>

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a><span data-ttu-id="85955-256">自訂路由慣例</span><span class="sxs-lookup"><span data-stu-id="85955-256">Custom Routing Conventions</span></span>

<span data-ttu-id="85955-257">目前的內建的慣例不會討論所有可能的 OData Uri。</span><span class="sxs-lookup"><span data-stu-id="85955-257">Currently the built-in conventions do not cover all possible OData URIs.</span></span> <span data-ttu-id="85955-258">您可以藉由實作加入新的慣例**IODataRoutingConvention**介面。</span><span class="sxs-lookup"><span data-stu-id="85955-258">You can add new conventions by implementing the **IODataRoutingConvention** interface.</span></span> <span data-ttu-id="85955-259">這個介面有兩種方法：</span><span class="sxs-lookup"><span data-stu-id="85955-259">This interface has two methods:</span></span>

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- <span data-ttu-id="85955-260">**SelectController**傳回控制器的名稱。</span><span class="sxs-lookup"><span data-stu-id="85955-260">**SelectController** returns the name of the controller.</span></span>
- <span data-ttu-id="85955-261">**SelectAction**傳回動作的名稱。</span><span class="sxs-lookup"><span data-stu-id="85955-261">**SelectAction** returns the name of the action.</span></span>

<span data-ttu-id="85955-262">這兩種方法，如果慣例不適用於該要求，方法應該傳回 null。</span><span class="sxs-lookup"><span data-stu-id="85955-262">For both methods, if the convention does not apply to that request, the method should return null.</span></span>

<span data-ttu-id="85955-263">**ODataPath**參數所代表的已剖析的 OData 資源路徑。</span><span class="sxs-lookup"><span data-stu-id="85955-263">The **ODataPath** parameter represents the parsed OData resource path.</span></span> <span data-ttu-id="85955-264">它包含一份 **[ODataPathSegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** 執行個體，其中每個區段的資源路徑。</span><span class="sxs-lookup"><span data-stu-id="85955-264">It contains a list of **[ODataPathSegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** instances, one for each segment of the resource path.</span></span> <span data-ttu-id="85955-265">**ODataPathSegment**是抽象類別; 每個區段的類型由衍生自類別**ODataPathSegment**。</span><span class="sxs-lookup"><span data-stu-id="85955-265">**ODataPathSegment** is an abstract class; each segment type is represented by a class that derives from **ODataPathSegment**.</span></span>

<span data-ttu-id="85955-266">**ODataPath.TemplatePath**屬性是字串，代表串連所有的路徑區段。</span><span class="sxs-lookup"><span data-stu-id="85955-266">The **ODataPath.TemplatePath** property is a string that represents the concatenation all of the path segments.</span></span> <span data-ttu-id="85955-267">例如，如果 URI 是`/Products(1)/Supplier`，路徑範本&quot;~/entityset/key/navigation&quot;。</span><span class="sxs-lookup"><span data-stu-id="85955-267">For example, if the URI is `/Products(1)/Supplier`, the path template is &quot;~/entityset/key/navigation&quot;.</span></span> <span data-ttu-id="85955-268">請注意，區段不能直接對應到 URI 區段。</span><span class="sxs-lookup"><span data-stu-id="85955-268">Notice that the segments don't correspond directly to URI segments.</span></span> <span data-ttu-id="85955-269">例如，實體索引鍵 (1) 以其本身**ODataPathSegment**。</span><span class="sxs-lookup"><span data-stu-id="85955-269">For example, the entity key (1) is represented as its own **ODataPathSegment**.</span></span>

<span data-ttu-id="85955-270">一般而言，實作**IODataRoutingConvention**會進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="85955-270">Typically, an implementation of **IODataRoutingConvention** does the following:</span></span>

1. <span data-ttu-id="85955-271">比較看這個慣例會套用至目前要求的路徑範本。</span><span class="sxs-lookup"><span data-stu-id="85955-271">Compare the path template to see if this convention applies to the current request.</span></span> <span data-ttu-id="85955-272">如果不適用，則傳回 null。</span><span class="sxs-lookup"><span data-stu-id="85955-272">If it does not apply, return null.</span></span>
2. <span data-ttu-id="85955-273">適用於慣例，如果使用的屬性**ODataPathSegment**衍生控制器和動作名稱的執行個體。</span><span class="sxs-lookup"><span data-stu-id="85955-273">If the convention applies, use properties of the **ODataPathSegment** instances to derive controller and action names.</span></span>
3. <span data-ttu-id="85955-274">對於動作，加入路由字典，其中應該繫結至動作參數 （通常是實體索引鍵） 中的任何值。</span><span class="sxs-lookup"><span data-stu-id="85955-274">For actions, add any values to the route dictionary that should bind to the action parameters (typically entity keys).</span></span>

<span data-ttu-id="85955-275">讓我們看看特定範例。</span><span class="sxs-lookup"><span data-stu-id="85955-275">Let's look at a specific example.</span></span> <span data-ttu-id="85955-276">內建路由慣例不支援瀏覽集合中進行索引。</span><span class="sxs-lookup"><span data-stu-id="85955-276">The built-in routing conventions do not support indexing into a navigation collection.</span></span> <span data-ttu-id="85955-277">換句話說，沒有任何慣例 uri，如下所示：</span><span class="sxs-lookup"><span data-stu-id="85955-277">In other words, there is no convention for URIs like the following:</span></span>

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

<span data-ttu-id="85955-278">以下是查詢的自訂的路由慣例，來處理這種類型。</span><span class="sxs-lookup"><span data-stu-id="85955-278">Here is a custom routing convention to handle this type of query.</span></span>

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

<span data-ttu-id="85955-279">附註：</span><span class="sxs-lookup"><span data-stu-id="85955-279">Notes:</span></span>

1. <span data-ttu-id="85955-280">我是衍生自**EntitySetRoutingConvention**，因為**SelectController**該類別中的方法是適用於這個新的路由慣例。</span><span class="sxs-lookup"><span data-stu-id="85955-280">I derive from **EntitySetRoutingConvention**, because the **SelectController** method in that class is appropriate for this new routing convention.</span></span> <span data-ttu-id="85955-281">也就是說，不必重新實作**SelectController**。</span><span class="sxs-lookup"><span data-stu-id="85955-281">That means I don't need to re-implement **SelectController**.</span></span>
2. <span data-ttu-id="85955-282">慣例僅適用於 GET 要求和路徑範本時才&quot;~/entityset/key/navigation/key&quot;。</span><span class="sxs-lookup"><span data-stu-id="85955-282">The convention applies only to GET requests, and only when the path template is &quot;~/entityset/key/navigation/key&quot;.</span></span>
3. <span data-ttu-id="85955-283">動作名稱是&quot;取得 {EntityType}&quot;，其中*{EntityType}*是瀏覽集合的型別。</span><span class="sxs-lookup"><span data-stu-id="85955-283">The action name is &quot;Get{EntityType}&quot;, where *{EntityType}* is the type of the navigation collection.</span></span> <span data-ttu-id="85955-284">例如， &quot;GetSupplier&quot;。</span><span class="sxs-lookup"><span data-stu-id="85955-284">For example, &quot;GetSupplier&quot;.</span></span> <span data-ttu-id="85955-285">您可以使用任何您喜歡的命名慣例 &#8212;請確定您符合控制器的動作。</span><span class="sxs-lookup"><span data-stu-id="85955-285">You can use any naming convention that you like &#8212; just make sure your controller actions match.</span></span>
4. <span data-ttu-id="85955-286">採取的動作名稱為兩個參數*金鑰*和*relatedKey*。</span><span class="sxs-lookup"><span data-stu-id="85955-286">The action takes two parameters named *key* and *relatedKey*.</span></span> <span data-ttu-id="85955-287">(如需某些預先定義的參數名稱的清單，請參閱[ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx)。)</span><span class="sxs-lookup"><span data-stu-id="85955-287">(For a list of some predefined parameter names, see [ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx).)</span></span>

<span data-ttu-id="85955-288">下一個步驟新增新的慣例路由慣例的清單。</span><span class="sxs-lookup"><span data-stu-id="85955-288">The next step is adding the new convention to the list of routing conventions.</span></span> <span data-ttu-id="85955-289">發生這種情況在設定期間，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="85955-289">This happens during configuration, as shown in the following code:</span></span>

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

<span data-ttu-id="85955-290">以下是一些其他範例路由慣例來研究實用：</span><span class="sxs-lookup"><span data-stu-id="85955-290">Here are some other sample routing conventions that be useful to study:</span></span>

- [<span data-ttu-id="85955-291">CompositeKeyRoutingConvention</span><span class="sxs-lookup"><span data-stu-id="85955-291">CompositeKeyRoutingConvention</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [<span data-ttu-id="85955-292">CustomNavigationRoutingConvention</span><span class="sxs-lookup"><span data-stu-id="85955-292">CustomNavigationRoutingConvention</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [<span data-ttu-id="85955-293">NonBindableActionRoutingConvention</span><span class="sxs-lookup"><span data-stu-id="85955-293">NonBindableActionRoutingConvention</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [<span data-ttu-id="85955-294">ODataVersionRouteConstraint</span><span class="sxs-lookup"><span data-stu-id="85955-294">ODataVersionRouteConstraint</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

<span data-ttu-id="85955-295">和 Web API 本身當然是開放原始碼，因此您可以看到[原始程式碼](http://aspnetwebstack.codeplex.com/)的內建路由慣例。</span><span class="sxs-lookup"><span data-stu-id="85955-295">And of course Web API itself is open-source, so you can see the [source code](http://aspnetwebstack.codeplex.com/) for the built-in routing conventions.</span></span> <span data-ttu-id="85955-296">這些定義在**System.Web.Http.OData.Routing.Conventions**命名空間。</span><span class="sxs-lookup"><span data-stu-id="85955-296">These are defined in the **System.Web.Http.OData.Routing.Conventions** namespace.</span></span>
