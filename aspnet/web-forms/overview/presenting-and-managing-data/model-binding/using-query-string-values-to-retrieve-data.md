---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: 使用查詢字串值來篩選資料與模型繫結和 web form |Microsoft 文件
author: tfitzmac
description: 此教學課程系列示範與 ASP.NET Web Form 專案中使用模型繫結的基本層面。 模型繫結進行資料互動詳細直線-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 03d20decf0eeff6062fbc6f8dd66f644b405c7cc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a><span data-ttu-id="d9a87-104">使用模型繫結和 web form 中的查詢字串值來篩選資料</span><span class="sxs-lookup"><span data-stu-id="d9a87-104">Using query string values to filter data with model binding and web forms</span></span>
====================
<span data-ttu-id="d9a87-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d9a87-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d9a87-106">此教學課程系列示範與 ASP.NET Web Form 專案中使用模型繫結的基本層面。</span><span class="sxs-lookup"><span data-stu-id="d9a87-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="d9a87-107">模型繫結進行更直覺比處理的資料來源物件 （例如 ObjectDataSource 或 SqlDataSource） 的資料互動。</span><span class="sxs-lookup"><span data-stu-id="d9a87-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="d9a87-108">此數列開頭簡介資料，並會移到更進階的概念中之後的教學課程。</span><span class="sxs-lookup"><span data-stu-id="d9a87-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="d9a87-109">本教學課程會示範如何查詢字串中傳遞的值，並使用該值來透過模型繫結來擷取資料。</span><span class="sxs-lookup"><span data-stu-id="d9a87-109">This tutorial shows how to pass a value in the query string and use that value to retrieve data through model binding.</span></span>
> 
> <span data-ttu-id="d9a87-110">本教學課程中建立的專案是[早](retrieving-data.md)數列的組件。</span><span class="sxs-lookup"><span data-stu-id="d9a87-110">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="d9a87-111">您可以[下載](https://go.microsoft.com/fwlink/?LinkId=286116)以 C# 或 VB 完整的專案</span><span class="sxs-lookup"><span data-stu-id="d9a87-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="d9a87-112">可下載的程式碼適用於 Visual Studio 2012 或 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="d9a87-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="d9a87-113">它會使用 Visual Studio 2012 範本，這可能會比 Visual Studio 2013 範本顯示在本教學課程稍有不同。</span><span class="sxs-lookup"><span data-stu-id="d9a87-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="d9a87-114">您將建置</span><span class="sxs-lookup"><span data-stu-id="d9a87-114">What you'll build</span></span>

<span data-ttu-id="d9a87-115">在此教學課程中，您將會：</span><span class="sxs-lookup"><span data-stu-id="d9a87-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="d9a87-116">加入新的頁面，以顯示 已註冊的課程學生</span><span class="sxs-lookup"><span data-stu-id="d9a87-116">Add a new page to show the enrolled courses for a student</span></span>
2. <span data-ttu-id="d9a87-117">為選取的查詢字串中的值為基礎的學生所擷取的已註冊的課程</span><span class="sxs-lookup"><span data-stu-id="d9a87-117">Retrieve the enrolled courses for the selected student based on a value in the query string</span></span>
3. <span data-ttu-id="d9a87-118">從格線檢視的查詢字串值的超連結加入新的頁面</span><span class="sxs-lookup"><span data-stu-id="d9a87-118">Add a hyperlink with a query string value from the grid view to the new page</span></span>

<span data-ttu-id="d9a87-119">在本教學課程的步驟會相當類似於您所執行先前[教學課程](sorting-paging-and-filtering-data.md)來篩選顯示的學生，根據使用者選項，在下拉式清單中。</span><span class="sxs-lookup"><span data-stu-id="d9a87-119">The steps in this tutorial are fairly similar to what you did in the earlier [tutorial](sorting-paging-and-filtering-data.md) to filter the displayed students based on the user selection in a drop down list.</span></span> <span data-ttu-id="d9a87-120">在此教學課程中，在您使用**控制項**中選取的方法，以指定的參數值來自的控制項屬性。</span><span class="sxs-lookup"><span data-stu-id="d9a87-120">In that tutorial, you used the **Control** attribute in the select method to specify that the parameter value comes from a control.</span></span> <span data-ttu-id="d9a87-121">在此教學課程中，您將使用**QueryString**選取方法，以指定的參數值來自查詢字串中的屬性。</span><span class="sxs-lookup"><span data-stu-id="d9a87-121">In this tutorial, you'll use the **QueryString** attribute in the select method to specify that the parameter value comes from the query string.</span></span>

## <a name="add-new-page-for-displaying-a-students-courses"></a><span data-ttu-id="d9a87-122">加入新的頁面，顯示一位學生課程</span><span class="sxs-lookup"><span data-stu-id="d9a87-122">Add new page for displaying a student's courses</span></span>

<span data-ttu-id="d9a87-123">加入新的 web 表單使用 Site.master 主版頁面，再將頁面命名**課程**。</span><span class="sxs-lookup"><span data-stu-id="d9a87-123">Add a new web form that uses the Site.master master page, and name the page **Courses**.</span></span>

<span data-ttu-id="d9a87-124">在**Courses.aspx**檔案中，加入要顯示所選的學生課程方格檢視。</span><span class="sxs-lookup"><span data-stu-id="d9a87-124">In the **Courses.aspx** file, add a grid view to display the courses for the selected student.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a><span data-ttu-id="d9a87-125">定義 select 方法</span><span class="sxs-lookup"><span data-stu-id="d9a87-125">Define the select method</span></span>

<span data-ttu-id="d9a87-126">在**Courses.aspx.cs**，您會將選取的方法與您在格線檢視中指定的名稱加入**SelectMethod**屬性。</span><span class="sxs-lookup"><span data-stu-id="d9a87-126">In **Courses.aspx.cs**, you will add the select method with the name you specified in the grid view's **SelectMethod** property.</span></span> <span data-ttu-id="d9a87-127">在該方法中，您會定義查詢來擷取一位學生課程，並指定參數來自具有相同名稱做為參數的查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="d9a87-127">In that method, you'll define the query for retrieving a student's courses, and specify that the parameter comes from a query string value with the same name as the parameter.</span></span>

<span data-ttu-id="d9a87-128">首先，您必須新增下列**使用**陳述式。</span><span class="sxs-lookup"><span data-stu-id="d9a87-128">First, you must add the following **using** statements.</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

<span data-ttu-id="d9a87-129">然後，Courses.aspx.cs 中加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="d9a87-129">Then, add the following code to Courses.aspx.cs:</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

<span data-ttu-id="d9a87-130">查詢字串屬性表示名為 StudentID 的查詢字串值會自動指派至這個方法中的參數。</span><span class="sxs-lookup"><span data-stu-id="d9a87-130">The QueryString attribute means that a query string value named StudentID is automatically assigned to the parameter in this method.</span></span>

## <a name="add-hyperlink-with-query-string-value"></a><span data-ttu-id="d9a87-131">加入超連結與查詢字串值</span><span class="sxs-lookup"><span data-stu-id="d9a87-131">Add hyperlink with query string value</span></span>

<span data-ttu-id="d9a87-132">在上 Students.aspx 方格檢視中，您將加入到新的課程頁面連結的超連結欄位。</span><span class="sxs-lookup"><span data-stu-id="d9a87-132">In the grid view on Students.aspx, you will add a hyperlink field that links to your new Courses page.</span></span> <span data-ttu-id="d9a87-133">超連結會包含與學生識別碼的查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="d9a87-133">The hyperlink will include a query string value with the student's id.</span></span>

<span data-ttu-id="d9a87-134">在 Students.aspx，加入下列欄位欄位的正下方的方格檢視資料行的總點數。</span><span class="sxs-lookup"><span data-stu-id="d9a87-134">In Students.aspx, add the following field to the grid view columns just below the field for Total Credits.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

<span data-ttu-id="d9a87-135">執行應用程式，請注意，資料格檢視現在會包含課程連結。</span><span class="sxs-lookup"><span data-stu-id="d9a87-135">Run the application and notice that the grid view now includes the Courses link.</span></span>

![加入超連結](using-query-string-values-to-retrieve-data/_static/image1.png)

<span data-ttu-id="d9a87-137">當您按一下其中一個連結時，您會看到該學生的已註冊的課程。</span><span class="sxs-lookup"><span data-stu-id="d9a87-137">When you click one of the links, you'll see that student's enrolled courses.</span></span>

![顯示課程](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="d9a87-139">結論</span><span class="sxs-lookup"><span data-stu-id="d9a87-139">Conclusion</span></span>

<span data-ttu-id="d9a87-140">在本教學課程中，您可以加入具有查詢字串值的連結。</span><span class="sxs-lookup"><span data-stu-id="d9a87-140">In this tutorial, you added a link with a query string value.</span></span> <span data-ttu-id="d9a87-141">您可以使用該查詢字串值的 select 方法參數值。</span><span class="sxs-lookup"><span data-stu-id="d9a87-141">You used that query string value for the parameter value in the select method.</span></span>

<span data-ttu-id="d9a87-142">在接下來[教學課程](adding-business-logic-layer.md)，您也會將程式碼從程式碼後置檔案中，移到商務邏輯層和資料存取層。</span><span class="sxs-lookup"><span data-stu-id="d9a87-142">In the next [tutorial](adding-business-logic-layer.md), you will move the code from the code-behind files into a business logic layer and a data access layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d9a87-143">[上一頁](integrating-jquery-ui.md)
> [下一頁](adding-business-logic-layer.md)</span><span class="sxs-lookup"><span data-stu-id="d9a87-143">[Previous](integrating-jquery-ui.md)
[Next](adding-business-logic-layer.md)</span></span>
