---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: "啟用 ASP.NET Web API 1 中的 CRUD 作業 |Microsoft 文件"
author: MikeWasson
description: "本教學課程會示範如何使用 ASP.NET Web API HTTP 服務中支援 CRUD 作業。 教學課程 Visual Studio 2012 Web 應用程式中使用的軟體版本..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 69b7d5453b6ff36d6e28a69428b016cb8cfd06e9
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2018
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="8bc4f-104">啟用 ASP.NET Web API 1 中的 CRUD 作業</span><span class="sxs-lookup"><span data-stu-id="8bc4f-104">Enabling CRUD Operations in ASP.NET Web API 1</span></span>
====================
<span data-ttu-id="8bc4f-105">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8bc4f-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="8bc4f-106">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="8bc4f-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="8bc4f-107">本教學課程會示範如何使用 ASP.NET Web API HTTP 服務中支援 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-107">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8bc4f-108">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="8bc4f-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="8bc4f-109">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="8bc4f-109">Visual Studio 2012</span></span>
> - <span data-ttu-id="8bc4f-110">Web API 1 （也適用於 Web API 2）</span><span class="sxs-lookup"><span data-stu-id="8bc4f-110">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="8bc4f-111">代表 CRUD&quot;建立、 讀取、 更新和刪除，&quot;哪些是四個基本資料庫作業。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-111">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="8bc4f-112">許多 HTTP 服務也模型透過 REST 或類似的 REST Api 的 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-112">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="8bc4f-113">在本教學課程中，您將建立非常簡單的 web 應用程式開發介面來管理的產品清單。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-113">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="8bc4f-114">名稱、 價格和類別，將會包含每個產品 (例如&quot;toys&quot;或&quot;硬體&quot;)，再加上產品識別碼。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-114">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="8bc4f-115">產品的 API 會公開下列方法。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-115">The products API will expose following methods.</span></span>

| <span data-ttu-id="8bc4f-116">動作</span><span class="sxs-lookup"><span data-stu-id="8bc4f-116">Action</span></span> | <span data-ttu-id="8bc4f-117">HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="8bc4f-117">HTTP method</span></span> | <span data-ttu-id="8bc4f-118">相對 URI</span><span class="sxs-lookup"><span data-stu-id="8bc4f-118">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8bc4f-119">取得所有產品的清單</span><span class="sxs-lookup"><span data-stu-id="8bc4f-119">Get a list of all products</span></span> | <span data-ttu-id="8bc4f-120">GET</span><span class="sxs-lookup"><span data-stu-id="8bc4f-120">GET</span></span> | <span data-ttu-id="8bc4f-121">/ api/產品</span><span class="sxs-lookup"><span data-stu-id="8bc4f-121">/api/products</span></span> |
| <span data-ttu-id="8bc4f-122">取得產品識別碼</span><span class="sxs-lookup"><span data-stu-id="8bc4f-122">Get a product by ID</span></span> | <span data-ttu-id="8bc4f-123">GET</span><span class="sxs-lookup"><span data-stu-id="8bc4f-123">GET</span></span> | <span data-ttu-id="8bc4f-124">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="8bc4f-124">/api/products/*id*</span></span> |
| <span data-ttu-id="8bc4f-125">取得產品類別目錄</span><span class="sxs-lookup"><span data-stu-id="8bc4f-125">Get a product by category</span></span> | <span data-ttu-id="8bc4f-126">GET</span><span class="sxs-lookup"><span data-stu-id="8bc4f-126">GET</span></span> | <span data-ttu-id="8bc4f-127">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="8bc4f-127">/api/products?category=*category*</span></span> |
| <span data-ttu-id="8bc4f-128">建立新的產品</span><span class="sxs-lookup"><span data-stu-id="8bc4f-128">Create a new product</span></span> | <span data-ttu-id="8bc4f-129">POST</span><span class="sxs-lookup"><span data-stu-id="8bc4f-129">POST</span></span> | <span data-ttu-id="8bc4f-130">/ api/產品</span><span class="sxs-lookup"><span data-stu-id="8bc4f-130">/api/products</span></span> |
| <span data-ttu-id="8bc4f-131">產品更新</span><span class="sxs-lookup"><span data-stu-id="8bc4f-131">Update a product</span></span> | <span data-ttu-id="8bc4f-132">PUT</span><span class="sxs-lookup"><span data-stu-id="8bc4f-132">PUT</span></span> | <span data-ttu-id="8bc4f-133">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="8bc4f-133">/api/products/*id*</span></span> |
| <span data-ttu-id="8bc4f-134">刪除產品</span><span class="sxs-lookup"><span data-stu-id="8bc4f-134">Delete a product</span></span> | <span data-ttu-id="8bc4f-135">DELETE</span><span class="sxs-lookup"><span data-stu-id="8bc4f-135">DELETE</span></span> | <span data-ttu-id="8bc4f-136">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="8bc4f-136">/api/products/*id*</span></span> |

<span data-ttu-id="8bc4f-137">請注意，部分 Uri 的路徑中包含產品識別碼。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-137">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="8bc4f-138">例如，若要取得其識別碼為 28 的產品，用戶端會傳送 GET 要求`http://hostname/api/products/28`。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-138">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="8bc4f-139">資源</span><span class="sxs-lookup"><span data-stu-id="8bc4f-139">Resources</span></span>

<span data-ttu-id="8bc4f-140">產品應用程式開發介面會定義兩個資源類型的 Uri:</span><span class="sxs-lookup"><span data-stu-id="8bc4f-140">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="8bc4f-141">資源</span><span class="sxs-lookup"><span data-stu-id="8bc4f-141">Resource</span></span> | <span data-ttu-id="8bc4f-142">URI</span><span class="sxs-lookup"><span data-stu-id="8bc4f-142">URI</span></span> |
| --- | --- |
| <span data-ttu-id="8bc4f-143">所有產品的清單。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-143">The list of all the products.</span></span> | <span data-ttu-id="8bc4f-144">/ api/產品</span><span class="sxs-lookup"><span data-stu-id="8bc4f-144">/api/products</span></span> |
| <span data-ttu-id="8bc4f-145">個別產品。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-145">An individual product.</span></span> | <span data-ttu-id="8bc4f-146">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="8bc4f-146">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="8bc4f-147">方法</span><span class="sxs-lookup"><span data-stu-id="8bc4f-147">Methods</span></span>

<span data-ttu-id="8bc4f-148">四個主要 HTTP 方法 （GET、 PUT、 POST 和 DELETE） 可以對應至 CRUD 作業，如下所示：</span><span class="sxs-lookup"><span data-stu-id="8bc4f-148">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="8bc4f-149">取得作業會擷取在指定的 URI 資源的表示法。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-149">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="8bc4f-150">取得應該在伺服器上沒有任何副作用。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-150">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="8bc4f-151">PUT 更新的資源時指定的 URI。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-151">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="8bc4f-152">PUT 也可用來建立新的資源在指定的 URI，如果伺服器允許用戶端指定新的 Uri。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-152">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="8bc4f-153">此教學課程中，應用程式開發介面不支援透過 PUT 建立。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-153">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="8bc4f-154">POST 建立新的資源。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-154">POST creates a new resource.</span></span> <span data-ttu-id="8bc4f-155">伺服器會指派新物件的 URI，並傳回此 URI 做為回應訊息的一部分。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-155">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="8bc4f-156">DELETE 會刪除的資源時指定的 URI。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-156">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="8bc4f-157">注意： PUT 方法會取代整個產品實體。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-157">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="8bc4f-158">也就是說，用戶端應該傳送更新的產品的完整表示法。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-158">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="8bc4f-159">如果您想要支援部分更新，更新程式最好方法。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-159">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="8bc4f-160">本教學課程未實作修補程式。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-160">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="8bc4f-161">建立新的 Web API 專案</span><span class="sxs-lookup"><span data-stu-id="8bc4f-161">Create a New Web API Project</span></span>

<span data-ttu-id="8bc4f-162">開始執行 Visual Studio，並選取**新專案**從**啟動**頁面。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-162">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="8bc4f-163">或從**檔案**功能表上，選取**新增**然後**專案**。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-163">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="8bc4f-164">在**範本**窗格中，選取**已安裝的範本**展開**Visual C#**節點。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-164">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="8bc4f-165">在下**Visual C#**，選取**Web**。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-165">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="8bc4f-166">在專案範本清單中選取**ASP.NET MVC 4 Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-166">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="8bc4f-167">將專案命名&quot;ProductStore&quot;按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-167">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="8bc4f-168">在**新增 ASP.NET MVC 4 專案**對話方塊中，選取**Web API**按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-168">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="8bc4f-169">加入模型</span><span class="sxs-lookup"><span data-stu-id="8bc4f-169">Adding a Model</span></span>

<span data-ttu-id="8bc4f-170">「模型」是代表應用程式中資料的物件。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-170">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="8bc4f-171">在 ASP.NET Web API 中，您可以使用強型別的 CLR 物件模型中，為，它們會自動序列化為 XML 或 JSON 用戶端。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-171">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="8bc4f-172">ProductStore api 中，我們的資料組成產品，因此我們將建立新的類別，名為`Product`。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-172">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="8bc4f-173">如果沒有顯示 方案總管 中，按一下**檢視**功能表，然後選取**方案總管 中**。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-173">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="8bc4f-174">在 [方案總管] 中，以滑鼠右鍵按一下**模型**資料夾。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-174">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="8bc4f-175">從內容 meny 中，選取**新增**，然後選取**類別**。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-175">From the context meny, select **Add**, then select **Class**.</span></span> <span data-ttu-id="8bc4f-176">將類別&quot;產品&quot;。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-176">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="8bc4f-177">加入下列屬性以`Product`類別。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-177">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="8bc4f-178">加入儲存機制</span><span class="sxs-lookup"><span data-stu-id="8bc4f-178">Adding a Repository</span></span>

<span data-ttu-id="8bc4f-179">我們需要儲存產品的集合。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-179">We need to store a collection of products.</span></span> <span data-ttu-id="8bc4f-180">最好區隔我們的服務實作中的集合。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-180">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="8bc4f-181">這樣一來，我們可以變更備份存放區，而不用重寫服務類別。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-181">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="8bc4f-182">這種設計稱為*儲存機制*模式。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-182">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="8bc4f-183">會定義儲存機制的泛型介面開始。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-183">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="8bc4f-184">在 [方案總管] 中，以滑鼠右鍵按一下**模型**資料夾。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-184">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="8bc4f-185">選取**新增**，然後選取**新項目**。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-185">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="8bc4f-186">在**範本**窗格中，選取**已安裝的範本**展開 C# 節點。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-186">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="8bc4f-187">在 C# 中，選取**程式碼**。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-187">Under C#, select **Code**.</span></span> <span data-ttu-id="8bc4f-188">在程式碼的範本清單中，選取**介面**。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-188">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="8bc4f-189">命名介面&quot;IProductRepository&quot;。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-189">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="8bc4f-190">加入下列實作：</span><span class="sxs-lookup"><span data-stu-id="8bc4f-190">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="8bc4f-191">現在將另一個類別加入至 [模型] 資料夾，名為&quot;ProductRepository。&quot;此類別會實作 `IProductRespository` 介面。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-191">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRespository` interface.</span></span> <span data-ttu-id="8bc4f-192">加入下列實作：</span><span class="sxs-lookup"><span data-stu-id="8bc4f-192">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="8bc4f-193">儲存機制會保留在本機記憶體中的清單。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-193">The repository keeps the list in local memory.</span></span> <span data-ttu-id="8bc4f-194">這是 [確定]，教學課程中，但在實際應用中，您會將資料儲存於外部的資料庫或雲端儲存體中。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-194">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="8bc4f-195">儲存機制模式會讓您更輕鬆地變更更新版本的實作。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-195">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="8bc4f-196">加入 Web API 控制器</span><span class="sxs-lookup"><span data-stu-id="8bc4f-196">Adding a Web API Controller</span></span>

<span data-ttu-id="8bc4f-197">如果您已使用 ASP.NET MVC，則您必須已熟悉控制站。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-197">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="8bc4f-198">在 ASP.NET Web API 中，*控制器*是處理來自用戶端的 HTTP 要求的類別。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-198">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="8bc4f-199">新增專案 精靈建立專案時，為您建立兩個控制站。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-199">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="8bc4f-200">若要查看它們，請展開 [方案總管] 中的 [控制器] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-200">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="8bc4f-201">HomeController 是傳統的 ASP.NET MVC 控制器。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-201">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="8bc4f-202">它會負責處理站台的 HTML 網頁，並與我們的 web 應用程式開發介面不直接相關。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-202">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="8bc4f-203">ValuesController 是範例 WebAPI 控制器。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-203">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="8bc4f-204">繼續進行和刪除 ValuesController，以滑鼠右鍵按一下方案總管 中的檔案，然後選取**刪除。**</span><span class="sxs-lookup"><span data-stu-id="8bc4f-204">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="8bc4f-205">現在加入新的控制站，如下所示：</span><span class="sxs-lookup"><span data-stu-id="8bc4f-205">Now add a new controller, as follows:</span></span>

<span data-ttu-id="8bc4f-206">在**方案總管 中**，以滑鼠右鍵按一下 控制器 資料夾。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-206">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="8bc4f-207">選取**新增**，然後選取 **控制器**。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-207">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="8bc4f-208">在**加入控制器**精靈 中，名稱控制器&quot;ProductsController&quot;。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-208">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="8bc4f-209">在**範本**下拉式清單中，選取**空白的 API 控制器**。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-209">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="8bc4f-210">然後按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-210">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="8bc4f-211">您不需要您 contollers 放入名為控制器的資料夾。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-211">It is not necessary to put your contollers into a folder named Controllers.</span></span> <span data-ttu-id="8bc4f-212">資料夾名稱並不重要;它只是方便方法來組織您的來源檔案。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-212">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="8bc4f-213">**加入控制器**精靈會建立名為 ProductsController.cs [控制器] 資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-213">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="8bc4f-214">如果此檔案尚未開啟，按兩下檔案將它開啟。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-214">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="8bc4f-215">加入下列**使用**陳述式：</span><span class="sxs-lookup"><span data-stu-id="8bc4f-215">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="8bc4f-216">加入欄位保存**IProductRepository**執行個體。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-216">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="8bc4f-217">呼叫`new ProductRepository()`控制器中不是最好的設計，因為它繫結至特定的實作控制器`IProductRepository`。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-217">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="8bc4f-218">更好的方法，請參閱[使用 Web 應用程式開發介面的相依性解析程式](../advanced/dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-218">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="8bc4f-219">取得資源</span><span class="sxs-lookup"><span data-stu-id="8bc4f-219">Getting a Resource</span></span>

<span data-ttu-id="8bc4f-220">ProductStore API 會公開數個&quot;讀取&quot;HTTP GET 方法的動作。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-220">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="8bc4f-221">每個動作將會對應到中的方法`ProductsController`類別。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-221">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="8bc4f-222">動作</span><span class="sxs-lookup"><span data-stu-id="8bc4f-222">Action</span></span> | <span data-ttu-id="8bc4f-223">HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="8bc4f-223">HTTP method</span></span> | <span data-ttu-id="8bc4f-224">相對 URI</span><span class="sxs-lookup"><span data-stu-id="8bc4f-224">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8bc4f-225">取得所有產品的清單</span><span class="sxs-lookup"><span data-stu-id="8bc4f-225">Get a list of all products</span></span> | <span data-ttu-id="8bc4f-226">GET</span><span class="sxs-lookup"><span data-stu-id="8bc4f-226">GET</span></span> | <span data-ttu-id="8bc4f-227">/ api/產品</span><span class="sxs-lookup"><span data-stu-id="8bc4f-227">/api/products</span></span> |
| <span data-ttu-id="8bc4f-228">取得產品識別碼</span><span class="sxs-lookup"><span data-stu-id="8bc4f-228">Get a product by ID</span></span> | <span data-ttu-id="8bc4f-229">GET</span><span class="sxs-lookup"><span data-stu-id="8bc4f-229">GET</span></span> | <span data-ttu-id="8bc4f-230">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="8bc4f-230">/api/products/*id*</span></span> |
| <span data-ttu-id="8bc4f-231">取得產品類別目錄</span><span class="sxs-lookup"><span data-stu-id="8bc4f-231">Get a product by category</span></span> | <span data-ttu-id="8bc4f-232">GET</span><span class="sxs-lookup"><span data-stu-id="8bc4f-232">GET</span></span> | <span data-ttu-id="8bc4f-233">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="8bc4f-233">/api/products?category=*category*</span></span> |

<span data-ttu-id="8bc4f-234">若要取得所有產品的清單，請將此方法加入`ProductsController`類別：</span><span class="sxs-lookup"><span data-stu-id="8bc4f-234">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="8bc4f-235">方法名稱開頭&quot;取得&quot;，好讓它對應至 GET 要求的慣例。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-235">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="8bc4f-236">此外，因為方法不有任何參數，則會對應至 URI，其中不包含*&quot;識別碼&quot;*中路徑區段。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-236">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="8bc4f-237">若要取得的產品識別碼，請將此方法加入`ProductsController`類別：</span><span class="sxs-lookup"><span data-stu-id="8bc4f-237">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="8bc4f-238">這個方法的名稱也會啟動具有&quot;取得&quot;，但這個方法有名稱為的參數*識別碼*。這個參數會對應至&quot;識別碼&quot;URI 路徑的區段。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-238">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="8bc4f-239">ASP.NET Web API framework 自動將 ID 轉換成正確的資料類型 (**int**) 參數。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-239">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="8bc4f-240">GetProduct 方法擲回例外狀況型別的**HttpResponseException**如果*識別碼*不正確。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-240">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="8bc4f-241">這個例外狀況會由架構轉譯為 404 （找不到） 錯誤。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-241">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="8bc4f-242">最後，將方法加入依分類搜尋產品：</span><span class="sxs-lookup"><span data-stu-id="8bc4f-242">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="8bc4f-243">要求 URI 具有查詢字串，如果 Web API 會嘗試比對控制器方法上的參數的查詢參數。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-243">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="8bc4f-244">因此，在表單的 URI"api/產品？ 類別 =*類別*」 將會對應至這個方法。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-244">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="8bc4f-245">建立資源</span><span class="sxs-lookup"><span data-stu-id="8bc4f-245">Creating a Resource</span></span>

<span data-ttu-id="8bc4f-246">接下來，我們會將方法加入`ProductsController`類別來建立新的產品。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-246">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="8bc4f-247">以下是簡單的實作的方法：</span><span class="sxs-lookup"><span data-stu-id="8bc4f-247">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="8bc4f-248">請注意此方法的相關兩件事：</span><span class="sxs-lookup"><span data-stu-id="8bc4f-248">Note two things about this method:</span></span>

- <span data-ttu-id="8bc4f-249">方法名稱開頭&quot;Post...&quot;.</span><span class="sxs-lookup"><span data-stu-id="8bc4f-249">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="8bc4f-250">若要建立新的產品，用戶端會傳送 HTTP POST 要求。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-250">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="8bc4f-251">方法會接受 Product 類型的參數。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-251">The method takes a parameter of type Product.</span></span> <span data-ttu-id="8bc4f-252">在 Web API 中，具有複雜類型的參數會從要求主體還原序列化。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-252">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="8bc4f-253">因此，我們會以 XML 或 JSON 格式傳送序列化的物件的表示產品，用戶端。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-253">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="8bc4f-254">這項實作也能運作，但還不算完整。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-254">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="8bc4f-255">在理想情況下，我們都希望 HTTP 回應對包括下列各項：</span><span class="sxs-lookup"><span data-stu-id="8bc4f-255">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="8bc4f-256">**回應碼：**根據預設，Web API framework 設定回應狀態碼 200 （確定）。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-256">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="8bc4f-257">但是，根據 HTTP/1.1 通訊協定，POST 要求所產生的資源，建立伺服器應該回覆狀態 201 （已建立）。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-257">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="8bc4f-258">**位置：**當伺服器建立資源時，它應該包含新資源的 URI 回應的位置標頭中。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-258">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="8bc4f-259">ASP.NET Web API 輕鬆地操作 HTTP 回應訊息。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-259">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="8bc4f-260">以下是改進的實作：</span><span class="sxs-lookup"><span data-stu-id="8bc4f-260">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="8bc4f-261">請注意，方法的傳回型別現在**HttpResponseMessage**。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-261">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="8bc4f-262">藉由傳回**HttpResponseMessage**而不是產品，我們可以控制的 HTTP 回應訊息，包括狀態碼和 Location 標頭的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-262">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="8bc4f-263">**CreateResponse**方法會建立**HttpResponseMessage**並自動將 Product 物件的序列化表示法寫入本文 fo 回應訊息。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-263">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="8bc4f-264">此範例不會驗證`Product`。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-264">This example does not validate the `Product`.</span></span> <span data-ttu-id="8bc4f-265">如需模型驗證資訊，請參閱[ASP.NET Web API 中的模型驗證](../formats-and-model-binding/model-validation-in-aspnet-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-265">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="8bc4f-266">更新資源</span><span class="sxs-lookup"><span data-stu-id="8bc4f-266">Updating a Resource</span></span>

<span data-ttu-id="8bc4f-267">使用 PUT 更新產品相當直接：</span><span class="sxs-lookup"><span data-stu-id="8bc4f-267">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="8bc4f-268">方法名稱開頭&quot;放...&quot;，因此 Web API 進行比對 PUT 要求。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-268">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="8bc4f-269">此方法會採用兩個參數、 產品識別碼和更新的產品。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-269">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="8bc4f-270">*識別碼*參數取自 URI 路徑，而*產品*參數會從要求主體還原序列化。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-270">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="8bc4f-271">根據預設，ASP.NET Web API 架構會取用來自路由的簡單參數類型和複雜類型的要求主體。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-271">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="8bc4f-272">刪除資源</span><span class="sxs-lookup"><span data-stu-id="8bc4f-272">Deleting a Resource</span></span>

<span data-ttu-id="8bc4f-273">若要刪除 resourse，定義 「 刪除...」 方法。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-273">To delete a resourse, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="8bc4f-274">如果成功刪除要求，它可以傳回狀態 200 (OK) 描述狀態; 實體主體狀態 202 （已接受） 如果仍已刪除暫止的;或狀態沒有實體主體 204 （沒有內容）。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-274">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="8bc4f-275">在此情況下，`DeleteProduct`方法有`void`傳回型別，因此 ASP.NET Web API 自動轉譯為此狀態碼 204 （沒有內容）。</span><span class="sxs-lookup"><span data-stu-id="8bc4f-275">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
