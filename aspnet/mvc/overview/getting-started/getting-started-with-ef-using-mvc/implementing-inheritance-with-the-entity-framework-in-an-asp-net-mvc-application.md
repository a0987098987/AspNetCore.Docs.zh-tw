---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: ASP.NET MVC 5 應用程式 (11 小時，共 12) 中實作 Entity Framework 6 的繼承 |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 的 ASP.NET MVC 5 應用程式...
ms.author: aspnetcontent
ms.date: 11/07/2014
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 782ccbbec94cc8ee27995b88b89b2d3bd0bfeb70
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818991"
---
<a name="implementing-inheritance-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-11-of-12"></a><span data-ttu-id="4b47d-103">ASP.NET MVC 5 應用程式 (11 小時，共 12) 中實作 Entity Framework 6 的繼承</span><span class="sxs-lookup"><span data-stu-id="4b47d-103">Implementing Inheritance with the Entity Framework 6 in an ASP.NET MVC 5 Application (11 of 12)</span></span>
====================
<span data-ttu-id="4b47d-104">藉由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="4b47d-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="4b47d-105">[下載已完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span><span class="sxs-lookup"><span data-stu-id="4b47d-105">[Download Completed Project](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) or [Download PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span></span>

> <span data-ttu-id="4b47d-106">Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4b47d-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 5 applications using the Entity Framework 6 Code First and Visual Studio 2013.</span></span> <span data-ttu-id="4b47d-107">如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="4b47d-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


<span data-ttu-id="4b47d-108">先前的教學課程中，您會處理並行例外狀況。</span><span class="sxs-lookup"><span data-stu-id="4b47d-108">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="4b47d-109">本教學課程將示範如何在資料模型中實作繼承。</span><span class="sxs-lookup"><span data-stu-id="4b47d-109">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="4b47d-110">在物件導向程式設計中，您可以使用[繼承](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming))以便[程式碼重複使用](http://en.wikipedia.org/wiki/Code_reuse)。</span><span class="sxs-lookup"><span data-stu-id="4b47d-110">In object-oriented programming, you can use [inheritance](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) to facilitate [code reuse](http://en.wikipedia.org/wiki/Code_reuse).</span></span> <span data-ttu-id="4b47d-111">在本教學課程中，您將變更 `Instructor` 和 `Student` 類別，讓它們衍生自 `Person` 基底類別，而此基底類別包含講師和學生通用的屬性，例如 `LastName`。</span><span class="sxs-lookup"><span data-stu-id="4b47d-111">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="4b47d-112">您不會新增或變更任何網頁，但是您將變更一些程式碼，這些變更將會自動反映在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="4b47d-112">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="options-for-mapping-inheritance-to-database-tables"></a><span data-ttu-id="4b47d-113">將繼承對應至資料庫資料表的選項</span><span class="sxs-lookup"><span data-stu-id="4b47d-113">Options for mapping inheritance to database tables</span></span>

<span data-ttu-id="4b47d-114">`Instructor`並`Student`中的類別`School`資料模型有完全相同的數個屬性：</span><span class="sxs-lookup"><span data-stu-id="4b47d-114">The `Instructor` and `Student` classes in the `School` data model have several properties that are identical:</span></span>

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

<span data-ttu-id="4b47d-116">假設您想要針對 `Instructor` 和 `Student` 實體所共用的屬性消除多餘的程式碼。</span><span class="sxs-lookup"><span data-stu-id="4b47d-116">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="4b47d-117">或者，您想要撰寫服務，以便用來格式化名稱，而無需在意名稱是來自講師還是學生。</span><span class="sxs-lookup"><span data-stu-id="4b47d-117">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="4b47d-118">您可以建立`Person`基底類別，其中只包含這些共用屬性，則請`Instructor`和`Student`實體繼承自該基底類別，如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="4b47d-118">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="4b47d-120">有幾種方式可以在資料庫中表示此繼承結構。</span><span class="sxs-lookup"><span data-stu-id="4b47d-120">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="4b47d-121">您可能會有`Person`包含學生和講師單一資料表中的相關資訊的資料表。</span><span class="sxs-lookup"><span data-stu-id="4b47d-121">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="4b47d-122">某些資料行僅適用於講師 (`HireDate`)，有些只適用於學生 (`EnrollmentDate`)，有一些兩個 (`LastName`， `FirstName`)。</span><span class="sxs-lookup"><span data-stu-id="4b47d-122">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="4b47d-123">一般而言，您必須*鑑別子*指出哪種類型的每個資料列的資料行代表。</span><span class="sxs-lookup"><span data-stu-id="4b47d-123">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="4b47d-124">例如，鑑別子資料行的 "Instructor" 代表講師，而 "Student" 代表學生。</span><span class="sxs-lookup"><span data-stu-id="4b47d-124">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![資料表每個 hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="4b47d-126">從單一資料庫資料表產生實體繼承結構的這個模式稱為*每個階層的資料表*(TPH) 繼承。</span><span class="sxs-lookup"><span data-stu-id="4b47d-126">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="4b47d-127">替代方法是讓資料庫看起來更像繼承結構。</span><span class="sxs-lookup"><span data-stu-id="4b47d-127">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="4b47d-128">例如，您可以在只有 [名稱] 欄位`Person`資料表，並有不同`Instructor`和`Student`包含日期欄位的資料表。</span><span class="sxs-lookup"><span data-stu-id="4b47d-128">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Table-per-type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="4b47d-130">這種模式的建立資料庫資料表，每個實體類別就稱為*每個類型的資料表*(TPT) 繼承。</span><span class="sxs-lookup"><span data-stu-id="4b47d-130">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="4b47d-131">還有另一個選項是將所有的非抽象類型對應至個別資料表。</span><span class="sxs-lookup"><span data-stu-id="4b47d-131">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="4b47d-132">類別的所有屬性 (包括繼承的屬性) 都會對應至對應資料表的資料行。</span><span class="sxs-lookup"><span data-stu-id="4b47d-132">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="4b47d-133">這個模式稱為一實體類一表 (TPC) 繼承。</span><span class="sxs-lookup"><span data-stu-id="4b47d-133">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="4b47d-134">如果您實作 TPC 繼承`Person`， `Student`，並`Instructor`稍早所示的類別`Student`和`Instructor`資料表看起來比之前一樣，實作繼承之後。</span><span class="sxs-lookup"><span data-stu-id="4b47d-134">If you implemented TPC inheritance for the `Person`, `Student`, and `Instructor` classes as shown earlier, the `Student` and `Instructor` tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="4b47d-135">TPC 和 TPH 繼承模式通常會提供更佳的效能在 Entity Framework 中，比起 TPT 繼承模式，因為 TPT 模式可能會導致複雜的聯結查詢。</span><span class="sxs-lookup"><span data-stu-id="4b47d-135">TPC and TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="4b47d-136">本教學課程將示範如何實作 TPH 繼承。</span><span class="sxs-lookup"><span data-stu-id="4b47d-136">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="4b47d-137">TPH 是 Entity Framework 中，預設值繼承模式，因此您只需要建立`Person`類別中，變更`Instructor`並`Student`類別來衍生自`Person`，加入新的類別，以`DbContext`，並建立移轉。</span><span class="sxs-lookup"><span data-stu-id="4b47d-137">TPH is the default inheritance pattern in the Entity Framework, so all you have to do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span> <span data-ttu-id="4b47d-138">(如需如何實作其他的繼承模式，請參閱[對應的每一類一表 (TPT) 繼承](https://msdn.microsoft.com/data/jj591617#2.5)並[對應資料表每個具象類別 (TPC) 繼承](https://msdn.microsoft.com/data/jj591617#2.6)MSDN 中Entity Framework 文件。）</span><span class="sxs-lookup"><span data-stu-id="4b47d-138">(For information about how to implement the other inheritance patterns, see [Mapping the Table-Per-Type (TPT) Inheritance](https://msdn.microsoft.com/data/jj591617#2.5) and [Mapping the Table-Per-Concrete Class (TPC) Inheritance](https://msdn.microsoft.com/data/jj591617#2.6) in the MSDN Entity Framework documentation.)</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="4b47d-139">建立 Person 類別</span><span class="sxs-lookup"><span data-stu-id="4b47d-139">Create the Person class</span></span>

<span data-ttu-id="4b47d-140">在 *模型*資料夾中，建立*Person.cs*並以下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="4b47d-140">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a><span data-ttu-id="4b47d-141">使 Student 和 Instructor 類別繼承自 Person 類別</span><span class="sxs-lookup"><span data-stu-id="4b47d-141">Make Student and Instructor classes inherit from Person</span></span>

<span data-ttu-id="4b47d-142">在  *Instructor.cs*，衍生`Instructor`類別從`Person`類別，並移除索引鍵和名稱的欄位。</span><span class="sxs-lookup"><span data-stu-id="4b47d-142">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="4b47d-143">程式碼看起來應該如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="4b47d-143">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="4b47d-144">進行類似變更*Student.cs*。</span><span class="sxs-lookup"><span data-stu-id="4b47d-144">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="4b47d-145">`Student`類別看起來如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="4b47d-145">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-the-person-entity-type-to-the-model"></a><span data-ttu-id="4b47d-146">將 Person 實體類型加入模型</span><span class="sxs-lookup"><span data-stu-id="4b47d-146">Add the Person Entity Type to the Model</span></span>

<span data-ttu-id="4b47d-147">在  *SchoolContext.cs*，新增`DbSet`屬性`Person`實體類型：</span><span class="sxs-lookup"><span data-stu-id="4b47d-147">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="4b47d-148">這就是 Entity Framework 為了設定單表繼承而必須執行的所有工作。</span><span class="sxs-lookup"><span data-stu-id="4b47d-148">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="4b47d-149">如您所見，更新資料庫時，會有`Person`資料表的位置`Student`和`Instructor`資料表。</span><span class="sxs-lookup"><span data-stu-id="4b47d-149">As you'll see, when the database is updated, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="create-and-update-a-migrations-file"></a><span data-ttu-id="4b47d-150">建立和更新移轉檔案</span><span class="sxs-lookup"><span data-stu-id="4b47d-150">Create and Update a Migrations File</span></span>

<span data-ttu-id="4b47d-151">在 套件管理員主控台 (PMC)，請輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="4b47d-151">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="4b47d-152">執行`Update-Database`PMC 命令。</span><span class="sxs-lookup"><span data-stu-id="4b47d-152">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="4b47d-153">命令會在此時失敗，因為我們有現有的資料移轉並不知道如何處理。</span><span class="sxs-lookup"><span data-stu-id="4b47d-153">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="4b47d-154">您收到錯誤訊息如下所示：</span><span class="sxs-lookup"><span data-stu-id="4b47d-154">You get an error message like the following one:</span></span>

> <span data-ttu-id="4b47d-155">*無法卸除物件 ' dbo。講師 ' 因為它由 FOREIGN KEY 條件約束參考。*</span><span class="sxs-lookup"><span data-stu-id="4b47d-155">*Could not drop object 'dbo.Instructor' because it is referenced by a FOREIGN KEY constraint.*</span></span>


<span data-ttu-id="4b47d-156">開啟*移轉\&l t; 時間戳記&gt;\_Inheritance.cs* ，並取代`Up`為下列程式碼的方法：</span><span class="sxs-lookup"><span data-stu-id="4b47d-156">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

<span data-ttu-id="4b47d-157">此程式碼負責下列資料庫更新工作：</span><span class="sxs-lookup"><span data-stu-id="4b47d-157">This code takes care of the following database update tasks:</span></span>

- <span data-ttu-id="4b47d-158">移除指向 Student 資料表的外部索引鍵條件約束和索引。</span><span class="sxs-lookup"><span data-stu-id="4b47d-158">Removes foreign key constraints and indexes that point to the Student table.</span></span>
- <span data-ttu-id="4b47d-159">將 Instructor 資料表重新命名為 Person，並對其進行所需的變更來儲存 Student 資料：</span><span class="sxs-lookup"><span data-stu-id="4b47d-159">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

    - <span data-ttu-id="4b47d-160">針對學生新增可為 null 的 EnrollmentDate。</span><span class="sxs-lookup"><span data-stu-id="4b47d-160">Adds nullable EnrollmentDate for students.</span></span>
    - <span data-ttu-id="4b47d-161">新增 Discriminator 資料行，以指出資料列適用於學生或講師。</span><span class="sxs-lookup"><span data-stu-id="4b47d-161">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>
    - <span data-ttu-id="4b47d-162">使 HireDate 成為可為 Null，因為學生資料列不會有雇用日期。</span><span class="sxs-lookup"><span data-stu-id="4b47d-162">Makes HireDate nullable since student rows won't have hire dates.</span></span>
    - <span data-ttu-id="4b47d-163">新增暫存欄位，它將用來更新指向學生的外部索引鍵。</span><span class="sxs-lookup"><span data-stu-id="4b47d-163">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="4b47d-164">當您將學生複製到 Person 資料表他們會收到新的主索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="4b47d-164">When you copy students into the Person table they'll get new primary key values.</span></span>
- <span data-ttu-id="4b47d-165">將 Student 資料表中的資料複製到 Person 資料表。</span><span class="sxs-lookup"><span data-stu-id="4b47d-165">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="4b47d-166">這會導致學生獲指派新的主索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="4b47d-166">This causes students to get assigned new primary key values.</span></span>
- <span data-ttu-id="4b47d-167">修正指向學生的外部索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="4b47d-167">Fixes foreign key values that point to students.</span></span>
- <span data-ttu-id="4b47d-168">重新建立外部索引鍵條件約束和索引，現在將它們指向 Person 資料表。</span><span class="sxs-lookup"><span data-stu-id="4b47d-168">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="4b47d-169">(如果您使用 GUID 而不是整數作為主索引鍵類型，學生的主索引鍵值將無需變更，而且可能已省略其中幾個步驟。)</span><span class="sxs-lookup"><span data-stu-id="4b47d-169">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="4b47d-170">執行`update-database`命令一次。</span><span class="sxs-lookup"><span data-stu-id="4b47d-170">Run the `update-database` command again.</span></span>

<span data-ttu-id="4b47d-171">（生產系統中您會變更對應下方法以防您曾經使用它來回到先前的資料庫版本。</span><span class="sxs-lookup"><span data-stu-id="4b47d-171">(In a production system you would make corresponding changes to the Down method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="4b47d-172">本教學課程中您將不會使用清單的方法。）</span><span class="sxs-lookup"><span data-stu-id="4b47d-172">For this tutorial you won't be using the Down method.)</span></span>

> [!NOTE]
> <span data-ttu-id="4b47d-173">可以移轉資料和制定的結構描述變更時，取得其他錯誤。</span><span class="sxs-lookup"><span data-stu-id="4b47d-173">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="4b47d-174">如果您收到的移轉錯誤無法解決，您可以藉由變更連接字串中的繼續進行本教學課程*Web.config*檔案，或藉由刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="4b47d-174">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or by deleting the database.</span></span> <span data-ttu-id="4b47d-175">簡單的方法是在資料庫重新命名*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="4b47d-175">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="4b47d-176">比方說，將資料庫名稱變更為 ContosoUniversity2，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="4b47d-176">For example, change the database name to ContosoUniversity2 as shown in the following example:</span></span>
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
> 
> <span data-ttu-id="4b47d-177">使用新資料庫時，沒有資料移轉，而`update-database`命令是很有可能能順利完成。</span><span class="sxs-lookup"><span data-stu-id="4b47d-177">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="4b47d-178">如需有關如何刪除資料庫的指示，請參閱 <<c0> [ 如何從 Visual Studio 2012 中卸除資料庫](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)。</span><span class="sxs-lookup"><span data-stu-id="4b47d-178">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="4b47d-179">如果您要繼續進行本教學課程採用這種方法，在本教學課程結尾處略過部署步驟，或將部署到新的網站和資料庫。</span><span class="sxs-lookup"><span data-stu-id="4b47d-179">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial or deploy to a new site and database.</span></span> <span data-ttu-id="4b47d-180">如果您將更新部署到您已被部署到已位於相同站台時，EF 會那里相同的錯誤時自動執行移轉。</span><span class="sxs-lookup"><span data-stu-id="4b47d-180">If you deploy an update to the same site you've been deploying to already, EF will get the same error there when it runs migrations automatically.</span></span> <span data-ttu-id="4b47d-181">如果您想要對移轉錯誤進行疑難排解時，最佳的資源是 Entity Framework 論壇或 StackOverflow.com 其中之一。</span><span class="sxs-lookup"><span data-stu-id="4b47d-181">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>


## <a name="testing"></a><span data-ttu-id="4b47d-182">測試</span><span class="sxs-lookup"><span data-stu-id="4b47d-182">Testing</span></span>

<span data-ttu-id="4b47d-183">執行網站，然後嘗試各種頁面。</span><span class="sxs-lookup"><span data-stu-id="4b47d-183">Run the site and try various pages.</span></span> <span data-ttu-id="4b47d-184">一切項目的運作與之前一樣。</span><span class="sxs-lookup"><span data-stu-id="4b47d-184">Everything works the same as it did before.</span></span>

<span data-ttu-id="4b47d-185">在 [**伺服器總管] 中，** 展開**資料 Connections\SchoolContext** ，然後**資料表**，，您會看到**學生**及**講師**已被取代的資料表**人員**資料表。</span><span class="sxs-lookup"><span data-stu-id="4b47d-185">In **Server Explorer,** expand **Data Connections\SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="4b47d-186">依序展開**Person**資料表，而且您看到它有使用中的資料行的所有**學生**並**講師**資料表。</span><span class="sxs-lookup"><span data-stu-id="4b47d-186">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

<span data-ttu-id="4b47d-188">以滑鼠右鍵按一下 Person 資料表，然後按一下 [顯示資料表資料] 以查看鑑別子資料行。</span><span class="sxs-lookup"><span data-stu-id="4b47d-188">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="4b47d-189">下圖說明新的 School 資料庫的結構：</span><span class="sxs-lookup"><span data-stu-id="4b47d-189">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="4b47d-191">部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="4b47d-191">Deploy to Azure</span></span>

<span data-ttu-id="4b47d-192">本章節會要求您已經完成選擇性**將應用程式部署至 Azure**一節[第 3 部分排序、 篩選和分頁](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)本教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="4b47d-192">This section requires you to have completed the optional **Deploying the app to Azure** section in [Part 3, Sorting, Filtering, and Paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) of this tutorial series.</span></span> <span data-ttu-id="4b47d-193">如果您刪除本機專案中的資料庫來判斷已解決的移轉錯誤，請略過此步驟中;或建立新的站台和資料庫，並部署到新的環境。</span><span class="sxs-lookup"><span data-stu-id="4b47d-193">If you had migrations errors that you resolved by deleting the database in your local project, skip this step; or create a new site and database, and deploy to the new environment.</span></span>

1. <span data-ttu-id="4b47d-194">在 Visual Studio 中的專案上按一下滑鼠右鍵**方案總管**，然後選取**發佈**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="4b47d-194">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>  
  
    ![在專案操作功能表中發行](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)
2. <span data-ttu-id="4b47d-196">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="4b47d-196">Click **Publish**.</span></span>  
  
    ![發行](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)  
  
   <span data-ttu-id="4b47d-198">Web 應用程式會在預設瀏覽器中開啟。</span><span class="sxs-lookup"><span data-stu-id="4b47d-198">The Web app will open in your default browser.</span></span>
3. <span data-ttu-id="4b47d-199">測試應用程式，以確認它是否運作。</span><span class="sxs-lookup"><span data-stu-id="4b47d-199">Test the application to verify it's working.</span></span>

    <span data-ttu-id="4b47d-200">第一次您執行頁面，來存取資料庫，Entity Framework 便會執行所有移轉`Up`才能讓資料庫維持在最新狀態與目前的資料模型的方法。</span><span class="sxs-lookup"><span data-stu-id="4b47d-200">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span>

## <a name="summary"></a><span data-ttu-id="4b47d-201">總結</span><span class="sxs-lookup"><span data-stu-id="4b47d-201">Summary</span></span>

<span data-ttu-id="4b47d-202">您已針對 `Person`、`Student` 和 `Instructor` 類別實作單表繼承。</span><span class="sxs-lookup"><span data-stu-id="4b47d-202">You've implemented table-per-hierarchy inheritance for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="4b47d-203">如需有關這個主題以及其他繼承結構的詳細資訊，請參閱[TPT 繼承模式](https://msdn.microsoft.com/data/jj618293)並[TPH 繼承模式](https://msdn.microsoft.com/data/jj618292)MSDN 上。</span><span class="sxs-lookup"><span data-stu-id="4b47d-203">For more information about this and other inheritance structures, see [TPT Inheritance Pattern](https://msdn.microsoft.com/data/jj618293) and [TPH Inheritance Pattern](https://msdn.microsoft.com/data/jj618292) on MSDN.</span></span> <span data-ttu-id="4b47d-204">在下一個教學課程中，您將了解如何處理各種相對進階的 Entity Framework 案例。</span><span class="sxs-lookup"><span data-stu-id="4b47d-204">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

<span data-ttu-id="4b47d-205">其他 Entity Framework 資源連結可在[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="4b47d-205">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4b47d-206">[上一頁](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一頁](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="4b47d-206">[Previous](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)</span></span>
