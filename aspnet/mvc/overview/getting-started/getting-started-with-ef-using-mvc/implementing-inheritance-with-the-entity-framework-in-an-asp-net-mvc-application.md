---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: 範本：在 ASP.NET MVC 5 應用程式中實作使用 EF 的繼承
description: 本教學課程將示範如何在資料模型中實作繼承。
author: tdykstra
ms.author: riande
ms.date: 01/21/2019
ms.topic: tutorial
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 79513edce7ac3044f6f547149400cba7d307edfa
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889885"
---
# <a name="template-implement-inheritance-with-ef-in-an-aspnet-mvc-5-app"></a><span data-ttu-id="a40ab-103">範本：在 ASP.NET MVC 5 應用程式中實作使用 EF 的繼承</span><span class="sxs-lookup"><span data-stu-id="a40ab-103">Template: Implement Inheritance with EF in an ASP.NET MVC 5 app</span></span>

<span data-ttu-id="a40ab-104">先前的教學課程中，您會處理並行例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a40ab-104">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="a40ab-105">本教學課程將示範如何在資料模型中實作繼承。</span><span class="sxs-lookup"><span data-stu-id="a40ab-105">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="a40ab-106">在物件導向程式設計中，您可以使用[繼承](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming))以便[程式碼重複使用](http://en.wikipedia.org/wiki/Code_reuse)。</span><span class="sxs-lookup"><span data-stu-id="a40ab-106">In object-oriented programming, you can use [inheritance](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) to facilitate [code reuse](http://en.wikipedia.org/wiki/Code_reuse).</span></span> <span data-ttu-id="a40ab-107">在本教學課程中，您將變更 `Instructor` 和 `Student` 類別，讓它們衍生自 `Person` 基底類別，而此基底類別包含講師和學生通用的屬性，例如 `LastName`。</span><span class="sxs-lookup"><span data-stu-id="a40ab-107">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="a40ab-108">您不會新增或變更任何網頁，但是您將變更一些程式碼，這些變更將會自動反映在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="a40ab-108">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

<span data-ttu-id="a40ab-109">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="a40ab-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a40ab-110">了解如何將繼承對應至資料庫</span><span class="sxs-lookup"><span data-stu-id="a40ab-110">Learn to map inheritance to database</span></span>
> * <span data-ttu-id="a40ab-111">建立 Person 類別</span><span class="sxs-lookup"><span data-stu-id="a40ab-111">Create the Person class</span></span>
> * <span data-ttu-id="a40ab-112">更新 Instructor 和 Student</span><span class="sxs-lookup"><span data-stu-id="a40ab-112">Update Instructor and Student</span></span>
> * <span data-ttu-id="a40ab-113">將人員加入至模型</span><span class="sxs-lookup"><span data-stu-id="a40ab-113">Add Person to the Model</span></span>
> * <span data-ttu-id="a40ab-114">建立和更新的移轉</span><span class="sxs-lookup"><span data-stu-id="a40ab-114">Create and Update Migrations</span></span>
> * <span data-ttu-id="a40ab-115">測試實作</span><span class="sxs-lookup"><span data-stu-id="a40ab-115">Test the implementation</span></span>
> * <span data-ttu-id="a40ab-116">部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="a40ab-116">Deploy to Azure</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a40ab-117">必要條件</span><span class="sxs-lookup"><span data-stu-id="a40ab-117">Prerequisites</span></span>

* [<span data-ttu-id="a40ab-118">實作繼承</span><span class="sxs-lookup"><span data-stu-id="a40ab-118">Implementing Inheritance</span></span>](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="map-inheritance-to-database"></a><span data-ttu-id="a40ab-119">將繼承對應至資料庫</span><span class="sxs-lookup"><span data-stu-id="a40ab-119">Map inheritance to database</span></span>

<span data-ttu-id="a40ab-120">`Instructor`並`Student`中的類別`School`資料模型有完全相同的數個屬性：</span><span class="sxs-lookup"><span data-stu-id="a40ab-120">The `Instructor` and `Student` classes in the `School` data model have several properties that are identical:</span></span>

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

<span data-ttu-id="a40ab-122">假設您想要針對 `Instructor` 和 `Student` 實體所共用的屬性消除多餘的程式碼。</span><span class="sxs-lookup"><span data-stu-id="a40ab-122">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="a40ab-123">或者，您想要撰寫服務，以便用來格式化名稱，而無需在意名稱是來自講師還是學生。</span><span class="sxs-lookup"><span data-stu-id="a40ab-123">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="a40ab-124">您可以建立`Person`基底類別，其中只包含這些共用屬性，則請`Instructor`和`Student`實體繼承自該基底類別，如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="a40ab-124">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="a40ab-126">有幾種方式可以在資料庫中表示此繼承結構。</span><span class="sxs-lookup"><span data-stu-id="a40ab-126">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="a40ab-127">您可能會有`Person`包含學生和講師單一資料表中的相關資訊的資料表。</span><span class="sxs-lookup"><span data-stu-id="a40ab-127">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="a40ab-128">某些資料行僅適用於講師 (`HireDate`)，有些只適用於學生 (`EnrollmentDate`)，有一些兩個 (`LastName`， `FirstName`)。</span><span class="sxs-lookup"><span data-stu-id="a40ab-128">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="a40ab-129">一般而言，您必須*鑑別子*指出哪種類型的每個資料列的資料行代表。</span><span class="sxs-lookup"><span data-stu-id="a40ab-129">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="a40ab-130">例如，鑑別子資料行的 "Instructor" 代表講師，而 "Student" 代表學生。</span><span class="sxs-lookup"><span data-stu-id="a40ab-130">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![資料表每個 hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="a40ab-132">從單一資料庫資料表產生實體繼承結構的這個模式稱為*每個階層的資料表*(TPH) 繼承。</span><span class="sxs-lookup"><span data-stu-id="a40ab-132">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="a40ab-133">替代方法是讓資料庫看起來更像繼承結構。</span><span class="sxs-lookup"><span data-stu-id="a40ab-133">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="a40ab-134">例如，您可以在只有 [名稱] 欄位`Person`資料表，並有不同`Instructor`和`Student`包含日期欄位的資料表。</span><span class="sxs-lookup"><span data-stu-id="a40ab-134">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Table-per-type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="a40ab-136">這種模式的建立資料庫資料表，每個實體類別就稱為*每個類型的資料表*(TPT) 繼承。</span><span class="sxs-lookup"><span data-stu-id="a40ab-136">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="a40ab-137">還有另一個選項是將所有的非抽象類型對應至個別資料表。</span><span class="sxs-lookup"><span data-stu-id="a40ab-137">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="a40ab-138">類別的所有屬性 (包括繼承的屬性) 都會對應至對應資料表的資料行。</span><span class="sxs-lookup"><span data-stu-id="a40ab-138">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="a40ab-139">這個模式稱為一實體類一表 (TPC) 繼承。</span><span class="sxs-lookup"><span data-stu-id="a40ab-139">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="a40ab-140">如果您實作 TPC 繼承`Person`， `Student`，並`Instructor`稍早所示的類別`Student`和`Instructor`資料表看起來比之前一樣，實作繼承之後。</span><span class="sxs-lookup"><span data-stu-id="a40ab-140">If you implemented TPC inheritance for the `Person`, `Student`, and `Instructor` classes as shown earlier, the `Student` and `Instructor` tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="a40ab-141">TPC 和 TPH 繼承模式通常會提供更佳的效能在 Entity Framework 中，比起 TPT 繼承模式，因為 TPT 模式可能會導致複雜的聯結查詢。</span><span class="sxs-lookup"><span data-stu-id="a40ab-141">TPC and TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="a40ab-142">本教學課程將示範如何實作 TPH 繼承。</span><span class="sxs-lookup"><span data-stu-id="a40ab-142">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="a40ab-143">TPH 是 Entity Framework 中，預設值繼承模式，因此您只需要建立`Person`類別中，變更`Instructor`並`Student`類別來衍生自`Person`，加入新的類別，以`DbContext`，並建立移轉。</span><span class="sxs-lookup"><span data-stu-id="a40ab-143">TPH is the default inheritance pattern in the Entity Framework, so all you have to do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span> <span data-ttu-id="a40ab-144">(如需如何實作其他的繼承模式，請參閱[對應的每一類一表 (TPT) 繼承](https://msdn.microsoft.com/data/jj591617#2.5)並[對應資料表每個具象類別 (TPC) 繼承](https://msdn.microsoft.com/data/jj591617#2.6)MSDN 中Entity Framework 文件。）</span><span class="sxs-lookup"><span data-stu-id="a40ab-144">(For information about how to implement the other inheritance patterns, see [Mapping the Table-Per-Type (TPT) Inheritance](https://msdn.microsoft.com/data/jj591617#2.5) and [Mapping the Table-Per-Concrete Class (TPC) Inheritance](https://msdn.microsoft.com/data/jj591617#2.6) in the MSDN Entity Framework documentation.)</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="a40ab-145">建立 Person 類別</span><span class="sxs-lookup"><span data-stu-id="a40ab-145">Create the Person class</span></span>

<span data-ttu-id="a40ab-146">在 *模型*資料夾中，建立*Person.cs*並以下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="a40ab-146">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="update-instructor-and-student"></a><span data-ttu-id="a40ab-147">更新 Instructor 和 Student</span><span class="sxs-lookup"><span data-stu-id="a40ab-147">Update Instructor and Student</span></span>

<span data-ttu-id="a40ab-148">現在更新*Instructor.cs*並*Sudent.cs*繼承值*Person.sc*。</span><span class="sxs-lookup"><span data-stu-id="a40ab-148">Now update the *Instructor.cs* and *Sudent.cs* to inherit values from the *Person.sc*.</span></span>

<span data-ttu-id="a40ab-149">在  *Instructor.cs*，衍生`Instructor`類別從`Person`類別，並移除索引鍵和名稱的欄位。</span><span class="sxs-lookup"><span data-stu-id="a40ab-149">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="a40ab-150">程式碼看起來應該如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="a40ab-150">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="a40ab-151">進行類似變更*Student.cs*。</span><span class="sxs-lookup"><span data-stu-id="a40ab-151">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="a40ab-152">`Student`類別看起來如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="a40ab-152">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-person-to-the-model"></a><span data-ttu-id="a40ab-153">將人員加入至模型</span><span class="sxs-lookup"><span data-stu-id="a40ab-153">Add Person to the Model</span></span>

<span data-ttu-id="a40ab-154">在  *SchoolContext.cs*，新增`DbSet`屬性`Person`實體類型：</span><span class="sxs-lookup"><span data-stu-id="a40ab-154">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="a40ab-155">這就是 Entity Framework 為了設定單表繼承而必須執行的所有工作。</span><span class="sxs-lookup"><span data-stu-id="a40ab-155">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="a40ab-156">如您所見，更新資料庫時，會有`Person`資料表的位置`Student`和`Instructor`資料表。</span><span class="sxs-lookup"><span data-stu-id="a40ab-156">As you'll see, when the database is updated, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="create-and-update-migrations"></a><span data-ttu-id="a40ab-157">建立和更新的移轉</span><span class="sxs-lookup"><span data-stu-id="a40ab-157">Create and Update Migrations</span></span>

<span data-ttu-id="a40ab-158">在 套件管理員主控台 (PMC)，請輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="a40ab-158">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="a40ab-159">執行`Update-Database`PMC 命令。</span><span class="sxs-lookup"><span data-stu-id="a40ab-159">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="a40ab-160">命令會在此時失敗，因為我們有現有的資料移轉並不知道如何處理。</span><span class="sxs-lookup"><span data-stu-id="a40ab-160">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="a40ab-161">您收到錯誤訊息如下所示：</span><span class="sxs-lookup"><span data-stu-id="a40ab-161">You get an error message like the following one:</span></span>

> <span data-ttu-id="a40ab-162">*無法卸除物件 ' dbo。講師 ' 因為它由 FOREIGN KEY 條件約束參考。*</span><span class="sxs-lookup"><span data-stu-id="a40ab-162">*Could not drop object 'dbo.Instructor' because it is referenced by a FOREIGN KEY constraint.*</span></span>


<span data-ttu-id="a40ab-163">開啟*移轉\&l t; 時間戳記&gt;\_Inheritance.cs* ，並取代`Up`為下列程式碼的方法：</span><span class="sxs-lookup"><span data-stu-id="a40ab-163">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

<span data-ttu-id="a40ab-164">此程式碼負責下列資料庫更新工作：</span><span class="sxs-lookup"><span data-stu-id="a40ab-164">This code takes care of the following database update tasks:</span></span>

- <span data-ttu-id="a40ab-165">移除指向 Student 資料表的外部索引鍵條件約束和索引。</span><span class="sxs-lookup"><span data-stu-id="a40ab-165">Removes foreign key constraints and indexes that point to the Student table.</span></span>
- <span data-ttu-id="a40ab-166">將 Instructor 資料表重新命名為 Person，並對其進行所需的變更來儲存 Student 資料：</span><span class="sxs-lookup"><span data-stu-id="a40ab-166">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

    - <span data-ttu-id="a40ab-167">針對學生新增可為 null 的 EnrollmentDate。</span><span class="sxs-lookup"><span data-stu-id="a40ab-167">Adds nullable EnrollmentDate for students.</span></span>
    - <span data-ttu-id="a40ab-168">新增 Discriminator 資料行，以指出資料列適用於學生或講師。</span><span class="sxs-lookup"><span data-stu-id="a40ab-168">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>
    - <span data-ttu-id="a40ab-169">使 HireDate 成為可為 Null，因為學生資料列不會有雇用日期。</span><span class="sxs-lookup"><span data-stu-id="a40ab-169">Makes HireDate nullable since student rows won't have hire dates.</span></span>
    - <span data-ttu-id="a40ab-170">新增暫存欄位，它將用來更新指向學生的外部索引鍵。</span><span class="sxs-lookup"><span data-stu-id="a40ab-170">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="a40ab-171">當您將學生複製到 Person 資料表他們會收到新的主索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="a40ab-171">When you copy students into the Person table they'll get new primary key values.</span></span>
- <span data-ttu-id="a40ab-172">將 Student 資料表中的資料複製到 Person 資料表。</span><span class="sxs-lookup"><span data-stu-id="a40ab-172">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="a40ab-173">這會導致學生獲指派新的主索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="a40ab-173">This causes students to get assigned new primary key values.</span></span>
- <span data-ttu-id="a40ab-174">修正指向學生的外部索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="a40ab-174">Fixes foreign key values that point to students.</span></span>
- <span data-ttu-id="a40ab-175">重新建立外部索引鍵條件約束和索引，現在將它們指向 Person 資料表。</span><span class="sxs-lookup"><span data-stu-id="a40ab-175">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="a40ab-176">(如果您使用 GUID 而不是整數作為主索引鍵類型，學生的主索引鍵值將無需變更，而且可能已省略其中幾個步驟。)</span><span class="sxs-lookup"><span data-stu-id="a40ab-176">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="a40ab-177">執行`update-database`命令一次。</span><span class="sxs-lookup"><span data-stu-id="a40ab-177">Run the `update-database` command again.</span></span>

<span data-ttu-id="a40ab-178">（生產系統中您會變更對應下方法以防您曾經使用它來回到先前的資料庫版本。</span><span class="sxs-lookup"><span data-stu-id="a40ab-178">(In a production system you would make corresponding changes to the Down method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="a40ab-179">本教學課程中您將不會使用清單的方法。）</span><span class="sxs-lookup"><span data-stu-id="a40ab-179">For this tutorial you won't be using the Down method.)</span></span>

> [!NOTE]
> <span data-ttu-id="a40ab-180">可以移轉資料和制定的結構描述變更時，取得其他錯誤。</span><span class="sxs-lookup"><span data-stu-id="a40ab-180">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="a40ab-181">如果您收到的移轉錯誤無法解決，您可以藉由變更連接字串中的繼續進行本教學課程*Web.config*檔案，或藉由刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="a40ab-181">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or by deleting the database.</span></span> <span data-ttu-id="a40ab-182">簡單的方法是在資料庫重新命名*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="a40ab-182">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="a40ab-183">比方說，將資料庫名稱變更為 ContosoUniversity2，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="a40ab-183">For example, change the database name to ContosoUniversity2 as shown in the following example:</span></span>
>
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
>
> <span data-ttu-id="a40ab-184">使用新資料庫時，沒有資料移轉，而`update-database`命令是很有可能能順利完成。</span><span class="sxs-lookup"><span data-stu-id="a40ab-184">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="a40ab-185">如需有關如何刪除資料庫的指示，請參閱 <<c0> [ 如何從 Visual Studio 2012 中卸除資料庫](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)。</span><span class="sxs-lookup"><span data-stu-id="a40ab-185">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="a40ab-186">如果您要繼續進行本教學課程採用這種方法，在本教學課程結尾處略過部署步驟，或將部署到新的網站和資料庫。</span><span class="sxs-lookup"><span data-stu-id="a40ab-186">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial or deploy to a new site and database.</span></span> <span data-ttu-id="a40ab-187">如果您將更新部署到您已被部署到已位於相同站台時，EF 會那里相同的錯誤時自動執行移轉。</span><span class="sxs-lookup"><span data-stu-id="a40ab-187">If you deploy an update to the same site you've been deploying to already, EF will get the same error there when it runs migrations automatically.</span></span> <span data-ttu-id="a40ab-188">如果您想要對移轉錯誤進行疑難排解時，最佳的資源是 Entity Framework 論壇或 StackOverflow.com 其中之一。</span><span class="sxs-lookup"><span data-stu-id="a40ab-188">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>

## <a name="test-the-implementation"></a><span data-ttu-id="a40ab-189">測試實作</span><span class="sxs-lookup"><span data-stu-id="a40ab-189">Test the implementation</span></span>

<span data-ttu-id="a40ab-190">執行網站，然後嘗試各種頁面。</span><span class="sxs-lookup"><span data-stu-id="a40ab-190">Run the site and try various pages.</span></span> <span data-ttu-id="a40ab-191">一切項目的運作與之前一樣。</span><span class="sxs-lookup"><span data-stu-id="a40ab-191">Everything works the same as it did before.</span></span>

<span data-ttu-id="a40ab-192">在 [**伺服器總管] 中，** 展開**資料 Connections\SchoolContext** ，然後**資料表**，，您會看到**學生**及**講師**已被取代的資料表**人員**資料表。</span><span class="sxs-lookup"><span data-stu-id="a40ab-192">In **Server Explorer,** expand **Data Connections\SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="a40ab-193">依序展開**Person**資料表，而且您看到它有使用中的資料行的所有**學生**並**講師**資料表。</span><span class="sxs-lookup"><span data-stu-id="a40ab-193">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

<span data-ttu-id="a40ab-194">以滑鼠右鍵按一下 Person 資料表，然後按一下 [顯示資料表資料] 以查看鑑別子資料行。</span><span class="sxs-lookup"><span data-stu-id="a40ab-194">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

<span data-ttu-id="a40ab-195">下圖說明新的 School 資料庫的結構：</span><span class="sxs-lookup"><span data-stu-id="a40ab-195">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="a40ab-197">部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="a40ab-197">Deploy to Azure</span></span>

<span data-ttu-id="a40ab-198">本章節會要求您已經完成選擇性**將應用程式部署至 Azure**一節[第 3 部分排序、 篩選和分頁](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)本教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="a40ab-198">This section requires you to have completed the optional **Deploying the app to Azure** section in [Part 3, Sorting, Filtering, and Paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) of this tutorial series.</span></span> <span data-ttu-id="a40ab-199">如果您刪除本機專案中的資料庫來判斷已解決的移轉錯誤，請略過此步驟中;或建立新的站台和資料庫，並部署到新的環境。</span><span class="sxs-lookup"><span data-stu-id="a40ab-199">If you had migrations errors that you resolved by deleting the database in your local project, skip this step; or create a new site and database, and deploy to the new environment.</span></span>

1. <span data-ttu-id="a40ab-200">在 Visual Studio 中的專案上按一下滑鼠右鍵**方案總管**，然後選取**發佈**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="a40ab-200">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>

2. <span data-ttu-id="a40ab-201">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="a40ab-201">Click **Publish**.</span></span>

    <span data-ttu-id="a40ab-202">在預設瀏覽器中，開啟 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a40ab-202">The Web app opens in your default browser.</span></span>

3. <span data-ttu-id="a40ab-203">測試應用程式，以確認它是否運作。</span><span class="sxs-lookup"><span data-stu-id="a40ab-203">Test the application to verify it's working.</span></span>

    <span data-ttu-id="a40ab-204">第一次您執行頁面，來存取資料庫，Entity Framework 便會執行所有移轉`Up`才能讓資料庫維持在最新狀態與目前的資料模型的方法。</span><span class="sxs-lookup"><span data-stu-id="a40ab-204">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="a40ab-205">取得程式碼</span><span class="sxs-lookup"><span data-stu-id="a40ab-205">Get the code</span></span>

[<span data-ttu-id="a40ab-206">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="a40ab-206">Download Completed Project</span></span>](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a><span data-ttu-id="a40ab-207">其他資源</span><span class="sxs-lookup"><span data-stu-id="a40ab-207">Additional resources</span></span>

<span data-ttu-id="a40ab-208">其他 Entity Framework 資源連結可在[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="a40ab-208">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

<span data-ttu-id="a40ab-209">如需有關這個主題以及其他繼承結構的詳細資訊，請參閱[TPT 繼承模式](https://msdn.microsoft.com/data/jj618293)並[TPH 繼承模式](https://msdn.microsoft.com/data/jj618292)MSDN 上。</span><span class="sxs-lookup"><span data-stu-id="a40ab-209">For more information about this and other inheritance structures, see [TPT Inheritance Pattern](https://msdn.microsoft.com/data/jj618293) and [TPH Inheritance Pattern](https://msdn.microsoft.com/data/jj618292) on MSDN.</span></span> <span data-ttu-id="a40ab-210">在下一個教學課程中，您將了解如何處理各種相對進階的 Entity Framework 案例。</span><span class="sxs-lookup"><span data-stu-id="a40ab-210">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a40ab-211">後續步驟</span><span class="sxs-lookup"><span data-stu-id="a40ab-211">Next steps</span></span>

<span data-ttu-id="a40ab-212">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="a40ab-212">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a40ab-213">了解如何將繼承對應至資料庫</span><span class="sxs-lookup"><span data-stu-id="a40ab-213">Learned to map inheritance to database</span></span>
> * <span data-ttu-id="a40ab-214">建立 Person 類別</span><span class="sxs-lookup"><span data-stu-id="a40ab-214">Created the Person class</span></span>
> * <span data-ttu-id="a40ab-215">更新的 Instructor 和 Student</span><span class="sxs-lookup"><span data-stu-id="a40ab-215">Updated Instructor and Student</span></span>
> * <span data-ttu-id="a40ab-216">已新增至模型的人員</span><span class="sxs-lookup"><span data-stu-id="a40ab-216">Added Person to the Model</span></span>
> * <span data-ttu-id="a40ab-217">建立和更新的移轉</span><span class="sxs-lookup"><span data-stu-id="a40ab-217">Created and Update Migrations</span></span>
> * <span data-ttu-id="a40ab-218">測試實作</span><span class="sxs-lookup"><span data-stu-id="a40ab-218">Tested the implementation</span></span>
> * <span data-ttu-id="a40ab-219">部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="a40ab-219">Deployed to Azure</span></span>

<span data-ttu-id="a40ab-220">請前往下一篇文章，以了解要注意的是在超出開發 ASP.NET web 應用程式使用 Entity Framework Code First 的基本概念時很有用的主題。</span><span class="sxs-lookup"><span data-stu-id="a40ab-220">Advance to the next article to learn about topics that are useful to be aware of when you go beyond the basics of developing ASP.NET web applications that use Entity Framework Code First.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="a40ab-221">進階的 Entity Framework 案例</span><span class="sxs-lookup"><span data-stu-id="a40ab-221">Advanced Entity Framework Scenarios</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)