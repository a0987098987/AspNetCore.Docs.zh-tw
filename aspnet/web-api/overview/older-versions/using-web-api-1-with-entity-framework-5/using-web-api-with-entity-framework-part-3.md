---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: "第 3 部分： 建立系統管理控制站 |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 6fadfb6e96ae287fc5f81516b7535e03853c7e6a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="part-3-creating-an-admin-controller"></a><span data-ttu-id="a11ff-102">第 3 部分： 建立系統管理控制站</span><span class="sxs-lookup"><span data-stu-id="a11ff-102">Part 3: Creating an Admin Controller</span></span>
====================
<span data-ttu-id="a11ff-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a11ff-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a11ff-104">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="a11ff-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a><span data-ttu-id="a11ff-105">新增系統管理控制站</span><span class="sxs-lookup"><span data-stu-id="a11ff-105">Add an Admin Controller</span></span>

<span data-ttu-id="a11ff-106">在本節中，我們會將新增的 Web API 控制器支援 CRUD （建立、 讀取、 更新和刪除） 的產品上的作業。</span><span class="sxs-lookup"><span data-stu-id="a11ff-106">In this section, we'll add a Web API controller that supports CRUD (create, read, update, and delete) operations on products.</span></span> <span data-ttu-id="a11ff-107">控制器將會使用 Entity Framework 資料庫層級與通訊。</span><span class="sxs-lookup"><span data-stu-id="a11ff-107">The controller will use Entity Framework to communicate with the database layer.</span></span> <span data-ttu-id="a11ff-108">只有系統管理員可以使用此控制站。</span><span class="sxs-lookup"><span data-stu-id="a11ff-108">Only administrators will be able to use this controller.</span></span> <span data-ttu-id="a11ff-109">客戶會透過另一個控制站存取的產品。</span><span class="sxs-lookup"><span data-stu-id="a11ff-109">Customers will access the products through another controller.</span></span>

<span data-ttu-id="a11ff-110">在 [方案總管] 中，以滑鼠右鍵按一下 [控制器] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="a11ff-110">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="a11ff-111">選取**新增**然後**控制器**。</span><span class="sxs-lookup"><span data-stu-id="a11ff-111">Select **Add** and then **Controller**.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

<span data-ttu-id="a11ff-112">在**加入控制器**對話方塊中，控制器`AdminController`。</span><span class="sxs-lookup"><span data-stu-id="a11ff-112">In the **Add Controller** dialog, name the controller `AdminController`.</span></span> <span data-ttu-id="a11ff-113">在下**範本**，選取&quot;使用 Entity Framework 的讀取/寫入動作的 API 控制器&quot;。</span><span class="sxs-lookup"><span data-stu-id="a11ff-113">Under **Template**, select &quot;API controller with read/write actions, using Entity Framework&quot;.</span></span> <span data-ttu-id="a11ff-114">在下**模型類別**，選取 「 產品 (ProductStore.Models) 」。</span><span class="sxs-lookup"><span data-stu-id="a11ff-114">Under **Model class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="a11ff-115">在下**資料內容**，選取 「&lt;新的資料內容&gt;"。</span><span class="sxs-lookup"><span data-stu-id="a11ff-115">Under **Data Context**, select "&lt;New Data Context&gt;".</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="a11ff-116">如果**模型類別**下拉式清單不會顯示任何模型類別，請確定您編譯專案。</span><span class="sxs-lookup"><span data-stu-id="a11ff-116">If the **Model class** drop-down does not show any model classes, make sure you compiled the project.</span></span> <span data-ttu-id="a11ff-117">Entity Framework 會使用反映，因此它需要編譯的組件。</span><span class="sxs-lookup"><span data-stu-id="a11ff-117">Entity Framework uses reflection, so it needs the compiled assembly.</span></span>


<span data-ttu-id="a11ff-118">選取 「&lt;新的資料內容&gt;」 將會開啟**新的資料內容**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="a11ff-118">Selecting "&lt;New Data Context&gt;" will open the **New Data Context** dialog.</span></span> <span data-ttu-id="a11ff-119">命名的資料內容`ProductStore.Models.OrdersContext`。</span><span class="sxs-lookup"><span data-stu-id="a11ff-119">Name the data context `ProductStore.Models.OrdersContext`.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

<span data-ttu-id="a11ff-120">按一下**確定**解除**新的資料內容**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="a11ff-120">Click **OK** to dismiss the **New Data Context** dialog.</span></span> <span data-ttu-id="a11ff-121">在**加入控制器**] 對話方塊中，按一下 [**新增**。</span><span class="sxs-lookup"><span data-stu-id="a11ff-121">In the **Add Controller** dialog, click **Add**.</span></span>

<span data-ttu-id="a11ff-122">以下是項目已加入至專案：</span><span class="sxs-lookup"><span data-stu-id="a11ff-122">Here's what got added to the project:</span></span>

- <span data-ttu-id="a11ff-123">類別，名為`OrdersContext`衍生自**DbContext**。</span><span class="sxs-lookup"><span data-stu-id="a11ff-123">A class named `OrdersContext` that derives from **DbContext**.</span></span> <span data-ttu-id="a11ff-124">這個類別會提供黏附 POCO 模型與資料庫之間。</span><span class="sxs-lookup"><span data-stu-id="a11ff-124">This class provides the glue between the POCO models and the database.</span></span>
- <span data-ttu-id="a11ff-125">名為此 Web API 控制器`AdminController`。</span><span class="sxs-lookup"><span data-stu-id="a11ff-125">A Web API controller named `AdminController`.</span></span> <span data-ttu-id="a11ff-126">此控制站上支援 CRUD 作業`Product`執行個體。</span><span class="sxs-lookup"><span data-stu-id="a11ff-126">This controller supports CRUD operations on `Product` instances.</span></span> <span data-ttu-id="a11ff-127">它會使用`OrdersContext`與 Entity Framework 進行的類別。</span><span class="sxs-lookup"><span data-stu-id="a11ff-127">It uses the `OrdersContext` class to communicate with Entity Framework.</span></span>
- <span data-ttu-id="a11ff-128">新資料庫的連接字串中的 Web.config 檔案。</span><span class="sxs-lookup"><span data-stu-id="a11ff-128">A new database connection string in the Web.config file.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

<span data-ttu-id="a11ff-129">開啟 OrdersContext.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="a11ff-129">Open the OrdersContext.cs file.</span></span> <span data-ttu-id="a11ff-130">請注意，建構函式會指定資料庫連接字串的名稱。</span><span class="sxs-lookup"><span data-stu-id="a11ff-130">Notice that the constructor specifies the name of the database connection string.</span></span> <span data-ttu-id="a11ff-131">這個名稱會參考已加入至 Web.config 連接字串。</span><span class="sxs-lookup"><span data-stu-id="a11ff-131">This name refers to the connection string that was added to Web.config.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

<span data-ttu-id="a11ff-132">將下列屬性新增至 `OrdersContext` 類別：</span><span class="sxs-lookup"><span data-stu-id="a11ff-132">Add the following properties to the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

<span data-ttu-id="a11ff-133">A **DbSet**代表一組可查詢的實體。</span><span class="sxs-lookup"><span data-stu-id="a11ff-133">A **DbSet** represents a set of entities that can be queried.</span></span> <span data-ttu-id="a11ff-134">以下是完整清單`OrdersContext`類別：</span><span class="sxs-lookup"><span data-stu-id="a11ff-134">Here is the complete listing for the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

<span data-ttu-id="a11ff-135">`AdminController`類別會定義五種實作基本 CRUD 功能的方法。</span><span class="sxs-lookup"><span data-stu-id="a11ff-135">The `AdminController` class defines five methods that implement basic CRUD functionality.</span></span> <span data-ttu-id="a11ff-136">每個方法會對應至用戶端可以叫用的 URI:</span><span class="sxs-lookup"><span data-stu-id="a11ff-136">Each method corresponds to a URI that the client can invoke:</span></span>

| <span data-ttu-id="a11ff-137">控制器方法</span><span class="sxs-lookup"><span data-stu-id="a11ff-137">Controller Method</span></span> | <span data-ttu-id="a11ff-138">說明</span><span class="sxs-lookup"><span data-stu-id="a11ff-138">Description</span></span> | <span data-ttu-id="a11ff-139">URI</span><span class="sxs-lookup"><span data-stu-id="a11ff-139">URI</span></span> | <span data-ttu-id="a11ff-140">HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="a11ff-140">HTTP Method</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a11ff-141">GetProducts</span><span class="sxs-lookup"><span data-stu-id="a11ff-141">GetProducts</span></span> | <span data-ttu-id="a11ff-142">取得所有產品。</span><span class="sxs-lookup"><span data-stu-id="a11ff-142">Gets all products.</span></span> | <span data-ttu-id="a11ff-143">應用程式開發介面/產品</span><span class="sxs-lookup"><span data-stu-id="a11ff-143">api/products</span></span> | <span data-ttu-id="a11ff-144">GET</span><span class="sxs-lookup"><span data-stu-id="a11ff-144">GET</span></span> |
| <span data-ttu-id="a11ff-145">GetProduct</span><span class="sxs-lookup"><span data-stu-id="a11ff-145">GetProduct</span></span> | <span data-ttu-id="a11ff-146">找到的產品識別碼。</span><span class="sxs-lookup"><span data-stu-id="a11ff-146">Finds a product by ID.</span></span> | <span data-ttu-id="a11ff-147">應用程式開發介面/產品/*識別碼*</span><span class="sxs-lookup"><span data-stu-id="a11ff-147">api/products/*id*</span></span> | <span data-ttu-id="a11ff-148">GET</span><span class="sxs-lookup"><span data-stu-id="a11ff-148">GET</span></span> |
| <span data-ttu-id="a11ff-149">PutProduct</span><span class="sxs-lookup"><span data-stu-id="a11ff-149">PutProduct</span></span> | <span data-ttu-id="a11ff-150">更新的產品。</span><span class="sxs-lookup"><span data-stu-id="a11ff-150">Updates a product.</span></span> | <span data-ttu-id="a11ff-151">應用程式開發介面/產品/*識別碼*</span><span class="sxs-lookup"><span data-stu-id="a11ff-151">api/products/*id*</span></span> | <span data-ttu-id="a11ff-152">PUT</span><span class="sxs-lookup"><span data-stu-id="a11ff-152">PUT</span></span> |
| <span data-ttu-id="a11ff-153">PostProduct</span><span class="sxs-lookup"><span data-stu-id="a11ff-153">PostProduct</span></span> | <span data-ttu-id="a11ff-154">建立新的產品。</span><span class="sxs-lookup"><span data-stu-id="a11ff-154">Creates a new product.</span></span> | <span data-ttu-id="a11ff-155">應用程式開發介面/產品</span><span class="sxs-lookup"><span data-stu-id="a11ff-155">api/products</span></span> | <span data-ttu-id="a11ff-156">POST</span><span class="sxs-lookup"><span data-stu-id="a11ff-156">POST</span></span> |
| <span data-ttu-id="a11ff-157">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="a11ff-157">DeleteProduct</span></span> | <span data-ttu-id="a11ff-158">刪除產品。</span><span class="sxs-lookup"><span data-stu-id="a11ff-158">Deletes a product.</span></span> | <span data-ttu-id="a11ff-159">應用程式開發介面/產品/*識別碼*</span><span class="sxs-lookup"><span data-stu-id="a11ff-159">api/products/*id*</span></span> | <span data-ttu-id="a11ff-160">DELETE</span><span class="sxs-lookup"><span data-stu-id="a11ff-160">DELETE</span></span> |

<span data-ttu-id="a11ff-161">每個方法呼叫`OrdersContext`查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="a11ff-161">Each method calls into `OrdersContext` to query the database.</span></span> <span data-ttu-id="a11ff-162">修改集合 （PUT、 POST 和 DELETE） 的方法呼叫`db.SaveChanges`保存到資料庫變更。</span><span class="sxs-lookup"><span data-stu-id="a11ff-162">The methods that modify the collection (PUT, POST, and DELETE) call `db.SaveChanges` to persist the changes to the database.</span></span> <span data-ttu-id="a11ff-163">控制站建立每個 HTTP 要求，然後處置，因此需要保存的變更，方法傳回之前。</span><span class="sxs-lookup"><span data-stu-id="a11ff-163">Controllers are created per HTTP request and then disposed, so it is necessary to persist changes before a method returns.</span></span>

## <a name="add-a-database-initializer"></a><span data-ttu-id="a11ff-164">加入的資料庫初始設定式</span><span class="sxs-lookup"><span data-stu-id="a11ff-164">Add a Database Initializer</span></span>

<span data-ttu-id="a11ff-165">Entity Framework 必須方便的功能可讓您在啟動時，在資料庫中填入，每當變更模型時，會自動重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="a11ff-165">Entity Framework has a nice feature that lets you populate the database on startup, and automatically recreate the database whenever the models change.</span></span> <span data-ttu-id="a11ff-166">您始終有一些測試資料，即使您變更模型，這項功能是在開發期間相當實用。</span><span class="sxs-lookup"><span data-stu-id="a11ff-166">This feature is useful during development, because you always have some test data, even if you change the models.</span></span>

<span data-ttu-id="a11ff-167">在 [方案總管] 中，以滑鼠右鍵按一下 [模型] 資料夾和建立新的類別，名為`OrdersContextInitializer`。</span><span class="sxs-lookup"><span data-stu-id="a11ff-167">In Solution Explorer, right-click the Models folder and create a new class named `OrdersContextInitializer`.</span></span> <span data-ttu-id="a11ff-168">貼上下列實作：</span><span class="sxs-lookup"><span data-stu-id="a11ff-168">Paste in the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

<span data-ttu-id="a11ff-169">透過繼承自**DropCreateDatabaseIfModelChanges**類別中，我們會告知 Entity Framework，每當我們修改模型類別時，請卸除資料庫。</span><span class="sxs-lookup"><span data-stu-id="a11ff-169">By inheriting from the **DropCreateDatabaseIfModelChanges** class, we are telling Entity Framework to drop the database whenever we modify the model classes.</span></span> <span data-ttu-id="a11ff-170">當 Entity Framework 建立 （或重新建立） 資料庫時，它會呼叫**種子**來擴展資料表的方法。</span><span class="sxs-lookup"><span data-stu-id="a11ff-170">When Entity Framework creates (or recreates) the database, it calls the **Seed** method to populate the tables.</span></span> <span data-ttu-id="a11ff-171">我們使用**種子**方法，將某些範例產品加上的範例訂單。</span><span class="sxs-lookup"><span data-stu-id="a11ff-171">We use the **Seed** method to add some example products plus an example order.</span></span>

<span data-ttu-id="a11ff-172">這項功能最適合用於測試，但並不使用**DropCreateDatabaseIfModelChanges**在實際執行環境、 類別，因為如果有人變更模型類別，您可能會失去您的資料。</span><span class="sxs-lookup"><span data-stu-id="a11ff-172">This feature is great for testing, but don't use the **DropCreateDatabaseIfModelChanges** class in production,, because you could lose your data if someone changes a model class.</span></span>

<span data-ttu-id="a11ff-173">接下來，開啟 Global.asax 並加入下列程式碼加入**應用程式\_啟動**方法：</span><span class="sxs-lookup"><span data-stu-id="a11ff-173">Next, open Global.asax and add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a><span data-ttu-id="a11ff-174">將要求傳送至控制器</span><span class="sxs-lookup"><span data-stu-id="a11ff-174">Send a Request to the Controller</span></span>

<span data-ttu-id="a11ff-175">此時，我們尚未撰寫任何用戶端程式碼，但您可以叫用的 web 應用程式開發介面使用網頁瀏覽器或 HTTP 偵錯工具，例如[Fiddler](http://www.fiddler2.com/fiddler2/)。</span><span class="sxs-lookup"><span data-stu-id="a11ff-175">At this point, we haven't written any client code, but you can invoke the web API using a web browser or an HTTP debugging tool such as [Fiddler](http://www.fiddler2.com/fiddler2/).</span></span> <span data-ttu-id="a11ff-176">在 Visual Studio 中，按 f5 鍵開始偵錯。</span><span class="sxs-lookup"><span data-stu-id="a11ff-176">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="a11ff-177">您的網頁瀏覽器會開啟`http://localhost:*portnum*/`，其中*portnum*是某些連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="a11ff-177">Your web browser will open to `http://localhost:*portnum*/`, where *portnum* is some port number.</span></span>

<span data-ttu-id="a11ff-178">傳送 HTTP 要求，以便 「`http://localhost:*portnum*/api/admin`。</span><span class="sxs-lookup"><span data-stu-id="a11ff-178">Send an HTTP request to "`http://localhost:*portnum*/api/admin`.</span></span> <span data-ttu-id="a11ff-179">第一次要求可能慢而無法完成，因為 Entify Framework 需要建立並植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="a11ff-179">The first request may be slow to complete, because Entify Framework needs to create and seed the database.</span></span> <span data-ttu-id="a11ff-180">回應應與下列類似：</span><span class="sxs-lookup"><span data-stu-id="a11ff-180">The response should something similar to the following:</span></span>

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

>[!div class="step-by-step"]
<span data-ttu-id="a11ff-181">[上一頁](using-web-api-with-entity-framework-part-2.md)
[下一頁](using-web-api-with-entity-framework-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="a11ff-181">[Previous](using-web-api-with-entity-framework-part-2.md)
[Next](using-web-api-with-entity-framework-part-4.md)</span></span>
