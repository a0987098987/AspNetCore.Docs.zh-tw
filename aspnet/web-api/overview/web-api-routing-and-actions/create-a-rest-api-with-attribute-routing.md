---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: 使用 ASP.NET Web API 2 中的屬性路由建立 REST API |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 18a44c280e6df1603837938d24d7d639d8c87cc2
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021413"
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="0a9f6-102">ASP.NET Web API 2 中的屬性路由建立 REST API</span><span class="sxs-lookup"><span data-stu-id="0a9f6-102">Create a REST API with Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="0a9f6-103">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0a9f6-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="0a9f6-104">Web API 2 支援新類型的路由，稱為*屬性路由*。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-104">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="0a9f6-105">屬性路由的一般概觀，請參閱 < [Web API 2 中的屬性路由](attribute-routing-in-web-api-2.md)。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-105">For a general overview of attribute routing, see [Attribute Routing in Web API 2](attribute-routing-in-web-api-2.md).</span></span> <span data-ttu-id="0a9f6-106">在本教學課程中，您將使用屬性路由建立 REST API 的書籍集合。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-106">In this tutorial, you will use attribute routing to create a REST API for a collection of books.</span></span> <span data-ttu-id="0a9f6-107">API 將會支援下列動作：</span><span class="sxs-lookup"><span data-stu-id="0a9f6-107">The API will support the following actions:</span></span>

| <span data-ttu-id="0a9f6-108">動作</span><span class="sxs-lookup"><span data-stu-id="0a9f6-108">Action</span></span> | <span data-ttu-id="0a9f6-109">範例 URI</span><span class="sxs-lookup"><span data-stu-id="0a9f6-109">Example URI</span></span> |
| --- | --- |
| <span data-ttu-id="0a9f6-110">取得所有書籍的清單。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-110">Get a list of all books.</span></span> | <span data-ttu-id="0a9f6-111">/ api/書籍</span><span class="sxs-lookup"><span data-stu-id="0a9f6-111">/api/books</span></span> |
| <span data-ttu-id="0a9f6-112">取得活頁簿的識別碼。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-112">Get a book by ID.</span></span> | <span data-ttu-id="0a9f6-113">/api/books/1</span><span class="sxs-lookup"><span data-stu-id="0a9f6-113">/api/books/1</span></span> |
| <span data-ttu-id="0a9f6-114">取得活頁簿的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-114">Get the details of a book.</span></span> | <span data-ttu-id="0a9f6-115">/api/books/1/details</span><span class="sxs-lookup"><span data-stu-id="0a9f6-115">/api/books/1/details</span></span> |
| <span data-ttu-id="0a9f6-116">取得內容類型的書籍清單。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-116">Get a list of books by genre.</span></span> | <span data-ttu-id="0a9f6-117">/api/books/fantasy</span><span class="sxs-lookup"><span data-stu-id="0a9f6-117">/api/books/fantasy</span></span> |
| <span data-ttu-id="0a9f6-118">取得發行日期的書籍清單。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-118">Get a list of books by publication date.</span></span> | <span data-ttu-id="0a9f6-119">/api/books/date/2013-02-16 /api/books/date/2013/02/16 （替代形式）</span><span class="sxs-lookup"><span data-stu-id="0a9f6-119">/api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form)</span></span> |
| <span data-ttu-id="0a9f6-120">取得特定作者的書籍清單。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-120">Get a list of books by a particular author.</span></span> | <span data-ttu-id="0a9f6-121">/api/authors/1/books</span><span class="sxs-lookup"><span data-stu-id="0a9f6-121">/api/authors/1/books</span></span> |

<span data-ttu-id="0a9f6-122">所有方法都是唯讀狀態 （HTTP GET 要求）。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-122">All methods are read-only (HTTP GET requests).</span></span>

<span data-ttu-id="0a9f6-123">資料層，我們將使用 Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-123">For the data layer, we'll use Entity Framework.</span></span> <span data-ttu-id="0a9f6-124">書籍記錄將會有下列欄位：</span><span class="sxs-lookup"><span data-stu-id="0a9f6-124">Book records will have the following fields:</span></span>

- <span data-ttu-id="0a9f6-125">識別碼</span><span class="sxs-lookup"><span data-stu-id="0a9f6-125">ID</span></span>
- <span data-ttu-id="0a9f6-126">標題</span><span class="sxs-lookup"><span data-stu-id="0a9f6-126">Title</span></span>
- <span data-ttu-id="0a9f6-127">內容類型</span><span class="sxs-lookup"><span data-stu-id="0a9f6-127">Genre</span></span>
- <span data-ttu-id="0a9f6-128">發行日期</span><span class="sxs-lookup"><span data-stu-id="0a9f6-128">Publication date</span></span>
- <span data-ttu-id="0a9f6-129">價格</span><span class="sxs-lookup"><span data-stu-id="0a9f6-129">Price</span></span>
- <span data-ttu-id="0a9f6-130">描述</span><span class="sxs-lookup"><span data-stu-id="0a9f6-130">Description</span></span>
- <span data-ttu-id="0a9f6-131">AuthorID （Authors 資料表的外部索引鍵）</span><span class="sxs-lookup"><span data-stu-id="0a9f6-131">AuthorID (foreign key to an Authors table)</span></span>

<span data-ttu-id="0a9f6-132">不過，對於大部分的要求，這個 API 會傳回此資料 （標題、 作者和內容類型） 的子集。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-132">For most requests, however, the API will return a subset of this data (title, author, and genre).</span></span> <span data-ttu-id="0a9f6-133">若要取得完整的記錄，用戶端要求`/api/books/{id}/details`。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-133">To get the complete record, the client requests `/api/books/{id}/details`.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0a9f6-134">必要條件</span><span class="sxs-lookup"><span data-stu-id="0a9f6-134">Prerequisites</span></span>

<span data-ttu-id="0a9f6-135">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community、 Professional 或 Enterprise edition。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-135">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional or Enterprise edition.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="0a9f6-136">建立 Visual Studio 專案</span><span class="sxs-lookup"><span data-stu-id="0a9f6-136">Create the Visual Studio Project</span></span>

<span data-ttu-id="0a9f6-137">執行 Visual Studio 啟動。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-137">Start by running Visual Studio.</span></span> <span data-ttu-id="0a9f6-138">從**檔案**功能表上，選取**新增**，然後選取**專案**。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-138">From the **File** menu, select **New** and then select **Project**.</span></span>

<span data-ttu-id="0a9f6-139">依序展開**已安裝** > **Visual C#** 類別目錄。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-139">Expand the **Installed** > **Visual C#** category.</span></span> <span data-ttu-id="0a9f6-140">底下**Visual C#**，選取**Web**。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-140">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="0a9f6-141">在專案範本清單中，選取**ASP.NET Web 應用程式 (.NET Framework)**。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-141">In the list of project templates, select **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="0a9f6-142">將專案命名為&quot;BooksAPI&quot;。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-142">Name the project &quot;BooksAPI&quot;.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

<span data-ttu-id="0a9f6-143">在 **新的 ASP.NET Web 應用程式**對話方塊中，選取**空白**範本。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-143">In the **New ASP.NET Web Application** dialog, select the **Empty** template.</span></span> <span data-ttu-id="0a9f6-144">在"新增資料夾和核心參考 」，選取**Web API**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-144">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="0a9f6-145">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-145">Click **OK**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

<span data-ttu-id="0a9f6-146">這會建立基本架構的專案已針對 Web API 的功能。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-146">This creates a skeleton project that is configured for Web API functionality.</span></span>

### <a name="domain-models"></a><span data-ttu-id="0a9f6-147">網域模型</span><span class="sxs-lookup"><span data-stu-id="0a9f6-147">Domain Models</span></span>

<span data-ttu-id="0a9f6-148">接下來，新增的網域模型的類別。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-148">Next, add classes for domain models.</span></span> <span data-ttu-id="0a9f6-149">在 [方案總管] 中，以滑鼠右鍵按一下 [模型] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-149">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="0a9f6-150">選取 **新增**，然後選取**類別**。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-150">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="0a9f6-151">將類別命名為 `Author` 。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-151">Name the class `Author`.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

<span data-ttu-id="0a9f6-152">您可以將 Author.cs 中的程式碼取代為下列：</span><span class="sxs-lookup"><span data-stu-id="0a9f6-152">Replace the code in Author.cs with the following:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

<span data-ttu-id="0a9f6-153">現在加入另一個類別，名為`Book`。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-153">Now add another class named `Book`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a><span data-ttu-id="0a9f6-154">新增 Web API 控制器</span><span class="sxs-lookup"><span data-stu-id="0a9f6-154">Add a Web API Controller</span></span>

<span data-ttu-id="0a9f6-155">在此步驟中，我們將新增為資料層會使用 Entity Framework 的 Web API 控制器。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-155">In this step, we'll add a Web API controller that uses Entity Framework as the data layer.</span></span>

<span data-ttu-id="0a9f6-156">按 CTRL+SHIFT+B 以建置專案。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-156">Press CTRL+SHIFT+B to build the project.</span></span> <span data-ttu-id="0a9f6-157">Entity Framework 會使用反射來探索模型中，屬性，因此需要編譯的組件，來建立資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-157">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

<span data-ttu-id="0a9f6-158">在 [方案總管] 中，以滑鼠右鍵按一下 [控制器] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-158">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="0a9f6-159">選取 **新增**，然後選取**控制站**。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-159">Select **Add**, then select **Controller**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

<span data-ttu-id="0a9f6-160">在 **新增 Scaffold**對話方塊中，選取**Web API 2 控制器與動作，使用 Entity Framework**。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-160">In the **Add Scaffold** dialog, select **Web API 2 Controller with actions, using Entity Framework**.</span></span>

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

<span data-ttu-id="0a9f6-161">在 [**新增控制器**] 對話方塊中，如**控制器名稱**，輸入&quot;BooksController&quot;。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-161">In the **Add Controller** dialog, for **Controller name**, enter &quot;BooksController&quot;.</span></span> <span data-ttu-id="0a9f6-162">選取 &quot;使用非同步控制器動作&quot;核取方塊。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-162">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="0a9f6-163">針對**模型類別**，選取&quot;活頁簿&quot;。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-163">For **Model class**, select &quot;Book&quot;.</span></span> <span data-ttu-id="0a9f6-164">(如果您沒有看到`Book`類別列在下拉式清單，請確定您建置專案。)然後按一下"+"按鈕。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-164">(If you don't see the `Book` class listed in the dropdown, make sure that you built the project.) Then click the "+" button.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

<span data-ttu-id="0a9f6-165">按一下 **新增**中**新的資料內容**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-165">Click **Add** in the **New Data Context** dialog.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

<span data-ttu-id="0a9f6-166">按一下 **新增**中**新增控制器**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-166">Click **Add** in the **Add Controller** dialog.</span></span> <span data-ttu-id="0a9f6-167">Scaffolding 會加入名為類別`BooksController`定義 API 控制器。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-167">The scaffolding adds a class named `BooksController` that defines the API controller.</span></span> <span data-ttu-id="0a9f6-168">它也會新增一個名為類別`BooksAPIContext`在 Models 資料夾中，其定義用於 Entity Framework 資料內容。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-168">It also adds a class named `BooksAPIContext` in the Models folder, which defines the data context for Entity Framework.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a><span data-ttu-id="0a9f6-169">植入資料庫</span><span class="sxs-lookup"><span data-stu-id="0a9f6-169">Seed the Database</span></span>

<span data-ttu-id="0a9f6-170">從 [工具] 功能表中，選取**NuGet 套件管理員**，然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-170">From the Tools menu, select **NuGet Package Manager**, and then select **Package Manager Console**.</span></span>

<span data-ttu-id="0a9f6-171">在 [套件管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="0a9f6-171">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

<span data-ttu-id="0a9f6-172">此命令會建立 Migrations 資料夾，並將新的程式碼檔案，名為 Configuration.cs。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-172">This command creates a Migrations folder and adds a new code file named Configuration.cs.</span></span> <span data-ttu-id="0a9f6-173">開啟此檔案並新增下列程式碼`Configuration.Seed`方法。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-173">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

<span data-ttu-id="0a9f6-174">在 [套件管理員主控台] 視窗中，輸入下列命令。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-174">In the Package Manager Console window, type the following commands.</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

<span data-ttu-id="0a9f6-175">這些命令會建立本機資料庫，並叫用種子方法，來擴展資料庫。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-175">These commands create a local database and invoke the Seed method to populate the database.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a><span data-ttu-id="0a9f6-176">新增的 DTO 類別</span><span class="sxs-lookup"><span data-stu-id="0a9f6-176">Add DTO Classes</span></span>

<span data-ttu-id="0a9f6-177">如果您執行應用程式現在，並將 GET 要求傳送至 /api/books/1，回應看起來如下所示。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-177">If you run the application now and send a GET request to /api/books/1, the response looks similar to the following.</span></span> <span data-ttu-id="0a9f6-178">（我新增縮排以提高可讀性）。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-178">(I added indentation for readability.)</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

<span data-ttu-id="0a9f6-179">相反地，我想這項要求傳回的欄位子集。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-179">Instead, I want this request to return a subset of the fields.</span></span> <span data-ttu-id="0a9f6-180">此外，我希望它傳回作者的名稱，而不是作者的識別碼。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-180">Also, I want it to return the author's name, rather than the author ID.</span></span> <span data-ttu-id="0a9f6-181">若要這麼做，我們要修改控制器方法，以傳回*資料傳輸物件*(DTO) 而不是 EF 模型。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-181">To accomplish this, we'll modify the controller methods to return a *data transfer object* (DTO) instead of the EF model.</span></span> <span data-ttu-id="0a9f6-182">DTO 是只設計來執行資料的物件。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-182">A DTO is an object that is designed only to carry data.</span></span>

<span data-ttu-id="0a9f6-183">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**新增** | **新資料夾**。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-183">In Solution Explorer, right-click the project and select **Add** | **New Folder**.</span></span> <span data-ttu-id="0a9f6-184">將資料夾命名&quot;Dto&quot;。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-184">Name the folder &quot;DTOs&quot;.</span></span> <span data-ttu-id="0a9f6-185">新增類別，名為`BookDto`Dto 資料夾中，使用下列定義：</span><span class="sxs-lookup"><span data-stu-id="0a9f6-185">Add a class named `BookDto` to the DTOs folder, with the following definition:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

<span data-ttu-id="0a9f6-186">新增另一個類別，名為`BookDetailDto`。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-186">Add another class named `BookDetailDto`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

<span data-ttu-id="0a9f6-187">接下來，更新`BooksController`類別，以傳回`BookDto`執行個體。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-187">Next, update the `BooksController` class to return `BookDto` instances.</span></span> <span data-ttu-id="0a9f6-188">我們將使用[Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx)方法，以專案`Book`執行個體`BookDto`執行個體。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-188">We'll use the [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) method to project `Book` instances to `BookDto` instances.</span></span> <span data-ttu-id="0a9f6-189">以下是控制器類別的更新程式碼。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-189">Here is the updated code for the controller class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> <span data-ttu-id="0a9f6-190">我刪掉`PutBook`， `PostBook`，和`DeleteBook`方法，因為本教學課程不需要它們。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-190">I deleted the `PutBook`, `PostBook`, and `DeleteBook` methods, because they aren't needed for this tutorial.</span></span>


<span data-ttu-id="0a9f6-191">現在如果您執行應用程式，並要求 /api/books/1，回應主體看起來應該像這樣：</span><span class="sxs-lookup"><span data-stu-id="0a9f6-191">Now if you run the application and request /api/books/1, the response body should look like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a><span data-ttu-id="0a9f6-192">新增路由屬性</span><span class="sxs-lookup"><span data-stu-id="0a9f6-192">Add Route Attributes</span></span>

<span data-ttu-id="0a9f6-193">接下來，我們會將轉換控制器以使用屬性路由。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-193">Next, we'll convert the controller to use attribute routing.</span></span> <span data-ttu-id="0a9f6-194">首先，新增**RoutePrefix**屬性控制站。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-194">First, add a **RoutePrefix** attribute to the controller.</span></span> <span data-ttu-id="0a9f6-195">此屬性會定義此控制站上的所有方法的初始 URI 區段。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-195">This attribute defines the initial URI segments for all methods on this controller.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

<span data-ttu-id="0a9f6-196">然後新增 **[路由]** 屬性至控制器動作，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0a9f6-196">Then add **[Route]** attributes to the controller actions, as follows:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

<span data-ttu-id="0a9f6-197">每個控制器方法的路由範本是前置詞加上字串中指定**路由**屬性。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-197">The route template for each controller method is the prefix plus the string specified in the **Route** attribute.</span></span> <span data-ttu-id="0a9f6-198">針對`GetBook`方法，路由範本中包含的參數化的字串&quot;{id: int}&quot;，會比對如果 URI 區段包含整數值。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-198">For the `GetBook` method, the route template includes the parameterized string &quot;{id:int}&quot;, which matches if the URI segment contains an integer value.</span></span>

| <span data-ttu-id="0a9f6-199">方法</span><span class="sxs-lookup"><span data-stu-id="0a9f6-199">Method</span></span> | <span data-ttu-id="0a9f6-200">路由範本</span><span class="sxs-lookup"><span data-stu-id="0a9f6-200">Route Template</span></span> | <span data-ttu-id="0a9f6-201">範例 URI</span><span class="sxs-lookup"><span data-stu-id="0a9f6-201">Example URI</span></span> |
| --- | --- | --- |
| `GetBooks` | <span data-ttu-id="0a9f6-202">"api/books"</span><span class="sxs-lookup"><span data-stu-id="0a9f6-202">"api/books"</span></span> | `http://localhost/api/books` |
| `GetBook` | <span data-ttu-id="0a9f6-203">「 書籍 api / / {id: int}"</span><span class="sxs-lookup"><span data-stu-id="0a9f6-203">"api/books/{id:int}"</span></span> | `http://localhost/api/books/5` |

## <a name="get-book-details"></a><span data-ttu-id="0a9f6-204">取得活頁簿詳細資料</span><span class="sxs-lookup"><span data-stu-id="0a9f6-204">Get Book Details</span></span>

<span data-ttu-id="0a9f6-205">若要取得活頁簿詳細資料，用戶端會傳送 GET 要求`/api/books/{id}/details`，其中 *{id}* 本書的識別碼。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-205">To get book details, the client will send a GET request to `/api/books/{id}/details`, where *{id}* is the ID of the book.</span></span>

<span data-ttu-id="0a9f6-206">將下列方法加入 `BooksController` 類別。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-206">Add the following method to the `BooksController` class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

<span data-ttu-id="0a9f6-207">如果您要求`/api/books/1/details`，回應看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="0a9f6-207">If you request `/api/books/1/details`, the response looks like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a><span data-ttu-id="0a9f6-208">依內容類型的書籍</span><span class="sxs-lookup"><span data-stu-id="0a9f6-208">Get Books By Genre</span></span>

<span data-ttu-id="0a9f6-209">若要取得特定的內容類型的書籍清單，用戶端會傳送 GET 要求`/api/books/genre`，其中*genre*是內容類型的名稱。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-209">To get a list of books in a specific genre, the client will send a GET request to `/api/books/genre`, where *genre* is the name of the genre.</span></span> <span data-ttu-id="0a9f6-210">(例如，`/api/books/fantasy`)。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-210">(For example, `/api/books/fantasy`.)</span></span>

<span data-ttu-id="0a9f6-211">將下列方法加入`BooksController`。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-211">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

<span data-ttu-id="0a9f6-212">這裡我們所定義的路由，其中包含 URI 範本中的 {genre} 參數。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-212">Here we are defining a route that contains a {genre} parameter in the URI template.</span></span> <span data-ttu-id="0a9f6-213">請注意，Web API 可以區分這些兩個 Uri，並將它們路由傳送至不同的方法：</span><span class="sxs-lookup"><span data-stu-id="0a9f6-213">Notice that Web API is able to distinguish these two URIs and route them to different methods:</span></span>

`/api/books/1`

`/api/books/fantasy`

<span data-ttu-id="0a9f6-214">這是因為`GetBook`方法包含 「 識別碼 」 區段必須是整數值的條件約束：</span><span class="sxs-lookup"><span data-stu-id="0a9f6-214">That's because the `GetBook` method includes a constraint that the "id" segment must be an integer value:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

<span data-ttu-id="0a9f6-215">如果您要求 /api/books/fantasy，回應看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="0a9f6-215">If you request /api/books/fantasy, the response looks like this:</span></span>

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a><span data-ttu-id="0a9f6-216">作者的書籍</span><span class="sxs-lookup"><span data-stu-id="0a9f6-216">Get Books By Author</span></span>

<span data-ttu-id="0a9f6-217">若要取得某位特定作者的書籍清單，用戶端會傳送 GET 要求`/api/authors/id/books`，其中*識別碼*作者的識別碼。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-217">To get a list of a books for a particular author, the client will send a GET request to `/api/authors/id/books`, where *id* is the ID of the author.</span></span>

<span data-ttu-id="0a9f6-218">將下列方法加入`BooksController`。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-218">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

<span data-ttu-id="0a9f6-219">有趣的此範例中，是因為&quot;書籍&quot;是處理的子資源&quot;作者&quot;。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-219">This example is interesting because &quot;books&quot; is treated a child resource of &quot;authors&quot;.</span></span> <span data-ttu-id="0a9f6-220">此模式是 RESTful Api 中相當常見。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-220">This pattern is quite common in RESTful APIs.</span></span>

<span data-ttu-id="0a9f6-221">波狀符號 （~） 路由範本中的會覆寫中的路由前置詞**RoutePrefix**屬性。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-221">The tilde (~) in the route template overrides the route prefix in the **RoutePrefix** attribute.</span></span>

## <a name="get-books-by-publication-date"></a><span data-ttu-id="0a9f6-222">取得活頁簿發行日期</span><span class="sxs-lookup"><span data-stu-id="0a9f6-222">Get Books By Publication Date</span></span>

<span data-ttu-id="0a9f6-223">若要取得依發行日期的書籍清單，用戶端會傳送 GET 要求`/api/books/date/yyyy-mm-dd`，其中*yyyy-mm-dd 的-* 的日期。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-223">To get a list of books by publication date, the client will send a GET request to `/api/books/date/yyyy-mm-dd`, where *yyyy-mm-dd* is the date.</span></span>

<span data-ttu-id="0a9f6-224">若要這樣做的其中一個方法如下：</span><span class="sxs-lookup"><span data-stu-id="0a9f6-224">Here is one way to do this:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

<span data-ttu-id="0a9f6-225">`{pubdate:datetime}`參數會限制為符合**DateTime**值。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-225">The `{pubdate:datetime}` parameter is constrained to match a **DateTime** value.</span></span> <span data-ttu-id="0a9f6-226">可以運作，但超出我們想要的實際更寬鬆。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-226">This works, but it's actually more permissive than we'd like.</span></span> <span data-ttu-id="0a9f6-227">比方說，這些 Uri 也會比對路由：</span><span class="sxs-lookup"><span data-stu-id="0a9f6-227">For example, these URIs will also match the route:</span></span>

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

<span data-ttu-id="0a9f6-228">沒有任何問題讓這些 Uri。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-228">There's nothing wrong with allowing these URIs.</span></span> <span data-ttu-id="0a9f6-229">不過，您也可以將規則運算式的條件約束加入至路由範本，以限制路由至特定的格式：</span><span class="sxs-lookup"><span data-stu-id="0a9f6-229">However, you can restrict the route to a particular format by adding a regular-expression constraint to the route template:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

<span data-ttu-id="0a9f6-230">現在僅日期格式&quot;yyyy-mm-dd 的-&quot;會比對。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-230">Now only dates in the form &quot;yyyy-mm-dd&quot; will match.</span></span> <span data-ttu-id="0a9f6-231">請注意，我們不使用 regex 驗證，我們會取得實際的日期。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-231">Notice that we don't use the regex to validate that we got a real date.</span></span> <span data-ttu-id="0a9f6-232">Web API 會嘗試將轉換成 URI 區段時，處理**DateTime**執行個體。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-232">That is handled when Web API tries to convert the URI segment into a **DateTime** instance.</span></span> <span data-ttu-id="0a9f6-233">無效的日期 2012 例如 '-47-99' 將無法進行轉換，而且用戶端會收到 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-233">An invalid date such as '2012-47-99' will fail to be converted, and the client will get a 404 error.</span></span>

<span data-ttu-id="0a9f6-234">您也可以支援斜線分隔符號 (`/api/books/date/yyyy/mm/dd`) 是新增另一個 **[Route]** 與其他規則運算式的屬性。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-234">You can also support a slash separator (`/api/books/date/yyyy/mm/dd`) by adding another **[Route]** attribute with a different regex.</span></span>

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

<span data-ttu-id="0a9f6-235">沒有細微但很重要明瞭。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-235">There is a subtle but important detail here.</span></span> <span data-ttu-id="0a9f6-236">第二個路由範本具有萬用字元 (\*) {pubdate} 參數的開頭：</span><span class="sxs-lookup"><span data-stu-id="0a9f6-236">The second route template has a wildcard character (\*) at the start of the {pubdate} parameter:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

<span data-ttu-id="0a9f6-237">這會告訴路由引擎該 {pubdate} 應該符合 URI 的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-237">This tells the routing engine that {pubdate} should match the rest of the URI.</span></span> <span data-ttu-id="0a9f6-238">根據預設，範本參數會比對單一的 URI 區段。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-238">By default, a template parameter matches a single URI segment.</span></span> <span data-ttu-id="0a9f6-239">在此情況下，我們要 {pubdate} 跨越數個 URI 區段：</span><span class="sxs-lookup"><span data-stu-id="0a9f6-239">In this case, we want {pubdate} to span several URI segments:</span></span>

`/api/books/date/2013/06/17`

## <a name="controller-code"></a><span data-ttu-id="0a9f6-240">控制器程式碼</span><span class="sxs-lookup"><span data-stu-id="0a9f6-240">Controller Code</span></span>

<span data-ttu-id="0a9f6-241">以下是 BooksController 類別的完整程式碼。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-241">Here is the complete code for the BooksController class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a><span data-ttu-id="0a9f6-242">總結</span><span class="sxs-lookup"><span data-stu-id="0a9f6-242">Summary</span></span>

<span data-ttu-id="0a9f6-243">屬性路由可讓您更多控制權和更大的彈性設計時為您的 API 的 Uri。</span><span class="sxs-lookup"><span data-stu-id="0a9f6-243">Attribute routing gives you more control and greater flexibility when designing the URIs for your API.</span></span>
