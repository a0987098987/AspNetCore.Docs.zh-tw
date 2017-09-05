---
title: "使用 EF 核心繼承-9 10 個 ASP.NET Core MVC"
author: tdykstra
description: "本教學課程將說明如何在資料模型中，使用 Entity Framework Core ASP.NET Core 應用程式中實作繼承。"
keywords: "ASP.NET Core，Entity Framework Core，繼承"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 41dc0db7-6f17-453e-aba6-633430609c74
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/inheritance
ms.openlocfilehash: 3c86dea145d2d4dec10c77e008f511cfe67975f9
ms.sourcegitcommit: 4e84d8bf5f404bb77f3d41665cf7e7374fc39142
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/05/2017
---
# <a name="inheritance---ef-core-with-aspnet-core-mvc-tutorial-9-of-10"></a><span data-ttu-id="ec7dc-104">繼承的 EF Core 與 ASP.NET Core MVC 教學課程 (9 / 10)</span><span class="sxs-lookup"><span data-stu-id="ec7dc-104">Inheritance - EF Core with ASP.NET Core MVC tutorial (9 of 10)</span></span>

<span data-ttu-id="ec7dc-105">由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ec7dc-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ec7dc-106">Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Core MVC web 應用程式使用 Entity Framework Core 和 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="ec7dc-107">教學課程系列的相關資訊，請參閱[系列的第一個教學課程](intro.md)。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="ec7dc-108">在上一個教學課程中，您可以處理並行存取例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-108">In the previous tutorial, you handled concurrency exceptions.</span></span> <span data-ttu-id="ec7dc-109">本教學課程會示範如何實作資料模型中的繼承。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-109">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="ec7dc-110">在物件導向程式設計中，您可以使用繼承，以便重複使用程式碼。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-110">In object-oriented programming, you can use inheritance to facilitate code reuse.</span></span> <span data-ttu-id="ec7dc-111">在此教學課程中，您要變更`Instructor`和`Student`類別，讓它們衍生自`Person`基底類別，其中包含屬性，例如`LastName`通用講師和學生。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-111">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="ec7dc-112">您將不會加入或變更任何網頁，但是您要變更的一些程式碼，而且這些變更將會自動反映在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-112">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="options-for-mapping-inheritance-to-database-tables"></a><span data-ttu-id="ec7dc-113">將繼承對應至資料庫資料表的選項</span><span class="sxs-lookup"><span data-stu-id="ec7dc-113">Options for mapping inheritance to database tables</span></span>

<span data-ttu-id="ec7dc-114">`Instructor`和`Student`學校資料模型中的類別有數個完全相同的屬性：</span><span class="sxs-lookup"><span data-stu-id="ec7dc-114">The `Instructor` and `Student` classes in the School data model have several properties that are identical:</span></span>

![學生和講師類別](inheritance/_static/no-inheritance.png)

<span data-ttu-id="ec7dc-116">假設您想要消除多餘的程式碼所共用的屬性`Instructor`和`Student`實體。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-116">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="ec7dc-117">或者您想要撰寫可以不含關懷名稱是否會來自講師或學生格式名稱的服務。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-117">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="ec7dc-118">您可以建立`Person`基底類別，其中包含這些共用屬性，則請`Instructor`和`Student`類別繼承自該基底類別，如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="ec7dc-118">You could create a `Person` base class that contains only those shared properties, then make the `Instructor` and `Student` classes inherit from that base class, as shown in the following illustration:</span></span>

![Student 與講師類別衍生自人員類別](inheritance/_static/inheritance.png)

<span data-ttu-id="ec7dc-120">有幾種的方式可能會表示此繼承結構，在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-120">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="ec7dc-121">您可能會有一個 Person 資料表，包含學生和講師單一資料表中的資訊。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-121">You could have a Person table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="ec7dc-122">某些資料行無法僅適用於講師 (HireDate)，有些則只能學生 (EnrollmentDate)，有些則兩個 (LastName FirstName)。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-122">Some of the columns could apply only to instructors (HireDate), some only to students (EnrollmentDate), some to both (LastName, FirstName).</span></span> <span data-ttu-id="ec7dc-123">一般而言，您必須指出每個資料列都代表哪種類型的鑑別子資料行。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-123">Typically, you'd have a discriminator column to indicate which type each row represents.</span></span> <span data-ttu-id="ec7dc-124">例如，鑑別子資料行可能會有 「 講師 」 講師和 「 學生 」 學生版。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-124">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![每個階層的資料表範例](inheritance/_static/tph.png)

<span data-ttu-id="ec7dc-126">這種模式的單一資料庫資料表產生實體繼承結構會呼叫每個階層的資料表 (TPH) 繼承。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-126">This pattern of generating an entity inheritance structure from a single database table is called table-per-hierarchy (TPH) inheritance.</span></span>

<span data-ttu-id="ec7dc-127">替代方法是讓資料庫起來更繼承結構。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-127">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="ec7dc-128">比方說，您無法只 Person 資料表中的名稱欄位並且個別位講師和學生的資料表與日期欄位。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-128">For example, you could have only the name fields in the Person table and have separate Instructor and Student tables with the date fields.</span></span>

![一類一表 (Table-Per-Type) 繼承](inheritance/_static/tpt.png)

<span data-ttu-id="ec7dc-130">這種模式的資料庫資料表針對每個實體類別會呼叫每個型別 (TPT) 繼承資料表。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-130">This pattern of making a database table for each entity class is called table per type (TPT) inheritance.</span></span>

<span data-ttu-id="ec7dc-131">還有另一個選項是將所有的非抽象類型對應至個別資料表。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-131">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="ec7dc-132">所有類別的屬性，包括繼承的屬性，對應至相對應資料表的資料行。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-132">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="ec7dc-133">這個模式稱為資料表每個具象類別 (TPC) 繼承。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-133">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="ec7dc-134">如果您實作 TPC 的人員、 學生和講師類別繼承，如先前所示，Student 與講師資料表看起來會一樣比之前未實作繼承之後。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-134">If you implemented TPC inheritance for the Person, Student, and Instructor classes as shown earlier, the Student and Instructor tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="ec7dc-135">TPC 和 TPH 繼承模式通常會傳遞更佳的效能比 TPT 繼承的模式，因為 TPT 模式可能會導致複雜聯結的查詢。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-135">TPC and TPH inheritance patterns generally deliver better performance than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="ec7dc-136">本教學課程會示範如何實作 TPH 繼承。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-136">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="ec7dc-137">TPH 是 Entity Framework Core 支援的只有繼承模式。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-137">TPH is the only inheritance pattern that the Entity Framework Core supports.</span></span>  <span data-ttu-id="ec7dc-138">您將會執行方式是建立`Person`類別中，變更`Instructor`和`Student`類別衍生自`Person`，將新的類別加入`DbContext`，和建立的移轉。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-138">What you'll do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span>

> [!TIP] 
> <span data-ttu-id="ec7dc-139">請考慮下列變更之前儲存專案的複本。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-139">Consider saving a copy of the project before making the following changes.</span></span>  <span data-ttu-id="ec7dc-140">然後如果您遇到的問題而需要從頭開始，會更輕鬆地回到整個序列的開頭啟動從儲存的專案，而不是反轉完成本教學課程的步驟或持續。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-140">Then if you run into problems and need to start over, it will be easier to start from the saved project instead of reversing steps done for this tutorial or going back to the beginning of the whole series.</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="ec7dc-141">建立個人類別</span><span class="sxs-lookup"><span data-stu-id="ec7dc-141">Create the Person class</span></span>

<span data-ttu-id="ec7dc-142">在 [模型] 資料夾中，建立 Person.cs 並取代為下列程式碼中的範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="ec7dc-142">In the Models folder, create Person.cs and replace the template code with the following code:</span></span>

<span data-ttu-id="ec7dc-143">[!code-csharp[Main](intro/samples/cu/Models/Person.cs)]</span><span class="sxs-lookup"><span data-stu-id="ec7dc-143">[!code-csharp[Main](intro/samples/cu/Models/Person.cs)]</span></span>

## <a name="make-student-and-instructor-classes-inherit-from-person"></a><span data-ttu-id="ec7dc-144">將 Student 與講師類別繼承自人員</span><span class="sxs-lookup"><span data-stu-id="ec7dc-144">Make Student and Instructor classes inherit from Person</span></span>

<span data-ttu-id="ec7dc-145">在*Instructor.cs*、 講師類別衍生自人員類別和移除索引鍵和名稱的欄位。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-145">In *Instructor.cs*, derive the Instructor class from the Person class and remove the key and name fields.</span></span> <span data-ttu-id="ec7dc-146">程式碼看起來像下列的範例：</span><span class="sxs-lookup"><span data-stu-id="ec7dc-146">The code will look like the following example:</span></span>

<span data-ttu-id="ec7dc-147">[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]</span><span class="sxs-lookup"><span data-stu-id="ec7dc-147">[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]</span></span>

<span data-ttu-id="ec7dc-148">中的相同變更*Student.cs*。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-148">Make the same changes in *Student.cs*.</span></span>

<span data-ttu-id="ec7dc-149">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]</span><span class="sxs-lookup"><span data-stu-id="ec7dc-149">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]</span></span>

## <a name="add-the-person-entity-type-to-the-data-model"></a><span data-ttu-id="ec7dc-150">加入資料模型的人員實體類型</span><span class="sxs-lookup"><span data-stu-id="ec7dc-150">Add the Person entity type to the data model</span></span>

<span data-ttu-id="ec7dc-151">新增人員實體類型*SchoolContext.cs*。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-151">Add the Person entity type to *SchoolContext.cs*.</span></span> <span data-ttu-id="ec7dc-152">新的線條會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-152">The new lines are highlighted.</span></span>

<span data-ttu-id="ec7dc-153">[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]</span><span class="sxs-lookup"><span data-stu-id="ec7dc-153">[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]</span></span>

<span data-ttu-id="ec7dc-154">這是所有 Entity Framework 必須以設定資料表每個階層繼承。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-154">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="ec7dc-155">您會發現，更新資料庫時，它會有一個 Person 資料表，來取代 「 學生 」 和 「 講師資料表。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-155">As you'll see, when the database is updated, it will have a Person table in place of the Student and Instructor tables.</span></span>

## <a name="create-and-customize-migration-code"></a><span data-ttu-id="ec7dc-156">建立及自訂移轉程式碼</span><span class="sxs-lookup"><span data-stu-id="ec7dc-156">Create and customize migration code</span></span>

<span data-ttu-id="ec7dc-157">儲存您的變更，並建置專案。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-157">Save your changes and build the project.</span></span> <span data-ttu-id="ec7dc-158">然後在專案資料夾中開啟 [命令] 視窗並輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="ec7dc-158">Then open the command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add Inheritance
```

<span data-ttu-id="ec7dc-159">不會執行`database update`尚未命令。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-159">Don't run the `database update` command yet.</span></span> <span data-ttu-id="ec7dc-160">該命令將會導致資料遺失，因為它將會卸除講師資料表，然後將學生資料表重新命名為 Person。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-160">That command will result in lost data because it will drop the Instructor table and rename the Student table to Person.</span></span> <span data-ttu-id="ec7dc-161">您要提供給自訂程式碼來保留現有的資料。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-161">You need to provide custom code to preserve existing data.</span></span>

<span data-ttu-id="ec7dc-162">開啟*移轉 /\<時間戳記 > _Inheritance.cs*和取代`Up`方法取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="ec7dc-162">Open *Migrations/\<timestamp>_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

<span data-ttu-id="ec7dc-163">[!code-csharp[Main](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]</span><span class="sxs-lookup"><span data-stu-id="ec7dc-163">[!code-csharp[Main](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]</span></span>

<span data-ttu-id="ec7dc-164">此程式碼會負責下列資料庫更新工作：</span><span class="sxs-lookup"><span data-stu-id="ec7dc-164">This code takes care of the following database update tasks:</span></span>

* <span data-ttu-id="ec7dc-165">移除外部索引鍵條件約束和學生資料表所指向的索引。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-165">Removes foreign key constraints and indexes that point to the Student table.</span></span>

* <span data-ttu-id="ec7dc-166">重新命名為 Person 講師資料表，然後儲存學生資料所需的變更：</span><span class="sxs-lookup"><span data-stu-id="ec7dc-166">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

* <span data-ttu-id="ec7dc-167">加入可為 null 的 EnrollmentDate 學生版。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-167">Adds nullable EnrollmentDate for students.</span></span>

* <span data-ttu-id="ec7dc-168">加入以指出資料列是一位學生或講師鑑別子資料行。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-168">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>

* <span data-ttu-id="ec7dc-169">由於學生資料列將不會有雇用日期，使 HireDate 可為 null。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-169">Makes HireDate nullable since student rows won't have hire dates.</span></span>

* <span data-ttu-id="ec7dc-170">將暫存的欄位會用來更新指向學生的外部索引鍵。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-170">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="ec7dc-171">當您複製到 [人員] 資料表的學生它們會取得新的主索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-171">When you copy students into the Person table they'll get new primary key values.</span></span>

* <span data-ttu-id="ec7dc-172">從 Person 資料表學生資料表複製資料。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-172">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="ec7dc-173">這會導致學生在指派新的主索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-173">This causes students to get assigned new primary key values.</span></span>

* <span data-ttu-id="ec7dc-174">修正指向學生的外部索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-174">Fixes foreign key values that point to students.</span></span>

* <span data-ttu-id="ec7dc-175">重新建立 foreign key 條件約束和索引，立即將它們指向 Person 資料表。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-175">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="ec7dc-176">（如果您使用 GUID 而不是整數做為主要索引鍵類型，學生的主索引鍵值不會有變更，並有幾個步驟可能已省略）。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-176">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="ec7dc-177">執行`database update`命令：</span><span class="sxs-lookup"><span data-stu-id="ec7dc-177">Run the `database update` command:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="ec7dc-178">(在生產系統中，您會使對應的變更`Down`萬一您曾經使用該項資訊來返回先前的資料庫版本的方法。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-178">(In a production system you would make corresponding changes to the `Down` method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="ec7dc-179">您將不會使用此教學課程`Down`方法。)</span><span class="sxs-lookup"><span data-stu-id="ec7dc-179">For this tutorial you won't be using the `Down` method.)</span></span>

> [!NOTE] 
> <span data-ttu-id="ec7dc-180">很可能在具有現有資料的資料庫進行結構描述變更時，取得其他錯誤。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-180">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="ec7dc-181">如果您移轉時發生錯誤，您無法解決，您可以變更連接字串中的資料庫名稱，或刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-181">If you get migration errors that you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="ec7dc-182">使用新資料庫時，若要移轉，沒有資料並更新資料庫指令更容易完成無誤。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-182">With a new database, there is no data to migrate, and the update-database command is more likely to complete without errors.</span></span> <span data-ttu-id="ec7dc-183">若要刪除的資料庫，請使用 SSOX 或執行`database drop`CLI 命令。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-183">To delete the database, use SSOX or run the `database drop` CLI command.</span></span>

## <a name="test-with-inheritance-implemented"></a><span data-ttu-id="ec7dc-184">使用繼承實作測試</span><span class="sxs-lookup"><span data-stu-id="ec7dc-184">Test with inheritance implemented</span></span>

<span data-ttu-id="ec7dc-185">執行網站，然後再次嘗試各種不同的頁面。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-185">Run the site and try various pages.</span></span> <span data-ttu-id="ec7dc-186">運作一切正常相同與以前一樣。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-186">Everything works the same as it did before.</span></span>

<span data-ttu-id="ec7dc-187">在**SQL Server 物件總管**，展開**資料連線/SchoolContext**然後**資料表**，而且您會看到 「 學生 」 和 「 講師資料表有被取代Person 資料表。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-187">In **SQL Server Object Explorer**, expand **Data Connections/SchoolContext** and then **Tables**, and you see that the Student and Instructor tables have been replaced by a Person table.</span></span> <span data-ttu-id="ec7dc-188">開啟 Person 資料表設計工具，您會看到它具有所有 Student 與講師資料表中使用的資料行。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-188">Open the Person table designer and you see that it has all of the columns that used to be in the Student and Instructor tables.</span></span>

![Person 資料表中 SSOX](inheritance/_static/ssox-person-table.png)

<span data-ttu-id="ec7dc-190">以滑鼠右鍵按一下 [人員] 資料表，然後按一下**顯示資料表資料**看見鑑別子資料行。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-190">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![Person 資料表中 SSOX-資料表的資料](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a><span data-ttu-id="ec7dc-192">總結</span><span class="sxs-lookup"><span data-stu-id="ec7dc-192">Summary</span></span>

<span data-ttu-id="ec7dc-193">您已實作的每個階層的資料表繼承`Person`， `Student`，和`Instructor`類別。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-193">You've implemented table-per-hierarchy inheritance for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="ec7dc-194">如需 Entity Framework 的核心中的繼承的詳細資訊，請參閱[繼承](https://docs.microsoft.com/ef/core/modeling/inheritance)。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-194">For more information about inheritance in Entity Framework Core, see [Inheritance](https://docs.microsoft.com/ef/core/modeling/inheritance).</span></span> <span data-ttu-id="ec7dc-195">在下一個教學課程中，您會看到如何處理各種不同的相對較進階的 Entity Framework 案例。</span><span class="sxs-lookup"><span data-stu-id="ec7dc-195">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ec7dc-196">[上一頁](concurrency.md)
[下一頁](advanced.md)</span><span class="sxs-lookup"><span data-stu-id="ec7dc-196">[Previous](concurrency.md)
[Next](advanced.md)</span></span>  
