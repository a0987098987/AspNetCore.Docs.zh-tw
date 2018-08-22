---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: ASP.NET MVC 應用程式 (10 的 8) 中實作 Entity Framework 的繼承 |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 應用程式...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 10ee0be62a1d601e323afc423e9022bed56f4f33
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830747"
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a><span data-ttu-id="bd299-103">ASP.NET MVC 應用程式 (10 的 8) 中實作 Entity Framework 的繼承</span><span class="sxs-lookup"><span data-stu-id="bd299-103">Implementing Inheritance with the Entity Framework in an ASP.NET MVC Application (8 of 10)</span></span>
====================
<span data-ttu-id="bd299-104">藉由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="bd299-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="bd299-105">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="bd299-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="bd299-106">Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 應用程式。</span><span class="sxs-lookup"><span data-stu-id="bd299-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="bd299-107">如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="bd299-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="bd299-108">您可以從頭開始教學課程系列或[下載入門專案，如本章](building-the-ef5-mvc4-chapter-downloads.md)並從這裡開始。</span><span class="sxs-lookup"><span data-stu-id="bd299-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="bd299-109">如果您遇到的問題，您無法解決，請[下載已完成的一章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現您的問題。</span><span class="sxs-lookup"><span data-stu-id="bd299-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="bd299-110">您通常可以找到問題的解決方案，藉由比較您的程式碼的完整程式碼。</span><span class="sxs-lookup"><span data-stu-id="bd299-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="bd299-111">一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="bd299-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="bd299-112">先前的教學課程中，您會處理並行例外狀況。</span><span class="sxs-lookup"><span data-stu-id="bd299-112">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="bd299-113">本教學課程將示範如何在資料模型中實作繼承。</span><span class="sxs-lookup"><span data-stu-id="bd299-113">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="bd299-114">在物件導向程式設計中，您可以使用繼承，以消除冗餘的程式碼。</span><span class="sxs-lookup"><span data-stu-id="bd299-114">In object-oriented programming, you can use inheritance to eliminate redundant code.</span></span> <span data-ttu-id="bd299-115">在本教學課程中，您將變更 `Instructor` 和 `Student` 類別，讓它們衍生自 `Person` 基底類別，而此基底類別包含講師和學生通用的屬性，例如 `LastName`。</span><span class="sxs-lookup"><span data-stu-id="bd299-115">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="bd299-116">您不會新增或變更任何網頁，但是您將變更一些程式碼，這些變更將會自動反映在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="bd299-116">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a><span data-ttu-id="bd299-117">每個階層的資料表與表 (Table-per-type) 繼承</span><span class="sxs-lookup"><span data-stu-id="bd299-117">Table-per-Hierarchy versus Table-per-Type Inheritance</span></span>

<span data-ttu-id="bd299-118">在物件導向程式設計中，您可以使用繼承，讓您輕鬆地使用相關的類別。</span><span class="sxs-lookup"><span data-stu-id="bd299-118">In object-oriented programming, you can use inheritance to make it easier to work with related classes.</span></span> <span data-ttu-id="bd299-119">例如，`Instructor`並`Student`中的類別`School`資料模型會共用多個屬性，這會導致多餘的程式碼：</span><span class="sxs-lookup"><span data-stu-id="bd299-119">For example, the `Instructor` and `Student` classes in the `School` data model share several properties, which results in redundant code:</span></span>

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

<span data-ttu-id="bd299-121">假設您想要針對 `Instructor` 和 `Student` 實體所共用的屬性消除多餘的程式碼。</span><span class="sxs-lookup"><span data-stu-id="bd299-121">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="bd299-122">您可以建立`Person`基底類別，其中只包含這些共用屬性，則請`Instructor`和`Student`實體繼承自該基底類別，如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="bd299-122">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

<span data-ttu-id="bd299-124">有幾種方式可以在資料庫中表示此繼承結構。</span><span class="sxs-lookup"><span data-stu-id="bd299-124">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="bd299-125">您可能會有`Person`包含學生和講師單一資料表中的相關資訊的資料表。</span><span class="sxs-lookup"><span data-stu-id="bd299-125">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="bd299-126">某些資料行僅適用於講師 (`HireDate`)，有些只適用於學生 (`EnrollmentDate`)，有一些兩個 (`LastName`， `FirstName`)。</span><span class="sxs-lookup"><span data-stu-id="bd299-126">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="bd299-127">一般而言，您必須*鑑別子*指出哪種類型的每個資料列的資料行代表。</span><span class="sxs-lookup"><span data-stu-id="bd299-127">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="bd299-128">例如，鑑別子資料行的 "Instructor" 代表講師，而 "Student" 代表學生。</span><span class="sxs-lookup"><span data-stu-id="bd299-128">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![資料表每個 hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

<span data-ttu-id="bd299-130">從單一資料庫資料表產生實體繼承結構的這個模式稱為*每個階層的資料表*(TPH) 繼承。</span><span class="sxs-lookup"><span data-stu-id="bd299-130">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="bd299-131">替代方法是讓資料庫看起來更像繼承結構。</span><span class="sxs-lookup"><span data-stu-id="bd299-131">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="bd299-132">例如，您可以在只有 [名稱] 欄位`Person`資料表，並有不同`Instructor`和`Student`包含日期欄位的資料表。</span><span class="sxs-lookup"><span data-stu-id="bd299-132">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Table-per-type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

<span data-ttu-id="bd299-134">這種模式的建立資料庫資料表，每個實體類別就稱為*每個類型的資料表*(TPT) 繼承。</span><span class="sxs-lookup"><span data-stu-id="bd299-134">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="bd299-135">TPH 繼承模式通常會提供更佳的效能比起 TPT 繼承模式，Entity Framework 中因為 TPT 模式可能會導致複雜的聯結查詢。</span><span class="sxs-lookup"><span data-stu-id="bd299-135">TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span> <span data-ttu-id="bd299-136">本教學課程將示範如何實作 TPH 繼承。</span><span class="sxs-lookup"><span data-stu-id="bd299-136">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="bd299-137">您將會先執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="bd299-137">You'll do that by performing the following steps:</span></span>

- <span data-ttu-id="bd299-138">建立`Person`類別，並變更`Instructor`並`Student`類別來衍生自`Person`。</span><span class="sxs-lookup"><span data-stu-id="bd299-138">Create a `Person` class and change the `Instructor` and `Student` classes to derive from `Person`.</span></span>
- <span data-ttu-id="bd299-139">加入資料庫內容類別中的模型與資料庫對應程式碼。</span><span class="sxs-lookup"><span data-stu-id="bd299-139">Add model-to-database mapping code to the database context class.</span></span>
- <span data-ttu-id="bd299-140">變更`InstructorID`並`StudentID`整個專案，以參考`PersonID`。</span><span class="sxs-lookup"><span data-stu-id="bd299-140">Change `InstructorID` and `StudentID` references throughout the project to `PersonID`.</span></span>

## <a name="creating-the-person-class"></a><span data-ttu-id="bd299-141">建立 Person 類別</span><span class="sxs-lookup"><span data-stu-id="bd299-141">Creating the Person Class</span></span>

 <span data-ttu-id="bd299-142">注意： 您無法再建立以下的類別，除非您更新使用這些類別的控制站之後，編譯專案。</span><span class="sxs-lookup"><span data-stu-id="bd299-142">Note: You won't be able to compile the project after creating the classes below until you update the controllers that uses these classes.</span></span> 

<span data-ttu-id="bd299-143">在 *模型*資料夾中，建立*Person.cs*並以下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="bd299-143">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="bd299-144">在  *Instructor.cs*，衍生`Instructor`類別從`Person`類別，並移除索引鍵和名稱的欄位。</span><span class="sxs-lookup"><span data-stu-id="bd299-144">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="bd299-145">程式碼看起來應該如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="bd299-145">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="bd299-146">進行類似變更*Student.cs*。</span><span class="sxs-lookup"><span data-stu-id="bd299-146">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="bd299-147">`Student`類別看起來如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="bd299-147">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a><span data-ttu-id="bd299-148">將 Person 實體類型加入模型</span><span class="sxs-lookup"><span data-stu-id="bd299-148">Adding the Person Entity Type to the Model</span></span>

<span data-ttu-id="bd299-149">在  *SchoolContext.cs*，新增`DbSet`屬性`Person`實體類型：</span><span class="sxs-lookup"><span data-stu-id="bd299-149">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="bd299-150">這就是 Entity Framework 為了設定單表繼承而必須執行的所有工作。</span><span class="sxs-lookup"><span data-stu-id="bd299-150">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="bd299-151">如您所見，當資料庫是重新建立，它會有`Person`資料表的位置`Student`和`Instructor`資料表。</span><span class="sxs-lookup"><span data-stu-id="bd299-151">As you'll see, when the database is re-created, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="changing-instructorid-and-studentid-to-personid"></a><span data-ttu-id="bd299-152">變更 PersonID InstructorID 和 StudentID</span><span class="sxs-lookup"><span data-stu-id="bd299-152">Changing InstructorID and StudentID to PersonID</span></span>

<span data-ttu-id="bd299-153">在  *SchoolContext.cs*，在講師-課程對應陳述式中，變更`MapRightKey("InstructorID")`到`MapRightKey("PersonID")`:</span><span class="sxs-lookup"><span data-stu-id="bd299-153">In *SchoolContext.cs*, in the Instructor-Course mapping statement, change `MapRightKey("InstructorID")` to `MapRightKey("PersonID")`:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

<span data-ttu-id="bd299-154">這項變更不是必要的;此外，它只會變更 InstructorID 中的資料行的多對多聯結資料表的名稱。</span><span class="sxs-lookup"><span data-stu-id="bd299-154">This change isn't required; it just changes the name of the InstructorID column in the many-to-many join table.</span></span> <span data-ttu-id="bd299-155">如果您保留 InstructorID 同名時，應用程式就仍然會正確運作。</span><span class="sxs-lookup"><span data-stu-id="bd299-155">If you left the name as InstructorID, the application would still work correctly.</span></span> <span data-ttu-id="bd299-156">以下是 已完成*SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="bd299-156">Here is the completed *SchoolContext.cs*:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

<span data-ttu-id="bd299-157">接下來您需要變更`InstructorID`來`PersonID`並`StudentID`來`PersonID`整個專案***除外***中的時間戳記的移轉檔案*移轉*資料夾。</span><span class="sxs-lookup"><span data-stu-id="bd299-157">Next you need to change `InstructorID` to `PersonID` and `StudentID` to `PersonID` throughout the project ***except*** in the time-stamped migrations files in the *Migrations* folder.</span></span> <span data-ttu-id="bd299-158">若要這樣做，您要尋找並開啟要變更的檔案，然後在開啟的檔案上執行全域變更。</span><span class="sxs-lookup"><span data-stu-id="bd299-158">To do that you'll find and open only the files that need to be changed, then perform a global change on the opened files.</span></span> <span data-ttu-id="bd299-159">中唯一的檔案*移轉*資料夾，您應該變更是*Migrations\Configuration.cs。*</span><span class="sxs-lookup"><span data-stu-id="bd299-159">The only file in the *Migrations* folder you should change is *Migrations\Configuration.cs.*</span></span>

1. > [!IMPORTANT]
   > <span data-ttu-id="bd299-160">關閉所有開啟的檔案，在 Visual Studio 中開始。</span><span class="sxs-lookup"><span data-stu-id="bd299-160">Begin by closing all the open files in Visual Studio.</span></span>
2. <span data-ttu-id="bd299-161">按一下 **尋找和取代-尋找所有的檔案**中**編輯**功能表，然後在專案中包含的所有檔案然後搜尋`InstructorID`。</span><span class="sxs-lookup"><span data-stu-id="bd299-161">Click **Find and Replace -- Find all Files** in the **Edit** menu, and then search for all files in the project that contain `InstructorID`.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. <span data-ttu-id="bd299-162">開啟每個檔案**尋找結果**視窗***除了***&lt;時間戳記&gt;*\_.cs*移轉檔案中的*移轉*資料夾中，按兩下每個檔案的一條線。</span><span class="sxs-lookup"><span data-stu-id="bd299-162">Open each file in the **Find Results** window ***except*** the &lt;time-stamp&gt;*\_.cs* migration files in the *Migrations* folder, by double-clicking one line for each file.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. <span data-ttu-id="bd299-163">開啟**檔案中取代**對話方塊，並變更**查看**來**所有開啟的文件**。</span><span class="sxs-lookup"><span data-stu-id="bd299-163">Open the **Replace in Files** dialog and change **Look in** to **All Open Documents**.</span></span>
5. <span data-ttu-id="bd299-164">使用**檔案中取代**對話方塊，即可變更所有`InstructorID`至 `PersonID.`</span><span class="sxs-lookup"><span data-stu-id="bd299-164">Use the **Replace in Files** dialog to change all `InstructorID` to `PersonID.`</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. <span data-ttu-id="bd299-165">尋找專案中所包含的所有檔案`StudentID`。</span><span class="sxs-lookup"><span data-stu-id="bd299-165">Find all the files in the project that contain `StudentID`.</span></span>
7. <span data-ttu-id="bd299-166">開啟每個檔案**尋找結果**視窗***除了***&lt;時間戳記&gt;*\_\*.cs*移轉檔案在 *移轉*資料夾中，按兩下每個檔案的一條線。</span><span class="sxs-lookup"><span data-stu-id="bd299-166">Open each file in the **Find Results** window ***except*** the &lt;time-stamp&gt;*\_\*.cs* migration files in the *Migrations* folder, by double-clicking one line for each file.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. <span data-ttu-id="bd299-167">開啟**檔案中取代**對話方塊，並變更**查看**來**所有開啟的文件**。</span><span class="sxs-lookup"><span data-stu-id="bd299-167">Open the **Replace in Files** dialog and change **Look in** to **All Open Documents**.</span></span>
9. <span data-ttu-id="bd299-168">使用**檔案中取代**對話方塊，即可變更所有`StudentID`至`PersonID`。</span><span class="sxs-lookup"><span data-stu-id="bd299-168">Use the **Replace in Files** dialog to change all `StudentID` to `PersonID`.</span></span>   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. <span data-ttu-id="bd299-169">建置專案。</span><span class="sxs-lookup"><span data-stu-id="bd299-169">Build the project.</span></span>

<span data-ttu-id="bd299-170">(請注意，此範例示範*缺點*的`classnameID`模式來命名主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="bd299-170">(Note that this demonstrates a *disadvantage* of the `classnameID` pattern for naming primary keys.</span></span> <span data-ttu-id="bd299-171">如果您有名稱主索引鍵識別碼不含類別名稱，前面加上*沒有*重新命名可能需要立即。)</span><span class="sxs-lookup"><span data-stu-id="bd299-171">If you had named primary keys ID without prefixing the class name, *no* renaming would be necessary now.)</span></span>

## <a name="create-and-update-a-migrations-file"></a><span data-ttu-id="bd299-172">建立和更新移轉檔案</span><span class="sxs-lookup"><span data-stu-id="bd299-172">Create and Update a Migrations File</span></span>

<span data-ttu-id="bd299-173">在 套件管理員主控台 (PMC)，請輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="bd299-173">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="bd299-174">執行`Update-Database`PMC 命令。</span><span class="sxs-lookup"><span data-stu-id="bd299-174">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="bd299-175">命令會在此時失敗，因為我們有現有的資料移轉並不知道如何處理。</span><span class="sxs-lookup"><span data-stu-id="bd299-175">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="bd299-176">您會收到下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="bd299-176">You get the following error:</span></span>

<span data-ttu-id="bd299-177">*ALTER TABLE 陳述式與 FOREIGN KEY 條件約束 」 FK\_dbo。部門\_dbo。人員\_PersonID"。衝突發生在資料庫"ContosoUniversity"，資料表"dbo。Person"資料行 'PersonID'。*</span><span class="sxs-lookup"><span data-stu-id="bd299-177">*The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK\_dbo.Department\_dbo.Person\_PersonID". The conflict occurred in database "ContosoUniversity", table "dbo.Person", column 'PersonID'.*</span></span>

<span data-ttu-id="bd299-178">開啟*移轉\&l t; 時間戳記&gt;\_Inheritance.cs* ，並取代`Up`為下列程式碼的方法：</span><span class="sxs-lookup"><span data-stu-id="bd299-178">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

<span data-ttu-id="bd299-179">執行`update-database`命令一次。</span><span class="sxs-lookup"><span data-stu-id="bd299-179">Run the `update-database` command again.</span></span>

> [!NOTE]
> <span data-ttu-id="bd299-180">可以移轉資料和制定的結構描述變更時，取得其他錯誤。</span><span class="sxs-lookup"><span data-stu-id="bd299-180">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="bd299-181">如果您收到的移轉錯誤無法解決，您可以藉由變更連接字串中的繼續進行本教學課程*Web.config*檔案，或刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="bd299-181">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or deleting the database.</span></span> <span data-ttu-id="bd299-182">簡單的方法是在資料庫重新命名*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="bd299-182">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="bd299-183">例如，將資料庫名稱變更為 CU\_測試下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="bd299-183">For example, change the database name to CU\_test as shown in the following example:</span></span>
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> <span data-ttu-id="bd299-184">使用新資料庫時，沒有資料移轉，而`update-database`命令是很有可能能順利完成。</span><span class="sxs-lookup"><span data-stu-id="bd299-184">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="bd299-185">如需有關如何刪除資料庫的指示，請參閱 <<c0> [ 如何從 Visual Studio 2012 中卸除資料庫](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)。</span><span class="sxs-lookup"><span data-stu-id="bd299-185">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="bd299-186">如果您要繼續進行本教學課程採用這個方法，略過部署步驟結尾的這個教學課程中，由於已部署的站台自動執行移轉時，會取得相同的錯誤。</span><span class="sxs-lookup"><span data-stu-id="bd299-186">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial, since the deployed site would get the same error when it runs migrations automatically.</span></span> <span data-ttu-id="bd299-187">如果您想要對移轉錯誤進行疑難排解時，最佳的資源是 Entity Framework 論壇或 StackOverflow.com 其中之一。</span><span class="sxs-lookup"><span data-stu-id="bd299-187">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>


## <a name="testing"></a><span data-ttu-id="bd299-188">測試</span><span class="sxs-lookup"><span data-stu-id="bd299-188">Testing</span></span>

<span data-ttu-id="bd299-189">執行網站，然後嘗試各種頁面。</span><span class="sxs-lookup"><span data-stu-id="bd299-189">Run the site and try various pages.</span></span> <span data-ttu-id="bd299-190">一切項目的運作與之前一樣。</span><span class="sxs-lookup"><span data-stu-id="bd299-190">Everything works the same as it did before.</span></span>

<span data-ttu-id="bd299-191">在 [**伺服器總管] 中，** 展開**SchoolContext** ，然後**資料表**，，您會看到**學生**和**講師**已被取代的資料表**人員**資料表。</span><span class="sxs-lookup"><span data-stu-id="bd299-191">In **Server Explorer,** expand **SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="bd299-192">依序展開**Person**資料表，而且您看到它有使用中的資料行的所有**學生**並**講師**資料表。</span><span class="sxs-lookup"><span data-stu-id="bd299-192">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="bd299-194">以滑鼠右鍵按一下 Person 資料表，然後按一下 [顯示資料表資料] 以查看鑑別子資料行。</span><span class="sxs-lookup"><span data-stu-id="bd299-194">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="bd299-195">下圖說明新的 School 資料庫的結構：</span><span class="sxs-lookup"><span data-stu-id="bd299-195">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a><span data-ttu-id="bd299-197">總結</span><span class="sxs-lookup"><span data-stu-id="bd299-197">Summary</span></span>

<span data-ttu-id="bd299-198">現在針對實作每個階層的資料表繼承`Person`， `Student`，和`Instructor`類別。</span><span class="sxs-lookup"><span data-stu-id="bd299-198">Table-per-hierarchy inheritance has now been implemented for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="bd299-199">如需有關這個主題以及其他繼承結構的詳細資訊，請參閱[繼承對應策略](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx)Morteza Manavi 部落格上。</span><span class="sxs-lookup"><span data-stu-id="bd299-199">For more information about this and other inheritance structures, see [Inheritance Mapping Strategies](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) on Morteza Manavi's blog.</span></span> <span data-ttu-id="bd299-200">在下一個教學課程中，您會看到一些方式來實作儲存機制和工作單元模式。</span><span class="sxs-lookup"><span data-stu-id="bd299-200">In the next tutorial you'll see some ways to implement the repository and unit of work patterns.</span></span>

<span data-ttu-id="bd299-201">其他 Entity Framework 資源連結可在[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="bd299-201">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bd299-202">[上一頁](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一頁](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="bd299-202">[Previous](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)</span></span>
