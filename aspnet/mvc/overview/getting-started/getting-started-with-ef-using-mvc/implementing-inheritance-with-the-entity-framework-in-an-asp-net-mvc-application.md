---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: 在 ASP.NET MVC 5 應用程式 (11 12 個) 中實作繼承與 Entity Framework 6 |Microsoft 文件
author: tdykstra
description: Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 5 應用程式使用 Entity Framework 6 Code First 和 Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1826659626106993d4796641492c62fcbd22a1b3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873400"
---
<a name="implementing-inheritance-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-11-of-12"></a><span data-ttu-id="e7839-103">實作 ASP.NET MVC 5 應用程式 (11 12 個) 的 Entity Framework 6 的繼承</span><span class="sxs-lookup"><span data-stu-id="e7839-103">Implementing Inheritance with the Entity Framework 6 in an ASP.NET MVC 5 Application (11 of 12)</span></span>
====================
<span data-ttu-id="e7839-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="e7839-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="e7839-105">[下載完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span><span class="sxs-lookup"><span data-stu-id="e7839-105">[Download Completed Project](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) or [Download PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span></span>

> <span data-ttu-id="e7839-106">Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e7839-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 5 applications using the Entity Framework 6 Code First and Visual Studio 2013.</span></span> <span data-ttu-id="e7839-107">如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="e7839-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


<span data-ttu-id="e7839-108">在上一個教學課程中，您會處理並行存取例外狀況。</span><span class="sxs-lookup"><span data-stu-id="e7839-108">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="e7839-109">本教學課程將示範如何在資料模型中實作繼承。</span><span class="sxs-lookup"><span data-stu-id="e7839-109">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="e7839-110">在物件導向程式設計中，您可以使用[繼承](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming))以便[程式碼重複使用](http://en.wikipedia.org/wiki/Code_reuse)。</span><span class="sxs-lookup"><span data-stu-id="e7839-110">In object-oriented programming, you can use [inheritance](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) to facilitate [code reuse](http://en.wikipedia.org/wiki/Code_reuse).</span></span> <span data-ttu-id="e7839-111">在本教學課程中，您將變更 `Instructor` 和 `Student` 類別，讓它們衍生自 `Person` 基底類別，而此基底類別包含講師和學生通用的屬性，例如 `LastName`。</span><span class="sxs-lookup"><span data-stu-id="e7839-111">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="e7839-112">您不會新增或變更任何網頁，但是您將變更一些程式碼，這些變更將會自動反映在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="e7839-112">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="options-for-mapping-inheritance-to-database-tables"></a><span data-ttu-id="e7839-113">將繼承對應至資料庫資料表的選項</span><span class="sxs-lookup"><span data-stu-id="e7839-113">Options for mapping inheritance to database tables</span></span>

<span data-ttu-id="e7839-114">`Instructor`和`Student`中的類別`School`資料模型有相同的幾個屬性：</span><span class="sxs-lookup"><span data-stu-id="e7839-114">The `Instructor` and `Student` classes in the `School` data model have several properties that are identical:</span></span>

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

<span data-ttu-id="e7839-116">假設您想要針對 `Instructor` 和 `Student` 實體所共用的屬性消除多餘的程式碼。</span><span class="sxs-lookup"><span data-stu-id="e7839-116">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="e7839-117">或者，您想要撰寫服務，以便用來格式化名稱，而無需在意名稱是來自講師還是學生。</span><span class="sxs-lookup"><span data-stu-id="e7839-117">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="e7839-118">您可以建立`Person`基底類別，其中包含這些共用屬性，則請`Instructor`和`Student`實體繼承自該基底類別，如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="e7839-118">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="e7839-120">有幾種方式可以在資料庫中表示此繼承結構。</span><span class="sxs-lookup"><span data-stu-id="e7839-120">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="e7839-121">您可能會有`Person`包含學生和講師單一資料表中的相關資訊的資料表。</span><span class="sxs-lookup"><span data-stu-id="e7839-121">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="e7839-122">某些資料行可以只對講師套用 (`HireDate`)，有些則只能學生 (`EnrollmentDate`)，有一些兩個 (`LastName`， `FirstName`)。</span><span class="sxs-lookup"><span data-stu-id="e7839-122">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="e7839-123">一般而言，您就必須*鑑別子*指出哪種類型的每個資料列的資料行代表。</span><span class="sxs-lookup"><span data-stu-id="e7839-123">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="e7839-124">例如，鑑別子資料行的 "Instructor" 代表講師，而 "Student" 代表學生。</span><span class="sxs-lookup"><span data-stu-id="e7839-124">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Table-per-hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="e7839-126">這種模式的單一資料庫資料表產生實體繼承結構會呼叫*資料表每個階層*(TPH) 繼承。</span><span class="sxs-lookup"><span data-stu-id="e7839-126">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="e7839-127">替代方法是讓資料庫看起來更像繼承結構。</span><span class="sxs-lookup"><span data-stu-id="e7839-127">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="e7839-128">例如，您無法在只有名稱欄位`Person`資料表，並有個別`Instructor`和`Student`日期欄位的資料表。</span><span class="sxs-lookup"><span data-stu-id="e7839-128">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Table-per-type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="e7839-130">此模式的資料庫資料表的每個實體類別就稱為*資料表每個型別*(TPT) 繼承。</span><span class="sxs-lookup"><span data-stu-id="e7839-130">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="e7839-131">還有另一個選項是將所有的非抽象類型對應至個別資料表。</span><span class="sxs-lookup"><span data-stu-id="e7839-131">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="e7839-132">類別的所有屬性 (包括繼承的屬性) 都會對應至對應資料表的資料行。</span><span class="sxs-lookup"><span data-stu-id="e7839-132">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="e7839-133">這個模式稱為一實體類一表 (TPC) 繼承。</span><span class="sxs-lookup"><span data-stu-id="e7839-133">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="e7839-134">如果您實作 TPC 繼承`Person`， `Student`，和`Instructor`類別稍早所示`Student`和`Instructor`資料表起來比之前未實作繼承之後沒有不同。</span><span class="sxs-lookup"><span data-stu-id="e7839-134">If you implemented TPC inheritance for the `Person`, `Student`, and `Instructor` classes as shown earlier, the `Student` and `Instructor` tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="e7839-135">TPC 和 TPH 繼承模式通常傳遞更佳的效能比 TPT 繼承的模式，Entity Framework 中因為 TPT 模式可能會導致複雜聯結的查詢。</span><span class="sxs-lookup"><span data-stu-id="e7839-135">TPC and TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="e7839-136">本教學課程將示範如何實作 TPH 繼承。</span><span class="sxs-lookup"><span data-stu-id="e7839-136">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="e7839-137">TPH 已在 Entity Framework 中，預設繼承的模式，所以您只需要建立`Person`類別中，變更`Instructor`和`Student`類別衍生自`Person`，將新的類別加入`DbContext`，並建立移轉。</span><span class="sxs-lookup"><span data-stu-id="e7839-137">TPH is the default inheritance pattern in the Entity Framework, so all you have to do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span> <span data-ttu-id="e7839-138">(如需如何實作其他的繼承模式的詳細資訊，請參閱[對應每個類型的資料表 (TPT) 繼承](https://msdn.microsoft.com/data/jj591617#2.5)和[對應資料表每個具象類別 (TPC) 繼承](https://msdn.microsoft.com/data/jj591617#2.6)MSDN 中Entity Framework 文件。）</span><span class="sxs-lookup"><span data-stu-id="e7839-138">(For information about how to implement the other inheritance patterns, see [Mapping the Table-Per-Type (TPT) Inheritance](https://msdn.microsoft.com/data/jj591617#2.5) and [Mapping the Table-Per-Concrete Class (TPC) Inheritance](https://msdn.microsoft.com/data/jj591617#2.6) in the MSDN Entity Framework documentation.)</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="e7839-139">建立 Person 類別</span><span class="sxs-lookup"><span data-stu-id="e7839-139">Create the Person class</span></span>

<span data-ttu-id="e7839-140">在*模型*資料夾中，建立*Person.cs*和範本程式碼取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="e7839-140">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a><span data-ttu-id="e7839-141">使 Student 和 Instructor 類別繼承自 Person 類別</span><span class="sxs-lookup"><span data-stu-id="e7839-141">Make Student and Instructor classes inherit from Person</span></span>

<span data-ttu-id="e7839-142">在*Instructor.cs*，衍生`Instructor`類別從`Person`類別，並移除索引鍵和名稱的欄位。</span><span class="sxs-lookup"><span data-stu-id="e7839-142">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="e7839-143">程式碼看起來應該如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="e7839-143">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="e7839-144">類似變更*Student.cs*。</span><span class="sxs-lookup"><span data-stu-id="e7839-144">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="e7839-145">`Student`類別看起來像下列的範例：</span><span class="sxs-lookup"><span data-stu-id="e7839-145">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-the-person-entity-type-to-the-model"></a><span data-ttu-id="e7839-146">將人員實體類型加入模型</span><span class="sxs-lookup"><span data-stu-id="e7839-146">Add the Person Entity Type to the Model</span></span>

<span data-ttu-id="e7839-147">在*SchoolContext.cs*，新增`DbSet`屬性`Person`實體類型：</span><span class="sxs-lookup"><span data-stu-id="e7839-147">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="e7839-148">這就是 Entity Framework 為了設定單表繼承而必須執行的所有工作。</span><span class="sxs-lookup"><span data-stu-id="e7839-148">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="e7839-149">您會發現，更新資料庫時，會有`Person`資料表取代`Student`和`Instructor`資料表。</span><span class="sxs-lookup"><span data-stu-id="e7839-149">As you'll see, when the database is updated, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="create-and-update-a-migrations-file"></a><span data-ttu-id="e7839-150">建立和更新的移轉檔案</span><span class="sxs-lookup"><span data-stu-id="e7839-150">Create and Update a Migrations File</span></span>

<span data-ttu-id="e7839-151">在 封裝管理員主控台 (PMC)，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="e7839-151">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="e7839-152">執行`Update-Database`PMC 命令。</span><span class="sxs-lookup"><span data-stu-id="e7839-152">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="e7839-153">命令會在此時失敗，因為我們有現有的資料移轉並不知道如何處理。</span><span class="sxs-lookup"><span data-stu-id="e7839-153">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="e7839-154">您收到錯誤訊息如下：</span><span class="sxs-lookup"><span data-stu-id="e7839-154">You get an error message like the following one:</span></span>

> <span data-ttu-id="e7839-155">*無法卸除物件 ' dbo。講師 ' 因為它正由 FOREIGN KEY 條件約束。*</span><span class="sxs-lookup"><span data-stu-id="e7839-155">*Could not drop object 'dbo.Instructor' because it is referenced by a FOREIGN KEY constraint.*</span></span>


<span data-ttu-id="e7839-156">開啟*移轉\&lt; 時間戳記&gt;\_Inheritance.cs*和取代`Up`方法取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="e7839-156">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

<span data-ttu-id="e7839-157">此程式碼負責下列資料庫更新工作：</span><span class="sxs-lookup"><span data-stu-id="e7839-157">This code takes care of the following database update tasks:</span></span>

- <span data-ttu-id="e7839-158">移除指向 Student 資料表的外部索引鍵條件約束和索引。</span><span class="sxs-lookup"><span data-stu-id="e7839-158">Removes foreign key constraints and indexes that point to the Student table.</span></span>
- <span data-ttu-id="e7839-159">將 Instructor 資料表重新命名為 Person，並對其進行所需的變更來儲存 Student 資料：</span><span class="sxs-lookup"><span data-stu-id="e7839-159">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

    - <span data-ttu-id="e7839-160">針對學生新增可為 null 的 EnrollmentDate。</span><span class="sxs-lookup"><span data-stu-id="e7839-160">Adds nullable EnrollmentDate for students.</span></span>
    - <span data-ttu-id="e7839-161">新增 Discriminator 資料行，以指出資料列適用於學生或講師。</span><span class="sxs-lookup"><span data-stu-id="e7839-161">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>
    - <span data-ttu-id="e7839-162">使 HireDate 成為可為 Null，因為學生資料列不會有雇用日期。</span><span class="sxs-lookup"><span data-stu-id="e7839-162">Makes HireDate nullable since student rows won't have hire dates.</span></span>
    - <span data-ttu-id="e7839-163">新增暫存欄位，它將用來更新指向學生的外部索引鍵。</span><span class="sxs-lookup"><span data-stu-id="e7839-163">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="e7839-164">當您複製到 [人員] 資料表的學生它們會取得新的主索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="e7839-164">When you copy students into the Person table they'll get new primary key values.</span></span>
- <span data-ttu-id="e7839-165">將 Student 資料表中的資料複製到 Person 資料表。</span><span class="sxs-lookup"><span data-stu-id="e7839-165">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="e7839-166">這會導致學生獲指派新的主索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="e7839-166">This causes students to get assigned new primary key values.</span></span>
- <span data-ttu-id="e7839-167">修正指向學生的外部索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="e7839-167">Fixes foreign key values that point to students.</span></span>
- <span data-ttu-id="e7839-168">重新建立外部索引鍵條件約束和索引，現在將它們指向 Person 資料表。</span><span class="sxs-lookup"><span data-stu-id="e7839-168">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="e7839-169">(如果您使用 GUID 而不是整數作為主索引鍵類型，學生的主索引鍵值將無需變更，而且可能已省略其中幾個步驟。)</span><span class="sxs-lookup"><span data-stu-id="e7839-169">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="e7839-170">執行`update-database`命令一次。</span><span class="sxs-lookup"><span data-stu-id="e7839-170">Run the `update-database` command again.</span></span>

<span data-ttu-id="e7839-171">（在生產系統中您會變更對應的向下方法萬一您曾經使用該項資訊來返回先前的資料庫版本。</span><span class="sxs-lookup"><span data-stu-id="e7839-171">(In a production system you would make corresponding changes to the Down method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="e7839-172">此教學課程中您將不會使用向下方法。）</span><span class="sxs-lookup"><span data-stu-id="e7839-172">For this tutorial you won't be using the Down method.)</span></span>

> [!NOTE]
> <span data-ttu-id="e7839-173">很可能在移轉資料和進行結構描述變更時，取得其他錯誤。</span><span class="sxs-lookup"><span data-stu-id="e7839-173">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="e7839-174">如果您移轉時發生錯誤無法解決，您可以繼續這個教學課程中的連接字串，進而*Web.config*檔案，或藉由刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="e7839-174">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or by deleting the database.</span></span> <span data-ttu-id="e7839-175">最簡單的方法是在資料庫重新命名*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="e7839-175">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="e7839-176">例如，將資料庫名稱改 ContosoUniversity2 如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="e7839-176">For example, change the database name to ContosoUniversity2 as shown in the following example:</span></span>
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
> 
> <span data-ttu-id="e7839-177">使用新資料庫時，沒有資料移轉，而`update-database`命令是更有可能能順利完成。</span><span class="sxs-lookup"><span data-stu-id="e7839-177">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="e7839-178">如需有關如何刪除資料庫的指示，請參閱[如何卸除資料庫，從 Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)。</span><span class="sxs-lookup"><span data-stu-id="e7839-178">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="e7839-179">如果您採用這個方法，才能繼續進行本教學課程，在本教學課程結尾處略過部署步驟，或部署為新的站台資料庫。</span><span class="sxs-lookup"><span data-stu-id="e7839-179">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial or deploy to a new site and database.</span></span> <span data-ttu-id="e7839-180">如果相同的站台，您已部署至已部署的更新，EF 會那里相同的錯誤時自動執行移轉。</span><span class="sxs-lookup"><span data-stu-id="e7839-180">If you deploy an update to the same site you've been deploying to already, EF will get the same error there when it runs migrations automatically.</span></span> <span data-ttu-id="e7839-181">如果您想要對移轉錯誤進行疑難排解時，最佳的資源是 Entity Framework 論壇或 StackOverflow.com 之一。</span><span class="sxs-lookup"><span data-stu-id="e7839-181">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>


## <a name="testing"></a><span data-ttu-id="e7839-182">測試</span><span class="sxs-lookup"><span data-stu-id="e7839-182">Testing</span></span>

<span data-ttu-id="e7839-183">執行網站，然後再次嘗試各種不同的頁面。</span><span class="sxs-lookup"><span data-stu-id="e7839-183">Run the site and try various pages.</span></span> <span data-ttu-id="e7839-184">一切項目的運作與之前一樣。</span><span class="sxs-lookup"><span data-stu-id="e7839-184">Everything works the same as it did before.</span></span>

<span data-ttu-id="e7839-185">在**伺服器總管 中，** 展開**資料 Connections\SchoolContext**然後**資料表**，而且您會看到的**學生**和**講師**資料表已取代**人員**資料表。</span><span class="sxs-lookup"><span data-stu-id="e7839-185">In **Server Explorer,** expand **Data Connections\SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="e7839-186">展開**人員**資料表，而且您看到它有使用中的資料行的所有**學生**和**講師**資料表。</span><span class="sxs-lookup"><span data-stu-id="e7839-186">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

<span data-ttu-id="e7839-188">以滑鼠右鍵按一下 Person 資料表，然後按一下 [顯示資料表資料] 以查看鑑別子資料行。</span><span class="sxs-lookup"><span data-stu-id="e7839-188">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="e7839-189">下圖說明新的 School 資料庫的結構：</span><span class="sxs-lookup"><span data-stu-id="e7839-189">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="e7839-191">部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="e7839-191">Deploy to Azure</span></span>

<span data-ttu-id="e7839-192">本節會要求您已經完成選擇性**將應用程式部署至 Azure**一節中[第 3 部分排序、 篩選和分頁](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)此教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="e7839-192">This section requires you to have completed the optional **Deploying the app to Azure** section in [Part 3, Sorting, Filtering, and Paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) of this tutorial series.</span></span> <span data-ttu-id="e7839-193">如果您在您解決方法是刪除您的本機專案中的資料庫的移轉錯誤，請略過此步驟中;建立新的站台和資料庫，或部署到新環境。</span><span class="sxs-lookup"><span data-stu-id="e7839-193">If you had migrations errors that you resolved by deleting the database in your local project, skip this step; or create a new site and database, and deploy to the new environment.</span></span>

1. <span data-ttu-id="e7839-194">在 Visual Studio 中的專案上按一下滑鼠右鍵**方案總管 中**選取**發行**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="e7839-194">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>  
  
    ![在專案內容功能表中發行](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)
2. <span data-ttu-id="e7839-196">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="e7839-196">Click **Publish**.</span></span>  
  
    ![發行](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)  
  
   <span data-ttu-id="e7839-198">Web 應用程式會在預設瀏覽器中開啟的。</span><span class="sxs-lookup"><span data-stu-id="e7839-198">The Web app will open in your default browser.</span></span>
3. <span data-ttu-id="e7839-199">測試應用程式，以確認它是否運作。</span><span class="sxs-lookup"><span data-stu-id="e7839-199">Test the application to verify it's working.</span></span>

    <span data-ttu-id="e7839-200">第一次您執行頁面，來存取資料庫時，Entity Framework 會執行所有移轉`Up`使資料庫的最新狀態的目前資料模型所需的方法。</span><span class="sxs-lookup"><span data-stu-id="e7839-200">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span>

## <a name="summary"></a><span data-ttu-id="e7839-201">總結</span><span class="sxs-lookup"><span data-stu-id="e7839-201">Summary</span></span>

<span data-ttu-id="e7839-202">您已針對 `Person`、`Student` 和 `Instructor` 類別實作單表繼承。</span><span class="sxs-lookup"><span data-stu-id="e7839-202">You've implemented table-per-hierarchy inheritance for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="e7839-203">如需有關這個主題以及其他的繼承結構的詳細資訊，請參閱[TPT 繼承模式](https://msdn.microsoft.com/data/jj618293)和[TPH 繼承模式](https://msdn.microsoft.com/data/jj618292)MSDN 上。</span><span class="sxs-lookup"><span data-stu-id="e7839-203">For more information about this and other inheritance structures, see [TPT Inheritance Pattern](https://msdn.microsoft.com/data/jj618293) and [TPH Inheritance Pattern](https://msdn.microsoft.com/data/jj618292) on MSDN.</span></span> <span data-ttu-id="e7839-204">在下一個教學課程中，您將了解如何處理各種相對進階的 Entity Framework 案例。</span><span class="sxs-lookup"><span data-stu-id="e7839-204">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

<span data-ttu-id="e7839-205">Entity Framework 中的其他資源連結位於[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="e7839-205">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e7839-206">[上一頁](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一頁](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="e7839-206">[Previous](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)</span></span>
