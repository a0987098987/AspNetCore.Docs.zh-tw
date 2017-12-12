---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: "使用模型繫結和 web form 整合 JQuery UI 日期選擇器 |Microsoft 文件"
author: tfitzmac
description: "此教學課程系列示範與 ASP.NET Web Form 專案中使用模型繫結的基本層面。 模型繫結進行資料互動詳細直線-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: da3c8f347a709a4c9a47fd0ecce5201d9b0cd1b1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a><span data-ttu-id="819e1-104">使用模型繫結和 web form 整合 JQuery UI 日期選擇器</span><span class="sxs-lookup"><span data-stu-id="819e1-104">Integrating JQuery UI Datepicker with model binding and web forms</span></span>
====================
<span data-ttu-id="819e1-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="819e1-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="819e1-106">此教學課程系列示範與 ASP.NET Web Form 專案中使用模型繫結的基本層面。</span><span class="sxs-lookup"><span data-stu-id="819e1-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="819e1-107">模型繫結進行更直覺比處理的資料來源物件 （例如 ObjectDataSource 或 SqlDataSource） 的資料互動。</span><span class="sxs-lookup"><span data-stu-id="819e1-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="819e1-108">此數列開頭簡介資料，並會移到更進階的概念中之後的教學課程。</span><span class="sxs-lookup"><span data-stu-id="819e1-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="819e1-109">本教學課程示範如何將 JQuery UI[日期選擇器 widget](http://jqueryui.com/datepicker/) Web 表單，並使用模型繫結至選取的值以更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="819e1-109">This tutorial shows how to add the JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) to a Web Form, and use model binding to update the database with the selected value.</span></span>
> 
> <span data-ttu-id="819e1-110">本教學課程中建立的專案是[第一個](retrieving-data.md)和[第二個](updating-deleting-and-creating-data.md)數列的組件。</span><span class="sxs-lookup"><span data-stu-id="819e1-110">This tutorial builds on the project created in the [first](retrieving-data.md) and [second](updating-deleting-and-creating-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="819e1-111">您可以[下載](https://go.microsoft.com/fwlink/?LinkId=286116)以 C# 或 VB 完整的專案</span><span class="sxs-lookup"><span data-stu-id="819e1-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="819e1-112">可下載的程式碼適用於 Visual Studio 2012 或 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="819e1-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="819e1-113">它會使用 Visual Studio 2012 範本，這可能會比 Visual Studio 2013 範本顯示在本教學課程稍有不同。</span><span class="sxs-lookup"><span data-stu-id="819e1-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="819e1-114">您將建置</span><span class="sxs-lookup"><span data-stu-id="819e1-114">What you'll build</span></span>

<span data-ttu-id="819e1-115">在此教學課程中，您將會：</span><span class="sxs-lookup"><span data-stu-id="819e1-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="819e1-116">將屬性加入至您的模型，以記錄學生註冊日期</span><span class="sxs-lookup"><span data-stu-id="819e1-116">Add a property to your model to record the student's enrollment date</span></span>
2. <span data-ttu-id="819e1-117">可讓使用者選取的 JQuery UI 日期選擇器 widget 註冊日期</span><span class="sxs-lookup"><span data-stu-id="819e1-117">Enable the user to select the enrollment date using the JQuery UI Datepicker widget</span></span>
3. <span data-ttu-id="819e1-118">註冊日期，強制執行驗證規則</span><span class="sxs-lookup"><span data-stu-id="819e1-118">Enforce validation rules for the enrollment date</span></span>

<span data-ttu-id="819e1-119">JQuery UI 日期選擇器小工具可讓使用者輕鬆地從使用者互動的欄位時，快顯行事曆選取日期。</span><span class="sxs-lookup"><span data-stu-id="819e1-119">The JQuery UI Datepicker widget enables users to easily select a date from a calendar that pops up when the user interacts with the field.</span></span> <span data-ttu-id="819e1-120">使用這個小工具可能會更方便使用者比手動輸入的日期。</span><span class="sxs-lookup"><span data-stu-id="819e1-120">Using this widget can be more convenient for users than manually typing a date.</span></span> <span data-ttu-id="819e1-121">將 [日期選擇器] widget 中整合至模型繫結使用的資料作業的網頁需要只有少量的額外工作。</span><span class="sxs-lookup"><span data-stu-id="819e1-121">Integrating the Datepicker widget into a page that uses model binding for data operations requires only a small amount of additional work.</span></span>

## <a name="add-a-new-property-to-the-model"></a><span data-ttu-id="819e1-122">將新屬性加入至模型</span><span class="sxs-lookup"><span data-stu-id="819e1-122">Add a new property to the model</span></span>

<span data-ttu-id="819e1-123">首先，您將加入**Datetime**您學生的屬性建立的模型，並將該變更移轉到資料庫。</span><span class="sxs-lookup"><span data-stu-id="819e1-123">First, you will add a **Datetime** property to your Student model and migrate that change to the database.</span></span> <span data-ttu-id="819e1-124">開啟**UniversityModels.cs**，並將反白顯示的程式碼加入至學生模型。</span><span class="sxs-lookup"><span data-stu-id="819e1-124">Open **UniversityModels.cs**, and add the highlighted code to the Student model.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

<span data-ttu-id="819e1-125">**RangeAttribute**是否包含強制執行驗證規則的屬性。</span><span class="sxs-lookup"><span data-stu-id="819e1-125">The **RangeAttribute** is included to enforce validation rules for the property.</span></span> <span data-ttu-id="819e1-126">此教學課程中，我們會假設 Contoso 大學創立在 2013 年 1 月 1 日，並因此先前註冊日期不是有效。</span><span class="sxs-lookup"><span data-stu-id="819e1-126">For this tutorial, we will assume that Contoso University was founded on January 1st, 2013 and therefore earlier enrollment dates are not valid.</span></span>

<span data-ttu-id="819e1-127">在封裝管理視窗中，加入在移轉執行命令**新增移轉 AddEnrollmentDate**。</span><span class="sxs-lookup"><span data-stu-id="819e1-127">In the Package Management window, add a migration by running the command **add-migration AddEnrollmentDate**.</span></span> <span data-ttu-id="819e1-128">請注意，移轉程式碼會將新的日期時間資料行加入學生資料表。</span><span class="sxs-lookup"><span data-stu-id="819e1-128">Notice that the migration code adds the new Datetime column to the Student table.</span></span> <span data-ttu-id="819e1-129">反白顯示的下列程式碼所示，以符合您在 RangeAttribute 中指定的值，加入新的資料行的預設值。</span><span class="sxs-lookup"><span data-stu-id="819e1-129">To match the value you specified in the RangeAttribute, add a default value for the new column, as shown in the highlighted code below.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

<span data-ttu-id="819e1-130">將您的變更儲存至移轉檔案。</span><span class="sxs-lookup"><span data-stu-id="819e1-130">Save your change to the migration file.</span></span>

<span data-ttu-id="819e1-131">您不需要重新植入資料。</span><span class="sxs-lookup"><span data-stu-id="819e1-131">You do not need to seed the data again.</span></span> <span data-ttu-id="819e1-132">因此，開啟**configuration.cs 中**Migrations 資料夾中，移除或註解中的程式碼**種子**方法。</span><span class="sxs-lookup"><span data-stu-id="819e1-132">Therefore, open **Configuration.cs** in the Migrations folder and remove or comment out the code in the **Seed** method.</span></span> <span data-ttu-id="819e1-133">儲存並關閉檔案。</span><span class="sxs-lookup"><span data-stu-id="819e1-133">Save and close the file.</span></span>

<span data-ttu-id="819e1-134">現在，執行命令**更新資料庫**。</span><span class="sxs-lookup"><span data-stu-id="819e1-134">Now, run the command **update-database**.</span></span> <span data-ttu-id="819e1-135">請注意，資料行存在於資料庫中的所有現有的記錄都有預設值為 EnrollmentDate。</span><span class="sxs-lookup"><span data-stu-id="819e1-135">Notice that the column now exists in the database and all of the existing records have the default value for EnrollmentDate.</span></span>

## <a name="add-dynamic-controls-for-enrollment-date"></a><span data-ttu-id="819e1-136">註冊日期集將動態控制項</span><span class="sxs-lookup"><span data-stu-id="819e1-136">Add dynamic controls for enrollment date</span></span>

<span data-ttu-id="819e1-137">您現在要加入控制項，顯示與編輯註冊日期。</span><span class="sxs-lookup"><span data-stu-id="819e1-137">You will now add controls for displaying and editing the enrollment date.</span></span> <span data-ttu-id="819e1-138">此時，這個值是透過文字方塊中編輯。</span><span class="sxs-lookup"><span data-stu-id="819e1-138">At this point, the value is edited through a text box.</span></span> <span data-ttu-id="819e1-139">稍後在教學課程中，您會變更為 [JQuery 日期選擇器] widget 中的文字方塊。</span><span class="sxs-lookup"><span data-stu-id="819e1-139">Later in the tutorial, you will change the text box to the JQuery Datepicker widget.</span></span>

<span data-ttu-id="819e1-140">首先，它是很重要的一點，您不需要進行任何變更**AddStudent.aspx**檔案。</span><span class="sxs-lookup"><span data-stu-id="819e1-140">First, it is important to note that you do not need to make any change to the **AddStudent.aspx** file.</span></span> <span data-ttu-id="819e1-141">Dynamicentity 自動會轉譯為新的屬性。</span><span class="sxs-lookup"><span data-stu-id="819e1-141">The DynamicEntity control will automatically render the new property.</span></span>

<span data-ttu-id="819e1-142">開啟**Students.aspx**，並加入下列反白顯示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="819e1-142">Open **Students.aspx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

<span data-ttu-id="819e1-143">執行應用程式，請注意，您可以設定註冊日期的值所輸入的日期。</span><span class="sxs-lookup"><span data-stu-id="819e1-143">Run the application and notice that you can set the value of the enrollment date by typing a date.</span></span> <span data-ttu-id="819e1-144">當加入新的學生：</span><span class="sxs-lookup"><span data-stu-id="819e1-144">When adding a new student:</span></span>

![設定的日期](integrating-jquery-ui/_static/image1.png)

<span data-ttu-id="819e1-146">或者，您也可以編輯現有的值：</span><span class="sxs-lookup"><span data-stu-id="819e1-146">Or, editing an existing value:</span></span>

![編輯日期](integrating-jquery-ui/_static/image2.png)

<span data-ttu-id="819e1-148">輸入日期是有效的但它可能不是您想要提供的客戶體驗。</span><span class="sxs-lookup"><span data-stu-id="819e1-148">Typing the date works, but it might not be the customer experience you wish to provide.</span></span> <span data-ttu-id="819e1-149">在下一步 區段中，您將啟用選取透過行事曆日期。</span><span class="sxs-lookup"><span data-stu-id="819e1-149">In the next section, you will enable selecting a date through a calendar.</span></span>

## <a name="install-nuget-package-to-work-with-jquery-ui"></a><span data-ttu-id="819e1-150">安裝 NuGet 套件，才能使用 JQuery UI</span><span class="sxs-lookup"><span data-stu-id="819e1-150">Install NuGet package to work with JQuery UI</span></span>

<span data-ttu-id="819e1-151">**汁 UI** NuGet 封裝可讓您輕鬆整合 JQuery UI widgets 到 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="819e1-151">The **Juice UI** NuGet package enables easy integration of the JQuery UI widgets into your web application.</span></span> <span data-ttu-id="819e1-152">若要使用此封裝，請透過 NuGet 安裝。</span><span class="sxs-lookup"><span data-stu-id="819e1-152">To use this package, install it through NuGet.</span></span>

![新增汁 UI](integrating-jquery-ui/_static/image3.png)

<span data-ttu-id="819e1-154">汁 UI，您所安裝的版本可能與您的應用程式中的 JQuery 版本衝突。</span><span class="sxs-lookup"><span data-stu-id="819e1-154">The version of Juice UI that you install may conflict with the version of JQuery in your application.</span></span> <span data-ttu-id="819e1-155">此教學課程之前，請嘗試執行您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="819e1-155">Before proceeding with this tutorial, try running your application.</span></span> <span data-ttu-id="819e1-156">如果您遇到 JavaScript 錯誤，您需要調解的 JQuery 版本。</span><span class="sxs-lookup"><span data-stu-id="819e1-156">If you encounter a JavaScript error, you need to reconcile the JQuery version.</span></span> <span data-ttu-id="819e1-157">您可以預期的 JQuery 版本加入到指令碼資料夾 （版本 1.8.2 撰寫本教學課程中的每次），或在 Site.master 指定 JQuery 檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="819e1-157">You can either add the expected version of JQuery to your Scripts folder (version 1.8.2 at time of writing this tutorial), or in Site.master specify the path to the JQuery file.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a><span data-ttu-id="819e1-158">自訂 DateTime 範本，以包括日期選擇器 widget</span><span class="sxs-lookup"><span data-stu-id="819e1-158">Customize DateTime template to include Datepicker widget</span></span>

<span data-ttu-id="819e1-159">您會將 [日期選擇器] widget 中，加入動態資料範本進行編輯，日期時間值。</span><span class="sxs-lookup"><span data-stu-id="819e1-159">You will add the Datepicker widget to the dynamic data template for editing a datetime value.</span></span> <span data-ttu-id="819e1-160">將 widget 新增至範本，它會自動轉譯加入新的學生表單和格線檢視中編輯學生版。</span><span class="sxs-lookup"><span data-stu-id="819e1-160">By adding the widget to the template, it is automatically rendered in both the form for adding a new student and in the grid view for editing students.</span></span> <span data-ttu-id="819e1-161">開啟**DateTime\_Edit.ascx**，並加入下列反白顯示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="819e1-161">Open **DateTime\_Edit.ascx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

<span data-ttu-id="819e1-162">在程式碼後置檔案中，您會將日期選擇器的最小和最大日期。</span><span class="sxs-lookup"><span data-stu-id="819e1-162">In the code-behind file, you will set the minimum and maximum dates for the DatePicker.</span></span> <span data-ttu-id="819e1-163">藉由設定這些值，您將會防止使用者瀏覽至無效的日期。</span><span class="sxs-lookup"><span data-stu-id="819e1-163">By setting these values, you will prevent users from navigating to invalid dates.</span></span> <span data-ttu-id="819e1-164">您將擷取的最小和最大值**RangeAttribute**上所提供的日期時間屬性。</span><span class="sxs-lookup"><span data-stu-id="819e1-164">You will retrieve the minimum and maximum values from the **RangeAttribute** on the DateTime property, if one is provided.</span></span> <span data-ttu-id="819e1-165">開啟**DateTime\_Edit.ascx.cs**，並將下列反白顯示的程式碼加入至頁面\_載入方法。</span><span class="sxs-lookup"><span data-stu-id="819e1-165">Open **DateTime\_Edit.ascx.cs**, and add the following highlighted code to the Page\_Load method.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

<span data-ttu-id="819e1-166">執行 web 應用程式，並瀏覽至 AddStudent 頁面。</span><span class="sxs-lookup"><span data-stu-id="819e1-166">Run the web application and navigate to the AddStudent page.</span></span> <span data-ttu-id="819e1-167">提供的欄位值，請注意，當您按一下文字方塊上註冊日期時，會顯示行事曆。</span><span class="sxs-lookup"><span data-stu-id="819e1-167">Provide values for the fields and notice that when you click on the text box for Enrollment Date, the calendar is displayed.</span></span>

![日期選擇器](integrating-jquery-ui/_static/image4.png)

<span data-ttu-id="819e1-169">選取一個日期，然後按一下**插入**。</span><span class="sxs-lookup"><span data-stu-id="819e1-169">Pick a date, and click **Insert**.</span></span> <span data-ttu-id="819e1-170">RangeAttribute 會強制執行伺服器上的驗證。</span><span class="sxs-lookup"><span data-stu-id="819e1-170">The RangeAttribute enforces validation on the server.</span></span> <span data-ttu-id="819e1-171">藉由設定 minDate 屬性上的日期選擇器，您也適用於驗證用戶端上。</span><span class="sxs-lookup"><span data-stu-id="819e1-171">By setting the minDate property on the Datepicker, you also apply validation on the client.</span></span> <span data-ttu-id="819e1-172">行事曆不讓使用者瀏覽至之前的 minDate 值的日期。</span><span class="sxs-lookup"><span data-stu-id="819e1-172">The calendar does not let the user navigate to a date prior to the value of minDate.</span></span>

<span data-ttu-id="819e1-173">當您編輯資料格檢視中的記錄時，也會顯示行事曆。</span><span class="sxs-lookup"><span data-stu-id="819e1-173">When you edit a record in the grid view, the calendar is also displayed.</span></span>

![在 GridView 中的日期選擇器](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a><span data-ttu-id="819e1-175">結論</span><span class="sxs-lookup"><span data-stu-id="819e1-175">Conclusion</span></span>

<span data-ttu-id="819e1-176">在本教學課程中，您將學會如何使用模型繫結的 web 表單中併入 JQuery widget。</span><span class="sxs-lookup"><span data-stu-id="819e1-176">In this tutorial, you learned how to incorporate a JQuery widget into a web form that uses model binding.</span></span>

<span data-ttu-id="819e1-177">在接下來[教學課程](using-query-string-values-to-retrieve-data.md)，選取資料時，您將使用的查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="819e1-177">In the next [tutorial](using-query-string-values-to-retrieve-data.md), you will use a query string value when selecting data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="819e1-178">[上一頁](sorting-paging-and-filtering-data.md)
[下一頁](using-query-string-values-to-retrieve-data.md)</span><span class="sxs-lookup"><span data-stu-id="819e1-178">[Previous](sorting-paging-and-filtering-data.md)
[Next](using-query-string-values-to-retrieve-data.md)</span></span>
