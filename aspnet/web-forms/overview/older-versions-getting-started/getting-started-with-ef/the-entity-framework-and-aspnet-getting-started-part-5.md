---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: "開始使用 Entity Framework 4.0 資料庫中第一次與 ASP.NET 4 Web Form-第 5 部分 |Microsoft 文件"
author: tdykstra
description: "Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 的 ASP.NET Web Form 應用程式。 範例應用程式是..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 5efc5ff367d5da5df060eba0028399af898a69fa
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a><span data-ttu-id="50586-104">開始使用 Entity Framework 4.0 資料庫中第一次和 ASP.NET 4 Web Form-第 5 部分</span><span class="sxs-lookup"><span data-stu-id="50586-104">Getting Started with Entity Framework 4.0 Database First and ASP.NET 4 Web Forms - Part 5</span></span>
====================
<span data-ttu-id="50586-105">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="50586-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="50586-106">Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Web Form 應用程式使用 Entity Framework 4.0 和 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="50586-106">The Contoso University sample web application demonstrates how to create ASP.NET Web Forms applications using the Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="50586-107">教學課程系列的相關資訊，請參閱[系列的第一個教學課程](the-entity-framework-and-aspnet-getting-started-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="50586-107">For information about the tutorial series, see [the first tutorial in the series](the-entity-framework-and-aspnet-getting-started-part-1.md)</span></span>


## <a name="working-with-related-data-continued"></a><span data-ttu-id="50586-108">使用相關資料，繼續</span><span class="sxs-lookup"><span data-stu-id="50586-108">Working with Related Data, Continued</span></span>

<span data-ttu-id="50586-109">在上一個教學課程中您開始使用`EntityDataSource`控制項使用相關資料。</span><span class="sxs-lookup"><span data-stu-id="50586-109">In the previous tutorial you began to use the `EntityDataSource` control to work with related data.</span></span> <span data-ttu-id="50586-110">顯示多個階層層級，並編輯導覽屬性中的資料。</span><span class="sxs-lookup"><span data-stu-id="50586-110">You displayed multiple levels of hierarchy and edited data in navigation properties.</span></span> <span data-ttu-id="50586-111">在本教學課程，您將會繼續使用相關的資料，加入和刪除關聯性，並可加入新的實體有現有的實體關聯性。</span><span class="sxs-lookup"><span data-stu-id="50586-111">In this tutorial you'll continue to work with related data by adding and deleting relationships and by adding a new entity that has a relationship to an existing entity.</span></span>

<span data-ttu-id="50586-112">您將建立一個頁面，將會指派給部門的課程。</span><span class="sxs-lookup"><span data-stu-id="50586-112">You'll create a page that adds courses that are assigned to departments.</span></span> <span data-ttu-id="50586-113">部門尚未存在，且當您建立新的課程，同時會建立它與現有的部門之間的關聯性。</span><span class="sxs-lookup"><span data-stu-id="50586-113">The departments already exist, and when you create a new course, at the same time you'll establish a relationship between it and an existing department.</span></span>

<span data-ttu-id="50586-114">[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="50586-114">[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)</span></span>

<span data-ttu-id="50586-115">您也會建立搭配講師交給課程 （新增您所選取的兩個實體之間的關聯性） 的多對多關聯性的頁面或從課程移除講師 (移除兩個實體之間的關聯性，您選取）。</span><span class="sxs-lookup"><span data-stu-id="50586-115">You'll also create a page that works with a many-to-many relationship by assigning an instructor to a course (adding a relationship between two entities that you select) or removing an instructor from a course (removing a relationship between two entities that you select).</span></span> <span data-ttu-id="50586-116">在資料庫中，加入一位講師和課程之間的關聯性會導致新的資料列加入至`CourseInstructor`關聯資料表，移除關聯性包含刪除資料列從`CourseInstructor`關聯資料表。</span><span class="sxs-lookup"><span data-stu-id="50586-116">In the database, adding a relationship between an instructor and a course results in a new row being added to the `CourseInstructor` association table; removing a relationship involves deleting a row from the `CourseInstructor` association table.</span></span> <span data-ttu-id="50586-117">不過，您可以 Entity Framework 中設定導覽屬性，而不會參考`CourseInstructor`明確資料表。</span><span class="sxs-lookup"><span data-stu-id="50586-117">However, you do this in the Entity Framework by setting navigation properties, without referring to the `CourseInstructor` table explicitly.</span></span>

<span data-ttu-id="50586-118">[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="50586-118">[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)</span></span>

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a><span data-ttu-id="50586-119">加入至現有的實體的實體與關聯性</span><span class="sxs-lookup"><span data-stu-id="50586-119">Adding an Entity with a Relationship to an Existing Entity</span></span>

<span data-ttu-id="50586-120">建立名為的新網頁*CoursesAdd.aspx*使用*Site.Master*主版頁面，並加入下列標記以`Content`控制項，名為`Content2`:</span><span class="sxs-lookup"><span data-stu-id="50586-120">Create a new web page named *CoursesAdd.aspx* that uses the *Site.Master* master page, and add the following markup to the `Content` control named `Content2`:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

<span data-ttu-id="50586-121">這個標記會建立`EntityDataSource`控制項選取課程，可插入，並指定的處理常式`Inserting`事件。</span><span class="sxs-lookup"><span data-stu-id="50586-121">This markup creates an `EntityDataSource` control that selects courses, that enables inserting, and that specifies a handler for the `Inserting` event.</span></span> <span data-ttu-id="50586-122">您將使用此處理常式來更新`Department`導覽屬性時新`Course`建立實體。</span><span class="sxs-lookup"><span data-stu-id="50586-122">You'll use the handler to update the `Department` navigation property when a new `Course` entity is created.</span></span>

<span data-ttu-id="50586-123">標記也會建立`DetailsView`用於加入新控制項`Course`實體。</span><span class="sxs-lookup"><span data-stu-id="50586-123">The markup also creates a `DetailsView` control to use for adding new `Course` entities.</span></span> <span data-ttu-id="50586-124">使用的繫結的欄位，標記`Course`實體屬性。</span><span class="sxs-lookup"><span data-stu-id="50586-124">The markup uses bound fields for `Course` entity properties.</span></span> <span data-ttu-id="50586-125">您必須輸入`CourseID`值，因為這不是系統產生的識別碼欄位。</span><span class="sxs-lookup"><span data-stu-id="50586-125">You have to enter the `CourseID` value because this is not a system-generated ID field.</span></span> <span data-ttu-id="50586-126">相反地，它是時，必須指定手動課程建立的課程數字。</span><span class="sxs-lookup"><span data-stu-id="50586-126">Instead, it's a course number that must be specified manually when the course is created.</span></span>

<span data-ttu-id="50586-127">您使用的範本欄位`Department`導覽屬性因為導覽屬性不能與`BoundField`控制項。</span><span class="sxs-lookup"><span data-stu-id="50586-127">You use a template field for the `Department` navigation property because navigation properties cannot be used with `BoundField` controls.</span></span> <span data-ttu-id="50586-128">[範本] 欄位提供下拉式清單選取部門。</span><span class="sxs-lookup"><span data-stu-id="50586-128">The template field provides a drop-down list to select the department.</span></span> <span data-ttu-id="50586-129">下拉式清單繫結至`Departments`實體集使用`Eval`而不是`Bind`、 再次因為您無法直接將繫結導覽屬性才能更新它們。</span><span class="sxs-lookup"><span data-stu-id="50586-129">The drop-down list is bound to the `Departments` entity set by using `Eval` rather than `Bind`, again because you cannot directly bind navigation properties in order to update them.</span></span> <span data-ttu-id="50586-130">指定的處理常式`DropDownList`控制項的`Init`事件，讓您可以儲存更新的程式碼使用的控制項的參考`DepartmentID`外部索引鍵。</span><span class="sxs-lookup"><span data-stu-id="50586-130">You specify a handler for the `DropDownList` control's `Init` event so that you can store a reference to the control for use by the code that updates the `DepartmentID` foreign key.</span></span>

<span data-ttu-id="50586-131">在*CoursesAdd.aspx.cs*只在部分類別宣告之後，加入 類別欄位，以保存至`DepartmentsDropDownList`控制項：</span><span class="sxs-lookup"><span data-stu-id="50586-131">In *CoursesAdd.aspx.cs* just after the partial-class declaration, add a class field to hold a reference to the `DepartmentsDropDownList` control:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

<span data-ttu-id="50586-132">加入的處理常式`DepartmentsDropDownList`控制項的`Init`事件，讓您可以儲存至控制項的參考。</span><span class="sxs-lookup"><span data-stu-id="50586-132">Add a handler for the `DepartmentsDropDownList` control's `Init` event so that you can store a reference to the control.</span></span> <span data-ttu-id="50586-133">這可讓您取得使用者所輸入的值，並用來更新`DepartmentID`值`Course`實體。</span><span class="sxs-lookup"><span data-stu-id="50586-133">This lets you get the value the user has entered and use it to update the `DepartmentID` value of the `Course` entity.</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

<span data-ttu-id="50586-134">加入的處理常式`DetailsView`控制項的`Inserting`事件：</span><span class="sxs-lookup"><span data-stu-id="50586-134">Add a handler for the `DetailsView` control's `Inserting` event:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

<span data-ttu-id="50586-135">當使用者按一下`Insert`、`Inserting`插入新的記錄之前，就會引發事件。</span><span class="sxs-lookup"><span data-stu-id="50586-135">When the user clicks `Insert`, the `Inserting` event is raised before the new record is inserted.</span></span> <span data-ttu-id="50586-136">此處理常式中的程式碼取得`DepartmentID`從`DropDownList`控制，並使用它來設定值，將會用於`DepartmentID`屬性`Course`實體。</span><span class="sxs-lookup"><span data-stu-id="50586-136">The code in the handler gets the `DepartmentID` from the `DropDownList` control and uses it to set the value that will be used for the `DepartmentID` property of the `Course` entity.</span></span>

<span data-ttu-id="50586-137">Entity Framework 會負責將新增到本課程的`Courses`相關聯的導覽屬性`Department`實體。</span><span class="sxs-lookup"><span data-stu-id="50586-137">The Entity Framework will take care of adding this course to the `Courses` navigation property of the associated `Department` entity.</span></span> <span data-ttu-id="50586-138">它也會加入至部門`Department`導覽屬性`Course`實體。</span><span class="sxs-lookup"><span data-stu-id="50586-138">It also adds the department to the `Department` navigation property of the `Course` entity.</span></span>

<span data-ttu-id="50586-139">執行網頁。</span><span class="sxs-lookup"><span data-stu-id="50586-139">Run the page.</span></span>

<span data-ttu-id="50586-140">[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="50586-140">[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)</span></span>

<span data-ttu-id="50586-141">輸入的識別碼、 標題、 數目信用額度，然後選取部門，然後按一下**插入**。</span><span class="sxs-lookup"><span data-stu-id="50586-141">Enter an ID, a title, a number of credits, and select a department, then click **Insert**.</span></span>

<span data-ttu-id="50586-142">執行*Courses.aspx*頁面上，並選取相同的部門，以查看新的課程。</span><span class="sxs-lookup"><span data-stu-id="50586-142">Run the *Courses.aspx* page, and select the same department to see the new course.</span></span>

<span data-ttu-id="50586-143">[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="50586-143">[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)</span></span>

## <a name="working-with-many-to-many-relationships"></a><span data-ttu-id="50586-144">使用 多對多關聯性</span><span class="sxs-lookup"><span data-stu-id="50586-144">Working with Many-to-Many Relationships</span></span>

<span data-ttu-id="50586-145">之間的關聯性`Courses`實體集和`People`實體集是多對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="50586-145">The relationship between the `Courses` entity set and the `People` entity set is a many-to-many relationship.</span></span> <span data-ttu-id="50586-146">A`Course`實體具有名為的導覽屬性`People`可以包含零個、 一個或多個相關`Person`（代表指派給教導該課程講師） 的實體。</span><span class="sxs-lookup"><span data-stu-id="50586-146">A `Course` entity has a navigation property named `People` that can contain zero, one, or more related `Person` entities (representing instructors assigned to teach that course).</span></span> <span data-ttu-id="50586-147">和`Person`實體具有名為的導覽屬性`Courses`可以包含零個、 一個或多個相關`Course`實體 (代表課程教導指派該講師)。</span><span class="sxs-lookup"><span data-stu-id="50586-147">And a `Person` entity has a navigation property named `Courses` that can contain zero, one, or more related `Course` entities (representing courses that instructor is assigned to teach).</span></span> <span data-ttu-id="50586-148">一個講師可能教導多個課程中，並可能由多個講師教一個課程。</span><span class="sxs-lookup"><span data-stu-id="50586-148">One instructor might teach multiple courses, and one course might be taught by multiple instructors.</span></span> <span data-ttu-id="50586-149">本節的逐步解說中，您會加入及移除之間的關聯性`Person`和`Course`藉由更新相關實體的導覽屬性的實體。</span><span class="sxs-lookup"><span data-stu-id="50586-149">In this section of the walkthrough, you'll add and remove relationships between `Person` and `Course` entities by updating the navigation properties of the related entities.</span></span>

<span data-ttu-id="50586-150">建立名為的新網頁*InstructorsCourses.aspx*使用*Site.Master*主版頁面，並加入下列標記以`Content`控制項，名為`Content2`:</span><span class="sxs-lookup"><span data-stu-id="50586-150">Create a new web page named *InstructorsCourses.aspx* that uses the *Site.Master* master page, and add the following markup to the `Content` control named `Content2`:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

<span data-ttu-id="50586-151">這個標記會建立`EntityDataSource`擷取名稱的控制項和`PersonID`的`Person`講師的實體。</span><span class="sxs-lookup"><span data-stu-id="50586-151">This markup creates an `EntityDataSource` control that retrieves the name and `PersonID` of `Person` entities for instructors.</span></span> <span data-ttu-id="50586-152">A`DropDrownList`控制項繫結至`EntityDataSource`控制項。</span><span class="sxs-lookup"><span data-stu-id="50586-152">A `DropDrownList` control is bound to the `EntityDataSource` control.</span></span> <span data-ttu-id="50586-153">`DropDownList`控制項指定的處理常式`DataBound`事件。</span><span class="sxs-lookup"><span data-stu-id="50586-153">The `DropDownList` control specifies a handler for the `DataBound` event.</span></span> <span data-ttu-id="50586-154">您將使用此處理常式，以進行資料繫結這兩個下拉式清單中顯示的課程。</span><span class="sxs-lookup"><span data-stu-id="50586-154">You'll use this handler to databind the two drop-down lists that display courses.</span></span>

<span data-ttu-id="50586-155">標記也會建立下列群組的指派給所選講師課程使用的控制項：</span><span class="sxs-lookup"><span data-stu-id="50586-155">The markup also creates the following group of controls to use for assigning a course to the selected instructor:</span></span>

- <span data-ttu-id="50586-156">A`DropDownList`控制項用於選取要指派的課程。</span><span class="sxs-lookup"><span data-stu-id="50586-156">A `DropDownList` control for selecting a course to assign.</span></span> <span data-ttu-id="50586-157">此控制項將會填入目前未指派給所選講師的課程。</span><span class="sxs-lookup"><span data-stu-id="50586-157">This control will be populated with courses that are currently not assigned to the selected instructor.</span></span>
- <span data-ttu-id="50586-158">A`Button`起始作業的控制項。</span><span class="sxs-lookup"><span data-stu-id="50586-158">A `Button` control to initiate the assignment.</span></span>
- <span data-ttu-id="50586-159">A`Label`控制項來顯示錯誤訊息，如果指派會失敗。</span><span class="sxs-lookup"><span data-stu-id="50586-159">A `Label` control to display an error message if the assignment fails.</span></span>

<span data-ttu-id="50586-160">最後，標記也會建立一組用於課程移除所選講師控制項。</span><span class="sxs-lookup"><span data-stu-id="50586-160">Finally, the markup also creates a group of controls to use for removing a course from the selected instructor.</span></span>

<span data-ttu-id="50586-161">在*InstructorsCourses.aspx.cs*，加入 using 陳述式：</span><span class="sxs-lookup"><span data-stu-id="50586-161">In *InstructorsCourses.aspx.cs*, add a using statement:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

<span data-ttu-id="50586-162">加入填入顯示課程的兩個下拉式清單的方法：</span><span class="sxs-lookup"><span data-stu-id="50586-162">Add a method for populating the two drop-down lists that display courses:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

<span data-ttu-id="50586-163">這個程式碼取得從所有課程`Courses`實體設定和都取得從課程`Courses`導覽屬性`Person`選取講師的實體。</span><span class="sxs-lookup"><span data-stu-id="50586-163">This code gets all courses from the `Courses` entity set and gets the courses from the `Courses` navigation property of the `Person` entity for the selected instructor.</span></span> <span data-ttu-id="50586-164">它會判斷哪些課程指派給該講師並據以填入下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="50586-164">It then determines which courses are assigned to that instructor and populates the drop-down lists accordingly.</span></span>

<span data-ttu-id="50586-165">加入的處理常式`Assign`按鈕的`Click`事件：</span><span class="sxs-lookup"><span data-stu-id="50586-165">Add a handler for the `Assign` button's `Click` event:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

<span data-ttu-id="50586-166">這個程式碼取得`Person`選取講師，實體取得`Course`實體所選的課程中，並加入至所選的課程`Courses`講師的導覽屬性`Person`實體。</span><span class="sxs-lookup"><span data-stu-id="50586-166">This code gets the `Person` entity for the selected instructor, gets the `Course` entity for the selected course, and adds the selected course to the `Courses` navigation property of the instructor's `Person` entity.</span></span> <span data-ttu-id="50586-167">然後，它會將變更儲存到資料庫並重新填入下拉式清單，因此可以立即看到結果。</span><span class="sxs-lookup"><span data-stu-id="50586-167">It then saves the changes to the database and repopulates the drop-down lists so the results can be seen immediately.</span></span>

<span data-ttu-id="50586-168">加入的處理常式`Remove`按鈕的`Click`事件：</span><span class="sxs-lookup"><span data-stu-id="50586-168">Add a handler for the `Remove` button's `Click` event:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

<span data-ttu-id="50586-169">這個程式碼取得`Person`選取講師，實體取得`Course`實體所選的課程中，並移除從所選的課程`Person`實體的`Courses`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="50586-169">This code gets the `Person` entity for the selected instructor, gets the `Course` entity for the selected course, and removes the selected course from the `Person` entity's `Courses` navigation property.</span></span> <span data-ttu-id="50586-170">然後，它會將變更儲存到資料庫並重新填入下拉式清單，因此可以立即看到結果。</span><span class="sxs-lookup"><span data-stu-id="50586-170">It then saves the changes to the database and repopulates the drop-down lists so the results can be seen immediately.</span></span>

<span data-ttu-id="50586-171">將程式碼加入`Page_Load`方法，以確保錯誤訊息不會顯示任何錯誤報告，並加入處理常式時`DataBound`和`SelectedIndexChanged`講師下拉式清單來擴展課程下拉式清單的事件：</span><span class="sxs-lookup"><span data-stu-id="50586-171">Add code to the `Page_Load` method that makes sure the error messages are not visible when there's no error to report, and add handlers for the `DataBound` and `SelectedIndexChanged` events of the instructors drop-down list to populate the courses drop-down lists:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

<span data-ttu-id="50586-172">執行網頁。</span><span class="sxs-lookup"><span data-stu-id="50586-172">Run the page.</span></span>

<span data-ttu-id="50586-173">[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="50586-173">[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)</span></span>

<span data-ttu-id="50586-174">選取 [instructor]。</span><span class="sxs-lookup"><span data-stu-id="50586-174">Select an instructor.</span></span> <span data-ttu-id="50586-175">**指派課程**下拉式清單會顯示不會教導講師，課程和**移除課程**下拉式清單會顯示已指派給講師的課程。</span><span class="sxs-lookup"><span data-stu-id="50586-175">The **Assign a Course** drop-down list displays the courses that the instructor doesn't teach, and the **Remove a Course** drop-down list displays the courses that the instructor is already assigned to.</span></span> <span data-ttu-id="50586-176">在**指派課程**區段中，選取課程，然後按一下**指派**。</span><span class="sxs-lookup"><span data-stu-id="50586-176">In the **Assign a Course** section, select a course and then click **Assign**.</span></span> <span data-ttu-id="50586-177">本課程會移至**移除課程**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="50586-177">The course moves to the **Remove a Course** drop-down list.</span></span> <span data-ttu-id="50586-178">選取在課程**移除課程**區段，然後按一下 **移除 * * *。*</span><span class="sxs-lookup"><span data-stu-id="50586-178">Select a course in the **Remove a Course** section and click **Remove***.*</span></span> <span data-ttu-id="50586-179">本課程會移至**指派課程**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="50586-179">The course moves to the **Assign a Course** drop-down list.</span></span>

<span data-ttu-id="50586-180">您現在有一些更多的方法來處理相關的資料。</span><span class="sxs-lookup"><span data-stu-id="50586-180">You have now seen some more ways to work with related data.</span></span> <span data-ttu-id="50586-181">在下列的教學課程中，您將學習如何使用資料模型中的繼承，以改善您的應用程式的可維護性。</span><span class="sxs-lookup"><span data-stu-id="50586-181">In the following tutorial, you'll learn how to use inheritance in the data model to improve the maintainability of your application.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="50586-182">[上一頁](the-entity-framework-and-aspnet-getting-started-part-4.md)
[下一頁](the-entity-framework-and-aspnet-getting-started-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="50586-182">[Previous](the-entity-framework-and-aspnet-getting-started-part-4.md)
[Next](the-entity-framework-and-aspnet-getting-started-part-6.md)</span></span>
