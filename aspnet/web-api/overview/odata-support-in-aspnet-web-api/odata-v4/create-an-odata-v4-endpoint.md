---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: "建立 ASP.NET Web API 2.2 使用 OData v4 端點 |Microsoft 文件"
author: MikeWasson
description: "開放式資料通訊協定 (OData) 是網站的資料存取通訊協定。 OData 提供統一的方式來查詢及管理透過 CRUD 作業的資料集..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/24/2014
ms.topic: article
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: a3f94818f9674b0e1e9a45b2a6cc9455edc79726
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a><span data-ttu-id="048a3-104">建立 ASP.NET Web API 2.2 使用 OData v4 端點</span><span class="sxs-lookup"><span data-stu-id="048a3-104">Create an OData v4 Endpoint Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="048a3-105">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="048a3-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="048a3-106">開放式資料通訊協定 (OData) 是網站的資料存取通訊協定。</span><span class="sxs-lookup"><span data-stu-id="048a3-106">The Open Data Protocol (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="048a3-107">OData 提供統一的方式來查詢與操作資料集，透過 CRUD 作業 （建立、 讀取、 更新和刪除）。</span><span class="sxs-lookup"><span data-stu-id="048a3-107">OData provides a uniform way to query and manipulate data sets through CRUD operations (create, read, update, and delete).</span></span>
> 
> <span data-ttu-id="048a3-108">ASP.NET Web 應用程式開發介面支援 v3 和 v4 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="048a3-108">ASP.NET Web API supports both v3 and v4 of the protocol.</span></span> <span data-ttu-id="048a3-109">您甚至可以執行的並行 v4 端點與 v3 端點。</span><span class="sxs-lookup"><span data-stu-id="048a3-109">You can even have a v4 endpoint that runs side-by-side with a v3 endpoint.</span></span>
> 
> <span data-ttu-id="048a3-110">本教學課程會示範如何建立支援 CRUD 作業的 OData v4 端點。</span><span class="sxs-lookup"><span data-stu-id="048a3-110">This tutorial shows how to create an OData v4 endpoint that supports CRUD operations.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="048a3-111">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="048a3-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="048a3-112">Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="048a3-112">Web API 2.2</span></span>
> - <span data-ttu-id="048a3-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="048a3-113">OData v4</span></span>
> - [<span data-ttu-id="048a3-114">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="048a3-114">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="048a3-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="048a3-115">Entity Framework 6</span></span>
> - <span data-ttu-id="048a3-116">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="048a3-116">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="048a3-117">教學課程版本</span><span class="sxs-lookup"><span data-stu-id="048a3-117">Tutorial versions</span></span>
> 
> <span data-ttu-id="048a3-118">OData 第 3 版，請參閱[建立 OData v3 端點](../odata-v3/creating-an-odata-endpoint.md)。</span><span class="sxs-lookup"><span data-stu-id="048a3-118">For the OData Version 3, see [Creating an OData v3 Endpoint](../odata-v3/creating-an-odata-endpoint.md).</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="048a3-119">建立 Visual Studio 專案</span><span class="sxs-lookup"><span data-stu-id="048a3-119">Create the Visual Studio Project</span></span>

<span data-ttu-id="048a3-120">在 Visual Studio 中，從**檔案**功能表上，選取**新增** &gt; **專案**。</span><span class="sxs-lookup"><span data-stu-id="048a3-120">In Visual Studio, from the **File** menu, select **New** &gt; **Project**.</span></span>

<span data-ttu-id="048a3-121">展開**已安裝** &gt; **範本** &gt; **Visual C#** &gt; **Web**，然後選取**ASP.NET Web 應用程式**範本。</span><span class="sxs-lookup"><span data-stu-id="048a3-121">Expand **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Web**, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="048a3-122">將專案命名&quot;ProductService&quot;。</span><span class="sxs-lookup"><span data-stu-id="048a3-122">Name the project &quot;ProductService&quot;.</span></span>

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

<span data-ttu-id="048a3-123">在**新專案**對話方塊中，選取**空**範本。</span><span class="sxs-lookup"><span data-stu-id="048a3-123">In the **New Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="048a3-124">在下&quot;加入資料夾和核心參考...&quot;，按一下  **Web API**。</span><span class="sxs-lookup"><span data-stu-id="048a3-124">Under &quot;Add folders and core references...&quot;, click **Web API**.</span></span> <span data-ttu-id="048a3-125">按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="048a3-125">Click **OK**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a><span data-ttu-id="048a3-126">OData 封裝安裝</span><span class="sxs-lookup"><span data-stu-id="048a3-126">Install the OData Packages</span></span>

<span data-ttu-id="048a3-127">從**工具**功能表上，選取**NuGet 套件管理員** &gt; **Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="048a3-127">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="048a3-128">在 [封裝管理員主控台] 視窗中，輸入：</span><span class="sxs-lookup"><span data-stu-id="048a3-128">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

<span data-ttu-id="048a3-129">此命令會安裝最新的 OData NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="048a3-129">This command installs the latest OData NuGet packages.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="048a3-130">將模型類別</span><span class="sxs-lookup"><span data-stu-id="048a3-130">Add a Model Class</span></span>

<span data-ttu-id="048a3-131">A*模型*是物件，代表應用程式中的資料實體。</span><span class="sxs-lookup"><span data-stu-id="048a3-131">A *model* is an object that represents a data entity in your application.</span></span>

<span data-ttu-id="048a3-132">在 [方案總管] 中，以滑鼠右鍵按一下 [模型] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="048a3-132">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="048a3-133">從內容功能表中，選取**新增** &gt; **類別**。</span><span class="sxs-lookup"><span data-stu-id="048a3-133">From the context menu, select **Add** &gt; **Class**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="048a3-134">依照慣例，模型類別都會放在 [模型] 資料夾中，但您不需要遵循這個慣例，在您自己的專案中。</span><span class="sxs-lookup"><span data-stu-id="048a3-134">By convention, model classes are placed in the Models folder, but you don't have to follow this convention in your own projects.</span></span>


<span data-ttu-id="048a3-135">將類別命名為 `Product` 。</span><span class="sxs-lookup"><span data-stu-id="048a3-135">Name the class `Product`.</span></span> <span data-ttu-id="048a3-136">在 Product.cs 檔案中，以下列內容取代未定案程式碼：</span><span class="sxs-lookup"><span data-stu-id="048a3-136">In the Product.cs file, replace the boilerplate code with the following:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

<span data-ttu-id="048a3-137">`Id`屬性是實體索引鍵。</span><span class="sxs-lookup"><span data-stu-id="048a3-137">The `Id` property is the entity key.</span></span> <span data-ttu-id="048a3-138">用戶端可以查詢的實體索引鍵。</span><span class="sxs-lookup"><span data-stu-id="048a3-138">Clients can query entities by key.</span></span> <span data-ttu-id="048a3-139">例如，若要取得識別碼 5 的產品，URI 是`/Products(5)`。</span><span class="sxs-lookup"><span data-stu-id="048a3-139">For example, to get the product with ID of 5, the URI is `/Products(5)`.</span></span> <span data-ttu-id="048a3-140">`Id`屬性也會在後端資料庫中的主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="048a3-140">The `Id` property will also be the primary key in the back-end database.</span></span>

## <a name="enable-entity-framework"></a><span data-ttu-id="048a3-141">啟用 Entity Framework</span><span class="sxs-lookup"><span data-stu-id="048a3-141">Enable Entity Framework</span></span>

<span data-ttu-id="048a3-142">此教學課程中，我們會使用 Entity Framework (EF) 程式碼第一次建立後端資料庫。</span><span class="sxs-lookup"><span data-stu-id="048a3-142">For this tutorial, we'll use Entity Framework (EF) Code First to create the back-end database.</span></span>

> [!NOTE]
> <span data-ttu-id="048a3-143">Web API OData 不需要 EF。</span><span class="sxs-lookup"><span data-stu-id="048a3-143">Web API OData does not require EF.</span></span> <span data-ttu-id="048a3-144">使用轉譯為資料庫實體模型的任何資料存取圖層。</span><span class="sxs-lookup"><span data-stu-id="048a3-144">Use any data-access layer that can translate database entities into models.</span></span>


<span data-ttu-id="048a3-145">首先，安裝 EF NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="048a3-145">First, install the NuGet package for EF.</span></span> <span data-ttu-id="048a3-146">從**工具**功能表上，選取**NuGet 套件管理員** &gt; **Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="048a3-146">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="048a3-147">在 [封裝管理員主控台] 視窗中，輸入：</span><span class="sxs-lookup"><span data-stu-id="048a3-147">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

<span data-ttu-id="048a3-148">開啟 Web.config 檔案中，並新增下列區段內**組態**項目，之後**c**項目。</span><span class="sxs-lookup"><span data-stu-id="048a3-148">Open the Web.config file, and add the following section inside the **configuration** element, after the **configSections** element.</span></span>

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

<span data-ttu-id="048a3-149">這個設定會新增 LocalDB 資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="048a3-149">This setting adds a connection string for a LocalDB database.</span></span> <span data-ttu-id="048a3-150">當您在本機執行應用程式時，會使用此資料庫。</span><span class="sxs-lookup"><span data-stu-id="048a3-150">This database will be used when you run the app locally.</span></span>

<span data-ttu-id="048a3-151">接下來，加入名為類別`ProductsContext`到 Models 資料夾：</span><span class="sxs-lookup"><span data-stu-id="048a3-151">Next, add a class named `ProductsContext` to the Models folder:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

<span data-ttu-id="048a3-152">在建構函式，`"name=ProductsContext"`提供連接字串的名稱。</span><span class="sxs-lookup"><span data-stu-id="048a3-152">In the constructor, `"name=ProductsContext"` gives the name of the connection string.</span></span>

## <a name="configure-the-odata-endpoint"></a><span data-ttu-id="048a3-153">設定 OData 端點</span><span class="sxs-lookup"><span data-stu-id="048a3-153">Configure the OData Endpoint</span></span>

<span data-ttu-id="048a3-154">開啟檔案的應用程式\_Start/WebApiConfig.cs。</span><span class="sxs-lookup"><span data-stu-id="048a3-154">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="048a3-155">加入下列**使用**陳述式：</span><span class="sxs-lookup"><span data-stu-id="048a3-155">Add the following **using** statements:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

<span data-ttu-id="048a3-156">然後加入下列程式碼加入**註冊**方法：</span><span class="sxs-lookup"><span data-stu-id="048a3-156">Then add the following code to the **Register** method:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

<span data-ttu-id="048a3-157">此程式碼會執行兩件事：</span><span class="sxs-lookup"><span data-stu-id="048a3-157">This code does two things:</span></span>

- <span data-ttu-id="048a3-158">建立實體資料模型 (EDM)。</span><span class="sxs-lookup"><span data-stu-id="048a3-158">Creates an Entity Data Model (EDM).</span></span>
- <span data-ttu-id="048a3-159">新增一個路由。</span><span class="sxs-lookup"><span data-stu-id="048a3-159">Adds a route.</span></span>

<span data-ttu-id="048a3-160">EDM 是抽象的資料模型。</span><span class="sxs-lookup"><span data-stu-id="048a3-160">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="048a3-161">EDM 用來建立服務的中繼資料文件。</span><span class="sxs-lookup"><span data-stu-id="048a3-161">The EDM is used to create the service metadata document.</span></span> <span data-ttu-id="048a3-162">**ODataConventionModelBuilder**類別會使用預設命名慣例來建立 EDM。</span><span class="sxs-lookup"><span data-stu-id="048a3-162">The **ODataConventionModelBuilder** class creates an EDM by using default naming conventions.</span></span> <span data-ttu-id="048a3-163">這個方法需要最少的程式碼。</span><span class="sxs-lookup"><span data-stu-id="048a3-163">This approach requires the least code.</span></span> <span data-ttu-id="048a3-164">如果您想要更充分掌控 EDM，您可以使用**ODataModelBuilder**類別來建立 EDM 藉由明確地新增內容、 金鑰和導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="048a3-164">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="048a3-165">A*路由*告知如何將 HTTP 要求傳送至端點的 Web API。</span><span class="sxs-lookup"><span data-stu-id="048a3-165">A *route* tells Web API how to route HTTP requests to the endpoint.</span></span> <span data-ttu-id="048a3-166">若要建立的 OData v4 路由，請呼叫**MapODataServiceRoute**擴充方法。</span><span class="sxs-lookup"><span data-stu-id="048a3-166">To create an OData v4 route, call the **MapODataServiceRoute** extension method.</span></span>

<span data-ttu-id="048a3-167">如果您的應用程式有多個 OData 端點，請針對每個建立個別的路由。</span><span class="sxs-lookup"><span data-stu-id="048a3-167">If your application has multiple OData endpoints, create a separate route for each.</span></span> <span data-ttu-id="048a3-168">唯一的路由名稱和前置詞，讓每一個路由。</span><span class="sxs-lookup"><span data-stu-id="048a3-168">Give each route a unique route name and prefix.</span></span>

## <a name="add-the-odata-controller"></a><span data-ttu-id="048a3-169">加入 OData 控制器</span><span class="sxs-lookup"><span data-stu-id="048a3-169">Add the OData Controller</span></span>

<span data-ttu-id="048a3-170">A*控制器*是用來處理 HTTP 要求的類別。</span><span class="sxs-lookup"><span data-stu-id="048a3-170">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="048a3-171">您建立個別的控制站，在您的 OData 服務中設定的每個實體。</span><span class="sxs-lookup"><span data-stu-id="048a3-171">You create a separate controller for each entity set in your OData service.</span></span> <span data-ttu-id="048a3-172">在此教學課程中，您會建立一個控制站，`Product`實體。</span><span class="sxs-lookup"><span data-stu-id="048a3-172">In this tutorial, you will create one controller, for the `Product` entity.</span></span>

<span data-ttu-id="048a3-173">在 [方案總管] 中，以滑鼠右鍵按一下 [控制器] 資料夾，然後選取**新增** &gt; **類別**。</span><span class="sxs-lookup"><span data-stu-id="048a3-173">In Solution Explorer, right-click the Controllers folder and select **Add** &gt; **Class**.</span></span> <span data-ttu-id="048a3-174">將類別命名為 `ProductsController` 。</span><span class="sxs-lookup"><span data-stu-id="048a3-174">Name the class `ProductsController`.</span></span>

> [!NOTE]
> <span data-ttu-id="048a3-175">針對 OData v3 會使用這個教學課程版本**加入控制器**scaffolding。</span><span class="sxs-lookup"><span data-stu-id="048a3-175">The version of this tutorial for OData v3 uses the **Add Controller** scaffolding.</span></span> <span data-ttu-id="048a3-176">目前沒有任何 OData v4 樣板。</span><span class="sxs-lookup"><span data-stu-id="048a3-176">Currently, there is no scaffolding for OData v4.</span></span>


<span data-ttu-id="048a3-177">以下列內容取代 ProductsController.cs 的未定案程式碼。</span><span class="sxs-lookup"><span data-stu-id="048a3-177">Replace the boilerplate code in ProductsController.cs with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

<span data-ttu-id="048a3-178">控制器會使用`ProductsContext`類別使用 EF 存取資料庫。</span><span class="sxs-lookup"><span data-stu-id="048a3-178">The controller uses the `ProductsContext` class to access the database using EF.</span></span> <span data-ttu-id="048a3-179">請注意，控制器會覆寫**處置**方法處置**ProductsContext**。</span><span class="sxs-lookup"><span data-stu-id="048a3-179">Notice that the controller overrides the **Dispose** method to dispose of the **ProductsContext**.</span></span>

<span data-ttu-id="048a3-180">這是控制器的起點。</span><span class="sxs-lookup"><span data-stu-id="048a3-180">This is the starting point for the controller.</span></span> <span data-ttu-id="048a3-181">接下來，我們要加入的所有 CRUD 作業的方法。</span><span class="sxs-lookup"><span data-stu-id="048a3-181">Next, we'll add methods for all of the CRUD operations.</span></span>

## <a name="querying-the-entity-set"></a><span data-ttu-id="048a3-182">查詢的實體集</span><span class="sxs-lookup"><span data-stu-id="048a3-182">Querying the Entity Set</span></span>

<span data-ttu-id="048a3-183">將下列方法加入`ProductsController`。</span><span class="sxs-lookup"><span data-stu-id="048a3-183">Add the following methods to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

<span data-ttu-id="048a3-184">無參數的版本`Get`方法會傳回整個產品集合。</span><span class="sxs-lookup"><span data-stu-id="048a3-184">The parameterless version of the `Get` method returns the entire Products collection.</span></span> <span data-ttu-id="048a3-185">`Get`方法*金鑰*參數由其索引鍵查閱產品 (在此情況下，`Id`屬性)。</span><span class="sxs-lookup"><span data-stu-id="048a3-185">The `Get` method with a *key* parameter looks up a product by its key (in this case, the `Id` property).</span></span>

<span data-ttu-id="048a3-186">**[EnableQuery]**屬性可讓用戶端使用查詢選項，例如 $filter、 $sort 和 $page 修改查詢。</span><span class="sxs-lookup"><span data-stu-id="048a3-186">The **[EnableQuery]** attribute enables clients to modify the query, by using query options such as $filter, $sort, and $page.</span></span> <span data-ttu-id="048a3-187">如需詳細資訊，請參閱[支援 OData 查詢選項](../supporting-odata-query-options.md)。</span><span class="sxs-lookup"><span data-stu-id="048a3-187">For more information, see [Supporting OData Query Options](../supporting-odata-query-options.md).</span></span>

## <a name="adding-an-entity-to-the-entity-set"></a><span data-ttu-id="048a3-188">將實體加入至實體集</span><span class="sxs-lookup"><span data-stu-id="048a3-188">Adding an Entity to the Entity Set</span></span>

<span data-ttu-id="048a3-189">若要啟用新的產品加入資料庫的用戶端，新增下列方法加入`ProductsController`。</span><span class="sxs-lookup"><span data-stu-id="048a3-189">To enable clients to add a new product to the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a><span data-ttu-id="048a3-190">更新實體</span><span class="sxs-lookup"><span data-stu-id="048a3-190">Updating an Entity</span></span>

<span data-ttu-id="048a3-191">OData 支援兩個不同的語意來更新實體，PATCH 和 PUT。</span><span class="sxs-lookup"><span data-stu-id="048a3-191">OData supports two different semantics for updating an entity, PATCH and PUT.</span></span>

- <span data-ttu-id="048a3-192">修補程式會執行部分更新。</span><span class="sxs-lookup"><span data-stu-id="048a3-192">PATCH performs a partial update.</span></span> <span data-ttu-id="048a3-193">用戶端指定要更新屬性。</span><span class="sxs-lookup"><span data-stu-id="048a3-193">The client specifies just the properties to update.</span></span>
- <span data-ttu-id="048a3-194">PUT 會取代整個實體。</span><span class="sxs-lookup"><span data-stu-id="048a3-194">PUT replaces the entire entity.</span></span>

<span data-ttu-id="048a3-195">PUT 的缺點是用戶端必須傳送的所有屬性的值中的實體，包括未變更的值。</span><span class="sxs-lookup"><span data-stu-id="048a3-195">The disadvantage of PUT is that the client must send values for all of the properties in the entity, including values that are not changing.</span></span> <span data-ttu-id="048a3-196">[OData 規格](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719)說明慣用修補程式。</span><span class="sxs-lookup"><span data-stu-id="048a3-196">The [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) states that PATCH is preferred.</span></span>

<span data-ttu-id="048a3-197">在任何情況下，以下是 PATCH 和 PUT 方法的程式碼：</span><span class="sxs-lookup"><span data-stu-id="048a3-197">In any case, here is the code for both PATCH and PUT methods:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

<span data-ttu-id="048a3-198">如果修補程式時，控制器會使用**差異&lt;T&gt;** 來追蹤變更的類型。</span><span class="sxs-lookup"><span data-stu-id="048a3-198">In the case of PATCH, the controller uses the **Delta&lt;T&gt;** type to track the changes.</span></span>

## <a name="deleting-an-entity"></a><span data-ttu-id="048a3-199">刪除實體</span><span class="sxs-lookup"><span data-stu-id="048a3-199">Deleting an Entity</span></span>

<span data-ttu-id="048a3-200">若要啟用用戶端從資料庫刪除產品，加入下列方法加入`ProductsController`。</span><span class="sxs-lookup"><span data-stu-id="048a3-200">To enable clients to delete a product from the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
