---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: 開始使用 Entity Framework 4.0 Database 中第一次與 ASP.NET 4 Web Form-第 8 節 |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 的 ASP.NET Web Forms 應用程式。 範例應用程式是...
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 545438a48c57bed00530f3d4cc143542d8999581
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826090"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a><span data-ttu-id="94482-104">開始使用 Entity Framework 4.0 Database 中第一次和 ASP.NET 4 Web Form 第 8 節</span><span class="sxs-lookup"><span data-stu-id="94482-104">Getting Started with Entity Framework 4.0 Database First and ASP.NET 4 Web Forms - Part 8</span></span>
====================
<span data-ttu-id="94482-105">藉由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="94482-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="94482-106">Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 4.0 和 Visual Studio 2010 的 ASP.NET Web Forms 應用程式。</span><span class="sxs-lookup"><span data-stu-id="94482-106">The Contoso University sample web application demonstrates how to create ASP.NET Web Forms applications using the Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="94482-107">教學課程系列的相關資訊，請參閱[系列的第一個教學課程](the-entity-framework-and-aspnet-getting-started-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="94482-107">For information about the tutorial series, see [the first tutorial in the series](the-entity-framework-and-aspnet-getting-started-part-1.md)</span></span>


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a><span data-ttu-id="94482-108">使用動態資料功能來格式化與驗證資料</span><span class="sxs-lookup"><span data-stu-id="94482-108">Using Dynamic Data Functionality to Format and Validate Data</span></span>

<span data-ttu-id="94482-109">在上一個教學課程中，您會實作預存程序。</span><span class="sxs-lookup"><span data-stu-id="94482-109">In the previous tutorial you implemented stored procedures.</span></span> <span data-ttu-id="94482-110">本教學課程將說明如何動態資料功能可以提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="94482-110">This tutorial will show you how Dynamic Data functionality can provide the following benefits:</span></span>

- <span data-ttu-id="94482-111">欄位會自動格式化顯示根據其資料類型。</span><span class="sxs-lookup"><span data-stu-id="94482-111">Fields are automatically formatted for display based on their data type.</span></span>
- <span data-ttu-id="94482-112">欄位會自動驗證根據其資料類型。</span><span class="sxs-lookup"><span data-stu-id="94482-112">Fields are automatically validated based on their data type.</span></span>
- <span data-ttu-id="94482-113">您可以加入自訂格式化與驗證行為的資料模型的中繼資料。</span><span class="sxs-lookup"><span data-stu-id="94482-113">You can add metadata to the data model to customize formatting and validation behavior.</span></span> <span data-ttu-id="94482-114">當您這樣做，您可以在一個位置，來新增格式化與驗證規則而 everywhere 自動套用您存取使用動態資料控制項欄位。</span><span class="sxs-lookup"><span data-stu-id="94482-114">When you do this, you can add the formatting and validation rules in just one place, and they're automatically applied everywhere you access the fields using Dynamic Data controls.</span></span>

<span data-ttu-id="94482-115">若要查看其運作方式，您要變更您用來顯示和編輯現有欄位的控制項*Students.aspx*頁面上，而且您會將格式化與驗證的中繼資料新增至的名稱和日期欄位`Student`實體類型。</span><span class="sxs-lookup"><span data-stu-id="94482-115">To see how this works, you'll change the controls you use to display and edit fields in the existing *Students.aspx* page, and you'll add formatting and validation metadata to the name and date fields of the `Student` entity type.</span></span>

<span data-ttu-id="94482-116">[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="94482-116">[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)</span></span>

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a><span data-ttu-id="94482-117">使用 DynamicField 和 DynamicControl 控制項</span><span class="sxs-lookup"><span data-stu-id="94482-117">Using DynamicField and DynamicControl Controls</span></span>

<span data-ttu-id="94482-118">開啟*Students.aspx*頁面並在`StudentsGridView`控制項取代**名稱**並**註冊日期**`TemplateField`以下列標記的項目：</span><span class="sxs-lookup"><span data-stu-id="94482-118">Open the *Students.aspx* page and in the `StudentsGridView` control replace the **Name** and **Enrollment Date** `TemplateField` elements with the following markup:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

<span data-ttu-id="94482-119">此標記會使用`DynamicControl`控制的位置`TextBox`並`Label`學生中的控制項的命名範本 欄位中，並使用`DynamicField`個註冊日期的控制項。</span><span class="sxs-lookup"><span data-stu-id="94482-119">This markup uses `DynamicControl` controls in place of `TextBox` and `Label` controls in the student name template field, and it uses a `DynamicField` control for the enrollment date.</span></span> <span data-ttu-id="94482-120">不指定任何格式字串。</span><span class="sxs-lookup"><span data-stu-id="94482-120">No format strings are specified.</span></span>

<span data-ttu-id="94482-121">新增`ValidationSummary`控制後`StudentsGridView`控制項。</span><span class="sxs-lookup"><span data-stu-id="94482-121">Add a `ValidationSummary` control after the `StudentsGridView` control.</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

<span data-ttu-id="94482-122">在`SearchGridView`控制項取代的標記**名稱**並**註冊日期**為您的資料行未`StudentsGridView`控制項，但省略`EditItemTemplate`項目。</span><span class="sxs-lookup"><span data-stu-id="94482-122">In the `SearchGridView` control replace the markup for the **Name** and **Enrollment Date** columns as you did in the `StudentsGridView` control, except omit the `EditItemTemplate` element.</span></span> <span data-ttu-id="94482-123">`Columns`項目`SearchGridView`控制項現在包含下列標記：</span><span class="sxs-lookup"><span data-stu-id="94482-123">The `Columns` element of the `SearchGridView` control now contains the following markup:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

<span data-ttu-id="94482-124">開啟*Students.aspx.cs*並新增下列`using`陳述式：</span><span class="sxs-lookup"><span data-stu-id="94482-124">Open *Students.aspx.cs* and add the following `using` statement:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

<span data-ttu-id="94482-125">新增網頁的處理常式`Init`事件：</span><span class="sxs-lookup"><span data-stu-id="94482-125">Add a handler for the page's `Init` event:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

<span data-ttu-id="94482-126">此程式碼可讓您指定格式化與驗證這些欄位的資料繫結控制項中，會提供動態資料`Student`實體。</span><span class="sxs-lookup"><span data-stu-id="94482-126">This code specifies that Dynamic Data will provide formatting and validation in these data-bound controls for fields of the `Student` entity.</span></span> <span data-ttu-id="94482-127">如果當您執行頁面時，您會收到錯誤訊息，如下列範例所示，這通常表示您忘記呼叫`EnableDynamicData`方法中的`Page_Init`:</span><span class="sxs-lookup"><span data-stu-id="94482-127">If you get an error message like the following example when you run the page, it typically means you've forgotten to call the `EnableDynamicData` method in `Page_Init`:</span></span>

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

<span data-ttu-id="94482-128">執行網頁。</span><span class="sxs-lookup"><span data-stu-id="94482-128">Run the page.</span></span>

<span data-ttu-id="94482-129">[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="94482-129">[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)</span></span>

<span data-ttu-id="94482-130">在 **註冊日期**資料行，因為屬性型別顯示的時間和日期`DateTime`。</span><span class="sxs-lookup"><span data-stu-id="94482-130">In the **Enrollment Date** column, the time is displayed along with the date because the property type is `DateTime`.</span></span> <span data-ttu-id="94482-131">您將會修正該問題稍後。</span><span class="sxs-lookup"><span data-stu-id="94482-131">You'll fix that later.</span></span>

<span data-ttu-id="94482-132">現在，請注意，動態資料就會自動提供基本的資料驗證。</span><span class="sxs-lookup"><span data-stu-id="94482-132">For now, notice that Dynamic Data automatically provides basic data validation.</span></span> <span data-ttu-id="94482-133">例如，按一下**編輯**，清除 [日期] 欄位，按一下**更新**，，您會看到，動態資料會自動將此必填的欄位因為值不是可為 null 的資料模型。</span><span class="sxs-lookup"><span data-stu-id="94482-133">For example, click **Edit**, clear the date field, click **Update**, and you see that Dynamic Data automatically makes this a required field because the value is not nullable in the data model.</span></span> <span data-ttu-id="94482-134">此頁面會顯示欄位和中的錯誤訊息之後的星號`ValidationSummary`控制項：</span><span class="sxs-lookup"><span data-stu-id="94482-134">The page displays an asterisk after the field and an error message in the `ValidationSummary` control:</span></span>

<span data-ttu-id="94482-135">[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="94482-135">[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)</span></span>

<span data-ttu-id="94482-136">您可以省略`ValidationSummary`控制項，因為您可以也將滑鼠指標停留星號，以查看錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="94482-136">You could omit the `ValidationSummary` control, because you can also hold the mouse pointer over the asterisk to see the error message:</span></span>

<span data-ttu-id="94482-137">[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="94482-137">[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)</span></span>

<span data-ttu-id="94482-138">動態資料也會驗證該輸入中的資料**註冊日期**欄位是有效的日期：</span><span class="sxs-lookup"><span data-stu-id="94482-138">Dynamic Data will also validate that data entered in the **Enrollment Date** field is a valid date:</span></span>

<span data-ttu-id="94482-139">[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="94482-139">[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)</span></span>

<span data-ttu-id="94482-140">如您所見，這是一般錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="94482-140">As you can see, this is a generic error message.</span></span> <span data-ttu-id="94482-141">下一節中，您會看到如何自訂訊息，以及驗證和格式化規則。</span><span class="sxs-lookup"><span data-stu-id="94482-141">In the next section you'll see how to customize messages as well as validation and formatting rules.</span></span>

## <a name="adding-metadata-to-the-data-model"></a><span data-ttu-id="94482-142">加入資料模型的中繼資料</span><span class="sxs-lookup"><span data-stu-id="94482-142">Adding Metadata to the Data Model</span></span>

<span data-ttu-id="94482-143">一般而言，您會想要自訂動態資料所提供的功能。</span><span class="sxs-lookup"><span data-stu-id="94482-143">Typically, you want to customize the functionality provided by Dynamic Data.</span></span> <span data-ttu-id="94482-144">例如，您可能會變更資料的顯示方式和錯誤訊息的內容。</span><span class="sxs-lookup"><span data-stu-id="94482-144">For example, you might change how data is displayed and the content of error messages.</span></span> <span data-ttu-id="94482-145">您通常也會自訂資料驗證規則，以提供更多的功能比動態資料所提供的功能會自動根據資料型別。</span><span class="sxs-lookup"><span data-stu-id="94482-145">You typically also customize data validation rules to provide more functionality than what Dynamic Data provides automatically based on data types.</span></span> <span data-ttu-id="94482-146">若要這樣做，您會建立對應至實體類型的部分類別。</span><span class="sxs-lookup"><span data-stu-id="94482-146">To do this, you create partial classes that correspond to entity types.</span></span>

<span data-ttu-id="94482-147">中**方案總管**，以滑鼠右鍵按一下**ContosoUniversity**專案，然後選取**加入參考**，並將參考加入`System.ComponentModel.DataAnnotations`。</span><span class="sxs-lookup"><span data-stu-id="94482-147">In **Solution Explorer**, right-click the **ContosoUniversity** project, select **Add Reference**, and add a reference to `System.ComponentModel.DataAnnotations`.</span></span>

<span data-ttu-id="94482-148">[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="94482-148">[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)</span></span>

<span data-ttu-id="94482-149">在  *DAL*資料夾中，建立新的類別檔案，將其命名*Student.cs*，並以下列程式碼取代範本程式碼中的。</span><span class="sxs-lookup"><span data-stu-id="94482-149">In the *DAL* folder, create a new class file, name it *Student.cs*, and replace the template code in it with the following code.</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

<span data-ttu-id="94482-150">此程式碼建立的部分類別`Student`實體。</span><span class="sxs-lookup"><span data-stu-id="94482-150">This code creates a partial class for the `Student` entity.</span></span> <span data-ttu-id="94482-151">`MetadataType`套用至這個部分類別的屬性會識別您用來指定中繼資料的類別。</span><span class="sxs-lookup"><span data-stu-id="94482-151">The `MetadataType` attribute applied to this partial class identifies the class that you're using to specify metadata.</span></span> <span data-ttu-id="94482-152">中繼資料類別可以具有任何名稱，但使用實體名稱再加上 「 中繼資料 」 是常見的作法。</span><span class="sxs-lookup"><span data-stu-id="94482-152">The metadata class can have any name, but using the entity name plus "Metadata" is a common practice.</span></span>

<span data-ttu-id="94482-153">套用至中繼資料類別中屬性的屬性會指定格式、 驗證、 規則及錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="94482-153">The attributes applied to properties in the metadata class specify formatting, validation, rules, and error messages.</span></span> <span data-ttu-id="94482-154">如下所示的屬性將會有下列結果：</span><span class="sxs-lookup"><span data-stu-id="94482-154">The attributes shown here will have the following results:</span></span>

- <span data-ttu-id="94482-155">`EnrollmentDate` 將會顯示為日期 （不含時間）。</span><span class="sxs-lookup"><span data-stu-id="94482-155">`EnrollmentDate` will display as a date (without a time).</span></span>
- <span data-ttu-id="94482-156">這兩個名稱欄位必須是 25 個字元，或小於長度和自訂錯誤訊息中提供。</span><span class="sxs-lookup"><span data-stu-id="94482-156">Both name fields must be 25 characters or less in length, and a custom error message is provided.</span></span>
- <span data-ttu-id="94482-157">這兩個名稱欄位是必要的並提供自訂的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="94482-157">Both name fields are required, and a custom error message is provided.</span></span>

<span data-ttu-id="94482-158">執行*Students.aspx*頁面，然後您會看到，日期現在會顯示不含時間：</span><span class="sxs-lookup"><span data-stu-id="94482-158">Run the *Students.aspx* page again, and you see that the dates are now displayed without times:</span></span>

<span data-ttu-id="94482-159">[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="94482-159">[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)</span></span>

<span data-ttu-id="94482-160">編輯資料列，然後再次嘗試清除 [名稱] 欄位中的值。</span><span class="sxs-lookup"><span data-stu-id="94482-160">Edit a row and try to clear the values in the name fields.</span></span> <span data-ttu-id="94482-161">只要您再按一下 [保留] 欄位中，會顯示指出欄位錯誤星號**更新**。</span><span class="sxs-lookup"><span data-stu-id="94482-161">The asterisks indicating field errors appear as soon as you leave a field, before you click **Update**.</span></span> <span data-ttu-id="94482-162">當您按一下 **更新**，此頁面會顯示您所指定的錯誤訊息文字。</span><span class="sxs-lookup"><span data-stu-id="94482-162">When you click **Update**, the page displays the error message text you specified.</span></span>

<span data-ttu-id="94482-163">[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="94482-163">[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)</span></span>

<span data-ttu-id="94482-164">嘗試輸入超過 25 個字元的名稱，請按一下**更新**，和此頁面會顯示您所指定的錯誤訊息文字。</span><span class="sxs-lookup"><span data-stu-id="94482-164">Try to enter names that are longer than 25 characters, click **Update**, and the page displays the error message text you specified.</span></span>

<span data-ttu-id="94482-165">[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="94482-165">[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)</span></span>

<span data-ttu-id="94482-166">既然您已設定的資料模型中繼資料中的這些格式化與驗證規則，規則將會自動套用可顯示或允許這些欄位中，變更每個頁面上，只要您使用`DynamicControl`或`DynamicField`控制項。</span><span class="sxs-lookup"><span data-stu-id="94482-166">Now that you've set up these formatting and validation rules in the data model metadata, the rules will automatically be applied on every page that displays or allows changes to these fields, so long as you use `DynamicControl` or `DynamicField` controls.</span></span> <span data-ttu-id="94482-167">這會減少的多餘的程式碼撰寫，您必須讓程式設計和測試更為容易，並且確保資料格式化與驗證是整個應用程式一致。</span><span class="sxs-lookup"><span data-stu-id="94482-167">This reduces the amount of redundant code you have to write, which makes programming and testing easier, and it ensures that data formatting and validation are consistent throughout an application.</span></span>

## <a name="more-information"></a><span data-ttu-id="94482-168">更多資訊</span><span class="sxs-lookup"><span data-stu-id="94482-168">More Information</span></span>

<span data-ttu-id="94482-169">以上總結這一系列的 Entity Framework 使用者入門教學課程。</span><span class="sxs-lookup"><span data-stu-id="94482-169">This concludes this series of tutorials on Getting Started with the Entity Framework.</span></span> <span data-ttu-id="94482-170">取得可協助您了解如何使用 Entity Framework 的更多資源，繼續進行[下一步 的 Entity Framework 教學課程系列的第一個教學課程](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)或瀏覽下列網站：</span><span class="sxs-lookup"><span data-stu-id="94482-170">For more resources to help you learn how to use the Entity Framework, continue with [the first tutorial in the next Entity Framework tutorial series](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) or visit the following sites:</span></span>

- [<span data-ttu-id="94482-171">Entity Framework 常見問題集</span><span class="sxs-lookup"><span data-stu-id="94482-171">Entity Framework FAQ</span></span>](http://www.ef-faq.org/introduction.html)
- [<span data-ttu-id="94482-172">Entity Framework 小組部落格</span><span class="sxs-lookup"><span data-stu-id="94482-172">The Entity Framework Team Blog</span></span>](https://blogs.msdn.com/b/adonet/)
- [<span data-ttu-id="94482-173">MSDN Library 中的 entity Framework</span><span class="sxs-lookup"><span data-stu-id="94482-173">Entity Framework in the MSDN Library</span></span>](https://msdn.microsoft.com/library/bb399572.aspx)
- [<span data-ttu-id="94482-174">在 MSDN 資料開發人員中心中的 entity Framework</span><span class="sxs-lookup"><span data-stu-id="94482-174">Entity Framework in the MSDN Data Developer Center</span></span>](https://msdn.microsoft.com/data/ef.aspx)
- [<span data-ttu-id="94482-175">MSDN Library 中的 EntityDataSource Web 伺服器控制項概觀</span><span class="sxs-lookup"><span data-stu-id="94482-175">EntityDataSource Web Server Control Overview in the MSDN Library</span></span>](https://msdn.microsoft.com/library/cc488502.aspx)
- [<span data-ttu-id="94482-176">EntityDataSource 控制項 MSDN Library 中的 API 參考</span><span class="sxs-lookup"><span data-stu-id="94482-176">EntityDataSource control API reference in the MSDN Library</span></span>](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [<span data-ttu-id="94482-177">MSDN 上的 entity Framework 論壇</span><span class="sxs-lookup"><span data-stu-id="94482-177">Entity Framework Forums on MSDN</span></span>](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [<span data-ttu-id="94482-178">Julie Lerman 的部落格</span><span class="sxs-lookup"><span data-stu-id="94482-178">Julie Lerman's blog</span></span>](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [<span data-ttu-id="94482-179">上一步</span><span class="sxs-lookup"><span data-stu-id="94482-179">Previous</span></span>](the-entity-framework-and-aspnet-getting-started-part-7.md)
