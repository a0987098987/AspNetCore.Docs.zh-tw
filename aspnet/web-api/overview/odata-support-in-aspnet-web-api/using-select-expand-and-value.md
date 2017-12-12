---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: "使用 $select，$expand、 與 ASP.NET Web API 2 OData 中的 $value |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/11/2013
ms.topic: article
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: f229cdbd8850a787dd3585e0640e8e66f6109331
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a><span data-ttu-id="be431-102">使用 $select，$expand、 與 ASP.NET Web API 2 OData 中的 $value</span><span class="sxs-lookup"><span data-stu-id="be431-102">Using $select, $expand, and $value in ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="be431-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="be431-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="be431-104">Web API 2 新增的支援 $ expand $select 和 OData $value 選項。</span><span class="sxs-lookup"><span data-stu-id="be431-104">Web API 2 adds support for the $expand, $select, and $value options in OData.</span></span> <span data-ttu-id="be431-105">這些選項可讓用戶端來控制從伺服器取回的表示法。</span><span class="sxs-lookup"><span data-stu-id="be431-105">These options allow a client to control the representation that it gets back from the server.</span></span>

- <span data-ttu-id="be431-106">**$expand**造成要在回應中的包含的內嵌的相關的實體。</span><span class="sxs-lookup"><span data-stu-id="be431-106">**$expand** causes related entities to be included inline in the response.</span></span>
- <span data-ttu-id="be431-107">**$select**選取要包含在回應中的屬性子集。</span><span class="sxs-lookup"><span data-stu-id="be431-107">**$select** selects a subset of properties to include in the response.</span></span>
- <span data-ttu-id="be431-108">**$value**取得屬性的原始值。</span><span class="sxs-lookup"><span data-stu-id="be431-108">**$value** gets the raw value of a property.</span></span>

## <a name="example-schema"></a><span data-ttu-id="be431-109">範例結構描述</span><span class="sxs-lookup"><span data-stu-id="be431-109">Example Schema</span></span>

<span data-ttu-id="be431-110">本文中，我將使用的 OData 服務，會定義三個實體： 產品、 供應商和類別目錄。</span><span class="sxs-lookup"><span data-stu-id="be431-110">For this article, I'll use an OData service that defines three entities: Product, Supplier, and Category.</span></span> <span data-ttu-id="be431-111">每個產品具有一個類別目錄和一個供應商。</span><span class="sxs-lookup"><span data-stu-id="be431-111">Each product has one category and one supplier.</span></span>

![](using-select-expand-and-value/_static/image1.png)

<span data-ttu-id="be431-112">以下是 C# 類別，定義實體模型：</span><span class="sxs-lookup"><span data-stu-id="be431-112">Here are the C# classes that define the entity models:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

<span data-ttu-id="be431-113">請注意，`Product`類別定義導覽屬性`Supplier`和`Category`。</span><span class="sxs-lookup"><span data-stu-id="be431-113">Notice that the `Product` class defines navigation properties for the `Supplier` and `Category`.</span></span> <span data-ttu-id="be431-114">`Category`類別會定義每個類別中之產品的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="be431-114">The `Category` class defines a navigation property for the products in each category.</span></span>

<span data-ttu-id="be431-115">若要建立此結構描述的 OData 端點，使用中所述的 Visual Studio 2013 scaffolding，[建立 ASP.NET Web API 中的 OData 端點](odata-v3/creating-an-odata-endpoint.md)。</span><span class="sxs-lookup"><span data-stu-id="be431-115">To create an OData endpoint for this schema, use the Visual Studio 2013 scaffolding, as described in [Creating an OData Endpoint in ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span></span> <span data-ttu-id="be431-116">新增產品、 類別和供應商的個別控制站。</span><span class="sxs-lookup"><span data-stu-id="be431-116">Add separate controllers for Product, Category, and Supplier.</span></span>

## <a name="enabling-expand-and-select"></a><span data-ttu-id="be431-117">啟用 $展開和 $select</span><span class="sxs-lookup"><span data-stu-id="be431-117">Enabling $expand and $select</span></span>

<span data-ttu-id="be431-118">在 Visual Studio 2013 中，Web API OData scaffolding 該自動支援 $expand 和 $select 建立控制器。</span><span class="sxs-lookup"><span data-stu-id="be431-118">In Visual Studio 2013, the Web API OData scaffolding creates a controller that automatically supports $expand and $select.</span></span> <span data-ttu-id="be431-119">如需參考，以下是支援 $ 需求展開和 $select 控制器中的。</span><span class="sxs-lookup"><span data-stu-id="be431-119">For reference, here are the requirements to support $expand and $select in a controller.</span></span>

<span data-ttu-id="be431-120">對於集合，控制站的`Get`方法必須傳回**IQueryable**。</span><span class="sxs-lookup"><span data-stu-id="be431-120">For collections, the controller's `Get` method must return an **IQueryable**.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

<span data-ttu-id="be431-121">對於單一實體，會傳回**SingleResult&lt;T&gt;**，其中 T 是**IQueryable** ，其中包含零個或一個實體。</span><span class="sxs-lookup"><span data-stu-id="be431-121">For single entities, return a **SingleResult&lt;T&gt;**, where T is an **IQueryable** that contains zero or one entities.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

<span data-ttu-id="be431-122">此外，裝飾您`Get`方法**[Queryable]**屬性，如先前的程式碼片段所示。</span><span class="sxs-lookup"><span data-stu-id="be431-122">Also, decorate your `Get` methods with the **[Queryable]** attribute, as shown in the previous code snippets.</span></span> <span data-ttu-id="be431-123">或者，呼叫**EnableQuerySupport**上**HttpConfiguration**在啟動時的物件。</span><span class="sxs-lookup"><span data-stu-id="be431-123">Alternatively, call **EnableQuerySupport** on the **HttpConfiguration** object at startup.</span></span> <span data-ttu-id="be431-124">(如需詳細資訊，請參閱[啟用 OData 查詢選項](supporting-odata-query-options.md#enable)。)</span><span class="sxs-lookup"><span data-stu-id="be431-124">(For more information, see [Enabling OData Query Options](supporting-odata-query-options.md#enable).)</span></span>

## <a name="using-expand"></a><span data-ttu-id="be431-125">使用 $展開</span><span class="sxs-lookup"><span data-stu-id="be431-125">Using $expand</span></span>

<span data-ttu-id="be431-126">當您查詢 OData 實體或集合時，預設回應不包含相關的實體。</span><span class="sxs-lookup"><span data-stu-id="be431-126">When you query an OData entity or collection, the default response does not include related entities.</span></span> <span data-ttu-id="be431-127">例如，以下是分類實體集的預設回應：</span><span class="sxs-lookup"><span data-stu-id="be431-127">For example, here is the default response for the Categories entity set:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

<span data-ttu-id="be431-128">如您所見，回應不包含任何產品，即使類別目錄 」 實體包含產品的導覽連結。</span><span class="sxs-lookup"><span data-stu-id="be431-128">As you can see, the response does not include any products, even though the Category entity has a Products navigation link.</span></span> <span data-ttu-id="be431-129">不過，用戶端可以使用 $展開來取得每個類別目錄的產品清單。</span><span class="sxs-lookup"><span data-stu-id="be431-129">However, the client can use $expand to get the list of products for each category.</span></span> <span data-ttu-id="be431-130">$Expand 選項則是放在要求的查詢字串：</span><span class="sxs-lookup"><span data-stu-id="be431-130">The $expand option goes in the query string of the request:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

<span data-ttu-id="be431-131">現在伺服器將會包含每個類別目錄，而這些分類則內嵌的產品。</span><span class="sxs-lookup"><span data-stu-id="be431-131">Now the server will include the products for each category, inline with the categories.</span></span> <span data-ttu-id="be431-132">以下是回應裝載：</span><span class="sxs-lookup"><span data-stu-id="be431-132">Here is the response payload:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

<span data-ttu-id="be431-133">請注意每個項目"value"陣列中的包含產品清單。</span><span class="sxs-lookup"><span data-stu-id="be431-133">Notice that each entry in the "value" array contains a Products list.</span></span>

<span data-ttu-id="be431-134">$ Expand 選項會採用以逗號分隔清單的導覽屬性，以展開。</span><span class="sxs-lookup"><span data-stu-id="be431-134">The $expand option takes a comma-separated list of navigation properties to expand.</span></span> <span data-ttu-id="be431-135">下列要求會展開類別目錄和產品的供應商。</span><span class="sxs-lookup"><span data-stu-id="be431-135">The following request expands both the category and the supplier for a product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

<span data-ttu-id="be431-136">以下是回應主體：</span><span class="sxs-lookup"><span data-stu-id="be431-136">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

<span data-ttu-id="be431-137">您可以展開瀏覽屬性的多個層級。</span><span class="sxs-lookup"><span data-stu-id="be431-137">You can expand more than one level of navigation property.</span></span> <span data-ttu-id="be431-138">下列範例包含的類別目錄的所有產品以及每項產品的供應商。</span><span class="sxs-lookup"><span data-stu-id="be431-138">The following example includes all the products for a category and also the supplier for each product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

<span data-ttu-id="be431-139">以下是回應主體：</span><span class="sxs-lookup"><span data-stu-id="be431-139">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

<span data-ttu-id="be431-140">根據預設，Web 應用程式開發介面會限制為 2 的最大展開深度。</span><span class="sxs-lookup"><span data-stu-id="be431-140">By default, Web API limits the maximum expansion depth to 2.</span></span> <span data-ttu-id="be431-141">可避免用戶端傳送複雜的要求，例如`$expand=Orders/OrderDetails/Product/Supplier/Region`，這可能是沒有效率的查詢，並建立大型的回應。</span><span class="sxs-lookup"><span data-stu-id="be431-141">That prevents the client from sending complex requests like `$expand=Orders/OrderDetails/Product/Supplier/Region`, which might be inefficient to query and create large responses.</span></span> <span data-ttu-id="be431-142">若要覆寫預設值，設定**MaxExpansionDepth**屬性**[Queryable]**屬性。</span><span class="sxs-lookup"><span data-stu-id="be431-142">To override the default, set the **MaxExpansionDepth** property on the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

<span data-ttu-id="be431-143">如需有關 $expand 選項，請參閱 < [Expand 系統查詢選項 ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand)官方 OData 文件中。</span><span class="sxs-lookup"><span data-stu-id="be431-143">For more information about the $expand option, see [Expand System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) in the official OData documentation.</span></span>

## <a name="using-select"></a><span data-ttu-id="be431-144">使用 $select</span><span class="sxs-lookup"><span data-stu-id="be431-144">Using $select</span></span>

<span data-ttu-id="be431-145">$Select 選項指定要包含在回應主體中的屬性子集。</span><span class="sxs-lookup"><span data-stu-id="be431-145">The $select option specifies a subset of properties to include in the response body.</span></span> <span data-ttu-id="be431-146">例如，若要取得只有名稱和每個產品的價格，使用下列查詢：</span><span class="sxs-lookup"><span data-stu-id="be431-146">For example, to get only the name and price of each product, use the following query:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

<span data-ttu-id="be431-147">以下是回應主體：</span><span class="sxs-lookup"><span data-stu-id="be431-147">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

<span data-ttu-id="be431-148">您可以結合 $select 和 $expand 中相同的查詢。</span><span class="sxs-lookup"><span data-stu-id="be431-148">You can combine $select and $expand in the same query.</span></span> <span data-ttu-id="be431-149">請確定已展開的屬性納入 $select 選項。</span><span class="sxs-lookup"><span data-stu-id="be431-149">Make sure to include the expanded property in the $select option.</span></span> <span data-ttu-id="be431-150">例如，下列要求會取得供應商與產品名稱。</span><span class="sxs-lookup"><span data-stu-id="be431-150">For example, the following request gets the product name and supplier.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

<span data-ttu-id="be431-151">以下是回應主體：</span><span class="sxs-lookup"><span data-stu-id="be431-151">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

<span data-ttu-id="be431-152">您也可以選取中的已展開屬性的屬性。</span><span class="sxs-lookup"><span data-stu-id="be431-152">You can also select the properties within an expanded property.</span></span> <span data-ttu-id="be431-153">下列要求會展開產品，並選取類別目錄名稱加上產品名稱。</span><span class="sxs-lookup"><span data-stu-id="be431-153">The following request expands Products and selects category name plus product name.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

<span data-ttu-id="be431-154">以下是回應主體：</span><span class="sxs-lookup"><span data-stu-id="be431-154">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

<span data-ttu-id="be431-155">如需 $select 選項的詳細資訊，請參閱[選取系統查詢選項 ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select)官方 OData 文件中。</span><span class="sxs-lookup"><span data-stu-id="be431-155">For more information about the $select option, see [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) in the official OData documentation.</span></span>

## <a name="getting-individual-properties-of-an-entity-value"></a><span data-ttu-id="be431-156">取得實體 ($value) 的個別屬性</span><span class="sxs-lookup"><span data-stu-id="be431-156">Getting Individual Properties of an Entity ($value)</span></span>

<span data-ttu-id="be431-157">有兩個實體從取得的個別屬性的 OData 用戶端的方式。</span><span class="sxs-lookup"><span data-stu-id="be431-157">There are two ways for an OData client to get an individual property from an entity.</span></span> <span data-ttu-id="be431-158">用戶端可以取得 OData 格式的值，或取得屬性的原始值。</span><span class="sxs-lookup"><span data-stu-id="be431-158">The client can either get the value in OData format, or get the raw value of the property.</span></span>

<span data-ttu-id="be431-159">下列要求會取得 OData 格式的屬性。</span><span class="sxs-lookup"><span data-stu-id="be431-159">The following request gets a property in OData format.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

<span data-ttu-id="be431-160">以下是範例回應 JSON 格式：</span><span class="sxs-lookup"><span data-stu-id="be431-160">Here is an example response in JSON format:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

<span data-ttu-id="be431-161">若要取得屬性的原始值，請將 $value 附加至的 URI:</span><span class="sxs-lookup"><span data-stu-id="be431-161">To get the raw value of the property, append $value to the URI:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

<span data-ttu-id="be431-162">以下是回應。</span><span class="sxs-lookup"><span data-stu-id="be431-162">Here is the response.</span></span> <span data-ttu-id="be431-163">請注意，內容類型"text/plain"不是 JSON。</span><span class="sxs-lookup"><span data-stu-id="be431-163">Notice that the content type is "text/plain", not JSON.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

<span data-ttu-id="be431-164">若要支援這些查詢在 OData 控制器中，新增名為`GetProperty`，其中`Property`是屬性的名稱。</span><span class="sxs-lookup"><span data-stu-id="be431-164">To support these queries in your OData controller, add a method named `GetProperty`, where `Property` is the name of the property.</span></span> <span data-ttu-id="be431-165">例如，要取得的名稱屬性的方法就會命名為`GetName`。</span><span class="sxs-lookup"><span data-stu-id="be431-165">For example, the method to get the Name property would be named `GetName`.</span></span> <span data-ttu-id="be431-166">這個方法應傳回該屬性的值：</span><span class="sxs-lookup"><span data-stu-id="be431-166">The method should return the value of that property:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
