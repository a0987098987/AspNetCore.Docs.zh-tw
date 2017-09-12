---
title: "ASP.NET Core MVC 中的模型驗證"
author: rick-anderson
description: "導入了 ASP.NET Core MVC 中的模型驗證。"
keywords: "ASP.NET Core，MVC、 驗證"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 3a8676dd-7ed8-4a05-bca2-44e288ab99ee
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/validation
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: be130c24f5baf643a4c9493a33ec45bdd4cc66ed
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-model-validation-in-aspnet-core-mvc"></a>ASP.NET Core MVC 中的模型驗證的簡介

由[Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-validation"></a>模型驗證的簡介

應用程式將資料儲存在資料庫之前，應用程式必須驗證的資料。 資料必須進行檢查，確認它適當地格式化類型和大小，並必須符合規則的潛在安全性威脅。 驗證是必要的雖然它可以是重複而且單調實作。 在 MVC 中，驗證是用戶端和伺服器上。

幸運的是，.NET 具有抽象化驗證屬性的驗證。 這些屬性包含驗證程式碼，藉此減少您必須撰寫的程式碼數量。

[檢視或從 GitHub 下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample)。

## <a name="validation-attributes"></a>驗證屬性

驗證屬性是設定模型驗證，因此它是在概念上類似資料庫資料表中的欄位上的驗證方法。 這包括條件約束，例如指派必要的欄位或資料型別。 其他的驗證類型包括套用到要強制執行商務規則，例如信用卡、 電話號碼或電子郵件地址資料的模式。 驗證屬性，請強制執行更簡單且更輕鬆地使用這些需求。

以下是 註解式`Movie`模型儲存電影的電視節目的相關資訊的應用程式。 大部分的屬性所需，且字串的數個屬性具有長度要求。 此外，還有數字範圍限制為就地`Price`從 0 至 $999.99，以及自訂驗證屬性的屬性。

[!code-csharp[Main](validation/sample/Movie.cs?range=6-31)]

只讀取透過模型會顯示此應用程式，以便更容易維護的程式碼的資料相關的規則。 以下是幾個常用的內建驗證屬性：

* `[CreditCard]`： 會驗證此屬性有信用卡格式。

* `[Compare]`： 驗證模型比對中的兩個屬性。

* `[EmailAddress]`： 會驗證此屬性有電子郵件格式。

* `[Phone]`： 會驗證此屬性有電話格式。

* `[Range]`： 驗證就是屬性值落在指定範圍內。

* `[RegularExpression]`： 會驗證指定的規則運算式符合的資料。

* `[Required]`： 使所需的屬性。

* `[StringLength]`： 驗證字串屬性最多具有指定的長度上限。

* `[Url]`： 驗證屬性的 URL 格式。

MVC 支援的任何屬性，衍生自`ValidationAttribute`進行驗證。 許多有用的驗證屬性位於[System.ComponentModel.DataAnnotations](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations)命名空間。

可能需要更多的功能提供的內建屬性的執行個體。 若是在這些時間點，您可以建立自訂的驗證屬性衍生自`ValidationAttribute`或變更您的模型來實作`IValidatableObject`。

## <a name="model-state"></a>模型狀態

模型狀態表示中提交的 HTML 表單值的驗證錯誤。

MVC 會繼續直到到達驗證欄位的錯誤 (依預設為 200) 的數目上限。 您可以藉由將下列程式碼中設定這個數字`ConfigureServices`方法中的*Startup.cs*檔案：

[!code-csharp[Main](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a>處理模型狀態錯誤

模型驗證發生在之前所叫用每個控制器動作和動作方法必須負責檢查`ModelState.IsValid`和作出適當回應。 在許多情況下，適當的反應就會傳回某種類型的錯誤回應，在理想情況下，詳列出模型驗證失敗的原因。

某些應用程式會選擇遵循標準慣例來處理模型驗證錯誤，其中案例篩選器可能會實作這類原則的適當位置。 您應該測試您的動作具有有效和無效的模型狀態的行為方式。

## <a name="manual-validation"></a>手動驗證

模型繫結和驗證完成後，您可能想要重複部分。 例如，使用者可能預期接收整數，欄位中輸入文字或您可能要計算的模型屬性的值。

您可能需要手動執行驗證。 若要這樣做，請呼叫`TryValidateModel`方法，如下所示：

[!code-csharp[Main](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a>自訂驗證

驗證屬性適用於大多數的驗證需求。 不過，一些驗證規則專屬於您企業中，因為它們並不是只需要確認欄位的一般資料驗證，或符合某個範圍的值。 針對這些案例，自訂驗證屬性會是很好的解決方案。 在 MVC 中建立您自己的自訂驗證屬性十分簡單。 只會繼承`ValidationAttribute`，並覆寫`IsValid`方法。 `IsValid`方法接受兩個參數，第一個是名為物件*值*第二個是`ValidationContext`名為物件*validationContext*。 *值*正在驗證您的自訂驗證程式的欄位參考的實際值。

在下列範例中，商務規則表示使用者可能無法設定內容類型為*傳統*1960年之後發行的電影。 `[ClassicMovie]`屬性會先檢查內容類型，如果是典型，然後檢查是否晚於 1960年的發行日期。 它會釋放 1960年之後，如果驗證失敗。 這個屬性接受整數參數代表年份可讓您驗證資料。 您可以擷取該屬性的建構函式，在參數的值如下所示：

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=9-28)]

`movie`變數代表上方`Movie`物件，其中包含要驗證的送出表單的資料。 在此情況下，驗證程式碼會檢查日期和內容類型中的`IsValid`方法`ClassicMovieAttribute`依據規則的類別。 成功驗證後`IsValid`傳回`ValidationResult.Success`程式碼，以及當驗證失敗，`ValidationResult`並出現錯誤訊息。 當使用者修改`Genre`欄位及提交表單時，`IsValid`方法`ClassicMovieAttribute`將電影是否傳統驗證。 任何內建屬性，例如套用`ClassicMovieAttribute`屬性例如`ReleaseDate`以確保驗證發生時，先前的程式碼範例所示。 因為此範例僅適用於`Movie`型別，較好的選擇是使用`IValidatableObject`下列段落中所示。

或者，這個相同的程式碼可以架設在模型中藉由實作`Validate`方法`IValidatableObject`介面。 自訂的驗證屬性適用於驗證個別屬性，而實作`IValidatableObject`可以用來實作類別層級驗證，如下所示。

[!code-csharp[Main](validation/sample/MovieIValidatable.cs?range=33-41)]

## <a name="client-side-validation"></a>用戶端驗證

用戶端驗證是絕佳方便使用者。 它會將儲存它們要否則花的時間等候往返到伺服器。 商務詞彙表示，即使是幾個句號乘以達數百次每天將加入最多會有很多時間、 支出和挫折度。 直接和立即驗證可讓使用者更有效率地運作，並產生較佳的品質，輸入和輸出。

您必須具有適當的 JavaScript 指令碼參考的檢視以進行用戶端驗證，如下所示的工作。

[!code-html[Main](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-html[Main](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

要驗證資料並顯示任何錯誤訊息，使用 JavaScript 則 MVC 會使用驗證屬性，除了從 模型屬性的型別中繼資料。 當您使用 MVC 來呈現模型使用從表單元素[標記協助程式](xref:mvc/views/tag-helpers/intro)或[HTML helper](xref:mvc/views/overview)將 HTML 5[資料屬性](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes)需要驗證，做為表單項目中如下所示。 MVC 會產生`data-`內建和自訂屬性的屬性。 您可以使用如下所示的相關標記協助程式的用戶端上顯示驗證錯誤：

[!code-html[Main](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

上述的標記協助程式會將下列 HTML 轉譯。 請注意，`data-`屬性的 html 輸出對應的驗證屬性`ReleaseDate`屬性。 `data-val-required`屬性包含錯誤訊息顯示如果使用者不會填入 [發行日期] 欄位中，而且該訊息會顯示隨附`<span>`項目。

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

用戶端驗證可避免送出之前格式無效。 [提交] 按鈕會執行 JavaScript，或者送出表單會顯示錯誤訊息。

MVC 判斷型別屬性，可能是覆寫使用的.NET 資料類型為基礎的屬性值`[DataType]`屬性。 基底`[DataType]`屬性不會實際的伺服器端驗證。 瀏覽器選擇他們自己的錯誤訊息，並顯示這些錯誤，不過他們希望，不過 jQuery 驗證不顯眼封裝可以覆寫的訊息，並與其他以一致的方式顯示。 發生這種情況最明顯當使用者套用`[DataType]`子類別，例如`[EmailAddress]`。

## <a name="iclientmodelvalidator"></a>IClientModelValidator

您可能會對您的自訂屬性，建立用戶端端邏輯和[不顯眼的驗證](http://jqueryvalidation.org/documentation/)它為您自動一部分驗證用戶端上執行。 第一個步驟是控制哪些資料屬性加入實作`IClientModelValidator`介面如下所示：

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

實作這個介面的屬性可以將 HTML 屬性，加入產生的欄位。 檢查輸出`ReleaseDate`項目會顯示除了現在沒有外，類似於上述範例中，HTML`data-val-classicmovie`中所定義的屬性`AddValidation`方法`IClientModelValidator`。

```html
<input class="form-control" type="datetime"
data-val="true"
data-val-classicmovie="Classic movies must have a release year earlier than 1960."
data-val-classicmovie-year="1960"
data-val-required="The ReleaseDate field is required."
id="ReleaseDate" name="ReleaseDate" value="" />
```

不顯眼的驗證會使用中的資料`data-`顯示錯誤訊息的屬性。 不過，jQuery 不了解規則或訊息，直到您將它們加入 jQuery 的`validator`物件。 這顯示在下列範例中，加入名為方法`classicmovie`包含自訂用戶端驗證程式碼，加入 jQuery`validator`物件。

[!code-javascript[Main](validation/sample/Views/Movies/Create.cshtml?range=71-93)]

現在 jQuery 有執行自訂的 JavaScript 驗證，以及該驗證程式碼會傳回 false 時所顯示的錯誤訊息的資訊。

## <a name="remote-validation"></a>遠端驗證

遠端驗證是很棒的功能，您需要驗證用戶端對伺服器上的資料上的資料時使用。 例如，可能需要您的應用程式，請確認是否有電子郵件或使用者名稱已在使用，而且它必須查詢大量的資料，若要這樣做。 下載大型的其中一個驗證的資料集或幾個欄位會耗用太多資源。 它可能也會公開機密資訊。 另一個方法是反覆存取要求驗證欄位。

您可以在兩個步驟的程序中實作遠端驗證。 首先，您必須加上註解您的模型與`[Remote]`屬性。 `[Remote]`屬性接受多個多載可用來導向到適當的程式碼以呼叫的用戶端 JavaScript。 此範例會指向`VerifyEmail`動作方法的`Users`控制站。

[!code-csharp[Main](validation/sample/User.cs?range=5-9)]

第二個步驟將驗證程式碼中對應的動作方法中所定義`[Remote]`屬性。 它會傳回`JsonResult`用戶端可以用來繼續執行或是暫停並顯示一則錯誤，如有需要。

[!code-none[Main](validation/sample/UsersController.cs?range=19-28)]

現在當使用者輸入的電子郵件，檢視中的 JavaScript 進行遠端呼叫是否已佔用該電子郵件，請參閱，而且如果是，接著會顯示錯誤訊息。 否則，使用者可以像往常一樣送出表單。
