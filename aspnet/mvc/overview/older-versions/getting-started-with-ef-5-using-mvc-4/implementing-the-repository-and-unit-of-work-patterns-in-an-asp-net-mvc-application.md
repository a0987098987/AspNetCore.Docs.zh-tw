---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: "ASP.NET MVC 應用程式 (9 / 10) 中實作的儲存機制和單位的工作模式 |Microsoft 文件"
author: tdykstra
description: "Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: c920dc8defe18b6f27d122c2cd1a6c6ffdaad608
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a><span data-ttu-id="e3bff-103">ASP.NET MVC 應用程式 (9 / 10) 中實作的儲存機制和單位的工作模式</span><span class="sxs-lookup"><span data-stu-id="e3bff-103">Implementing the Repository and Unit of Work Patterns in an ASP.NET MVC Application (9 of 10)</span></span>
====================
<span data-ttu-id="e3bff-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="e3bff-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="e3bff-105">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="e3bff-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="e3bff-106">Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="e3bff-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="e3bff-107">教學課程系列的相關資訊，請參閱[系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="e3bff-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="e3bff-108">您可以從頭開始教學課程系列或[下載本章節的入門專案](building-the-ef5-mvc4-chapter-downloads.md)和從這裡開始。</span><span class="sxs-lookup"><span data-stu-id="e3bff-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="e3bff-109">如果您遇到的問題，您無法解決，[下載已完成的章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現問題。</span><span class="sxs-lookup"><span data-stu-id="e3bff-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="e3bff-110">您通常可以藉由比較您的程式碼完成的程式碼會發現問題的解決方案。</span><span class="sxs-lookup"><span data-stu-id="e3bff-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="e3bff-111">一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="e3bff-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="e3bff-112">在上一個教學課程中您可以使用繼承減少多餘的程式碼中`Student`和`Instructor`實體類別。</span><span class="sxs-lookup"><span data-stu-id="e3bff-112">In the previous tutorial you used inheritance to reduce redundant code in the `Student` and `Instructor` entity classes.</span></span> <span data-ttu-id="e3bff-113">在本教學課程中，您會看到一些 CRUD 作業使用的儲存機制和工作模式的單位。</span><span class="sxs-lookup"><span data-stu-id="e3bff-113">In this tutorial you'll see some ways to use the repository and unit of work patterns for CRUD operations.</span></span> <span data-ttu-id="e3bff-114">如同上一個教學課程中，在這一個您要變更您的程式碼的運作的方式與網頁您已經建立而非建立新的頁面。</span><span class="sxs-lookup"><span data-stu-id="e3bff-114">As in the previous tutorial, in this one you'll change the way your code works with pages you already created rather than creating new pages.</span></span>

## <a name="the-repository-and-unit-of-work-patterns"></a><span data-ttu-id="e3bff-115">儲存機制和單位的工作模式</span><span class="sxs-lookup"><span data-stu-id="e3bff-115">The Repository and Unit of Work Patterns</span></span>

<span data-ttu-id="e3bff-116">儲存機制和工作模式的單位被要建立資料存取層和應用程式的商務邏輯層之間的抽象層。</span><span class="sxs-lookup"><span data-stu-id="e3bff-116">The repository and unit of work patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.</span></span> <span data-ttu-id="e3bff-117">實作這些模式可以協助您隔離您的應用程式資料存放區中的變更，以及有助於自動化的單元測試為導向的開發 (TDD)。</span><span class="sxs-lookup"><span data-stu-id="e3bff-117">Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).</span></span>

<span data-ttu-id="e3bff-118">在本教學課程中，您將實作每個實體類型的儲存機制類別。</span><span class="sxs-lookup"><span data-stu-id="e3bff-118">In this tutorial you'll implement a repository class for each entity type.</span></span> <span data-ttu-id="e3bff-119">如`Student`實體類型，您將建立的儲存機制介面及儲存機制類別。</span><span class="sxs-lookup"><span data-stu-id="e3bff-119">For the `Student` entity type you'll create a repository interface and a repository class.</span></span> <span data-ttu-id="e3bff-120">時，在控制器初始化儲存機制，您將使用的介面，以便控制器將會接受實作儲存機制介面的任何物件的參考。</span><span class="sxs-lookup"><span data-stu-id="e3bff-120">When you instantiate the repository in your controller, you'll use the interface so that the controller will accept a reference to any object that implements the repository interface.</span></span> <span data-ttu-id="e3bff-121">當控制器會執行 web 伺服器 下時，它就會收到適用於 Entity Framework 的儲存機制。</span><span class="sxs-lookup"><span data-stu-id="e3bff-121">When the controller runs under a web server, it receives a repository that works with the Entity Framework.</span></span> <span data-ttu-id="e3bff-122">當控制器會執行單元測試類別下時，它就會收到適用於您就可以輕鬆地操作進行測試，例如記憶體中集合的方式儲存資料的儲存機制。</span><span class="sxs-lookup"><span data-stu-id="e3bff-122">When the controller runs under a unit test class, it receives a repository that works with data stored in a way that you can easily manipulate for testing, such as an in-memory collection.</span></span>

<span data-ttu-id="e3bff-123">您稍後在本教學課程中使用多個儲存機制和工作類別的一個單位`Course`和`Department`中的實體類型`Course`控制站。</span><span class="sxs-lookup"><span data-stu-id="e3bff-123">Later in the tutorial you'll use multiple repositories and a unit of work class for the `Course` and `Department` entity types in the `Course` controller.</span></span> <span data-ttu-id="e3bff-124">工作類別的單元協調多個儲存機制的工作，藉由建立它們的所有共用的單一資料庫內容類別。</span><span class="sxs-lookup"><span data-stu-id="e3bff-124">The unit of work class coordinates the work of multiple repositories by creating a single database context class shared by all of them.</span></span> <span data-ttu-id="e3bff-125">如果您想要能夠執行自動化的單元測試，您會建立並使用這些類別的介面，如同您對`Student`儲存機制。</span><span class="sxs-lookup"><span data-stu-id="e3bff-125">If you wanted to be able to perform automated unit testing, you'd create and use interfaces for these classes in the same way you did for the `Student` repository.</span></span> <span data-ttu-id="e3bff-126">不過，為了簡化教學課程，您將建立和使用這些類別沒有介面。</span><span class="sxs-lookup"><span data-stu-id="e3bff-126">However, to keep the tutorial simple, you'll create and use these classes without interfaces.</span></span>

<span data-ttu-id="e3bff-127">下圖顯示手法內容類別，相較於未使用的儲存機制或單位的工作模式完全控制站之間的關聯性的其中一種方式。</span><span class="sxs-lookup"><span data-stu-id="e3bff-127">The following illustration shows one way to conceptualize the relationships between the controller and context classes compared to not using the repository or unit of work pattern at all.</span></span>

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

<span data-ttu-id="e3bff-129">您將不會在此教學課程中建立單元測試。</span><span class="sxs-lookup"><span data-stu-id="e3bff-129">You won't create unit tests in this tutorial series.</span></span> <span data-ttu-id="e3bff-130">如簡介 TDD 的 MVC 應用程式會使用儲存機制模式，請參閱[逐步解說： 使用 ASP.NET MVC 中 TDD](https://msdn.microsoft.com/en-us/library/ff847525.aspx)。</span><span class="sxs-lookup"><span data-stu-id="e3bff-130">For an introduction to TDD with an MVC application that uses the repository pattern, see [Walkthrough: Using TDD with ASP.NET MVC](https://msdn.microsoft.com/en-us/library/ff847525.aspx).</span></span> <span data-ttu-id="e3bff-131">如需儲存機制模式的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="e3bff-131">For more information about the repository pattern, see the following resources:</span></span>

- <span data-ttu-id="e3bff-132">[儲存機制模式](https://msdn.microsoft.com/en-us/library/ff649690.aspx)MSDN 上。</span><span class="sxs-lookup"><span data-stu-id="e3bff-132">[The Repository Pattern](https://msdn.microsoft.com/en-us/library/ff649690.aspx) on MSDN.</span></span>
- <span data-ttu-id="e3bff-133">[使用儲存機制和工作單位的模式與 Entity Framework 4.0](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) Entity Framework 小組部落格上。</span><span class="sxs-lookup"><span data-stu-id="e3bff-133">[Using Repository and Unit of Work patterns with Entity Framework 4.0](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) on the Entity Framework team blog.</span></span>
- <span data-ttu-id="e3bff-134">[敏捷式軟體開發 Entity Framework 4 儲存機制](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/)Julie Lerman 部落格文章系列。</span><span class="sxs-lookup"><span data-stu-id="e3bff-134">[Agile Entity Framework 4 Repository](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) series of posts on Julie Lerman's blog.</span></span>
- <span data-ttu-id="e3bff-135">[建置瀏覽 HTML5/jQuery 應用程式的帳戶](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx)Dan Wahlin 部落格上。</span><span class="sxs-lookup"><span data-stu-id="e3bff-135">[Building the Account at a Glance HTML5/jQuery Application](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) on Dan Wahlin's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="e3bff-136">有許多方式來實作的儲存機制和工作模式的單位。</span><span class="sxs-lookup"><span data-stu-id="e3bff-136">There are many ways to implement the repository and unit of work patterns.</span></span> <span data-ttu-id="e3bff-137">不論工作類別的一個單位，您可以使用儲存機制類別。</span><span class="sxs-lookup"><span data-stu-id="e3bff-137">You can use repository classes with or without a unit of work class.</span></span> <span data-ttu-id="e3bff-138">您可以實作所有的實體類型，或另一個用於每種類型的單一儲存機制。</span><span class="sxs-lookup"><span data-stu-id="e3bff-138">You can implement a single repository for all entity types, or one for each type.</span></span> <span data-ttu-id="e3bff-139">如果您實作每個類型的其中一個，您可以使用個別的類別、 泛型基底類別和衍生的類別，或抽象的基底類別和衍生的類別。</span><span class="sxs-lookup"><span data-stu-id="e3bff-139">If you implement one for each type, you can use separate classes, a generic base class and derived classes, or an abstract base class and derived classes.</span></span> <span data-ttu-id="e3bff-140">您可以將商務邏輯併入您的儲存機制，或將其限制資料存取邏輯。</span><span class="sxs-lookup"><span data-stu-id="e3bff-140">You can include business logic in your repository or restrict it to data access logic.</span></span> <span data-ttu-id="e3bff-141">您也可以建置成您的資料庫內容類別的抽象層，使用[IDbSet](https://msdn.microsoft.com/en-us/library/gg679233(v=vs.103).aspx)那里介面，而不是[DbSet](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset(v=vs.103).aspx)實體集的類型。</span><span class="sxs-lookup"><span data-stu-id="e3bff-141">You can also build an abstraction layer into your database context class by using [IDbSet](https://msdn.microsoft.com/en-us/library/gg679233(v=vs.103).aspx) interfaces there instead of [DbSet](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset(v=vs.103).aspx) types for your entity sets.</span></span> <span data-ttu-id="e3bff-142">實作抽象層，在此教學課程中所示的方法是其中一個選項，您應考慮所有的案例和環境的建議。</span><span class="sxs-lookup"><span data-stu-id="e3bff-142">The approach to implementing an abstraction layer shown in this tutorial is one option for you to consider, not a recommendation for all scenarios and environments.</span></span>


## <a name="creating-the-student-repository-class"></a><span data-ttu-id="e3bff-143">建立學生儲存機制類別</span><span class="sxs-lookup"><span data-stu-id="e3bff-143">Creating the Student Repository Class</span></span>

<span data-ttu-id="e3bff-144">在*DAL*資料夾中，建立名為的類別檔案*IStudentRepository.cs* ，並以下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="e3bff-144">In the *DAL* folder, create a class file named *IStudentRepository.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="e3bff-145">此程式碼會宣告一組典型 CRUD 方法，包含兩個讀取的方法，會傳回所有`Student`實體，以及尋找單一`Student`實體的識別碼。</span><span class="sxs-lookup"><span data-stu-id="e3bff-145">This code declares a typical set of CRUD methods, including two read methods — one that returns all `Student` entities, and one that finds a single `Student` entity by ID.</span></span>

<span data-ttu-id="e3bff-146">在*DAL*資料夾中，建立名為的類別檔案*StudentRepository.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="e3bff-146">In the *DAL* folder, create a class file named *StudentRepository.cs* file.</span></span> <span data-ttu-id="e3bff-147">將現有的程式碼取代下列程式碼，它會實作`IStudentRepository`介面：</span><span class="sxs-lookup"><span data-stu-id="e3bff-147">Replace the existing code with the following code, which implements the `IStudentRepository` interface:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="e3bff-148">資料庫內容中定義類別變數，而建構函式必須要有傳遞內容的執行個體中呼叫的物件：</span><span class="sxs-lookup"><span data-stu-id="e3bff-148">The database context is defined in a class variable, and the constructor expects the calling object to pass in an instance of the context:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

<span data-ttu-id="e3bff-149">您可以具現化新的內容在儲存機制，但然後如果您使用一個控制器中的多個儲存機制時，每個最後會出現在個別的內容。</span><span class="sxs-lookup"><span data-stu-id="e3bff-149">You could instantiate a new context in the repository, but then if you used multiple repositories in one controller, each would end up with a separate context.</span></span> <span data-ttu-id="e3bff-150">稍後您將使用中的多個儲存機制`Course`控制站，而且您會看到一個單位的工作類別如何確保所有儲存機制使用相同的內容。</span><span class="sxs-lookup"><span data-stu-id="e3bff-150">Later you'll use multiple repositories in the `Course` controller, and you'll see how a unit of work class can ensure that all repositories use the same context.</span></span>

<span data-ttu-id="e3bff-151">儲存機制實作[IDisposable](https://msdn.microsoft.com/en-us/library/system.idisposable.aspx)並處置的資料庫內容，因為您將看到稍控制器，而其 CRUD 方法進行呼叫的資料庫內容中稍早所見的相同方式。</span><span class="sxs-lookup"><span data-stu-id="e3bff-151">The repository implements [IDisposable](https://msdn.microsoft.com/en-us/library/system.idisposable.aspx) and disposes the database context as you saw earlier in the controller, and its CRUD methods make calls to the database context in the same way that you saw earlier.</span></span>

## <a name="change-the-student-controller-to-use-the-repository"></a><span data-ttu-id="e3bff-152">變更為使用儲存機制的學生控制器</span><span class="sxs-lookup"><span data-stu-id="e3bff-152">Change the Student Controller to Use the Repository</span></span>

<span data-ttu-id="e3bff-153">在*StudentController.cs*，目前在類別中的程式碼取代下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="e3bff-153">In *StudentController.cs*, replace the code currently in the class with the following code.</span></span> <span data-ttu-id="e3bff-154">所做的變更會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="e3bff-154">The changes are highlighted.</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

<span data-ttu-id="e3bff-155">控制器現在會宣告實作的物件的類別變數`IStudentRepository`介面，而不是內容類別：</span><span class="sxs-lookup"><span data-stu-id="e3bff-155">The controller now declares a class variable for an object that implements the `IStudentRepository` interface instead of the context class:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

<span data-ttu-id="e3bff-156">預設 （無參數） 建構函式會建立新的內容執行個體，並可選擇性的建構函式可讓呼叫端將內容執行個體中。</span><span class="sxs-lookup"><span data-stu-id="e3bff-156">The default (parameterless) constructor creates a new context instance, and an optional constructor allows the caller to pass in a context instance.</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

<span data-ttu-id="e3bff-157">(如果您使用*相依性插入*，或 DI，您不需要預設建構函式，因為 DI 軟體會確保一律會提供正確的儲存機制物件。)</span><span class="sxs-lookup"><span data-stu-id="e3bff-157">(If you were using *dependency injection*, or DI, you wouldn't need the default constructor because the DI software would ensure that the correct repository object would always be provided.)</span></span>

<span data-ttu-id="e3bff-158">在 CRUD 方法中，而非內容現在稱為儲存機制：</span><span class="sxs-lookup"><span data-stu-id="e3bff-158">In the CRUD methods, the repository is now called instead of the context:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

<span data-ttu-id="e3bff-159">和`Dispose`方法現在會處置而非內容儲存機制：</span><span class="sxs-lookup"><span data-stu-id="e3bff-159">And the `Dispose` method now disposes the repository instead of the context:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

<span data-ttu-id="e3bff-160">執行站台，然後按一下**學生** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="e3bff-160">Run the site and click the **Students** tab.</span></span>

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

<span data-ttu-id="e3bff-162">網頁看起來並之前變更為使用儲存機制中，程式碼和其他學生頁面也使用相同的運作方式相同。</span><span class="sxs-lookup"><span data-stu-id="e3bff-162">The page looks and works the same as it did before you changed the code to use the repository, and the other Student pages also work the same.</span></span> <span data-ttu-id="e3bff-163">不過，沒有一項重要差異方式`Index`控制器方法進行篩選和排序。</span><span class="sxs-lookup"><span data-stu-id="e3bff-163">However, there's an important difference in the way the `Index` method of the controller does filtering and ordering.</span></span> <span data-ttu-id="e3bff-164">這個方法的原始版本包含下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="e3bff-164">The original version of this method contained the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

<span data-ttu-id="e3bff-165">已更新`Index`方法包含下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="e3bff-165">The updated `Index` method contains the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

<span data-ttu-id="e3bff-166">僅反白顯示的程式碼已變更。</span><span class="sxs-lookup"><span data-stu-id="e3bff-166">Only the highlighted code has changed.</span></span>

<span data-ttu-id="e3bff-167">程式碼，原始的版本中`students`型別為`IQueryable`物件。</span><span class="sxs-lookup"><span data-stu-id="e3bff-167">In the original version of the code, `students` is typed as an `IQueryable` object.</span></span> <span data-ttu-id="e3bff-168">查詢不傳送至資料庫，直到它會轉換成集合，例如使用方法`ToList`，這並不會發生之前的索引檢視存取學生模型。</span><span class="sxs-lookup"><span data-stu-id="e3bff-168">The query isn't sent to the database until it's converted into a collection using a method such as `ToList`, which doesn't occur until the Index view accesses the student model.</span></span> <span data-ttu-id="e3bff-169">`Where`上述原始的程式碼中的方法會變成`WHERE`傳送至資料庫的 SQL 查詢中的子句。</span><span class="sxs-lookup"><span data-stu-id="e3bff-169">The `Where` method in the original code above becomes a `WHERE` clause in the SQL query that is sent to the database.</span></span> <span data-ttu-id="e3bff-170">而表示選取的實體會傳回資料庫中。</span><span class="sxs-lookup"><span data-stu-id="e3bff-170">That in turn means that only the selected entities are returned by the database.</span></span> <span data-ttu-id="e3bff-171">不過，因為變更而`context.Students`至`studentRepository.GetStudents()`、`students`這個陳述式之後的變數`IEnumerable`包括在所有學生的資料庫中的集合。</span><span class="sxs-lookup"><span data-stu-id="e3bff-171">However, as a result of changing `context.Students` to `studentRepository.GetStudents()`, the `students` variable after this statement is an `IEnumerable` collection that includes all students in the database.</span></span> <span data-ttu-id="e3bff-172">套用的最終結果`Where`方法相同，但是工作現在都是在 web 伺服器上，而不是由資料庫的記憶體中。</span><span class="sxs-lookup"><span data-stu-id="e3bff-172">The end result of applying the `Where` method is the same, but now the work is done in memory on the web server and not by the database.</span></span> <span data-ttu-id="e3bff-173">傳回的大量資料的查詢，這可能會沒有效率。</span><span class="sxs-lookup"><span data-stu-id="e3bff-173">For queries that return large volumes of data, this can be inefficient.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="e3bff-174">**IQueryable vs。IEnumerable**</span><span class="sxs-lookup"><span data-stu-id="e3bff-174">**IQueryable vs. IEnumerable**</span></span>
> 
> <span data-ttu-id="e3bff-175">實作儲存機制所示，即使您已輸入中的項目之後**搜尋**方塊查詢傳送至 SQL Server 會傳回所有學生資料列，因為它不會包含您的搜尋準則：</span><span class="sxs-lookup"><span data-stu-id="e3bff-175">After you implement the repository as shown here, even if you enter something in the **Search** box the query sent to SQL Server returns all Student rows because it doesn't include your search criteria:</span></span>
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> <span data-ttu-id="e3bff-176">因為儲存機制執行查詢，而不需要知道關於搜尋準則，此查詢會傳回所有學生資料。</span><span class="sxs-lookup"><span data-stu-id="e3bff-176">This query returns all of the student data because the repository executed the query without knowing about the search criteria.</span></span> <span data-ttu-id="e3bff-177">排序、 套用的搜尋條件，以及選取的資料子集的分頁 （在此情況下，顯示只有 3 個資料列） 會在記憶體中完成的程序時，更新`ToPagedList`上呼叫方法`IEnumerable`集合。</span><span class="sxs-lookup"><span data-stu-id="e3bff-177">The process of sorting, applying search criteria, and selecting a subset of the data for paging (showing only 3 rows in this case) is done in memory later when the `ToPagedList` method is called on the `IEnumerable` collection.</span></span>
> 
> <span data-ttu-id="e3bff-178">在程式碼 （在您實作儲存機制） 的先前版本中，查詢不會傳送至資料庫之前套用的搜尋條件之後，當`ToPagedList`上呼叫`IQueryable`物件。</span><span class="sxs-lookup"><span data-stu-id="e3bff-178">In the previous version of the code (before you implemented the repository), the query is not sent to the database until after you apply the search criteria, when `ToPagedList` is called on the `IQueryable` object.</span></span>
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> <span data-ttu-id="e3bff-179">當上呼叫 ToPagedList`IQueryable`物件傳送至 SQL Server 查詢會指定搜尋字串，因此會傳回符合搜尋準則的資料列，並沒有篩選必須在記憶體中完成。</span><span class="sxs-lookup"><span data-stu-id="e3bff-179">When ToPagedList is called on an `IQueryable` object, the query sent to SQL Server specifies the search string, and as a result only rows that meet the search criteria are returned, and no filtering needs to be done in memory.</span></span>
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> <span data-ttu-id="e3bff-180">（下列教學課程說明如何檢查查詢傳送至 SQL Server）。</span><span class="sxs-lookup"><span data-stu-id="e3bff-180">(The following tutorial explains how to examine queries sent to SQL Server.)</span></span>


<span data-ttu-id="e3bff-181">下列章節示範如何實作儲存機制的方法可讓您指定資料庫應該這項工作。</span><span class="sxs-lookup"><span data-stu-id="e3bff-181">The following section shows how to implement repository methods that enable you to specify that this work should be done by the database.</span></span>

<span data-ttu-id="e3bff-182">現在您已建立控制器與 Entity Framework 資料庫內容之間的抽象層。</span><span class="sxs-lookup"><span data-stu-id="e3bff-182">You've now created an abstraction layer between the controller and the Entity Framework database context.</span></span> <span data-ttu-id="e3bff-183">如果您要執行自動化的單元測試與此應用程式，您可以建立替代儲存機制類別中實作的單元測試專案`IStudentRepository` *。*</span><span class="sxs-lookup"><span data-stu-id="e3bff-183">If you were going to perform automated unit testing with this application, you could create an alternative repository class in a unit test project that implements `IStudentRepository`*.*</span></span> <span data-ttu-id="e3bff-184">而不是呼叫要讀取和寫入資料的內容，此模擬儲存機制類別無法管理記憶體中集合，若要測試控制器函式。</span><span class="sxs-lookup"><span data-stu-id="e3bff-184">Instead of calling the context to read and write data, this mock repository class could manipulate in-memory collections in order to test controller functions.</span></span>

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a><span data-ttu-id="e3bff-185">實作泛型的儲存機制和單位的工作類別</span><span class="sxs-lookup"><span data-stu-id="e3bff-185">Implement a Generic Repository and a Unit of Work Class</span></span>

<span data-ttu-id="e3bff-186">建立每個實體類型的儲存機制類別可能會導致大量多餘的程式碼，它可能會導致部分更新。</span><span class="sxs-lookup"><span data-stu-id="e3bff-186">Creating a repository class for each entity type could result in a lot of redundant code, and it could result in partial updates.</span></span> <span data-ttu-id="e3bff-187">例如，假設您有要更新兩個不同的實體類型做為在相同交易的一部分。</span><span class="sxs-lookup"><span data-stu-id="e3bff-187">For example, suppose you have to update two different entity types as part of the same transaction.</span></span> <span data-ttu-id="e3bff-188">如果每個使用不同的資料庫內容執行個體，其中可能會成功，且其他可能會失敗。</span><span class="sxs-lookup"><span data-stu-id="e3bff-188">If each uses a separate database context instance, one might succeed and the other might fail.</span></span> <span data-ttu-id="e3bff-189">一種方法，以減少多餘的程式碼是使用泛型的儲存機制和確認的方法之一，所有儲存機制使用相同的資料庫內容 （因此協調所有更新），請使用工作類別的單位。</span><span class="sxs-lookup"><span data-stu-id="e3bff-189">One way to minimize redundant code is to use a generic repository, and one way to ensure that all repositories use the same database context (and thus coordinate all updates) is to use a unit of work class.</span></span>

<span data-ttu-id="e3bff-190">在本章節的教學課程中，您將建立`GenericRepository`類別和`UnitOfWork`類別，並使用它們`Course`控制站來存取`Department`和`Course`實體集。</span><span class="sxs-lookup"><span data-stu-id="e3bff-190">In this section of the tutorial, you'll create a `GenericRepository` class and a `UnitOfWork` class, and use them in the `Course` controller to access both the `Department` and the `Course` entity sets.</span></span> <span data-ttu-id="e3bff-191">如前所述，為了簡化這個部分的教學課程，您不建立這些類別的介面。</span><span class="sxs-lookup"><span data-stu-id="e3bff-191">As explained earlier, to keep this part of the tutorial simple, you aren't creating interfaces for these classes.</span></span> <span data-ttu-id="e3bff-192">如果您要使用它們來促進 TDD，您會通常會實作這些介面具有相同的方式，但`Student`儲存機制。</span><span class="sxs-lookup"><span data-stu-id="e3bff-192">But if you were going to use them to facilitate TDD, you'd typically implement them with interfaces the same way you did the `Student` repository.</span></span>

### <a name="create-a-generic-repository"></a><span data-ttu-id="e3bff-193">建立一般的儲存機制</span><span class="sxs-lookup"><span data-stu-id="e3bff-193">Create a Generic Repository</span></span>

<span data-ttu-id="e3bff-194">在*DAL*資料夾中，建立*GenericRepository.cs* ，並以下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="e3bff-194">In the *DAL* folder, create *GenericRepository.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

<span data-ttu-id="e3bff-195">資料庫內容和儲存機制針對具現化的實體集，會宣告類別變數：</span><span class="sxs-lookup"><span data-stu-id="e3bff-195">Class variables are declared for the database context and for the entity set that the repository is instantiated for:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

<span data-ttu-id="e3bff-196">建構函式會接受資料庫的內容執行個體，並初始化實體集合變數：</span><span class="sxs-lookup"><span data-stu-id="e3bff-196">The constructor accepts a database context instance and initializes the entity set variable:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

<span data-ttu-id="e3bff-197">`Get`方法可讓呼叫的程式碼指定篩選條件和資料行排序結果，請使用 lambda 運算式和字串參數可讓呼叫端提供以逗號分隔的清單導覽屬性的積極式載入：</span><span class="sxs-lookup"><span data-stu-id="e3bff-197">The `Get` method uses lambda expressions to allow the calling code to specify a filter condition and a column to order the results by, and a string parameter lets the caller provide a comma-delimited list of navigation properties for eager loading:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

<span data-ttu-id="e3bff-198">程式碼`Expression<Func<TEntity, bool>> filter`表示呼叫端會提供 lambda 運算式，根據`TEntity`類型，以及此運算式會傳回布林值。</span><span class="sxs-lookup"><span data-stu-id="e3bff-198">The code `Expression<Func<TEntity, bool>> filter` means the caller will provide a lambda expression based on the `TEntity` type, and this expression will return a Boolean value.</span></span> <span data-ttu-id="e3bff-199">例如，如果儲存機制針對具現化`Student`中呼叫方法的程式碼可能會指定實體類型`student => student.LastName == "Smith`&quot;如`filter`參數。</span><span class="sxs-lookup"><span data-stu-id="e3bff-199">For example, if the repository is instantiated for the `Student` entity type, the code in the calling method might specify `student => student.LastName == "Smith`&quot; for the `filter` parameter.</span></span>

<span data-ttu-id="e3bff-200">程式碼`Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy`也表示呼叫端將會提供 lambda 運算式。</span><span class="sxs-lookup"><span data-stu-id="e3bff-200">The code `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` also means the caller will provide a lambda expression.</span></span> <span data-ttu-id="e3bff-201">但在此情況下，運算式的輸入是`IQueryable`物件`TEntity`型別。</span><span class="sxs-lookup"><span data-stu-id="e3bff-201">But in this case, the input to the expression is an `IQueryable` object for the `TEntity` type.</span></span> <span data-ttu-id="e3bff-202">運算式將傳回的已排序的版本`IQueryable`物件。</span><span class="sxs-lookup"><span data-stu-id="e3bff-202">The expression will return an ordered version of that `IQueryable` object.</span></span> <span data-ttu-id="e3bff-203">例如，如果儲存機制針對具現化`Student`中呼叫方法的程式碼可能會指定實體類型`q => q.OrderBy(s => s.LastName)`如`orderBy`參數。</span><span class="sxs-lookup"><span data-stu-id="e3bff-203">For example, if the repository is instantiated for the `Student` entity type, the code in the calling method might specify `q => q.OrderBy(s => s.LastName)` for the `orderBy` parameter.</span></span>

<span data-ttu-id="e3bff-204">中的程式碼`Get`方法會建立`IQueryable`物件，然後再套用篩選條件運算式，如果有一個：</span><span class="sxs-lookup"><span data-stu-id="e3bff-204">The code in the `Get` method creates an `IQueryable` object and then applies the filter expression if there is one:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

<span data-ttu-id="e3bff-205">接下來它之後，套用 eager 載入運算式剖析的逗號分隔的清單：</span><span class="sxs-lookup"><span data-stu-id="e3bff-205">Next it applies the eager-loading expressions after parsing the comma-delimited list:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

<span data-ttu-id="e3bff-206">最後，它會套用`orderBy`如果有一個運算式，並傳回結果; 否則它會傳回結果為未排序的查詢：</span><span class="sxs-lookup"><span data-stu-id="e3bff-206">Finally, it applies the `orderBy` expression if there is one and returns the results; otherwise it returns the results from the unordered query:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

<span data-ttu-id="e3bff-207">當您呼叫`Get`方法，您可以執行篩選及排序`IEnumerable`而不是這些函式提供參數之方法所傳回的集合。</span><span class="sxs-lookup"><span data-stu-id="e3bff-207">When you call the `Get` method, you could do filtering and sorting on the `IEnumerable` collection returned by the method instead of providing parameters for these functions.</span></span> <span data-ttu-id="e3bff-208">但是，排序和篩選工作會再完成 web 伺服器上的記憶體中。</span><span class="sxs-lookup"><span data-stu-id="e3bff-208">But the sorting and filtering work would then be done in memory on the web server.</span></span> <span data-ttu-id="e3bff-209">使用這些參數，您可以確保工作都是資料庫，而不是 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="e3bff-209">By using these parameters, you ensure that the work is done by the database rather than the web server.</span></span> <span data-ttu-id="e3bff-210">替代方式是建立特定的實體類型的衍生的類別並新增特殊`Get`方法，例如`GetStudentsInNameOrder`或`GetStudentsByName`。</span><span class="sxs-lookup"><span data-stu-id="e3bff-210">An alternative is to create derived classes for specific entity types and add specialized `Get` methods, such as `GetStudentsInNameOrder` or `GetStudentsByName`.</span></span> <span data-ttu-id="e3bff-211">不過，在複雜的應用程式，這可能會造成大量資料這類衍生的類別和方法，這可能是更多工作來維護。</span><span class="sxs-lookup"><span data-stu-id="e3bff-211">However, in a complex application, this can result in a large number of such derived classes and specialized methods, which could be more work to maintain.</span></span>

<span data-ttu-id="e3bff-212">中的程式碼`GetByID`， `Insert`，和`Update`方法很類似您看到非泛型儲存機制中。</span><span class="sxs-lookup"><span data-stu-id="e3bff-212">The code in the `GetByID`, `Insert`, and `Update` methods is similar to what you saw in the non-generic repository.</span></span> <span data-ttu-id="e3bff-213">(您未提供中的積極式載入參數`GetByID`簽章，因為您無法使用積極式載入`Find`方法。)</span><span class="sxs-lookup"><span data-stu-id="e3bff-213">(You aren't providing an eager loading parameter in the `GetByID` signature, because you can't do eager loading with the `Find` method.)</span></span>

<span data-ttu-id="e3bff-214">兩個多載可供`Delete`方法：</span><span class="sxs-lookup"><span data-stu-id="e3bff-214">Two overloads are provided for the `Delete` method:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

<span data-ttu-id="e3bff-215">其中一個這些可讓您傳入剛才識別碼的實體被刪除，一個接受實體執行個體。</span><span class="sxs-lookup"><span data-stu-id="e3bff-215">One of these lets you pass in just the ID of the entity to be deleted, and one takes an entity instance.</span></span> <span data-ttu-id="e3bff-216">如同[處理並行](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)教學課程，並行處理，您必須`Delete`採用實體執行個體方法包含追蹤屬性的原始值。</span><span class="sxs-lookup"><span data-stu-id="e3bff-216">As you saw in the [Handling Concurrency](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial, for concurrency handling you need a `Delete` method that takes an entity instance that includes the original value of a tracking property.</span></span>

<span data-ttu-id="e3bff-217">這個泛型的儲存機制會處理一般 CRUD 需求。</span><span class="sxs-lookup"><span data-stu-id="e3bff-217">This generic repository will handle typical CRUD requirements.</span></span> <span data-ttu-id="e3bff-218">當特定實體類型有更複雜的篩選或排序等的特殊需求，您可以建立具有該類型的其他方法的衍生的類別。</span><span class="sxs-lookup"><span data-stu-id="e3bff-218">When a particular entity type has special requirements, such as more complex filtering or ordering, you can create a derived class that has additional methods for that type.</span></span>

## <a name="creating-the-unit-of-work-class"></a><span data-ttu-id="e3bff-219">建立工作類別的單位</span><span class="sxs-lookup"><span data-stu-id="e3bff-219">Creating the Unit of Work Class</span></span>

<span data-ttu-id="e3bff-220">工作類別的單元有一個用途： 若要確定當您使用多個儲存機制時，它們共用單一資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="e3bff-220">The unit of work class serves one purpose: to make sure that when you use multiple repositories, they share a single database context.</span></span> <span data-ttu-id="e3bff-221">這樣一來，完成的工作單位時，您可以呼叫`SaveChanges`內容的執行個體上的方法，及保證所有相關變更都將經過協調。</span><span class="sxs-lookup"><span data-stu-id="e3bff-221">That way, when a unit of work is complete you can call the `SaveChanges` method on that instance of the context and be assured that all related changes will be coordinated.</span></span> <span data-ttu-id="e3bff-222">所有的類別需要都會`Save`方法與每個儲存機制的屬性。</span><span class="sxs-lookup"><span data-stu-id="e3bff-222">All that the class needs is a `Save` method and a property for each repository.</span></span> <span data-ttu-id="e3bff-223">每個儲存機制屬性會傳回已經與其他儲存機制執行個體使用相同的資料庫內容執行個體產生的儲存機制執行個體。</span><span class="sxs-lookup"><span data-stu-id="e3bff-223">Each repository property returns a repository instance that has been instantiated using the same database context instance as the other repository instances.</span></span>

<span data-ttu-id="e3bff-224">在*DAL*資料夾中，建立名為的類別檔案*UnitOfWork.cs*和範本程式碼取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="e3bff-224">In the *DAL* folder, create a class file named *UnitOfWork.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

<span data-ttu-id="e3bff-225">程式碼建立的資料庫內容和每個儲存機制的類別變數。</span><span class="sxs-lookup"><span data-stu-id="e3bff-225">The code creates class variables for the database context and each repository.</span></span> <span data-ttu-id="e3bff-226">如`context`變數時，新的內容會具現化：</span><span class="sxs-lookup"><span data-stu-id="e3bff-226">For the `context` variable, a new context is instantiated:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

<span data-ttu-id="e3bff-227">儲存機制中的每個屬性會檢查是否已存在於儲存機制。</span><span class="sxs-lookup"><span data-stu-id="e3bff-227">Each repository property checks whether the repository already exists.</span></span> <span data-ttu-id="e3bff-228">如果沒有，它會具現化的儲存機制，在內容執行個體中傳遞。</span><span class="sxs-lookup"><span data-stu-id="e3bff-228">If not, it instantiates the repository, passing in the context instance.</span></span> <span data-ttu-id="e3bff-229">如此一來，所有儲存機制會共用相同的內容執行個體。</span><span class="sxs-lookup"><span data-stu-id="e3bff-229">As a result, all repositories share the same context instance.</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

<span data-ttu-id="e3bff-230">`Save`方法呼叫`SaveChanges`上的資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="e3bff-230">The `Save` method calls `SaveChanges` on the database context.</span></span>

<span data-ttu-id="e3bff-231">具現化資料庫的內容，在類別變數中，任何類別一樣`UnitOfWork`類別會實作`IDisposable`並處置內容。</span><span class="sxs-lookup"><span data-stu-id="e3bff-231">Like any class that instantiates a database context in a class variable, the `UnitOfWork` class implements `IDisposable` and disposes the context.</span></span>

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a><span data-ttu-id="e3bff-232">變更使用 UnitOfWork 類別和儲存機制的課程控制器</span><span class="sxs-lookup"><span data-stu-id="e3bff-232">Changing the Course Controller to use the UnitOfWork Class and Repositories</span></span>

<span data-ttu-id="e3bff-233">您目前已在中的程式碼取代*CourseController.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="e3bff-233">Replace the code you currently have in *CourseController.cs* with the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

<span data-ttu-id="e3bff-234">這個程式碼加入的類別變數`UnitOfWork`類別。</span><span class="sxs-lookup"><span data-stu-id="e3bff-234">This code adds a class variable for the `UnitOfWork` class.</span></span> <span data-ttu-id="e3bff-235">(如果在這裡使用的介面，則不會初始化的變數; 相反地，您也會實作兩個建構函式模式，就如同您對`Student`儲存機制。)</span><span class="sxs-lookup"><span data-stu-id="e3bff-235">(If you were using interfaces here, you wouldn't initialize the variable here; instead, you'd implement a pattern of two constructors just as you did for the `Student` repository.)</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

<span data-ttu-id="e3bff-236">在類別的其餘部分，將資料庫內容的所有參考都被參考適當的儲存機制，使用`UnitOfWork`屬性來存取儲存機制。</span><span class="sxs-lookup"><span data-stu-id="e3bff-236">In the rest of the class, all references to the database context are replaced by references to the appropriate repository, using `UnitOfWork` properties to access the repository.</span></span> <span data-ttu-id="e3bff-237">`Dispose`方法將處置`UnitOfWork`執行個體。</span><span class="sxs-lookup"><span data-stu-id="e3bff-237">The `Dispose` method disposes the `UnitOfWork` instance.</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

<span data-ttu-id="e3bff-238">執行站台，然後按一下**課程** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="e3bff-238">Run the site and click the **Courses** tab.</span></span>

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="e3bff-240">網頁看起來，並為您的變更以前, 一樣，和其他課程頁面也使用相同的運作方式相同。</span><span class="sxs-lookup"><span data-stu-id="e3bff-240">The page looks and works the same as it did before your changes, and the other Course pages also work the same.</span></span>

## <a name="summary"></a><span data-ttu-id="e3bff-241">總結</span><span class="sxs-lookup"><span data-stu-id="e3bff-241">Summary</span></span>

<span data-ttu-id="e3bff-242">您現在已實作儲存機制和工作模式的單位。</span><span class="sxs-lookup"><span data-stu-id="e3bff-242">You have now implemented both the repository and unit of work patterns.</span></span> <span data-ttu-id="e3bff-243">您使用 lambda 運算式做為泛型之儲存機制中的方法參數。</span><span class="sxs-lookup"><span data-stu-id="e3bff-243">You have used lambda expressions as method parameters in the generic repository.</span></span> <span data-ttu-id="e3bff-244">如需有關如何使用這些運算式與`IQueryable`物件，請參閱[IQueryable(T) 介面 (System.Linq)](https://msdn.microsoft.com/en-us/library/bb351562.aspx) MSDN Library 中。</span><span class="sxs-lookup"><span data-stu-id="e3bff-244">For more information about how to use these expressions with an `IQueryable` object, see [IQueryable(T) Interface (System.Linq)](https://msdn.microsoft.com/en-us/library/bb351562.aspx) in the MSDN Library.</span></span> <span data-ttu-id="e3bff-245">在下一個教學課程，您將了解如何處理某些進階案例。</span><span class="sxs-lookup"><span data-stu-id="e3bff-245">In the next tutorial you'll learn how to handle some advanced scenarios.</span></span>

<span data-ttu-id="e3bff-246">Entity Framework 中的其他資源連結位於[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="e3bff-246">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="e3bff-247">[上一頁](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[下一頁](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="e3bff-247">[Previous](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)</span></span>
