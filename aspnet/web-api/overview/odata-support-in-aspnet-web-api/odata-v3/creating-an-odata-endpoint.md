---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: 建立 Web API 2 OData v3 端點 |Microsoft Docs
author: MikeWasson
description: 開放式資料通訊協定 (OData) 是網站的資料存取通訊協定。 OData 提供統一的方式來組織資料、 查詢資料，和操作資料...
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 654f697c8d095d45ba31e2808c52f9ad24b606c8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825767"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a><span data-ttu-id="03ba7-104">建立 Web API 2 OData v3 端點</span><span class="sxs-lookup"><span data-stu-id="03ba7-104">Creating an OData v3 Endpoint with Web API 2</span></span>
====================
<span data-ttu-id="03ba7-105">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="03ba7-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="03ba7-106">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="03ba7-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="03ba7-107">[開放式資料通訊協定](http://www.odata.org/)(OData) 是網站的資料存取通訊協定。</span><span class="sxs-lookup"><span data-stu-id="03ba7-107">The [Open Data Protocol](http://www.odata.org/) (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="03ba7-108">OData 提供統一的方式來組織資料、 查詢資料，和操作資料集，透過 CRUD 作業 （建立、 讀取、 更新和刪除）。</span><span class="sxs-lookup"><span data-stu-id="03ba7-108">OData provides a uniform way to structure data, query the data, and manipulate the data set through CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="03ba7-109">OData 支援 AtomPub (XML) 和 JSON 格式。</span><span class="sxs-lookup"><span data-stu-id="03ba7-109">OData supports both AtomPub (XML) and JSON formats.</span></span> <span data-ttu-id="03ba7-110">OData 也會定義用來公開資料的中繼資料。</span><span class="sxs-lookup"><span data-stu-id="03ba7-110">OData also defines a way to expose metadata about the data.</span></span> <span data-ttu-id="03ba7-111">用戶端可以使用中繼資料來探索的型別資訊和資料集的關聯性。</span><span class="sxs-lookup"><span data-stu-id="03ba7-111">Clients can use the metadata to discover the type information and relationships for the data set.</span></span>
> 
> <span data-ttu-id="03ba7-112">ASP.NET Web API 可讓您更輕鬆地建立資料集的 OData 端點。</span><span class="sxs-lookup"><span data-stu-id="03ba7-112">ASP.NET Web API makes it easy to create an OData endpoint for a data set.</span></span> <span data-ttu-id="03ba7-113">您可以控制完全的 OData 作業端點支援。</span><span class="sxs-lookup"><span data-stu-id="03ba7-113">You can control exactly which OData operations the endpoint supports.</span></span> <span data-ttu-id="03ba7-114">您可以裝載多個 OData 端點，以及非 OData 端點。</span><span class="sxs-lookup"><span data-stu-id="03ba7-114">You can host multiple OData endpoints, alongside non-OData endpoints.</span></span> <span data-ttu-id="03ba7-115">您可以完整控制您的資料模型、 後端商務邏輯和資料層。</span><span class="sxs-lookup"><span data-stu-id="03ba7-115">You have full control over your data model, back-end business logic, and data layer.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="03ba7-116">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="03ba7-116">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="03ba7-117">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="03ba7-117">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="03ba7-118">Web API 2</span><span class="sxs-lookup"><span data-stu-id="03ba7-118">Web API 2</span></span>
> - <span data-ttu-id="03ba7-119">OData 版本 3</span><span class="sxs-lookup"><span data-stu-id="03ba7-119">OData Version 3</span></span>
> - <span data-ttu-id="03ba7-120">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="03ba7-120">Entity Framework 6</span></span>
> - [<span data-ttu-id="03ba7-121">Fiddler Web 偵錯 Proxy （選擇性）</span><span class="sxs-lookup"><span data-stu-id="03ba7-121">Fiddler Web Debugging Proxy (Optional)</span></span>](http://www.fiddler2.com)
> 
> <span data-ttu-id="03ba7-122">已加入 web API OData 支援[ASP.NET 和 Web 工具 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650)。</span><span class="sxs-lookup"><span data-stu-id="03ba7-122">Web API OData support was added in [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="03ba7-123">不過，本教學課程會使用 Visual Studio 2013 中已加入的 scaffolding。</span><span class="sxs-lookup"><span data-stu-id="03ba7-123">However, this tutorial uses scaffolding that was added in Visual Studio 2013.</span></span>


<span data-ttu-id="03ba7-124">在本教學課程中，您將建立簡單的 OData 端點的用戶端可以查詢。</span><span class="sxs-lookup"><span data-stu-id="03ba7-124">In this tutorial, you will create a simple OData endpoint that clients can query.</span></span> <span data-ttu-id="03ba7-125">您也會建立 C# 用戶端端點。</span><span class="sxs-lookup"><span data-stu-id="03ba7-125">You will also create a C# client for the endpoint.</span></span> <span data-ttu-id="03ba7-126">完成本教學課程之後下, 一組教學課程示範如何新增更多的功能，包括實體關聯性，動作，並選取 展開 / $ $。</span><span class="sxs-lookup"><span data-stu-id="03ba7-126">After you complete this tutorial, the next set of tutorials show how to add more functionality, including entity relations, actions, and $expand/$select.</span></span>

- [<span data-ttu-id="03ba7-127">建立 Visual Studio 專案</span><span class="sxs-lookup"><span data-stu-id="03ba7-127">Create the Visual Studio Project</span></span>](#create-project)
- [<span data-ttu-id="03ba7-128">加入實體模型</span><span class="sxs-lookup"><span data-stu-id="03ba7-128">Add an Entity Model</span></span>](#add-model)
- [<span data-ttu-id="03ba7-129">加入 OData 控制器</span><span class="sxs-lookup"><span data-stu-id="03ba7-129">Add an OData Controller</span></span>](#add-controller)
- [<span data-ttu-id="03ba7-130">新增 EDM 和路由</span><span class="sxs-lookup"><span data-stu-id="03ba7-130">Add the EDM and Route</span></span>](#edm)
- [<span data-ttu-id="03ba7-131">（選擇性） 資料庫中植入</span><span class="sxs-lookup"><span data-stu-id="03ba7-131">Seed the Database (Optional)</span></span>](#seed-db)
- [<span data-ttu-id="03ba7-132">瀏覽 OData 端點</span><span class="sxs-lookup"><span data-stu-id="03ba7-132">Exploring the OData Endpoint</span></span>](#explore)
- [<span data-ttu-id="03ba7-133">OData 序列化格式</span><span class="sxs-lookup"><span data-stu-id="03ba7-133">OData Serialization Formats</span></span>](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a><span data-ttu-id="03ba7-134">建立 Visual Studio 專案</span><span class="sxs-lookup"><span data-stu-id="03ba7-134">Create the Visual Studio Project</span></span>

<span data-ttu-id="03ba7-135">在本教學課程中，您將建立支援基本的 CRUD 作業的 OData 端點。</span><span class="sxs-lookup"><span data-stu-id="03ba7-135">In this tutorial, you will create an OData endpoint that supports basic CRUD operations.</span></span> <span data-ttu-id="03ba7-136">端點會公開單一資源，產品清單。</span><span class="sxs-lookup"><span data-stu-id="03ba7-136">The endpoint will expose a single resource, a list of products.</span></span> <span data-ttu-id="03ba7-137">稍後的教學課程會新增更多功能。</span><span class="sxs-lookup"><span data-stu-id="03ba7-137">Later tutorials will add more features.</span></span>

<span data-ttu-id="03ba7-138">啟動 Visual Studio，然後選取**新的專案**從 [開始] 頁面。</span><span class="sxs-lookup"><span data-stu-id="03ba7-138">Start Visual Studio and select **New Project** from the Start page.</span></span> <span data-ttu-id="03ba7-139">或從**檔案**功能表上，選取**新增**，然後**專案**。</span><span class="sxs-lookup"><span data-stu-id="03ba7-139">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="03ba7-140">在 **範本**窗格中，選取**已安裝的範本**並展開 Visual C# 節點。</span><span class="sxs-lookup"><span data-stu-id="03ba7-140">In the **Templates** pane, select **Installed Templates** and expand the Visual C# node.</span></span> <span data-ttu-id="03ba7-141">底下**Visual C#**，選取**Web**。</span><span class="sxs-lookup"><span data-stu-id="03ba7-141">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="03ba7-142">選取  **ASP.NET Web 應用程式**範本。</span><span class="sxs-lookup"><span data-stu-id="03ba7-142">Select **the ASP.NET Web Application** template.</span></span>

![](creating-an-odata-endpoint/_static/image1.png)

<span data-ttu-id="03ba7-143">在 **新的 ASP.NET 專案**對話方塊中，選取**空白**範本。</span><span class="sxs-lookup"><span data-stu-id="03ba7-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="03ba7-144">在下&quot;新增的資料夾和核心參考...&quot;，檢查**Web API**。</span><span class="sxs-lookup"><span data-stu-id="03ba7-144">Under &quot;Add folders and core references for...&quot;, check **Web API**.</span></span> <span data-ttu-id="03ba7-145">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="03ba7-145">Click **OK**.</span></span>

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a><span data-ttu-id="03ba7-146">加入實體模型</span><span class="sxs-lookup"><span data-stu-id="03ba7-146">Add an Entity Model</span></span>

<span data-ttu-id="03ba7-147">「模型」是代表應用程式中資料的物件。</span><span class="sxs-lookup"><span data-stu-id="03ba7-147">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="03ba7-148">本教學課程中，我們需要表示一項產品的模型。</span><span class="sxs-lookup"><span data-stu-id="03ba7-148">For this tutorial, we need a model that represents a product.</span></span> <span data-ttu-id="03ba7-149">模型會對應至我們的 OData 實體類型。</span><span class="sxs-lookup"><span data-stu-id="03ba7-149">The model corresponds to our OData entity type.</span></span>

<span data-ttu-id="03ba7-150">在 [方案總管] 中，以滑鼠右鍵按一下 [模型] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="03ba7-150">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="03ba7-151">從操作功能表中，選取**新增**然後選取**類別**。</span><span class="sxs-lookup"><span data-stu-id="03ba7-151">From the context menu, select **Add** then select **Class**.</span></span>

![](creating-an-odata-endpoint/_static/image3.png)

<span data-ttu-id="03ba7-152">在 [**加入新**項目] 對話方塊，將類別命名為&quot;產品&quot;。</span><span class="sxs-lookup"><span data-stu-id="03ba7-152">In the **Add New** Item dialog, name the class &quot;Product&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="03ba7-153">依照慣例，模型類別位於 Models 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="03ba7-153">By convention, model classes are placed in the Models folder.</span></span> <span data-ttu-id="03ba7-154">您不需要遵循此慣例，在您自己的專案中，但我們將在本教學課程中使用它。</span><span class="sxs-lookup"><span data-stu-id="03ba7-154">You don't have to follow this convention in your own projects, but we'll use it for this tutorial.</span></span>


<span data-ttu-id="03ba7-155">在 Product.cs 檔案中，新增下列類別定義：</span><span class="sxs-lookup"><span data-stu-id="03ba7-155">In the Product.cs file, add the following class definition:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

<span data-ttu-id="03ba7-156">ID 屬性將會是實體索引鍵。</span><span class="sxs-lookup"><span data-stu-id="03ba7-156">The ID property will be the entity key.</span></span> <span data-ttu-id="03ba7-157">用戶端可以查詢產品識別碼。</span><span class="sxs-lookup"><span data-stu-id="03ba7-157">Clients can query products by ID.</span></span> <span data-ttu-id="03ba7-158">這個欄位也會在後端資料庫中的主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="03ba7-158">This field would also be the primary key in the back-end database.</span></span>

<span data-ttu-id="03ba7-159">現在就建置專案。</span><span class="sxs-lookup"><span data-stu-id="03ba7-159">Build the project now.</span></span> <span data-ttu-id="03ba7-160">在下一個步驟中，我們將使用一些使用反映來尋找產品類型的 Visual Studio scaffolding。</span><span class="sxs-lookup"><span data-stu-id="03ba7-160">In the next step, we'll use some Visual Studio scaffolding that uses reflection to find the Product type.</span></span>

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a><span data-ttu-id="03ba7-161">加入 OData 控制器</span><span class="sxs-lookup"><span data-stu-id="03ba7-161">Add an OData Controller</span></span>

<span data-ttu-id="03ba7-162">A*控制器*是處理 HTTP 要求的類別。</span><span class="sxs-lookup"><span data-stu-id="03ba7-162">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="03ba7-163">您可以定義每個實體集在您的 OData 服務的另一個控制站。</span><span class="sxs-lookup"><span data-stu-id="03ba7-163">You define a separate controller for each entity set in you OData service.</span></span> <span data-ttu-id="03ba7-164">在本教學課程中，我們將建立單一控制器。</span><span class="sxs-lookup"><span data-stu-id="03ba7-164">In this tutorial, we'll create a single controller.</span></span>

<span data-ttu-id="03ba7-165">在 [方案總管] 中，以滑鼠右鍵按一下 [控制器] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="03ba7-165">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="03ba7-166">選取 **新增**，然後選取**控制站**。</span><span class="sxs-lookup"><span data-stu-id="03ba7-166">Select **Add** and then select **Controller**.</span></span>

![](creating-an-odata-endpoint/_static/image5.png)

<span data-ttu-id="03ba7-167">在 **新增 Scaffold**對話方塊中，選取&quot;Web API 2 OData 控制器與動作，使用 Entity Framework&quot;。</span><span class="sxs-lookup"><span data-stu-id="03ba7-167">In the **Add Scaffold** dialog, select &quot;Web API 2 OData Controller with actions, using Entity Framework&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image6.png)

<span data-ttu-id="03ba7-168">在 [**新增控制器**] 對話方塊中，將控制器"ProductsController"命名。</span><span class="sxs-lookup"><span data-stu-id="03ba7-168">In the **Add Controller** dialog, name the controller "ProductsController".</span></span> <span data-ttu-id="03ba7-169">選取 &quot;使用非同步控制器動作&quot;核取方塊。</span><span class="sxs-lookup"><span data-stu-id="03ba7-169">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="03ba7-170">在 **模型**下拉式清單中選取產品類別。</span><span class="sxs-lookup"><span data-stu-id="03ba7-170">In the **Model** drop-down list, select the Product class.</span></span>

![](creating-an-odata-endpoint/_static/image7.png)

<span data-ttu-id="03ba7-171">按一下 [**新資料內容...** ] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="03ba7-171">Click the **New data context...** button.</span></span> <span data-ttu-id="03ba7-172">保留的資料內容型別的預設名稱，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="03ba7-172">Leave the default name for the data context type, and click **Add**.</span></span>

![](creating-an-odata-endpoint/_static/image8.png)

<span data-ttu-id="03ba7-173">在 加入控制器的 新增控制器 對話方塊中，按一下 新增。</span><span class="sxs-lookup"><span data-stu-id="03ba7-173">Click Add in the Add Controller dialog to add the controller.</span></span>

![](creating-an-odata-endpoint/_static/image9.png)

<span data-ttu-id="03ba7-174">注意： 是否您收到錯誤訊息，指出&quot;取得類型時發生錯誤...&quot;，請確定您新增的產品類別之後，內建的 Visual Studio 專案。</span><span class="sxs-lookup"><span data-stu-id="03ba7-174">Note: If you get an error message that says &quot;There was an error getting the type...&quot;, make sure that you built the Visual Studio project after you added the Product class.</span></span> <span data-ttu-id="03ba7-175">Scaffolding 會使用反映來尋找類別。</span><span class="sxs-lookup"><span data-stu-id="03ba7-175">The scaffolding uses reflection to find the class.</span></span>

![](creating-an-odata-endpoint/_static/image10.png)

<span data-ttu-id="03ba7-176">Scaffolding 會將兩個程式碼檔案加入專案：</span><span class="sxs-lookup"><span data-stu-id="03ba7-176">The scaffolding adds two code files to the project:</span></span>

- <span data-ttu-id="03ba7-177">Products.cs 定義 Web API 控制器實作 OData 端點。</span><span class="sxs-lookup"><span data-stu-id="03ba7-177">Products.cs defines the Web API controller that implements the OData endpoint.</span></span>
- <span data-ttu-id="03ba7-178">ProductServiceContext.cs 提供方法來查詢基礎資料庫，並使用 Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="03ba7-178">ProductServiceContext.cs provides methods to query the underlying database, using Entity Framework.</span></span>

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a><span data-ttu-id="03ba7-179">新增 EDM 和路由</span><span class="sxs-lookup"><span data-stu-id="03ba7-179">Add the EDM and Route</span></span>

<span data-ttu-id="03ba7-180">在 方案總管 中，展開 應用程式\_啟動資料夾，然後開啟名為 WebApiConfig.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="03ba7-180">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="03ba7-181">這個類別會包含 Web api 組態程式碼。</span><span class="sxs-lookup"><span data-stu-id="03ba7-181">This class holds configuration code for Web API.</span></span> <span data-ttu-id="03ba7-182">您可以將此程式碼取代為下列：</span><span class="sxs-lookup"><span data-stu-id="03ba7-182">Replace this code with the following:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

<span data-ttu-id="03ba7-183">此程式碼會執行兩件事：</span><span class="sxs-lookup"><span data-stu-id="03ba7-183">This code does two things:</span></span>

- <span data-ttu-id="03ba7-184">建立 OData 端點的 Entity Data Model (EDM)。</span><span class="sxs-lookup"><span data-stu-id="03ba7-184">Creates an Entity Data Model (EDM) for the OData endpoint.</span></span>
- <span data-ttu-id="03ba7-185">新增端點的路由。</span><span class="sxs-lookup"><span data-stu-id="03ba7-185">Adds a route for the endpoint.</span></span>

<span data-ttu-id="03ba7-186">EDM 是資料的抽象模型。</span><span class="sxs-lookup"><span data-stu-id="03ba7-186">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="03ba7-187">EDM 用來建立中繼資料文件，並定義服務的 Uri。</span><span class="sxs-lookup"><span data-stu-id="03ba7-187">The EDM is used to create the metadata document and define the URIs for the service.</span></span> <span data-ttu-id="03ba7-188">**ODataConventionModelBuilder**會使用一組預設命名慣例 EDM 建立 EDM。</span><span class="sxs-lookup"><span data-stu-id="03ba7-188">The **ODataConventionModelBuilder** creates an EDM by using a set of default naming conventions EDM.</span></span> <span data-ttu-id="03ba7-189">這個方法需要最少的程式碼。</span><span class="sxs-lookup"><span data-stu-id="03ba7-189">This approach requires the least code.</span></span> <span data-ttu-id="03ba7-190">如果您想要更充分掌控 EDM 中，您可以使用**ODataModelBuilder**藉由新增屬性、 金鑰和導覽屬性明確地建立 EDM 的類別。</span><span class="sxs-lookup"><span data-stu-id="03ba7-190">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="03ba7-191">**EntitySet**方法會將設定為 EDM 實體：</span><span class="sxs-lookup"><span data-stu-id="03ba7-191">The **EntitySet** method adds an entity set to the EDM:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

<span data-ttu-id="03ba7-192">字串 「 產品 」 會定義實體集的名稱。</span><span class="sxs-lookup"><span data-stu-id="03ba7-192">The string "Products" defines the name of the entity set.</span></span> <span data-ttu-id="03ba7-193">控制站的名稱必須符合實體集的名稱。</span><span class="sxs-lookup"><span data-stu-id="03ba7-193">The name of the controller must match the name of the entity set.</span></span> <span data-ttu-id="03ba7-194">在本教學課程中，實體集名為 「 產品 」 和控制站命名為`ProductsController`。</span><span class="sxs-lookup"><span data-stu-id="03ba7-194">In this tutorial, the entity set is named "Products" and the controller is named `ProductsController`.</span></span> <span data-ttu-id="03ba7-195">如果您名為的實體集 」 ProductSet 」，您會將控制器命名`ProductSetController`。</span><span class="sxs-lookup"><span data-stu-id="03ba7-195">If you named the entity set "ProductSet", you would name the controller `ProductSetController`.</span></span> <span data-ttu-id="03ba7-196">請注意，端點可以有多個實體集。</span><span class="sxs-lookup"><span data-stu-id="03ba7-196">Note that an endpoint can have multiple entity sets.</span></span> <span data-ttu-id="03ba7-197">呼叫**EntitySet&lt;T&gt;** 每個實體集，並接著定義相對應的控制項。</span><span class="sxs-lookup"><span data-stu-id="03ba7-197">Call **EntitySet&lt;T&gt;** for each entity set, and then define a corresponding controller.</span></span>

<span data-ttu-id="03ba7-198">**MapODataRoute**方法會將 OData 端點的路由。</span><span class="sxs-lookup"><span data-stu-id="03ba7-198">The **MapODataRoute** method adds a route for the OData endpoint.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

<span data-ttu-id="03ba7-199">第一個參數是路由的易記名稱。</span><span class="sxs-lookup"><span data-stu-id="03ba7-199">The first parameter is a friendly name for the route.</span></span> <span data-ttu-id="03ba7-200">您的服務的用戶端看不到此名稱。</span><span class="sxs-lookup"><span data-stu-id="03ba7-200">Clients of your service do not see this name.</span></span> <span data-ttu-id="03ba7-201">第二個參數是端點的 URI 前置詞。</span><span class="sxs-lookup"><span data-stu-id="03ba7-201">The second parameter is the URI prefix for the endpoint.</span></span> <span data-ttu-id="03ba7-202">指定此程式碼，Products 實體集的 URI 是 http://<em>hostname</em>  /odata/產品。</span><span class="sxs-lookup"><span data-stu-id="03ba7-202">Given this code, the URI for the Products entity set is http://<em>hostname</em>/odata/Products.</span></span> <span data-ttu-id="03ba7-203">您的應用程式可以有一個以上的 OData 端點。</span><span class="sxs-lookup"><span data-stu-id="03ba7-203">Your application can have more than one OData endpoint.</span></span> <span data-ttu-id="03ba7-204">針對每個端點，呼叫<strong>MapODataRoute</strong>並提供唯一的路由名稱和唯一的 URI 前置詞。</span><span class="sxs-lookup"><span data-stu-id="03ba7-204">For each endpoint, call <strong>MapODataRoute</strong> and provide a unique route name and a unique URI prefix.</span></span>

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a><span data-ttu-id="03ba7-205">（選擇性） 資料庫中植入</span><span class="sxs-lookup"><span data-stu-id="03ba7-205">Seed the Database (Optional)</span></span>

<span data-ttu-id="03ba7-206">在此步驟中，您將使用 Entity Framework 來植入部分測試資料的資料庫。</span><span class="sxs-lookup"><span data-stu-id="03ba7-206">In this step, you will use Entity Framework to seed the database with some test data.</span></span> <span data-ttu-id="03ba7-207">這個步驟是選擇性的但它可讓您立即測試您的 OData 端點。</span><span class="sxs-lookup"><span data-stu-id="03ba7-207">This step is optional, but it lets you test out your OData endpoint right away.</span></span>

<span data-ttu-id="03ba7-208">從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="03ba7-208">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="03ba7-209">在 [套件管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="03ba7-209">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

<span data-ttu-id="03ba7-210">這會將新增名為移轉和名為 Configuration.cs 程式碼檔案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="03ba7-210">This adds a folder named Migrations and a code file named Configuration.cs.</span></span>

![](creating-an-odata-endpoint/_static/image12.png)

<span data-ttu-id="03ba7-211">開啟此檔案並新增下列程式碼`Configuration.Seed`方法。</span><span class="sxs-lookup"><span data-stu-id="03ba7-211">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

<span data-ttu-id="03ba7-212">在 [套件管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="03ba7-212">In the Package Manager Console Window, enter the following commands:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

<span data-ttu-id="03ba7-213">這些命令會產生程式碼會建立資料庫，然後執行該程式碼。</span><span class="sxs-lookup"><span data-stu-id="03ba7-213">These commands generate code that creates the database, and then executes that code.</span></span>

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a><span data-ttu-id="03ba7-214">瀏覽 OData 端點</span><span class="sxs-lookup"><span data-stu-id="03ba7-214">Exploring the OData Endpoint</span></span>

<span data-ttu-id="03ba7-215">在本節中，我們將使用[Fiddler Web 偵錯 Proxy](http://www.fiddler2.com)將要求傳送至端點並檢查回應訊息。</span><span class="sxs-lookup"><span data-stu-id="03ba7-215">In this section, we'll use the [Fiddler Web Debugging Proxy](http://www.fiddler2.com) to send requests to the endpoint and examine the response messages.</span></span> <span data-ttu-id="03ba7-216">這有助於您了解 OData 端點的功能。</span><span class="sxs-lookup"><span data-stu-id="03ba7-216">This will help you to understand the capabilities of an OData endpoint.</span></span>

<span data-ttu-id="03ba7-217">在 Visual Studio 中，按下 f5 鍵啟動偵錯。</span><span class="sxs-lookup"><span data-stu-id="03ba7-217">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="03ba7-218">根據預設，Visual Studio 會開啟您的瀏覽器`http://localhost:*port*`，其中*連接埠*專案設定中所設定的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="03ba7-218">By default, Visual Studio opens your browser to `http://localhost:*port*`, where *port* is the port number configured in the project settings.</span></span>

<span data-ttu-id="03ba7-219">您可以變更專案設定中的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="03ba7-219">You can change the port number in the project settings.</span></span> <span data-ttu-id="03ba7-220">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="03ba7-220">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="03ba7-221">在 [屬性] 視窗中，選取**Web**。</span><span class="sxs-lookup"><span data-stu-id="03ba7-221">In the properties window, select **Web**.</span></span> <span data-ttu-id="03ba7-222">輸入下方的連接埠號碼**專案 Url**。</span><span class="sxs-lookup"><span data-stu-id="03ba7-222">Enter the port number under **Project Url**.</span></span>

### <a name="service-document"></a><span data-ttu-id="03ba7-223">服務文件</span><span class="sxs-lookup"><span data-stu-id="03ba7-223">Service Document</span></span>

<span data-ttu-id="03ba7-224">*服務文件*包含 OData 端點的實體集的清單。</span><span class="sxs-lookup"><span data-stu-id="03ba7-224">The *service document* contains a list of the entity sets for the OData endpoint.</span></span> <span data-ttu-id="03ba7-225">若要取得服務文件，GET 要求傳送至服務根 URI。</span><span class="sxs-lookup"><span data-stu-id="03ba7-225">To get the service document, send a GET request to the root URI of the service.</span></span>

<span data-ttu-id="03ba7-226">使用 Fiddler，輸入下列 URI 中的**Composer**索引標籤： `http://localhost:port/odata/`，其中*連接埠*的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="03ba7-226">Using Fiddler, enter the following URI in the **Composer** tab: `http://localhost:port/odata/`, where *port* is the port number.</span></span>

![](creating-an-odata-endpoint/_static/image13.png)

<span data-ttu-id="03ba7-227">按一下 [ **Execute** ] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="03ba7-227">Click the **Execute** button.</span></span> <span data-ttu-id="03ba7-228">Fiddler 會將 HTTP GET 要求傳送至您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="03ba7-228">Fiddler sends an HTTP GET request to your application.</span></span> <span data-ttu-id="03ba7-229">您應該會看到 Web 工作階段清單中的回應。</span><span class="sxs-lookup"><span data-stu-id="03ba7-229">You should see the response in the Web Sessions list.</span></span> <span data-ttu-id="03ba7-230">如果一切運作，狀態碼將會是 200。</span><span class="sxs-lookup"><span data-stu-id="03ba7-230">If everything is working, the status code will be 200.</span></span>

![](creating-an-odata-endpoint/_static/image14.png)

<span data-ttu-id="03ba7-231">按兩下以查看詳細資料，回應訊息偵測器索引標籤中的 Web 工作階段清單中的回應。</span><span class="sxs-lookup"><span data-stu-id="03ba7-231">Double-click the response in the Web Sessions list to see the details of the response message in the Inspectors tab.</span></span>

![](creating-an-odata-endpoint/_static/image15.png)

<span data-ttu-id="03ba7-232">未經處理的 HTTP 回應訊息看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="03ba7-232">The raw HTTP response message should look similar to the following:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

<span data-ttu-id="03ba7-233">根據預設，Web API 會傳回 AtomPub 格式的服務文件。</span><span class="sxs-lookup"><span data-stu-id="03ba7-233">By default, Web API returns the service document in AtomPub format.</span></span> <span data-ttu-id="03ba7-234">若要要求 JSON，請將下列標頭加入 HTTP 要求：</span><span class="sxs-lookup"><span data-stu-id="03ba7-234">To request JSON, add the following header to the HTTP request:</span></span>

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

<span data-ttu-id="03ba7-235">現在的 HTTP 回應會包含 JSON 承載：</span><span class="sxs-lookup"><span data-stu-id="03ba7-235">Now the HTTP response contains a JSON payload:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a><span data-ttu-id="03ba7-236">服務中繼資料文件</span><span class="sxs-lookup"><span data-stu-id="03ba7-236">Service Metadata Document</span></span>

<span data-ttu-id="03ba7-237">*服務的中繼資料文件*描述使用稱為概念結構定義語言 (CSDL) 的 XML 語言的服務資料模型。</span><span class="sxs-lookup"><span data-stu-id="03ba7-237">The *service metadata document* describes the data model of the service, using an XML language called the Conceptual Schema Definition Language (CSDL).</span></span> <span data-ttu-id="03ba7-238">中繼資料文件會在服務中，會顯示資料的結構，而且可用來產生用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="03ba7-238">The metadata document shows the structure of the data in the service, and can be used to generate client code.</span></span>

<span data-ttu-id="03ba7-239">若要取得中繼資料文件，將 GET 要求傳送至`http://localhost:port/odata/$metadata`。</span><span class="sxs-lookup"><span data-stu-id="03ba7-239">To get the metadata document, send a GET request to `http://localhost:port/odata/$metadata`.</span></span> <span data-ttu-id="03ba7-240">以下是本教學課程中所示的端點的中繼資料。</span><span class="sxs-lookup"><span data-stu-id="03ba7-240">Here is the metadata for the endpoint shown in this tutorial.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a><span data-ttu-id="03ba7-241">實體集</span><span class="sxs-lookup"><span data-stu-id="03ba7-241">Entity Set</span></span>

<span data-ttu-id="03ba7-242">若要取得的產品實體集，將 GET 要求傳送至`http://localhost:port/odata/Products`。</span><span class="sxs-lookup"><span data-stu-id="03ba7-242">To get the Products entity set, send a GET request to `http://localhost:port/odata/Products`.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a><span data-ttu-id="03ba7-243">實體</span><span class="sxs-lookup"><span data-stu-id="03ba7-243">Entity</span></span>

<span data-ttu-id="03ba7-244">若要取得個別產品，將 GET 要求傳送至`http://localhost:port/odata/Products(1)`，其中"1"是產品識別碼。</span><span class="sxs-lookup"><span data-stu-id="03ba7-244">To get an individual product, send a GET request to `http://localhost:port/odata/Products(1)`, where "1" is the product ID.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a><span data-ttu-id="03ba7-245">OData 序列化格式</span><span class="sxs-lookup"><span data-stu-id="03ba7-245">OData Serialization Formats</span></span>

<span data-ttu-id="03ba7-246">OData 支援數種序列化格式：</span><span class="sxs-lookup"><span data-stu-id="03ba7-246">OData supports several serialization formats:</span></span>

- <span data-ttu-id="03ba7-247">Atom Pub (XML)</span><span class="sxs-lookup"><span data-stu-id="03ba7-247">Atom Pub (XML)</span></span>
- <span data-ttu-id="03ba7-248">JSON 的"light"（在 OData v3 中引進）</span><span class="sxs-lookup"><span data-stu-id="03ba7-248">JSON "light" (introduced in OData v3)</span></span>
- <span data-ttu-id="03ba7-249">JSON 的"verbose"(OData v2)</span><span class="sxs-lookup"><span data-stu-id="03ba7-249">JSON "verbose" (OData v2)</span></span>

<span data-ttu-id="03ba7-250">根據預設，Web API 會使用 AtomPubJSON"light"格式。</span><span class="sxs-lookup"><span data-stu-id="03ba7-250">By default, Web API uses AtomPubJSON "light" format.</span></span> 

<span data-ttu-id="03ba7-251">若要取得 AtomPub 格式，請設定 Accept 標頭為"application/atom + xml"。</span><span class="sxs-lookup"><span data-stu-id="03ba7-251">To get AtomPub format, set the Accept header to "application/atom+xml".</span></span> <span data-ttu-id="03ba7-252">以下是範例回應主體：</span><span class="sxs-lookup"><span data-stu-id="03ba7-252">Here is an example response body:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

<span data-ttu-id="03ba7-253">您可以看到 Atom 格式的一項明顯的缺點： 它是更多詳細資訊，JSON light 比。</span><span class="sxs-lookup"><span data-stu-id="03ba7-253">You can see one obvious disadvantage of the Atom format: It's a lot more verbose than the JSON light.</span></span> <span data-ttu-id="03ba7-254">不過，如果您有了解 AtomPub 的用戶端時，用戶端可能會透過 JSON 偏好該格式。</span><span class="sxs-lookup"><span data-stu-id="03ba7-254">However, if you have a client that understands AtomPub, the client might prefer that format over JSON.</span></span>

<span data-ttu-id="03ba7-255">以下是實體的相同的 JSON light 版本：</span><span class="sxs-lookup"><span data-stu-id="03ba7-255">Here is the JSON light version of the same entity:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

<span data-ttu-id="03ba7-256">OData 通訊協定第 3 版中引進了 JSON light 的格式。</span><span class="sxs-lookup"><span data-stu-id="03ba7-256">The JSON light format was introduced in version 3 of the OData protocol.</span></span> <span data-ttu-id="03ba7-257">回溯相容性，用戶端可以要求較舊的"verbose"JSON 格式。</span><span class="sxs-lookup"><span data-stu-id="03ba7-257">For backward compatibility, a client can request the older "verbose" JSON format.</span></span> <span data-ttu-id="03ba7-258">若要要求詳細資訊 JSON，請設定 Accept 標頭為`application/json;odata=verbose`。</span><span class="sxs-lookup"><span data-stu-id="03ba7-258">To request verbose JSON, set the Accept header to `application/json;odata=verbose`.</span></span> <span data-ttu-id="03ba7-259">以下是詳細的版本：</span><span class="sxs-lookup"><span data-stu-id="03ba7-259">Here is the verbose version:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

<span data-ttu-id="03ba7-260">此格式會傳達在回應主體中，整個工作階段可以加入可觀的額外負荷的更多中繼資料。</span><span class="sxs-lookup"><span data-stu-id="03ba7-260">This format conveys more metadata in the response body, which can add considerable overhead over an entire session.</span></span> <span data-ttu-id="03ba7-261">此外，它會加入一層間接層包裝在一個名為"d"屬性中的物件。</span><span class="sxs-lookup"><span data-stu-id="03ba7-261">Also, it adds a level of indirection by wrapping the object in a property named "d".</span></span>

## <a name="next-steps"></a><span data-ttu-id="03ba7-262">後續步驟</span><span class="sxs-lookup"><span data-stu-id="03ba7-262">Next Steps</span></span>

- [<span data-ttu-id="03ba7-263">新增實體關聯性</span><span class="sxs-lookup"><span data-stu-id="03ba7-263">Add Entity Relations</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="03ba7-264">加入 OData 動作</span><span class="sxs-lookup"><span data-stu-id="03ba7-264">Add OData Actions</span></span>](odata-actions.md)
- [<span data-ttu-id="03ba7-265">從.NET 用戶端呼叫 OData 服務</span><span class="sxs-lookup"><span data-stu-id="03ba7-265">Call the OData Service From a .NET Client</span></span>](calling-an-odata-service-from-a-net-client.md)
