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
ms.openlocfilehash: cd24a85a05e427f83d28cae876431d04cc295f17
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="routing-conventions-in-aspnet-web-api-2-odata"></a><span data-ttu-id="39040-103">路由慣例，在 ASP.NET Web API 2 Odata</span><span class="sxs-lookup"><span data-stu-id="39040-103">Routing Conventions in ASP.NET Web API 2 Odata</span></span>
====================
<span data-ttu-id="39040-104">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="39040-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="39040-105">本文說明 Web API OData 端點所使用的路由慣例。</span><span class="sxs-lookup"><span data-stu-id="39040-105">This article describes the routing conventions that Web API uses for OData endpoints.</span></span>


<span data-ttu-id="39040-106">當 Web API 取得 OData 要求時，它會將要求對應至控制器名稱和動作名稱中。</span><span class="sxs-lookup"><span data-stu-id="39040-106">When Web API gets an OData request, it maps the request to a controller name and an action name.</span></span> <span data-ttu-id="39040-107">對應為基礎的 HTTP 方法和 URI。</span><span class="sxs-lookup"><span data-stu-id="39040-107">The mapping is based on the HTTP method and the URI.</span></span> <span data-ttu-id="39040-108">例如，`GET /odata/Products(1)`對應至`ProductsController.GetProduct`。</span><span class="sxs-lookup"><span data-stu-id="39040-108">For example, `GET /odata/Products(1)` maps to `ProductsController.GetProduct`.</span></span>

<span data-ttu-id="39040-109">在本文的第 1 部分，我會描述的內建的 OData 路由慣例。</span><span class="sxs-lookup"><span data-stu-id="39040-109">In part 1 of this article, I describe the built-in OData routing conventions.</span></span> <span data-ttu-id="39040-110">這些慣例專為 OData 端點，這些屬性取代預設的 Web API 路由系統。</span><span class="sxs-lookup"><span data-stu-id="39040-110">These conventions are designed specifically for OData endpoints, and they replace the default Web API routing system.</span></span> <span data-ttu-id="39040-111">(當您呼叫時，會發生取代**MapODataRoute**。)</span><span class="sxs-lookup"><span data-stu-id="39040-111">(The replacement happens when you call **MapODataRoute**.)</span></span>

<span data-ttu-id="39040-112">在第 2 部分，我會示範如何加入自訂路由慣例。</span><span class="sxs-lookup"><span data-stu-id="39040-112">In part 2, I show how to add custom routing conventions.</span></span> <span data-ttu-id="39040-113">目前的內建的慣例不涵蓋整個範圍的 OData Uri，但您可以將其處理其他情況下擴充。</span><span class="sxs-lookup"><span data-stu-id="39040-113">Currently the built-in conventions do not cover the entire range of OData URIs, but you can extend them to handle additional cases.</span></span>

- [<span data-ttu-id="39040-114">內建路由慣例</span><span class="sxs-lookup"><span data-stu-id="39040-114">Built-in Routing Conventions</span></span>](#conventions)
- [<span data-ttu-id="39040-115">自訂路由慣例</span><span class="sxs-lookup"><span data-stu-id="39040-115">Custom Routing Conventions</span></span>](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a><span data-ttu-id="39040-116">內建路由慣例</span><span class="sxs-lookup"><span data-stu-id="39040-116">Built-in Routing Conventions</span></span>

<span data-ttu-id="39040-117">我描述 Web API 中的 OData 路由慣例之前，最好先了解 OData Uri。</span><span class="sxs-lookup"><span data-stu-id="39040-117">Before I describe the OData routing conventions in Web API, it's helpful to understand OData URIs.</span></span> <span data-ttu-id="39040-118">[OData URI](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/)所組成：</span><span class="sxs-lookup"><span data-stu-id="39040-118">An [OData URI](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/) consists of:</span></span>

- <span data-ttu-id="39040-119">服務根目錄</span><span class="sxs-lookup"><span data-stu-id="39040-119">The service root</span></span>
- <span data-ttu-id="39040-120">資源路徑</span><span class="sxs-lookup"><span data-stu-id="39040-120">The resource path</span></span>
- <span data-ttu-id="39040-121">查詢選項</span><span class="sxs-lookup"><span data-stu-id="39040-121">Query options</span></span>

![](odata-routing-conventions/_static/image1.png)

<span data-ttu-id="39040-122">路由，重要的部分是資源路徑。</span><span class="sxs-lookup"><span data-stu-id="39040-122">For routing, the important part is the resource path.</span></span> <span data-ttu-id="39040-123">資源路徑會分成區段。</span><span class="sxs-lookup"><span data-stu-id="39040-123">The resource path is divided into segments.</span></span> <span data-ttu-id="39040-124">例如，`/Products(1)/Supplier`有三個區段：</span><span class="sxs-lookup"><span data-stu-id="39040-124">For example, `/Products(1)/Supplier` has three segments:</span></span>

- <span data-ttu-id="39040-125">`Products`指的是名為 「 產品 」 實體集。</span><span class="sxs-lookup"><span data-stu-id="39040-125">`Products` refers to an entity set named "Products".</span></span>
- <span data-ttu-id="39040-126">`1`從集合中選取單一實體為實體索引鍵。</span><span class="sxs-lookup"><span data-stu-id="39040-126">`1` is an entity key, selecting a single entity from the set.</span></span>
- <span data-ttu-id="39040-127">`Supplier`為選取相關的實體的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="39040-127">`Supplier` is a navigation property that selects a related entity.</span></span>

<span data-ttu-id="39040-128">因此這個路徑會挑選出 1 產品的供應商。</span><span class="sxs-lookup"><span data-stu-id="39040-128">So this path picks out the supplier of product 1.</span></span>

> [!NOTE]
> <span data-ttu-id="39040-129">OData 路徑區段未一律對應至 URI 區段。</span><span class="sxs-lookup"><span data-stu-id="39040-129">OData path segments do not always correspond to URI segments.</span></span> <span data-ttu-id="39040-130">例如，"1"會被視為路徑區段。</span><span class="sxs-lookup"><span data-stu-id="39040-130">For example, "1" is considered a path segment.</span></span>


<span data-ttu-id="39040-131">**控制器名稱。**</span><span class="sxs-lookup"><span data-stu-id="39040-131">**Controller Names.**</span></span> <span data-ttu-id="39040-132">控制器名稱一定被衍生自的實體集資源路徑的根目錄。</span><span class="sxs-lookup"><span data-stu-id="39040-132">The controller name is always derived from the entity set at the root of the resource path.</span></span> <span data-ttu-id="39040-133">例如，如果資源路徑是`/Products(1)/Supplier`，Web API 會尋找名為控制器`ProductsController`。</span><span class="sxs-lookup"><span data-stu-id="39040-133">For example, if the resource path is `/Products(1)/Supplier`, Web API looks for a controller named `ProductsController`.</span></span>

<span data-ttu-id="39040-134">**動作名稱。**</span><span class="sxs-lookup"><span data-stu-id="39040-134">**Action Names.**</span></span> <span data-ttu-id="39040-135">下列表格所列的動作名稱衍生自的路徑區段，再加上的實體資料模型 (EDM)。</span><span class="sxs-lookup"><span data-stu-id="39040-135">Action names are derived from the path segments plus the entity data model (EDM), as listed in the following tables.</span></span> <span data-ttu-id="39040-136">在某些情況下，您會有兩種選項的動作名稱。</span><span class="sxs-lookup"><span data-stu-id="39040-136">In some cases, you have two choices for the action name.</span></span> <span data-ttu-id="39040-137">例如，「 取得 」 或&quot;GetProducts&quot;。</span><span class="sxs-lookup"><span data-stu-id="39040-137">For example, "Get" or &quot;GetProducts&quot;.</span></span>

<span data-ttu-id="39040-138">**查詢實體**</span><span class="sxs-lookup"><span data-stu-id="39040-138">**Querying Entities**</span></span>

| <span data-ttu-id="39040-139">要求</span><span class="sxs-lookup"><span data-stu-id="39040-139">Request</span></span> | <span data-ttu-id="39040-140">範例 URI</span><span class="sxs-lookup"><span data-stu-id="39040-140">Example URI</span></span> | <span data-ttu-id="39040-141">動作名稱</span><span class="sxs-lookup"><span data-stu-id="39040-141">Action Name</span></span> | <span data-ttu-id="39040-142">範例動作</span><span class="sxs-lookup"><span data-stu-id="39040-142">Example Action</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="39040-143">取得 /entityset</span><span class="sxs-lookup"><span data-stu-id="39040-143">GET /entityset</span></span> | <span data-ttu-id="39040-144">/ 產品</span><span class="sxs-lookup"><span data-stu-id="39040-144">/Products</span></span> | <span data-ttu-id="39040-145">GetEntitySet 或 Get</span><span class="sxs-lookup"><span data-stu-id="39040-145">GetEntitySet or Get</span></span> | <span data-ttu-id="39040-146">GetProducts</span><span class="sxs-lookup"><span data-stu-id="39040-146">GetProducts</span></span> |
| <span data-ttu-id="39040-147">取得 /entityset(key)</span><span class="sxs-lookup"><span data-stu-id="39040-147">GET /entityset(key)</span></span> | <span data-ttu-id="39040-148">/Products(1)</span><span class="sxs-lookup"><span data-stu-id="39040-148">/Products(1)</span></span> | <span data-ttu-id="39040-149">GetEntityType 或 Get</span><span class="sxs-lookup"><span data-stu-id="39040-149">GetEntityType or Get</span></span> | <span data-ttu-id="39040-150">GetProduct</span><span class="sxs-lookup"><span data-stu-id="39040-150">GetProduct</span></span> |
| <span data-ttu-id="39040-151">取得 /entityset （金鑰）/轉換</span><span class="sxs-lookup"><span data-stu-id="39040-151">GET /entityset(key)/cast</span></span> | <span data-ttu-id="39040-152">/ /Models.Book 產品 (1)</span><span class="sxs-lookup"><span data-stu-id="39040-152">/Products(1)/Models.Book</span></span> | <span data-ttu-id="39040-153">GetEntityType 或 Get</span><span class="sxs-lookup"><span data-stu-id="39040-153">GetEntityType or Get</span></span> | <span data-ttu-id="39040-154">GetBook</span><span class="sxs-lookup"><span data-stu-id="39040-154">GetBook</span></span> |

<span data-ttu-id="39040-155">如需詳細資訊，請參閱[建立唯讀的 OData 端點](odata-v3/creating-an-odata-endpoint.md)。</span><span class="sxs-lookup"><span data-stu-id="39040-155">For more information, see [Create a Read-Only OData Endpoint](odata-v3/creating-an-odata-endpoint.md).</span></span>

<span data-ttu-id="39040-156">**建立、 更新及刪除實體**</span><span class="sxs-lookup"><span data-stu-id="39040-156">**Creating, Updating, and Deleting Entities**</span></span>

| <span data-ttu-id="39040-157">要求</span><span class="sxs-lookup"><span data-stu-id="39040-157">Request</span></span> | <span data-ttu-id="39040-158">範例 URI</span><span class="sxs-lookup"><span data-stu-id="39040-158">Example URI</span></span> | <span data-ttu-id="39040-159">動作名稱</span><span class="sxs-lookup"><span data-stu-id="39040-159">Action Name</span></span> | <span data-ttu-id="39040-160">範例動作</span><span class="sxs-lookup"><span data-stu-id="39040-160">Example Action</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="39040-161">張貼 /entityset</span><span class="sxs-lookup"><span data-stu-id="39040-161">POST /entityset</span></span> | <span data-ttu-id="39040-162">/ 產品</span><span class="sxs-lookup"><span data-stu-id="39040-162">/Products</span></span> | <span data-ttu-id="39040-163">PostEntityType 或 Post</span><span class="sxs-lookup"><span data-stu-id="39040-163">PostEntityType or Post</span></span> | <span data-ttu-id="39040-164">PostProduct</span><span class="sxs-lookup"><span data-stu-id="39040-164">PostProduct</span></span> |
| <span data-ttu-id="39040-165">PUT /entityset(key)</span><span class="sxs-lookup"><span data-stu-id="39040-165">PUT /entityset(key)</span></span> | <span data-ttu-id="39040-166">/Products(1)</span><span class="sxs-lookup"><span data-stu-id="39040-166">/Products(1)</span></span> | <span data-ttu-id="39040-167">PutEntityType 或 Put</span><span class="sxs-lookup"><span data-stu-id="39040-167">PutEntityType or Put</span></span> | <span data-ttu-id="39040-168">PutProduct</span><span class="sxs-lookup"><span data-stu-id="39040-168">PutProduct</span></span> |
| <span data-ttu-id="39040-169">PUT /entityset （金鑰）/轉換</span><span class="sxs-lookup"><span data-stu-id="39040-169">PUT /entityset(key)/cast</span></span> | <span data-ttu-id="39040-170">/ /Models.Book 產品 (1)</span><span class="sxs-lookup"><span data-stu-id="39040-170">/Products(1)/Models.Book</span></span> | <span data-ttu-id="39040-171">PutEntityType 或 Put</span><span class="sxs-lookup"><span data-stu-id="39040-171">PutEntityType or Put</span></span> | <span data-ttu-id="39040-172">PutBook</span><span class="sxs-lookup"><span data-stu-id="39040-172">PutBook</span></span> |
| <span data-ttu-id="39040-173">修補程式 /entityset(key)</span><span class="sxs-lookup"><span data-stu-id="39040-173">PATCH /entityset(key)</span></span> | <span data-ttu-id="39040-174">/Products(1)</span><span class="sxs-lookup"><span data-stu-id="39040-174">/Products(1)</span></span> | <span data-ttu-id="39040-175">PatchEntityType 或修補程式</span><span class="sxs-lookup"><span data-stu-id="39040-175">PatchEntityType or Patch</span></span> | <span data-ttu-id="39040-176">PatchProduct</span><span class="sxs-lookup"><span data-stu-id="39040-176">PatchProduct</span></span> |
| <span data-ttu-id="39040-177">修補程式 /entityset （金鑰）/轉換</span><span class="sxs-lookup"><span data-stu-id="39040-177">PATCH /entityset(key)/cast</span></span> | <span data-ttu-id="39040-178">/ /Models.Book 產品 (1)</span><span class="sxs-lookup"><span data-stu-id="39040-178">/Products(1)/Models.Book</span></span> | <span data-ttu-id="39040-179">PatchEntityType 或修補程式</span><span class="sxs-lookup"><span data-stu-id="39040-179">PatchEntityType or Patch</span></span> | <span data-ttu-id="39040-180">PatchBook</span><span class="sxs-lookup"><span data-stu-id="39040-180">PatchBook</span></span> |
| <span data-ttu-id="39040-181">刪除 /entityset(key)</span><span class="sxs-lookup"><span data-stu-id="39040-181">DELETE /entityset(key)</span></span> | <span data-ttu-id="39040-182">/Products(1)</span><span class="sxs-lookup"><span data-stu-id="39040-182">/Products(1)</span></span> | <span data-ttu-id="39040-183">DeleteEntityType 或刪除</span><span class="sxs-lookup"><span data-stu-id="39040-183">DeleteEntityType or Delete</span></span> | <span data-ttu-id="39040-184">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="39040-184">DeleteProduct</span></span> |
| <span data-ttu-id="39040-185">轉換/刪除 /entityset （索引鍵）</span><span class="sxs-lookup"><span data-stu-id="39040-185">DELETE /entityset(key)/cast</span></span> | <span data-ttu-id="39040-186">/ /Models.Book 產品 (1)</span><span class="sxs-lookup"><span data-stu-id="39040-186">/Products(1)/Models.Book</span></span> | <span data-ttu-id="39040-187">DeleteEntityType 或刪除</span><span class="sxs-lookup"><span data-stu-id="39040-187">DeleteEntityType or Delete</span></span> | <span data-ttu-id="39040-188">DeleteBook</span><span class="sxs-lookup"><span data-stu-id="39040-188">DeleteBook</span></span> |

<span data-ttu-id="39040-189">**查詢的導覽屬性**</span><span class="sxs-lookup"><span data-stu-id="39040-189">**Querying a Navigation Property**</span></span>

| <span data-ttu-id="39040-190">要求</span><span class="sxs-lookup"><span data-stu-id="39040-190">Request</span></span> | <span data-ttu-id="39040-191">範例 URI</span><span class="sxs-lookup"><span data-stu-id="39040-191">Example URI</span></span> | <span data-ttu-id="39040-192">動作名稱</span><span class="sxs-lookup"><span data-stu-id="39040-192">Action Name</span></span> | <span data-ttu-id="39040-193">範例動作</span><span class="sxs-lookup"><span data-stu-id="39040-193">Example Action</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="39040-194">GET /entityset （金鑰） / 瀏覽</span><span class="sxs-lookup"><span data-stu-id="39040-194">GET /entityset(key)/navigation</span></span> | <span data-ttu-id="39040-195">/ 產品 （1）/供應商</span><span class="sxs-lookup"><span data-stu-id="39040-195">/Products(1)/Supplier</span></span> | <span data-ttu-id="39040-196">GetNavigationFromEntityType 或 GetNavigation</span><span class="sxs-lookup"><span data-stu-id="39040-196">GetNavigationFromEntityType or GetNavigation</span></span> | <span data-ttu-id="39040-197">GetSupplierFromProduct</span><span class="sxs-lookup"><span data-stu-id="39040-197">GetSupplierFromProduct</span></span> |
| <span data-ttu-id="39040-198">取得 /entityset （金鑰）/轉換/瀏覽</span><span class="sxs-lookup"><span data-stu-id="39040-198">GET /entityset(key)/cast/navigation</span></span> | <span data-ttu-id="39040-199">/ /Models.Book/Author 產品 (1)</span><span class="sxs-lookup"><span data-stu-id="39040-199">/Products(1)/Models.Book/Author</span></span> | <span data-ttu-id="39040-200">GetNavigationFromEntityType 或 GetNavigation</span><span class="sxs-lookup"><span data-stu-id="39040-200">GetNavigationFromEntityType or GetNavigation</span></span> | <span data-ttu-id="39040-201">GetAuthorFromBook</span><span class="sxs-lookup"><span data-stu-id="39040-201">GetAuthorFromBook</span></span> |

<span data-ttu-id="39040-202">如需詳細資訊，請參閱[使用實體關係](odata-v3/working-with-entity-relations.md)。</span><span class="sxs-lookup"><span data-stu-id="39040-202">For more information, see [Working with Entity Relations](odata-v3/working-with-entity-relations.md).</span></span>

<span data-ttu-id="39040-203">**建立和刪除連結**</span><span class="sxs-lookup"><span data-stu-id="39040-203">**Creating and Deleting Links**</span></span>

| <span data-ttu-id="39040-204">要求</span><span class="sxs-lookup"><span data-stu-id="39040-204">Request</span></span> | <span data-ttu-id="39040-205">範例 URI</span><span class="sxs-lookup"><span data-stu-id="39040-205">Example URI</span></span> | <span data-ttu-id="39040-206">動作名稱</span><span class="sxs-lookup"><span data-stu-id="39040-206">Action Name</span></span> |
| --- | --- | --- |
| <span data-ttu-id="39040-207">POST /entityset （金鑰） / $links/瀏覽</span><span class="sxs-lookup"><span data-stu-id="39040-207">POST /entityset(key)/$links/navigation</span></span> | <span data-ttu-id="39040-208">/ 產品 （1） / $ 連結/供應商</span><span class="sxs-lookup"><span data-stu-id="39040-208">/Products(1)/$links/Supplier</span></span> | <span data-ttu-id="39040-209">CreateLink</span><span class="sxs-lookup"><span data-stu-id="39040-209">CreateLink</span></span> |
| <span data-ttu-id="39040-210">PUT 的 /entityset （金鑰） / $links/瀏覽</span><span class="sxs-lookup"><span data-stu-id="39040-210">PUT /entityset(key)/$links/navigation</span></span> | <span data-ttu-id="39040-211">/ 產品 （1） / $ 連結/供應商</span><span class="sxs-lookup"><span data-stu-id="39040-211">/Products(1)/$links/Supplier</span></span> | <span data-ttu-id="39040-212">CreateLink</span><span class="sxs-lookup"><span data-stu-id="39040-212">CreateLink</span></span> |
| <span data-ttu-id="39040-213">刪除 /entityset （金鑰） / $links/瀏覽</span><span class="sxs-lookup"><span data-stu-id="39040-213">DELETE /entityset(key)/$links/navigation</span></span> | <span data-ttu-id="39040-214">/ 產品 （1） / $ 連結/供應商</span><span class="sxs-lookup"><span data-stu-id="39040-214">/Products(1)/$links/Supplier</span></span> | <span data-ttu-id="39040-215">DeleteLink</span><span class="sxs-lookup"><span data-stu-id="39040-215">DeleteLink</span></span> |
| <span data-ttu-id="39040-216">刪除 /entityset(key)/$links/navigation(relatedKey)</span><span class="sxs-lookup"><span data-stu-id="39040-216">DELETE /entityset(key)/$links/navigation(relatedKey)</span></span> | <span data-ttu-id="39040-217">/Products/(1)/$links/Suppliers(1)</span><span class="sxs-lookup"><span data-stu-id="39040-217">/Products/(1)/$links/Suppliers(1)</span></span> | <span data-ttu-id="39040-218">DeleteLink</span><span class="sxs-lookup"><span data-stu-id="39040-218">DeleteLink</span></span> |

<span data-ttu-id="39040-219">如需詳細資訊，請參閱[使用實體關係](odata-v3/working-with-entity-relations.md)。</span><span class="sxs-lookup"><span data-stu-id="39040-219">For more information, see [Working with Entity Relations](odata-v3/working-with-entity-relations.md).</span></span>

<span data-ttu-id="39040-220">**屬性**</span><span class="sxs-lookup"><span data-stu-id="39040-220">**Properties**</span></span>

<span data-ttu-id="39040-221">*需要 Web API 2*</span><span class="sxs-lookup"><span data-stu-id="39040-221">*Requires Web API 2*</span></span>

| <span data-ttu-id="39040-222">要求</span><span class="sxs-lookup"><span data-stu-id="39040-222">Request</span></span> | <span data-ttu-id="39040-223">範例 URI</span><span class="sxs-lookup"><span data-stu-id="39040-223">Example URI</span></span> | <span data-ttu-id="39040-224">動作名稱</span><span class="sxs-lookup"><span data-stu-id="39040-224">Action Name</span></span> | <span data-ttu-id="39040-225">範例動作</span><span class="sxs-lookup"><span data-stu-id="39040-225">Example Action</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="39040-226">GET /entityset （金鑰） / 屬性</span><span class="sxs-lookup"><span data-stu-id="39040-226">GET /entityset(key)/property</span></span> | <span data-ttu-id="39040-227">/ 產品 （1）/名稱</span><span class="sxs-lookup"><span data-stu-id="39040-227">/Products(1)/Name</span></span> | <span data-ttu-id="39040-228">GetPropertyFromEntityType 或 GetProperty</span><span class="sxs-lookup"><span data-stu-id="39040-228">GetPropertyFromEntityType or GetProperty</span></span> | <span data-ttu-id="39040-229">GetNameFromProduct</span><span class="sxs-lookup"><span data-stu-id="39040-229">GetNameFromProduct</span></span> |
| <span data-ttu-id="39040-230">取得 /entityset （金鑰）/轉換/屬性</span><span class="sxs-lookup"><span data-stu-id="39040-230">GET /entityset(key)/cast/property</span></span> | <span data-ttu-id="39040-231">/ /Models.Book/Author 產品 (1)</span><span class="sxs-lookup"><span data-stu-id="39040-231">/Products(1)/Models.Book/Author</span></span> | <span data-ttu-id="39040-232">GetPropertyFromEntityType 或 GetProperty</span><span class="sxs-lookup"><span data-stu-id="39040-232">GetPropertyFromEntityType or GetProperty</span></span> | <span data-ttu-id="39040-233">GetTitleFromBook</span><span class="sxs-lookup"><span data-stu-id="39040-233">GetTitleFromBook</span></span> |

<span data-ttu-id="39040-234">**動作**</span><span class="sxs-lookup"><span data-stu-id="39040-234">**Actions**</span></span>

| <span data-ttu-id="39040-235">要求</span><span class="sxs-lookup"><span data-stu-id="39040-235">Request</span></span> | <span data-ttu-id="39040-236">範例 URI</span><span class="sxs-lookup"><span data-stu-id="39040-236">Example URI</span></span> | <span data-ttu-id="39040-237">動作名稱</span><span class="sxs-lookup"><span data-stu-id="39040-237">Action Name</span></span> | <span data-ttu-id="39040-238">範例動作</span><span class="sxs-lookup"><span data-stu-id="39040-238">Example Action</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="39040-239">POST /entityset （金鑰） / 動作</span><span class="sxs-lookup"><span data-stu-id="39040-239">POST /entityset(key)/action</span></span> | <span data-ttu-id="39040-240">/ 產品 （1）/速率</span><span class="sxs-lookup"><span data-stu-id="39040-240">/Products(1)/Rate</span></span> | <span data-ttu-id="39040-241">ActionNameOnEntityType 或 ActionName</span><span class="sxs-lookup"><span data-stu-id="39040-241">ActionNameOnEntityType or ActionName</span></span> | <span data-ttu-id="39040-242">RateOnProduct</span><span class="sxs-lookup"><span data-stu-id="39040-242">RateOnProduct</span></span> |
| <span data-ttu-id="39040-243">後 /entityset （金鑰）/轉換/動作</span><span class="sxs-lookup"><span data-stu-id="39040-243">POST /entityset(key)/cast/action</span></span> | <span data-ttu-id="39040-244">/ /Models.Book/CheckOut 產品 (1)</span><span class="sxs-lookup"><span data-stu-id="39040-244">/Products(1)/Models.Book/CheckOut</span></span> | <span data-ttu-id="39040-245">ActionNameOnEntityType 或 ActionName</span><span class="sxs-lookup"><span data-stu-id="39040-245">ActionNameOnEntityType or ActionName</span></span> | <span data-ttu-id="39040-246">CheckOutOnBook</span><span class="sxs-lookup"><span data-stu-id="39040-246">CheckOutOnBook</span></span> |

<span data-ttu-id="39040-247">如需詳細資訊，請參閱[OData 動作](odata-v3/odata-actions.md)。</span><span class="sxs-lookup"><span data-stu-id="39040-247">For more information, see [OData Actions](odata-v3/odata-actions.md).</span></span>

<span data-ttu-id="39040-248">**方法簽章**</span><span class="sxs-lookup"><span data-stu-id="39040-248">**Method Signatures**</span></span>

<span data-ttu-id="39040-249">以下是一些方法簽章的規則：</span><span class="sxs-lookup"><span data-stu-id="39040-249">Here are some rules for the method signatures:</span></span>

- <span data-ttu-id="39040-250">如果路徑包含索引鍵，動作應該有一個名為參數*金鑰*。</span><span class="sxs-lookup"><span data-stu-id="39040-250">If the path contains a key, the action should have a parameter named *key*.</span></span>
- <span data-ttu-id="39040-251">如果路徑包含索引鍵到導覽屬性，此動作應該具有名為參數*relatedKey*。</span><span class="sxs-lookup"><span data-stu-id="39040-251">If the path contains a key into a navigation property, the action should have a parameter named *relatedKey*.</span></span>
- <span data-ttu-id="39040-252">裝飾*金鑰*和*relatedKey*參數**[FromODataUri]**參數。</span><span class="sxs-lookup"><span data-stu-id="39040-252">Decorate *key* and *relatedKey* parameters with the **[FromODataUri]** parameter.</span></span>
- <span data-ttu-id="39040-253">POST 和 PUT 要求需要實體類型的參數。</span><span class="sxs-lookup"><span data-stu-id="39040-253">POST and PUT requests take a parameter of the entity type.</span></span>
- <span data-ttu-id="39040-254">PATCH 要求接受參數的型別**差異&lt;T&gt;**，其中*T*是實體類型。</span><span class="sxs-lookup"><span data-stu-id="39040-254">PATCH requests take a parameter of type **Delta&lt;T&gt;**, where *T* is the entity type.</span></span>

<span data-ttu-id="39040-255">如需參考，以下是範例，顯示每個內建的 OData 路由慣例的方法簽章。</span><span class="sxs-lookup"><span data-stu-id="39040-255">For reference, here is an example that shows method signatures for every built-in OData routing convention.</span></span>

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a><span data-ttu-id="39040-256">自訂路由慣例</span><span class="sxs-lookup"><span data-stu-id="39040-256">Custom Routing Conventions</span></span>

<span data-ttu-id="39040-257">目前的內建的慣例不會討論所有可能的 OData Uri。</span><span class="sxs-lookup"><span data-stu-id="39040-257">Currently the built-in conventions do not cover all possible OData URIs.</span></span> <span data-ttu-id="39040-258">您可以藉由實作加入新的慣例**IODataRoutingConvention**介面。</span><span class="sxs-lookup"><span data-stu-id="39040-258">You can add new conventions by implementing the **IODataRoutingConvention** interface.</span></span> <span data-ttu-id="39040-259">這個介面有兩種方法：</span><span class="sxs-lookup"><span data-stu-id="39040-259">This interface has two methods:</span></span>

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- <span data-ttu-id="39040-260">**SelectController**傳回控制器的名稱。</span><span class="sxs-lookup"><span data-stu-id="39040-260">**SelectController** returns the name of the controller.</span></span>
- <span data-ttu-id="39040-261">**SelectAction**傳回動作的名稱。</span><span class="sxs-lookup"><span data-stu-id="39040-261">**SelectAction** returns the name of the action.</span></span>

<span data-ttu-id="39040-262">這兩種方法，如果慣例不適用於該要求，方法應該傳回 null。</span><span class="sxs-lookup"><span data-stu-id="39040-262">For both methods, if the convention does not apply to that request, the method should return null.</span></span>

<span data-ttu-id="39040-263">**ODataPath**參數所代表的已剖析的 OData 資源路徑。</span><span class="sxs-lookup"><span data-stu-id="39040-263">The **ODataPath** parameter represents the parsed OData resource path.</span></span> <span data-ttu-id="39040-264">它包含一份 **[ODataPathSegment](https://msdn.microsoft.com/en-us/library/system.web.http.odata.routing.odatapathsegment.aspx)** 執行個體，其中每個區段的資源路徑。</span><span class="sxs-lookup"><span data-stu-id="39040-264">It contains a list of **[ODataPathSegment](https://msdn.microsoft.com/en-us/library/system.web.http.odata.routing.odatapathsegment.aspx)** instances, one for each segment of the resource path.</span></span> <span data-ttu-id="39040-265">**ODataPathSegment**是抽象類別; 每個區段的類型由衍生自類別**ODataPathSegment**。</span><span class="sxs-lookup"><span data-stu-id="39040-265">**ODataPathSegment** is an abstract class; each segment type is represented by a class that derives from **ODataPathSegment**.</span></span>

<span data-ttu-id="39040-266">**ODataPath.TemplatePath**屬性是字串，代表串連所有的路徑區段。</span><span class="sxs-lookup"><span data-stu-id="39040-266">The **ODataPath.TemplatePath** property is a string that represents the concatenation all of the path segments.</span></span> <span data-ttu-id="39040-267">例如，如果 URI 是`/Products(1)/Supplier`，路徑範本&quot;~/entityset/key/navigation&quot;。</span><span class="sxs-lookup"><span data-stu-id="39040-267">For example, if the URI is `/Products(1)/Supplier`, the path template is &quot;~/entityset/key/navigation&quot;.</span></span> <span data-ttu-id="39040-268">請注意，區段不能直接對應到 URI 區段。</span><span class="sxs-lookup"><span data-stu-id="39040-268">Notice that the segments don't correspond directly to URI segments.</span></span> <span data-ttu-id="39040-269">例如，實體索引鍵 (1) 以其本身**ODataPathSegment**。</span><span class="sxs-lookup"><span data-stu-id="39040-269">For example, the entity key (1) is represented as its own **ODataPathSegment**.</span></span>

<span data-ttu-id="39040-270">一般而言，實作**IODataRoutingConvention**會進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="39040-270">Typically, an implementation of **IODataRoutingConvention** does the following:</span></span>

1. <span data-ttu-id="39040-271">比較看這個慣例會套用至目前要求的路徑範本。</span><span class="sxs-lookup"><span data-stu-id="39040-271">Compare the path template to see if this convention applies to the current request.</span></span> <span data-ttu-id="39040-272">如果不適用，則傳回 null。</span><span class="sxs-lookup"><span data-stu-id="39040-272">If it does not apply, return null.</span></span>
2. <span data-ttu-id="39040-273">適用於慣例，如果使用的屬性**ODataPathSegment**衍生控制器和動作名稱的執行個體。</span><span class="sxs-lookup"><span data-stu-id="39040-273">If the convention applies, use properties of the **ODataPathSegment** instances to derive controller and action names.</span></span>
3. <span data-ttu-id="39040-274">對於動作，加入路由字典，其中應該繫結至動作參數 （通常是實體索引鍵） 中的任何值。</span><span class="sxs-lookup"><span data-stu-id="39040-274">For actions, add any values to the route dictionary that should bind to the action parameters (typically entity keys).</span></span>

<span data-ttu-id="39040-275">讓我們看看特定範例。</span><span class="sxs-lookup"><span data-stu-id="39040-275">Let's look at a specific example.</span></span> <span data-ttu-id="39040-276">內建路由慣例不支援瀏覽集合中進行索引。</span><span class="sxs-lookup"><span data-stu-id="39040-276">The built-in routing conventions do not support indexing into a navigation collection.</span></span> <span data-ttu-id="39040-277">換句話說，沒有任何慣例 uri，如下所示：</span><span class="sxs-lookup"><span data-stu-id="39040-277">In other words, there is no convention for URIs like the following:</span></span>

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

<span data-ttu-id="39040-278">以下是查詢的自訂的路由慣例，來處理這種類型。</span><span class="sxs-lookup"><span data-stu-id="39040-278">Here is a custom routing convention to handle this type of query.</span></span>

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

<span data-ttu-id="39040-279">附註：</span><span class="sxs-lookup"><span data-stu-id="39040-279">Notes:</span></span>

1. <span data-ttu-id="39040-280">我是衍生自**EntitySetRoutingConvention**，因為**SelectController**該類別中的方法是適用於這個新的路由慣例。</span><span class="sxs-lookup"><span data-stu-id="39040-280">I derive from **EntitySetRoutingConvention**, because the **SelectController** method in that class is appropriate for this new routing convention.</span></span> <span data-ttu-id="39040-281">也就是說，不必重新實作**SelectController**。</span><span class="sxs-lookup"><span data-stu-id="39040-281">That means I don't need to re-implement **SelectController**.</span></span>
2. <span data-ttu-id="39040-282">慣例僅適用於 GET 要求和路徑範本時才&quot;~/entityset/key/navigation/key&quot;。</span><span class="sxs-lookup"><span data-stu-id="39040-282">The convention applies only to GET requests, and only when the path template is &quot;~/entityset/key/navigation/key&quot;.</span></span>
3. <span data-ttu-id="39040-283">動作名稱是&quot;取得 {EntityType}&quot;，其中*{EntityType}*是瀏覽集合的型別。</span><span class="sxs-lookup"><span data-stu-id="39040-283">The action name is &quot;Get{EntityType}&quot;, where *{EntityType}* is the type of the navigation collection.</span></span> <span data-ttu-id="39040-284">例如， &quot;GetSupplier&quot;。</span><span class="sxs-lookup"><span data-stu-id="39040-284">For example, &quot;GetSupplier&quot;.</span></span> <span data-ttu-id="39040-285">您可以使用任何您喜歡的命名慣例 &#8212;請確定您符合控制器的動作。</span><span class="sxs-lookup"><span data-stu-id="39040-285">You can use any naming convention that you like &#8212; just make sure your controller actions match.</span></span>
4. <span data-ttu-id="39040-286">採取的動作名稱為兩個參數*金鑰*和*relatedKey*。</span><span class="sxs-lookup"><span data-stu-id="39040-286">The action takes two parameters named *key* and *relatedKey*.</span></span> <span data-ttu-id="39040-287">(如需某些預先定義的參數名稱的清單，請參閱[ODataRouteConstants](https://msdn.microsoft.com/en-us/library/system.web.http.odata.routing.odatarouteconstants.aspx)。)</span><span class="sxs-lookup"><span data-stu-id="39040-287">(For a list of some predefined parameter names, see [ODataRouteConstants](https://msdn.microsoft.com/en-us/library/system.web.http.odata.routing.odatarouteconstants.aspx).)</span></span>

<span data-ttu-id="39040-288">下一個步驟新增新的慣例路由慣例的清單。</span><span class="sxs-lookup"><span data-stu-id="39040-288">The next step is adding the new convention to the list of routing conventions.</span></span> <span data-ttu-id="39040-289">發生這種情況在設定期間，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="39040-289">This happens during configuration, as shown in the following code:</span></span>

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

<span data-ttu-id="39040-290">以下是一些其他範例路由慣例來研究實用：</span><span class="sxs-lookup"><span data-stu-id="39040-290">Here are some other sample routing conventions that be useful to study:</span></span>

- [<span data-ttu-id="39040-291">CompositeKeyRoutingConvention</span><span class="sxs-lookup"><span data-stu-id="39040-291">CompositeKeyRoutingConvention</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [<span data-ttu-id="39040-292">CustomNavigationRoutingConvention</span><span class="sxs-lookup"><span data-stu-id="39040-292">CustomNavigationRoutingConvention</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [<span data-ttu-id="39040-293">NonBindableActionRoutingConvention</span><span class="sxs-lookup"><span data-stu-id="39040-293">NonBindableActionRoutingConvention</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [<span data-ttu-id="39040-294">ODataVersionRouteConstraint</span><span class="sxs-lookup"><span data-stu-id="39040-294">ODataVersionRouteConstraint</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

<span data-ttu-id="39040-295">和 Web API 本身當然是開放原始碼，因此您可以看到[原始程式碼](http://aspnetwebstack.codeplex.com/)的內建路由慣例。</span><span class="sxs-lookup"><span data-stu-id="39040-295">And of course Web API itself is open-source, so you can see the [source code](http://aspnetwebstack.codeplex.com/) for the built-in routing conventions.</span></span> <span data-ttu-id="39040-296">這些定義在**System.Web.Http.OData.Routing.Conventions**命名空間。</span><span class="sxs-lookup"><span data-stu-id="39040-296">These are defined in the **System.Web.Http.OData.Routing.Conventions** namespace.</span></span>
