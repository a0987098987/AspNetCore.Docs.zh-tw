---
title: "在 ASP.NET Core 標記協助程式"
author: rick-anderson
description: "瞭解什麼標記協助程式，以及如何在 ASP.NET Core 中使用它們。"
keywords: "ASP.NET Core，標記協助程式"
ms.author: riande
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/intro
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 06644b8359fb5ccc2e61a17a4c6e20e354d5ceef
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2017
---
# <a name="introduction-to-tag-helpers-in-aspnet-core"></a>在 ASP.NET Core 標記協助程式簡介 

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-are-tag-helpers"></a>標記協助程式有哪些？

標記協助程式可讓伺服器端程式碼參與建立和呈現 HTML Razor 檔案中的項目。 例如，內建`ImageTagHelper`可以將版本號碼附加至映像名稱。 每當映像變更時，伺服器會產生新的唯一版本映像，讓用戶端保證以取得目前的映像 （而不是過期的快取映像）。 有許多內建的標記協助程式的一般工作-例如建立表單、 連結、 載入資產和更多的和更多可用公用 GitHub 儲存機制中以及 NuGet 封裝。 標記協助程式以 C# 中，撰寫，與它們目標項目名稱、 屬性名稱，或父標記的 HTML 項目。 例如，內建`LabelTagHelper`可鎖定目標 HTML`<label>`項目時`LabelTagHelper`屬性會套用。 如果您已熟悉[HTML Helper](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers)，標記協助減少 Razor 檢視的 HTML 和 C# 之間的明確轉換。 在許多情況下，HTML Helper 提供的替代方式給特定的標記協助程式，但請務必識別標記協助程式不會取代 HTML Helper，並不是標記協助程式的每個 HTML Helper。 [標記協助程式相較於 HTML Helper](#tag-helpers-compared-to-html-helpers)說明詳細的差異。

## <a name="what-tag-helpers-provide"></a>標記協助程式所提供的內容

**HTML 易用的開發經驗**在大部分的情況，Razor 標記使用標記協助程式看起來像標準 HTML。 熟悉 HTML/CSS/JavaScript 前端設計工具可以編輯 Razor，無需了解 C# Razor 語法。

**豐富的 IntelliSense 環境，以建立 HTML 和 Razor 標記**這種清晰對比，以 HTML Helper，伺服器端建立 Razor 檢視中的標記之前的方法。 [標記協助程式相較於 HTML Helper](#tag-helpers-compared-to-html-helpers)說明詳細的差異。 [標記協助程式的 IntelliSense 支援](#intellisense-support-for-tag-helpers)說明 IntelliSense 環境。 甚至 Razor C# 語法有經驗的開發人員會更有效率使用比撰寫 C# Razor 標記的標記協助程式。

**讓您更具生產力且能夠產生更強固、 可靠的方式和維護的程式碼使用伺服器上的資訊僅適用於**比方說，在過去在更新映像宗旨已變更的映像名稱，當您變更映像。 影像應該主動快取基於效能考量，除非您變更影像的名稱，就可能會取得過時複本的用戶端。 在過去，編輯映像之後，名稱已變更，並需要更新的 web 應用程式中的映像的每個參考。 不只是這非常人工大量，也很容易發生 （您可能遺漏了參考、 不小心輸入錯誤的字串，依此類推） 錯誤內建`ImageTagHelper`可以這樣做為您自動。 `ImageTagHelper`可以將版本附加數字的映像名稱，所以每當映像變更時，伺服器會自動產生新的唯一版本映像。 用戶端保證取得目前的映像。 使用此健全性和人力節省來自基本上可用`ImageTagHelper`。

大部分的內建的標記協助程式和目標現有的 HTML 元素的元素提供伺服器端的屬性。 例如，`<input>`所使用的檢視中的許多項目的*檢視/帳戶*資料夾包含`asp-for`屬性，它會擷取到轉譯的 HTML 與指定的模型屬性的名稱。 下列的 Razor 標記：

```html
<label asp-for="Email"></label>
```

會產生下列 HTML:

```html
<label for="Email">Email</label>
```

`asp-for`屬性可由`For`屬性`LabelTagHelper`。 請參閱[撰寫標記協助程式](authoring.md)如需詳細資訊。

## <a name="managing-tag-helper-scope"></a>管理標記協助程式範圍

標記協助程式範圍由多種`@addTagHelper`， `@removeTagHelper`，而"！"退出字元。

<a name=add-helper-label></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a>`@addTagHelper`提供標記協助程式

如果您建立新 ASP.NET Core web 應用程式名為*AuthoringTagHelpers* （使用無驗證），下列*Views/_ViewImports.cshtml*檔案會加入至您的專案：

[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

`@addTagHelper`指示詞使標記協助程式可供檢視。 在此案例中，檢視檔案是*Views/_ViewImports.cshtml*，預設會繼承中的所有檢視檔案*檢視*資料夾和子的目錄; 可用標記協助程式。 上述程式碼使用的萬用字元語法 (「\*") 來指定在指定的組件中的所有標記協助程式 (*Microsoft.AspNetCore.Mvc.TagHelpers*) 將可在每個檢視檔案*檢視*目錄或子目錄。 之後的第一個參數`@addTagHelper`指定載入標記協助程式 (我們使用 「\*"的所有標記協助程式)，而第二個參數會指定包含標記協助程式的組件 」 Microsoft.AspNetCore.Mvc.TagHelpers"。 *Microsoft.AspNetCore.Mvc.TagHelpers*是內建 ASP.NET Core 標記協助程式組件。

若要公開所有將在此專案中的標記協助程式 (這會建立名為組件*AuthoringTagHelpers*)，您會使用下列：

[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

如果您的專案包含`EmailTagHelper`與預設的命名空間 (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`)，您可以提供標記協助程式的完整限定的名稱 (FQN):

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

若要新增的檢視使用 FQN 標記協助程式，您先新增 FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`)，並接著組件名稱 (*AuthoringTagHelpers*)。 大部分的開發人員想要使用 「\*"的萬用字元語法。 萬用字元語法可讓您插入萬用字元"\*"FQN 中的尾碼。 例如，任何下列指示詞會顯示`EmailTagHelper`:

```csharp
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

如先前所述，新增`@addTagHelper`指示詞加入*Views/_ViewImports.cshtml*檔案讓標記協助程式中的所有檢視檔案*檢視*目錄和子目錄。 您可以使用`@addTagHelper`指示詞中特定的檢視檔案，如果您想要選擇加入以公開這些檢視表標記協助專家。

<a name=remove-razor-directives-label></a>

### <a name="removetaghelper-removes-tag-helpers"></a>`@removeTagHelper`移除標記協助程式

`@removeTagHelper`具有相同的兩個參數，做為`@addTagHelper`，並且會移除先前新增標記協助程式。 例如，`@removeTagHelper`從檢視套用至特定檢視移除指定的標記協助程式。 使用`@removeTagHelper`中*Views/Folder/_ViewImports.cshtml*檔案從所有檢視中都移除指定的標記協助程式*資料夾*。

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a>控制標記協助程式範圍*_ViewImports.cshtml*檔案

您可以加入*_ViewImports.cshtml*任何檢視資料夾中，檢視引擎要套用至和指示詞從這兩個檔案和*Views/_ViewImports.cshtml*檔案。 如果您加入空白*Views/Home/_ViewImports.cshtml*檔，以取得*首頁*檢視，會有任何變更因為*_ViewImports.cshtml*檔案是加總。 任何`@addTagHelper`指示詞加入*Views/Home/_ViewImports.cshtml*檔案 (不在預設*Views/_ViewImports.cshtml*檔案) 會公開至檢視這些標記協助程式只能在*首頁*資料夾。

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a>選擇退出個別項目

您可以停用在項目層級，以標記協助程式退出字元標記協助程式 ("！")。 例如，`Email`中已停用驗證`<span>`標記協助程式退出字元：

```csharp
<!span asp-validation-for="Email" class="text-danger"></!span>
```

您必須套用標記協助程式退出字元的開頭和結尾標記。 （在 Visual Studio 編輯器中自動將退出字元加入結尾標記，當您加入一個開頭標記）。 加入選擇退出字元之後，此項目和標記協助程式屬性不再顯示在特殊的字型。

<a name=prefix-razor-directives-label></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a>使用`@tagHelperPrefix`進行明確標記協助程式使用方式

`@tagHelperPrefix`指示詞可讓您指定要啟用標記協助程式支援，並讓標記協助程式使用明確標記前置詞字串。 例如，您可以加入下列標記以*Views/_ViewImports.cshtml*檔案：

```html
@tagHelperPrefix th:
```
在下列程式碼影像，標記協助程式前置詞設定為`th:`，因此只有在使用前置詞的元素`th:`支援標記協助程式 （啟用標記協助程式的項目會有特殊的字型）。 `<label>`和`<input>`元素有標記協助程式前置詞且標記協助程式功能，同時`<span>`元素則否。

![影像](intro/_static/thp.png)

相同的階層規則套用至`@addTagHelper`也適用於`@tagHelperPrefix`。

## <a name="intellisense-support-for-tag-helpers"></a>標記協助程式的 IntelliSense 支援

當您在 Visual Studio 中建立新的 ASP.NET web 應用程式時，它會加入 NuGet 套件 」 Microsoft.AspNetCore.Razor.Tools"。 這是封裝會新增標記協助程式工具。

請考慮將寫入 HTML`<label>`項目。 當您輸入`<l`在 Visual Studio 編輯器中，IntelliSense 會顯示相符項目：

![影像](intro/_static/label.png)

不只取得 HTML 說明，但圖示 ("@"符號與在其下的"<>")。

![影像](intro/_static/tagSym.png)

識別項目，為標記協助程式的目標。 純的 HTML 項目 (例如`fieldset`) 顯示 「 <> 」 圖示。

純 HTML`<label>`標記棕色字型的屬性，以紅色顯示 （使用預設 Visual Studio 色彩佈景主題） 的 HTML 標記和屬性值以藍色。

![影像](intro/_static/LableHtmlTag.png)

您輸入之後`<label`，IntelliSense 就會列出可用的 HTML/CSS 屬性和標記協助程式為目標屬性：

![影像](intro/_static/labelattr.png)

IntelliSense 陳述式完成可讓您輸入 tab 鍵來完成選取的值的陳述式：

![影像](intro/_static/stmtcomplete.png)

輸入標記協助程式屬性，因為變更標記和屬性的字型。 使用預設 Visual Studio"Blue"或"Light"色彩佈景主題，字型為粗體紫色。 如果您使用 「 暗色調 」 佈景主題的字型是粗體藍綠色。 使用預設佈景主題取得這份文件中的影像。

![影像](intro/_static/labelaspfor2.png)

您可以輸入 Visual Studio *CompleteWord*快顯 (Ctrl + 空格鍵是[預設](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio)置於雙引號內 ("")，而您現在在 C# 中，就像您會在 C# 類別。 IntelliSense 會顯示頁面模型上所有的方法和屬性。 方法和屬性可用的屬性類型，所以`ModelExpression`。 在下列影像中，我編輯`Register` 檢視中，所以`RegisterViewModel`可用。

![影像](intro/_static/intellemail.png)

IntelliSense 會列出的屬性和方法在頁面上的模型。 豐富的 IntelliSense 環境，可協助您選取的 CSS 類別：

![影像](intro/_static/iclass.png)

![影像](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a>相較於 HTML Helper 的標記協助程式

標記協助程式附加至 HTML 元素在 Razor 檢視中，雖然[HTML Helper](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers)會叫用的方法與 HTML 顛倒 Razor 檢視中。 請考慮下列的 Razor 標記 CSS 類別 「 標題 」 建立的 HTML 標籤：

```html
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

在 (`@`) 符號會告知 Razor 這是程式碼啟動。 下面兩個參數 ("FirstName"和"名字:") 是字串，因此[IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense)無法幫助。 最後的引數：

```html
new {@class="caption"}
```

用來代表屬性的匿名物件。 因為**類別**是保留的關鍵字在 C# 中，使用`@`符號，以強制執行 C# 來解譯"@class="做為符號 （屬性名稱）。 前端的設計工具 (人熟悉 HTML/CSS/JavaScript 和其他用戶端技術，但不是熟悉 C# 和 Razor)，大部分的行是外部角色。 有沒有來自 IntelliSense 的說明，必須撰寫的整行。

使用`LabelTagHelper`，相同的標記可以寫成：

![影像](intro/_static/label2.png)

標記協助程式版本，當您輸入`<l`在 Visual Studio 編輯器中，IntelliSense 會顯示相符項目：

![影像](intro/_static/label.png)

IntelliSense 可協助您撰寫的整行。 `LabelTagHelper`也預設為設定的內容`asp-for`到 「 名字 」; 屬性值 ("FirstName")依照 camel 命名法的大小寫慣例的內容轉換為句子空間，其中每個新的大小寫字母，就會發生在屬性名稱所組成。 在下列標記：

![影像](intro/_static/label2.png)

會產生：

```html
<label class="caption" for="FirstName">First Name</label>
```

如果您將內容加入到依照 camel 命名法的大小寫慣例使用句子大小寫的內容不使用`<label>`。 例如: 

![影像](intro/_static/1stName.png)

會產生：

```html
<label class="caption" for="FirstName">Name First</label>
```

下圖的程式碼顯示的表單一部分*Views/Account/Register.cshtml* Razor 檢視舊版 ASP.NET 4.5.x MVC 範本產生隨附於 Visual Studio 2015。

![影像](intro/_static/regCS.png)

Visual Studio 編輯器顯示 C# 程式碼以灰色背景。 例如， `AntiForgeryToken` HTML Helper:

```html
@Html.AntiForgeryToken()
```

即會顯示灰色背景。 大部分的登錄檢視中標記為 C#。 使用標記協助程式的對等方法來比較：

![影像](intro/_static/regTH.png)

標記是更簡潔且更容易讀取、 編輯和維護的 HTML Helper 方法。 C# 程式碼會降低該伺服器需要了解最小值。 Visual Studio 編輯器顯示標記以特殊的字型標記協助程式的目標。

請考慮*電子郵件*群組：

[!code-csharp[Main](intro/sample/Register.cshtml?range=12-18)]

每個 「 asp-」 屬性的值為"Email"，但是"Email"不是字串。 在此內容中，"Email"是 C# 模型運算式屬性`RegisterViewModel`。

在 Visual Studio 編輯器可協助您撰寫**所有**中的標記的註冊表單中，而 Visual Studio 不提供任何說明中的 HTML Helper 方法的程式碼的大部分標記協助程式方法。 [標記協助程式的 IntelliSense 支援](#intellisense-support-for-tag-helpers)會深入探討在使用 Visual Studio 編輯器中的標記協助程式。

## <a name="tag-helpers-compared-to-web-server-controls"></a>相較於 Web 伺服器控制項的標記協助程式

* 標記協助程式沒有; 與其相關的項目它們只參與呈現的項目和內容。 ASP.NET [Web 伺服器控制項](https://msdn.microsoft.com/library/7698y1f0.aspx)宣告和頁面叫用。

* [Web 伺服器控制項](https://msdn.microsoft.com/library/zsyt68f1.aspx)具有非一般的生命週期可以讓開發及偵錯更加困難。

* Web 伺服器控制項可讓您使用的用戶端控制項，將功能加入至用戶端文件物件模型 (DOM) 項目。 標記協助程式有沒有 DOM

* Web 伺服器控制項包括瀏覽器自動偵測。 標記協助程式不知道的瀏覽器。

* 多個標記協助程式就可以處理相同的項目 (請參閱[避免標記協助程式衝突](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts)) 時通常無法撰寫的 Web 伺服器控制項。

* 標記協助程式的標籤和 HTML 項目，它們範圍，內容可以修改，但不直接修改頁面上的其他任何項目。 Web 伺服器控制項具有更廣泛的範圍，而且可以執行的動作會影響您的網頁; 的其他部分啟用非預期的副作用。

* Web 伺服器控制項使用類型轉換器，將字串轉換成物件。 標記協助程式，您可以使用原生方式在 C# 中，因此您不需要進行類型轉換。

* Web 伺服器控制項使用[System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel)來實作元件和控制項的 run-time 和設計階段行為。 `System.ComponentModel`包含基底類別和介面的實作屬性和型別轉換子、 繫結至資料來源，以及授權元件。 對比，以標記協助程式，通常衍生自`TagHelper`，而`TagHelper`基底類別會公開只有兩個方法，`Process`和`ProcessAsync`。

## <a name="customizing-the-tag-helper-element-font"></a>自訂標記協助程式項目字型

您可以自訂字型和顏色標示從**工具** > **選項** > **環境** > **字型和色彩**:

![影像](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a>其他資源

* [撰寫標記協助程式](authoring.md)
* [使用表單](../working-with-forms.md)
* [GitHub 上的 TagHelperSamples](https://github.com/dpaquette/TagHelperSamples)含有標記協助程式的範例使用[Bootstrap](http://getbootstrap.com/)。

<!--
* [Working with Forms ](xref:mvc/views/working-with-forms)
-->
