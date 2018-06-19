---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: 新增模型和控制站 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 015bb9698d81387d03ea8f9629316fb53232e708
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879578"
---
<a name="add-models-and-controllers"></a><span data-ttu-id="cc93a-102">新增模型和控制站</span><span class="sxs-lookup"><span data-stu-id="cc93a-102">Add Models and Controllers</span></span>
====================
<span data-ttu-id="cc93a-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cc93a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="cc93a-104">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="cc93a-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="cc93a-105">在本節中，您會將模型類別，可定義資料庫實體。</span><span class="sxs-lookup"><span data-stu-id="cc93a-105">In this section, you will add model classes that define the database entities.</span></span> <span data-ttu-id="cc93a-106">然後您會加入這些實體執行 CRUD 作業的 Web API 控制器。</span><span class="sxs-lookup"><span data-stu-id="cc93a-106">Then you will add Web API controllers that perform CRUD operations on those entities.</span></span>

## <a name="add-model-classes"></a><span data-ttu-id="cc93a-107">新增模型類別</span><span class="sxs-lookup"><span data-stu-id="cc93a-107">Add Model Classes</span></span>

<span data-ttu-id="cc93a-108">在本教學課程中，我們將建立資料庫，使用 Entity Framework (EF) 的 「 程式碼優先 」 方法。</span><span class="sxs-lookup"><span data-stu-id="cc93a-108">In this tutorial, we'll create the database by using the "Code First" approach to Entity Framework (EF).</span></span> <span data-ttu-id="cc93a-109">Code first 您撰寫 C# 類別對應至資料庫資料表，並 EF 建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="cc93a-109">With Code First, you write C# classes that correspond to database tables, and EF creates the database.</span></span> <span data-ttu-id="cc93a-110">(如需詳細資訊，請參閱[Entity Framework 開發方式](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf)。)</span><span class="sxs-lookup"><span data-stu-id="cc93a-110">(For more information, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span></span>

<span data-ttu-id="cc93a-111">我們一開始定義為 POCOs （純舊 CLR 物件） 的網域物件。</span><span class="sxs-lookup"><span data-stu-id="cc93a-111">We start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="cc93a-112">我們將建立下列 POCOs:</span><span class="sxs-lookup"><span data-stu-id="cc93a-112">We will create the following POCOs:</span></span>

- <span data-ttu-id="cc93a-113">作者</span><span class="sxs-lookup"><span data-stu-id="cc93a-113">Author</span></span>
- <span data-ttu-id="cc93a-114">活頁簿</span><span class="sxs-lookup"><span data-stu-id="cc93a-114">Book</span></span>

<span data-ttu-id="cc93a-115">在 [方案總管] 中，以滑鼠右鍵按一下 [模型] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="cc93a-115">In Solution Explorer, right click the Models folder.</span></span> <span data-ttu-id="cc93a-116">選取**新增**，然後選取**類別**。</span><span class="sxs-lookup"><span data-stu-id="cc93a-116">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="cc93a-117">將類別命名為 `Author` 。</span><span class="sxs-lookup"><span data-stu-id="cc93a-117">Name the class `Author`.</span></span>

![](part-2/_static/image1.png)

<span data-ttu-id="cc93a-118">所有 Author.cs 中的未定案程式碼取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="cc93a-118">Replace all of the boilerplate code in Author.cs with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample1.cs)]

<span data-ttu-id="cc93a-119">加入另一個類別，名為`Book`，下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="cc93a-119">Add another class named `Book`, with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample2.cs)]

<span data-ttu-id="cc93a-120">Entity Framework 會使用這些模型來建立資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="cc93a-120">Entity Framework will use these models to create database tables.</span></span> <span data-ttu-id="cc93a-121">每個模型`Id`屬性就會成為資料庫資料表的主索引鍵資料行。</span><span class="sxs-lookup"><span data-stu-id="cc93a-121">For each model, the `Id` property will become the primary key column of the database table.</span></span>

<span data-ttu-id="cc93a-122">在活頁簿類別中，`AuthorId`定義外部索引鍵`Author`資料表。</span><span class="sxs-lookup"><span data-stu-id="cc93a-122">In the Book class, the `AuthorId` defines a foreign key into the `Author` table.</span></span> <span data-ttu-id="cc93a-123">（為了簡單起見，我假設每個活頁簿有單一的作者。）活頁簿類別也包含相關的導覽屬性`Author`。</span><span class="sxs-lookup"><span data-stu-id="cc93a-123">(For simplicity, I'm assuming that each book has a single author.) The book class also contains a navigation property to the related `Author`.</span></span> <span data-ttu-id="cc93a-124">您可以使用導覽屬性來存取相關`Author`程式碼中。</span><span class="sxs-lookup"><span data-stu-id="cc93a-124">You can use the navigation property to access the related `Author` in code.</span></span> <span data-ttu-id="cc93a-125">我更多導覽屬性，在第 4 部分[處理實體關聯](part-4.md)。</span><span class="sxs-lookup"><span data-stu-id="cc93a-125">I say more about navigation properties in part 4, [Handling Entity Relations](part-4.md).</span></span>

## <a name="add-web-api-controllers"></a><span data-ttu-id="cc93a-126">加入 Web API 控制器</span><span class="sxs-lookup"><span data-stu-id="cc93a-126">Add Web API Controllers</span></span>

<span data-ttu-id="cc93a-127">在本節中，我們會將 Web API 控制器支援 CRUD 作業 （建立、 讀取、 更新和刪除）。</span><span class="sxs-lookup"><span data-stu-id="cc93a-127">In this section, we'll add Web API controllers that support CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="cc93a-128">控制器會使用 Entity Framework 來與資料庫層進行通訊。</span><span class="sxs-lookup"><span data-stu-id="cc93a-128">The controllers will use Entity Framework to communicate with the database layer.</span></span>

<span data-ttu-id="cc93a-129">首先，您可以刪除 Controllers/ValuesController.cs 的檔案。</span><span class="sxs-lookup"><span data-stu-id="cc93a-129">First, you can delete the file Controllers/ValuesController.cs.</span></span> <span data-ttu-id="cc93a-130">這個檔案包含的範例 Web 應用程式開發介面控制站，但您不需要在此教學課程。</span><span class="sxs-lookup"><span data-stu-id="cc93a-130">This file contains an example Web API controller, but you don't need it for this tutorial.</span></span>

![](part-2/_static/image2.png)

<span data-ttu-id="cc93a-131">接下來，建置專案。</span><span class="sxs-lookup"><span data-stu-id="cc93a-131">Next, build the project.</span></span> <span data-ttu-id="cc93a-132">Web API scaffolding 使用反映來尋找模型類別，因此，它需要編譯的組件。</span><span class="sxs-lookup"><span data-stu-id="cc93a-132">The Web API scaffolding uses reflection to find the model classes, so it needs the compiled assembly.</span></span>

<span data-ttu-id="cc93a-133">在 [方案總管] 中，以滑鼠右鍵按一下 [控制器] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="cc93a-133">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="cc93a-134">選取**新增**，然後選取**控制器**。</span><span class="sxs-lookup"><span data-stu-id="cc93a-134">Select **Add**, then select **Controller**.</span></span>

![](part-2/_static/image3.png)

<span data-ttu-id="cc93a-135">在**新增 Scaffold**對話方塊中，選取 「 Web API 2 控制器動作，使用 Entity Framework"。</span><span class="sxs-lookup"><span data-stu-id="cc93a-135">In the **Add Scaffold** dialog, select "Web API 2 Controller with actions, using Entity Framework".</span></span> <span data-ttu-id="cc93a-136">按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="cc93a-136">Click **Add**.</span></span>

![](part-2/_static/image4.png)

<span data-ttu-id="cc93a-137">在**加入控制器** 對話方塊中，執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="cc93a-137">In the **Add Controller** dialog, do the following:</span></span>

1. <span data-ttu-id="cc93a-138">在**模型類別**下拉式清單中，選取`Author`類別。</span><span class="sxs-lookup"><span data-stu-id="cc93a-138">In the **Model class** dropdown, select the `Author` class.</span></span> <span data-ttu-id="cc93a-139">（如果您沒有看到它列在下拉式清單，請確定您建置專案。）</span><span class="sxs-lookup"><span data-stu-id="cc93a-139">(If you don't see it listed in the dropdown, make sure that you built the project.)</span></span>
2. <span data-ttu-id="cc93a-140">請檢查 「 使用非同步控制器動作 」。</span><span class="sxs-lookup"><span data-stu-id="cc93a-140">Check "Use async controller actions".</span></span>
3. <span data-ttu-id="cc93a-141">將控制器名稱做為&quot;AuthorsController&quot;。</span><span class="sxs-lookup"><span data-stu-id="cc93a-141">Leave the controller name as &quot;AuthorsController&quot;.</span></span>
4. <span data-ttu-id="cc93a-142">按一下加號 （+） 按鈕旁的 **資料內容類別**。</span><span class="sxs-lookup"><span data-stu-id="cc93a-142">Click plus (+) button next to **Data Context Class**.</span></span>

![](part-2/_static/image5.png)

<span data-ttu-id="cc93a-143">在**新的資料內容** 對話方塊中，保留預設名稱，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="cc93a-143">In the **New Data Context** dialog, leave the default name and click **Add**.</span></span>

![](part-2/_static/image6.png)

<span data-ttu-id="cc93a-144">按一下**新增**完成**加入控制器**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="cc93a-144">Click **Add** to complete the **Add Controller** dialog.</span></span> <span data-ttu-id="cc93a-145">此對話方塊會將兩個類別加入至您的專案：</span><span class="sxs-lookup"><span data-stu-id="cc93a-145">The dialog adds two classes to your project:</span></span>

- <span data-ttu-id="cc93a-146">`AuthorsController` 定義此 Web API 控制器。</span><span class="sxs-lookup"><span data-stu-id="cc93a-146">`AuthorsController` defines a Web API controller.</span></span> <span data-ttu-id="cc93a-147">控制器會執行 REST API 用戶端用來執行在清單上的 CRUD 作業的作者。</span><span class="sxs-lookup"><span data-stu-id="cc93a-147">The controller implements the REST API that clients use to perform CRUD operations on the list of authors.</span></span>
- <span data-ttu-id="cc93a-148">`BookServiceContext` 在執行階段，其中包含填入物件，包含來自資料庫、 變更追蹤和保存資料與資料庫管理實體的物件。</span><span class="sxs-lookup"><span data-stu-id="cc93a-148">`BookServiceContext` manages entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span> <span data-ttu-id="cc93a-149">它繼承自`DbContext`。</span><span class="sxs-lookup"><span data-stu-id="cc93a-149">It inherits from `DbContext`.</span></span>

![](part-2/_static/image7.png)

<span data-ttu-id="cc93a-150">此時，一次建置專案。</span><span class="sxs-lookup"><span data-stu-id="cc93a-150">At this point, build the project again.</span></span> <span data-ttu-id="cc93a-151">現在，請透過相同的步驟，若要加入的 API 控制器`Book`實體。</span><span class="sxs-lookup"><span data-stu-id="cc93a-151">Now go through the same steps to add an API controller for `Book` entities.</span></span> <span data-ttu-id="cc93a-152">此時，請選取`Book`模型類別，然後選取現有`BookServiceContext`資料內容類別的類別。</span><span class="sxs-lookup"><span data-stu-id="cc93a-152">This time, select `Book` for the model class, and select the existing `BookServiceContext` class for the data context class.</span></span> <span data-ttu-id="cc93a-153">（不建立新的資料內容）。按一下**新增**加入控制器。</span><span class="sxs-lookup"><span data-stu-id="cc93a-153">(Don't create a new data context.) Click **Add** to add the controller.</span></span>

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> <span data-ttu-id="cc93a-154">[上一頁](part-1.md)
> [下一頁](part-3.md)</span><span class="sxs-lookup"><span data-stu-id="cc93a-154">[Previous](part-1.md)
[Next](part-3.md)</span></span>
