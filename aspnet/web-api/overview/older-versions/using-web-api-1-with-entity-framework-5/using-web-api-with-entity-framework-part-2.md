---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 第 2 部分： 建立網域模型 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 84631494c1be266c21e5e5702182df717b1d29b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="part-2-creating-the-domain-models"></a><span data-ttu-id="13f9c-102">第 2 部分： 建立網域模型</span><span class="sxs-lookup"><span data-stu-id="13f9c-102">Part 2: Creating the Domain Models</span></span>
====================
<span data-ttu-id="13f9c-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="13f9c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="13f9c-104">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="13f9c-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a><span data-ttu-id="13f9c-105">加入模型</span><span class="sxs-lookup"><span data-stu-id="13f9c-105">Add Models</span></span>

<span data-ttu-id="13f9c-106">有三種方法 Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="13f9c-106">There are three ways to approach Entity Framework:</span></span>

- <span data-ttu-id="13f9c-107">資料庫優先 （contract-first）： 使用資料庫時，開始和 Entity Framework 產生的程式碼。</span><span class="sxs-lookup"><span data-stu-id="13f9c-107">Database-first: You start with a database, and Entity Framework generates the code.</span></span>
- <span data-ttu-id="13f9c-108">模型優先： 開始您以視覺化的模型，與 Entity Framework 產生資料庫與程式碼。</span><span class="sxs-lookup"><span data-stu-id="13f9c-108">Model-first: You start with a visual model, and Entity Framework generates both the database and code.</span></span>
- <span data-ttu-id="13f9c-109">程式碼優先： 開始您以程式碼中，與 Entity Framework 產生資料庫。</span><span class="sxs-lookup"><span data-stu-id="13f9c-109">Code-first: You start with code, and Entity Framework generates the database.</span></span>

<span data-ttu-id="13f9c-110">我們使用的程式碼第一個方法，讓我們一開始定義為 POCOs （純舊 CLR 物件） 的網域物件。</span><span class="sxs-lookup"><span data-stu-id="13f9c-110">We are using the code-first approach, so we start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="13f9c-111">使用程式碼優先方法時，網域物件不需要任何額外的程式碼，以支援資料庫層級，例如交易或持續性。</span><span class="sxs-lookup"><span data-stu-id="13f9c-111">With the code-first approach, domain objects don't need any extra code to support the database layer, such as transactions or persistence.</span></span> <span data-ttu-id="13f9c-112">(具體而言，它們不需要繼承自[EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx)類別。)您仍然可以使用資料註解來控制 Entity Framework 建立的資料庫結構描述的方式。</span><span class="sxs-lookup"><span data-stu-id="13f9c-112">(Specifically, they do not need to inherit from the [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) class.) You can still use data annotations to control how Entity Framework creates the database schema.</span></span>

<span data-ttu-id="13f9c-113">因為 POCOs 不會包含任何額外的屬性描述[的資料庫狀態](https://msdn.microsoft.com/library/system.data.entitystate.aspx)，他們可以輕鬆地序列化為 JSON 或 XML。</span><span class="sxs-lookup"><span data-stu-id="13f9c-113">Because POCOs do not carry any extra properties that describe [database state](https://msdn.microsoft.com/library/system.data.entitystate.aspx), they can easily be serialized to JSON or XML.</span></span> <span data-ttu-id="13f9c-114">不過，這不表示您應該一律公開給用戶端，直接 Entity Framework 模型稍後在本教學課程中，我們會看到。</span><span class="sxs-lookup"><span data-stu-id="13f9c-114">However, that does not mean you should always expose your Entity Framework models directly to clients, as we'll see later in the tutorial.</span></span>

<span data-ttu-id="13f9c-115">我們將建立下列 POCOs:</span><span class="sxs-lookup"><span data-stu-id="13f9c-115">We will create the following POCOs:</span></span>

- <span data-ttu-id="13f9c-116">產品</span><span class="sxs-lookup"><span data-stu-id="13f9c-116">Product</span></span>
- <span data-ttu-id="13f9c-117">訂單</span><span class="sxs-lookup"><span data-stu-id="13f9c-117">Order</span></span>
- <span data-ttu-id="13f9c-118">OrderDetail</span><span class="sxs-lookup"><span data-stu-id="13f9c-118">OrderDetail</span></span>

<span data-ttu-id="13f9c-119">若要建立每個類別，以滑鼠右鍵按一下方案總管 中的 模型 資料夾。</span><span class="sxs-lookup"><span data-stu-id="13f9c-119">To create each class, right-click the Models folder in Solution Explorer.</span></span> <span data-ttu-id="13f9c-120">從內容功能表中，選取**新增**，然後選取 **類別。**</span><span class="sxs-lookup"><span data-stu-id="13f9c-120">From the context menu, select **Add** and then select **Class.**</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

<span data-ttu-id="13f9c-121">新增`Product`類別具有下列實作：</span><span class="sxs-lookup"><span data-stu-id="13f9c-121">Add a `Product` class with the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

<span data-ttu-id="13f9c-122">依照慣例，Entity Framework 會使用`Id`屬性做為主要金鑰並將它對應至資料庫資料表中識別資料行。</span><span class="sxs-lookup"><span data-stu-id="13f9c-122">By convention, Entity Framework uses the `Id` property as the primary key and maps it to an identity column in the database table.</span></span> <span data-ttu-id="13f9c-123">當您建立新`Product`執行個體，您將不會將值設`Id`，因為資料庫產生值。</span><span class="sxs-lookup"><span data-stu-id="13f9c-123">When you create a new `Product` instance, you won't set a value for `Id`, because the database generates the value.</span></span>

<span data-ttu-id="13f9c-124">**ScaffoldColumn**屬性會告知 ASP.NET MVC 略過`Id`產生編輯器表單時的屬性。</span><span class="sxs-lookup"><span data-stu-id="13f9c-124">The **ScaffoldColumn** attribute tells ASP.NET MVC to skip the `Id` property when generating an editor form.</span></span> <span data-ttu-id="13f9c-125">**需要**屬性用來驗證模型。</span><span class="sxs-lookup"><span data-stu-id="13f9c-125">The **Required** attribute is used to validate the model.</span></span> <span data-ttu-id="13f9c-126">它會指定`Name`屬性必須是非空白字串。</span><span class="sxs-lookup"><span data-stu-id="13f9c-126">It specifies that the `Name` property must be a non-empty string.</span></span>

<span data-ttu-id="13f9c-127">新增`Order`類別：</span><span class="sxs-lookup"><span data-stu-id="13f9c-127">Add the `Order` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

<span data-ttu-id="13f9c-128">新增`OrderDetail`類別：</span><span class="sxs-lookup"><span data-stu-id="13f9c-128">Add the `OrderDetail` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a><span data-ttu-id="13f9c-129">外部索引鍵關聯</span><span class="sxs-lookup"><span data-stu-id="13f9c-129">Foreign Key Relations</span></span>

<span data-ttu-id="13f9c-130">訂單包含許多訂單詳細資料，以及每個訂單詳細資料是指單一產品。</span><span class="sxs-lookup"><span data-stu-id="13f9c-130">An order contains many order details, and each order detail refers to a single product.</span></span> <span data-ttu-id="13f9c-131">若要代表這些關聯性，`OrderDetail`類別會定義名為屬性`OrderId`和`ProductId`。</span><span class="sxs-lookup"><span data-stu-id="13f9c-131">To represent these relations, the `OrderDetail` class defines properties named `OrderId` and `ProductId`.</span></span> <span data-ttu-id="13f9c-132">Entity Framework 會推斷這些屬性代表外部索引鍵，而且會將外部索引鍵條件約束加入至資料庫。</span><span class="sxs-lookup"><span data-stu-id="13f9c-132">Entity Framework will infer that these properties represent foreign keys, and will add foreign-key constraints to the database.</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

<span data-ttu-id="13f9c-133">`Order`和`OrderDetail`類別也會包含 「 瀏覽 」 屬性，其中包含相關物件的參考。</span><span class="sxs-lookup"><span data-stu-id="13f9c-133">The `Order` and `OrderDetail` classes also include "navigation" properties, which contain references to the related objects.</span></span> <span data-ttu-id="13f9c-134">指定的順序，您可以瀏覽至訂單中產品所遵循的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="13f9c-134">Given an order, you can navigate to the products in the order by following the navigation properties.</span></span>

<span data-ttu-id="13f9c-135">現在編譯專案。</span><span class="sxs-lookup"><span data-stu-id="13f9c-135">Compile the project now.</span></span> <span data-ttu-id="13f9c-136">Entity Framework 會使用反映來探索模型的屬性，因此需要建立資料庫結構描述編譯的組件。</span><span class="sxs-lookup"><span data-stu-id="13f9c-136">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

## <a name="configure-the-media-type-formatters"></a><span data-ttu-id="13f9c-137">設定媒體類型格式器</span><span class="sxs-lookup"><span data-stu-id="13f9c-137">Configure the Media-Type Formatters</span></span>

<span data-ttu-id="13f9c-138">A[媒體類型格式器](../../formats-and-model-binding/media-formatters.md)是將資料序列化時 Web API 寫入 HTTP 回應主體的物件。</span><span class="sxs-lookup"><span data-stu-id="13f9c-138">A [media-type formatter](../../formats-and-model-binding/media-formatters.md) is an object that serializes your data when Web API writes the HTTP response body.</span></span> <span data-ttu-id="13f9c-139">內建的格式器支援 JSON 和 XML 輸出。</span><span class="sxs-lookup"><span data-stu-id="13f9c-139">The built-in formatters support JSON and XML output.</span></span> <span data-ttu-id="13f9c-140">根據預設，這兩個這些格式器序列化值的所有物件。</span><span class="sxs-lookup"><span data-stu-id="13f9c-140">By default, both of these formatters serialize all objects by value.</span></span>

<span data-ttu-id="13f9c-141">值序列化會有問題，如果物件圖形包含循環參考。</span><span class="sxs-lookup"><span data-stu-id="13f9c-141">Serialization by value creates a problem if an object graph contains circular references.</span></span> <span data-ttu-id="13f9c-142">完全與如此`Order`和`OrderDetail`類別，因為每個持有另的參考。</span><span class="sxs-lookup"><span data-stu-id="13f9c-142">That's exactly the case with the `Order` and `OrderDetail` classes, because each holds a reference to the other.</span></span> <span data-ttu-id="13f9c-143">格式器會遵循撰寫的值，每個物件參考，並移圓形中。</span><span class="sxs-lookup"><span data-stu-id="13f9c-143">The formatter will follow the references, writing each object by value, and go in circles.</span></span> <span data-ttu-id="13f9c-144">因此，我們需要變更預設行為。</span><span class="sxs-lookup"><span data-stu-id="13f9c-144">Therefore, we need to change the default behavior.</span></span>

<span data-ttu-id="13f9c-145">在 方案總管 中，展開 應用程式\_啟動資料夾，然後開啟名為 WebApiConfig.cs 的檔案。</span><span class="sxs-lookup"><span data-stu-id="13f9c-145">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="13f9c-146">將下列程式碼加入 `WebApiConfig` 類別：</span><span class="sxs-lookup"><span data-stu-id="13f9c-146">Add the following code to the `WebApiConfig` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

<span data-ttu-id="13f9c-147">此程式碼設定 JSON 格式器，來保留物件參考，並完全移除管線的 XML 格式器。</span><span class="sxs-lookup"><span data-stu-id="13f9c-147">This code sets the JSON formatter to preserve object references, and removes the XML formatter from the pipeline entirely.</span></span> <span data-ttu-id="13f9c-148">（您可以設定 XML 格式器，來保留物件參考，但更多的工作，我們只會為此應用程式需要 JSON。</span><span class="sxs-lookup"><span data-stu-id="13f9c-148">(You can configure the XML formatter to preserve object references, but it's a little more work, and we only need JSON for this application.</span></span> <span data-ttu-id="13f9c-149">如需詳細資訊，請參閱[處理循環的物件參考](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)。)</span><span class="sxs-lookup"><span data-stu-id="13f9c-149">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="13f9c-150">[上一頁](using-web-api-with-entity-framework-part-1.md)
> [下一頁](using-web-api-with-entity-framework-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="13f9c-150">[Previous](using-web-api-with-entity-framework-part-1.md)
[Next](using-web-api-with-entity-framework-part-3.md)</span></span>
