---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: 什麼是 Entity Framework 4.0 新功能 |Microsoft 文件
author: tdykstra
description: 此教學課程系列為基礎所建立的開始使用 Entity Framework 4.0 教學課程系列的 Contoso 大學 web 應用程式。 I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 04444ce98fa60045cf617a6c518dd55677258148
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889182"
---
<a name="whats-new-in-the-entity-framework-40"></a><span data-ttu-id="c44ef-104">什麼是 Entity Framework 4.0 新功能</span><span class="sxs-lookup"><span data-stu-id="c44ef-104">What's New in the Entity Framework 4.0</span></span>
====================
<span data-ttu-id="c44ef-105">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c44ef-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="c44ef-106">此教學課程系列為基礎所建立的 Contoso 大學 web 應用程式[Entity Framework 使用者入門](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="c44ef-106">This tutorial series builds on the Contoso University web application that is created by the [Getting Started with the Entity Framework](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) tutorial series.</span></span> <span data-ttu-id="c44ef-107">如果您未完成先前的教學課程，您可以身分本教學課程的起始點來[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)您會建立。</span><span class="sxs-lookup"><span data-stu-id="c44ef-107">If you didn't complete the earlier tutorials, as a starting point for this tutorial you can [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) that you would have created.</span></span> <span data-ttu-id="c44ef-108">您也可以[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)完整的教學課程所建立。</span><span class="sxs-lookup"><span data-stu-id="c44ef-108">You can also [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) that is created by the complete tutorial series.</span></span> <span data-ttu-id="c44ef-109">如果您有疑問的教學課程，您可以張貼他們[ASP.NET Entity Framework 論壇](https://forums.asp.net/1227.aspx)。</span><span class="sxs-lookup"><span data-stu-id="c44ef-109">If you have questions about the tutorials, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx).</span></span>


<span data-ttu-id="c44ef-110">在上一個教學課程中，您會看到一些方法來使用 Entity Framework 的 web 應用程式的效能最大化。</span><span class="sxs-lookup"><span data-stu-id="c44ef-110">In the previous tutorial you saw some methods for maximizing the performance of a web application that uses the Entity Framework.</span></span> <span data-ttu-id="c44ef-111">本教學課程會檢閱一些最重要的新功能，Entity Framework 中，第 4 版中，它會連結到資源提供更完整的所有新功能的簡介。</span><span class="sxs-lookup"><span data-stu-id="c44ef-111">This tutorial reviews some of the most important new features in version 4 of the Entity Framework, and it links to resources that provide a more complete introduction to all of the new features.</span></span> <span data-ttu-id="c44ef-112">本教學課程中反白顯示的功能包括：</span><span class="sxs-lookup"><span data-stu-id="c44ef-112">The features highlighted in this tutorial include the following:</span></span>

- <span data-ttu-id="c44ef-113">外部索引鍵關聯。</span><span class="sxs-lookup"><span data-stu-id="c44ef-113">Foreign-key associations.</span></span>
- <span data-ttu-id="c44ef-114">執行使用者定義的 SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="c44ef-114">Executing user-defined SQL commands.</span></span>
- <span data-ttu-id="c44ef-115">模型優先開發。</span><span class="sxs-lookup"><span data-stu-id="c44ef-115">Model-first development.</span></span>
- <span data-ttu-id="c44ef-116">POCO 支援。</span><span class="sxs-lookup"><span data-stu-id="c44ef-116">POCO support.</span></span>

<span data-ttu-id="c44ef-117">此外，本教學課程將簡單介紹*程式碼優先開發*，未來 Entity Framework 的下一個版本的功能。</span><span class="sxs-lookup"><span data-stu-id="c44ef-117">In addition, the tutorial will briefly introduce *code-first development*, a feature that's coming in the next release of the Entity Framework.</span></span>

<span data-ttu-id="c44ef-118">若要開始本教學課程，請啟動 Visual Studio 並開啟您在上一個教學課程使用 Contoso 大學 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c44ef-118">To start the tutorial, start Visual Studio and open the Contoso University web application that you were working with in the previous tutorial.</span></span>

## <a name="foreign-key-associations"></a><span data-ttu-id="c44ef-119">外部索引鍵關聯</span><span class="sxs-lookup"><span data-stu-id="c44ef-119">Foreign-Key Associations</span></span>

<span data-ttu-id="c44ef-120">Entity Framework 3.5 版包含導覽屬性，但其中不包含資料模型中的外部索引鍵屬性。</span><span class="sxs-lookup"><span data-stu-id="c44ef-120">Version 3.5 of the Entity Framework included navigation properties, but it didn't include foreign-key properties in the data model.</span></span> <span data-ttu-id="c44ef-121">例如，`CourseID`和`StudentID`的資料行`StudentGrade`將忽略資料表`StudentGrade`實體。</span><span class="sxs-lookup"><span data-stu-id="c44ef-121">For example, the `CourseID` and `StudentID` columns of the `StudentGrade` table would be omitted from the `StudentGrade` entity.</span></span>

<span data-ttu-id="c44ef-122">[![Image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c44ef-122">[![Image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)</span></span>

<span data-ttu-id="c44ef-123">這種方法的原因是，嚴格來說，外部索引鍵是實體的實作詳細資料，並且不屬於概念性資料模型中。</span><span class="sxs-lookup"><span data-stu-id="c44ef-123">The reason for this approach was that, strictly speaking, foreign keys are a physical implementation detail and don't belong in a conceptual data model.</span></span> <span data-ttu-id="c44ef-124">不過，事實上，通常會很容易使用程式碼中的實體時，您可以直接存取的外部索引鍵。</span><span class="sxs-lookup"><span data-stu-id="c44ef-124">However, as a practical matter, it's often easier to work with entities in code when you have direct access to the foreign keys.</span></span>

<span data-ttu-id="c44ef-125">針對如何外部索引鍵的資料模型的範例，可簡化您的程式碼，請考慮如何您原本的程式碼*DepartmentsAdd.aspx*未加上的頁面。</span><span class="sxs-lookup"><span data-stu-id="c44ef-125">For an example of how foreign keys in the data model can simplify your code, consider how you would have had to code the *DepartmentsAdd.aspx* page without them.</span></span> <span data-ttu-id="c44ef-126">在`Department`實體，`Administrator`屬性是對應至外部索引鍵`PersonID`中`Person`實體。</span><span class="sxs-lookup"><span data-stu-id="c44ef-126">In the `Department` entity, the `Administrator` property is a foreign key that corresponds to `PersonID` in the `Person` entity.</span></span> <span data-ttu-id="c44ef-127">若要建立新的部門 」 和 「 以系統管理員之間的關聯，您必須執行所有已設定的值`Administrator`屬性`ItemInserting`資料繫結控制項的事件處理常式：</span><span class="sxs-lookup"><span data-stu-id="c44ef-127">In order to establish the association between a new department and its administrator, all you had to do was set the value for the `Administrator` property in the `ItemInserting` event handler of the databound control:</span></span>

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

<span data-ttu-id="c44ef-128">資料模型中的外部索引鍵，不會處理`Inserting`事件的資料來源控制項，而不是`ItemInserting`事件的資料繫結控制項，以實體加入至實體集之前，取得實體本身的參考。</span><span class="sxs-lookup"><span data-stu-id="c44ef-128">Without foreign keys in the data model, you'd handle the `Inserting` event of the data source control instead of the `ItemInserting` event of the databound control, in order to get a reference to the entity itself before the entity is added to the entity set.</span></span> <span data-ttu-id="c44ef-129">當您擁有該參考時，您會建立在下列範例類似的使用程式碼的關聯：</span><span class="sxs-lookup"><span data-stu-id="c44ef-129">When you have that reference, you establish the association using code like that in the following examples:</span></span>

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

<span data-ttu-id="c44ef-130">您可以看到在 Entity Framework 小組[上外部索引鍵關聯的部落格文章](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx)，有更大的差異，在程式碼複雜度其他情況。</span><span class="sxs-lookup"><span data-stu-id="c44ef-130">As you can see in the Entity Framework team's [blog post on Foreign Key associations](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx), there are other cases where the difference in code complexity is much greater.</span></span> <span data-ttu-id="c44ef-131">為了滿足那些喜歡存留較簡單的程式碼為了概念資料模型中的實作詳細資料的需求，Entity Framework 現在可以讓您在資料模型中包含外部索引鍵的選項。</span><span class="sxs-lookup"><span data-stu-id="c44ef-131">To meet the needs of those who prefer to live with implementation details in the conceptual data model for the sake of simpler code, the Entity Framework now gives you the option of including foreign keys in the data model.</span></span>

<span data-ttu-id="c44ef-132">在 Entity Framework 詞彙中，如果您在資料模型中包含外部索引鍵使用*外部索引鍵關聯*，如果您排除您所使用的外部索引鍵和*獨立關聯*。</span><span class="sxs-lookup"><span data-stu-id="c44ef-132">In Entity Framework terminology, if you include foreign keys in the data model you're using *foreign key associations*, and if you exclude foreign keys you're using *independent associations*.</span></span>

## <a name="executing-user-defined-sql-commands"></a><span data-ttu-id="c44ef-133">執行使用者定義的 SQL 命令</span><span class="sxs-lookup"><span data-stu-id="c44ef-133">Executing User-Defined SQL Commands</span></span>

<span data-ttu-id="c44ef-134">在舊版的 Entity Framework 中，有無簡單的方式來即時建立您自己的 SQL 命令，並且執行它們。</span><span class="sxs-lookup"><span data-stu-id="c44ef-134">In earlier versions of the Entity Framework, there was no easy way to create your own SQL commands on the fly and run them.</span></span> <span data-ttu-id="c44ef-135">Entity Framework 動態產生的 SQL 命令，或是您必須建立預存程序，並為函式將它匯入。</span><span class="sxs-lookup"><span data-stu-id="c44ef-135">Either the Entity Framework dynamically generated SQL commands for you, or you had to create a stored procedure and import it as a function.</span></span> <span data-ttu-id="c44ef-136">第 4 版新增`ExecuteStoreQuery`和`ExecuteStoreCommand`方法`ObjectContext`類別可讓您更輕鬆地傳遞直接對資料庫的任何查詢。</span><span class="sxs-lookup"><span data-stu-id="c44ef-136">Version 4 adds `ExecuteStoreQuery` and `ExecuteStoreCommand` methods the `ObjectContext` class that make it easier for you to pass any query directly to the database.</span></span>

<span data-ttu-id="c44ef-137">假設 Contoso 大學系統管理員想要能夠在資料庫中執行大量變更，而不需要經歷建立預存程序，並在匯入資料模型的程序。</span><span class="sxs-lookup"><span data-stu-id="c44ef-137">Suppose Contoso University administrators want to be able to perform bulk changes in the database without having to go through the process of creating a stored procedure and importing it into the data model.</span></span> <span data-ttu-id="c44ef-138">其第一個要求是對可讓他們變更數目的資料庫中的所有課程信用額度的頁面。</span><span class="sxs-lookup"><span data-stu-id="c44ef-138">Their first request is for a page that lets them change the number of credits for all courses in the database.</span></span> <span data-ttu-id="c44ef-139">他們想要可以輸入要使用的值相乘的數字在網頁上，每個`Course`資料列的`Credits`資料行。</span><span class="sxs-lookup"><span data-stu-id="c44ef-139">On the web page, they want to be able to enter a number to use to multiply the value of every `Course` row's `Credits` column.</span></span>

<span data-ttu-id="c44ef-140">建立新的頁面使用*Site.Master*主版頁面並將其命名*UpdateCredits.aspx*。</span><span class="sxs-lookup"><span data-stu-id="c44ef-140">Create a new page that uses the *Site.Master* master page and name it *UpdateCredits.aspx*.</span></span> <span data-ttu-id="c44ef-141">然後加入下列標記以`Content`控制項，名為`Content2`:</span><span class="sxs-lookup"><span data-stu-id="c44ef-141">Then add the following markup to the `Content` control named `Content2`:</span></span>

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

<span data-ttu-id="c44ef-142">這個標記會建立`TextBox`控制使用者可以在其中輸入數值，`Button`控制項才能執行此命令中，按一下與`Label`控制項，以便指出受影響的資料列數目。</span><span class="sxs-lookup"><span data-stu-id="c44ef-142">This markup creates a `TextBox` control in which the user can enter the multiplier value, a `Button` control to click in order to execute the command, and a `Label` control for indicating the number of rows affected.</span></span>

<span data-ttu-id="c44ef-143">開啟*UpdateCredits.aspx.cs*，並加入下列`using`陳述式和按鈕的處理常式`Click`事件：</span><span class="sxs-lookup"><span data-stu-id="c44ef-143">Open *UpdateCredits.aspx.cs*, and add the following `using` statement and a handler for the button's `Click` event:</span></span>

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

<span data-ttu-id="c44ef-144">此程式碼會執行 SQL`Update`命令使用文字方塊內的值，並使用標籤顯示的受影響的資料列數目。</span><span class="sxs-lookup"><span data-stu-id="c44ef-144">This code executes the SQL `Update` command using the value in the text box and uses the label to display the number of rows affected.</span></span> <span data-ttu-id="c44ef-145">執行網頁之前，先執行*Courses.aspx*頁面，即可取得 「 之前 」 圖片的一些資料。</span><span class="sxs-lookup"><span data-stu-id="c44ef-145">Before you run the page, run the *Courses.aspx* page to get a "before" picture of some data.</span></span>

<span data-ttu-id="c44ef-146">[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="c44ef-146">[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)</span></span>

<span data-ttu-id="c44ef-147">執行*UpdateCredits.aspx*乘數，輸入 「 10 」，然後按**Execute**。</span><span class="sxs-lookup"><span data-stu-id="c44ef-147">Run *UpdateCredits.aspx*, enter "10" as the multiplier, and then click **Execute**.</span></span>

<span data-ttu-id="c44ef-148">[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="c44ef-148">[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)</span></span>

<span data-ttu-id="c44ef-149">執行*Courses.aspx*頁面，即可查看變更的資料。</span><span class="sxs-lookup"><span data-stu-id="c44ef-149">Run the *Courses.aspx* page again to see the changed data.</span></span>

<span data-ttu-id="c44ef-150">[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="c44ef-150">[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)</span></span>

<span data-ttu-id="c44ef-151">(如果您想要設定的信用額度數目設回其原始值，在*UpdateCredits.aspx.cs*變更`Credits * {0}`至`Credits / {0}`並重新執行 頁面上，輸入 10 做為除數。)</span><span class="sxs-lookup"><span data-stu-id="c44ef-151">(If you want to set the number of credits back to their original values, in *UpdateCredits.aspx.cs* change `Credits * {0}` to `Credits / {0}` and re-run the page, entering 10 as the divisor.)</span></span>

<span data-ttu-id="c44ef-152">如需有關如何執行您在程式碼中定義的查詢的詳細資訊，請參閱[如何： 直接執行命令對資料來源](https://msdn.microsoft.com/library/ee358769.aspx)。</span><span class="sxs-lookup"><span data-stu-id="c44ef-152">For more information about executing queries that you define in code, see [How to: Directly Execute Commands Against the Data Source](https://msdn.microsoft.com/library/ee358769.aspx).</span></span>

## <a name="model-first-development"></a><span data-ttu-id="c44ef-153">模型優先開發</span><span class="sxs-lookup"><span data-stu-id="c44ef-153">Model-First Development</span></span>

<span data-ttu-id="c44ef-154">這些逐步解說中您會先建立資料庫和資料庫結構為基礎的資料模型，則產生。</span><span class="sxs-lookup"><span data-stu-id="c44ef-154">In these walkthroughs you created the database first and then generated the data model based on the database structure.</span></span> <span data-ttu-id="c44ef-155">Entity Framework 4 中，您可以改為開頭的資料模型，並產生資料模型結構為基礎的資料庫。</span><span class="sxs-lookup"><span data-stu-id="c44ef-155">In the Entity Framework 4 you can start with the data model instead and generate the database based on the data model structure.</span></span> <span data-ttu-id="c44ef-156">如果您要建立的資料庫不存在的應用程式，模型第一種方法可讓您建立實體和關聯性在概念上對應用程式時不擔心實體實作詳細資料意義.</span><span class="sxs-lookup"><span data-stu-id="c44ef-156">If you're creating an application for which the database doesn't already exist, the model-first approach enables you to create entities and relationships that make sense conceptually for the application, while not worrying about physical implementation details.</span></span> <span data-ttu-id="c44ef-157">（這會保持只能透過開發的初始階段，則為 true 不過。</span><span class="sxs-lookup"><span data-stu-id="c44ef-157">(This remains true only through the initial stages of development, however.</span></span> <span data-ttu-id="c44ef-158">最後將會建立資料庫，並會在實際執行資料，並重新建立從模型將無法再實際;此時，您會回到資料庫第一種方法。）</span><span class="sxs-lookup"><span data-stu-id="c44ef-158">Eventually the database will be created and will have production data in it, and recreating it from the model will no longer be practical; at that point you'll be back to the database-first approach.)</span></span>

<span data-ttu-id="c44ef-159">在本章節的教學課程中，您將建立簡單的資料模型，並從其中產生資料庫。</span><span class="sxs-lookup"><span data-stu-id="c44ef-159">In this section of the tutorial, you'll create a simple data model and generate the database from it.</span></span>

<span data-ttu-id="c44ef-160">在**方案總管 中**，以滑鼠右鍵按一下*DAL*資料夾，然後選取**加入新項目**。</span><span class="sxs-lookup"><span data-stu-id="c44ef-160">In **Solution Explorer**, right-click the *DAL* folder and select **Add New Item**.</span></span> <span data-ttu-id="c44ef-161">在**加入新項目**對話方塊的 **已安裝的範本**選取**資料**，然後選取  **ADO.NET 實體資料模型**範本.</span><span class="sxs-lookup"><span data-stu-id="c44ef-161">In the **Add New Item** dialog box, under **Installed Templates** select **Data** and then select the **ADO.NET Entity Data Model** template.</span></span> <span data-ttu-id="c44ef-162">新的檔案名稱*AlumniAssociationModel.edmx*按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="c44ef-162">Name the new file *AlumniAssociationModel.edmx* and click **Add**.</span></span>

<span data-ttu-id="c44ef-163">[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="c44ef-163">[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)</span></span>

<span data-ttu-id="c44ef-164">這會啟動實體資料模型精靈。</span><span class="sxs-lookup"><span data-stu-id="c44ef-164">This launches the Entity Data Model Wizard.</span></span> <span data-ttu-id="c44ef-165">在**選擇模型內容**步驟中，選取**空的模型**，然後按一下 **完成**。</span><span class="sxs-lookup"><span data-stu-id="c44ef-165">In the **Choose Model Contents** step, select **Empty Model** and then click **Finish**.</span></span>

<span data-ttu-id="c44ef-166">[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="c44ef-166">[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)</span></span>

<span data-ttu-id="c44ef-167">**Entity Data Model Designer**與空白設計介面隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="c44ef-167">The **Entity Data Model Designer** opens with a blank design surface.</span></span> <span data-ttu-id="c44ef-168">拖曳**實體**項目從**工具箱**拖曳至設計介面。</span><span class="sxs-lookup"><span data-stu-id="c44ef-168">Drag an **Entity** item from the **Toolbox** onto the design surface.</span></span>

<span data-ttu-id="c44ef-169">[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="c44ef-169">[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)</span></span>

<span data-ttu-id="c44ef-170">變更實體名稱，從`Entity1`至`Alumnus`，變更`Id`屬性名稱，以`AlumnusId`，並加入新的純量屬性，名為`Name`。</span><span class="sxs-lookup"><span data-stu-id="c44ef-170">Change the entity name from `Entity1` to `Alumnus`, change the `Id` property name to `AlumnusId`, and add a new scalar property named `Name`.</span></span> <span data-ttu-id="c44ef-171">若要將新屬性加入您可以按 Enter 鍵的名稱變更之後`Id`資料行，或以滑鼠右鍵按一下實體，並選取**加入純量屬性**。</span><span class="sxs-lookup"><span data-stu-id="c44ef-171">To add new properties you can press Enter after changing the name of the `Id` column, or right-click the entity and select **Add Scalar Property**.</span></span> <span data-ttu-id="c44ef-172">新屬性的預設類型是`String`，這是此簡單的示範，不過沒有關係，但當然您也可以變更中的資料類型等**屬性**視窗。</span><span class="sxs-lookup"><span data-stu-id="c44ef-172">The default type for new properties is `String`, which is fine for this simple demonstration, but of course you can change things like data type in the **Properties** window.</span></span>

<span data-ttu-id="c44ef-173">相同的方式來建立另一個實體並將其命名`Donation`。</span><span class="sxs-lookup"><span data-stu-id="c44ef-173">Create another entity the same way and name it `Donation`.</span></span> <span data-ttu-id="c44ef-174">變更`Id`屬性`DonationId`並新增名為的純量屬性`DateAndAmount`。</span><span class="sxs-lookup"><span data-stu-id="c44ef-174">Change the `Id` property to `DonationId` and add a scalar property named `DateAndAmount`.</span></span>

<span data-ttu-id="c44ef-175">[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="c44ef-175">[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)</span></span>

<span data-ttu-id="c44ef-176">若要新增這兩個實體之間的關聯，以滑鼠右鍵按一下`Alumnus`實體中，選取**新增**，然後選取**關聯**。</span><span class="sxs-lookup"><span data-stu-id="c44ef-176">To add an association between these two entities, right-click the `Alumnus` entity, select **Add**, and then select **Association**.</span></span>

<span data-ttu-id="c44ef-177">[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="c44ef-177">[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)</span></span>

<span data-ttu-id="c44ef-178">中的預設值**新增關聯**對話方塊都是您想要的結果 （一對多、 包含導覽屬性，包含外部索引鍵），所以只需按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="c44ef-178">The default values in the **Add Association** dialog box are what you want (one-to-many, include navigation properties, include foreign keys), so just click **OK**.</span></span>

<span data-ttu-id="c44ef-179">[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="c44ef-179">[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)</span></span>

<span data-ttu-id="c44ef-180">設計工具加入關聯線和外部索引鍵屬性。</span><span class="sxs-lookup"><span data-stu-id="c44ef-180">The designer adds an association line and a foreign-key property.</span></span>

<span data-ttu-id="c44ef-181">[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="c44ef-181">[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)</span></span>

<span data-ttu-id="c44ef-182">現在您已準備好要建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="c44ef-182">Now you're ready to create the database.</span></span> <span data-ttu-id="c44ef-183">以滑鼠右鍵按一下設計介面，並選取**由模型產生資料庫**。</span><span class="sxs-lookup"><span data-stu-id="c44ef-183">Right-click the design surface and select **Generate Database from Model**.</span></span>

<span data-ttu-id="c44ef-184">[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)</span><span class="sxs-lookup"><span data-stu-id="c44ef-184">[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)</span></span>

<span data-ttu-id="c44ef-185">這會啟動產生資料庫精靈。</span><span class="sxs-lookup"><span data-stu-id="c44ef-185">This launches the Generate Database Wizard.</span></span> <span data-ttu-id="c44ef-186">（如果您看到警告，指出沒有對應的實體，您可以忽略這些緩。）</span><span class="sxs-lookup"><span data-stu-id="c44ef-186">(If you see warnings that indicate that the entities aren't mapped, you can ignore those for the time being.)</span></span>

<span data-ttu-id="c44ef-187">在**選擇資料連接**步驟中，按一下 **新連線**。</span><span class="sxs-lookup"><span data-stu-id="c44ef-187">In the **Choose Your Data Connection** step, click **New Connection**.</span></span>

<span data-ttu-id="c44ef-188">[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="c44ef-188">[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)</span></span>

<span data-ttu-id="c44ef-189">在**連接屬性**對話方塊，選取 SQL Server Express 的本機執行個體，為資料庫命名`AlumniAsssociation`。</span><span class="sxs-lookup"><span data-stu-id="c44ef-189">In the **Connection Properties** dialog box, select the local SQL Server Express instance and name the database `AlumniAsssociation`.</span></span>

<span data-ttu-id="c44ef-190">[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)</span><span class="sxs-lookup"><span data-stu-id="c44ef-190">[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)</span></span>

<span data-ttu-id="c44ef-191">按一下**是**當詢問您要建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="c44ef-191">Click **Yes** when you're asked if you want to create the database.</span></span> <span data-ttu-id="c44ef-192">當**選擇資料連接**步驟一次，請按一下 **下一步**。</span><span class="sxs-lookup"><span data-stu-id="c44ef-192">When the **Choose Your Data Connection** step is displayed again, click **Next**.</span></span>

<span data-ttu-id="c44ef-193">在**摘要和設定**步驟中，按一下 **完成**。</span><span class="sxs-lookup"><span data-stu-id="c44ef-193">In the **Summary and Settings** step, click **Finish**.</span></span>

<span data-ttu-id="c44ef-194">[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)</span><span class="sxs-lookup"><span data-stu-id="c44ef-194">[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)</span></span>

<span data-ttu-id="c44ef-195">A *.sql*使用資料定義語言 (DDL) 命令來建立檔案，但您尚未尚未執行的命令。</span><span class="sxs-lookup"><span data-stu-id="c44ef-195">A *.sql* file with the data definition language (DDL) commands is created, but the commands haven't been run yet.</span></span>

<span data-ttu-id="c44ef-196">[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)</span><span class="sxs-lookup"><span data-stu-id="c44ef-196">[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)</span></span>

<span data-ttu-id="c44ef-197">使用一種工具，例如**SQL Server Management Studio**執行指令碼，並建立資料表，您可能執行您在建立時`School`資料庫[快速入門教學課程系列的第一個教學課程](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="c44ef-197">Use a tool such as **SQL Server Management Studio** to run the script and create the tables, as you might have done when you created the `School` database for [the first tutorial in the Getting Started tutorial series](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md).</span></span> <span data-ttu-id="c44ef-198">（除非您下載此資料庫。）</span><span class="sxs-lookup"><span data-stu-id="c44ef-198">(Unless you downloaded the database.)</span></span>

<span data-ttu-id="c44ef-199">您現在可以使用`AlumniAssociation`資料模型，在您的 web pages 您所使用的相同方式`School`模型。</span><span class="sxs-lookup"><span data-stu-id="c44ef-199">You can now use the `AlumniAssociation` data model in your web pages the same way you've been using the `School` model.</span></span> <span data-ttu-id="c44ef-200">若要再試一次時，一些資料加入至資料表並建立顯示資料的網頁。</span><span class="sxs-lookup"><span data-stu-id="c44ef-200">To try this out, add some data to the tables and create a web page that displays the data.</span></span>

<span data-ttu-id="c44ef-201">使用**伺服器總管**，加入下列的資料列來`Alumnus`和`Donation`資料表。</span><span class="sxs-lookup"><span data-stu-id="c44ef-201">Using **Server Explorer**, add the following rows to the `Alumnus` and `Donation` tables.</span></span>

<span data-ttu-id="c44ef-202">[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)</span><span class="sxs-lookup"><span data-stu-id="c44ef-202">[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)</span></span>

<span data-ttu-id="c44ef-203">建立名為的新網頁*Alumni.aspx*使用*Site.Master*主版頁面。</span><span class="sxs-lookup"><span data-stu-id="c44ef-203">Create a new web page named *Alumni.aspx* that uses the *Site.Master* master page.</span></span> <span data-ttu-id="c44ef-204">加入下列標記以`Content`控制項，名為`Content2`:</span><span class="sxs-lookup"><span data-stu-id="c44ef-204">Add the following markup to the `Content` control named `Content2`:</span></span>

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

<span data-ttu-id="c44ef-205">這個標記會建立巢狀`GridView`控制外部顯示申請書名稱以及與內部的一個顯示捐贈日期和金額。</span><span class="sxs-lookup"><span data-stu-id="c44ef-205">This markup creates nested `GridView` controls, the outer one to display alumni names and the inner one to display donation dates and amounts.</span></span>

<span data-ttu-id="c44ef-206">開啟*Alumni.aspx.cs*。</span><span class="sxs-lookup"><span data-stu-id="c44ef-206">Open *Alumni.aspx.cs*.</span></span> <span data-ttu-id="c44ef-207">新增`using`陳述式的資料存取層和外部的處理常式`GridView`控制項的`RowDataBound`事件：</span><span class="sxs-lookup"><span data-stu-id="c44ef-207">Add a `using` statement for the data access layer and a handler for the outer `GridView` control's `RowDataBound` event:</span></span>

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

<span data-ttu-id="c44ef-208">此程式碼將內部`GridView`控制使用`Donations`導覽屬性的目前資料列`Alumnus`實體。</span><span class="sxs-lookup"><span data-stu-id="c44ef-208">This code databinds the inner `GridView` control using the `Donations` navigation property of the current row's `Alumnus` entity.</span></span>

<span data-ttu-id="c44ef-209">執行網頁。</span><span class="sxs-lookup"><span data-stu-id="c44ef-209">Run the page.</span></span>

<span data-ttu-id="c44ef-210">[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)</span><span class="sxs-lookup"><span data-stu-id="c44ef-210">[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)</span></span>

<span data-ttu-id="c44ef-211">(注意： 此頁面包含在可下載專案中，但讓它正常運作，您必須在本機 SQL Server Express 執行個體中建立資料庫，則資料庫不包含 *.mdf*檔案*應用程式\_資料*資料夾。)</span><span class="sxs-lookup"><span data-stu-id="c44ef-211">(Note: This page is included in the downloadable project, but to make it work you must create the database in your local SQL Server Express instance; the database isn't included as an *.mdf* file in the *App\_Data* folder.)</span></span>

<span data-ttu-id="c44ef-212">如需有關如何使用 Entity Framework 模型優先 （contract-first） 功能的詳細資訊，請參閱[Entity Framework 4 中的模型優先](https://msdn.microsoft.com/data/ff830362.aspx)。</span><span class="sxs-lookup"><span data-stu-id="c44ef-212">For more information about using the model-first feature of the Entity Framework, see [Model-First in the Entity Framework 4](https://msdn.microsoft.com/data/ff830362.aspx).</span></span>

## <a name="poco-support"></a><span data-ttu-id="c44ef-213">POCO 支援</span><span class="sxs-lookup"><span data-stu-id="c44ef-213">POCO Support</span></span>

<span data-ttu-id="c44ef-214">當您使用網域導向設計方法時，您會設計資料類別，代表資料與商務網域相關的行為。</span><span class="sxs-lookup"><span data-stu-id="c44ef-214">When you use domain-driven design methodology, you design data classes that represent data and behavior that's relevant to the business domain.</span></span> <span data-ttu-id="c44ef-215">應該獨立於任何特定的技術，用來儲存這些類別 （保存） 資料。也就是說，它們應該*永續性無知*。</span><span class="sxs-lookup"><span data-stu-id="c44ef-215">These classes should be independent of any specific technology used to store (persist) the data; in other words, they should be *persistence ignorant*.</span></span> <span data-ttu-id="c44ef-216">永續性無知之也可以進行類別容易單元測試因為單元測試專案可以使用任何持續性技術是最方便的測試。</span><span class="sxs-lookup"><span data-stu-id="c44ef-216">Persistence ignorance can also make a class easier to unit test because the unit test project can use whatever persistence technology is most convenient for testing.</span></span> <span data-ttu-id="c44ef-217">舊版的 Entity Framework 提供有限的支援永續性無知之實體類別必須繼承自`EntityObject`類別，並因此包含許多實體架構專屬功能。</span><span class="sxs-lookup"><span data-stu-id="c44ef-217">Earlier versions of the Entity Framework offered limited support for persistence ignorance because entity classes had to inherit from the `EntityObject` class and thus included a great deal of Entity Framework-specific functionality.</span></span>

<span data-ttu-id="c44ef-218">Entity Framework 4 導入了使用不會繼承的實體類別的能力`EntityObject`類別，並因此不知道會持續性。</span><span class="sxs-lookup"><span data-stu-id="c44ef-218">The Entity Framework 4 introduces the ability to use entity classes that don't inherit from the `EntityObject` class and therefore are persistence ignorant.</span></span> <span data-ttu-id="c44ef-219">在 Entity Framework 內容中，像這樣的類別通常稱為*純舊 CLR 物件*（POCO 或 POCOs）。</span><span class="sxs-lookup"><span data-stu-id="c44ef-219">In the context of the Entity Framework, classes like this are typically called *plain-old CLR objects* (POCO, or POCOs).</span></span> <span data-ttu-id="c44ef-220">您可以手動撰寫 POCO 類別或您可以自動產生根據現有的資料模型使用 Entity Framework 所提供的文字範本轉換工具組 (T4) 範本。</span><span class="sxs-lookup"><span data-stu-id="c44ef-220">You can write POCO classes manually, or you can automatically generate them based on an existing data model using Text Template Transformation Toolkit (T4) templates provided by the Entity Framework.</span></span>

<span data-ttu-id="c44ef-221">如需使用 POCOs Entity Framework 中的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="c44ef-221">For more information about using POCOs in the Entity Framework, see the following resources:</span></span>

- <span data-ttu-id="c44ef-222">[處理 POCO 實體](https://msdn.microsoft.com/library/dd456853.aspx)。</span><span class="sxs-lookup"><span data-stu-id="c44ef-222">[Working with POCO Entities](https://msdn.microsoft.com/library/dd456853.aspx).</span></span> <span data-ttu-id="c44ef-223">這是 POCOs，概觀與其他有更詳細資訊的文件連結的 MSDN 文件。</span><span class="sxs-lookup"><span data-stu-id="c44ef-223">This is an MSDN document that's an overview of POCOs, with links to other documents that have more detailed information.</span></span>
- <span data-ttu-id="c44ef-224">[逐步解說： POCO Entity Framework 的範本](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx)這是 Entity Framework 的開發小組，關於 POCOs 其他部落格文章的連結的部落格文章。</span><span class="sxs-lookup"><span data-stu-id="c44ef-224">[Walkthrough: POCO Template for the Entity Framework](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx) This is a blog post from the Entity Framework development team, with links to other blog posts about POCOs.</span></span>

## <a name="code-first-development"></a><span data-ttu-id="c44ef-225">程式碼優先開發</span><span class="sxs-lookup"><span data-stu-id="c44ef-225">Code-First Development</span></span>

<span data-ttu-id="c44ef-226">POCO Entity Framework 4 中的支援仍需要您建立資料模型，並將實體類別連結至資料模型。</span><span class="sxs-lookup"><span data-stu-id="c44ef-226">POCO support in the Entity Framework 4 still requires that you create a data model and link your entity classes to the data model.</span></span> <span data-ttu-id="c44ef-227">Entity Framework 的下一個版本會包含名為的功能*程式碼優先開發*。</span><span class="sxs-lookup"><span data-stu-id="c44ef-227">The next release of the Entity Framework will include a feature called *code-first development*.</span></span> <span data-ttu-id="c44ef-228">這項功能可讓您使用 Entity Framework POCO 類別而不需要使用資料模型設計工具或資料模型的 XML 檔案。</span><span class="sxs-lookup"><span data-stu-id="c44ef-228">This feature enables you to use the Entity Framework with your own POCO classes without needing to use either the data model designer or a data model XML file.</span></span> <span data-ttu-id="c44ef-229">(因此，此選項也已經呼叫*純程式碼*;*程式碼優先*和*純程式碼*都會指向相同的 Entity Framework 功能。)</span><span class="sxs-lookup"><span data-stu-id="c44ef-229">(Therefore, this option has also been called *code-only*; *code-first* and *code-only* both refer to the same Entity Framework feature.)</span></span>

<span data-ttu-id="c44ef-230">如需有關如何使用程式開發的程式碼第一個方法的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="c44ef-230">For more information about using the code-first approach to development, see the following resources:</span></span>

- <span data-ttu-id="c44ef-231">[程式碼優先使用 Entity Framework 4 開發](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx)。</span><span class="sxs-lookup"><span data-stu-id="c44ef-231">[Code-First Development with Entity Framework 4](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx).</span></span> <span data-ttu-id="c44ef-232">這是所介紹的程式碼優先開發 Scott Guthrie 的部落格文章。</span><span class="sxs-lookup"><span data-stu-id="c44ef-232">This is a blog post by Scott Guthrie introducing code-first development.</span></span>
- [<span data-ttu-id="c44ef-233">Entity Framework 開發小組部落格-張貼標記的 CodeOnly</span><span class="sxs-lookup"><span data-stu-id="c44ef-233">Entity Framework Development Team Blog - posts tagged CodeOnly</span></span>](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [<span data-ttu-id="c44ef-234">Entity Framework 開發小組部落格-張貼標記 Code First</span><span class="sxs-lookup"><span data-stu-id="c44ef-234">Entity Framework Development Team Blog - posts tagged Code First</span></span>](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [<span data-ttu-id="c44ef-235">MVC Music Store 教學課程-第 4 部分： 模型和資料存取</span><span class="sxs-lookup"><span data-stu-id="c44ef-235">MVC Music Store tutorial - Part 4: Models and Data Access</span></span>](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [<span data-ttu-id="c44ef-236">開始使用 MVC 3-第 4 部分： Entity Framework 程式碼優先開發</span><span class="sxs-lookup"><span data-stu-id="c44ef-236">Getting Started with MVC 3 - Part 4: Entity Framework Code-First Development</span></span>](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

<span data-ttu-id="c44ef-237">此外，新的 MVC 程式碼的第一個教學課程可建立類似於 Contoso 大學應用程式的應用程式預計在 2011 年的 spring 中發行 [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)</span><span class="sxs-lookup"><span data-stu-id="c44ef-237">In addition, a new MVC Code-First tutorial that builds an application similar to the Contoso University application is projected to be published in the spring of 2011 at [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)</span></span>

## <a name="more-information"></a><span data-ttu-id="c44ef-238">更多資訊</span><span class="sxs-lookup"><span data-stu-id="c44ef-238">More Information</span></span>

<span data-ttu-id="c44ef-239">完成此動作以新增功能在 Entity Framework 與此繼續 Entity Framework 的教學課程系列的概觀。</span><span class="sxs-lookup"><span data-stu-id="c44ef-239">This completes the overview to what's new in the Entity Framework and this Continuing with the Entity Framework tutorial series.</span></span> <span data-ttu-id="c44ef-240">如需有關 Entity Framework 4 中未涵蓋的新功能的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="c44ef-240">For more information about new features in the Entity Framework 4 that aren't covered here, see the following resources:</span></span>

- <span data-ttu-id="c44ef-241">[What's New in ADO.NET](https://msdn.microsoft.com/library/ex6y04yf.aspx)的 Entity Framework 版本 4 中的新功能的 MSDN 主題。</span><span class="sxs-lookup"><span data-stu-id="c44ef-241">[What's New in ADO.NET](https://msdn.microsoft.com/library/ex6y04yf.aspx) MSDN topic on new features in version 4 of the Entity Framework.</span></span>
- <span data-ttu-id="c44ef-242">[宣佈適用於 Entity Framework 4 版本](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx)中第 4 版新功能相關的 Entity Framework 開發小組的部落格文章。</span><span class="sxs-lookup"><span data-stu-id="c44ef-242">[Announcing the release of Entity Framework 4](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx) The Entity Framework development team's blog post about new features in version 4.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c44ef-243">上一步</span><span class="sxs-lookup"><span data-stu-id="c44ef-243">Previous</span></span>](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
