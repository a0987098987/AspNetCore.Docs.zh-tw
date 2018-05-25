---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: 與 Web API 2 OData v3 支援實體關係 |Microsoft 文件
author: MikeWasson
description: 大部分的資料集定義實體之間的關聯性： 客戶具有訂單。書籍的作者。產品有供應商。 使用 OData 用戶端可以瀏覽透過...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: dec7e10e59cc2441c967afe062df227b105106a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a><span data-ttu-id="34c3b-104">在與 Web API 2 OData v3 支援實體的關聯</span><span class="sxs-lookup"><span data-stu-id="34c3b-104">Supporting Entity Relations in OData v3 with Web API 2</span></span>
====================
<span data-ttu-id="34c3b-105">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="34c3b-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="34c3b-106">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="34c3b-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="34c3b-107">大部分的資料集定義實體之間的關聯性： 客戶具有訂單。書籍的作者。產品有供應商。</span><span class="sxs-lookup"><span data-stu-id="34c3b-107">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="34c3b-108">使用 OData 用戶端可以瀏覽透過實體的關聯。</span><span class="sxs-lookup"><span data-stu-id="34c3b-108">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="34c3b-109">指定的產品，您可以找到供應商。</span><span class="sxs-lookup"><span data-stu-id="34c3b-109">Given a product, you can find the supplier.</span></span> <span data-ttu-id="34c3b-110">您也可以建立或移除關聯性。</span><span class="sxs-lookup"><span data-stu-id="34c3b-110">You can also create or remove relationships.</span></span> <span data-ttu-id="34c3b-111">例如，您可以設定產品的供應商。</span><span class="sxs-lookup"><span data-stu-id="34c3b-111">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="34c3b-112">本教學課程會示範如何在 ASP.NET Web API 中支援這些作業。</span><span class="sxs-lookup"><span data-stu-id="34c3b-112">This tutorial shows how to support these operations in ASP.NET Web API.</span></span> <span data-ttu-id="34c3b-113">本教學課程是根據本教學課程[建立 Web API 2 OData v3 端點](creating-an-odata-endpoint.md)。</span><span class="sxs-lookup"><span data-stu-id="34c3b-113">The tutorial builds on the tutorial [Creating an OData v3 Endpoint with Web API 2](creating-an-odata-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="34c3b-114">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="34c3b-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="34c3b-115">Web API 2</span><span class="sxs-lookup"><span data-stu-id="34c3b-115">Web API 2</span></span>
> - <span data-ttu-id="34c3b-116">OData 第 3 版</span><span class="sxs-lookup"><span data-stu-id="34c3b-116">OData Version 3</span></span>
> - <span data-ttu-id="34c3b-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="34c3b-117">Entity Framework 6</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="34c3b-118">新增 Supplier 實體</span><span class="sxs-lookup"><span data-stu-id="34c3b-118">Add a Supplier Entity</span></span>

<span data-ttu-id="34c3b-119">首先，我們需要將新的實體類型新增至我們的 OData 摘要。</span><span class="sxs-lookup"><span data-stu-id="34c3b-119">First we need to add a new entity type to our OData feed.</span></span> <span data-ttu-id="34c3b-120">我們會將新增`Supplier`類別。</span><span class="sxs-lookup"><span data-stu-id="34c3b-120">We'll add a `Supplier` class.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

<span data-ttu-id="34c3b-121">這個類別會使用實體索引鍵的字串。</span><span class="sxs-lookup"><span data-stu-id="34c3b-121">This class uses a string for the entity key.</span></span> <span data-ttu-id="34c3b-122">在實務上，可能會比使用整數索引鍵較不常見。</span><span class="sxs-lookup"><span data-stu-id="34c3b-122">In practice, that might be less common than using an integer key.</span></span> <span data-ttu-id="34c3b-123">但值得看到 OData 處理整數以外的其他金鑰類型的方式。</span><span class="sxs-lookup"><span data-stu-id="34c3b-123">But it's worth seeing how OData handles other key types besides integers.</span></span>

<span data-ttu-id="34c3b-124">接下來，我們將建立關聯，藉由新增`Supplier`屬性`Product`類別：</span><span class="sxs-lookup"><span data-stu-id="34c3b-124">Next, we'll create a relation by adding a `Supplier` property to the `Product` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

<span data-ttu-id="34c3b-125">加入新**DbSet**至`ProductServiceContext`類別，讓 Entity Framework 會包含`Supplier`資料庫中的資料表。</span><span class="sxs-lookup"><span data-stu-id="34c3b-125">Add a new **DbSet** to the `ProductServiceContext` class, so that Entity Framework will include the `Supplier` table in the database.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

<span data-ttu-id="34c3b-126">在 WebApiConfig.cs，加入 「 供應 」 實體的 EDM 模型：</span><span class="sxs-lookup"><span data-stu-id="34c3b-126">In WebApiConfig.cs, add a "Suppliers" entity to the EDM model:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a><span data-ttu-id="34c3b-127">導覽屬性</span><span class="sxs-lookup"><span data-stu-id="34c3b-127">Navigation Properties</span></span>

<span data-ttu-id="34c3b-128">若要取得之產品的供應商，用戶端會傳送 GET 要求：</span><span class="sxs-lookup"><span data-stu-id="34c3b-128">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

<span data-ttu-id="34c3b-129">「 供應商 」 是導覽屬性上的以下`Product`型別。</span><span class="sxs-lookup"><span data-stu-id="34c3b-129">Here "Supplier" is a navigation property on the `Product` type.</span></span> <span data-ttu-id="34c3b-130">在此情況下，`Supplier`指的是單一項目，但導覽屬性也可以傳回 （-一對多或多對多關聯性） 的集合。</span><span class="sxs-lookup"><span data-stu-id="34c3b-130">In this case, `Supplier` refers to a single item, but a navigation property can also return a collection (one-to-many or many-to-many relation).</span></span>

<span data-ttu-id="34c3b-131">若要支援此要求，加入下列方法加入`ProductsController`類別：</span><span class="sxs-lookup"><span data-stu-id="34c3b-131">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

<span data-ttu-id="34c3b-132">*金鑰*參數是產品的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="34c3b-132">The *key* parameter is the key of the product.</span></span> <span data-ttu-id="34c3b-133">方法會傳回相關的實體 & #8212 在此情況下，`Supplier`執行個體。</span><span class="sxs-lookup"><span data-stu-id="34c3b-133">The method returns the related entity&#8212in this case, a `Supplier` instance.</span></span> <span data-ttu-id="34c3b-134">方法名稱和參數名稱都很重要。</span><span class="sxs-lookup"><span data-stu-id="34c3b-134">The method name and parameter name are both important.</span></span> <span data-ttu-id="34c3b-135">一般情況下，如果導覽屬性名為"X"，您需要加入名為"GetX"的方法。</span><span class="sxs-lookup"><span data-stu-id="34c3b-135">In general, if the navigation property is named "X", you need to add a method named "GetX".</span></span> <span data-ttu-id="34c3b-136">此方法必須採用名為"*金鑰*"符合父系的索引鍵的資料類型。</span><span class="sxs-lookup"><span data-stu-id="34c3b-136">The method must take a parameter named "*key*" that matches the data type of the parent's key.</span></span>

<span data-ttu-id="34c3b-137">它也是一定要包含 **[FromOdataUri]** 屬性*金鑰*參數。</span><span class="sxs-lookup"><span data-stu-id="34c3b-137">It is also important to include the **[FromOdataUri]** attribute in the *key* parameter.</span></span> <span data-ttu-id="34c3b-138">這個屬性會告知使用 OData 語法規則，它會剖析要求 URI 中的索引鍵時的 Web API。</span><span class="sxs-lookup"><span data-stu-id="34c3b-138">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

## <a name="creating-and-deleting-links"></a><span data-ttu-id="34c3b-139">建立和刪除連結</span><span class="sxs-lookup"><span data-stu-id="34c3b-139">Creating and Deleting Links</span></span>

<span data-ttu-id="34c3b-140">OData 支援建立或移除兩個實體之間的關聯性。</span><span class="sxs-lookup"><span data-stu-id="34c3b-140">OData supports creating or removing relationships between two entities.</span></span> <span data-ttu-id="34c3b-141">在 OData 術語中，關聯性是 「 連結 」。</span><span class="sxs-lookup"><span data-stu-id="34c3b-141">In OData terminology, the relationship is a "link."</span></span> <span data-ttu-id="34c3b-142">每個連結都與表單 URI*實體*/$links /*實體*。</span><span class="sxs-lookup"><span data-stu-id="34c3b-142">Each link has a URI with the form *entity*/$links/*entity*.</span></span> <span data-ttu-id="34c3b-143">例如，產品的供應商的連結看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="34c3b-143">For example, the link from product to supplier looks like this:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

<span data-ttu-id="34c3b-144">若要建立新的連結，用戶端會傳送 POST 要求到 URI 的連結。</span><span class="sxs-lookup"><span data-stu-id="34c3b-144">To create a new link, the client sends a POST request to the link URI.</span></span> <span data-ttu-id="34c3b-145">在要求主體是目標實體的 URI。</span><span class="sxs-lookup"><span data-stu-id="34c3b-145">The body of the request is the URI of the target entity.</span></span> <span data-ttu-id="34c3b-146">例如，假設具有索引鍵 」 CTSO 「 供應商。</span><span class="sxs-lookup"><span data-stu-id="34c3b-146">For example, suppose there is a supplier with the key "CTSO".</span></span> <span data-ttu-id="34c3b-147">若要建立從 「 Product(1)"至"Supplier('CTSO') 」 的連結，用戶端會傳送要求，如下所示：</span><span class="sxs-lookup"><span data-stu-id="34c3b-147">To create a link from "Product(1)" to "Supplier('CTSO')", the client sends a request like the following:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

<span data-ttu-id="34c3b-148">若要刪除的連結，用戶端會傳送給連結 URI 將 DELETE 要求。</span><span class="sxs-lookup"><span data-stu-id="34c3b-148">To delete a link, the client sends a DELETE request to the link URI.</span></span>

<span data-ttu-id="34c3b-149">**建立連結**</span><span class="sxs-lookup"><span data-stu-id="34c3b-149">**Creating Links**</span></span>

<span data-ttu-id="34c3b-150">若要啟用用戶端建立產品供應商的連結，加入下列程式碼加入`ProductsController`類別：</span><span class="sxs-lookup"><span data-stu-id="34c3b-150">To enable a client to create product-supplier links, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

<span data-ttu-id="34c3b-151">這個方法會採用三個參數：</span><span class="sxs-lookup"><span data-stu-id="34c3b-151">This method takes three parameters:</span></span>

- <span data-ttu-id="34c3b-152">*索引鍵*： 父實體 (product) 的索引鍵</span><span class="sxs-lookup"><span data-stu-id="34c3b-152">*key*: The key to the parent entity (the product)</span></span>
- <span data-ttu-id="34c3b-153">*navigationProperty*： 瀏覽屬性的名稱。</span><span class="sxs-lookup"><span data-stu-id="34c3b-153">*navigationProperty*: The name of the navigation property.</span></span> <span data-ttu-id="34c3b-154">在此範例中，唯一有效的導覽屬性會是 「 供應商 」。</span><span class="sxs-lookup"><span data-stu-id="34c3b-154">In this example, the only valid navigation property is "Supplier".</span></span>
- <span data-ttu-id="34c3b-155">*連結*: OData URI 的相關實體。</span><span class="sxs-lookup"><span data-stu-id="34c3b-155">*link*: The OData URI of the related entity.</span></span> <span data-ttu-id="34c3b-156">這個值是取自的要求主體。</span><span class="sxs-lookup"><span data-stu-id="34c3b-156">This value is taken from the request body.</span></span> <span data-ttu-id="34c3b-157">例如，連結 URI 可能是"`http://localhost/odata/Suppliers('CTSO')`，這表示供應商識別碼 = 'CTSO'。</span><span class="sxs-lookup"><span data-stu-id="34c3b-157">For example, the link URI might be "`http://localhost/odata/Suppliers('CTSO')`, meaning the supplier with ID = ‘CTSO'.</span></span>

<span data-ttu-id="34c3b-158">方法會使用連結來尋找供應商。</span><span class="sxs-lookup"><span data-stu-id="34c3b-158">The method uses the link to look up the supplier.</span></span> <span data-ttu-id="34c3b-159">如果找到相符的供應商，則方法會設定`Product.Supplier`屬性，並將結果儲存到資料庫。</span><span class="sxs-lookup"><span data-stu-id="34c3b-159">If the matching supplier is found, the method sets the `Product.Supplier` property and saves the result to the database.</span></span>

<span data-ttu-id="34c3b-160">最困難的部分剖析 URI 的連結。</span><span class="sxs-lookup"><span data-stu-id="34c3b-160">The hardest part is parsing the link URI.</span></span> <span data-ttu-id="34c3b-161">基本上，您要模擬的結果將 GET 要求傳送至該 URI。</span><span class="sxs-lookup"><span data-stu-id="34c3b-161">Basically, you need to simulate the result of sending a GET request to that URI.</span></span> <span data-ttu-id="34c3b-162">下列 helper 方法顯示如何執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="34c3b-162">The following helper method shows how to do this.</span></span> <span data-ttu-id="34c3b-163">方法會叫用 Web API 路由程序，並取得回**ODataPath**執行個體，表示已剖析的 OData 路徑。</span><span class="sxs-lookup"><span data-stu-id="34c3b-163">The method invokes the Web API routing process and gets back an **ODataPath** instance that represents the parsed OData path.</span></span> <span data-ttu-id="34c3b-164">連結 URI，其中一個區段應該是實體索引鍵。</span><span class="sxs-lookup"><span data-stu-id="34c3b-164">For a link URI, one of the segments should be the entity key.</span></span> <span data-ttu-id="34c3b-165">（如果沒有，用戶端傳送不正確的 URI。）</span><span class="sxs-lookup"><span data-stu-id="34c3b-165">(If not, the client sent a bad URI.)</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

<span data-ttu-id="34c3b-166">**刪除連結**</span><span class="sxs-lookup"><span data-stu-id="34c3b-166">**Deleting Links**</span></span>

<span data-ttu-id="34c3b-167">若要刪除的連結，加入下列程式碼加入`ProductsController`類別：</span><span class="sxs-lookup"><span data-stu-id="34c3b-167">To delete a link, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

<span data-ttu-id="34c3b-168">在此範例中，導覽屬性是單一`Supplier`實體。</span><span class="sxs-lookup"><span data-stu-id="34c3b-168">In this example, the navigation property is a single `Supplier` entity.</span></span> <span data-ttu-id="34c3b-169">如果導覽屬性是集合，來刪除連結的 URI 必須包含相關的實體索引鍵。</span><span class="sxs-lookup"><span data-stu-id="34c3b-169">If the navigation property is a collection, the URI to delete a link must include a key for the related entity.</span></span> <span data-ttu-id="34c3b-170">例如: </span><span class="sxs-lookup"><span data-stu-id="34c3b-170">For example:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

<span data-ttu-id="34c3b-171">此要求會移除客戶 1 中的順序為 1。</span><span class="sxs-lookup"><span data-stu-id="34c3b-171">This request removes order 1 from customer 1.</span></span> <span data-ttu-id="34c3b-172">在此情況下，DeleteLink 方法會具有下列簽章：</span><span class="sxs-lookup"><span data-stu-id="34c3b-172">In this case, the DeleteLink method will have the following signature:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

<span data-ttu-id="34c3b-173">*RelatedKey*參數會提供相關實體的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="34c3b-173">The *relatedKey* parameter gives the key for the related entity.</span></span> <span data-ttu-id="34c3b-174">因此在您`DeleteLink`方法時，主要實體的查閱*金鑰*參數，找出相關的實體所*relatedKey*參數，然後移除關聯。</span><span class="sxs-lookup"><span data-stu-id="34c3b-174">So in your `DeleteLink` method, look up the primary entity by the *key* parameter, find the related entity by the *relatedKey* parameter, and then remove the association.</span></span> <span data-ttu-id="34c3b-175">根據您的資料模型，您可能需要實作這兩個版本的`DeleteLink`。</span><span class="sxs-lookup"><span data-stu-id="34c3b-175">Depending on your data model, you might need to implement both versions of `DeleteLink`.</span></span> <span data-ttu-id="34c3b-176">Web API 會呼叫要求 URI 為基礎的正確版本。</span><span class="sxs-lookup"><span data-stu-id="34c3b-176">Web API will call the correct version based on the request URI.</span></span>
