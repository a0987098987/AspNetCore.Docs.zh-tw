---
title: "在表單的 ASP.NET Core 標記協助程式"
author: rick-anderson
description: "描述搭配表單的標記協助程式的內建。"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/working-with-forms
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9fd51755e1dc9a1dfb9ab5cc4558f7da9475ce32
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-using-tag-helpers-in-forms-in-aspnet-core"></a>在表單的 ASP.NET Core 使用標記協助程式簡介

由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Dave Paquette](https://twitter.com/Dave_Paquette)，和[Jerrie Pelser](https://github.com/jerriep)

本文件將示範使用表單和常用在表單上的 HTML 項目。 HTML[表單](https://www.w3.org/TR/html401/interact/forms.html)項目會提供張貼至伺服器的備份資料的主要機制 web 應用程式使用。 大部分的這份文件描述[標記協助程式](tag-helpers/intro.md)及其如何協助您有效率地建立強大的 HTML 表單。 我們建議您先閱讀[標記協助程式簡介](tag-helpers/intro.md)閱讀這份文件之前。

在許多情況下，HTML Helper 提供的替代方式給特定的標記協助程式，但請務必識別標記協助程式不會取代 HTML Helper，並不是標記協助程式的每個 HTML Helper。 替代的 HTML Helper 存在時，它被提及。

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a>表單標記協助程式

[表單](https://www.w3.org/TR/html401/interact/forms.html)標記協助程式：

* 會產生 HTML [\<表單 >](https://www.w3.org/TR/html401/interact/forms.html) `action`之 MVC 控制器動作或具名的路由的屬性值

* 產生隱藏[要求驗證語彙基元](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)以防止跨站台要求偽造 (搭配使用時`[ValidateAntiForgeryToken]`HTTP Post 動作方法中的屬性)

* 提供`asp-route-<Parameter Name>`屬性，其中`<Parameter Name>`新增至路由值。 `routeValues`參數`Html.BeginForm`和`Html.BeginRouteForm`提供類似的功能。

* 有替代的 HTML Helper`Html.BeginForm`和`Html.BeginRouteForm`

範例：

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

上述表單標記協助程式會產生下列 HTML:

```HTML
<form method="post" action="/Demo/Register">
     <!-- Input and Submit elements -->
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

MVC 執行階段產生`action`屬性值的表單標記協助程式屬性`asp-controller`和`asp-action`。 表單標記協助程式也會產生隱藏[要求驗證語彙基元](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)以防止跨站台要求偽造 (搭配使用時`[ValidateAntiForgeryToken]`HTTP Post 動作方法中的屬性)。 防止跨站台要求偽造的純 HTML 表單是很困難，表單標記協助程式提供此服務。

### <a name="using-a-named-route"></a>使用具名的路由

`asp-route`標記協助程式屬性也可以產生標記的 html`action`屬性。 應用程式，但[路由](../../fundamentals/routing.md)名為`register`可以使用 [註冊] 頁面的下列標記：

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

中的檢視表的許多*檢視/帳戶*資料夾 (當您建立新的 web 應用程式，以產生*個別使用者帳戶*) 包含[asp-路由-returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms)屬性：

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
>內建的範本， `returnUrl` ，才會填入自動當您嘗試存取授權的資源，但不是驗證或授權。 當您嘗試未經授權的存取時，安全性的中介軟體您重新導向到的登入頁面`returnUrl`設定。

## <a name="the-input-tag-helper"></a>輸入的標記協助程式

輸入標記協助程式繫結 HTML [\<輸入 >](https://www.w3.org/wiki/HTML/Elements/input)模型中的運算式 razor 檢視的元素。

語法：

```HTML
<input asp-for="<Expression Name>" />
```

輸入的標記協助程式：

* 會產生`id`和`name`HTML 屬性中指定的運算式名稱`asp-for`屬性。 `asp-for="Property1.Property2"` 相當於 `m => m.Property1.Property2`。 運算式的名稱是用途`asp-for`屬性值。 請參閱[運算式名稱](#expression-names)一節以取得其他資訊。

* 設定的 HTML`type`屬性值為基礎的模型型別和[資料註解](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)屬性套用至模型屬性

* 將不會覆寫 HTML`type`時指定的其中一個屬性值

* 會產生[HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)驗證屬性從[資料註解](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)屬性套用至模型屬性

* 具有重疊的 HTML Helper 功能`Html.TextBoxFor`和`Html.EditorFor`。 請參閱**輸入標記協助程式的 HTML Helper 替代**如需詳細資訊。

* 提供強型別。 如果名稱屬性變更，而且您沒有更新標記協助程式會出現的錯誤如下所示：

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

`Input`標記協助程式設定的 HTML`type`屬性根據.NET 類型。 下表列出一些常見的.NET 型別和產生的 HTML 型別 （不是每個.NET 型別列出）。

|.NET 型別|輸入的類型|
|---|---|
|Bool|type=”checkbox”|
|String|type=”text”|
|DateTime|type=”datetime”|
|Byte|type=”number”|
|Int|type=”number”|
|Single、Double|type=”number”|


下表顯示一些常見[資料註解](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)屬性輸入的標記協助程式將會對應至特定的輸入類型 （並非所有驗證屬性會都列出）：


|屬性|輸入的類型|
|---|---|
|[EmailAddress]|type=”email”|
|[Url]|type=”url”|
|[HiddenInput]|type=”hidden”|
|[Phone]|type=”tel”|
|[DataType(DataType.Password)]| type=”password”|
|[DataType(DataType.Date)]| type=”date”|
|[DataType(DataType.Time)]| type=”time”|


範例：

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

上述程式碼會產生下列 HTML:

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid e-mail address."
              data-val-required="The Email Address field is required."
              id="Email" name="Email" value="" /> <br>
       Password:
       <input type="password" data-val="true"
              data-val-required="The Password field is required."
              id="Password" name="Password" /><br>
       <button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

套用至資料註解`Email`和`Password`屬性產生模型的中繼資料。 輸入標記協助程式會消耗模型中繼資料，並產生[HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*`屬性 (請參閱[模型驗證](../models/validation.md))。 這些屬性描述附加至輸入欄位的驗證。 這會提供不顯眼的 HTML5 和[jQuery](https://jquery.com/)驗證。 不顯眼的屬性具有格式`data-val-rule="Error Message"`，其中的規則是驗證規則的名稱 (例如`data-val-required`， `data-val-email`，`data-val-maxlength`等。)如果屬性中提供的錯誤訊息，就會顯示的值為`data-val-rule`屬性。 另外還有的表單屬性`data-val-ruleName-argumentName="argumentValue"`可提供其他詳細資料的規則，比方說， `data-val-maxlength-max="1024"` 。

### <a name="html-helper-alternatives-to-input-tag-helper"></a>輸入標記協助程式的 HTML Helper 替代項目

`Html.TextBox``Html.TextBoxFor`，`Html.Editor`和`Html.EditorFor`具有重疊的功能與輸入標記協助程式。 輸入標記協助程式將會自動設定`type`屬性;`Html.TextBox`和`Html.TextBoxFor`將不會。 `Html.Editor`和`Html.EditorFor`處理集合、 複雜的物件和範本; 不會輸入標記協助程式。 輸入標記協助程式`Html.EditorFor`和`Html.TextBoxFor`強型別 （使用 lambda 運算式）。`Html.TextBox`和`Html.Editor`不 （其使用的運算式名稱）。

### <a name="htmlattributes"></a>HtmlAttributes

`@Html.Editor()`和`@Html.EditorFor()`使用特殊`ViewDataDictionary`名為項目`htmlAttributes`時執行它們的預設範本。 此行為選擇性使用來增強`additionalViewData`參數。 「 HtmlAttributes"的索引鍵不區分大小寫。 索引鍵 」 htmlAttributes 」 類似於處理`htmlAttributes`物件傳遞到輸入像 helper `@Html.TextBox()`。

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a>運算式名稱

`asp-for`屬性值是`ModelExpression`和 lambda 運算式的右邊。 因此，`asp-for="Property1"`變成`m => m.Property1`中產生的程式碼，這也是為什麼您不需要加上前置詞`Model`。 您可以使用"@"字元在內嵌運算式的開頭，並移動後，再`m.`:

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

會產生下列訊息：

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

集合屬性時，與`asp-for="CollectionProperty[23].Member"`會產生相同的名稱`asp-for="CollectionProperty[i].Member"`時`i`值`23`。

### <a name="navigating-child-properties"></a>瀏覽子屬性

您也可以瀏覽至使用檢視模型的屬性路徑的子屬性。 請考慮更複雜的模型類別，其中包含子系`Address`屬性。

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

在檢視中，我們將繫結至`Address.AddressLine1`:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

下列 HTML 會產生`Address.AddressLine1`:

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a>運算式名稱和集合

範例中，包含陣列的模型`Colors`:

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

動作方法：

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

下列的 Razor 示範如何存取特定`Color`項目：

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

*Views/Shared/EditorTemplates/String.cshtml*範本：

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

範例使用`List<T>`:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

下列的 Razor 顯示如何反覆查看的集合：

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

*Views/Shared/EditorTemplates/ToDoItem.cshtml*範本：

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
>一律使用`for`(和*不* `foreach`) 來逐一查看清單。 運算式評估在 LINQ 中的索引子可能會耗費資源，應該降到最低。

&nbsp;

>[!NOTE]
>上述加上註解的範例程式碼會示範如何，您必須取代的 lambda 運算式`@`運算子來存取每個`ToDoItem`清單中。

## <a name="the-textarea-tag-helper"></a>Textarea 標記協助程式

`Textarea Tag Helper`標記協助程式是輸入標記協助程式類似。

* 會產生`id`和`name`屬性和資料的驗證屬性的模型從[ \<textarea >](https://www.w3.org/wiki/HTML/Elements/textarea)項目。

* 提供強型別。

* HTML Helper 替代程序：`Html.TextAreaFor`

範例：

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

會產生下列 HTML:

```HTML
<form method="post" action="/Demo/RegisterTextArea">
  <textarea data-val="true"
   data-val-maxlength="The field Description must be a string or array type with a maximum length of &#x27;1024&#x27;."
   data-val-maxlength-max="1024"
   data-val-minlength="The field Description must be a string or array type with a minimum length of &#x27;5&#x27;."
   data-val-minlength-min="5"
   id="Description" name="Description">
  </textarea>
  <button type="submit">Test</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-label-tag-helper"></a>標籤標記協助程式

* 會產生標籤標題和`for`屬性[ <label> ](https://www.w3.org/wiki/HTML/Elements/label)運算式名稱的項目

* HTML Helper 的替代方式： `Html.LabelFor`。

`Label Tag Helper`純 HTML label 項目透過提供下列優點：

* 您會自動取得的描述性標籤值`Display`屬性。 預定的顯示名稱可能會隨著時間和的組合`Display`屬性和標籤標記協助程式會套用`Display`地方使用它。

* 更少原始程式碼中的標記

* 強型別與模型屬性。

範例：

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

下列 HTML 會產生`<label>`項目：

```HTML
<label for="Email">Email Address</label>
```

產生的標籤標記協助程式`for`與相關聯的屬性值，"Email"，這是識別碼`<input>`項目。 標記協助程式產生一致`id`和`for`以便能夠正確地建立關聯的項目。 在此範例中的標題來自`Display`屬性。 如果模型未包含`Display`屬性標題會是運算式的屬性名稱。

## <a name="the-validation-tag-helpers"></a>驗證標記協助程式

有兩個驗證標記協助程式。 `Validation Message Tag Helper` （這會顯示單一屬性的驗證訊息上您的模型），而`Validation Summary Tag Helper`（這會顯示驗證錯誤的摘要）。 `Input Tag Helper`新增 HTML5 用戶端端輸入元素為基礎的資料模型類別上的註釋屬性的驗證屬性。 伺服器上，也會執行驗證。 驗證標記協助程式在發生驗證錯誤時，會顯示這些錯誤訊息。

### <a name="the-validation-message-tag-helper"></a>驗證訊息標記協助程式

* 新增[HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"`屬性[跨越](https://developer.mozilla.org/docs/Web/HTML/Element/span)元素，其會附加在指定的模型屬性的輸入欄位的驗證錯誤訊息。 用戶端端驗證錯誤時， [jQuery](https://jquery.com/)顯示中的錯誤訊息`<span>`項目。

* 驗證也會在伺服器上的位置。 用戶端可能具有停用 JavaScript 和一些驗證只能在伺服器端。

* HTML Helper 替代程序：`Html.ValidationMessageFor`

`Validation Message Tag Helper`搭配`asp-validation-for`上的 HTML 屬性[跨越](https://developer.mozilla.org/docs/Web/HTML/Element/span)項目。

```HTML
<span asp-validation-for="Email"></span>
```

驗證訊息標記協助程式將會產生下列 HTML:

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

您通常會使用`Validation Message Tag Helper`之後`Input`標記協助程式針對相同的屬性。 這麼做會顯示附近造成錯誤的輸入任何驗證錯誤訊息。

> [!NOTE]
> 您必須使用正確的 JavaScript 檢視和[jQuery](https://jquery.com/)指令碼參考在用戶端驗證的位置。 請參閱[模型驗證](../models/validation.md)如需詳細資訊。

伺服器端驗證錯誤，就會發生 （例如當您自訂伺服器端驗證用戶端驗證已停用），MVC 會放在該錯誤訊息的主體為`<span>`項目。

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a>驗證摘要的標記協助程式

* 目標`<div>`的項目`asp-validation-summary`屬性

* HTML Helper 替代程序：`@Html.ValidationSummary`

`Validation Summary Tag Helper`用來顯示驗證訊息的摘要。 `asp-validation-summary`屬性值可以是下列任一項：

|asp-validation-summary|顯示驗證訊息|
|--- |--- |
|ValidationSummary.All|屬性和模型層級|
|ValidationSummary.ModelOnly|型號|
|ValidationSummary.None|無|

### <a name="sample"></a>範例

在下列範例中，資料模型以裝飾`DataAnnotation`產生驗證錯誤訊息的屬性`<input>`項目。  發生驗證錯誤時，驗證標記協助程式會顯示錯誤訊息：

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

產生的 HTML （如果模型有效）：

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid e-mail address."
   data-val="true"> <br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-select-tag-helper"></a>選取標記協助程式

* 會產生[選取](https://www.w3.org/wiki/HTML/Elements/select)以及相關聯[選項](https://www.w3.org/wiki/HTML/Elements/option)屬性的模型項目。

* 有替代的 HTML Helper`Html.DropDownListFor`和`Html.ListBoxFor`

`Select Tag Helper` `asp-for`指定模型的屬性名稱，如[選取](https://www.w3.org/wiki/HTML/Elements/select)項目和`asp-items`指定[選項](https://www.w3.org/wiki/HTML/Elements/option)項目。  例如: 

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

範例：

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

`Index`方法初始化`CountryViewModel`、 設定選取的國家/地區，並將其傳遞給`Index`檢視。

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

HTTP POST`Index`方法會顯示選取項目：

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

`Index`檢視：

[!code-cshtml[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

如此就會產生 （具有"CA"選取) 下列 HTML:

```html
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

> [!NOTE]
> 我們不建議使用`ViewBag`或`ViewData`與選取的標記協助程式。 檢視模型是較為提供 MVC 中繼資料以及通常較不容易發生問題。

`asp-for`屬性值是特殊案例，而不需要`Model`首碼，其他標記協助程式屬性般 (例如`asp-items`)

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a>列舉繫結

通常很方便使用`<select>`與`enum`屬性，並產生`SelectListItem`項目從`enum`值。

範例：

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

`GetEnumSelectList`方法會產生`SelectList`列舉的物件。

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

您可以裝飾的列舉值清單`Display`屬性，以取得更豐富的 UI:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

會產生下列 HTML:

```HTML
  <form method="post" action="/Home/IndexEnum">
         <select data-val="true" data-val-required="The EnumCountry field is required."
                 id="EnumCountry" name="EnumCountry">
             <option value="0">United Mexican States</option>
             <option value="1">United States of America</option>
             <option value="2">Canada</option>
             <option value="3">France</option>
             <option value="4">Germany</option>
             <option selected="selected" value="5">Spain</option>
         </select>
         <br /><button type="submit">Register</button>
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

### <a name="option-group"></a>群組選項

HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup)檢視模型包含一或多個時，會產生元素`SelectListGroup`物件。

`CountryViewModelGroup`群組`SelectListItem`"北美"和"Europe"分組的項目：

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

兩個群組如下所示：

![選項群組範例](working-with-forms/_static/grp.png)

產生的 HTML:

```HTML
 <form method="post" action="/Home/IndexGroup">
      <select id="Country" name="Country">
          <optgroup label="North America">
              <option value="MEX">Mexico</option>
              <option value="CAN">Canada</option>
              <option value="US">USA</option>
          </optgroup>
          <optgroup label="Europe">
              <option value="FR">France</option>
              <option value="ES">Spain</option>
              <option value="DE">Germany</option>
          </optgroup>
      </select>
      <br /><button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
```

### <a name="multiple-select"></a>多個選取

選取標記協助程式會自動產生[多個 ="多個 「](http://w3c.github.io/html-reference/select.html)屬性中指定的屬性，如果`asp-for`屬性是`IEnumerable`。 例如，假設下列模型：

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

使用下列檢視：

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

會產生下列 HTML:

```HTML
<form method="post" action="/Home/IndexMultiSelect">
    <select id="CountryCodes"
    multiple="multiple"
    name="CountryCodes"><option value="MX">Mexico</option>
<option value="CA">Canada</option>
<option value="US">USA</option>
<option value="FR">France</option>
<option value="ES">Spain</option>
<option value="DE">Germany</option>
</select>
    <br /><button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

### <a name="no-selection"></a>沒有選取項目

如果您發現自己使用多個頁面中的 「 未指定"選項，您可以建立範本，以便消除重複的 HTML:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

*Views/Shared/EditorTemplates/CountryViewModel.cshtml*範本：

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

將 HTML 加入[\<選項 >](https://www.w3.org/wiki/HTML/Elements/option)項目並不限於*沒有選取項目*案例。 例如，下列檢視和動作方法會產生 HTML 類似於上述程式碼：

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

正確`<option>`將選取項目 (包含`selected="selected"`屬性) 根據目前`Country`值。

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
 ```

## <a name="additional-resources"></a>其他資源

* [標記協助程式](tag-helpers/intro.md)

* [HTML 表單元素](https://www.w3.org/TR/html401/interact/forms.html)

* [要求驗證語彙基元](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)

* [模型繫結](../models/model-binding.md)

* [模型驗證](../models/validation.md)

* [資料註解](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)

* [程式碼片段，這份文件](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample)。
