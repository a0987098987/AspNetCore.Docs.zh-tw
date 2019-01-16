---
title: ASP.NET Core MVC 中的模型驗證
author: tdykstra
description: 了解 ASP.NET Core MVC 中的模型驗證。
ms.author: riande
ms.custom: mvc
ms.date: 01/04/2019
uid: mvc/models/validation
ms.openlocfilehash: f3a34972006b5fdee307c9a8d9989b2cc1e36893
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099379"
---
# <a name="model-validation-in-aspnet-core-mvc"></a>ASP.NET Core MVC 中的模型驗證

作者：[Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-validation"></a>模型驗證簡介

應用程式必須驗證資料，才能將資料儲存在資料庫中。 資料必須檢查是否有潛在安全性威脅、確認其類型和大小是否適當地格式化，而且必須符合您的規則。 驗證是必要的，但它在實作方面可能鎖碎繁複。 在 MVC 中，驗證會在用戶端和伺服器上進行。

但幸好 .NET 已將驗證抽象化為驗證屬性。 這些屬性包含驗證程式碼，藉此減少您必須撰寫的程式碼數量。

在 ASP.NET Core 2.2 與更新版本中，若 ASP.NET Core 執行階段可以判斷給定模型圖形不需要驗證，它會跳過驗證。 當驗證無法驗證或沒有任何關聯之驗證程式的模型時，跳過驗證可大幅改善效能。 跳過的驗證包括諸如基本類型集合 (`byte[]`、`string[]`、`Dictionary<string, string>` 等) 的物件，或沒有任何驗證程式的複雜物件。

[從 GitHub 檢視或下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample)。

## <a name="validation-attributes"></a>驗證屬性

驗證屬性是設定模型驗證的方式，因此概念上類似於資料庫資料表中的欄位驗證。 包括條件約束，例如指派資料類型或必要欄位。 其他驗證類型還包括將模式套用至資料以強制執行商務規則，例如信用卡、電話號碼或電子郵件地址。 驗證屬性可讓您更輕鬆地強制執行這些需求，而且使用起來更容易。

驗證屬性指定於屬性層級： 

```csharp
[Required]
public string MyProperty { get; set; } 
```

以下是應用程式中已註解的 `Movie` 模型，其儲存電影和電視節目的相關資訊。 大部分屬性都是必要的，而且有幾個字串屬性具有長度需求。 此外，`Price` 屬性還有 0 至 $999.99 的數字範圍限制，以及自訂驗證屬性。

[!code-csharp[](validation/sample/Movie.cs?range=6-29)]

只要透過模型讀取就會顯示此應用程式資料的相關規則，因此更容易維護程式碼。 以下是幾個常用的內建驗證屬性：

* `[CreditCard]`：驗證屬性是否具有信用卡格式。

* `[Compare]`：驗證模型比對中的兩個屬性。

* `[EmailAddress]`：驗證屬性是否具有電子郵件格式。

* `[Phone]`：驗證屬性是否具有電話格式。

* `[Range]`：驗證屬性值是否落在指定的範圍內。

* `[RegularExpression]`：驗證資料是否符合指定的規則運算式。

* `[Required]`：將屬性設定為必要。

* `[StringLength]`：驗證字串屬性的長度是否不超過指定的上限。

* `[Url]`：驗證屬性是否具有 URL 格式。

MVC 支援將任何衍生自 `ValidationAttribute` 的屬性用於驗證。 您可以在 [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) 命名空間中找到許多實用的驗證屬性。

有時候您可能需要比內建屬性所提供更多的功能。 此時，您可以藉由衍生自 `ValidationAttribute` 或變更您的模型來實作 `IValidatableObject`，以建立自訂驗證屬性。

## <a name="notes-on-the-use-of-the-required-attribute"></a>使用必要屬性的注意事項

不可為 Null 的[實值類型](/dotnet/csharp/language-reference/keywords/value-types) (如 `decimal`、`int`、`float` 和 `DateTime`) 原本就是必要項目，而且不需要 `Required` 屬性。 應用程式不會對標示為 `Required` 之不可為 Null 的型別執行伺服器端驗證檢查。

與驗證和驗證屬性無關的 MVC 模型繫結，會拒絕送出含有遺漏值或空白字元之不可為 Null 型別的表單欄位。 如果目標屬性 (property) 上沒有 `BindRequired` 屬性 (attribute)，模型繫結會忽略不可為 Null 型別的遺漏資料 (傳入表單資料中不會有表單欄位)。

[BindRequired 屬性](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (另請參閱 <xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes>) 適用於確保表單資料已完成。 套用至屬性時，模型繫結系統需要該屬性的值。 套用至類型時，模型繫結系統需要該類型之所有屬性的值。

當您使用 [Nullable\<T> 類型](/dotnet/csharp/programming-guide/nullable-types/) (例如 `decimal?` 或 `System.Nullable<decimal>`) 並將它標示為 `Required` 時，就會將屬性視為標準可為 Null 的型別 (例如 `string`) 來執行伺服器端驗證檢查。

用戶端驗證要求表單欄位的值必須對應至模型屬性 (若已標示為 `Required`)，以及不可為 Null 型別的屬性 (若未標示為 `Required`)。 `Required` 可用來控制用戶端驗證錯誤訊息。

## <a name="model-state"></a>模型狀態

模型狀態代表送出之 HTML 表單值中的驗證錯誤。

MVC 會繼續驗證欄位，直到達到最大錯誤數目為止 (預設為 200 個)。 您可以在 `Startup.ConfigureServices` 中使用下列程式碼來設定此數目：

[!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors)]

## <a name="handle-model-state-errors"></a>處理模型狀態錯誤

執行控制器動作前，會先進行模型驗證。 此動作的責任為檢查 `ModelState.IsValid` 並做出適當回應。 在許多情況下，適當回應就是傳回錯誤回應，最好能夠詳細列出模型驗證失敗的原因。

::: moniker range=">= aspnetcore-2.1"

當 `ModelState.IsValid` 使用 `[ApiController]` 屬性在 Web API 控制器中求出 `false` 時，會傳回包含問題詳細資料的自動 HTTP 400 回應。 如需詳細資訊，請參閱[自動 HTTP 400 回應](xref:web-api/index#automatic-http-400-responses)。

::: moniker-end

某些應用程式會選擇遵循標準慣例來處理模型驗證錯誤，在此情況下，篩選可能是實作這類原則的適當位置。 您應該測試動作在有效和無效模型狀態中的行為。

## <a name="manual-validation"></a>手動驗證

模型繫結和驗證完成之後，您可能想要重複其中數個部分。 例如，使用者可能在預期整數的欄位中輸入文字，或是您可能需要計算模型的屬性值。

此時，您可能需要手動執行驗證。 若要執行這項操作，請呼叫 `TryValidateModel` 方法，如下所示：

[!code-csharp[](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a>自訂驗證

驗證屬性符合大多數驗證需求。 不過，某些驗證規則是您業務特有的。 您的規則可能不是常用的資料驗證技術，例如確定欄位為必要或符合某個範圍的值。 在此情況下，自訂驗證屬性會是很好的解決方案。 在 MVC 中建立您自己的自訂驗證屬性十分簡單。 只要繼承自 `ValidationAttribute` 並覆寫 `IsValid` 方法即可。 `IsValid` 方法接受兩個參數，第一個是名為 *value* 的物件，第二個是名為 *validationContext* 的 `ValidationContext` 物件。 *Value* 是指您的自訂驗證程式正在驗證之欄位中的實際值。

在下列範例中，商務規則表示使用者可能未將 1960 年以後發行之電影的內容類型設定為 *Classic*。 `[ClassicMovie]` 屬性會先檢查內容類型，如果是 Classic，會再檢查發行日期是否晚於 1960 年。 如果是在 1960 年以後發行，則驗證失敗。 用來驗證資料的屬性接受代表年份的整數參數。 您可以擷取屬性建構函式中的參數值，如下所示：

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?range=9-28)]

上述 `movie` 變數代表 `Movie` 物件，其中包含送出待驗證之表單中的資料。 在本例中，驗證程式碼會根據規則，檢查 `ClassicMovieAttribute` 類別之 `IsValid` 方法中的日期和內容類型。 驗證成功時，`IsValid` 會傳回 `ValidationResult.Success` 程式碼。 驗證失敗時，會傳回 `ValidationResult` 和錯誤訊息：

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?range=55-58)]

當使用者修改 `Genre` 欄位並送出表單時，`ClassicMovieAttribute` 的 `IsValid` 方法會確認電影是否為 Classic。 如同任何內建屬性，將 `ClassicMovieAttribute` 套用至 `ReleaseDate` 等屬性可確保進行驗證，如上述程式碼範例所示。 由於此範例僅適用於 `Movie` 類型，使用 `IValidatableObject` 會是更好的選擇，如下一個段落所示。

或者，您也可以透過在 `IValidatableObject` 介面上實作 `Validate` 方法，將此相同的程式碼放在模型中。 自訂驗證屬性適用於驗證個別屬性，而實作 `IValidatableObject` 則可用來實作類別層級驗證，如下所示。

[!code-csharp[](validation/sample/MovieIValidatable.cs?range=32-40)]

## <a name="client-side-validation"></a>用戶端驗證

用戶端驗證對使用者而言很方便。 它為使用者省下花在等候伺服器往返的時間。 對商務而言，即便是幾秒，若每天乘上好幾百倍，也可能讓您花上許多時間和費用，而倍感挫折。 直接和立即驗證可讓使用者更有效率地工作，並產生更佳品質的輸入和輸出。

您必須具有適當 JavaScript 指令碼參考的檢視，以確保用戶端驗證如下所示正常運作。

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

[jQuery 低調驗證](https://github.com/aspnet/jquery-validation-unobtrusive) (jQuery Unobtrusive Validation) 指令碼是建置在熱門 [jQuery 驗證](https://jqueryvalidation.org/) 外掛程式上的自訂 Microsoft 前端程式庫。 若沒有 jQuery 低調驗證，您就必須在兩個地方撰寫相同的驗證邏輯程式碼：一次在模型屬性 (property) 上的伺服器端驗證屬性 (attribute)，另一次在用戶端指令碼 (jQuery 驗證的 [`validate()`](https://jqueryvalidation.org/validate/) 方法範例顯示這可能會變得多麼複雜)。 相反地，MVC 的[標籤協助程式](xref:mvc/views/tag-helpers/intro)和 [HTML 協助程式](xref:mvc/views/overview)能夠使用模型屬性 (property) 中的驗證屬性 (attribute) 和類型中繼資料，來轉譯需要驗證之表單項目中的 HTML 5 [data- 屬性 (attribute)](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes)。 MVC 針對內建和自訂屬性都會產生 `data-` 屬性。 jQuery 低調驗證會接著剖析 `data-` 屬性並將邏輯傳遞至 jQuery 驗證，以有效地將伺服器端驗證邏輯「複製」到用戶端。 您可以使用相關的標籤協助程式，來顯示用戶端的驗證錯誤，如下所示：

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

上述標籤協助程式會轉譯下列 HTML。 請注意，HTML 輸出中的 `data-` 屬性 (attribute) 會對應至 `ReleaseDate` 屬性 (property) 的驗證屬性 (attribute)。 下面的 `data-val-required` 屬性包含使用者未填入發行日期欄位時所要顯示的錯誤訊息。 jQuery 低調驗證會將此值傳遞至 jQuery 驗證的 [`required()`](https://jqueryvalidation.org/required-method/) 方法，然後在隨附的 **\<span>** 項目中顯示該訊息。

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
                id="ReleaseDate" name="ReleaseDate" value="" />
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

用戶端驗證可避免送出無效的表單。 [送出] 按鈕會執行 JavaScript 以送出表單或顯示錯誤訊息。

MVC 會根據屬性 (property) 的 .NET 資料類型來決定類型屬性 (attribute) 值，並可能使用 `[DataType]` 屬性 (attribute) 加以覆寫。 基底 `[DataType]` 屬性不會執行任何實際伺服器端驗證。 瀏覽器會選擇自己的錯誤訊息，並按照想要的方式來顯示這些錯誤，不過 jQuery 低調驗證套件可能會覆寫這些訊息，並以與其他訊息一致的方式來顯示。 當使用者套用 `[DataType]` 子類別 (例如 `[EmailAddress]`) 時，最有可能發生此情況。

### <a name="add-validation-to-dynamic-forms"></a>將驗證新增至動態表單

由於 jQuery 低調驗證會在第一次載入頁面時將驗證邏輯和傳遞參數至 jQuery 驗證，因此動態產生的表單不會自動展示驗證。 相反地，您必須指示 jQuery 低調驗證在建立動態表單之後立即進行剖析。 例如，下列程式碼示範如何在透過 AJAX 新增的表單上設定用戶端驗證。

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

`$.validator.unobtrusive.parse()` 方法接受 jQuery 選取器的一個引數。 此方法會指示 jQuery 低調驗證在該選取器內剖析表單的 `data-` 屬性。 這些屬性的值會接著傳遞至 jQuery 驗證外掛程式，讓表單展示所需的用戶端驗證規則。

### <a name="add-validation-to-dynamic-controls"></a>將驗證新增至動態控制項

您也可以在動態產生個別控制項 (例如 `<input/>` 和 `<select/>`) 時，更新表單上的驗證規則。 您無法將這些項目的選取器直接傳遞至 `parse()` 方法，因為周圍的表單已經過剖析，因此不會更新。 相反地，您會先移除現有的驗證資料，再重新剖析整份表單，如下所示：

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

## <a name="iclientmodelvalidator"></a>IClientModelValidator

您可以建立自訂屬性的用戶端端邏輯，為 [jQuery Validation](http://jqueryvalidation.org/documentation/) 建立配接器的[低調驗證](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) 會將它當作驗證的一部分自動為您在用戶端上執行。 第一個步驟是實作 `IClientModelValidator` 介面，以控制要新增的 data- 屬性，如下所示：

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

實作此介面的屬性可以新增至 HTML 屬性來產生欄位。 檢查 `ReleaseDate` 項目的輸出會顯示類似於上述範例的 HTML，不同之處在於現在有 `IClientModelValidator` 的 `AddValidation` 方法中所定義的 `data-val-classicmovie` 屬性。

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

低調驗證使用 `data-` 屬性中的資料來顯示錯誤訊息。 不過，在您將規則或訊息新增至 jQuery 的 `validator` 物件之前，jQuery 並不清楚有哪些規則或訊息。 這顯示在下列範例中，該範例會將自訂 `classicmovie` 用戶端驗證方法新增到 jQuery `validator` 物件。 如需 `unobtrusive.adapters.add` 方法的說明，請參閱 [ASP.NET MVC 中的低調用戶端驗證](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) \(英文\)。

[!code-javascript[](validation/sample/Views/Movies/Create.cshtml?name=snippet_UnobtrusiveValidation)]

使用上述程式碼時，`classicmovie` 方法會在電影發行日期執行用戶端驗證。 若方法傳回 `false`，會顯示錯誤訊息。

## <a name="remote-validation"></a>遠端驗證

遠端驗證功能適用於需要針對伺服器上的資料來驗證用戶端上的資料之情況。 例如，您的應用程式可能需要確認電子郵件或使用者名稱是否已在使用中；為了執行這項操作，它必須查詢大量資料。 為驗證一或多個欄位而下載大型資料集會耗用太多資源， 也可能會公開機密資訊。 替代方案是提出一個往返要求來驗證一個欄位。

您可以在兩個步驟的程序中實作遠端驗證。 首先，您必須為模型加註 `[Remote]` 屬性。 `[Remote]` 屬性接受多個多載，您可以使用這些多載將用戶端 JavaScript 導向至適當的程式碼進行呼叫。 下列範例指向 `Users` 控制器的 `VerifyEmail` 動作方法。

[!code-csharp[](validation/sample/User.cs?range=7-8)]

第二個步驟將驗證程式碼放在 `[Remote]` 屬性中所定義的對應動作方法中。 根據 jQuery 驗證[遠端](https://jqueryvalidation.org/remote-method/)方法文件，伺服器回應必須是符合下列任一條件的 JSON 字串：

* 有效元素的 `"true"`。
* 無效元素的 `"false"`、`undefined` 或 `null`，使用預設錯誤訊息。

若伺服器回應是字串 (例如 `"That name is already taken, try peter123 instead"`)，該字串會取代預設字串而顯示為自訂錯誤訊息。

`VerifyEmail` 方法的定義遵循這些規則，如下所示。 如果電子郵件已在使用中，則會傳回驗證錯誤訊息；如果電子郵件可用，則會傳回 `true`，並將結果包裝在 `JsonResult` 物件中。 用戶端可接著使用此傳回值繼續進行，或顯示錯誤訊息 (如有需要)。

[!code-csharp[](validation/sample/UsersController.cs?range=19-28)]

現在，當使用者輸入電子郵件時，檢視中的 JavaScript 會發出遠端呼叫，以查看該電子郵件是否已在使用中；若在使用中，則會顯示錯誤訊息。 否則，使用者可以像往常一樣送出表單。

`[Remote]` 屬性 (attribute) 的 `AdditionalFields` 屬性 (property) 可用於針對伺服器上的資料來驗證欄位組合。 例如，如果上述 `User` 模型有兩個額外的屬性 `FirstName` 和 `LastName`，您可能想要確認沒有任何現有的使用者已有該組名稱。 您可以定義新的屬性，如下列程式碼所示：

[!code-csharp[](validation/sample/User.cs?range=10-13)]

`AdditionalFields` 可能已明確設定為 `"FirstName"` 和 `"LastName"` 字串，但使用上述 [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) 運算子可簡化稍後的重構。 執行驗證的動作方法必須接受兩個引數，一個是 `FirstName` 的值，另一個是 `LastName` 的值。

[!code-csharp[](validation/sample/UsersController.cs?range=30-39)]

現在，當使用者輸入名字和姓氏時，JavaScript 會：

* 發出遠端呼叫以查看該組名稱是否已在使用中。
* 如果已在使用中，則會顯示錯誤訊息。
* 如果不在使用中，則使用者可以送出表單。

如果您需要驗證具有 `[Remote]` 屬性的兩個或多個額外欄位，請以逗號分隔清單來提供這些欄位。 例如，若要將 `MiddleName` 屬性 (Property) 新增至模型，請設定 `[Remote]` 屬性 (attribute)，如下列程式碼所示：

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

如同所有屬性引數，`AdditionalFields` 必須是常數運算式。 因此，您不得使用[字串插值](/dotnet/csharp/language-reference/keywords/interpolated-strings)或呼叫 [`string.Join()`](https://msdn.microsoft.com/library/system.string.join(v=vs.110).aspx) 來初始化 `AdditionalFields`。 針對每個新增 `[Remote]` 屬性的額外欄位，都必須另外新增一個引數至控制器動作方法。
