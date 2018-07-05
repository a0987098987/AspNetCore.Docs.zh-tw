---
uid: mvc/overview/getting-started/introduction/adding-validation
title: 新增驗證 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 9f35ca15-e216-4db6-9ebf-24380b0f31b4
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-validation
msc.type: authoredcontent
ms.openlocfilehash: 7348b0b51f7db516f398b218d37c9163cb186d73
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373405"
---
<a name="adding-validation"></a>新增驗證
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

在本節中，您會將驗證邏輯加入`Movie`模型，而您將確保的每當使用者嘗試建立或編輯電影，使用應用程式執行驗證規則。

## <a name="keeping-things-dry"></a>項目保持 DRY

ASP.NET mvc 的核心設計原則之一就是[DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself) (&quot;Don't Repeat Yourself&quot;)。 ASP.NET MVC 鼓勵您一次，指定功能或行為，然後讓它會反映在應用程式中的所有位置。 這可減少您需要撰寫的程式碼數量，並可讓您撰寫較少的錯誤，很容易出錯且更容易維護的程式碼。

ASP.NET MVC 和 Entity Framework Code First 所提供的驗證支援就是執行 DRY 準則的動作的絕佳範例。 您可以以宣告方式指定在單一位置 （在模型類別中） 中的驗證規則和規則是任何位置強制執行應用程式中。

讓我們看看如何您可以利用這項驗證支援的電影應用程式中。

## <a name="adding-validation-rules-to-the-movie-model"></a>將驗證規則新增至電影模型

一開始要先新增一些驗證邏輯，以`Movie`類別。

開啟 *Movie.cs* 檔案。 請注意[ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空間不包含`System.Web`。 DataAnnotations 提供一組內建的驗證屬性，您可以以宣告方式套用至任何類別或屬性。 (它也包含格式化屬性，例如[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)可協助進行格式化，但不提供任何驗證。)

現在更新`Movie`類別，以充分利用內建[ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)， [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)， [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)，以及[`Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)驗證屬性。 取代`Movie`取代為下列類別：

[!code-csharp[Main](adding-validation/samples/sample1.cs?highlight=5,13-15,18-19,22-23)]

[ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)屬性設定的最大長度的字串，它會在資料庫上設定這項限制，因此會變更資料庫結構描述。 以滑鼠右鍵按一下**電影**資料表中**伺服器總管**然後按一下**開啟資料表定義**:

![](adding-validation/_static/image1.png)

在上圖中，您可以看到所有字串欄位會設定為[NVARCHAR (MAX)](https://technet.microsoft.com/library/ms186939.aspx)。 我們將使用移轉來更新結構描述。 建置方案，然後再開啟**Package Manager Console**視窗並輸入下列命令：

[!code-console[Main](adding-validation/samples/sample2.cmd)]

此命令完成時，Visual Studio 會開啟可定義新的類別檔案`DbMIgration`衍生類別指定名稱 (`DataAnnotations`)，然後在`Up`方法，您可以看到更新的結構描述條件約束的程式碼：

[!code-csharp[Main](adding-validation/samples/sample3.cs)]

`Genre`欄位已不再可為 null （也就是您必須輸入的值）。 `Rating`欄位的最大長度為 5 和`Title`最大長度為 60。 3 上的最小長度`Title`上的範圍和`Price`未建立結構描述變更。

檢查影片結構描述：

![](adding-validation/_static/image2.png)

字串欄位會顯示新的長度限制和`Genre`不再處於勾選狀態為可為 null。

驗證屬性會指定您想要強制執行模型屬性套用的行為。 `Required` 和 `MinimumLength` 屬性 (attribute) 指出屬性 (property) 必須是值；但無法防止使用者輸入空格以滿足此驗證。 [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)屬性用來限制的字元可以是輸入。 上述程式碼中的 `Genre` 和 `Rating` 必須只使用字母 (不允許使用空白字元、數字及特殊字元)。 [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)屬性會限制要在指定範圍內的值。 `StringLength` 屬性可讓您設定字串屬性的最大長度，並選擇性設定其最小長度。 實值型別 (例如`decimal, int, float, DateTime`) 原本就有必要，而且不需要`Required`屬性。

程式碼會先確認您的模型類別指定的驗證規則會強制執行，才能應用程式會將變更儲存在資料庫中。 例如，下列程式碼將會擲回[DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception(v=vs.103).aspx)例外狀況時`SaveChanges`呼叫方法，因為數個需要`Movie`屬性值會遺失：

[!code-csharp[Main](adding-validation/samples/sample4.cs)]

上述程式碼會擲回下列例外狀況：

*一個或多個實體的驗證失敗。請參閱 'EntityValidationErrors' 屬性，如需詳細資訊。*

具有自動強制執行的.NET Framework 的驗證規則有助於讓您的應用程式更穩固。 它也確保您不會忘記要驗證某些項目，不小心讓不正確的資料進入資料庫。

## <a name="validation-error-ui-in-aspnet-mvc"></a>驗證錯誤 ASP.NET MVC 中的 UI

執行應用程式，並瀏覽至 */Movies* URL。

按一下 **新建**連結，可新增一部新電影。 使用無效值填寫表單。 jQuery 用戶端驗證一偵測到錯誤，就會顯示錯誤訊息。

![8_validationErrors](adding-validation/_static/image3.png)

> [!NOTE]
> 以 jQuery 驗證支援非英文的地區設定，請使用逗號 （"，"） 的小數點中,，您必須加入 NuGet 全球化，如先前所述，在本教學課程。


請注意如何表單已自動使用紅色外框色彩來反白顯示的文字方塊，其中包含無效的資料，並發出每一個旁適當的驗證錯誤訊息。 用戶端 (使用 JavaScript 和 jQuery) 與伺服器端 (若使用者已停用 JavaScript 時) 都會強制執行這些錯誤。

實質的好處是，您不需要變更任何程式中的程式碼`MoviesController`類別或*Create.cshtml*才能啟用這項驗證 UI 的檢視。 您稍早在本教學課程中建立的控制器和檢視會自動拾取您指定的驗證規則 (在 `Movie` 模型類別的屬性 (property) 上使用驗證屬性 (attribute))。 使用 `Edit` 動作方法測試驗證，即會套用相同的驗證。

直到沒有任何用戶端驗證錯誤後，表單資料才會傳送至伺服器。 您可以使用時，將中斷點放在 HTTP Post 方法，來確認這[fiddler 工具](http://fiddler2.com/fiddler2/)，或 IE [F12 開發人員工具](https://msdn.microsoft.com/ie/aa740478)。

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>如何驗證發生在 建立檢視，以及建立動作方法

您可能奇怪如何在不更新控制器或檢視程式碼的狀況下產生驗證 UI。 下一步 的清單顯示什麼`Create`中的方法`MovieController`類別看起來像。 它們與您建立的方式加以稍早在本教學課程中相同。

[!code-csharp[Main](adding-validation/samples/sample5.cs)]

第一個 (HTTP GET)`Create` 動作方法會顯示初始建立表單。 第二個 (`[HttpPost]`) 版本處理表單張貼。 第二個 `Create` 方法 (`HttpPost` 版本) 會呼叫`ModelState.IsValid` 檢查電影是否有任何驗證錯誤。 呼叫此方法會評估已套用至物件的所有驗證屬性。 如果物件有驗證錯誤，則 `Create` 方法會重新顯示表單。 如果沒有任何錯誤，方法即會將新的電影儲存到資料庫。 在影片範例中，**表單不張貼至伺服器的用戶端; 上偵測到驗證錯誤時，第二個** `Create`**絕不會呼叫方法**。 如果您停用 JavaScript 瀏覽器中，用戶端驗證已停用與 HTTP POST`Create`方法呼叫`ModelState.IsValid`檢查電影是否有任何驗證錯誤。

您可以在 `HttpPost Create` 方法中設定中斷點，並確認永遠不會呼叫該方法，用戶端驗證就不會在偵測到驗證錯誤時，提交表單資料。 如果您停用瀏覽器的 JavaScript，然後提交有錯誤的表單，就會叫用中斷點。 您仍可使用沒有 JavaScript 的完整驗證。 下圖顯示如何停用 Internet Explorer 中的 JavaScript。

![](adding-validation/_static/image5.png)

![](adding-validation/_static/image6.png)

下圖顯示如何停用 FireFox 瀏覽器的 JavaScript。

![](adding-validation/_static/image7.png)

下圖顯示如何停用 Chrome 瀏覽器的 JavaScript。

![](adding-validation/_static/image8.png)

以下是*Create.cshtml*您稍早在本教學課程中包含 scaffold 的檢視範本。 上述動作方法會使用它來顯示初始表單，以及在發生錯誤時重新顯示它。

[!code-cshtml[Main](adding-validation/samples/sample6.cshtml?highlight=16-17)]

請注意程式碼的使用方式`Html.EditorFor`輸出的協助專家`<input>`每個項目`Movie`屬性。 此協助程式旁會呼叫`Html.ValidationMessageFor`helper 方法。 這兩個 helper 方法會使用控制器傳遞至檢視的模型物件 (在此情況下，`Movie`物件)。 它們會自動尋找適當的模型並顯示錯誤訊息中指定的驗證屬性。

此方法最好的一點在於，控制器和 `Create` 檢視範本對要強制執行的實際驗證規則，或顯示的特定錯誤訊息，全都一無所知。 驗證規則和錯誤字串只在 `Movie` 類別中指定。 這些相同的驗證規則會自動套用到 `Edit` 檢視，以及任何其他您可能建立以編輯模型的檢視範本。

如果您想要稍後變更驗證邏輯，則可以只在一個位置新增至模型的驗證屬性 (在此範例中，`movie`類別)。 您不必擔心應用程式的不同部分會與規則強制執行的方式不一致，所有的驗證邏輯都是在同一個地方定義，用於所有位置。 這會讓程式碼非常整齊乾淨，容易維護及發展。 這表示，您會完全接受*DRY*原則。

## <a name="using-datatype-attributes"></a>使用 DataType 屬性

開啟 *Movie.cs* 檔案並檢查 `Movie` 類別。 [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空間會提供除了一組內建的驗證屬性的格式化屬性。 我們已套用[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)發行日期和價格欄位的列舉值。 下列程式碼示範`ReleaseDate`並`Price`屬性以適當[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)屬性。

[!code-csharp[Main](adding-validation/samples/sample7.cs)]

[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性只提供檢視引擎格式化資料的提示 (並提供屬性，例如`<a>`用於 URL 和`<a href="mailto:EmailAddress.com">`電子郵件。 您可以使用[RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)来驗證的資料格式屬性。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性用來指定比資料庫內建類型更特定的資料類型，它們***不***驗證屬性。 在此案例中，我們只想要追蹤日期，而非日期和時間。 [DataType 列舉](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)提供許多資料類型，例如*日期、 時間、 PhoneNumber、 Currency、 EmailAddress*和更多功能。 `DataType` 屬性也可讓應用程式自動提供類型的特定功能。 比方說，`mailto:`可以為建立連結[DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)，並可以針對提供的日期選擇器[DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)支援的瀏覽器[HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性會發出 HTML 5[資料集](http://ejohn.org/blog/html-5-data-attributes/)(phishing 英文發音如*data dash*) HTML 5 瀏覽器可以了解的屬性。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性不提供任何驗證。

`DataType.Date` 未指定顯示日期的格式。 根據預設，server 為基礎的預設格式顯示資料欄位[CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)。

`DisplayFormat` 屬性用來明確指定日期格式：


[!code-csharp[Main](adding-validation/samples/sample8.cs)]


`ApplyFormatInEditMode`設定可讓您指定，而指定的格式應該也會套用時的值會顯示在文字方塊中進行編輯。 (您可能不想要的某些欄位 — 比方說，例如貨幣值，您可能不想在文字方塊中的貨幣符號進行編輯。)

您可以使用[displayformat 無法](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)屬性，但它通常是個不錯的主意，若要使用[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)也屬性。 `DataType`屬性會傳達*語意*的資料做為相對於如何呈現在螢幕上，並提供下列優點，就無法取得與`DisplayFormat`:

- 瀏覽器可以啟用 HTML5 功能 （例如顯示日曆控制項、 適合地區設定的貨幣符號、 電子郵件連結、 等等。）。
- 根據預設，瀏覽器會呈現資料使用正確的格式，根據您[地區設定](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx)。
- [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性可以讓 MVC 選擇正確的欄位範本呈現的資料 ( [displayformat 無法](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)如果由本身會使用字串範本)。 如需詳細資訊，請參閱 Brad Wilson [ASP.NET MVC 2 範本](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)。 （針對 MVC 2 所撰寫，但這份文件仍適用於目前版本的 ASP.NET MVC。）

如果您使用`DataType`屬性與 [日期] 欄位中，您必須指定`DisplayFormat`也為了確保欄位在 Chrome 瀏覽器中正確轉譯的屬性。 如需詳細資訊，請參閱 <<c0> [ 這個 StackOverflow 執行緒](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie)。

> [!NOTE]
> jQuery 驗證不適用於[範圍](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)屬性並[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)。 例如，下列程式碼一律會顯示用戶端驗證錯誤，即使當日期位在指定範圍中也一樣：
> 
> [!code-csharp[Main](adding-validation/samples/sample9.cs)]
> 
> 您必須停用 jQuery 日期驗證以使用[範圍](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)屬性搭配[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)。 它通常不是最好的作法是編譯您的模型，因此使用中的硬性日期[範圍](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)屬性並[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)建議您不要使用。


下列程式碼會顯示一行上的結合屬性：

[!code-csharp[Main](adding-validation/samples/sample10.cs?highlight=6,10)]

在數列的下一個部分中，我們會檢閱應用程式，並對自動產生的 `Details` 和 `Delete` 方法進行一些改良。

> [!div class="step-by-step"]
> [上一頁](adding-a-new-field.md)
> [下一頁](examining-the-details-and-delete-methods.md)
