---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: ASP.NET MVC 應用程式 (10 個 9) 中實作存放庫和工作單元模式 |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 應用程式...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9de8b33e4c533b90b7653544a6814d1ee756cf50
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814565"
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a><span data-ttu-id="2f212-103">ASP.NET MVC 應用程式 (10 個 9) 中實作存放庫和工作單元模式</span><span class="sxs-lookup"><span data-stu-id="2f212-103">Implementing the Repository and Unit of Work Patterns in an ASP.NET MVC Application (9 of 10)</span></span>
====================
<span data-ttu-id="2f212-104">藉由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="2f212-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="2f212-105">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="2f212-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="2f212-106">Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2f212-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="2f212-107">如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="2f212-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="2f212-108">您可以從頭開始教學課程系列或[下載入門專案，如本章](building-the-ef5-mvc4-chapter-downloads.md)並從這裡開始。</span><span class="sxs-lookup"><span data-stu-id="2f212-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="2f212-109">如果您遇到的問題，您無法解決，請[下載已完成的一章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現您的問題。</span><span class="sxs-lookup"><span data-stu-id="2f212-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="2f212-110">您通常可以找到問題的解決方案，藉由比較您的程式碼的完整程式碼。</span><span class="sxs-lookup"><span data-stu-id="2f212-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="2f212-111">一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="2f212-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="2f212-112">您可以先前的教學課程中使用繼承來減少多餘的程式碼中`Student`和`Instructor`實體類別。</span><span class="sxs-lookup"><span data-stu-id="2f212-112">In the previous tutorial you used inheritance to reduce redundant code in the `Student` and `Instructor` entity classes.</span></span> <span data-ttu-id="2f212-113">在本教學課程中，您會看到一些方法，若要使用的儲存機制和工作單元模式的 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="2f212-113">In this tutorial you'll see some ways to use the repository and unit of work patterns for CRUD operations.</span></span> <span data-ttu-id="2f212-114">如同先前的教學課程中，在這一個您要變更您的程式碼的運作的方式與頁面您已經建立，而非建立新的頁面。</span><span class="sxs-lookup"><span data-stu-id="2f212-114">As in the previous tutorial, in this one you'll change the way your code works with pages you already created rather than creating new pages.</span></span>

## <a name="the-repository-and-unit-of-work-patterns"></a><span data-ttu-id="2f212-115">存放庫和工作單元模式</span><span class="sxs-lookup"><span data-stu-id="2f212-115">The Repository and Unit of Work Patterns</span></span>

<span data-ttu-id="2f212-116">存放庫和工作單元模式被要建立資料存取層與應用程式的商務邏輯層之間的抽象層。</span><span class="sxs-lookup"><span data-stu-id="2f212-116">The repository and unit of work patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.</span></span> <span data-ttu-id="2f212-117">實作這些模式可協助隔離您的應用程式與資料存放區中的變更，並可促進自動化單元測試或測試驅動開發 (TDD)。</span><span class="sxs-lookup"><span data-stu-id="2f212-117">Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).</span></span>

<span data-ttu-id="2f212-118">在本教學課程中，您將實作每個實體類型的儲存機制類別。</span><span class="sxs-lookup"><span data-stu-id="2f212-118">In this tutorial you'll implement a repository class for each entity type.</span></span> <span data-ttu-id="2f212-119">針對`Student`實體類型，您將建立的儲存機制介面和儲存機制類別。</span><span class="sxs-lookup"><span data-stu-id="2f212-119">For the `Student` entity type you'll create a repository interface and a repository class.</span></span> <span data-ttu-id="2f212-120">當您具現化您的控制器中的存放庫會使用介面，讓控制器將會接受實作存放庫介面的任何物件的參考。</span><span class="sxs-lookup"><span data-stu-id="2f212-120">When you instantiate the repository in your controller, you'll use the interface so that the controller will accept a reference to any object that implements the repository interface.</span></span> <span data-ttu-id="2f212-121">當控制器會在 web 伺服器下執行時，它就會收到與 Entity Framework 搭配運作的存放庫。</span><span class="sxs-lookup"><span data-stu-id="2f212-121">When the controller runs under a web server, it receives a repository that works with the Entity Framework.</span></span> <span data-ttu-id="2f212-122">當控制器來執行單元測試類別時，它就會收到適用於您可以輕易地操作進行測試，例如記憶體中集合的方式儲存資料的儲存機制。</span><span class="sxs-lookup"><span data-stu-id="2f212-122">When the controller runs under a unit test class, it receives a repository that works with data stored in a way that you can easily manipulate for testing, such as an in-memory collection.</span></span>

<span data-ttu-id="2f212-123">稍後在本教學課程中，您將使用多個存放庫和工作類別的單位`Course`並`Department`中的實體型別`Course`控制站。</span><span class="sxs-lookup"><span data-stu-id="2f212-123">Later in the tutorial you'll use multiple repositories and a unit of work class for the `Course` and `Department` entity types in the `Course` controller.</span></span> <span data-ttu-id="2f212-124">工作類別的單位協調多個儲存機制的工作建立的所有人都共用的單一資料庫內容類別。</span><span class="sxs-lookup"><span data-stu-id="2f212-124">The unit of work class coordinates the work of multiple repositories by creating a single database context class shared by all of them.</span></span> <span data-ttu-id="2f212-125">如果您想要能夠執行自動化的單元測試，您會建立並使用這些類別的介面，在相同的方式，您對`Student`存放庫。</span><span class="sxs-lookup"><span data-stu-id="2f212-125">If you wanted to be able to perform automated unit testing, you'd create and use interfaces for these classes in the same way you did for the `Student` repository.</span></span> <span data-ttu-id="2f212-126">不過，為了簡化本教學課程，您會建立並使用這些類別，而不需要的介面。</span><span class="sxs-lookup"><span data-stu-id="2f212-126">However, to keep the tutorial simple, you'll create and use these classes without interfaces.</span></span>

<span data-ttu-id="2f212-127">下圖顯示一個概念化方式，控制站和相較於不使用的存放庫或工作單位模式完全的內容類別之間的關聯性。</span><span class="sxs-lookup"><span data-stu-id="2f212-127">The following illustration shows one way to conceptualize the relationships between the controller and context classes compared to not using the repository or unit of work pattern at all.</span></span>

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

<span data-ttu-id="2f212-129">在本教學課程系列中，您將不會建立單元測試。</span><span class="sxs-lookup"><span data-stu-id="2f212-129">You won't create unit tests in this tutorial series.</span></span> <span data-ttu-id="2f212-130">使用儲存機制模式的 MVC 應用程式使用 tdd 的簡介，請參閱[逐步解說： 使用 ASP.NET MVC 中 TDD](https://msdn.microsoft.com/library/ff847525.aspx)。</span><span class="sxs-lookup"><span data-stu-id="2f212-130">For an introduction to TDD with an MVC application that uses the repository pattern, see [Walkthrough: Using TDD with ASP.NET MVC](https://msdn.microsoft.com/library/ff847525.aspx).</span></span> <span data-ttu-id="2f212-131">如需儲存機制模式的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="2f212-131">For more information about the repository pattern, see the following resources:</span></span>

- <span data-ttu-id="2f212-132">[儲存機制模式](https://msdn.microsoft.com/library/ff649690.aspx)MSDN 上。</span><span class="sxs-lookup"><span data-stu-id="2f212-132">[The Repository Pattern](https://msdn.microsoft.com/library/ff649690.aspx) on MSDN.</span></span>
- <span data-ttu-id="2f212-133">[使用 Entity Framework 4.0 中的儲存機制和工作單位模式](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)Entity Framework 小組部落格上。</span><span class="sxs-lookup"><span data-stu-id="2f212-133">[Using Repository and Unit of Work patterns with Entity Framework 4.0](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) on the Entity Framework team blog.</span></span>
- <span data-ttu-id="2f212-134">[敏捷式軟體開發 Entity Framework 4 存放庫](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/)的一系列 Julie Lerman 的部落格上的文章。</span><span class="sxs-lookup"><span data-stu-id="2f212-134">[Agile Entity Framework 4 Repository](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) series of posts on Julie Lerman's blog.</span></span>
- <span data-ttu-id="2f212-135">[建置快速 HTML5/jQuery 應用程式的帳戶](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx)Dan Wahlin 部落格上。</span><span class="sxs-lookup"><span data-stu-id="2f212-135">[Building the Account at a Glance HTML5/jQuery Application](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) on Dan Wahlin's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="2f212-136">有許多方式可實作的存放庫和工作單元模式。</span><span class="sxs-lookup"><span data-stu-id="2f212-136">There are many ways to implement the repository and unit of work patterns.</span></span> <span data-ttu-id="2f212-137">您可以使用儲存機制類別，無論工作類別的單位。</span><span class="sxs-lookup"><span data-stu-id="2f212-137">You can use repository classes with or without a unit of work class.</span></span> <span data-ttu-id="2f212-138">您可以實作所有實體類型，或另一個用於每種類型的單一儲存機制。</span><span class="sxs-lookup"><span data-stu-id="2f212-138">You can implement a single repository for all entity types, or one for each type.</span></span> <span data-ttu-id="2f212-139">如果您實作每種類型一個，您可以使用個別的類別、 泛型基底類別衍生的類別，或抽象的基底類別和衍生的類別。</span><span class="sxs-lookup"><span data-stu-id="2f212-139">If you implement one for each type, you can use separate classes, a generic base class and derived classes, or an abstract base class and derived classes.</span></span> <span data-ttu-id="2f212-140">您可以納入您的存放庫中的商務邏輯，或將其限制資料存取邏輯。</span><span class="sxs-lookup"><span data-stu-id="2f212-140">You can include business logic in your repository or restrict it to data access logic.</span></span> <span data-ttu-id="2f212-141">您也可以建置到您的資料庫內容類別的抽象層，使用[IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx)那里介面而非[DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx)類型的實體集。</span><span class="sxs-lookup"><span data-stu-id="2f212-141">You can also build an abstraction layer into your database context class by using [IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx) interfaces there instead of [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) types for your entity sets.</span></span> <span data-ttu-id="2f212-142">若要實作的抽象層，本教學課程中，方法是您應考慮，不建議所有的案例和環境的其中一個選項。</span><span class="sxs-lookup"><span data-stu-id="2f212-142">The approach to implementing an abstraction layer shown in this tutorial is one option for you to consider, not a recommendation for all scenarios and environments.</span></span>


## <a name="creating-the-student-repository-class"></a><span data-ttu-id="2f212-143">建立學生儲存機制類別</span><span class="sxs-lookup"><span data-stu-id="2f212-143">Creating the Student Repository Class</span></span>

<span data-ttu-id="2f212-144">在  *DAL*資料夾中，建立名為的類別檔案*IStudentRepository.cs*並以下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="2f212-144">In the *DAL* folder, create a class file named *IStudentRepository.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="2f212-145">此程式碼會宣告一組典型 CRUD 方法，包括兩個讀取的方法，會傳回所有`Student`實體，以及尋找單一`Student`實體的識別碼。</span><span class="sxs-lookup"><span data-stu-id="2f212-145">This code declares a typical set of CRUD methods, including two read methods — one that returns all `Student` entities, and one that finds a single `Student` entity by ID.</span></span>

<span data-ttu-id="2f212-146">在  *DAL*資料夾中，建立名為的類別檔案*StudentRepository.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="2f212-146">In the *DAL* folder, create a class file named *StudentRepository.cs* file.</span></span> <span data-ttu-id="2f212-147">現有的程式碼取代為下列程式碼，它會實作`IStudentRepository`介面：</span><span class="sxs-lookup"><span data-stu-id="2f212-147">Replace the existing code with the following code, which implements the `IStudentRepository` interface:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="2f212-148">資料庫內容定義在類別變數中，並建構函式預期呼叫的物件，用於傳遞內容的執行個體中：</span><span class="sxs-lookup"><span data-stu-id="2f212-148">The database context is defined in a class variable, and the constructor expects the calling object to pass in an instance of the context:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

<span data-ttu-id="2f212-149">您可以具現化新的內容，在儲存機制，但然後如果您使用一個控制器中的多個存放庫時，每個會得到不同的內容。</span><span class="sxs-lookup"><span data-stu-id="2f212-149">You could instantiate a new context in the repository, but then if you used multiple repositories in one controller, each would end up with a separate context.</span></span> <span data-ttu-id="2f212-150">稍後您將使用中的多個存放庫`Course`控制站，而且您會看到如何工作類別的單位可以確保所有的存放庫會使用相同的內容。</span><span class="sxs-lookup"><span data-stu-id="2f212-150">Later you'll use multiple repositories in the `Course` controller, and you'll see how a unit of work class can ensure that all repositories use the same context.</span></span>

<span data-ttu-id="2f212-151">存放庫會實作[IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx)和處置的資料庫內容，因為您將看到稍早在控制器中，而其 CRUD 方法呼叫的資料庫內容中稍早所見的相同方式。</span><span class="sxs-lookup"><span data-stu-id="2f212-151">The repository implements [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) and disposes the database context as you saw earlier in the controller, and its CRUD methods make calls to the database context in the same way that you saw earlier.</span></span>

## <a name="change-the-student-controller-to-use-the-repository"></a><span data-ttu-id="2f212-152">變更學生控制器使用的儲存機制</span><span class="sxs-lookup"><span data-stu-id="2f212-152">Change the Student Controller to Use the Repository</span></span>

<span data-ttu-id="2f212-153">在  *StudentController.cs*，目前在類別中的程式碼取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="2f212-153">In *StudentController.cs*, replace the code currently in the class with the following code.</span></span> <span data-ttu-id="2f212-154">所做的變更已醒目提示。</span><span class="sxs-lookup"><span data-stu-id="2f212-154">The changes are highlighted.</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

<span data-ttu-id="2f212-155">控制器現在會宣告一個類別變數的物件，實作`IStudentRepository`介面，而不是內容類別：</span><span class="sxs-lookup"><span data-stu-id="2f212-155">The controller now declares a class variable for an object that implements the `IStudentRepository` interface instead of the context class:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

<span data-ttu-id="2f212-156">預設 （無參數） 建構函式會建立新的內容執行個體，並選擇性的建構函式可讓呼叫者傳入的內容執行個體。</span><span class="sxs-lookup"><span data-stu-id="2f212-156">The default (parameterless) constructor creates a new context instance, and an optional constructor allows the caller to pass in a context instance.</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

<span data-ttu-id="2f212-157">(如果您使用*相依性插入*，或 DI，您不需要預設建構函式，因為 DI 軟體可確保一律會提供正確的存放庫物件。)</span><span class="sxs-lookup"><span data-stu-id="2f212-157">(If you were using *dependency injection*, or DI, you wouldn't need the default constructor because the DI software would ensure that the correct repository object would always be provided.)</span></span>

<span data-ttu-id="2f212-158">在 CRUD 方法中，而非內容現在稱為 「 存放庫：</span><span class="sxs-lookup"><span data-stu-id="2f212-158">In the CRUD methods, the repository is now called instead of the context:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

<span data-ttu-id="2f212-159">和`Dispose`方法現在會處置而非內容存放庫：</span><span class="sxs-lookup"><span data-stu-id="2f212-159">And the `Dispose` method now disposes the repository instead of the context:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

<span data-ttu-id="2f212-160">執行站台，然後按一下**學生** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="2f212-160">Run the site and click the **Students** tab.</span></span>

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

<span data-ttu-id="2f212-162">頁面的外觀與之前變更程式碼以使用儲存機制，以及的其他學生頁面也使用相同的運作方式相同。</span><span class="sxs-lookup"><span data-stu-id="2f212-162">The page looks and works the same as it did before you changed the code to use the repository, and the other Student pages also work the same.</span></span> <span data-ttu-id="2f212-163">不過，還有一項重要差異方式`Index`控制器方法未篩選和排序。</span><span class="sxs-lookup"><span data-stu-id="2f212-163">However, there's an important difference in the way the `Index` method of the controller does filtering and ordering.</span></span> <span data-ttu-id="2f212-164">這個方法的原始版本包含下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="2f212-164">The original version of this method contained the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

<span data-ttu-id="2f212-165">已更新`Index`方法包含下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="2f212-165">The updated `Index` method contains the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

<span data-ttu-id="2f212-166">僅反白顯示的程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="2f212-166">Only the highlighted code has changed.</span></span>

<span data-ttu-id="2f212-167">在原始版本的程式碼中，`students`型別為`IQueryable`物件。</span><span class="sxs-lookup"><span data-stu-id="2f212-167">In the original version of the code, `students` is typed as an `IQueryable` object.</span></span> <span data-ttu-id="2f212-168">查詢不傳送至資料庫，直到它會轉換成集合，例如使用方法`ToList`，這並不會發生，直到 [索引] 檢視存取學生模型。</span><span class="sxs-lookup"><span data-stu-id="2f212-168">The query isn't sent to the database until it's converted into a collection using a method such as `ToList`, which doesn't occur until the Index view accesses the student model.</span></span> <span data-ttu-id="2f212-169">`Where`上面的原始程式碼的方法會變成`WHERE`傳送至資料庫的 SQL 查詢中的子句。</span><span class="sxs-lookup"><span data-stu-id="2f212-169">The `Where` method in the original code above becomes a `WHERE` clause in the SQL query that is sent to the database.</span></span> <span data-ttu-id="2f212-170">這又表示選取的實體，會傳回資料庫。</span><span class="sxs-lookup"><span data-stu-id="2f212-170">That in turn means that only the selected entities are returned by the database.</span></span> <span data-ttu-id="2f212-171">不過，因為變更而`context.Students`要`studentRepository.GetStudents()`，則`students`此陳述式之後的變數`IEnumerable`包含資料庫中的所有學生的集合。</span><span class="sxs-lookup"><span data-stu-id="2f212-171">However, as a result of changing `context.Students` to `studentRepository.GetStudents()`, the `students` variable after this statement is an `IEnumerable` collection that includes all students in the database.</span></span> <span data-ttu-id="2f212-172">套用的最終結果`Where`方法相同，但是現在在 web 伺服器上，而不是由資料庫的記憶體中完成工作。</span><span class="sxs-lookup"><span data-stu-id="2f212-172">The end result of applying the `Where` method is the same, but now the work is done in memory on the web server and not by the database.</span></span> <span data-ttu-id="2f212-173">傳回大量資料的查詢，這可能會沒有效率。</span><span class="sxs-lookup"><span data-stu-id="2f212-173">For queries that return large volumes of data, this can be inefficient.</span></span>

> [!TIP]
> 
> <span data-ttu-id="2f212-174">**IQueryable vs。IEnumerable**</span><span class="sxs-lookup"><span data-stu-id="2f212-174">**IQueryable vs. IEnumerable**</span></span>
> 
> <span data-ttu-id="2f212-175">實作存放庫如此處所示，即使您已輸入中的項目後**搜尋**方塊查詢傳送至 SQL Server 會傳回所有的學生資料列，因為它不包含您的搜尋準則：</span><span class="sxs-lookup"><span data-stu-id="2f212-175">After you implement the repository as shown here, even if you enter something in the **Search** box the query sent to SQL Server returns all Student rows because it doesn't include your search criteria:</span></span>
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> <span data-ttu-id="2f212-176">因為存放庫執行查詢，而不需要知道有關搜尋條件，此查詢會傳回所有的學生資料。</span><span class="sxs-lookup"><span data-stu-id="2f212-176">This query returns all of the student data because the repository executed the query without knowing about the search criteria.</span></span> <span data-ttu-id="2f212-177">排序、 套用的搜尋條件，和選取的資料子集 （在此情況下，顯示只有 3 個資料列） 的分頁在記憶體中的程序時，更新`ToPagedList`上呼叫方法`IEnumerable`集合。</span><span class="sxs-lookup"><span data-stu-id="2f212-177">The process of sorting, applying search criteria, and selecting a subset of the data for paging (showing only 3 rows in this case) is done in memory later when the `ToPagedList` method is called on the `IEnumerable` collection.</span></span>
> 
> <span data-ttu-id="2f212-178">在程式碼 （在您實作儲存機制） 之前的先前版本中，查詢不會傳送給資料庫之前套用的搜尋條件之後，當`ToPagedList`上呼叫`IQueryable`物件。</span><span class="sxs-lookup"><span data-stu-id="2f212-178">In the previous version of the code (before you implemented the repository), the query is not sent to the database until after you apply the search criteria, when `ToPagedList` is called on the `IQueryable` object.</span></span>
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> <span data-ttu-id="2f212-179">當上呼叫 ToPagedList`IQueryable`物件傳送至 SQL Server 查詢會指定搜尋字串，如此一來，系統會傳回符合搜尋準則的資料列，並沒有篩選必須在記憶體中完成。</span><span class="sxs-lookup"><span data-stu-id="2f212-179">When ToPagedList is called on an `IQueryable` object, the query sent to SQL Server specifies the search string, and as a result only rows that meet the search criteria are returned, and no filtering needs to be done in memory.</span></span>
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> <span data-ttu-id="2f212-180">（下列教學課程說明如何檢查傳送至 SQL Server 的查詢）。</span><span class="sxs-lookup"><span data-stu-id="2f212-180">(The following tutorial explains how to examine queries sent to SQL Server.)</span></span>


<span data-ttu-id="2f212-181">下一節示範如何實作存放庫方法，讓您指定資料庫應該完成此工作。</span><span class="sxs-lookup"><span data-stu-id="2f212-181">The following section shows how to implement repository methods that enable you to specify that this work should be done by the database.</span></span>

<span data-ttu-id="2f212-182">您現在已建立控制器與 Entity Framework 資料庫內容之間的抽象層。</span><span class="sxs-lookup"><span data-stu-id="2f212-182">You've now created an abstraction layer between the controller and the Entity Framework database context.</span></span> <span data-ttu-id="2f212-183">如果您要執行自動化的單元測試與此應用程式，您可以建立替代的儲存機制類別中實作的單元測試專案`IStudentRepository` *。*</span><span class="sxs-lookup"><span data-stu-id="2f212-183">If you were going to perform automated unit testing with this application, you could create an alternative repository class in a unit test project that implements `IStudentRepository`*.*</span></span> <span data-ttu-id="2f212-184">而不是呼叫要讀取和寫入資料的內容，此模擬儲存機制類別無法管理記憶體中集合，才能測試控制器的函式。</span><span class="sxs-lookup"><span data-stu-id="2f212-184">Instead of calling the context to read and write data, this mock repository class could manipulate in-memory collections in order to test controller functions.</span></span>

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a><span data-ttu-id="2f212-185">實作泛型儲存機制和單位的工作類別</span><span class="sxs-lookup"><span data-stu-id="2f212-185">Implement a Generic Repository and a Unit of Work Class</span></span>

<span data-ttu-id="2f212-186">建立每個實體類型的儲存機制類別可能會導致大量多餘的程式碼，並可能會造成部分更新。</span><span class="sxs-lookup"><span data-stu-id="2f212-186">Creating a repository class for each entity type could result in a lot of redundant code, and it could result in partial updates.</span></span> <span data-ttu-id="2f212-187">例如，假設您有更新的相同交易一部分的兩個不同的實體類型。</span><span class="sxs-lookup"><span data-stu-id="2f212-187">For example, suppose you have to update two different entity types as part of the same transaction.</span></span> <span data-ttu-id="2f212-188">如果每個使用不同的資料庫內容執行個體，其中可能會成功，而且其他可能會失敗。</span><span class="sxs-lookup"><span data-stu-id="2f212-188">If each uses a separate database context instance, one might succeed and the other might fail.</span></span> <span data-ttu-id="2f212-189">減少多餘的程式碼的方法之一是使用一般的存放庫和一個可確實的所有存放庫使用相同的資料庫內容 （並因此協調所有更新） 是使用工作類別的單位。</span><span class="sxs-lookup"><span data-stu-id="2f212-189">One way to minimize redundant code is to use a generic repository, and one way to ensure that all repositories use the same database context (and thus coordinate all updates) is to use a unit of work class.</span></span>

<span data-ttu-id="2f212-190">在本節的教學課程中，您將建立`GenericRepository`類別和`UnitOfWork`類別，並使用它們`Course`控制站來存取`Department`和`Course`實體集。</span><span class="sxs-lookup"><span data-stu-id="2f212-190">In this section of the tutorial, you'll create a `GenericRepository` class and a `UnitOfWork` class, and use them in the `Course` controller to access both the `Department` and the `Course` entity sets.</span></span> <span data-ttu-id="2f212-191">如先前所述，為了簡化本教學課程的這個部分，您沒有建立這些類別的介面。</span><span class="sxs-lookup"><span data-stu-id="2f212-191">As explained earlier, to keep this part of the tutorial simple, you aren't creating interfaces for these classes.</span></span> <span data-ttu-id="2f212-192">如果您要使用它們來促進 TDD，您會通常會實作這些介面使用相同的方式，但`Student`存放庫。</span><span class="sxs-lookup"><span data-stu-id="2f212-192">But if you were going to use them to facilitate TDD, you'd typically implement them with interfaces the same way you did the `Student` repository.</span></span>

### <a name="create-a-generic-repository"></a><span data-ttu-id="2f212-193">建立一般的存放庫</span><span class="sxs-lookup"><span data-stu-id="2f212-193">Create a Generic Repository</span></span>

<span data-ttu-id="2f212-194">在  *DAL*資料夾中，建立*GenericRepository.cs*並以下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="2f212-194">In the *DAL* folder, create *GenericRepository.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

<span data-ttu-id="2f212-195">資料庫內容和存放庫針對具現化的實體集，會宣告類別變數：</span><span class="sxs-lookup"><span data-stu-id="2f212-195">Class variables are declared for the database context and for the entity set that the repository is instantiated for:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

<span data-ttu-id="2f212-196">建構函式會接受資料庫內容執行個體，並初始化實體設定變數：</span><span class="sxs-lookup"><span data-stu-id="2f212-196">The constructor accepts a database context instance and initializes the entity set variable:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

<span data-ttu-id="2f212-197">`Get`方法使用 lambda 運算式，以允許呼叫的程式碼，以指定的篩選條件和資料行排序結果，以及字串參數可讓呼叫端提供的積極式載入導覽屬性以逗號分隔清單：</span><span class="sxs-lookup"><span data-stu-id="2f212-197">The `Get` method uses lambda expressions to allow the calling code to specify a filter condition and a column to order the results by, and a string parameter lets the caller provide a comma-delimited list of navigation properties for eager loading:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

<span data-ttu-id="2f212-198">程式碼`Expression<Func<TEntity, bool>> filter`表示呼叫端會提供 lambda 運算式，根據`TEntity`類型，以及此運算式會傳回布林值。</span><span class="sxs-lookup"><span data-stu-id="2f212-198">The code `Expression<Func<TEntity, bool>> filter` means the caller will provide a lambda expression based on the `TEntity` type, and this expression will return a Boolean value.</span></span> <span data-ttu-id="2f212-199">例如，如果存放庫針對具現化`Student`中呼叫方法的程式碼可能會指定實體類型`student => student.LastName == "Smith`&quot;如`filter`參數。</span><span class="sxs-lookup"><span data-stu-id="2f212-199">For example, if the repository is instantiated for the `Student` entity type, the code in the calling method might specify `student => student.LastName == "Smith`&quot; for the `filter` parameter.</span></span>

<span data-ttu-id="2f212-200">程式碼`Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy`也表示呼叫者會提供 lambda 運算式。</span><span class="sxs-lookup"><span data-stu-id="2f212-200">The code `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` also means the caller will provide a lambda expression.</span></span> <span data-ttu-id="2f212-201">但在此情況下，運算式的輸入`IQueryable`物件`TEntity`型別。</span><span class="sxs-lookup"><span data-stu-id="2f212-201">But in this case, the input to the expression is an `IQueryable` object for the `TEntity` type.</span></span> <span data-ttu-id="2f212-202">運算式將傳回的已排序的版本`IQueryable`物件。</span><span class="sxs-lookup"><span data-stu-id="2f212-202">The expression will return an ordered version of that `IQueryable` object.</span></span> <span data-ttu-id="2f212-203">例如，如果存放庫針對具現化`Student`中呼叫方法的程式碼可能會指定實體類型`q => q.OrderBy(s => s.LastName)`如`orderBy`參數。</span><span class="sxs-lookup"><span data-stu-id="2f212-203">For example, if the repository is instantiated for the `Student` entity type, the code in the calling method might specify `q => q.OrderBy(s => s.LastName)` for the `orderBy` parameter.</span></span>

<span data-ttu-id="2f212-204">中的程式碼`Get`方法會建立`IQueryable`物件，然後再套用篩選條件運算式，如果有一個：</span><span class="sxs-lookup"><span data-stu-id="2f212-204">The code in the `Get` method creates an `IQueryable` object and then applies the filter expression if there is one:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

<span data-ttu-id="2f212-205">接下來它積極式載入會將運算式套用之後剖析逗號分隔的清單：</span><span class="sxs-lookup"><span data-stu-id="2f212-205">Next it applies the eager-loading expressions after parsing the comma-delimited list:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

<span data-ttu-id="2f212-206">最後，它會套用`orderBy`運算式，如果有的話，並傳回結果，否則會傳回結果從排序的查詢：</span><span class="sxs-lookup"><span data-stu-id="2f212-206">Finally, it applies the `orderBy` expression if there is one and returns the results; otherwise it returns the results from the unordered query:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

<span data-ttu-id="2f212-207">當您呼叫`Get`方法中，您可以執行篩選和排序`IEnumerable`方法而非提供參數，這些函式所傳回的集合。</span><span class="sxs-lookup"><span data-stu-id="2f212-207">When you call the `Get` method, you could do filtering and sorting on the `IEnumerable` collection returned by the method instead of providing parameters for these functions.</span></span> <span data-ttu-id="2f212-208">但是，排序和篩選工作就會對執行 web 伺服器上的記憶體。</span><span class="sxs-lookup"><span data-stu-id="2f212-208">But the sorting and filtering work would then be done in memory on the web server.</span></span> <span data-ttu-id="2f212-209">藉由使用這些參數，您可以確保工作由資料庫，而不是 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="2f212-209">By using these parameters, you ensure that the work is done by the database rather than the web server.</span></span> <span data-ttu-id="2f212-210">替代方法是建立特定實體型別的衍生的類別並新增特製化`Get`方法，例如`GetStudentsInNameOrder`或`GetStudentsByName`。</span><span class="sxs-lookup"><span data-stu-id="2f212-210">An alternative is to create derived classes for specific entity types and add specialized `Get` methods, such as `GetStudentsInNameOrder` or `GetStudentsByName`.</span></span> <span data-ttu-id="2f212-211">不過，在複雜的應用程式，這可能會造成大量的這種特殊的方法，可能是更多工作來維護及衍生的類別中。</span><span class="sxs-lookup"><span data-stu-id="2f212-211">However, in a complex application, this can result in a large number of such derived classes and specialized methods, which could be more work to maintain.</span></span>

<span data-ttu-id="2f212-212">中的程式碼`GetByID`， `Insert`，和`Update`方法是類似於您所見的內容中的非泛型儲存機制。</span><span class="sxs-lookup"><span data-stu-id="2f212-212">The code in the `GetByID`, `Insert`, and `Update` methods is similar to what you saw in the non-generic repository.</span></span> <span data-ttu-id="2f212-213">(您未提供中的參數，積極式載入`GetByID`簽章，因為您無法使用積極式載入`Find`方法。)</span><span class="sxs-lookup"><span data-stu-id="2f212-213">(You aren't providing an eager loading parameter in the `GetByID` signature, because you can't do eager loading with the `Find` method.)</span></span>

<span data-ttu-id="2f212-214">兩個多載可供`Delete`方法：</span><span class="sxs-lookup"><span data-stu-id="2f212-214">Two overloads are provided for the `Delete` method:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

<span data-ttu-id="2f212-215">其中一個這些可讓您傳入只是實體的識別碼來刪除，而且另一個則採用實體執行個體。</span><span class="sxs-lookup"><span data-stu-id="2f212-215">One of these lets you pass in just the ID of the entity to be deleted, and one takes an entity instance.</span></span> <span data-ttu-id="2f212-216">如您所見中[處理並行](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)教學課程中，並行處理，您需要`Delete`方法會採用實體執行個體，包括追蹤屬性的原始值。</span><span class="sxs-lookup"><span data-stu-id="2f212-216">As you saw in the [Handling Concurrency](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial, for concurrency handling you need a `Delete` method that takes an entity instance that includes the original value of a tracking property.</span></span>

<span data-ttu-id="2f212-217">這個一般的存放庫會處理一般的 CRUD 需求。</span><span class="sxs-lookup"><span data-stu-id="2f212-217">This generic repository will handle typical CRUD requirements.</span></span> <span data-ttu-id="2f212-218">當特定的實體類型有特殊需求，例如更複雜的篩選或排序，您可以建立有額外的方法，該類型的衍生的類別。</span><span class="sxs-lookup"><span data-stu-id="2f212-218">When a particular entity type has special requirements, such as more complex filtering or ordering, you can create a derived class that has additional methods for that type.</span></span>

## <a name="creating-the-unit-of-work-class"></a><span data-ttu-id="2f212-219">建立工作類別的單位</span><span class="sxs-lookup"><span data-stu-id="2f212-219">Creating the Unit of Work Class</span></span>

<span data-ttu-id="2f212-220">工作類別的單位有一個用途： 若要確定，當您使用多個存放庫時，它們會共用單一資料庫的內容。</span><span class="sxs-lookup"><span data-stu-id="2f212-220">The unit of work class serves one purpose: to make sure that when you use multiple repositories, they share a single database context.</span></span> <span data-ttu-id="2f212-221">這樣一來，完成的工作單位時，您可以呼叫`SaveChanges`內容的執行個體上的方法，及保證所有相關的變更將會協調。</span><span class="sxs-lookup"><span data-stu-id="2f212-221">That way, when a unit of work is complete you can call the `SaveChanges` method on that instance of the context and be assured that all related changes will be coordinated.</span></span> <span data-ttu-id="2f212-222">所有這些類別的需求是`Save`方法和屬性，以每個存放庫。</span><span class="sxs-lookup"><span data-stu-id="2f212-222">All that the class needs is a `Save` method and a property for each repository.</span></span> <span data-ttu-id="2f212-223">每個存放庫屬性會傳回已具現化與其他存放庫執行個體使用相同的資料庫內容執行個體的存放庫執行個體。</span><span class="sxs-lookup"><span data-stu-id="2f212-223">Each repository property returns a repository instance that has been instantiated using the same database context instance as the other repository instances.</span></span>

<span data-ttu-id="2f212-224">在  *DAL*資料夾中，建立名為的類別檔案*UnitOfWork.cs*並以下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="2f212-224">In the *DAL* folder, create a class file named *UnitOfWork.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

<span data-ttu-id="2f212-225">程式碼會建立資料庫內容和每個存放庫的類別變數。</span><span class="sxs-lookup"><span data-stu-id="2f212-225">The code creates class variables for the database context and each repository.</span></span> <span data-ttu-id="2f212-226">針對`context`變數時，新的內容會具現化：</span><span class="sxs-lookup"><span data-stu-id="2f212-226">For the `context` variable, a new context is instantiated:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

<span data-ttu-id="2f212-227">每個存放庫的屬性會檢查是否已存在於存放庫。</span><span class="sxs-lookup"><span data-stu-id="2f212-227">Each repository property checks whether the repository already exists.</span></span> <span data-ttu-id="2f212-228">如果沒有，它會具現化的存放庫中，傳入內容執行個體。</span><span class="sxs-lookup"><span data-stu-id="2f212-228">If not, it instantiates the repository, passing in the context instance.</span></span> <span data-ttu-id="2f212-229">如此一來，所有存放庫都會共用相同的內容執行個體。</span><span class="sxs-lookup"><span data-stu-id="2f212-229">As a result, all repositories share the same context instance.</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

<span data-ttu-id="2f212-230">`Save`方法呼叫`SaveChanges`上的資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="2f212-230">The `Save` method calls `SaveChanges` on the database context.</span></span>

<span data-ttu-id="2f212-231">和在類別變數中，資料庫內容會具現化任何類別一樣`UnitOfWork`類別會實作`IDisposable`和處置內容。</span><span class="sxs-lookup"><span data-stu-id="2f212-231">Like any class that instantiates a database context in a class variable, the `UnitOfWork` class implements `IDisposable` and disposes the context.</span></span>

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a><span data-ttu-id="2f212-232">變更使用 UnitOfWork 類別和儲存機制的課程控制器</span><span class="sxs-lookup"><span data-stu-id="2f212-232">Changing the Course Controller to use the UnitOfWork Class and Repositories</span></span>

<span data-ttu-id="2f212-233">將程式碼中目前沒有*CourseController.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="2f212-233">Replace the code you currently have in *CourseController.cs* with the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

<span data-ttu-id="2f212-234">這個程式碼加入的類別變數`UnitOfWork`類別。</span><span class="sxs-lookup"><span data-stu-id="2f212-234">This code adds a class variable for the `UnitOfWork` class.</span></span> <span data-ttu-id="2f212-235">(如果您在這裡使用的介面，您不會初始化的變數; 相反地，您會實作兩個建構函式的模式就像您對`Student`存放庫。)</span><span class="sxs-lookup"><span data-stu-id="2f212-235">(If you were using interfaces here, you wouldn't initialize the variable here; instead, you'd implement a pattern of two constructors just as you did for the `Student` repository.)</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

<span data-ttu-id="2f212-236">在類別的其餘部分，所有參考的資料庫內容會以都取代參考適當的存放庫中，使用`UnitOfWork`屬性來存取儲存機制。</span><span class="sxs-lookup"><span data-stu-id="2f212-236">In the rest of the class, all references to the database context are replaced by references to the appropriate repository, using `UnitOfWork` properties to access the repository.</span></span> <span data-ttu-id="2f212-237">`Dispose`方法將處置`UnitOfWork`執行個體。</span><span class="sxs-lookup"><span data-stu-id="2f212-237">The `Dispose` method disposes the `UnitOfWork` instance.</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

<span data-ttu-id="2f212-238">執行站台，然後按一下**課程** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="2f212-238">Run the site and click the **Courses** tab.</span></span>

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="2f212-240">頁面的外觀與之前變更，以及其他課程頁面也使用相同的運作方式相同。</span><span class="sxs-lookup"><span data-stu-id="2f212-240">The page looks and works the same as it did before your changes, and the other Course pages also work the same.</span></span>

## <a name="summary"></a><span data-ttu-id="2f212-241">總結</span><span class="sxs-lookup"><span data-stu-id="2f212-241">Summary</span></span>

<span data-ttu-id="2f212-242">您現在已實作存放庫和工作單元模式。</span><span class="sxs-lookup"><span data-stu-id="2f212-242">You have now implemented both the repository and unit of work patterns.</span></span> <span data-ttu-id="2f212-243">您已使用 lambda 運算式做為一般的存放庫中的方法參數。</span><span class="sxs-lookup"><span data-stu-id="2f212-243">You have used lambda expressions as method parameters in the generic repository.</span></span> <span data-ttu-id="2f212-244">如需有關如何使用這些運算式搭配`IQueryable`物件，請參閱 < [IQueryable(T) 介面 (System.Linq)](https://msdn.microsoft.com/library/bb351562.aspx) MSDN Library 中。</span><span class="sxs-lookup"><span data-stu-id="2f212-244">For more information about how to use these expressions with an `IQueryable` object, see [IQueryable(T) Interface (System.Linq)](https://msdn.microsoft.com/library/bb351562.aspx) in the MSDN Library.</span></span> <span data-ttu-id="2f212-245">在下一個教學課程，您將了解如何處理一些進階的案例。</span><span class="sxs-lookup"><span data-stu-id="2f212-245">In the next tutorial you'll learn how to handle some advanced scenarios.</span></span>

<span data-ttu-id="2f212-246">其他 Entity Framework 資源連結可在[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="2f212-246">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2f212-247">[上一頁](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一頁](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="2f212-247">[Previous](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)</span></span>
