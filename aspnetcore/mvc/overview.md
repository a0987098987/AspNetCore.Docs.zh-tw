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
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2017
---
# <a name="overview-of-aspnet-core-mvc"></a><span data-ttu-id="425e2-104">ASP.NET Core MVC 概觀</span><span class="sxs-lookup"><span data-stu-id="425e2-104">Overview of ASP.NET Core MVC</span></span>

<span data-ttu-id="425e2-105">由[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="425e2-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="425e2-106">ASP.NET Core MVC 建立 web 應用程式的豐富架構，而且應用程式開發介面使用模型檢視控制器設計模式。</span><span class="sxs-lookup"><span data-stu-id="425e2-106">ASP.NET Core MVC is a rich framework for building web apps and APIs using the Model-View-Controller design pattern.</span></span>

## <a name="what-is-the-mvc-pattern"></a><span data-ttu-id="425e2-107">什麼是 MVC 模式？</span><span class="sxs-lookup"><span data-stu-id="425e2-107">What is the MVC pattern?</span></span>

<span data-ttu-id="425e2-108">模型檢視控制器 (MVC) 架構模式可將三個主要元件的群組，將應用程式： 模型、 檢視和控制器。</span><span class="sxs-lookup"><span data-stu-id="425e2-108">The Model-View-Controller (MVC) architectural pattern separates an application into three main groups of components: Models, Views, and Controllers.</span></span> <span data-ttu-id="425e2-109">這個模式有助於達到[的重要性分離](http://deviq.com/separation-of-concerns/)。</span><span class="sxs-lookup"><span data-stu-id="425e2-109">This pattern helps to achieve [separation of concerns](http://deviq.com/separation-of-concerns/).</span></span> <span data-ttu-id="425e2-110">使用此模式，使用者要求會路由至控制器負責使用模型執行使用者動作和 （或） 擷取查詢的結果。</span><span class="sxs-lookup"><span data-stu-id="425e2-110">Using this pattern, user requests are routed to a Controller which is responsible for working with the Model to perform user actions and/or retrieve results of queries.</span></span> <span data-ttu-id="425e2-111">控制器會選擇檢視將顯示給使用者，並提供與它需要的任何模型資料。</span><span class="sxs-lookup"><span data-stu-id="425e2-111">The Controller chooses the View to display to the user, and provides it with any Model data it requires.</span></span>

<span data-ttu-id="425e2-112">下圖顯示三個主要元件，以及哪些參考其他：</span><span class="sxs-lookup"><span data-stu-id="425e2-112">The following diagram shows the three main components and which ones reference the others:</span></span>

![MVC 模式](overview/_static/mvc.png)

<span data-ttu-id="425e2-114">此的責任就可協助您擴充應用程式複雜度，因為它是您更輕鬆地編寫、 偵錯和測試的項目 （模型、 檢視或控制站） 的單一作業 (且後面會跟[單一責任原則](http://deviq.com/single-responsibility-principle/)).</span><span class="sxs-lookup"><span data-stu-id="425e2-114">This delineation of responsibilities helps you scale the application in terms of complexity because it’s easier to code, debug, and test something (model, view, or controller) that has a single job (and follows the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/)).</span></span> <span data-ttu-id="425e2-115">它是更難更新、 測試及偵錯程式碼具有相依性跨越兩個或多個三個區域。</span><span class="sxs-lookup"><span data-stu-id="425e2-115">It's more difficult to update, test, and debug code that has dependencies spread across two or more of these three areas.</span></span> <span data-ttu-id="425e2-116">例如，使用者介面邏輯通常會比商務邏輯經常變更。</span><span class="sxs-lookup"><span data-stu-id="425e2-116">For example, user interface logic tends to change more frequently than business logic.</span></span> <span data-ttu-id="425e2-117">如果呈現程式碼和商務邏輯結合在單一物件中時，您必須修改包含商務邏輯，每當您變更使用者介面的物件。</span><span class="sxs-lookup"><span data-stu-id="425e2-117">If presentation code and business logic are combined in a single object, you have to modify an object containing business logic every time you change the user interface.</span></span> <span data-ttu-id="425e2-118">這是可能會造成錯誤，而且需要重新測試所有的商務邏輯的每個的簡潔使用者介面變更之後。</span><span class="sxs-lookup"><span data-stu-id="425e2-118">This is likely to introduce errors and require the retesting of all business logic after every minimal user interface change.</span></span>

> [!NOTE]
> <span data-ttu-id="425e2-119">檢視和控制器取決於模型。</span><span class="sxs-lookup"><span data-stu-id="425e2-119">Both the view and the controller depend on the model.</span></span> <span data-ttu-id="425e2-120">不過，模型會取決於檢視都控制器。</span><span class="sxs-lookup"><span data-stu-id="425e2-120">However, the model depends on neither the view nor the controller.</span></span> <span data-ttu-id="425e2-121">這是其中一個分隔的索引鍵的優點。</span><span class="sxs-lookup"><span data-stu-id="425e2-121">This is one of the key benefits of the separation.</span></span> <span data-ttu-id="425e2-122">此分隔可讓您的視覺呈現獨立建置和測試模型。</span><span class="sxs-lookup"><span data-stu-id="425e2-122">This separation allows the model to be built and tested independent of the visual presentation.</span></span>

### <a name="model-responsibilities"></a><span data-ttu-id="425e2-123">模型的責任</span><span class="sxs-lookup"><span data-stu-id="425e2-123">Model Responsibilities</span></span>

<span data-ttu-id="425e2-124">在 MVC 應用程式中的模型代表應用程式和任何的商務邏輯或應該由它來執行作業的狀態。</span><span class="sxs-lookup"><span data-stu-id="425e2-124">The Model in an MVC application represents the state of the application and any business logic or operations that should be performed by it.</span></span> <span data-ttu-id="425e2-125">商務邏輯應該封裝在模型中，以及任何實作邏輯，來保存應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="425e2-125">Business logic should be encapsulated in the model, along with any implementation logic for persisting the state of the application.</span></span> <span data-ttu-id="425e2-126">強型別檢視通常會顯示於該檢視; 使用 ViewModel 類型特別設計來包含的資料控制站會建立和填入這些 ViewModel 來自執行個體模型。</span><span class="sxs-lookup"><span data-stu-id="425e2-126">Strongly-typed views will typically use ViewModel types specifically designed to contain the data to display on that view; the controller will create and populate these ViewModel instances from the model.</span></span>

> [!NOTE]
> <span data-ttu-id="425e2-127">有許多方式來組織中使用 MVC 架構模式的應用程式的模型。</span><span class="sxs-lookup"><span data-stu-id="425e2-127">There are many ways to organize the model in an app that uses the MVC architectural pattern.</span></span> <span data-ttu-id="425e2-128">深入了解一些[不同種類的模型型別](http://deviq.com/kinds-of-models/)。</span><span class="sxs-lookup"><span data-stu-id="425e2-128">Learn more about some [different kinds of model types](http://deviq.com/kinds-of-models/).</span></span>

### <a name="view-responsibilities"></a><span data-ttu-id="425e2-129">檢視的責任</span><span class="sxs-lookup"><span data-stu-id="425e2-129">View Responsibilities</span></span>

<span data-ttu-id="425e2-130">檢視必須負責透過使用者介面中呈現內容。</span><span class="sxs-lookup"><span data-stu-id="425e2-130">Views are responsible for presenting content through the user interface.</span></span> <span data-ttu-id="425e2-131">他們使用[Razor 檢視引擎](#razor-view-engine)HTML 標記中內嵌的.NET 程式碼。</span><span class="sxs-lookup"><span data-stu-id="425e2-131">They use the [Razor view engine](#razor-view-engine) to embed .NET code in HTML markup.</span></span> <span data-ttu-id="425e2-132">應該在檢視內的最小邏輯，以及它們中的任何邏輯應該與關聯呈現內容。</span><span class="sxs-lookup"><span data-stu-id="425e2-132">There should be minimal logic within views, and any logic in them should relate to presenting content.</span></span> <span data-ttu-id="425e2-133">如果您發現需要執行更多的邏輯檢視中的檔案以顯示複雜模型中的資料，請考慮使用[檢視元件](views/view-components.md)，ViewModel，或檢視表範本來簡化檢視。</span><span class="sxs-lookup"><span data-stu-id="425e2-133">If you find the need to perform a great deal of logic in view files in order to display data from a complex model, consider using a [View Component](views/view-components.md), ViewModel, or view template to simplify the view.</span></span>

### <a name="controller-responsibilities"></a><span data-ttu-id="425e2-134">控制器的責任</span><span class="sxs-lookup"><span data-stu-id="425e2-134">Controller Responsibilities</span></span>

<span data-ttu-id="425e2-135">控制器會處理使用者互動、 處理模型，並且在最後選擇要呈現檢視的元件。</span><span class="sxs-lookup"><span data-stu-id="425e2-135">Controllers are the components that handle user interaction, work with the model, and ultimately select a view to render.</span></span> <span data-ttu-id="425e2-136">MVC 應用程式中，檢視只能顯示資訊。控制器會處理和回應使用者輸入和互動。</span><span class="sxs-lookup"><span data-stu-id="425e2-136">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span> <span data-ttu-id="425e2-137">在 MVC 模式中，控制器初始的進入點，而且會負責選取哪一個模型類型來處理和轉譯的檢視 (它可控制其名稱-因此應用程式的給定要求的回應方式)。</span><span class="sxs-lookup"><span data-stu-id="425e2-137">In the MVC pattern, the controller is the initial entry point, and is responsible for selecting which model types to work with and which view to render (hence its name - it controls how the app responds to a given request).</span></span>

> [!NOTE]
> <span data-ttu-id="425e2-138">控制站應該不能過於複雜太多的責任。</span><span class="sxs-lookup"><span data-stu-id="425e2-138">Controllers should not be overly complicated by too many responsibilities.</span></span> <span data-ttu-id="425e2-139">若要防止控制器邏輯變得過於複雜，請使用[單一責任原則](http://deviq.com/single-responsibility-principle/)從控制站移至網域模型的推播的商務邏輯。</span><span class="sxs-lookup"><span data-stu-id="425e2-139">To keep controller logic from becoming overly complex, use the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/) to push business logic out of the controller and into the domain model.</span></span>

>[!TIP]
> <span data-ttu-id="425e2-140">如果您發現控制器動作經常執行的動作類型，您可以依照[不要重複自行原則](http://deviq.com/don-t-repeat-yourself/)移動這些常見的動作，藉以[篩選](#filters)。</span><span class="sxs-lookup"><span data-stu-id="425e2-140">If you find that your controller actions frequently perform the same kinds of actions, you can follow the [Don't Repeat Yourself principle](http://deviq.com/don-t-repeat-yourself/) by moving these common actions into [filters](#filters).</span></span>

## <a name="what-is-aspnet-core-mvc"></a><span data-ttu-id="425e2-141">ASP.NET Core MVC 是什麼</span><span class="sxs-lookup"><span data-stu-id="425e2-141">What is ASP.NET Core MVC</span></span>

<span data-ttu-id="425e2-142">ASP.NET Core MVC 架構是輕量型、 開放來源時，可高度測試展示架構針對 ASP.NET Core 與使用方式最佳化。</span><span class="sxs-lookup"><span data-stu-id="425e2-142">The ASP.NET Core MVC framework is a lightweight, open source, highly testable presentation framework optimized for use with ASP.NET Core.</span></span>

<span data-ttu-id="425e2-143">ASP.NET Core MVC 提供模式為準的方式建置動態網站，可讓清楚的重要性分離。</span><span class="sxs-lookup"><span data-stu-id="425e2-143">ASP.NET Core MVC provides a patterns-based way to build dynamic websites that enables a clean separation of concerns.</span></span> <span data-ttu-id="425e2-144">它可讓您完全掌控標記，支援 tdd 的開發，並使用最新 web 標準。</span><span class="sxs-lookup"><span data-stu-id="425e2-144">It gives you full control over markup, supports TDD-friendly development and uses the latest web standards.</span></span>

## <a name="features"></a><span data-ttu-id="425e2-145">功能</span><span class="sxs-lookup"><span data-stu-id="425e2-145">Features</span></span>

<span data-ttu-id="425e2-146">ASP.NET Core MVC 包括下列各項：</span><span class="sxs-lookup"><span data-stu-id="425e2-146">ASP.NET Core MVC includes the following:</span></span>

* [<span data-ttu-id="425e2-147">路由傳送</span><span class="sxs-lookup"><span data-stu-id="425e2-147">Routing</span></span>](#routing)
* [<span data-ttu-id="425e2-148">模型繫結</span><span class="sxs-lookup"><span data-stu-id="425e2-148">Model binding</span></span>](#model-binding)
* [<span data-ttu-id="425e2-149">模型驗證</span><span class="sxs-lookup"><span data-stu-id="425e2-149">Model validation</span></span>](#model-validation)
* [<span data-ttu-id="425e2-150">相依性插入</span><span class="sxs-lookup"><span data-stu-id="425e2-150">Dependency injection</span></span>](../fundamentals/dependency-injection.md)
* [<span data-ttu-id="425e2-151">篩選</span><span class="sxs-lookup"><span data-stu-id="425e2-151">Filters</span></span>](#filters)
* [<span data-ttu-id="425e2-152">區域</span><span class="sxs-lookup"><span data-stu-id="425e2-152">Areas</span></span>](#areas)
* [<span data-ttu-id="425e2-153">Web 應用程式開發介面</span><span class="sxs-lookup"><span data-stu-id="425e2-153">Web APIs</span></span>](#web-apis)
* [<span data-ttu-id="425e2-154">可測試性</span><span class="sxs-lookup"><span data-stu-id="425e2-154">Testability</span></span>](#testability)
* [<span data-ttu-id="425e2-155">Razor 檢視引擎</span><span class="sxs-lookup"><span data-stu-id="425e2-155">Razor view engine</span></span>](#razor-view-engine)
* [<span data-ttu-id="425e2-156">強型別的檢視</span><span class="sxs-lookup"><span data-stu-id="425e2-156">Strongly typed views</span></span>](#strongly-typed-views)
* [<span data-ttu-id="425e2-157">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="425e2-157">Tag Helpers</span></span>](#tag-helpers)
* [<span data-ttu-id="425e2-158">檢視元件</span><span class="sxs-lookup"><span data-stu-id="425e2-158">View Components</span></span>](#view-components)

### <a name="routing"></a><span data-ttu-id="425e2-159">路由</span><span class="sxs-lookup"><span data-stu-id="425e2-159">Routing</span></span>

<span data-ttu-id="425e2-160">最上層的建置 ASP.NET Core MVC [ASP.NET Core 路由](../fundamentals/routing.md)、 功能強大的 URL 對映元件，可讓您建置包含易於且可搜尋的 Url 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="425e2-160">ASP.NET Core MVC is built on top of [ASP.NET Core's routing](../fundamentals/routing.md), a powerful URL-mapping component that lets you build applications that have comprehensible and searchable URLs.</span></span> <span data-ttu-id="425e2-161">這可讓您定義您的應用程式 URL 命名模式，適用於搜尋引擎最佳化 (SEO) 和連結的產生，而不考慮如何組織您的網頁伺服器上的檔案。</span><span class="sxs-lookup"><span data-stu-id="425e2-161">This enables you to define your application's URL naming patterns that work well for search engine optimization (SEO) and for link generation, without regard for how the files on your web server are organized.</span></span> <span data-ttu-id="425e2-162">您可以定義您的路由使用方便的路由範本語法支援路由值條件約束、 預設值和選擇性的值。</span><span class="sxs-lookup"><span data-stu-id="425e2-162">You can define your routes using a convenient route template syntax that supports route value constraints, defaults and optional values.</span></span>

<span data-ttu-id="425e2-163">*慣例為基礎的路由*可讓您全域定義的 URL 格式，在您的應用程式接受和每個格式的方式會對應至特定的動作方法上指定的控制器。</span><span class="sxs-lookup"><span data-stu-id="425e2-163">*Convention-based routing* enables you to globally define the URL formats that your application accepts and how each of those formats maps to a specific action method on given controller.</span></span> <span data-ttu-id="425e2-164">收到的連入要求時，路由引擎會剖析 URL 和符合以其中一種已定義的 URL 格式，然後再呼叫相關聯的控制器動作方法。</span><span class="sxs-lookup"><span data-stu-id="425e2-164">When an incoming request is received, the routing engine parses the URL and matches it to one of the defined URL formats, and then calls the associated controller's action method.</span></span>

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="425e2-165">*屬性路由*可讓您指定的路由資訊，而將您的控制器和動作以定義您的應用程式路由的屬性。</span><span class="sxs-lookup"><span data-stu-id="425e2-165">*Attribute routing* enables you to specify routing information by decorating your controllers and actions with attributes that define your application's routes.</span></span> <span data-ttu-id="425e2-166">這表示路由定義會緊控制器和動作與它們相關聯。</span><span class="sxs-lookup"><span data-stu-id="425e2-166">This means that your route definitions are placed next to the controller and action with which they're associated.</span></span>

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

### <a name="model-binding"></a><span data-ttu-id="425e2-167">模型繫結</span><span class="sxs-lookup"><span data-stu-id="425e2-167">Model binding</span></span>

<span data-ttu-id="425e2-168">ASP.NET Core MVC[模型繫結](models/model-binding.md)將表單值、 路由資料、 查詢字串參數 （HTTP 標頭），用戶端要求資料轉換為控制器可以處理的物件。</span><span class="sxs-lookup"><span data-stu-id="425e2-168">ASP.NET Core MVC [model binding](models/model-binding.md) converts client request data  (form values, route data, query string parameters, HTTP headers) into objects that the controller can handle.</span></span> <span data-ttu-id="425e2-169">如此一來，控制器邏輯不需要執行工作的理解內送的要求資料。它只會有做為它的動作方法參數的資料。</span><span class="sxs-lookup"><span data-stu-id="425e2-169">As a result, your controller logic doesn't have to do the work of figuring out the incoming request data; it simply has the data as parameters to its action methods.</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a><span data-ttu-id="425e2-170">模型驗證</span><span class="sxs-lookup"><span data-stu-id="425e2-170">Model validation</span></span>

<span data-ttu-id="425e2-171">ASP.NET Core MVC 支援[驗證](models/validation.md)而將您的模型物件與資料註解驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="425e2-171">ASP.NET Core MVC supports [validation](models/validation.md) by decorating your model object with data annotation validation attributes.</span></span> <span data-ttu-id="425e2-172">驗證屬性值在公佈到伺服器之前，先檢查用戶端，以及呼叫控制器動作之前，伺服器上。</span><span class="sxs-lookup"><span data-stu-id="425e2-172">The validation attributes are checked on the client side before values are posted to the server, as well as on the server before the controller action is called.</span></span>

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

<span data-ttu-id="425e2-173">控制器的動作：</span><span class="sxs-lookup"><span data-stu-id="425e2-173">A controller action:</span></span>

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

<span data-ttu-id="425e2-174">架構會處理驗證要求資料在用戶端和伺服器上。</span><span class="sxs-lookup"><span data-stu-id="425e2-174">The framework will handle validating request data both on the client and on the server.</span></span> <span data-ttu-id="425e2-175">模型類型上指定的驗證邏輯加入至不顯眼的附註形式呈現的檢視，並強制執行瀏覽器中使用[jQuery 驗證](https://jqueryvalidation.org/)。</span><span class="sxs-lookup"><span data-stu-id="425e2-175">Validation logic specified on model types is added to the rendered views as unobtrusive annotations and is enforced in the browser with [jQuery Validation](https://jqueryvalidation.org/).</span></span>

### <a name="dependency-injection"></a><span data-ttu-id="425e2-176">相依性插入</span><span class="sxs-lookup"><span data-stu-id="425e2-176">Dependency injection</span></span>

<span data-ttu-id="425e2-177">ASP.NET Core 已內建支援[相依性插入 (DI)](../fundamentals/dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="425e2-177">ASP.NET Core has built-in support for [dependency injection (DI)](../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="425e2-178">在 ASP.NET Core MVC 中，[控制器](controllers/dependency-injection.md)可以要求所需服務透過其建構函式，讓他們遵循[明確的相依性原則](http://deviq.com/explicit-dependencies-principle/)。</span><span class="sxs-lookup"><span data-stu-id="425e2-178">In ASP.NET Core MVC, [controllers](controllers/dependency-injection.md) can request needed services through their constructors, allowing them to follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="425e2-179">您的應用程式也可以使用[檢視中檔案的相依性插入](views/dependency-injection.md)，並使用`@inject`指示詞：</span><span class="sxs-lookup"><span data-stu-id="425e2-179">Your app can also use [dependency injection in view files](views/dependency-injection.md), using the `@inject` directive:</span></span>

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

### <a name="filters"></a><span data-ttu-id="425e2-180">篩選條件</span><span class="sxs-lookup"><span data-stu-id="425e2-180">Filters</span></span>

<span data-ttu-id="425e2-181">[篩選](controllers/filters.md)協助開發人員封裝跨領域的考量，例如例外狀況處理或授權。</span><span class="sxs-lookup"><span data-stu-id="425e2-181">[Filters](controllers/filters.md) help developers encapsulate cross-cutting concerns, like exception handling or authorization.</span></span> <span data-ttu-id="425e2-182">篩選會啟用動作方法執行自訂的前置和後置處理邏輯，並可設定在給定的要求執行管線內的特定點上執行。</span><span class="sxs-lookup"><span data-stu-id="425e2-182">Filters enable running custom pre- and post-processing logic for action methods, and can be configured to run at certain points within the execution pipeline for a given request.</span></span> <span data-ttu-id="425e2-183">篩選條件可以套用至控制器或動作做為屬性 （或可以全域執行）。</span><span class="sxs-lookup"><span data-stu-id="425e2-183">Filters can be applied to controllers or actions as attributes (or can be run globally).</span></span> <span data-ttu-id="425e2-184">多個篩選器 (例如`Authorize`) 隨附的架構。</span><span class="sxs-lookup"><span data-stu-id="425e2-184">Several filters (such as `Authorize`) are included in the framework.</span></span>


```csharp
[Authorize]
   public class AccountController : Controller
   {
```

### <a name="areas"></a><span data-ttu-id="425e2-185">區域</span><span class="sxs-lookup"><span data-stu-id="425e2-185">Areas</span></span>

<span data-ttu-id="425e2-186">[區域](controllers/areas.md)提供資料的大型 ASP.NET Core MVC Web 應用程式分割成較小功能群組的方式。</span><span class="sxs-lookup"><span data-stu-id="425e2-186">[Areas](controllers/areas.md) provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="425e2-187">一個區域就是應用程式中的一個 MVC 結構。</span><span class="sxs-lookup"><span data-stu-id="425e2-187">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="425e2-188">在 MVC 專案中，邏輯元件，例如模型、 控制器和檢視保留在不同的資料夾，而且若要建立這些元件之間的關聯性則 MVC 會使用命名慣例。</span><span class="sxs-lookup"><span data-stu-id="425e2-188">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="425e2-189">大型應用程式中，它可能比較有利分割成個別高功能層級區域的 應用程式。</span><span class="sxs-lookup"><span data-stu-id="425e2-189">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="425e2-190">例如，電子商務應用程式與多個業務單位，例如簽出、 帳單和搜尋等。每個這些單位具有自己的邏輯元件檢視、 控制器、 和模型。</span><span class="sxs-lookup"><span data-stu-id="425e2-190">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span>

### <a name="web-apis"></a><span data-ttu-id="425e2-191">Web 應用程式開發介面</span><span class="sxs-lookup"><span data-stu-id="425e2-191">Web APIs</span></span>

<span data-ttu-id="425e2-192">正在建立網站的絕佳平台，除了 ASP.NET Core MVC 會有絕佳的支援，來建置 Web 應用程式開發介面。</span><span class="sxs-lookup"><span data-stu-id="425e2-192">In addition to being a great platform for building web sites, ASP.NET Core MVC has great support for building Web APIs.</span></span> <span data-ttu-id="425e2-193">您可以建立服務，可達到廣泛的用戶端包括瀏覽器和行動裝置。</span><span class="sxs-lookup"><span data-stu-id="425e2-193">You can build services that can reach a broad range of clients including browsers and mobile devices.</span></span>

<span data-ttu-id="425e2-194">此架構包含支援的內建支援的 HTTP 內容交涉[格式化資料](models/formatting.md)為 JSON 或 XML。</span><span class="sxs-lookup"><span data-stu-id="425e2-194">The framework includes support for HTTP content-negotiation with built-in support for [formatting data](models/formatting.md) as JSON or XML.</span></span> <span data-ttu-id="425e2-195">寫入[自訂格式器](advanced/custom-formatters.md)加入自己的格式支援。</span><span class="sxs-lookup"><span data-stu-id="425e2-195">Write [custom formatters](advanced/custom-formatters.md) to add support for your own formats.</span></span>

<span data-ttu-id="425e2-196">用於啟用支援超連結產生。</span><span class="sxs-lookup"><span data-stu-id="425e2-196">Use link generation to enable support for hypermedia.</span></span> <span data-ttu-id="425e2-197">輕鬆啟用支援[跨原始資源共用 (CORS)](http://www.w3.org/TR/cors/) ，讓您的 Web Api 可以橫跨多個 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="425e2-197">Easily enable support for [cross-origin resource sharing (CORS)](http://www.w3.org/TR/cors/) so that your Web APIs can be shared across multiple Web applications.</span></span>

### <a name="testability"></a><span data-ttu-id="425e2-198">可測試性</span><span class="sxs-lookup"><span data-stu-id="425e2-198">Testability</span></span>

<span data-ttu-id="425e2-199">架構的使用介面和相依性插入會導致它適用於單元測試，並架構包含功能 （例如 Entity Framework TestHost 和 InMemory 提供者），使[整合測試](../testing/integration-testing.md)快速簡便也。</span><span class="sxs-lookup"><span data-stu-id="425e2-199">The framework's use of interfaces and dependency injection make it well-suited to unit testing, and the framework includes features (like a TestHost and InMemory provider for Entity Framework) that make [integration testing](../testing/integration-testing.md) quick and easy as well.</span></span> <span data-ttu-id="425e2-200">深入了解[測試控制器邏輯](controllers/testing.md)。</span><span class="sxs-lookup"><span data-stu-id="425e2-200">Learn more about [testing controller logic](controllers/testing.md).</span></span>

### <a name="razor-view-engine"></a><span data-ttu-id="425e2-201">Razor 檢視引擎</span><span class="sxs-lookup"><span data-stu-id="425e2-201">Razor view engine</span></span>

<span data-ttu-id="425e2-202">[ASP.NET Core MVC 檢視](views/overview.md)使用[Razor 檢視引擎](views/razor.md)來呈現檢視。</span><span class="sxs-lookup"><span data-stu-id="425e2-202">[ASP.NET Core MVC views](views/overview.md) use the [Razor view engine](views/razor.md) to render views.</span></span> <span data-ttu-id="425e2-203">Razor 是壓縮、 易懂和流暢的範本標記語言來定義使用內嵌的 C# 程式碼的檢視。</span><span class="sxs-lookup"><span data-stu-id="425e2-203">Razor is a compact, expressive and fluid template markup language for defining views using embedded C# code.</span></span> <span data-ttu-id="425e2-204">Razor 用於動態產生的 web 內容伺服器上。</span><span class="sxs-lookup"><span data-stu-id="425e2-204">Razor is used to dynamically generate web content on the server.</span></span> <span data-ttu-id="425e2-205">您完全可以混合伺服端程式碼使用用戶端端內容和作業程式碼。</span><span class="sxs-lookup"><span data-stu-id="425e2-205">You can cleanly mix server code with client side content and code.</span></span>

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

<span data-ttu-id="425e2-206">使用 Razor 檢視引擎，您可以定義[配置](views/layout.md)，[部分檢視](views/partial.md)和可取代的區段。</span><span class="sxs-lookup"><span data-stu-id="425e2-206">Using the Razor view engine you can define [layouts](views/layout.md), [partial views](views/partial.md) and replaceable sections.</span></span>

### <a name="strongly-typed-views"></a><span data-ttu-id="425e2-207">強型別的檢視</span><span class="sxs-lookup"><span data-stu-id="425e2-207">Strongly typed views</span></span>

<span data-ttu-id="425e2-208">在 MVC razor 檢視可以是強型別根據您的模型。</span><span class="sxs-lookup"><span data-stu-id="425e2-208">Razor views in MVC can be strongly typed based on your model.</span></span> <span data-ttu-id="425e2-209">控制站可以強型別的模型傳遞至檢視，讓您檢視，以具有型別檢查和 IntelliSense 支援。</span><span class="sxs-lookup"><span data-stu-id="425e2-209">Controllers can pass a strongly typed model to views enabling your views to have type checking and IntelliSense support.</span></span>

<span data-ttu-id="425e2-210">例如，下列檢視定義的模型型別`IEnumerable<Product>`:</span><span class="sxs-lookup"><span data-stu-id="425e2-210">For example, the following view defines a model of type `IEnumerable<Product>`:</span></span>

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a><span data-ttu-id="425e2-211">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="425e2-211">Tag Helpers</span></span>

<span data-ttu-id="425e2-212">[標記協助程式](views/tag-helpers/intro.md)啟用建立和呈現 HTML Razor 檔案中的項目加入伺服器端程式碼。</span><span class="sxs-lookup"><span data-stu-id="425e2-212">[Tag Helpers](views/tag-helpers/intro.md) enable server side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="425e2-213">您可用來定義自訂標籤標記協助程式 (例如， `<environment>`) 或修改現有的標籤行為 (例如， `<label>`)。</span><span class="sxs-lookup"><span data-stu-id="425e2-213">You can use tag helpers to define custom tags (for example, `<environment>`) or to modify the behavior of existing tags (for example, `<label>`).</span></span> <span data-ttu-id="425e2-214">標記協助程式繫結至特定項目名稱和其屬性為基礎的項目。</span><span class="sxs-lookup"><span data-stu-id="425e2-214">Tag Helpers bind to specific elements based on the element name and its attributes.</span></span> <span data-ttu-id="425e2-215">它們提供伺服器端轉譯，同時仍然保留 HTML 編輯經驗的優點。</span><span class="sxs-lookup"><span data-stu-id="425e2-215">They provide the benefits of server-side rendering while still preserving an HTML editing experience.</span></span>

<span data-ttu-id="425e2-216">有許多內建的標記協助程式的一般工作-例如建立表單、 連結、 載入資產和更多的和更多可用公用 GitHub 儲存機制中以及 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="425e2-216">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="425e2-217">標記協助程式以 C# 中，撰寫，與它們目標項目名稱、 屬性名稱，或父標記的 HTML 項目。</span><span class="sxs-lookup"><span data-stu-id="425e2-217">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="425e2-218">例如，內建 LinkTagHelper 可以用來建立的連結`Login`動作`AccountsController`:</span><span class="sxs-lookup"><span data-stu-id="425e2-218">For example, the built-in LinkTagHelper can be used to create a link to the `Login` action of the `AccountsController`:</span></span>

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

<span data-ttu-id="425e2-219">`EnvironmentTagHelper`可用來在您檢視 （例如 raw 或縮短） 為基礎的執行階段環境，例如開發、 預備或生產環境中包含不同的指令碼：</span><span class="sxs-lookup"><span data-stu-id="425e2-219">The `EnvironmentTagHelper` can be used to include different scripts in your views (for example, raw or minified) based on the runtime environment, such as Development, Staging, or Production:</span></span>

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

<span data-ttu-id="425e2-220">標記協助程式提供 HTML 易用的開發經驗和豐富的 IntelliSense 環境，以建立 HTML 和 Razor 標記。</span><span class="sxs-lookup"><span data-stu-id="425e2-220">Tag Helpers provide an HTML-friendly development experience and a rich IntelliSense environment for creating HTML and Razor markup.</span></span> <span data-ttu-id="425e2-221">大部分的內建的標記協助程式和目標現有的 HTML 元素的元素提供伺服器端的屬性。</span><span class="sxs-lookup"><span data-stu-id="425e2-221">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span>

### <a name="view-components"></a><span data-ttu-id="425e2-222">檢視元件</span><span class="sxs-lookup"><span data-stu-id="425e2-222">View Components</span></span>

<span data-ttu-id="425e2-223">[檢視元件](views/view-components.md)可讓您封裝呈現邏輯和整個應用程式中重複加以使用。</span><span class="sxs-lookup"><span data-stu-id="425e2-223">[View Components](views/view-components.md) allow you to package rendering logic and reuse it throughout the application.</span></span> <span data-ttu-id="425e2-224">它們類似於[部分檢視](views/partial.md)，但具有相關聯的邏輯。</span><span class="sxs-lookup"><span data-stu-id="425e2-224">They're similar to [partial views](views/partial.md), but with associated logic.</span></span>
