---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 使用 Entity Framework 4.0 和 ObjectDataSource 控制項，第 3 部分： 排序和篩選 |Microsoft 文件
author: tdykstra
description: 此教學課程系列為基礎所建立的開始使用 Entity Framework 4.0 教學課程系列的 Contoso 大學 web 應用程式。 I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: e412d3ad98a37931e7190a4909cb09fa2abfb3d0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887651"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a><span data-ttu-id="b6a3a-104">使用 Entity Framework 4.0 和 ObjectDataSource 控制項，第 3 部分： 排序和篩選</span><span class="sxs-lookup"><span data-stu-id="b6a3a-104">Using the Entity Framework 4.0 and the ObjectDataSource Control, Part 3: Sorting and Filtering</span></span>
====================
<span data-ttu-id="b6a3a-105">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="b6a3a-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="b6a3a-106">此教學課程系列為基礎所建立的 Contoso 大學 web 應用程式[開始使用 Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started)教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-106">This tutorial series builds on the Contoso University web application that is created by the [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) tutorial series.</span></span> <span data-ttu-id="b6a3a-107">如果您未完成先前的教學課程，您可以身分本教學課程的起始點來[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)您會建立。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-107">If you didn't complete the earlier tutorials, as a starting point for this tutorial you can [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) that you would have created.</span></span> <span data-ttu-id="b6a3a-108">您也可以[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)完整的教學課程所建立。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-108">You can also [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) that is created by the complete tutorial series.</span></span> <span data-ttu-id="b6a3a-109">如果您有疑問的教學課程，您可以張貼他們[ASP.NET Entity Framework 論壇](https://forums.asp.net/1227.aspx)。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-109">If you have questions about the tutorials, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx).</span></span>


<span data-ttu-id="b6a3a-110">在上一個教學課程中您會實作儲存機制中的模式使用 Entity Framework 的多層式架構 web 應用程式和`ObjectDataSource`控制項。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-110">In the previous tutorial you implemented the repository pattern in an n-tier web application that uses the Entity Framework and the `ObjectDataSource` control.</span></span> <span data-ttu-id="b6a3a-111">本教學課程會示範如何執行排序和篩選，以及處理主從式案例。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-111">This tutorial shows how to do sorting and filtering and handle master-detail scenarios.</span></span> <span data-ttu-id="b6a3a-112">您會加入下列的增強功能*Departments.aspx*頁面：</span><span class="sxs-lookup"><span data-stu-id="b6a3a-112">You'll add the following enhancements to the *Departments.aspx* page:</span></span>

- <span data-ttu-id="b6a3a-113">若要允許使用者依名稱選取部門的文字方塊。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-113">A text box to allow users to select departments by name.</span></span>
- <span data-ttu-id="b6a3a-114">顯示在方格中每個部門的課程的清單。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-114">A list of courses for each department that's shown in the grid.</span></span>
- <span data-ttu-id="b6a3a-115">按一下資料行標題來排序功能。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-115">The ability to sort by clicking column headings.</span></span>

<span data-ttu-id="b6a3a-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b6a3a-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span></span>

## <a name="adding-the-ability-to-sort-gridview-columns"></a><span data-ttu-id="b6a3a-117">將功能加入至排序 GridView 的資料行</span><span class="sxs-lookup"><span data-stu-id="b6a3a-117">Adding the Ability to Sort GridView Columns</span></span>

<span data-ttu-id="b6a3a-118">開啟*Departments.aspx*頁面上，並加入`SortParameterName="sortExpression"`屬性`ObjectDataSource`控制項，名為`DepartmentsObjectDataSource`。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-118">Open the *Departments.aspx* page and add a `SortParameterName="sortExpression"` attribute to the `ObjectDataSource` control named `DepartmentsObjectDataSource`.</span></span> <span data-ttu-id="b6a3a-119">(您將在稍後建立`GetDepartments`方法參數的名稱為`sortExpression`。)控制項的開頭標記的標記現在會類似下列的範例。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-119">(Later you'll create a `GetDepartments` method that takes a parameter named `sortExpression`.) The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

<span data-ttu-id="b6a3a-120">新增`AllowSorting="true"`屬性的開頭標記`GridView`控制項。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-120">Add the `AllowSorting="true"` attribute to the opening tag of the `GridView` control.</span></span> <span data-ttu-id="b6a3a-121">控制項的開頭標記的標記現在會類似下列的範例。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-121">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

<span data-ttu-id="b6a3a-122">在*Departments.aspx.cs*，藉由呼叫設定預設的排序次序`GridView`控制項的`Sort`方法從`Page_Load`方法：</span><span class="sxs-lookup"><span data-stu-id="b6a3a-122">In *Departments.aspx.cs*, set the default sort order by calling the `GridView` control's `Sort` method from the `Page_Load` method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

<span data-ttu-id="b6a3a-123">您可以加入程式碼來排序或篩選在商務邏輯類別或儲存機制類別。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-123">You can add code that sorts or filters in either the business logic class or the repository class.</span></span> <span data-ttu-id="b6a3a-124">如果您執行商務邏輯類別中，排序或篩選的工作會執行從資料庫擷取資料之後因為商務邏輯類別使用的`IEnumerable`儲存機制所傳回的物件。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-124">If you do it in the business logic class, the sorting or filtering work will be done after the data is retrieved from the database, because the business logic class is working with an `IEnumerable` object returned by the repository.</span></span> <span data-ttu-id="b6a3a-125">如果您加入排序和篩選程式碼中的儲存機制類別和 LINQ 運算式之前執行物件查詢已經轉換成`IEnumerable`物件，您的命令將會傳遞到資料庫以進行處理，通常更有效率。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-125">If you add sorting and filtering code in the repository class and you do it before a LINQ expression or object query has been converted to an `IEnumerable` object, your commands will be passed through to the database for processing, which is typically more efficient.</span></span> <span data-ttu-id="b6a3a-126">在本教學課程，您將實作的排序和篩選會使要由資料庫執行的處理方式 — 也就是在儲存機制中。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-126">In this tutorial you'll implement sorting and filtering in a way that causes the processing to be done by the database — that is, in the repository.</span></span>

<span data-ttu-id="b6a3a-127">若要加入排序功能，您必須將新方法加入至儲存機制介面和儲存機制類別以及有關商務邏輯類別。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-127">To add sorting capability, you must add a new method to the repository interface and repository classes as well as to the business logic class.</span></span> <span data-ttu-id="b6a3a-128">在*ISchoolRepository.cs*檔案中，加入新`GetDepartments`採用方法`sortExpression`會用來排序，則會傳回清單中的部門的參數：</span><span class="sxs-lookup"><span data-stu-id="b6a3a-128">In the *ISchoolRepository.cs* file, add a new `GetDepartments` method that takes a `sortExpression` parameter that will be used to sort the list of departments that's returned:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

<span data-ttu-id="b6a3a-129">`sortExpression`參數會指定要排序的資料行和排序方向。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-129">The `sortExpression` parameter will specify the column to sort on and the sort direction.</span></span>

<span data-ttu-id="b6a3a-130">加入新的方法的程式碼*SchoolRepository.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="b6a3a-130">Add code for the new method to the *SchoolRepository.cs* file:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

<span data-ttu-id="b6a3a-131">變更現有無參數`GetDepartments`方法來呼叫新方法：</span><span class="sxs-lookup"><span data-stu-id="b6a3a-131">Change the existing parameterless `GetDepartments` method to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

<span data-ttu-id="b6a3a-132">在測試專案中，新增下列新方法加入*MockSchoolRepository.cs*:</span><span class="sxs-lookup"><span data-stu-id="b6a3a-132">In the test project, add the following new method to *MockSchoolRepository.cs*:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

<span data-ttu-id="b6a3a-133">如果您要建立任何單元測試，依存於這個方法傳回已排序的清單，您需要排序清單，再加以傳回。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-133">If you were going to create any unit tests that depended on this method returning a sorted list, you would need to sort the list before returning it.</span></span> <span data-ttu-id="b6a3a-134">您將不會建立測試像那樣在本教學課程，讓方法可以只傳回未排序的清單的部門。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-134">You won't be creating tests like that in this tutorial, so the method can just return the unsorted list of departments.</span></span>

<span data-ttu-id="b6a3a-135">在*SchoolBL.cs*檔案中，將下列新方法加入商務邏輯類別：</span><span class="sxs-lookup"><span data-stu-id="b6a3a-135">In the *SchoolBL.cs* file, add the following new method to the business logic class:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

<span data-ttu-id="b6a3a-136">此程式碼會將排序參數傳遞至儲存機制方法。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-136">This code passes the sort parameter to the repository method.</span></span>

<span data-ttu-id="b6a3a-137">執行*Departments.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-137">Run the *Departments.aspx* page.</span></span>

<span data-ttu-id="b6a3a-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="b6a3a-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span></span>

<span data-ttu-id="b6a3a-139">您現在可以按一下任何欄標題，排序依據該資料行。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-139">You can now click any column heading to sort by that column.</span></span> <span data-ttu-id="b6a3a-140">如果已排序的資料行，按一下標題會反轉排序方向。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-140">If the column is already sorted, clicking the heading reverses the sort direction.</span></span>

## <a name="adding-a-search-box"></a><span data-ttu-id="b6a3a-141">新增搜尋方塊</span><span class="sxs-lookup"><span data-stu-id="b6a3a-141">Adding a Search Box</span></span>

<span data-ttu-id="b6a3a-142">本節中，您會加入 [搜尋] 文字方塊中，將其連結到`ObjectDataSource`控制使用控制參數，並將方法加入商務邏輯類別，若要支援篩選。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-142">In this section you'll add a search text box, link it to the `ObjectDataSource` control using a control parameter, and add a method to the business logic class to support filtering.</span></span>

<span data-ttu-id="b6a3a-143">開啟*Departments.aspx*頁面上，加入下列標記之間的標題和第一個`ObjectDataSource`控制項：</span><span class="sxs-lookup"><span data-stu-id="b6a3a-143">Open the *Departments.aspx* page and add the following markup between the heading and the first `ObjectDataSource` control:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

<span data-ttu-id="b6a3a-144">在`ObjectDataSource`控制項，名為`DepartmentsObjectDataSource`，執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="b6a3a-144">In the `ObjectDataSource` control named `DepartmentsObjectDataSource`, do the following:</span></span>

- <span data-ttu-id="b6a3a-145">新增`SelectParameters`名為的參數項目`nameSearchString`來取得在輸入的值`SearchTextBox`控制項。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-145">Add a `SelectParameters` element for a parameter named `nameSearchString` that gets the value entered in the `SearchTextBox` control.</span></span>
- <span data-ttu-id="b6a3a-146">變更`SelectMethod`屬性值，以`GetDepartmentsByName`。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-146">Change the `SelectMethod` attribute value to `GetDepartmentsByName`.</span></span> <span data-ttu-id="b6a3a-147">（您會建立這個方法之後）。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-147">(You'll create this method later.)</span></span>

<span data-ttu-id="b6a3a-148">標記`ObjectDataSource`控制項現在會類似下列的範例：</span><span class="sxs-lookup"><span data-stu-id="b6a3a-148">The markup for the `ObjectDataSource` control now resembles the following example:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

<span data-ttu-id="b6a3a-149">在*ISchoolRepository.cs*，新增`GetDepartmentsByName`採用兩者方法`sortExpression`和`nameSearchString`參數：</span><span class="sxs-lookup"><span data-stu-id="b6a3a-149">In *ISchoolRepository.cs*, add a `GetDepartmentsByName` method that takes both `sortExpression` and `nameSearchString` parameters:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

<span data-ttu-id="b6a3a-150">在*SchoolRepository.cs*，加入下列新方法：</span><span class="sxs-lookup"><span data-stu-id="b6a3a-150">In *SchoolRepository.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

<span data-ttu-id="b6a3a-151">此程式碼使用`Where`方法，以選取包含搜尋字串的項目。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-151">This code uses a `Where` method to select items that contain the search string.</span></span> <span data-ttu-id="b6a3a-152">如果搜尋字串是空的則會選取所有記錄。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-152">If the search string is empty, all records will be selected.</span></span> <span data-ttu-id="b6a3a-153">請注意，當您指定方法呼叫一起在如下的單一陳述式 (`Include`，然後`OrderBy`，然後`Where`)、`Where`方法必須永遠是最後一個。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-153">Note that when you specify method calls together in one statement like this (`Include`, then `OrderBy`, then `Where`), the `Where` method must always be last.</span></span>

<span data-ttu-id="b6a3a-154">變更現有`GetDepartments`採用方法`sortExpression`參數來呼叫新方法：</span><span class="sxs-lookup"><span data-stu-id="b6a3a-154">Change the existing `GetDepartments` method that takes a `sortExpression` parameter to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

<span data-ttu-id="b6a3a-155">在*MockSchoolRepository.cs*在測試專案中，將下列新方法：</span><span class="sxs-lookup"><span data-stu-id="b6a3a-155">In *MockSchoolRepository.cs* in the test project, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

<span data-ttu-id="b6a3a-156">在*SchoolBL.cs*，加入下列新方法：</span><span class="sxs-lookup"><span data-stu-id="b6a3a-156">In *SchoolBL.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

<span data-ttu-id="b6a3a-157">執行*Departments.aspx*頁面上，並輸入搜尋字串，並確定選取邏輯運作。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-157">Run the *Departments.aspx* page and enter a search string to make sure that the selection logic works.</span></span> <span data-ttu-id="b6a3a-158">將文字方塊保留空白，然後再次嘗試搜尋，請確定傳回的所有記錄。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-158">Leave the text box empty and try a search to make sure that all records are returned.</span></span>

<span data-ttu-id="b6a3a-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="b6a3a-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span></span>

## <a name="adding-a-details-column-for-each-grid-row"></a><span data-ttu-id="b6a3a-160">每個格線資料列加入詳細資料的資料行</span><span class="sxs-lookup"><span data-stu-id="b6a3a-160">Adding a Details Column for Each Grid Row</span></span>

<span data-ttu-id="b6a3a-161">接下來，您會想要查看所有顯示在右邊的資料格方格的每個部門的課程。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-161">Next, you want to see all of the courses for each department displayed in the right-hand cell of the grid.</span></span> <span data-ttu-id="b6a3a-162">若要這樣做，您將使用巢狀`GridView`控制項和資料繫結它將資料從`Courses`導覽屬性`Department`實體。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-162">To do this, you'll use a nested `GridView` control and databind it to data from the `Courses` navigation property of the `Department` entity.</span></span>

<span data-ttu-id="b6a3a-163">開啟*Departments.aspx*和中的標記`GridView`控制，請指定處理常式的`RowDataBound`事件。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-163">Open *Departments.aspx* and in the markup for the `GridView` control, specify a handler for the `RowDataBound` event.</span></span> <span data-ttu-id="b6a3a-164">控制項的開頭標記的標記現在會類似下列的範例。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-164">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

<span data-ttu-id="b6a3a-165">加入新`TemplateField`之後的項目`Administrator`範本欄位：</span><span class="sxs-lookup"><span data-stu-id="b6a3a-165">Add a new `TemplateField` element after the `Administrator` template field:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

<span data-ttu-id="b6a3a-166">這個標記會建立巢狀`GridView`控制項，顯示的課程編號和標題的課程的清單。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-166">This markup creates a nested `GridView` control that shows the course number and title of a list of courses.</span></span> <span data-ttu-id="b6a3a-167">其並未指定資料來源，因為您將資料繫結中的程式碼在`RowDataBound`處理常式。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-167">It does not specify a data source because you'll databind it in code in the `RowDataBound` handler.</span></span>

<span data-ttu-id="b6a3a-168">開啟*Departments.aspx.cs*並加入下列處理常式`RowDataBound`事件：</span><span class="sxs-lookup"><span data-stu-id="b6a3a-168">Open *Departments.aspx.cs* and add the following handler for the `RowDataBound` event:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

<span data-ttu-id="b6a3a-169">這個程式碼取得`Department`事件引數，從實體轉換`Courses`導覽屬性來`List`集合，並將巢狀的`GridView`至集合。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-169">This code gets the `Department` entity from the event arguments, converts the `Courses` navigation property to a `List` collection, and databinds the nested `GridView` to the collection.</span></span>

<span data-ttu-id="b6a3a-170">開啟*SchoolRepository.cs*檔案，並指定的積極式載入`Courses`導覽屬性，藉由呼叫`Include`方法中建立的物件查詢中`GetDepartmentsByName`方法。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-170">Open the *SchoolRepository.cs* file and specify eager loading for the `Courses` navigation property by calling the `Include` method in the object query that you create in the `GetDepartmentsByName` method.</span></span> <span data-ttu-id="b6a3a-171">`return`陳述式中的`GetDepartmentsByName`方法現在會類似下列的範例。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-171">The `return` statement in the `GetDepartmentsByName` method now resembles the following example.</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

<span data-ttu-id="b6a3a-172">執行網頁。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-172">Run the page.</span></span> <span data-ttu-id="b6a3a-173">排序和篩選您先前加入的功能，除了 GridView 控制項現在會顯示每個部門的巢狀的課程詳細資料。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-173">In addition to the sorting and filtering capability that you added earlier, the GridView control now shows nested course details for each department.</span></span>

<span data-ttu-id="b6a3a-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="b6a3a-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span></span>

<span data-ttu-id="b6a3a-175">如此即完成排序、 篩選，和主版詳細資料案例簡介。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-175">This completes the introduction to sorting, filtering, and master-detail scenarios.</span></span> <span data-ttu-id="b6a3a-176">在下一個教學課程中，您會看到如何處理並行存取。</span><span class="sxs-lookup"><span data-stu-id="b6a3a-176">In the next tutorial, you'll see how to handle concurrency.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b6a3a-177">[上一頁](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [下一頁](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="b6a3a-177">[Previous](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span></span>
