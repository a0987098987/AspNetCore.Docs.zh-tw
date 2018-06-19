---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: 使用 ASP.NET Web API 2.2 OData v4 中的實體關聯 |Microsoft 文件
author: MikeWasson
description: 大部分的資料集定義實體之間的關聯性： 客戶具有訂單。書籍的作者。產品有供應商。 使用 OData 用戶端可以瀏覽透過...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 4127fb2fa83f513a4faeb222068ca99f234ece22
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508097"
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="68a55-104">使用 ASP.NET Web API 2.2 OData v4 中的實體關聯</span><span class="sxs-lookup"><span data-stu-id="68a55-104">Entity Relations in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="68a55-105">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="68a55-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="68a55-106">大部分的資料集定義實體之間的關聯性： 客戶具有訂單。書籍的作者。產品有供應商。</span><span class="sxs-lookup"><span data-stu-id="68a55-106">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="68a55-107">使用 OData 用戶端可以瀏覽透過實體的關聯。</span><span class="sxs-lookup"><span data-stu-id="68a55-107">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="68a55-108">指定的產品，您可以找到供應商。</span><span class="sxs-lookup"><span data-stu-id="68a55-108">Given a product, you can find the supplier.</span></span> <span data-ttu-id="68a55-109">您也可以建立或移除關聯性。</span><span class="sxs-lookup"><span data-stu-id="68a55-109">You can also create or remove relationships.</span></span> <span data-ttu-id="68a55-110">例如，您可以設定產品的供應商。</span><span class="sxs-lookup"><span data-stu-id="68a55-110">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="68a55-111">本教學課程會示範如何支援使用 ASP.NET Web API OData v4 這些作業。</span><span class="sxs-lookup"><span data-stu-id="68a55-111">This tutorial shows how to support these operations in OData v4 using ASP.NET Web API.</span></span> <span data-ttu-id="68a55-112">本教學課程是根據本教學課程[建立 OData v4 端點使用 ASP.NET Web API 2](create-an-odata-v4-endpoint.md)。</span><span class="sxs-lookup"><span data-stu-id="68a55-112">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="68a55-113">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="68a55-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="68a55-114">Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="68a55-114">Web API 2.1</span></span>
> - <span data-ttu-id="68a55-115">OData v4</span><span class="sxs-lookup"><span data-stu-id="68a55-115">OData v4</span></span>
> - [<span data-ttu-id="68a55-116">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="68a55-116">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="68a55-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="68a55-117">Entity Framework 6</span></span>
> - <span data-ttu-id="68a55-118">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="68a55-118">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="68a55-119">教學課程版本</span><span class="sxs-lookup"><span data-stu-id="68a55-119">Tutorial versions</span></span>
> 
> <span data-ttu-id="68a55-120">OData 第 3 版，請參閱[支援 OData v3 中的實體關聯](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations)。</span><span class="sxs-lookup"><span data-stu-id="68a55-120">For the OData Version 3, see [Supporting Entity Relations in OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="68a55-121">新增 Supplier 實體</span><span class="sxs-lookup"><span data-stu-id="68a55-121">Add a Supplier Entity</span></span>

> [!NOTE]
> <span data-ttu-id="68a55-122">本教學課程是根據本教學課程[建立 OData v4 端點使用 ASP.NET Web API 2](create-an-odata-v4-endpoint.md)。</span><span class="sxs-lookup"><span data-stu-id="68a55-122">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>


<span data-ttu-id="68a55-123">首先，我們需要相關的實體。</span><span class="sxs-lookup"><span data-stu-id="68a55-123">First, we need a related entity.</span></span> <span data-ttu-id="68a55-124">將類別命名為`Supplier`Models 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="68a55-124">Add a class named `Supplier` in the Models folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

<span data-ttu-id="68a55-125">將導覽屬性，加入`Product`類別：</span><span class="sxs-lookup"><span data-stu-id="68a55-125">Add a navigation property to the `Product` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

<span data-ttu-id="68a55-126">加入新**DbSet**至`ProductsContext`類別，讓 Entity Framework 會包含在資料庫中的供應商資料表。</span><span class="sxs-lookup"><span data-stu-id="68a55-126">Add a new **DbSet** to the `ProductsContext` class, so that Entity Framework will include the Supplier table in the database.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

<span data-ttu-id="68a55-127">在 WebApiConfig.cs，加入&quot;供應商&quot;實體集的實體資料模型：</span><span class="sxs-lookup"><span data-stu-id="68a55-127">In WebApiConfig.cs, add a &quot;Suppliers&quot; entity set to the entity data model:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a><span data-ttu-id="68a55-128">新增的供應商控制站</span><span class="sxs-lookup"><span data-stu-id="68a55-128">Add a Suppliers Controller</span></span>

<span data-ttu-id="68a55-129">新增`SuppliersController`類別到 [控制器] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="68a55-129">Add a `SuppliersController` class to the Controllers folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="68a55-130">我不會顯示如何新增此控制器的 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="68a55-130">I won't show how to add CRUD operations for this controller.</span></span> <span data-ttu-id="68a55-131">會與產品控制站相同的步驟 (請參閱[建立 OData v4 端點](create-an-odata-v4-endpoint.md))。</span><span class="sxs-lookup"><span data-stu-id="68a55-131">The steps are the same as for the Products controller (see [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md)).</span></span>

## <a name="getting-related-entities"></a><span data-ttu-id="68a55-132">取得相關的實體</span><span class="sxs-lookup"><span data-stu-id="68a55-132">Getting Related Entities</span></span>

<span data-ttu-id="68a55-133">若要取得之產品的供應商，用戶端會傳送 GET 要求：</span><span class="sxs-lookup"><span data-stu-id="68a55-133">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

<span data-ttu-id="68a55-134">若要支援此要求，加入下列方法加入`ProductsController`類別：</span><span class="sxs-lookup"><span data-stu-id="68a55-134">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

<span data-ttu-id="68a55-135">這個方法會使用預設的命名慣例</span><span class="sxs-lookup"><span data-stu-id="68a55-135">This method uses a default naming convention</span></span>

- <span data-ttu-id="68a55-136">方法名稱： GetX，其中 X 是導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="68a55-136">Method name: GetX, where X is the navigation property.</span></span>
- <span data-ttu-id="68a55-137">參數名稱：*金鑰*</span><span class="sxs-lookup"><span data-stu-id="68a55-137">Parameter name: *key*</span></span>

<span data-ttu-id="68a55-138">如果您遵照這個命名慣例，Web 應用程式開發介面會自動將 HTTP 要求對應至控制器方法。</span><span class="sxs-lookup"><span data-stu-id="68a55-138">If you follow this naming convention, Web API automatically maps the HTTP request to the controller method.</span></span>

<span data-ttu-id="68a55-139">範例 HTTP 要求：</span><span class="sxs-lookup"><span data-stu-id="68a55-139">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

<span data-ttu-id="68a55-140">範例 HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="68a55-140">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a><span data-ttu-id="68a55-141">取得相關的集合</span><span class="sxs-lookup"><span data-stu-id="68a55-141">Getting a related collection</span></span>

<span data-ttu-id="68a55-142">在上述範例中，產品會有一家供應商。</span><span class="sxs-lookup"><span data-stu-id="68a55-142">In the previous example, a product has one supplier.</span></span> <span data-ttu-id="68a55-143">導覽屬性也可以傳回的集合。</span><span class="sxs-lookup"><span data-stu-id="68a55-143">A navigation property can also return a collection.</span></span> <span data-ttu-id="68a55-144">下列程式碼會取得產品的供應商：</span><span class="sxs-lookup"><span data-stu-id="68a55-144">The following code gets the products for a supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

<span data-ttu-id="68a55-145">在此情況下，方法會傳回**IQueryable**而不是**SingleResult&lt;T&gt;**</span><span class="sxs-lookup"><span data-stu-id="68a55-145">In this case, the method returns an **IQueryable** instead of a **SingleResult&lt;T&gt;**</span></span>

<span data-ttu-id="68a55-146">範例 HTTP 要求：</span><span class="sxs-lookup"><span data-stu-id="68a55-146">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

<span data-ttu-id="68a55-147">範例 HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="68a55-147">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a><span data-ttu-id="68a55-148">建立實體之間的關聯性</span><span class="sxs-lookup"><span data-stu-id="68a55-148">Creating a Relationship Between Entities</span></span>

<span data-ttu-id="68a55-149">OData 支援建立或移除現有的兩個實體之間的關聯性。</span><span class="sxs-lookup"><span data-stu-id="68a55-149">OData supports creating or removing relationships between two existing entities.</span></span> <span data-ttu-id="68a55-150">OData v4 術語中，在關聯性是&quot;參考&quot;。</span><span class="sxs-lookup"><span data-stu-id="68a55-150">In OData v4 terminology, the relationship is a &quot;reference&quot;.</span></span> <span data-ttu-id="68a55-151">(關聯性已呼叫 OData v3*連結*。</span><span class="sxs-lookup"><span data-stu-id="68a55-151">(In OData v3, the relationship was called a *link*.</span></span> <span data-ttu-id="68a55-152">通訊協定差異不影響此教學課程中。）</span><span class="sxs-lookup"><span data-stu-id="68a55-152">The protocol differences don't matter for this tutorial.)</span></span>

<span data-ttu-id="68a55-153">參考都有它自己的 URI，使用此表單`/Entity/NavigationProperty/$ref`。</span><span class="sxs-lookup"><span data-stu-id="68a55-153">A reference has its own URI, with the form `/Entity/NavigationProperty/$ref`.</span></span> <span data-ttu-id="68a55-154">例如，以下是 URI 定址的產品和產品的供應商之間的參考：</span><span class="sxs-lookup"><span data-stu-id="68a55-154">For example, here is the URI to address the reference between a product and its supplier:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

<span data-ttu-id="68a55-155">若要加入關聯性，用戶端傳送 POST 或 PUT 要求到這個地址。</span><span class="sxs-lookup"><span data-stu-id="68a55-155">To add a relationship, the client sends a POST or PUT request to this address.</span></span>

- <span data-ttu-id="68a55-156">如果導覽屬性是單一實體，例如將`Product.Supplier`。</span><span class="sxs-lookup"><span data-stu-id="68a55-156">PUT if the navigation property is a single entity, such as `Product.Supplier`.</span></span>
- <span data-ttu-id="68a55-157">如果導覽屬性是集合，例如張貼`Supplier.Products`。</span><span class="sxs-lookup"><span data-stu-id="68a55-157">POST if the navigation property is a collection, such as `Supplier.Products`.</span></span>

<span data-ttu-id="68a55-158">在要求主體包含關聯性中的其他實體的 URI。</span><span class="sxs-lookup"><span data-stu-id="68a55-158">The body of the request contains the URI of the other entity in the relation.</span></span> <span data-ttu-id="68a55-159">以下是範例要求：</span><span class="sxs-lookup"><span data-stu-id="68a55-159">Here is an example request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

<span data-ttu-id="68a55-160">在此範例中，用戶端傳送 PUT 要求到`/Products(6)/Supplier/$ref`，這是 $ref URI`Supplier`的產品 id = 6。</span><span class="sxs-lookup"><span data-stu-id="68a55-160">In this example, the client sends a PUT request to `/Products(6)/Supplier/$ref`, which is the $ref URI for the `Supplier` of the product with ID = 6.</span></span> <span data-ttu-id="68a55-161">如果要求成功，伺服器會傳送 「 204 （沒有內容） 的回應：</span><span class="sxs-lookup"><span data-stu-id="68a55-161">If the request succeeds, the server sends a 204 (No Content) response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

<span data-ttu-id="68a55-162">以下是要加入的關聯性的控制器方法`Product`:</span><span class="sxs-lookup"><span data-stu-id="68a55-162">Here is the controller method to add a relationship to a `Product`:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

<span data-ttu-id="68a55-163">*NavigationProperty*參數會指定要設定哪一個關聯性。</span><span class="sxs-lookup"><span data-stu-id="68a55-163">The *navigationProperty* parameter specifies which relationship to set.</span></span> <span data-ttu-id="68a55-164">(如果實體有一個以上的導覽屬性，您可以加入更多`case`陳述式。)</span><span class="sxs-lookup"><span data-stu-id="68a55-164">(If there is more than one navigation property on the entity, you can add more `case` statements.)</span></span>

<span data-ttu-id="68a55-165">*連結*參數包含供應商的 URI。</span><span class="sxs-lookup"><span data-stu-id="68a55-165">The *link* parameter contains the URI of the supplier.</span></span> <span data-ttu-id="68a55-166">Web 應用程式開發介面會自動剖析要求主體，以取得此參數的值。</span><span class="sxs-lookup"><span data-stu-id="68a55-166">Web API automatically parses the request body to get the value for this parameter.</span></span>

<span data-ttu-id="68a55-167">若要查閱供應商，我們需要的識別碼 （或金鑰），這是屬於*連結*參數。</span><span class="sxs-lookup"><span data-stu-id="68a55-167">To look up the supplier, we need the ID (or key), which is part of the *link* parameter.</span></span> <span data-ttu-id="68a55-168">若要這樣做，請使用下列 helper 方法：</span><span class="sxs-lookup"><span data-stu-id="68a55-168">To do this, use the following helper method:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

<span data-ttu-id="68a55-169">基本上，此方法會使用 OData 程式庫分成區段中的 URI 路徑中，尋找區段包含索引鍵，並將索引鍵轉換成正確的型別。</span><span class="sxs-lookup"><span data-stu-id="68a55-169">Basically, this method uses the OData library to split the URI path into segments, find the segment that contains the key, and convert the key into the correct type.</span></span>

## <a name="deleting-a-relationship-between-entities"></a><span data-ttu-id="68a55-170">刪除實體之間的關聯性</span><span class="sxs-lookup"><span data-stu-id="68a55-170">Deleting a Relationship Between Entities</span></span>

<span data-ttu-id="68a55-171">若要刪除關聯性，用戶端會傳送 HTTP DELETE 要求給 $ref URI:</span><span class="sxs-lookup"><span data-stu-id="68a55-171">To delete a relationship, the client sends an HTTP DELETE request to the $ref URI:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

<span data-ttu-id="68a55-172">以下是控制器方法刪除產品與供應商之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="68a55-172">Here is the controller method to delete the relationship between a Product and a Supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

<span data-ttu-id="68a55-173">在此情況下，`Product.Supplier`是&quot;1&quot; 1 對多關聯性，所以您可以移除關聯性，只要設定結尾`Product.Supplier`至`null`。</span><span class="sxs-lookup"><span data-stu-id="68a55-173">In this case, `Product.Supplier` is the &quot;1&quot; end of a 1-to-many relation, so you can remove the relationship just by setting `Product.Supplier` to `null`.</span></span>

<span data-ttu-id="68a55-174">在&quot;許多&quot;關聯性，用戶端必須指定哪些相關實體中移除。</span><span class="sxs-lookup"><span data-stu-id="68a55-174">In the &quot;many&quot; end of a relationship, the client must specify which related entity to remove.</span></span> <span data-ttu-id="68a55-175">若要這樣做，用戶端傳送要求的查詢字串中的相關實體的 URI。</span><span class="sxs-lookup"><span data-stu-id="68a55-175">To do so, the client sends the URI of the related entity in the query string of the request.</span></span> <span data-ttu-id="68a55-176">例如，若要移除 「 產品 1"從"1"的供應商：</span><span class="sxs-lookup"><span data-stu-id="68a55-176">For example, to remove "Product 1" from "Supplier 1":</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

<span data-ttu-id="68a55-177">若要支援此 Web API 中，我們需要加入額外的參數中`DeleteRef`方法。</span><span class="sxs-lookup"><span data-stu-id="68a55-177">To support this in Web API, we need to include an extra parameter in the `DeleteRef` method.</span></span> <span data-ttu-id="68a55-178">以下是要移除的產品，從控制器方法`Supplier.Products`關聯。</span><span class="sxs-lookup"><span data-stu-id="68a55-178">Here is the controller method to remove a product from the `Supplier.Products` relation.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

<span data-ttu-id="68a55-179">*金鑰*參數是供應商的索引鍵和*relatedKey*參數是要移除的產品金鑰`Products`關聯性。</span><span class="sxs-lookup"><span data-stu-id="68a55-179">The *key* parameter is the key for the supplier, and the *relatedKey* parameter is the key for the product to remove from the `Products` relationship.</span></span> <span data-ttu-id="68a55-180">請注意，Web 應用程式開發介面會自動從查詢字串取得的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="68a55-180">Note that Web API automatically gets the key from the query string.</span></span>
