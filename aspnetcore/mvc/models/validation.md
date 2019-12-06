---
title: ASP.NET Core MVC 中的模型驗證
author: rick-anderson
description: 了解 ASP.NET Core MVC 和 Razor Pages 中的模型驗證。
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: mvc/models/validation
ms.openlocfilehash: 7a6017141eb1016128c4a135c187479717580bb5
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881033"
---
# <a name="model-validation-in-aspnet-core-mvc-and-razor-pages"></a>ASP.NET Core MVC 和 Razor Pages 中的模型驗證

::: moniker range=">= aspnetcore-3.0"

依[Kirk Larkin](https://github.com/serpent5)

本文說明如何在 ASP.NET Core MVC 或 Razor Pages 應用程式中驗證使用者輸入。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/samples) ([如何下載](xref:index#how-to-download-a-sample))。

## <a name="model-state"></a>模型狀態

模型狀態代表來自兩個子系統的錯誤：模型繫結和模型驗證。 源自[模型](model-binding.md)系結的錯誤通常是資料轉換錯誤。 例如，在整數位段中輸入 "x"。 模型驗證會在模型系結之後發生，並報告資料不符合商務規則的錯誤。 例如，在欄位中輸入0時，預期會有1到5之間的評等。

模型系結和模型驗證會在執行控制器動作或 Razor Pages 處理常式方法之前進行。 針對 Web 應用程式，此應用程式的責任為檢查 `ModelState.IsValid` 並做出適當回應。 Web 應用程式通常會重新顯示頁面，並出現錯誤訊息：

[!code-csharp[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=3-6)]

如果 Web API 控制器具有 `[ApiController]` 屬性，則該控制器就不需要檢查 `ModelState.IsValid`。 在此情況下，當模型狀態無效時，會傳回包含錯誤詳細資料的自動 HTTP 400 回應。 如需詳細資訊，請參閱[自動 HTTP 400 回應](xref:web-api/index#automatic-http-400-responses)。

## <a name="rerun-validation"></a>重新執行驗證

驗證會自動進行，但建議您以手動方式重複進行。 例如，您可能會計算屬性的值，並在將屬性設定為計算的值之後重新執行驗證。 若要重新執行驗證，請呼叫 `TryValidateModel` 方法，如下所示：

[!code-csharp[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml.cs?name=snippet_TryValidate&highlight=3-6)]

## <a name="validation-attributes"></a>驗證屬性

驗證屬性可讓您指定模型屬性的驗證規則。 範例應用程式的下列範例顯示以驗證屬性標注的模型類別。 `[ClassicMovie]` 屬性是自訂驗證屬性，其他的則是內建。 未顯示 `[ClassicMovieWithClientValidator]`。 `[ClassicMovieWithClientValidator]` 會顯示執行自訂屬性的替代方式。

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/Movie.cs?name=snippet_Class)]

## <a name="built-in-attributes"></a>內建屬性

下列是部分內建驗證屬性：

* `[CreditCard]`：驗證屬性是否具有信用卡格式。
* `[Compare]`：驗證模型中的兩個屬性是否相符。
* `[EmailAddress]`：驗證屬性是否具有電子郵件格式。
* `[Phone]`：驗證屬性是否具有電話號碼格式。
* `[Range]`：驗證屬性值是否落在指定的範圍內。
* `[RegularExpression]`：驗證屬性值符合指定的正則運算式。
* `[Required]`：驗證欄位不是 null。 如需此屬性行為的詳細資料，請參閱[`[Required]` 屬性](#required-attribute)。
* `[StringLength]`：驗證字串屬性值未超過指定的長度限制。
* `[Url]`：驗證屬性是否具有 URL 格式。
* `[Remote]`：藉由在伺服器上呼叫動作方法，驗證用戶端上的輸入。 如需此屬性行為的詳細資料，請參閱 `[`[遠端] ' 屬性] （#remote 屬性）。

您可以在 [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) 命名空間中找到驗證屬性的完整清單。

### <a name="error-messages"></a>錯誤訊息

驗證屬性可讓您指定要顯示的無效輸入錯誤訊息。 例如：

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

就內部而言，屬性會使用欄位名稱的預留位置呼叫 `String.Format`，有時候也會使用其他預留位置。 例如：

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

套用至 `Name` 屬性時，透過上述程式碼建立的錯誤訊息會是 「名稱長度必須介於 6 和 8 之間」。

若要找出哪些參數傳遞給 `String.Format` 以取得特定屬性的錯誤訊息，請參閱 [DataAnnotations 原始程式碼](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations)。

## <a name="required-attribute"></a>[必要] 屬性

根據預設，驗證系統會將不可為 Null 的參數或屬性視為具有 `[Required]` 屬性。 例如 `decimal` 和 `int` 的[實值型別](/dotnet/csharp/language-reference/keywords/value-types)不可為 Null。

### <a name="required-validation-on-the-server"></a>[必要] 在伺服器上執行驗證

在伺服器上，如果必要的值屬性為 Null，則會視為遺失。 不可為 null 的欄位一律有效，而且永遠不會顯示 `[Required]` 屬性的錯誤訊息。

不過，不可為 Null 屬性的模型繫結可能會失敗，並產生一則錯誤訊息，例如 `The value '' is invalid`。 若要針對不可為 Null 型別的伺服器端驗證指定自訂錯誤訊息，您可以使用下列選項：

* 將欄位設成可為 Null (例如 `decimal?`，而不是 `decimal`)。 [可為 Null\<T>](/dotnet/csharp/programming-guide/nullable-types/) 實值型別會視為如同標準的可為 Null 型別。
* 指定模型繫結要使用的預設錯誤訊息，如下列範例所示：

  [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=5-6)]

  如需您可以為其設定預設訊息之模型繫結錯誤的詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>。

### <a name="required-validation-on-the-client"></a>[必要] 在用戶端上執行驗證

不可為 Null 的型別和字串，會在用戶端上以不同於在伺服器上的方式處理。 在用戶端上：

* 只有在為值的輸入進行輸入後，才會將該值視為存在。 因此，用戶端驗證處理非 Null 型別的方式，會與可為 Null 型別的方式相同。
* 字串欄位中的空白字元，會以 jQuery 驗證[必要](https://jqueryvalidation.org/required-method/)方法視為有效的輸入。 如果僅輸入空白字元，則伺服器端驗證會將必要的字串欄位視為無效。

如先前所述，不可為 Null 的型別會視為具有 `[Required]` 屬性。 這表示即使您不套用 `[Required]` 屬性，也可以取得用戶端驗證。 但是，如果您不使用該屬性，就會出現預設的錯誤訊息。 若要指定自訂錯誤訊息，請使用該屬性。

## <a name="remote-attribute"></a>[遠端] 屬性

`[Remote]` 屬性會實作用戶端驗證，該驗證需要呼叫伺服器上的方法來判斷欄位輸入是否有效。 例如，應用程式可能需要驗證使用者名稱是否已經在使用中。

實作遠端驗證：

1. 為 JavaScript 建立動作方法來呼叫。  JQuery 驗證[遠端](https://jqueryvalidation.org/remote-method/)方法會預期 JSON 回應：

   * `true` 表示輸入資料有效。
   * `false`、`undefined` 或 `null` 表示輸入無效。 顯示預設錯誤訊息。
   * 任何其他字串都表示輸入無效。 將字串顯示為自訂錯誤訊息。

   自訂錯誤訊息的動作方法範例如下：

   [!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. 在模型類別中，使用指向驗證動作方法的 `[Remote]` 屬性 (Attribute) 來標註屬性 (Property)，如下列範例所示：

   [!code-csharp[](validation/samples/3.x/ValidationSample/Models/User.cs?name=snippet_Email)]
 
   `[Remote]` 屬性位於 `Microsoft.AspNetCore.Mvc` 命名空間。
   
### <a name="additional-fields"></a>其他欄位

`[Remote]` 屬性 (Attribute) 的 `AdditionalFields` 屬性 (Property) 可讓您針對伺服器上的資料來驗證欄位組合。 例如，如果 `User` 模型具 `FirstName` 和 `LastName` 屬性，建議您確認沒有任何現有使用者已具有該組名稱。 下列範例顯示如何使用 `AdditionalFields`：

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/User.cs?name=snippet_Name&highlight=1,5)]

`AdditionalFields` 可以明確設定為字串 "FirstName" 和 "LastName"，但使用[nameof](/dotnet/csharp/language-reference/keywords/nameof)運算子可簡化稍後的重構。 此驗證的動作方法必須同時接受 `firstName` 和 `lastName` 引數：

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyName)]

當使用者輸入名字或姓氏時，JavaScript 會進行遠端呼叫以查看該組名稱是否已在使用中。

若要驗證兩個或多個其他欄位，請以逗號分隔清單來提供這些欄位。 例如，若要將 `MiddleName` 屬性 (Property) 新增至模型，請設定 `[Remote]` 屬性 (Attribute)，如下列範例所示：

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

如同所有屬性引數，`AdditionalFields` 必須是常數運算式。 因此，請勿使用[內插字串](/dotnet/csharp/language-reference/keywords/interpolated-strings)或呼叫 <xref:System.String.Join*> 來初始化 `AdditionalFields`。

## <a name="alternatives-to-built-in-attributes"></a>內建屬性的替代項目

如果您需要內建屬性未提供的驗證，您可以：

* [建立自訂屬性](#custom-attributes)。
* [實作 IValidatableObject](#ivalidatableobject)。

## <a name="custom-attributes"></a>自訂屬性

在內建驗證屬性無法處理的情況下，您可以建立自訂驗證屬性。 建立繼承自 <xref:System.ComponentModel.DataAnnotations.ValidationAttribute> 的類別，並覆寫 <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> 方法。

`IsValid` 方法會接受名為 *value* 的物件，這是要驗證的輸入。 多載也會接受 `ValidationContext` 物件，該物件會提供其他資訊，例如模型繫結所建立的模型執行個體。

下列範例會驗證 *Classic* 內容類型中電影的發行日期，不晚於指定的年份。 `[ClassicMovie]` 屬性：

* 只在伺服器上執行。
* 若為傳統電影，會驗證發行日期：

[!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieAttribute.cs?name=snippet_Class)]

上述範例中的 `movie` 變數代表 `Movie` 物件，該物件包含表單送出中的資料。 驗證失敗時，會傳回 `ValidationResult` 和錯誤訊息。

## <a name="ivalidatableobject"></a>IValidatableObject

上述範例僅適用於 `Movie` 型別。 類別層級驗證的另一個選項，是在模型類別中實作 `IValidatableObject`，如下列範例所示：

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/ValidatableMovie.cs?name=snippet_Class&highlight=1,26-34)]

## <a name="top-level-node-validation"></a>最上層節點驗證

最上層節點包括：

* 動作參數
* 控制器屬性
* 頁面處理常式參數
* 頁面模型屬性

除了驗證模型屬性，也會驗證模型所繫結的最上層節點。 在來自範例應用程式的下列範例中，`VerifyPhone` 方法會使用 <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> 來驗證 `phone` 動作參數：

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

最上層節點可以搭配使用 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> 和驗證屬性。 在來自範例應用程式的下列範例中，`CheckAge` 方法會指定提交表單時必須從查詢字串繫結 `age` 參數：

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_CheckAgeSignature)]

在 [檢查年齡] 頁面 (*CheckAge.cshtml*) 中，有兩個表單。 第一個表單會提交 `99` 的 `Age` 值做為查詢字串參數： `https://localhost:5001/Users/CheckAge?Age=99`。

從查詢字串提交正確格式化的 `age` 時，即會驗證表單。

[檢查年齡] 頁面上的第二個表單會在要求本文中提交 `Age` 值，而且驗證會失敗。 由於 `age` 參數必須來自查詢字串，因此繫結會失敗。

## <a name="maximum-errors"></a>最大錯誤數

達到最大錯誤數 (預設為 200) 時，就會停止驗證。 您可以在 `Startup.ConfigureServices` 中使用下列程式碼來設定此數目：

[!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=4)]

## <a name="maximum-recursion"></a>最大遞迴

<xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> 會周遊所正驗證之模型的物件圖形。 對於深度或無限遞迴的模型，驗證可能會導致堆疊溢位。 [MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) 可用來在訪客遞迴超過設定的深度時，提早停止驗證。 `MvcOptions.MaxValidationDepth` 的預設值為32。

## <a name="automatic-short-circuit"></a>自動最少運算

如果模型圖形不需要驗證，則驗證會自動進行最少運算 (略過)。 執行階段會略過驗證的物件，包含基本集合 (例如 `byte[]`、`string[]`、`Dictionary<string, string>`) 和沒有任何驗證程式的複雜物件圖形。

## <a name="disable-validation"></a>停用驗證

停用驗證：

1. 建立不會將任何欄位標記為無效的 `IObjectModelValidator` 實作。

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/NullObjectModelValidator.cs?name=snippet_Class)]

1. 將下列程式碼新增至 `Startup.ConfigureServices`，以取代相依性插入容器中的預設 `IObjectModelValidator` 實作。

   [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_DisableValidation)]

您仍可能會看到來自模型繫結的模型狀態錯誤。

## <a name="client-side-validation"></a>用戶端驗證

用戶端驗證可避免送出無效的表單。 [送出] 按鈕會執行 JavaScript 以送出表單或顯示錯誤訊息。

當表單上存在輸入錯誤時，用戶端驗證可避免伺服器上不必要的來回往返。 *_Layout.cshtml* 和 *_ValidationScriptsPartial.cshtml* 中的下列指令碼參考支援用戶端驗證：

[!code-cshtml[](validation/samples/3.x/ValidationSample/Views/Shared/_Layout.cshtml?name=snippet_Scripts)]

[!code-cshtml[](validation/samples/3.x/ValidationSample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_Scripts)]

[jQuery 低調驗證](https://github.com/aspnet/jquery-validation-unobtrusive) (jQuery Unobtrusive Validation) 指令碼是建置在熱門 [jQuery 驗證](https://jqueryvalidation.org/) 外掛程式上的自訂 Microsoft 前端程式庫。 若沒有 jQuery 低調驗證，您就必須在兩個地方撰寫相同的驗證邏輯程式碼：一次在模型屬性 (Property) 上的伺服器端驗證屬性 (Attribute)，另一次在用戶端指令碼中。 反之，[標記協助程式](xref:mvc/views/tag-helpers/intro)和 [HTML 協助程式](xref:mvc/views/overview)使用模型屬性 (Property) 中的驗證屬性 (Attribute) 和類型中繼資料，來轉譯需要驗證之表單項目的 HTML 5 `data-` 屬性 (Attribute)。 jQuery 低調驗證會剖析 `data-` 屬性並將邏輯傳遞至 jQuery 驗證，以有效地將伺服器端驗證邏輯「複製」到用戶端。 您可以使用標記協助程式來顯示用戶端的驗證錯誤，如下所示：

[!code-cshtml[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=3-4)]

上述標記協助程式會轉譯下列 HTML：

```html
<div class="form-group">
    <label class="control-label" for="Movie_ReleaseDate">Release Date</label>
    <input class="form-control" type="date" data-val="true"
        data-val-required="The Release Date field is required."
        id="Movie_ReleaseDate" name="Movie.ReleaseDate" value="">
    <span class="text-danger field-validation-valid"
        data-valmsg-for="Movie.ReleaseDate" data-valmsg-replace="true"></span>
</div>
```

請注意，HTML 輸出中的 `data-` 屬性 (attribute) 會對應至 `Movie.ReleaseDate` 屬性 (property) 的驗證屬性 (attribute)。 `data-val-required` 屬性包含使用者未填入發行日期欄位時所要顯示的錯誤訊息。 jQuery 不顯眼的驗證會將此值傳遞至 jQuery Validate [required （）](https://jqueryvalidation.org/required-method/)方法，然後在伴隨的 **\<範圍 >** 元素中顯示該訊息。

資料型別驗證是根據屬性的 .NET 型別，除非是由 `[DataType]` 屬性覆寫。 瀏覽器具有自己的預設錯誤訊息，但 jQuery 驗證低調驗證套件可以覆寫這些訊息。 `[DataType]` 屬性和子類別 (例如 `[EmailAddress]`) 可讓您指定錯誤訊息。

## <a name="unobtrusive-validation"></a>不顯眼的驗證

如需不顯眼驗證的相關資訊，請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/1111)。

### <a name="add-validation-to-dynamic-forms"></a>將驗證新增至動態表單

jQuery 低調驗證會在第一次載入頁面時將驗證邏輯和傳遞參數至 jQuery 驗證。 因此，驗證不會在動態產生的表單上自動運作。 若要啟用驗證，請指示 jQuery 低調驗證在建立動態表單之後立即進行剖析。 例如，下列程式碼會在透過 AJAX 新增的表單上設定用戶端驗證。

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

`$.validator.unobtrusive.parse()` 方法接受 jQuery 選取器的一個引數。 此方法會指示 jQuery 低調驗證在該選取器內剖析表單的 `data-` 屬性。 然後，這些屬性的值會傳遞至 jQuery 驗證外掛程式。

### <a name="add-validation-to-dynamic-controls"></a>將驗證新增至動態控制項

`$.validator.unobtrusive.parse()` 方法適用於整個表單，而非動態產生的個別控制項 (例如 `<input>` 和 `<select/>`)。 若要重新剖析表單，請移除先前剖析表單時新增的驗證資料，如下列範例所示：

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add control. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        $(form).removeData("validator")    // Added by jQuery Validate
               .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="custom-client-side-validation"></a>自訂用戶端驗證

自訂用戶端驗證是透過產生使用自訂 jQuery 驗證配接器的 `data-` HTML 屬性來進行。 下列配接器程式碼範例，是針對本文稍早介紹的 `[ClassicMovie]` 和 `[ClassicMovieWithClientValidator]` 屬性所撰寫：

[!code-javascript[](validation/samples/3.x/ValidationSample/wwwroot/js/classicMovieValidator.js)]

如需如何撰寫配接器的資訊，請參閱 [jQuery 驗證文件](https://jqueryvalidation.org/documentation/)。

用於指定欄位之配接器的使用，是由 `data-` 屬性觸發，因此會：

* 將欄位加上旗標為待驗證 (`data-val="true"`)。
* 識別驗證規則名稱和錯誤訊息文字 (例如 `data-val-rulename="Error message."`)。
* 提供驗證程式需要的任何其他參數 (例如 `data-val-rulename-param1="value"`)。

下列範例顯示範例應用程式之 `ClassicMovie` 屬性的 `data-` 屬性：

```html
<input class="form-control" type="date"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year no later than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The Release Date field is required."
    id="Movie_ReleaseDate" name="Movie.ReleaseDate" value="">
```

如前文所述，[標記協助程式](xref:mvc/views/tag-helpers/intro)和 [HTML 協助程式](xref:mvc/views/overview) 使用來自驗證屬性的資訊來轉譯 `data-` 屬性。 有二個選項可撰寫用於建立自訂 `data-` HTML 屬性的程式碼：

* 建立衍生自 `AttributeAdapterBase<TAttribute>` 的類別，以及可實作 `IValidationAttributeAdapterProvider` 的類別，並在 DI 中註冊您的屬性及其配接器。 此方法遵循[單一職責原則](https://wikipedia.org/wiki/Single_responsibility_principle)，其中伺服器相關與用戶端相關的驗證程式碼位於不同類別中。 配接器也具有優點，由於其在 DI 中註冊，因此可以使用 DI 中的其他服務 (如需要)。
* 在您的 `ValidationAttribute` 類別中實作 `IClientModelValidator`。 如果屬性不執行任何伺服器端驗證，且不需要任何來自 DI 的服務，則此方法可能適合。

### <a name="attributeadapter-for-client-side-validation"></a>用戶端驗證的屬性配接器

這種在 HTML 中轉譯 `data-` 屬性的方法，由範例應用程式中的 `ClassicMovie` 屬性使用。 使用此方法來新增用戶端驗證：

1. 建立自訂驗證屬性的屬性配接器類別。 自 [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2) 衍生類別。 建立會將 `data-` 屬性新增至轉譯輸出的 `AddValidation` 方法，如下列範例所示：

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieAttributeAdapter.cs?name=snippet_Class)]

1. 建立實作 <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider> 的配接器提供者類別。 在 `GetAttributeAdapter` 方法中將自訂屬性傳遞至配接器的建構函式，如下列範例所示：

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/CustomValidationAttributeAdapterProvider.cs?name=snippet_Class)]

1. 在 `Startup.ConfigureServices` 中註冊 DI 的配接器提供者：

   [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=9-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a>用戶端驗證的 IClientModelValidator

這種在 HTML 中轉譯 `data-` 屬性的方法，由範例應用程式中的 `ClassicMovieWithClientValidator` 屬性使用。 使用此方法來新增用戶端驗證：

* 在自訂驗證屬性中，實作 `IClientModelValidator` 介面並建立 `AddValidation` 方法。 在 `AddValidation` 方法中，為驗證新增 `data-` 屬性，如下列範例所示：

  [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieWithClientValidatorAttribute.cs?name=snippet_Class)]

## <a name="disable-client-side-validation"></a>停用用戶端驗證

下列程式碼會停用 Razor Pages 中的用戶端驗證：

[!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_DisableClientValidation&highlight=2-5)]

停用用戶端驗證的其他選項：

* 將所有 *. cshtml*檔案中 `_ValidationScriptsPartial` 的參考批註在一起。
* 移除*Pages\Shared\_ValidationScriptsPartial*的內容。

上述方法不會防止用戶端驗證 ASP.NET Core 身分識別 Razor 類別庫。 如需詳細資訊，請參閱<xref:security/authentication/scaffold-identity>。

## <a name="additional-resources"></a>其他資源

* [System.ComponentModel.DataAnnotations 命名空間](xref:System.ComponentModel.DataAnnotations)
* [模型繫結](model-binding.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

本文說明如何在 ASP.NET Core MVC 或 Razor Pages 應用程式中驗證使用者輸入。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([如何下載](xref:index#how-to-download-a-sample))。

## <a name="model-state"></a>模型狀態

模型狀態代表來自兩個子系統的錯誤：模型繫結和模型驗證。 源自[模型繫結](model-binding.md)的錯誤通常是資料轉換錯誤 (例如在必須輸入整數的欄位中輸入 "x")。 模型驗證發生於模型繫結之後，並會在資料不符合商務規則時回報錯誤 (例如在必須輸入 1 到 5 之間評等的欄位中輸入 0)。

模型繫結和驗證會在執行控制器動作或 Razor Pages 處理常式方法之前進行。 針對 Web 應用程式，此應用程式的責任為檢查 `ModelState.IsValid` 並做出適當回應。 Web 應用程式通常會重新顯示頁面，並出現錯誤訊息：

[!code-csharp[](validation/samples_snapshot/2.x/Create.cshtml.cs?name=snippet&highlight=3-6)]

如果 Web API 控制器具有 `[ApiController]` 屬性，則該控制器就不需要檢查 `ModelState.IsValid`。 在此情況下，當模型狀態無效時，會傳回包含錯誤詳細資料的自動 HTTP 400 回應。 如需詳細資訊，請參閱[自動 HTTP 400 回應](xref:web-api/index#automatic-http-400-responses)。

## <a name="rerun-validation"></a>重新執行驗證

驗證會自動進行，但建議您以手動方式重複進行。 例如，您可能會計算屬性的值，並在將屬性設定為計算的值之後重新執行驗證。 若要重新執行驗證，請呼叫 `TryValidateModel` 方法，如下所示：

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a>驗證屬性

驗證屬性可讓您指定模型屬性的驗證規則。 下列來自[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample)的範例，顯示以驗證屬性標註的模型類別。 `[ClassicMovie]` 屬性是自訂驗證屬性，其他的則是內建。 [未顯示] 是 `[ClassicMovie2]`，這會顯示執行自訂屬性的替代方式。

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a>內建屬性

內建驗證屬性包括：

* `[CreditCard]`：驗證屬性是否具有信用卡格式。
* `[Compare]`：驗證模型中的兩個屬性是否相符。 例如， *Register.cshtml.cs*檔案會使用 `[Compare]` 來驗證兩個輸入的密碼相符。 [Scaffold 身分識別](xref:security/authentication/scaffold-identity) 以查看註冊程式碼。
* `[EmailAddress]`：驗證屬性是否具有電子郵件格式。
* `[Phone]`：驗證屬性是否具有電話號碼格式。
* `[Range]`：驗證屬性值是否落在指定的範圍內。
* `[RegularExpression]`：驗證屬性值符合指定的正則運算式。
* `[Required]`：驗證欄位不是 null。 如需此屬性行為的詳細資料，請參閱[`[Required]` 屬性](#required-attribute)。
* `[StringLength]`：驗證字串屬性值未超過指定的長度限制。
* `[Url]`：驗證屬性是否具有 URL 格式。
* `[Remote]`：藉由在伺服器上呼叫動作方法，驗證用戶端上的輸入。 如需此屬性行為的詳細資料，請參閱[`[Remote]` 屬性](#remote-attribute)。

您可以在 [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) 命名空間中找到驗證屬性的完整清單。

### <a name="error-messages"></a>錯誤訊息

驗證屬性可讓您指定要顯示的無效輸入錯誤訊息。 例如：

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

就內部而言，屬性會使用欄位名稱的預留位置呼叫 `String.Format`，有時候也會使用其他預留位置。 例如：

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

套用至 `Name` 屬性時，透過上述程式碼建立的錯誤訊息會是 「名稱長度必須介於 6 和 8 之間」。

若要找出哪些參數傳遞給 `String.Format` 以取得特定屬性的錯誤訊息，請參閱 [DataAnnotations 原始程式碼](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations)。

## <a name="required-attribute"></a>[必要] 屬性

根據預設，驗證系統會將不可為 Null 的參數或屬性視為具有 `[Required]` 屬性。 例如 `decimal` 和 `int` 的[實值型別](/dotnet/csharp/language-reference/keywords/value-types)不可為 Null。

### <a name="required-validation-on-the-server"></a>[必要] 在伺服器上執行驗證

在伺服器上，如果必要的值屬性為 Null，則會視為遺失。 不可為 Null 的欄位一律有效，且一律不會顯示 [必要] 屬性的錯誤訊息。

不過，不可為 Null 屬性的模型繫結可能會失敗，並產生一則錯誤訊息，例如 `The value '' is invalid`。 若要針對不可為 Null 型別的伺服器端驗證指定自訂錯誤訊息，您可以使用下列選項：

* 將欄位設成可為 Null (例如 `decimal?`，而不是 `decimal`)。 [可為 Null\<T>](/dotnet/csharp/programming-guide/nullable-types/) 實值型別會視為如同標準的可為 Null 型別。
* 指定模型繫結要使用的預設錯誤訊息，如下列範例所示：

  [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  如需您可以為其設定預設訊息之模型繫結錯誤的詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>。

### <a name="required-validation-on-the-client"></a>[必要] 在用戶端上執行驗證

不可為 Null 的型別和字串，會在用戶端上以不同於在伺服器上的方式處理。 在用戶端上：

* 只有在為值的輸入進行輸入後，才會將該值視為存在。 因此，用戶端驗證處理非 Null 型別的方式，會與可為 Null 型別的方式相同。
* 字串欄位中的空白字元，會以 jQuery 驗證[必要](https://jqueryvalidation.org/required-method/)方法視為有效的輸入。 如果僅輸入空白字元，則伺服器端驗證會將必要的字串欄位視為無效。

如先前所述，不可為 Null 的型別會視為具有 `[Required]` 屬性。 這表示即使您不套用 `[Required]` 屬性，也可以取得用戶端驗證。 但是，如果您不使用該屬性，就會出現預設的錯誤訊息。 若要指定自訂錯誤訊息，請使用該屬性。

## <a name="remote-attribute"></a>[遠端] 屬性

`[Remote]` 屬性會實作用戶端驗證，該驗證需要呼叫伺服器上的方法來判斷欄位輸入是否有效。 例如，應用程式可能需要驗證使用者名稱是否已經在使用中。

實作遠端驗證：

1. 為 JavaScript 建立動作方法來呼叫。  JQuery 驗證[遠端](https://jqueryvalidation.org/remote-method/)方法會預期 JSON 回應：

   * `"true"` 表示輸入資料有效。
   * `"false"`、`undefined` 或 `null` 表示輸入無效。  顯示預設錯誤訊息。
   * 任何其他字串都表示輸入無效。 將字串顯示為自訂錯誤訊息。

   自訂錯誤訊息的動作方法範例如下：

   [!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. 在模型類別中，使用指向驗證動作方法的 `[Remote]` 屬性 (Attribute) 來標註屬性 (Property)，如下列範例所示：

   [!code-csharp[](validation/samples/2.x/ValidationSample/Models/User.cs?name=snippet_UserEmailProperty)]
 
   `[Remote]` 屬性位於 `Microsoft.AspNetCore.Mvc` 命名空間。 如不使用 `Microsoft.AspNetCore.App` 或 `Microsoft.AspNetCore.All` 中繼套件，請安裝 [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) NuGet 套件。
   
### <a name="additional-fields"></a>其他欄位

`[Remote]` 屬性 (Attribute) 的 `AdditionalFields` 屬性 (Property) 可讓您針對伺服器上的資料來驗證欄位組合。 例如，如果 `User` 模型具 `FirstName` 和 `LastName` 屬性，建議您確認沒有任何現有使用者已具有該組名稱。 下列範例顯示如何使用 `AdditionalFields`：

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/User.cs?name=snippet_UserNameProperties)]

`AdditionalFields` 可以明確設定為 `"FirstName"` 和 `"LastName"`的字串，但使用[nameof](/dotnet/csharp/language-reference/keywords/nameof)運算子可簡化稍後的重構。 這項驗證的動作方法必須接受名字和姓氏引數：

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyName)]

當使用者輸入名字或姓氏時，JavaScript 會進行遠端呼叫以查看該組名稱是否已在使用中。

若要驗證兩個或多個其他欄位，請以逗號分隔清單來提供這些欄位。 例如，若要將 `MiddleName` 屬性 (Property) 新增至模型，請設定 `[Remote]` 屬性 (Attribute)，如下列範例所示：

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

如同所有屬性引數，`AdditionalFields` 必須是常數運算式。 因此，請勿使用[內插字串](/dotnet/csharp/language-reference/keywords/interpolated-strings)或呼叫 <xref:System.String.Join*> 來初始化 `AdditionalFields`。

## <a name="alternatives-to-built-in-attributes"></a>內建屬性的替代項目

如果您需要內建屬性未提供的驗證，您可以：

* [建立自訂屬性](#custom-attributes)。
* [實作 IValidatableObject](#ivalidatableobject)。

## <a name="custom-attributes"></a>自訂屬性

在內建驗證屬性無法處理的情況下，您可以建立自訂驗證屬性。 建立繼承自 <xref:System.ComponentModel.DataAnnotations.ValidationAttribute> 的類別，並覆寫 <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> 方法。

`IsValid` 方法會接受名為 *value* 的物件，這是要驗證的輸入。 多載也會接受 `ValidationContext` 物件，該物件會提供其他資訊，例如模型繫結所建立的模型執行個體。

下列範例會驗證 *Classic* 內容類型中電影的發行日期，不晚於指定的年份。 `[ClassicMovie2]` 屬性會先檢查內容類型，如果是 *Classic* 才會繼續。 針對識別為 Classic 的電影，會檢查發行日期，以確定其不晚於傳遞至屬性建構函式的限制。)

[!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

上述範例中的 `movie` 變數代表 `Movie` 物件，該物件包含表單送出中的資料。 `IsValid` 方法會檢查日期與內容類型。 驗證成功時，`IsValid` 會傳回 `ValidationResult.Success` 程式碼。 驗證失敗時，會傳回 `ValidationResult` 和錯誤訊息。

## <a name="ivalidatableobject"></a>IValidatableObject

上述範例僅適用於 `Movie` 型別。 類別層級驗證的另一個選項，是在模型類別中實作 `IValidatableObject`，如下列範例所示：

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

## <a name="top-level-node-validation"></a>最上層節點驗證

最上層節點包括：

* 動作參數
* 控制器屬性
* 頁面處理常式參數
* 頁面模型屬性

除了驗證模型屬性，也會驗證模型所繫結的最上層節點。 在來自範例應用程式的下列範例中，`VerifyPhone` 方法會使用 <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> 來驗證 `phone` 動作參數：

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

最上層節點可以搭配使用 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> 和驗證屬性。 在來自範例應用程式的下列範例中，`CheckAge` 方法會指定提交表單時必須從查詢字串繫結 `age` 參數：

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_CheckAge)]

在 [檢查年齡] 頁面 (*CheckAge.cshtml*) 中，有兩個表單。 第一個表單會提交 `Age` 值 `99` 作為查詢字串：`https://localhost:5001/Users/CheckAge?Age=99`。

從查詢字串提交正確格式化的 `age` 時，即會驗證表單。

[檢查年齡] 頁面上的第二個表單會在要求本文中提交 `Age` 值，而且驗證會失敗。 由於 `age` 參數必須來自查詢字串，因此繫結會失敗。

執行 `CompatibilityVersion.Version_2_1` 或更新版本時，根據預設會啟用最上層節點驗證。 否則，會停用最上層節點驗證。 預設選項可以藉由設定 (`Startup.ConfigureServices`) 中的 <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> 屬性來覆寫，如下所示：

[!code-csharp[](validation/samples_snapshot/2.x/Startup.cs?name=snippet_AddMvc&highlight=4)]

## <a name="maximum-errors"></a>最大錯誤數

達到最大錯誤數 (預設為 200) 時，就會停止驗證。 您可以在 `Startup.ConfigureServices` 中使用下列程式碼來設定此數目：

[!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

## <a name="maximum-recursion"></a>最大遞迴

<xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> 會周遊所正驗證之模型的物件圖形。 針對非常深或無限遞迴的模型，驗證可能會導致堆疊溢位。 [MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) 可用來在訪客遞迴超過設定的深度時，提早停止驗證。 使用 `CompatibilityVersion.Version_2_2` 或更新版本執行時，`MvcOptions.MaxValidationDepth` 的預設值是32。 針對舊版，值為 Null，表示沒有深度條件約束。

## <a name="automatic-short-circuit"></a>自動最少運算

如果模型圖形不需要驗證，則驗證會自動進行最少運算 (略過)。 執行階段會略過驗證的物件，包含基本集合 (例如 `byte[]`、`string[]`、`Dictionary<string, string>`) 和沒有任何驗證程式的複雜物件圖形。

## <a name="disable-validation"></a>停用驗證

停用驗證：

1. 建立不會將任何欄位標記為無效的 `IObjectModelValidator` 實作。

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. 將下列程式碼新增至 `Startup.ConfigureServices`，以取代相依性插入容器中的預設 `IObjectModelValidator` 實作。

   [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_DisableValidation)]

您仍可能會看到來自模型繫結的模型狀態錯誤。

## <a name="client-side-validation"></a>用戶端驗證

用戶端驗證可避免送出無效的表單。 [送出] 按鈕會執行 JavaScript 以送出表單或顯示錯誤訊息。

當表單上存在輸入錯誤時，用戶端驗證可避免伺服器上不必要的來回往返。 *_Layout.cshtml* 和 *_ValidationScriptsPartial.cshtml* 中的下列指令碼參考支援用戶端驗證：

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

[jQuery 低調驗證](https://github.com/aspnet/jquery-validation-unobtrusive) (jQuery Unobtrusive Validation) 指令碼是建置在熱門 [jQuery 驗證](https://jqueryvalidation.org/) 外掛程式上的自訂 Microsoft 前端程式庫。 若沒有 jQuery 低調驗證，您就必須在兩個地方撰寫相同的驗證邏輯程式碼：一次在模型屬性 (Property) 上的伺服器端驗證屬性 (Attribute)，另一次在用戶端指令碼中。 反之，[標記協助程式](xref:mvc/views/tag-helpers/intro)和 [HTML 協助程式](xref:mvc/views/overview)使用模型屬性 (Property) 中的驗證屬性 (Attribute) 和類型中繼資料，來轉譯需要驗證之表單項目的 HTML 5 `data-` 屬性 (Attribute)。 jQuery 低調驗證會剖析 `data-` 屬性並將邏輯傳遞至 jQuery 驗證，以有效地將伺服器端驗證邏輯「複製」到用戶端。 您可以使用標記協助程式來顯示用戶端的驗證錯誤，如下所示：

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

上述標記協助程式會轉譯下列 HTML。

```html
<form action="/Movies/Create" method="post">
    <div class="form-horizontal">
        <h4>Movie</h4>
        <div class="text-danger"></div>
        <div class="form-group">
            <label class="col-md-2 control-label" for="ReleaseDate">ReleaseDate</label>
            <div class="col-md-10">
                <input class="form-control" type="datetime"
                data-val="true" data-val-required="The ReleaseDate field is required."
                id="ReleaseDate" name="ReleaseDate" value="">
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

請注意，HTML 輸出中的 `data-` 屬性 (attribute) 會對應至 `ReleaseDate` 屬性 (property) 的驗證屬性 (attribute)。 `data-val-required` 屬性包含使用者未填入發行日期欄位時所要顯示的錯誤訊息。 jQuery 不顯眼的驗證會將此值傳遞至 jQuery Validate [required （）](https://jqueryvalidation.org/required-method/)方法，然後在伴隨的 **\<範圍 >** 元素中顯示該訊息。

資料型別驗證是根據屬性的 .NET 型別，除非是由 `[DataType]` 屬性覆寫。 瀏覽器具有自己的預設錯誤訊息，但 jQuery 驗證低調驗證套件可以覆寫這些訊息。 `[DataType]` 屬性和子類別 (例如 `[EmailAddress]`) 可讓您指定錯誤訊息。

### <a name="add-validation-to-dynamic-forms"></a>將驗證新增至動態表單

jQuery 低調驗證會在第一次載入頁面時將驗證邏輯和傳遞參數至 jQuery 驗證。 因此，驗證不會在動態產生的表單上自動運作。 若要啟用驗證，請指示 jQuery 低調驗證在建立動態表單之後立即進行剖析。 例如，下列程式碼會在透過 AJAX 新增的表單上設定用戶端驗證。

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

`$.validator.unobtrusive.parse()` 方法接受 jQuery 選取器的一個引數。 此方法會指示 jQuery 低調驗證在該選取器內剖析表單的 `data-` 屬性。 然後，這些屬性的值會傳遞至 jQuery 驗證外掛程式。

### <a name="add-validation-to-dynamic-controls"></a>將驗證新增至動態控制項

`$.validator.unobtrusive.parse()` 方法適用於整個表單，而非動態產生的個別控制項 (例如 `<input>` 和 `<select/>`)。 若要重新剖析表單，請移除先前剖析表單時新增的驗證資料，如下列範例所示：

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add control. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        $(form).removeData("validator")    // Added by jQuery Validate
               .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="custom-client-side-validation"></a>自訂用戶端驗證

自訂用戶端驗證是透過產生使用自訂 jQuery 驗證配接器的 `data-` HTML 屬性來進行。 下列配接器程式碼範例，是針對本文稍早介紹的 `ClassicMovie` 和 `ClassicMovie2` 屬性所撰寫：

[!code-javascript[](validation/samples/2.x/ValidationSample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

如需如何撰寫配接器的資訊，請參閱 [jQuery 驗證文件](https://jqueryvalidation.org/documentation/)。

用於指定欄位之配接器的使用，是由 `data-` 屬性觸發，因此會：

* 將欄位加上旗標為待驗證 (`data-val="true"`)。
* 識別驗證規則名稱和錯誤訊息文字 (例如 `data-val-rulename="Error message."`)。
* 提供驗證程式需要的任何其他參數 (例如 `data-val-rulename-parm1="value"`)。

下列範例顯示範例應用程式之 `ClassicMovie` 屬性的 `data-` 屬性：

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="">
```

如前文所述，[標記協助程式](xref:mvc/views/tag-helpers/intro)和 [HTML 協助程式](xref:mvc/views/overview) 使用來自驗證屬性的資訊來轉譯 `data-` 屬性。 有二個選項可撰寫用於建立自訂 `data-` HTML 屬性的程式碼：

* 建立衍生自 `AttributeAdapterBase<TAttribute>` 的類別，以及可實作 `IValidationAttributeAdapterProvider` 的類別，並在 DI 中註冊您的屬性及其配接器。 此方法遵循[單一職責原則](https://wikipedia.org/wiki/Single_responsibility_principle)，其中伺服器相關與用戶端相關的驗證程式碼位於不同類別中。 配接器也具有優點，由於其在 DI 中註冊，因此可以使用 DI 中的其他服務 (如需要)。
* 在您的 `ValidationAttribute` 類別中實作 `IClientModelValidator`。 如果屬性不執行任何伺服器端驗證，且不需要任何來自 DI 的服務，則此方法可能適合。

### <a name="attributeadapter-for-client-side-validation"></a>用戶端驗證的屬性配接器

這種在 HTML 中轉譯 `data-` 屬性的方法，由範例應用程式中的 `ClassicMovie` 屬性使用。 使用此方法來新增用戶端驗證：

1. 建立自訂驗證屬性的屬性配接器類別。 自 [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2) 衍生類別。 建立會將 `data-` 屬性新增至轉譯輸出的 `AddValidation` 方法，如下列範例所示：

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. 建立實作 <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider> 的配接器提供者類別。 在 `GetAttributeAdapter` 方法中將自訂屬性傳遞至配接器的建構函式，如下列範例所示：

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. 在 `Startup.ConfigureServices` 中註冊 DI 的配接器提供者：

   [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a>用戶端驗證的 IClientModelValidator

這種在 HTML 中轉譯 `data-` 屬性的方法，由範例應用程式中的 `ClassicMovie2` 屬性使用。 使用此方法來新增用戶端驗證：

* 在自訂驗證屬性中，實作 `IClientModelValidator` 介面並建立 `AddValidation` 方法。 在 `AddValidation` 方法中，為驗證新增 `data-` 屬性，如下列範例所示：

  [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a>停用用戶端驗證

下列程式碼會停用 MVC 檢視中的用戶端驗證：

[!code-csharp[](validation/samples_snapshot/2.x/Startup2.cs?name=snippet_DisableClientValidation)]

以及在 Razor Pages 中：

[!code-csharp[](validation/samples_snapshot/2.x/Startup3.cs?name=snippet_DisableClientValidation)]

停用用戶端驗證的另一個選項，是將 *.cshtml* 檔案中對 `_ValidationScriptsPartial` 的參考標記為註解。

## <a name="additional-resources"></a>其他資源

* [System.ComponentModel.DataAnnotations 命名空間](xref:System.ComponentModel.DataAnnotations)
* [模型繫結](model-binding.md)

::: moniker-end
