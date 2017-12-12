---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: "排序、 分頁和篩選資料，包含模型繫結和 web form |Microsoft 文件"
author: tfitzmac
description: "此教學課程系列示範與 ASP.NET Web Form 專案中使用模型繫結的基本層面。 模型繫結進行資料互動詳細直線-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: 94fc84533be5fcbcf0612fcdcabea7dee738d89b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a><span data-ttu-id="47fd8-104">排序、 分頁和篩選資料，包含模型繫結和 web form</span><span class="sxs-lookup"><span data-stu-id="47fd8-104">Sorting, paging, and filtering data with model binding and web forms</span></span>
====================
<span data-ttu-id="47fd8-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="47fd8-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="47fd8-106">此教學課程系列示範與 ASP.NET Web Form 專案中使用模型繫結的基本層面。</span><span class="sxs-lookup"><span data-stu-id="47fd8-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="47fd8-107">模型繫結進行更直覺比處理的資料來源物件 （例如 ObjectDataSource 或 SqlDataSource） 的資料互動。</span><span class="sxs-lookup"><span data-stu-id="47fd8-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="47fd8-108">此數列開頭簡介資料，並會移到更進階的概念中之後的教學課程。</span><span class="sxs-lookup"><span data-stu-id="47fd8-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="47fd8-109">本教學課程會示範如何加入排序、 分頁和篩選資料的模型繫結。</span><span class="sxs-lookup"><span data-stu-id="47fd8-109">This tutorial shows how to add sorting, paging, and filtering of the data through model binding.</span></span>
> 
> <span data-ttu-id="47fd8-110">本教學課程是在第一個建立的專案[一部分](retrieving-data.md)的數列。</span><span class="sxs-lookup"><span data-stu-id="47fd8-110">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="47fd8-111">您可以[下載](https://go.microsoft.com/fwlink/?LinkId=286116)以 C# 或 VB 完整的專案</span><span class="sxs-lookup"><span data-stu-id="47fd8-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="47fd8-112">可下載的程式碼適用於 Visual Studio 2012 或 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="47fd8-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="47fd8-113">它會使用 Visual Studio 2012 範本，這可能會比 Visual Studio 2013 範本顯示在本教學課程稍有不同。</span><span class="sxs-lookup"><span data-stu-id="47fd8-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="47fd8-114">您將建置</span><span class="sxs-lookup"><span data-stu-id="47fd8-114">What you'll build</span></span>

<span data-ttu-id="47fd8-115">在此教學課程中，您將會：</span><span class="sxs-lookup"><span data-stu-id="47fd8-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="47fd8-116">啟用資料的排序和分頁</span><span class="sxs-lookup"><span data-stu-id="47fd8-116">Enable sorting and paging of the data</span></span>
2. <span data-ttu-id="47fd8-117">啟用選取為依據的使用者資料的篩選</span><span class="sxs-lookup"><span data-stu-id="47fd8-117">Enable filtering of the data based on a selection by the user</span></span>

## <a name="add-sorting"></a><span data-ttu-id="47fd8-118">加入排序</span><span class="sxs-lookup"><span data-stu-id="47fd8-118">Add sorting</span></span>

<span data-ttu-id="47fd8-119">在 GridView 中排序，是非常簡單。</span><span class="sxs-lookup"><span data-stu-id="47fd8-119">Enabling sorting in the GridView is very easy.</span></span> <span data-ttu-id="47fd8-120">在 Student.aspx 檔案中，只要設定**AllowSorting**至**true** GridView 中。</span><span class="sxs-lookup"><span data-stu-id="47fd8-120">In the Student.aspx file, simply set **AllowSorting** to **true** in the GridView.</span></span> <span data-ttu-id="47fd8-121">您不需要設定**SortExpression** DataField 會自動使用為每個資料行值。</span><span class="sxs-lookup"><span data-stu-id="47fd8-121">You do not need to set a **SortExpression** value for each column as the DataField is automatically used.</span></span> <span data-ttu-id="47fd8-122">在 GridView 修改查詢來包含所選取的值排序資料。</span><span class="sxs-lookup"><span data-stu-id="47fd8-122">The GridView modifies the query to include ordering the data by the selected value.</span></span> <span data-ttu-id="47fd8-123">反白顯示的下列程式碼顯示新增，您必須先啟用排序。</span><span class="sxs-lookup"><span data-stu-id="47fd8-123">The highlighted code below shows the addition you need to make to enable sorting.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

<span data-ttu-id="47fd8-124">執行 web 應用程式和測試排序學生記錄不同的資料行中的值。</span><span class="sxs-lookup"><span data-stu-id="47fd8-124">Run the web application, and test sorting student records by the values in different columns.</span></span>

![排序學生版](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a><span data-ttu-id="47fd8-126">加入分頁</span><span class="sxs-lookup"><span data-stu-id="47fd8-126">Add paging</span></span>

<span data-ttu-id="47fd8-127">啟用分頁功能也非常容易。</span><span class="sxs-lookup"><span data-stu-id="47fd8-127">Enabling paging is also very easy.</span></span> <span data-ttu-id="47fd8-128">在 GridView 中設定**AllowPaging**屬性**true**並設定**PageSize**屬性，以您想要在每個頁面上顯示的記錄數目。</span><span class="sxs-lookup"><span data-stu-id="47fd8-128">In the GridView, set the **AllowPaging** property to **true** and set the **PageSize** property to the number of records you wish to display on each page.</span></span> <span data-ttu-id="47fd8-129">在本教學課程中，您可以將它設定為 4。</span><span class="sxs-lookup"><span data-stu-id="47fd8-129">In this tutorial, you can set it to 4.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

<span data-ttu-id="47fd8-130">執行 web 應用程式，並注意現在記錄會分割多個頁面上，不能超過 4 顯示在單一頁面上的記錄。</span><span class="sxs-lookup"><span data-stu-id="47fd8-130">Run the web application, and notice that now the records are divided over multiple pages with no more than 4 records displayed on a single page.</span></span>

![加入分頁](sorting-paging-and-filtering-data/_static/image4.png)

<span data-ttu-id="47fd8-132">延後的查詢執行會改善應用程式效率。</span><span class="sxs-lookup"><span data-stu-id="47fd8-132">Deferred query execution improves the application efficiency.</span></span> <span data-ttu-id="47fd8-133">而不會擷取整個資料集，GridView 修改查詢以擷取目前頁面的記錄。</span><span class="sxs-lookup"><span data-stu-id="47fd8-133">Instead of retrieving the entire data set, the GridView modifies the query to retrieve only the records for the current page.</span></span>

## <a name="filter-records-by-user-selection"></a><span data-ttu-id="47fd8-134">依使用者選取項目來篩選記錄</span><span class="sxs-lookup"><span data-stu-id="47fd8-134">Filter records by user selection</span></span>

<span data-ttu-id="47fd8-135">模型繫結會新增數個屬性可讓您指定如何在模型繫結方法中設定參數的值。</span><span class="sxs-lookup"><span data-stu-id="47fd8-135">Model binding adds several attributes which enable you to designate how to set the value for a parameter in a model binding method.</span></span> <span data-ttu-id="47fd8-136">這些屬性都是**System.Web.ModelBinding**命名空間。</span><span class="sxs-lookup"><span data-stu-id="47fd8-136">These attributes are in the **System.Web.ModelBinding** namespace.</span></span> <span data-ttu-id="47fd8-137">包括：</span><span class="sxs-lookup"><span data-stu-id="47fd8-137">They include:</span></span>

- <span data-ttu-id="47fd8-138">控制項</span><span class="sxs-lookup"><span data-stu-id="47fd8-138">Control</span></span>
- <span data-ttu-id="47fd8-139">Cookie</span><span class="sxs-lookup"><span data-stu-id="47fd8-139">Cookie</span></span>
- <span data-ttu-id="47fd8-140">表單</span><span class="sxs-lookup"><span data-stu-id="47fd8-140">Form</span></span>
- <span data-ttu-id="47fd8-141">設定檔</span><span class="sxs-lookup"><span data-stu-id="47fd8-141">Profile</span></span>
- <span data-ttu-id="47fd8-142">QueryString</span><span class="sxs-lookup"><span data-stu-id="47fd8-142">QueryString</span></span>
- <span data-ttu-id="47fd8-143">RouteData</span><span class="sxs-lookup"><span data-stu-id="47fd8-143">RouteData</span></span>
- <span data-ttu-id="47fd8-144">工作階段</span><span class="sxs-lookup"><span data-stu-id="47fd8-144">Session</span></span>
- <span data-ttu-id="47fd8-145">使用者設定檔</span><span class="sxs-lookup"><span data-stu-id="47fd8-145">UserProfile</span></span>
- <span data-ttu-id="47fd8-146">ViewState</span><span class="sxs-lookup"><span data-stu-id="47fd8-146">ViewState</span></span>

<span data-ttu-id="47fd8-147">在本教學課程中，您將使用控制項的值來篩選在 GridView 中顯示的記錄。</span><span class="sxs-lookup"><span data-stu-id="47fd8-147">In this tutorial, you will use a control's value to filter which records are displayed in the GridView.</span></span> <span data-ttu-id="47fd8-148">您將加入**控制項**屬性設定為您稍早建立的查詢方法。</span><span class="sxs-lookup"><span data-stu-id="47fd8-148">You will add the **Control** attribute to the query method you had created earlier.</span></span> <span data-ttu-id="47fd8-149">在[稍後](using-query-string-values-to-retrieve-data.md)教學課程中，您將會套用**QueryString**屬性設定為指定的參數值來自查詢字串值的參數。</span><span class="sxs-lookup"><span data-stu-id="47fd8-149">In a [later](using-query-string-values-to-retrieve-data.md) tutorial, you will apply the **QueryString** attribute to a parameter to specify that the parameter value comes from a query string value.</span></span>

<span data-ttu-id="47fd8-150">首先，ValidationSummary 上述新增的下拉式清單，篩選會顯示哪些學生。</span><span class="sxs-lookup"><span data-stu-id="47fd8-150">First, above the ValidationSummary, add a drop down list for filtering which students are shown.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

<span data-ttu-id="47fd8-151">在程式碼後置檔案中，修改選取的方法，以接收的值從控制項，而設定的參數名稱為控制項提供值的名稱。</span><span class="sxs-lookup"><span data-stu-id="47fd8-151">In the code-behind file, modify the select method to receive a value from the control, and set the name of the parameter to the name of the control that provides the value.</span></span>

<span data-ttu-id="47fd8-152">您必須新增**使用**陳述式**System.Web.ModelBinding**解決控制項屬性的命名空間。</span><span class="sxs-lookup"><span data-stu-id="47fd8-152">You must add a **using** statement for the **System.Web.ModelBinding** namespace to resolve the Control attribute.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

<span data-ttu-id="47fd8-153">下列程式碼顯示重新能夠用來篩選傳回的資料，根據值的下拉式清單選取方法。</span><span class="sxs-lookup"><span data-stu-id="47fd8-153">The following code shows the select method re-worked to filter the returned data based on the value of the drop down list.</span></span> <span data-ttu-id="47fd8-154">參數會指定此參數的值來自具有相同名稱的控制項之前，請加入控制項屬性。</span><span class="sxs-lookup"><span data-stu-id="47fd8-154">Adding a control attribute before a parameter specifies that the value for this parameter comes from a control with the same name.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

<span data-ttu-id="47fd8-155">執行 web 應用程式，並從下拉式清單來篩選學員清單中選取不同的值。</span><span class="sxs-lookup"><span data-stu-id="47fd8-155">Run the web application and select different values from the drop down list to filter the list of students.</span></span>

![篩選學生版](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a><span data-ttu-id="47fd8-157">結論</span><span class="sxs-lookup"><span data-stu-id="47fd8-157">Conclusion</span></span>

<span data-ttu-id="47fd8-158">在本教學課程中，您可以啟用資料的排序和分頁。</span><span class="sxs-lookup"><span data-stu-id="47fd8-158">In this tutorial, you enabled sorting and paging of the data.</span></span> <span data-ttu-id="47fd8-159">您也可以啟用控制項的值來篩選資料。</span><span class="sxs-lookup"><span data-stu-id="47fd8-159">You also enabled filtering the data by the value of a control.</span></span>

<span data-ttu-id="47fd8-160">在接下來[教學課程](integrating-jquery-ui.md)JQuery UI 小工具整合到動態資料範本，您將提升 UI。</span><span class="sxs-lookup"><span data-stu-id="47fd8-160">In the next [tutorial](integrating-jquery-ui.md) you will enhance the UI by integrating a JQuery UI widget into the dynamic data template.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="47fd8-161">[上一頁](updating-deleting-and-creating-data.md)
[下一頁](integrating-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="47fd8-161">[Previous](updating-deleting-and-creating-data.md)
[Next](integrating-jquery-ui.md)</span></span>
