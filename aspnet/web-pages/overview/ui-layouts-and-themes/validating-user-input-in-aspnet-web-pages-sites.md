---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: "驗證使用者輸入，在 ASP.NET Web Pages (Razor) 站台 |Microsoft 文件"
author: tfitzmac
description: "本文將討論如何驗證您從使用者取得的資訊&mdash;也就是以確定使用者輸入有效 HTML 中的資訊中 form 存新檔..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 3bde2a4ea69577ebcbe3e9e89a7ee07e6ece8dd1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a>ASP.NET Web Pages (Razor) 網站中驗證使用者輸入
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文將討論如何驗證您從使用者取得的資訊&mdash;亦即，若要確定使用者輸入有效 HTML 中的資訊表單 ASP.NET Web Pages (Razor) 網站中。
> 
> 您將學習：
> 
> - 如何檢查使用者的輸入符合您定義的驗證準則。
> - 如何判斷是否已通過所有驗證測試。
> - 如何顯示驗證錯誤 （以及如何將它們格式化）。
> - 如何驗證不會直接來自使用者的資料。
> 
> 這些是 ASP.NET 程式設計概念，文章中：
> 
> - `Validation`協助程式。
> - `Html.ValidationSummary`和`Html.ValidationMessage`方法。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> 本教學課程也適用於 ASP.NET Web Pages 2。


本文包含下列章節：

- [使用者輸入驗證的概觀](#Overview_of_User_Input_Validation)
- [驗證使用者輸入](#Validating_User_Input)
- [加入用戶端驗證](#Adding_Client-Side_Validation)
- [格式化的驗證錯誤](#Formatting_Validation_Errors)
- [驗證不會直接來自使用者的資料](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a>使用者輸入驗證的概觀

如果您要求使用者在頁面中輸入資訊 — 比方說，在表單，請務必確定使用者輸入的值是有效。 例如，您不想處理遺失重要資訊的表單。

當使用者輸入 HTML 表單的值時，這些輸入的值是字串。 在許多情況下，您需要的值會是其他資料類型，例如整數或日期。 因此，您也必須確定使用者輸入的值可以正確地轉換成適當的資料類型。

您也可能會有一些限制的值。 即使使用者正確輸入的整數，例如，您可能需要確定值落在特定範圍內。

![使用 CSS 樣式類別的驗證錯誤](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> **重要**驗證使用者輸入也是很重要的安全性。 當您限制使用者可以在表單中輸入的值時，會減少某人可以輸入值，可能會危害您的站台安全性的機會。


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a>驗證使用者輸入

在 ASP.NET Web Pages 2 中，您可以使用`Validator`測試使用者輸入的協助程式。 基本的方法是執行下列作業：

1. 判斷哪個輸入您想要驗證項目 （欄位）。

    您通常會驗證中的值`<input>`表單中的項目。 不過，最好以驗證所有輸入，即使輸入來自受條件約束的項目，例如`<select>`清單。 這有助於確保使用者不略過網頁上的控制項，並送出表單。
2. 網頁程式碼中加入個別的驗證檢查，每個輸入項目所使用的方法`Validation`協助程式。

    若要檢查必要的欄位，請使用`Validation.RequireField(field, [error message])`（適用於個別的欄位） 或`Validation.RequireFields(field1, field2, ...))`（如欄位的清單）。 對於其他類型的驗證，使用`Validation.Add(field, ValidationType)`。 如`ValidationType`，您可以使用這些選項：

    `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField [, error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min, max [, error message])`  
`Validator.RegEx(pattern [, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`
3. 當提交頁面時，請檢查驗證是否已通過檢查`Validation.IsValid`:

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    如果有任何驗證錯誤，則會略過正常網頁處理。 比方說，如果頁的目的是要更新資料庫，您沒有進行，直到所有驗證錯誤已獲得都修正。
4. 如果有驗證錯誤，在網頁標記中顯示錯誤訊息使用`Html.ValidationSummary`或`Html.ValidationMessage`，或兩者。

下列範例將說明這些步驟的頁面。

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

若要驗證的運作方式，請執行此頁面並故意出錯。 例如，以下是頁面看起來像如果您忘記輸入課程名稱，如果您輸入，而且如果您輸入無效的日期：

![呈現的網頁中的驗證錯誤](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a>加入用戶端驗證

根據預設，使用者送出頁面之後，會驗證使用者輸入，也就是執行伺服器程式碼中的驗證。 這種方法的缺點是使用者不知道它們所做錯誤直到後的送出此頁面。 如果表單是長或太複雜，報告錯誤，只有在送出頁面之後，才可以對使用者很不方便。

您可以新增支援，以在用戶端指令碼中執行驗證。 在此情況下，使用者在瀏覽器中，會執行驗證。 例如，假設您指定的值應為整數。 如果使用者輸入非整數值，只要使用者離開項目欄位被報告的錯誤。 使用者會立即回應，方便的時候。 用戶端為基礎的驗證也可以減少使用者已送出多個錯誤，更正表單的次數。

> [!NOTE]
> 即使您使用用戶端驗證時，在伺服器程式碼永遠也執行驗證。 在伺服端程式碼中執行驗證是安全性措施，以防使用者略過用戶端為基礎的驗證。


1. 註冊頁面中的下列 JavaScript 程式庫：  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

 兩個程式庫是內容傳遞網路 (CDN)，從可載入，因此您不一定需要對您的電腦或伺服器。 不過，您必須擁有的本機副本*jquery.validate.unobtrusive.js*。 如果您沒有已正在使用 WebMatrix 範本 (例如**入門網站**)，其中包含文件庫，請建立 Web Pages 站台中，取決於**入門網站**。 然後將複製*.js*檔案目前的站台。
2. 在標記中，針對您正在驗證時，每個項目加入至呼叫`Validation.For(field)`。 這個方法會發出用戶端驗證所使用的屬性。 (而不是發出實際的 JavaScript 程式碼，方法就會發出屬性，例如`data-val-...`。 這些屬性支援用來執行工作的 jQuery 不顯眼的用戶端驗證）。

下列頁面會顯示如何將用戶端驗證功能新增至先前範例所示。

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

並非所有用戶端上執行的驗證檢查。 特別是，不會在用戶端上執行 （整數、 日期和等等） 的資料類型驗證。 在用戶端和伺服器運作下列檢查：

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

在此範例中，用戶端程式碼不適用於測試的是有效的日期。 不過，測試會執行伺服器程式碼中。

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a>格式化的驗證錯誤

您可以控制如何藉由定義 CSS 類別具有下列保留的名稱會顯示驗證錯誤：

- `field-validation-error`. 定義的輸出`Html.ValidationMessage`方法時顯示錯誤。
- `field-validation-valid`. 定義的輸出`Html.ValidationMessage`方法時沒有發生錯誤。
- `input-validation-error`. 定義如何`<input>`發生錯誤時，會呈現項目。 (例如，您可以使用這個類別來設定的背景色彩&lt;輸入&gt;為不同的色彩如果其值為無效的項目。)只有在用戶端驗證 （在 ASP.NET Web Pages 2) 會使用這個 CSS 類別。
- `input-validation-valid`. 定義的外觀`<input>`時沒有發生錯誤的項目。
- `validation-summary-errors`. 定義的輸出`Html.ValidationSummary`顯示錯誤清單的方法。
- `validation-summary-valid`. 定義的輸出`Html.ValidationSummary`方法時沒有發生錯誤。

下列`<style>`區塊顯示錯誤條件的規則。

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

如果您從文章中稍早範例頁面中包含這個樣式區塊，錯誤顯示看起來像下圖：

![使用 CSS 樣式類別的驗證錯誤](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> 如果您不使用用戶端驗證在 ASP.NET Web Pages 2，CSS 類別的`<input>`項目 (`input-validation-error`和`input-validation-valid`沒有任何作用。


### <a name="static-and-dynamic-error-display"></a>靜態和動態錯誤顯示

CSS 規則成對出現，例如`validation-summary-errors`和`validation-summary-valid`。 這些配對可讓您定義這兩個條件的規則： 錯誤狀況和 「 標準 」 （非錯誤） 條件。 請務必了解，會一律會呈現錯誤顯示的標記，即使沒有任何錯誤。 例如，若有`Html.ValidationSummary`方法標記中的，第一次要求頁面時，即使網頁原始檔會包含下列標記：

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

換句話說，`Html.ValidationSummary`方法一律會呈現`<div>`項目和清單，即使錯誤清單是空的。 同樣地，`Html.ValidationMessage`方法一律會呈現`<span>`項目做為個別欄位錯誤，即使沒有任何錯誤的預留位置。

在某些情況下，顯示錯誤訊息可能會造成頁面以重新排列而且可能會導致元素在頁面上四處移動。 結尾的 CSS 規則`-valid`可讓您定義的版面配置，有助於防止這個問題。 例如，您可以定義`field-validation-error`和`field-validation-valid`兩者有相同固定大小。 這樣一來，欄位的顯示區域是靜態，不會變更頁面流程，如果會顯示錯誤訊息。

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a>驗證不會直接來自使用者的資料

有時候，您必須驗證不會直接來自 HTML 表單的資訊。 典型的範例是的頁面，其中一個值傳入查詢字串，如下列範例所示：

`http://server/myapp/EditClassInformation?classid=1022`

您想要在此情況下，請確定傳遞至頁面的值 (這裡的值為 1022年`classid`) 有效。 您無法直接使用`Validation`協助程式執行這項驗證。 不過，您可以使用其他功能的驗證系統，例如顯示驗證錯誤訊息的能力。

> [!NOTE] 
> 
> **重要**一律驗證您從取得的值*任何*來源，包括表單欄位值、 查詢字串值和 cookie 值。 很容易讓人變更這些值 （可能是適用於惡意用途）。 因此您必須檢查這些值，以保護您的應用程式。


下列範例會示範如何，您可能會驗證傳遞查詢字串中的值。 中的程式碼測試的值不是空白，且它是整數。

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

請注意，要求不是送出表單時，會執行測試 (`if(!IsPost)`)。 這項測試會傳送要求頁面時，第一次，但不是要求時送出表單。

若要顯示這個錯誤，您可以將錯誤加入至驗證錯誤清單藉由呼叫`Validation.AddFormError("message")`。 如果頁面包含呼叫`Html.ValidationSummary`方法時，錯誤會顯示，如同使用者輸入驗證錯誤。

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>其他資源

[使用在 ASP.NET Web Pages 站台中的 HTML 表單](https://go.microsoft.com/fwlink/?LinkID=202892)
