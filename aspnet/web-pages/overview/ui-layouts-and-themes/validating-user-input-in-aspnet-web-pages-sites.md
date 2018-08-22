---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: 驗證使用者輸入，在 ASP.NET Web Pages (Razor) 網站 |Microsoft Docs
author: tfitzmac
description: 這篇文章討論如何驗證您從使用者取得的資訊&mdash;也就是若要確定使用者輸入有效 HTML 中的資訊表單中存新檔...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 761d6965883f46e1253f1fb0105cb0d4539fcf9d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834203"
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a>ASP.NET Web Pages (Razor) 網站中驗證使用者輸入
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 這篇文章討論如何驗證您從使用者取得的資訊&mdash;也就是若要確定使用者輸入有效 HTML 中的資訊表單中的 ASP.NET Web Pages (Razor) 網站。
> 
> 您將學到什麼：
> 
> - 如何檢查使用者的輸入符合您定義的驗證準則。
> - 如何判斷是否已通過所有驗證測試。
> - 如何顯示驗證錯誤 （以及如何將它們格式化）。
> - 如何驗證不是直接來自使用者的資料。
> 
> 這些是 ASP.NET 程式設計概念，文件中：
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
- [新增用戶端驗證](#Adding_Client-Side_Validation)
- [格式化驗證錯誤](#Formatting_Validation_Errors)
- [不是直接來自使用者的驗證資料](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a>使用者輸入驗證的概觀

如果您要求使用者在頁面中輸入資訊 — 比方說，到表單，務必要確定這些輸入的值都有效。 比方說，您不想要處理的表單，遺失重要資訊。

當使用者輸入 HTML 表單中的值時，其輸入的值會是字串。 在許多情況下，您需要的值會是一些其他資料類型，例如整數或日期。 因此，您也必須確定使用者輸入的值可以正確地轉換成適當的資料類型。

您可能還有一些限制的值。 即使使用者已正確地輸入整數，例如，您可能需要確定此值落在特定範圍內。

![使用 CSS 樣式類別的驗證錯誤](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> **重要**驗證使用者輸入也是很重要的安全性。 當您限制使用者可以在表單中輸入的值時，您就會減少某人可以輸入值，可能會危害您的站台安全性的機會。


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a>驗證使用者輸入

在 ASP.NET Web Pages 2 中，您可以使用`Validator`測試使用者輸入的協助程式。 基本的方法是執行下列動作：

1. 判斷哪個輸入您想要驗證項目 （欄位）。

    您通常會驗證中的值`<input>`表單中的項目。 不過，最好以驗證所有輸入，即使輸入來自受條件約束的項目，例如`<select>`清單。 這有助於確保使用者不略過網頁上的控制項，並送出表單。
2. 網頁程式碼中加入個別的驗證檢查針對每個輸入項目所使用的方法`Validation`協助程式。

    若要檢查必要的欄位，請使用`Validation.RequireField(field, [error message])`（適用於個別的欄位） 或`Validation.RequireFields(field1, field2, ...))`（適用於欄位的清單）。 對於其他類型的驗證，使用`Validation.Add(field, ValidationType)`。 針對`ValidationType`，您可以使用這些選項：

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
3. 當提交頁面時，檢查是否已通過驗證檢查`Validation.IsValid`:

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    如果有任何驗證錯誤，則會略過正常頁面處理。 例如，如果頁的目的是要更新資料庫，您不這麼做之前已修正的所有驗證錯誤。
4. 如果有驗證錯誤，在網頁標記中顯示的錯誤訊息使用`Html.ValidationSummary`或`Html.ValidationMessage`，或兩者。

下列範例說明這些步驟的頁面。

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

若要查看驗證的運作方式，執行此頁面並刻意犯錯。 例如，以下是頁面看起來像如果您忘了輸入課程名稱，如果您輸入，而且如果您輸入無效的日期：

![呈現頁面中的驗證錯誤](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a>新增用戶端驗證

根據預設，在使用者送出頁面之後，會驗證使用者輸入，也就是在伺服器程式碼會執行驗證。 這種方法的缺點是，使用者不知道他們已經錯誤直到後的送出該頁面。 如果表單是長或太複雜，報告錯誤，只有在送出頁面之後，才可以是使用者不方便。

您可以新增用戶端指令碼中執行驗證的支援。 在此情況下，當使用者在瀏覽器中，會執行驗證。 例如，假設您指定的值應為整數。 如果使用者輸入非整數值，當使用者離開項目欄位，就是會報告的錯誤。 使用者會取得立即意見反應，也就是方便的時候。 用戶端為基礎的驗證也可以減少使用者必須提交表單，以修正多個錯誤的次數。

> [!NOTE]
> 即使您使用用戶端驗證時，在伺服器程式碼時，會永遠也執行驗證。 執行伺服器程式碼中的驗證是安全性措施，以防使用者略過用戶端為基礎的驗證。


1. 註冊頁面中的下列 JavaScript 程式庫：  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   兩個程式庫是內容傳遞網路 (CDN)，從載入的因此您不需要對您的電腦或伺服器。 不過，您必須擁有的本機副本*jquery.validate.unobtrusive.js*。 如果您未已經使用 WebMatrix 範本 (例如**入門網站**)，包含程式庫中，建立網頁站台為基礎**入門網站**。 然後複製 *.js*您目前的站台的檔案。
2. 在標記中，針對您要驗證時，每個項目中加入呼叫`Validation.For(field)`。 這個方法會發出用戶端驗證所使用的屬性。 (而不是發出實際的 JavaScript 程式碼，方法會發出屬性，例如`data-val-...`。 這些屬性，支援不顯眼的用戶端驗證，會使用 jQuery 來執行工作。）

下列頁面會顯示如何將用戶端驗證功能新增至稍早所示的範例。

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

並非所有的用戶端上執行的驗證檢查。 特別的是，不會在用戶端上執行 （整數、 日期等等） 的資料類型驗證。 在用戶端和伺服器運作的下列檢查：

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

在此範例中，有效的日期的測試不適用於用戶端程式碼。 不過，在伺服器程式碼將會執行測試。

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a>格式化驗證錯誤

您可以控制如何驗證錯誤會顯示藉由定義 CSS 類別具有下列的保留的名稱：

- `field-validation-error`. 定義的輸出`Html.ValidationMessage`方法時顯示的錯誤。
- `field-validation-valid`. 定義的輸出`Html.ValidationMessage`方法時沒有發生錯誤。
- `input-validation-error`. 定義如何`<input>`項目會轉譯為錯誤時。 (例如，您可以使用這個類別，若要設定的背景色彩&lt;輸入&gt;不同的色彩，其值為無效的項目。)只有在用戶端驗證 （在 ASP.NET Web Pages 2) 會使用這個 CSS 類別。
- `input-validation-valid`. 定義的外觀`<input>`項目時沒有發生錯誤。
- `validation-summary-errors`. 定義的輸出`Html.ValidationSummary`方法，它會顯示錯誤清單。
- `validation-summary-valid`. 定義的輸出`Html.ValidationSummary`方法時沒有發生錯誤。

下列`<style>`區塊顯示的錯誤狀況的規則。

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

如果您稍早在本文中的 從範例頁面中包含這個樣式區塊，錯誤顯示看起來像下圖中：

![使用 CSS 樣式類別的驗證錯誤](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> 如果您不使用 ASP.NET Web Pages 2 的用戶端驗證，CSS 類別`<input>`項目 (`input-validation-error`和`input-validation-valid`不具有任何作用。


### <a name="static-and-dynamic-error-display"></a>靜態和動態錯誤顯示

CSS 規則會成對出現，例如`validation-summary-errors`和`validation-summary-valid`。 這些配對可讓您定義這兩個條件的規則： 錯誤狀況和 「 標準 」 （非錯誤） 條件。 請務必了解，一律會呈現錯誤顯示的標記，即使沒有任何錯誤。 例如，如果頁面有`Html.ValidationSummary`在標記中的方法，第一次要求頁面時，即使網頁原始檔會包含下列標記：

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

亦即`Html.ValidationSummary`方法一律會呈現`<div>`項目和清單，即使錯誤清單是空的。 同樣地，`Html.ValidationMessage`方法一律會呈現`<span>`項目做為個別欄位錯誤，即使沒有任何錯誤的預留位置。

在某些情況下，顯示錯誤訊息可能會導致要重新整理頁面，並導致項目中移動頁面上。 結尾的 CSS 規則`-valid`可讓您定義的版面配置，有助於避免這個問題。 例如，您可以定義`field-validation-error`和`field-validation-valid`兩者有相同固定大小。 如此一來，該欄位的顯示區域是靜態，不會變更頁面流程，如果會顯示錯誤訊息。

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a>不是直接來自使用者的驗證資料

有時候，您必須驗證不是直接來自 HTML 表單的資訊。 典型的範例是值傳入查詢字串，如下列範例所示的其中一個頁面：

`http://server/myapp/EditClassInformation?classid=1022`

您想要在此情況下，請確定傳遞至頁面的值 (這裡的值為 1022年`classid`) 有效。 您無法直接使用`Validation`協助程式來執行這項驗證。 不過，您可以使用驗證系統，例如能夠顯示驗證錯誤訊息的其他功能。

> [!NOTE] 
> 
> **重要**一律驗證值取自*任何*來源，包括表單欄位值、 查詢字串值，以及 cookie 值。 很容易讓人 （可能用於惡意用途） 變更這些值。 因此您必須檢查這些值，以保護您的應用程式。


下列範例會示範如何傳遞查詢字串中的值可能會通過驗證。 程式碼會測試值不是空的它是一個整數。

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

請注意，在要求不是送出表單時，執行測試 (`if(!IsPost)`)。 這項測試會通過要求頁面時，第一次，但不是要求已送出表單時。

若要顯示這個錯誤，您可以加入錯誤的驗證錯誤清單，藉由呼叫`Validation.AddFormError("message")`。 如果頁面中包含呼叫`Html.ValidationSummary`方法，該錯誤會顯示，如同使用者輸入驗證錯誤。

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>其他資源

[使用 ASP.NET Web Pages 網站中的 HTML 表單](https://go.microsoft.com/fwlink/?LinkID=202892)
