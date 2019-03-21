---
title: ASP.NET Core MVC 中的檢視
author: ardalis
description: 了解檢視如何處理 ASP.NET Core MVC 中的應用程式資料呈現和使用者互動。
ms.author: riande
ms.date: 12/12/2017
uid: mvc/views/overview
ms.openlocfilehash: 0ee1fef9e9da15d91427a2eb5b5f530a0b77ce33
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265383"
---
# <a name="views-in-aspnet-core-mvc"></a>ASP.NET Core MVC 中的檢視

作者：[Steve Smith](https://ardalis.com/) 和 [Luke Latham](https://github.com/guardrex)

本文件說明 ASP.NET Core MVC 應用程式中所使用的檢視。 如需 Razor 頁面的資訊，請參閱 [Razor 頁面簡介](xref:razor-pages/index)。

在模型檢視控制器 (MVC) 模式中，「檢視」會處理應用程式的資料呈現和使用者互動。 檢視是具有內嵌 [Razor 標記](xref:mvc/views/razor)的 HTML 範本。 Razor 標記是與 HTML 標記互動的程式碼，可以產生傳送至用戶端的網頁。

在 ASP.NET Core MVC 中，檢視是在 Razor 標記中使用 [C# 程式設計語言](/dotnet/csharp/)的 *.cshtml* 檔案。 通常，檢視檔案會分組成針對每個應用程式之[控制器](xref:mvc/controllers/actions)而命名的資料夾。 資料夾會儲存至應用程式根目錄的 *Views* 資料夾中：

![Visual Studio 方案總管中的 Views 資料夾是與 Home 資料夾一起開啟，以顯示 About.cshtml、Contact.cshtml 和 Index.cshtml 檔案](overview/_static/views_solution_explorer.png)

*Home* 控制器是由 *Views* 資料夾內的 *Home* 資料夾所呈現。 *Home* 資料夾包含 *About*、*Contact* 和 *Index* (首頁) 網頁的檢視。 使用者要求這三個網頁中的其中一個時，*Home* 控制器中的控制器動作可決定使用這三個檢視的哪一個來建置網頁並將其傳回給使用者。

使用[配置](xref:mvc/views/layout)，提供一致的網頁區段，並減少程式碼重複。 配置通常包含標頭、導覽和功能表項目，以及頁尾。 標頭和頁尾通常會包含許多中繼資料項目以及指令碼和樣式表連結的未定案標記。 配置可協助您避免在檢視中使用此未定案標記。

[部分檢視](xref:mvc/views/partial)透過管理檢視的可重複使用部分，來減少程式碼重複。 例如，部分檢視可用於數個檢視中出現之部落格網站上的作者自傳。 作者自傳是一般檢視內容，不需要執行程式碼就能產生網頁內容。 檢視單獨透過模型繫結就可以使用作者自傳內容，因此最適合使用這類型內容的部分檢視。

[檢視元件](xref:mvc/views/view-components)與部分檢視類似，可讓您減少重複的程式碼，但它們適用於需要程式碼在伺服器上執行才能轉譯網頁的檢視內容。 轉譯的內容需要資料庫互動時 (例如網站購物車)，檢視元件十分有用。 檢視元件不會限制為模型繫結，以產生網頁輸出。

## <a name="benefits-of-using-views"></a>使用檢視的優點

檢視可協助在 MVC 應用程式內建立[關注區隔](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns)，方法是區隔使用者介面標記與應用程式的其他部分。 遵循 SoC 設計可讓您的應用程式模組化，以提供數個優點：

* 應用程式較容易維護，因為其組織性較佳。 檢視一般會依應用程式功能分組。 這可讓您在處理功能時更輕鬆地找到相關檢視。
* 應用程式的組件是鬆散耦合的。 您可以分別從商務邏輯和資料存取元件來建置和更新應用程式的檢視。 您可以修改應用程式的檢視，而不一定需要更新應用程式的其他部分。
* 因為檢視是個別單位，所以可以更輕鬆地測試應用程式的使用者介面部分。
* 為達到更佳的編排情況，較不希望使用者介面的區段意外地重複。

## <a name="creating-a-view"></a>建立檢視

會在 *Views/[ControllerName]* 資料夾中建立控制器特有的檢視。 在控制器之間共用的檢視會放在 *Views/Shared* 資料夾中。 若要建立檢視，請新增檔案，並讓它與建立關聯的控制器動作同名，且副檔名為 *.cshtml*。 若要在 *Home* 控制器中建立與 *About* 動作對應的檢視，請在 *Views/Home* 資料夾中建立 *About.cshtml* 檔案：

[!code-cshtml[](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

*Razor* 標記的開頭為 `@` 符號。 在大括弧 (`{ ... }`) 所設定的 [Razor 程式碼區塊](xref:mvc/views/razor#razor-code-blocks)內放入 C# 程式碼，即可執行 C# 陳述式。 例如，請參閱上方將 "About" 指派給 `ViewData["Title"]`。 只要使用 `@` 符號參考值，就可以在 HTML 內顯示值。 請參閱上方 `<h2>` 和 `<h3>` 項目的內容。

上述檢視內容只是轉譯給使用者之整個網頁的一部分。 其餘的頁面配置以及檢視的其他通用層面指定於其他檢視檔案中。 若要深入了解，請參閱[配置主題](xref:mvc/views/layout)。

## <a name="how-controllers-specify-views"></a>控制器指定檢視的方式

檢視通常會從動作傳回為 [ViewResult](/dotnet/api/microsoft.aspnetcore.mvc.viewresult)，這是 [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) 的類型。 動作方法可以直接建立並傳回 `ViewResult`，但這通常不會進行。 因為大部分的控制器都是繼承自[控制器](/dotnet/api/microsoft.aspnetcore.mvc.controller)，所以您只需要使用 `View` 協助程式方法傳回 `ViewResult`：

*HomeController.cs*

[!code-csharp[](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

傳回此動作時，最後一個區段中所顯示的 *About.cshtml* 檢視會轉譯為下列網頁：

![關於 Edge 瀏覽器中所轉譯的頁面](overview/_static/about-page.png)

`View` 協助程式方法具有數個多載。 您可以選擇性地指定：

* 要傳回的明確檢視：

  ```csharp
  return View("Orders");
  ```

* 要傳遞至檢視的[模型](xref:mvc/models/model-binding)：

  ```csharp
  return View(Orders);
  ```

* 檢視和模型：

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a>檢視探索

動作傳回檢視時，會進行稱為「檢視探索」的程序。 此程序根據檢視名稱來決定使用的檢視檔案。 

`View` 方法的預設行為 (`return View();`) 是傳回的檢視與從中呼叫它的動作方法同名。 例如，使用控制器的 *About* `ActionResult` 方法名稱來搜尋名為 *About.cshtml* 的檢視檔案。 首先，執行階段會在 *Views/[ControllerName]* 資料夾中尋找檢視。 如果在這裡找不到相符的檢視，則會搜尋檢視的 *Shared* 資料夾。

如果您使用 `return View();` 以隱含方式傳回 `ViewResult`，或使用 `return View("<ViewName>");` 將檢視名稱明確地傳遞至 `View` 方法，則不重要。 在這兩種情況下，檢視探索會依此順序搜尋相符的檢視檔案：

   1. *Views/\[ControllerName]/\[ViewName].cshtml*
   1. *Views/Shared/\[ViewName].cshtml*

可以提供檢視檔案路徑，而不是檢視名稱。 如果使用從應用程式根目錄開始的絕對路徑 (選擇性地開始於 "/" 或 "~/")，則必須指定 *.cshtml* 副檔名：

```csharp
return View("Views/Home/About.cshtml");
```

您也可以使用相對路徑來指定不同目錄中的檢視，但沒有 *.cshtml* 副檔名。 在 `HomeController` 內，您可以使用相對路徑傳回 *Manage* 檢視的 *Index* 檢視：

```csharp
return View("../Manage/Index");
```

同樣地，您可以使用 "./" 前置詞來指出目前的控制器特定目錄：

```csharp
return View("./About");
```

[部分檢視](xref:mvc/views/partial)和[檢視元件](xref:mvc/views/view-components)使用類似 (但不同) 的探索機制。

您可以使用自訂 [IViewLocationExpander](/dotnet/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander)，來自訂檢視如何位在應用程式內的預設慣例。

檢視探索依賴如何依檔案名稱來尋找檢視檔案。 如果基礎檔案系統區分大小寫，則檢視名稱可能會區分大小寫。 基於作業系統之間的相容性，控制器與動作名稱和建立關聯的檢視資料夾和檔案名稱之間的大小寫必須相符。 如果您遇到使用區分大小寫的檔案系統時找不到檢視檔案的錯誤，請確認所要求檢視檔案與實際檢視檔案名稱之間的大小寫符合。

請遵循最佳做法來組織檢視的檔案結構，以反映控制器、動作與檢視之間的關聯性來進行維護和避免混淆。

## <a name="passing-data-to-views"></a>將資料傳遞至檢視

使用數種方法將資料傳遞至檢視：

* 強型別資料：viewmodel
* 弱型別資料
  * `ViewData` (`ViewDataAttribute`)
  * `ViewBag`

### <a name="strongly-typed-data-viewmodel"></a>強型別資料 (viewmodel)

最穩健的方法是在檢視中指定 [model](xref:mvc/models/model-binding) 類型。 此模型通常稱為 *viewmodel*。 您可以透過動作將 viewmodel 類型執行個體傳遞至檢視。

使用 viewmodel 將資料傳遞至檢視，讓檢視利用「強式」檢查。 *強式型別* (或*強型別*) 代表每個變數與常數都有明確定義的類型 (例如，`string`、`int` 或 `DateTime`)。 在編譯時期檢查檢視中所使用之類型的有效性。

[Visual Studio](https://www.visualstudio.com/vs/) 與 [Visual Studio Code](https://code.visualstudio.com/) 會使用稱為 [IntelliSense](/visualstudio/ide/using-intellisense) 的功能，列出強型別類別成員。 當您想要查看 viewmodel 的屬性時，請鍵入 viewmodel 的變數名稱，後面接著句號 (`.`)。 這有助於更快速地撰寫程式碼，並且減少錯誤。

使用 `@model` 指示詞來指定模型。 搭配使用模型與 `@Model`：

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

若要將模型提供給檢視，控制器會將它當成參數傳遞：

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

您可以提供給檢視的模型類型沒有任何限制。 建議您使用定義最少行為或沒有定義行為 (方法) 的簡單的 CLR 物件 (POCO) viewmodel。 通常，viewmodel 類別會儲存至應用程式根目錄的 *Models* 資料夾或個別 *ViewModels* 資料夾。 上述範例中所使用的 *Address* viewmodel 是儲存至名為 *Address.cs* 之檔案中的 POCO viewmodel：

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

您可以將相同的類別用於 viewmodel 類型和商務模型類型。 不過，使用不同的模型可讓您的檢視因應用程式的商務邏輯和資料存取部分而不同。 模型將[模型繫結](xref:mvc/models/model-binding)和[驗證](xref:mvc/models/validation)用於依使用者傳送至應用程式的資料時，模型與 viewmodel 的區隔也提供安全性優點。

<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-viewdata-attribute-and-viewbag"></a>弱型別資料 (ViewData、ViewData 屬性與 ViewBag)

Razor 頁面中沒有 `ViewBag`。

除了強型別檢視之外，檢視還可以存取*弱型別* (也稱為*鬆散型別*) 資料集合。 與強式型別不同，「弱式型別」 (或「鬆散型別」) 表示您未明確宣告所使用資料的類型。 您可以使用弱型別資料的集合，對控制器與檢視傳遞將少量的資料進出。

| 在 ... 之間傳遞資料                        | 範例                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| 控制器和檢視                             | 使用資料填入下拉式清單。                                          |
| 檢視和[配置檢視](xref:mvc/views/layout)   | 透過檢視檔案，在配置檢視中設定 **\<title>** 項目內容。  |
| [部分檢視](xref:mvc/views/partial)和檢視 | 一種小工具，可根據使用者所要求的網頁來顯示資料。      |

此集合可以透過控制器和檢視上的 `ViewData` 或 `ViewBag` 屬性進行參考。 `ViewData` 屬性是弱型別物件的字典。 `ViewBag` 屬性是 `ViewData` 中提供基礎 `ViewData` 集合之動態屬性的包裝函式。

`ViewData` 和 `ViewBag` 是在執行階段動態解析。 因為它們未提供編譯時間類型檢查，所以兩者通常會比使用 viewmodel 更容易發生錯誤。 因此，有些開發人員會盡量少使用或不使用 `ViewData` 和 `ViewBag`。

<a name="VD"></a>

**ViewData**

`ViewData` 是透過 `string` 索引鍵存取的 [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) 物件。 字串資料可以直接儲存和使用，而不需要轉換；但必須在擷取其他 `ViewData` 物件值時將其轉換為特定類型。 您可以使用 `ViewData` 將資料從控制器傳遞至檢視以及在檢視內傳遞資料，包括[部分檢視](xref:mvc/views/partial)和[配置](xref:mvc/views/layout)。

下列範例使用運作中 `ViewData` 來設定問候語和地址的值：

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

::: moniker range=">= aspnetcore-2.1"

**ViewData 屬性**

使用 [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) 的另一種方法是 [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute)。 控制器或 Razor 頁面模型上裝飾以 `[ViewData]` 的屬性會將其值儲存在字典並從中載入。

在下列範例中，包含 `Title` 屬性的首頁控制器是以 `[ViewData]` 裝飾。 `About` 方法會設定 [About] 檢視的標題：

```csharp
public class HomeController : Controller
{
    [ViewData]
    public string Title { get; set; }

    public IActionResult About()
    {
        Title = "About Us";
        ViewData["Message"] = "Your application description page.";

        return View();
    }
}
```

在 [About] 檢視上，存取 `Title` 屬性作為模型屬性：

```cshtml
<h1>@Model.Title</h1>
```

在此配置中，標題會從 ViewData 字典中讀取：

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

::: moniker-end

**ViewBag**

Razor 頁面中沒有 `ViewBag`。

`ViewBag` 是可動態存取 `ViewData` 中所儲存物件的 [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) 物件。 `ViewBag` 的使用更為方便，因為它不需要進行轉換。 下列範例示範如何使用 `ViewBag`，而其結果與上方使用 `ViewData` 相同：

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

**同時使用 ViewData 和 ViewBag**

Razor 頁面中沒有 `ViewBag`。

因為 `ViewData` 和 `ViewBag` 參照相同的基礎 `ViewData` 集合，所以您可以同時使用 `ViewData` 和 `ViewBag`，並在讀取和寫入值時於其間混合使用和比對。

使用 `ViewBag` 設定標題，並使用 *About.cshtml* 檢視頂端的 `ViewData` 來設定描述：

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

讀取屬性，但反向使用 `ViewData` 和 `ViewBag`。 在 *_Layout.cshtml* 檔案中，使用 `ViewData` 取得標題，並使用 `ViewBag` 取得描述：

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

請記住，針對 `ViewData`，不需要轉換字串。 您可以使用 `@ViewData["Title"]`，而不需要轉換。

可以同時使用 `ViewData` 和 `ViewBag`，與混合使用和比對讀取與寫入屬性一樣。 會轉譯下列標記：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

**ViewData 與 ViewBag 之間的差異摘要**

 Razor 頁面中沒有 `ViewBag`。

* `ViewData`
  * 衍生自 [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary)，因此它的字典屬性十分有用，例如 `ContainsKey`、`Add`、`Remove` 和 `Clear`。
  * 字典中的索引鍵是字串，因此允許空白字元。 範例：`ViewData["Some Key With Whitespace"]`
  * 在檢視中必須轉換任何 `string` 以外的類型，才能使用 `ViewData`。
* `ViewBag`
  * 衍生自 [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata)，因此允許使用點標記法 (`@ViewBag.SomeKey = <value or object>`) 來建立動態屬性，而不需要轉換。 `ViewBag` 的語法可以更快速地新增至控制器和檢視。
  * 檢查 Null 值更簡單。 範例：`@ViewBag.Person?.Name`

**何時使用 ViewData 或 ViewBag**

`ViewData` 和 `ViewBag` 是同樣有效的方式，都可以在控制器與檢視之間傳遞少量資料。 根據喜好設定來選擇使用哪一個。 您可以混合使用與比對 `ViewData` 和 `ViewBag` 物件；不過，使用一致的方法，較容易讀取和維護程式碼。 這兩種方法是在執行階段動態解析，因此容易導致執行階段錯誤。 有些開發小組會避免使用它們。

### <a name="dynamic-views"></a>動態檢視

如果檢視未使用 `@model` 來宣告模型類型，但具有傳遞給它們的模型執行個體 (例如，`return View(Address);`)，則可以動態參考執行個體的屬性：

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

此功能提供彈性，但未提供編譯保護或 IntelliSense。 如果屬性不存在，則網頁產生會在執行階段失敗。

## <a name="more-view-features"></a>其他檢視功能

[標籤協助程式](xref:mvc/views/tag-helpers/intro)可讓您輕鬆地將伺服器端行為新增至現有 HTML 標籤。 使用標籤協助程式就不需要在檢視內撰寫自訂程式碼或協助程式。 標籤協助程式會以屬性形式套用至 HTML 項目，而且無法處理它們的編輯器會忽略它們。 這可讓您在各種工具中編輯和轉譯檢視標記。

許多內建 HTML 協助程式都可以產生自訂 HTML 標記。 [檢視元件](xref:mvc/views/view-components)可以處理更複雜的使用者介面邏輯。 檢視元件所提供的 SoC 與該控制器和檢視所提供的 SoC 相同。 它們不需要動作和檢視來處理一般使用者介面項目所使用的資料。

與許多其他 ASP.NET Core 層面一樣，檢視支援[相依性插入](xref:fundamentals/dependency-injection)，允許將服務[插入檢視](xref:mvc/views/dependency-injection)。
