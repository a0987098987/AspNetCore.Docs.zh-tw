---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: ASP.NET MVC 應用程式建立更複雜的資料模型 |Microsoft 文件
author: tdykstra
description: Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 5 應用程式使用 Entity Framework 6 Code First 和 Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 46f7f3c9-274f-4649-811d-92222a9b27e2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: fd8bf6502b0dd261505a86a2ed86d4c3f42e8755
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877095"
---
<a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application"></a><span data-ttu-id="8759a-103">ASP.NET MVC 應用程式建立更複雜的資料模型</span><span class="sxs-lookup"><span data-stu-id="8759a-103">Creating a More Complex Data Model for an ASP.NET MVC Application</span></span>
====================
<span data-ttu-id="8759a-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8759a-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="8759a-105">[下載完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span><span class="sxs-lookup"><span data-stu-id="8759a-105">[Download Completed Project](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) or [Download PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span></span>

> <span data-ttu-id="8759a-106">Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8759a-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 5 applications using the Entity Framework 6 Code First and Visual Studio 2013.</span></span> <span data-ttu-id="8759a-107">如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="8759a-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


<span data-ttu-id="8759a-108">在先前的教學課程中您使用過的簡單資料模型的三個實體所組成。</span><span class="sxs-lookup"><span data-stu-id="8759a-108">In the previous tutorials you worked with a simple data model that was composed of three entities.</span></span> <span data-ttu-id="8759a-109">在本教學課程，您可以加入多個實體和關聯性，您將自訂資料模型，藉由指定的格式、 驗證和資料庫對應規則。</span><span class="sxs-lookup"><span data-stu-id="8759a-109">In this tutorial you'll add more entities and relationships and you'll customize the data model by specifying formatting, validation, and database mapping rules.</span></span> <span data-ttu-id="8759a-110">您會看到兩種方式可以自訂資料模型： 藉由加入屬性至實體類別，並將程式碼加入至資料庫內容類別。</span><span class="sxs-lookup"><span data-stu-id="8759a-110">You'll see two ways to customize the data model: by adding attributes to entity classes and by adding code to the database context class.</span></span>

<span data-ttu-id="8759a-111">當您完成時，實體類別會構成如下列圖例中所顯示的完整資料模型：</span><span class="sxs-lookup"><span data-stu-id="8759a-111">When you're finished, the entity classes will make up the completed data model that's shown in the following illustration:</span></span>

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a><span data-ttu-id="8759a-113">使用屬性自訂資料模型</span><span class="sxs-lookup"><span data-stu-id="8759a-113">Customize the Data Model by Using Attributes</span></span>

<span data-ttu-id="8759a-114">在本節中，您會了解到如何使用指定格式、驗證和資料庫對應規則的屬性來自訂資料模型。</span><span class="sxs-lookup"><span data-stu-id="8759a-114">In this section you'll see how to customize the data model by using attributes that specify formatting, validation, and database mapping rules.</span></span> <span data-ttu-id="8759a-115">然後會在下列章節的幾個您建立完整`School`加入資料模型屬性類別已建立並建立新的類別，在模型中剩餘的實體類型。</span><span class="sxs-lookup"><span data-stu-id="8759a-115">Then in several of the following sections you'll create the complete `School` data model by adding attributes to the classes you already created and creating new classes for the remaining entity types in the model.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="8759a-116">資料類型屬性</span><span class="sxs-lookup"><span data-stu-id="8759a-116">The DataType Attribute</span></span>

<span data-ttu-id="8759a-117">針對學生的註冊日期，所有網頁目前都會同時顯示時間和日期，即使您針對此欄位只需要日期而已。</span><span class="sxs-lookup"><span data-stu-id="8759a-117">For student enrollment dates, all of the web pages currently display the time along with the date, although all you care about for this field is the date.</span></span> <span data-ttu-id="8759a-118">使用資料註解屬性，您可以透過僅對一個程式碼進行變更，來修正每個顯示資料的檢視上的顯示格式。</span><span class="sxs-lookup"><span data-stu-id="8759a-118">By using data annotation attributes, you can make one code change that will fix the display format in every view that shows the data.</span></span> <span data-ttu-id="8759a-119">為了查看如何進行此操作的範例，您將會新增一個屬性至 `Student` 類別中的 `EnrollmentDate` 屬性。</span><span class="sxs-lookup"><span data-stu-id="8759a-119">To see an example of how to do that, you'll add an attribute to the `EnrollmentDate` property in the `Student` class.</span></span>

<span data-ttu-id="8759a-120">在*Models\Student.cs*，新增`using`陳述式`System.ComponentModel.DataAnnotations`命名空間和新增`DataType`和`DisplayFormat`屬性至`EnrollmentDate`屬性，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="8759a-120">In *Models\Student.cs*, add a `using` statement for the `System.ComponentModel.DataAnnotations` namespace and add `DataType` and `DisplayFormat` attributes to the `EnrollmentDate` property, as shown in the following example:</span></span>

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,12-13)]

<span data-ttu-id="8759a-121">[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性用來指定資料庫內建型別比更特定的資料類型。</span><span class="sxs-lookup"><span data-stu-id="8759a-121">The [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attribute is used to specify a data type that is more specific than the database intrinsic type.</span></span> <span data-ttu-id="8759a-122">在此案例中，我們只想要追蹤日期，而非日期和時間。</span><span class="sxs-lookup"><span data-stu-id="8759a-122">In this case we only want to keep track of the date, not the date and time.</span></span> <span data-ttu-id="8759a-123">[DataType 列舉](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)提供許多資料類型，例如*日期、 時間、 電話號碼、 貨幣、 EmailAddress*等等。</span><span class="sxs-lookup"><span data-stu-id="8759a-123">The [DataType Enumeration](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) provides for many data types, such as *Date, Time, PhoneNumber, Currency, EmailAddress* and more.</span></span> <span data-ttu-id="8759a-124">`DataType` 屬性也可讓應用程式自動提供類型的特定功能。</span><span class="sxs-lookup"><span data-stu-id="8759a-124">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="8759a-125">比方說，`mailto:`可建立連結的[DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)，而且可以提供日期選擇器[DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)支援的瀏覽器[HTML5](http://html5.org/).</span><span class="sxs-lookup"><span data-stu-id="8759a-125">For example, a `mailto:` link can be created for [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), and a date selector can be provided for [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) in browsers that support [HTML5](http://html5.org/).</span></span> <span data-ttu-id="8759a-126">[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性發出 HTML 5[資料-](http://ejohn.org/blog/html-5-data-attributes/) (phishing 英文發音如*資料虛線*) 能夠辨識的 html5 瀏覽器的屬性。</span><span class="sxs-lookup"><span data-stu-id="8759a-126">The [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributes emits HTML 5 [data-](http://ejohn.org/blog/html-5-data-attributes/) (pronounced *data dash*) attributes that HTML 5 browsers can understand.</span></span> <span data-ttu-id="8759a-127">[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性不提供任何驗證。</span><span class="sxs-lookup"><span data-stu-id="8759a-127">The [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributes do not provide any validation.</span></span>

<span data-ttu-id="8759a-128">`DataType.Date` 未指定顯示日期的格式。</span><span class="sxs-lookup"><span data-stu-id="8759a-128">`DataType.Date` does not specify the format of the date that is displayed.</span></span> <span data-ttu-id="8759a-129">根據預設，資料欄位會顯示根據基礎在伺服器上的預設格式[CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)。</span><span class="sxs-lookup"><span data-stu-id="8759a-129">By default, the data field is displayed according to the default formats based on the server's [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).</span></span>

<span data-ttu-id="8759a-130">`DisplayFormat` 屬性用來明確指定日期格式：</span><span class="sxs-lookup"><span data-stu-id="8759a-130">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


<span data-ttu-id="8759a-131">`ApplyFormatInEditMode`設定指定指定的格式應該也套用時的值會顯示在文字方塊中進行編輯。</span><span class="sxs-lookup"><span data-stu-id="8759a-131">The `ApplyFormatInEditMode` setting specifies that the specified formatting should also be applied when the value is displayed in a text box for editing.</span></span> <span data-ttu-id="8759a-132">(您可能不想要的某些欄位，例如，貨幣值，您可能不想在文字方塊中的貨幣符號進行編輯。)</span><span class="sxs-lookup"><span data-stu-id="8759a-132">(You might not want that for some fields — for example, for currency values, you might not want the currency symbol in the text box for editing.)</span></span>

<span data-ttu-id="8759a-133">您可以使用[DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)屬性本身，但它通常是最好使用[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)也屬性。</span><span class="sxs-lookup"><span data-stu-id="8759a-133">You can use the [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribute by itself, but it's generally a good idea to use the [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attribute also.</span></span> <span data-ttu-id="8759a-134">`DataType`屬性傳達*語意*的資料做為相對於如何呈現在畫面上，並提供下列優點，就無法取得與`DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="8759a-134">The `DataType` attribute conveys the *semantics* of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with `DisplayFormat`:</span></span>

- <span data-ttu-id="8759a-135">瀏覽器可以啟用 HTML5 功能 (例如顯示日曆控制項、適合地區設定的貨幣符號、電子郵件連結、一些用戶端輸入驗證等等)。</span><span class="sxs-lookup"><span data-stu-id="8759a-135">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, some client-side input validation, etc.).</span></span>
- <span data-ttu-id="8759a-136">根據預設，瀏覽器將會轉譯資料使用正確的格式，根據您[地區設定](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx)。</span><span class="sxs-lookup"><span data-stu-id="8759a-136">By default, the browser will render data using the correct format based on your [locale](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).</span></span>
- <span data-ttu-id="8759a-137">[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性可以讓 MVC 選擇正確的欄位範本轉譯資料 ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)使用字串的範本)。</span><span class="sxs-lookup"><span data-stu-id="8759a-137">The [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attribute can enable MVC to choose the right field template to render the data (the [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) uses the string template).</span></span> <span data-ttu-id="8759a-138">如需詳細資訊，請參閱 Brad Wilson 的[ASP.NET MVC 2 範本](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)。</span><span class="sxs-lookup"><span data-stu-id="8759a-138">For more information, see Brad Wilson's [ASP.NET MVC 2 Templates](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html).</span></span> <span data-ttu-id="8759a-139">（針對 MVC 2 所撰寫，不過這篇文章仍適用於目前版本的 ASP.NET MVC。）</span><span class="sxs-lookup"><span data-stu-id="8759a-139">(Though written for MVC 2, this article still applies to the current version of ASP.NET MVC.)</span></span>

<span data-ttu-id="8759a-140">如果您使用`DataType`屬性使用 [日期] 欄位中，您必須指定`DisplayFormat`屬性也以確保欄位在 Chrome 瀏覽器中正確轉譯。</span><span class="sxs-lookup"><span data-stu-id="8759a-140">If you use the `DataType` attribute with a date field, you have to specify the `DisplayFormat` attribute also in order to ensure that the field renders correctly in Chrome browsers.</span></span> <span data-ttu-id="8759a-141">如需詳細資訊，請參閱[這個 StackOverflow 執行緒](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie)。</span><span class="sxs-lookup"><span data-stu-id="8759a-141">For more information, see [this StackOverflow thread](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).</span></span>

<span data-ttu-id="8759a-142">如需如何處理在 MVC 中的其他日期格式的詳細資訊，請移至[MVC 5 簡介： 檢查編輯方法與編輯檢視](../introduction/examining-the-edit-methods-and-edit-view.md)和搜尋的頁面中&quot;國際化&quot;。</span><span class="sxs-lookup"><span data-stu-id="8759a-142">For more information about how to handle other date formats in MVC, go to [MVC 5 Introduction: Examining the Edit Methods and Edit View](../introduction/examining-the-edit-methods-and-edit-view.md) and search in the page for &quot;internationalization&quot;.</span></span>

<span data-ttu-id="8759a-143">再次執行學生索引頁，並請注意，時間不會再顯示註冊日期。</span><span class="sxs-lookup"><span data-stu-id="8759a-143">Run the Student Index page again and notice that times are no longer displayed for the enrollment dates.</span></span> <span data-ttu-id="8759a-144">會使用任何檢視，則為 true 的相同`Student`模型。</span><span class="sxs-lookup"><span data-stu-id="8759a-144">The same will be true for any view that uses the `Student` model.</span></span>

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a><span data-ttu-id="8759a-146">StringLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="8759a-146">The StringLengthAttribute</span></span>

<span data-ttu-id="8759a-147">您也可以使用屬性指定資料驗證規則和驗證錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="8759a-147">You can also specify data validation rules and validation error messages using attributes.</span></span> <span data-ttu-id="8759a-148">[StringLength 屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)設定資料庫中的最大長度，並提供用戶端和伺服器端的 ASP.NET MVC 驗證。</span><span class="sxs-lookup"><span data-stu-id="8759a-148">The [StringLength attribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) sets the maximum length in the database and provides client side and server side validation for ASP.NET MVC.</span></span> <span data-ttu-id="8759a-149">您也可以在此屬性中指定最小字串長度，但最小值不會對資料庫結構描述造成任何影響。</span><span class="sxs-lookup"><span data-stu-id="8759a-149">You can also specify the minimum string length in this attribute, but the minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="8759a-150">假設您想要確保使用者不會在名稱中輸入超過 50 個字元。</span><span class="sxs-lookup"><span data-stu-id="8759a-150">Suppose you want to ensure that users don't enter more than 50 characters for a name.</span></span> <span data-ttu-id="8759a-151">若要加入這項限制，加入[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)屬性至`LastName`和`FirstMidName`屬性，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="8759a-151">To add this limitation, add [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attributes to the `LastName` and `FirstMidName` properties, as shown in the following example:</span></span>

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

<span data-ttu-id="8759a-152">[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)屬性將不會防止使用者輸入名稱的泛空白字元。</span><span class="sxs-lookup"><span data-stu-id="8759a-152">The [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute won't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="8759a-153">您可以使用[RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)將限制套用至輸入的屬性。</span><span class="sxs-lookup"><span data-stu-id="8759a-153">You can use the [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) attribute to apply restrictions to the input.</span></span> <span data-ttu-id="8759a-154">例如下列程式碼需要第一個字元是大寫，而其餘字元是依字母順序排列：</span><span class="sxs-lookup"><span data-stu-id="8759a-154">For example the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

<span data-ttu-id="8759a-155">[MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx)屬性會提供類似的功能[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)屬性，但不會提供用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="8759a-155">The [MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) attribute provides similar functionality to the [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute but doesn't provide client side validation.</span></span>

<span data-ttu-id="8759a-156">執行應用程式，然後按一下**學生** 索引標籤。您會收到下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="8759a-156">Run the application and click the **Students** tab. You get the following error:</span></span>

<span data-ttu-id="8759a-157">*備份 'SchoolContext' 內容的模型已變更，因為所建立的資料庫。請考慮使用 Code First 移轉來更新資料庫 ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269))。*</span><span class="sxs-lookup"><span data-stu-id="8759a-157">*The model backing the 'SchoolContext' context has changed since the database was created. Consider using Code First Migrations to update the database ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).*</span></span>

<span data-ttu-id="8759a-158">資料庫模型已變更，則需要在資料庫結構描述變更的方式和 Entity Framework 偵測到。</span><span class="sxs-lookup"><span data-stu-id="8759a-158">The database model has changed in a way that requires a change in the database schema, and Entity Framework detected that.</span></span> <span data-ttu-id="8759a-159">您將使用移轉，而不會遺失任何資料，使用 UI 新增到資料庫中更新結構描述。</span><span class="sxs-lookup"><span data-stu-id="8759a-159">You'll use migrations to update the schema without losing any data that you added to the database by using the UI.</span></span> <span data-ttu-id="8759a-160">如果您變更資料所建立的`Seed`方法，將會變更回其原始狀態，因為[AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx)中使用的方法`Seed`方法。</span><span class="sxs-lookup"><span data-stu-id="8759a-160">If you changed data that was created by the `Seed` method, that will be changed back to its original state because of the [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) method that you're using in the `Seed` method.</span></span> <span data-ttu-id="8759a-161">([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx)等於從資料庫詞彙中的 「 更新插入 」 作業。)</span><span class="sxs-lookup"><span data-stu-id="8759a-161">([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) is equivalent to an "upsert" operation from database terminology.)</span></span>

<span data-ttu-id="8759a-162">請在套件管理員主控台 (PMC) 中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="8759a-162">In the Package Manager Console (PMC), enter the following commands:</span></span>

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

<span data-ttu-id="8759a-163">`add-migration`命令會建立名為*&lt;時間戳記&gt;\_MaxLengthOnNames.cs*。</span><span class="sxs-lookup"><span data-stu-id="8759a-163">The `add-migration` command creates a file named *&lt;timeStamp&gt;\_MaxLengthOnNames.cs*.</span></span> <span data-ttu-id="8759a-164">此檔案包含了 `Up` 方法中的程式碼，可更新資料庫，使其符合目前的資料模型。</span><span class="sxs-lookup"><span data-stu-id="8759a-164">This file contains code in the `Up` method that will update the database to match the current data model.</span></span> <span data-ttu-id="8759a-165">`update-database` 命令執行了該程式碼。</span><span class="sxs-lookup"><span data-stu-id="8759a-165">The `update-database` command ran that code.</span></span>

<span data-ttu-id="8759a-166">移轉檔案名稱的前面加上時間戳記 Entity framework 用於排序的移轉。</span><span class="sxs-lookup"><span data-stu-id="8759a-166">The timestamp prepended to the migrations file name is used by Entity Framework to order the migrations.</span></span> <span data-ttu-id="8759a-167">您可以建立多個移轉之前執行`update-database`命令，然後再移轉的所有會套用已建立的順序。</span><span class="sxs-lookup"><span data-stu-id="8759a-167">You can create multiple migrations before running the `update-database` command, and then all of the migrations are applied in the order in which they were created.</span></span>

<span data-ttu-id="8759a-168">執行**建立**頁面，然後輸入任一名稱長度超過 50 個字元。</span><span class="sxs-lookup"><span data-stu-id="8759a-168">Run the **Create** page, and enter either name longer than 50 characters.</span></span> <span data-ttu-id="8759a-169">當您按一下 [建立] 時，用戶端驗證便會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="8759a-169">When you click **Create**, client side validation shows an error message.</span></span>

![用戶端 val 錯誤](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a><span data-ttu-id="8759a-171">資料行屬性</span><span class="sxs-lookup"><span data-stu-id="8759a-171">The Column Attribute</span></span>

<span data-ttu-id="8759a-172">您也可以使用屬性控制您的類別和屬性對應到資料庫的方式。</span><span class="sxs-lookup"><span data-stu-id="8759a-172">You can also use attributes to control how your classes and properties are mapped to the database.</span></span> <span data-ttu-id="8759a-173">假設您已針對名字欄位使用 `FirstMidName` 作為名稱，因為欄位中可能也會包含中間名。</span><span class="sxs-lookup"><span data-stu-id="8759a-173">Suppose you had used the name `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span> <span data-ttu-id="8759a-174">但您想要將資料庫資料行命名為 `FirstName`，因為撰寫臨機操作查詢資料庫的使用者比較習慣該名稱。</span><span class="sxs-lookup"><span data-stu-id="8759a-174">But you want the database column to be named `FirstName`, because users who will be writing ad-hoc queries against the database are accustomed to that name.</span></span> <span data-ttu-id="8759a-175">若要進行此對應，您可以使用 `Column` 屬性。</span><span class="sxs-lookup"><span data-stu-id="8759a-175">To make this mapping, you can use the `Column` attribute.</span></span>

<span data-ttu-id="8759a-176">`Column` 屬性指定當建立資料庫時，`Student` 資料表中對應到 `FirstMidName` 屬性的資料行會命名為 `FirstName`。</span><span class="sxs-lookup"><span data-stu-id="8759a-176">The `Column` attribute specifies that when the database is created, the column of the `Student` table that maps to the `FirstMidName` property will be named `FirstName`.</span></span> <span data-ttu-id="8759a-177">換句話說，當您的程式碼參照 `Student.FirstMidName` 時，資料便會來自 `Student` 資料表中的 `FirstName` 資料行或在其中更新。</span><span class="sxs-lookup"><span data-stu-id="8759a-177">In other words, when your code refers to `Student.FirstMidName`, the data will come from or be updated in the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="8759a-178">如果您未指定資料行名稱，會收到相同的名稱與屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="8759a-178">If you don't specify column names, they are given the same name as the property name.</span></span>

<span data-ttu-id="8759a-179">在*Student.cs* file、 add`using`陳述式[System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx)並加入至資料行名稱屬性`FirstMidName`屬性，如下所示下列反白顯示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="8759a-179">In the *Student.cs* file, add a `using` statement for [System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) and add the column name attribute to the `FirstMidName` property, as shown in the following highlighted code:</span></span>

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

<span data-ttu-id="8759a-180">新增[資料行屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx)變更備份 SchoolContext，因此它將不會符合資料庫的模型。</span><span class="sxs-lookup"><span data-stu-id="8759a-180">The addition of the [Column attribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) changes the model backing the SchoolContext, so it won't match the database.</span></span> <span data-ttu-id="8759a-181">若要建立其他移轉 PMC 中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="8759a-181">Enter the following commands in the PMC to create another migration:</span></span>

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

<span data-ttu-id="8759a-182">在**伺服器總管**，開啟*學生*資料表設計工具，按兩下*學生*資料表。</span><span class="sxs-lookup"><span data-stu-id="8759a-182">In **Server Explorer**, open the *Student* table designer by double-clicking the *Student* table.</span></span>

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="8759a-183">下圖顯示原始的資料行名稱，因為它已套用的前兩個移轉作業之前。</span><span class="sxs-lookup"><span data-stu-id="8759a-183">The following image shows the original column name as it was before you applied the first two migrations.</span></span> <span data-ttu-id="8759a-184">除了從變更的資料行名稱`FirstMidName`至`FirstName`，兩個名稱的資料行已從`MAX`長度為 50 個字元。</span><span class="sxs-lookup"><span data-stu-id="8759a-184">In addition to the column name changing from `FirstMidName` to `FirstName`, the two name columns have changed from `MAX` length to 50 characters.</span></span>

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

<span data-ttu-id="8759a-185">您也可以變更資料庫對應使用[Fluent API](https://msdn.microsoft.com/data/jj591617)，稍後在本教學課程，您會看到如下。</span><span class="sxs-lookup"><span data-stu-id="8759a-185">You can also make database mapping changes using the [Fluent API](https://msdn.microsoft.com/data/jj591617), as you'll see later in this tutorial.</span></span>

> [!NOTE]
> <span data-ttu-id="8759a-186">若您嘗試在完成建立下列章節中所有的實體類別前進行編譯，您可能會收到編譯器錯誤。</span><span class="sxs-lookup"><span data-stu-id="8759a-186">If you try to compile before you finish creating all of the entity classes in the following sections, you might get compiler errors.</span></span>


## <a name="complete-changes-to-the-student-entity"></a><span data-ttu-id="8759a-187">完成學生實體的變更</span><span class="sxs-lookup"><span data-stu-id="8759a-187">Complete Changes to the Student Entity</span></span>

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="8759a-189">在*Models\Student.cs*，稍早為下列程式碼取代您所加入的程式碼。</span><span class="sxs-lookup"><span data-stu-id="8759a-189">In *Models\Student.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="8759a-190">所做的變更已醒目提示。</span><span class="sxs-lookup"><span data-stu-id="8759a-190">The changes are highlighted.</span></span>

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=11,13,15,18,22,25-32)]

### <a name="the-required-attribute"></a><span data-ttu-id="8759a-191">必要的屬性</span><span class="sxs-lookup"><span data-stu-id="8759a-191">The Required Attribute</span></span>

<span data-ttu-id="8759a-192">[必要的屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)讓名稱屬性的必要的欄位。</span><span class="sxs-lookup"><span data-stu-id="8759a-192">The [Required attribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) makes the name properties required fields.</span></span> <span data-ttu-id="8759a-193">`Required attribute`不需要使用實值類型，例如 DateTime、 int、 double、 和 float。</span><span class="sxs-lookup"><span data-stu-id="8759a-193">The `Required attribute` is not needed for value types such as DateTime, int, double, and float.</span></span> <span data-ttu-id="8759a-194">因此原本就視為必要的欄位，實值類型無法指派 null 值。</span><span class="sxs-lookup"><span data-stu-id="8759a-194">Value types cannot be assigned a null value, so they are inherently treated as required fields.</span></span> <span data-ttu-id="8759a-195">您也可以移除[必要的屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)並取代的最小長度參數`StringLength`屬性：</span><span class="sxs-lookup"><span data-stu-id="8759a-195">You could remove the [Required attribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) and replace it with a minimum length parameter for the `StringLength` attribute:</span></span>

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs?highlight=2)]

### <a name="the-display-attribute"></a><span data-ttu-id="8759a-196">顯示屬性</span><span class="sxs-lookup"><span data-stu-id="8759a-196">The Display Attribute</span></span>

<span data-ttu-id="8759a-197">`Display` 屬性指定了文字方塊的標題應為「名字」、「姓氏」、「全名」及「註冊日期」，而非每個執行個體中的屬性名稱 (沒有使用空格鍵分隔單字的名稱)。</span><span class="sxs-lookup"><span data-stu-id="8759a-197">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date" instead of the property name in each instance (which has no space dividing the words).</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="8759a-198">FullName 計算內容</span><span class="sxs-lookup"><span data-stu-id="8759a-198">The FullName Calculated Property</span></span>

<span data-ttu-id="8759a-199">`FullName` 為一個計算屬性，會傳回藉由串連兩個其他屬性而建立的值。</span><span class="sxs-lookup"><span data-stu-id="8759a-199">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="8759a-200">因此只有`get`存取子，且沒有`FullName`會在資料庫中產生資料行。</span><span class="sxs-lookup"><span data-stu-id="8759a-200">Therefore it has only a `get` accessor, and no `FullName` column will be generated in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="8759a-201">建立 Instructor 實體</span><span class="sxs-lookup"><span data-stu-id="8759a-201">Create the Instructor Entity</span></span>

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="8759a-203">建立*Models\Instructor.cs*，範本程式碼取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="8759a-203">Create *Models\Instructor.cs*, replacing the template code with the following code:</span></span>

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs)]

<span data-ttu-id="8759a-204">請注意，當中有幾個屬性跟 `Student` 和 `Instructor` 實體中的一樣。</span><span class="sxs-lookup"><span data-stu-id="8759a-204">Notice that several properties are the same in the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="8759a-205">在本系列稍後的[實作繼承](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)教學課程中，您會對此程式碼進行重構以消除冗餘。</span><span class="sxs-lookup"><span data-stu-id="8759a-205">In the [Implementing Inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial later in this series, you'll refactor this code to eliminate the redundancy.</span></span>

<span data-ttu-id="8759a-206">您也可以如下所示撰寫講師類別，您可以在同一行，讓多個屬性：</span><span class="sxs-lookup"><span data-stu-id="8759a-206">You can put multiple attributes on one line, so you could also write the instructor class as follows:</span></span>

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a><span data-ttu-id="8759a-207">課程和 OfficeAssignment 導覽屬性</span><span class="sxs-lookup"><span data-stu-id="8759a-207">The Courses and OfficeAssignment Navigation Properties</span></span>

<span data-ttu-id="8759a-208">`Courses` 和 `OfficeAssignment` 屬性為導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="8759a-208">The `Courses` and `OfficeAssignment` properties are navigation properties.</span></span> <span data-ttu-id="8759a-209">已稍早所述，它們通常會定義為[虛擬](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx)，讓他們可以利用稱為 Entity Framework 功能[消極式載入](https://msdn.microsoft.com/magazine/hh205756.aspx)。</span><span class="sxs-lookup"><span data-stu-id="8759a-209">As was explained earlier, they are typically defined as [virtual](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) so that they can take advantage of an Entity Framework feature called [lazy loading](https://msdn.microsoft.com/magazine/hh205756.aspx).</span></span> <span data-ttu-id="8759a-210">此外，如果導覽屬性都可以保存多個實體，其類型必須實作[ICollection&lt;T&gt; ](https://msdn.microsoft.com/library/92t2ye13.aspx)介面。</span><span class="sxs-lookup"><span data-stu-id="8759a-210">In addition, if a navigation property can hold multiple entities, its type must implement the [ICollection&lt;T&gt;](https://msdn.microsoft.com/library/92t2ye13.aspx) Interface.</span></span> <span data-ttu-id="8759a-211">例如[IList&lt;T&gt; ](https://msdn.microsoft.com/library/5y536ey6.aspx)但不是會限定[IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx)因為`IEnumerable<T>`不會實作[新增](https://msdn.microsoft.com/library/63ywd54z.aspx).</span><span class="sxs-lookup"><span data-stu-id="8759a-211">For example [IList&lt;T&gt;](https://msdn.microsoft.com/library/5y536ey6.aspx) qualifies but not [IEnumerable&lt;T&gt;](https://msdn.microsoft.com/library/9eekhta0.aspx) because `IEnumerable<T>` doesn't implement [Add](https://msdn.microsoft.com/library/63ywd54z.aspx).</span></span>

<span data-ttu-id="8759a-212">講師可以教導任意數目的課程，因此`Courses`做為集合定義`Course`實體。</span><span class="sxs-lookup"><span data-stu-id="8759a-212">An instructor can teach any number of courses, so `Courses` is defined as a collection of `Course` entities.</span></span>

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

<span data-ttu-id="8759a-213">我們的商務規則狀態講師只能使用最多一個辦事處的資料，因此`OfficeAssignment`定義為單一`OfficeAssignment`實體 (它可能是`null`如果被不指派任何 office)。</span><span class="sxs-lookup"><span data-stu-id="8759a-213">Our business rules state an instructor can only have at most one office, so `OfficeAssignment` is defined as a single `OfficeAssignment` entity (which may be `null` if no office is assigned).</span></span>

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="8759a-214">建立 OfficeAssignment 實體</span><span class="sxs-lookup"><span data-stu-id="8759a-214">Create the OfficeAssignment Entity</span></span>

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

<span data-ttu-id="8759a-216">建立*Models\OfficeAssignment.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="8759a-216">Create *Models\OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

<span data-ttu-id="8759a-217">建置專案時，會將儲存您的變更，並確認您還未做任何複製和貼上，編譯器可以攔截的錯誤。</span><span class="sxs-lookup"><span data-stu-id="8759a-217">Build the project, which saves your changes and verifies you haven't made any copy and paste errors the compiler can catch.</span></span>

### <a name="the-key-attribute"></a><span data-ttu-id="8759a-218">索引鍵屬性</span><span class="sxs-lookup"><span data-stu-id="8759a-218">The Key Attribute</span></span>

<span data-ttu-id="8759a-219">以 0 或-1 一個之間沒有關聯性`Instructor`和`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="8759a-219">There's a one-to-zero-or-one relationship between the `Instructor` and the `OfficeAssignment` entities.</span></span> <span data-ttu-id="8759a-220">Office 指派相對於講師指派，才會存在，因此其主索引鍵也是其外部索引鍵`Instructor`實體。</span><span class="sxs-lookup"><span data-stu-id="8759a-220">An office assignment only exists in relation to the instructor it's assigned to, and therefore its primary key is also its foreign key to the `Instructor` entity.</span></span> <span data-ttu-id="8759a-221">Entity Framework 無法自動辨識，但是`InstructorID`為主要索引鍵此實體的因為其名稱未遵循`ID`或*classname* `ID`命名慣例。</span><span class="sxs-lookup"><span data-stu-id="8759a-221">But the Entity Framework can't automatically recognize `InstructorID` as the primary key of this entity because its name doesn't follow the `ID` or *classname* `ID` naming convention.</span></span> <span data-ttu-id="8759a-222">因此，必須使用 `Key` 屬性將其識別為 PK：</span><span class="sxs-lookup"><span data-stu-id="8759a-222">Therefore, the `Key` attribute is used to identify it as the key:</span></span>

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

<span data-ttu-id="8759a-223">您也可以使用`Key`屬性如果實體沒有自己的主索引鍵，但您想要指定不同的名稱屬性`classnameID`或`ID`。</span><span class="sxs-lookup"><span data-stu-id="8759a-223">You can also use the `Key` attribute if the entity does have its own primary key but you want to name the property something different than `classnameID` or `ID`.</span></span> <span data-ttu-id="8759a-224">根據預設 EF 視為金鑰非產生資料庫因為資料行是識別的關聯性。</span><span class="sxs-lookup"><span data-stu-id="8759a-224">By default EF treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-foreignkey-attribute"></a><span data-ttu-id="8759a-225">ForeignKey 屬性</span><span class="sxs-lookup"><span data-stu-id="8759a-225">The ForeignKey Attribute</span></span>

<span data-ttu-id="8759a-226">有一對零-或-一關聯性或兩個實體之間的一對一關聯性時 (這類之間`OfficeAssignment`和`Instructor`)，EF 不算出關聯性的一端是主體，而且相依的結尾。</span><span class="sxs-lookup"><span data-stu-id="8759a-226">When there is a one-to-zero-or-one relationship or a one-to-one relationship between two entities (such as between `OfficeAssignment` and `Instructor`), EF can't work out which end of the relationship is the principal and which end is dependent.</span></span> <span data-ttu-id="8759a-227">一對一關聯性為其他類別的每個類別中有參考導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="8759a-227">One-to-one relationships have a reference navigation property in each class to the other class.</span></span> <span data-ttu-id="8759a-228">[ForeignKey 屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx)可以套用至相依的類別，以建立關聯性。</span><span class="sxs-lookup"><span data-stu-id="8759a-228">The [ForeignKey Attribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) can be applied to the dependent class to establish the relationship.</span></span> <span data-ttu-id="8759a-229">如果您省略[ForeignKey 屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx)，當您嘗試建立移轉作業時，收到下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="8759a-229">If you omit the [ForeignKey Attribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), you get the following error when you try to create the migration:</span></span>

<span data-ttu-id="8759a-230">*無法判定類型 'ContosoUniversity.Models.OfficeAssignment' 和 'ContosoUniversity.Models.Instructor' 之間的關聯的主要端點。此關聯的主要端點必須使用明確設定關聯性 fluent 應用程式開發介面或資料註解。*</span><span class="sxs-lookup"><span data-stu-id="8759a-230">*Unable to determine the principal end of an association between the types 'ContosoUniversity.Models.OfficeAssignment' and 'ContosoUniversity.Models.Instructor'. The principal end of this association must be explicitly configured using either the relationship fluent API or data annotations.*</span></span>

<span data-ttu-id="8759a-231">稍後在教學課程中，您會看到如何使用來設定此關聯性 fluent 應用程式開發的應用程式開發介面。</span><span class="sxs-lookup"><span data-stu-id="8759a-231">Later in the tutorial you'll see how to configure this relationship with the fluent API.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="8759a-232">講師導覽屬性</span><span class="sxs-lookup"><span data-stu-id="8759a-232">The Instructor Navigation Property</span></span>

<span data-ttu-id="8759a-233">`Instructor`實體有允許 null`OfficeAssignment`導覽屬性 （因為講師可能沒有 office 指派），而`OfficeAssignment`實體具有不可為 null`Instructor`導覽屬性 （因為無法 office 指派沒有講師-存在`InstructorID`不可為 null)。</span><span class="sxs-lookup"><span data-stu-id="8759a-233">The `Instructor` entity has a nullable `OfficeAssignment` navigation property (because an instructor might not have an office assignment), and the `OfficeAssignment` entity has a non-nullable `Instructor` navigation property (because an office assignment can't exist without an instructor -- `InstructorID` is non-nullable).</span></span> <span data-ttu-id="8759a-234">當`Instructor`實體具有相關`OfficeAssignment`實體，每個實體會有另一個在其導覽屬性的參考。</span><span class="sxs-lookup"><span data-stu-id="8759a-234">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity will have a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="8759a-235">您無法將`[Required]`講師導覽屬性，指定必須有相關的講師，但您不需要這樣做，因為 InstructorID 外部索引鍵 （這也是這個資料表的索引鍵） 是不可為 null 的屬性。</span><span class="sxs-lookup"><span data-stu-id="8759a-235">You could put a `[Required]` attribute on the Instructor navigation property to specify that there must be a related instructor, but you don't have to do that because the InstructorID foreign key (which is also the key to this table) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="8759a-236">修改 Course 實體</span><span class="sxs-lookup"><span data-stu-id="8759a-236">Modify the Course Entity</span></span>

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

<span data-ttu-id="8759a-238">在*Models\Course.cs*，稍早為下列程式碼取代您所加入的程式碼：</span><span class="sxs-lookup"><span data-stu-id="8759a-238">In *Models\Course.cs*, replace the code you added earlier with the following code:</span></span>

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

<span data-ttu-id="8759a-239">課程實體具有外部索引鍵屬性`DepartmentID`指向相關`Department`實體，且具有`Department`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="8759a-239">The course entity has a foreign key property `DepartmentID` which points to the related `Department` entity and it has a `Department` navigation property.</span></span> <span data-ttu-id="8759a-240">當您針對相關實體具有一個導覽屬性時，Entity Framework 便不需要您為資料模型新增一個外部索引鍵屬性。</span><span class="sxs-lookup"><span data-stu-id="8759a-240">The Entity Framework doesn't require you to add a foreign key property to your data model when you have a navigation property for a related entity.</span></span> <span data-ttu-id="8759a-241">在需要時會 EF 會自動在資料庫中建立外部索引鍵。</span><span class="sxs-lookup"><span data-stu-id="8759a-241">EF automatically creates foreign keys in the database wherever they are needed.</span></span> <span data-ttu-id="8759a-242">但在資料模型中擁有外部索引鍵，可讓更新變得更為簡單和有效率。</span><span class="sxs-lookup"><span data-stu-id="8759a-242">But having the foreign key in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="8759a-243">例如，當您擷取課程實體，若要編輯，`Department`實體為 null 如果您不載入它，因此當您更新課程實體中，您必須先擷取`Department`實體。</span><span class="sxs-lookup"><span data-stu-id="8759a-243">For example, when you fetch a course entity to edit, the `Department` entity is null if you don't load it, so when you update the course entity, you would have to first fetch the `Department` entity.</span></span> <span data-ttu-id="8759a-244">當外部索引鍵屬性`DepartmentID`是否包含在資料模型，您不需要擷取`Department`實體之後才更新。</span><span class="sxs-lookup"><span data-stu-id="8759a-244">When the foreign key property `DepartmentID` is included in the data model, you don't need to fetch the `Department` entity before you update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="8759a-245">DatabaseGenerated 屬性</span><span class="sxs-lookup"><span data-stu-id="8759a-245">The DatabaseGenerated Attribute</span></span>

<span data-ttu-id="8759a-246">[DatabaseGenerated 屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx)與[無](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx)參數`CourseID`屬性指定主索引鍵值是使用者所提供，而不是不是資料庫產生。</span><span class="sxs-lookup"><span data-stu-id="8759a-246">The [DatabaseGenerated attribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) with the [None](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) parameter on the `CourseID` property specifies that primary key values are provided by the user rather than generated by the database.</span></span>

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

<span data-ttu-id="8759a-247">根據預設，Entity Framework 會假設資料庫會產生主索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="8759a-247">By default, the Entity Framework assumes that primary key values are generated by the database.</span></span> <span data-ttu-id="8759a-248">這是您在大多數案例下所希望的情況。</span><span class="sxs-lookup"><span data-stu-id="8759a-248">That's what you want in most scenarios.</span></span> <span data-ttu-id="8759a-249">不過，對於`Course`實體，您會使用使用者指定課程數字，例如 1000年一連串一個部門，另一個部門，2000年數列等等。</span><span class="sxs-lookup"><span data-stu-id="8759a-249">However, for `Course` entities, you'll use a user-specified course number such as a 1000 series for one department, a 2000 series for another department, and so on.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="8759a-250">外部索引鍵和導覽屬性</span><span class="sxs-lookup"><span data-stu-id="8759a-250">Foreign Key and Navigation Properties</span></span>

<span data-ttu-id="8759a-251">外部索引鍵屬性和導覽屬性中的`Course`實體反映的下列關聯性：</span><span class="sxs-lookup"><span data-stu-id="8759a-251">The foreign key properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

- <span data-ttu-id="8759a-252">課程會指派給一個部門，因此基於上述理由，會有一個 `DepartmentID` 外部索引鍵和一個 `Department` 導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="8759a-252">A course is assigned to one department, so there's a `DepartmentID` foreign key and a `Department` navigation property for the reasons mentioned above.</span></span> 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- <span data-ttu-id="8759a-253">由於課程可由任何數量的學生進行註冊，因此 `Enrollments` 導覽屬性為一個集合：</span><span class="sxs-lookup"><span data-stu-id="8759a-253">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span> 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- <span data-ttu-id="8759a-254">課程可由多個講師進行教授，因此 `Instructors` 導覽屬性為一個集合：</span><span class="sxs-lookup"><span data-stu-id="8759a-254">A course may be taught by multiple instructors, so the `Instructors` navigation property is a collection:</span></span> 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="create-the-department-entity"></a><span data-ttu-id="8759a-255">建立 Department 實體</span><span class="sxs-lookup"><span data-stu-id="8759a-255">Create the Department Entity</span></span>

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

<span data-ttu-id="8759a-257">建立*Models\Department.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="8759a-257">Create *Models\Department.cs* with the following code:</span></span>

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a><span data-ttu-id="8759a-258">資料行屬性</span><span class="sxs-lookup"><span data-stu-id="8759a-258">The Column Attribute</span></span>

<span data-ttu-id="8759a-259">您所用的稍早[資料行屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx)若要變更資料行名稱的對應。</span><span class="sxs-lookup"><span data-stu-id="8759a-259">Earlier you used the [Column attribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) to change column name mapping.</span></span> <span data-ttu-id="8759a-260">在程式碼`Department`實體，`Column`屬性用來變更 SQL 資料類型對應，以便將會定義資料行，使用 SQL Server [money](https://msdn.microsoft.com/library/ms179882.aspx)資料庫中的類型：</span><span class="sxs-lookup"><span data-stu-id="8759a-260">In the code for the `Department` entity, the `Column` attribute is being used to change SQL data type mapping so that the column will be defined using the SQL Server [money](https://msdn.microsoft.com/library/ms179882.aspx) type in the database:</span></span>

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

<span data-ttu-id="8759a-261">資料行對應通常是不必要的因為 Entity Framework 通常會選擇根據您定義屬性的 CLR 類型的適當 SQL Server 資料類型。</span><span class="sxs-lookup"><span data-stu-id="8759a-261">Column mapping is generally not required, because the Entity Framework usually chooses the appropriate SQL Server data type based on the CLR type that you define for the property.</span></span> <span data-ttu-id="8759a-262">CLR `decimal` 類型會對應到 SQL Server 的 `decimal` 類型。</span><span class="sxs-lookup"><span data-stu-id="8759a-262">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="8759a-263">但在此情況下您知道貨幣金額將會保留資料行和[money](https://msdn.microsoft.com/library/ms179882.aspx)資料類型是更適合的。</span><span class="sxs-lookup"><span data-stu-id="8759a-263">But in this case you know that the column will be holding currency amounts, and the [money](https://msdn.microsoft.com/library/ms179882.aspx) data type is more appropriate for that.</span></span> <span data-ttu-id="8759a-264">如需有關 CLR 資料型別和 SQL Server 資料型別如何比對的詳細資訊，請參閱[適用於 Entity Framework 的 SqlClient](https://msdn.microsoft.com/library/bb896344.aspx)。</span><span class="sxs-lookup"><span data-stu-id="8759a-264">For more information about CLR data types and how they match to SQL Server data types, see [SqlClient for Entity FrameworkTypes](https://msdn.microsoft.com/library/bb896344.aspx).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="8759a-265">外部索引鍵和導覽屬性</span><span class="sxs-lookup"><span data-stu-id="8759a-265">Foreign Key and Navigation Properties</span></span>

<span data-ttu-id="8759a-266">外部索引鍵及導覽屬性反映了下列關聯性：</span><span class="sxs-lookup"><span data-stu-id="8759a-266">The foreign key and navigation properties reflect the following relationships:</span></span>

- <span data-ttu-id="8759a-267">部門可以有或沒有一位系統管理員，而系統管理員一律為講師。</span><span class="sxs-lookup"><span data-stu-id="8759a-267">A department may or may not have an administrator, and an administrator is always an instructor.</span></span> <span data-ttu-id="8759a-268">因此`InstructorID`屬性是包含外部索引鍵為`Instructor`後面會加上的實體以及問號`int`輸入指定將標示為可為 null 的屬性。導覽屬性名為`Administrator`但保留`Instructor`實體：</span><span class="sxs-lookup"><span data-stu-id="8759a-268">Therefore the `InstructorID` property is included as the foreign key to the `Instructor` entity, and a question mark is added after the `int` type designation to mark the property as nullable.The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span> 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- <span data-ttu-id="8759a-269">部門可能有許多課程，所以`Courses`導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="8759a-269">A department may have many courses, so there's a `Courses` navigation property:</span></span> 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > <span data-ttu-id="8759a-270">根據慣例，Entity Framework 會為不可為 Null 的外部索引鍵和多對多關聯性啟用串聯刪除。</span><span class="sxs-lookup"><span data-stu-id="8759a-270">By convention, the Entity Framework enables cascade delete for non-nullable foreign keys and for many-to-many relationships.</span></span> <span data-ttu-id="8759a-271">這可能會導致循環串聯刪除規則，並在您嘗試新增移轉時造成例外狀況。</span><span class="sxs-lookup"><span data-stu-id="8759a-271">This can result in circular cascade delete rules, which will cause an exception when you try to add a migration.</span></span> <span data-ttu-id="8759a-272">例如，如果您未定義`Department.InstructorID`為可為 null 的屬性，您會收到下列例外狀況訊息: 「 參考關聯性將會導致不允許循環參考。 」</span><span class="sxs-lookup"><span data-stu-id="8759a-272">For example, if you didn't define the `Department.InstructorID` property as nullable, you'd get the following exception message: "The referential relationship will result in a cyclical reference that's not allowed."</span></span> <span data-ttu-id="8759a-273">如果您的商務規則所需`InstructorID`屬性成為不可為 null，您必須使用下列的 fluent API 陳述式來停用串聯刪除關聯性上：</span><span class="sxs-lookup"><span data-stu-id="8759a-273">If your business rules required `InstructorID` property to be non-nullable, you would have to use the following fluent API statement to disable cascade delete on the relationship:</span></span> 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]


## <a name="modify-the-enrollment-entity"></a><span data-ttu-id="8759a-274">修改註冊實體</span><span class="sxs-lookup"><span data-stu-id="8759a-274">Modify the Enrollment Entity</span></span>

![Enrollment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

 <span data-ttu-id="8759a-276">在*Models\Enrollment.cs*，稍早為下列程式碼取代您所加入的程式碼</span><span class="sxs-lookup"><span data-stu-id="8759a-276">In *Models\Enrollment.cs*, replace the code you added earlier with the following code</span></span>

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=1,15)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="8759a-277">外部索引鍵和導覽屬性</span><span class="sxs-lookup"><span data-stu-id="8759a-277">Foreign Key and Navigation Properties</span></span>

<span data-ttu-id="8759a-278">外部索引鍵屬性及導覽屬性反映了下列關聯性：</span><span class="sxs-lookup"><span data-stu-id="8759a-278">The foreign key properties and navigation properties reflect the following relationships:</span></span>

- <span data-ttu-id="8759a-279">註冊記錄乃針對單一課程，因此當中包含了一個 `CourseID` 外部索引鍵屬性及一個 `Course` 導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="8759a-279">An enrollment record is for a single course, so there's a `CourseID` foreign key property and a `Course` navigation property:</span></span> 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs)]
- <span data-ttu-id="8759a-280">註冊記錄乃針對單一學生，因此當中包含了一個 `StudentID` 外部索引鍵屬性及一個 `Student` 導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="8759a-280">An enrollment record is for a single student, so there's a `StudentID` foreign key property and a `Student` navigation property:</span></span> 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]

### <a name="many-to-many-relationships"></a><span data-ttu-id="8759a-281">多對多關聯性</span><span class="sxs-lookup"><span data-stu-id="8759a-281">Many-to-Many Relationships</span></span>

<span data-ttu-id="8759a-282">多對多關聯性之間`Student`和`Course`實體，而`Enrollment`實體做為多對多聯結資料表*裝載*資料庫中。</span><span class="sxs-lookup"><span data-stu-id="8759a-282">There's a many-to-many relationship between the `Student` and `Course` entities, and the `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="8759a-283">這表示`Enrollment`資料表包含額外的資料以外的聯結資料表的外部索引鍵 (在這個情況下，主索引鍵和`Grade`屬性)。</span><span class="sxs-lookup"><span data-stu-id="8759a-283">This means that the `Enrollment` table contains additional data besides foreign keys for the joined tables (in this case, a primary key and a `Grade` property).</span></span>

<span data-ttu-id="8759a-284">下列圖例展示了在實體圖表中這些關聯性的樣子。</span><span class="sxs-lookup"><span data-stu-id="8759a-284">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="8759a-285">(此圖表使用產生的[Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); 建立圖表不屬於本教學課程，正只為一個實例。)</span><span class="sxs-lookup"><span data-stu-id="8759a-285">(This diagram was generated using the [Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); creating the diagram isn't part of the tutorial, it's just being used here as an illustration.)</span></span>

![Student-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

<span data-ttu-id="8759a-287">每個關聯性線條的一端和星號 1 (\*)，在指出的一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="8759a-287">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="8759a-288">如果`Enrollment`資料表未包含等級資訊，它只需要包含兩個外部索引鍵`CourseID`和`StudentID`。</span><span class="sxs-lookup"><span data-stu-id="8759a-288">If the `Enrollment` table didn't include grade information, it would only need to contain the two foreign keys `CourseID` and `StudentID`.</span></span> <span data-ttu-id="8759a-289">在此情況下，它就會對應至多對多聯結資料表*不裝載*(或*純聯結資料表*) 在資料庫中，以及您不會有完全為它建立模型類別。</span><span class="sxs-lookup"><span data-stu-id="8759a-289">In that case, it would correspond to a many-to-many join table *without payload* (or a *pure join table*) in the database, and you wouldn't have to create a model class for it at all.</span></span> <span data-ttu-id="8759a-290">`Instructor`和`Course`實體具有多對多關聯性，該類型，如您所見，它們之間沒有任何實體類別：</span><span class="sxs-lookup"><span data-stu-id="8759a-290">The `Instructor` and `Course` entities have that kind of many-to-many relationship, and as you can see, there is no entity class between them:</span></span>

![Instructor-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

<span data-ttu-id="8759a-292">聯結資料表所需要的是資料庫，不過，如下列的資料庫圖表中所示：</span><span class="sxs-lookup"><span data-stu-id="8759a-292">A join table is required in the database, however, as shown in the following database diagram:</span></span>

![Instructor-Course_many-to-many_relationship_tables](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

<span data-ttu-id="8759a-294">Entity Framework 會自動建立`CourseInstructor`資料表，以及您讀取和更新的讀取和更新的間接`Instructor.Courses`和`Course.Instructors`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="8759a-294">The Entity Framework automatically creates the `CourseInstructor` table, and you read and update it indirectly by reading and updating the `Instructor.Courses` and `Course.Instructors` navigation properties.</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="8759a-295">顯示關聯性的實體圖表</span><span class="sxs-lookup"><span data-stu-id="8759a-295">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="8759a-296">下列圖例顯示了 Entity Framework Power Tools 為完成的 School 模型建立的圖表。</span><span class="sxs-lookup"><span data-stu-id="8759a-296">The following illustration shows the diagram that the Entity Framework Power Tools create for the completed School model.</span></span>

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

<span data-ttu-id="8759a-298">除了多對多關聯性線條 (\*至\*) 和一個對多關聯性線條 (1 到\*)，這裡可以看到的以 0 或-1 一個關聯線 (1 對 0..1) 之間`Instructor`和`OfficeAssignment`實體和 0-或--一對多關聯性線條 (0..1 對\*) 講師和 Department 實體之間。</span><span class="sxs-lookup"><span data-stu-id="8759a-298">Besides the many-to-many relationship lines (\* to \*) and the one-to-many relationship lines (1 to \*), you can see here the one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities and the zero-or-one-to-many relationship line (0..1 to \*) between the Instructor and Department entities.</span></span>

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a><span data-ttu-id="8759a-299">將程式碼加入至資料庫內容中自訂資料模型</span><span class="sxs-lookup"><span data-stu-id="8759a-299">Customize the Data Model by adding Code to the Database Context</span></span>

<span data-ttu-id="8759a-300">接下來您要加入新的實體來`SchoolContext`類別和自訂的對應使用某些[fluent API](https://msdn.microsoft.com/data/jj591617)呼叫。</span><span class="sxs-lookup"><span data-stu-id="8759a-300">Next you'll add the new entities to the `SchoolContext` class and customize some of the mapping using [fluent API](https://msdn.microsoft.com/data/jj591617) calls.</span></span> <span data-ttu-id="8759a-301">應用程式開發介面是"fluent 應用程式開發，因為它通常由串連一系列的方法呼叫一起成單一陳述式，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="8759a-301">The API is "fluent" because it's often used by stringing a series of method calls together into a single statement, as in the following example:</span></span>

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

<span data-ttu-id="8759a-302">在本教學課程中，您將使用 fluent API 僅適用於資料庫對應時，您不能與屬性。</span><span class="sxs-lookup"><span data-stu-id="8759a-302">In this tutorial you'll use the fluent API only for database mapping that you can't do with attributes.</span></span> <span data-ttu-id="8759a-303">然而，您也可以使用 Fluent API 指定大部分透過屬性可完成的格式、驗證及對應規則。</span><span class="sxs-lookup"><span data-stu-id="8759a-303">However, you can also use the fluent API to specify most of the formatting, validation, and mapping rules that you can do by using attributes.</span></span> <span data-ttu-id="8759a-304">某些屬性 (例如 `MinimumLength`) 無法使用 Fluent API 來套用。</span><span class="sxs-lookup"><span data-stu-id="8759a-304">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="8759a-305">如前所述，`MinimumLength`並不會變更結構描述中，它只適用於用戶端和伺服器端驗證規則</span><span class="sxs-lookup"><span data-stu-id="8759a-305">As mentioned previously, `MinimumLength` doesn't change the schema, it only applies a client and server side validation rule</span></span>

<span data-ttu-id="8759a-306">某些開發人員偏好單獨使用 Fluent API，使其實體類別保持「整潔」。</span><span class="sxs-lookup"><span data-stu-id="8759a-306">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="8759a-307">若想要的話，您也可以混合使用屬性和 Fluent API，並且有一些自訂項目只能使用 Fluent API 完成。但一般來說，建議的做法是在這兩種方法中選擇其中一項，然後盡可能的一致使用該方法。</span><span class="sxs-lookup"><span data-stu-id="8759a-307">You can mix attributes and fluent API if you want, and there are a few customizations that can only be done by using fluent API, but in general the recommended practice is to choose one of these two approaches and use that consistently as much as possible.</span></span>

<span data-ttu-id="8759a-308">若要加入新的實體資料模型，並執行您未使用屬性做的資料庫對應，取代中的程式碼*DAL\SchoolContext.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="8759a-308">To add the new entities to the data model and perform database mapping that you didn't do by using attributes, replace the code in *DAL\SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

<span data-ttu-id="8759a-309">新的陳述式中[OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx)方法，會設定 「 多對多聯結資料表：</span><span class="sxs-lookup"><span data-stu-id="8759a-309">The new statement in the [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) method configures the many-to-many join table:</span></span>

- <span data-ttu-id="8759a-310">之間的多對多關聯性`Instructor`和`Course`實體，程式碼指定聯結資料表的資料表和資料行名稱。</span><span class="sxs-lookup"><span data-stu-id="8759a-310">For the many-to-many relationship between the `Instructor` and `Course` entities, the code specifies the table and column names for the join table.</span></span> <span data-ttu-id="8759a-311">程式碼第一次可以設定多對多關聯性為您沒有這個程式碼，但如果在未呼叫它，就會預設名稱例如`InstructorInstructorID`如`InstructorID`資料行。</span><span class="sxs-lookup"><span data-stu-id="8759a-311">Code First can configure the many-to-many relationship for you without this code, but if you don't call it, you will get default names such as `InstructorInstructorID` for the `InstructorID` column.</span></span>

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

<span data-ttu-id="8759a-312">下列程式碼提供如何您也可以使用 fluent 應用程式開發的應用程式開發介面而非屬性來指定之間的關聯性的範例`Instructor`和`OfficeAssignment`實體：</span><span class="sxs-lookup"><span data-stu-id="8759a-312">The following code provides an example of how you could have used fluent API instead of attributes to specify the relationship between the `Instructor` and `OfficeAssignment` entities:</span></span>

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

<span data-ttu-id="8759a-313">如需 「 fluent API 」 陳述式在幕後的執行資訊，請參閱[Fluent API](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx)部落格文章。</span><span class="sxs-lookup"><span data-stu-id="8759a-313">For information about what "fluent API" statements are doing behind the scenes, see the [Fluent API](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) blog post.</span></span>

## <a name="seed-the-database-with-test-data"></a><span data-ttu-id="8759a-314">使用測試資料植入資料庫</span><span class="sxs-lookup"><span data-stu-id="8759a-314">Seed the Database with Test Data</span></span>

<span data-ttu-id="8759a-315">中的程式碼取代*Migrations\Configuration.cs*檔案取代下列程式碼，以針對您已建立新的實體提供種子資料。</span><span class="sxs-lookup"><span data-stu-id="8759a-315">Replace the code in the *Migrations\Configuration.cs* file with the following code in order to provide seed data for the new entities you've created.</span></span>

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

<span data-ttu-id="8759a-316">如同第一個教學課程中，大部分的這段程式碼只會更新或建立新的實體物件，載入所需的測試內容中的範例資料。</span><span class="sxs-lookup"><span data-stu-id="8759a-316">As you saw in the first tutorial, most of this code simply updates or creates new entity objects and loads sample data into properties as required for testing.</span></span> <span data-ttu-id="8759a-317">不過，請注意如何`Course`具有多對多關聯性的實體與`Instructor`實體，會處理：</span><span class="sxs-lookup"><span data-stu-id="8759a-317">However, notice how the `Course` entity, which has a many-to-many relationship with the `Instructor` entity, is handled:</span></span>

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

<span data-ttu-id="8759a-318">當您建立`Course`物件，您初始化`Instructors`導覽屬性為空集合，使用程式碼`Instructors = new List<Instructor>()`。</span><span class="sxs-lookup"><span data-stu-id="8759a-318">When you create a `Course` object, you initialize the `Instructors` navigation property as an empty collection using the code `Instructors = new List<Instructor>()`.</span></span> <span data-ttu-id="8759a-319">這樣做可新增`Instructor`至此相關實體`Course`使用`Instructors.Add`方法。</span><span class="sxs-lookup"><span data-stu-id="8759a-319">This makes it possible to add `Instructor` entities that are related to this `Course` by using the `Instructors.Add` method.</span></span> <span data-ttu-id="8759a-320">如果您未建立空的清單，您就可以新增這些關聯性，因為`Instructors`屬性會是 null，而且不會有`Add`方法。</span><span class="sxs-lookup"><span data-stu-id="8759a-320">If you didn't create an empty list, you wouldn't be able to add these relationships, because the `Instructors` property would be null and wouldn't have an `Add` method.</span></span> <span data-ttu-id="8759a-321">您也可以加入清單初始化建構函式。</span><span class="sxs-lookup"><span data-stu-id="8759a-321">You could also add the list initialization to the constructor.</span></span>

## <a name="add-a-migration-and-update-the-database"></a><span data-ttu-id="8759a-322">新增的移轉，並更新資料庫</span><span class="sxs-lookup"><span data-stu-id="8759a-322">Add a Migration and Update the Database</span></span>

<span data-ttu-id="8759a-323">從 PMC 輸入`add-migration`命令 (不會做`update-database`尚未命令):</span><span class="sxs-lookup"><span data-stu-id="8759a-323">From the PMC, enter the `add-migration` command (don't do the `update-database` command yet):</span></span>

`add-Migration ComplexDataModel`

<span data-ttu-id="8759a-324">若您嘗試在這個時間點執行 `update-database` 命令，您會接收到下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="8759a-324">If you tried to run the `update-database` command at this point (don't do it yet), you would get the following error:</span></span>

<span data-ttu-id="8759a-325">*與外部索引鍵條件約束的 ALTER TABLE 陳述式衝突 」 FK\_dbo。課程\_dbo。部門\_DepartmentID"。衝突發生在資料庫"ContosoUniversity"，資料表"dbo。部門 」，資料行 'DepartmentID'。*</span><span class="sxs-lookup"><span data-stu-id="8759a-325">*The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK\_dbo.Course\_dbo.Department\_DepartmentID". The conflict occurred in database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.*</span></span>

<span data-ttu-id="8759a-326">有時當您執行移轉以現有的資料，您需要 stub 資料插入資料庫以符合外部索引鍵條件約束，而且，您必須立即執行的作業。</span><span class="sxs-lookup"><span data-stu-id="8759a-326">Sometimes when you execute migrations with existing data, you need to insert stub data into the database to satisfy foreign key constraints, and that's what you have to do now.</span></span> <span data-ttu-id="8759a-327">產生的程式碼中 ComplexDataModel`Up`方法會將不可為 null`DepartmentID`外部索引鍵`Course`資料表。</span><span class="sxs-lookup"><span data-stu-id="8759a-327">The generated code in the ComplexDataModel `Up` method adds a non-nullable `DepartmentID` foreign key to the `Course` table.</span></span> <span data-ttu-id="8759a-328">因為已經有資料列中的`Course`資料表時執行的程式碼，`AddColumn`作業將會失敗，因為 SQL Server 並不知道何值，將置於不可為 null 的資料行。</span><span class="sxs-lookup"><span data-stu-id="8759a-328">Because there are already rows in the `Course` table when the code runs, the `AddColumn` operation will fail because SQL Server doesn't know what value to put in the column that can't be null.</span></span> <span data-ttu-id="8759a-329">因此必須變更為新的資料行提供預設值，並建立名為"Temp"做為預設部門 stub 部門的程式碼。</span><span class="sxs-lookup"><span data-stu-id="8759a-329">Therefore have to change the code to give the new column a default value, and create a stub department named "Temp" to act as the default department.</span></span> <span data-ttu-id="8759a-330">如此一來，現有`Course`相關資料列，將所有會"Temp"部門之後`Up`方法執行。</span><span class="sxs-lookup"><span data-stu-id="8759a-330">As a result, existing `Course` rows will all be related to the "Temp" department after the `Up` method runs.</span></span> <span data-ttu-id="8759a-331">您可以將它們產生關聯到正確的部門中`Seed`方法。</span><span class="sxs-lookup"><span data-stu-id="8759a-331">You can relate them to the correct departments in the `Seed` method.</span></span>

<span data-ttu-id="8759a-332">編輯&lt;*時間戳記&gt;\_ComplexDataModel.cs*檔標記為註解加入 Course 資料表中，[DepartmentID] 欄的程式碼行並加入下列反白顯示程式碼 （加上註解線條會也反白顯示）：</span><span class="sxs-lookup"><span data-stu-id="8759a-332">Edit the &lt;*timestamp&gt;\_ComplexDataModel.cs* file, comment out the line of code that adds the DepartmentID column to the Course table, and add the following highlighted code (the commented line is also highlighted):</span></span>

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

<span data-ttu-id="8759a-333">當`Seed`方法執行時，它會插入資料列中的`Department`資料表和它將與現有`Course`這些新的資料列`Department`資料列。</span><span class="sxs-lookup"><span data-stu-id="8759a-333">When the `Seed` method runs, it will insert rows in the `Department` table and it will relate existing `Course` rows to those new `Department` rows.</span></span> <span data-ttu-id="8759a-334">如果您尚未加入任何課程 UI 中，您然後不再需要"Temp"部門或預設值在`Course.DepartmentID`資料行。</span><span class="sxs-lookup"><span data-stu-id="8759a-334">If you haven't added any courses in the UI, you would then no longer need the "Temp" department or the default value on the `Course.DepartmentID` column.</span></span> <span data-ttu-id="8759a-335">若要允許的可能性，有人可能已經加入課程使用的應用程式，您也要更新`Seed`方法程式碼，請確認所有`Course`資料列 (不只是由先前執行的插入`Seed`方法) 有有效`DepartmentID`之前移除預設的值從資料行值，並刪除"Temp"部門。</span><span class="sxs-lookup"><span data-stu-id="8759a-335">To allow for the possibility that someone might have added courses by using the application, you'd also want to update the `Seed` method code to ensure that all `Course` rows (not just the ones inserted by earlier runs of the `Seed` method) have valid `DepartmentID` values before you remove the default value from the column and delete the "Temp" department.</span></span>

<span data-ttu-id="8759a-336">當您完成編輯之後&lt;*時間戳記&gt;\_ComplexDataModel.cs*檔案中，輸入`update-database`執行移轉 PMC 命令。</span><span class="sxs-lookup"><span data-stu-id="8759a-336">After you have finished editing the &lt;*timestamp&gt;\_ComplexDataModel.cs* file, enter the `update-database` command in the PMC to execute the migration.</span></span>

[!code-powershell[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.ps1)]

> [!NOTE]
> <span data-ttu-id="8759a-337">很可能在移轉資料和進行結構描述變更時，取得其他錯誤。</span><span class="sxs-lookup"><span data-stu-id="8759a-337">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="8759a-338">如果收到無法解決的移轉錯誤，您可以變更連接字串中的資料庫名稱，或刪除該資料庫。</span><span class="sxs-lookup"><span data-stu-id="8759a-338">If you get migration errors you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="8759a-339">最簡單的方法是在資料庫重新命名*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="8759a-339">The simplest approach is to rename the database in *Web.config* file.</span></span> <span data-ttu-id="8759a-340">下列範例顯示名稱變更為 CU\_測試：</span><span class="sxs-lookup"><span data-stu-id="8759a-340">The following example shows the name changed to CU\_Test:</span></span>
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample36.xml?highlight=1)]
> 
> <span data-ttu-id="8759a-341">使用新資料庫時，沒有資料移轉，而`update-database`命令是更有可能能順利完成。</span><span class="sxs-lookup"><span data-stu-id="8759a-341">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="8759a-342">如需有關如何刪除資料庫的指示，請參閱[如何卸除資料庫，從 Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)。</span><span class="sxs-lookup"><span data-stu-id="8759a-342">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span>
> 
> <span data-ttu-id="8759a-343">如果失敗，另一個您可以嘗試的動作是在 PMC 中輸入下列命令以重新初始化資料庫：</span><span class="sxs-lookup"><span data-stu-id="8759a-343">If that fails, another thing you can try is re-initialize the database by entering the following command in the PMC:</span></span>
> 
> `update-database -TargetMigration:0`


<span data-ttu-id="8759a-344">開啟中的資料庫**伺服器總管**像之前，並展開**資料表**節點以查看所有資料表具有已建立。</span><span class="sxs-lookup"><span data-stu-id="8759a-344">Open the database in **Server Explorer** as you did earlier, and expand the **Tables** node to see that all of the tables have been created.</span></span> <span data-ttu-id="8759a-345">(如果仍有**伺服器總管**稍早的時間從開啟中，按一下**重新整理** 按鈕。)</span><span class="sxs-lookup"><span data-stu-id="8759a-345">(If you still have **Server Explorer** open from the earlier time, click the **Refresh** button.)</span></span>

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image16.png)

<span data-ttu-id="8759a-346">您未建立的模型類別`CourseInstructor`資料表。</span><span class="sxs-lookup"><span data-stu-id="8759a-346">You didn't create a model class for the `CourseInstructor` table.</span></span> <span data-ttu-id="8759a-347">如前所述，這是一個聯結資料表之間的多對多關聯性`Instructor`和`Course`實體。</span><span class="sxs-lookup"><span data-stu-id="8759a-347">As explained earlier, this is a join table for the many-to-many relationship between the `Instructor` and `Course` entities.</span></span>

<span data-ttu-id="8759a-348">以滑鼠右鍵按一下`CourseInstructor`資料表，然後選取**顯示資料表資料**驗證中它可能是有資料`Instructor`實體加入至`Course.Instructors`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="8759a-348">Right-click the `CourseInstructor` table and select **Show Table Data** to verify that it has data in it as a result of the `Instructor` entities you added to the `Course.Instructors` navigation property.</span></span>

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image17.png)

## <a name="summary"></a><span data-ttu-id="8759a-350">總結</span><span class="sxs-lookup"><span data-stu-id="8759a-350">Summary</span></span>

<span data-ttu-id="8759a-351">您現在已有了更複雜的資料模型和對應的資料庫。</span><span class="sxs-lookup"><span data-stu-id="8759a-351">You now have a more complex data model and corresponding database.</span></span> <span data-ttu-id="8759a-352">下列教學課程中您會深入了解不同的方式來存取相關的資料。</span><span class="sxs-lookup"><span data-stu-id="8759a-352">In the following tutorial you'll learn more about different ways to access related data.</span></span>

<span data-ttu-id="8759a-353">請在您所喜歡本教學課程的方式，我們可以改進留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="8759a-353">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="8759a-354">您也可以要求在新的主題[顯示我如何使用程式碼](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。</span><span class="sxs-lookup"><span data-stu-id="8759a-354">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span>

<span data-ttu-id="8759a-355">Entity Framework 中的其他資源連結位於[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="8759a-355">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8759a-356">[上一頁](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一頁](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="8759a-356">[Previous](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span></span>
