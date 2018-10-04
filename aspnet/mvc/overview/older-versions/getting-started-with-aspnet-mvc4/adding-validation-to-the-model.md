---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: 將驗證加入至模型 |Microsoft Docs
author: Rick-Anderson
description: 注意： 本教學課程中的更新的版本就可以使用這裡使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 更容易遵循，並示範...
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 5819d789f31b9452d40ae3aa7f821f101ae126ce
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578311"
---
<a name="adding-validation-to-the-model"></a>將驗證加入至模型
====================
藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > 本教學課程中的更新的版本可[此處](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它更安全、 更容易遵循，並示範更多的功能。


在本節中，您會將驗證邏輯加入`Movie`模型，而您將確保的每當使用者嘗試建立或編輯電影，使用應用程式執行驗證規則。

## <a name="keeping-things-dry"></a>項目保持 DRY

ASP.NET mvc 的核心設計原則之一，是 DRY (&quot;Don't Repeat Yourself&quot;)。 ASP.NET MVC 鼓勵您一次，指定功能或行為，然後讓它會反映在應用程式中的所有位置。 這可減少您需要撰寫的程式碼數量，並可讓您撰寫較少的錯誤，很容易出錯且更容易維護的程式碼。

ASP.NET MVC 和 Entity Framework Code First 所提供的驗證支援就是執行 DRY 準則的動作的絕佳範例。 您可以以宣告方式指定在單一位置 （在模型類別中） 中的驗證規則和規則是任何位置強制執行應用程式中。

讓我們看看如何您可以利用這項驗證支援的電影應用程式中。

## <a name="adding-validation-rules-to-the-movie-model"></a>將驗證規則新增至電影模型

一開始要先新增一些驗證邏輯，以`Movie`類別。

開啟 *Movie.cs* 檔案。 新增`using`陳述式參考的檔案頂端[ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空間：

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

請注意命名空間不包含`System.Web`。 DataAnnotations 提供一組內建的驗證屬性，您可以以宣告方式套用至任何類別或屬性。

現在更新`Movie`類別，以充分利用內建[ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)， [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)，並[ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)驗證屬性. 使用下列程式碼作為範例，要套用屬性的位置。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

執行應用程式，您仍會收到下列執行的階段錯誤：

***備份 'MovieDBContext' 內容的模型已變更，因為所建立的資料庫。請考慮使用 Code First 移轉更新資料庫 ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269))。***

我們將使用移轉來更新結構描述。 建置方案，然後再開啟**Package Manager Console**視窗並輸入下列命令：

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

此命令完成時，Visual Studio 會開啟可定義新的類別檔案`DbMIgration`衍生類別指定名稱 (*AddDataAnnotationsMig*)，然後在`Up`方法，您可以看到更新的程式碼結構描述條件約束。 `Title`並`Genre`欄位不再是可為 null （也就是您必須輸入的值） 和`Rating`欄位的最大長度為 5。

驗證屬性會指定您想要強制執行模型屬性套用的行為。 `Required`屬性會指出屬性必須有值; 在此範例中，電影必須具有值`Title`， `ReleaseDate`， `Genre`，和`Price`屬性才會生效。 `Range` 屬性會將值限制在指定的範圍內。 `StringLength` 屬性可讓您設定字串屬性的最大長度，並選擇性設定其最小長度。 內建類型 (例如`decimal, int, float, DateTime`) 預設為必要項，而且不需要`Required`屬性。

程式碼會先確認您的模型類別指定的驗證規則會強制執行，才能應用程式會將變更儲存在資料庫中。 例如，下列程式碼將會擲回例外狀況時`SaveChanges`呼叫方法，因為數個需要`Movie`屬性值遺漏，且價格為零 （這是超出有效範圍）。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

具有自動強制執行的.NET Framework 的驗證規則有助於讓您的應用程式更穩固。 它也確保您不會忘記要驗證某些項目，不小心讓不正確的資料進入資料庫。

以下是完整的程式碼，更新清單*Movie.cs*檔案：

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>驗證錯誤 ASP.NET MVC 中的 UI

重新執行應用程式，並瀏覽至 */Movies* URL。

按一下 新建**連結，可新增一部新電影。 填妥表單中使用某些無效的值，然後按一下 [**建立**] 按鈕。

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> 以 jQuery 驗證支援非英文的地區設定，請使用逗號 (&quot;，&quot;) 的小數點，您必須加入*globalize.js*和您的特定*cultures/globalize.cultures.js*檔案 (從[ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) 和使用的 JavaScript `Globalize.parseFloat`。 下列程式碼會顯示在 Views\Movies\Edit.cshtml 檔案，才能使用已修改&quot;FR-FR&quot;文化特性：


[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

請注意如何表單已自動使用紅色外框色彩來反白顯示的文字方塊，其中包含無效的資料，並發出每一個旁適當的驗證錯誤訊息。 用戶端 (使用 JavaScript 和 jQuery) 與伺服器端 (若使用者已停用 JavaScript 時) 都會強制執行這些錯誤。

實質的好處是，您不需要變更任何程式中的程式碼`MoviesController`類別或*Create.cshtml*才能啟用這項驗證 UI 的檢視。 您稍早在本教學課程中建立的控制器和檢視會自動拾取您指定的驗證規則 (在 `Movie` 模型類別的屬性 (property) 上使用驗證屬性 (attribute))。

您可能已經注意到的屬性`Title`並`Genre`，必要的屬性不會強制執行直到您提交表單 (點擊**建立**按鈕)，或輸入欄位中輸入文字，並將其移除。 欄位的最初是空的 （例如 [建立] 檢視中的欄位），且具有必要的屬性與其他任何驗證屬性，您可以執行下列命令來觸發驗證：

1. 在欄位的索引標籤。
2. 輸入一些文字。
3. 按下 Tab 鍵切換至下一個欄位。
4. 回欄位的索引標籤。
5. 移除文字。
6. 按下 Tab 鍵切換至下一個欄位。

上述的序列將會觸發必要的驗證，而不叫用 [提交] 按鈕。 只要不需要輸入任何欄位中按 [提交] 按鈕，將會觸發用戶端驗證。 直到沒有任何用戶端驗證錯誤後，表單資料才會傳送至伺服器。 您可以將中斷點放在 HTTP Post 方法，或使用測試[fiddler 工具](http://fiddler2.com/fiddler2/)或 IE 9 [F12 開發人員工具](https://msdn.microsoft.com/ie/aa740478)。

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>如何驗證發生在 建立檢視，以及建立動作方法

您可能奇怪如何在不更新控制器或檢視程式碼的狀況下產生驗證 UI。 下一步 的清單顯示什麼`Create`中的方法`MovieController`類別看起來像。 它們與您建立的方式加以稍早在本教學課程中相同。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

第一個 (HTTP GET)`Create` 動作方法會顯示初始建立表單。 第二個 (`[HttpPost]`) 版本處理表單張貼。 第二個 `Create` 方法 (`HttpPost` 版本) 會呼叫`ModelState.IsValid` 檢查電影是否有任何驗證錯誤。 呼叫此方法會評估已套用至物件的所有驗證屬性。 如果物件有驗證錯誤，則 `Create` 方法會重新顯示表單。 如果沒有任何錯誤，方法即會將新的電影儲存到資料庫。 在影片範例中我們使用，**表單不張貼至伺服器的用戶端; 上偵測到驗證錯誤時，第二個** `Create`**絕不會呼叫方法**。 如果您停用 JavaScript 瀏覽器中，用戶端驗證已停用與 HTTP POST`Create`方法呼叫`ModelState.IsValid`檢查電影是否有任何驗證錯誤。

您可以在 `HttpPost Create` 方法中設定中斷點，並確認永遠不會呼叫該方法，用戶端驗證就不會在偵測到驗證錯誤時，提交表單資料。 如果您停用瀏覽器的 JavaScript，然後提交有錯誤的表單，就會叫用中斷點。 您仍可使用沒有 JavaScript 的完整驗證。 下圖顯示如何停用 Internet Explorer 中的 JavaScript。

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

下圖顯示如何停用 FireFox 瀏覽器的 JavaScript。

![](adding-validation-to-the-model/_static/image5.png)

下圖顯示如何搭配 Chrome 瀏覽器中停用 JavaScript。

![](adding-validation-to-the-model/_static/image6.png)

以下是*Create.cshtml*您稍早在本教學課程中包含 scaffold 的檢視範本。 上述動作方法會使用它來顯示初始表單，以及在發生錯誤時重新顯示它。

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

請注意程式碼的使用方式`Html.EditorFor`輸出的協助專家`<input>`每個項目`Movie`屬性。 此協助程式旁會呼叫`Html.ValidationMessageFor`helper 方法。 這兩個 helper 方法會使用控制器傳遞至檢視的模型物件 (在此情況下，`Movie`物件)。 它們會自動尋找適當的模型並顯示錯誤訊息中指定的驗證屬性。

什麼是這種方法的優點在於控制器和 Create 檢視範本都不知道的任何項目顯示的特定錯誤訊息或強制執行的實際驗證規則相關。 驗證規則和錯誤字串只在 `Movie` 類別中指定。 這些相同的驗證規則會自動套用 [編輯] 檢視和任何其他檢視範本可能會建立以編輯模型。

如果您想要稍後變更驗證邏輯，則可以只在一個位置新增至模型的驗證屬性 (在此範例中，`movie`類別)。 您不必擔心應用程式的不同部分會與規則強制執行的方式不一致，所有的驗證邏輯都是在同一個地方定義，用於所有位置。 這會讓程式碼非常整齊乾淨，容易維護及發展。 這表示您會完全接受 DRY 原則。

## <a name="adding-formatting-to-the-movie-model"></a>加入至電影模型格式設定

開啟 *Movie.cs* 檔案並檢查 `Movie` 類別。 [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空間會提供除了一組內建的驗證屬性的格式化屬性。 我們已套用[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)發行日期和價格欄位的列舉值。 下列程式碼示範`ReleaseDate`並`Price`屬性以適當[ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)屬性。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)屬性不是驗證屬性，它們用來指示檢視引擎如何呈現 HTML。 在上面的範例，`DataType.Date`屬性會顯示影片日期為日期，而不需要的時間。 例如，下列[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)屬性不會驗證資料的格式：

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

以上所列的屬性只會提供檢視引擎格式化資料的提示 (並提供屬性，例如&lt;&gt;用於 URL 並&lt;a =&quot;mailto:EmailAddress.com&quot; &gt;電子郵件。 您可以使用[RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)来驗證的資料格式屬性。

若要使用的替代方法`DataType`屬性，您也可以明確設定[ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx)值。 下列程式碼顯示以日期格式字串的發行日期屬性 (亦即&quot;d&quot;)。 您會使用此對話方塊來指定您不想要時間發行日期的一部分。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

完整`Movie`類別如下所示。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

執行應用程式，並瀏覽至`Movies`控制站。 適當格式化的發行日期和價格。 下圖顯示 發行日期和價格使用&quot;FR-FR&quot;與文化特性。

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

下圖顯示相同的資料顯示的預設文化特性 （美式英文）。

![](adding-validation-to-the-model/_static/image8.png)

在數列的下一個部分中，我們會檢閱應用程式，並對自動產生的 `Details` 和 `Delete` 方法進行一些改良。

> [!div class="step-by-step"]
> [上一頁](adding-a-new-field-to-the-movie-model-and-table.md)
> [下一頁](examining-the-details-and-delete-methods.md)
