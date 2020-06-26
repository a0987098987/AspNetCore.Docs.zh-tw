---
title: 在 ASP.NET Core MVC 中處理控制器要求
author: ardalis
description: ''
ms.author: riande
ms.date: 12/05/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: mvc/controllers/actions
ms.openlocfilehash: 0c91edc947b1a17f2dd36b281afe348aa8611bd7
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85406910"
---
# <a name="handle-requests-with-controllers-in-aspnet-core-mvc"></a><span data-ttu-id="682c4-102">在 ASP.NET Core MVC 中處理控制器要求</span><span class="sxs-lookup"><span data-stu-id="682c4-102">Handle requests with controllers in ASP.NET Core MVC</span></span>

<span data-ttu-id="682c4-103">作者：[Steve Smith](https://ardalis.com/) 和 [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="682c4-103">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="682c4-104">控制器、動作和動作結果是開發人員使用 ASP.NET Core MVC 建置應用程式的基礎部分。</span><span class="sxs-lookup"><span data-stu-id="682c4-104">Controllers, actions, and action results are a fundamental part of how developers build apps using ASP.NET Core MVC.</span></span>

## <a name="what-is-a-controller"></a><span data-ttu-id="682c4-105">什麼是控制器？</span><span class="sxs-lookup"><span data-stu-id="682c4-105">What is a Controller?</span></span>

<span data-ttu-id="682c4-106">控制器是用來定義和分組一組動作。</span><span class="sxs-lookup"><span data-stu-id="682c4-106">A controller is used to define and group a set of actions.</span></span> <span data-ttu-id="682c4-107">動作 (或「動作方法」\*\*) 是控制器上處理要求的方法。</span><span class="sxs-lookup"><span data-stu-id="682c4-107">An action (or *action method*) is a method on a controller which handles requests.</span></span> <span data-ttu-id="682c4-108">控制器會以邏輯方式將類似動作群組在一起。</span><span class="sxs-lookup"><span data-stu-id="682c4-108">Controllers logically group similar actions together.</span></span> <span data-ttu-id="682c4-109">這項動作彙總允許統一套用一組通用規則 (例如路由、快取和授權)。</span><span class="sxs-lookup"><span data-stu-id="682c4-109">This aggregation of actions allows common sets of rules, such as routing, caching, and authorization, to be applied collectively.</span></span> <span data-ttu-id="682c4-110">要求會透過[路由](xref:mvc/controllers/routing)對應至動作。</span><span class="sxs-lookup"><span data-stu-id="682c4-110">Requests are mapped to actions through [routing](xref:mvc/controllers/routing).</span></span>

<span data-ttu-id="682c4-111">依照慣例，控制器類別：</span><span class="sxs-lookup"><span data-stu-id="682c4-111">By convention, controller classes:</span></span>

* <span data-ttu-id="682c4-112">位於專案的根層級*控制器*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="682c4-112">Reside in the project's root-level *Controllers* folder.</span></span>
* <span data-ttu-id="682c4-113">繼承自 `Microsoft.AspNetCore.Mvc.Controller`。</span><span class="sxs-lookup"><span data-stu-id="682c4-113">Inherit from `Microsoft.AspNetCore.Mvc.Controller`.</span></span>

<span data-ttu-id="682c4-114">控制器是至少下列其中一個條件為 true 的可具現化類別：</span><span class="sxs-lookup"><span data-stu-id="682c4-114">A controller is an instantiable class in which at least one of the following conditions is true:</span></span>

* <span data-ttu-id="682c4-115">類別名稱尾碼為 `Controller` 。</span><span class="sxs-lookup"><span data-stu-id="682c4-115">The class name is suffixed with `Controller`.</span></span>
* <span data-ttu-id="682c4-116">類別繼承自名稱尾碼為的類別 `Controller` 。</span><span class="sxs-lookup"><span data-stu-id="682c4-116">The class inherits from a class whose name is suffixed with `Controller`.</span></span>
* <span data-ttu-id="682c4-117">`[Controller]`屬性會套用至類別。</span><span class="sxs-lookup"><span data-stu-id="682c4-117">The `[Controller]` attribute is applied to the class.</span></span>

<span data-ttu-id="682c4-118">控制器類別不得具有相關聯的 `[NonController]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="682c4-118">A controller class must not have an associated `[NonController]` attribute.</span></span>

<span data-ttu-id="682c4-119">控制器應該遵循[明確相依性準則](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)。</span><span class="sxs-lookup"><span data-stu-id="682c4-119">Controllers should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span> <span data-ttu-id="682c4-120">有幾種方法可以實作此準則。</span><span class="sxs-lookup"><span data-stu-id="682c4-120">There are a couple of approaches to implementing this principle.</span></span> <span data-ttu-id="682c4-121">如果多個控制器動作需要相同的服務，請考慮使用[建構函式插入](xref:mvc/controllers/dependency-injection#constructor-injection)來要求這些相依性。</span><span class="sxs-lookup"><span data-stu-id="682c4-121">If multiple controller actions require the same service, consider using [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to request those dependencies.</span></span> <span data-ttu-id="682c4-122">如果只有單一動作方法需要服務，請考慮使用[動作插入](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)來要求相依性。</span><span class="sxs-lookup"><span data-stu-id="682c4-122">If the service is needed by only a single action method, consider using [Action Injection](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) to request the dependency.</span></span>

<span data-ttu-id="682c4-123">在\*\*模型檢視控制器\*\*\*\*\*\*\*\*\*\* 模式內，控制器負責初始處理要求和模型具現化。</span><span class="sxs-lookup"><span data-stu-id="682c4-123">Within the **M**odel-**V**iew-**C**ontroller pattern, a controller is responsible for the initial processing of the request and instantiation of the model.</span></span> <span data-ttu-id="682c4-124">一般而言，應該在模型內執行商業決策。</span><span class="sxs-lookup"><span data-stu-id="682c4-124">Generally, business decisions should be performed within the model.</span></span>

<span data-ttu-id="682c4-125">控制器會使用模型處理結果 (如果有的話)，並傳回適當檢視和其相關聯的檢視資料或 API 呼叫結果。</span><span class="sxs-lookup"><span data-stu-id="682c4-125">The controller takes the result of the model's processing (if any) and returns either the proper view and its associated view data or the result of the API call.</span></span> <span data-ttu-id="682c4-126">深入了解 [ASP.NET Core MVC 概觀](xref:mvc/overview)和 [ASP.NET Core MVC 與 Visual Studio 使用者入門](xref:tutorials/first-mvc-app/start-mvc)。</span><span class="sxs-lookup"><span data-stu-id="682c4-126">Learn more at [Overview of ASP.NET Core MVC](xref:mvc/overview) and [Get started with ASP.NET Core MVC and Visual Studio](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="682c4-127">控制器是「UI 層級」\*\* 抽象概念。</span><span class="sxs-lookup"><span data-stu-id="682c4-127">The controller is a *UI-level* abstraction.</span></span> <span data-ttu-id="682c4-128">其負責確保要求資料有效，並選擇應該傳回的檢視 (或 API 的結果)。</span><span class="sxs-lookup"><span data-stu-id="682c4-128">Its responsibilities are to ensure request data is valid and to choose which view (or result for an API) should be returned.</span></span> <span data-ttu-id="682c4-129">在構造良好的應用程式中，不會直接包括資料存取或商務邏輯。</span><span class="sxs-lookup"><span data-stu-id="682c4-129">In well-factored apps, it doesn't directly include data access or business logic.</span></span> <span data-ttu-id="682c4-130">相反地，控制器會委派給處理這些責任的服務。</span><span class="sxs-lookup"><span data-stu-id="682c4-130">Instead, the controller delegates to services handling these responsibilities.</span></span>

## <a name="defining-actions"></a><span data-ttu-id="682c4-131">定義動作</span><span class="sxs-lookup"><span data-stu-id="682c4-131">Defining Actions</span></span>

<span data-ttu-id="682c4-132">控制器上的公用方法（屬性除外） `[NonAction]` 是動作。</span><span class="sxs-lookup"><span data-stu-id="682c4-132">Public methods on a controller, except those with the `[NonAction]` attribute, are actions.</span></span> <span data-ttu-id="682c4-133">動作上的參數會繫結至要求資料，並使用[模型繫結](xref:mvc/models/model-binding)進行驗證。</span><span class="sxs-lookup"><span data-stu-id="682c4-133">Parameters on actions are bound to request data and are validated using [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="682c4-134">繫結模型的所有項目都會進行模型驗證。</span><span class="sxs-lookup"><span data-stu-id="682c4-134">Model validation occurs for everything that's model-bound.</span></span> <span data-ttu-id="682c4-135">`ModelState.IsValid` 屬性值指出模型繫結和驗證是否成功。</span><span class="sxs-lookup"><span data-stu-id="682c4-135">The `ModelState.IsValid` property value indicates whether model binding and validation succeeded.</span></span>

<span data-ttu-id="682c4-136">動作方法應該包含將要求對應至商務關注的邏輯。</span><span class="sxs-lookup"><span data-stu-id="682c4-136">Action methods should contain logic for mapping a request to a business concern.</span></span> <span data-ttu-id="682c4-137">商務關注應該一般呈現為控制器透過[相依性插入](xref:mvc/controllers/dependency-injection)存取的服務。</span><span class="sxs-lookup"><span data-stu-id="682c4-137">Business concerns should typically be represented as services that the controller accesses through [dependency injection](xref:mvc/controllers/dependency-injection).</span></span> <span data-ttu-id="682c4-138">動作接著會將商務動作的結果對應至應用程式狀態。</span><span class="sxs-lookup"><span data-stu-id="682c4-138">Actions then map the result of the business action to an application state.</span></span>

<span data-ttu-id="682c4-139">動作可以傳回任何項目，但經常會傳回可產生回應的 `IActionResult` 執行個體 (或適用於非同步方法的 `Task<IActionResult>`)。</span><span class="sxs-lookup"><span data-stu-id="682c4-139">Actions can return anything, but frequently return an instance of `IActionResult` (or `Task<IActionResult>` for async methods) that produces a response.</span></span> <span data-ttu-id="682c4-140">動作方法負責選擇「回應類型」\*\*。</span><span class="sxs-lookup"><span data-stu-id="682c4-140">The action method is responsible for choosing *what kind of response*.</span></span> <span data-ttu-id="682c4-141">動作結果「會執行對應項目」\*\*。</span><span class="sxs-lookup"><span data-stu-id="682c4-141">The action result *does the responding*.</span></span>

### <a name="controller-helper-methods"></a><span data-ttu-id="682c4-142">控制器協助程式方法</span><span class="sxs-lookup"><span data-stu-id="682c4-142">Controller Helper Methods</span></span>

<span data-ttu-id="682c4-143">控制器通常繼承自[控制器](/dotnet/api/microsoft.aspnetcore.mvc.controller)，但這不是必要的。</span><span class="sxs-lookup"><span data-stu-id="682c4-143">Controllers usually inherit from [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller), although this isn't required.</span></span> <span data-ttu-id="682c4-144">衍生自 `Controller` 提供對三種類別的協助程式方法的存取：</span><span class="sxs-lookup"><span data-stu-id="682c4-144">Deriving from `Controller` provides access to three categories of helper methods:</span></span>

#### <a name="1-methods-resulting-in-an-empty-response-body"></a><span data-ttu-id="682c4-145">1. 產生空白回應主體的方法</span><span class="sxs-lookup"><span data-stu-id="682c4-145">1. Methods resulting in an empty response body</span></span>

<span data-ttu-id="682c4-146">因為回應本文缺少要描述的內容，所以未包括 `Content-Type` HTTP 回應標頭。</span><span class="sxs-lookup"><span data-stu-id="682c4-146">No `Content-Type` HTTP response header is included, since the response body lacks content to describe.</span></span>

<span data-ttu-id="682c4-147">此類別內有兩種結果類型：「重新導向」和「HTTP 狀態碼」。</span><span class="sxs-lookup"><span data-stu-id="682c4-147">There are two result types within this category: Redirect and HTTP Status Code.</span></span>

* <span data-ttu-id="682c4-148">**HTTP 狀態碼**</span><span class="sxs-lookup"><span data-stu-id="682c4-148">**HTTP Status Code**</span></span>

    <span data-ttu-id="682c4-149">此類型會傳回 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="682c4-149">This type returns an HTTP status code.</span></span> <span data-ttu-id="682c4-150">此類型的一些協助程式方法是 `BadRequest`、`NotFound` 和 `Ok`。</span><span class="sxs-lookup"><span data-stu-id="682c4-150">A couple of helper methods of this type are `BadRequest`, `NotFound`, and `Ok`.</span></span> <span data-ttu-id="682c4-151">例如，`return BadRequest();` 在執行時會產生 400 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="682c4-151">For example, `return BadRequest();` produces a 400 status code when executed.</span></span> <span data-ttu-id="682c4-152">`BadRequest`、`NotFound` 和 `Ok` 這類方法進行多載時，就不再適合作為 HTTP 狀態碼回應程式，因為會執行內容交涉。</span><span class="sxs-lookup"><span data-stu-id="682c4-152">When methods such as `BadRequest`, `NotFound`, and `Ok` are overloaded, they no longer qualify as HTTP Status Code responders, since content negotiation is taking place.</span></span>

* <span data-ttu-id="682c4-153">**重新導向**</span><span class="sxs-lookup"><span data-stu-id="682c4-153">**Redirect**</span></span>

    <span data-ttu-id="682c4-154">此類型會傳回到某個動作或目的地的重新導向 (使用 `Redirect`、`LocalRedirect`、`RedirectToAction` 或 `RedirectToRoute`)。</span><span class="sxs-lookup"><span data-stu-id="682c4-154">This type returns a redirect to an action or destination (using `Redirect`, `LocalRedirect`, `RedirectToAction`, or `RedirectToRoute`).</span></span> <span data-ttu-id="682c4-155">例如，`return RedirectToAction("Complete", new {id = 123});` 會重新導向至 `Complete`，並傳遞匿名物件。</span><span class="sxs-lookup"><span data-stu-id="682c4-155">For example, `return RedirectToAction("Complete", new {id = 123});` redirects to `Complete`, passing an anonymous object.</span></span>

    <span data-ttu-id="682c4-156">「重新導向」結果類型不同於主要用來新增 `Location` HTTP 回應標頭的「HTTP 狀態碼」類型。</span><span class="sxs-lookup"><span data-stu-id="682c4-156">The Redirect result type differs from the HTTP Status Code type primarily in the addition of a `Location` HTTP response header.</span></span>

#### <a name="2-methods-resulting-in-a-non-empty-response-body-with-a-predefined-content-type"></a><span data-ttu-id="682c4-157">2. 以預先定義的內容類型產生非空白回應主體的方法</span><span class="sxs-lookup"><span data-stu-id="682c4-157">2. Methods resulting in a non-empty response body with a predefined content type</span></span>

<span data-ttu-id="682c4-158">此類別中的大多數協助程式方法包括 `ContentType` 屬性，可讓您設定 `Content-Type` 回應標頭來描述回應本文。</span><span class="sxs-lookup"><span data-stu-id="682c4-158">Most helper methods in this category include a `ContentType` property, allowing you to set the `Content-Type` response header to describe the response body.</span></span>

<span data-ttu-id="682c4-159">此類別內有兩種結果類型：[檢視](xref:mvc/views/overview)和[格式化回應](xref:web-api/advanced/formatting)。</span><span class="sxs-lookup"><span data-stu-id="682c4-159">There are two result types within this category: [View](xref:mvc/views/overview) and [Formatted Response](xref:web-api/advanced/formatting).</span></span>

* <span data-ttu-id="682c4-160">**檢視**</span><span class="sxs-lookup"><span data-stu-id="682c4-160">**View**</span></span>

    <span data-ttu-id="682c4-161">此類型會傳回使用模型來轉譯 HTML 的檢視。</span><span class="sxs-lookup"><span data-stu-id="682c4-161">This type returns a view which uses a model to render HTML.</span></span> <span data-ttu-id="682c4-162">例如，`return View(customer);` 會將模型傳遞至資料繫結的檢視。</span><span class="sxs-lookup"><span data-stu-id="682c4-162">For example, `return View(customer);` passes a model to the view for data-binding.</span></span>

* <span data-ttu-id="682c4-163">**格式化回應**</span><span class="sxs-lookup"><span data-stu-id="682c4-163">**Formatted Response**</span></span>

    <span data-ttu-id="682c4-164">此類型會傳回 JSON 或類似的資料交換格式，以特定方式來代表物件。</span><span class="sxs-lookup"><span data-stu-id="682c4-164">This type returns JSON or a similar data exchange format to represent an object in a specific manner.</span></span> <span data-ttu-id="682c4-165">例如，`return Json(customer);` 會將所提供的物件序列化為 JSON 格式。</span><span class="sxs-lookup"><span data-stu-id="682c4-165">For example, `return Json(customer);` serializes the provided object into JSON format.</span></span>
    
    <span data-ttu-id="682c4-166">此類型的其他通用方法包括 `File` 與 `PhysicalFile`。</span><span class="sxs-lookup"><span data-stu-id="682c4-166">Other common methods of this type include `File` and `PhysicalFile`.</span></span> <span data-ttu-id="682c4-167">例如，`return PhysicalFile(customerFilePath, "text/xml");` 會傳回 [PhysicalFileResult](/dotnet/api/microsoft.aspnetcore.mvc.physicalfileresult)。</span><span class="sxs-lookup"><span data-stu-id="682c4-167">For example, `return PhysicalFile(customerFilePath, "text/xml");` returns [PhysicalFileResult](/dotnet/api/microsoft.aspnetcore.mvc.physicalfileresult).</span></span>

#### <a name="3-methods-resulting-in-a-non-empty-response-body-formatted-in-a-content-type-negotiated-with-the-client"></a><span data-ttu-id="682c4-168">3. 在與用戶端協商的內容類型中格式化為非空白回應主體的方法</span><span class="sxs-lookup"><span data-stu-id="682c4-168">3. Methods resulting in a non-empty response body formatted in a content type negotiated with the client</span></span>

<span data-ttu-id="682c4-169">此類別普遍稱為**內容交涉**。</span><span class="sxs-lookup"><span data-stu-id="682c4-169">This category is better known as **Content Negotiation**.</span></span> <span data-ttu-id="682c4-170">只要動作傳回 [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult) 類型或 [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) 實作以外的某個項目，就會套用[內容交涉](xref:web-api/advanced/formatting#content-negotiation)。</span><span class="sxs-lookup"><span data-stu-id="682c4-170">[Content negotiation](xref:web-api/advanced/formatting#content-negotiation) applies whenever an action returns an [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult) type or something other than an [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) implementation.</span></span> <span data-ttu-id="682c4-171">傳回非 `IActionResult` 實作的動作 (例如，`object`) 也會傳回「格式化回應」。</span><span class="sxs-lookup"><span data-stu-id="682c4-171">An action that returns a non-`IActionResult` implementation (for example, `object`) also returns a Formatted Response.</span></span>

<span data-ttu-id="682c4-172">此類型的一些協助程式方法包括 `BadRequest`、`CreatedAtRoute` 和 `Ok`。</span><span class="sxs-lookup"><span data-stu-id="682c4-172">Some helper methods of this type include `BadRequest`, `CreatedAtRoute`, and `Ok`.</span></span> <span data-ttu-id="682c4-173">這些方法的範例分別包括 `return BadRequest(modelState);`、`return CreatedAtRoute("routename", values, newobject);` 和 `return Ok(value);`。</span><span class="sxs-lookup"><span data-stu-id="682c4-173">Examples of these methods include `return BadRequest(modelState);`, `return CreatedAtRoute("routename", values, newobject);`, and `return Ok(value);`, respectively.</span></span> <span data-ttu-id="682c4-174">請注意，只有在傳遞值時，`BadRequest` 和 `Ok` 才會執行內容交涉；如果未傳遞值，則會改成作為「HTTP 狀態碼」結果類型。</span><span class="sxs-lookup"><span data-stu-id="682c4-174">Note that `BadRequest` and `Ok` perform content negotiation only when passed a value; without being passed a value, they instead serve as HTTP Status Code result types.</span></span> <span data-ttu-id="682c4-175">相反地，`CreatedAtRoute` 方法一律會執行內容交涉，因為其多載全部都需要傳遞值。</span><span class="sxs-lookup"><span data-stu-id="682c4-175">The `CreatedAtRoute` method, on the other hand, always performs content negotiation since its overloads all require that a value be passed.</span></span>

### <a name="cross-cutting-concerns"></a><span data-ttu-id="682c4-176">跨領域關注</span><span class="sxs-lookup"><span data-stu-id="682c4-176">Cross-Cutting Concerns</span></span>

<span data-ttu-id="682c4-177">應用程式通常會共用其工作流程的各部分。</span><span class="sxs-lookup"><span data-stu-id="682c4-177">Applications typically share parts of their workflow.</span></span> <span data-ttu-id="682c4-178">範例包括需要驗證購物車存取權的應用程式，或快取某些頁面上資料的應用程式。</span><span class="sxs-lookup"><span data-stu-id="682c4-178">Examples include an app that requires authentication to access the shopping cart, or an app that caches data on some pages.</span></span> <span data-ttu-id="682c4-179">若要在動作方法之前或之後執行邏輯，請使用 *filter*。</span><span class="sxs-lookup"><span data-stu-id="682c4-179">To perform logic before or after an action method, use a *filter*.</span></span> <span data-ttu-id="682c4-180">在交叉關注上使用[篩選](xref:mvc/controllers/filters)可減少重複。</span><span class="sxs-lookup"><span data-stu-id="682c4-180">Using [Filters](xref:mvc/controllers/filters) on cross-cutting concerns can reduce duplication.</span></span>

<span data-ttu-id="682c4-181">大部分的篩選屬性 (例如 `[Authorize]`) 可以套用至控制器或動作層級 (視所需的細微性層級而定)。</span><span class="sxs-lookup"><span data-stu-id="682c4-181">Most filter attributes, such as `[Authorize]`, can be applied at the controller or action level depending upon the desired level of granularity.</span></span>

<span data-ttu-id="682c4-182">錯誤處理和回應快取通常是跨領域關注：</span><span class="sxs-lookup"><span data-stu-id="682c4-182">Error handling and response caching are often cross-cutting concerns:</span></span>
* [<span data-ttu-id="682c4-183">處理錯誤</span><span class="sxs-lookup"><span data-stu-id="682c4-183">Handle errors</span></span>](xref:mvc/controllers/filters#exception-filters)
* [<span data-ttu-id="682c4-184">回應快取</span><span class="sxs-lookup"><span data-stu-id="682c4-184">Response Caching</span></span>](xref:performance/caching/response)

<span data-ttu-id="682c4-185">許多跨領域關注都可以使用篩選或自訂[中介軟體](xref:fundamentals/middleware/index)進行處理。</span><span class="sxs-lookup"><span data-stu-id="682c4-185">Many cross-cutting concerns can be handled using filters or custom [middleware](xref:fundamentals/middleware/index).</span></span>
