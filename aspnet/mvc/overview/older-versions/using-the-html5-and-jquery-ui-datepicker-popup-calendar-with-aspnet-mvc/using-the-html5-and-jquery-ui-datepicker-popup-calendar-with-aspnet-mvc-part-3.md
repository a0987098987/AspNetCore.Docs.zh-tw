---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: "使用 HTML5 與 jQuery UI 日期選擇器快顯行事曆搭配 ASP.NET MVC-第 3 篇 |Microsoft 文件"
author: Rick-Anderson
description: "本教學課程將告訴您如何使用編輯器範本、 顯示範本與 jQuery UI 日期選擇器快顯行事曆，ASP.NET MV 中的基本概念..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: dc81961094928025e25cf62ce4d51d12bc67b80c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a><span data-ttu-id="19f27-103">使用 HTML5 與 jQuery UI 日期選擇器快顯行事曆搭配 ASP.NET MVC-3 部分</span><span class="sxs-lookup"><span data-stu-id="19f27-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 3</span></span>
====================
<span data-ttu-id="19f27-104">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="19f27-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="19f27-105">本教學課程將告訴您如何使用編輯器範本、 顯示範本與 jQuery UI 日期選擇器快顯行事曆，ASP.NET MVC Web 應用程式中的基本概念。</span><span class="sxs-lookup"><span data-stu-id="19f27-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>


## <a name="working-with-complex-types"></a><span data-ttu-id="19f27-106">使用複雜型別</span><span class="sxs-lookup"><span data-stu-id="19f27-106">Working with Complex Types</span></span>

<span data-ttu-id="19f27-107">本節中您會建立位址類別，並了解如何建立範本以顯示它。</span><span class="sxs-lookup"><span data-stu-id="19f27-107">In this section you'll create an address class and learn how to create a template to display it.</span></span>

<span data-ttu-id="19f27-108">在*模型*資料夾中，建立新的類別檔案命名為*Person.cs*會在其中放置兩種類型：`Person`類別和`Address`類別。</span><span class="sxs-lookup"><span data-stu-id="19f27-108">In the *Models* folder, create a new class file named *Person.cs* where you'll put two types: a `Person` class and an `Address` class.</span></span> <span data-ttu-id="19f27-109">`Person`類別所包含的屬性，其類型為`Address`。</span><span class="sxs-lookup"><span data-stu-id="19f27-109">The `Person` class will contain a property that's typed as `Address`.</span></span> <span data-ttu-id="19f27-110">`Address`類型是複雜類型，這表示它不其中一個內建型別，例如`int`， `string`，或`double`。</span><span class="sxs-lookup"><span data-stu-id="19f27-110">The `Address` type is a complex type, meaning it's not one of the built-in types like `int`, `string`, or `double`.</span></span> <span data-ttu-id="19f27-111">相反地，它會有數個屬性。</span><span class="sxs-lookup"><span data-stu-id="19f27-111">Instead, it has several properties.</span></span> <span data-ttu-id="19f27-112">新類別的程式碼看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="19f27-112">The code for the new classes looks like this:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

<span data-ttu-id="19f27-113">在`Movie`控制站，加入下列`PersonDetail`動作以顯示人員執行個體：</span><span class="sxs-lookup"><span data-stu-id="19f27-113">In the `Movie` controller, add the following `PersonDetail` action to display a person instance:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

<span data-ttu-id="19f27-114">然後加入下列程式碼加入`Movie`控制站，以填入`Person`以特定範例資料模型：</span><span class="sxs-lookup"><span data-stu-id="19f27-114">Then add the following code to the `Movie` controller to populate the `Person` model with some sample data:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

<span data-ttu-id="19f27-115">開啟*Views\Movies\PersonDetail.cshtml*檔案，然後加入下列標記`PersonDetail`檢視。</span><span class="sxs-lookup"><span data-stu-id="19f27-115">Open the *Views\Movies\PersonDetail.cshtml* file and add the following markup for the `PersonDetail` view.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

<span data-ttu-id="19f27-116">按 Ctrl + F5 執行應用程式，並瀏覽到*電影/PersonDetail*。</span><span class="sxs-lookup"><span data-stu-id="19f27-116">Press Ctrl+F5 to run the application and navigate to *Movies/PersonDetail*.</span></span>

<span data-ttu-id="19f27-117">`PersonDetail`檢視不包含`Address`複雜類型，為您可以看到在這個螢幕擷取畫面。</span><span class="sxs-lookup"><span data-stu-id="19f27-117">The `PersonDetail` view doesn't contain the `Address` complex type, as you can see in this screenshot.</span></span> <span data-ttu-id="19f27-118">（沒有位址會顯示）。</span><span class="sxs-lookup"><span data-stu-id="19f27-118">(No address is shown.)</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

<span data-ttu-id="19f27-119">`Address`模型並不會因為它是複雜類型。</span><span class="sxs-lookup"><span data-stu-id="19f27-119">The `Address` model data is not displayed because it's a complex type.</span></span> <span data-ttu-id="19f27-120">若要顯示的地址資訊，請開啟*Views\Movies\PersonDetail.cshtml*檔案一次，然後加入下列標記。</span><span class="sxs-lookup"><span data-stu-id="19f27-120">To display the address information, open the *Views\Movies\PersonDetail.cshtml* file again and add the following markup.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

<span data-ttu-id="19f27-121">完整標記`PersonDetail`現在檢視看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="19f27-121">The complete markup for the `PersonDetail` now view looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

<span data-ttu-id="19f27-122">再次執行應用程式，並顯示`PersonDetail`檢視。</span><span class="sxs-lookup"><span data-stu-id="19f27-122">Run the application again and display the `PersonDetail` view.</span></span> <span data-ttu-id="19f27-123">現在會顯示位址資訊：</span><span class="sxs-lookup"><span data-stu-id="19f27-123">The address information is now displayed:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a><span data-ttu-id="19f27-124">建立複雜類型的範本</span><span class="sxs-lookup"><span data-stu-id="19f27-124">Creating a Template for a Complex Type</span></span>

<span data-ttu-id="19f27-125">在本節中，您將建立的範本，將會用來呈現`Address`複雜型別。</span><span class="sxs-lookup"><span data-stu-id="19f27-125">In this section you'll create a template that will be used to render the `Address` complex type.</span></span> <span data-ttu-id="19f27-126">當您建立的範本`Address`型別，ASP.NET MVC 可以自動使用它來設定應用程式中的任何位置的位址模型的格式。</span><span class="sxs-lookup"><span data-stu-id="19f27-126">When you create a template for the `Address` type, ASP.NET MVC can automatically use it to format an address model anywhere in the application.</span></span> <span data-ttu-id="19f27-127">這可讓您控制的轉譯的方式`Address`從一個位置的應用程式中的型別。</span><span class="sxs-lookup"><span data-stu-id="19f27-127">This gives you a way to control the rendering of the `Address` type from just one place in the application.</span></span>

<span data-ttu-id="19f27-128">在*Views\Shared\DisplayTemplates*資料夾中，建立名為強型別部分檢視**位址**:</span><span class="sxs-lookup"><span data-stu-id="19f27-128">In the *Views\Shared\DisplayTemplates* folder, create a strongly typed partial view named **Address**:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

<span data-ttu-id="19f27-129">按一下**新增**，然後開啟 新*Views\Shared\DisplayTemplates\Address.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="19f27-129">Click **Add**, and then open the new *Views\Shared\DisplayTemplates\Address.cshtml* file.</span></span> <span data-ttu-id="19f27-130">新的檢視包含下列產生的標記：</span><span class="sxs-lookup"><span data-stu-id="19f27-130">The new view contains the following generated markup:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

<span data-ttu-id="19f27-131">執行應用程式，並顯示`PersonDetail`檢視。</span><span class="sxs-lookup"><span data-stu-id="19f27-131">Run the application and display the `PersonDetail` view.</span></span> <span data-ttu-id="19f27-132">這次`Address`您剛才建立的範本用來顯示`Address`複雜類型，所以顯示類似下列所示：</span><span class="sxs-lookup"><span data-stu-id="19f27-132">This time, the `Address` template that you just created is used to display the `Address` complex type, so the display looks like the following:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a><span data-ttu-id="19f27-133">指定模型的顯示格式和範本的方式摘要：</span><span class="sxs-lookup"><span data-stu-id="19f27-133">Summary: Ways to Specify the Model Display Format and Template</span></span>

<span data-ttu-id="19f27-134">您所見，您可以使用下列方法指定的格式或模型屬性的範本：</span><span class="sxs-lookup"><span data-stu-id="19f27-134">You've seen that you can specify the format or template for a model property by using the following approaches:</span></span>

- <span data-ttu-id="19f27-135">套用`DisplayFormat`屬性設定為模型中的屬性。</span><span class="sxs-lookup"><span data-stu-id="19f27-135">Applying the `DisplayFormat` attribute to a property in the model.</span></span> <span data-ttu-id="19f27-136">例如，下列程式碼會導致顯示不含時間的日期：</span><span class="sxs-lookup"><span data-stu-id="19f27-136">For example, the following code causes the date to be displayed without the time:</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- <span data-ttu-id="19f27-137">套用[DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx)屬性中的模型和指定的資料類型的屬性。</span><span class="sxs-lookup"><span data-stu-id="19f27-137">Applying a [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) attribute to a property in the model and specifying the data type.</span></span> <span data-ttu-id="19f27-138">例如，下列程式碼會導致顯示不含時間日期。</span><span class="sxs-lookup"><span data-stu-id="19f27-138">For example, the following code causes the date to be displayed without the time.</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    <span data-ttu-id="19f27-139">如果應用程式包含*date.cshtml*中的範本*Views\Shared\DisplayTemplates*資料夾或*Views\Movies\DisplayTemplates*資料夾中，該範本將用來呈現`DateTime`屬性。</span><span class="sxs-lookup"><span data-stu-id="19f27-139">If the application contains a *date.cshtml* template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder, that template will be used to render the `DateTime` property.</span></span> <span data-ttu-id="19f27-140">否則內建 ASP.NET 樣板化系統會顯示為日期屬性。</span><span class="sxs-lookup"><span data-stu-id="19f27-140">Otherwise the built-in ASP.NET templating system displays the property as a date.</span></span>
- <span data-ttu-id="19f27-141">建立顯示範本中的*Views\Shared\DisplayTemplates*資料夾或*Views\Movies\DisplayTemplates*資料夾名稱符合您想要格式化的資料類型。</span><span class="sxs-lookup"><span data-stu-id="19f27-141">Creating a display template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder whose name matches the data type that you want to format.</span></span> <span data-ttu-id="19f27-142">例如，您看到*Views\Shared\DisplayTemplates\DateTime.cshtml*用於呈現`DateTime`模型，但不會加入至模型的屬性，而不需要任何標記加入至檢視中的屬性。</span><span class="sxs-lookup"><span data-stu-id="19f27-142">For example, you saw that the *Views\Shared\DisplayTemplates\DateTime.cshtml* was used to render `DateTime` properties in a model, without adding an attribute to the model and without adding any markup to views.</span></span>
- <span data-ttu-id="19f27-143">使用[UIHint](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)屬性上指定的範本，以顯示模型屬性的模型。</span><span class="sxs-lookup"><span data-stu-id="19f27-143">Using the [UIHint](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute on the model to specify the template to display the model property.</span></span>
- <span data-ttu-id="19f27-144">明確地加入要的顯示範本名稱[Html.DisplayFor](https://msdn.microsoft.com/en-us/library/ee407420.aspx)呼叫在檢視中。</span><span class="sxs-lookup"><span data-stu-id="19f27-144">Explicitly adding the display template name to the [Html.DisplayFor](https://msdn.microsoft.com/en-us/library/ee407420.aspx) call in a view.</span></span>

<span data-ttu-id="19f27-145">您使用的方法，取決於您要在您的應用程式中的項目。</span><span class="sxs-lookup"><span data-stu-id="19f27-145">The approach you use depends on what you need to do in your application.</span></span> <span data-ttu-id="19f27-146">是很常見混用這些方法，以取得所需要的格式，您需要的位置。</span><span class="sxs-lookup"><span data-stu-id="19f27-146">It's not uncommon to mix these approaches to get exactly the kind of formatting that you need.</span></span>

<span data-ttu-id="19f27-147">在下一節中，您將切換一下話題位元，從自訂向自訂輸入的方式顯示資料的方式移動。</span><span class="sxs-lookup"><span data-stu-id="19f27-147">In the next section, you'll switch gears a bit and move from customizing how data is displayed to customizing how it's entered.</span></span> <span data-ttu-id="19f27-148">您將會連結到編輯檢視中的應用程式，以便提供靈活的方式來指定日期的 jQuery 日期選擇器。</span><span class="sxs-lookup"><span data-stu-id="19f27-148">You'll hook up the jQuery datepicker to the edit views in the application in order to provide a slick way to specify dates.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="19f27-149">[上一頁](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[下一頁](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="19f27-149">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span></span>
