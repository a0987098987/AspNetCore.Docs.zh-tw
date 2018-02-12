---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: "在使用 ASP.NET Web API OData v4 開啟類型 |Microsoft 文件"
author: microsoft
description: "在 OData v4、 開啟類型會是 stuctured 型別包含動態內容，以及任何型別定義中宣告的屬性。 開啟..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2014
ms.topic: article
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: fe67b9a11a82b55d5f3e0e5f1b0cee10a58833d2
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2018
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="fe0fb-104">在使用 ASP.NET Web API OData v4 開啟類型</span><span class="sxs-lookup"><span data-stu-id="fe0fb-104">Open Types in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="fe0fb-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="fe0fb-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="fe0fb-106">在 OData v4*開啟輸入*stuctured 型別，其中包含動態內容，以及任何型別定義中宣告的屬性。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-106">In OData v4, an *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="fe0fb-107">開放式類型可讓您加入的彈性資料模型。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-107">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="fe0fb-108">本教學課程會示範如何使用 ASP.NET Web API OData 中開啟的型別。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-108">This tutorial shows how to use open types in ASP.NET Web API OData.</span></span>
> 
> <span data-ttu-id="fe0fb-109">本教學課程假設您已經知道如何建立 ASP.NET Web API 中的 OData 端點。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-109">This tutorial assumes that you already know how to create an OData endpoint in ASP.NET Web API.</span></span> <span data-ttu-id="fe0fb-110">如果沒有，請先閱讀[建立 OData v4 端點](create-an-odata-v4-endpoint.md)第一次。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-110">If not, start by reading [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md) first.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="fe0fb-111">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="fe0fb-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="fe0fb-112">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="fe0fb-112">Web API OData 5.3</span></span>
> - <span data-ttu-id="fe0fb-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="fe0fb-113">OData v4</span></span>


<span data-ttu-id="fe0fb-114">首先，一些 OData 術語：</span><span class="sxs-lookup"><span data-stu-id="fe0fb-114">First, some OData terminology:</span></span>

- <span data-ttu-id="fe0fb-115">實體類型： 索引鍵的結構化型別。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-115">Entity type: A structured type with a key.</span></span>
- <span data-ttu-id="fe0fb-116">複雜型別： 結構化的型別沒有索引鍵。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-116">Complex type: A structured type without a key.</span></span>
- <span data-ttu-id="fe0fb-117">開啟型別： 具有動態屬性的型別。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-117">Open type: A type with dynamic properties.</span></span> <span data-ttu-id="fe0fb-118">實體類型和複雜類型可以是開啟的。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-118">Both entity types and complex types can be open.</span></span>

<span data-ttu-id="fe0fb-119">動態屬性的值可以是基本類型、 複雜型別或列舉型別。或任何一種類型的集合。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-119">The value of a dynamic property can be a primitive type, complex type, or enumeration type; or a collection of any of those types.</span></span> <span data-ttu-id="fe0fb-120">如需開啟類型的詳細資訊，請參閱[OData v4 規格](http://www.odata.org/documentation/odata-version-4-0/)。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-120">For more information about open types, see the [OData v4 specification](http://www.odata.org/documentation/odata-version-4-0/).</span></span>

## <a name="install-the-web-odata-libraries"></a><span data-ttu-id="fe0fb-121">安裝 Web OData 程式庫</span><span class="sxs-lookup"><span data-stu-id="fe0fb-121">Install the Web OData Libraries</span></span>

<span data-ttu-id="fe0fb-122">使用 NuGet 封裝管理員安裝最新的 Web API OData 程式庫。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-122">Use NuGet Package Manager to install the latest Web API OData libraries.</span></span> <span data-ttu-id="fe0fb-123">從 [封裝管理員主控台] 視窗中：</span><span class="sxs-lookup"><span data-stu-id="fe0fb-123">From the Package Manager Console window:</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a><span data-ttu-id="fe0fb-124">定義 CLR 類型</span><span class="sxs-lookup"><span data-stu-id="fe0fb-124">Define the CLR Types</span></span>

<span data-ttu-id="fe0fb-125">會定義成 CLR 類型的 EDM 模型開始。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-125">Start by defining the EDM models as CLR types.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="fe0fb-126">建立實體資料模型 (EDM) 時，</span><span class="sxs-lookup"><span data-stu-id="fe0fb-126">When the Entity Data Model (EDM) is created,</span></span>

- <span data-ttu-id="fe0fb-127">`Category`是列舉類型。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-127">`Category` is an enumeration type.</span></span>
- <span data-ttu-id="fe0fb-128">`Address`是複雜類型。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-128">`Address` is a complex type.</span></span> <span data-ttu-id="fe0fb-129">（它並沒有索引鍵，因此它不是實體類型。）</span><span class="sxs-lookup"><span data-stu-id="fe0fb-129">(It does not have a key, so it is not an entity type.)</span></span>
- <span data-ttu-id="fe0fb-130">`Customer`為實體類型。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-130">`Customer` is an entity type.</span></span> <span data-ttu-id="fe0fb-131">（它有索引鍵）。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-131">(It has a key.)</span></span>
- <span data-ttu-id="fe0fb-132">`Press`是開啟的複雜類型。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-132">`Press` is an open complex type.</span></span>
- <span data-ttu-id="fe0fb-133">`Book`這是開啟的實體類型。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-133">`Book` is an open entity type.</span></span>

<span data-ttu-id="fe0fb-134">若要建立開啟型別，CLR 型別必須具有型別的屬性`IDictionary<string, object>`，其中保留的動態內容。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-134">To create an open type, the CLR type must have a property of type `IDictionary<string, object>`, which holds the dynamic properties.</span></span>

## <a name="build-the-edm-model"></a><span data-ttu-id="fe0fb-135">建置的 EDM 模型</span><span class="sxs-lookup"><span data-stu-id="fe0fb-135">Build the EDM Model</span></span>

<span data-ttu-id="fe0fb-136">如果您使用**ODataConventionModelBuilder**建立 EDM`Press`和`Book`開啟的目前狀態為基礎的類型會自動新增`IDictionary<string, object>`屬性。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-136">If you use **ODataConventionModelBuilder** to create the EDM, `Press` and `Book` are automatically added as open types, based on the presence of a `IDictionary<string, object>` property.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="fe0fb-137">您也可以建置 EDM 明確地使用**ODataModelBuilder**。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-137">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a><span data-ttu-id="fe0fb-138">加入 OData 控制器</span><span class="sxs-lookup"><span data-stu-id="fe0fb-138">Add an OData Controller</span></span>

<span data-ttu-id="fe0fb-139">接下來，加入 OData 控制器。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-139">Next, add an OData controller.</span></span> <span data-ttu-id="fe0fb-140">此教學課程中，我們會使用簡化的控制器，只支援 GET 和 POST 要求，並使用記憶體中清單來儲存實體。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-140">For this tutorial, we'll use a simplified controller that just supports GET and POST requests, and uses an in-memory list to store entities.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="fe0fb-141">請注意，第一個`Book`執行個體沒有任何動態屬性。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-141">Notice that the first `Book` instance has no dynamic properties.</span></span> <span data-ttu-id="fe0fb-142">第二個`Book`執行個體都有下列的動態屬性：</span><span class="sxs-lookup"><span data-stu-id="fe0fb-142">The second `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="fe0fb-143">「 發行 」: 基本型別</span><span class="sxs-lookup"><span data-stu-id="fe0fb-143">"Published": Primitive type</span></span>
- <span data-ttu-id="fe0fb-144">「 著作人 」: 基本型別的集合</span><span class="sxs-lookup"><span data-stu-id="fe0fb-144">"Authors": Collection of primitive types</span></span>
- <span data-ttu-id="fe0fb-145">「 OtherCategories": 列舉型別的集合。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-145">"OtherCategories": Collection of enumeration types.</span></span>

<span data-ttu-id="fe0fb-146">此外，`Press`屬性，`Book`執行個體都有下列的動態屬性：</span><span class="sxs-lookup"><span data-stu-id="fe0fb-146">Also, the `Press` property of that `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="fe0fb-147">"部落格": 基本型別</span><span class="sxs-lookup"><span data-stu-id="fe0fb-147">"Blog": Primitive type</span></span>
- <span data-ttu-id="fe0fb-148">「 位址 」: 複雜型別</span><span class="sxs-lookup"><span data-stu-id="fe0fb-148">"Address": Complex type</span></span>

## <a name="query-the-metadata"></a><span data-ttu-id="fe0fb-149">查詢的中繼資料</span><span class="sxs-lookup"><span data-stu-id="fe0fb-149">Query the Metadata</span></span>

<span data-ttu-id="fe0fb-150">若要取得 OData 中繼資料文件，傳送 GET 要求來`~/$metadata`。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-150">To get the OData metadata document, send a GET request to `~/$metadata`.</span></span> <span data-ttu-id="fe0fb-151">回應主體看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="fe0fb-151">The response body should look similar to this:</span></span>

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

<span data-ttu-id="fe0fb-152">中繼資料文件中，您可以看到：</span><span class="sxs-lookup"><span data-stu-id="fe0fb-152">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="fe0fb-153">如`Book`和`Press`類型、 值`OpenType`屬性為 true。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-153">For the `Book` and `Press` types, the value of the `OpenType` attribute is true.</span></span> <span data-ttu-id="fe0fb-154">`Customer`和`Address`類型沒有這個屬性。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-154">The `Customer` and `Address` types don't have this attribute.</span></span>
- <span data-ttu-id="fe0fb-155">`Book`實體類型有三個宣告的屬性： ISBN、 標題和按。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-155">The `Book` entity type has three declared properties: ISBN, Title, and Press.</span></span> <span data-ttu-id="fe0fb-156">OData 中繼資料不包含`Book.Properties`從 CLR 類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-156">The OData metadata does not include the `Book.Properties` property from the CLR class.</span></span>
- <span data-ttu-id="fe0fb-157">同樣地，`Press`複雜型別具有只有兩個宣告的屬性： 名稱和類別目錄。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-157">Similarly, the `Press` complex type has only two declared properties: Name and Category.</span></span> <span data-ttu-id="fe0fb-158">中繼資料不包含`Press.DynamicProperties`從 CLR 類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-158">The metadata does not include the `Press.DynamicProperties` property from the CLR class.</span></span>

## <a name="query-an-entity"></a><span data-ttu-id="fe0fb-159">查詢實體</span><span class="sxs-lookup"><span data-stu-id="fe0fb-159">Query an Entity</span></span>

<span data-ttu-id="fe0fb-160">若要取得 ISBN 的書本等於"978-0-7356-7942-9"，傳送 GET 要求來`~/Books('978-0-7356-7942-9')`。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-160">To get the book with ISBN equal to "978-0-7356-7942-9", send a GET request to `~/Books('978-0-7356-7942-9')`.</span></span> <span data-ttu-id="fe0fb-161">回應主體看起來應該如下所示。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-161">The response body should look similar to the following.</span></span> <span data-ttu-id="fe0fb-162">（縮排，讓它更容易閱讀。）</span><span class="sxs-lookup"><span data-stu-id="fe0fb-162">(Indented to make it more readable.)</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

<span data-ttu-id="fe0fb-163">請注意，動態屬性的宣告屬性以內嵌。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-163">Notice that the dynamic properties are included inline with the declared properties.</span></span>

## <a name="post-an-entity"></a><span data-ttu-id="fe0fb-164">POST 實體</span><span class="sxs-lookup"><span data-stu-id="fe0fb-164">POST an Entity</span></span>

<span data-ttu-id="fe0fb-165">若要加入的書籍實體，傳送 POST 要求到`~/Books`。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-165">To add a Book entity, send a POST request to `~/Books`.</span></span> <span data-ttu-id="fe0fb-166">用戶端可以要求裝載中設定動態屬性。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-166">The client can set dynamic properties in the request payload.</span></span>

<span data-ttu-id="fe0fb-167">以下是範例要求。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-167">Here is an example request.</span></span> <span data-ttu-id="fe0fb-168">請注意 「 價格 」 和 「 已發佈 」 屬性。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-168">Note the "Price" and "Published" properties.</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

<span data-ttu-id="fe0fb-169">如果您在控制器方法中設定中斷點，您可以看到 Web API 加入這些屬性，以`Properties`字典。</span><span class="sxs-lookup"><span data-stu-id="fe0fb-169">If you set a breakpoint in the controller method, you can see that Web API added these properties to the `Properties` dictionary.</span></span>

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="fe0fb-170">其他資源</span><span class="sxs-lookup"><span data-stu-id="fe0fb-170">Additional Resources</span></span>

[<span data-ttu-id="fe0fb-171">OData 開啟類型範例</span><span class="sxs-lookup"><span data-stu-id="fe0fb-171">OData Open Type Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
