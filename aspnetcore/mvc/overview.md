---
title: ASP.NET Core MVC 概觀
author: ardalis
description: 了解 ASP.NET Core MVC 何以是建置使用模型檢視控制器設計模式之 Web 應用程式和 API 的豐富架構。
ms.author: riande
ms.date: 01/28/2020
uid: mvc/overview
ms.openlocfilehash: a0d1e364bf4cda4ad30c5070c9e61e6972759bb0
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2020
ms.locfileid: "77171817"
---
# <a name="overview-of-aspnet-core-mvc"></a><span data-ttu-id="da1ea-103">ASP.NET Core MVC 概觀</span><span class="sxs-lookup"><span data-stu-id="da1ea-103">Overview of ASP.NET Core MVC</span></span>

<span data-ttu-id="da1ea-104">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="da1ea-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="da1ea-105">ASP.NET Core MVC 是建置使用模型檢視控制器設計模式之 Web 應用程式和 API 的豐富架構。</span><span class="sxs-lookup"><span data-stu-id="da1ea-105">ASP.NET Core MVC is a rich framework for building web apps and APIs using the Model-View-Controller design pattern.</span></span>

## <a name="what-is-the-mvc-pattern"></a><span data-ttu-id="da1ea-106">什麼是 MVC 模式？</span><span class="sxs-lookup"><span data-stu-id="da1ea-106">What is the MVC pattern?</span></span>

<span data-ttu-id="da1ea-107">模型檢視控制器 (MVC) 架構模式可將一個應用程式劃分成三組主要元件：模型、檢視和控制器。</span><span class="sxs-lookup"><span data-stu-id="da1ea-107">The Model-View-Controller (MVC) architectural pattern separates an application into three main groups of components: Models, Views, and Controllers.</span></span> <span data-ttu-id="da1ea-108">此模式有助於 [Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) (關注點分離)。</span><span class="sxs-lookup"><span data-stu-id="da1ea-108">This pattern helps to achieve [separation of concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span> <span data-ttu-id="da1ea-109">透過此模式，使用者要求會路由傳送至控制器，再由其負責使用模型來執行使用者動作及 (或) 擷取查詢結果。</span><span class="sxs-lookup"><span data-stu-id="da1ea-109">Using this pattern, user requests are routed to a Controller which is responsible for working with the Model to perform user actions and/or retrieve results of queries.</span></span> <span data-ttu-id="da1ea-110">控制器會選擇要向使用者顯示的檢視，並在其中提供任何所需的模型資料。</span><span class="sxs-lookup"><span data-stu-id="da1ea-110">The Controller chooses the View to display to the user, and provides it with any Model data it requires.</span></span>

<span data-ttu-id="da1ea-111">下圖顯示三個主要元件及彼此的參考關係：</span><span class="sxs-lookup"><span data-stu-id="da1ea-111">The following diagram shows the three main components and which ones reference the others:</span></span>

![MVC 模式](overview/_static/mvc.png)

<span data-ttu-id="da1ea-113">此職責劃分有助於您根據複雜度來調整應用程式，因為如果模型、檢視或控制器只有一項作業，就會更容易撰寫程式碼、進行偵錯及測試。</span><span class="sxs-lookup"><span data-stu-id="da1ea-113">This delineation of responsibilities helps you scale the application in terms of complexity because it's easier to code, debug, and test something (model, view, or controller) that has a single job.</span></span> <span data-ttu-id="da1ea-114">如果程式碼相依於這三個區域當中兩個以上，就很難進行更新、測試及偵錯。</span><span class="sxs-lookup"><span data-stu-id="da1ea-114">It's more difficult to update, test, and debug code that has dependencies spread across two or more of these three areas.</span></span> <span data-ttu-id="da1ea-115">例如，使用者介面邏輯通常比商務邏輯更常變更。</span><span class="sxs-lookup"><span data-stu-id="da1ea-115">For example, user interface logic tends to change more frequently than business logic.</span></span> <span data-ttu-id="da1ea-116">如果將展示程式碼和商務邏輯結合成一個物件，則每次使用者介面變更時，都必須修改含有商務邏輯的物件。</span><span class="sxs-lookup"><span data-stu-id="da1ea-116">If presentation code and business logic are combined in a single object, an object containing business logic must be modified every time the user interface is changed.</span></span> <span data-ttu-id="da1ea-117">這通常會導致錯誤，而需要在每次使用者介面微幅變更之後重新測試商務邏輯。</span><span class="sxs-lookup"><span data-stu-id="da1ea-117">This often introduces errors and requires the retesting of business logic after every minimal user interface change.</span></span>

> [!NOTE]
> <span data-ttu-id="da1ea-118">檢視和控制器都相依於模型。</span><span class="sxs-lookup"><span data-stu-id="da1ea-118">Both the view and the controller depend on the model.</span></span> <span data-ttu-id="da1ea-119">不過，模型並不相依於檢視或控制器。</span><span class="sxs-lookup"><span data-stu-id="da1ea-119">However, the model depends on neither the view nor the controller.</span></span> <span data-ttu-id="da1ea-120">這是分離的主要優點之一。</span><span class="sxs-lookup"><span data-stu-id="da1ea-120">This is one of the key benefits of the separation.</span></span> <span data-ttu-id="da1ea-121">此分離可讓模型在建立與測試時獨立於視覺展示。</span><span class="sxs-lookup"><span data-stu-id="da1ea-121">This separation allows the model to be built and tested independent of the visual presentation.</span></span>

### <a name="model-responsibilities"></a><span data-ttu-id="da1ea-122">模型職責</span><span class="sxs-lookup"><span data-stu-id="da1ea-122">Model Responsibilities</span></span>

<span data-ttu-id="da1ea-123">MVC 應用程式中的模型代表應用程式的狀態，以及應用程式應該執行的任何商務邏輯或作業。</span><span class="sxs-lookup"><span data-stu-id="da1ea-123">The Model in an MVC application represents the state of the application and any business logic or operations that should be performed by it.</span></span> <span data-ttu-id="da1ea-124">商務邏輯應該與保存應用程式狀態的任何實作邏輯，一起封裝在模型中。</span><span class="sxs-lookup"><span data-stu-id="da1ea-124">Business logic should be encapsulated in the model, along with any implementation logic for persisting the state of the application.</span></span> <span data-ttu-id="da1ea-125">強型別檢視通常會使用 ViewModel 類型，其設計目的是為了包含要在該檢視上顯示的資料。</span><span class="sxs-lookup"><span data-stu-id="da1ea-125">Strongly-typed views typically use ViewModel types designed to contain the data to display on that view.</span></span> <span data-ttu-id="da1ea-126">控制器會從模型建立並填入這些 ViewModel 執行個體。</span><span class="sxs-lookup"><span data-stu-id="da1ea-126">The controller creates and populates these ViewModel instances from the model.</span></span>

### <a name="view-responsibilities"></a><span data-ttu-id="da1ea-127">檢視職責</span><span class="sxs-lookup"><span data-stu-id="da1ea-127">View Responsibilities</span></span>

<span data-ttu-id="da1ea-128">檢視會負責透過使用者介面展示內容。</span><span class="sxs-lookup"><span data-stu-id="da1ea-128">Views are responsible for presenting content through the user interface.</span></span> <span data-ttu-id="da1ea-129">其使用 [Razor 檢視引擎](#razor-view-engine)將 .NET 程式碼內嵌於 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="da1ea-129">They use the [Razor view engine](#razor-view-engine) to embed .NET code in HTML markup.</span></span> <span data-ttu-id="da1ea-130">檢視內應該有基本邏輯，而且其中的任何邏輯都應該與展示內容相關。</span><span class="sxs-lookup"><span data-stu-id="da1ea-130">There should be minimal logic within views, and any logic in them should relate to presenting content.</span></span> <span data-ttu-id="da1ea-131">如果您需要在檢視檔案中執行大量邏輯以便顯示複雜模型中的資料，請考慮使用[檢視元件](views/view-components.md)、ViewModel 或檢視範本來簡化檢視。</span><span class="sxs-lookup"><span data-stu-id="da1ea-131">If you find the need to perform a great deal of logic in view files in order to display data from a complex model, consider using a [View Component](views/view-components.md), ViewModel, or view template to simplify the view.</span></span>

### <a name="controller-responsibilities"></a><span data-ttu-id="da1ea-132">控制器職責</span><span class="sxs-lookup"><span data-stu-id="da1ea-132">Controller Responsibilities</span></span>

<span data-ttu-id="da1ea-133">控制器是處理使用者互動、使用模型，並在最終選取要呈現之檢視的元件。</span><span class="sxs-lookup"><span data-stu-id="da1ea-133">Controllers are the components that handle user interaction, work with the model, and ultimately select a view to render.</span></span> <span data-ttu-id="da1ea-134">在 MVC 應用程式中，檢視只會顯示資訊；控制器則會處理和回應使用者輸入和互動。</span><span class="sxs-lookup"><span data-stu-id="da1ea-134">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span> <span data-ttu-id="da1ea-135">在 MVC 模式中，控制器是初始進入點，負責選取要使用的模型類型及要呈現的檢視 (如其名稱所指，它會控制應用程式回應指定要求的方式)。</span><span class="sxs-lookup"><span data-stu-id="da1ea-135">In the MVC pattern, the controller is the initial entry point, and is responsible for selecting which model types to work with and which view to render (hence its name - it controls how the app responds to a given request).</span></span>

> [!NOTE]
> <span data-ttu-id="da1ea-136">控制器不應該因太多職責而過於複雜。</span><span class="sxs-lookup"><span data-stu-id="da1ea-136">Controllers shouldn't be overly complicated by too many responsibilities.</span></span> <span data-ttu-id="da1ea-137">若要防止控制器邏輯變得過於複雜，請將控制器中的商務邏輯推送到領域模型中。</span><span class="sxs-lookup"><span data-stu-id="da1ea-137">To keep controller logic from becoming overly complex, push business logic out of the controller and into the domain model.</span></span>

>[!TIP]
> <span data-ttu-id="da1ea-138">如果您發現控制器動作經常執行相同的動作類型，請這些常見的動作移到[篩選](#filters)中。</span><span class="sxs-lookup"><span data-stu-id="da1ea-138">If you find that your controller actions frequently perform the same kinds of actions, move these common actions into [filters](#filters).</span></span>

## <a name="what-is-aspnet-core-mvc"></a><span data-ttu-id="da1ea-139">什麼是 ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="da1ea-139">What is ASP.NET Core MVC</span></span>

<span data-ttu-id="da1ea-140">ASP.NET Core MVC 架構是輕量型、開放原始碼且可高度測試的展示架構，並已經過最佳化可搭配 ASP.NET Core 使用。</span><span class="sxs-lookup"><span data-stu-id="da1ea-140">The ASP.NET Core MVC framework is a lightweight, open source, highly testable presentation framework optimized for use with ASP.NET Core.</span></span>

<span data-ttu-id="da1ea-141">ASP.NET Core MVC 可讓您透過模式建立動態網站，以清楚關注點分離。</span><span class="sxs-lookup"><span data-stu-id="da1ea-141">ASP.NET Core MVC provides a patterns-based way to build dynamic websites that enables a clean separation of concerns.</span></span> <span data-ttu-id="da1ea-142">它可讓您完全掌控標記，支援適合 TDD 的開發，並使用最新的網站標準。</span><span class="sxs-lookup"><span data-stu-id="da1ea-142">It gives you full control over markup, supports TDD-friendly development and uses the latest web standards.</span></span>

## <a name="features"></a><span data-ttu-id="da1ea-143">特性</span><span class="sxs-lookup"><span data-stu-id="da1ea-143">Features</span></span>

<span data-ttu-id="da1ea-144">ASP.NET Core MVC 包括下列各項：</span><span class="sxs-lookup"><span data-stu-id="da1ea-144">ASP.NET Core MVC includes the following:</span></span>

* [<span data-ttu-id="da1ea-145">路由</span><span class="sxs-lookup"><span data-stu-id="da1ea-145">Routing</span></span>](#routing)
* [<span data-ttu-id="da1ea-146">模型繫結</span><span class="sxs-lookup"><span data-stu-id="da1ea-146">Model binding</span></span>](#model-binding)
* [<span data-ttu-id="da1ea-147">模型驗證</span><span class="sxs-lookup"><span data-stu-id="da1ea-147">Model validation</span></span>](#model-validation)
* [<span data-ttu-id="da1ea-148">相依性插入</span><span class="sxs-lookup"><span data-stu-id="da1ea-148">Dependency injection</span></span>](../fundamentals/dependency-injection.md)
* [<span data-ttu-id="da1ea-149">篩選條件</span><span class="sxs-lookup"><span data-stu-id="da1ea-149">Filters</span></span>](#filters)
* [<span data-ttu-id="da1ea-150">區域</span><span class="sxs-lookup"><span data-stu-id="da1ea-150">Areas</span></span>](#areas)
* [<span data-ttu-id="da1ea-151">Web API</span><span class="sxs-lookup"><span data-stu-id="da1ea-151">Web APIs</span></span>](#web-apis)
* [<span data-ttu-id="da1ea-152">可測試性</span><span class="sxs-lookup"><span data-stu-id="da1ea-152">Testability</span></span>](#testability)
* [<span data-ttu-id="da1ea-153">Razor 檢視引擎</span><span class="sxs-lookup"><span data-stu-id="da1ea-153">Razor view engine</span></span>](#razor-view-engine)
* [<span data-ttu-id="da1ea-154">強型別檢視</span><span class="sxs-lookup"><span data-stu-id="da1ea-154">Strongly typed views</span></span>](#strongly-typed-views)
* [<span data-ttu-id="da1ea-155">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="da1ea-155">Tag Helpers</span></span>](#tag-helpers)
* [<span data-ttu-id="da1ea-156">檢視元件</span><span class="sxs-lookup"><span data-stu-id="da1ea-156">View Components</span></span>](#view-components)

### <a name="routing"></a><span data-ttu-id="da1ea-157">路由</span><span class="sxs-lookup"><span data-stu-id="da1ea-157">Routing</span></span>

<span data-ttu-id="da1ea-158">ASP.NET Core MVC 是以 [ASP.NET Core 路由](../fundamentals/routing.md)為建置基礎且功能強大的 URL 對應元件，可讓您建置具有可理解且可搜尋之 URL 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="da1ea-158">ASP.NET Core MVC is built on top of [ASP.NET Core's routing](../fundamentals/routing.md), a powerful URL-mapping component that lets you build applications that have comprehensible and searchable URLs.</span></span> <span data-ttu-id="da1ea-159">這可讓您定義適用於搜尋引擎最佳化 (SEO) 和連結產生的應用程式 URL 命名模式，而不需要考慮如何在網頁伺服器上組織檔案。</span><span class="sxs-lookup"><span data-stu-id="da1ea-159">This enables you to define your application's URL naming patterns that work well for search engine optimization (SEO) and for link generation, without regard for how the files on your web server are organized.</span></span> <span data-ttu-id="da1ea-160">您可以使用方便且支援路由值條件約束、預設值和選用值的路由範本語法，來定義您的路由。</span><span class="sxs-lookup"><span data-stu-id="da1ea-160">You can define your routes using a convenient route template syntax that supports route value constraints, defaults and optional values.</span></span>

<span data-ttu-id="da1ea-161">「以慣例為基礎的路由」可讓您全域定義應用程式接受的的 URL 格式，以及每種格式如何對應至指定控制器上的特定動作方法。</span><span class="sxs-lookup"><span data-stu-id="da1ea-161">*Convention-based routing* enables you to globally define the URL formats that your application accepts and how each of those formats maps to a specific action method on given controller.</span></span> <span data-ttu-id="da1ea-162">當收到內送要求時，路由引擎會剖析 URL 定並將它對應至其中一個已定義的 URL 格式，再呼叫關聯控制器的動作方法。</span><span class="sxs-lookup"><span data-stu-id="da1ea-162">When an incoming request is received, the routing engine parses the URL and matches it to one of the defined URL formats, and then calls the associated controller's action method.</span></span>

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="da1ea-163">「屬性路由」可讓您指定路由資訊，方法是使用定義應用程式路由的屬性來裝飾控制器和動作。</span><span class="sxs-lookup"><span data-stu-id="da1ea-163">*Attribute routing* enables you to specify routing information by decorating your controllers and actions with attributes that define your application's routes.</span></span> <span data-ttu-id="da1ea-164">這表示路由定義會緊接著其所關聯的控制器和動作。</span><span class="sxs-lookup"><span data-stu-id="da1ea-164">This means that your route definitions are placed next to the controller and action with which they're associated.</span></span>

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

### <a name="model-binding"></a><span data-ttu-id="da1ea-165">模型繫結</span><span class="sxs-lookup"><span data-stu-id="da1ea-165">Model binding</span></span>

<span data-ttu-id="da1ea-166">ASP.NET Core MVC [模型繫結](models/model-binding.md)會將用戶端要求資料 (表單值、路由資料、查詢字串參數、HTTP 標頭)，轉換成控制器可以處理的物件。</span><span class="sxs-lookup"><span data-stu-id="da1ea-166">ASP.NET Core MVC [model binding](models/model-binding.md) converts client request data  (form values, route data, query string parameters, HTTP headers) into objects that the controller can handle.</span></span> <span data-ttu-id="da1ea-167">因此，您的控制器邏輯不必理解內送要求資料，它只會將資料當作其動作方法的參數。</span><span class="sxs-lookup"><span data-stu-id="da1ea-167">As a result, your controller logic doesn't have to do the work of figuring out the incoming request data; it simply has the data as parameters to its action methods.</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
```

### <a name="model-validation"></a><span data-ttu-id="da1ea-168">模型驗證</span><span class="sxs-lookup"><span data-stu-id="da1ea-168">Model validation</span></span>

<span data-ttu-id="da1ea-169">ASP.NET Core MVC 支援使用資料註解驗證屬性來裝置模型物件，藉此進行[驗證](models/validation.md)。</span><span class="sxs-lookup"><span data-stu-id="da1ea-169">ASP.NET Core MVC supports [validation](models/validation.md) by decorating your model object with data annotation validation attributes.</span></span> <span data-ttu-id="da1ea-170">這些驗證屬性會在用戶端進行檢查，再將其值張貼至伺服器，而且會在伺服器上進行檢查，再呼叫控制器動作。</span><span class="sxs-lookup"><span data-stu-id="da1ea-170">The validation attributes are checked on the client side before values are posted to the server, as well as on the server before the controller action is called.</span></span>

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

<span data-ttu-id="da1ea-171">控制器動作：</span><span class="sxs-lookup"><span data-stu-id="da1ea-171">A controller action:</span></span>

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

<span data-ttu-id="da1ea-172">此架構會在用戶端和伺服器上處理驗證要求資料。</span><span class="sxs-lookup"><span data-stu-id="da1ea-172">The framework handles validating request data both on the client and on the server.</span></span> <span data-ttu-id="da1ea-173">模型類型上所指定的驗證邏輯會以不顯眼的註解形式新增呈現的檢視，並使用 [jQuery 驗證](https://jqueryvalidation.org/)在瀏覽器中強制執行。</span><span class="sxs-lookup"><span data-stu-id="da1ea-173">Validation logic specified on model types is added to the rendered views as unobtrusive annotations and is enforced in the browser with [jQuery Validation](https://jqueryvalidation.org/).</span></span>

### <a name="dependency-injection"></a><span data-ttu-id="da1ea-174">相依性插入</span><span class="sxs-lookup"><span data-stu-id="da1ea-174">Dependency injection</span></span>

<span data-ttu-id="da1ea-175">ASP.NET Core 內建[相依性插入 (DI)](../fundamentals/dependency-injection.md) 支援。</span><span class="sxs-lookup"><span data-stu-id="da1ea-175">ASP.NET Core has built-in support for [dependency injection (DI)](../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="da1ea-176">在 ASP.NET Core MVC 中，[控制器](controllers/dependency-injection.md)可以透過其建構函式要求所需服務，以便遵循 [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) (明確的相依性原則)。</span><span class="sxs-lookup"><span data-stu-id="da1ea-176">In ASP.NET Core MVC, [controllers](controllers/dependency-injection.md) can request needed services through their constructors, allowing them to follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

<span data-ttu-id="da1ea-177">您的應用程式也可以使用 [ 指示詞，](views/dependency-injection.md)在檢視檔案中使用相依性插入`@inject`：</span><span class="sxs-lookup"><span data-stu-id="da1ea-177">Your app can also use [dependency injection in view files](views/dependency-injection.md), using the `@inject` directive:</span></span>

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

### <a name="filters"></a><span data-ttu-id="da1ea-178">篩選器。</span><span class="sxs-lookup"><span data-stu-id="da1ea-178">Filters</span></span>

<span data-ttu-id="da1ea-179">[篩選](controllers/filters.md)可協助開發人員封裝交叉關注，例如例外狀況處理或授權。</span><span class="sxs-lookup"><span data-stu-id="da1ea-179">[Filters](controllers/filters.md) help developers encapsulate cross-cutting concerns, like exception handling or authorization.</span></span> <span data-ttu-id="da1ea-180">篩選可執行動作方法的自訂處理前後邏輯，並可設定為在指定要求之執行管線內的特定時間點執行。</span><span class="sxs-lookup"><span data-stu-id="da1ea-180">Filters enable running custom pre- and post-processing logic for action methods, and can be configured to run at certain points within the execution pipeline for a given request.</span></span> <span data-ttu-id="da1ea-181">篩選可當作屬性套用至控制器或動作 (也可全域執行)。</span><span class="sxs-lookup"><span data-stu-id="da1ea-181">Filters can be applied to controllers or actions as attributes (or can be run globally).</span></span> <span data-ttu-id="da1ea-182">此架構隨附數個篩選 (例如`Authorize`)。</span><span class="sxs-lookup"><span data-stu-id="da1ea-182">Several filters (such as `Authorize`) are included in the framework.</span></span> <span data-ttu-id="da1ea-183">`[Authorize]` 是用來建立 MVC 授權篩選的屬性。</span><span class="sxs-lookup"><span data-stu-id="da1ea-183">`[Authorize]` is the attribute that is used to create MVC authorization filters.</span></span>

```csharp
[Authorize]
public class AccountController : Controller
```

### <a name="areas"></a><span data-ttu-id="da1ea-184">區域</span><span class="sxs-lookup"><span data-stu-id="da1ea-184">Areas</span></span>

<span data-ttu-id="da1ea-185">[區域](controllers/areas.md)可讓您將大型 ASP.NET Core MVC Web 應用程式分割成較小的功能群組。</span><span class="sxs-lookup"><span data-stu-id="da1ea-185">[Areas](controllers/areas.md) provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="da1ea-186">一個區域是應用程式內的一個 MVC 結構。</span><span class="sxs-lookup"><span data-stu-id="da1ea-186">An area is an MVC structure inside an application.</span></span> <span data-ttu-id="da1ea-187">在 MVC 專案中，模型、控制器和檢視等邏輯元件會保留在不同的資料夾中，而且 MVC 會使用命名慣例來建立這些元件之間的關聯性。</span><span class="sxs-lookup"><span data-stu-id="da1ea-187">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="da1ea-188">針對大型應用程式，將應用程式分割成個別高功能層級區域可能較有利。</span><span class="sxs-lookup"><span data-stu-id="da1ea-188">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="da1ea-189">例如，具有多個業務單位的電子商務應用程式，例如結帳、計費和搜尋等。每個單位都有自己的邏輯元件視圖、控制器和模型。</span><span class="sxs-lookup"><span data-stu-id="da1ea-189">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span>

### <a name="web-apis"></a><span data-ttu-id="da1ea-190">Web API</span><span class="sxs-lookup"><span data-stu-id="da1ea-190">Web APIs</span></span>

<span data-ttu-id="da1ea-191">ASP.NET Core MVC 除了是建立網站的理想平台之外，也對建置 Web API 提供絕佳的支援。</span><span class="sxs-lookup"><span data-stu-id="da1ea-191">In addition to being a great platform for building web sites, ASP.NET Core MVC has great support for building Web APIs.</span></span> <span data-ttu-id="da1ea-192">您可以建置可供廣大用戶端使用的服務，包括瀏覽器和行動裝置。</span><span class="sxs-lookup"><span data-stu-id="da1ea-192">You can build services that reach a broad range of clients including browsers and mobile devices.</span></span>

<span data-ttu-id="da1ea-193">此架構包含使用內建支援的 HTTP 內容交涉支援，以[格式化資料](xref:web-api/advanced/formatting)為 JSON 或 XML。</span><span class="sxs-lookup"><span data-stu-id="da1ea-193">The framework includes support for HTTP content-negotiation with built-in support to [format data](xref:web-api/advanced/formatting) as JSON or XML.</span></span> <span data-ttu-id="da1ea-194">撰寫[自訂格式器](xref:web-api/advanced/custom-formatters)可新增您專屬格式的支援。</span><span class="sxs-lookup"><span data-stu-id="da1ea-194">Write [custom formatters](xref:web-api/advanced/custom-formatters) to add support for your own formats.</span></span>

<span data-ttu-id="da1ea-195">使用連結產生可支援超媒體。</span><span class="sxs-lookup"><span data-stu-id="da1ea-195">Use link generation to enable support for hypermedia.</span></span> <span data-ttu-id="da1ea-196">輕鬆就可支援[跨原始來源資源共用 (CORS)](https://www.w3.org/TR/cors/) ，讓您的 Web API 可跨多個 Web 應用程式共用。</span><span class="sxs-lookup"><span data-stu-id="da1ea-196">Easily enable support for [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/) so that your Web APIs can be shared across multiple Web applications.</span></span>

### <a name="testability"></a><span data-ttu-id="da1ea-197">可測試性</span><span class="sxs-lookup"><span data-stu-id="da1ea-197">Testability</span></span>

<span data-ttu-id="da1ea-198">此架構使用介面和相依性插入，因此相當適用於單元測試，而此架構所包含的功能 (例如 Entity Framework 的 TestHost 和 InMemory 提供者) 也讓您可以輕鬆快速地進行[整合測試](xref:test/integration-tests)。</span><span class="sxs-lookup"><span data-stu-id="da1ea-198">The framework's use of interfaces and dependency injection make it well-suited to unit testing, and the framework includes features (like a TestHost and InMemory provider for Entity Framework) that make [integration tests](xref:test/integration-tests) quick and easy as well.</span></span> <span data-ttu-id="da1ea-199">深入了解[如何測試控制器邏輯](controllers/testing.md)。</span><span class="sxs-lookup"><span data-stu-id="da1ea-199">Learn more about [how to test controller logic](controllers/testing.md).</span></span>

### <a name="razor-view-engine"></a><span data-ttu-id="da1ea-200">Razor 檢視引擎</span><span class="sxs-lookup"><span data-stu-id="da1ea-200">Razor view engine</span></span>

<span data-ttu-id="da1ea-201">[ASP.NET Core MVC 檢視](views/overview.md)使用 [Razor 檢視引擎](views/razor.md)來呈現檢視。</span><span class="sxs-lookup"><span data-stu-id="da1ea-201">[ASP.NET Core MVC views](views/overview.md) use the [Razor view engine](views/razor.md) to render views.</span></span> <span data-ttu-id="da1ea-202">Razor 是簡潔、易懂且流暢的範本標記語言，使用內嵌 C# 程式碼來定義檢視。</span><span class="sxs-lookup"><span data-stu-id="da1ea-202">Razor is a compact, expressive and fluid template markup language for defining views using embedded C# code.</span></span> <span data-ttu-id="da1ea-203">Razor 可用來動態產生伺服器上的 Web 內容。</span><span class="sxs-lookup"><span data-stu-id="da1ea-203">Razor is used to dynamically generate web content on the server.</span></span> <span data-ttu-id="da1ea-204">您可以完全混合伺服端程式碼以及用戶端內容和程式碼。</span><span class="sxs-lookup"><span data-stu-id="da1ea-204">You can cleanly mix server code with client side content and code.</span></span>

```cshtml
<ul>
    @for (int i = 0; i < 5; i++) {
        <li>List item @i</li>
    }
</ul>
```

<span data-ttu-id="da1ea-205">您可以使用 Razor 檢視引擎，定義[配置](views/layout.md)、[部分檢視](views/partial.md)及可取代的區段。</span><span class="sxs-lookup"><span data-stu-id="da1ea-205">Using the Razor view engine you can define [layouts](views/layout.md), [partial views](views/partial.md) and replaceable sections.</span></span>

### <a name="strongly-typed-views"></a><span data-ttu-id="da1ea-206">強型別檢視</span><span class="sxs-lookup"><span data-stu-id="da1ea-206">Strongly typed views</span></span>

<span data-ttu-id="da1ea-207">根據您的模型，MVC 中的 Razor 檢視可能是強型別。</span><span class="sxs-lookup"><span data-stu-id="da1ea-207">Razor views in MVC can be strongly typed based on your model.</span></span> <span data-ttu-id="da1ea-208">控制器可以將強型別模型傳遞至檢視，讓您的檢視具有類型檢查和 IntelliSense 支援。</span><span class="sxs-lookup"><span data-stu-id="da1ea-208">Controllers can pass a strongly typed model to views enabling your views to have type checking and IntelliSense support.</span></span>

<span data-ttu-id="da1ea-209">例如，下列檢視會呈現 `IEnumerable<Product>` 類型的模型：</span><span class="sxs-lookup"><span data-stu-id="da1ea-209">For example, the following view renders a model of type `IEnumerable<Product>`:</span></span>

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a><span data-ttu-id="da1ea-210">標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="da1ea-210">Tag Helpers</span></span>

<span data-ttu-id="da1ea-211">[標籤協助程式](views/tag-helpers/intro.md)可讓伺服器端程式碼參與建立及轉譯 Razor 檔案中 HTML 元素的過程。</span><span class="sxs-lookup"><span data-stu-id="da1ea-211">[Tag Helpers](views/tag-helpers/intro.md) enable server side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="da1ea-212">您可以使用標籤協助程式定義自訂標籤 (例如 `<environment>`)，或修改現有標籤 (例如 `<label>`) 的行為。</span><span class="sxs-lookup"><span data-stu-id="da1ea-212">You can use tag helpers to define custom tags (for example, `<environment>`) or to modify the behavior of existing tags (for example, `<label>`).</span></span> <span data-ttu-id="da1ea-213">標籤協助程式會根據元素名稱及其屬性，繫結至特定元素。</span><span class="sxs-lookup"><span data-stu-id="da1ea-213">Tag Helpers bind to specific elements based on the element name and its attributes.</span></span> <span data-ttu-id="da1ea-214">其提供伺服器端轉譯優點，同時仍然保留 HTML 編輯體驗。</span><span class="sxs-lookup"><span data-stu-id="da1ea-214">They provide the benefits of server-side rendering while still preserving an HTML editing experience.</span></span>

<span data-ttu-id="da1ea-215">有許多適用於一般工作 (例如建立表單和連結、載入資產等) 的內建標籤協助程式，還有更多位於公用 GitHub 存放庫及作為 NuGet 套件來提供。</span><span class="sxs-lookup"><span data-stu-id="da1ea-215">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="da1ea-216">標籤協助程式是以 C# 撰寫，並根據元素名稱、屬性名稱或上層標籤來設定目標 HTML 元素。</span><span class="sxs-lookup"><span data-stu-id="da1ea-216">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="da1ea-217">例如，內建 LinkTagHelper 可用來建立 `Login` 之 `AccountsController` 動作的連結：</span><span class="sxs-lookup"><span data-stu-id="da1ea-217">For example, the built-in LinkTagHelper can be used to create a link to the `Login` action of the `AccountsController`:</span></span>

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

<span data-ttu-id="da1ea-218">`EnvironmentTagHelper` 可根據執行階段環境 (例如開發、預備或生產)，將不同的指令碼加入檢視：</span><span class="sxs-lookup"><span data-stu-id="da1ea-218">The `EnvironmentTagHelper` can be used to include different scripts in your views (for example, raw or minified) based on the runtime environment, such as Development, Staging, or Production:</span></span>

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

<span data-ttu-id="da1ea-219">標籤協助程式提供適合 HTML 的開發體驗和豐富的 IntelliSense 環境，以建立 HTML 和 Razor 標記。</span><span class="sxs-lookup"><span data-stu-id="da1ea-219">Tag Helpers provide an HTML-friendly development experience and a rich IntelliSense environment for creating HTML and Razor markup.</span></span> <span data-ttu-id="da1ea-220">大部分的內建標籤協助程式都是以現有的 HTML 項目為目標，並提供項目的伺服器端屬性。</span><span class="sxs-lookup"><span data-stu-id="da1ea-220">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span>

### <a name="view-components"></a><span data-ttu-id="da1ea-221">檢視元件</span><span class="sxs-lookup"><span data-stu-id="da1ea-221">View Components</span></span>

<span data-ttu-id="da1ea-222">[檢視元件](views/view-components.md)可讓您封裝轉譯邏輯，並在整個應用程式中重複使用。</span><span class="sxs-lookup"><span data-stu-id="da1ea-222">[View Components](views/view-components.md) allow you to package rendering logic and reuse it throughout the application.</span></span> <span data-ttu-id="da1ea-223">其類似於[部分檢視](views/partial.md)，但具有相關聯的邏輯。</span><span class="sxs-lookup"><span data-stu-id="da1ea-223">They're similar to [partial views](views/partial.md), but with associated logic.</span></span>

## <a name="compatibility-version"></a><span data-ttu-id="da1ea-224">相容性版本</span><span class="sxs-lookup"><span data-stu-id="da1ea-224">Compatibility version</span></span>

<span data-ttu-id="da1ea-225"><xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> 方法可讓應用程式加入或退出 ASP.NET Core MVC 2.1 或更新版本所引入的可能重大行為變更。</span><span class="sxs-lookup"><span data-stu-id="da1ea-225">The <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> method allows an app to opt-in or opt-out of potentially breaking behavior changes introduced in ASP.NET Core MVC 2.1 or later.</span></span>

<span data-ttu-id="da1ea-226">如需詳細資訊，請參閱 <xref:mvc/compatibility-version>。</span><span class="sxs-lookup"><span data-stu-id="da1ea-226">For more information, see <xref:mvc/compatibility-version>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="da1ea-227">其他資源</span><span class="sxs-lookup"><span data-stu-id="da1ea-227">Additional resources</span></span>

* <span data-ttu-id="da1ea-228">適用于 ASP.NET Core MVC &ndash; 強型別單元測試程式庫[的 MyTested AspNetCore](https://github.com/ivaylokenov/MyTested.AspNetCore.Mvc) ，提供流暢的介面來測試 Mvc 和 Web API 應用程式。</span><span class="sxs-lookup"><span data-stu-id="da1ea-228">[MyTested.AspNetCore.Mvc - Fluent Testing Library for ASP.NET Core MVC](https://github.com/ivaylokenov/MyTested.AspNetCore.Mvc) &ndash; Strongly-typed unit testing library, providing a fluent interface for testing MVC and web API apps.</span></span> <span data-ttu-id="da1ea-229">（*不是由 Microsoft 維護或支援）。*</span><span class="sxs-lookup"><span data-stu-id="da1ea-229">(*Not maintained or supported by Microsoft.*)</span></span>
* [<span data-ttu-id="da1ea-230">將 Razor 元件整合到 Razor Pages 和 MVC 應用程式中</span><span class="sxs-lookup"><span data-stu-id="da1ea-230">Integrate Razor components into Razor Pages and MVC apps</span></span>](xref:blazor/hosting-model-configuration#integrate-razor-components-into-razor-pages-and-mvc-apps)

