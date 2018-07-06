---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: 使用查詢字串值來使用模型繫結篩選資料，以及 web form |Microsoft Docs
author: tfitzmac
description: 本系列教學課程示範使用 ASP.NET Web Form 專案中的模型繫結的基本層面。 模型繫結進行資料互動更多簡單-...
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: b4615d004a32d00b91635bc321a2d4ea792fddbe
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837259"
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a><span data-ttu-id="e289d-104">使用模型繫結和 web form 中的查詢字串值來篩選資料</span><span class="sxs-lookup"><span data-stu-id="e289d-104">Using query string values to filter data with model binding and web forms</span></span>
====================
<span data-ttu-id="e289d-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e289d-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e289d-106">本系列教學課程示範使用 ASP.NET Web Form 專案中的模型繫結的基本層面。</span><span class="sxs-lookup"><span data-stu-id="e289d-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="e289d-107">模型繫結進行更簡單的比處理資料來源物件 （例如 ObjectDataSource 或 SqlDataSource） 的資料互動。</span><span class="sxs-lookup"><span data-stu-id="e289d-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="e289d-108">此系列簡介資料的開頭，並在稍後的教學課程中將移至更進階的概念。</span><span class="sxs-lookup"><span data-stu-id="e289d-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="e289d-109">本教學課程會示範如何在查詢字串中傳遞的值，並使用該值來擷取資料，透過模型繫結。</span><span class="sxs-lookup"><span data-stu-id="e289d-109">This tutorial shows how to pass a value in the query string and use that value to retrieve data through model binding.</span></span>
> 
> <span data-ttu-id="e289d-110">本教學課程中建立的專案是根據[早](retrieving-data.md)系列的一部分。</span><span class="sxs-lookup"><span data-stu-id="e289d-110">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="e289d-111">您可以[下載](https://go.microsoft.com/fwlink/?LinkId=286116)完整的專案，以 C# 或 vb。</span><span class="sxs-lookup"><span data-stu-id="e289d-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="e289d-112">可下載的程式碼適用於 Visual Studio 2012 或 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="e289d-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="e289d-113">它會使用 Visual Studio 2012 範本，也就是本教學課程中的 Visual Studio 2013 範本稍有不同。</span><span class="sxs-lookup"><span data-stu-id="e289d-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="e289d-114">您將建置</span><span class="sxs-lookup"><span data-stu-id="e289d-114">What you'll build</span></span>

<span data-ttu-id="e289d-115">在本教學課程中，您將會：</span><span class="sxs-lookup"><span data-stu-id="e289d-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="e289d-116">加入新的頁面，以便顯示已註冊的課程的學生身分</span><span class="sxs-lookup"><span data-stu-id="e289d-116">Add a new page to show the enrolled courses for a student</span></span>
2. <span data-ttu-id="e289d-117">擷取查詢字串中的值為基礎的所選學生的已註冊的課程</span><span class="sxs-lookup"><span data-stu-id="e289d-117">Retrieve the enrolled courses for the selected student based on a value in the query string</span></span>
3. <span data-ttu-id="e289d-118">從格線檢視中加入新的頁面的超連結，使用查詢字串值</span><span class="sxs-lookup"><span data-stu-id="e289d-118">Add a hyperlink with a query string value from the grid view to the new page</span></span>

<span data-ttu-id="e289d-119">在本教學課程的步驟都相當類似您在先前[教學課程](sorting-paging-and-filtering-data.md)來篩選顯示的學生，取決於使用者選擇的下拉式清單中。</span><span class="sxs-lookup"><span data-stu-id="e289d-119">The steps in this tutorial are fairly similar to what you did in the earlier [tutorial](sorting-paging-and-filtering-data.md) to filter the displayed students based on the user selection in a drop down list.</span></span> <span data-ttu-id="e289d-120">在該教學課程中，您已使用**控制**中選取的方法，以指定的參數值來自控制項的屬性。</span><span class="sxs-lookup"><span data-stu-id="e289d-120">In that tutorial, you used the **Control** attribute in the select method to specify that the parameter value comes from a control.</span></span> <span data-ttu-id="e289d-121">在本教學課程中，您將使用**QueryString**在選取的方法，以指定的參數值來自查詢字串中的屬性。</span><span class="sxs-lookup"><span data-stu-id="e289d-121">In this tutorial, you'll use the **QueryString** attribute in the select method to specify that the parameter value comes from the query string.</span></span>

## <a name="add-new-page-for-displaying-a-students-courses"></a><span data-ttu-id="e289d-122">加入新的頁面，來顯示某學生的課程</span><span class="sxs-lookup"><span data-stu-id="e289d-122">Add new page for displaying a student's courses</span></span>

<span data-ttu-id="e289d-123">加入新的 web form 使用 Site.master 主版頁面，再將頁面命名**課程**。</span><span class="sxs-lookup"><span data-stu-id="e289d-123">Add a new web form that uses the Site.master master page, and name the page **Courses**.</span></span>

<span data-ttu-id="e289d-124">在  **Courses.aspx**檔案中，新增 方格檢視來顯示所選取學生的課程。</span><span class="sxs-lookup"><span data-stu-id="e289d-124">In the **Courses.aspx** file, add a grid view to display the courses for the selected student.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a><span data-ttu-id="e289d-125">定義 select 方法</span><span class="sxs-lookup"><span data-stu-id="e289d-125">Define the select method</span></span>

<span data-ttu-id="e289d-126">在  **Courses.aspx.cs**，將您在格線檢視中指定的名稱與 select 方法**SelectMethod**屬性。</span><span class="sxs-lookup"><span data-stu-id="e289d-126">In **Courses.aspx.cs**, you will add the select method with the name you specified in the grid view's **SelectMethod** property.</span></span> <span data-ttu-id="e289d-127">在該方法中，您將定義查詢來擷取某學生的課程，並指定參數來自具有相同名稱做為參數的查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="e289d-127">In that method, you'll define the query for retrieving a student's courses, and specify that the parameter comes from a query string value with the same name as the parameter.</span></span>

<span data-ttu-id="e289d-128">首先，您必須新增下列**使用**陳述式。</span><span class="sxs-lookup"><span data-stu-id="e289d-128">First, you must add the following **using** statements.</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

<span data-ttu-id="e289d-129">然後，將下列程式碼新增至 Courses.aspx.cs 中：</span><span class="sxs-lookup"><span data-stu-id="e289d-129">Then, add the following code to Courses.aspx.cs:</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

<span data-ttu-id="e289d-130">查詢字串屬性表示，為 StudentID 的查詢字串值會自動指派給這個方法中的參數。</span><span class="sxs-lookup"><span data-stu-id="e289d-130">The QueryString attribute means that a query string value named StudentID is automatically assigned to the parameter in this method.</span></span>

## <a name="add-hyperlink-with-query-string-value"></a><span data-ttu-id="e289d-131">加入超連結，以查詢字串值</span><span class="sxs-lookup"><span data-stu-id="e289d-131">Add hyperlink with query string value</span></span>

<span data-ttu-id="e289d-132">在上 Students.aspx 方格檢視中，您將加入超連結欄位是連結到您的新課程頁面。</span><span class="sxs-lookup"><span data-stu-id="e289d-132">In the grid view on Students.aspx, you will add a hyperlink field that links to your new Courses page.</span></span> <span data-ttu-id="e289d-133">超連結會包含查詢字串值的學生識別碼。</span><span class="sxs-lookup"><span data-stu-id="e289d-133">The hyperlink will include a query string value with the student's id.</span></span>

<span data-ttu-id="e289d-134">在 Students.aspx，加入下列欄位之欄位的正下方的方格檢視資料行的信用總額。</span><span class="sxs-lookup"><span data-stu-id="e289d-134">In Students.aspx, add the following field to the grid view columns just below the field for Total Credits.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

<span data-ttu-id="e289d-135">執行應用程式，並請注意，[格線] 檢視現在會包含 [課程] 連結。</span><span class="sxs-lookup"><span data-stu-id="e289d-135">Run the application and notice that the grid view now includes the Courses link.</span></span>

![加入超連結](using-query-string-values-to-retrieve-data/_static/image1.png)

<span data-ttu-id="e289d-137">當您按一下其中一個連結時，您會看到該學生的已註冊的課程。</span><span class="sxs-lookup"><span data-stu-id="e289d-137">When you click one of the links, you'll see that student's enrolled courses.</span></span>

![顯示課程](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="e289d-139">結論</span><span class="sxs-lookup"><span data-stu-id="e289d-139">Conclusion</span></span>

<span data-ttu-id="e289d-140">在本教學課程中，您可以加入連結，以使用查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="e289d-140">In this tutorial, you added a link with a query string value.</span></span> <span data-ttu-id="e289d-141">您可以使用該查詢字串值選取方法的參數值。</span><span class="sxs-lookup"><span data-stu-id="e289d-141">You used that query string value for the parameter value in the select method.</span></span>

<span data-ttu-id="e289d-142">在接下來[教學課程](adding-business-logic-layer.md)，您也會將程式碼從程式碼後置檔案中，移到商業邏輯層和資料存取層。</span><span class="sxs-lookup"><span data-stu-id="e289d-142">In the next [tutorial](adding-business-logic-layer.md), you will move the code from the code-behind files into a business logic layer and a data access layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e289d-143">[上一頁](integrating-jquery-ui.md)
> [下一頁](adding-business-logic-layer.md)</span><span class="sxs-lookup"><span data-stu-id="e289d-143">[Previous](integrating-jquery-ui.md)
[Next](adding-business-logic-layer.md)</span></span>
