---
title: "檢視概觀"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 668c320d-c050-45e3-8161-2f460dc93b2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: a93ee8165be52e33c2e7da4d3fee2c8225864db9
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="rendering-html-with-views-in-aspnet-core-mvc"></a>將 HTML 呈現使用 ASP.NET Core MVC 中的檢視

由[Steve Smith](http://ardalis.com)

ASP.NET Core MVC 控制器可能會傳回使用的格式化的結果*檢視*。

## <a name="what-are-views"></a>檢視有哪些？

在模型檢視控制器 (MVC) 模式中，*檢視*封裝與應用程式的使用者互動的呈現方式詳細資料。 檢視是內嵌程式碼與 HTML 範本產生要傳送給用戶端內容。 檢視使用[Razor 語法](razor.md)，可讓程式碼與 HTML 互動最少的程式碼或儀式。

ASP.NET Core MVC 檢視*.cshtml*預設會在儲存檔案*檢視*應用程式中的資料夾。 一般而言，每個控制器將具有自己的資料夾，其中都是適用於特定控制器動作的檢視。

![在 [方案總管] 的 [檢視] 資料夾](overview/_static/views_solution_explorer.png)

動作特定的檢視，除了[部分檢視](partial.md)，[版面配置和其他特殊的檢視檔案](layout.md)可用來協助降低重複，並允許在應用程式的檢視內的重複使用。

## <a name="benefits-of-using-views"></a>使用檢視的優點

檢視提供[的重要性分離](http://deviq.com/separation-of-concerns/)MVC 應用程式，封裝使用者介面層級標記分開商務邏輯中。 ASP.NET MVC 檢視使用[Razor 語法](razor.md)，讓切換 HTML 標記和伺服器端邏輯輕鬆的方式。 一般的重複性層面應用程式的使用者介面可輕易地重複使用的檢視之間[版面配置和共用的指示詞](layout.md)或[部分檢視](partial.md)。

## <a name="creating-a-view"></a>建立檢視

檢視特定的控制站中建立*檢視 / [ControllerName]*資料夾。 在控制站之間共用的檢視會放在*/檢視表/共用*資料夾。 檢視檔案與它關聯之的控制器的動作，相同的名稱，並加入*.cshtml*檔案副檔名。 例如，若要建立的檢視*有關*動作*首頁*控制站，您就必須建立*About.cshtml*檔案  */檢視表/首頁*資料夾。

範例的檢視檔案 (*About.cshtml*):

[!code-html[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

*Razor*程式碼由表示`@`符號。 C# 陳述式會執行的 Razor 程式碼區塊，已設定 off 大括號內 (`{` `}`)，例如指派 [關於] 以`ViewData["Title"]`如上所示的項目。 Razor 可以用來顯示值在 HTML 內只參考的值與`@`符號，如中所示`<h2>`和`<h3>`上述項目。

此檢視會著重於只輸出，它是負責的部分。 其餘的頁面配置和其他常用的部分檢視的其他地方指定。 深入了解[配置和共用的檢視邏輯](layout.md)。

## <a name="how-do-controllers-specify-views"></a>控制器指定檢視如何？

檢視通常會傳回動作為`ViewResult`。 動作方法可以建立並傳回`ViewResult`直接，但是通常如果您的控制器是繼承自`Controller`，只要您將使用`View`helper 方法與這個範例示範：

*HomeController.cs*

[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

`View` Helper 方法擁有數個多載來傳回檢視表更輕鬆地進行應用程式開發人員。 您可以選擇性地指定要傳回的檢視，以及要傳遞至檢視的模型物件。

這個動作會傳回*About.cshtml*呈現檢視上面所示：

![有關頁面](overview/_static/about-page.png)

### <a name="view-discovery"></a>檢視探索

當動作傳回的檢視時，這個程序稱為*檢視探索*進行。 此程序會決定將使用的檢視檔案。 除非指定了特定的檢視檔案，則執行階段會尋找控制器特定檢視第一次，則會尋找相符的檢視名稱中*共用*資料夾。

當動作傳回`View`方法，就像這樣`return View();`，動作名稱當做檢視表名稱。 例如，如果這從名為"Index"的動作方法呼叫，將相當於 「 索引 」 的檢視名稱傳入。 檢視名稱可以明確地傳遞給方法 (`return View("SomeView");`)。 在這兩種情況下，檢視探索會搜尋相符的檢視檔案中：

   1. 檢視 /<ControllerName>/<ViewName>.cshtml

   2. 檢視/共用/<ViewName>.cshtml

>[!TIP]
> 我們建議您遵循的慣例只傳回`View()`從可能的因為它會造成更具彈性且更容易重構程式碼時的動作。

可以提供的檢視檔案的路徑，而不是檢視表名稱。 在此情況下， *.cshtml*延伸模組必須指定檔案路徑的一部分。 路徑必須是相對於應用程式根目錄 (並可以選擇性地啟動與"/"或"~ /")。 例如：`return View("Views/Home/About.cshtml");`

> [!NOTE]
> [部分檢視](partial.md)和[檢視元件](view-components.md)使用類似 （但不是完全相同） 的探索機制。

> [!NOTE]
> 您可以自訂有關檢視的所在位置的應用程式中使用自訂的預設慣例`IViewLocationExpander`。

>[!TIP]
> 可能會區分大小寫的基礎檔案系統根據檢視名稱。 作業系統之間的相容性，永遠符合控制器和動作名稱與相關聯的檢視資料夾和檔案名稱之間的大小寫。

## <a name="passing-data-to-views"></a>將資料傳遞至檢視

您可以將資料傳遞至檢視使用數種機制。 最健全的作法是指定*模型*檢視中的類型 (通常稱為*viewmodel*，若要在區別商務網域模型類型)，然後將此類型的執行個體傳遞至檢視從動作。 我們建議您將資料傳遞至檢視使用模型或檢視模型。 這可讓檢視以充分利用強式型別檢查。 您可以指定的模型檢視，使用`@model`指示詞：

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1]}} -->

```html
@model WebApplication1.ViewModels.Address
   <h2>Contact</h2>
   <address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

一旦模型已指定的檢視，傳送至檢視執行個體可以存取在強類型的方式，使用`@Model`如上所示。 若要提供檢視的模型類型的執行個體，控制器會將它做為參數：

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

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

沒有任何限制可供檢視，以做為模型的型別。 我們建議您傳遞純舊 CLR 物件 (POCO) 檢視模型少甚至沒有行為，以便可以其他地方封裝商務邏輯，在應用程式。 舉例來說，這種方法是*位址*viewmodel 上述範例中使用：

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

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
> 不會讓您為您的商務模型類型和顯示的模型型別使用相同的類別。 不過，使它們保持在個別可讓您檢視，以獨立會因您的網域或持續性的模型，並提供一些安全性優點 (針對使用者會將傳送至應用程式使用的模型[模型繫結](../models/model-binding.md))。

### <a name="loosely-typed-data"></a>鬆散型別的資料

除了強型別檢視中，所有檢視都可以存取資料的鬆散型別集合。 這個相同的集合，可透過參考`ViewData`或`ViewBag`控制器和檢視上的屬性。 `ViewBag`屬性是周圍的包裝函式`ViewData`，該集合上提供的動態檢視。 它不是個別的集合。

`ViewData`一個字典物件存取透過`string`索引鍵。 您可以儲存和擷取中的物件，必須先將它們轉換成特定類型，當您擷取它們。 您可以使用`ViewData`至控制器中的資料傳遞至檢視，以及檢視 （和部分檢視和配置） 內。 字串資料可以儲存並直接使用，而不需要轉型。

設定某些值`ViewData`動作：

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

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3, 6]}} -->

```html
@{
       // Requires cast
       var address = ViewData["Address"] as Address;
   }

   @ViewData["Greeting"] World!

   <address>
       @address.Name<br />
       @address.Street<br />
       @address.City, @address.State @address.PostalCode
   </address>
   ```

`ViewBag`物件提供儲存在物件的動態存取`ViewData`。 這可以是更方便使用，因為它不需要進行轉型。 上面使用的相同範例`ViewBag`而不是強型別`address`檢視中的執行個體：

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1, 4, 5, 6]}} -->

```html
@ViewBag.Greeting World!

   <address>
       @ViewBag.Address.Name<br />
       @ViewBag.Address.Street<br />
       @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
   </address>
   ```

> [!NOTE]
> 因為都會指向相同的基礎`ViewData`集合，您可以混合和比對之間`ViewData`和`ViewBag`讀取和寫入的值，如果方便時。

### <a name="dynamic-views"></a>動態檢視

檢視未宣告的模型型別，而沒有傳遞給它們的模型執行個體可以動態地參考這個執行個體。 例如，如果執行個體`Address`傳遞至檢視，不會宣告`@model`，檢視就仍然可以動態示參考執行個體的屬性：

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [13, 16, 17, 18]}} -->

```html
<address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

此功能可提供一些彈性，但不是提供任何編譯保護或 IntelliSense。 如果屬性不存在，頁面將會在執行階段失敗。

## <a name="more-view-features"></a>詳細的檢視功能

[標記協助程式](tag-helpers/intro.md)讓您輕鬆將伺服器端行為加入至現有的 HTML 標記，避免需要使用自訂程式碼或檢視內的協助程式。 標記協助程式做為屬性套用至 HTML 項目，則會忽略不熟悉，允許編輯和各種不同的工具所呈現的檢視標記的編輯器。 標記協助程式有許多用途，而且特別是可以[使用表單](working-with-forms.md)容易得多。

產生自訂的 HTML 標記可達到許多內建 HTML Helper 和更複雜的 UI 邏輯 （可能具備它自己的資料需求） 可以封裝在[檢視元件](view-components.md)。 檢視元件提供相同重要性分離，控制器和檢視提供，並可免除動作和檢視表的需要來處理常見的 UI 項目所使用的資料。

如同許多其他 ASP.NET Core 方面，檢視支援[相依性插入](../../fundamentals/dependency-injection.md)，讓服務變成[插入檢視](dependency-injection.md)。
