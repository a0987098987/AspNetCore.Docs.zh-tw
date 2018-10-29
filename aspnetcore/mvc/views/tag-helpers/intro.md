---
title: ASP.NET Core 中的標籤協助程式
author: rick-anderson
description: 了解何謂標籤協助程式，以及如何在 ASP.NET Core 中使用。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 2/14/2018
uid: mvc/views/tag-helpers/intro
ms.openlocfilehash: 4b9bceb3ce0153af2d9a30c402febe09707145b7
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477302"
---
# <a name="tag-helpers-in-aspnet-core"></a>ASP.NET Core 中的標籤協助程式

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-are-tag-helpers"></a>何謂標籤協助程式？

標籤協助程式可讓伺服器端程式碼參與建立和轉譯 Razor 檔案中的 HTML 項目。 例如，內建 `ImageTagHelper` 可以將版本號碼附加至映像名稱。 只要映像變更，伺服器就會產生映像的新唯一版本，以保證用戶端可以取得最新的映像 (而不是過期的快取映像)。 有許多適用於一般工作 (例如建立表單和連結、載入資產等) 的內建標籤協助程式，還有更多位於公用 GitHub 存放庫及作為 NuGet 套件來提供。 標籤協助程式是以 C# 編寫，並根據項目名稱、屬性名稱或上層標籤來設定目標 HTML 項目。 例如，套用 `LabelTagHelper` 屬性時，內建 `LabelTagHelper` 可以將目標設為 HTML `<label>` 項目。 如果您熟悉 [HTML 協助程式](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers)，則標籤協助程式可減少 Razor 檢視中 HTML 與 C# 之間的明確轉換。 在許多情況下，HTML 協助程式都會提供特定標籤協助程式的替代方式，但請務必辨識標籤協助程式未取代 HTML 協助程式，而且每個 HTML 協助程式都沒有標籤協助程式。 [標籤協助程式與 HTML 協助程式的比較](#tag-helpers-compared-to-html-helpers)會詳述差異。

## <a name="what-tag-helpers-provide"></a>標籤協助程式所提供的內容

**HTML 友善開發經驗**：在大多數情況下，使用標籤 (tag) 協助程式的 Razor 標記 (markup) 看起來就像標準 HTML。 熟悉 HTML/CSS/JavaScript 的前端設計工具不需要學習 C# Razor 語法，就可以編輯 Razor。

**建立 HTML 和 Razor 標記的豐富 IntelliSense 環境**：這與 HTML 協助程式明確對比，而 HTML 協助程式是伺服器端在 Razor 檢視中建立標記的舊方法。 [標籤協助程式與 HTML 協助程式的比較](#tag-helpers-compared-to-html-helpers)會詳述差異。 [標籤協助程式的 IntelliSense 支援](#intellisense-support-for-tag-helpers)說明 IntelliSense 環境。 有 Razor C# 語法使用經驗的開發人員，使用標籤 (tag) 協助程式會比使用撰寫 C# Razor 標記 (markup) 更具生產力。

**讓您更具生產力，而且可以只使用伺服器上的可用資訊來產生更強固、可靠和易維護的程式碼**：例如，在過去，更新映像的目的是要在您變更映像時變更映像名稱。 基於效能考量，應該主動快取影像，而且除非您變更影像的名稱，否則用戶端會有取得過時複本的風險。 在過去，編輯映像之後，必須變更名稱，而且需要更新 Web 應用程式中映像的每個參考。 這不只十分耗人力，也很容易發生錯誤 (您可能遺漏參考、不小心地輸入錯誤字串，依此類推)內建 `ImageTagHelper` 可以自動執行這項作業。 `ImageTagHelper` 可以將版本號碼附加到映像名稱後面；因此，只要映像變更，伺服器就會自動產生映像的新唯一版本。 用戶端保證會取得目前的映像。 使用 `ImageTagHelper`，此健全性和人力節省基本上是免費的。

大部分的內建的標籤協助程式都可以在標準的 HTML 元素中使用，可為元素提供伺服器端的屬性。 例如，在 [檢視/帳戶] 資料夾內許多檢視中所使用的 `<input>` 元素會包含 `asp-for` 屬性。 此屬性會擷取指定之模型屬性的名稱，並將其放入轉譯的 HTML 中。 請考慮下列模型的 Razor 檢視：

```csharp
public class Movie
{
    public int ID { get; set; }
    public string Title { get; set; }
    public DateTime ReleaseDate { get; set; }
    public string Genre { get; set; }
    public decimal Price { get; set; }
}
```

下列 Razor 標記：

```cshtml
<label asp-for="Movie.Title"></label>
```

產生下列 HTML：

```html
<label for="Movie_Title">Title</label>
```

`asp-for` 屬性由 [LabelTagHelper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.labeltaghelper?view=aspnetcore-2.0) 中的 `For` 提供。 如需詳細資訊，請參閱[編寫標籤協助程式](xref:mvc/views/tag-helpers/authoring)。

## <a name="managing-tag-helper-scope"></a>管理標籤協助程式範圍

標籤協助程式範圍是透過 `@addTagHelper`、`@removeTagHelper` 和 "!" 退出字元的組合所控制。

<a name="add-helper-label"></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a>`@addTagHelper` 讓標籤協助程式可用

如果您建立名為 *AuthoringTagHelpers* 的新 ASP.NET Core Web 應用程式，則會將下列 *Views/_ViewImports.cshtml* 檔案新增至您的專案：

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

`@addTagHelper` 指示詞讓標籤協助程式可供檢視使用。 在此情況下，檢視檔案是 *Pages/_ViewImports.cshtml*，預設會由 *Pages* 資料夾和子資料夾中的所有檔案繼承；讓標籤協助程式可用。 上述程式碼使用萬用字元語法 ("\*")，指定 *Views* 目錄或子目錄中每個檢視檔案皆可使用指定組件 (*Microsoft.AspNetCore.Mvc.TagHelpers*) 中的所有標籤協助程式。 `@addTagHelper` 後面的第一個參數指定要載入的標籤協助程式 (使用 "\*" 表示所有標籤協助程式)，而第二個參數 "Microsoft.AspNetCore.Mvc.TagHelpers" 指定包含標籤協助程式的組件。 *Microsoft.AspNetCore.Mvc.TagHelpers* 是內建 ASP.NET Core 標籤協助程式的組件。

若要公開此專案中的所有標籤協助程式 (這會建立名為 *AuthoringTagHelpers* 的組件)，請使用下列內容：

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

如果您的專案包含具有預設命名空間 (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) 的 `EmailTagHelper`，則您可以提供標籤協助程式的完整名稱 (FQN)：

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

若要使用 FQN 將標籤協助程式新增至檢視，請依序新增 FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) 和組件名稱 (*AuthoringTagHelpers*)。 大部分開發人員都會想要使用 "\*" 萬用字元語法。 萬用字元語法可讓您在 FQN 中插入萬用字元 "\*" 作為尾碼。 例如，下列任何指示詞都會顯示 `EmailTagHelper`：

```cshtml
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

如前所述，將 `@addTagHelper` 指示詞新增至 *Views/_ViewImports.cshtml* 檔案，讓 *Views* 目錄和子目錄中的所有檢視檔案都能使用標籤協助程式。 如果您想要選擇只向那些檢視公開標籤協助程式，則可以使用特定檢視檔案中的 `@addTagHelper` 指示詞。

<a name="remove-razor-directives-label"></a>

### <a name="removetaghelper-removes-tag-helpers"></a>`@removeTagHelper` 會移除標籤協助程式

`@removeTagHelper` 有兩個參數與 `@addTagHelper` 相同，而且會移除先前新增的標籤協助程式。 例如，套用至特定檢視的 `@removeTagHelper` 會從檢視中移除指定的標籤協助程式。 在 *Views/Folder/_ViewImports.cshtml* 檔案中使用 `@removeTagHelper`，會從 *Folder* 的所有檢視中移除所指定的標籤協助程式。

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a>使用 *_ViewImports.cshtml* 檔案控制標籤協助程式範圍

您可以將 *_ViewImports.cshtml* 新增至任何檢視資料夾，而且檢視引擎會套用該檔案和 *Views/_ViewImports.cshtml* 檔案中的指示詞。 如果您已為 *Home* 檢視新增空白 *Views/Home/_ViewImports.cshtml* 檔案，則不會進行變更；因為 *_ViewImports.cshtml* 檔案是加總的。 您新增至 *Views/Home/_ViewImports.cshtml* 檔案的任何 `@addTagHelper` 指示詞 (不在預設 *Views/_ViewImports.cshtml* 檔案中)，只會向 *Home* 資料夾中的檢視公開這些標籤協助程式。

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a>退出個別項目

您可以停用項目層級中包含標籤協助程式退出字元 ("!") 的標籤協助程式。 例如，在包含標籤協助程式退出字元的 `<span>` 中，停用 `Email` 驗證：

```cshtml
<!span asp-validation-for="Email" class="text-danger"></!span>
```

您必須將標籤協助程式退出字元套用至開頭和結尾標籤  (將退出字元新增至開頭標籤時，Visual Studio 編輯器會將退出字元自動新增至結尾標籤)。 在您新增退出字元之後，就不會再以特殊字型顯示項目和標籤協助程式屬性。

<a name="prefix-razor-directives-label"></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a>使用 `@tagHelperPrefix` 以明確使用標籤協助程式

`@tagHelperPrefix` 指示詞可讓您指定標籤前置詞字串以啟用標籤協助程式支援，並明確使用。 例如，您可以將下列標記新增至 *Views/_ViewImports.cshtml* 檔案：

```cshtml
@tagHelperPrefix th:
```
在下列程式碼影像中，標籤協助程式前置詞設定為 `th:`，因此只有使用前置詞 `th:` 的項目才支援標籤協助程式 (啟用標籤協助程式的項目具有特殊字型)。 `<label>` 和 `<input>` 項目具有標籤協助程式前置詞並且已啟用標籤協助程式，而 `<span>` 項目則否。

![影像](intro/_static/thp.png)

套用至 `@addTagHelper` 的相同階層規則也會套用至 `@tagHelperPrefix`。

## <a name="self-closing-tag-helpers"></a>自行結尾的標籤協助程式

許多標籤協助程式無法作為自行結尾的標籤。 某些標籤協助程式專門用作自行結尾的標籤。 使用非專門用作自行結尾的標籤協助程式會隱藏轉譯輸出。 讓標籤協助程式自行結尾，會在轉譯輸出中產生自行結尾的標籤。 如需詳細資訊，請參閱[編寫標籤協助程式](xref:mvc/views/tag-helpers/authoring)中的[這項附註](xref:mvc/views/tag-helpers/authoring#self-closing)。

## <a name="intellisense-support-for-tag-helpers"></a>標籤協助程式的 IntelliSense 支援

當您在 Visual Studio 中建立新的 ASP.NET Core Web 應用程式時，它會新增 NuGet 套件 "Microsoft.AspNetCore.Razor.Tools"。 這是新增標籤協助程式工具的套件。

請考慮撰寫 HTML `<label>` 項目。 只要您在 Visual Studio 編輯器中輸入 `<l`，IntelliSense 就會顯示相符的項目：

![影像](intro/_static/label.png)

您不只可以取得 HTML 協助，還可以取得圖示 (其下的 "@" symbol with "<>")。

![影像](intro/_static/tagSym.png)

識別標籤協助程式設為目標的項目。 純 HTML 項目 (例如 `fieldset`) 會顯示 "<>" 圖示。

純 HTML `<label>` 標籤會以棕色字型顯示 HTML 標籤 (具有預設 Visual Studio 色彩佈景主題)、以紅色顯示屬性，並以藍色顯示屬性值。

![影像](intro/_static/LableHtmlTag.png)

在您輸入 `<label` 之後，IntelliSense 會列出可用的 HTML/CSS 屬性以及設為目標的標籤協助程式屬性：

![影像](intro/_static/labelattr.png)

IntelliSense 陳述式完成可讓您輸入 Tab 鍵，來完成具有所選取值的陳述式：

![影像](intro/_static/stmtcomplete.png)

只要輸入標籤協助程式屬性，標籤和屬性字型就會變更。 使用預設 Visual Studio "Blue" 或 "Light" 色彩佈景主題，而且字型為粗體紫色。 如果要使用 "Dark" 佈景主題，則字型是粗體藍綠色。 本文件中的影像是使用預設佈景主題所取得。

![影像](intro/_static/labelaspfor2.png)

您可以在雙引號 ("") 內輸入 Visual Studio *CompleteWord* 快速鍵 (Ctrl+空格鍵是[預設值](/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio)，而且現在可以使用 C#，就像使用 C# 類別一樣。 IntelliSense 會顯示頁面模型上的所有方法和屬性。 因為屬性類型是 `ModelExpression`，所以可以使用方法和屬性。 在下列影像中，我正在編輯 `Register` 檢視，因此 `RegisterViewModel` 可供使用。

![影像](intro/_static/intellemail.png)

IntelliSense 會列出頁面上模型可用的屬性和方法。 豐富的 IntelliSense 環境可協助您選取 CSS 類別：

![影像](intro/_static/iclass.png)

![影像](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a>標籤協助程式與 HTML 協助程式的比較

標籤協助程式會附加至 Razor 檢視中的 HTML 項目，而 [HTML 協助程式](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) 會叫用為與 Razor 檢視中的 HTML 顛倒的方法。 請考慮下列 Razor 標記，以建立具有 CSS 類別 "caption" 的 HTML 標籤：

```cshtml
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

at (`@`) 符號會告知 Razor 這是程式碼啟動。 下兩個參數 ("FirstName" 和 "First Name:") 是字串，因此 [IntelliSense](/visualstudio/ide/using-intellisense) 沒有幫助。 最後一個引數：

```cshtml
new {@class="caption"}
```

是用來代表屬性的匿名物件。 因為 <strong>class</strong> 是 C# 中的保留關鍵字，所以您可以使用 `@` 符號來強制 C# 將 "@class=" 解譯為符號 (屬性名稱)。 針對前端設計人員 (熟悉 HTML/CSS/JavaScript 和其他用戶端技術，但不熟悉 C# 和 Razor)，這行大部分為外部。 您必須編寫整行，而 IntelliSense 沒有任何幫助。

使用 `LabelTagHelper`，可以將相同的標記撰寫為：

![影像](intro/_static/label2.png)

使用標籤協助程式版本時，只要您在 Visual Studio 編輯器中輸入 `<l`，IntelliSense 就會顯示相符的項目：

![影像](intro/_static/label.png)

IntelliSense 可協助您撰寫整行。 `LabelTagHelper` 也預設為將 `asp-for` 屬性值 ("FirstName") 的內容設定為 "First Name"；它會將依照 Camel 命名法大小寫慣例的屬性轉換成句子，而句子包含具有空格且發生每個新大寫字母的屬性名稱。 在下列標記中：

![影像](intro/_static/label2.png)

產生：

```cshtml
<label class="caption" for="FirstName">First Name</label>
```

如果您將內容新增至 `<label>`，則不會使用依照 Camel 命名法大小寫慣例的句子到句子大小寫內容。 例如: 

![影像](intro/_static/1stName.png)

產生：

```cshtml
<label class="caption" for="FirstName">Name First</label>
```

下列程式碼影像顯示透過 Visual Studio 2015 所含的舊版 ASP.NET 4.5.x MVC 範本所產生的 *Views/Account/Register.cshtml* Razor 檢視的 Form 部分。

![影像](intro/_static/regCS.png)

Visual Studio 編輯器會以灰色背景顯示 C# 程式碼。 例如，`AntiForgeryToken` HTML 協助程式：

```cshtml
@Html.AntiForgeryToken()
```

以灰色背景顯示。 註冊檢視中的大部分標記都是 C#。 將該方法與使用標籤協助程式的對等方法進行比較：

![影像](intro/_static/regTH.png)

與 HTML 協助程式方法相較之下，標記更為簡潔，而且更容易讀取、編輯和維護。 C# 程式碼會減少到伺服器所知道的最小值。 Visual Studio 編輯器會以特殊字型顯示標籤 (tag) 協助程式設為目標的標籤 (markup)。

請考慮使用 *Email* 群組：

[!code-csharp[](intro/sample/Register.cshtml?range=12-18)]

每個 "asp-" 屬性的值都是 "Email"，但 "Email" 不是字串。 在此內容中，"Email" 是 `RegisterViewModel` 的 C# 模型運算式屬性。

Visual Studio 編輯器可協助您撰寫註冊表單之標籤 (tag) 協助程式方法中的**所有**標籤 (markup)，而 Visual Studio 不提供 HTML 協助程式方法中大部分程式碼的協助。 [標籤協助程式的 IntelliSense 支援](#intellisense-support-for-tag-helpers)會詳述如何在 Visual Studio 編輯器中使用標籤協助程式。

## <a name="tag-helpers-compared-to-web-server-controls"></a>標籤協助程式與網頁伺服器控制項的比較

* 標籤協助程式未擁有與其建立關聯的項目；它們只會參與轉譯項目和內容。 ASP.NET [Web 伺服器控制項](https://msdn.microsoft.com/library/7698y1f0.aspx)是在頁面上宣告和叫用。

* [網頁伺服器控制項](https://msdn.microsoft.com/library/zsyt68f1.aspx)的非一般生命週期讓開發和偵錯更為困難。

* 網頁伺服器控制項可讓您使用用戶端控制項，來新增用戶端文件物件模型 (DOM) 項目的功能。 標籤協助程式沒有 DOM。

* Web 伺服器控制項包括自動瀏覽器偵測。 標籤協助程式不知道瀏覽器。

* 雖然您通常無法撰寫網頁伺服器控制項，但是多個標籤協助程式可以處理相同的項目 (請參閱[避免標籤協助程式衝突](xref:mvc/views/tag-helpers/authoring#avoid-tag-helper-conflicts))。

* 標籤協助程式可以修改設為其範圍之 HTML 項目的標籤和內容，但不會直接修改頁面上的其他任何項目。 網頁伺服器控制項的範圍更為廣泛，而且可以執行的動作會影響您網頁的其他部分；具有非預期的副作用。

* 網頁伺服器控制項使用類型轉換器以將字串轉換成物件。 使用標籤協助程式時，您可以使用 C# 以原生方式工作，因此不需要進行類型轉換。

* 網頁伺服器控制項使用 [System.ComponentModel](/dotnet/api/system.componentmodel)，來實作元件和控制項的執行階段和設計階段行為。 `System.ComponentModel` 包含基底類別和介面，以便實作屬性和類型轉換器、繫結至資料來源，以及授權元件。 與一般衍生自 `TagHelper` 的標籤協助程式相反，而且 `TagHelper` 基底類別只會公開兩個方法：`Process` 和 `ProcessAsync`。

## <a name="customizing-the-tag-helper-element-font"></a>自訂標籤協助程式項目字型

您可以從 [工具] > [選項] > [環境][字型和色彩] >  來自訂字型和顏色標示：

![影像](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a>其他資源

* [撰寫標記協助程式](xref:mvc/views/tag-helpers/authoring)
* [使用表單](xref:mvc/views/working-with-forms)
* [GitHub 上的 TagHelperSamples](https://github.com/dpaquette/TagHelperSamples) 包含使用[啟動程序](http://getbootstrap.com/) 的標籤協助程式範例。
