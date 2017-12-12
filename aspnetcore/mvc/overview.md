---
title: "ASP.NET Core MVC 概觀"
author: ardalis
description: "了解如何 ASP.NET Core MVC 是用於建置 web 應用程式的豐富架構以及應用程式開發介面使用模型檢視控制器設計模式。"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 89af38d1-52e0-4db7-b791-dbce909b0714
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/overview
ms.openlocfilehash: 2492b6aa4602dbbf3b9cd3dca00d40690c640cab
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="overview-of-aspnet-core-mvc"></a>ASP.NET Core MVC 概觀

由[Steve Smith](https://ardalis.com/)

ASP.NET Core MVC 建立 web 應用程式的豐富架構，而且應用程式開發介面使用模型檢視控制器設計模式。

## <a name="what-is-the-mvc-pattern"></a>什麼是 MVC 模式？

模型檢視控制器 (MVC) 架構模式可將三個主要元件的群組，將應用程式： 模型、 檢視和控制器。 這個模式有助於達到[的重要性分離](http://deviq.com/separation-of-concerns/)。 使用此模式，使用者要求會路由至控制器負責使用模型執行使用者動作和 （或） 擷取查詢的結果。 控制器會選擇檢視將顯示給使用者，並提供與它需要的任何模型資料。

下圖顯示三個主要元件，以及哪些參考其他：

![MVC 模式](overview/_static/mvc.png)

此的責任就可協助您擴充應用程式複雜度，因為它是您更輕鬆地編寫、 偵錯和測試的項目 （模型、 檢視或控制站） 的單一作業 (且後面會跟[單一責任原則](http://deviq.com/single-responsibility-principle/)). 它是更難更新、 測試及偵錯程式碼具有相依性跨越兩個或多個三個區域。 例如，使用者介面邏輯通常會比商務邏輯經常變更。 如果呈現程式碼和商務邏輯結合在單一物件中時，您必須修改包含商務邏輯，每當您變更使用者介面的物件。 這是可能會造成錯誤，而且需要重新測試所有的商務邏輯的每個的簡潔使用者介面變更之後。

> [!NOTE]
> 檢視和控制器取決於模型。 不過，模型會取決於檢視都控制器。 這是其中一個分隔的索引鍵的優點。 此分隔可讓您的視覺呈現獨立建置和測試模型。

### <a name="model-responsibilities"></a>模型的責任

在 MVC 應用程式中的模型代表應用程式和任何的商務邏輯或應該由它來執行作業的狀態。 商務邏輯應該封裝在模型中，以及任何實作邏輯，來保存應用程式的狀態。 強型別檢視通常會顯示於該檢視; 使用 ViewModel 類型特別設計來包含的資料控制站會建立和填入這些 ViewModel 來自執行個體模型。

> [!NOTE]
> 有許多方式來組織中使用 MVC 架構模式的應用程式的模型。 深入了解一些[不同種類的模型型別](http://deviq.com/kinds-of-models/)。

### <a name="view-responsibilities"></a>檢視的責任

檢視必須負責透過使用者介面中呈現內容。 他們使用[Razor 檢視引擎](#razor-view-engine)HTML 標記中內嵌的.NET 程式碼。 應該在檢視內的最小邏輯，以及它們中的任何邏輯應該與關聯呈現內容。 如果您發現需要執行更多的邏輯檢視中的檔案以顯示複雜模型中的資料，請考慮使用[檢視元件](views/view-components.md)，ViewModel，或檢視表範本來簡化檢視。

### <a name="controller-responsibilities"></a>控制器的責任

控制器會處理使用者互動、 處理模型，並且在最後選擇要呈現檢視的元件。 MVC 應用程式中，檢視只能顯示資訊。控制器會處理和回應使用者輸入和互動。 在 MVC 模式中，控制器初始的進入點，而且會負責選取哪一個模型類型來處理和轉譯的檢視 (它可控制其名稱-因此應用程式的給定要求的回應方式)。

> [!NOTE]
> 控制站應該不能過於複雜太多的責任。 若要防止控制器邏輯變得過於複雜，請使用[單一責任原則](http://deviq.com/single-responsibility-principle/)從控制站移至網域模型的推播的商務邏輯。

>[!TIP]
> 如果您發現控制器動作經常執行的動作類型，您可以依照[不要重複自行原則](http://deviq.com/don-t-repeat-yourself/)移動這些常見的動作，藉以[篩選](#filters)。

## <a name="what-is-aspnet-core-mvc"></a>ASP.NET Core MVC 是什麼

ASP.NET Core MVC 架構是輕量型、 開放來源時，可高度測試展示架構針對 ASP.NET Core 與使用方式最佳化。

ASP.NET Core MVC 提供模式為準的方式建置動態網站，可讓清楚的重要性分離。 它可讓您完全掌控標記，支援 tdd 的開發，並使用最新 web 標準。

## <a name="features"></a>功能

ASP.NET Core MVC 包括下列各項：

* [路由傳送](#routing)
* [模型繫結](#model-binding)
* [模型驗證](#model-validation)
* [相依性插入](../fundamentals/dependency-injection.md)
* [篩選](#filters)
* [區域](#areas)
* [Web 應用程式開發介面](#web-apis)
* [可測試性](#testability)
* [Razor 檢視引擎](#razor-view-engine)
* [強型別的檢視](#strongly-typed-views)
* [標記協助程式](#tag-helpers)
* [檢視元件](#view-components)

### <a name="routing"></a>路由

最上層的建置 ASP.NET Core MVC [ASP.NET Core 路由](../fundamentals/routing.md)、 功能強大的 URL 對映元件，可讓您建置包含易於且可搜尋的 Url 的應用程式。 這可讓您定義您的應用程式 URL 命名模式，適用於搜尋引擎最佳化 (SEO) 和連結的產生，而不考慮如何組織您的網頁伺服器上的檔案。 您可以定義您的路由使用方便的路由範本語法支援路由值條件約束、 預設值和選擇性的值。

*慣例為基礎的路由*可讓您全域定義的 URL 格式，在您的應用程式接受和每個格式的方式會對應至特定的動作方法上指定的控制器。 收到的連入要求時，路由引擎會剖析 URL 和符合以其中一種已定義的 URL 格式，然後再呼叫相關聯的控制器動作方法。

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

*屬性路由*可讓您指定的路由資訊，而將您的控制器和動作以定義您的應用程式路由的屬性。 這表示路由定義會緊控制器和動作與它們相關聯。

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
  [HttpGet("{id}")]
  public IActionResult GetProduct(int id)
  {
    ...
  }
}
```

### <a name="model-binding"></a>模型繫結

ASP.NET Core MVC[模型繫結](models/model-binding.md)將表單值、 路由資料、 查詢字串參數 （HTTP 標頭），用戶端要求資料轉換為控制器可以處理的物件。 如此一來，控制器邏輯不需要執行工作的理解內送的要求資料。它只會有做為它的動作方法參數的資料。

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a>模型驗證

ASP.NET Core MVC 支援[驗證](models/validation.md)而將您的模型物件與資料註解驗證屬性。 驗證屬性值在公佈到伺服器之前，先檢查用戶端，以及呼叫控制器動作之前，伺服器上。

```csharp
using System.ComponentModel.DataAnnotations;
public class LoginViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Display(Name = "Remember me?")]
    public bool RememberMe { get; set; }
}
```

控制器的動作：

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // If we got this far, something failed, redisplay form
    return View(model);
}
```

架構會處理驗證要求資料在用戶端和伺服器上。 模型類型上指定的驗證邏輯加入至不顯眼的附註形式呈現的檢視，並強制執行瀏覽器中使用[jQuery 驗證](https://jqueryvalidation.org/)。

### <a name="dependency-injection"></a>相依性插入

ASP.NET Core 已內建支援[相依性插入 (DI)](../fundamentals/dependency-injection.md)。 在 ASP.NET Core MVC 中，[控制器](controllers/dependency-injection.md)可以要求所需服務透過其建構函式，讓他們遵循[明確的相依性原則](http://deviq.com/explicit-dependencies-principle/)。

您的應用程式也可以使用[檢視中檔案的相依性插入](views/dependency-injection.md)，並使用`@inject`指示詞：

```cshtml
@inject SomeService ServiceName
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ServiceName.GetTitle</title>
</head>
<body>
    <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

### <a name="filters"></a>篩選條件

[篩選](controllers/filters.md)協助開發人員封裝跨領域的考量，例如例外狀況處理或授權。 篩選會啟用動作方法執行自訂的前置和後置處理邏輯，並可設定在給定的要求執行管線內的特定點上執行。 篩選條件可以套用至控制器或動作做為屬性 （或可以全域執行）。 多個篩選器 (例如`Authorize`) 隨附的架構。


```csharp
[Authorize]
   public class AccountController : Controller
   {
```

### <a name="areas"></a>區域

[區域](controllers/areas.md)提供資料的大型 ASP.NET Core MVC Web 應用程式分割成較小功能群組的方式。 一個區域就是應用程式中的一個 MVC 結構。 在 MVC 專案中，邏輯元件，例如模型、 控制器和檢視保留在不同的資料夾，而且若要建立這些元件之間的關聯性則 MVC 會使用命名慣例。 大型應用程式中，它可能比較有利分割成個別高功能層級區域的 應用程式。 例如，電子商務應用程式與多個業務單位，例如簽出、 帳單和搜尋等。每個這些單位具有自己的邏輯元件檢視、 控制器、 和模型。

### <a name="web-apis"></a>Web API

正在建立網站的絕佳平台，除了 ASP.NET Core MVC 會有絕佳的支援，來建置 Web 應用程式開發介面。 您可以建立服務，可達到廣泛的用戶端包括瀏覽器和行動裝置。

此架構包含支援的內建支援的 HTTP 內容交涉[格式化資料](models/formatting.md)為 JSON 或 XML。 寫入[自訂格式器](advanced/custom-formatters.md)加入自己的格式支援。

用於啟用支援超連結產生。 輕鬆啟用支援[跨原始資源共用 (CORS)](http://www.w3.org/TR/cors/) ，讓您的 Web Api 可以橫跨多個 Web 應用程式。

### <a name="testability"></a>可測試性

架構的使用介面和相依性插入會導致它適用於單元測試，並架構包含功能 （例如 Entity Framework TestHost 和 InMemory 提供者），使[整合測試](../testing/integration-testing.md)快速簡便也。 深入了解[測試控制器邏輯](controllers/testing.md)。

### <a name="razor-view-engine"></a>Razor 檢視引擎

[ASP.NET Core MVC 檢視](views/overview.md)使用[Razor 檢視引擎](views/razor.md)來呈現檢視。 Razor 是壓縮、 易懂和流暢的範本標記語言來定義使用內嵌的 C# 程式碼的檢視。 Razor 用於動態產生的 web 內容伺服器上。 您完全可以混合伺服端程式碼使用用戶端端內容和作業程式碼。

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

使用 Razor 檢視引擎，您可以定義[配置](views/layout.md)，[部分檢視](views/partial.md)和可取代的區段。

### <a name="strongly-typed-views"></a>強型別的檢視

在 MVC razor 檢視可以是強型別根據您的模型。 控制站可以強型別的模型傳遞至檢視，讓您檢視，以具有型別檢查和 IntelliSense 支援。

例如，下列檢視定義的模型型別`IEnumerable<Product>`:

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a>標記協助程式

[標記協助程式](views/tag-helpers/intro.md)啟用建立和呈現 HTML Razor 檔案中的項目加入伺服器端程式碼。 您可用來定義自訂標籤標記協助程式 (例如， `<environment>`) 或修改現有的標籤行為 (例如， `<label>`)。 標記協助程式繫結至特定項目名稱和其屬性為基礎的項目。 它們提供伺服器端轉譯，同時仍然保留 HTML 編輯經驗的優點。

有許多內建的標記協助程式的一般工作-例如建立表單、 連結、 載入資產和更多的和更多可用公用 GitHub 儲存機制中以及 NuGet 封裝。 標記協助程式以 C# 中，撰寫，與它們目標項目名稱、 屬性名稱，或父標記的 HTML 項目。 例如，內建 LinkTagHelper 可以用來建立的連結`Login`動作`AccountsController`:

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

`EnvironmentTagHelper`可用來在您檢視 （例如 raw 或縮短） 為基礎的執行階段環境，例如開發、 預備或生產環境中包含不同的指令碼：

```cshtml
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.1.4.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery">
    </script>
</environment>
```

標記協助程式提供 HTML 易用的開發經驗和豐富的 IntelliSense 環境，以建立 HTML 和 Razor 標記。 大部分的內建的標記協助程式和目標現有的 HTML 元素的元素提供伺服器端的屬性。

### <a name="view-components"></a>檢視元件

[檢視元件](views/view-components.md)可讓您封裝呈現邏輯和整個應用程式中重複加以使用。 它們類似於[部分檢視](views/partial.md)，但具有相關聯的邏輯。
