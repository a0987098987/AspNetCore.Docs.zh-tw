---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: 使用 HTML5 與 jQuery UI Datepicker 快顯行事曆搭配 ASP.NET MVC-第 3 部分 |Microsoft Docs
author: Rick-Anderson
description: 本教學課程將教導您如何使用編輯器範本、 顯示範本與 jQuery UI datepicker 快顯行事曆，ASP.NET MV 中的基本概念...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: 184d93323ac6f2cb7f14d32b401cf6a400eb79a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832028"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a><span data-ttu-id="14872-103">ASP.NET MVC： 第 3 部分中使用 HTML5 與 jQuery UI Datepicker 快顯行事曆</span><span class="sxs-lookup"><span data-stu-id="14872-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 3</span></span>
====================
<span data-ttu-id="14872-104">藉由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="14872-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="14872-105">本教學課程將教導您如何使用編輯器範本、 顯示範本與 jQuery UI datepicker 快顯行事曆，ASP.NET MVC Web 應用程式中的基本概念。</span><span class="sxs-lookup"><span data-stu-id="14872-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>


## <a name="working-with-complex-types"></a><span data-ttu-id="14872-106">使用複雜型別</span><span class="sxs-lookup"><span data-stu-id="14872-106">Working with Complex Types</span></span>

<span data-ttu-id="14872-107">在本節中，您會建立位址類別及了解如何建立樣板來顯示它。</span><span class="sxs-lookup"><span data-stu-id="14872-107">In this section you'll create an address class and learn how to create a template to display it.</span></span>

<span data-ttu-id="14872-108">在 *模型*資料夾中，建立新的類別檔案，名為*Person.cs*會在其中放置兩種類型：`Person`類別和`Address`類別。</span><span class="sxs-lookup"><span data-stu-id="14872-108">In the *Models* folder, create a new class file named *Person.cs* where you'll put two types: a `Person` class and an `Address` class.</span></span> <span data-ttu-id="14872-109">`Person`類別將包含屬性的類型為`Address`。</span><span class="sxs-lookup"><span data-stu-id="14872-109">The `Person` class will contain a property that's typed as `Address`.</span></span> <span data-ttu-id="14872-110">`Address`類型是複雜類型，這表示它不其中一個內建的型別，例如`int`， `string`，或`double`。</span><span class="sxs-lookup"><span data-stu-id="14872-110">The `Address` type is a complex type, meaning it's not one of the built-in types like `int`, `string`, or `double`.</span></span> <span data-ttu-id="14872-111">相反地，它有數個屬性。</span><span class="sxs-lookup"><span data-stu-id="14872-111">Instead, it has several properties.</span></span> <span data-ttu-id="14872-112">新的類別的程式碼看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="14872-112">The code for the new classes looks like this:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

<span data-ttu-id="14872-113">在 `Movie`控制站，新增下列`PersonDetail`動作以顯示 person 執行個體：</span><span class="sxs-lookup"><span data-stu-id="14872-113">In the `Movie` controller, add the following `PersonDetail` action to display a person instance:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

<span data-ttu-id="14872-114">然後新增下列程式碼`Movie`控制器來填入`Person`具有一些範例資料的模型：</span><span class="sxs-lookup"><span data-stu-id="14872-114">Then add the following code to the `Movie` controller to populate the `Person` model with some sample data:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

<span data-ttu-id="14872-115">開啟*Views\Movies\PersonDetail.cshtml*檔案，並新增下列標記`PersonDetail`檢視。</span><span class="sxs-lookup"><span data-stu-id="14872-115">Open the *Views\Movies\PersonDetail.cshtml* file and add the following markup for the `PersonDetail` view.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

<span data-ttu-id="14872-116">按 Ctrl + F5 執行應用程式，並瀏覽到*電影/PersonDetail*。</span><span class="sxs-lookup"><span data-stu-id="14872-116">Press Ctrl+F5 to run the application and navigate to *Movies/PersonDetail*.</span></span>

<span data-ttu-id="14872-117">`PersonDetail`檢視不包含`Address`複雜型別，您可以看到此螢幕擷取畫面中。</span><span class="sxs-lookup"><span data-stu-id="14872-117">The `PersonDetail` view doesn't contain the `Address` complex type, as you can see in this screenshot.</span></span> <span data-ttu-id="14872-118">（沒有位址會顯示）。</span><span class="sxs-lookup"><span data-stu-id="14872-118">(No address is shown.)</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

<span data-ttu-id="14872-119">`Address`模型並不會因為它是複雜型別。</span><span class="sxs-lookup"><span data-stu-id="14872-119">The `Address` model data is not displayed because it's a complex type.</span></span> <span data-ttu-id="14872-120">若要顯示的地址資訊，請開啟*Views\Movies\PersonDetail.cshtml*檔案一次，並新增下列標記。</span><span class="sxs-lookup"><span data-stu-id="14872-120">To display the address information, open the *Views\Movies\PersonDetail.cshtml* file again and add the following markup.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

<span data-ttu-id="14872-121">完整標記`PersonDetail`現在檢視看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="14872-121">The complete markup for the `PersonDetail` now view looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

<span data-ttu-id="14872-122">再次執行應用程式，並顯示`PersonDetail`檢視。</span><span class="sxs-lookup"><span data-stu-id="14872-122">Run the application again and display the `PersonDetail` view.</span></span> <span data-ttu-id="14872-123">現在會顯示的位址資訊：</span><span class="sxs-lookup"><span data-stu-id="14872-123">The address information is now displayed:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a><span data-ttu-id="14872-124">建立範本的複雜型別</span><span class="sxs-lookup"><span data-stu-id="14872-124">Creating a Template for a Complex Type</span></span>

<span data-ttu-id="14872-125">在本節中，您將建立的範本，將會用來呈現`Address`複雜型別。</span><span class="sxs-lookup"><span data-stu-id="14872-125">In this section you'll create a template that will be used to render the `Address` complex type.</span></span> <span data-ttu-id="14872-126">當您建立的範本`Address`型別，ASP.NET MVC 可以自動利用它來設定應用程式中的任何位置的位址模型的格式。</span><span class="sxs-lookup"><span data-stu-id="14872-126">When you create a template for the `Address` type, ASP.NET MVC can automatically use it to format an address model anywhere in the application.</span></span> <span data-ttu-id="14872-127">這可讓您控制呈現的方式`Address`從應用程式中的一個位置的型別。</span><span class="sxs-lookup"><span data-stu-id="14872-127">This gives you a way to control the rendering of the `Address` type from just one place in the application.</span></span>

<span data-ttu-id="14872-128">在  *Views\Shared\DisplayTemplates*資料夾中，建立名為強型別部分檢視**地址**:</span><span class="sxs-lookup"><span data-stu-id="14872-128">In the *Views\Shared\DisplayTemplates* folder, create a strongly typed partial view named **Address**:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

<span data-ttu-id="14872-129">按一下 **新增**，然後開啟 新*Views\Shared\DisplayTemplates\Address.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="14872-129">Click **Add**, and then open the new *Views\Shared\DisplayTemplates\Address.cshtml* file.</span></span> <span data-ttu-id="14872-130">新的檢視包含下列產生的標記：</span><span class="sxs-lookup"><span data-stu-id="14872-130">The new view contains the following generated markup:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

<span data-ttu-id="14872-131">執行應用程式，並顯示`PersonDetail`檢視。</span><span class="sxs-lookup"><span data-stu-id="14872-131">Run the application and display the `PersonDetail` view.</span></span> <span data-ttu-id="14872-132">此時`Address`您剛才建立的範本用來顯示`Address`複雜型別，所以顯示看起來如下所示：</span><span class="sxs-lookup"><span data-stu-id="14872-132">This time, the `Address` template that you just created is used to display the `Address` complex type, so the display looks like the following:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a><span data-ttu-id="14872-133">指定的模型顯示格式和範本的方式摘要：</span><span class="sxs-lookup"><span data-stu-id="14872-133">Summary: Ways to Specify the Model Display Format and Template</span></span>

<span data-ttu-id="14872-134">您已了解您可以使用下列方法指定的格式或模型屬性的範本：</span><span class="sxs-lookup"><span data-stu-id="14872-134">You've seen that you can specify the format or template for a model property by using the following approaches:</span></span>

- <span data-ttu-id="14872-135">套用`DisplayFormat`屬性加入模型中的屬性。</span><span class="sxs-lookup"><span data-stu-id="14872-135">Applying the `DisplayFormat` attribute to a property in the model.</span></span> <span data-ttu-id="14872-136">例如，下列程式碼會導致顯示不包含時間的日期：</span><span class="sxs-lookup"><span data-stu-id="14872-136">For example, the following code causes the date to be displayed without the time:</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- <span data-ttu-id="14872-137">套用[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)屬性中的模型和指定的資料類型的屬性。</span><span class="sxs-lookup"><span data-stu-id="14872-137">Applying a [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute to a property in the model and specifying the data type.</span></span> <span data-ttu-id="14872-138">例如，下列程式碼會導致顯示不包含時間的日期。</span><span class="sxs-lookup"><span data-stu-id="14872-138">For example, the following code causes the date to be displayed without the time.</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    <span data-ttu-id="14872-139">如果應用程式含有*date.cshtml*中的範本*Views\Shared\DisplayTemplates*資料夾或*Views\Movies\DisplayTemplates*資料夾中，該範本將用來呈現`DateTime`屬性。</span><span class="sxs-lookup"><span data-stu-id="14872-139">If the application contains a *date.cshtml* template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder, that template will be used to render the `DateTime` property.</span></span> <span data-ttu-id="14872-140">否則內建的 ASP.NET 範本化系統會顯示為日期的屬性。</span><span class="sxs-lookup"><span data-stu-id="14872-140">Otherwise the built-in ASP.NET templating system displays the property as a date.</span></span>
- <span data-ttu-id="14872-141">建立顯示範本中的*Views\Shared\DisplayTemplates*資料夾或*Views\Movies\DisplayTemplates*資料夾名稱符合您想要格式化的資料類型。</span><span class="sxs-lookup"><span data-stu-id="14872-141">Creating a display template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder whose name matches the data type that you want to format.</span></span> <span data-ttu-id="14872-142">例如，您看到*Views\Shared\DisplayTemplates\DateTime.cshtml*用來呈現`DateTime`在模型中，加入至模型屬性而不將任何標記新增至檢視的屬性。</span><span class="sxs-lookup"><span data-stu-id="14872-142">For example, you saw that the *Views\Shared\DisplayTemplates\DateTime.cshtml* was used to render `DateTime` properties in a model, without adding an attribute to the model and without adding any markup to views.</span></span>
- <span data-ttu-id="14872-143">使用[UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)來指定要顯示的 模型屬性的範本模型的屬性。</span><span class="sxs-lookup"><span data-stu-id="14872-143">Using the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute on the model to specify the template to display the model property.</span></span>
- <span data-ttu-id="14872-144">明確地將顯示的範本名稱，以[Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx)在檢視中呼叫。</span><span class="sxs-lookup"><span data-stu-id="14872-144">Explicitly adding the display template name to the [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) call in a view.</span></span>

<span data-ttu-id="14872-145">您使用的方法取決於您要在您的應用程式中執行動作。</span><span class="sxs-lookup"><span data-stu-id="14872-145">The approach you use depends on what you need to do in your application.</span></span> <span data-ttu-id="14872-146">是不常見混用這些方法來取得所需要的格式，您需要的位置。</span><span class="sxs-lookup"><span data-stu-id="14872-146">It's not uncommon to mix these approaches to get exactly the kind of formatting that you need.</span></span>

<span data-ttu-id="14872-147">在下一步 區段中，您將切換有點一下話題，並將從自訂向自訂輸入的方式顯示資料的方式移動。</span><span class="sxs-lookup"><span data-stu-id="14872-147">In the next section, you'll switch gears a bit and move from customizing how data is displayed to customizing how it's entered.</span></span> <span data-ttu-id="14872-148">您將會連接到應用程式，以便提供靈活的方式，來指定日期中的 [編輯] 檢視的 jQuery 日期選擇器。</span><span class="sxs-lookup"><span data-stu-id="14872-148">You'll hook up the jQuery datepicker to the edit views in the application in order to provide a slick way to specify dates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="14872-149">[上一頁](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [下一頁](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="14872-149">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span></span>
