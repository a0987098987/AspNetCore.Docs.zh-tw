---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: 在 ASP.NET MVC 應用程式 (10-8) 中實作繼承與 Entity Framework |Microsoft 文件
author: tdykstra
description: Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: ee088f841bdb68f4806b0b62be7d379b9eab9f8c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a><span data-ttu-id="89b37-103">實作 ASP.NET MVC 應用程式 (10-8) 中的 Entity Framework 的繼承</span><span class="sxs-lookup"><span data-stu-id="89b37-103">Implementing Inheritance with the Entity Framework in an ASP.NET MVC Application (8 of 10)</span></span>
====================
<span data-ttu-id="89b37-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="89b37-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="89b37-105">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="89b37-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="89b37-106">Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="89b37-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="89b37-107">如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="89b37-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="89b37-108">您可以從頭開始教學課程系列或[下載本章節的入門專案](building-the-ef5-mvc4-chapter-downloads.md)和從這裡開始。</span><span class="sxs-lookup"><span data-stu-id="89b37-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="89b37-109">如果您遇到的問題，您無法解決，[下載已完成的章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現問題。</span><span class="sxs-lookup"><span data-stu-id="89b37-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="89b37-110">您通常可以藉由比較您的程式碼完成的程式碼會發現問題的解決方案。</span><span class="sxs-lookup"><span data-stu-id="89b37-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="89b37-111">一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="89b37-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="89b37-112">在上一個教學課程中，您會處理並行存取例外狀況。</span><span class="sxs-lookup"><span data-stu-id="89b37-112">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="89b37-113">本教學課程將示範如何在資料模型中實作繼承。</span><span class="sxs-lookup"><span data-stu-id="89b37-113">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="89b37-114">在物件導向程式設計中，您可以使用繼承，以消除重複的程式碼。</span><span class="sxs-lookup"><span data-stu-id="89b37-114">In object-oriented programming, you can use inheritance to eliminate redundant code.</span></span> <span data-ttu-id="89b37-115">在本教學課程中，您將變更 `Instructor` 和 `Student` 類別，讓它們衍生自 `Person` 基底類別，而此基底類別包含講師和學生通用的屬性，例如 `LastName`。</span><span class="sxs-lookup"><span data-stu-id="89b37-115">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="89b37-116">您不會新增或變更任何網頁，但是您將變更一些程式碼，這些變更將會自動反映在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="89b37-116">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a><span data-ttu-id="89b37-117">資料表每個階層與資料表每個類型的繼承</span><span class="sxs-lookup"><span data-stu-id="89b37-117">Table-per-Hierarchy versus Table-per-Type Inheritance</span></span>

<span data-ttu-id="89b37-118">在物件導向程式設計中，您可以使用繼承，讓您更輕鬆地使用相關的類別。</span><span class="sxs-lookup"><span data-stu-id="89b37-118">In object-oriented programming, you can use inheritance to make it easier to work with related classes.</span></span> <span data-ttu-id="89b37-119">例如，`Instructor`和`Student`中的類別`School`資料模型共用數個屬性，這會導致多餘的程式碼：</span><span class="sxs-lookup"><span data-stu-id="89b37-119">For example, the `Instructor` and `Student` classes in the `School` data model share several properties, which results in redundant code:</span></span>

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

<span data-ttu-id="89b37-121">假設您想要針對 `Instructor` 和 `Student` 實體所共用的屬性消除多餘的程式碼。</span><span class="sxs-lookup"><span data-stu-id="89b37-121">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="89b37-122">您可以建立`Person`基底類別，其中包含這些共用屬性，則請`Instructor`和`Student`實體繼承自該基底類別，如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="89b37-122">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

<span data-ttu-id="89b37-124">有幾種方式可以在資料庫中表示此繼承結構。</span><span class="sxs-lookup"><span data-stu-id="89b37-124">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="89b37-125">您可能會有`Person`包含學生和講師單一資料表中的相關資訊的資料表。</span><span class="sxs-lookup"><span data-stu-id="89b37-125">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="89b37-126">某些資料行可以只對講師套用 (`HireDate`)，有些則只能學生 (`EnrollmentDate`)，有一些兩個 (`LastName`， `FirstName`)。</span><span class="sxs-lookup"><span data-stu-id="89b37-126">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="89b37-127">一般而言，您就必須*鑑別子*指出哪種類型的每個資料列的資料行代表。</span><span class="sxs-lookup"><span data-stu-id="89b37-127">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="89b37-128">例如，鑑別子資料行的 "Instructor" 代表講師，而 "Student" 代表學生。</span><span class="sxs-lookup"><span data-stu-id="89b37-128">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Table-per-hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

<span data-ttu-id="89b37-130">這種模式的單一資料庫資料表產生實體繼承結構會呼叫*資料表每個階層*(TPH) 繼承。</span><span class="sxs-lookup"><span data-stu-id="89b37-130">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="89b37-131">替代方法是讓資料庫看起來更像繼承結構。</span><span class="sxs-lookup"><span data-stu-id="89b37-131">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="89b37-132">例如，您無法在只有名稱欄位`Person`資料表，並有個別`Instructor`和`Student`日期欄位的資料表。</span><span class="sxs-lookup"><span data-stu-id="89b37-132">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Table-per-type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

<span data-ttu-id="89b37-134">此模式的資料庫資料表的每個實體類別就稱為*資料表每個型別*(TPT) 繼承。</span><span class="sxs-lookup"><span data-stu-id="89b37-134">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="89b37-135">因為 TPT 模式可能會導致複雜的聯結查詢 TPH 繼承模式通常 TPT 繼承的模式，比在 Entity framework 提供更佳的效能。</span><span class="sxs-lookup"><span data-stu-id="89b37-135">TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span> <span data-ttu-id="89b37-136">本教學課程將示範如何實作 TPH 繼承。</span><span class="sxs-lookup"><span data-stu-id="89b37-136">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="89b37-137">您將會執行，藉由執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="89b37-137">You'll do that by performing the following steps:</span></span>

- <span data-ttu-id="89b37-138">建立`Person`類別，並變更`Instructor`和`Student`類別衍生自`Person`。</span><span class="sxs-lookup"><span data-stu-id="89b37-138">Create a `Person` class and change the `Instructor` and `Student` classes to derive from `Person`.</span></span>
- <span data-ttu-id="89b37-139">資料庫內容類別加入至資料庫模型對應程式碼。</span><span class="sxs-lookup"><span data-stu-id="89b37-139">Add model-to-database mapping code to the database context class.</span></span>
- <span data-ttu-id="89b37-140">變更`InstructorID`和`StudentID`整個專案的參考`PersonID`。</span><span class="sxs-lookup"><span data-stu-id="89b37-140">Change `InstructorID` and `StudentID` references throughout the project to `PersonID`.</span></span>

## <a name="creating-the-person-class"></a><span data-ttu-id="89b37-141">建立個人類別</span><span class="sxs-lookup"><span data-stu-id="89b37-141">Creating the Person Class</span></span>

 <span data-ttu-id="89b37-142">注意： 您將無法建立下列類別，除非您更新使用這些類別的控制站之後，編譯專案。</span><span class="sxs-lookup"><span data-stu-id="89b37-142">Note: You won't be able to compile the project after creating the classes below until you update the controllers that uses these classes.</span></span> 

<span data-ttu-id="89b37-143">在*模型*資料夾中，建立*Person.cs*和範本程式碼取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="89b37-143">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="89b37-144">在*Instructor.cs*，衍生`Instructor`類別從`Person`類別，並移除索引鍵和名稱的欄位。</span><span class="sxs-lookup"><span data-stu-id="89b37-144">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="89b37-145">程式碼看起來應該如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="89b37-145">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="89b37-146">類似變更*Student.cs*。</span><span class="sxs-lookup"><span data-stu-id="89b37-146">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="89b37-147">`Student`類別看起來像下列的範例：</span><span class="sxs-lookup"><span data-stu-id="89b37-147">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a><span data-ttu-id="89b37-148">加入至模型的人員實體類型</span><span class="sxs-lookup"><span data-stu-id="89b37-148">Adding the Person Entity Type to the Model</span></span>

<span data-ttu-id="89b37-149">在*SchoolContext.cs*，新增`DbSet`屬性`Person`實體類型：</span><span class="sxs-lookup"><span data-stu-id="89b37-149">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="89b37-150">這就是 Entity Framework 為了設定單表繼承而必須執行的所有工作。</span><span class="sxs-lookup"><span data-stu-id="89b37-150">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="89b37-151">您會發現，當資料庫是重新建立，它會有`Person`資料表取代`Student`和`Instructor`資料表。</span><span class="sxs-lookup"><span data-stu-id="89b37-151">As you'll see, when the database is re-created, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="changing-instructorid-and-studentid-to-personid"></a><span data-ttu-id="89b37-152">變更 PersonID InstructorID 和 StudentID</span><span class="sxs-lookup"><span data-stu-id="89b37-152">Changing InstructorID and StudentID to PersonID</span></span>

<span data-ttu-id="89b37-153">在*SchoolContext.cs*，講師課程對應陳述式中變更`MapRightKey("InstructorID")`至`MapRightKey("PersonID")`:</span><span class="sxs-lookup"><span data-stu-id="89b37-153">In *SchoolContext.cs*, in the Instructor-Course mapping statement, change `MapRightKey("InstructorID")` to `MapRightKey("PersonID")`:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

<span data-ttu-id="89b37-154">這項變更不是必要的。只會變更 InstructorID 中的資料行的多對多聯結資料表的名稱。</span><span class="sxs-lookup"><span data-stu-id="89b37-154">This change isn't required; it just changes the name of the InstructorID column in the many-to-many join table.</span></span> <span data-ttu-id="89b37-155">如果您保留 InstructorID 相同的名稱時，應用程式就仍然正常運作。</span><span class="sxs-lookup"><span data-stu-id="89b37-155">If you left the name as InstructorID, the application would still work correctly.</span></span> <span data-ttu-id="89b37-156">以下是 已完成*SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="89b37-156">Here is the completed *SchoolContext.cs*:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

<span data-ttu-id="89b37-157">接下來您必須變更`InstructorID`至`PersonID`和`StudentID`至`PersonID`整個專案***除了***的時間戳記移轉檔案中*移轉*資料夾。</span><span class="sxs-lookup"><span data-stu-id="89b37-157">Next you need to change `InstructorID` to `PersonID` and `StudentID` to `PersonID` throughout the project ***except*** in the time-stamped migrations files in the *Migrations* folder.</span></span> <span data-ttu-id="89b37-158">若要這樣做，您要尋找並開啟予以變更，需要的檔案，然後在開啟的檔案上執行全域的變更。</span><span class="sxs-lookup"><span data-stu-id="89b37-158">To do that you'll find and open only the files that need to be changed, then perform a global change on the opened files.</span></span> <span data-ttu-id="89b37-159">中唯一的檔案*移轉*應該變更的資料夾是*Migrations\Configuration.cs。*</span><span class="sxs-lookup"><span data-stu-id="89b37-159">The only file in the *Migrations* folder you should change is *Migrations\Configuration.cs.*</span></span>

1. > [!IMPORTANT]
   > <span data-ttu-id="89b37-160">關閉所有開啟的檔案，Visual Studio 中開始。</span><span class="sxs-lookup"><span data-stu-id="89b37-160">Begin by closing all the open files in Visual Studio.</span></span>
2. <span data-ttu-id="89b37-161">按一下**尋找和取代-尋找所有檔案**中**編輯**功能表上，並在專案中包含的所有檔案然後搜尋`InstructorID`。</span><span class="sxs-lookup"><span data-stu-id="89b37-161">Click **Find and Replace -- Find all Files** in the **Edit** menu, and then search for all files in the project that contain `InstructorID`.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. <span data-ttu-id="89b37-162">開啟的每個檔案**尋找結果**視窗***除了***&lt;時間戳記&gt;*\_.cs*移轉檔案中*移轉*資料夾中，按兩下每個檔案的一條線。</span><span class="sxs-lookup"><span data-stu-id="89b37-162">Open each file in the **Find Results** window ***except*** the &lt;time-stamp&gt;*\_.cs* migration files in the *Migrations* folder, by double-clicking one line for each file.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. <span data-ttu-id="89b37-163">開啟**檔案中取代**對話方塊，並變更**查看**至**所有開啟的文件**。</span><span class="sxs-lookup"><span data-stu-id="89b37-163">Open the **Replace in Files** dialog and change **Look in** to **All Open Documents**.</span></span>
5. <span data-ttu-id="89b37-164">使用**檔案中取代**對話方塊，即可變更所有`InstructorID`至 `PersonID.`</span><span class="sxs-lookup"><span data-stu-id="89b37-164">Use the **Replace in Files** dialog to change all `InstructorID` to `PersonID.`</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. <span data-ttu-id="89b37-165">尋找專案中所包含的所有檔案`StudentID`。</span><span class="sxs-lookup"><span data-stu-id="89b37-165">Find all the files in the project that contain `StudentID`.</span></span>
7. <span data-ttu-id="89b37-166">開啟的每個檔案**尋找結果**視窗***除了***&lt;時間戳記&gt;*\_\*.cs*移轉檔案在*移轉*資料夾中，按兩下每個檔案的一條線。</span><span class="sxs-lookup"><span data-stu-id="89b37-166">Open each file in the **Find Results** window ***except*** the &lt;time-stamp&gt;*\_\*.cs* migration files in the *Migrations* folder, by double-clicking one line for each file.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. <span data-ttu-id="89b37-167">開啟**檔案中取代**對話方塊，並變更**查看**至**所有開啟的文件**。</span><span class="sxs-lookup"><span data-stu-id="89b37-167">Open the **Replace in Files** dialog and change **Look in** to **All Open Documents**.</span></span>
9. <span data-ttu-id="89b37-168">使用**檔案中取代**對話方塊，即可變更所有`StudentID`至`PersonID`。</span><span class="sxs-lookup"><span data-stu-id="89b37-168">Use the **Replace in Files** dialog to change all `StudentID` to `PersonID`.</span></span>   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. <span data-ttu-id="89b37-169">建置專案。</span><span class="sxs-lookup"><span data-stu-id="89b37-169">Build the project.</span></span>

<span data-ttu-id="89b37-170">(請注意，這將示範*缺點*的`classnameID`模式來命名主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="89b37-170">(Note that this demonstrates a *disadvantage* of the `classnameID` pattern for naming primary keys.</span></span> <span data-ttu-id="89b37-171">如果您有不含類別名稱，前面加上命名主索引鍵識別碼*沒有*重新命名為必要操作現在。)</span><span class="sxs-lookup"><span data-stu-id="89b37-171">If you had named primary keys ID without prefixing the class name, *no* renaming would be necessary now.)</span></span>

## <a name="create-and-update-a-migrations-file"></a><span data-ttu-id="89b37-172">建立和更新的移轉檔案</span><span class="sxs-lookup"><span data-stu-id="89b37-172">Create and Update a Migrations File</span></span>

<span data-ttu-id="89b37-173">在 封裝管理員主控台 (PMC)，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="89b37-173">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="89b37-174">執行`Update-Database`PMC 命令。</span><span class="sxs-lookup"><span data-stu-id="89b37-174">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="89b37-175">命令會在此時失敗，因為我們有現有的資料移轉並不知道如何處理。</span><span class="sxs-lookup"><span data-stu-id="89b37-175">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="89b37-176">您會收到下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="89b37-176">You get the following error:</span></span>

<span data-ttu-id="89b37-177">*與外部索引鍵條件約束的 ALTER TABLE 陳述式衝突 」 FK\_dbo。部門\_dbo。人員\_PersonID"。衝突發生在資料庫"ContosoUniversity"，資料表"dbo。Person"，'PersonID' 資料行。*</span><span class="sxs-lookup"><span data-stu-id="89b37-177">*The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK\_dbo.Department\_dbo.Person\_PersonID". The conflict occurred in database "ContosoUniversity", table "dbo.Person", column 'PersonID'.*</span></span>

<span data-ttu-id="89b37-178">開啟*移轉\&lt; 時間戳記&gt;\_Inheritance.cs*和取代`Up`方法取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="89b37-178">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

<span data-ttu-id="89b37-179">執行`update-database`命令一次。</span><span class="sxs-lookup"><span data-stu-id="89b37-179">Run the `update-database` command again.</span></span>

> [!NOTE]
> <span data-ttu-id="89b37-180">很可能在移轉資料和進行結構描述變更時，取得其他錯誤。</span><span class="sxs-lookup"><span data-stu-id="89b37-180">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="89b37-181">如果您移轉時發生錯誤無法解決，您可以繼續這個教學課程中的連接字串，進而*Web.config*檔案或刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="89b37-181">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or deleting the database.</span></span> <span data-ttu-id="89b37-182">最簡單的方法是在資料庫重新命名*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="89b37-182">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="89b37-183">例如，將資料庫名稱變更為 CU\_測試下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="89b37-183">For example, change the database name to CU\_test as shown in the following example:</span></span>
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> <span data-ttu-id="89b37-184">使用新資料庫時，沒有資料移轉，而`update-database`命令是更有可能能順利完成。</span><span class="sxs-lookup"><span data-stu-id="89b37-184">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="89b37-185">如需有關如何刪除資料庫的指示，請參閱[如何卸除資料庫，從 Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)。</span><span class="sxs-lookup"><span data-stu-id="89b37-185">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="89b37-186">如果您採用這個方法，才能繼續進行本教學課程，略過部署步驟在本教學課程結尾處之後自動執行移轉時，已部署的站台會得到相同的錯誤。</span><span class="sxs-lookup"><span data-stu-id="89b37-186">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial, since the deployed site would get the same error when it runs migrations automatically.</span></span> <span data-ttu-id="89b37-187">如果您想要對移轉錯誤進行疑難排解時，最佳的資源是 Entity Framework 論壇或 StackOverflow.com 之一。</span><span class="sxs-lookup"><span data-stu-id="89b37-187">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>


## <a name="testing"></a><span data-ttu-id="89b37-188">測試</span><span class="sxs-lookup"><span data-stu-id="89b37-188">Testing</span></span>

<span data-ttu-id="89b37-189">執行網站，然後再次嘗試各種不同的頁面。</span><span class="sxs-lookup"><span data-stu-id="89b37-189">Run the site and try various pages.</span></span> <span data-ttu-id="89b37-190">一切項目的運作與之前一樣。</span><span class="sxs-lookup"><span data-stu-id="89b37-190">Everything works the same as it did before.</span></span>

<span data-ttu-id="89b37-191">在**伺服器總管 中，**展開**SchoolContext**然後**資料表**，而且您會看到的**學生**和**講師**資料表已取代**人員**資料表。</span><span class="sxs-lookup"><span data-stu-id="89b37-191">In **Server Explorer,** expand **SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="89b37-192">展開**人員**資料表，而且您看到它有使用中的資料行的所有**學生**和**講師**資料表。</span><span class="sxs-lookup"><span data-stu-id="89b37-192">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="89b37-194">以滑鼠右鍵按一下 Person 資料表，然後按一下 [顯示資料表資料] 以查看鑑別子資料行。</span><span class="sxs-lookup"><span data-stu-id="89b37-194">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="89b37-195">下圖說明新的 School 資料庫的結構：</span><span class="sxs-lookup"><span data-stu-id="89b37-195">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a><span data-ttu-id="89b37-197">總結</span><span class="sxs-lookup"><span data-stu-id="89b37-197">Summary</span></span>

<span data-ttu-id="89b37-198">現在的實作每個階層的資料表繼承`Person`， `Student`，和`Instructor`類別。</span><span class="sxs-lookup"><span data-stu-id="89b37-198">Table-per-hierarchy inheritance has now been implemented for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="89b37-199">如需有關這個主題以及其他的繼承結構的詳細資訊，請參閱[繼承對應策略](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx)Morteza Manavi 部落格上。</span><span class="sxs-lookup"><span data-stu-id="89b37-199">For more information about this and other inheritance structures, see [Inheritance Mapping Strategies](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) on Morteza Manavi's blog.</span></span> <span data-ttu-id="89b37-200">在下一個教學課程中，您會看到一些方式來實作的儲存機制和工作模式的單位。</span><span class="sxs-lookup"><span data-stu-id="89b37-200">In the next tutorial you'll see some ways to implement the repository and unit of work patterns.</span></span>

<span data-ttu-id="89b37-201">Entity Framework 中的其他資源連結位於[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="89b37-201">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="89b37-202">[上一頁](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一頁](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="89b37-202">[Previous](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)</span></span>
