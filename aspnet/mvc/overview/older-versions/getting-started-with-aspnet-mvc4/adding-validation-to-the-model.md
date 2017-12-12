---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: "將驗證加入至模型 |Microsoft 文件"
author: Rick-Anderson
description: "注意： 本教學課程中的更新的版本這裡會提供使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 容易遵循，以及示範..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 73332d168e2f22621cb234a6591f3ce0eeed802f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="adding-validation-to-the-model"></a>將驗證加入至模型
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教學課程的更新的版本時使用[這裡](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它更安全、 容易遵循，及示範更多的功能。


在這個本節您會將驗證邏輯加入`Movie`模型，而您要確保驗證規則會強制執行任何使用者嘗試建立或編輯電影使用應用程式的時間。

## <a name="keeping-things-dry"></a>保留項目乾

其中一個核心設計原則，ASP.NET mvc 是乾 (&quot;不重複自行&quot;)。 ASP.NET MVC 鼓勵您一次，指定的功能或行為，然後讓它 everywhere 反映應用程式中。 這可減少您需要撰寫程式碼數量，並讓您撰寫較少的錯誤很容易出錯且更容易維護的程式碼。

ASP.NET MVC 和 Entity Framework Code First 所提供的驗證支援是在動作中的乾主體的絕佳範例。 您可以以宣告方式指定在單一位置 （以模型類別） 的驗證規則，則會強制執行規則 everywhere 應用程式中。

讓我們看看如何您可以利用這項驗證支援的電影應用程式中。

## <a name="adding-validation-rules-to-the-movie-model"></a>將驗證規則加入至電影模型

藉由新增一些驗證邏輯，以開始，您將`Movie`類別。

開啟 *Movie.cs* 檔案。 新增`using`在參考的檔案最上方的陳述式[ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx)命名空間：

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

請注意命名空間不包含`System.Web`。 DataAnnotations 提供一組內建的驗證屬性，您可以以宣告方式套用至任何類別或屬性。

立即更新`Movie`類別，以充分利用內建[ `Required` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx)， [ `StringLength` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)，和[ `Range` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.rangeattribute.aspx)驗證屬性. 使用下列程式碼做為要套用屬性的範例。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

執行應用程式，您仍會收到下列執行的階段錯誤：

***備份 'MovieDBContext' 內容的模型已變更，因為所建立的資料庫。請考慮使用 Code First 移轉來更新資料庫 ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269))。***

我們將使用 移轉更新結構描述。 建置方案，然後再開啟**Package Manager Console**視窗並輸入下列命令：

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

此命令完成時，Visual Studio 會開啟定義新的類別檔案`DbMIgration`衍生的類別具有指定名稱 (*AddDataAnnotationsMig*)，然後在`Up`方法，您可以看到更新的程式碼結構描述條件約束。 `Title`和`Genre`欄位不再是可為 null （也就是說，您必須輸入的值） 和`Rating`欄位的最大長度為 5。

驗證屬性會指定您想要強制執行模型屬性套用的行為。 `Required`屬性會指出屬性必須有一個值; 在此範例中，影片必須有值`Title`， `ReleaseDate`， `Genre`，和`Price`屬性才會生效。 `Range` 屬性會將值限制在指定的範圍內。 `StringLength` 屬性可讓您設定字串屬性的最大長度，並選擇性設定其最小長度。 內建類型 (例如`decimal, int, float, DateTime`) 所需的預設值，而且無須`Required`屬性。

程式碼第一次可確保您的模型類別指定的驗證規則會強制執行之前會先應用程式資料庫中儲存的變更。 例如，下列程式碼將會擲回例外狀況時`SaveChanges`呼叫方法，因為數個需要`Movie`屬性值為遺失，價格為零 （亦即超出有效範圍）。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

需要.NET framework 自動強制執行的驗證規則可協助讓您的應用程式更穩固。 它也確保您不會忘記要驗證某些項目，不小心讓不正確的資料進入資料庫。

以下是完整的程式碼已更新*Movie.cs*檔案：

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>驗證錯誤 ASP.NET MVC 中的 UI

重新執行應用程式，並瀏覽至*/Movies* URL。

按一下**新建**連結加入新的電影。 填寫表單具有一些無效的值，然後按一下 [**建立**] 按鈕。

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> 若要支援 jQuery 驗證非英文的地區設定，請使用逗號 (&quot;，&quot;) 的小數點，您必須加入*globalize.js*和您的特定*cultures/globalize.cultures.js*檔案 (從[https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) 和使用 JavaScript `Globalize.parseFloat`。 下列程式碼將示範使用 Views\Movies\Edit.cshtml 檔案所做的修改&quot;FR-FR&quot;文化特性：


[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

請注意如何表單已自動使用紅色框線色彩來反白顯示包含無效的資料，以及發出適當的驗證錯誤訊息，每個旁邊的文字方塊。 用戶端 (使用 JavaScript 和 jQuery) 與伺服器端 (若使用者已停用 JavaScript 時) 都會強制執行這些錯誤。

實際的好處是： 您不需要變更單一行中的程式碼`MoviesController`類別或*Create.cshtml*才能啟用這項驗證 UI 的檢視。 您稍早在本教學課程中建立的控制器和檢視會自動拾取您指定的驗證規則 (在 `Movie` 模型類別的屬性 (property) 上使用驗證屬性 (attribute))。

您可能已注意到屬性`Title`和`Genre`，必要的屬性不會強制執行直到您送出表單 (叫用**建立**按鈕)，或輸入欄位中輸入文字，並移除它。 欄位的最初是空的 （例如上建立檢視的欄位），且具有必要的屬性與其他任何驗證屬性，您可以執行下列命令來驗證觸發程序：

1. 欄位 索引標籤。
2. 輸入一些文字。
3. 索引標籤時。
4. 回到欄位 索引標籤。
5. 移除文字。
6. 索引標籤時。

上述順序將會觸發必要的驗證，而不叫用 [提交] 按鈕。 只要不需要輸入的任何欄位按下 [提交] 按鈕，將會觸發用戶端驗證。 直到沒有任何用戶端驗證錯誤後，表單資料才會傳送至伺服器。 您可以藉由將中斷點放在 HTTP Post 方法，或使用測試[fiddler 工具](http://fiddler2.com/fiddler2/)或 IE 9 [F12 開發人員工具](https://msdn.microsoft.com/en-us/ie/aa740478)。

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>如何驗證會在中建立檢視，並建立動作方法

您可能奇怪如何在不更新控制器或檢視程式碼的狀況下產生驗證 UI。 下一步的清單顯示什麼`Create`方法`MovieController`類別看起來像。 變更就會維持與您建立的方式加以稍早在本教學課程。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

第一個 (HTTP GET)`Create` 動作方法會顯示初始建立表單。 第二個 (`[HttpPost]`) 版本處理表單張貼。 第二個 `Create` 方法 (`HttpPost` 版本) 會呼叫`ModelState.IsValid` 檢查電影是否有任何驗證錯誤。 呼叫此方法會評估已套用至物件的所有驗證屬性。 如果物件有驗證錯誤，則 `Create` 方法會重新顯示表單。 如果沒有任何錯誤，方法即會將新的電影儲存到資料庫。 在影片範例中我們使用**時有驗證錯誤的用戶端; 上偵測到不表單張貼至伺服器，第二個** `Create`**方法絕不會呼叫**。 如果您在瀏覽器中啟用 JavaScript，停用用戶端驗證和 HTTP POST`Create`方法呼叫`ModelState.IsValid`，檢查電影是否有任何驗證錯誤。

您可以在 `HttpPost Create` 方法中設定中斷點，並確認永遠不會呼叫該方法，用戶端驗證就不會在偵測到驗證錯誤時，提交表單資料。 如果您停用瀏覽器的 JavaScript，然後提交有錯誤的表單，就會叫用中斷點。 您仍可使用沒有 JavaScript 的完整驗證。 下圖顯示如何停用 Internet Explorer 中的 JavaScript。

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

下圖顯示如何停用 FireFox 瀏覽器的 JavaScript。

![](adding-validation-to-the-model/_static/image5.png)

下圖顯示如何使用 Chrome 瀏覽器中停用 JavaScript。

![](adding-validation-to-the-model/_static/image6.png)

以下是*Create.cshtml*您稍早在本教學課程中建立結構的檢視範本。 上述動作方法會使用它來顯示初始表單，以及在發生錯誤時重新顯示它。

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

請注意程式碼如何使用`Html.EditorFor`輸出的協助專家`<input>`每個項目`Movie`屬性。 此協助程式旁是呼叫`Html.ValidationMessageFor`helper 方法。 這兩個 helper 方法會使用控制器的傳遞至檢視的模型物件 (在此情況下，`Movie`物件)。 它們會自動尋找適當的模型並顯示錯誤訊息中指定的驗證屬性。

這種方法的優點是，控制器和建立檢視表範本都不知道的任何項目有關強制執行的實際驗證規則或顯示的特定錯誤訊息。 驗證規則和錯誤字串只在 `Movie` 類別中指定。 這些相同的驗證規則會自動套用編輯檢視和任何其他檢視範本可能會建立編輯您的模型。

如果您想要稍後變更驗證邏輯，您可以在一個位置藉由加入至模型的驗證屬性 (在此範例中，`movie`類別)。 您不必擔心應用程式的不同部分會與規則強制執行的方式不一致，所有的驗證邏輯都是在同一個地方定義，用於所有位置。 這會讓程式碼非常整齊乾淨，容易維護及發展。 這表示，您可以完全接受乾原則。

## <a name="adding-formatting-to-the-movie-model"></a>加入影片模型格式設定

開啟 *Movie.cs* 檔案並檢查 `Movie` 類別。 [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx)命名空間提供除了一組內建的驗證屬性的格式屬性。 我們已經套用[ `DataType` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx)發行日期和價格欄位的列舉值。 下列程式碼會示範`ReleaseDate`和`Price`具有適當的屬性[ `DisplayFormat` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx)屬性。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

[ `DataType` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx)屬性不是驗證屬性，它們用來告訴檢視引擎呈現 HTML。 在上述範例`DataType.Date`屬性顯示為日期，影片日期沒有時間。 例如，下列[ `DataType` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx)屬性不會驗證資料的格式：

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

以上所列的屬性只會提供檢視引擎將資料格式化的提示 (例如提供屬性和&lt;&gt; url 的和&lt;href =&quot;mailto:EmailAddress.com&quot; &gt;電子郵件。 您可以使用[RegularExpression](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)来驗證的資料格式屬性。

另一個方法，使用`DataType`屬性，您也可以明確設定[ `DataFormatString` ](https://msdn.microsoft.com/en-us/library/system.string.format.aspx)值。 下列程式碼將示範使用日期的格式字串的發行日期屬性 (也就是&quot;d&quot;)。 您會使用這個指定不希望時間做為發行日期的一部分。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

完整`Movie`類別如下所示。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

執行應用程式，並瀏覽至`Movies`控制站。 發行日期和價格會妥善格式化。 下圖顯示 發行日期和價格使用&quot;FR-FR&quot;與文化特性。

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

下圖顯示相同的資料顯示的預設文化特性 （美式英文）。

![](adding-validation-to-the-model/_static/image8.png)

在數列的下一個部分中，我們會檢閱應用程式，並對自動產生的 `Details` 和 `Delete` 方法進行一些改良。

>[!div class="step-by-step"]
[上一頁](adding-a-new-field-to-the-movie-model-and-table.md)
[下一頁](examining-the-details-and-delete-methods.md)
