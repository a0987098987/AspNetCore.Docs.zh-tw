---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: 使用 Entity Framework 4.0 和 ObjectDataSource 控制項，第 2 部分： 加入商務邏輯層和單元測試 |Microsoft 文件
author: tdykstra
description: 此教學課程系列為基礎所建立的開始使用 Entity Framework 4.0 教學課程系列的 Contoso 大學 web 應用程式。 I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: ecdfb2bdc546f55778ec4cc1f61aa66e129134ea
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888311"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a><span data-ttu-id="a59b2-104">使用 Entity Framework 4.0 和 ObjectDataSource 控制項，第 2 部分： 加入商務邏輯層和單元測試</span><span class="sxs-lookup"><span data-stu-id="a59b2-104">Using the Entity Framework 4.0 and the ObjectDataSource Control, Part 2: Adding a Business Logic Layer and Unit Tests</span></span>
====================
<span data-ttu-id="a59b2-105">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a59b2-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="a59b2-106">此教學課程系列為基礎所建立的 Contoso 大學 web 應用程式[開始使用 Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started)教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="a59b2-106">This tutorial series builds on the Contoso University web application that is created by the [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) tutorial series.</span></span> <span data-ttu-id="a59b2-107">如果您未完成先前的教學課程，您可以身分本教學課程的起始點來[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)您會建立。</span><span class="sxs-lookup"><span data-stu-id="a59b2-107">If you didn't complete the earlier tutorials, as a starting point for this tutorial you can [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) that you would have created.</span></span> <span data-ttu-id="a59b2-108">您也可以[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)完整的教學課程所建立。</span><span class="sxs-lookup"><span data-stu-id="a59b2-108">You can also [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) that is created by the complete tutorial series.</span></span> <span data-ttu-id="a59b2-109">如果您有疑問的教學課程，您可以張貼他們[ASP.NET Entity Framework 論壇](https://forums.asp.net/1227.aspx)。</span><span class="sxs-lookup"><span data-stu-id="a59b2-109">If you have questions about the tutorials, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx).</span></span>


<span data-ttu-id="a59b2-110">在上一個教學課程中，您建立使用 Entity Framework 的多層式架構 web 應用程式和`ObjectDataSource`控制項。</span><span class="sxs-lookup"><span data-stu-id="a59b2-110">In the previous tutorial you created an n-tier web application using the Entity Framework and the `ObjectDataSource` control.</span></span> <span data-ttu-id="a59b2-111">本教學課程示範如何將商務邏輯加入同時維持商務邏輯層 (BLL) 資料存取層 (DAL) 不同，並顯示如何建立 BLL 自動化的單元測試。</span><span class="sxs-lookup"><span data-stu-id="a59b2-111">This tutorial shows how to add business logic while keeping the business-logic layer (BLL) and the data-access layer (DAL) separate, and it shows how to create automated unit tests for the BLL.</span></span>

<span data-ttu-id="a59b2-112">在本教學課程中，您將完成下列工作：</span><span class="sxs-lookup"><span data-stu-id="a59b2-112">In this tutorial you'll complete the following tasks:</span></span>

- <span data-ttu-id="a59b2-113">建立儲存機制介面宣告您需要的資料存取方法。</span><span class="sxs-lookup"><span data-stu-id="a59b2-113">Create a repository interface that declares the data-access methods you need.</span></span>
- <span data-ttu-id="a59b2-114">儲存機制類別中實作儲存機制的介面。</span><span class="sxs-lookup"><span data-stu-id="a59b2-114">Implement the repository interface in the repository class.</span></span>
- <span data-ttu-id="a59b2-115">建立呼叫儲存機制類別，以執行資料存取功能的商務邏輯類別。</span><span class="sxs-lookup"><span data-stu-id="a59b2-115">Create a business-logic class that calls the repository class to perform data-access functions.</span></span>
- <span data-ttu-id="a59b2-116">連接`ObjectDataSource`控制項至儲存機制類別而不是商務邏輯類別。</span><span class="sxs-lookup"><span data-stu-id="a59b2-116">Connect the `ObjectDataSource` control to the business-logic class instead of to the repository class.</span></span>
- <span data-ttu-id="a59b2-117">建立單元測試專案和其資料存放區的使用記憶體中集合的儲存機制類別。</span><span class="sxs-lookup"><span data-stu-id="a59b2-117">Create a unit-test project and a repository class that uses in-memory collections for its data store.</span></span>
- <span data-ttu-id="a59b2-118">建立您想要加入至商務邏輯類別，則執行測試，並查看它失敗的商務邏輯的單元測試。</span><span class="sxs-lookup"><span data-stu-id="a59b2-118">Create a unit test for business logic that you want to add to the business-logic class, then run the test and see it fail.</span></span>
- <span data-ttu-id="a59b2-119">商務邏輯在類別中實作商務邏輯，然後重新執行單元測試，並查看其通過。</span><span class="sxs-lookup"><span data-stu-id="a59b2-119">Implement the business logic in the business-logic class, then re-run the unit test and see it pass.</span></span>

<span data-ttu-id="a59b2-120">您將使用*Departments.aspx*和*DepartmentsAdd.aspx*您在上一個教學課程中建立的網頁。</span><span class="sxs-lookup"><span data-stu-id="a59b2-120">You'll work with the *Departments.aspx* and *DepartmentsAdd.aspx* pages that you created in the previous tutorial.</span></span>

## <a name="creating-a-repository-interface"></a><span data-ttu-id="a59b2-121">建立儲存機制介面</span><span class="sxs-lookup"><span data-stu-id="a59b2-121">Creating a Repository Interface</span></span>

<span data-ttu-id="a59b2-122">您一開始所建立的儲存機制介面。</span><span class="sxs-lookup"><span data-stu-id="a59b2-122">You'll begin by creating the repository interface.</span></span>

<span data-ttu-id="a59b2-123">[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a59b2-123">[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)</span></span>

<span data-ttu-id="a59b2-124">在*DAL*資料夾中，建立新的類別檔案，其命名*ISchoolRepository.cs*，並以下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="a59b2-124">In the *DAL* folder, create a new class file, name it *ISchoolRepository.cs*, and replace the existing code with the following code:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

<span data-ttu-id="a59b2-125">介面會定義一種方法的每個 CRUD （建立、 讀取、 更新、 刪除） 您建立儲存機制類別中的方法。</span><span class="sxs-lookup"><span data-stu-id="a59b2-125">The interface defines one method for each of the CRUD (create, read, update, delete) methods that you created in the repository class.</span></span>

<span data-ttu-id="a59b2-126">在`SchoolRepository`類別*SchoolRepository.cs*，指出這個類別會實作`ISchoolRepository`介面：</span><span class="sxs-lookup"><span data-stu-id="a59b2-126">In the `SchoolRepository` class in *SchoolRepository.cs*, indicate that this class implements the `ISchoolRepository` interface:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a><span data-ttu-id="a59b2-127">建立商務邏輯類別</span><span class="sxs-lookup"><span data-stu-id="a59b2-127">Creating a Business-Logic Class</span></span>

<span data-ttu-id="a59b2-128">接下來，您將建立商務邏輯類別。</span><span class="sxs-lookup"><span data-stu-id="a59b2-128">Next, you'll create the business-logic class.</span></span> <span data-ttu-id="a59b2-129">您執行這項操作，讓您可以新增將由執行的商務邏輯`ObjectDataSource`控制，您將不會執行，尚未雖然。</span><span class="sxs-lookup"><span data-stu-id="a59b2-129">You do this so that you can add business logic that will be executed by the `ObjectDataSource` control, although you will not do that yet.</span></span> <span data-ttu-id="a59b2-130">現在，新的商務邏輯類別只會執行相同的 CRUD 作業的儲存機制。</span><span class="sxs-lookup"><span data-stu-id="a59b2-130">For now, the new business-logic class will only perform the same CRUD operations that the repository does.</span></span>

<span data-ttu-id="a59b2-131">[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a59b2-131">[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)</span></span>

<span data-ttu-id="a59b2-132">建立新的資料夾並將其命名*BLL*。</span><span class="sxs-lookup"><span data-stu-id="a59b2-132">Create a new folder and name it *BLL*.</span></span> <span data-ttu-id="a59b2-133">(在真實世界應用程式中，商務邏輯層會通常會實作為類別程式庫 — 個別的專案，但為了簡化這個教學課程，BLL 類別將會保留在專案資料夾。)</span><span class="sxs-lookup"><span data-stu-id="a59b2-133">(In a real-world application, the business-logic layer would typically be implemented as a class library — a separate project — but to keep this tutorial simple, BLL classes will be kept in a project folder.)</span></span>

<span data-ttu-id="a59b2-134">在*BLL*資料夾中，建立新的類別檔案，其命名*SchoolBL.cs*，並以下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="a59b2-134">In the *BLL* folder, create a new class file, name it *SchoolBL.cs*, and replace the existing code with the following code:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

<span data-ttu-id="a59b2-135">此程式碼會建立您所見的相同 CRUD 方法稍早在儲存機制類別中，但而不是直接存取 Entity Framework 方法，它會呼叫在儲存機制類別方法。</span><span class="sxs-lookup"><span data-stu-id="a59b2-135">This code creates the same CRUD methods you saw earlier in the repository class, but instead of accessing the Entity Framework methods directly, it calls the repository class methods.</span></span>

<span data-ttu-id="a59b2-136">保存至儲存機制類別的參考類別變數定義為介面類型，並儲存機制類別具現化的程式碼包含兩個建構函式中。</span><span class="sxs-lookup"><span data-stu-id="a59b2-136">The class variable that holds a reference to the repository class is defined as an interface type, and the code that instantiates the repository class is contained in two constructors.</span></span> <span data-ttu-id="a59b2-137">無參數建構函式將會使用`ObjectDataSource`控制項。</span><span class="sxs-lookup"><span data-stu-id="a59b2-137">The parameterless constructor will be used by the `ObjectDataSource` control.</span></span> <span data-ttu-id="a59b2-138">它會建立的執行個體`SchoolRepository`您稍早建立的類別。</span><span class="sxs-lookup"><span data-stu-id="a59b2-138">It creates an instance of the `SchoolRepository` class that you created earlier.</span></span> <span data-ttu-id="a59b2-139">其他建構函式可讓任何商務邏輯類別，以在任何實作儲存機制介面的物件具現化的程式碼。</span><span class="sxs-lookup"><span data-stu-id="a59b2-139">The other constructor allows whatever code that instantiates the business-logic class to pass in any object that implements the repository interface.</span></span>

<span data-ttu-id="a59b2-140">儲存機制類別和兩個建構函式呼叫的 CRUD 方法可讓您選擇任何後端資料存放區中使用商務邏輯類別。</span><span class="sxs-lookup"><span data-stu-id="a59b2-140">The CRUD methods that call the repository class and the two constructors make it possible to use the business-logic class with whatever back-end data store you choose.</span></span> <span data-ttu-id="a59b2-141">商務邏輯類別不需要知道它正在呼叫的類別將資料的保存。</span><span class="sxs-lookup"><span data-stu-id="a59b2-141">The business-logic class does not need to be aware of how the class that it's calling persists the data.</span></span> <span data-ttu-id="a59b2-142">(這通常稱為*永續性無知之*。)這有助於單元測試，因為您可以連接使用的項目為簡單的儲存機制實作商務邏輯類別做為記憶體中`List`來儲存資料的集合。</span><span class="sxs-lookup"><span data-stu-id="a59b2-142">(This is often called *persistence ignorance*.) This facilitates unit testing, because you can connect the business-logic class to a repository implementation that uses something as simple as in-memory `List` collections to store data.</span></span>

> [!NOTE]
> <span data-ttu-id="a59b2-143">實體物件技術上來說，是仍不持續性-不知道，因為它們正在執行個體化的類別繼承自 Entity Framework`EntityObject`類別。</span><span class="sxs-lookup"><span data-stu-id="a59b2-143">Technically, the entity objects are still not persistence-ignorant, because they're instantiated from classes that inherit from the Entity Framework's `EntityObject` class.</span></span> <span data-ttu-id="a59b2-144">您可以使用完整的持續性忽略*純舊 CLR 物件*，或*POCOs*，來取代繼承的物件`EntityObject`類別。</span><span class="sxs-lookup"><span data-stu-id="a59b2-144">For complete persistence ignorance, you can use *plain old CLR objects*, or *POCOs*, in place of objects that inherit from the `EntityObject` class.</span></span> <span data-ttu-id="a59b2-145">使用 POCOs 超出本教學課程的範圍。</span><span class="sxs-lookup"><span data-stu-id="a59b2-145">Using POCOs is beyond the scope of this tutorial.</span></span> <span data-ttu-id="a59b2-146">如需詳細資訊，請參閱[可測試性和 Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx) MSDN 網站上。)</span><span class="sxs-lookup"><span data-stu-id="a59b2-146">For more information, see [Testability and Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx) on the MSDN website.)</span></span>


<span data-ttu-id="a59b2-147">現在您可以連接`ObjectDataSource`控制項加入至商務邏輯類別而不是至儲存機制，並確認一切運作與以前一樣。</span><span class="sxs-lookup"><span data-stu-id="a59b2-147">Now you can connect the `ObjectDataSource` controls to the business-logic class instead of to the repository and verify that everything works as it did before.</span></span>

<span data-ttu-id="a59b2-148">在*Departments.aspx*和*DepartmentsAdd.aspx*，變更出現的每個`TypeName="ContosoUniversity.DAL.SchoolRepository"`至`TypeName="ContosoUniversity.BLL.SchoolBL`"。</span><span class="sxs-lookup"><span data-stu-id="a59b2-148">In *Departments.aspx* and *DepartmentsAdd.aspx*, change each occurrence of `TypeName="ContosoUniversity.DAL.SchoolRepository"` to `TypeName="ContosoUniversity.BLL.SchoolBL`".</span></span> <span data-ttu-id="a59b2-149">（有四個執行個體中所有。）</span><span class="sxs-lookup"><span data-stu-id="a59b2-149">(There are four instances in all.)</span></span>

<span data-ttu-id="a59b2-150">執行*Departments.aspx*和*DepartmentsAdd.aspx*頁面，以確認它們仍然運作之前一樣。</span><span class="sxs-lookup"><span data-stu-id="a59b2-150">Run the *Departments.aspx* and *DepartmentsAdd.aspx* pages to verify that they still work as they did before.</span></span>

<span data-ttu-id="a59b2-151">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="a59b2-151">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)</span></span>

<span data-ttu-id="a59b2-152">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="a59b2-152">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)</span></span>

## <a name="creating-a-unit-test-project-and-repository-implementation"></a><span data-ttu-id="a59b2-153">建立單元測試專案和儲存機制實作</span><span class="sxs-lookup"><span data-stu-id="a59b2-153">Creating a Unit-Test Project and Repository Implementation</span></span>

<span data-ttu-id="a59b2-154">將新的專案加入方案中使用**測試專案**範本，並將其命名`ContosoUniversity.Tests`。</span><span class="sxs-lookup"><span data-stu-id="a59b2-154">Add a new project to the solution using the **Test Project** template, and name it `ContosoUniversity.Tests`.</span></span>

<span data-ttu-id="a59b2-155">在測試專案中，加入參考`System.Data.Entity`並加入專案參考`ContosoUniversity`專案。</span><span class="sxs-lookup"><span data-stu-id="a59b2-155">In the test project, add a reference to `System.Data.Entity` and add a project reference to the `ContosoUniversity` project.</span></span>

<span data-ttu-id="a59b2-156">您現在可以建立包含單元測試，您將使用的儲存機制類別。</span><span class="sxs-lookup"><span data-stu-id="a59b2-156">You can now create the repository class that you'll use with unit tests.</span></span> <span data-ttu-id="a59b2-157">這個儲存機制的資料存放區會在類別中。</span><span class="sxs-lookup"><span data-stu-id="a59b2-157">The data store for this repository will be within the class.</span></span>

<span data-ttu-id="a59b2-158">[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="a59b2-158">[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)</span></span>

<span data-ttu-id="a59b2-159">在測試專案中，建立新的類別檔案，其命名*MockSchoolRepository.cs*，並以下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="a59b2-159">In the test project, create a new class file, name it *MockSchoolRepository.cs*, and replace the existing code with the following code:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

<span data-ttu-id="a59b2-160">這個儲存機制類別具有相同的 CRUD 方法，直接存取 Entity Framework，但可使用`List`與資料庫而不是記憶體中的集合。</span><span class="sxs-lookup"><span data-stu-id="a59b2-160">This repository class has the same CRUD methods as the one that accesses the Entity Framework directly, but they work with `List` collections in memory instead of with a database.</span></span> <span data-ttu-id="a59b2-161">這項功能可更容易設定和驗證商務邏輯類別的單元測試的測試類別。</span><span class="sxs-lookup"><span data-stu-id="a59b2-161">This makes it easier for a test class to set up and validate unit tests for the business-logic class.</span></span>

## <a name="creating-unit-tests"></a><span data-ttu-id="a59b2-162">建立單元測試</span><span class="sxs-lookup"><span data-stu-id="a59b2-162">Creating Unit Tests</span></span>

<span data-ttu-id="a59b2-163">**測試**專案範本建立虛設常式單元測試類別，和您的下一個工作是由您想要加入至商務邏輯類別的商務邏輯加入單元測試方法，以修改此類別。</span><span class="sxs-lookup"><span data-stu-id="a59b2-163">The **Test** project template created a stub unit test class for you, and your next task is to modify this class by adding unit test methods to it for business logic that you want to add to the business-logic class.</span></span>

<span data-ttu-id="a59b2-164">[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="a59b2-164">[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)</span></span>

<span data-ttu-id="a59b2-165">在 Contoso 大學任何個別講師只能是單一部門的系統管理員，而且您需要加入商務邏輯來強制執行此規則。</span><span class="sxs-lookup"><span data-stu-id="a59b2-165">At Contoso University, any individual instructor can only be the administrator of a single department, and you need to add business logic to enforce this rule.</span></span> <span data-ttu-id="a59b2-166">一開始會加入測試及執行測試以查看失敗。</span><span class="sxs-lookup"><span data-stu-id="a59b2-166">You will start by adding tests and running the tests to see them fail.</span></span> <span data-ttu-id="a59b2-167">然後將加入程式碼，並重新執行測試，預期看到測試成功。</span><span class="sxs-lookup"><span data-stu-id="a59b2-167">You'll then add the code and rerun the tests, expecting to see them pass.</span></span>

<span data-ttu-id="a59b2-168">開啟*UnitTest1.cs*檔案，然後加入`using`您 ContosoUniversity 專案中建立商務邏輯和資料存取層級的陳述式：</span><span class="sxs-lookup"><span data-stu-id="a59b2-168">Open the *UnitTest1.cs* file and add `using` statements for the business logic and data-access layers that you created in the ContosoUniversity project:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

<span data-ttu-id="a59b2-169">取代`TestMethod1`方法使用下列方法：</span><span class="sxs-lookup"><span data-stu-id="a59b2-169">Replace the `TestMethod1` method with the following methods:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

<span data-ttu-id="a59b2-170">`CreateSchoolBL`方法會建立儲存機制建立的類別，您的單元測試專案，然後將傳遞至商務邏輯類別的新執行個體的執行個體。</span><span class="sxs-lookup"><span data-stu-id="a59b2-170">The `CreateSchoolBL` method creates an instance of the repository class that you created for the unit test project, which it then passes to a new instance of the business-logic class.</span></span> <span data-ttu-id="a59b2-171">然後，此方法會插入測試方法中，您可以使用三個部門使用商務邏輯類別。</span><span class="sxs-lookup"><span data-stu-id="a59b2-171">The method then uses the business-logic class to insert three departments that you can use in test methods.</span></span>

<span data-ttu-id="a59b2-172">測試方法確認有人嘗試插入新的部門由相同的系統管理員為現有的部門，或當有人嘗試更新部門的系統管理員設定的個人識別碼為商務邏輯類別擲回例外狀況使用者已經是另一個部門的系統管理員。</span><span class="sxs-lookup"><span data-stu-id="a59b2-172">The test methods verify that the business-logic class throws an exception if someone tries to insert a new department with the same administrator as an existing department, or if someone tries to update a department's administrator by setting it to the ID of a person who is already the administrator of another department.</span></span>

<span data-ttu-id="a59b2-173">例外狀況類別尚未建立，因此將不會編譯此程式碼。</span><span class="sxs-lookup"><span data-stu-id="a59b2-173">You haven't created the exception class yet, so this code will not compile.</span></span> <span data-ttu-id="a59b2-174">若要取得其進行編譯，以滑鼠右鍵按一下`DuplicateAdministratorException`選取**產生**，然後**類別**。</span><span class="sxs-lookup"><span data-stu-id="a59b2-174">To get it to compile, right-click `DuplicateAdministratorException` and select **Generate**, and then **Class**.</span></span>

<span data-ttu-id="a59b2-175">[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="a59b2-175">[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)</span></span>

<span data-ttu-id="a59b2-176">這會在您可以刪除的測試專案建立類別主要專案中建立例外狀況類別之後。</span><span class="sxs-lookup"><span data-stu-id="a59b2-176">This creates a class in the test project which you can delete after you've created the exception class in the main project.</span></span> <span data-ttu-id="a59b2-177">實作商務邏輯。</span><span class="sxs-lookup"><span data-stu-id="a59b2-177">and implemented the business logic.</span></span>

<span data-ttu-id="a59b2-178">執行測試專案。</span><span class="sxs-lookup"><span data-stu-id="a59b2-178">Run the test project.</span></span> <span data-ttu-id="a59b2-179">如預期般，測試就會失敗。</span><span class="sxs-lookup"><span data-stu-id="a59b2-179">As expected, the tests fail.</span></span>

<span data-ttu-id="a59b2-180">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="a59b2-180">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)</span></span>

## <a name="adding-business-logic-to-make-a-test-pass"></a><span data-ttu-id="a59b2-181">加入商務邏輯來進行測試</span><span class="sxs-lookup"><span data-stu-id="a59b2-181">Adding Business Logic to Make a Test Pass</span></span>

<span data-ttu-id="a59b2-182">接下來，您將實作商務邏輯，可讓您更難以將設定為部門的系統管理員已經是另一個部門的系統管理員的人。</span><span class="sxs-lookup"><span data-stu-id="a59b2-182">Next, you'll implement the business logic that makes it impossible to set as the administrator of a department someone who is already administrator of another department.</span></span> <span data-ttu-id="a59b2-183">您將會擲回例外狀況於商務邏輯層，並再加以攔截，展示層如果使用者編輯部門，並按一下**更新**之後選取已經是系統管理員的人。</span><span class="sxs-lookup"><span data-stu-id="a59b2-183">You'll throw an exception from the business-logic layer, and then catch it in the presentation layer if a user edits a department and clicks **Update** after selecting someone who is already an administrator.</span></span> <span data-ttu-id="a59b2-184">（您也無法從已系統管理員，才能轉譯頁面上，下拉式清單中移除講師但這裡的用途是要使用商務邏輯層）。</span><span class="sxs-lookup"><span data-stu-id="a59b2-184">(You could also remove instructors from the drop-down list who are already administrators before you render the page, but the purpose here is to work with the business-logic layer.)</span></span>

<span data-ttu-id="a59b2-185">開始建立當使用者嘗試進行講師多個部門的系統管理員時，會擲回的例外狀況類別。</span><span class="sxs-lookup"><span data-stu-id="a59b2-185">Start by creating the exception class that you'll throw when a user tries to make an instructor the administrator of more than one department.</span></span> <span data-ttu-id="a59b2-186">在主專案中，建立新的類別檔案中*BLL*資料夾中，其命名*DuplicateAdministratorException.cs*，並以下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="a59b2-186">In the main project, create a new class file in the *BLL* folder, name it *DuplicateAdministratorException.cs*, and replace the existing code with the following code:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

<span data-ttu-id="a59b2-187">現在刪除暫存*DuplicateAdministratorException.cs*您稍早建立的測試專案中才能編譯的檔案。</span><span class="sxs-lookup"><span data-stu-id="a59b2-187">Now delete the temporary *DuplicateAdministratorException.cs* file that you created in the test project earlier in order to be able to compile.</span></span>

<span data-ttu-id="a59b2-188">在主專案中，開啟*SchoolBL.cs*檔案，然後加入下列方法，其中包含驗證邏輯。</span><span class="sxs-lookup"><span data-stu-id="a59b2-188">In the main project, open the *SchoolBL.cs* file and add the following method that contains the validation logic.</span></span> <span data-ttu-id="a59b2-189">（程式碼參照的方法，您將在稍後建立）。</span><span class="sxs-lookup"><span data-stu-id="a59b2-189">(The code refers to a method that you'll create later.)</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

<span data-ttu-id="a59b2-190">當您要插入或更新時，會呼叫這個方法`Department`實體，以檢查另一個部門是否已經有相同的系統管理員。</span><span class="sxs-lookup"><span data-stu-id="a59b2-190">You'll call this method when you're inserting or updating `Department` entities in order to check whether another department already has the same administrator.</span></span>

<span data-ttu-id="a59b2-191">程式碼會呼叫方法來搜尋資料庫`Department`具有相同的實體`Administrator`插入或更新與實體的屬性值。</span><span class="sxs-lookup"><span data-stu-id="a59b2-191">The code calls a method to search the database for a `Department` entity that has the same `Administrator` property value as the entity being inserted or updated.</span></span> <span data-ttu-id="a59b2-192">如果找到，則程式碼擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a59b2-192">If one is found, the code throws an exception.</span></span> <span data-ttu-id="a59b2-193">如果正在插入或更新的實體沒有任何驗證檢查是必要`Administrator`如果在更新期間呼叫的方法擲回的值和任何例外狀況和`Department`找到相符項目實體`Department`正在更新實體。</span><span class="sxs-lookup"><span data-stu-id="a59b2-193">No validation check is required if the entity being inserted or updated has no `Administrator` value, and no exception is thrown if the method is called during an update and the `Department` entity found matches the `Department` entity being updated.</span></span>

<span data-ttu-id="a59b2-194">呼叫的新方法，從`Insert`和`Update`方法：</span><span class="sxs-lookup"><span data-stu-id="a59b2-194">Call the new method from the `Insert` and `Update` methods:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

<span data-ttu-id="a59b2-195">在*ISchoolRepository.cs*，加入新的資料存取方法的下列宣告：</span><span class="sxs-lookup"><span data-stu-id="a59b2-195">In *ISchoolRepository.cs*, add the following declaration for the new data-access method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

<span data-ttu-id="a59b2-196">在*SchoolRepository.cs*，加入下列`using`陳述式：</span><span class="sxs-lookup"><span data-stu-id="a59b2-196">In *SchoolRepository.cs*, add the following `using` statement:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

<span data-ttu-id="a59b2-197">在*SchoolRepository.cs*，新增下列新的資料存取方法：</span><span class="sxs-lookup"><span data-stu-id="a59b2-197">In *SchoolRepository.cs*, add the following new data-access method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

<span data-ttu-id="a59b2-198">此程式碼會擷取`Department`具有指定的系統管理員的實體。</span><span class="sxs-lookup"><span data-stu-id="a59b2-198">This code retrieves `Department` entities that have a specified administrator.</span></span> <span data-ttu-id="a59b2-199">（如果有的話），則應該找到只能有一個部門。</span><span class="sxs-lookup"><span data-stu-id="a59b2-199">Only one department should be found (if any).</span></span> <span data-ttu-id="a59b2-200">不過，因為沒有條件約束內建資料庫，傳回型別是集合萬一發現多個部門。</span><span class="sxs-lookup"><span data-stu-id="a59b2-200">However, because no constraint is built into the database, the return type is a collection in case multiple departments are found.</span></span>

<span data-ttu-id="a59b2-201">根據預設，物件內容從資料庫擷取實體時，它會追蹤的它們在其物件狀態管理員。</span><span class="sxs-lookup"><span data-stu-id="a59b2-201">By default, when the object context retrieves entities from the database, it keeps track of them in its object state manager.</span></span> <span data-ttu-id="a59b2-202">`MergeOption.NoTracking`參數會指定此追蹤不會完成此查詢。</span><span class="sxs-lookup"><span data-stu-id="a59b2-202">The `MergeOption.NoTracking` parameter specifies that this tracking will not be done for this query.</span></span> <span data-ttu-id="a59b2-203">這是必要的因為查詢可能會傳回您想要更新的確切實體，然後您就無法將該實體。</span><span class="sxs-lookup"><span data-stu-id="a59b2-203">This is necessary because the query might return the exact entity that you're trying to update, and then you would not be able to attach that entity.</span></span> <span data-ttu-id="a59b2-204">例如，如果您編輯中的歷程記錄部門*Departments.aspx*頁面並保留不變的系統管理員，此查詢會傳回記錄部門。</span><span class="sxs-lookup"><span data-stu-id="a59b2-204">For example, if you edit the History department in the *Departments.aspx* page and leave the administrator unchanged, this query will return the History department.</span></span> <span data-ttu-id="a59b2-205">如果`NoTracking`未設定，內容物件會在其物件狀態管理員中已經有記錄 department 實體。</span><span class="sxs-lookup"><span data-stu-id="a59b2-205">If `NoTracking` is not set, the object context would already have the History department entity in its object state manager.</span></span> <span data-ttu-id="a59b2-206">然後物件內容附加從檢視狀態時，都會重新建立歷程記錄 department 實體時，會擲回例外狀況指出`"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`。</span><span class="sxs-lookup"><span data-stu-id="a59b2-206">Then when you attach the History department entity that's re-created from view state, the object context would throw an exception that says `"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`.</span></span>

<span data-ttu-id="a59b2-207">(若要指定替代`MergeOption.NoTracking`，您可以建立新的物件內容，只針對此查詢。</span><span class="sxs-lookup"><span data-stu-id="a59b2-207">(As an alternative to specifying `MergeOption.NoTracking`, you could create a new object context just for this query.</span></span> <span data-ttu-id="a59b2-208">因為新的物件內容會有它自己的物件狀態管理員，會有任何衝突時您呼叫`Attach`方法。</span><span class="sxs-lookup"><span data-stu-id="a59b2-208">Because the new object context would have its own object state manager, there would be no conflict when you call the `Attach` method.</span></span> <span data-ttu-id="a59b2-209">新的物件內容會與原始的物件內容中，共用中繼資料和資料庫連接，因此這個替代方法的效能負面影響很小。</span><span class="sxs-lookup"><span data-stu-id="a59b2-209">The new object context would share metadata and database connection with the original object context, so the performance penalty of this alternate approach would be minimal.</span></span> <span data-ttu-id="a59b2-210">方法在這裡顯示，不過，導入了`NoTracking`選項，您會發現在其他內容中很有用的。</span><span class="sxs-lookup"><span data-stu-id="a59b2-210">The approach shown here, however, introduces the `NoTracking` option, which you'll find useful in other contexts.</span></span> <span data-ttu-id="a59b2-211">`NoTracking`選項會討論在這一系列之後的教學課程中進一步。)</span><span class="sxs-lookup"><span data-stu-id="a59b2-211">The `NoTracking` option is discussed further in a later tutorial in this series.)</span></span>

<span data-ttu-id="a59b2-212">在測試專案中，加入新的資料存取方法*MockSchoolRepository.cs*:</span><span class="sxs-lookup"><span data-stu-id="a59b2-212">In the test project, add the new data-access method to *MockSchoolRepository.cs*:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

<span data-ttu-id="a59b2-213">此程式碼使用 LINQ 來執行相同的資料選取範圍，`ContosoUniversity`專案儲存機制使用 LINQ to Entities 的。</span><span class="sxs-lookup"><span data-stu-id="a59b2-213">This code uses LINQ to perform the same data selection that the `ContosoUniversity` project repository uses LINQ to Entities for.</span></span>

<span data-ttu-id="a59b2-214">再次執行測試專案。</span><span class="sxs-lookup"><span data-stu-id="a59b2-214">Run the test project again.</span></span> <span data-ttu-id="a59b2-215">測試這一次會成功。</span><span class="sxs-lookup"><span data-stu-id="a59b2-215">This time the tests pass.</span></span>

<span data-ttu-id="a59b2-216">[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="a59b2-216">[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)</span></span>

## <a name="handling-objectdatasource-exceptions"></a><span data-ttu-id="a59b2-217">ObjectDataSource 例外狀況處理</span><span class="sxs-lookup"><span data-stu-id="a59b2-217">Handling ObjectDataSource Exceptions</span></span>

<span data-ttu-id="a59b2-218">在`ContosoUniversity`專案中，執行*Departments.aspx*頁面上，然後再次嘗試變更部門的系統管理員已經是另一個部門的系統管理員的人。</span><span class="sxs-lookup"><span data-stu-id="a59b2-218">In the `ContosoUniversity` project, run the *Departments.aspx* page and try to change the administrator for a department to someone who is already administrator for another department.</span></span> <span data-ttu-id="a59b2-219">（請記住，您可以只進行編輯期間本教學課程中，新增的部門因為資料庫會恢復預先載入具有無效的資料）。您會得到下列伺服器錯誤頁面：</span><span class="sxs-lookup"><span data-stu-id="a59b2-219">(Remember that you can only edit departments that you added during this tutorial, because the database comes preloaded with invalid data.) You get the following server error page:</span></span>

<span data-ttu-id="a59b2-220">[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="a59b2-220">[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)</span></span>

<span data-ttu-id="a59b2-221">您不希望使用者看到這種錯誤 頁面上，因此您需要加入錯誤處理程式碼。</span><span class="sxs-lookup"><span data-stu-id="a59b2-221">You don't want users to see this kind of error page, so you need to add error-handling code.</span></span> <span data-ttu-id="a59b2-222">開啟*Departments.aspx*和指定的處理常式`OnUpdated`事件`DepartmentsObjectDataSource`。</span><span class="sxs-lookup"><span data-stu-id="a59b2-222">Open *Departments.aspx* and specify a handler for the `OnUpdated` event of the `DepartmentsObjectDataSource`.</span></span> <span data-ttu-id="a59b2-223">`ObjectDataSource`現在開頭標記類似下列的範例。</span><span class="sxs-lookup"><span data-stu-id="a59b2-223">The `ObjectDataSource` opening tag now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

<span data-ttu-id="a59b2-224">在*Departments.aspx.cs*，加入下列`using`陳述式：</span><span class="sxs-lookup"><span data-stu-id="a59b2-224">In *Departments.aspx.cs*, add the following `using` statement:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

<span data-ttu-id="a59b2-225">加入下列處理常式`Updated`事件：</span><span class="sxs-lookup"><span data-stu-id="a59b2-225">Add the following handler for the `Updated` event:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

<span data-ttu-id="a59b2-226">如果`ObjectDataSource`控制項攔截例外狀況，當它嘗試執行更新時，事件引數中傳遞例外狀況 (`e`) 至這個處理常式。</span><span class="sxs-lookup"><span data-stu-id="a59b2-226">If the `ObjectDataSource` control catches an exception when it tries to perform the update, it passes the exception in the event argument (`e`) to this handler.</span></span> <span data-ttu-id="a59b2-227">此處理常式中的程式碼會檢查以查看例外狀況是否重複的系統管理員例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a59b2-227">The code in the handler checks to see if the exception is the duplicate administrator exception.</span></span> <span data-ttu-id="a59b2-228">如果是，程式碼會建立包含錯誤訊息的驗證程式控制項`ValidationSummary`控制項來顯示。</span><span class="sxs-lookup"><span data-stu-id="a59b2-228">If it is, the code creates a validator control that contains an error message for the `ValidationSummary` control to display.</span></span>

<span data-ttu-id="a59b2-229">執行網頁，並嘗試再次某人的兩個部門系統管理員。</span><span class="sxs-lookup"><span data-stu-id="a59b2-229">Run the page and attempt to make someone the administrator of two departments again.</span></span> <span data-ttu-id="a59b2-230">這次`ValidationSummary`控制項顯示的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="a59b2-230">This time the `ValidationSummary` control displays an error message.</span></span>

<span data-ttu-id="a59b2-231">[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="a59b2-231">[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)</span></span>

<span data-ttu-id="a59b2-232">類似變更*DepartmentsAdd.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="a59b2-232">Make similar changes to the *DepartmentsAdd.aspx* page.</span></span> <span data-ttu-id="a59b2-233">在*DepartmentsAdd.aspx*，指定的處理常式`OnInserted`事件`DepartmentsObjectDataSource`。</span><span class="sxs-lookup"><span data-stu-id="a59b2-233">In *DepartmentsAdd.aspx*, specify a handler for the `OnInserted` event of the `DepartmentsObjectDataSource`.</span></span> <span data-ttu-id="a59b2-234">產生的標記將類似下列的範例。</span><span class="sxs-lookup"><span data-stu-id="a59b2-234">The resulting markup will resemble the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

<span data-ttu-id="a59b2-235">在*DepartmentsAdd.aspx.cs*，加入相同`using`陳述式：</span><span class="sxs-lookup"><span data-stu-id="a59b2-235">In *DepartmentsAdd.aspx.cs*, add the same `using` statement:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

<span data-ttu-id="a59b2-236">加入下列事件處理常式：</span><span class="sxs-lookup"><span data-stu-id="a59b2-236">Add the following event handler:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

<span data-ttu-id="a59b2-237">您現在可以測試*DepartmentsAdd.aspx.cs*頁面，確認它也會正確處理嘗試讓一人以上部門的系統管理員。</span><span class="sxs-lookup"><span data-stu-id="a59b2-237">You can now test the *DepartmentsAdd.aspx.cs* page to verify that it also correctly handles attempts to make one person the administrator of more than one department.</span></span>

<span data-ttu-id="a59b2-238">如此即完成實作使用的儲存機制模式的介紹`ObjectDataSource`與 Entity Framework 的控制項。</span><span class="sxs-lookup"><span data-stu-id="a59b2-238">This completes the introduction to implementing the repository pattern for using the `ObjectDataSource` control with the Entity Framework.</span></span> <span data-ttu-id="a59b2-239">如需儲存機制模式和可測試性的詳細資訊，請參閱 MSDN 技術白皮書[可測試性和 Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx)。</span><span class="sxs-lookup"><span data-stu-id="a59b2-239">For more information about the repository pattern and testability, see the MSDN whitepaper [Testability and Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx).</span></span>

<span data-ttu-id="a59b2-240">下列教學課程中，您會看到如何加入排序和篩選應用程式的功能。</span><span class="sxs-lookup"><span data-stu-id="a59b2-240">In the following tutorial you'll see how to add sorting and filtering functionality to the application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a59b2-241">[上一頁](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
> [下一頁](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)</span><span class="sxs-lookup"><span data-stu-id="a59b2-241">[Previous](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
[Next](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)</span></span>
