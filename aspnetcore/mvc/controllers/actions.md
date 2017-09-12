---
title: "處理要求中 ASP.NET Core MVC 控制站"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 07/03/2017
ms.topic: article
ms.assetid: 9da9eb52-8583-4069-af91-155ba3529d7f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/actions
ms.openlocfilehash: 5dc6c7dc70027bb79875f389d535119a2543b873
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="handling-requests-with-controllers-in-aspnet-core-mvc"></a><span data-ttu-id="dbebb-103">處理要求中 ASP.NET Core MVC 控制站</span><span class="sxs-lookup"><span data-stu-id="dbebb-103">Handling requests with controllers in ASP.NET Core MVC</span></span>

<span data-ttu-id="dbebb-104">由[Steve Smith](https://ardalis.com/)和[Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="dbebb-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="dbebb-105">控制器、 動作和動作結果是開發人員如何建置使用 ASP.NET Core MVC 應用程式的基礎部分。</span><span class="sxs-lookup"><span data-stu-id="dbebb-105">Controllers, actions, and action results are a fundamental part of how developers build apps using ASP.NET Core MVC.</span></span>

## <a name="what-is-a-controller"></a><span data-ttu-id="dbebb-106">什麼是控制站？</span><span class="sxs-lookup"><span data-stu-id="dbebb-106">What is a Controller?</span></span>

<span data-ttu-id="dbebb-107">在控制站用來定義和群組的一組動作。</span><span class="sxs-lookup"><span data-stu-id="dbebb-107">A controller is used to define and group a set of actions.</span></span> <span data-ttu-id="dbebb-108">動作 (或*動作方法*) 是一種方法來處理要求的控制器。</span><span class="sxs-lookup"><span data-stu-id="dbebb-108">An action (or *action method*) is a method on a controller which handles requests.</span></span> <span data-ttu-id="dbebb-109">控制站以邏輯方式分組類似動作。</span><span class="sxs-lookup"><span data-stu-id="dbebb-109">Controllers logically group similar actions together.</span></span> <span data-ttu-id="dbebb-110">此彙總的動作可讓一般組規則，例如路由、 快取，以及授權，統一套用。</span><span class="sxs-lookup"><span data-stu-id="dbebb-110">This aggregation of actions allows common sets of rules, such as routing, caching, and authorization, to be applied collectively.</span></span> <span data-ttu-id="dbebb-111">要求會對應至動作透過[路由](xref:mvc/controllers/routing)。</span><span class="sxs-lookup"><span data-stu-id="dbebb-111">Requests are mapped to actions through [routing](xref:mvc/controllers/routing).</span></span>

<span data-ttu-id="dbebb-112">依照慣例，控制器類別：</span><span class="sxs-lookup"><span data-stu-id="dbebb-112">By convention, controller classes:</span></span>
* <span data-ttu-id="dbebb-113">位於專案的根層級*控制器*資料夾</span><span class="sxs-lookup"><span data-stu-id="dbebb-113">Reside in the project's root-level *Controllers* folder</span></span>
* <span data-ttu-id="dbebb-114">繼承自`Microsoft.AspNetCore.Mvc.Controller`</span><span class="sxs-lookup"><span data-stu-id="dbebb-114">Inherit from `Microsoft.AspNetCore.Mvc.Controller`</span></span>

<span data-ttu-id="dbebb-115">在控制站是可具現化類別至少一個下列條件為 true:</span><span class="sxs-lookup"><span data-stu-id="dbebb-115">A controller is an instantiable class in which at least one of the following conditions is true:</span></span>
* <span data-ttu-id="dbebb-116">類別名稱後置字元"Controller"</span><span class="sxs-lookup"><span data-stu-id="dbebb-116">The class name is suffixed with "Controller"</span></span>
* <span data-ttu-id="dbebb-117">類別繼承自類別，其名稱的後置字元"Controller"</span><span class="sxs-lookup"><span data-stu-id="dbebb-117">The class inherits from a class whose name is suffixed with "Controller"</span></span>
* <span data-ttu-id="dbebb-118">類別以裝飾`[Controller]`屬性</span><span class="sxs-lookup"><span data-stu-id="dbebb-118">The class is decorated with the `[Controller]` attribute</span></span>

<span data-ttu-id="dbebb-119">控制器類別不具有相關聯`[NonController]`屬性。</span><span class="sxs-lookup"><span data-stu-id="dbebb-119">A controller class must not have an associated `[NonController]` attribute.</span></span>

<span data-ttu-id="dbebb-120">控制站應該遵循[明確的相依性原則](http://deviq.com/explicit-dependencies-principle/)。</span><span class="sxs-lookup"><span data-stu-id="dbebb-120">Controllers should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span> <span data-ttu-id="dbebb-121">有幾種方法可以實作此原則。</span><span class="sxs-lookup"><span data-stu-id="dbebb-121">There are a couple approaches to implementing this principle.</span></span> <span data-ttu-id="dbebb-122">如果多個控制器的動作需要在相同的服務，請考慮使用[建構函式插入](xref:mvc/controllers/dependency-injection#constructor-injection)要求的相依性。</span><span class="sxs-lookup"><span data-stu-id="dbebb-122">If multiple controller actions require the same service, consider using [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to request those dependencies.</span></span> <span data-ttu-id="dbebb-123">如果只有單一動作方法所需的服務，請考慮使用[動作資料隱碼](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)要求的相依性。</span><span class="sxs-lookup"><span data-stu-id="dbebb-123">If the service is needed by only a single action method, consider using [Action Injection](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) to request the dependency.</span></span>

<span data-ttu-id="dbebb-124">內**M**模型-**V**檢視-**C**ontroller 模式控制站會負責初始要求的處理和具現化的模型。</span><span class="sxs-lookup"><span data-stu-id="dbebb-124">Within the **M**odel-**V**iew-**C**ontroller pattern, a controller is responsible for the initial processing of the request and instantiation of the model.</span></span> <span data-ttu-id="dbebb-125">一般而言，模型內，應執行的商業決策。</span><span class="sxs-lookup"><span data-stu-id="dbebb-125">Generally, business decisions should be performed within the model.</span></span>

<span data-ttu-id="dbebb-126">控制器使用模型的處理 （如果有的話） 的結果，並傳回適當的檢視和其相關聯的檢視資料或應用程式開發介面呼叫的結果。</span><span class="sxs-lookup"><span data-stu-id="dbebb-126">The controller takes the result of the model's processing (if any) and returns either the proper view and its associated view data or the result of the API call.</span></span> <span data-ttu-id="dbebb-127">進一步了解在[ASP.NET Core MVC 概觀](xref:mvc/overview)和[開始使用 ASP.NET Core MVC 和 Visual Studio](xref:tutorials/first-mvc-app/start-mvc)。</span><span class="sxs-lookup"><span data-stu-id="dbebb-127">Learn more at [Overview of ASP.NET Core MVC](xref:mvc/overview) and [Getting started with ASP.NET Core MVC and Visual Studio](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="dbebb-128">控制器是*UI 層級*抽象概念。</span><span class="sxs-lookup"><span data-stu-id="dbebb-128">The controller is a *UI-level* abstraction.</span></span> <span data-ttu-id="dbebb-129">其負責的部分是以確保要求的資料無效，並選擇應傳回哪些檢視 （或應用程式開發介面的結果）。</span><span class="sxs-lookup"><span data-stu-id="dbebb-129">Its responsibilities are to ensure request data is valid and to choose which view (or result for an API) should be returned.</span></span> <span data-ttu-id="dbebb-130">在構造良好的應用程式，它不會直接包含資料存取或商務邏輯。</span><span class="sxs-lookup"><span data-stu-id="dbebb-130">In well-factored apps, it does not directly include data access or business logic.</span></span> <span data-ttu-id="dbebb-131">相反地，服務處理這些責任委派給控制器。</span><span class="sxs-lookup"><span data-stu-id="dbebb-131">Instead, the controller delegates to services handling these responsibilities.</span></span>

## <a name="defining-actions"></a><span data-ttu-id="dbebb-132">定義動作</span><span class="sxs-lookup"><span data-stu-id="dbebb-132">Defining Actions</span></span>

<span data-ttu-id="dbebb-133">控制站上的公用方法，但不包括使用裝飾`[NonAction]`屬性，是動作。</span><span class="sxs-lookup"><span data-stu-id="dbebb-133">Public methods on a controller, except those decorated with the `[NonAction]` attribute, are actions.</span></span> <span data-ttu-id="dbebb-134">在 [動作] 參數會要求資料繫結，並使用驗證的[模型繫結](xref:mvc/models/model-binding)。</span><span class="sxs-lookup"><span data-stu-id="dbebb-134">Parameters on actions are bound to request data and are validated using [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="dbebb-135">模型驗證就會發生的模型繫結的所有項目。</span><span class="sxs-lookup"><span data-stu-id="dbebb-135">Model validation occurs for everything that's model-bound.</span></span> <span data-ttu-id="dbebb-136">`ModelState.IsValid`屬性值表示模型繫結和驗證是否成功。</span><span class="sxs-lookup"><span data-stu-id="dbebb-136">The `ModelState.IsValid` property value indicates whether model binding and validation succeeded.</span></span>

<span data-ttu-id="dbebb-137">動作方法應該包含邏輯將要求對應至商務問題。</span><span class="sxs-lookup"><span data-stu-id="dbebb-137">Action methods should contain logic for mapping a request to a business concern.</span></span> <span data-ttu-id="dbebb-138">透過存取控制站的服務應該通常表示業務疑慮[相依性插入](xref:mvc/controllers/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="dbebb-138">Business concerns should typically be represented as services that the controller accesses through [dependency injection](xref:mvc/controllers/dependency-injection).</span></span> <span data-ttu-id="dbebb-139">然後，動作會將商務動作的結果對應至應用程式狀態。</span><span class="sxs-lookup"><span data-stu-id="dbebb-139">Actions then map the result of the business action to an application state.</span></span>

<span data-ttu-id="dbebb-140">動作可以傳回任何項目，但經常傳回的執行個體`IActionResult`(或`Task<IActionResult>`非同步方法) 產生回應。</span><span class="sxs-lookup"><span data-stu-id="dbebb-140">Actions can return anything, but frequently return an instance of `IActionResult` (or `Task<IActionResult>` for async methods) that produces a response.</span></span> <span data-ttu-id="dbebb-141">動作方法會負責選擇*何種回應*。</span><span class="sxs-lookup"><span data-stu-id="dbebb-141">The action method is responsible for choosing *what kind of response*.</span></span> <span data-ttu-id="dbebb-142">在動作結果*未回應*。</span><span class="sxs-lookup"><span data-stu-id="dbebb-142">The action result *does the responding*.</span></span>

### <a name="controller-helper-methods"></a><span data-ttu-id="dbebb-143">控制器的 Helper 方法</span><span class="sxs-lookup"><span data-stu-id="dbebb-143">Controller Helper Methods</span></span>

<span data-ttu-id="dbebb-144">控制站通常是繼承自[控制器](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller)，不過這並非必要。</span><span class="sxs-lookup"><span data-stu-id="dbebb-144">Controllers usually inherit from [Controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller), although this is not required.</span></span> <span data-ttu-id="dbebb-145">衍生自`Controller`提供三種 helper 方法的存取：</span><span class="sxs-lookup"><span data-stu-id="dbebb-145">Deriving from `Controller` provides access to three categories of helper methods:</span></span>

#### <a name="1-methods-resulting-in-an-empty-response-body"></a><span data-ttu-id="dbebb-146">1.產生空的回應主體中的方法</span><span class="sxs-lookup"><span data-stu-id="dbebb-146">1. Methods resulting in an empty response body</span></span>

<span data-ttu-id="dbebb-147">否`Content-Type`HTTP 回應標頭都會包括在內，因為回應主體缺少來描述的內容。</span><span class="sxs-lookup"><span data-stu-id="dbebb-147">No `Content-Type` HTTP response header is included, since the response body lacks content to describe.</span></span>

<span data-ttu-id="dbebb-148">此類別內的兩種結果類型： 重新導向與 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="dbebb-148">There are two result types within this category: Redirect and HTTP Status Code.</span></span>

* <span data-ttu-id="dbebb-149">**HTTP 狀態碼**</span><span class="sxs-lookup"><span data-stu-id="dbebb-149">**HTTP Status Code**</span></span>

    <span data-ttu-id="dbebb-150">這種類型會傳回 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="dbebb-150">This type returns an HTTP status code.</span></span> <span data-ttu-id="dbebb-151">此類型的一些協助程式方法都是`BadRequest`， `NotFound`，和`Ok`。</span><span class="sxs-lookup"><span data-stu-id="dbebb-151">A couple helper methods of this type are `BadRequest`, `NotFound`, and `Ok`.</span></span> <span data-ttu-id="dbebb-152">例如，`return BadRequest();`產生 400 狀態碼執行時。</span><span class="sxs-lookup"><span data-stu-id="dbebb-152">For example, `return BadRequest();` produces a 400 status code when executed.</span></span> <span data-ttu-id="dbebb-153">當這類方法`BadRequest`， `NotFound`，和`Ok`會多載，它們不會再限定為 HTTP 狀態碼回應，因為執行內容交涉。</span><span class="sxs-lookup"><span data-stu-id="dbebb-153">When methods such as `BadRequest`, `NotFound`, and `Ok` are overloaded, they no longer qualify as HTTP Status Code responders, since content negotiation is taking place.</span></span>

* <span data-ttu-id="dbebb-154">**重新導向**</span><span class="sxs-lookup"><span data-stu-id="dbebb-154">**Redirect**</span></span>

    <span data-ttu-id="dbebb-155">這種類型會傳回重新導向到某個動作或目的地 (使用`Redirect`， `LocalRedirect`， `RedirectToAction`，或`RedirectToRoute`)。</span><span class="sxs-lookup"><span data-stu-id="dbebb-155">This type returns a redirect to an action or destination (using `Redirect`, `LocalRedirect`, `RedirectToAction`, or `RedirectToRoute`).</span></span> <span data-ttu-id="dbebb-156">例如，`return RedirectToAction("Complete", new {id = 123});`重新導向至`Complete`，傳遞匿名物件。</span><span class="sxs-lookup"><span data-stu-id="dbebb-156">For example, `return RedirectToAction("Complete", new {id = 123});` redirects to `Complete`, passing an anonymous object.</span></span>

    <span data-ttu-id="dbebb-157">重新導向結果型別不同於 HTTP 狀態碼類型中加入主要`Location`HTTP 回應標頭。</span><span class="sxs-lookup"><span data-stu-id="dbebb-157">The Redirect result type differs from the HTTP Status Code type primarily in the addition of a `Location` HTTP response header.</span></span>

#### <a name="2-methods-resulting-in-a-non-empty-response-body-with-a-predefined-content-type"></a><span data-ttu-id="dbebb-158">2.導致非空白的回應主體的預先定義的內容類型的方法</span><span class="sxs-lookup"><span data-stu-id="dbebb-158">2. Methods resulting in a non-empty response body with a predefined content type</span></span>

<span data-ttu-id="dbebb-159">此類別中的大多數協助程式方法包括`ContentType`屬性，可讓您設定`Content-Type`來描述回應主體的回應標頭。</span><span class="sxs-lookup"><span data-stu-id="dbebb-159">Most helper methods in this category include a `ContentType` property, allowing you to set the `Content-Type` response header to describe the response body.</span></span>

<span data-ttu-id="dbebb-160">此類別內的兩種結果類型：[檢視](xref:mvc/views/overview)和[格式化回應](xref:mvc/models/formatting)。</span><span class="sxs-lookup"><span data-stu-id="dbebb-160">There are two result types within this category: [View](xref:mvc/views/overview) and [Formatted Response](xref:mvc/models/formatting).</span></span>

* <span data-ttu-id="dbebb-161">**檢視**</span><span class="sxs-lookup"><span data-stu-id="dbebb-161">**View**</span></span>

    <span data-ttu-id="dbebb-162">這種類型傳回使用模型來呈現 HTML 的檢視表。</span><span class="sxs-lookup"><span data-stu-id="dbebb-162">This type returns a view which uses a model to render HTML.</span></span> <span data-ttu-id="dbebb-163">例如，`return View(customer);`將模型傳遞至資料繫結的檢視。</span><span class="sxs-lookup"><span data-stu-id="dbebb-163">For example, `return View(customer);` passes a model to the view for data-binding.</span></span>

* <span data-ttu-id="dbebb-164">**格式化的回應**</span><span class="sxs-lookup"><span data-stu-id="dbebb-164">**Formatted Response**</span></span>

    <span data-ttu-id="dbebb-165">這種類型會傳回以特定方式代表物件的 JSON 或類似的資料交換格式。</span><span class="sxs-lookup"><span data-stu-id="dbebb-165">This type returns JSON or a similar data exchange format to represent an object in a specific manner.</span></span> <span data-ttu-id="dbebb-166">例如，`return Json(customer);`所提供的物件序列化為 JSON 格式。</span><span class="sxs-lookup"><span data-stu-id="dbebb-166">For example, `return Json(customer);` serializes the provided object into JSON format.</span></span>
    
    <span data-ttu-id="dbebb-167">此類型的其他常用的方法包括`File`， `PhysicalFile`，和`VirtualFile`。</span><span class="sxs-lookup"><span data-stu-id="dbebb-167">Other common methods of this type include `File`, `PhysicalFile`, and `VirtualFile`.</span></span> <span data-ttu-id="dbebb-168">例如，`return PhysicalFile(customerFilePath, "text/xml");`傳回所描述的 XML 檔案`Content-Type`"text/xml"的回應標頭值。</span><span class="sxs-lookup"><span data-stu-id="dbebb-168">For example, `return PhysicalFile(customerFilePath, "text/xml");` returns an XML file described by a `Content-Type` response header value of "text/xml".</span></span>

#### <a name="3-methods-resulting-in-a-non-empty-response-body-formatted-in-a-content-type-negotiated-with-the-client"></a><span data-ttu-id="dbebb-169">3.方法所產生的非空白的回應主體中格式化與用戶端交涉的內容類型</span><span class="sxs-lookup"><span data-stu-id="dbebb-169">3. Methods resulting in a non-empty response body formatted in a content type negotiated with the client</span></span>

<span data-ttu-id="dbebb-170">這個類別是更好稱為**內容交涉**。</span><span class="sxs-lookup"><span data-stu-id="dbebb-170">This category is better known as **Content Negotiation**.</span></span> <span data-ttu-id="dbebb-171">[內容交涉](xref:mvc/models/formatting#content-negotiation)套用時的動作傳回[ObjectResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.objectresult)類型或內容以外[IActionResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iactionresult)實作。</span><span class="sxs-lookup"><span data-stu-id="dbebb-171">[Content negotiation](xref:mvc/models/formatting#content-negotiation) applies whenever an action returns an [ObjectResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.objectresult) type or something other than an [IActionResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iactionresult) implementation.</span></span> <span data-ttu-id="dbebb-172">傳回非動作`IActionResult`實作 (例如， `object`) 也會傳回格式化的回應。</span><span class="sxs-lookup"><span data-stu-id="dbebb-172">An action that returns a non-`IActionResult` implementation (for example, `object`) also returns a Formatted Response.</span></span>

<span data-ttu-id="dbebb-173">此類型的一些協助程式方法包括`BadRequest`， `CreatedAtRoute`，和`Ok`。</span><span class="sxs-lookup"><span data-stu-id="dbebb-173">Some helper methods of this type include `BadRequest`, `CreatedAtRoute`, and `Ok`.</span></span> <span data-ttu-id="dbebb-174">這些方法的範例包括`return BadRequest(modelState);`， `return CreatedAtRoute("routename", values, newobject);`，和`return Ok(value);`分別。</span><span class="sxs-lookup"><span data-stu-id="dbebb-174">Examples of these methods include `return BadRequest(modelState);`, `return CreatedAtRoute("routename", values, newobject);`, and `return Ok(value);`, respectively.</span></span> <span data-ttu-id="dbebb-175">請注意，`BadRequest`和`Ok`執行內容交涉的值傳遞時，才; 而不傳遞一個值，它們而是做為 HTTP 狀態碼的結果型別。</span><span class="sxs-lookup"><span data-stu-id="dbebb-175">Note that `BadRequest` and `Ok` perform content negotiation only when passed a value; without being passed a value, they instead serve as HTTP Status Code result types.</span></span> <span data-ttu-id="dbebb-176">`CreatedAtRoute`方法時，相反地，一律會執行內容交涉，因為所有需要的值將會收到其多載。</span><span class="sxs-lookup"><span data-stu-id="dbebb-176">The `CreatedAtRoute` method, on the other hand, always performs content negotiation since its overloads all require that a value be passed.</span></span>

### <a name="cross-cutting-concerns"></a><span data-ttu-id="dbebb-177">跨領域考量</span><span class="sxs-lookup"><span data-stu-id="dbebb-177">Cross-Cutting Concerns</span></span>

<span data-ttu-id="dbebb-178">應用程式通常會共用工作流程的組件。</span><span class="sxs-lookup"><span data-stu-id="dbebb-178">Applications typically share parts of their workflow.</span></span> <span data-ttu-id="dbebb-179">範例包括應用程式需要驗證存取購物車，或是某些頁面上的資料會快取的應用程式。</span><span class="sxs-lookup"><span data-stu-id="dbebb-179">Examples include an app that requires authentication to access the shopping cart, or an app that caches data on some pages.</span></span> <span data-ttu-id="dbebb-180">若要執行的邏輯，在動作方法的前後，使用*篩選*。</span><span class="sxs-lookup"><span data-stu-id="dbebb-180">To perform logic before or after an action method, use a *filter*.</span></span> <span data-ttu-id="dbebb-181">使用[篩選](xref:mvc/controllers/filters)交互碼橫切入顧慮上可以減少重複，讓他們遵循[不重複自行 （乾） 原則](http://deviq.com/don-t-repeat-yourself/)。</span><span class="sxs-lookup"><span data-stu-id="dbebb-181">Using [Filters](xref:mvc/controllers/filters) on cross-cutting concerns can reduce duplication, allowing them to follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="dbebb-182">最篩選屬性，例如`[Authorize]`，可以套用在控制器或動作層級所需的資料粒度層級而定。</span><span class="sxs-lookup"><span data-stu-id="dbebb-182">Most filter attributes, such as `[Authorize]`, can be applied at the controller or action level depending upon the desired level of granularity.</span></span>

<span data-ttu-id="dbebb-183">錯誤處理和回應的快取通常會跨領域考量：</span><span class="sxs-lookup"><span data-stu-id="dbebb-183">Error handling and response caching are often cross-cutting concerns:</span></span>
   * [<span data-ttu-id="dbebb-184">錯誤處理</span><span class="sxs-lookup"><span data-stu-id="dbebb-184">Error handling</span></span>](xref:mvc/controllers/filters#exception-filters)
   * [<span data-ttu-id="dbebb-185">回應快取</span><span class="sxs-lookup"><span data-stu-id="dbebb-185">Response Caching</span></span>](xref:performance/caching/response)

<span data-ttu-id="dbebb-186">可以使用篩選器或自訂處理許多跨領域考量[中介軟體](xref:fundamentals/middleware)。</span><span class="sxs-lookup"><span data-stu-id="dbebb-186">Many cross-cutting concerns can be handled using filters or custom [middleware](xref:fundamentals/middleware).</span></span>
