---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: "開始使用 Entity Framework 4.0 資料庫中第一次與 ASP.NET 4 Web Form-第 4 部分 |Microsoft 文件"
author: tdykstra
description: "Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 的 ASP.NET Web Form 應用程式。 範例應用程式是..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: 06d129384fc78db21ad1b9224781deab6a0e91a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a><span data-ttu-id="a3d89-104">開始使用 Entity Framework 4.0 資料庫中第一次和 ASP.NET 4 Web Form 第 4 部分</span><span class="sxs-lookup"><span data-stu-id="a3d89-104">Getting Started with Entity Framework 4.0 Database First and ASP.NET 4 Web Forms - Part 4</span></span>
====================
<span data-ttu-id="a3d89-105">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a3d89-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="a3d89-106">Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Web Form 應用程式使用 Entity Framework 4.0 和 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="a3d89-106">The Contoso University sample web application demonstrates how to create ASP.NET Web Forms applications using the Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="a3d89-107">教學課程系列的相關資訊，請參閱[系列的第一個教學課程](the-entity-framework-and-aspnet-getting-started-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="a3d89-107">For information about the tutorial series, see [the first tutorial in the series](the-entity-framework-and-aspnet-getting-started-part-1.md)</span></span>


## <a name="working-with-related-data"></a><span data-ttu-id="a3d89-108">使用相關的資料</span><span class="sxs-lookup"><span data-stu-id="a3d89-108">Working with Related Data</span></span>

<span data-ttu-id="a3d89-109">在上一個教學課程，您使用`EntityDataSource`控制項來篩選、 排序和分組資料。</span><span class="sxs-lookup"><span data-stu-id="a3d89-109">In the previous tutorial you used the `EntityDataSource` control to filter, sort, and group data.</span></span> <span data-ttu-id="a3d89-110">在此教學課程中，將顯示和更新的相關的資料。</span><span class="sxs-lookup"><span data-stu-id="a3d89-110">In this tutorial you'll display and update related data.</span></span>

<span data-ttu-id="a3d89-111">您將建立顯示一份講師講師頁面。</span><span class="sxs-lookup"><span data-stu-id="a3d89-111">You'll create the Instructors page that shows a list of instructors.</span></span> <span data-ttu-id="a3d89-112">當您選取講師時，您會看到一份由該講師教導的課程。</span><span class="sxs-lookup"><span data-stu-id="a3d89-112">When you select an instructor, you see a list of courses taught by that instructor.</span></span> <span data-ttu-id="a3d89-113">當您選取的課程時，您會看到程序過程以及一份學生課程中註冊的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="a3d89-113">When you select a course, you see details for the course and a list of students enrolled in the course.</span></span> <span data-ttu-id="a3d89-114">您可以編輯講師姓名、 雇用日期，以及 office 指派。</span><span class="sxs-lookup"><span data-stu-id="a3d89-114">You can edit the instructor name, hire date, and office assignment.</span></span> <span data-ttu-id="a3d89-115">Office 指派是您透過導覽屬性來存取個別的實體集。</span><span class="sxs-lookup"><span data-stu-id="a3d89-115">The office assignment is a separate entity set that you access through a navigation property.</span></span>

<span data-ttu-id="a3d89-116">您可以連結主要資料標記或程式碼的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="a3d89-116">You can link master data to detail data in markup or in code.</span></span> <span data-ttu-id="a3d89-117">在本教學課程的這個部分，您將使用這兩種方法。</span><span class="sxs-lookup"><span data-stu-id="a3d89-117">In this part of the tutorial, you'll use both methods.</span></span>

<span data-ttu-id="a3d89-118">[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a3d89-118">[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)</span></span>

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a><span data-ttu-id="a3d89-119">顯示和更新 GridView 控制項中的相關的實體</span><span class="sxs-lookup"><span data-stu-id="a3d89-119">Displaying and Updating Related Entities in a GridView Control</span></span>

<span data-ttu-id="a3d89-120">建立名為的新網頁*Instructors.aspx*使用*Site.Master*主版頁面，並加入下列標記以`Content`控制項，名為`Content2`:</span><span class="sxs-lookup"><span data-stu-id="a3d89-120">Create a new web page named *Instructors.aspx* that uses the *Site.Master* master page, and add the following markup to the `Content` control named `Content2`:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

<span data-ttu-id="a3d89-121">這個標記會建立`EntityDataSource`講師和啟用更新控制項。</span><span class="sxs-lookup"><span data-stu-id="a3d89-121">This markup creates an `EntityDataSource` control that selects instructors and enables updates.</span></span> <span data-ttu-id="a3d89-122">`div`項目會呈現在左邊，您可以將資料行稍後新增右邊的標記。</span><span class="sxs-lookup"><span data-stu-id="a3d89-122">The `div` element configures markup to render on the left so that you can add a column on the right later.</span></span>

<span data-ttu-id="a3d89-123">之間`EntityDataSource`標記和結束`</div>`標記中，加入下列標記建立`GridView`控制項和`Label`控制項，您將使用的錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="a3d89-123">Between the `EntityDataSource` markup and the closing `</div>` tag, add the following markup that creates a `GridView` control and a `Label` control that you'll use for error messages:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

<span data-ttu-id="a3d89-124">這`GridView`控制項可讓資料列選取、 反白顯示選取的資料列以淺灰色背景色彩，並指定處理常式 （您將在稍後建立） 的`SelectedIndexChanged`和`Updating`事件。</span><span class="sxs-lookup"><span data-stu-id="a3d89-124">This `GridView` control enables row selection, highlights the selected row with a light gray background color, and specifies handlers (which you'll create later) for the `SelectedIndexChanged` and `Updating` events.</span></span> <span data-ttu-id="a3d89-125">它也會指定`PersonID`如`DataKeyNames`屬性，以便在所選取的資料列索引鍵的值可以傳遞至另一個您稍後會加入的控制項。</span><span class="sxs-lookup"><span data-stu-id="a3d89-125">It also specifies `PersonID` for the `DataKeyNames` property, so that the key value of the selected row can be passed to another control that you'll add later.</span></span>

<span data-ttu-id="a3d89-126">最後一個資料行包含講師 office 指派會儲存在瀏覽屬性的`Person`實體因為它來自相關聯的實體。</span><span class="sxs-lookup"><span data-stu-id="a3d89-126">The last column contains the instructor's office assignment, which is stored in a navigation property of the `Person` entity because it comes from an associated entity.</span></span> <span data-ttu-id="a3d89-127">請注意，`EditItemTemplate`項目會指定`Eval`而不是`Bind`，因為`GridView`控制項不能直接繫結至導覽屬性才能更新它們。</span><span class="sxs-lookup"><span data-stu-id="a3d89-127">Notice that the `EditItemTemplate` element specifies `Eval` instead of `Bind`, because the `GridView` control cannot directly bind to navigation properties in order to update them.</span></span> <span data-ttu-id="a3d89-128">您要更新程式碼中的 office 指派。</span><span class="sxs-lookup"><span data-stu-id="a3d89-128">You'll update the office assignment in code.</span></span> <span data-ttu-id="a3d89-129">若要這樣做，您必須參考`TextBox`控制項，而且您會取得並儲存於`TextBox`控制項的`Init`事件。</span><span class="sxs-lookup"><span data-stu-id="a3d89-129">To do that, you'll need a reference to the `TextBox` control, and you'll get and save that in the `TextBox` control's `Init` event.</span></span>

<span data-ttu-id="a3d89-130">遵循`GridView`控制項是`Label`錯誤訊息所使用的控制項。</span><span class="sxs-lookup"><span data-stu-id="a3d89-130">Following the `GridView` control is a `Label` control that's used for error messages.</span></span> <span data-ttu-id="a3d89-131">控制項的`Visible`屬性是`false`，及檢視狀態已關閉，使僅當程式碼會顯示錯誤回應中出現的標籤。</span><span class="sxs-lookup"><span data-stu-id="a3d89-131">The control's `Visible` property is `false`, and view state is turned off, so that the label will appear only when code makes it visible in response to an error.</span></span>

<span data-ttu-id="a3d89-132">開啟*Instructors.aspx.cs*檔案，然後加入下列`using`陳述式：</span><span class="sxs-lookup"><span data-stu-id="a3d89-132">Open the *Instructors.aspx.cs* file and add the following `using` statement:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

<span data-ttu-id="a3d89-133">部分類別名稱宣告，以保存 office 指派文字方塊中的參考之後，立即將私用類別欄位。</span><span class="sxs-lookup"><span data-stu-id="a3d89-133">Add a private class field immediately after the partial-class name declaration to hold a reference to the office assignment text box.</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

<span data-ttu-id="a3d89-134">新增虛設常式`SelectedIndexChanged`您要稍後加入程式碼的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="a3d89-134">Add a stub for the `SelectedIndexChanged` event handler that you'll add code for later.</span></span> <span data-ttu-id="a3d89-135">也新增 office 指派的處理常式`TextBox`控制項的`Init`事件，讓您可以儲存的參考`TextBox`控制項。</span><span class="sxs-lookup"><span data-stu-id="a3d89-135">Also add a handler for the office assignment `TextBox` control's `Init` event so that you can store a reference to the `TextBox` control.</span></span> <span data-ttu-id="a3d89-136">您將使用這個參考來取得使用者輸入才能更新實體的導覽屬性相關聯的值。</span><span class="sxs-lookup"><span data-stu-id="a3d89-136">You'll use this reference to get the value the user entered in order to update the entity associated with the navigation property.</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

<span data-ttu-id="a3d89-137">您將使用`GridView`控制項的`Updating`事件來更新`Location`屬性相關聯的`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="a3d89-137">You'll use the `GridView` control's `Updating` event to update the `Location` property of the associated `OfficeAssignment` entity.</span></span> <span data-ttu-id="a3d89-138">加入下列處理常式`Updating`事件：</span><span class="sxs-lookup"><span data-stu-id="a3d89-138">Add the following handler for the `Updating` event:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

<span data-ttu-id="a3d89-139">當使用者按一下 執行這個程式碼**更新**中`GridView`資料列。</span><span class="sxs-lookup"><span data-stu-id="a3d89-139">This code is run when the user clicks **Update** in a `GridView` row.</span></span> <span data-ttu-id="a3d89-140">程式碼使用 LINQ to Entities 擷取`OfficeAssignment`目前與相關聯的實體`Person`實體，使用`PersonID`之選取的資料列的事件引數。</span><span class="sxs-lookup"><span data-stu-id="a3d89-140">The code uses LINQ to Entities to retrieve the `OfficeAssignment` entity that's associated with the current `Person` entity, using the `PersonID` of the selected row from the event argument.</span></span>

<span data-ttu-id="a3d89-141">程式碼接著會使用其中一個動作中的值而定`InstructorOfficeTextBox`控制項：</span><span class="sxs-lookup"><span data-stu-id="a3d89-141">The code then takes one of the following actions depending on the value in the `InstructorOfficeTextBox` control:</span></span>

- <span data-ttu-id="a3d89-142">在文字方塊中的值而且沒有任何`OfficeAssignment`實體，若要更新，它會建立一個。</span><span class="sxs-lookup"><span data-stu-id="a3d89-142">If the text box has a value and there's no `OfficeAssignment` entity to update, it creates one.</span></span>
- <span data-ttu-id="a3d89-143">在文字方塊中的值而且沒有`OfficeAssignment`便會更新實體，`Location`屬性值。</span><span class="sxs-lookup"><span data-stu-id="a3d89-143">If the text box has a value and there's an `OfficeAssignment` entity, it updates the `Location` property value.</span></span>
- <span data-ttu-id="a3d89-144">如果是空的文字方塊和`OfficeAssignment`實體存在，它會刪除實體。</span><span class="sxs-lookup"><span data-stu-id="a3d89-144">If the text box is empty and an `OfficeAssignment` entity exists, it deletes the entity.</span></span>

<span data-ttu-id="a3d89-145">在此之後，它會將所做的變更儲存到資料庫。</span><span class="sxs-lookup"><span data-stu-id="a3d89-145">After this, it saves the changes to the database.</span></span> <span data-ttu-id="a3d89-146">如果發生例外狀況，它會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="a3d89-146">If an exception occurs, it displays an error message.</span></span>

<span data-ttu-id="a3d89-147">執行網頁。</span><span class="sxs-lookup"><span data-stu-id="a3d89-147">Run the page.</span></span>

<span data-ttu-id="a3d89-148">[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a3d89-148">[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)</span></span>

<span data-ttu-id="a3d89-149">按一下**編輯**並在文字方塊中，變更所有欄位。</span><span class="sxs-lookup"><span data-stu-id="a3d89-149">Click **Edit** and all fields change to text boxes.</span></span>

<span data-ttu-id="a3d89-150">[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="a3d89-150">[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)</span></span>

<span data-ttu-id="a3d89-151">變更任何這些值，包括**Office 指派**。</span><span class="sxs-lookup"><span data-stu-id="a3d89-151">Change any of these values, including **Office Assignment**.</span></span> <span data-ttu-id="a3d89-152">按一下**更新**您將會看到反映清單中的變更。</span><span class="sxs-lookup"><span data-stu-id="a3d89-152">Click **Update** and you'll see the changes reflected in the list.</span></span>

## <a name="displaying-related-entities-in-a-separate-control"></a><span data-ttu-id="a3d89-153">在個別控制項中顯示相關的實體</span><span class="sxs-lookup"><span data-stu-id="a3d89-153">Displaying Related Entities in a Separate Control</span></span>

<span data-ttu-id="a3d89-154">每個講師可以教導一或多個課程，讓您將新增`EntityDataSource`控制項和`GridView`控制項清單與任何講師講師中選取相關聯的課程`GridView`控制項。</span><span class="sxs-lookup"><span data-stu-id="a3d89-154">Each instructor can teach one or more courses, so you'll add an `EntityDataSource` control and a `GridView` control to list the courses associated with whichever instructor is selected in the instructors `GridView` control.</span></span> <span data-ttu-id="a3d89-155">若要建立的標題和`EntityDataSource`課程實體控制項，加入下列標記之間的錯誤訊息`Label`控制項和關閉`</div>`標記：</span><span class="sxs-lookup"><span data-stu-id="a3d89-155">To create a heading and the `EntityDataSource` control for courses entities, add the following markup between the error message `Label` control and the closing `</div>` tag:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

<span data-ttu-id="a3d89-156">`Where`參數包含的值`PersonID`講師中選取的資料列的`InstructorsGridView`控制項。</span><span class="sxs-lookup"><span data-stu-id="a3d89-156">The `Where` parameter contains the value of the `PersonID` of the instructor whose row is selected in the `InstructorsGridView` control.</span></span> <span data-ttu-id="a3d89-157">`Where`屬性包含可取得所有相關聯的子選擇命令`Person`實體`Course`實體的`People`導覽屬性，並選取`Course`實體只有當其中一個相關聯的`Person`實體包含所選`PersonID`值。</span><span class="sxs-lookup"><span data-stu-id="a3d89-157">The `Where` property contains a subselect command that gets all associated `Person` entities from a `Course` entity's `People` navigation property and selects the `Course` entity only if one of the associated `Person` entities contains the selected `PersonID` value.</span></span>

<span data-ttu-id="a3d89-158">若要建立`GridView`控制項。 加入下列標記緊接`CoursesEntityDataSource`控制項 (在關閉前`</div>`標記):</span><span class="sxs-lookup"><span data-stu-id="a3d89-158">To create the `GridView` control., add the following markup immediately following the `CoursesEntityDataSource` control (before the closing `</div>` tag):</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

<span data-ttu-id="a3d89-159">因為如果沒有講師選取時，就會顯示沒有課程`EmptyDataTemplate`就會包含項目。</span><span class="sxs-lookup"><span data-stu-id="a3d89-159">Because no courses will be displayed if no instructor is selected, an `EmptyDataTemplate` element is included.</span></span>

<span data-ttu-id="a3d89-160">執行網頁。</span><span class="sxs-lookup"><span data-stu-id="a3d89-160">Run the page.</span></span>

<span data-ttu-id="a3d89-161">[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="a3d89-161">[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)</span></span>

<span data-ttu-id="a3d89-162">選取 講師具有指派，一個或多個課程，課程出現在清單中。</span><span class="sxs-lookup"><span data-stu-id="a3d89-162">Select an instructor who has one or more courses assigned, and the course or courses appear in the list.</span></span> <span data-ttu-id="a3d89-163">(注意： 雖然資料庫結構描述允許多個課程，提供與資料庫的測試資料中沒有講師確實具有一個以上的課程。</span><span class="sxs-lookup"><span data-stu-id="a3d89-163">(Note: although the database schema allows multiple courses, in the test data supplied with the database no instructor actually has more than one course.</span></span> <span data-ttu-id="a3d89-164">您可以加入課程資料庫自行使用**伺服器總管**視窗或*CoursesAdd.aspx*頁面上，您將新增更新的教學課程中。)</span><span class="sxs-lookup"><span data-stu-id="a3d89-164">You can add courses to the database yourself using the **Server Explorer** window or the *CoursesAdd.aspx* page, which you'll add in a later tutorial.)</span></span>

<span data-ttu-id="a3d89-165">[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="a3d89-165">[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)</span></span>

<span data-ttu-id="a3d89-166">`CoursesGridView`控制項會顯示只有幾個課程欄位。</span><span class="sxs-lookup"><span data-stu-id="a3d89-166">The `CoursesGridView` control shows only a few course fields.</span></span> <span data-ttu-id="a3d89-167">若要顯示的課程的所有詳細資料，您將使用`DetailsView`課程使用者選取的控制項。</span><span class="sxs-lookup"><span data-stu-id="a3d89-167">To display all the details for a course, you'll use a `DetailsView` control for the course that the user selects.</span></span> <span data-ttu-id="a3d89-168">在*Instructors.aspx*，結尾後面加入下列標記`</div>`標記 (請確定您將這個標記**之後**右 div 標記中，非之前):</span><span class="sxs-lookup"><span data-stu-id="a3d89-168">In *Instructors.aspx*, add the following markup after the closing `</div>` tag (make sure you place this markup **after** the closing div tag, not before it):</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

<span data-ttu-id="a3d89-169">這個標記會建立`EntityDataSource`繫結至控制項`Courses`實體集。</span><span class="sxs-lookup"><span data-stu-id="a3d89-169">This markup creates an `EntityDataSource` control that's bound to the `Courses` entity set.</span></span> <span data-ttu-id="a3d89-170">`Where`屬性選取課程使用`CourseID`課程中所選取的資料列的值`GridView`控制項。</span><span class="sxs-lookup"><span data-stu-id="a3d89-170">The `Where` property selects a course using the `CourseID` value of the selected row in the courses `GridView` control.</span></span> <span data-ttu-id="a3d89-171">標記指定的處理常式`Selected`事件，會顯示學生成績稍後使用，這是另一個階層中較低的層級。</span><span class="sxs-lookup"><span data-stu-id="a3d89-171">The markup specifies a handler for the `Selected` event, which you'll use later for displaying student grades, which is another level lower in the hierarchy.</span></span>

<span data-ttu-id="a3d89-172">在*Instructors.aspx.cs*，建立下列虛設常式`CourseDetailsEntityDataSource_Selected`方法。</span><span class="sxs-lookup"><span data-stu-id="a3d89-172">In *Instructors.aspx.cs*, create the following stub for the `CourseDetailsEntityDataSource_Selected` method.</span></span> <span data-ttu-id="a3d89-173">（您可以填寫此虛設常式，稍後在本教學課程中，現在，您需要它，頁面將會編譯及執行）。</span><span class="sxs-lookup"><span data-stu-id="a3d89-173">(You'll fill this stub out later in the tutorial; for now, you need it so that the page will compile and run.)</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

<span data-ttu-id="a3d89-174">執行網頁。</span><span class="sxs-lookup"><span data-stu-id="a3d89-174">Run the page.</span></span>

<span data-ttu-id="a3d89-175">[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="a3d89-175">[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)</span></span>

<span data-ttu-id="a3d89-176">一開始會沒有課程詳細資料，因為沒有課程已選取。</span><span class="sxs-lookup"><span data-stu-id="a3d89-176">Initially there are no course details because no course is selected.</span></span> <span data-ttu-id="a3d89-177">選取具有指派，課程講師，然後選取 課程來查看詳細資料。</span><span class="sxs-lookup"><span data-stu-id="a3d89-177">Select an instructor who has a course assigned, and then select a course to see the details.</span></span>

<span data-ttu-id="a3d89-178">[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="a3d89-178">[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)</span></span>

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a><span data-ttu-id="a3d89-179">使用 EntityDataSource 「 選取 」 事件，以顯示相關的資料</span><span class="sxs-lookup"><span data-stu-id="a3d89-179">Using the EntityDataSource "Selected" Event to Display Related Data</span></span>

<span data-ttu-id="a3d89-180">最後，您會想要顯示所有已註冊的學生版和其等級所選的課程。</span><span class="sxs-lookup"><span data-stu-id="a3d89-180">Finally, you want to show all of the enrolled students and their grades for the selected course.</span></span> <span data-ttu-id="a3d89-181">若要這樣做，您將使用`Selected`事件`EntityDataSource`控制項繫結至課程`DetailsView`。</span><span class="sxs-lookup"><span data-stu-id="a3d89-181">To do this, you'll use the `Selected` event of the `EntityDataSource` control bound to the course `DetailsView`.</span></span>

<span data-ttu-id="a3d89-182">在*Instructors.aspx*，加入下列標記之後`DetailsView`控制項：</span><span class="sxs-lookup"><span data-stu-id="a3d89-182">In *Instructors.aspx*, add the following markup after the `DetailsView` control:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

<span data-ttu-id="a3d89-183">這個標記會建立`ListView`顯示一份其等級選取課程的學生的控制項。</span><span class="sxs-lookup"><span data-stu-id="a3d89-183">This markup creates a `ListView` control that displays a list of students and their grades for the selected course.</span></span> <span data-ttu-id="a3d89-184">沒有指定資料來源是因為您將資料繫結程式碼中的控制項。</span><span class="sxs-lookup"><span data-stu-id="a3d89-184">No data source is specified because you'll databind the control in code.</span></span> <span data-ttu-id="a3d89-185">`EmptyDataTemplate`項目會提供任何課程已不選取時顯示訊息，在此情況下，沒有任何要顯示的學生。</span><span class="sxs-lookup"><span data-stu-id="a3d89-185">The `EmptyDataTemplate` element provides a message to display when no course is selected—in that case, there are no students to display.</span></span> <span data-ttu-id="a3d89-186">`LayoutTemplate`項目會建立 HTML 表格顯示清單中，而`ItemTemplate`指定要顯示的資料行。</span><span class="sxs-lookup"><span data-stu-id="a3d89-186">The `LayoutTemplate` element creates an HTML table to display the list, and the `ItemTemplate` specifies the columns to display.</span></span> <span data-ttu-id="a3d89-187">學生識別碼 」 和 「 學生成績都是來自`StudentGrade`實體和學生名稱是來自`Person`實體，Entity Framework 可讓在`Person`導覽屬性`StudentGrade`實體。</span><span class="sxs-lookup"><span data-stu-id="a3d89-187">The student ID and the student grade are from the `StudentGrade` entity, and the student name is from the `Person` entity that the Entity Framework makes available in the `Person` navigation property of the `StudentGrade` entity.</span></span>

<span data-ttu-id="a3d89-188">在*Instructors.aspx.cs*，取代附加虛設常式超出`CourseDetailsEntityDataSource_Selected`方法取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="a3d89-188">In *Instructors.aspx.cs*, replace the stubbed-out `CourseDetailsEntityDataSource_Selected` method with the following code:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

<span data-ttu-id="a3d89-189">此事件的事件引數提供的集合，其中將包含零個項目，如果選取任何項目或一個項目表單中選取的資料如果`Course`選取實體。</span><span class="sxs-lookup"><span data-stu-id="a3d89-189">The event argument for this event provides the selected data in the form of a collection, which will have zero items if nothing is selected or one item if a `Course` entity is selected.</span></span> <span data-ttu-id="a3d89-190">如果`Course`選取實體時，程式碼會使用`First`方法，將集合轉換為單一物件。</span><span class="sxs-lookup"><span data-stu-id="a3d89-190">If a `Course` entity is selected, the code uses the `First` method to convert the collection to a single object.</span></span> <span data-ttu-id="a3d89-191">接著會取得`StudentGrade`實體的導覽屬性，將它們轉換成一個集合，並繫結`GradesListView`控制項至集合。</span><span class="sxs-lookup"><span data-stu-id="a3d89-191">It then gets `StudentGrade` entities from the navigation property, converts them to a collection, and binds the `GradesListView` control to the collection.</span></span>

<span data-ttu-id="a3d89-192">這樣已足以顯示，但是您想要確定空的資料範本中的訊息會顯示第一次顯示網頁時，每當課程未選取。</span><span class="sxs-lookup"><span data-stu-id="a3d89-192">This is sufficient to display grades, but you want to make sure that the message in the empty data template is displayed the first time the page is displayed and whenever a course is not selected.</span></span> <span data-ttu-id="a3d89-193">若要這樣做，請建立下列方法，您會從兩個地方呼叫：</span><span class="sxs-lookup"><span data-stu-id="a3d89-193">To do that, create the following method, which you'll call from two places:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

<span data-ttu-id="a3d89-194">呼叫此新方法，從`Page_Load`方法，以顯示頁面會顯示空的資料範本的第一個時間。</span><span class="sxs-lookup"><span data-stu-id="a3d89-194">Call this new method from the `Page_Load` method to display the empty data template the first time the page is displayed.</span></span> <span data-ttu-id="a3d89-195">並從呼叫`InstructorsGridView_SelectedIndexChanged`方法講師選取時，會引發該事件，因為這表示新的課程會載入課程`GridView`尚未選取控制項和 none。</span><span class="sxs-lookup"><span data-stu-id="a3d89-195">And call it from the `InstructorsGridView_SelectedIndexChanged` method because that event is raised when an instructor is selected, which means new courses are loaded into the courses `GridView` control and none is selected yet.</span></span> <span data-ttu-id="a3d89-196">以下是兩個呼叫：</span><span class="sxs-lookup"><span data-stu-id="a3d89-196">Here are the two calls:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

<span data-ttu-id="a3d89-197">執行網頁。</span><span class="sxs-lookup"><span data-stu-id="a3d89-197">Run the page.</span></span>

<span data-ttu-id="a3d89-198">[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="a3d89-198">[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)</span></span>

<span data-ttu-id="a3d89-199">選取已指派，課程講師，然後選取 課程。</span><span class="sxs-lookup"><span data-stu-id="a3d89-199">Select an instructor that has a course assigned, and then select the course.</span></span>

<span data-ttu-id="a3d89-200">[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="a3d89-200">[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)</span></span>

<span data-ttu-id="a3d89-201">您現在有幾個方法來處理相關的資料。</span><span class="sxs-lookup"><span data-stu-id="a3d89-201">You have now seen a few ways to work with related data.</span></span> <span data-ttu-id="a3d89-202">在下列的教學課程中，您將學習如何加入現有的實體之間的關聯性如何移除關聯性，以及如何加入新的實體有現有的實體關聯性。</span><span class="sxs-lookup"><span data-stu-id="a3d89-202">In the following tutorial, you'll learn how to add relationships between existing entities, how to remove relationships, and how to add a new entity that has a relationship to an existing entity.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a3d89-203">[上一頁](the-entity-framework-and-aspnet-getting-started-part-3.md)
[下一頁](the-entity-framework-and-aspnet-getting-started-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="a3d89-203">[Previous](the-entity-framework-and-aspnet-getting-started-part-3.md)
[Next](the-entity-framework-and-aspnet-getting-started-part-5.md)</span></span>
