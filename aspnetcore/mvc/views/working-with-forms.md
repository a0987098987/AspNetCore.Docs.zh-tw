---
title: ASP.NET Core 表單中的標籤協助程式
author: rick-anderson
description: 描述搭配表單使用的內建標籤協助程式。
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: mvc/views/working-with-forms
ms.openlocfilehash: 380d47d33706b3197dba3b9f7e3e1f186e27115f
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64890813"
---
# <a name="tag-helpers-in-forms-in-aspnet-core"></a>ASP.NET Core 表單中的標籤協助程式

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[N. Taylor Mullen](https://github.com/NTaylorMullen)、[Dave Paquette](https://twitter.com/Dave_Paquette) 和 [Jerrie Pelser](https://github.com/jerriep)

本文件示範使用表單和常用在表單上的 HTML 項目。 HTML [表單](https://www.w3.org/TR/html401/interact/forms.html)項目提供用來將資料張貼回伺服器的主要機制 Web 應用程式。 本文件的大部分在描述[標籤協助程式](tag-helpers/intro.md)，以及它們如何協助您有效率地建立強大的 HTML 表單。 我們建議您先閱讀[標籤協助程式簡介](tag-helpers/intro.md)，然後才閱讀這份文件。

在許多情況下，HTML 協助程式都會提供特定標籤協助程式的替代方式，但請務必辨識標籤協助程式未取代 HTML 協助程式，而且每個 HTML 協助程式都沒有標籤協助程式。 有 HTML 協助程式替代存在時，便會予以提及。

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a>表單標籤協助程式

[表單](https://www.w3.org/TR/html401/interact/forms.html)標籤協助程式：

* 產生 MVC 控制器動作或具名路由的 HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) `action` 屬性值

* 產生隱藏的[要求驗證權杖](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)，以防止跨站台要求偽造 (搭配 HTTP Post 動作方法中的 `[ValidateAntiForgeryToken]` 屬性使用時)

* 提供 `asp-route-<Parameter Name>` 屬性，其中 `<Parameter Name>` 新增至路由值。 `Html.BeginForm` 和 `Html.BeginRouteForm` 的 `routeValues` 參數提供類似的功能。

* 有 HTML 協助程式的替代 `Html.BeginForm` 和 `Html.BeginRouteForm`

範例：

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

上述表單標籤協助程式會產生下列 HTML：

```HTML
<form method="post" action="/Demo/Register">
    <!-- Input and Submit elements -->
    <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
</form>
```

MVC 執行階段會從表單標籤協助程式屬性 `asp-controller` 和 `asp-action` 產生 `action` 屬性值。 表單標籤協助程式也會產生隱藏的[要求驗證權杖](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)，以防止跨站台要求偽造 (搭配 HTTP Post 動作方法中的 `[ValidateAntiForgeryToken]` 屬性使用時)。 保護純粹的 HTML 表單抵禦跨站台要求偽造很困難，而表單標籤協助程式為您提供此服務。

### <a name="using-a-named-route"></a>使用具名路由

`asp-route` 標籤協助程式屬性也可以產生 HTML `action` 屬性的標記。 具有名為 `register` 之[路由](../../fundamentals/routing.md)的應用程式，可能針對註冊頁面使用下列標記：

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

*Views/Account* 資料夾 (當您建立具有「個別使用者帳戶」的新 Web 應用程式時產生) 中的許多檢視表，包含 [asp-route-returnurl](xref:mvc/views/working-with-forms) 屬性：

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
>使用內建的範本，`returnUrl` 只會在您嘗試存取授權的資源，但未經過驗證或授權時才自動填入。 當您嘗試未經授權的存取時，安全性中介軟體會將您重新導向到登入頁面，並設定 `returnUrl`。

## <a name="the-form-action-tag-helper"></a>表單動作標記協助程式

表單動作標記協助程式會在產生的 `<button ...>` 或 `<input type="image" ...>` 標記上產生 `formaction` 屬性。 `formaction` 屬性可讓您控制表單提交其資料的位置。 它會繫結至類型 `image` 的 [\<input>](https://www.w3.org/wiki/HTML/Elements/input) 元素和 [\<button>](https://www.w3.org/wiki/HTML/Elements/button) 元素。 表單動作標記協助程式允許使用多個 [AnchorTagHelper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) `asp-` 屬性來控制會為相應元素產生哪個 `formaction` 連結。

支援 [AnchorTagHelper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) 屬性來控制 `formaction` 的值：

|屬性|說明|
|---|---|
|[asp-controller](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-controller)|控制器的名稱。|
|[asp-action](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-action)|動作方法的名稱。|
|[asp-area](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-area)|區域的名稱。|
|[asp-page](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-page)|Razor 頁面的名稱。|
|[asp-page-handler](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-page-handler)|Razor 頁面處理常式的名稱。|
|[asp-route](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-route)|路由的名稱。|
|[asp-route-{value}](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-route-value)|單一 URL 路由值。 例如，`asp-route-id="1234"`。|
|[asp-all-route-data](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-all-route-data)|所有路由值。|
|[asp-fragment](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-fragment)|URL 片段。|

### <a name="submit-to-controller-example"></a>提交至控制器範例

輸入或選取按鈕時，下列標記會將表單提交到 `HomeController` 的 `Index` 動作：

```cshtml
<form method="post">
    <button asp-controller="Home" asp-action="Index">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" asp-controller="Home" 
                                asp-action="Index">
</form>
```

上述標記會產生下列 HTML：

```html
<form method="post">
    <button formaction="/Home">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" formaction="/Home">
</form>
```

### <a name="submit-to-page-example"></a>提交至頁面範例

下列標記會將表單提交到 `About` Razor 頁面：

```cshtml
<form method="post">
    <button asp-page="About">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" asp-page="About">
</form>
```

上述標記會產生下列 HTML：

```html
<form method="post">
    <button formaction="/About">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" formaction="/About">
</form>
```

### <a name="submit-to-route-example"></a>提交至路由範例

請考量 `/Home/Test` 端點：

```csharp
public class HomeController : Controller
{
    [Route("/Home/Test", Name = "Custom")]
    public string Test()
    {
        return "This is the test page";
    }
}
```

下列標記會將表單提交到 `/Home/Test` 端點。

```cshtml
<form method="post">
    <button asp-route="Custom">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" asp-route="Custom">
</form>
```

上述標記會產生下列 HTML：

```html
<form method="post">
    <button formaction="/Home/Test">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" formaction="/Home/Test">
</form>
```

## <a name="the-input-tag-helper"></a>輸入標籤協助程式

輸入標籤協助程式會將 HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) 項目繫結到 Razor 檢視中的模型運算式。

語法：

```HTML
<input asp-for="<Expression Name>">
```

輸入標籤協助程式：

* 會為 `asp-for` 屬性中指定的運算式名稱產生 `id` 和 `name` HTML 屬性。 `asp-for="Property1.Property2"` 相當於 `m => m.Property1.Property2`。 運算式的名稱用於 `asp-for` 屬性值。 請參閱[運算式名稱](#expression-names)一節以取得其他資訊。

* 根據套用至模型屬性的模型類型和[資料註解](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)屬性，設定 HTML `type` 屬性值

* 已指定 HTML `type` 屬性值時不會予以覆寫

* 從套用至模型屬性的[資料註解](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)屬性產生 [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) 驗證屬性

* `Html.TextBoxFor` 和 `Html.EditorFor` 具有 HTML 協助程式功能重疊。 如需詳細資訊，請參閱**輸入標籤協助程式的 HTML 協助程式替代**。

* 提供強型別。 如果屬性的名稱變更，而您未更新標籤協助程式，則會出現如下所示的錯誤：

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

`Input` 標籤協助程式會根據 .NET 型別設定 HTML `type` 屬性。 下表列出一些常見的 .NET 型別和產生的 HTML 型別 (不是每個 .NET 類型都列出)。

|.NET 型別|輸入類型|
|---|---|
|Bool|type="checkbox"|
|String|type="text"|
|DateTime|type=["datetime-local"](https://developer.mozilla.org/docs/Web/HTML/Element/input/datetime-local)|
|Byte|type="number"|
|Int|type="number"|
|Single、Double|type="number"|

下表顯示輸入標籤協助程式將對應至特定的輸入類型的一些常見[資料註解](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)屬性 (不是每個驗證屬性都列出)：

|屬性|輸入類型|
|---|---|
|[EmailAddress]|type="email"|
|[Url]|type="url"|
|[HiddenInput]|type="hidden"|
|[Phone]|type="tel"|
|[DataType(DataType.Password)]|type="password"|
|[DataType(DataType.Date)]|type="date"|
|[DataType(DataType.Time)]|type="time"|

範例：

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

上述程式碼會產生下列 HTML：

```HTML
  <form method="post" action="/Demo/RegisterInput">
      Email:
      <input type="email" data-val="true"
             data-val-email="The Email Address field is not a valid email address."
             data-val-required="The Email Address field is required."
             id="Email" name="Email" value=""><br>
      Password:
      <input type="password" data-val="true"
             data-val-required="The Password field is required."
             id="Password" name="Password"><br>
      <button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
   </form>
```

套用至 `Email` 和 `Password` 屬性的資料註解會產生模型的中繼資料。 輸入標籤協助程式會取用模型中繼資料，並產生 [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` 屬性 (請參閱[模型驗證](../models/validation.md))。 這些屬性描述要附加至輸入欄位的驗證程式。 這提供低調的 HTML5 和 [jQuery](https://jquery.com/) 驗證。 低調屬性具有格式 `data-val-rule="Error Message"`，其中 rule 是驗證規則的名稱 (例如 `data-val-required`、`data-val-email`、`data-val-maxlength` 等。)如果屬性中提供錯誤訊息，就會顯示為 `data-val-rule` 屬性的值。 另外還有 `data-val-ruleName-argumentName="argumentValue"` 格式的屬性，提供規則的其他詳細資料，例如 `data-val-maxlength-max="1024"`。

### <a name="html-helper-alternatives-to-input-tag-helper"></a>輸入標籤協助程式的 HTML 標記替代

`Html.TextBox`、`Html.TextBoxFor`、`Html.Editor` 和 `Html.EditorFor` 具有與輸入標籤協助程式重疊的功能。 輸入標籤協助程式將會自動設定 `type` 屬性；而 `Html.TextBox` 和 `Html.TextBoxFor` 不會。 `Html.Editor` 和 `Html.EditorFor` 會處理集合、複雜的物件和範本；而輸入標籤協助程式不會。 輸入標籤協助程式、`Html.EditorFor` 和 `Html.TextBoxFor` 為強型別 (它們使用 Lambda 運算式)；而 `Html.TextBox` 和 `Html.Editor` 不是 (它們使用運算式名稱)。

### <a name="htmlattributes"></a>HtmlAttributes

`@Html.Editor()` 和 `@Html.EditorFor()` 執行它們的預設範本時，使用特殊的 `ViewDataDictionary` 項目，名為 `htmlAttributes`。 此行為會選擇性地使用 `additionalViewData` 參數來增強。 索引鍵 "htmlAttributes" 不區分大小寫。 索引鍵 "htmlAttributes" 的處理方式類似於傳遞給輸入協助程式 (例如 `@Html.TextBox()`)的 `htmlAttributes` 物件。

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a>運算式名稱

`asp-for` 屬性值是 `ModelExpression` 和 Lambda 運算式的右邊。 因此，`asp-for="Property1"` 在產生的程式碼中變成 `m => m.Property1`，這也是為什麼您不需要加上前置詞 `Model` 的原因。 您可以使用 "\@" 字元來起始內嵌運算式並將它移到 `m.` 前面：

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe">
```

產生下列內容：

```HTML
<input type="text" id="joe" name="joe" value="Joe">
```

搭配集合屬性，當 `i` 的值為 `23` 時，`asp-for="CollectionProperty[23].Member"` 會產生與 `asp-for="CollectionProperty[i].Member"` 相同的名稱。

當 ASP.NET Core MVC 計算 `ModelExpression` 的值時，會檢查幾個來源，其中包括`ModelState`。 請考慮 `<input type="text" asp-for="@Name">`。 導出的 `value` 屬性是第一個非 null 的值，來自：

* 索引鍵為 "Name" 的 `ModelState` 項目。
* 運算式 `Model.Name` 的結果。

### <a name="navigating-child-properties"></a>巡覽子屬性

您也可以使用檢視模型的屬性路徑巡覽至子屬性。 請考慮更複雜的模型類別，其中包含子系 `Address` 屬性。

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

在檢視中，我們繫結至 `Address.AddressLine1`：

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

會為 `Address.AddressLine1` 產生下列 HTML：

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="">
```

### <a name="expression-names-and-collections"></a>運算式名稱和集合

範例，包含 `Colors` 陣列的模型：

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

動作方法：

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

下列 Razor 示範如何存取特定 `Color` 項目：

[!code-HTML[](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

*Views/Shared/EditorTemplates/String.cshtml* 範本：

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

使用 `List<T>` 的範例：

[!code-csharp[](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

下列 Razor 示範如何逐一查看集合：

[!code-HTML[](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

*Views/Shared/EditorTemplates/ToDoItem.cshtml* 範本：

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]

當值將在 `asp-for` 或 `Html.DisplayFor` 相當內容中使用時，應該使用 `foreach` (如果可能的話)。 一般而言，`for` 比 `foreach` 好 (若案例允許的話)，因為它不需要配置列舉程式；不過，評估 LINQ 運算式中的索引子可能成本高昂且應該儘可能避免。

&nbsp;

>[!NOTE]
>上述加上註解的範例程式碼示範如何將 Lambda 運算式取代為 `@` 運算子，來存取清單中的每個 `ToDoItem`。

## <a name="the-textarea-tag-helper"></a>Textarea 標籤協助程式

`Textarea Tag Helper` 標籤協助程式類似於輸入標籤協助程式。

* 會從 [\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea) 項目的模型產生 `id` 和 `name` 屬性，以及資料驗證屬性。

* 提供強型別。

* HTML 協助程式替代：`Html.TextAreaFor`

範例：

[!code-csharp[](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

會產生下列 HTML：

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
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
</form>
```

## <a name="the-label-tag-helper"></a>標籤標籤協助程式

* 會針對運算式名稱產生 [\<label>](https://www.w3.org/wiki/HTML/Elements/label)元素的相關標籤標題和 `for` 屬性

* HTML 協助程式替代：`Html.LabelFor`。

`Label Tag Helper` 相較於純粹的 HTML標籤項目，提供下列優點：

* 您會自動從 `Display` 屬性取得描述性的標籤值。 預定的顯示名稱可能會隨著時間而改變，`Display` 屬性和標籤標籤協助程式的組合會在使用它的每個地方都套用 `Display`。

* 原始程式碼中較少標記

* 強型別與模型屬性。

範例：

[!code-csharp[](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

會為 `<label>` 項目產生下列 HTML：

```HTML
<label for="Email">Email Address</label>
```

標籤標籤協助程式產生了 "Email" 的 `for` 屬性值，這是與 `<input>` 項目建立關聯的識別碼。 標籤協助程式會產生一致的 `id` 和 `for` 項目，所以能夠正確地相關聯。 在此範例中的標題來自 `Display` 屬性。 如果模型未包含 `Display` 屬性，標題會是運算式的屬性名稱。

## <a name="the-validation-tag-helpers"></a>驗證標籤協助程式

有兩個驗證標籤協助程式。 `Validation Message Tag Helper` (這會顯示模型上單一屬性的驗證訊息)，和 `Validation Summary Tag Helper` (這會顯示驗證錯誤的摘要)。 `Input Tag Helper` 會根據您模型類別上的資料註釋屬性，將 HTML5 用戶端端驗證屬性新增至輸入項目。 也會在伺服器上執行驗證。 驗證標籤協助程式會在發生驗證錯誤時顯示這些錯誤訊息。

### <a name="the-validation-message-tag-helper"></a>驗證訊息標籤協助程式

* 新增 [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)`data-valmsg-for="property"` 屬性至 [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) 項目，它會將驗證錯誤訊息附加在指定模型屬性的輸入欄位。 發生用戶端驗證錯誤時，[jQuery](https://jquery.com/) 會在 `<span>` 項目顯示錯誤訊息。

* 也會在伺服器上發生驗證。 用戶端可能停用 JavaScript，而一些驗證只能在伺服器端進行。

* HTML 協助程式替代：`Html.ValidationMessageFor`

`Validation Message Tag Helper` 與 HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) 項目上的 `asp-validation-for` 屬性搭配使用。

```HTML
<span asp-validation-for="Email"></span>
```

驗證訊息標籤協助程式將會產生下列 HTML：

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

您通常會在 `Input` 標籤協助程式之後針對相同的屬性使用 `Validation Message Tag Helper`。 這麼做會在造成錯誤的輸入附近顯示任何驗證錯誤訊息。

> [!NOTE]
> 您必須具有正確 JavaScript 和 [jQuery](https://jquery.com/) 指令碼參考的檢視。 如需詳細資訊，請參閱[模型驗證](../models/validation.md)。

發生伺服器端驗證錯誤時 (例如當您有自訂伺服器端驗證或是已停用用戶端驗證時)，MVC 會將該錯誤訊息放置為 `<span>` 項目的主體。

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a>驗證摘要標籤協助程式

* 使用 `asp-validation-summary` 屬性設定 `<div>` 項目的目標

* HTML 協助程式替代：`@Html.ValidationSummary`

`Validation Summary Tag Helper` 用來顯示驗證訊息的摘要。 `asp-validation-summary` 屬性值可以是下列任一項：

|asp-validation-summary|顯示的驗證訊息|
|--- |--- |
|ValidationSummary.All|屬性和模型層級|
|ValidationSummary.ModelOnly|型號|
|ValidationSummary.None|無|

### <a name="sample"></a>範例

在下列範例中，資料模型裝飾了 `DataAnnotation` 屬性，它會在 `<input>` 項目產生驗證錯誤訊息。  在發生驗證錯誤時，驗證標籤協助程式會顯示錯誤訊息：

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

產生的 HTML (當模型有效時)：

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid email address."
   data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
</form>
```

## <a name="the-select-tag-helper"></a>選取標籤協助程式

* 為模型的屬性產生 [select](https://www.w3.org/wiki/HTML/Elements/select) 以及相關聯的 [option](https://www.w3.org/wiki/HTML/Elements/option) 項目。

* 有 HTML 協助程式的替代 `Html.DropDownListFor` 和 `Html.ListBoxFor`

`Select Tag Helper` `asp-for` 指定 [select](https://www.w3.org/wiki/HTML/Elements/select) 項目的模型屬性名稱，而 `asp-items` 指定 [option](https://www.w3.org/wiki/HTML/Elements/option) 項目。  例如：

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

範例：

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

`Index` 方法會初始化 `CountryViewModel`、設定選取的國家/地區，並將其傳遞給 `Index` 檢視。

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=8-13)]

HTTP POST `Index` 方法會顯示選取項目：

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

`Index` 檢視：

[!code-cshtml[](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

它會產生下列 HTML (並選取 "CA")：

```html
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
   </form>
```

> [!NOTE]
> 我們不建議使用 `ViewBag` 或 `ViewData` 搭配選取標籤協助程式。 檢視模型在提供 MVC 中繼資料方面比較強大，且通常較不容易發生問題。

`asp-for` 屬性值是特殊案例，不需要 `Model` 前置詞，其他標籤協助程式屬性則需要 (例如 `asp-items`)

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a>列舉繫結

使用 `<select>` 搭配 `enum` 屬性，並從 `enum` 值產生 `SelectListItem` 項目通常很方便。

範例：

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

`GetEnumSelectList` 方法會產生列舉的 `SelectList` 物件。

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

您可以用 `Display` 屬性裝飾列舉值清單，以取得更豐富的 UI：

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

會產生下列 HTML：

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
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
    </form>
```

### <a name="option-group"></a>選項群組

當檢視模型包含一或多個 `SelectListGroup` 物件時，會產生 HTML [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) 項目。

`CountryViewModelGroup` 將 `SelectListItem` 項目分組成「北美洲」和「歐洲」群組：

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

兩個群組如下所示：

![選項群組範例](working-with-forms/_static/grp.png)

產生的 HTML：

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
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
 </form>
```

### <a name="multiple-select"></a>多重選項

選取標籤協助程式會自動產生 [multiple = "multiple"](http://w3c.github.io/html-reference/select.html) 屬性，如果 `asp-for` 屬性中指定的屬性是 `IEnumerable`。 例如，假設有以下的模型：

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

具有下列檢視：

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

產生下列 HTML：

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
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
</form>
```

### <a name="no-selection"></a>沒有選取項目

如果您發現自己在多個頁面中使用「未指定」的選項，您可以建立範本，以避免重複 HTML：

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

*Views/Shared/EditorTemplates/CountryViewModel.cshtml* 範本：

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

新增 HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) 項目並不限於「沒有選取項目」的案例。 例如，下列檢視和動作方法會產生類似於上述程式碼的 HTML：

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

將會選取正確的 `<option>` 項目 (包含 `selected="selected"` 屬性)，視目前的 `Country` 值而定。

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
 </form>
 ```

## <a name="additional-resources"></a>其他資源

* <xref:mvc/views/tag-helpers/intro>
* [HTML 表單項目](https://www.w3.org/TR/html401/interact/forms.html)
* [要求驗證權杖](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)
* <xref:mvc/models/model-binding>
* <xref:mvc/models/validation>
* [IAttributeAdapter 介面](/dotnet/api/Microsoft.AspNetCore.Mvc.DataAnnotations.IAttributeAdapter)
* [此文件的程式碼片段](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/working-with-forms/sample/final)
