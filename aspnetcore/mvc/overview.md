---
title: ASP.NET Core MVC 概觀
author: ardalis
description: 了解 ASP.NET Core MVC 何以是建置使用模型檢視控制器設計模式之 Web 應用程式和 API 的豐富架構。
ms.author: riande
ms.date: 02/12/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: mvc/overview
ms.openlocfilehash: ae382feb152f490e46df969887401060965d8c4e
ms.sourcegitcommit: 6a71b560d897e13ad5b61d07afe4fcb57f8ef6dc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/09/2020
ms.locfileid: "84106529"
---
# <a name="overview-of-aspnet-core-mvc"></a>ASP.NET Core MVC 概觀

作者：[Steve Smith](https://ardalis.com/)

ASP.NET Core MVC 是建置使用模型檢視控制器設計模式之 Web 應用程式和 API 的豐富架構。

## <a name="what-is-the-mvc-pattern"></a>什麼是 MVC 模式？

模型檢視控制器 (MVC) 架構模式可將一個應用程式劃分成三組主要元件：模型、檢視和控制器。 此模式有助於 [Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) (關注點分離)。 透過此模式，使用者要求會路由傳送至控制器，再由其負責使用模型來執行使用者動作及 (或) 擷取查詢結果。 控制器會選擇要向使用者顯示的檢視，並在其中提供任何所需的模型資料。

下圖顯示三個主要元件及彼此的參考關係：

![MVC 模式](overview/_static/mvc.png)

此職責劃分有助於您根據複雜度來調整應用程式，因為如果模型、檢視或控制器只有一項作業，就會更容易撰寫程式碼、進行偵錯及測試。 如果程式碼相依於這三個區域當中兩個以上，就很難進行更新、測試及偵錯。 例如，使用者介面邏輯通常比商務邏輯更常變更。 如果將展示程式碼和商務邏輯結合成一個物件，則每次使用者介面變更時，都必須修改含有商務邏輯的物件。 這通常會導致錯誤，而需要在每次使用者介面微幅變更之後重新測試商務邏輯。

> [!NOTE]
> 檢視和控制器都相依於模型。 不過，模型並不相依於檢視或控制器。 這是分離的主要優點之一。 此分離可讓模型在建立與測試時獨立於視覺展示。

### <a name="model-responsibilities"></a>模型職責

MVC 應用程式中的模型代表應用程式的狀態，以及應用程式應該執行的任何商務邏輯或作業。 商務邏輯應該與保存應用程式狀態的任何實作邏輯，一起封裝在模型中。 強型別檢視通常會使用 ViewModel 類型，其設計目的是為了包含要在該檢視上顯示的資料。 控制器會從模型建立並填入這些 ViewModel 執行個體。

### <a name="view-responsibilities"></a>檢視職責

檢視會負責透過使用者介面展示內容。 他們會使用[ Razor view engine](#razor-view-engine) ，在 HTML 標籤中內嵌 .net 程式碼。 檢視內應該有基本邏輯，而且其中的任何邏輯都應該與展示內容相關。 如果您需要在檢視檔案中執行大量邏輯以便顯示複雜模型中的資料，請考慮使用[檢視元件](views/view-components.md)、ViewModel 或檢視範本來簡化檢視。

### <a name="controller-responsibilities"></a>控制器職責

控制器是處理使用者互動、使用模型，並在最終選取要呈現之檢視的元件。 在 MVC 應用程式中，檢視只會顯示資訊，而控制器則會處理及回應使用者輸入和互動。 在 MVC 模式中，控制器是初始進入點，負責選取要使用的模型類型及要呈現的檢視 (如其名稱所指，它會控制應用程式回應指定要求的方式)。

> [!NOTE]
> 控制器不應該因太多職責而過於複雜。 若要防止控制器邏輯變得過於複雜，請將控制器中的商務邏輯推送到領域模型中。

>[!TIP]
> 如果您發現控制器動作經常執行相同的動作類型，請這些常見的動作移到[篩選](#filters)中。

## <a name="what-is-aspnet-core-mvc"></a>什麼是 ASP.NET Core MVC

ASP.NET Core MVC 架構是輕量型、開放原始碼且可高度測試的展示架構，並已經過最佳化可搭配 ASP.NET Core 使用。

ASP.NET Core MVC 可讓您透過模式建立動態網站，以清楚關注點分離。 它可讓您完全掌控標記，支援適合 TDD 的開發，並使用最新的網站標準。

## <a name="features"></a>特性

ASP.NET Core MVC 包括下列各項：

* [路由](#routing)
* [模型繫結](#model-binding)
* [模型驗證](#model-validation)
* [相依性插入](../fundamentals/dependency-injection.md)
* [篩選器](#filters)
* [區域](#areas)
* [Web API](#web-apis)
* [可測試性](#testability)
* [Razor視圖引擎](#razor-view-engine)
* [強型別視圖](#strongly-typed-views)
* [標籤協助程式](#tag-helpers)
* [視圖元件](#view-components)

### <a name="routing"></a>路由

ASP.NET Core MVC 是以 [ASP.NET Core 路由](../fundamentals/routing.md)為建置基礎且功能強大的 URL 對應元件，可讓您建置具有可理解且可搜尋之 URL 的應用程式。 這可讓您定義適用於搜尋引擎最佳化 (SEO) 和連結產生的應用程式 URL 命名模式，而不需要考慮如何在網頁伺服器上組織檔案。 您可以使用方便且支援路由值條件約束、預設值和選用值的路由範本語法，來定義您的路由。

「以慣例為基礎的路由」** 可讓您全域定義應用程式接受的的 URL 格式，以及每種格式如何對應至指定控制器上的特定動作方法。 當收到內送要求時，路由引擎會剖析 URL 定並將它對應至其中一個已定義的 URL 格式，再呼叫關聯控制器的動作方法。

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

「屬性路由」** 可讓您指定路由資訊，方法是使用定義應用程式路由的屬性來裝飾控制器和動作。 這表示路由定義會緊接著其所關聯的控制器和動作。

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

ASP.NET Core MVC [模型繫結](models/model-binding.md)會將用戶端要求資料 (表單值、路由資料、查詢字串參數、HTTP 標頭)，轉換成控制器可以處理的物件。 因此，您的控制器邏輯不必理解內送要求資料，它只會將資料當作其動作方法的參數。

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
```

### <a name="model-validation"></a>模型驗證

ASP.NET Core MVC 支援使用資料註解驗證屬性來裝置模型物件，藉此進行[驗證](models/validation.md)。 這些驗證屬性會在用戶端進行檢查，再將其值張貼至伺服器，而且會在伺服器上進行檢查，再呼叫控制器動作。

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

控制器動作：

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // At this point, something failed, redisplay form
    return View(model);
}
```

此架構會在用戶端和伺服器上處理驗證要求資料。 模型類型上所指定的驗證邏輯會以不顯眼的註解形式新增呈現的檢視，並使用 [jQuery 驗證](https://jqueryvalidation.org/)在瀏覽器中強制執行。

### <a name="dependency-injection"></a>相依性插入

ASP.NET Core 內建[相依性插入 (DI)](../fundamentals/dependency-injection.md) 支援。 在 ASP.NET Core MVC 中，[控制器](controllers/dependency-injection.md)可以透過其建構函式要求所需服務，以便遵循 [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) (明確的相依性原則)。

您的應用程式也可以使用 `@inject` 指示詞，[在檢視檔案中使用相依性插入](views/dependency-injection.md)：

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

### <a name="filters"></a>篩選器

[篩選](controllers/filters.md)可協助開發人員封裝交叉關注，例如例外狀況處理或授權。 篩選可執行動作方法的自訂處理前後邏輯，並可設定為在指定要求之執行管線內的特定時間點執行。 篩選可當作屬性套用至控制器或動作 (也可全域執行)。 此架構隨附數個篩選 (例如`Authorize`)。 `[Authorize]` 是用來建立 MVC 授權篩選的屬性。

```csharp
[Authorize]
public class AccountController : Controller
```

### <a name="areas"></a>區域

[區域](controllers/areas.md)提供將大型 ASP.NET Core MVC Web 應用程式分割成較小功能群組的方式。 一個區域是應用程式內的一個 MVC 結構。 在 MVC 專案中，模型、控制器和檢視等邏輯元件會保留在不同的資料夾中，而且 MVC 會使用命名慣例來建立這些元件之間的關聯性。 針對大型應用程式，將應用程式分割成個別高功能層級區域可能較有利。 例如，具有多個業務單位的電子商務應用程式，例如結帳、計費和搜尋等。每個單位都有自己的邏輯元件視圖、控制器和模型。

### <a name="web-apis"></a>Web API

ASP.NET Core MVC 除了是建立網站的理想平台之外，也對建置 Web API 提供絕佳的支援。 您可以建置可供廣大用戶端使用的服務，包括瀏覽器和行動裝置。

此架構包含使用內建支援的 HTTP 內容交涉支援，以[格式化資料](xref:web-api/advanced/formatting)為 JSON 或 XML。 撰寫[自訂格式器](xref:web-api/advanced/custom-formatters)可新增您專屬格式的支援。

使用連結產生可支援超媒體。 輕鬆就可支援[跨原始來源資源共用 (CORS)](https://www.w3.org/TR/cors/) ，讓您的 Web API 可跨多個 Web 應用程式共用。

### <a name="testability"></a>可測試性

此架構使用介面和相依性插入，因此相當適用於單元測試，而此架構所包含的功能 (例如 Entity Framework 的 TestHost 和 InMemory 提供者) 也讓您可以輕鬆快速地進行[整合測試](xref:test/integration-tests)。 深入了解[如何測試控制器邏輯](controllers/testing.md)。

### <a name="razor-view-engine"></a>Razor視圖引擎

[ASP.NET CORE MVC views](views/overview.md)會使用[ Razor view engine](views/razor.md)來呈現 views。 Razor是一種精簡、表達和流暢的範本標記語言，可使用內嵌的 c # 程式碼來定義視圖。 Razor是用來在伺服器上動態產生 web 內容。 您可以完全混合伺服端程式碼以及用戶端內容和程式碼。

```cshtml
<ul>
    @for (int i = 0; i < 5; i++) {
        <li>List item @i</li>
    }
</ul>
```

Razor您可以使用 view engine 來定義[版面](views/layout.md)配置、[部分視圖](views/partial.md)和可取代的區段。

### <a name="strongly-typed-views"></a>強型別檢視

RazorMVC 中的 views 可以根據您的模型來進行強型別。 控制器可以將強型別模型傳遞至檢視，讓您的檢視具有類型檢查和 IntelliSense 支援。

例如，下列檢視會呈現 `IEnumerable<Product>` 類型的模型：

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a>標籤協助程式

[標記](views/tag-helpers/intro.md)協助程式可讓伺服器端程式碼參與建立和轉譯檔案中的 HTML 元素 Razor 。 您可以使用標籤協助程式定義自訂標籤 (例如 `<environment>`)，或修改現有標籤 (例如 `<label>`) 的行為。 標籤協助程式會根據元素名稱及其屬性，繫結至特定元素。 其提供伺服器端轉譯優點，同時仍然保留 HTML 編輯體驗。

有許多適用於一般工作 (例如建立表單和連結、載入資產等) 的內建標籤協助程式，還有更多位於公用 GitHub 存放庫及作為 NuGet 套件來提供。 標籤協助程式是以 C# 編寫，並根據項目名稱、屬性名稱或上層標籤來設定目標 HTML 項目。 例如，內建 LinkTagHelper 可用來建立 `AccountsController` 之 `Login` 動作的連結：

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

`EnvironmentTagHelper` 可根據執行階段環境 (例如開發、預備或生產)，將不同的指令碼加入檢視：

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

標籤協助程式提供 HTML 易懂的開發體驗，以及豐富的 IntelliSense 環境來建立 HTML 和 Razor 標記。 大部分的內建標籤協助程式都是以現有的 HTML 元素為目標，並提供元素的伺服器端屬性。

### <a name="view-components"></a>檢視元件

[檢視元件](views/view-components.md)可讓您封裝轉譯邏輯，並在整個應用程式中重複使用。 其類似於[部分檢視](views/partial.md)，但具有相關聯的邏輯。

## <a name="compatibility-version"></a>相容性版本

<xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> 方法可讓應用程式加入或退出 ASP.NET Core MVC 2.1 或更新版本所引入的可能重大行為變更。

如需詳細資訊，請參閱 <xref:mvc/compatibility-version> 。

## <a name="additional-resources"></a>其他資源

* 適用于 ASP.NET Core MVC：強型別單元測試連結[庫的 MyTested AspNetCore](https://github.com/ivaylokenov/MyTested.AspNetCore.Mvc)，提供流暢的介面來測試 mvc 和 Web API 應用程式。 （*不是由 Microsoft 維護或支援）。*
* <xref:blazor/integrate-components>
