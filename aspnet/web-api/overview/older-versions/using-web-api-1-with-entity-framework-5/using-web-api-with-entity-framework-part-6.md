---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: "第 6 單元： 建立產品和順序控制站 |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: ef7674476e0db334642daa29e352f615135b07ab
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="part-6-creating-product-and-order-controllers"></a><span data-ttu-id="446e3-102">第 6 單元： 建立產品和順序控制站</span><span class="sxs-lookup"><span data-stu-id="446e3-102">Part 6: Creating Product and Order Controllers</span></span>
====================
<span data-ttu-id="446e3-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="446e3-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="446e3-104">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="446e3-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a><span data-ttu-id="446e3-105">新增產品控制站</span><span class="sxs-lookup"><span data-stu-id="446e3-105">Add a Products Controller</span></span>

<span data-ttu-id="446e3-106">系統管理控制站是有系統管理員權限使用者。</span><span class="sxs-lookup"><span data-stu-id="446e3-106">The Admin controller is for users who have administrator privileges.</span></span> <span data-ttu-id="446e3-107">客戶，相反地，可以檢視產品，但無法建立、 更新或刪除它們。</span><span class="sxs-lookup"><span data-stu-id="446e3-107">Customers, on the other hand, can view products but cannot create, update, or delete them.</span></span>

<span data-ttu-id="446e3-108">我們可以輕鬆地限制存取權的 Post、 Put 和 Delete 方法，同時保留 Get 方法。</span><span class="sxs-lookup"><span data-stu-id="446e3-108">We can easily restrict access to the Post, Put, and Delete methods, while leaving the Get methods open.</span></span> <span data-ttu-id="446e3-109">看傳回產品的資料：</span><span class="sxs-lookup"><span data-stu-id="446e3-109">But look at the data that is returned for a product:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

<span data-ttu-id="446e3-110">`ActualCost`屬性不應該為可見的客戶 ！</span><span class="sxs-lookup"><span data-stu-id="446e3-110">The `ActualCost` property should not be visible to customers!</span></span> <span data-ttu-id="446e3-111">解決方法是定義*資料傳輸物件*(DTO)，其中包含應該顯示給客戶的屬性子集。</span><span class="sxs-lookup"><span data-stu-id="446e3-111">The solution is to define a *data transfer object* (DTO) that includes a subset of properties that should be visible to customers.</span></span> <span data-ttu-id="446e3-112">我們將使用 LINQ 專案`Product`執行個體來`ProductDTO`執行個體。</span><span class="sxs-lookup"><span data-stu-id="446e3-112">We will use LINQ to project `Product` instances to `ProductDTO` instances.</span></span>

<span data-ttu-id="446e3-113">將類別命名為`ProductDTO`到 Models 資料夾。</span><span class="sxs-lookup"><span data-stu-id="446e3-113">Add a class named `ProductDTO` to the Models folder.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

<span data-ttu-id="446e3-114">現在加入控制器。</span><span class="sxs-lookup"><span data-stu-id="446e3-114">Now add the controller.</span></span> <span data-ttu-id="446e3-115">在 [方案總管] 中，以滑鼠右鍵按一下 [控制器] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="446e3-115">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="446e3-116">選取**新增**，然後選取**控制器**。</span><span class="sxs-lookup"><span data-stu-id="446e3-116">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="446e3-117">在**加入控制器**對話方塊中，控制器&quot;ProductsController&quot;。</span><span class="sxs-lookup"><span data-stu-id="446e3-117">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="446e3-118">在下**範本**，選取**空白的 API 控制器**。</span><span class="sxs-lookup"><span data-stu-id="446e3-118">Under **Template**, select **Empty API controller**.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

<span data-ttu-id="446e3-119">取代為下列程式碼的原始程式檔中的所有項目：</span><span class="sxs-lookup"><span data-stu-id="446e3-119">Replace everything in the source file with the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

<span data-ttu-id="446e3-120">控制器仍會使用`OrdersContext`查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="446e3-120">The controller still uses the `OrdersContext` to query the database.</span></span> <span data-ttu-id="446e3-121">但不傳回`Product`執行個體直接，我們稱之為`MapProducts`投影至其`ProductDTO`執行個體：</span><span class="sxs-lookup"><span data-stu-id="446e3-121">But instead of returning `Product` instances directly, we call `MapProducts` to project them onto `ProductDTO` instances:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

<span data-ttu-id="446e3-122">`MapProducts`方法會傳回**IQueryable**，因此我們可以撰寫其他查詢參數的結果。</span><span class="sxs-lookup"><span data-stu-id="446e3-122">The `MapProducts` method returns an **IQueryable**, so we can compose the result with other query parameters.</span></span> <span data-ttu-id="446e3-123">您可以看到此中`GetProduct`方法，將**其中**子句加入查詢：</span><span class="sxs-lookup"><span data-stu-id="446e3-123">You can see this in the `GetProduct` method, which adds a **where** clause to the query:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a><span data-ttu-id="446e3-124">新增訂單控制站</span><span class="sxs-lookup"><span data-stu-id="446e3-124">Add an Orders Controller</span></span>

<span data-ttu-id="446e3-125">接下來，加入可讓使用者建立和檢視訂單的控制站。</span><span class="sxs-lookup"><span data-stu-id="446e3-125">Next, add a controller that lets users create and view orders.</span></span>

<span data-ttu-id="446e3-126">我們將開始另一個 DTO。</span><span class="sxs-lookup"><span data-stu-id="446e3-126">We'll start with another DTO.</span></span> <span data-ttu-id="446e3-127">在方案總管 中，以滑鼠右鍵按一下 模型 資料夾並加入類別，名為`OrderDTO`使用下列實作：</span><span class="sxs-lookup"><span data-stu-id="446e3-127">In Solution Explorer, right-click the Models folder and add a class named `OrderDTO` Use the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

<span data-ttu-id="446e3-128">現在加入控制器。</span><span class="sxs-lookup"><span data-stu-id="446e3-128">Now add the controller.</span></span> <span data-ttu-id="446e3-129">在 [方案總管] 中，以滑鼠右鍵按一下 [控制器] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="446e3-129">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="446e3-130">選取**新增**，然後選取**控制器**。</span><span class="sxs-lookup"><span data-stu-id="446e3-130">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="446e3-131">在**加入控制器** 對話方塊中，設定下列選項：</span><span class="sxs-lookup"><span data-stu-id="446e3-131">In the **Add Controller** dialog, set the following options:</span></span>

- <span data-ttu-id="446e3-132">在下**控制器名稱**，輸入 「 OrdersController"。</span><span class="sxs-lookup"><span data-stu-id="446e3-132">Under **Controller Name**, enter "OrdersController".</span></span>
- <span data-ttu-id="446e3-133">在下**範本**，選取"使用 Entity Framework 的讀取/寫入動作的 API 控制器 」。</span><span class="sxs-lookup"><span data-stu-id="446e3-133">Under **Template**, select "API controller with read/write actions, using Entity Framework".</span></span>
- <span data-ttu-id="446e3-134">在下**模型類別**，選取&quot;順序 (ProductStore.Models)&quot;。</span><span class="sxs-lookup"><span data-stu-id="446e3-134">Under **Model class**, select &quot;Order (ProductStore.Models)&quot;.</span></span>
- <span data-ttu-id="446e3-135">在下**資料內容類別**，選取&quot;OrdersContext (ProductStore.Models)&quot;。</span><span class="sxs-lookup"><span data-stu-id="446e3-135">Under **Data context class**, select &quot;OrdersContext (ProductStore.Models)&quot;.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

<span data-ttu-id="446e3-136">按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="446e3-136">Click **Add**.</span></span> <span data-ttu-id="446e3-137">這樣會加入名為 OrdersController.cs 的檔案。</span><span class="sxs-lookup"><span data-stu-id="446e3-137">This adds a file named OrdersController.cs.</span></span> <span data-ttu-id="446e3-138">接下來，我們必須修改控制器的預設實作。</span><span class="sxs-lookup"><span data-stu-id="446e3-138">Next, we need to modify the default implementation of the controller.</span></span>

<span data-ttu-id="446e3-139">首先，刪除`PutOrder`和`DeleteOrder`方法。</span><span class="sxs-lookup"><span data-stu-id="446e3-139">First, delete the `PutOrder` and `DeleteOrder` methods.</span></span> <span data-ttu-id="446e3-140">此範例中，客戶無法修改或刪除現有的訂單。</span><span class="sxs-lookup"><span data-stu-id="446e3-140">For this sample, customers cannot modify or delete existing orders.</span></span> <span data-ttu-id="446e3-141">在實際應用中，您將需要許多後端邏輯來處理這些情況。</span><span class="sxs-lookup"><span data-stu-id="446e3-141">In a real application, you would need lots of back-end logic to handle these cases.</span></span> <span data-ttu-id="446e3-142">（例如，已訂單已出貨？）</span><span class="sxs-lookup"><span data-stu-id="446e3-142">(For example, was the order already shipped?)</span></span>

<span data-ttu-id="446e3-143">變更`GetOrders`方法來傳回使用者所屬的訂單：</span><span class="sxs-lookup"><span data-stu-id="446e3-143">Change the `GetOrders` method to return just the orders that belong to the user:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

<span data-ttu-id="446e3-144">變更`GetOrder`方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="446e3-144">Change the `GetOrder` method as follows:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

<span data-ttu-id="446e3-145">以下是我們對方法的變更：</span><span class="sxs-lookup"><span data-stu-id="446e3-145">Here are the changes that we made to the method:</span></span>

- <span data-ttu-id="446e3-146">傳回值是`OrderDTO`執行個體，而不是`Order`。</span><span class="sxs-lookup"><span data-stu-id="446e3-146">The return value is an `OrderDTO` instance, instead of an `Order`.</span></span>
- <span data-ttu-id="446e3-147">當我們查詢資料庫中的訂單時，我們使用[DbQuery.Include](https://msdn.microsoft.com/en-us/library/gg696395)方法來提取相關`OrderDetail`和`Product`實體。</span><span class="sxs-lookup"><span data-stu-id="446e3-147">When we query the database for the order, we use the [DbQuery.Include](https://msdn.microsoft.com/en-us/library/gg696395) method to fetch the related `OrderDetail` and `Product` entities.</span></span>
- <span data-ttu-id="446e3-148">我們使用投影來扁平化結果。</span><span class="sxs-lookup"><span data-stu-id="446e3-148">We flatten the result by using a projection.</span></span>

<span data-ttu-id="446e3-149">HTTP 回應將包含產品與數量的陣列：</span><span class="sxs-lookup"><span data-stu-id="446e3-149">The HTTP response will contain an array of products with quantities:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

<span data-ttu-id="446e3-150">此格式為便於用戶端使用比原來的物件圖形包含巢狀的實體 （順序、 詳細資料，以及產品）。</span><span class="sxs-lookup"><span data-stu-id="446e3-150">This format is easier for clients to consume than the original object graph, which contains nested entities (order, details, and products).</span></span>

<span data-ttu-id="446e3-151">考慮到它的最後一個方法`PostOrder`。</span><span class="sxs-lookup"><span data-stu-id="446e3-151">The last method to consider it `PostOrder`.</span></span> <span data-ttu-id="446e3-152">現在，這個方法會採用`Order`執行個體。</span><span class="sxs-lookup"><span data-stu-id="446e3-152">Right now, this method takes an `Order` instance.</span></span> <span data-ttu-id="446e3-153">會發生什麼情況，但如果用戶端傳送的要求內文，像這樣：</span><span class="sxs-lookup"><span data-stu-id="446e3-153">But consider what happens if a client sends a request body like this:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

<span data-ttu-id="446e3-154">這是結構完善的順序，和 Entity Framework 將愉快地保存他們將其插入到資料庫。</span><span class="sxs-lookup"><span data-stu-id="446e3-154">This is a well-structured order, and Entity Framework will happily insert it into the database.</span></span> <span data-ttu-id="446e3-155">但它包含之前不存在的產品實體。</span><span class="sxs-lookup"><span data-stu-id="446e3-155">But it contains a Product entity that did not exist previously.</span></span> <span data-ttu-id="446e3-156">用戶端，只在資料庫中建立新的產品 ！</span><span class="sxs-lookup"><span data-stu-id="446e3-156">The client just created a new product in our database!</span></span> <span data-ttu-id="446e3-157">當他們看到 koala 引起的順序時，這會是順序 fullfilment 部門的意外。</span><span class="sxs-lookup"><span data-stu-id="446e3-157">This will be a suprise to the order fullfilment department, when they see an order for koala bears.</span></span> <span data-ttu-id="446e3-158">重點是，請小心實際上您接受 POST 或 PUT 要求中的資料。</span><span class="sxs-lookup"><span data-stu-id="446e3-158">The moral is, be really careful about the data you accept in a POST or PUT request.</span></span>

<span data-ttu-id="446e3-159">若要避免這個問題，變更`PostOrder`方法以讓`OrderDTO`執行個體。</span><span class="sxs-lookup"><span data-stu-id="446e3-159">To avoid this problem, change the `PostOrder` method to take an `OrderDTO` instance.</span></span> <span data-ttu-id="446e3-160">使用`OrderDTO`建立`Order`。</span><span class="sxs-lookup"><span data-stu-id="446e3-160">Use the `OrderDTO` to create the `Order`.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

<span data-ttu-id="446e3-161">請注意，我們使用`ProductID`和`Quantity`屬性，以及我們忽略任何用戶端傳送的產品名稱或價格的值。</span><span class="sxs-lookup"><span data-stu-id="446e3-161">Notice that we use the `ProductID` and `Quantity` properties, and we ignore any values that the client sent for either product name or price.</span></span> <span data-ttu-id="446e3-162">如果產品識別碼不是有效的它將違反外部索引鍵條件約束，在資料庫中，並插入將會失敗，因為它應該。</span><span class="sxs-lookup"><span data-stu-id="446e3-162">If the product ID is not valid, it will violate the foreign key constraint in the database, and the insert will fail, as it should.</span></span>

<span data-ttu-id="446e3-163">以下是完整`PostOrder`方法：</span><span class="sxs-lookup"><span data-stu-id="446e3-163">Here is the complete `PostOrder` method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

<span data-ttu-id="446e3-164">最後，會加入**授權**屬性控制站：</span><span class="sxs-lookup"><span data-stu-id="446e3-164">Finally, add the **Authorize** attribute to the controller:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

<span data-ttu-id="446e3-165">現在已註冊的使用者可以建立或檢視訂單。</span><span class="sxs-lookup"><span data-stu-id="446e3-165">Now only registered users can create or view orders.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="446e3-166">[上一頁](using-web-api-with-entity-framework-part-5.md)
[下一頁](using-web-api-with-entity-framework-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="446e3-166">[Previous](using-web-api-with-entity-framework-part-5.md)
[Next](using-web-api-with-entity-framework-part-7.md)</span></span>
