---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: 與模型繫結和 web form 整合 JQuery UI Datepicker |Microsoft Docs
author: Rick-Anderson
description: 本系列教學課程示範使用 ASP.NET Web Form 專案中的模型繫結的基本層面。 模型繫結進行資料互動更多簡單-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: ff1b17295c58d40d55bdcd4346b83121b579bb4c
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020555"
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a><span data-ttu-id="99e84-104">與模型繫結和 web form 整合 JQuery UI Datepicker</span><span class="sxs-lookup"><span data-stu-id="99e84-104">Integrating JQuery UI Datepicker with model binding and web forms</span></span>
====================
<span data-ttu-id="99e84-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="99e84-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="99e84-106">本系列教學課程示範使用 ASP.NET Web Form 專案中的模型繫結的基本層面。</span><span class="sxs-lookup"><span data-stu-id="99e84-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="99e84-107">模型繫結進行更簡單的比處理資料來源物件 （例如 ObjectDataSource 或 SqlDataSource） 的資料互動。</span><span class="sxs-lookup"><span data-stu-id="99e84-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="99e84-108">此系列簡介資料的開頭，並在稍後的教學課程中將移至更進階的概念。</span><span class="sxs-lookup"><span data-stu-id="99e84-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="99e84-109">本教學課程示範如何新增 JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) Web 表單，並使用模型繫結至選取的值以更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="99e84-109">This tutorial shows how to add the JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) to a Web Form, and use model binding to update the database with the selected value.</span></span>
> 
> <span data-ttu-id="99e84-110">本教學課程中建立的專案是根據[第一個](retrieving-data.md)並[第二個](updating-deleting-and-creating-data.md)系列的一部分。</span><span class="sxs-lookup"><span data-stu-id="99e84-110">This tutorial builds on the project created in the [first](retrieving-data.md) and [second](updating-deleting-and-creating-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="99e84-111">您可以[下載](https://go.microsoft.com/fwlink/?LinkId=286116)完整的專案，以 C# 或 vb。</span><span class="sxs-lookup"><span data-stu-id="99e84-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="99e84-112">可下載的程式碼適用於 Visual Studio 2012 或 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="99e84-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="99e84-113">它會使用 Visual Studio 2012 範本，也就是本教學課程中的 Visual Studio 2013 範本稍有不同。</span><span class="sxs-lookup"><span data-stu-id="99e84-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="99e84-114">您將建置</span><span class="sxs-lookup"><span data-stu-id="99e84-114">What you'll build</span></span>

<span data-ttu-id="99e84-115">在本教學課程中，您將會：</span><span class="sxs-lookup"><span data-stu-id="99e84-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="99e84-116">將屬性加入至您的模型，來記錄學生的註冊日期</span><span class="sxs-lookup"><span data-stu-id="99e84-116">Add a property to your model to record the student's enrollment date</span></span>
2. <span data-ttu-id="99e84-117">讓使用者選取使用 JQuery UI Datepicker 小工具的註冊日期</span><span class="sxs-lookup"><span data-stu-id="99e84-117">Enable the user to select the enrollment date using the JQuery UI Datepicker widget</span></span>
3. <span data-ttu-id="99e84-118">強制執行驗證規則的註冊日期</span><span class="sxs-lookup"><span data-stu-id="99e84-118">Enforce validation rules for the enrollment date</span></span>

<span data-ttu-id="99e84-119">JQuery UI Datepicker 小工具可讓使用者輕鬆地從使用者所互動的欄位時快顯行事曆選取日期。</span><span class="sxs-lookup"><span data-stu-id="99e84-119">The JQuery UI Datepicker widget enables users to easily select a date from a calendar that pops up when the user interacts with the field.</span></span> <span data-ttu-id="99e84-120">使用這個小工具可能會更方便使用者比手動輸入的日期。</span><span class="sxs-lookup"><span data-stu-id="99e84-120">Using this widget can be more convenient for users than manually typing a date.</span></span> <span data-ttu-id="99e84-121">Datepicker 小工具整合的資料作業中使用模型繫結的頁面要求只有少量額外的工作。</span><span class="sxs-lookup"><span data-stu-id="99e84-121">Integrating the Datepicker widget into a page that uses model binding for data operations requires only a small amount of additional work.</span></span>

## <a name="add-a-new-property-to-the-model"></a><span data-ttu-id="99e84-122">將新屬性加入至模型</span><span class="sxs-lookup"><span data-stu-id="99e84-122">Add a new property to the model</span></span>

<span data-ttu-id="99e84-123">首先，您將在其中加入**Datetime**屬性，以讓學生模型，並將該變更移轉至資料庫。</span><span class="sxs-lookup"><span data-stu-id="99e84-123">First, you will add a **Datetime** property to your Student model and migrate that change to the database.</span></span> <span data-ttu-id="99e84-124">開啟**UniversityModels.cs**，並將醒目提示的程式碼新增至學生的模型。</span><span class="sxs-lookup"><span data-stu-id="99e84-124">Open **UniversityModels.cs**, and add the highlighted code to the Student model.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

<span data-ttu-id="99e84-125">**RangeAttribute**是否包含強制執行驗證規則的屬性。</span><span class="sxs-lookup"><span data-stu-id="99e84-125">The **RangeAttribute** is included to enforce validation rules for the property.</span></span> <span data-ttu-id="99e84-126">本教學課程中，我們假設 Contoso 大學創立在 2013 年 1 月 1 日起，並因此先前的註冊日期不正確。</span><span class="sxs-lookup"><span data-stu-id="99e84-126">For this tutorial, we will assume that Contoso University was founded on January 1st, 2013 and therefore earlier enrollment dates are not valid.</span></span>

<span data-ttu-id="99e84-127">在 [套件管理] 視窗中，新增移轉執行命令**新增移轉 AddEnrollmentDate**。</span><span class="sxs-lookup"><span data-stu-id="99e84-127">In the Package Management window, add a migration by running the command **add-migration AddEnrollmentDate**.</span></span> <span data-ttu-id="99e84-128">請注意，移轉程式碼會將新的日期時間資料行新增至 Student 資料表。</span><span class="sxs-lookup"><span data-stu-id="99e84-128">Notice that the migration code adds the new Datetime column to the Student table.</span></span> <span data-ttu-id="99e84-129">若要符合您在 RangeAttribute 中指定的值，請新增新的資料行的預設值，下列反白顯示的程式碼所示。</span><span class="sxs-lookup"><span data-stu-id="99e84-129">To match the value you specified in the RangeAttribute, add a default value for the new column, as shown in the highlighted code below.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

<span data-ttu-id="99e84-130">將變更儲存到移轉檔案中。</span><span class="sxs-lookup"><span data-stu-id="99e84-130">Save your change to the migration file.</span></span>

<span data-ttu-id="99e84-131">您不需要重新植入資料。</span><span class="sxs-lookup"><span data-stu-id="99e84-131">You do not need to seed the data again.</span></span> <span data-ttu-id="99e84-132">因此，開啟**Configuration.cs**在 [移轉] 資料夾並移除或註解中的程式碼**種子**方法。</span><span class="sxs-lookup"><span data-stu-id="99e84-132">Therefore, open **Configuration.cs** in the Migrations folder and remove or comment out the code in the **Seed** method.</span></span> <span data-ttu-id="99e84-133">儲存並關閉檔案。</span><span class="sxs-lookup"><span data-stu-id="99e84-133">Save and close the file.</span></span>

<span data-ttu-id="99e84-134">現在，執行命令**更新資料庫**。</span><span class="sxs-lookup"><span data-stu-id="99e84-134">Now, run the command **update-database**.</span></span> <span data-ttu-id="99e84-135">請注意，資料行存在於資料庫中的所有現有的記錄都有預設值的 EnrollmentDate。</span><span class="sxs-lookup"><span data-stu-id="99e84-135">Notice that the column now exists in the database and all of the existing records have the default value for EnrollmentDate.</span></span>

## <a name="add-dynamic-controls-for-enrollment-date"></a><span data-ttu-id="99e84-136">新增動態控制項個註冊日期</span><span class="sxs-lookup"><span data-stu-id="99e84-136">Add dynamic controls for enrollment date</span></span>

<span data-ttu-id="99e84-137">您現在要加入控制項來顯示和編輯註冊日期。</span><span class="sxs-lookup"><span data-stu-id="99e84-137">You will now add controls for displaying and editing the enrollment date.</span></span> <span data-ttu-id="99e84-138">到目前為止，是可以透過 [文字] 方塊中編輯值。</span><span class="sxs-lookup"><span data-stu-id="99e84-138">At this point, the value is edited through a text box.</span></span> <span data-ttu-id="99e84-139">稍後在教學課程中，您會變更至 JQuery Datepicker 小工具的文字方塊。</span><span class="sxs-lookup"><span data-stu-id="99e84-139">Later in the tutorial, you will change the text box to the JQuery Datepicker widget.</span></span>

<span data-ttu-id="99e84-140">首先，請務必請注意，您不需要進行任何變更**AddStudent.aspx**檔案。</span><span class="sxs-lookup"><span data-stu-id="99e84-140">First, it is important to note that you do not need to make any change to the **AddStudent.aspx** file.</span></span> <span data-ttu-id="99e84-141">DynamicEntity 控制項將自動呈現新的屬性。</span><span class="sxs-lookup"><span data-stu-id="99e84-141">The DynamicEntity control will automatically render the new property.</span></span>

<span data-ttu-id="99e84-142">開啟**Students.aspx**，並新增下列醒目提示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="99e84-142">Open **Students.aspx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

<span data-ttu-id="99e84-143">執行應用程式，並請注意，您可以設定的註冊日期值所輸入的日期。</span><span class="sxs-lookup"><span data-stu-id="99e84-143">Run the application and notice that you can set the value of the enrollment date by typing a date.</span></span> <span data-ttu-id="99e84-144">當加入新的學生：</span><span class="sxs-lookup"><span data-stu-id="99e84-144">When adding a new student:</span></span>

![設定的日期](integrating-jquery-ui/_static/image1.png)

<span data-ttu-id="99e84-146">或者，您也可以編輯現有的值：</span><span class="sxs-lookup"><span data-stu-id="99e84-146">Or, editing an existing value:</span></span>

![編輯日期](integrating-jquery-ui/_static/image2.png)

<span data-ttu-id="99e84-148">輸入日期的運作方式，但它可能不是您想要提供的客戶體驗。</span><span class="sxs-lookup"><span data-stu-id="99e84-148">Typing the date works, but it might not be the customer experience you wish to provide.</span></span> <span data-ttu-id="99e84-149">在下一步 區段中，您將啟用選取透過行事曆的日期。</span><span class="sxs-lookup"><span data-stu-id="99e84-149">In the next section, you will enable selecting a date through a calendar.</span></span>

## <a name="install-nuget-package-to-work-with-jquery-ui"></a><span data-ttu-id="99e84-150">安裝 NuGet 套件，才能使用 JQuery UI</span><span class="sxs-lookup"><span data-stu-id="99e84-150">Install NuGet package to work with JQuery UI</span></span>

<span data-ttu-id="99e84-151">**汁 UI** NuGet 套件可讓您輕鬆整合到 web 應用程式的 JQuery UI widgets。</span><span class="sxs-lookup"><span data-stu-id="99e84-151">The **Juice UI** NuGet package enables easy integration of the JQuery UI widgets into your web application.</span></span> <span data-ttu-id="99e84-152">若要使用此套件，請透過 NuGet 安裝。</span><span class="sxs-lookup"><span data-stu-id="99e84-152">To use this package, install it through NuGet.</span></span>

![新增汁 UI](integrating-jquery-ui/_static/image3.png)

<span data-ttu-id="99e84-154">汁 UI，您所安裝的版本可能與您的應用程式中的 JQuery 版本衝突。</span><span class="sxs-lookup"><span data-stu-id="99e84-154">The version of Juice UI that you install may conflict with the version of JQuery in your application.</span></span> <span data-ttu-id="99e84-155">繼續進行本教學課程之前，請嘗試執行您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="99e84-155">Before proceeding with this tutorial, try running your application.</span></span> <span data-ttu-id="99e84-156">如果您遇到 JavaScript 錯誤時，您需要調解的 JQuery 版本。</span><span class="sxs-lookup"><span data-stu-id="99e84-156">If you encounter a JavaScript error, you need to reconcile the JQuery version.</span></span> <span data-ttu-id="99e84-157">您可以預期的 JQuery 版本加入您的指令碼資料夾 （版本 1.8.2 撰寫本教學課程中的每次），或在 Site.master 指定 JQuery 檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="99e84-157">You can either add the expected version of JQuery to your Scripts folder (version 1.8.2 at time of writing this tutorial), or in Site.master specify the path to the JQuery file.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a><span data-ttu-id="99e84-158">自訂日期時間的範本，以包含 Datepicker 小工具</span><span class="sxs-lookup"><span data-stu-id="99e84-158">Customize DateTime template to include Datepicker widget</span></span>

<span data-ttu-id="99e84-159">編輯日期時間值的動態資料範本，您將加入 Datepicker 小工具。</span><span class="sxs-lookup"><span data-stu-id="99e84-159">You will add the Datepicker widget to the dynamic data template for editing a datetime value.</span></span> <span data-ttu-id="99e84-160">將小工具加入至範本，它會自動轉譯這兩種格式來新增新的學生和格線檢視中編輯學生版。</span><span class="sxs-lookup"><span data-stu-id="99e84-160">By adding the widget to the template, it is automatically rendered in both the form for adding a new student and in the grid view for editing students.</span></span> <span data-ttu-id="99e84-161">開啟**DateTime\_Edit.ascx**，並新增下列醒目提示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="99e84-161">Open **DateTime\_Edit.ascx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

<span data-ttu-id="99e84-162">在程式碼後置檔案中，您將設定的最小和最大日期 datepicker。</span><span class="sxs-lookup"><span data-stu-id="99e84-162">In the code-behind file, you will set the minimum and maximum dates for the DatePicker.</span></span> <span data-ttu-id="99e84-163">藉由設定這些值，將會導致使用者無法瀏覽到無效的日期。</span><span class="sxs-lookup"><span data-stu-id="99e84-163">By setting these values, you will prevent users from navigating to invalid dates.</span></span> <span data-ttu-id="99e84-164">您將擷取的最小和最大值**RangeAttribute**上的日期時間屬性，如果提供的其中一個。</span><span class="sxs-lookup"><span data-stu-id="99e84-164">You will retrieve the minimum and maximum values from the **RangeAttribute** on the DateTime property, if one is provided.</span></span> <span data-ttu-id="99e84-165">開啟**DateTime\_Edit.ascx.cs**，並將下列反白顯示的程式碼新增至頁面\_負載方法。</span><span class="sxs-lookup"><span data-stu-id="99e84-165">Open **DateTime\_Edit.ascx.cs**, and add the following highlighted code to the Page\_Load method.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

<span data-ttu-id="99e84-166">執行 web 應用程式，並瀏覽至 AddStudent 頁面。</span><span class="sxs-lookup"><span data-stu-id="99e84-166">Run the web application and navigate to the AddStudent page.</span></span> <span data-ttu-id="99e84-167">提供的欄位值，並請注意，當您按一下文字方塊上個註冊日期時，會顯示行事曆。</span><span class="sxs-lookup"><span data-stu-id="99e84-167">Provide values for the fields and notice that when you click on the text box for Enrollment Date, the calendar is displayed.</span></span>

![日期選擇器](integrating-jquery-ui/_static/image4.png)

<span data-ttu-id="99e84-169">挑選日期，然後按一下**插入**。</span><span class="sxs-lookup"><span data-stu-id="99e84-169">Pick a date, and click **Insert**.</span></span> <span data-ttu-id="99e84-170">RangeAttribute 會強制執行伺服器上的驗證。</span><span class="sxs-lookup"><span data-stu-id="99e84-170">The RangeAttribute enforces validation on the server.</span></span> <span data-ttu-id="99e84-171">藉由設定 Datepicker minDate; 屬性，您也適用於驗證用戶端上。</span><span class="sxs-lookup"><span data-stu-id="99e84-171">By setting the minDate property on the Datepicker, you also apply validation on the client.</span></span> <span data-ttu-id="99e84-172">行事曆不會讓使用者瀏覽至 minDate 值之前的日期。</span><span class="sxs-lookup"><span data-stu-id="99e84-172">The calendar does not let the user navigate to a date prior to the value of minDate.</span></span>

<span data-ttu-id="99e84-173">當您編輯格線檢視中的記錄時，也會顯示行事曆。</span><span class="sxs-lookup"><span data-stu-id="99e84-173">When you edit a record in the grid view, the calendar is also displayed.</span></span>

![在 GridView 的 Datepicker](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a><span data-ttu-id="99e84-175">結論</span><span class="sxs-lookup"><span data-stu-id="99e84-175">Conclusion</span></span>

<span data-ttu-id="99e84-176">在本教學課程中，您已了解如何使用模型繫結的 web 表單中納入 JQuery 小工具。</span><span class="sxs-lookup"><span data-stu-id="99e84-176">In this tutorial, you learned how to incorporate a JQuery widget into a web form that uses model binding.</span></span>

<span data-ttu-id="99e84-177">在接下來[教學課程](using-query-string-values-to-retrieve-data.md)，選取資料時，您將使用的查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="99e84-177">In the next [tutorial](using-query-string-values-to-retrieve-data.md), you will use a query string value when selecting data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="99e84-178">[上一頁](sorting-paging-and-filtering-data.md)
> [下一頁](using-query-string-values-to-retrieve-data.md)</span><span class="sxs-lookup"><span data-stu-id="99e84-178">[Previous](sorting-paging-and-filtering-data.md)
[Next](using-query-string-values-to-retrieve-data.md)</span></span>
