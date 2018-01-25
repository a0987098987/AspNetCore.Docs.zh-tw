---
title: "ASP.NET Core MVC 中的檢視"
author: ardalis
description: "了解如何檢視處理的應用程式資料的呈現與 ASP.NET Core MVC 中的使用者互動。"
ms.author: riande
manager: wpickett
ms.date: 12/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: dc36c76dbd7d82a926e39d8a8ab3a2a53b65d954
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="views-in-aspnet-core-mvc"></a>ASP.NET Core MVC 中的檢視

由[Steve Smith](https://ardalis.com/)和[Luke Latham](https://github.com/guardrex)

本文件說明 ASP.NET Core MVC 應用程式中使用的檢視。 在 Razor 頁面上的資訊，請參閱[Razor 頁面的簡介](xref:mvc/razor-pages/index)。

在**M**模型-**V**檢視-**C**ontroller (MVC) 模式*檢視*處理應用程式的資料呈現與使用者互動。 檢視是 HTML 範本與內嵌[Razor 標記](xref:mvc/views/razor)。 Razor 標記是與 HTML 標記，以產生傳送至用戶端的網頁互動的程式碼。

ASP.NET Core MVC 中，檢視都*.cshtml*檔案使用[C# 程式設計語言](/dotnet/csharp/)Razor 標記中。 通常，檢視檔案會分組成資料夾名為每個應用程式的[控制器](xref:mvc/controllers/actions)。 資料夾會儲存在*檢視*根目錄中的應用程式的資料夾：

![Visual Studio 方案總管 中的 檢視 資料夾是主資料夾 資料夾開啟顯示 About.cshtml、 Contact.cshtml 和 Index.cshtml 檔案開啟](overview/_static/views_solution_explorer.png)

*首頁*控制器由*首頁*資料夾內*檢視*資料夾。 *首頁*資料夾包含檢視*有關*，*連絡人*，和*索引*（首頁） 網頁。 當使用者要求這些三個網頁中的控制器動作的其中一個*首頁*控制器決定這三個檢視用來建置和網頁傳回給使用者。

使用[配置](xref:mvc/views/layout)來提供一致的網頁區段，並減少重複的程式碼。 配置通常包含標頭、 導覽和功能表項目和頁尾。 頁首和頁尾通常包含許多中繼資料的項目和連結指令碼和樣式資產的未定案標記。 配置可協助您避免在您檢視這個未定案標記。

[部分檢視](xref:mvc/views/partial)管理檢視的可重複使用的組件時，減少程式碼重複。 例如，部分檢視可用於數個檢視中會出現在部落格網站上的作者自傳。 作者自傳是一般的檢視內容，而不需要執行，才能產生的網頁內容的程式碼。 作者自傳內容是內容的可檢視的模型繫結，方式，因此適合用於這種類型中的部分檢視。

[檢視元件](xref:mvc/views/view-components)，在於它們可讓您減少重複的程式碼，都設為部分類似的檢視，但是它們很適用於需要在伺服器上呈現網頁所執行的程式碼的檢視內容。 呈現的內容需要資料庫互動，例如購物車的網站時，檢視元件就很有用。 不限制檢視元件，才能產生網頁輸出模型繫結。

## <a name="benefits-of-using-views"></a>使用檢視的優點

檢視可以協助建立[ **S**eparation **o**f **C**oncerns (SoC) 設計](http://deviq.com/separation-of-concerns/)分隔從使用者介面標記 MVC 應用程式中應用程式的其他部分。 遵循 SoC 設計可讓您的應用程式模組，提供數個優點：

* 應用程式很容易維護，因為組織較佳。 檢視通常會依應用程式功能分組。 這可讓您更輕鬆地找到相關的檢視，使用一項功能時。
* 鬆散耦合的應用程式組件。 您可以建置並更新商務邏輯和資料存取元件分開的應用程式的檢視。 您可以修改應用程式的檢視，而不一定需要更新應用程式的其他部分。
* 它是您更輕鬆地測試應用程式的使用者介面部分，因為檢視表的個別單位。
* 因為較佳的組織，而較不可能是您會意外地重複的區段的使用者介面。

## <a name="creating-a-view"></a>建立檢視

檢視特定的控制站中建立*檢視 / [ControllerName]*資料夾。 在控制站之間共用的檢視會放在*Views/Shared*資料夾。 若要建立的檢視，將新檔案，並提供相同的名稱與它關聯之的控制器的動作*.cshtml*檔案副檔名。 若要建立對應於檢視*有關*中的動作*首頁*控制器，建立*About.cshtml*檔案中*Views/Home*資料夾：

[!code-cshtml[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

*Razor*標記開頭`@`符號。 執行的 C# 陳述式放入 C# 程式碼內[Razor 程式碼區塊](xref:mvc/views/razor#razor-code-blocks)設定 off，大括號括住 (`{ ... }`)。 例如，請參閱 [關於] 的指派`ViewData["Title"]`如上所示。 您可以顯示在 HTML 中的值只參考的值與`@`符號。 請參閱的內容`<h2>`和`<h3>`上述項目。

如上所示的檢視內容是只有一部分會呈現給使用者的整個網頁。 其餘的頁面配置和其他常用的部分檢視的其他檢視檔案中指定。 若要進一步了解，請參閱[配置主題](xref:mvc/views/layout)。

## <a name="how-controllers-specify-views"></a>控制器指定檢視的方式

檢視通常會傳回動作為[ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult)，這是一種[ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult)。 動作方法可以建立並傳回`ViewResult`直接，但是一般來說，不進行。 因為大部分的控制站繼承自[控制器](/aspnet/core/api/microsoft.aspnetcore.mvc.controller)，您只需要使用`View`helper 方法以傳回`ViewResult`:

*HomeController.cs*

[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

這個動作會傳回*About.cshtml*的最後一節中所顯示的檢視會轉譯為下列網頁：

![關於在 microsoft Edge 瀏覽器中呈現的頁面](overview/_static/about-page.png)

`View` Helper 方法擁有數個多載。 您可以選擇性地指定：

* 要傳回明確的檢視：

  ```csharp
  return View("Orders");
  ```
* A[模型](xref:mvc/models/model-binding)来傳遞至檢視：

  ```csharp
  return View(Orders);
  ```
* 檢視和模型：

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a>檢視探索

當動作傳回的檢視時，這個程序稱為*檢視探索*進行。 此程序決定根據檢視名稱使用的檢視檔案。 

預設行為`View`方法 (`return View();`) 會傳回與動作方法會呼叫同名的檢視。 例如，*有關*`ActionResult`控制站的方法名稱用來搜尋名為的檢視檔案*About.cshtml*。 首先，執行階段會尋找*檢視 / [ControllerName]*檢視的資料夾。 如果找不到那里相符的檢視，它會搜尋*共用*檢視的資料夾。

如果您以隱含方式傳回，它並不重要`ViewResult`與`return View();`或明確地將傳遞至檢視表名稱`View`方法`return View("<ViewName>");`。 在這兩種情況下，檢視探索會搜尋相符的檢視檔案，依此順序：

   1. *Views/\[ControllerName]\[ViewName].cshtml*
   1. *Views/Shared/\[ViewName].cshtml*

可以提供的檢視檔案的路徑，而不是檢視表名稱。 如果使用 啟動應用程式根目錄的絕對路徑 (選擇性地從"/"或"~ /")，則*.cshtml*必須指定延伸模組：

```csharp
return View("Views/Home/About.cshtml");
```

您也可以使用相對路徑來指定不同的目錄，而檢視*.cshtml*延伸模組。 內部`HomeController`，您可以傳回*索引*檢視您*管理*檢視具有相對路徑：

```csharp
return View("../Manage/Index");
```

同樣地，您可以指出與目前的控制站的特定目錄 」。 /"前置詞：

```csharp
return View("./About");
```

[部分檢視](xref:mvc/views/partial)和[檢視元件](xref:mvc/views/view-components)使用類似 （但不是完全相同） 的探索機制。

您可以自訂如何檢視位於應用程式中使用自訂的預設慣例[IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander)。

檢視探索依賴檔案名稱來尋找檢視的檔案。 如果基礎檔案系統區分大小寫，檢視名稱會可能區分大小寫。 作業系統之間的相容性，請控制器和動作名稱與相關聯的檢視資料夾和檔案名稱之間的大小寫須相符。 如果您遇到的錯誤，使用區分大小寫的檔案系統時找不到檢視檔案，確認其大小寫符合要求的檢視檔案和實際的檢視檔案名稱之間。

請遵循最佳作法來組織您的檢視，以反映控制器、 動作和可維護性和避免困擾的檢視之間的關聯性的檔案結構。

## <a name="passing-data-to-views"></a>將資料傳遞至檢視

您可以將資料傳遞至使用數種方法的檢視。 最健全的作法是指定[模型](xref:mvc/models/model-binding)檢視中的型別。 這種模型通常稱為*viewmodel*。 您傳遞至檢視的 viewmodel 類型的執行個體的動作。

將資料傳遞至檢視中使用 viewmodel 可讓檢視來善用*強式*類型檢查。 *強式類型*(或*強型別*) 表示每個變數和常數有明確定義的型別 (例如， `string`， `int`，或`DateTime`)。 在編譯時期檢查有效性的檢視中使用的類型。

[Visual Studio](https://www.visualstudio.com/vs/)和[Visual Studio Code](https://code.visualstudio.com/)列出強型別類別成員使用一項功能稱為[IntelliSense](/visualstudio/ide/using-intellisense)。 當您想要查看的 viewmodel 屬性時，輸入變數名稱，後面接著句號 viewmodel (`.`)。 這有助於減少錯誤更快撰寫程式碼。

指定模型使用`@model`指示詞。 使用模型與`@Model`:

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

若要提供模型給檢視，控制器會將它做為參數：

```csharp
public IActionResult Contact()
{
    ViewData["Message"] = "Your contact page.";

    var viewModel = new Address()
    {
        Name = "Microsoft",
        Street = "One Microsoft Way",
        City = "Redmond",
        State = "WA",
        PostalCode = "98052-6399"
    };

    return View(viewModel);
}
```

沒有任何限制，您可以提供給檢視的模型型別。 我們建議您使用**P**lain **O**ld **C**LR **O**物件 (POCO) viewmodels 少量或沒有定義的行為 （方法）。 通常，viewmodel 類別可能會儲存在*模型*資料夾或個別*ViewModels*根目錄中的應用程式的資料夾。 *位址*viewmodel 使用上述範例中是儲存在名為 POCO viewmodel *Address.cs*:

```csharp
namespace WebApplication1.ViewModels
{
    public class Address
    {
        public string Name { get; set; }
        public string Street { get; set; }
        public string City { get; set; }
        public string State { get; set; }
        public string PostalCode { get; set; }
    }
}
```

> [!NOTE]
> 會造成任何問題 viewmodel 類型和您的商務模型類型使用相同的類別。 不過，使用不同的模型可讓您的檢視而異獨立的商務邏輯和資料存取應用程式部分。 模型和 viewmodels 分離也提供安全性優點，當模型使用時[模型繫結](xref:mvc/models/model-binding)和[驗證](xref:mvc/models/validation)資料傳送到應用程式的使用者。


<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-and-viewbag"></a>弱型別資料 （別的 ViewData 和 ViewBag）

注意： `ViewBag` Razor 頁面中沒有。

除了強型別檢視表檢視可以存取*弱型別*(也稱為*鬆散型別*) 的資料集合。 不同於強式類型，*弱式類型*(或*鬆散類型*) 表示您沒有明確宣告的您所使用的資料類型。 您可以使用弱式型別資料的集合，用來傳遞資料移轉入和控制器和檢視的資訊量很少。

| 傳遞之間的資料...                        | 範例                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| 在控制器與檢視                             | 填入下拉式清單的資料。                                          |
| 檢視和[版面配置檢視](xref:mvc/views/layout)   | 設定**\<標題 >**版面配置檢視的檢視檔案中的項目內容。  |
| [部分檢視](xref:mvc/views/partial)和檢視 | 一種 widget，會根據使用者要求網頁顯示資料。      |

此集合可透過參考`ViewData`或`ViewBag`控制器和檢視上的屬性。 `ViewData`屬性是弱型別物件的字典。 `ViewBag`屬性是周圍的包裝函式`ViewData`基礎提供動態內容`ViewData`集合。

`ViewData`和`ViewBag`以動態方式在執行階段解析。 由於它們不提供編譯時間類型檢查，因此兩者都是通常更容易發生錯誤比使用 viewmodel。 基於這個原因，有些開發人員想要使用最少或從不`ViewData`和`ViewBag`。


<a name="VD"></a>

**ViewData**

`ViewData`是[ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary)物件透過存取`string`索引鍵。 字串資料可以儲存和使用直接，而不需要轉型，但您必須轉換為其他`ViewData`物件特定類型的值，當您擷取它們。 您可以使用`ViewData`來控制站中的資料傳遞至檢視和檢視，包括內[部分檢視](xref:mvc/views/partial)和[配置](xref:mvc/views/layout)。

以下是設定問候語和位址使用的值範例`ViewData`動作：

```csharp
public IActionResult SomeAction()
{
    ViewData["Greeting"] = "Hello";
    ViewData["Address"]  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

使用檢視中的資料：

```cshtml
@{
    // Since Address isn't a string, it requires a cast.
    var address = ViewData["Address"] as Address;
}

@ViewData["Greeting"] World!

<address>
    @address.Name<br>
    @address.Street<br>
    @address.City, @address.State @address.PostalCode
</address>
```

**ViewBag**

注意： `ViewBag` Razor 頁面中沒有。

`ViewBag`是[DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata)物件，提供儲存在物件的動態存取`ViewData`。 `ViewBag`可能更方便使用，因為它不需要進行轉型。 下列範例示範如何使用`ViewBag`與使用相同的結果與`ViewData`上方：

```csharp
public IActionResult SomeAction()
{
    ViewBag.Greeting = "Hello";
    ViewBag.Address  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

```cshtml
@ViewBag.Greeting World!

<address>
    @ViewBag.Address.Name<br>
    @ViewBag.Address.Street<br>
    @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
</address>
```

**同時使用別的 ViewData ViewBag**

注意： `ViewBag` Razor 頁面中沒有。

因為`ViewData`和`ViewBag`參考相同的基礎`ViewData`集合，您可以同時使用`ViewData`和`ViewBag`並混用，而且符合之間讀取和寫入的值時。

設定標題使用`ViewBag`和描述使用`ViewData`頂端*About.cshtml*檢視：

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

讀取內容，但是反向使用`ViewData`和`ViewBag`。 在*_Layout.cshtml*檔案中，取得標題使用`ViewData`並取得描述使用`ViewBag`:

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

請記住，字串不需要轉型為`ViewData`。 您可以使用`@ViewData["Title"]`而不用轉型。

使用這兩個`ViewData`和`ViewBag`在相同時間運作方式，為沒有混合和比對讀取和寫入的屬性。 會呈現下列標記：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

**ViewBag 別的 ViewData 之間差異的摘要**

 `ViewBag`無法使用 Razor 頁面。

* `ViewData`
  * 衍生自[ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary)，因此它的字典屬性會很有用，例如`ContainsKey`， `Add`， `Remove`，和`Clear`。
  * 字典中的索引鍵是字串，因此允許空白字元。 範例：`ViewData["Some Key With Whitespace"]`
  * 任何型別以外`string`必須轉換中要使用的檢視`ViewData`。
* `ViewBag`
  * 衍生自[DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata)，因此它可以建立動態屬性使用點標記法 (`@ViewBag.SomeKey = <value or object>`)，而且不會執行轉換需要。 語法`ViewBag`可更快速地將加入至控制器和檢視。
  * 容易檢查 null 值。 範例：`@ViewBag.Person?.Name`

**何時使用別的 ViewData 或 ViewBag**

同時`ViewData`和`ViewBag`同樣會傳遞小量的資料在控制器和檢視之間的有效方法。 若要使用哪一個選擇根據喜好設定。 您可以混合和比對`ViewData`和`ViewBag`物件，不過，程式碼是容易讀取與維護與使用一致的其中一個方法。 這兩種方法是在執行階段以動態方式解決，因此容易導致執行階段錯誤。 某些開發團隊予以避免。

### <a name="dynamic-views"></a>動態檢視

不要宣告模型的檢視類型使用`@model`但可以是傳遞給它們的模型執行個體 (例如， `return View(Address);`) 可以動態地參考執行個體的屬性：

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

此功能，可提供彈性，但並未提供編譯保護或 IntelliSense。 如果屬性不存在，網頁產生在執行階段失敗。

## <a name="more-view-features"></a>詳細的檢視功能

[標記協助程式](xref:mvc/views/tag-helpers/intro)讓您輕鬆將伺服器端行為加入至現有的 HTML 標記。 使用標記協助程式，可避免需要撰寫自訂程式碼或在檢視內的協助程式。 標記協助程式會做為屬性套用至 HTML 元素，而且會忽略由無法處理它們的編輯器。 這可讓您編輯並呈現檢視標記中的各種工具。

許多內建的 HTML Helper 可達到產生自訂的 HTML 標記。 更複雜的使用者介面邏輯可以處理由[檢視元件](xref:mvc/views/view-components)。 檢視元件提供的相同 SoC 該控制站，並檢視所提供。 它們不需要動作和一般使用者介面項目所使用的資料處理的檢視。

如同許多其他 ASP.NET Core 方面，檢視支援[相依性插入](xref:fundamentals/dependency-injection)，讓服務變成[插入檢視](xref:mvc/views/dependency-injection)。
