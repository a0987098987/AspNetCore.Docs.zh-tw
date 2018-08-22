---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: 使用 HTML5 與 jQuery UI Datepicker 快顯行事曆搭配 ASP.NET MVC-第 2 部分 |Microsoft Docs
author: Rick-Anderson
description: 本教學課程將教導您如何使用編輯器範本、 顯示範本與 jQuery UI datepicker 快顯行事曆，ASP.NET MV 中的基本概念...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 572185507016afc26fc2e06ede55361b2daed277
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830534"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a><span data-ttu-id="590ec-103">使用 HTML5 與 jQuery UI Datepicker 快顯行事曆搭配 ASP.NET MVC-第 2 部分</span><span class="sxs-lookup"><span data-stu-id="590ec-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 2</span></span>
====================
<span data-ttu-id="590ec-104">藉由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="590ec-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="590ec-105">本教學課程將教導您如何使用編輯器範本、 顯示範本與 jQuery UI datepicker 快顯行事曆，ASP.NET MVC Web 應用程式中的基本概念。</span><span class="sxs-lookup"><span data-stu-id="590ec-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>


## <a name="adding-an-automatic-datetime-template"></a><span data-ttu-id="590ec-106">新增自動的 DateTime 範本</span><span class="sxs-lookup"><span data-stu-id="590ec-106">Adding an Automatic DateTime Template</span></span>

<span data-ttu-id="590ec-107">在本教學課程的第一個部分，您會看到如何將屬性加入至模型，以明確指定的格式，以及如何明確地指定用來呈現模型的範本。</span><span class="sxs-lookup"><span data-stu-id="590ec-107">In the first part of this tutorial, you saw how you can add attributes to the model to explicitly specify formatting, and how you can explicitly specify the template that's used to render the model.</span></span> <span data-ttu-id="590ec-108">例如， [Displayformat.{0](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)屬性，在下列程式碼明確指定的格式`ReleaseDate`屬性。</span><span class="sxs-lookup"><span data-stu-id="590ec-108">For example, the [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribute in the following code explicity specifies the formatting for the `ReleaseDate` property.</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

<span data-ttu-id="590ec-109">在下列範例中， [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)屬性，使用`Date`列舉型別，可讓您指定日期範本，應該用來呈現模型。</span><span class="sxs-lookup"><span data-stu-id="590ec-109">In the following example, the [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute, using the `Date` enumeration, specifies that the date template should be used to render the model.</span></span> <span data-ttu-id="590ec-110">如果您的專案中沒有任何日期範本，則會使用內建日期範本。</span><span class="sxs-lookup"><span data-stu-id="590ec-110">If there's no date template in your project, the built-in date template is used.</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

<span data-ttu-id="590ec-111">不過，ASP。MVC 可以執行類型比對使用慣例優於組態，藉由尋找符合型別名稱的範本。</span><span class="sxs-lookup"><span data-stu-id="590ec-111">However, ASP.MVC can perform type matching using convention-over-configuration, by looking for a template that matches the name of a type.</span></span> <span data-ttu-id="590ec-112">這可讓您建立的範本，而不需完全使用任何屬性或程式碼，自動格式化資料。</span><span class="sxs-lookup"><span data-stu-id="590ec-112">This lets you create a template that automatically formats data without using any attributes or code at all.</span></span> <span data-ttu-id="590ec-113">此部分的教學課程中，您將建立的範本，就會自動套用至模型屬性的型別[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)。</span><span class="sxs-lookup"><span data-stu-id="590ec-113">For this part of the tutorial, you'll create a template that's automatically applied to model properties of type [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx).</span></span> <span data-ttu-id="590ec-114">您不需要使用屬性或其他設定來指定範本，應該用來呈現所有模型屬性的型別[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)。</span><span class="sxs-lookup"><span data-stu-id="590ec-114">You won't need to use an attribute or other configuration to specify that the template should be used to render all model properties of type [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx).</span></span>

<span data-ttu-id="590ec-115">您也將了解自訂個別屬性或甚至是個別欄位的顯示方式。</span><span class="sxs-lookup"><span data-stu-id="590ec-115">You'll also learn a way to customize the display of individual properties or even individual fields.</span></span>

<span data-ttu-id="590ec-116">若要開始，讓我們移除現有的格式資訊，並在應用程式中顯示完整的日期。</span><span class="sxs-lookup"><span data-stu-id="590ec-116">To begin, let's remove existing formatting information and display full dates in the application.</span></span>

<span data-ttu-id="590ec-117">開啟*Movie.cs*檔案，並標記為註解`DataType`屬性`ReleaseDate`屬性：</span><span class="sxs-lookup"><span data-stu-id="590ec-117">Open the *Movie.cs* file and comment out the `DataType` attribute on the `ReleaseDate` property:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

<span data-ttu-id="590ec-118">按 CTRL+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="590ec-118">Press CTRL+F5 to run the application.</span></span>

<span data-ttu-id="590ec-119">請注意，`ReleaseDate`屬性現在會顯示日期和時間，因為這是預設值時不提供的任何格式的資訊。</span><span class="sxs-lookup"><span data-stu-id="590ec-119">Notice that the `ReleaseDate` property now displays both the date and time, because that's the default when no formatting information is provided.</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a><span data-ttu-id="590ec-120">新增的 CSS 樣式，用於測試新的範本</span><span class="sxs-lookup"><span data-stu-id="590ec-120">Adding CSS Styles for Testing New Templates</span></span>

<span data-ttu-id="590ec-121">建立用於格式化日期的範本之前，您將新增一些您可以套用至新範本的 CSS 樣式規則。</span><span class="sxs-lookup"><span data-stu-id="590ec-121">Before you create a template for formatting dates, you'll add a few CSS style rules that you can apply to the new templates.</span></span> <span data-ttu-id="590ec-122">這些功能會協助您確認呈現的網頁使用新的範本。</span><span class="sxs-lookup"><span data-stu-id="590ec-122">These will help you verify that the rendered page is using the new template.</span></span>

<span data-ttu-id="590ec-123">開啟*Content\Site.cs*s 檔案，並將下列 CSS 規則新增至檔案最下方：</span><span class="sxs-lookup"><span data-stu-id="590ec-123">Open the *Content\Site.cs*s file and add the following CSS rules to the bottom of the file:</span></span>

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a><span data-ttu-id="590ec-124">新增日期時間的顯示範本</span><span class="sxs-lookup"><span data-stu-id="590ec-124">Adding DateTime Display Templates</span></span>

<span data-ttu-id="590ec-125">現在您可以建立新的範本。</span><span class="sxs-lookup"><span data-stu-id="590ec-125">Now you can create the new template.</span></span> <span data-ttu-id="590ec-126">在  *Views\Movies*資料夾中，建立*DisplayTemplates*資料夾。</span><span class="sxs-lookup"><span data-stu-id="590ec-126">In the *Views\Movies* folder, create a *DisplayTemplates* folder.</span></span>

<span data-ttu-id="590ec-127">在  *Views\Shared*資料夾中，建立*DisplayTemplates*資料夾以及*EditorTemplates*資料夾。</span><span class="sxs-lookup"><span data-stu-id="590ec-127">In the *Views\Shared* folder, create a *DisplayTemplates* folder and an *EditorTemplates* folder.</span></span>

<span data-ttu-id="590ec-128">中的顯示範本*Views\Shared\DisplayTemplates*資料夾可供所有控制站。</span><span class="sxs-lookup"><span data-stu-id="590ec-128">The display templates in the *Views\Shared\DisplayTemplates* folder will be used by all controllers.</span></span> <span data-ttu-id="590ec-129">中的顯示範本*Views\Movie\DisplayTemplates*資料夾將僅供`Movie`控制站。</span><span class="sxs-lookup"><span data-stu-id="590ec-129">The display templates in the *Views\Movie\DisplayTemplates* folder will be used only by the `Movie` controller.</span></span> <span data-ttu-id="590ec-130">(如果具有相同名稱的範本會出現在這兩個資料夾中的範本*Views\Movie\DisplayTemplates*資料夾 — 也就是更特定的範本，會針對檢視所傳回的優先`Movie`控制器。)</span><span class="sxs-lookup"><span data-stu-id="590ec-130">(If a template with the same name appears in both folders, the template in the *Views\Movie\DisplayTemplates* folder — that is, the more specific template — takes precedence for views returned by the `Movie` controller.)</span></span>

<span data-ttu-id="590ec-131">在 [**方案總管] 中**，展開*檢視*資料夾中，展開*共用*資料夾，然後再以滑鼠右鍵按一下*Views\Shared\DisplayTemplates*資料夾。</span><span class="sxs-lookup"><span data-stu-id="590ec-131">In **Solution Explorer**, expand the *Views* folder, expand the *Shared* folder, and then right-click the *Views\Shared\DisplayTemplates* folder.</span></span>

<span data-ttu-id="590ec-132">按一下 **新增**，然後按一下**檢視**。</span><span class="sxs-lookup"><span data-stu-id="590ec-132">Click **Add** and then click **View**.</span></span> <span data-ttu-id="590ec-133">**加入檢視**對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="590ec-133">The **Add View** dialog box is displayed.</span></span>

<span data-ttu-id="590ec-134">在 **檢視表名稱**方塊中，輸入`DateTime`。</span><span class="sxs-lookup"><span data-stu-id="590ec-134">In the **View name** box, type `DateTime`.</span></span> <span data-ttu-id="590ec-135">（您必須使用此名稱以符合類型的名稱）。</span><span class="sxs-lookup"><span data-stu-id="590ec-135">(You must use this name in order to match the name of the type.)</span></span>

<span data-ttu-id="590ec-136">選取 **建立成局部檢視**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="590ec-136">Select the **Create as a partial view** check box.</span></span> <span data-ttu-id="590ec-137">請確定**使用版面配置頁或主版頁面**並**建立強型別檢視**未選取核取方塊。</span><span class="sxs-lookup"><span data-stu-id="590ec-137">Make sure that the **Use a layout or master page** and **Create a strongly-typed view** check boxes are not selected.</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

<span data-ttu-id="590ec-138">按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="590ec-138">Click **Add**.</span></span> <span data-ttu-id="590ec-139">A *DateTime.cshtml*中建立範本*Views\Shared\DisplayTemplates*。</span><span class="sxs-lookup"><span data-stu-id="590ec-139">A *DateTime.cshtml* template is created in the *Views\Shared\DisplayTemplates*.</span></span>

<span data-ttu-id="590ec-140">下圖顯示*檢視*中的資料夾**方案總管**之後`DateTime`建立顯示和編輯程式的範本。</span><span class="sxs-lookup"><span data-stu-id="590ec-140">The following image shows the *Views* folder in **Solution Explorer** after the `DateTime` display and editor templates are created.</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

<span data-ttu-id="590ec-141">開啟*Views\Shared\DisplayTemplates\DateTime.cshtml*檔案，並新增下列標記中，它會使用[String.Format](https://msdn.microsoft.com/library/system.string.format.aspx)可設定為不包含時間的日期格式之屬性的方法。</span><span class="sxs-lookup"><span data-stu-id="590ec-141">Open the *Views\Shared\DisplayTemplates\DateTime.cshtml* file and add the following markup, which uses the [String.Format](https://msdn.microsoft.com/library/system.string.format.aspx) method to format the property as a date without the time.</span></span> <span data-ttu-id="590ec-142">(`{0:d}`格式指定簡短日期格式。)</span><span class="sxs-lookup"><span data-stu-id="590ec-142">(The `{0:d}` format specifies short date format.)</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

<span data-ttu-id="590ec-143">重複此步驟，建立`DateTime`中的範本*Views\Movie\DisplayTemplates*資料夾。</span><span class="sxs-lookup"><span data-stu-id="590ec-143">Repeat this step to create a `DateTime` template in the *Views\Movie\DisplayTemplates* folder.</span></span> <span data-ttu-id="590ec-144">使用中的下列程式碼*Views\Movie\DisplayTemplates\DateTime.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="590ec-144">Use the following code in the *Views\Movie\DisplayTemplates\DateTime.cshtml* file.</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

<span data-ttu-id="590ec-145">`loud-1` CSS 類別會將日期是以紅色粗體文字顯示。</span><span class="sxs-lookup"><span data-stu-id="590ec-145">The `loud-1` CSS class causes the date to be display in bold red text.</span></span> <span data-ttu-id="590ec-146">您加入`loud-1`就如同暫時的措施，讓您可以輕鬆地查看使用這個特殊範本中時的 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="590ec-146">You added the `loud-1` CSS class just as a temporary measure so you can easily see when this particular template is being used.</span></span>

<span data-ttu-id="590ec-147">您已完成建立和自訂的 ASP.NET 用來顯示日期的範本。</span><span class="sxs-lookup"><span data-stu-id="590ec-147">What you've done is created and customized templates that ASP.NET will use to display dates.</span></span> <span data-ttu-id="590ec-148">更多一般範本 (在*Views\Shared\DisplayTemplates*資料夾) 會顯示簡單的簡短日期。</span><span class="sxs-lookup"><span data-stu-id="590ec-148">The more general template (in the *Views\Shared\DisplayTemplates* folder) displays a simple short date.</span></span> <span data-ttu-id="590ec-149">範本特別`Movie`控制器 (在*Views\Movies\DisplayTemplates*資料夾) 會也格式化為簡短日期顯示為紅色粗體文字。</span><span class="sxs-lookup"><span data-stu-id="590ec-149">The template that's specifically for the `Movie` controller (in the *Views\Movies\DisplayTemplates* folder) displays a short date that's also formatted as bold red text.</span></span>

<span data-ttu-id="590ec-150">按 CTRL+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="590ec-150">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="590ec-151">瀏覽器呈現應用程式的 [索引] 檢視。</span><span class="sxs-lookup"><span data-stu-id="590ec-151">The browser renders the Index view for the application.</span></span>

<span data-ttu-id="590ec-152">`ReleaseDate`現在以紅色粗體字不包含的時間顯示日期的屬性。這可協助您確認`DateTime`中的樣板化 helper *Views\Movies\DisplayTemplates*透過選取資料夾`DateTime`共用資料夾中的樣板化 helper (*Views\Shared\DisplayTemplates*)。</span><span class="sxs-lookup"><span data-stu-id="590ec-152">The `ReleaseDate` property now displays the date in a bold red font without the time.This helps you confirm that the `DateTime` templated helper in the *Views\Movies\DisplayTemplates* folder is selected over the `DateTime` templated helper in the shared folder (*Views\Shared\DisplayTemplates*).</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

<span data-ttu-id="590ec-153">現在可以重新命名*Views\Movies\DisplayTemplates\DateTime.cshtml*的檔案*Views\Movies\DisplayTemplates\LoudDateTime.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="590ec-153">Now rename the *Views\Movies\DisplayTemplates\DateTime.cshtml* file to *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.</span></span>

<span data-ttu-id="590ec-154">按 CTRL+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="590ec-154">Press CTRL+F5 to run the application.</span></span>

<span data-ttu-id="590ec-155">這次`ReleaseDate`屬性來顯示日期不包含的時間而紅色粗體字。</span><span class="sxs-lookup"><span data-stu-id="590ec-155">This time the `ReleaseDate` property displays a date without the time and without the bold red font.</span></span> <span data-ttu-id="590ec-156">這說明了具有資料的名稱的範本類型 (在此情況下`DateTime`) 自動用來顯示該類型的所有模型屬性。</span><span class="sxs-lookup"><span data-stu-id="590ec-156">This illustrates that a template that has the name of the data type (in this case `DateTime`) is automatically used to display all model properties of that type.</span></span> <span data-ttu-id="590ec-157">重新命名之後*DateTime.cshtml*的檔案*LoudDateTime.cshtml*，ASP.NET 導致找不到範本中的*Views\Movies\DisplayTemplates*資料夾，所以使用*DateTime.cshtml*範本，從 * Views\Movies\Shared\*資料夾。</span><span class="sxs-lookup"><span data-stu-id="590ec-157">After you renamed the *DateTime.cshtml* file to *LoudDateTime.cshtml*, ASP.NET no longer found a template in the *Views\Movies\DisplayTemplates* folder, so it used the *DateTime.cshtml* template from the *Views\Movies\Shared\* folder.</span></span>

<span data-ttu-id="590ec-158">（範本比對是區分大小寫，因此您無法建立範本檔案名稱具有任何大小寫。</span><span class="sxs-lookup"><span data-stu-id="590ec-158">(The template matching is case insensitive, so you could have created the template file name with any casing.</span></span> <span data-ttu-id="590ec-159">例如， *DATETIME.chstml、 datetime.cshtml*，並*DaTeTiMe.cshtml*所有符合`DateTime`型別。)</span><span class="sxs-lookup"><span data-stu-id="590ec-159">For example, *DATETIME.chstml, datetime.cshtml*, and *DaTeTiMe.cshtml* would all match the `DateTime` type.)</span></span>

<span data-ttu-id="590ec-160">若要檢閱： 到目前為止，`ReleaseDate`欄位會顯示使用*Views\Movies\DisplayTemplates\DateTime.cshtml*範本，使用簡短日期格式顯示資料，但否則加入沒有特殊的格式。</span><span class="sxs-lookup"><span data-stu-id="590ec-160">To review: at this point, the `ReleaseDate` field is being displayed using the *Views\Movies\DisplayTemplates\DateTime.cshtml* template, which displays the data using a short date format, but otherwise adds no special format.</span></span>

### <a name="using-uihint-to-specify-a-display-template"></a><span data-ttu-id="590ec-161">使用 UIHint 指定顯示範本</span><span class="sxs-lookup"><span data-stu-id="590ec-161">Using UIHint to Specify a Display Template</span></span>

<span data-ttu-id="590ec-162">如果您的 web 應用程式有許多`DateTime`欄位和您想要僅限日期的格式，顯示它們的全部或大部分的預設*DateTime.cshtml*範本是不錯的方法。</span><span class="sxs-lookup"><span data-stu-id="590ec-162">If your web application has many `DateTime` fields and by default you want to display all or most of them in date-only format, the *DateTime.cshtml* template is a good approach.</span></span> <span data-ttu-id="590ec-163">但是，如果您有幾個日期您想要顯示完整日期和時間的地方嗎？</span><span class="sxs-lookup"><span data-stu-id="590ec-163">But what if you have a few dates where you want to display the full date and time?</span></span> <span data-ttu-id="590ec-164">沒問題。</span><span class="sxs-lookup"><span data-stu-id="590ec-164">No problem.</span></span> <span data-ttu-id="590ec-165">您可以建立額外的範本，並使用[UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)屬性來指定格式的完整日期和時間。</span><span class="sxs-lookup"><span data-stu-id="590ec-165">You can create an additional template and use the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute to specify formatting for the full date and time.</span></span> <span data-ttu-id="590ec-166">您接著可以選擇性地套用該範本。</span><span class="sxs-lookup"><span data-stu-id="590ec-166">You can then selectively apply that template.</span></span> <span data-ttu-id="590ec-167">您可以使用[UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)屬性在模型層級，或者您可以指定在檢視內的範本。</span><span class="sxs-lookup"><span data-stu-id="590ec-167">You can use the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute at the model level or you can specify the template inside a view.</span></span> <span data-ttu-id="590ec-168">在本節中，您會看到如何使用`UIHint`屬性選擇性地變更某些情況下的日期時間欄位的格式。</span><span class="sxs-lookup"><span data-stu-id="590ec-168">In this section, you'll see how to use the `UIHint` attribute to selectively change the formatting for some instances of date-time fields.</span></span>

<span data-ttu-id="590ec-169">開啟*Views\Movies\DisplayTemplates\LoudDateTime.cshtml*檔案，並以下列內容取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="590ec-169">Open the *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* file and replace the existing code with the following:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

<span data-ttu-id="590ec-170">這樣會導致要顯示的完整日期和時間，並新增讓文字，綠色和大型的 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="590ec-170">This causes the full date and time to be displayed and adds the CSS class that makes the text green and large.</span></span>

<span data-ttu-id="590ec-171">開啟*Movie.cs*檔案，並新增[UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)屬性設定為`ReleaseDate`屬性，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="590ec-171">Open the *Movie.cs* file and add the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute to the `ReleaseDate` property, as shown in the following example:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

<span data-ttu-id="590ec-172">這會告訴 ASP.NET MVC，它會顯示當`ReleaseDate`屬性 (具體而言，並不是任何`DateTime`物件)，它應該使用*LoudDateTime.cshtml*範本。</span><span class="sxs-lookup"><span data-stu-id="590ec-172">This tells ASP.NET MVC that when it displays the `ReleaseDate` property (specifically, and not just any `DateTime` object), it should use the *LoudDateTime.cshtml* template.</span></span>

<span data-ttu-id="590ec-173">按 CTRL+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="590ec-173">Press CTRL+F5 to run the application.</span></span>

<span data-ttu-id="590ec-174">請注意，`ReleaseDate`屬性現在會顯示日期和時間的大型的綠色字型。</span><span class="sxs-lookup"><span data-stu-id="590ec-174">Notice that the `ReleaseDate` property now displays the date and time in a large green font.</span></span>

<span data-ttu-id="590ec-175">返回`UIHint`屬性中*Movie.cs*檔案，並註解化，因此*LoudDateTime.cshtml*不會使用範本。</span><span class="sxs-lookup"><span data-stu-id="590ec-175">Return to the `UIHint` attribute in the *Movie.cs* file and comment it out so the *LoudDateTime.cshtml* template won't be used.</span></span> <span data-ttu-id="590ec-176">再次執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="590ec-176">Run the application again.</span></span> <span data-ttu-id="590ec-177">發行日期時，不會顯示大型和綠色。</span><span class="sxs-lookup"><span data-stu-id="590ec-177">The release date is not displayed large and green.</span></span> <span data-ttu-id="590ec-178">這會驗證*Views\Shared\DisplayTemplates\DateTime.cshtml*範本會在 索引 和 詳細資料檢視。</span><span class="sxs-lookup"><span data-stu-id="590ec-178">This verifies that the *Views\Shared\DisplayTemplates\DateTime.cshtml* template is used in the Index and Details views.</span></span>

<span data-ttu-id="590ec-179">如先前所述，您也可以套用範本，以在檢視中，它可讓您將範本套用到個別的執行個體的一些資料。</span><span class="sxs-lookup"><span data-stu-id="590ec-179">As mentioned earlier, you can also apply a template in a view, which lets you apply the template to an individual instance of some data.</span></span> <span data-ttu-id="590ec-180">開啟*Views\Movies\Details.cshtml*檢視。</span><span class="sxs-lookup"><span data-stu-id="590ec-180">Open the *Views\Movies\Details.cshtml* view.</span></span> <span data-ttu-id="590ec-181">新增`"LoudDateTime"`做為第二個參數的[Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx)呼叫`ReleaseDate`欄位。</span><span class="sxs-lookup"><span data-stu-id="590ec-181">Add `"LoudDateTime"` as the second parameter of the [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) call for the `ReleaseDate` field.</span></span> <span data-ttu-id="590ec-182">完成的程式碼如下所示：</span><span class="sxs-lookup"><span data-stu-id="590ec-182">The completed code looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

<span data-ttu-id="590ec-183">這會指定`LoudDateTime`範本應該用來顯示模型屬性，不論何種屬性套用至模型。</span><span class="sxs-lookup"><span data-stu-id="590ec-183">This specifies that the `LoudDateTime` template should be used to display the model property, regardless of what attributes are applied to the model.</span></span>

<span data-ttu-id="590ec-184">按 CTRL+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="590ec-184">Press CTRL+F5 to run the application.</span></span>

<span data-ttu-id="590ec-185">驗證是否使用影片索引頁*Views\Shared\DisplayTemplates\DateTime.cshtml*範本 （紅色粗體） 和*Movie\Details*網頁使用*Views\Movies\DisplayTemplates\LoudDateTime.cshtml*範本 （大型和綠色）。</span><span class="sxs-lookup"><span data-stu-id="590ec-185">Verify that the movie index page is using the *Views\Shared\DisplayTemplates\DateTime.cshtml* template (red bold) and the *Movie\Details* page is using the *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* template (large and green).</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

<span data-ttu-id="590ec-186">在下一步 區段中，您將建立複雜類型的範本。</span><span class="sxs-lookup"><span data-stu-id="590ec-186">In the next section, you'll create a template for a complex type.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="590ec-187">[上一頁](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [下一頁](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="590ec-187">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)</span></span>
