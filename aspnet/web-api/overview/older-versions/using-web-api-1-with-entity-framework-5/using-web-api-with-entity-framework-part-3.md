---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 第 3 部分： 建立管理員控制器 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 7ad0ec27021514b447e569e479a9e9127e3f75fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832975"
---
<a name="part-3-creating-an-admin-controller"></a><span data-ttu-id="da902-102">第 3 部分： 建立管理員控制器</span><span class="sxs-lookup"><span data-stu-id="da902-102">Part 3: Creating an Admin Controller</span></span>
====================
<span data-ttu-id="da902-103">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="da902-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="da902-104">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="da902-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a><span data-ttu-id="da902-105">新增系統管理員控制站</span><span class="sxs-lookup"><span data-stu-id="da902-105">Add an Admin Controller</span></span>

<span data-ttu-id="da902-106">在本節中，我們會將新增的 Web API 控制器支援 CRUD （建立、 讀取、 更新和刪除） 產品上的作業。</span><span class="sxs-lookup"><span data-stu-id="da902-106">In this section, we'll add a Web API controller that supports CRUD (create, read, update, and delete) operations on products.</span></span> <span data-ttu-id="da902-107">控制器會使用 Entity Framework 與資料庫層級進行通訊。</span><span class="sxs-lookup"><span data-stu-id="da902-107">The controller will use Entity Framework to communicate with the database layer.</span></span> <span data-ttu-id="da902-108">只有系統管理員可以使用此控制站。</span><span class="sxs-lookup"><span data-stu-id="da902-108">Only administrators will be able to use this controller.</span></span> <span data-ttu-id="da902-109">客戶會透過另一個控制站存取產品。</span><span class="sxs-lookup"><span data-stu-id="da902-109">Customers will access the products through another controller.</span></span>

<span data-ttu-id="da902-110">在 [方案總管] 中，以滑鼠右鍵按一下 [控制器] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="da902-110">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="da902-111">選取 **新增**，然後**控制站**。</span><span class="sxs-lookup"><span data-stu-id="da902-111">Select **Add** and then **Controller**.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

<span data-ttu-id="da902-112">在 [**新增控制器**] 對話方塊中，將控制器命名`AdminController`。</span><span class="sxs-lookup"><span data-stu-id="da902-112">In the **Add Controller** dialog, name the controller `AdminController`.</span></span> <span data-ttu-id="da902-113">底下**樣板**，選取&quot;讀取/寫入動作，使用 Entity Framework 的 API 控制器&quot;。</span><span class="sxs-lookup"><span data-stu-id="da902-113">Under **Template**, select &quot;API controller with read/write actions, using Entity Framework&quot;.</span></span> <span data-ttu-id="da902-114">底下**模型類別**，選取 [產品 (ProductStore.Models)]。</span><span class="sxs-lookup"><span data-stu-id="da902-114">Under **Model class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="da902-115">底下**資料內容**，選取 「&lt;新的資料內容&gt;"。</span><span class="sxs-lookup"><span data-stu-id="da902-115">Under **Data Context**, select "&lt;New Data Context&gt;".</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="da902-116">如果**模型類別**下拉式清單不會顯示任何模型類別，請確定您編譯專案。</span><span class="sxs-lookup"><span data-stu-id="da902-116">If the **Model class** drop-down does not show any model classes, make sure you compiled the project.</span></span> <span data-ttu-id="da902-117">Entity Framework 會使用反映，因此它需要編譯的組件。</span><span class="sxs-lookup"><span data-stu-id="da902-117">Entity Framework uses reflection, so it needs the compiled assembly.</span></span>


<span data-ttu-id="da902-118">選取 「&lt;新的資料內容&gt;」 就會開啟**新的資料內容**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="da902-118">Selecting "&lt;New Data Context&gt;" will open the **New Data Context** dialog.</span></span> <span data-ttu-id="da902-119">命名的資料內容`ProductStore.Models.OrdersContext`。</span><span class="sxs-lookup"><span data-stu-id="da902-119">Name the data context `ProductStore.Models.OrdersContext`.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

<span data-ttu-id="da902-120">按一下  **確定**以關閉**新的資料內容**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="da902-120">Click **OK** to dismiss the **New Data Context** dialog.</span></span> <span data-ttu-id="da902-121">在 [**新增控制器**] 對話方塊中，按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="da902-121">In the **Add Controller** dialog, click **Add**.</span></span>

<span data-ttu-id="da902-122">以下是項目加入至專案：</span><span class="sxs-lookup"><span data-stu-id="da902-122">Here's what got added to the project:</span></span>

- <span data-ttu-id="da902-123">類別，名為`OrdersContext`衍生自**DbContext**。</span><span class="sxs-lookup"><span data-stu-id="da902-123">A class named `OrdersContext` that derives from **DbContext**.</span></span> <span data-ttu-id="da902-124">這個類別提供 POCO 模型與資料庫之間的連結。</span><span class="sxs-lookup"><span data-stu-id="da902-124">This class provides the glue between the POCO models and the database.</span></span>
- <span data-ttu-id="da902-125">名為的 Web API 控制器`AdminController`。</span><span class="sxs-lookup"><span data-stu-id="da902-125">A Web API controller named `AdminController`.</span></span> <span data-ttu-id="da902-126">此控制站支援上的 CRUD 作業`Product`執行個體。</span><span class="sxs-lookup"><span data-stu-id="da902-126">This controller supports CRUD operations on `Product` instances.</span></span> <span data-ttu-id="da902-127">它會使用`OrdersContext`類別與 Entity Framework 進行通訊。</span><span class="sxs-lookup"><span data-stu-id="da902-127">It uses the `OrdersContext` class to communicate with Entity Framework.</span></span>
- <span data-ttu-id="da902-128">新資料庫的連接字串中的 Web.config 檔案。</span><span class="sxs-lookup"><span data-stu-id="da902-128">A new database connection string in the Web.config file.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

<span data-ttu-id="da902-129">開啟 OrdersContext.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="da902-129">Open the OrdersContext.cs file.</span></span> <span data-ttu-id="da902-130">請注意，建構函式會指定資料庫連接字串的名稱。</span><span class="sxs-lookup"><span data-stu-id="da902-130">Notice that the constructor specifies the name of the database connection string.</span></span> <span data-ttu-id="da902-131">這個名稱指的是已加入至 Web.config 的連接字串。</span><span class="sxs-lookup"><span data-stu-id="da902-131">This name refers to the connection string that was added to Web.config.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

<span data-ttu-id="da902-132">將下列屬性新增至 `OrdersContext` 類別：</span><span class="sxs-lookup"><span data-stu-id="da902-132">Add the following properties to the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

<span data-ttu-id="da902-133">A **DbSet**代表一組可查詢的實體。</span><span class="sxs-lookup"><span data-stu-id="da902-133">A **DbSet** represents a set of entities that can be queried.</span></span> <span data-ttu-id="da902-134">以下是完整清單，如`OrdersContext`類別：</span><span class="sxs-lookup"><span data-stu-id="da902-134">Here is the complete listing for the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

<span data-ttu-id="da902-135">`AdminController`類別會定義五個實作基本的 CRUD 功能的方法。</span><span class="sxs-lookup"><span data-stu-id="da902-135">The `AdminController` class defines five methods that implement basic CRUD functionality.</span></span> <span data-ttu-id="da902-136">每個方法會對應至用戶端可以叫用的 URI:</span><span class="sxs-lookup"><span data-stu-id="da902-136">Each method corresponds to a URI that the client can invoke:</span></span>

| <span data-ttu-id="da902-137">控制器方法</span><span class="sxs-lookup"><span data-stu-id="da902-137">Controller Method</span></span> | <span data-ttu-id="da902-138">描述</span><span class="sxs-lookup"><span data-stu-id="da902-138">Description</span></span> | <span data-ttu-id="da902-139">URI</span><span class="sxs-lookup"><span data-stu-id="da902-139">URI</span></span> | <span data-ttu-id="da902-140">HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="da902-140">HTTP Method</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="da902-141">GetProducts</span><span class="sxs-lookup"><span data-stu-id="da902-141">GetProducts</span></span> | <span data-ttu-id="da902-142">取得所有產品。</span><span class="sxs-lookup"><span data-stu-id="da902-142">Gets all products.</span></span> | <span data-ttu-id="da902-143">api/產品</span><span class="sxs-lookup"><span data-stu-id="da902-143">api/products</span></span> | <span data-ttu-id="da902-144">GET</span><span class="sxs-lookup"><span data-stu-id="da902-144">GET</span></span> |
| <span data-ttu-id="da902-145">GetProduct</span><span class="sxs-lookup"><span data-stu-id="da902-145">GetProduct</span></span> | <span data-ttu-id="da902-146">尋找產品識別碼。</span><span class="sxs-lookup"><span data-stu-id="da902-146">Finds a product by ID.</span></span> | <span data-ttu-id="da902-147">api/產品/*識別碼*</span><span class="sxs-lookup"><span data-stu-id="da902-147">api/products/*id*</span></span> | <span data-ttu-id="da902-148">GET</span><span class="sxs-lookup"><span data-stu-id="da902-148">GET</span></span> |
| <span data-ttu-id="da902-149">PutProduct</span><span class="sxs-lookup"><span data-stu-id="da902-149">PutProduct</span></span> | <span data-ttu-id="da902-150">更新的產品。</span><span class="sxs-lookup"><span data-stu-id="da902-150">Updates a product.</span></span> | <span data-ttu-id="da902-151">api/產品/*識別碼*</span><span class="sxs-lookup"><span data-stu-id="da902-151">api/products/*id*</span></span> | <span data-ttu-id="da902-152">PUT</span><span class="sxs-lookup"><span data-stu-id="da902-152">PUT</span></span> |
| <span data-ttu-id="da902-153">PostProduct</span><span class="sxs-lookup"><span data-stu-id="da902-153">PostProduct</span></span> | <span data-ttu-id="da902-154">建立新的產品。</span><span class="sxs-lookup"><span data-stu-id="da902-154">Creates a new product.</span></span> | <span data-ttu-id="da902-155">api/產品</span><span class="sxs-lookup"><span data-stu-id="da902-155">api/products</span></span> | <span data-ttu-id="da902-156">POST</span><span class="sxs-lookup"><span data-stu-id="da902-156">POST</span></span> |
| <span data-ttu-id="da902-157">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="da902-157">DeleteProduct</span></span> | <span data-ttu-id="da902-158">刪除產品。</span><span class="sxs-lookup"><span data-stu-id="da902-158">Deletes a product.</span></span> | <span data-ttu-id="da902-159">api/產品/*識別碼*</span><span class="sxs-lookup"><span data-stu-id="da902-159">api/products/*id*</span></span> | <span data-ttu-id="da902-160">DELETE</span><span class="sxs-lookup"><span data-stu-id="da902-160">DELETE</span></span> |

<span data-ttu-id="da902-161">每個方法會呼叫`OrdersContext`查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="da902-161">Each method calls into `OrdersContext` to query the database.</span></span> <span data-ttu-id="da902-162">修改集合 （PUT、 POST 和 DELETE） 的方法呼叫`db.SaveChanges`保存到資料庫變更。</span><span class="sxs-lookup"><span data-stu-id="da902-162">The methods that modify the collection (PUT, POST, and DELETE) call `db.SaveChanges` to persist the changes to the database.</span></span> <span data-ttu-id="da902-163">控制站建立每個 HTTP 要求，然後處置，因此必須保存的變更，方法傳回之前。</span><span class="sxs-lookup"><span data-stu-id="da902-163">Controllers are created per HTTP request and then disposed, so it is necessary to persist changes before a method returns.</span></span>

## <a name="add-a-database-initializer"></a><span data-ttu-id="da902-164">新增資料庫初始設定式</span><span class="sxs-lookup"><span data-stu-id="da902-164">Add a Database Initializer</span></span>

<span data-ttu-id="da902-165">Entity Framework 有不錯的功能，可讓您在啟動時，資料庫中填入資料和模型變更時，會自動重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="da902-165">Entity Framework has a nice feature that lets you populate the database on startup, and automatically recreate the database whenever the models change.</span></span> <span data-ttu-id="da902-166">這項功能是在開發期間非常有用，因為您永遠有一些測試資料，即使您變更模型。</span><span class="sxs-lookup"><span data-stu-id="da902-166">This feature is useful during development, because you always have some test data, even if you change the models.</span></span>

<span data-ttu-id="da902-167">在 [方案總管] 中，以滑鼠右鍵按一下 [模型] 資料夾，並建立新的類別，名為`OrdersContextInitializer`。</span><span class="sxs-lookup"><span data-stu-id="da902-167">In Solution Explorer, right-click the Models folder and create a new class named `OrdersContextInitializer`.</span></span> <span data-ttu-id="da902-168">貼上下列實作：</span><span class="sxs-lookup"><span data-stu-id="da902-168">Paste in the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

<span data-ttu-id="da902-169">藉由繼承自**DropCreateDatabaseIfModelChanges**類別中，我們會告訴 Entity Framework，使卸除資料庫，每當我們修改模型類別。</span><span class="sxs-lookup"><span data-stu-id="da902-169">By inheriting from the **DropCreateDatabaseIfModelChanges** class, we are telling Entity Framework to drop the database whenever we modify the model classes.</span></span> <span data-ttu-id="da902-170">當 Entity Framework 建立 （或重新建立） 的資料庫時，它會呼叫**種子**來填入資料表的方法。</span><span class="sxs-lookup"><span data-stu-id="da902-170">When Entity Framework creates (or recreates) the database, it calls the **Seed** method to populate the tables.</span></span> <span data-ttu-id="da902-171">我們會使用**種子**將某些範例產品再加上範例訂單的方法。</span><span class="sxs-lookup"><span data-stu-id="da902-171">We use the **Seed** method to add some example products plus an example order.</span></span>

<span data-ttu-id="da902-172">這項功能很適合用於測試，但不用**DropCreateDatabaseIfModelChanges**類別在實際執行環境、，因為如果有人變更模型類別，您可能會遺失您的資料。</span><span class="sxs-lookup"><span data-stu-id="da902-172">This feature is great for testing, but don't use the **DropCreateDatabaseIfModelChanges** class in production,, because you could lose your data if someone changes a model class.</span></span>

<span data-ttu-id="da902-173">接下來，開啟 Global.asax，然後將下列程式碼加入**應用程式\_啟動**方法：</span><span class="sxs-lookup"><span data-stu-id="da902-173">Next, open Global.asax and add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a><span data-ttu-id="da902-174">將要求傳送至控制器</span><span class="sxs-lookup"><span data-stu-id="da902-174">Send a Request to the Controller</span></span>

<span data-ttu-id="da902-175">到目前為止，我們還沒有撰寫任何用戶端程式碼，但您可以叫用的 web API 使用網頁瀏覽器或 HTTP 偵錯工具，例如[Fiddler](http://www.fiddler2.com/fiddler2/)。</span><span class="sxs-lookup"><span data-stu-id="da902-175">At this point, we haven't written any client code, but you can invoke the web API using a web browser or an HTTP debugging tool such as [Fiddler](http://www.fiddler2.com/fiddler2/).</span></span> <span data-ttu-id="da902-176">在 Visual Studio 中，按下 f5 鍵啟動偵錯。</span><span class="sxs-lookup"><span data-stu-id="da902-176">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="da902-177">您的網頁瀏覽器會開啟`http://localhost:*portnum*/`，其中*portnum*是某些連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="da902-177">Your web browser will open to `http://localhost:*portnum*/`, where *portnum* is some port number.</span></span>

<span data-ttu-id="da902-178">傳送 HTTP 要求，以 「`http://localhost:*portnum*/api/admin`。</span><span class="sxs-lookup"><span data-stu-id="da902-178">Send an HTTP request to "`http://localhost:*portnum*/api/admin`.</span></span> <span data-ttu-id="da902-179">第一個要求可能慢而無法完成，因為 Entify Framework，就必須建立並植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="da902-179">The first request may be slow to complete, because Entify Framework needs to create and seed the database.</span></span> <span data-ttu-id="da902-180">回應應該類似如下：</span><span class="sxs-lookup"><span data-stu-id="da902-180">The response should something similar to the following:</span></span>

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> <span data-ttu-id="da902-181">[上一頁](using-web-api-with-entity-framework-part-2.md)
> [下一頁](using-web-api-with-entity-framework-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="da902-181">[Previous](using-web-api-with-entity-framework-part-2.md)
[Next](using-web-api-with-entity-framework-part-4.md)</span></span>
