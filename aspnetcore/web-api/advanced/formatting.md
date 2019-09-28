---
title: 在 ASP.NET Core Web API 中格式化回應資料
author: ardalis
description: 了解如何在 ASP.NET Core Web API 中格式化回應資料。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 8/22/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: e503df3d81efbb2800503c0cb4ff5ae093b6e1ac
ms.sourcegitcommit: 023495344053dc59115c80538f0ece935e7490a2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2019
ms.locfileid: "71592357"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a><span data-ttu-id="84dce-103">在 ASP.NET Core Web API 中格式化回應資料</span><span class="sxs-lookup"><span data-stu-id="84dce-103">Format response data in ASP.NET Core Web API</span></span>

<span data-ttu-id="84dce-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫</span><span class="sxs-lookup"><span data-stu-id="84dce-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="84dce-105">ASP.NET Core MVC 支援格式化回應資料。</span><span class="sxs-lookup"><span data-stu-id="84dce-105">ASP.NET Core MVC has support for formatting response data.</span></span> <span data-ttu-id="84dce-106">您可以使用特定格式或回應用戶端要求的格式來格式化回應資料。</span><span class="sxs-lookup"><span data-stu-id="84dce-106">Response data can be formatted using specific formats or in response to client requested format.</span></span>

<span data-ttu-id="84dce-107">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="84dce-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="84dce-108">特定格式的動作結果</span><span class="sxs-lookup"><span data-stu-id="84dce-108">Format-specific Action Results</span></span>

<span data-ttu-id="84dce-109">某些動作結果類型是特定格式所特有的，例如 <xref:Microsoft.AspNetCore.Mvc.JsonResult> 和 <xref:Microsoft.AspNetCore.Mvc.ContentResult>。</span><span class="sxs-lookup"><span data-stu-id="84dce-109">Some action result types are specific to a particular format, such as <xref:Microsoft.AspNetCore.Mvc.JsonResult> and <xref:Microsoft.AspNetCore.Mvc.ContentResult>.</span></span> <span data-ttu-id="84dce-110">無論用戶端喜好設定為何，動作都可以傳回以特定格式格式化的結果。</span><span class="sxs-lookup"><span data-stu-id="84dce-110">Actions can return results that are formatted in a particular format, regardless of client preferences.</span></span> <span data-ttu-id="84dce-111">例如， `JsonResult`傳回會傳回 JSON 格式的資料。</span><span class="sxs-lookup"><span data-stu-id="84dce-111">For example, returning `JsonResult` returns JSON-formatted data.</span></span> <span data-ttu-id="84dce-112">傳回`ContentResult`或字串會傳回純文字格式的字串資料。</span><span class="sxs-lookup"><span data-stu-id="84dce-112">Returning `ContentResult` or a string returns plain-text-formatted string data.</span></span>

<span data-ttu-id="84dce-113">動作不需要傳回任何特定的類型。</span><span class="sxs-lookup"><span data-stu-id="84dce-113">An action isn't required to return any specific type.</span></span> <span data-ttu-id="84dce-114">ASP.NET Core 支援任何物件傳回值。</span><span class="sxs-lookup"><span data-stu-id="84dce-114">ASP.NET Core supports any object return value.</span></span>  <span data-ttu-id="84dce-115">傳回不<xref:Microsoft.AspNetCore.Mvc.IActionResult>是型別物件之動作的結果，會使用適當<xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter>的實作為序列化。</span><span class="sxs-lookup"><span data-stu-id="84dce-115">Results from actions that return objects that are not <xref:Microsoft.AspNetCore.Mvc.IActionResult> types are serialized using the appropriate <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> implementation.</span></span> <span data-ttu-id="84dce-116">如需詳細資訊，請參閱<xref:web-api/action-return-types>。</span><span class="sxs-lookup"><span data-stu-id="84dce-116">For more information, see <xref:web-api/action-return-types>.</span></span>

<span data-ttu-id="84dce-117">內建 helper 方法<xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*>會傳回 JSON 格式的資料：[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]</span><span class="sxs-lookup"><span data-stu-id="84dce-117">The built-in helper method <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> returns JSON-formatted data: [!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]</span></span>

<span data-ttu-id="84dce-118">下載範例會傳回作者清單。</span><span class="sxs-lookup"><span data-stu-id="84dce-118">The sample download returns the list of authors.</span></span> <span data-ttu-id="84dce-119">使用 F12 瀏覽器開發人員工具或[Postman](https://www.getpostman.com/tools)搭配先前的程式碼：</span><span class="sxs-lookup"><span data-stu-id="84dce-119">Using the F12 browser developer tools or [Postman](https://www.getpostman.com/tools) with the previous code:</span></span>

* <span data-ttu-id="84dce-120">顯示包含**內容類型：** `application/json; charset=utf-8`的回應標頭。</span><span class="sxs-lookup"><span data-stu-id="84dce-120">The response header containing **content-type:** `application/json; charset=utf-8` is displayed.</span></span>
* <span data-ttu-id="84dce-121">系統會顯示要求標頭。</span><span class="sxs-lookup"><span data-stu-id="84dce-121">The request headers are displayed.</span></span> <span data-ttu-id="84dce-122">例如， `Accept`標頭。</span><span class="sxs-lookup"><span data-stu-id="84dce-122">For example, the `Accept` header.</span></span> <span data-ttu-id="84dce-123">前面的程式碼會忽略標頭。`Accept`</span><span class="sxs-lookup"><span data-stu-id="84dce-123">The `Accept` header is ignored by the preceding code.</span></span>

<span data-ttu-id="84dce-124">若要傳回純文字格式化資料，請使用 <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> 和 <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> 協助程式：</span><span class="sxs-lookup"><span data-stu-id="84dce-124">To return plain text formatted data, use <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> and the <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> helper:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_about)]

<span data-ttu-id="84dce-125">在上述程式碼中， `Content-Type`傳回的`text/plain`是。</span><span class="sxs-lookup"><span data-stu-id="84dce-125">In the preceding code, the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="84dce-126">傳回字串傳遞`Content-Type` `text/plain`下列內容：</span><span class="sxs-lookup"><span data-stu-id="84dce-126">Returning a string delivers `Content-Type` of `text/plain`:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_string)]

<span data-ttu-id="84dce-127">針對具有多個傳回類型的動作`IActionResult`，傳回。</span><span class="sxs-lookup"><span data-stu-id="84dce-127">For actions with multiple return types, return `IActionResult`.</span></span> <span data-ttu-id="84dce-128">例如，根據執行的作業結果傳回不同的 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="84dce-128">For example, returning different HTTP status codes based on the result of operations performed.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="84dce-129">內容協調</span><span class="sxs-lookup"><span data-stu-id="84dce-129">Content negotiation</span></span>

<span data-ttu-id="84dce-130">當用戶端指定[Accept 標頭](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)時，就會發生內容協商。</span><span class="sxs-lookup"><span data-stu-id="84dce-130">Content negotiation occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="84dce-131">ASP.NET Core 使用的預設格式為[JSON](https://json.org/)。</span><span class="sxs-lookup"><span data-stu-id="84dce-131">The default format used by ASP.NET Core is [JSON](https://json.org/).</span></span> <span data-ttu-id="84dce-132">內容協商為：</span><span class="sxs-lookup"><span data-stu-id="84dce-132">Content negotiation is:</span></span>

* <span data-ttu-id="84dce-133">由<xref:Microsoft.AspNetCore.Mvc.ObjectResult>執行。</span><span class="sxs-lookup"><span data-stu-id="84dce-133">Implemented by <xref:Microsoft.AspNetCore.Mvc.ObjectResult>.</span></span>
* <span data-ttu-id="84dce-134">內建自 helper 方法所傳回的狀態碼特定動作結果。</span><span class="sxs-lookup"><span data-stu-id="84dce-134">Built into the status code-specific action results returned from the helper methods.</span></span> <span data-ttu-id="84dce-135">動作結果 helper 方法是以為基礎`ObjectResult`。</span><span class="sxs-lookup"><span data-stu-id="84dce-135">The action results helper methods are based on `ObjectResult`.</span></span>

<span data-ttu-id="84dce-136">傳回模型型別時，傳回型別會是`ObjectResult`。</span><span class="sxs-lookup"><span data-stu-id="84dce-136">When a model type is returned,  the return type is `ObjectResult`.</span></span>

<span data-ttu-id="84dce-137">下列動作方法使用 `Ok` 和 `NotFound` 協助程式方法：</span><span class="sxs-lookup"><span data-stu-id="84dce-137">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_search)]

<span data-ttu-id="84dce-138">除非要求另一個格式，且伺服器可以傳回要求的格式，否則會傳回 JSON 格式的回應。</span><span class="sxs-lookup"><span data-stu-id="84dce-138">A JSON-formatted response is returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="84dce-139">[Fiddler](https://www.telerik.com/fiddler)或[Postman](https://www.getpostman.com/tools)這類工具`Accept`可以設定標頭來指定傳回格式。</span><span class="sxs-lookup"><span data-stu-id="84dce-139">Tools such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/tools) can set the `Accept` header to specify the return format.</span></span> <span data-ttu-id="84dce-140">`Accept`當包含伺服器支援的類型時，就會傳回該類型。</span><span class="sxs-lookup"><span data-stu-id="84dce-140">When the `Accept` contains a type the server supports, that type is returned.</span></span>

<span data-ttu-id="84dce-141">根據預設，ASP.NET Core 只支援 JSON。</span><span class="sxs-lookup"><span data-stu-id="84dce-141">By default, ASP.NET Core only supports JSON.</span></span> <span data-ttu-id="84dce-142">對於不會變更預設值的應用程式，一律會傳回 JSON 格式的回應，而不論用戶端要求為何。</span><span class="sxs-lookup"><span data-stu-id="84dce-142">For apps not changing the default, JSON-formatted responses are alway returned regardless of the client request.</span></span> <span data-ttu-id="84dce-143">下一節將說明如何新增其他格式器。</span><span class="sxs-lookup"><span data-stu-id="84dce-143">The next section shows how to add additional formatters.</span></span>

<span data-ttu-id="84dce-144">控制器動作可以傳回 Poco （簡單的 CLR 物件）。</span><span class="sxs-lookup"><span data-stu-id="84dce-144">Controller actions can return POCOs (Plain Old CLR Objects).</span></span> <span data-ttu-id="84dce-145">當 POCO 傳回時，執行時間會自動建立`ObjectResult`包裝物件的。</span><span class="sxs-lookup"><span data-stu-id="84dce-145">When a POCO is returned, the runtime automatically creates an `ObjectResult` that wraps the object.</span></span> <span data-ttu-id="84dce-146">用戶端會取得已格式化的序列化物件。</span><span class="sxs-lookup"><span data-stu-id="84dce-146">The client gets the formatted serialized object.</span></span> <span data-ttu-id="84dce-147">如果傳回的物件是`null` `204 No Content` ，則會傳迴響應。</span><span class="sxs-lookup"><span data-stu-id="84dce-147">If the object being returned is `null`, a `204 No Content` response is returned.</span></span>

<span data-ttu-id="84dce-148">傳回物件類型：</span><span class="sxs-lookup"><span data-stu-id="84dce-148">Returning an object type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_alias)]

<span data-ttu-id="84dce-149">在上述程式碼中，有效作者別名`200 OK`的要求會傳回含有作者資料的回應。</span><span class="sxs-lookup"><span data-stu-id="84dce-149">In the preceding code, a request for a valid author alias returns a `200 OK` response with the author's data.</span></span> <span data-ttu-id="84dce-150">對無效別名`204 No Content`的要求會傳迴響應。</span><span class="sxs-lookup"><span data-stu-id="84dce-150">A request for an invalid alias returns a `204 No Content` response.</span></span>

### <a name="the-accept-header"></a><span data-ttu-id="84dce-151">Accept 標頭</span><span class="sxs-lookup"><span data-stu-id="84dce-151">The Accept header</span></span>

<span data-ttu-id="84dce-152">當要求中出現`Accept`標頭時，就會進行內容協商。</span><span class="sxs-lookup"><span data-stu-id="84dce-152">Content *negotiation* takes place when an `Accept` header appears in the request.</span></span> <span data-ttu-id="84dce-153">當要求包含 accept 標頭時，ASP.NET Core：</span><span class="sxs-lookup"><span data-stu-id="84dce-153">When a request contains an accept header, ASP.NET Core:</span></span>

* <span data-ttu-id="84dce-154">依照喜好設定順序，列舉 accept 標頭中的媒體類型。</span><span class="sxs-lookup"><span data-stu-id="84dce-154">Enumerates the media types in the accept header in preference order.</span></span>
* <span data-ttu-id="84dce-155">嘗試尋找可以使用其中一種指定的格式產生回應的格式器。</span><span class="sxs-lookup"><span data-stu-id="84dce-155">Tries to find a formatter that can produce a response in one of the formats specified.</span></span>

<span data-ttu-id="84dce-156">如果找不到可滿足用戶端要求的格式器，請 ASP.NET Core：</span><span class="sxs-lookup"><span data-stu-id="84dce-156">If no formatter is found that can satisfy the client's request, ASP.NET Core:</span></span>

* <span data-ttu-id="84dce-157">`406 Not Acceptable` 如果<xref:Microsoft.AspNetCore.Mvc.MvcOptions>已設定，則會傳回，或-</span><span class="sxs-lookup"><span data-stu-id="84dce-157">Returns `406 Not Acceptable` if <xref:Microsoft.AspNetCore.Mvc.MvcOptions> has been set, or -</span></span>
* <span data-ttu-id="84dce-158">嘗試尋找可產生回應的第一個格式器。</span><span class="sxs-lookup"><span data-stu-id="84dce-158">Tries to find the first formatter that can produce a response.</span></span>

<span data-ttu-id="84dce-159">如果未針對要求的格式設定格式器，則會使用可格式化物件的第一個格式器。</span><span class="sxs-lookup"><span data-stu-id="84dce-159">If no formatter is configured for the requested format, the first formatter that can format the object is used.</span></span> <span data-ttu-id="84dce-160">如果要求`Accept`中未出現標頭：</span><span class="sxs-lookup"><span data-stu-id="84dce-160">If no `Accept` header appears in the request:</span></span>

* <span data-ttu-id="84dce-161">第一個可以處理物件的格式器是用來序列化回應。</span><span class="sxs-lookup"><span data-stu-id="84dce-161">The first formatter that can handle the object is used to serialize the response.</span></span>
* <span data-ttu-id="84dce-162">沒有任何進行中的協商。</span><span class="sxs-lookup"><span data-stu-id="84dce-162">There isn't any negotiation taking place.</span></span> <span data-ttu-id="84dce-163">伺服器正在決定要傳回的格式。</span><span class="sxs-lookup"><span data-stu-id="84dce-163">The server is determining what format to return.</span></span>

<span data-ttu-id="84dce-164">如果 Accept 標頭包含`*/*`，除非在上<xref:Microsoft.AspNetCore.Mvc.MvcOptions>將設`RespectBrowserAcceptHeader`為 true，否則會忽略標頭。</span><span class="sxs-lookup"><span data-stu-id="84dce-164">If the Accept header contains `*/*`, the Header is ignored unless `RespectBrowserAcceptHeader` is set to true on <xref:Microsoft.AspNetCore.Mvc.MvcOptions>.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="84dce-165">瀏覽器和內容協調</span><span class="sxs-lookup"><span data-stu-id="84dce-165">Browsers and content negotiation</span></span>

<span data-ttu-id="84dce-166">與一般 API 用戶端不同的是`Accept` ，網頁瀏覽器會提供標頭。</span><span class="sxs-lookup"><span data-stu-id="84dce-166">Unlike typical API clients, web browsers supply `Accept` headers.</span></span> <span data-ttu-id="84dce-167">網頁瀏覽器會指定許多格式，包括萬用字元。</span><span class="sxs-lookup"><span data-stu-id="84dce-167">Web browser specify many formats, including wildcards.</span></span> <span data-ttu-id="84dce-168">根據預設，當架構偵測到要求來自瀏覽器時：</span><span class="sxs-lookup"><span data-stu-id="84dce-168">By default, when the framework detects that the request is coming from a browser:</span></span>

* <span data-ttu-id="84dce-169">已`Accept`忽略標頭。</span><span class="sxs-lookup"><span data-stu-id="84dce-169">The `Accept` header is ignored.</span></span>
* <span data-ttu-id="84dce-170">除非另有設定，否則內容會以 JSON 格式傳回。</span><span class="sxs-lookup"><span data-stu-id="84dce-170">The content is returned in JSON, unless otherwise configured.</span></span>

<span data-ttu-id="84dce-171">使用 Api 時，這會在瀏覽器之間提供更一致的體驗。</span><span class="sxs-lookup"><span data-stu-id="84dce-171">This provides a more consistent experience across browsers when consuming APIs.</span></span>

<span data-ttu-id="84dce-172">若要設定應用程式以接受瀏覽器 accept 標<xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader>頭`true`，請將設為：</span><span class="sxs-lookup"><span data-stu-id="84dce-172">To configure an app to honor browser accept headers, set <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> to `true`:</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end

### <a name="configure-formatters"></a><span data-ttu-id="84dce-173">設定格式器</span><span class="sxs-lookup"><span data-stu-id="84dce-173">Configure formatters</span></span>

<span data-ttu-id="84dce-174">需要支援其他格式的應用程式可以新增適當的 NuGet 套件，並設定支援。</span><span class="sxs-lookup"><span data-stu-id="84dce-174">Apps that need to support additional formats can add the appropriate NuGet packages and configure support.</span></span> <span data-ttu-id="84dce-175">輸入和輸出有個別的格式器。</span><span class="sxs-lookup"><span data-stu-id="84dce-175">There are separate formatters for input and output.</span></span> <span data-ttu-id="84dce-176">[模型](xref:mvc/models/model-binding)系結會使用輸入格式器。</span><span class="sxs-lookup"><span data-stu-id="84dce-176">Input formatters are used by [Model Binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="84dce-177">輸出格式器是用來格式化回應。</span><span class="sxs-lookup"><span data-stu-id="84dce-177">Output formatters are used to format responses.</span></span> <span data-ttu-id="84dce-178">如需建立自訂格式器的詳細資訊，請參閱[自訂格式化](xref:web-api/advanced/custom-formatters)器。</span><span class="sxs-lookup"><span data-stu-id="84dce-178">For information on creating a custom formatter, see [Custom Formatters](xref:web-api/advanced/custom-formatters).</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="add-xml-format-support"></a><span data-ttu-id="84dce-179">新增 XML 格式支援</span><span class="sxs-lookup"><span data-stu-id="84dce-179">Add XML format support</span></span>

<span data-ttu-id="84dce-180">使用<xref:System.Xml.Serialization.XmlSerializer>執行的 XML 格式器是透過<xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>呼叫來設定：</span><span class="sxs-lookup"><span data-stu-id="84dce-180">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/3.0sample/Startup.cs?name=snippet)]

<span data-ttu-id="84dce-181">上述程式碼會使用來`XmlSerializer`序列化結果。</span><span class="sxs-lookup"><span data-stu-id="84dce-181">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="84dce-182">使用上述程式碼時，控制器方法應該根據要求的`Accept`標頭傳回適當的格式。</span><span class="sxs-lookup"><span data-stu-id="84dce-182">When using the preceding code, controller methods should return the appropriate format based on the request's `Accept` header.</span></span>

### <a name="configure-systemtextjson-based-formatters"></a><span data-ttu-id="84dce-183">設定 System.Text.Json-based 格式器</span><span class="sxs-lookup"><span data-stu-id="84dce-183">Configure System.Text.Json-based formatters</span></span>

<span data-ttu-id="84dce-184">`System.Text.Json` 型格式器可以使用 `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions` 來設定。</span><span class="sxs-lookup"><span data-stu-id="84dce-184">Features for the `System.Text.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`.</span></span>

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    // Use the default property (Pascal) casing.
    options.SerializerOptions.PropertyNamingPolicy = null;

    // Configure a custom converter.
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="84dce-185">以每個動作為基礎的輸出序列化選項，可以使用`JsonResult`進行設定。</span><span class="sxs-lookup"><span data-stu-id="84dce-185">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="84dce-186">例如:</span><span class="sxs-lookup"><span data-stu-id="84dce-186">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerOptions
    {
        options.WriteIndented = true,
    });
}
```

### <a name="add-newtonsoftjson-based-json-format-support"></a><span data-ttu-id="84dce-187">新增 Newtonsoft.Json 型 JSON 格式支援</span><span class="sxs-lookup"><span data-stu-id="84dce-187">Add Newtonsoft.Json-based JSON format support</span></span>

<span data-ttu-id="84dce-188">在 ASP.NET Core 3.0 之前，預設使用的`Newtonsoft.Json` JSON 格式器會使用封裝來執行。</span><span class="sxs-lookup"><span data-stu-id="84dce-188">Prior to ASP.NET Core 3.0, the default used JSON formatters implemented using the `Newtonsoft.Json` package.</span></span> <span data-ttu-id="84dce-189">在 ASP.NET Core 3.0 或更新版本中，預設 JSON 格式器是以 `System.Text.Json` 為基礎。</span><span class="sxs-lookup"><span data-stu-id="84dce-189">In ASP.NET Core 3.0 or later, the default JSON formatters are based on `System.Text.Json`.</span></span> <span data-ttu-id="84dce-190">藉由`Newtonsoft.Json`安裝[NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet 套件並在中`Startup.ConfigureServices`進行設定，即可取得根據格式的格式器和功能的支援。</span><span class="sxs-lookup"><span data-stu-id="84dce-190">Support for `Newtonsoft.Json` based formatters and features is available by installing the [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package and configuring it in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](./formatting/3.0sample/StartupNewtonsoftJson.cs?name=snippet)]

<span data-ttu-id="84dce-191">某些功能可能不適用於以`System.Text.Json`為基礎的格式器，而且需要參考以為基礎的`Newtonsoft.Json`格式器。</span><span class="sxs-lookup"><span data-stu-id="84dce-191">Some features may not work well with `System.Text.Json`-based formatters and require a reference to the `Newtonsoft.Json`-based formatters.</span></span> <span data-ttu-id="84dce-192">如果應用程式`Newtonsoft.Json`有下列情況，請繼續使用以為基礎的格式器：</span><span class="sxs-lookup"><span data-stu-id="84dce-192">Continue using the `Newtonsoft.Json`-based formatters if the app:</span></span>

* <span data-ttu-id="84dce-193">使用`Newtonsoft.Json`屬性。</span><span class="sxs-lookup"><span data-stu-id="84dce-193">Uses `Newtonsoft.Json` attributes.</span></span> <span data-ttu-id="84dce-194">例如，`[JsonProperty]` 或 `[JsonIgnore]`。</span><span class="sxs-lookup"><span data-stu-id="84dce-194">For example, `[JsonProperty]` or `[JsonIgnore]`.</span></span>
* <span data-ttu-id="84dce-195">自訂序列化設定。</span><span class="sxs-lookup"><span data-stu-id="84dce-195">Customizes the serialization settings.</span></span>
* <span data-ttu-id="84dce-196">依賴提供的`Newtonsoft.Json`功能。</span><span class="sxs-lookup"><span data-stu-id="84dce-196">Relies on features that `Newtonsoft.Json` provides.</span></span>
* <span data-ttu-id="84dce-197">設定 `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`。</span><span class="sxs-lookup"><span data-stu-id="84dce-197">Configures `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span></span> <span data-ttu-id="84dce-198">在 ASP.NET Core 3.0 版之前，`JsonResult.SerializerSettings` 接受 `Newtonsoft.Json` 專屬的 `JsonSerializerSettings` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="84dce-198">Prior to ASP.NET Core 3.0, `JsonResult.SerializerSettings` accepts an instance of `JsonSerializerSettings` that is specific to `Newtonsoft.Json`.</span></span>
* <span data-ttu-id="84dce-199">產生 [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) 文件。</span><span class="sxs-lookup"><span data-stu-id="84dce-199">Generates [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) documentation.</span></span>

<span data-ttu-id="84dce-200">您可以使用`Newtonsoft.Json` `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`來設定以為基礎之格式器的功能：</span><span class="sxs-lookup"><span data-stu-id="84dce-200">Features for the `Newtonsoft.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:</span></span>

```csharp
services.AddControllers().AddNewtonsoftJson(options =>
{
    // Use the default property (Pascal) casing
    options.SerializerSettings.ContractResolver = new DefaultContractResolver();

    // Configure a custom converter
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="84dce-201">以每個動作為基礎的輸出序列化選項，可以使用`JsonResult`進行設定。</span><span class="sxs-lookup"><span data-stu-id="84dce-201">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="84dce-202">例如:</span><span class="sxs-lookup"><span data-stu-id="84dce-202">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerSettings
    {
        options.Formatting = Formatting.Indented,
    });
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

### <a name="add-xml-format-support"></a><span data-ttu-id="84dce-203">新增 XML 格式支援</span><span class="sxs-lookup"><span data-stu-id="84dce-203">Add XML format support</span></span>

<span data-ttu-id="84dce-204">XML 格式設定需要[AspNetCore 的 xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="84dce-204">XML formatting requires the [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet package.</span></span>

<span data-ttu-id="84dce-205">使用<xref:System.Xml.Serialization.XmlSerializer>執行的 XML 格式器是透過<xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>呼叫來設定：</span><span class="sxs-lookup"><span data-stu-id="84dce-205">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet)]

<span data-ttu-id="84dce-206">上述程式碼會使用來`XmlSerializer`序列化結果。</span><span class="sxs-lookup"><span data-stu-id="84dce-206">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="84dce-207">使用上述程式碼時，控制器方法應該根據要求的`Accept`標頭傳回適當的格式。</span><span class="sxs-lookup"><span data-stu-id="84dce-207">When using the preceding code, controller methods should return the appropriate format based on the request's `Accept` header.</span></span>

::: moniker-end

### <a name="specify-a-format"></a><span data-ttu-id="84dce-208">指定格式</span><span class="sxs-lookup"><span data-stu-id="84dce-208">Specify a format</span></span>

<span data-ttu-id="84dce-209">若要限制回應格式，請[`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute)套用篩選準則。</span><span class="sxs-lookup"><span data-stu-id="84dce-209">To restrict the response formats, apply the [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter.</span></span> <span data-ttu-id="84dce-210">就像大部分的`[Produces]` [篩選器](xref:mvc/controllers/filters)一樣，可以在動作、控制器或全域範圍套用：</span><span class="sxs-lookup"><span data-stu-id="84dce-210">Like most [Filters](xref:mvc/controllers/filters), `[Produces]` can be applied at the action, controller, or global scope:</span></span>

[!code-csharp[](./formatting/3.0sample/Controllers/WeatherForecastController.cs?name=snippet)]

<span data-ttu-id="84dce-211">上述[`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute)篩選準則：</span><span class="sxs-lookup"><span data-stu-id="84dce-211">The preceding [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter:</span></span>

* <span data-ttu-id="84dce-212">強制控制器內的所有動作傳回 JSON 格式的回應。</span><span class="sxs-lookup"><span data-stu-id="84dce-212">Forces all actions within the controller to return JSON-formatted responses.</span></span>
* <span data-ttu-id="84dce-213">如果設定了其他格式器，而且用戶端指定了不同的格式，則會傳回 JSON。</span><span class="sxs-lookup"><span data-stu-id="84dce-213">If other formatters are configured and the client specifies a different format, JSON is returned.</span></span>

<span data-ttu-id="84dce-214">如需詳細資訊，請參閱[篩選](xref:mvc/controllers/filters)。</span><span class="sxs-lookup"><span data-stu-id="84dce-214">For more information, see [Filters](xref:mvc/controllers/filters).</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="84dce-215">特殊案例格式器</span><span class="sxs-lookup"><span data-stu-id="84dce-215">Special case formatters</span></span>

<span data-ttu-id="84dce-216">有些特殊案例是使用內建格式器所實作。</span><span class="sxs-lookup"><span data-stu-id="84dce-216">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="84dce-217">根據預設， `string`傳回類型的格式為*text/純文字*（如果透過`Accept`標頭要求，則為*text/html* ）。</span><span class="sxs-lookup"><span data-stu-id="84dce-217">By default, `string` return types are formatted as *text/plain* (*text/html* if requested via the `Accept` header).</span></span> <span data-ttu-id="84dce-218">您可以藉由移除<xref:Microsoft.AspNetCore.Mvc.Formatters.TextOutputFormatter>來刪除這個行為。</span><span class="sxs-lookup"><span data-stu-id="84dce-218">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.TextOutputFormatter>.</span></span> <span data-ttu-id="84dce-219">已移除方法中的`Configure`格式器。</span><span class="sxs-lookup"><span data-stu-id="84dce-219">Formatters are removed in the `Configure` method.</span></span> <span data-ttu-id="84dce-220">`204 No Content` 傳回`null`時，具有模型物件傳回類型的動作會返回。</span><span class="sxs-lookup"><span data-stu-id="84dce-220">Actions that have a model object return type return `204 No Content` when returning `null`.</span></span> <span data-ttu-id="84dce-221">您可以藉由移除<xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>來刪除這個行為。</span><span class="sxs-lookup"><span data-stu-id="84dce-221">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>.</span></span> <span data-ttu-id="84dce-222">下列程式碼會移除 `TextOutputFormatter` 和 `HttpNoContentOutputFormatter`。</span><span class="sxs-lookup"><span data-stu-id="84dce-222">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupTextOutputFormatter.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupTextOutputFormatter.cs?name=snippet)]
::: moniker-end

<span data-ttu-id="84dce-223">若沒有`string` `406 Not Acceptable`，傳回類型會傳回。 `TextOutputFormatter`</span><span class="sxs-lookup"><span data-stu-id="84dce-223">Without the `TextOutputFormatter`, `string` return types return `406 Not Acceptable`.</span></span> <span data-ttu-id="84dce-224">如果 XML 格式器存在，則會`string`格式化傳回的`TextOutputFormatter`類型（如果已移除）。</span><span class="sxs-lookup"><span data-stu-id="84dce-224">If an XML formatter exists, it formats `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="84dce-225">如果沒有 `HttpNoContentOutputFormatter`，則會使用已設定的格式器來格式化 Null 物件。</span><span class="sxs-lookup"><span data-stu-id="84dce-225">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="84dce-226">例如:</span><span class="sxs-lookup"><span data-stu-id="84dce-226">For example:</span></span>

* <span data-ttu-id="84dce-227">JSON 格式器會傳回具有主體的`null`回應。</span><span class="sxs-lookup"><span data-stu-id="84dce-227">The JSON formatter returns a response with a body of `null`.</span></span>
* <span data-ttu-id="84dce-228">Xml 格式器會傳回具有屬性`xsi:nil="true"`集的空 xml 元素。</span><span class="sxs-lookup"><span data-stu-id="84dce-228">The XML formatter returns an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="84dce-229">回應格式 URL 對應</span><span class="sxs-lookup"><span data-stu-id="84dce-229">Response format URL mappings</span></span>

<span data-ttu-id="84dce-230">用戶端可以要求特定格式做為 URL 的一部分，例如：</span><span class="sxs-lookup"><span data-stu-id="84dce-230">Clients can request a particular format as part of the URL, for example:</span></span>

* <span data-ttu-id="84dce-231">在查詢字串或部分路徑中。</span><span class="sxs-lookup"><span data-stu-id="84dce-231">In the query string or part of the path.</span></span>
* <span data-ttu-id="84dce-232">使用格式特定的副檔名，例如 .xml 或. json。</span><span class="sxs-lookup"><span data-stu-id="84dce-232">By using a format-specific file extension such as .xml or .json.</span></span>

<span data-ttu-id="84dce-233">應該在 API 所使用的路由中指定要求路徑的對應。</span><span class="sxs-lookup"><span data-stu-id="84dce-233">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="84dce-234">例如:</span><span class="sxs-lookup"><span data-stu-id="84dce-234">For example:</span></span>

[!code-csharp[](./formatting/sample/Controllers/ProductsController.cs?name=snippet)]

<span data-ttu-id="84dce-235">先前的路由可讓要求的格式指定為選用的副檔名。</span><span class="sxs-lookup"><span data-stu-id="84dce-235">The preceding route allows the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="84dce-236">屬性會檢查`RouteData`中的格式值是否存在，並在建立回應時，將回應格式對應至適當的格式器。 [`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute)</span><span class="sxs-lookup"><span data-stu-id="84dce-236">The [`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) attribute checks for the existence of the format value in the `RouteData` and maps the response format to the appropriate formatter when the response is created.</span></span>

|           <span data-ttu-id="84dce-237">路由</span><span class="sxs-lookup"><span data-stu-id="84dce-237">Route</span></span>        |             <span data-ttu-id="84dce-238">格式器</span><span class="sxs-lookup"><span data-stu-id="84dce-238">Formatter</span></span>              |
|------------------------|------------------------------------|
|   `/api/products/5`    |    <span data-ttu-id="84dce-239">預設輸出格式器</span><span class="sxs-lookup"><span data-stu-id="84dce-239">The default output formatter</span></span>    |
| `/api/products/5.json` | <span data-ttu-id="84dce-240">JSON 格式器 (如果已設定)</span><span class="sxs-lookup"><span data-stu-id="84dce-240">The JSON formatter (if configured)</span></span> |
| `/api/products/5.xml`  | <span data-ttu-id="84dce-241">XML 格式器 (如果已設定)</span><span class="sxs-lookup"><span data-stu-id="84dce-241">The XML formatter (if configured)</span></span>  |
