---
title: 將驗證新增至 ASP.NET Core MVC 應用程式
author: rick-anderson
description: 如何將驗證新增至 ASP.NET Core 應用程式。
ms.author: riande
ms.date: 04/13/2017
uid: tutorials/first-mvc-app/validation
ms.openlocfilehash: 431715e7c584d3ee381cbafb42171a7c01dddb3a
ms.sourcegitcommit: 4e87712029de2aceb1cf2c52e9e3dda8195a5b8e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2018
ms.locfileid: "53382052"
---
# <a name="add-validation-to-an-aspnet-core-mvc-app"></a>將驗證新增至 ASP.NET Core MVC 應用程式

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本節內容：

* 將驗證邏輯新增至 `Movie` 模型。
* 確保只要使用者建立或編輯電影，就會強制執行驗證規則。

## <a name="keeping-things-dry"></a>項目保持 DRY

MVC 的設計原則之一是[DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) (「不自行重複」)。 ASP.NET Core MVC 鼓勵您只指定一次功能或行為，然後讓它反映到應用程式的所有位置。 這會減少您需要撰寫的程式碼數量，並讓您撰寫的程式碼錯誤較不容易出錯、更容易測試，以及更容易維護。

MVC 和 Entity Framework Core Code First 所提供的驗證支援就是執行 DRY 準則的絶佳範例。 您可以宣告方式在單一位置指定驗證規則 (在模型類別中) ，而規則可在應用程式的任何位置強制執行。

## <a name="adding-validation-rules-to-the-movie-model"></a>將驗證規則新增至電影模型

開啟 *Movie.cs* 檔案。 DataAnnotations 提供一組內建的驗證屬性 (attribute)，您可以宣告方式將其套用至類別或屬性 (property)。 (它也包含如 `DataType` 的格式化屬性，可協助進行格式化，但不提供驗證。)

更新 `Movie` 類別，以充分利用內建的 `Required`、`StringLength`、`RegularExpression` 和 `Range` 驗證屬性。

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie22/Models/MovieDateRatingDA.cs?name=snippet1)]

驗證屬性會指定您想要對套用目標模型屬性強制執行的行為：

* `Required` 和 `MinimumLength` 屬性 (attribute) 指出屬性 (property) 必須是值；但無法防止使用者輸入空格以滿足此驗證。 
* `RegularExpression` 屬性則用來限制可輸入的字元。 上述程式碼中的 `Genre` 和 `Rating` 必須只使用字母 (第一個字母大寫，不允許使用空格、數字和特殊字元)。
* `Range` 屬性會將值限制在指定的範圍內。 
* `StringLength` 屬性可讓您設定字串屬性的最大長度，並選擇性設定其最小長度。 
* 實值型別 (如`decimal`、`int`、`float`、`DateTime`) 原本就是必要項目，而且不需要 `[Required]` 屬性。

擁有 ASP.NET Core 自動強制執行的驗證規則有助於讓您的應用程式更穩固。 它也確保您不會忘記要驗證某些項目，不小心讓不正確的資料進入資料庫。

## <a name="validation-error-ui-in-mvc"></a>MVC 中的驗證錯誤 UI

執行應用程式並巡覽至電影控制器。

點選 [新建] 連結以新增新電影。 使用無效值填寫表單。 jQuery 用戶端驗證一偵測到錯誤，就會顯示錯誤訊息。

![有多個 jQuery 用戶端驗證錯誤的電影檢視表單](~/tutorials/first-mvc-app/validation/_static/val.png)

[!INCLUDE[](~/includes/currency.md)]

請注意表單如何在包含無效值的每個欄位中自動呈現適當的驗證錯誤訊息。 用戶端 (使用 JavaScript 和 jQuery) 與伺服器端 (若使用者已停用 JavaScript 時) 都會強制執行這些錯誤。

最明顯的好處是，您不需要為了啟用這項驗證 UI 而變更 `MoviesController` 類別或 *Create.cshtml* 檢視的程式碼，一行都不用。 您稍早在本教學課程中建立的控制器和檢視會自動拾取您指定的驗證規則 (在 `Movie` 模型類別的屬性 (property) 上使用驗證屬性 (attribute))。 使用 `Edit` 動作方法測試驗證，即會套用相同的驗證。

要一直到沒有任何用戶端驗證錯誤之後，才會將表單資料傳送到伺服器。 藉由使用 [Fiddler 工具](http://www.telerik.com/fiddler)，或 [F12 開發人員工具](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/)，您可以將中斷點放入 `HTTP Post` 方法來驗證。

## <a name="how-validation-works"></a>驗證的運作方式

您可能奇怪如何在不更新控制器或檢視程式碼的狀況下產生驗證 UI。 下列程式碼示範兩種 `Create` 方法。

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Controllers/MoviesController.cs?name=snippetCreate)]

第一個 (HTTP GET)`Create` 動作方法會顯示初始建立表單。 第二個 (`[HttpPost]`) 版本處理表單張貼。 第二個 `Create` 方法 (`[HttpPost]` 版本) 會呼叫`ModelState.IsValid` 檢查電影是否有任何驗證錯誤。 呼叫此方法會評估已套用至物件的所有驗證屬性。 如果物件有驗證錯誤，則 `Create` 方法會重新顯示表單。 如果沒有任何錯誤，方法即會將新的電影儲存到資料庫。 在影片範例中，在用戶端上偵測到驗證錯誤時，表單不會發佈至伺服器；出現用戶端驗證錯誤時，一定不會呼叫第二個 `Create` 方法。 如果您停用瀏覽器的 JavaScript，用戶端驗證也會停用，而且您可以測試 HTTP POST `Create` 方法 `ModelState.IsValid` 偵測任何驗證錯誤。

您可以在 `[HttpPost] Create` 方法中設定中斷點，並確認永遠不會呼叫該方法，用戶端驗證就不會在偵測到驗證錯誤時，提交表單資料。 如果您停用瀏覽器的 JavaScript，然後提交有錯誤的表單，就會叫用中斷點。 您仍可使用沒有 JavaScript 的完整驗證。 

下圖顯示如何停用 FireFox 瀏覽器的 JavaScript。

![Firefox：在 [選項] 的 [內容] 索引標籤中，取消核取 [啟用 Javascript] 核取方塊。](~/tutorials/first-mvc-app/validation/_static/ff.png)

下圖顯示如何停用 Chrome 瀏覽器的 JavaScript。

![Google Chrome：在 [內容] 設定的 [Javascript] 區段中，選取 [不允許任何站台執行 JavaScript]。](~/tutorials/first-mvc-app/validation/_static/chrome.png)

停用 JavaScript 之後，張貼無效的資料並逐步執行偵錯工具。

![偵錯張貼的無效資料時，ModelState.IsValid 上的 IntelliSense 會顯示值為 false。](~/tutorials/first-mvc-app/validation/_static/ms.png)

下列標記顯示部分 *Create.cshtml* 檢視範本：

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/CreateRatingBrevity.html)]

動作方法會使用上述標記來顯示初始表單，以及在發生錯誤時重新顯示它。

[輸入標記協助程式](xref:mvc/views/working-with-forms) 會使用 [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) 屬性，並產生在用戶端上進行 jQuery 驗證所需的 HTML 屬性。 [驗證標記協助程式](xref:mvc/views/working-with-forms#the-validation-tag-helpers)會顯示驗證錯誤。 如需詳細資訊，請參閱[驗證](xref:mvc/models/validation)。

此方法最好的一點在於，控制器和 `Create` 檢視範本對要強制執行的實際驗證規則，或顯示的特定錯誤訊息，全都一無所知。 驗證規則和錯誤字串只在 `Movie` 類別中指定。 這些相同的驗證規則會自動套用到 `Edit` 檢視，以及任何其他您可能建立以編輯模型的檢視範本。

當您需要變更驗證邏輯時，您可以在模型中新增驗證屬性 (本例中為 `Movie` 類別)，只在一個位置完成作業。 您不必擔心應用程式的不同部分會與規則強制執行的方式不一致，所有的驗證邏輯都是在同一個地方定義，用於所有位置。 這會讓程式碼非常整齊乾淨，容易維護及發展。 這表示您會完全接受 DRY 原則。

## <a name="using-datatype-attributes"></a>使用 DataType 屬性

開啟 *Movie.cs* 檔案並檢查 `Movie` 類別。 除了一組內建的驗證屬性之外，`System.ComponentModel.DataAnnotations` 命名空間還提供了格式屬性。 發行日期和價格欄位已經套用 `DataType` 列舉值。 下列程式碼會示範具有適當 `DataType` 屬性 (attribute) 的 `ReleaseDate` 和 `Price` 屬性 (property)。

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

`DataType` 屬性只會提供檢視引擎格式化資料的提示 (同時會提供一些項目/屬性，例如 URL 的 `<a>` 以及用於電子郵件的 `<a href="mailto:EmailAddress.com">`)。 您可使用 `RegularExpression` 屬性来驗證資料的格式。 `DataType`屬性用於指定比資料庫內建類型更特定的資料類型，這些並非驗證屬性。 本例中，我們只想要追蹤日期，不追蹤時間。 `DataType` 列舉提供許多資料類型，例如 Date、Time、PhoneNumber、Currency、EmailAddress 等等。 `DataType` 屬性也可讓應用程式自動提供類型的特定功能。 例如，可建立 `DataType.EmailAddress` 的 `mailto:` 連結，而且可以在支援 HTML5 的瀏覽器中提供 `DataType.Date` 的日期選擇器。 `DataType` 屬性會發出 HTML 5 瀏覽器了解的 HTML 5 `data-` (讀音 data dash) 屬性。 `DataType` 屬性**不**會提供任何驗證。

`DataType.Date` 未指定顯示日期的格式。 根據預設，將依據以伺服器 `CultureInfo` 為基礎的預設格式顯示資料欄位。

`DisplayFormat` 屬性用來明確指定日期格式：

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

`ApplyFormatInEditMode` 設定可指定在文字方塊中顯示值以供編輯時，也應該套用的格式。 (您也許不想讓它出現在某些欄位中，例如貨幣值，可能會不希望在文字方塊中出現貨幣符號可進行編輯。)

您可單獨使用 `DisplayFormat` 屬性，但通常最好是使用 `DataType` 屬性。 `DataType` 屬性會傳逹資料的語意，而不是在螢幕上呈現資料的方式，並提供下列使用 DisplayFormat 無法取得的優點：

* 瀏覽器可以啟用 HTML5 功能 (例如顯示日曆控制項、適合地區設定的貨幣符號、電子郵件連結等等)。

* 根據預設，瀏覽器將根據您的地區設定，使用正確的格式呈現資料。

* `DataType` 屬性可以讓 MVC 選擇正確的欄位範本來轉譯資料 (如果單獨使用，`DisplayFormat` 會使用字串範本)。

> [!NOTE]
> jQuery 驗證無法用於 `Range` 屬性與 `DateTime`。 例如，下列程式碼一律會顯示用戶端驗證錯誤，即使當日期位在指定範圍中也一樣：
>
> `[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]`

您需要停用 jQuery 日期驗證以使用附帶 `DateTime` 的 `Range` 屬性。 編譯模型中的硬性日期通常不是良好的做法，因此不建議使用 `Range` 屬性和 `DateTime`。

下列程式碼會顯示一行上的結合屬性：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/MovieDateRatingDAmult.cs?name=snippet1)]

在本系列的下一篇中，我們將檢閱應用程式，並對自動產生的 `Details` 和 `Delete` 方法進行一些改良。

## <a name="additional-resources"></a>其他資源

* [使用表單](xref:mvc/views/working-with-forms)
* [全球化和當地語系化](xref:fundamentals/localization)
* [標記協助程式簡介](xref:mvc/views/tag-helpers/intro)
* [撰寫標記協助程式](xref:mvc/views/tag-helpers/authoring)

> [!div class="step-by-step"]
> [上一頁](new-field.md)
> [下一頁](details.md)  
